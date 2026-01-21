# KIỂM THỬ PHẦN MỀM – SOFTWARE TESTING

## 📌 Thông tin chung

- **Môn học:** Kiểm thử phần mềm
- **Sinh viên:** Hoàng Minh Thiện
- **MSSV:** BIT230390
- **Lớp:** 23IT2
- **Giảng viên:** Phạm Tuấn Anh
- **Học kỳ:** 8

Repo này dùng để **lưu trữ toàn bộ bài tập thực hành, tài liệu và báo cáo** trong suốt quá trình học môn Kiểm thử phần mềm.

## 🎯 Mục tiêu học phần

- Làm quen và **thực hành các công cụ kiểm thử phổ biến trong thực tế**
- Rèn luyện kỹ năng:
  - Viết test case, test plan
  - Thực hiện kiểm thử đơn vị, tích hợp, giao diện, hiệu năng, bảo mật
  - Quản lý lỗi và viết tài liệu kiểm thử
- Vận dụng kiến thức kiểm thử vào **dự án nhóm**
- Tiếp cận **công cụ kiểm thử hiện đại có ứng dụng mô hình ngôn ngữ lớn (LLM/AI)**

## 🧰 Các công cụ kiểm thử sử dụng

| Nhóm công cụ | Công cụ |
| ------------ | ------- |

---

## 📂 Cấu trúc thư mục

```
KiemThuPhanMem/
├── week-01/                    # Bài thực hành tuần 1 - UI Testing
│   └── Kết quả.png           # Ảnh kết quả
├── week-02/                    # Bài thực hành tuần 2 - Unit Testing
│   ├── student-analyzer/      # Dự án JUnit 5
│   │   ├── pom.xml
│   │   ├── README.md
│   │   └── src/
│   └── ...
├── week-03/                    # (Được lưu bên ngoài folder này)
│   └── automation_test/cypress-exercise/      # Dự án Cypress E2E Testing
│       ├── cypress/
│       │   ├── e2e/          # Test specifications
│       │   └── support/       # Support files
│       ├── cypress.config.js
│       ├── package.json
│       └── README.md
└── README.md                   # File này
```

---

## ✅ Kết quả thực hành

### 🗓️ Tuần 1: UI Testing

**Nội dung:** Kiểm thử giao diện người dùng

**Công cụ sử dụng:** [Can't Unsee](https://cantunsee.space/)

**Loại kiểm thử:** UI Testing (Kiểm thử giao diện người dùng)

**Điểm đạt được:** 6430 điểm

**Trình duyệt:** Google Chrome (đã đăng nhập, có dấu hiệu cá nhân)

📸 **Ảnh kết quả minh chứng:**  
[Click để xem ảnh kết quả Tuần 1](week-01/Kết%20quả.png)

---

### 🗓️ Tuần 2: Unit Testing với JUnit 5

**Nội dung:** Unit Testing - Phân tích dữ liệu điểm số học sinh

**Công nghệ:** Java, JUnit 5, Maven

**Loại kiểm thử:** Unit Testing (Kiểm thử đơn vị)

**Kết quả:**

- ✅ Tất cả test case **PASS**
- ✅ Maven build: **BUILD SUCCESS**

**📁 Chi tiết bài thực hành:**  
➡️ [Xem tại thư mục week-02/student-analyzer](week-02/student-analyzer)

---

### 🗓️ Tuần 3: End-to-End Testing với Cypress

**Nội dung:** Kiểm thử tự động E2E cho trang web SauceDemo

**Công nghệ:** Cypress, Node.js, JavaScript

**Loại kiểm thử:** End-to-End Testing (Kiểm thử đầu cuối)

**Website được test:** [SauceDemo](https://www.saucedemo.com)

**Kết quả:**

- ✅ Tổng số test specs: **3 files**
- ✅ Tổng số test cases: **6 tests**
- ✅ Kết quả: **Tất cả PASS (100%)**

**Các test scenarios đã thực hiện:**

1. **Kiểm thử Đăng nhập** (2 tests)
   - ✅ Đăng nhập thành công với thông tin hợp lệ
   - ✅ Hiển thị thông báo lỗi với thông tin không hợp lệ

2. **Kiểm thử Giỏ hàng** (3 tests)
   - ✅ Thêm sản phẩm vào giỏ hàng
   - ✅ Sắp xếp sản phẩm theo giá
   - ✅ Xóa sản phẩm khỏi giỏ hàng

3. **Kiểm thử Thanh toán** (1 test)
   - ✅ Hoàn thành quy trình thanh toán đầy đủ

**📁 Chi tiết bài thực hành:**  
➡️ [Xem tại thư mục cypress-exercise](../cypress-exercise)

## 📌 Ghi chú

Mỗi bài thực hành đều có:

- ✔️ Mã nguồn hoàn chỉnh
- ✔️ README mô tả chi tiết
- ✔️ Kết quả kiểm thử minh chứng
- ✔️ Commit và quản lý theo từng branch/tuần trên GitHub
