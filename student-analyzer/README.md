# StudentAnalyzer - Unit Testing with JUnit 5

Bài tập thực hành kiểm thử đơn vị - Phân tích dữ liệu điểm số học sinh

---

## 📋 Mục tiêu học tập

- Làm quen với kiểm thử đơn vị (Unit Test) bằng JUnit 5
- Biết cách viết test case cho các phương thức Java
- Thực hành validate dữ liệu và xử lý các trường hợp biên
- Biết cách chạy kiểm thử bằng Maven
- Làm quen với quy trình làm việc với Git/GitHub

---

## 🎯 Mô tả bài toán

Xây dựng chương trình Java để phân tích điểm số học sinh (giá trị từ 0 đến 10), có các chức năng:

- Đếm số học sinh đạt loại **Giỏi** (điểm ≥ 8.0)
- Tính **điểm trung bình** của các điểm hợp lệ
- Bỏ qua các giá trị không hợp lệ (< 0 hoặc > 10)
- Trả về 0 nếu danh sách rỗng hoặc không có điểm hợp lệ

---

## 🛠️ Công nghệ sử dụng

| Công cụ | Phiên bản/Chi tiết |
|---------|-------------------|
| Ngôn ngữ | Java 8+ |
| Framework kiểm thử | JUnit 5 |
| Công cụ build | Maven |
| IDE | Visual Studio Code |

---

## 📁 Cấu trúc dự án

```
student-analyzer/
├── pom.xml
├── README.md
└── src/
    ├── main/java/com/example/
    │   ├── StudentAnalyzer.java      (lớp chính)
    │   └── Main.java                 (class main)
    └── test/java/com/example/
        └── StudentAnalyzerTest.java  (test cases)
```

---

## 📝 Mô tả các lớp

### StudentAnalyzer.java

Lớp chứa logic xử lý dữ liệu điểm số:

| Phương thức | Mô tả |
|-----------|------|
| `countExcellentStudents(List<Double> scores)` | Đếm số học sinh có điểm ≥ 8.0 (chỉ tính điểm hợp lệ) |
| `calculateValidAverage(List<Double> scores)` | Tính trung bình cộng của các điểm hợp lệ |

### StudentAnalyzerTest.java

Lớp chứa các ca kiểm thử đơn vị cho `StudentAnalyzer`:

- **Trường hợp bình thường**: Danh sách có cả điểm hợp lệ và không hợp lệ
- **Trường hợp biên**: Giá trị 0, 10 (ranh giới)
- **Trường hợp đặc biệt**: Danh sách rỗng, toàn bộ điểm không hợp lệ

---

## 🚀 Hướng dẫn chạy

### Chạy toàn bộ kiểm thử

Mở terminal tại thư mục gốc (chứa `pom.xml`) và thực hiện:

```bash
mvn clean test
```

### Kết quả mong đợi

- ✅ Maven build thành công
- ✅ Tất cả test case đều pass
- ✅ Hiển thị: `BUILD SUCCESS`
- ✅ Báo cáo test trong thư mục: `target/surefire-reports/`

---

## 📊 Kết quả kiểm thử

Sau khi chạy `mvn test`, kiểm tra file báo cáo:

- `target/surefire-reports/TEST-com.example.StudentAnalyzerTest.xml`

---

## 👨‍💻 Tác giả

- Sinh viên: Hoàng Minh Thiện _ BIT230390
- Lớp: 23IT2
- Ngày: 2026

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
