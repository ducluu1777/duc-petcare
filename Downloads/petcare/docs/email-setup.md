# Gửi email bằng Gmail SMTP

PetCare dùng một tài khoản Gmail để gửi OTP, mã đặt lại mật khẩu và email booking/đơn hàng qua `smtp.gmail.com:465` với TLS. Không cần Resend hay tên miền gửi mail riêng.

## Cấu hình

1. Bật **Xác minh 2 bước** cho tài khoản Google gửi thư.
2. Mở [Mật khẩu ứng dụng của Google](https://myaccount.google.com/apppasswords), tạo mật khẩu cho PetCare và sao chép mật khẩu 16 ký tự.
3. Điền vào `petcare-be/.env` trên máy hoặc biến môi trường của backend khi triển khai:

   ```env
   MAIL_USER="dia-chi-gui@gmail.com"
   MAIL_PASS="mat-khau-ung-dung-google"
   ```

   `MAIL_PASS` là **App Password**, không phải mật khẩu đăng nhập Google hay API key Resend. Có thể dán kèm khoảng trắng giữa các nhóm ký tự; backend tự bỏ khoảng trắng. Không đưa hai biến này vào frontend hoặc gửi mật khẩu qua chat.

4. Kiểm tra từ thư mục backend:

   ```powershell
   cd petcare-be
   npm run mail:verify
   ```

   Lệnh kiểm tra kết nối và xác thực, không gửi email hay ghi database. Nếu báo xác thực thất bại, kiểm tra đúng địa chỉ Gmail và tạo lại App Password.

5. Khởi động lại backend để nạp cấu hình mới. Sau đó có thể thử yêu cầu OTP bằng email của chính bạn và kiểm tra hộp thư, kể cả mục Spam.

## Khi có lỗi

- Không thấy mục Mật khẩu ứng dụng: kiểm tra Xác minh 2 bước và điều kiện tài khoản theo [hướng dẫn Google](https://support.google.com/mail/answer/185833?hl=vi).
- Sau khi đổi mật khẩu Google: tạo lại App Password vì Google có thể thu hồi mật khẩu ứng dụng cũ.
- Kết nối SMTP thành công chưa chứng minh email đã đến hộp thư; cần thử một email thật để kiểm tra việc phát thư.
- OTP không gửi được thì API báo lỗi. Email nghiệp vụ gửi lỗi không hoàn tác booking/đơn đã lưu.
- Gmail có giới hạn gửi thư và có thể chặn kết nối đáng ngờ; phù hợp demo hoặc lưu lượng nhỏ. Xem [hướng dẫn Gmail của Nodemailer](https://nodemailer.com/guides/using-gmail).
