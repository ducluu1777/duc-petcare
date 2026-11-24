# Cẩm nang nội dung PetCare

## Định hướng

Viết từ góc nhìn của người đang chăm một thành viên trong gia đình: ấm áp, tôn trọng sự gắn bó với thú cưng, nhưng vẫn rõ ràng và có căn cứ. Giọng điệu thân thiện như người đồng hành, chuyên nghiệp ở các nội dung về chăm sóc, giá, lịch hẹn và thanh toán.

Ưu tiên gọi tên thú cưng khi đã biết tên; dùng “bé” tự nhiên, không lạm dụng. Viết câu ngắn, chủ động, giải thích điều sẽ xảy ra tiếp theo và điều khách cần làm. Không dùng emoji trong trợ lý AI, chatbot hoặc thông báo nghiệp vụ.

## Ma trận giọng điệu

PetCare giữ nguyên tiếng nói thương hiệu ở mọi kênh, nhưng điều chỉnh mức độ thân thiện, trang trọng và chi tiết theo tình huống.

| Tình huống | Sắc thái | Cách viết |
| --- | --- | --- |
| Giới thiệu dịch vụ, sản phẩm | Ấm áp, có hứng thú vừa phải | Nêu lợi ích cụ thể, điều kiện áp dụng và đường dẫn hành động. Không dùng lời khen tuyệt đối. |
| Tư vấn trong chat | Gần gũi, bình tĩnh | Xưng “mình”, gọi khách là “bạn”; trả lời trực tiếp trong 1–3 câu rồi hỏi một thông tin còn thiếu nếu cần. |
| Đặt lịch, giá và thanh toán | Rõ ràng, trung tính | Nêu số tiền, trạng thái, điều kiện và bước tiếp theo. Đặt thông tin quan trọng trước lời giải thích. |
| Lỗi hoặc thao tác chưa thành công | Tôn trọng, hướng giải quyết | Nói điều chưa thực hiện được, dữ liệu nào còn được giữ và khách cần làm gì tiếp theo. Không đổ lỗi cho khách. |
| Khiếu nại hoặc cần nhân viên | Điềm tĩnh, hỗ trợ | Xác nhận nhu cầu ngắn gọn rồi đưa ngay kênh liên hệ hoặc cách gặp nhân viên. Không trả lời vòng sang nội dung hướng dẫn chung. |
| Sức khỏe thú cưng | Thận trọng, có giới hạn | Chỉ cung cấp thông tin chăm sóc chung; khuyên liên hệ bác sĩ thú y khi có triệu chứng, vết thương hoặc câu hỏi điều trị. |

## Quy tắc từ ngữ

Điểm khách tích được sau thanh toán gọi thống nhất là **Điểm tích lũy**, bao gồm nhãn số dư, lịch sử cộng/trừ, đổi voucher và thông báo.

Khi áp voucher không được vì mã không tồn tại, tạm ngưng, hết hạn/lượt, chưa có trong ví, đã dùng hoặc hóa đơn không đủ điều kiện, dùng chung câu **“Voucher không khả dụng.”** Không diễn giải riêng việc khách chưa đổi bằng điểm. Điều kiện voucher vẫn thể hiện trong phần thông tin voucher.

**Nên dùng:** “mình”, “bạn”, “bé” khi tự nhiên, “giá tham khảo”, “cân lại”, “xác nhận”, “đang còn”, “tạm hết”, “thử lại”, “liên hệ nhân viên”.

**Cần tránh:** đổi lẫn “mình” và “em”, lặp “bé cưng”, gọi khách là “Sen” hoặc thú cưng là “Boss” trong nội dung nghiệp vụ, tiếng lóng, emoji, dấu chấm than liên tiếp, từ ngữ khoa trương và lời hứa về kết quả.

Không mở đầu mọi câu bằng “Dạ”. Chỉ chào ở lượt đầu hoặc khi ngữ cảnh cần sự trang trọng. Khi khách viết “tôi cần gặp nhân viên”, trả lời thẳng: “Bạn có thể gọi 0941 900 966 hoặc gửi thông tin tại trang Liên hệ. Nhân viên PetCare sẽ tiếp nhận trao đổi của bạn.”

**Nhãn nghiệp vụ:** Không đưa enum hoặc mã lỗi nội bộ lên nội dung hiển thị. Viết “Đã tiếp nhận”, “Đã hủy”, “Nhân viên”, “Số tiền không khớp” thay cho `ARRIVED`, `CANCELLED`, `STAFF`, `AMOUNT_MISMATCH`. Nếu chưa nhận diện giá trị, dùng nhãn như “Cần kiểm tra giao dịch”; không hiển thị mã thô. Giữ nguyên mã lịch, đơn, thanh toán, tên model và nội dung người dùng tự nhập để tra cứu đúng.

## Trụ cột nội dung

- **Hướng dẫn chăm sóc:** Chia sẻ kiến thức thực tế, dễ áp dụng và phân biệt rõ thông tin tham khảo với tư vấn chuyên môn. Không chẩn đoán, hứa chữa khỏi hoặc thay thế bác sĩ thú y.
- **Quy trình và dịch vụ:** Nói rõ nhân viên tiếp nhận, trao đổi lưu ý, cân thực tế, xác nhận thay đổi giá nếu có, rồi mới bắt đầu. Giá và cân nặng khai báo khi đặt lịch là ước tính.
- **Câu chuyện và hình ảnh thật:** Chỉ đăng ca chăm sóc, câu chuyện cứu hộ, ảnh trước–sau hoặc nội dung khách gửi khi có tư liệu xác thực và sự đồng ý phù hợp. Không dựng khách hàng, đánh giá, kết quả, chứng nhận hay số liệu.
- **Đánh giá và nội dung cộng đồng:** Trích đúng phản hồi có thật, giữ nguyên ý và không biến đánh giá thành cam kết cho mọi trường hợp. Nội dung bắt trend là lựa chọn cho kênh giải trí, không thay thế hướng dẫn vận hành.
- **Đặt lịch và mua sắm:** Đưa đường dẫn tới dịch vụ, giá tham khảo, đặt lịch và sản phẩm gần với nội dung đang đọc; nêu điều kiện quan trọng trước khi khách xác nhận.

## Ranh giới theo phạm vi PetCare

- Phạm vi dịch vụ gồm Spa/Grooming và cửa hàng. Chỉ giới thiệu những dịch vụ đang mở trong catalog.
- Chỉ nêu tiện ích, thời gian, ưu đãi và cách xử lý đã được triển khai hoặc chốt trong BA. Ưu đãi cần khớp trạng thái bật/tạm ngưng hiện tại.
- Với cân nặng, giá và giờ hẹn: giải thích giá đặt lịch là tạm tính, cân nặng thực tế được xác nhận tại tiệm và khách được báo trước nếu giá thay đổi.
- Dùng mô tả trung tính như “vệ sinh tai” hoặc “vệ sinh tuyến hậu môn”; không hứa phòng hay chữa bệnh.
- Không tự tạo nội dung blog, social proof hoặc câu chuyện cứu hộ khi chưa có nguồn và người duyệt.

## Ví dụ giọng điệu

**Nên viết:** “Giá đặt lịch là mức tham khảo theo cân nặng bạn nhập. Khi bé đến tiệm, nhân viên sẽ cân lại và báo bạn nếu giá cần điều chỉnh. Dịch vụ chỉ bắt đầu sau khi bạn đồng ý.”

**Tránh viết:** “Bé sẽ được chăm sóc hoàn hảo, sạch khỏe tuyệt đối với đội ngũ chuyên gia hàng đầu.”

**Nên viết trong chat:** “Mình tìm thấy 2 hồ sơ thú cưng của bạn. Bạn muốn đặt lịch cho Bông hay Mít?”

**Tránh viết trong chat:** “Dạ, em tìm thấy 2 bé cưng siêu đáng yêu của bạn ạ. Bạn muốn em hỗ trợ gì thêm không ạ?”

**Nên viết khi có lỗi:** “Mình chưa kiểm tra được khung giờ lúc này. Thông tin bạn đã nhập vẫn được giữ; bạn thử lại sau ít phút hoặc gọi 0941 900 966.”

**Tránh viết khi có lỗi:** “Ối! Hệ thống đang bận rồi. Bạn vui lòng thao tác lại nhé!”

## Kiểm tra nội dung trên màn hình nhỏ

- Giữ tiêu đề và đoạn mở đầu đủ ngắn để đọc nhanh trên điện thoại; chuyển điều kiện dài sang phần chi tiết gần thao tác liên quan.
- Ở các mốc 320, 375, 768 và 1280 px, kiểm tra không có cuộn ngang toàn trang, chữ đè ảnh, nút bị cắt hoặc thông tin giá/trạng thái bị khuất.
- Cho phép tên dịch vụ, tên sản phẩm, giống loài và thông báo lỗi dài tự xuống dòng; không ép nhãn dài vào một hàng cố định.
- Ảnh cần co theo cột và giữ vùng cắt có chủ ý; kiểm tra thẻ ảnh, gallery, bảng giá và trạng thái ảnh lỗi trên cả mobile và desktop.
- Nếu bảng hoặc dải tab cần cuộn ngang, giới hạn vùng cuộn vào chính thành phần đó và để dấu hiệu thao tác vẫn dễ nhận biết.
- Không dùng `overflow-hidden` để che phần chữ hoặc điều khiển bị tràn; xác định phần tử gây tràn rồi sửa giới hạn kích thước, breakpoint hoặc cách xuống dòng.

## Nguồn tham khảo

Cẩm nang này áp dụng các nguyên tắc về tiếng nói thương hiệu nhất quán, điều chỉnh sắc thái theo ngữ cảnh và dùng bảng từ nên dùng/cần tránh từ các tài liệu sau. Quy tắc nghiệp vụ PetCare trong BA và dữ liệu đang vận hành luôn được ưu tiên khi viết nội dung cụ thể.

- [Hướng dẫn viết content cho dịch vụ thú cưng chuyên nghiệp — VolxAI](https://volxai.com/blog/huong-dan-viet-content-ve-dich-vu-thu-cung-chuyen-nghiep)
- [Tone of Voice là gì? 4 khía cạnh và cách xây dựng giọng điệu — Kind Content](https://kindcontent.net/brand-voice/tone-of-voice/)
- [Hiểu đúng về Brand Tone of Voice — Advertising Vietnam](https://advertisingvietnam.com/article/hieu-dung-ve-brand-tone-of-voice-cach-thuong-hieu-the-hien-ban-sac-va-giao-tiep-ca-nhan-hoa-voi-nguoi-dung-muc-tieu-p21587)
- [Brand Voice là gì? 9 mẹo xây dựng Brand Voice — Brands Vietnam](https://www.brandsvietnam.com/congdong/topic/334011-brand-voice-la-gi-9-meo-xay-dung-brand-voice-cho-thuong-hieu-qua-vi-du-thuc-te-tu-dove)

