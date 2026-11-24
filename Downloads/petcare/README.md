# PetCare

Website đặt lịch Spa, mua sản phẩm và quản lý vận hành cửa hàng thú cưng. Frontend dùng Next.js, backend dùng NestJS/Prisma, database PostgreSQL chạy trong Docker.

**Người mới bắt đầu tại [Hướng dẫn Windows bằng thao tác từng bước](docs/HUONG_DAN_WINDOWS_NPM.md).** Có cấu hình mẫu, tạo Admin, chạy cổng 3000/3001, bật Cloudflare Tunnel, mô phỏng thanh toán SePay và cách xử lý lỗi thường gặp. Những lần chạy tiếp theo chỉ cần mục 3 trong hướng dẫn.

| Thành phần | Địa chỉ local |
| --- | --- |
| Website | http://localhost:3000 |
| Đăng nhập nội bộ | http://localhost:3000/dang-nhap-admin |
| Backend API | http://localhost:3001/api/v1 |
| API health | http://localhost:3001/api/v1/health |
| Swagger | http://localhost:3001/api/docs |
| PostgreSQL | localhost:5434 |
~
## Tài liệu bàn giao

| Cần tìm | Tài liệu |
| --- | --- |
| Windows/npm, tạo Admin, chuyển DB, tunnel và thử webhook | [Hướng dẫn chạy dự án](docs/HUONG_DAN_WINDOWS_NPM.md) |
| Quy tắc và phạm vi nghiệp vụ | [Đặc tả BA](petcare-be/BA_REQUIREMENTS.md) |
| Cấu trúc dữ liệu, lưu ý nâng cấp DB | [Database](petcare-be/DATABASE.md) |
| Ngân hàng, tài khoản ảo và khu vực giao | [SePay và giao hàng](docs/sepay-online-delivery-setup.md) |
| Gửi OTP và email nghiệp vụ | [Gmail SMTP](docs/email-setup.md) |
| Model, kho kiến thức, vector và log AI | [Vận hành trợ lý AI](docs/ai-assistant-deployment.md) |
| Cách viết nội dung giao diện và chatbot | [Cẩm nang nội dung](docs/content-voice-guide.md) |
| Lệnh chạy/build từng ứng dụng | [Backend](petcare-be/README.md), [Frontend](petcare-fe/README.md) |

Danh sách endpoint được cung cấp trực tiếp qua Swagger khi API chạy. Chi tiết bảng, cột và quan hệ theo [Prisma schema](petcare-be/prisma/schema.prisma); không dùng bản kế hoạch API hoặc sơ đồ schema lịch sử thay cho source hiện hành.

## Kiểm tra trước khi nghiệm thu

Chưa có xác nhận nghiệm thu toàn sản phẩm trên môi trường nhận bàn giao. Cần hoàn tất các mục sau và ghi kết quả tại thời điểm bàn giao:

- [ ] Cài từ hướng dẫn, API/DB khỏe; đăng nhập khách và Admin/Staff đúng quyền.
- [ ] Đặt/đổi/hủy lịch; tiếp nhận, bắt đầu chăm sóc bằng cân đã lưu, sửa cân khi cần, thu tiền và bàn giao.
- [ ] Mua online, nhận tại cửa hàng/giao hàng và bán tại quầy; đối chiếu voucher, tồn kho, điểm, hóa đơn và email.
- [ ] SePay Test mode: khớp, sai thông tin, gửi lặp, hết hạn, webhook gián đoạn và xác nhận tay; kiểm tra đồng thời trên DB thực trước khi kết luận chống ghi trùng.
- [ ] Upload ảnh, hồ sơ, liên hệ, đánh giá, AI và theo dõi mức sử dụng; kiểm tra responsive/điều hướng toàn bộ route.
- [ ] Rà các thao tác nghiệp vụ còn rườm rà; xác minh phiếu hoàn tất chỉ đọc, phiếu hủy vẫn tra cứu được.

Tài khoản/mật khẩu, API key và cấu hình `.env` thực tế được bàn giao riêng. File `.env.example` chỉ là mẫu; không đóng gói secret hoặc bản sao dữ liệu khách cùng tài liệu công khai.
