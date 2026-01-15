# Bài tập thực hành kiểm thử với JUnit

## Chủ đề: Phân tích dữ liệu điểm số học sinh

---

## 1. Mục tiêu học tập

- Làm quen với kiểm thử đơn vị (Unit Test) bằng JUnit 5
- Biết cách viết test case cho các phương thức Java
- Thực hành validate dữ liệu và xử lý các trường hợp biên
- Biết cách chạy kiểm thử bằng Maven
- Làm quen với quy trình làm việc với GitHub (commit, issue)

---

## 2. Mô tả bài toán

Xây dựng một chương trình Java dùng để phân tích điểm số học sinh (từ 0 đến 10), bao gồm:

- Đếm số lượng học sinh đạt loại **Giỏi** (điểm ≥ 8.0)
- Tính **điểm trung bình** của các điểm hợp lệ
- Bỏ qua các giá trị điểm không hợp lệ (nhỏ hơn 0 hoặc lớn hơn 10)
- Nếu danh sách rỗng hoặc không có điểm hợp lệ thì trả về 0

---

## 3. Công nghệ sử dụng

- Ngôn ngữ: Java
- Kiểm thử: JUnit 5
- Công cụ build: Maven
- IDE: Visual Studio Code

---

## 4. Cấu trúc thư mục

student-analyzer
│
├── pom.xml
│
├── src
│ ├── main
│ │ └── java
│ │ └── com
│ │ └── example
│ │ └── StudentAnalyzer.java
│ │
│ └── test
│ └── java
│ └── com
│ └── example
│ └── StudentAnalyzerTest.java
│
└── README.md

---

## 5. Mô tả các lớp

### 5.1. Lớp StudentAnalyzer

Chứa các phương thức xử lý nghiệp vụ:

- `countExcellentStudents(List<Double> scores)`  
  Đếm số học sinh có điểm ≥ 8.0 (chỉ tính các điểm hợp lệ).

- `calculateValidAverage(List<Double> scores)`  
  Tính điểm trung bình của các điểm hợp lệ trong danh sách.

---

### 5.2. Lớp StudentAnalyzerTest

Chứa các ca kiểm thử đơn vị cho lớp `StudentAnalyzer` bằng JUnit 5.

Các nhóm test case:

- Trường hợp bình thường (có cả điểm hợp lệ và không hợp lệ)
- Trường hợp danh sách rỗng
- Trường hợp giá trị biên (0, 10)
- Trường hợp toàn bộ dữ liệu không hợp lệ

---

## 6. Cách chạy chương trình và kiểm thử

### 6.1. Chạy kiểm thử bằng Maven

Mở terminal tại thư mục gốc của project (nơi chứa file `pom.xml`) và chạy:

## bash: `mvn clean test`

### 6.2. Kết quả mong đợi

Maven build thành công
Các test case đều pass
Hiển thị thông báo: `BUILD SUCCESS`

## 7. Kết quả kiểm thử

Tổng số test case: 3
Số test pass: 3
Failures: 0
Errors: 0
Điều này cho thấy các phương thức trong chương trình hoạt động đúng theo yêu cầu đề bài.

## 8. Kết luận

Thông qua bài thực hành này, sinh viên đã:
Hiểu cách viết và chạy kiểm thử đơn vị với JUnit 5
Biết cách tổ chức project Java theo chuẩn Maven
Biết cách phát hiện và xử lý lỗi thông qua kiểm thử tự động

## 9. Tài liệu tham khảo

JUnit 5 User Guide: https://junit.org/junit5/docs/current/user-guide/
Maven Documentation: https://maven.apache.org/guides/index.html
