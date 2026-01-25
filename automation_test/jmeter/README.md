# Báo Cáo Kiểm Thử Hiệu Năng với JMeter

## 1. Thông Tin Chung

**Người thực hiện:** [Tên sinh viên]  
**Ngày thực hiện:** [Ngày/tháng/năm]  
**Website kiểm thử:** [URL của website]

## 2. Mục Tiêu Kiểm Thử

Mục tiêu của bài kiểm thử này là:

- Đánh giá hiệu năng của website dưới các mức tải khác nhau
- Xác định thời gian phản hồi (Response Time) của server
- Đo lường khả năng xử lý yêu cầu (Throughput)
- Phát hiện lỗi và điểm nghẽn hiệu năng

## 3. Môi Trường Kiểm Thử

**Thông tin hệ thống:**

- **JMeter version:** [Ví dụ: 5.6.3]
- **Java version:** [Ví dụ: Java 11]
- **Hệ điều hành:** [Ví dụ: Windows 11]
- **Cấu hình máy:** [RAM, CPU]

**Website được chọn:**

- **URL:** [URL đầy đủ]
- **Lý do chọn:** [Ví dụ: Website của dự án nhóm / Website công khai / etc.]

## 4. Kịch Bản Kiểm Thử

### Thread Group 1: Kịch Bản Cơ Bản

**Cấu hình:**

- **Số lượng người dùng (Threads):** 10
- **Ramp-up Period:** 1 giây
- **Loop Count:** 5 lần lặp
- **Tổng số requests:** 50 (10 users × 5 loops)

**Hành vi:**

- Gửi HTTP GET request đến trang chủ của website

**Mục đích:**

- Kiểm thử hiệu năng cơ bản với tải nhẹ
- Thiết lập baseline cho các kịch bản phức tạp hơn

---

### Thread Group 2: Kịch Bản Tải Nặng

**Cấu hình:**

- **Số lượng người dùng (Threads):** 50
- **Ramp-up Period:** 30 giây
- **Loop Count:** 1 lần (hoặc Infinite với Duration)
- **Tổng thời gian chạy:** ~30-60 giây

**Hành vi:**

- HTTP GET request đến trang chủ
- HTTP GET request đến 1 trang con: [Tên trang con, ví dụ: /search, /about, /article/123]

**Mục đích:**

- Kiểm thử khả năng chịu tải của website
- Mô phỏng nhiều người dùng truy cập đồng thời
- Đánh giá hiệu năng khi có nhiều requests đến các trang khác nhau

---

### Thread Group 3: Kịch Bản Tùy Chỉnh

**Cấu hình:**

- **Số lượng người dùng (Threads):** 20
- **Ramp-up Period:** 10 giây
- **Duration:** 60 giây
- **Loop Count:** Forever (chạy theo thời gian)

**Hành vi:**

- **Option 1 (nếu website hỗ trợ POST):**
  - HTTP POST request đến form tìm kiếm hoặc form đăng nhập
  - Các tham số: [Liệt kê các parameters]
- **Option 2 (nếu chỉ dùng GET):**
  - HTTP GET request đến trang con thứ nhất: [URL]
  - HTTP GET request đến trang con thứ hai: [URL]

**Mục đích:**

- Kiểm thử hành vi phức tạp hơn của người dùng thực
- Test các endpoint khác nhau
- Đánh giá hiệu năng với requests POST (nếu có)

## 5. Cấu Hình JMeter

### HTTP Request Defaults

```
Server Name or IP: [domain của website]
Port: 443 (HTTPS) hoặc 80 (HTTP)
Protocol: https hoặc http
```

### Listeners Sử Dụng

- **View Results Tree:** Xem chi tiết từng request/response
- **Summary Report:** Xem tổng hợp kết quả
- **Aggregate Report:** Xem các chỉ số thống kê chi tiết
- **Graph Results:** (Optional) Hiển thị biểu đồ

## 6. Kết Quả Kiểm Thử

### Thread Group 1: Kịch Bản Cơ Bản

| Metric                    | Value                      |
| ------------------------- | -------------------------- |
| **Số samples**            | [Ví dụ: 50]                |
| **Average Response Time** | [Ví dụ: 245 ms]            |
| **Min Response Time**     | [Ví dụ: 180 ms]            |
| **Max Response Time**     | [Ví dụ: 520 ms]            |
| **Throughput**            | [Ví dụ: 15.2 requests/sec] |
| **Error Rate**            | [Ví dụ: 0.00%]             |
| **Received KB/sec**       | [Ví dụ: 125.3 KB/sec]      |

**Screenshots:**

![Thread Group 1 - Summary Report](results/thread-group-1-summary.png)

**Nhận xét:**

- [Phân tích kết quả: Response time có chấp nhận được không? Có lỗi không?]
- [Ví dụ: Website phản hồi tốt với tải nhẹ, thời gian phản hồi trung bình dưới 300ms]

---

### Thread Group 2: Kịch Bản Tải Nặng

| Metric                    | Value                      |
| ------------------------- | -------------------------- |
| **Số samples**            | [Ví dụ: 100]               |
| **Average Response Time** | [Ví dụ: 850 ms]            |
| **Min Response Time**     | [Ví dụ: 320 ms]            |
| **Max Response Time**     | [Ví dụ: 2150 ms]           |
| **Throughput**            | [Ví dụ: 12.8 requests/sec] |
| **Error Rate**            | [Ví dụ: 2.00%]             |
| **Received KB/sec**       | [Ví dụ: 98.5 KB/sec]       |

**Screenshots:**

![Thread Group 2 - Summary Report](results/thread-group-2-summary.png)

**Nhận xét:**

- [Phân tích: Website có chịu được tải nặng không?]
- [Ví dụ: Thời gian phản hồi tăng đáng kể khi số người dùng tăng lên 50. Xuất hiện 2% lỗi timeout]

---

### Thread Group 3: Kịch Bản Tùy Chỉnh

| Metric                    | Value                      |
| ------------------------- | -------------------------- |
| **Số samples**            | [Ví dụ: 240]               |
| **Average Response Time** | [Ví dụ: 450 ms]            |
| **Min Response Time**     | [Ví dụ: 200 ms]            |
| **Max Response Time**     | [Ví dụ: 1200 ms]           |
| **Throughput**            | [Ví dụ: 14.5 requests/sec] |
| **Error Rate**            | [Ví dụ: 0.50%]             |
| **Received KB/sec**       | [Ví dụ: 110.2 KB/sec]      |

**Screenshots:**

![Thread Group 3 - Summary Report](results/thread-group-3-summary.png)

**Nhận xét:**

- [Phân tích kết quả cho kịch bản tùy chỉnh]
- [Ví dụ: POST requests có thời gian phản hồi cao hơn GET requests]

## 7. File Kết Quả

Các file CSV và screenshots được lưu trong thư mục `results/`:

- `thread-group-1-results.csv` - Dữ liệu chi tiết Thread Group 1
- `thread-group-2-results.csv` - Dữ liệu chi tiết Thread Group 2
- `thread-group-3-results.csv` - Dữ liệu chi tiết Thread Group 3
- `thread-group-1-summary.png` - Screenshot Summary Report Thread Group 1
- `thread-group-2-summary.png` - Screenshot Summary Report Thread Group 2
- `thread-group-3-summary.png` - Screenshot Summary Report Thread Group 3

## 8. Phân Tích Tổng Hợp

### So Sánh Các Kịch Bản

| Thread Group     | Avg Response Time | Throughput | Error Rate |
| ---------------- | ----------------- | ---------- | ---------- |
| TG1 (Basic)      | [ms]              | [req/s]    | [%]        |
| TG2 (Heavy Load) | [ms]              | [req/s]    | [%]        |
| TG3 (Custom)     | [ms]              | [req/s]    | [%]        |

### Nhận Xét Chung

**Điểm mạnh:**

- [Ví dụ: Website xử lý tốt với tải nhẹ và trung bình]
- [Ví dụ: Tỷ lệ lỗi thấp trong hầu hết các trường hợp]

**Điểm yếu:**

- [Ví dụ: Thời gian phản hồi tăng đáng kể khi số người dùng > 40]
- [Ví dụ: Xuất hiện lỗi timeout khi tải nặng]

**Khuyến nghị:**

- [Ví dụ: Cần tối ưu hóa database queries để giảm response time]
- [Ví dụ: Cân nhắc sử dụng caching để cải thiện hiệu năng]
- [Ví dụ: Nâng cấp server resources nếu dự kiến có nhiều người dùng truy cập đồng thời]

## 9. Kết Luận

[Tóm tắt kết quả kiểm thử, đánh giá tổng thể về hiệu năng của website, và đưa ra các đề xuất cải thiện]

Ví dụ:

> Sau khi thực hiện các kịch bản kiểm thử, website cho thấy hiệu năng ổn định ở mức tải nhẹ và trung bình với thời gian phản hồi trung bình dưới 500ms. Tuy nhiên, khi số lượng người dùng đồng thời tăng lên trên 40, thời gian phản hồi tăng đáng kể và xuất hiện một số lỗi. Điều này cho thấy cần có các biện pháp tối ưu hóa để website có thể xử lý tốt hơn trong điều kiện tải cao.

---

## Phụ Lục

### File JMeter Test Plan

- File: `performance-test.jmx`
- Đường dẫn: `automation_test/jmeter/performance-test.jmx`

### Lưu Ý Khi Thực Hiện

- Tuân thủ chính sách sử dụng của website
- Không gửi quá nhiều requests để tránh bị chặn (rate limiting)
- Thực hiện kiểm thử vào giờ thấp điểm nếu test trên production
- Luôn có sự cho phép từ chủ sở hữu website trước khi test
