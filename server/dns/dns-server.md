# Cài đặt DNS Server Bind9

## Bước 1: Cài đặt dịch vụ Bind9

Trên máy Ubuntu, chạy lệnh cài đặt gói dịch vụ DNS:
```bash
sudo apt update
sudo apt install bind9 bind9utils bind9-doc -y
```
## Bước 2: Cấu hình Options (Cho phép truy vấn)

Cấu hình để DNS Server này trả lời các truy vấn từ mạng LAN và máy thật.

Mở file cấu hình:
```bash
sudo nano /etc/bind/named.conf.options
```
Chỉnh sửa nội dung:

```bash
options {
        directory "/var/cache/bind";

        recursion yes;

        allow-query { any; };

        forwarders {
                8.8.8.8;
        };
};
``` 
## Bước 3: Khai báo Zone (Tên miền của bạn)

Giả sử tên miền là: enterprise.local

Mở file:
```bash
sudo nano /etc/bind/named.conf.local
```
Thêm đoạn cấu hình sau:
```bash
zone "enterprise.local" {
    type master;
    file "/etc/bind/db.enterprise.local";
};
```
## Bước 4: Tạo file dữ liệu cho Tên miền

Đây là nơi ánh xạ tên miền vào IP 172.16.10.10.

Tạo file mới từ file mẫu:
```bash
sudo cp /etc/bind/db.local /etc/bind/db.enterprise.local
```
Mở file:

sudo nano /etc/bind/db.enterprise.local

Chỉnh sửa nội dung thành:
```bash
;
; BIND data file for enterprise.local
;
$TTL    604800
@       IN      SOA     ns1.enterprise.local. admin.enterprise.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;

@       IN      NS      ns1.enterprise.local.
@       IN      A       172.16.10.10
ns1     IN      A       172.16.10.10
www     IN      A       172.16.10.10
```
## Bước 5: Kiểm tra và Khởi động lại

Kiểm tra lỗi cú pháp:
```bash
sudo named-checkconf
```
Khởi động lại dịch vụ:
```bash
sudo systemctl restart bind9
```
Kiểm tra trạng thái dịch vụ:
```bash
systemctl status bind9
```
## Bước 6: Thử nghiệm từ Máy thật

Để máy thật hiểu được tên miền này, có 2 cách:

### Cách 1 (Chuẩn)

Đổi DNS trên máy thật thành IP cổng port1 của FortiGate.

Lưu ý:
- Tạo VIP cho Port 53 UDP/TCP
- Tạo Firewall Policy cho DNS

### Cách 2 (Nhanh)

Sửa file hosts trên Windows.

Đường dẫn:
```bash
C:\Windows\System32\drivers\etc\hosts
```
Thêm dòng:
```bash
10.255.0.2  www.enterprise.local
```
## Kiểm tra phân giải DNS

Dùng lệnh:
```bash
nslookup www.enterprise.local
```
Hoặc:
```bash
ping www.enterprise.local
```
Nếu trả về IP:
```bash
172.16.10.10
```
