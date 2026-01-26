# JMeter Performance Testing - Hướng Dẫn Đầy Đủ

## 📋 Mục Lục

1. [Giới Thiệu](#giới-thiệu)
2. [Cài Đặt JMeter](#cài-đặt-jmeter)
3. [Tổng Quan Test Plan](#tổng-quan-test-plan)
4. [Kịch Bản Kiểm Thử](#kịch-bản-kiểm-thử)
5. [Hướng Dẫn Chạy Test](#hướng-dẫn-chạy-test)
6. [Thu Thập và Phân Tích Kết Quả](#thu-thập-và-phân-tích-kết-quả)
7. [Troubleshooting](#troubleshooting)

---

## Giới Thiệu

Thư mục này chứa test plan JMeter để kiểm thử hiệu năng website Reddit (hoặc website khác bạn chọn). Test plan bao gồm 3 Thread Groups mô phỏng các kịch bản tải khác nhau.

### Files Quan Trọng

- **`reddit-performance-test.jmx`** - JMeter test plan chính
- **`REPORT_TEMPLATE.md`** - Template để viết báo cáo kết quả
- **`results/`** - Thư mục lưu kết quả test (CSV và screenshots)

---

## Cài Đặt JMeter

### Bước 1: Cài Đặt Java

JMeter yêu cầu Java 8 trở lên. Kiểm tra phiên bản Java:

```bash
java -version
```

Nếu chưa có Java, tải và cài đặt từ:

- **Oracle JDK:** https://www.oracle.com/java/technologies/downloads/
- **OpenJDK:** https://adoptium.net/

### Bước 2: Tải JMeter

1. Truy cập: https://jmeter.apache.org/download_jmeter.cgi
2. Tải file **apache-jmeter-5.6.3.zip** (hoặc phiên bản mới nhất)
3. Giải nén vào thư mục dễ truy cập (ví dụ: `C:\JMeter` hoặc `~/JMeter`)

### Bước 3: Chạy JMeter

**Windows:**

```powershell
cd C:\JMeter\apache-jmeter-5.6.3\bin
.\jmeter.bat
```

**Linux/Mac:**

```bash
cd ~/JMeter/apache-jmeter-5.6.3/bin
./jmeter.sh
```

JMeter GUI sẽ mở ra.

---

## Tổng Quan Test Plan

Test plan `reddit-performance-test.jmx` đã được cấu hình sẵn với cấu trúc sau:

```
Reddit Performance Test Plan
├── HTTP Request Defaults (www.reddit.com, HTTPS:443)
├── HTTP Header Manager (User-Agent, Accept headers)
├── Thread Group 1 - Basic Scenario (10 users, 5 loops)
│   ├── GET - Reddit Homepage (/)
│   └── TG1 - CSV Results
├── Thread Group 2 - Heavy Load (50 users, 30s ramp-up)
│   ├── GET - Reddit Homepage (/)
│   ├── GET - /r/popular/
│   └── TG2 - CSV Results
├── Thread Group 3 - Custom Scenario (20 users, 60s duration)
│   ├── GET - /r/all/
│   ├── GET - /r/AskReddit/
│   └── TG3 - CSV Results
├── View Results Tree
├── Summary Report
├── Aggregate Report
└── Graph Results
```

---

## Kịch Bản Kiểm Thử

### Thread Group 1: Kịch Bản Cơ Bản

**Mục đích:** Kiểm thử hiệu năng cơ bản với tải nhẹ, thiết lập baseline metrics

**Cấu hình:**

- **Số lượng người dùng (Threads):** 10
- **Ramp-up Period:** 1 giây
- **Loop Count:** 5 lần lặp
- **Tổng số requests:** 50 (10 users × 5 loops)

**Hành vi:**

- Gửi HTTP GET request đến trang chủ Reddit (`/`)

**Kết quả mong đợi:**

- Response time: < 500ms (trong điều kiện tốt)
- Error rate: 0%
- Throughput ổn định

---

### Thread Group 2: Kịch Bản Tải Nặng

**Mục đích:** Kiểm thử khả năng chịu tải, phát hiện bottleneck khi có nhiều người dùng đồng thời

**Cấu hình:**

- **Số lượng người dùng (Threads):** 50
- **Ramp-up Period:** 30 giây (tăng dần tải)
- **Loop Count:** 1 lần
- **Tổng số requests:** 100 (50 users × 2 requests)

**Hành vi:**

- HTTP GET request đến trang chủ (`/`)
- HTTP GET request đến `/r/popular/`

**Kết quả mong đợi:**

- Response time có thể tăng so với TG1
- Có thể xuất hiện lỗi nếu vượt quá giới hạn
- Throughput cao hơn TG1

---

### Thread Group 3: Kịch Bản Tùy Chỉnh

**Mục đích:** Kiểm thử hành vi phức tạp, mô phỏng traffic liên tục trong thời gian dài

**Cấu hình:**

- **Số lượng người dùng (Threads):** 20
- **Ramp-up Period:** 10 giây
- **Duration:** 60 giây
- **Loop Count:** Forever (chạy theo thời gian)

**Hành vi:**

- HTTP GET request đến `/r/all/`
- HTTP GET request đến `/r/AskReddit/`

**Kết quả mong đợi:**

- Số lượng samples cao (vì chạy liên tục 60s)
- Đánh giá performance dưới sustained load
- Phát hiện memory leak hoặc resource issues

---

### So Sánh Các Kịch Bản

| Aspect             | TG1: Basic       | TG2: Heavy Load         | TG3: Custom                      |
| ------------------ | ---------------- | ----------------------- | -------------------------------- |
| **Users**          | 10               | 50                      | 20                               |
| **Complexity**     | Thấp (1 request) | Trung bình (2 requests) | Cao (2 requests, duration-based) |
| **Duration**       | ~1-2s            | ~30s                    | 60s                              |
| **Type**           | Loop-based       | Loop-based              | Duration-based                   |
| **Purpose**        | Baseline         | Stress test             | Sustained load                   |
| **Total Requests** | 50               | 100                     | ~240+ (tùy response time)        |

---

## Hướng Dẫn Chạy Test

### Bước 1: Mở JMeter và Load Test Plan

1. Chạy JMeter (xem phần [Cài Đặt JMeter](#cài-đặt-jmeter))
2. Trong JMeter GUI: **File → Open**
3. Chọn file: `reddit-performance-test.jmx`

### Bước 2: Kiểm Tra Cấu Hình

- Xem cây menu bên trái để đảm bảo tất cả Thread Groups và Listeners đã được load
- Kiểm tra HTTP Request Defaults đã đúng domain

### Bước 3: Chạy TỪNG Thread Group Riêng Lẻ

> ⚠️ **QUAN TRỌNG:** Chạy từng Thread Group một để tránh bị Reddit rate limiting!

#### Chạy Thread Group 1:

1. **Disable TG2 & TG3:**
   - Right-click "Thread Group 2 - Heavy Load" → **Disable**
   - Right-click "Thread Group 3 - Custom Scenario" → **Disable**

2. **Chạy test:**
   - Click nút ▶️ **Start** (hoặc `Ctrl+R`)
   - Chờ test hoàn thành (~5-10 giây)

3. **Xem kết quả:**
   - Click vào **"Summary Report"** trong cây menu
   - Ghi lại các metrics (hoặc chụp screenshot)

4. **Dừng và Clear:**
   - Click nút ⏹️ **Stop** (hoặc `Ctrl+.`)
   - **Run → Clear All** (hoặc icon chổi quét 🧹)

5. **Chụp screenshot:**
   - Chụp màn hình Summary Report
   - Lưu: `results/thread-group-1-summary.png`

#### Chạy Thread Group 2:

1. **Enable TG2, Disable TG1 & TG3:**
   - Right-click "Thread Group 1" → **Disable**
   - Right-click "Thread Group 2" → **Enable**
   - Right-click "Thread Group 3" → **Disable**

2. Lặp lại các bước như TG1
3. Chụp screenshot: `results/thread-group-2-summary.png`

#### Chạy Thread Group 3:

1. **Enable TG3, Disable TG1 & TG2:**
   - Disable TG1 & TG2
   - Enable TG3

2. Lặp lại các bước như TG1 (test sẽ chạy 60 giây)
3. Chụp screenshot: `results/thread-group-3-summary.png`

### Bước 4: Kiểm Tra File CSV

File CSV đã được tự động lưu trong thư mục `results/`:

- `thread-group-1-results.csv`
- `thread-group-2-results.csv`
- `thread-group-3-results.csv`

---

## Thu Thập và Phân Tích Kết Quả

### Metrics Cần Ghi Lại

Từ **Summary Report**, ghi lại các chỉ số sau cho mỗi Thread Group:

| Metric         | Cột trong Summary Report | Ý Nghĩa                       |
| -------------- | ------------------------ | ----------------------------- |
| **Samples**    | # Samples                | Tổng số requests              |
| **Average**    | Average (ms)             | Thời gian phản hồi trung bình |
| **Min**        | Min (ms)                 | Thời gian phản hồi nhỏ nhất   |
| **Max**        | Max (ms)                 | Thời gian phản hồi lớn nhất   |
| **Throughput** | Throughput (req/sec)     | Số requests/giây              |
| **Error %**    | Error %                  | Tỷ lệ lỗi                     |
| **KB/sec**     | KB/sec                   | Băng thông nhận được          |

### Đánh Giá Chất Lượng

| Metric            | Excellent      | Good    | Acceptable   | Poor     |
| ----------------- | -------------- | ------- | ------------ | -------- |
| **Response Time** | < 200ms        | < 500ms | < 1000ms     | > 1000ms |
| **Error Rate**    | 0%             | < 0.5%  | < 1%         | > 1%     |
| **Throughput**    | Cao và ổn định | Ổn định | Có biến động | Thấp     |

### Phân Tích Kết Quả

**So sánh giữa các Thread Groups:**

- Response time có tăng khi số users tăng không?
- Error rate có chấp nhận được không?
- Throughput có scale tốt không?

**Phát hiện vấn đề:**

- Error rate cao → Server không chịu được tải
- Response time tăng đột ngột → Bottleneck
- Throughput thấp → Giới hạn băng thông hoặc server

---

## Troubleshooting

### Lỗi Thường Gặp

#### 1. Connection Timeout / Refused

- Kiểm tra URL và protocol (http/https)
- Kiểm tra firewall/proxy
- Website có thể đang chặn requests

#### 2. HTTP 429 (Too Many Requests)

- **Nguyên nhân:** Rate limiting của website
- **Giải pháp:**
  - Chờ 5-10 phút trước khi chạy lại
  - Giảm số lượng threads
  - Thêm Timer delays giữa các requests

#### 3. HTTP 403 Forbidden

- Website chặn JMeter user-agent
- **Giải pháp:** Thêm HTTP Header Manager với User-Agent giống browser

#### 4. HTTP 404 Not Found

- Kiểm tra lại path của requests
- Đảm bảo URL tồn tại trên website

#### 5. JMeter Chạy Chậm / Lag

- **Giải pháp:**
  - Tắt "View Results Tree" khi chạy test chính thức
  - Tăng Java Heap Size: Edit `jmeter.bat`, sửa `set HEAP=-Xms1g -Xmx1g`
  - Chạy ở non-GUI mode (xem bên dưới)

### Chạy Test Ở Non-GUI Mode (Nâng Cao)

Để có hiệu năng tốt hơn, chạy JMeter không dùng giao diện:

```powershell
# Windows
cd C:\JMeter\apache-jmeter-5.6.3\bin
.\jmeter.bat -n -t reddit-performance-test.jmx -l results/all-results.jtl -e -o results/html-report

# Linux/Mac
./jmeter.sh -n -t reddit-performance-test.jmx -l results/all-results.jtl -e -o results/html-report
```

**Giải thích:**

- `-n` : non-GUI mode
- `-t` : test plan file
- `-l` : results file (.jtl)
- `-e -o` : tạo HTML dashboard report

---

## Best Practices

### ✅ Nên Làm

- Chạy từng Thread Group riêng lẻ trước khi chạy tất cả
- Bắt đầu với số users nhỏ (5-10) để verify setup
- Clear results (Run → Clear All) giữa các lần test
- Chờ 1-2 phút giữa các lần test
- Kiểm tra kết nối internet ổn định
- Chụp screenshots ngay sau khi test xong
- Ghi chú thời gian và điều kiện test

### ❌ Không Nên Làm

- Chạy tất cả Thread Groups cùng lúc lần đầu
- Test liên tục nhiều lần trong thời gian ngắn
- Tăng số threads quá cao (có thể bị chặn IP)
- Quên disable "View Results Tree" khi test lớn
- Test website production vào giờ cao điểm
- Test website không có permission

---

## Lưu Ý Quan Trọng

> ⚠️ **CẢNH BÁO**
>
> - **KHÔNG** test website không có quyền sở hữu hoặc cho phép
> - **KHÔNG** gửi quá nhiều requests (có thể vi phạm rate limiting hoặc DDoS)
> - **KHÔNG** test trên production server vào giờ cao điểm
> - Tuân thủ Terms of Service của website
> - Với Reddit, nên giữ test ở mức vừa phải vì có rate limiting nghiêm ngặt

---

## Checklist Hoàn Thành

- [ ] Đã cài đặt Java
- [ ] Đã tải và cài đặt JMeter
- [ ] Đã mở file test plan `reddit-performance-test.jmx`
- [ ] Đã kiểm tra cấu hình các Thread Groups
- [ ] Đã chạy Thread Group 1 và lưu screenshot
- [ ] Đã chạy Thread Group 2 và lưu screenshot
- [ ] Đã chạy Thread Group 3 và lưu screenshot
- [ ] Đã kiểm tra 3 file CSV trong thư mục results
- [ ] Đã ghi lại metrics vào báo cáo (xem `REPORT_TEMPLATE.md`)
- [ ] Đã viết nhận xét và phân tích
- [ ] Đã hoàn thành phần kết luận

---

## Tài Liệu Tham Khảo

- **JMeter User Manual:** https://jmeter.apache.org/usermanual/
- **Best Practices:** https://jmeter.apache.org/usermanual/best-practices.html
- **JMeter Plugins:** https://jmeter-plugins.org/
- **Reddit API Guidelines:** https://www.reddit.com/wiki/api

---

**Chúc bạn thực hiện thành công! 🚀**

> **Ghi chú:** Sau khi chạy xong test, sử dụng file `REPORT_TEMPLATE.md` để viết báo cáo kết quả đầy đủ.
