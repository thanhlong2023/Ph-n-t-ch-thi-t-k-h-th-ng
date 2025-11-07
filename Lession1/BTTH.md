# Phân tích & đề xuất thiết kế hệ thống logistics

## 1. Actors & chức năng

- Khách hàng: Tạo đơn, theo dõi trạng thái, chỉnh địa chỉ, yêu cầu hỗ trợ.
- Nhân viên giao hàng (Courier): Nhận phân công, cập nhật mốc trạng thái (Picked, In-Transit, Delivered/Failed), báo sự cố.
- Điều phối (Dispatcher): Phân tuyến, cân bằng tải, xử lý đơn chậm.
- Quản lý (Manager): Xem báo cáo hiệu suất khu vực, tỷ lệ đúng hạn, danh sách tồn đọng.
- CEO: Dashboard tổng quan (doanh thu, đơn/ngày, vùng hoạt động, KPI).
- Dịch vụ ngoài: Map API, Payment Gateway, Notification Service.

## 2. Phân loại theo hệ thống thông tin

- TPS: Tạo đơn, cập nhật trạng thái, xử lý thanh toán, ghi TrackingEvent.
- MIS: Báo cáo hiệu suất khu vực, tồn đọng, tỷ lệ giao đúng hạn tuần/tháng.
- DSS: Tối ưu tuyến giao, dự báo nhu cầu theo mùa, đề xuất tái phân phối nguồn lực.
- EIS: Dashboard CEO (chỉ số chiến lược, cảnh báo khu vực trễ, xu hướng doanh thu).

## 3. Mô hình phát triển đề xuất

- Chọn: Agile (Scrum + Incremental Microservices).
- Lý do: Nhiều phân hệ có thể triển khai dần (Tracking, Routing, Reporting); yêu cầu thay đổi theo thực tế vận hành; cần feedback nhanh từ khách hàng và đội giao hàng; dễ mở rộng thêm DSS sau khi nền TPS ổn định.

## 4. UML diagrams đề xuất

1. Use Case Diagram: Xác định phạm vi chức năng & tác nhân.
2. Activity Diagram: Luồng vòng đời đơn hàng từ tạo đến kết thúc.
3. Sequence Diagram: Tương tác cập nhật trạng thái giao hàng (Courier app → API → Tracking → Notification).
4. Class Diagram: Mô hình thực thể lõi (User, Order, Package, RouteAssignment, TrackingEvent, Vehicle).

## 5. Use Case (rút gọn)

```mermaid
graph LR
    Customer((Khách hàng)) --- UC1[Tạo đơn]
    Customer --- UC2[Theo dõi trạng thái]
    Courier((Giao hàng)) --- UC3[Cập nhật trạng thái]
    Dispatcher((Điều phối)) --- UC4[Phân tuyến]
    Dispatcher --- UC5[Tối ưu lộ trình]
    Manager((Quản lý)) --- UC6[Báo cáo hiệu suất]
    CEO((CEO)) --- UC7[Dashboard tổng quan]
    External((Dịch vụ ngoài)) --- UC8[Thanh toán/Map/Push]
```

## 6. Activity (Vòng đời đơn hàng)

```mermaid
flowchart TD
    A[Tạo đơn] --> B[Kiểm tra hợp lệ]
    B -->|Hợp lệ| C[Phân tuyến]
    B -->|Lỗi| Z[Yêu cầu sửa/Hủy]
    C --> D[Phân công courier]
    D --> E[Đã lấy hàng]
    E --> F[Đang vận chuyển]
    F --> G{Giao thành công?}
    G -->|Có| H[Hoàn tất]
    G -->|Không| I[Xử lý thất bại / hoàn trả]
    I --> F
```

## 7. Sequence (Cập nhật trạng thái)

```mermaid
sequenceDiagram
    participant CourierApp
    participant API
    participant TrackingSvc
    participant Notify
    CourierApp->>API: POST /status (Delivered)
    API->>TrackingSvc: recordEvent(orderId, Delivered)
    TrackingSvc-->>API: OK
    API->>Notify: send(customer, delivered)
    Notify-->>CourierApp: Ack push sent
```

## 8. Class Diagram (lõi)

```mermaid
classDiagram
    class User {
      +id
      +name
      +role
    }
    class Order {
      +id
      +customerId
      +status
    }
    class Package {
      +id
      +orderId
      +weight
    }
    class RouteAssignment {
      +id
      +orderId
      +courierId
      +routePoints
    }
    class TrackingEvent {
      +id
      +orderId
      +type
      +timestamp
    }
    class Vehicle {
      +id
      +courierId
      +plate
    }
    User "1" -- "many" Order
    Order "1" -- "many" Package
    Order "1" -- "many" TrackingEvent
    Order "1" -- "0..1" RouteAssignment
    User "1" -- "many" RouteAssignment : courier
    User "1" -- "many" Vehicle : owns
```

## 9. Lộ trình triển khai ngắn

- Sprint 1–2: TPS (Order, Status, Tracking).
- Sprint 3–4: MIS (Báo cáo khu vực, tồn đọng).
- Sprint 5: EIS Dashboard + cảnh báo.
- Sprint 6+: DSS tối ưu lộ trình & dự báo.

## 10. Tóm tắt

TPS: Giao dịch & trạng thái đơn. MIS: Báo cáo vận hành. DSS: Tối ưu & dự báo. EIS: Chiến lược cấp cao. Mô hình: Agile đảm bảo linh hoạt và giá trị gia tăng sớm. UML: Use Case, Activity, Sequence, Class hỗ trợ phân tích toàn diện.
