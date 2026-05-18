# IP PLAN

## 1. Tổng quan hệ thống mạng

Hệ thống mạng được thiết kế theo mô hình Enterprise Campus Network gồm:

- Core Layer (DSW1, DSW2)
- Access Layer (ASW1 → ASW6)
- Firewall HA (FW-1, FW-2)
- Dual ISP Failover
- DMZ Zone
- Các VLAN cho từng phòng ban

---

# 2. Kế hoạch địa chỉ IP

| VLAN | Phòng Ban | Network | Gateway |
|------|------------|------------|------------|
| VLAN 10 | Phòng Kế Toán | 10.10.10.0/24 | 10.10.10.1 |
| VLAN 20 | Phòng Nhân Sự | 10.10.20.0/24 | 10.10.20.1 |
| VLAN 30 | Phòng Kỹ Thuật | 10.10.30.0/24 | 10.10.30.1 |
| VLAN 40 | Phòng Kinh Doanh | 10.10.40.0/24 | 10.10.40.1 |
| VLAN 50 | Ban Giám Đốc | 10.10.50.0/24 | 10.10.50.1 |
| VLAN 60 | Phòng IT | 10.10.60.0/24 | 10.10.60.1 |
| VLAN 99 | DMZ Web Server | 172.16.10.0/24 | 172.16.10.2 |

---

# 3. Địa chỉ IP WAN & Transit

## ISP-1

| Thiết Bị | Interface | IP Address |
|----------|------------|------------|
| ISP-1 | G1/0 | 203.10.10.1/30 |
| R1 | G1/0 | 203.10.10.2/30 |

---

## ISP-2

| Thiết Bị | Interface | IP Address |
|----------|------------|------------|
| ISP-2 | G1/0 | 198.51.100.1/30 |
| R2 | G1/0 | 198.51.100.2/30 |

---

## Kết nối Router ↔ Firewall

| Kết Nối | Network |
|----------|------------|
| R1 ↔ FW-1 | 10.255.0.0/30 |
| R2 ↔ FW-2 | 10.255.0.12/30 |

---

## Firewall HA / Sync

| Kết Nối | Network |
|----------|------------|
| FW-1 ↔ FW-2 HA | 10.255.255.0/30 |

---

## Core Network

| Kết Nối | Network |
|----------|------------|
| FW ↔ Distribution Switch | 10.254.0.0/24 |

---

# 4. DHCP Scope

| VLAN | DHCP Range |
|------|------------|
| VLAN 10 | 10.10.10.101 - 10.10.10.254 |
| VLAN 20 | 10.10.20.101 - 10.10.20.254 |
| VLAN 30 | 10.10.30.101 - 10.10.30.254 |
| VLAN 40 | 10.10.40.101 - 10.10.40.254 |
| VLAN 50 | 10.10.50.101 - 10.10.50.254 |
| VLAN 60 | 10.10.60.101 - 10.10.60.254 |

---

# 5. DMZ Zone

| Thiết Bị | IP Address | Dịch Vụ |
|----------|------------|------------|
| Web Server | 172.16.10.10 | HTTP/HTTPS |



