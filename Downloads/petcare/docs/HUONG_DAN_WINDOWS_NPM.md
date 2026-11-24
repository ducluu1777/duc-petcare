# Hướng dẫn mở PetCare trên Windows

Lần đầu làm mục 1–2. Từ lần sau, chỉ làm mục 3. Mục 5 dành cho lúc thử thanh toán QR.

Các lệnh dưới đây dùng `npm`, chạy được trên Windows và macOS. Nếu PowerShell trên Windows chặn `npm.ps1`, dùng `npm.cmd` thay cho `npm`, hoặc chuyển terminal sang **Command Prompt**.

## 1. Cài phần mềm

Tải và cài ba ứng dụng, giữ các lựa chọn mặc định:

- **Node.js 22.x**, từ bản **22.22.3** trở lên; npm được cài kèm.
- **Docker Desktop**. Nếu được yêu cầu bật WSL 2, làm theo thông báo rồi khởi động lại máy.
- **Visual Studio Code (VS Code)**.

Giải nén dự án vào một thư mục, ví dụ `C:\petcare`. Mở VS Code → **File → Open Folder** → chọn thư mục đó.

## 2. Chuẩn bị lần đầu

### A. Tạo file cấu hình

Trong danh sách file bên trái VS Code:

1. Mở thư mục **petcare-be**. Nếu chưa có `.env`, sao chép `.env.example` rồi đổi tên bản sao thành **`.env`**.
2. Mở thư mục **petcare-fe**. Nếu chưa có `.env.local`, sao chép `.env.example` rồi đổi tên bản sao thành **`.env.local`**.
3. Nếu đã nhận các file cấu hình từ người bàn giao, giữ nguyên, không chép đè.

Mở các file vừa tạo và kiểm tra:

| File                    | Nội dung cần có                                                                        |
| ----------------------- | -------------------------------------------------------------------------------------- |
| `petcare-be/.env`       | `PORT=3001` và thêm dòng `NODE_ENV=development`                                        |
| `petcare-be/.env`       | `DATABASE_URL` dùng cổng `5434`, tên DB `petcare_db`; mật khẩu theo thông tin bàn giao |
| `petcare-fe/.env.local` | `NEXT_PUBLIC_API_URL="http://localhost:3001/api/v1"`                                   |

Nếu dùng cấu hình mẫu, thay `JWT_SECRET` và `JWT_REFRESH_SECRET` bằng **hai chuỗi ngẫu nhiên khác nhau, mỗi chuỗi ít nhất 32 ký tự**. Không giữ giá trị mẫu. Bấm **Ctrl+S** để lưu.

### B. Cài dự án

Đây là phần cần nhập lệnh, chỉ làm lần đầu:

1. Mở **Docker Desktop**, chờ ứng dụng chạy xong.
2. Trong VS Code, chuột phải thư mục **petcare-be** → **Open in Integrated Terminal**. Dán từng dòng dưới đây, bấm Enter và chờ xong mới chạy dòng tiếp:

   ```powershell
   npm i
   npm run prisma:generate
   docker compose up -d postgres
   ```

   `npm i` tải các thư viện cần thiết. Chạy lại bước này nếu vừa xóa `node_modules` hoặc nhận bản code có thay đổi thư viện.

3. Chuột phải thư mục **petcare-fe** → **Open in Integrated Terminal**, chạy:

   ```powershell
   npm i
   ```

**Dữ liệu và tài khoản:**

- Máy này đã có DB PetCare: dùng tiếp dữ liệu và Admin hiện tại.
- Cài sang máy khác: nhận file `petcare.dump` và tài khoản Admin từ người bàn giao, rồi làm [bước nhập dữ liệu](../petcare-be/DATABASE.md#chuyển-database-sang-máy-khác).
- Muốn bắt đầu với DB rỗng: tại terminal của **petcare-be**, chạy `npm run prisma:push`, rồi [tạo Admin mới](../petcare-be/DATABASE.md#tạo-admin-cho-database-mới). Nếu có cảnh báo mất dữ liệu, dừng lại.

## 3. Mỗi lần mở dự án

**Bật database:** mở **Docker Desktop → Containers**, tìm `petcare-postgres` và bấm **Start**. Nếu nó nằm trong một nhóm, mở nhóm để tìm. Thấy **Running** là được. [Hướng dẫn Docker Desktop](https://docs.docker.com/desktop/use-desktop/container/).

**Bật website và API:**

1. Chuột phải **petcare-be** → **Open in Integrated Terminal**, chạy `npm run dev`. Giữ terminal này mở.
2. Chuột phải **petcare-fe** → **Open in Integrated Terminal**, chạy `npm run dev`. Giữ terminal thứ hai mở.
3. Chờ cả hai chạy xong; website dùng cổng **3000**, API dùng cổng **3001**.

Mở trình duyệt:

| Bạn muốn         | Địa chỉ                               |
| ---------------- | ------------------------------------- |
| Dùng website     | http://localhost:3000                 |
| Vào Admin        | http://localhost:3000/dang-nhap-admin |
| Kiểm tra kết nối | http://localhost:3001/api/v1/health   |

Dùng tài khoản Admin đã được bàn giao. Trang kiểm tra cần hiện `status: "ok"` và `database.connected: true`.

**Khi tắt:** chọn từng cửa sổ lệnh đang chạy trong VS Code, bấm **Ctrl+C**. Trong Docker Desktop, bấm **Stop** ở `petcare-postgres`. Không bấm **Delete**.

## 4. Điền email, ảnh và AI

Mở **petcare-be/.env**, điền thông tin của bạn vào các dòng tương ứng:

| Muốn dùng  | Điền các dòng                                                                       |
| ---------- | ----------------------------------------------------------------------------------- |
| Gửi email  | `MAIL_USER`: Gmail gửi thư; `MAIL_PASS`: [mật khẩu ứng dụng Google](email-setup.md) |
| Upload ảnh | `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`              |
| Trợ lý AI  | `GEMINI_API_KEY`; giữ tên model theo file mẫu                                       |

## 5. Thử thanh toán QR bằng Cloudflare

### A. Mở đường kết nối từ SePay về máy

1. Bật dự án theo mục 3. Kiểm tra `AUTH_DEV_OTP_PREVIEW=false` trong backend `.env` trước khi mở tunnel; nếu vừa đổi, lưu và chạy lại API.
2. Tải **Windows 64-bit Executable** từ [Cloudflare Downloads](https://developers.cloudflare.com/tunnel/downloads/#windows). Đổi tên file thành **cloudflared.exe**, đặt trong một thư mục riêng.
3. Mở thư mục đó bằng File Explorer. Bấm thanh địa chỉ ở trên, gõ **powershell**, bấm Enter.
4. Trong cửa sổ vừa mở, dán lệnh:

   ```powershell
   .\cloudflared.exe tunnel --url http://localhost:3001
   ```

5. Giữ cửa sổ mở. Sao chép địa chỉ dạng **https://abc.trycloudflare.com** được in ra. Đây là địa chỉ tunnel, không cần tài khoản Cloudflare.

### B. Điền thông tin trên SePay

1. Đăng nhập SePay, bật **Test mode** và tạo một tài khoản ngân hàng thử.
2. Mở **petcare-be/.env**, điền thông tin tài khoản thử vào bốn dòng `SEPAY_BANK_CODE`, `SEPAY_BANK_NAME`, `SEPAY_ACCOUNT_NUMBER`, `SEPAY_ACCOUNT_NAME`. Tài khoản thường để trống `SEPAY_MAIN_ACCOUNT_NUMBER`; [tài khoản ảo xem tại đây](sepay-online-delivery-setup.md#tài-khoản-nhận-tiền-và-webhook).
3. Trên SePay, thêm webhook theo bảng:

   | Ô cần điền | Giá trị                                                       |
   | ---------- | ------------------------------------------------------------- |
   | URL        | Địa chỉ tunnel vừa sao chép + `/api/v1/payment/sepay/webhook` |
   | Sự kiện    | Có tiền vào                                                   |
   | Định dạng  | JSON                                                          |
   | Tài khoản  | Tài khoản thử vừa tạo                                         |
   | Xác thực   | HMAC-SHA256                                                   |
   | Trạng thái | Bật                                                           |

   Ví dụ URL đầy đủ: `https://abc.trycloudflare.com/api/v1/payment/sepay/webhook`.

4. Chép **HMAC Secret** của webhook vào `SEPAY_WEBHOOK_SECRET` trong `.env`. Lưu file và chạy lại **petcare-be** như mục 3.
5. Trong cấu hình nhận diện mã thanh toán của SePay, thêm hai mẫu loại **Số và chữ**: `PCSV` có phần sau dài **8 ký tự**; `PCOD` có phần sau dài **8–20 ký tự**. Cho phép cả hai nếu có bộ lọc mã.

### C. Thử một đơn

1. Trên PetCare, đặt một đơn sản phẩm thử rồi mở trang thanh toán.
2. Lấy **mã PCOD…** và **tổng tiền** trên QR.
3. Trong **SePay Test mode → mô phỏng giao dịch**, chọn **Tiền vào**, đúng tài khoản thử, đúng tổng tiền; nội dung là mã PCOD vừa lấy.
4. Quay lại PetCare, kiểm tra đơn hiện **Đã thanh toán** và Admin ghi nhận đúng khoản thu.

Không chuyển tiền thật để thử. Dịch vụ Spa dùng mã **PCSV…**, đơn sản phẩm dùng **PCOD…**. Không lấy mã lịch BK hoặc mã đơn OD thay mã thanh toán.

Mỗi lần mở lại tunnel, lấy URL mới và sửa webhook trên SePay. Khi xong, **Ctrl+C** để tắt tunnel. Website vẫn dùng `localhost:3000`, không cần sửa địa chỉ frontend. [Tham khảo Cloudflare Quick Tunnel](https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/).

## 6. Nếu chưa chạy được

| Bạn thấy gì?                            | Kiểm tra trước                                                                                      |
| --------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Web không mở                            | Hai mục dev đã chạy chưa? Có cửa sổ nào báo lỗi không?                                              |
| Không có dữ liệu / không đăng nhập được | Docker có hiện `petcare-postgres` đang Running không? Đã nhập DB bàn giao chưa?                     |
| PowerShell chặn npm.ps1                 | Trong VS Code, chọn Terminal → Select Default Profile → Command Prompt, rồi dừng và bấm Run lại dev |
| Báo cổng đang dùng                      | Dừng cửa sổ chạy cũ, chỉ giữ một dev cho mỗi thư mục                                                |
| Sửa cấu hình nhưng chưa đổi             | Lưu file và dừng/chạy lại ứng dụng                                                                  |
| SePay gửi lỗi hoặc đơn chưa thanh toán  | Mở lịch sử webhook trên SePay; kiểm tra URL tunnel, HMAC Secret, mã và số tiền                      |

Hướng dẫn đã đối chiếu source và tài liệu công cụ; chưa kiểm tra cài mới trực tiếp trên Windows.
