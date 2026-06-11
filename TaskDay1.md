
---

# 1. Test API Đăng ký Sinh viên

Controller:

```java
@RequestMapping("/api/v1/students")
```

API:

```http
POST /api/v1/students/register
```

---

## Trong Postman

### Method

```http
POST
```

### URL

```http
http://localhost:8080/api/v1/students/register
```

### Body

Chọn:

```text
Body -> raw -> JSON
```

JSON:

```json
{
    "fullName": "Nguyen Van A",
    "email": "nguyenvana@gmail.com",
    "password": "123456",
    "phone": "0988888888"
}
```

---

## Kết quả thành công

```json
{
    "message": "Student registered successfully",
    "data": {
        "id": 4,
        "fullName": "Nguyen Van A",
        "email": "nguyenvana@gmail.com",
        "phone": "0988888888",
        "role": "STUDENT",
        "active": true,
        "createdAt": "2026-06-10T12:00:00"
    }
}
```

---

# 2. CRUD Người dùng (Admin)

Controller:

```java
@RequestMapping("/api/v1/admin/users")
```

---

# 2.1 Lấy danh sách User

## GET

```http
http://localhost:8080/api/v1/admin/users
```

---

## Có phân trang

```http
http://localhost:8080/api/v1/admin/users?page=0&size=5
```

---

## Tìm kiếm

```http
http://localhost:8080/api/v1/admin/users?keyword=admin
```

---

## Lọc role

```http
http://localhost:8080/api/v1/admin/users?role=STUDENT
```

Role gồm:

```text
ADMIN
LECTURER
STUDENT
```

---

# 2.2 Lấy User theo ID

## GET

```http
http://localhost:8080/api/v1/admin/users/1
```

---

# 2.3 Tạo User

## POST

```http
http://localhost:8080/api/v1/admin/users
```

---

## Body JSON

```json
{
    "fullName": "Tran Van B",
    "email": "tranvanb@gmail.com",
    "password": "123456",
    "phone": "0999999999",
    "role": "LECTURER",
    "active": true
}
```

---

# 2.4 Cập nhật User

## PUT

```http
http://localhost:8080/api/v1/admin/users/2
```

---

## Body

```json
{
    "fullName": "Tran Van B Updated",
    "email": "tranvanb@gmail.com",
    "password": "123456",
    "phone": "0111111111",
    "role": "LECTURER",
    "active": true
}
```

---

# 2.5 Xóa mềm User

## DELETE

```http
http://localhost:8080/api/v1/admin/users/2
```

---

## Thành công

Status:

```http
204 No Content
```

---

# 3. CRUD Khóa học

Controller:

```java
@RequestMapping("/api/v1/admin/courses")
```

---

# 3.1 Lấy danh sách khóa học

## GET

```http
http://localhost:8080/api/v1/admin/courses
```

---

## Phân trang

```http
http://localhost:8080/api/v1/admin/courses?page=0&size=5
```

---

## Tìm kiếm

```http
http://localhost:8080/api/v1/admin/courses?keyword=java
```

---

# 3.2 Lấy khóa học theo ID

## GET

```http
http://localhost:8080/api/v1/admin/courses/1
```

---

# 3.3 Tạo khóa học

## POST

```http
http://localhost:8080/api/v1/admin/courses
```

---

## Body JSON

```json
{
    "code": "IT103",
    "name": "Spring Boot API",
    "description": "RESTful API with Spring Boot",
    "maxStudents": 40,
    "active": true
}
```

---

# 3.4 Cập nhật khóa học

## PUT

```http
http://localhost:8080/api/v1/admin/courses/1
```

---

## Body

```json
{
    "code": "IT101",
    "name": "Java Web Advanced",
    "description": "Updated course",
    "maxStudents": 35,
    "active": true
}
```

---

# 3.5 Xóa mềm khóa học

## DELETE

```http
http://localhost:8080/api/v1/admin/courses/1
```

---

# 4. Đăng ký khóa học cho Student

Controller:

```java
@RequestMapping("/api/v1/student/courses")
```

---

# 4.1 Đăng ký khóa học

## POST

```http
http://localhost:8080/api/v1/student/courses/1/enrollments
```

---

## Body JSON

```json
{
    "studentId": 3
}
```

Ý nghĩa:

* `1` = courseId
* `3` = studentId

---

## Kết quả

```json
{
    "message": "Enrollment created successfully",
    "data": {
        "id": 1,
        "studentId": 3,
        "studentName": "Demo Student",
        "courseId": 1,
        "courseCode": "IT101",
        "courseName": "Java Web Fundamentals",
        "status": "ENROLLED",
        "enrolledAt": "2026-06-10T12:00:00"
    }
}
```

---

# 4.2 Xem khóa học đã đăng ký

## GET

```http
http://localhost:8080/api/v1/student/courses/enrollments?studentId=3
```

---

# 5. Dữ liệu mẫu đã có sẵn

Khi chạy project, `DataInitializer` tự tạo:

## Admin

```text
admin@it211.local
Admin@123
```

## Lecturer

```text
lecturer@it211.local
Lecturer@123
```

## Student

```text
student@it211.local
Student@123
```

---

# 6. Vì sao chưa cần Token/Login?

Trong `SecurityConfig`:

```java
.authorizeHttpRequests(authorize -> authorize.anyRequest().permitAll())
```

Nghĩa là:

```text
Tất cả API đều được phép truy cập
```

nên Postman chưa cần:

* JWT
* Bearer Token
* Login

---

# 7. Header chuẩn trong Postman

Ở tab Headers thêm:

```text
Content-Type: application/json
```

---

# 8. Các lỗi thường gặp

## 400 Bad Request

Sai JSON hoặc thiếu field.

Ví dụ:

```json
{
   "email": ""
}
```

---

## 404 Not Found

Sai URL.

---

## 405 Method Not Allowed

Sai method:

* dùng GET thay vì POST
* dùng POST thay vì PUT

---

## 500 Internal Server Error

Lỗi backend/service/database.

Xem console Spring Boot để biết chi tiết.

---

# 9. Thứ tự test chuẩn

Nên test theo thứ tự:

```text
1. Register Student
2. GET Users
3. Create Course
4. GET Courses
5. Enroll Course
6. GET Enrollments
```

---

# 10. Cấu trúc API tổng hợp

| Chức năng         | Method | API                                               |
| ----------------- | ------ | ------------------------------------------------- |
| Đăng ký sinh viên | POST   | `/api/v1/students/register`                       |
| Danh sách user    | GET    | `/api/v1/admin/users`                             |
| User theo ID      | GET    | `/api/v1/admin/users/{id}`                        |
| Tạo user          | POST   | `/api/v1/admin/users`                             |
| Update user       | PUT    | `/api/v1/admin/users/{id}`                        |
| Delete user       | DELETE | `/api/v1/admin/users/{id}`                        |
| Danh sách course  | GET    | `/api/v1/admin/courses`                           |
| Course theo ID    | GET    | `/api/v1/admin/courses/{id}`                      |
| Tạo course        | POST   | `/api/v1/admin/courses`                           |
| Update course     | PUT    | `/api/v1/admin/courses/{id}`                      |
| Delete course     | DELETE | `/api/v1/admin/courses/{id}`                      |
| Enroll khóa học   | POST   | `/api/v1/student/courses/{courseId}/enrollments`  |
| Xem enrollment    | GET    | `/api/v1/student/courses/enrollments?studentId=3` |
