
# 🚀 Prometheus Blackbox Monitoring Setup

Tài liệu này hướng dẫn cách triển khai và chạy hệ thống monitoring sử dụng:

* Prometheus
* Blackbox Exporter
* Alertmanager

Hệ thống được dùng để kiểm tra trạng thái HTTP endpoint (kiểm tra phản hồi 2xx) và gửi cảnh báo khi có sự cố.

---

# 📦 1. Kiến trúc hệ thống

Hệ thống hoạt động theo mô hình:

* Prometheus gửi request đến Blackbox Exporter
* Blackbox Exporter thực hiện probe tới các website mục tiêu
* Prometheus lưu metrics và đánh giá alert rule
* Alertmanager nhận alert và gửi thông báo

Luồng hoạt động:

Prometheus → Blackbox Exporter → Website cần monitor
Prometheus → Alertmanager

---

# 🛠 2. Yêu cầu hệ thống

Trước khi chạy dự án cần chuẩn bị:

* Docker và Docker Compose (khuyến nghị)
  hoặc
* Cài đặt Prometheus, Blackbox Exporter và Alertmanager bằng binary trên Linux

Các port mặc định sử dụng:

* 9090 – Prometheus
* 9115 – Blackbox Exporter
* 9093 – Alertmanager

---

# 📂 3. Cấu trúc thư mục đề xuất

Thư mục dự án nên được tổ chức theo cấu trúc:

* Thư mục chứa file docker-compose
* Thư mục cấu hình Prometheus
* Thư mục cấu hình Blackbox
* Thư mục cấu hình Alertmanager

Việc tách riêng cấu hình giúp dễ bảo trì và quản lý.

---

# ⚙️ 4. Cấu hình hệ thống

Hệ thống cần các cấu hình sau:

* Prometheus:

  * Cấu hình scrape Blackbox Exporter
  * Cấu hình file chứa danh sách target
  * Cấu hình alert rules
  * Cấu hình kết nối Alertmanager

* Blackbox Exporter:

  * Cấu hình module kiểm tra HTTP 2xx
  * Thiết lập phương thức GET
  * Cho phép redirect nếu cần
  * Cấu hình IPv4 hoặc IPv6

* File danh sách target:

  * Danh sách các website cần monitor
  * Có thể phân nhóm bằng label

---

# 🐳 5. Chạy bằng Docker Compose (Khuyến nghị)

Các bước triển khai:

1. Chuẩn bị đầy đủ file cấu hình
2. Kiểm tra lại đường dẫn mount volume
3. Khởi động hệ thống bằng Docker Compose
4. Kiểm tra container đã chạy thành công

Sau khi khởi động, hệ thống sẽ tự động:

* Prometheus scrape mỗi 10 giây
* Blackbox thực hiện probe
* Alertmanager sẵn sàng nhận cảnh báo

---

# 🌐 6. Truy cập giao diện

Sau khi hệ thống chạy thành công, có thể truy cập:

* Giao diện Prometheus để xem metrics và target
* Trang metrics của Blackbox Exporter
* Giao diện Alertmanager để xem alert

---

# 📊 7. Kiểm tra Metrics

Trong giao diện Prometheus có thể:

* Kiểm tra trạng thái probe_success

  * Giá trị 1: Website hoạt động
  * Giá trị 0: Website không phản hồi

* Kiểm tra thời gian phản hồi qua probe_duration_seconds

---

# 🚨 8. Cấu hình Alert

Có thể tạo rule cảnh báo khi:

* Website down trong một khoảng thời gian nhất định
* Thời gian phản hồi vượt ngưỡng
* Lỗi SSL hoặc lỗi DNS

Alert sẽ được gửi đến Alertmanager để xử lý tiếp (email, Slack, Telegram…).

---

# 🔁 9. Reload hoặc Restart hệ thống

Sau khi chỉnh sửa cấu hình:

* Có thể restart container Prometheus
* Hoặc reload cấu hình nếu đã bật lifecycle

---

# 🧪 10. Debug khi không scrape được

Nếu target không hoạt động:

* Kiểm tra trang Targets trong Prometheus
* Kiểm tra DNS
* Kiểm tra SSL
* Kiểm tra firewall
* Kiểm tra network giữa container

---

# 🧹 11. Dừng hệ thống

Có thể dừng toàn bộ container khi không sử dụng.

---

# ✅ Kết quả sau khi triển khai

Sau khi hoàn tất, hệ thống sẽ:

* Monitor HTTP endpoint theo chu kỳ
* Phát hiện website down
* Gửi cảnh báo tự động
* Lưu trữ metrics phục vụ phân tích

---
