# Kịch Bản Kiểm Thử Chi Tiết

Tài liệu này mô tả chi tiết 3 kịch bản kiểm thử (Thread Groups) cho bài tập JMeter.

---

## Thread Group 1: Kịch Bản Cơ Bản

### Mục Đích

- Kiểm thử hiệu năng cơ bản của website với tải nhẹ
- Thiết lập baseline performance metrics
- Xác định thời gian phản hồi trung bình trong điều kiện bình thường

### Cấu Hình Thread Group

| Parameter             | Value    | Giải Thích                             |
| --------------------- | -------- | -------------------------------------- |
| **Number of Threads** | 10       | 10 người dùng ảo đồng thời             |
| **Ramp-up Period**    | 1 second | Tất cả 10 users khởi động trong 1 giây |
| **Loop Count**        | 5        | Mỗi user lặp lại 5 lần                 |
| **Tổng Requests**     | 50       | 10 × 5 = 50 requests                   |

### HTTP Requests

#### Request 1: Homepage

- **Type:** HTTP Request Sampler
- **Name:** `Homepage Request`
- **Method:** GET
- **Path:** `/`
- **Description:** Gửi GET request đến trang chủ của website

### Kịch Bản Hành Vi Người Dùng

```
User 1-10 → Truy cập trang chủ 5 lần
```

Mô phỏng 10 người dùng cùng lúc reload trang chủ nhiều lần.

### Kết Quả Mong Đợi

- Response time: < 500ms (trong điều kiện tốt)
- Error rate: 0%
- Throughput: Ổn định

---

## Thread Group 2: Kịch Bản Tải Nặng

### Mục Đích

- Kiểm thử khả năng chịu tải của website
- Đánh giá hiệu năng khi có nhiều người dùng đồng thời
- Phát hiện bottleneck và giới hạn của server
- Test nhiều endpoints khác nhau

### Cấu Hình Thread Group

| Parameter             | Value      | Giải Thích                             |
| --------------------- | ---------- | -------------------------------------- |
| **Number of Threads** | 50         | 50 người dùng ảo đồng thời             |
| **Ramp-up Period**    | 30 seconds | Tăng dần từ 0 → 50 users trong 30 giây |
| **Loop Count**        | 1          | Mỗi user chạy 1 lần                    |
| **Tổng Requests**     | 100        | 50 users × 2 requests = 100 requests   |

**Giải thích Ramp-up:**

- Giây 0: 0 users
- Giây 10: ~17 users
- Giây 20: ~33 users
- Giây 30: 50 users
- Mô phỏng tải tăng dần một cách tự nhiên

### HTTP Requests

#### Request 1: Homepage

- **Type:** HTTP Request Sampler
- **Name:** `Homepage Request`
- **Method:** GET
- **Path:** `/`
- **Description:** Truy cập trang chủ

#### Request 2: Subpage

- **Type:** HTTP Request Sampler
- **Name:** `Subpage Request`
- **Method:** GET
- **Path:** `[Tự chọn]`
- **Examples:**
  - `/about` - Trang giới thiệu
  - `/search?q=test` - Trang tìm kiếm
  - `/products` - Trang sản phẩm
  - `/blog/article-123` - Một bài viết cụ thể
  - `/category/news` - Trang danh mục

> **Lưu ý:** Chọn một trang con thực tế có trên website bạn test. Kiểm tra bằng cách mở browser và truy cập URL đó trước.

### Kịch Bản Hành Vi Người Dùng

```
User 1 → Homepage → Subpage
User 2 → Homepage → Subpage
...
User 50 → Homepage → Subpage
```

Mỗi user tải 2 trang (homepage + 1 subpage), tổng cộng 50 users.

### Kết Quả Mong Đợi

- Response time có thể tăng so với Thread Group 1
- Có thể xuất hiện lỗi nếu server không chịu được tải
- Throughput cao hơn Thread Group 1
- Phát hiện giới hạn của website

---

## Thread Group 3: Kịch Bản Tùy Chỉnh

### Mục Đích

- Kiểm thử hành vi phức tạp hơn của người dùng thực
- Test các loại request khác nhau (POST hoặc nhiều GET)
- Đánh giá hiệu năng trong thời gian dài (duration-based)
- Mô phỏng traffic liên tục

### Cấu Hình Thread Group

| Parameter             | Value              | Giải Thích                             |
| --------------------- | ------------------ | -------------------------------------- |
| **Number of Threads** | 20                 | 20 người dùng ảo đồng thời             |
| **Ramp-up Period**    | 10 seconds         | Tăng dần từ 0 → 20 users trong 10 giây |
| **Loop Count**        | Forever (Infinite) | Lặp vô hạn                             |
| **Duration**          | 60 seconds         | Chạy trong 60 giây                     |

**Giải thích:**

- 20 users sẽ được khởi động dần trong 10 giây đầu
- Từ giây 10-60, tất cả 20 users chạy liên tục
- Mỗi user lặp lại các requests cho đến khi hết 60 giây
- Tổng số requests phụ thuộc vào tốc độ phản hồi của server

### HTTP Requests - Option A: POST Request

**Chọn option này nếu website hỗ trợ POST (ví dụ: form tìm kiếm, đăng nhập, submit)**

#### Request 1: Search POST

- **Type:** HTTP Request Sampler
- **Name:** `Search POST Request`
- **Method:** POST
- **Path:** `/search` (hoặc URL của form)
- **Parameters:**
  - Name: `q` (hoặc tên parameter thực tế)
  - Value: `test query`

**Cách tìm parameters:**

1. Mở website trong browser
2. Mở Developer Tools (F12)
3. Vào tab **Network**
4. Submit form (search, login, etc.)
5. Click vào request vừa gửi
6. Xem tab **Payload** hoặc **Request** để thấy parameters

#### Request 2: GET Subpage

- **Method:** GET
- **Path:** `[Tự chọn]` - Ví dụ: `/results`, `/dashboard`

### HTTP Requests - Option B: Multiple GET Requests

**Chọn option này nếu website không có form POST hoặc không muốn test POST**

#### Request 1: Subpage 1

- **Type:** HTTP Request Sampler
- **Name:** `Subpage 1 Request`
- **Method:** GET
- **Path:** `[Tự chọn 1]`
- **Examples:**
  - `/about`
  - `/products`
  - `/contact`

#### Request 2: Subpage 2

- **Type:** HTTP Request Sampler
- **Name:** `Subpage 2 Request`
- **Method:** GET
- **Path:** `[Tự chọn 2]`
- **Examples:**
  - `/blog`
  - `/faq`
  - `/services`

> **Lưu ý:** Chọn 2 trang con khác nhau để test nhiều endpoints.

### Kịch Bản Hành Vi Người Dùng

**Option A (POST):**

```
User 1-20 → [Repeat for 60s] → POST Search → GET Results
```

**Option B (GET):**

```
User 1-20 → [Repeat for 60s] → GET Subpage1 → GET Subpage2
```

Mỗi user lặp lại chu trình này trong 60 giây.

### Kết Quả Mong Đợi

- Số lượng samples cao hơn (vì chạy liên tục 60s)
- Response time trung bình, đánh giá performance dưới tải sustained
- Có thể phát hiện memory leak hoặc vấn đề về resource nếu có

---

## So Sánh Các Thread Groups

| Aspect             | TG1: Basic       | TG2: Heavy Load         | TG3: Custom                       |
| ------------------ | ---------------- | ----------------------- | --------------------------------- |
| **Users**          | 10               | 50                      | 20                                |
| **Complexity**     | Thấp (1 request) | Trung bình (2 requests) | Cao (2 requests, duration-based)  |
| **Duration**       | ~1-2s            | ~30s                    | 60s                               |
| **Type**           | Loop-based       | Loop-based              | Duration-based                    |
| **Purpose**        | Baseline         | Stress test             | Sustained load + Complex behavior |
| **Total Requests** | 50               | 100                     | ~240+ (tùy response time)         |

---

## Cấu Hình Chung (Cho Cả 3 Thread Groups)

### HTTP Request Defaults

Thêm **HTTP Request Defaults** vào Test Plan để không phải lặp lại cấu hình:

```
Server Name or IP: [domain] (ví dụ: example.com)
Port: 443 (HTTPS) hoặc 80 (HTTP)
Protocol: https hoặc http
```

### Listeners Đề Xuất

1. **View Results Tree** - Xem chi tiết từng request/response
2. **Summary Report** - Xem tổng hợp kết quả
3. **Aggregate Report** - Xem thống kê chi tiết
4. **Graph Results** (Optional) - Xem biểu đồ

---

## Tùy Chỉnh Nâng Cao (Optional)

### Thêm Timers (Think Time)

Để mô phỏng thực tế hơn, thêm delays giữa các requests:

1. Click chuột phải vào **Thread Group**
2. **Add** → **Timer** → **Constant Timer** hoặc **Uniform Random Timer**
3. Cấu hình delay (ví dụ: 1000ms = 1 giây)

### Thêm Assertions

Kiểm tra response có đúng không:

1. Click chuột phải vào **HTTP Request**
2. **Add** → **Assertions** → **Response Assertion**
3. Cấu hình:
   - **Response Code:** `200`
   - **Response Text:** Contains `[keyword]`

### Thêm HTTP Header Manager

Giả lập browser thực:

1. **Add** → **Config Element** → **HTTP Header Manager**
2. Thêm headers:
   - `User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36`
   - `Accept: text/html,application/xhtml+xml`

---

## Lưu Ý Khi Thực Hiện

### ⚠️ Cảnh Báo

- **Không** test website production vào giờ cao điểm
- **Không** gửi quá nhiều requests (tránh DDoS)
- **Luôn** có permission từ chủ sở hữu website
- **Tuân thủ** Terms of Service và robots.txt

### ✅ Best Practices

1. **Test từng Thread Group riêng lẻ** trước khi chạy tất cả
2. **Bắt đầu với số users nhỏ** (5-10) để verify setup
3. **Tăng dần** số lượng users để tìm giới hạn
4. **Chạy nhiều lần** để đảm bảo kết quả nhất quán
5. **So sánh kết quả** giữa các lần chạy
6. **Ghi chép đầy đủ** trong report

---

## Checklist Trước Khi Chạy

- [ ] Đã chọn website để test
- [ ] Đã cài đặt JMeter và Java
- [ ] Đã tạo HTTP Request Defaults
- [ ] Đã cấu hình Thread Group 1 (10 users, 5 loops)
- [ ] Đã cấu hình Thread Group 2 (50 users, 30s ramp-up)
- [ ] Đã cấu hình Thread Group 3 (20 users, 60s duration)
- [ ] Đã thêm Listeners (Summary Report, View Results Tree)
- [ ] Đã lưu Test Plan (.jmx)
- [ ] Đã tạo thư mục `results/` để lưu kết quả
- [ ] Đã verify tất cả URLs tồn tại và accessible

---

**Sẵn sàng để bắt đầu kiểm thử! 🚀**
