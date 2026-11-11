# Session13 - Bài 1: Hello Spring Security và cơ chế mặc định

Java 21, Spring Boot, Spring Security và Lombok. Employee có id, fullName, salary; API trả danh sách 3 nhân viên được fix cứng.

## Chạy ứng dụng

1. Mở project bằng IntelliJ, chọn JDK 21 và tải lại Gradle.
2. Chạy `BaitapSession13Application`.
3. Trong tab Run/Console, tìm dòng `Using generated security password:`. Copy mật khẩu ngay sau dấu hai chấm.

Dependency `spring-boot-starter-security` tự bật bảo vệ mặc định. Tài khoản mặc định là **user**, mật khẩu được sinh ngẫu nhiên khi chạy ứng dụng và thay đổi sau mỗi lần khởi động lại.

## Thử API chưa xác thực

Trong Postman chọn **GET**, Authorization → **No Auth**:

```text
GET http://localhost:8080/api/v1/employees
Accept: application/json
```

Thêm header Accept như trên để kiểm tra API. Kết quả là **401 Unauthorized**, vì chưa gửi tài khoản/mật khẩu.

## Dùng Basic Auth trong Postman

1. Giữ nguyên method GET và URL.
2. Chọn Authorization → Type → **Basic Auth**.
3. Username: **user**.
4. Password: dán mật khẩu của lần chạy hiện tại, lấy từ Console.
5. Chọn Send.

Đúng tài khoản/mật khẩu sẽ trả **200 OK**:

```json
[
  {"id": 1, "fullName": "Nguyen Van An", "salary": 10000000},
  {"id": 2, "fullName": "Tran Thi Binh", "salary": 12000000},
  {"id": 3, "fullName": "Le Van Cuong", "salary": 15000000}
]
```

Sai mật khẩu vẫn trả **401**. Nếu khởi động lại ứng dụng, lấy mật khẩu mới từ Console để cập nhật Postman.

## Quan sát trang đăng nhập mặc định

Mở `http://localhost:8080/api/v1/employees` bằng trình duyệt khi chưa đăng nhập. Với request nhận HTML, Spring Security chuyển hướng **302** đến `/login` và hiển thị form đăng nhập mặc định. Đăng nhập bằng user và mật khẩu hiện tại để xem API.

Request API với Accept: application/json trả 401; request trình duyệt với Accept: text/html thường được chuyển đến trang login. Đây là hành vi mặc định theo loại nội dung client yêu cầu.

Build/cache và log kiểm tra chứa mật khẩu sinh ngẫu nhiên được đặt ngoài thư mục nộp. Khi kéo thả GitHub, bỏ build/.gradle/.idea nếu IntelliJ tạo thêm.
