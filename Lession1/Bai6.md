# Chọn sơ đồ UML phù hợp

| Tình huống                                                  | Sơ đồ UML          | Giải thích ngắn                      |
| ----------------------------------------------------------- | ------------------ | ------------------------------------ |
| A. Mô tả chức năng người dùng trên ứng dụng học tiếng Anh   | Use Case Diagram   | Chỉ ra các tác nhân và chức năng     |
| B. Mô tả lớp NguoiDung, KhoaHoc, BaiHoc và quan hệ          | Class Diagram      | Biểu diễn lớp, thuộc tính, quan hệ   |
| C. Luồng học viên: bắt đầu → vào học → làm bài → hoàn thành | Activity Diagram   | Mô tả quy trình công việc tuần tự    |
| D. Cách hệ thống triển khai trên máy chủ, thiết bị          | Deployment Diagram | Phân bố các nút (nodes) và artifacts |
| E. Thứ tự tương tác khi học viên nộp bài                    | Sequence Diagram   | Trình tự thông điệp giữa đối tượng   |

## Ví dụ minh họa (rút gọn)

### A. Use Case (Ứng dụng học tiếng Anh)

```mermaid
graph LR
    User((Người học)) --- UC1[Đăng nhập]
    User --- UC2[Chọn khóa học]
    User --- UC3[Làm bài tập]
    User --- UC4[Xem tiến độ]
    Admin((Quản trị)) --- UC5[Quản lý khóa học]
```

### B. Class Diagram

```mermaid
classDiagram
    class User {
        +id: int
        +name: string
        +login(): void
    }

    class Course {
        +id: int
        +title: string
        +addLesson(): void
    }

    class Enrollment {
        +userId: int
        +courseId: int
        +enrollDate: Date
    }

    class Lesson {
        +id: int
        +topic: string
        +doExercise(): void
    }

    %% Relationships
    User "1" -- "many" Enrollment : registers >
    Course "1" -- "many" Enrollment : has >
    Course "1" -- "many" Lesson : contains >

```

### C. Activity Diagram

```mermaid
flowchart TD
    A[Bắt đầu] --> B[Vào khóa học]
    B --> C[Làm bài tập]
    C --> D[Nộp bài]
    D --> E[Hoàn thành]
```

### D. Deployment Diagram (khái quát)

```mermaid
graph LR
    Mobile[Thiết bị di động] --> API[Server API]
    Web[Trình duyệt Web] --> API
    API --> DB[(Database)]
    API --> Media[Content Server]
```

### E. Sequence Diagram (Nộp bài)

```mermaid
sequenceDiagram
    participant User
    participant App
    participant API
    participant DB
    User->>App: Nộp bài
    App->>API: POST /submit
    API->>DB: Lưu kết quả
    DB-->>API: OK
    API-->>App: Xác nhận
    App-->>User: Hiển thị hoàn thành
```
