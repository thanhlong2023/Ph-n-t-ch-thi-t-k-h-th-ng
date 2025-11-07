# SDLC 6 GIAI ĐOẠN – HỆ THỐNG ĐĂNG KÝ TIÊM CHỦNG ONLINE

## 1. PLANNING (Lập kế hoạch)

- Mục tiêu: Số hóa đăng ký tiêm; giảm 60% thời gian xử lý thủ công; theo dõi trạng thái tiêm.
- Phạm vi ban đầu (MVP): Đăng ký người dân, xét duyệt & xếp lịch, đánh dấu đã tiêm.
- Stakeholders: Người dân, Nhân viên trung tâm, Quản trị hệ thống, Cơ quan y tế.
- Giả định: Người dân có thể truy cập web/mobile; dữ liệu cá nhân được cung cấp trung thực.
- Rủi ro: Đăng ký trùng lặp, bảo mật thông tin sức khỏe, tải cao khi mở đợt tiêm.
- Giảm thiểu: Captcha + xác thực số điện thoại; mã hóa dữ liệu; hàng đợi xử lý (queue).
- Timeline (ước lượng): 2w Planning+Analysis, 3w Design, 6w Dev, 3w Testing, 1w Deploy, liên tục Maintenance.

## 2. REQUIREMENT ANALYSIS (Phân tích yêu cầu)

### Chức năng (Functional)

- Người dân: Tạo hồ sơ, đăng ký mũi tiêm, xem lịch, hủy/đổi lịch (giới hạn).
- Trung tâm: Duyệt đăng ký, xếp lịch theo năng lực (slot), cập nhật trạng thái tiêm.
- Quản lý: Báo cáo số người đã tiêm/chưa tiêm, xuất thống kê theo độ tuổi/khu vực.
- Hệ thống: Gửi email/SMS nhắc lịch, ngăn đăng ký trùng cùng mũi, ghi lịch sử thay đổi.

### Phi chức năng (Non-functional)

- Bảo mật: HTTPS, RBAC, mã hóa trường nhạy cảm (CMND/CCCD).
- Hiệu năng: 1000 đăng ký/phút giờ cao điểm; phản hồi < 3s.
- Khả dụng: Uptime ≥ 99%.
- Mở rộng: Thêm module quản lý phản ứng sau tiêm.

### Use Case (UML chọn: Use Case Diagram)

Actors: Citizen, Staff, Admin, NotificationService
Use Cases: RegisterDose, ViewSchedule, ModifySchedule, ApproveRegistration, AssignSlot, MarkVaccinated, GenerateReport.

## 3. SYSTEM DESIGN (Thiết kế hệ thống)

### Kiến trúc

Web/Mobile Client → API Gateway → Services (Registration / Scheduling / Vaccination / Reporting / Notification) → Database (Relational + Cache) → Message Queue (gửi thông báo).

### Mô hình dữ liệu lõi

- Citizen(id, name, dob, idNumber, phone, address)
- Registration(id, citizenId, doseNumber, status, createdAt)
- Schedule(id, registrationId, slotTime, location, staffId, status)
- VaccinationRecord(id, citizenId, doseNumber, vaccineType, vaccinatedAt, adverseNote)
- Staff(id, name, role)

### UML đề xuất

- Use Case Diagram: Phạm vi chức năng.
- Class Diagram: Thực thể trên.
- Sequence Diagram: Quy trình đăng ký → duyệt → xếp lịch.
- Activity Diagram: Dòng xử lý buổi tiêm (check-in → tiêm → ghi nhận).

### Bảo mật

- RBAC: Roles = Citizen, Staff, Admin.
- JWT + Refresh token.
- Audit log cho thay đổi trạng thái tiêm.

### Tích hợp

- SMS/Email gateway cho nhắc lịch.
- API nội bộ báo cáo lên hệ thống y tế cấp tỉnh.

## 4. IMPLEMENTATION (Triển khai phát triển)

- Backend: Node.js / Java (REST API), phân tầng service + repository.
- Frontend: React (Web), React Native (Mobile).
- Database: PostgreSQL (quan hệ), Redis (cache slot lịch).
- Queue: RabbitMQ hoặc AWS SQS cho gửi thông báo.
- CI/CD: GitHub Actions / Azure DevOps build + test + deploy staging.
- Migrations cho schema; seed dữ liệu danh sách điểm tiêm.

## 5. TESTING (Kiểm thử)

Loại test:

- Unit: Service & validation logic (coverage ≥ 80%).
- Integration: API endpoints (đăng ký, duyệt, xếp lịch).
- Load Test: Kịch bản 1000 đăng ký/phút (khi mở chiến dịch).
- Security Test: Kiểm tra SQL Injection, XSS, CSRF.
- UAT: Nhân viên trung tâm thử quy trình thực tế.
  Kịch bản chính:
- Đăng ký mũi 1 → duyệt → xếp lịch → đánh dấu đã tiêm.
- Thử đăng ký trùng mũi đã tiêm (phải bị chặn).
- Đổi lịch trước 24h (thành công), đổi lịch sau hạn (bị từ chối).
- Cao điểm mở đợt: hàng đợi không rớt request.

## 6. DEPLOYMENT & MAINTENANCE (Triển khai & Bảo trì)

- Môi trường: Dev → Staging → Prod (blue/green hoặc rolling).
- Hạ tầng: Container (Docker) + Orchestrator (Kubernetes/Azure AKS).
- Monitoring: Metrics (CPU, đăng ký/phút), Logs tập trung (ELK).
- Backup: Database daily + point-in-time recovery.
- Bảo trì: Monthly security patch, Quarterly performance review.
- Cải tiến: Thêm module phản ứng sau tiêm, tích hợp lịch tiêm mũi tiếp theo tự động.
- SLA: Phản hồi sự cố nghiêm trọng < 1h; khắc phục < 4h.

## Deliverables (Đầu ra)

| Giai đoạn                | Deliverables                               |
| ------------------------ | ------------------------------------------ |
| Planning                 | Project charter, Scope doc, Risk log       |
| Analysis                 | SRS, Use case list, Non-functional spec    |
| Design                   | Architecture diagram, Data model, UML set  |
| Implementation           | Source code, API docs, Deployment scripts  |
| Testing                  | Test cases, Reports, UAT sign-off          |
| Deployment & Maintenance | Runbook, Monitoring dashboard, Backup plan |

## Chỉ số thành công

- Tỷ lệ lỗi đăng ký < 1%.
- Thời gian xử lý duyệt ≤ 24h.
- ≥ 95% lịch tiêm đúng hạn.
- Zero vi phạm rò rỉ dữ liệu trong 12 tháng.
