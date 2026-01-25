# Hướng Dẫn Cài Đặt và Sử Dụng JMeter

Tài liệu này hướng dẫn chi tiết cách cài đặt JMeter, tạo Test Plan, và cấu hình các kịch bản kiểm thử hiệu năng.

## Bước 1: Cài Đặt JMeter

### 1.1. Cài Đặt Java

JMeter yêu cầu Java 8 trở lên. Kiểm tra phiên bản Java:

```bash
java -version
```

Nếu chưa có Java, tải và cài đặt từ:

- **Oracle JDK:** https://www.oracle.com/java/technologies/downloads/
- **OpenJDK:** https://adoptium.net/

### 1.2. Tải JMeter

1. Truy cập: https://jmeter.apache.org/download_jmeter.cgi
2. Tải file ZIP hoặc TGZ (ví dụ: `apache-jmeter-5.6.3.zip`)
3. Giải nén vào thư mục bất kỳ (ví dụ: `C:\JMeter` hoặc `~/JMeter`)

### 1.3. Chạy JMeter

**Windows:**

```bash
cd C:\JMeter\apache-jmeter-5.6.3\bin
jmeter.bat
```

**Linux/Mac:**

```bash
cd ~/JMeter/apache-jmeter-5.6.3/bin
./jmeter.sh
```

JMeter GUI sẽ mở ra.

---

## Bước 2: Tạo Test Plan Mới

1. Mở JMeter
2. Bạn sẽ thấy một **Test Plan** mặc định
3. Click chuột phải vào **Test Plan** → chọn tên: `Performance Test - [Tên Website]`

---

## Bước 3: Cấu Hình HTTP Request Defaults

Để tránh lặp lại URL cho mỗi request, ta sử dụng **HTTP Request Defaults**.

1. Click chuột phải vào **Test Plan**
2. **Add** → **Config Element** → **HTTP Request Defaults**
3. Cấu hình:
   - **Server Name or IP:** `example.com` (không có http:// hoặc https://)
   - **Port Number:** `443` (cho HTTPS) hoặc `80` (cho HTTP)
   - **Protocol:** `https` hoặc `http`

> **Lưu ý:** Thay `example.com` bằng domain của website bạn muốn test.

---

## Bước 4: Tạo Thread Group 1 - Kịch Bản Cơ Bản

### 4.1. Tạo Thread Group

1. Click chuột phải vào **Test Plan**
2. **Add** → **Threads (Users)** → **Thread Group**
3. Đổi tên thành: `Thread Group 1 - Basic Scenario`

### 4.2. Cấu Hình Thread Group

Trong panel bên phải, cấu hình:

- **Number of Threads (users):** `10`
- **Ramp-up period (seconds):** `1`
- **Loop Count:** `5`

Giải thích:

- 10 người dùng ảo
- Khởi động tất cả trong 1 giây
- Mỗi người dùng lặp lại 5 lần → Tổng: 50 requests

### 4.3. Thêm HTTP Request

1. Click chuột phải vào **Thread Group 1**
2. **Add** → **Sampler** → **HTTP Request**
3. Đổi tên: `Homepage Request`
4. Cấu hình:
   - **Path:** `/` (trang chủ)
   - **Method:** `GET`

> Các thông tin Server/Protocol đã được cấu hình trong HTTP Request Defaults.

---

## Bước 5: Tạo Thread Group 2 - Kịch Bản Tải Nặng

### 5.1. Tạo Thread Group

1. Click chuột phải vào **Test Plan**
2. **Add** → **Threads (Users)** → **Thread Group**
3. Đổi tên: `Thread Group 2 - Heavy Load`

### 5.2. Cấu Hình Thread Group

- **Number of Threads (users):** `50`
- **Ramp-up period (seconds):** `30`
- **Loop Count:** `1`

Giải thích:

- 50 người dùng ảo
- Khởi động dần trong 30 giây (tăng dần tải)
- Mỗi user chạy 1 lần

### 5.3. Thêm HTTP Requests

#### Request 1: Homepage

1. Click chuột phải vào **Thread Group 2**
2. **Add** → **Sampler** → **HTTP Request**
3. Tên: `Homepage Request`
4. Path: `/`

#### Request 2: Subpage

1. Click chuột phải vào **Thread Group 2**
2. **Add** → **Sampler** → **HTTP Request**
3. Tên: `Subpage Request` (ví dụ: About Page)
4. Path: `/about` (hoặc bất kỳ trang con nào bạn chọn)

> **Lưu ý:** Chọn một trang con thực tế có trên website. Ví dụ: `/search`, `/contact`, `/products`, `/category/news`, v.v.

---

## Bước 6: Tạo Thread Group 3 - Kịch Bản Tùy Chỉnh

### 6.1. Tạo Thread Group

1. Click chuột phải vào **Test Plan**
2. **Add** → **Threads (Users)** → **Thread Group**
3. Đổi tên: `Thread Group 3 - Custom Scenario`

### 6.2. Cấu Hình Thread Group

- **Number of Threads (users):** `20`
- **Ramp-up period (seconds):** `10`
- **Loop Count:** Chọn **Forever** (hoặc **Infinite**)
- **Duration (seconds):** `60`

Giải thích:

- 20 người dùng
- Khởi động trong 10 giây
- Chạy liên tục trong 60 giây

### 6.3. Thêm HTTP Requests

Bạn có 2 lựa chọn:

#### **Option A: POST Request (nếu website hỗ trợ)**

1. **Add** → **Sampler** → **HTTP Request**
2. Tên: `Search POST Request`
3. Cấu hình:
   - **Method:** `POST`
   - **Path:** `/search` (hoặc URL của form)
   - **Parameters:** (Click Add button)
     - Name: `q` (hoặc tên parameter của search box)
     - Value: `test query`

> Để biết tên parameter, mở website, mở Developer Tools (F12), submit form và xem request trong tab Network.

#### **Option B: Multiple GET Requests**

1. **Add** → **Sampler** → **HTTP Request**
   - Tên: `Subpage 1 Request`
   - Path: `/page1` (ví dụ: `/products`)

2. **Add** → **Sampler** → **HTTP Request**
   - Tên: `Subpage 2 Request`
   - Path: `/page2` (ví dụ: `/blog`)

---

## Bước 7: Thêm Listeners để Thu Thập Kết Quả

Listeners giúp bạn xem và lưu kết quả kiểm thử.

### 7.1. View Results Tree

Hiển thị chi tiết từng request/response.

1. Click chuột phải vào **Thread Group** (có thể thêm vào từng Thread Group hoặc Test Plan để xem tất cả)
2. **Add** → **Listener** → **View Results Tree**

### 7.2. Summary Report

Hiển thị tổng hợp các chỉ số quan trọng.

1. Click chuột phải vào **Test Plan** (để xem tổng hợp tất cả)
2. **Add** → **Listener** → **Summary Report**

### 7.3. Aggregate Report (Tùy chọn)

Cung cấp thống kê chi tiết hơn.

1. Click chuột phải vào **Test Plan**
2. **Add** → **Listener** → **Aggregate Report**

### 7.4. Graph Results (Tùy chọn)

Hiển thị biểu đồ thời gian phản hồi.

1. Click chuột phải vào **Test Plan**
2. **Add** → **Listener** → **Graph Results**

---

## Bước 8: Lưu Test Plan

1. **File** → **Save Test Plan as...**
2. Lưu vào: `automation_test/jmeter/performance-test.jmx`

---

## Bước 9: Chạy Kiểm Thử

### 9.1. Chạy Từng Thread Group

**Cách 1: Disable các Thread Group không muốn chạy**

- Click chuột phải vào Thread Group → **Disable**
- Chỉ enable Thread Group muốn test

**Cách 2: Chạy tất cả**

- Nhấn nút **Start** (mũi tên xanh) trên toolbar
- Hoặc **Run** → **Start**

### 9.2. Theo Dõi Kết Quả

- Xem **View Results Tree** để thấy chi tiết từng request
- Xem **Summary Report** để thấy số liệu tổng hợp

---

## Bước 10: Lưu Kết Quả

### 10.1. Lưu CSV File

1. Trong **Summary Report** hoặc **Aggregate Report**
2. Click nút **Configure** (biểu tượng bánh răng)
3. Hoặc nhấn nút **Save Table Data**
4. Chọn vị trí lưu: `automation_test/jmeter/results/thread-group-X-results.csv`

### 10.2. Chụp Screenshots

1. Chạy từng Thread Group
2. Khi hoàn thành, xem **Summary Report**
3. Chụp màn hình (Windows: Win+Shift+S, Mac: Cmd+Shift+4)
4. Lưu vào: `automation_test/jmeter/results/thread-group-X-summary.png`

### 10.3. Cấu Hình Tự Động Lưu

Để tự động lưu kết quả vào file:

1. Click vào **Summary Report** listener
2. Tại mục **Filename** phía dưới, nhập đường dẫn:
   ```
   automation_test/jmeter/results/summary-report.csv
   ```
3. Sau khi chạy xong, file sẽ được lưu tự động

---

## Bước 11: Phân Tích Kết Quả

### Các Chỉ Số Quan Trọng

| Metric           | Ý Nghĩa                              | Giá Trị Tốt                              |
| ---------------- | ------------------------------------ | ---------------------------------------- |
| **# Samples**    | Tổng số requests                     | Phụ thuộc vào cấu hình                   |
| **Average (ms)** | Thời gian phản hồi trung bình        | < 500ms (tốt), < 1000ms (chấp nhận được) |
| **Min/Max (ms)** | Thời gian phản hồi nhỏ nhất/lớn nhất | Chênh lệch không quá lớn                 |
| **Std. Dev.**    | Độ lệch chuẩn                        | Càng thấp càng tốt (ổn định)             |
| **Error %**      | Tỷ lệ lỗi                            | 0% là tốt nhất, < 1% chấp nhận được      |
| **Throughput**   | Số requests/giây                     | Càng cao càng tốt                        |
| **KB/sec**       | Băng thông nhận được                 | Phụ thuộc vào nội dung page              |

### So Sánh Kết Quả

- So sánh **Average Response Time** giữa các Thread Groups
- Xem **Error %** để phát hiện vấn đề
- Kiểm tra **Throughput** để đánh giá khả năng xử lý

---

## Bước 12: Viết Báo Cáo

1. Mở file `README.md` trong thư mục `jmeter`
2. Điền thông tin vào các phần:
   - Website được chọn
   - Kết quả từng Thread Group (copy từ Summary Report)
   - Screenshots
   - Phân tích và nhận xét

---

## Lưu Ý Quan Trọng

> **⚠️ CẢNH BÁO**
>
> - **KHÔNG** test website không có quyền sở hữu hoặc cho phép
> - **KHÔNG** gửi quá nhiều requests (có thể vi phạm rate limiting)
> - **KHÔNG** test trên production server vào giờ cao điểm
> - Nếu test website công khai, giữ số lượng requests ở mức vừa phải
> - Tuân thủ Terms of Service của website

### Best Practices

1. **Test trên môi trường development/staging trước**
2. **Bắt đầu với số lượng threads nhỏ** (5-10) để đảm bảo setup đúng
3. **Tăng dần tải** để tìm điểm giới hạn
4. **Chạy test nhiều lần** để đảm bảo kết quả nhất quán
5. **Ghi chú lại mọi thay đổi** trong cấu hình

---

## Troubleshooting

### Lỗi Thường Gặp

**1. Connection refused / Timeout**

- Kiểm tra URL và protocol (http/https)
- Kiểm tra firewall/proxy
- Website có thể đang chặn requests

**2. 403 Forbidden**

- Website có thể chặn JMeter user-agent
- Thêm HTTP Header Manager:
  - Add → Config Element → HTTP Header Manager
  - Add header: `User-Agent: Mozilla/5.0 ...`

**3. 404 Not Found**

- Kiểm tra lại path của requests
- Đảm bảo URL tồn tại trên website

**4. Kết quả không hiển thị**

- Đảm bảo Listeners được enable
- Thử click nút "Clear" và chạy lại

---

## Tài Liệu Tham Khảo

- **JMeter User Manual:** https://jmeter.apache.org/usermanual/
- **Best Practices:** https://jmeter.apache.org/usermanual/best-practices.html
- **JMeter Plugins:** https://jmeter-plugins.org/

---

**Chúc bạn thực hiện thành công! 🚀**
