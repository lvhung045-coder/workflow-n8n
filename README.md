# Hệ Thống Tự Động Gửi Email Khách Hàng (n8n Workflow)

Quy trình tự động hóa quét dữ liệu từ Google Sheets, lọc danh sách khách hàng chưa xử lý và tự động gửi email phản hồi qua Gmail.

## 🚀 Các bước hoạt động
1. **Trigger:** Lập lịch định kỳ kiểm tra dữ liệu trang tính.
2. **Filter:** Lọc ra các hàng có trạng thái chưa gửi mail.
3. **Gmail:** Tự động gửi email với nội dung cá nhân hóa.
4. **Update:** Cập nhật trạng thái "Đã gửi" vào Google Sheets để tránh gửi trùng lặp.

## 📥 Cách cài đặt và sử dụng
1. Tải file `.json` trong repository này về máy.
2. Mở n8n -> chọn menu **Import from File** -> chọn file vừa tải.
3. Kết nối thông tin xác thực (Credentials) tài khoản Google Sheets và Gmail của bạn.
4. Bật chế độ **Published** để chạy tự động.
# Hệ Thống Tự Động Phân Tích Phản Hồi Khách Hàng & Gửi Mail Bằng AI (n8n Workflow)

Quy trình tự động hóa đọc phản hồi khách hàng từ Google Sheets, sử dụng mô hình ngôn ngữ lớn (OpenAI Chat Model) để phân tích cảm xúc, tự động tạo nội dung phản hồi cá nhân hóa, gửi email qua Gmail và cập nhật nhãn trạng thái trở lại trang tính.

---

## 📌 Sơ đồ quy trình (Workflow Pipeline)

Quy trình gồm 7 bước chính được xử lý tự động theo chu kỳ:

1. **Mỗi 30 giây (Schedule Trigger):** Lập lịch tự động kích hoạt workflow sau mỗi 30 giây.
2. **Đọc phản hồi từ sheet (Google Sheets - Read):** Đọc các hàng dữ liệu phản hồi mới từ khách hàng.
3. **Lọc dòng chưa xử lý (Filter):** Lọc bỏ các dòng đã gửi để tránh xử lý hoặc gửi mail trùng lặp.
4. **AI phân tích và soạn phản hồi (LangChain Chain + OpenAI Chat Model + Structured Output Parser):**
   - Phân tích cảm xúc (Tích cực / Tiêu cực / Trung lập) và phân loại nội dung phản hồi.
   - Tự động sinh nội dung email trả lời tương ứng với thái độ của khách.
   - Định dạng đầu ra thành JSON có cấu trúc chuẩn để truyền tiếp dữ liệu.
5. **Gửi email phản hồi (Gmail - Send message):** Tự động gửi thư trả lời đến địa chỉ email của khách hàng.
6. **Chuẩn bị ghi vào sheet (Edit Fields / Set):** Định dạng và gán lại các giá trị nhãn AI, thời gian gửi và trạng thái xử lý.
7. **Ghi nhãn và đánh dấu đã gửi (Google Sheets - Update):** Cập nhật nhãn phân loại của AI và đánh dấu trạng thái "Đã gửi" vào hàng tương ứng trên Sheets.

---

## 🛠 Chuẩn bị trước khi chạy (Prerequisites)

1. **Bảng tính Google Sheets:** Tạo trang tính chứa tối thiểu các cột sau:
   - `Họ tên`
   - `Email`
   - `Nội dung phản hồi`
   - `Nhãn AI` *(để trống, AI sẽ tự điền)*
   - `Trạng thái` *(giá trị mặc định là Chưa xử lý)*
2. **Tài khoản n8n:** Bản Self-hosted (Docker) hoặc n8n Cloud.
3. **Thông tin xác thực (Credentials) cần kết nối trong n8n:**
   - **Google Sheets OAuth2 API:** Quyền đọc và cập nhật dữ liệu bảng tính.
   - **OpenAI API Key:** Cần có API key hợp lệ để cấp cho node `OpenAI Chat Model`.
   - **Gmail OAuth2 API:** Quyền gửi email qua tài khoản Gmail.

---

## 🚀 Hướng dẫn cài đặt và sử dụng (Setup Guide)

### Bước 1: Nhập Workflow vào n8n
1. Tải file workflow dạng `.json` từ repository này về máy tính (hoặc sao chép toàn bộ nội dung file JSON).
2. Mở n8n của bạn:
   - **Cách 1:** Bấm vào biểu tượng dấu 3 chấm `...` ở menu góc trên bên trái -> Chọn **Import from File** -> Chọn file vừa tải.
   - **Cách 2:** Mở trang soạn thảo n8n trống và nhấn tổ hợp phím `Ctrl + V` (hoặc `Cmd + V` trên macOS) để dán trực tiếp.

### Bước 2: Cấu hình lại thông tin kết nối (Credentials)
Sau khi import thành công, mở từng node để liên kết lại tài khoản của bạn:
- **Node `Đọc phản hồi từ sheet` & `Ghi nhãn và đánh dấu đã gửi`:** Chọn tài khoản Google Sheets của bạn, chọn đúng ID file trang tính và Sheet con chứa dữ liệu.
- **Node `OpenAI Chat Model`:** Chọn tài khoản OpenAI và dán OpenAI API Key của bạn.
- **Node `Gửi email phản hồi`:** Chọn kết nối Gmail của bạn.

### Bước 3: Kiểm thử và Kích hoạt
1. Nhấp vào nút **Execute Workflow** (Thực thi) ở thanh dưới cùng để chạy thử nghiệm một lượt.
2. Kiểm tra email phản hồi nhận được và kiểm tra cột trạng thái trên Google Sheets xem đã được cập nhật nhãn chưa.
3. Gạt công tắc chuyển trạng thái ở góc trên bên phải sang **Published** để hệ thống tự động chạy ngầm mỗi 2 phút.
