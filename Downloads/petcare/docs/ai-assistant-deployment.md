# Vận hành trợ lý AI PetCare

## Cấu hình

Điền trong `petcare-be/.env`, sau đó khởi động lại API:

```dotenv
GEMINI_API_KEY="<api-key-cua-ban>"
GEMINI_MODEL="gemini-3.1-flash-lite"
GEMINI_EMBEDDING_MODEL="gemini-embedding-001"
AI_KNOWLEDGE_INDEX_PATH=".cache/ai/knowledge-index.json"
```

Model ID trên là cấu hình mẫu của dự án. Quyền truy cập, quota và chi phí thực tế phụ thuộc tài khoản nhà cung cấp; kiểm tra khi bàn giao. Không đưa key vào frontend hoặc Git. Thư mục cache cần quyền ghi; khi chạy container, dùng đường dẫn trên volume bền vững để tránh lập lại vector sau mỗi lần dựng container.

## AI lấy thông tin từ đâu?

- **Kiến thức nghiệp vụ:** sáu file Markdown trong [thư mục knowledge](../petcare-be/src/modules/ai-assistant/knowledge) là nội dung đóng gói ban đầu. API bổ sung các mục chưa có vào `AiKnowledgeEntry`, rồi đọc những mục đang bật trong DB.
- **Giá, tồn, dịch vụ, giờ trống và cấu hình công khai:** công cụ backend đọc dữ liệu hiện hành khi model cần. Không đưa toàn bộ DB hoặc thông tin riêng của khách vào kho vector.
- **Hội thoại:** model nhận câu hỏi cùng lịch sử được gửi trong request. Câu ngắn và lời chào vẫn qua model; không lấy một đoạn chính sách bất kỳ để trả thay model.

RAG là tìm các đoạn kiến thức liên quan rồi cung cấp cho model. Dự án dùng embedding 768 chiều, tìm theo độ tương đồng và lưu cache trên đĩa; chưa cần dịch vụ vector database riêng. Số đoạn có thể đổi theo nội dung, xem trạng thái thực tế trong Admin hoặc CLI thay vì dùng số lượng từ báo cáo cũ.

Model có thể mở bộ chọn sản phẩm hoặc lịch bằng công cụ `showChoices`. Khách vẫn xem lại và xác nhận thao tác. Thêm giỏ chưa tạo đơn hoặc thu tiền; tạo lịch dùng API có xác thực, kiểm tra chủ hồ sơ, giá/công suất và chống gửi lặp. AI không xác nhận thanh toán chỉ vì khách nhắn đã chuyển tiền.

## Cập nhật kiến thức

**Khi API đã dùng DB:** chỉnh nội dung trong mục quản lý kiến thức AI của Admin. Lưu hoặc bật/tắt mục sẽ tải lại kiến thức và lập lại embedding. Không đưa mật khẩu, thông tin cá nhân hoặc tài liệu nội bộ vào phần kiến thức công khai này.

**Khi sửa bộ nội dung đóng gói cho lần cài mới:** sửa file trong `knowledge/`, giữ tiêu đề và cập nhật dòng `<!-- reviewed: YYYY-MM-DD -->`. Từ thư mục `petcare-be`, chạy:

```powershell
npm run ai:knowledge -- "Cần chuẩn bị gì trước buổi hẹn?"
npm run build
```

CLI trên kiểm tra **bộ Markdown đóng gói**, có thể gọi embedding và ghi cache; không nạp bản chỉnh sửa từ DB và không tạo booking/đơn hay gửi email. Khi có key, `indexedChunks` phải bằng `chunks`; mã thoát lỗi nghĩa là chưa tạo đủ vector. Khi chưa có key, tìm từ khóa vẫn có thể hoạt động nhưng chưa chứng minh model thật trả lời được.

Sửa Markdown rồi restart **không ghi đè mục đã có cùng ID trong DB**. Muốn cập nhật bản đang phục vụ thì sửa qua Admin; sau đó kiểm tra lại nội dung trả lời. Giá/tồn/lịch đổi trong Admin không cần lập lại vector kiến thức.

Nest build chép Markdown vào `dist`. Khi triển khai backend, giữ cả các file kiến thức trong `dist`, không chỉ sao chép JavaScript.

## Theo dõi và xử lý lỗi

| Xem ở đâu? | Dùng để làm gì? |
| --- | --- |
| Dashboard và mục AI trong Admin | Model cấu hình, lượt dùng/hạn mức nội bộ, token được ghi nhận, lỗi và fallback, kiến thức/index |
| Terminal backend, log `ai_reply` | Nguồn trả lời, model, thời gian; khi fallback có lý do và trạng thái provider nếu có |
| Network của trình duyệt | Phản hồi `/api/v1/ai/chat` hoặc metadata `/api/v1/ai/chat/stream`, có `source: model` hoặc `source: fallback` |
| Tài khoản Google/provider | Quyền model, quota và hóa đơn thực tế; hạn mức PetCare không phải quota/billing của Google |

Log ứng dụng không phải nội dung suy luận nội bộ của model. Không chia sẻ log có key hoặc dữ liệu khách.

Fallback chỉ dùng khi model không khả dụng hoặc không hoàn tất phản hồi: thiếu key, quota, lỗi provider, timeout, nội dung trống hoặc vòng gọi công cụ không hoàn tất. Lỗi embedding/tra tài liệu không tự tắt model. Kiểm tra `source` khi đánh giá AI, vì câu dự phòng hiển thị được không chứng minh model đang chạy.

## Kiểm tra khi bàn giao

- Gõ lời chào, cung cấp thông tin ngắn như “mèo 5 kg”, đổi chủ đề và yêu cầu gặp nhân viên: trả lời theo ngữ cảnh, không tự tạo giao dịch.
- Hỏi chính sách và giá/tồn/lịch: kiểm tra câu trả lời đúng kiến thức/catalog hiện hành, có bước tiếp theo phù hợp.
- Thêm giỏ và đặt lịch trong chat: có xác nhận cuối, đúng tài khoản/chủ hồ sơ; gửi lại không tạo hai lịch.
- Thử lỗi provider/quota trong môi trường kiểm thử, kiểm tra fallback và log; xem hạn mức và token trong Admin.
- Xác minh model thật trả lời với key của môi trường nhận bàn giao. CLI embedding thành công không thay cho bước này.

Xem thêm [hướng dẫn chạy dự án](HUONG_DAN_WINDOWS_NPM.md), [BA](../petcare-be/BA_REQUIREMENTS.md) và [cẩm nang nội dung](content-voice-guide.md).
