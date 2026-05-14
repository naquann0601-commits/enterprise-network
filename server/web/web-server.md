# Cài đặt Web Server

## Bước 1: Cài đặt dịch vụ Nginx

Sử dụng công cụ quản lý gói apt để cài đặt Nginx (được ưa chuộng hơn Apache vì hiệu năng cao và tiết kiệm tài nguyên trong môi trường ảo hóa):
```bash
sudo apt update
sudo apt install nginx -y
```
## Bước 2: Kiểm tra trạng thái dịch vụ

Sau khi cài đặt, bạn cần đảm bảo dịch vụ đã khởi chạy thành công:

- Lệnh kiểm tra:
```bash
systemctl status nginx
```
- Kỳ vọng:
Dòng trạng thái phải hiển thị: active (running)

## Bước 3: Cấu hình Firewall (UFW) trên Ubuntu

Nếu máy Ubuntu của bạn đang bật tường lửa nội bộ, bạn cần cho phép lưu lượng Web đi qua:
```bash
sudo ufw allow 'Nginx Full'
```
## Bước 4: Kiểm tra kết nối từ nội bộ DMZ

Mở trình duyệt trên máy Ubuntu Desktop (hoặc dùng lệnh curl nếu là bản Server) và truy cập vào:
```bash
http://localhost
```
hoặc
```bash
http://172.16.10.10
```
Nếu thấy trang:

Welcome to nginx!

=> Bạn đã cài đặt thành công ở mức local.

## Bước 5: Cấu hình Policy trên FortiGate (Quan trọng cho đồ án)

Để máy từ vùng LAN hoặc Outside có thể truy cập được Web Server này, bạn cần thực hiện trên FortiGate:

### 1. Virtual IP (DNAT)

Nếu muốn truy cập từ ngoài Internet, hãy tạo một VIP ánh xạ IP Public (port1) sang IP nội bộ 172.16.10.10.

### 2. Firewall Policy

- Incoming Interface:
Cổng nhận yêu cầu (ví dụ LAN hoặc port1)

- Outgoing Interface:
DMZ_VLAN99

- Source/Destination:
all hoặc cụ thể IP máy khách

- Service:
HTTP (TCP 80) và HTTPS (TCP 443)

(routing từ máy thật về firewall)

## Bước 6: Thay đổi nội dung trang Web (Tùy chỉnh cho Thesis)

Để trang web hiển thị nội dung đồ án của bạn, hãy chỉnh sửa file HTML mặc định:
```bash
sudo nano /var/www/html/index.html
```
Sau khi chỉnh sửa:
- Ctrl + O để lưu
- Ctrl + X để thoát

