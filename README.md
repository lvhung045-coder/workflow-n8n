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
