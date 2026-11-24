# Cấu hình giao hàng và SePay cho đơn online

Nếu cài lần đầu hoặc cần bật tunnel local, xem [Hướng dẫn Windows và npm](HUONG_DAN_WINDOWS_NPM.md). Có hướng dẫn Cloudflare Tunnel, tài khoản Admin và các ca thử không dùng tiền thật.

## Khu vực giao

Vào **Admin → Đơn hàng & kho → Khu vực giao**. Nhập tỉnh/thành, phường/xã và phí cố định bằng đồng; phí `0` hợp lệ. Chỉ khu vực đang bật xuất hiện ở checkout. Không có khu vực nào thì khách chỉ chọn nhận tại cửa hàng. PetCare không tự tra địa giới hay khoảng cách; nhân viên cần kiểm tra tên phường/xã trước khi công bố.

## Tài khoản nhận tiền và webhook

Trong `petcare-be/.env`, đặt `PORT=3001` để backend lắng nghe cổng mà bạn sẽ trỏ tunnel tới. Điền các biến SePay theo tài khoản trên SePay:

```env
SEPAY_BANK_CODE=MBBank
SEPAY_BANK_NAME=MBBank
SEPAY_ACCOUNT_NUMBER=<số VA dùng để nhận QR>
SEPAY_ACCOUNT_NAME=<tên chủ tài khoản hiển thị trên SePay>
SEPAY_MAIN_ACCOUNT_NUMBER=<tài khoản chính liên kết với VA>
SEPAY_WEBHOOK_SECRET=<secret HMAC-SHA256 đã tạo trong SePay>
```

Thay toàn bộ giá trị `<...>` bằng giá trị thật, không giữ dấu ngoặc. Với VA chính thức, `SEPAY_ACCOUNT_NUMBER` là số VA dùng để dựng QR, còn `SEPAY_MAIN_ACCOUNT_NUMBER` là tài khoản chính mà SePay gửi trong webhook `accountNumber`; webhook cũng phải có `subAccount` trùng số VA. Nếu nhận tiền trực tiếp vào tài khoản thường, `SEPAY_ACCOUNT_NUMBER` là số tài khoản nhận và để trống `SEPAY_MAIN_ACCOUNT_NUMBER`. `SEPAY_WEBHOOK_SECRET` phải giống hệt secret HMAC-SHA256 đã nhập/tạo trên SePay. Chỉ lưu secret ở backend, không đưa vào FE hoặc Git. QR chỉ sẵn sàng khi đủ tài khoản nhận và secret; webhook không xử lý giao dịch khi thiếu secret.

Trên SePay, tạo một webhook **Có tiền vào**, định dạng JSON, chọn đúng tài khoản nhận và xác thực **HMAC-SHA256** bằng cùng secret. Với tunnel trỏ tới `http://localhost:3001`, URL webhook có dạng `https://<tunnel-host>/api/v1/payment/sepay/webhook`. Nếu SePay có bộ lọc mã thanh toán, cho phép cả hai tiền tố `PCSV` và `PCOD`, đồng thời bảo đảm payload có trường `code`. Trong cấu hình nhận diện mã, dùng loại **Số và chữ**: `PCSV` có hậu tố 8 ký tự; `PCOD` có hậu tố từ 8 đến 20 ký tự để nhận đủ cả POS và đơn online.

Mã thanh toán dịch vụ/lịch hẹn bắt đầu bằng `PCSV` và có thêm 8 ký tự chữ-số. Mã đơn sản phẩm tại checkout bắt đầu bằng `PCOD` và có thêm 20 ký tự; bán sản phẩm tại quầy cũng dùng `PCOD` và thêm 8 ký tự. Các mã được tạo riêng cho từng khoản; webhook dùng chúng để ghép giao dịch. Cấu hình tương ứng cả Test mode và Live. API phải truy cập được qua HTTPS công khai để SePay gửi webhook.

PetCare kiểm tra chữ ký trên raw body và timestamp ±5 phút, tài khoản nhận, mã giao dịch, chiều tiền vào và **số tiền chính xác**. Webhook gửi lặp cùng ID không thu thêm. Giao dịch hợp lệ tự xác nhận đơn online, thanh toán lịch hẹn và bán tại quầy; sai thông tin, sai số tiền hoặc đến sau hạn thanh toán được đưa vào **Admin → Đơn hàng → Giao dịch cần đối chiếu**. Đơn online chưa thanh toán tự hủy khi hết 24 giờ, trả tồn/voucher và thông báo khách. Nhân viên kiểm tra giao dịch lệch trên SePay, liên hệ khách hoặc hoàn tiền ngoài PetCare, rồi ghi cách xử lý. Nút **Đã xử lý** chỉ đóng mục đối chiếu, không tự xác nhận thanh toán.

## Kiểm tra không dùng tiền thật

Bật **Test mode** tại SePay, tạo tài khoản ngân hàng và webhook thử, rồi mô phỏng giao dịch đúng mã/số tiền. Kiểm tra các trường hợp sai số tiền, sai tài khoản, mã không tồn tại và gửi lại cùng ID trên lịch sử webhook và danh sách đối chiếu. Test mode tách với Live; dùng secret, tài khoản và endpoint QA riêng, không trỏ giao dịch giả vào môi trường phục vụ khách.

Tài liệu SePay: [tích hợp webhook](https://docs.sepay.vn/tich-hop-webhooks.html), [xác thực HMAC](https://developer.sepay.vn/vi/sepay-webhooks/xac-thuc), [Test mode](https://developer.sepay.vn/vi/sepay-webhooks/test-mode/bat-dau-nhanh).
