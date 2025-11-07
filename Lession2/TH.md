**BÁO CÁO PHÂN TÍCH YÊU CẦU BAN ĐẦU**

**Dự án:** Xây dựng Hệ thống Quản lý Trung tâm Đào tạo (TMS - Training Management System)
**Khách hàng:** Trung tâm Anh ngữ ABC
**Ngày:** 06/11/2025
**Người biên soạn:** Chuyên viên Phân tích Hệ thống

---

## 1. Phân tích Môi trường Hệ thống

**Tình hình hiện tại:** Trung tâm ABC đang quản lý toàn bộ hoạt động (học viên, lịch học, học phí, điểm danh) chủ yếu bằng các tệp Excel riêng lẻ, Google Sheets, và nhóm Zalo. Quy trình thủ công này gây ra sai sót (trùng lịch), tốn thời gian đối soát (học phí), và không thể cung cấp báo cáo tức thời cho ban giám đốc.

**Mục tiêu hệ thống (TMS):** Xây dựng một nền tảng web-based tập trung để số hóa và tự động hóa toàn bộ quy trình vận hành của trung tâm.

Các yếu tố môi trường sau đây sẽ tác động trực tiếp đến thiết kế của hệ thống:

- **Người dùng (Users):**
  - **Ban Giám đốc:** Cần xem báo cáo tổng quan (doanh thu, sĩ số).
  - **Nhân viên Tư vấn/Admin:** Người dùng chính (heavy-user), thực hiện các tác vụ ghi danh, thu phí, xếp lớp.
  - **Quản lý Học vụ:** Sắp xếp lịch giáo viên, quản lý chương trình học.
  - **Giáo viên:** Điểm danh, nhập điểm, xem lịch dạy.
  - **Học viên/Phụ huynh:** Xem lịch học, theo dõi học phí, xem kết quả học tập.
- **Hệ thống Bên ngoài (External Systems):**
  - **Cổng thanh toán:** Hệ thống cần tích hợp với (VNPay, MoMo) để cho phép phụ huynh đóng học phí online.
  - **Dịch vụ SMS/Email:** Kết nối với dịch vụ (ví dụ: Twilio, SendGrid) để gửi thông báo tự động (nhắc lịch học, thông báo nợ phí).
- **Quy trình Nghiệp vụ (Business Processes):**
  - Quy trình tư vấn và ghi danh học viên mới.
  - Quy trình xếp lớp và khai giảng lớp mới.
  - Quy trình thu học phí (trực tiếp và online).
  - Quy trình điểm danh và báo cáo tình hình lớp học.
- **Luật lệ và Quy định (Regulations):**
  - **Nghị định 13/2023/NĐ-CP (Bảo vệ Dữ liệu Cá nhân):** Hệ thống _bắt buộc_ phải có cơ chế lưu trữ an toàn, mã hóa thông tin nhạy cảm (CCCD, SĐT) và phải có sự đồng ý của phụ huynh khi thu thập dữ liệu.
  - **Quy định về Hóa đơn điện tử:** Hệ thống phải có khả năng tích hợp với nhà cung cấp hóa đơn điện tử để xuất hóa đơn khi thu phí.

---

## 2. Xác định Các bên Liên quan (Stakeholders)

Việc xác định đúng và phân cấp các bên liên quan là then chốt để quản lý kỳ vọng và ưu tiên các tính năng.

| Stakeholder                | Vai trò                | Mối quan tâm chính                                                                                                                               | Mức độ ưu tiên |
| :------------------------- | :--------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------- | :------------- |
| **Ban Giám đốc**           | (Nhà tài trợ)          | 1. **Báo cáo doanh thu** chính xác, real-time.<br>2. Tối ưu chi phí vận hành.<br>3. Khả năng mở rộng hệ thống khi mở thêm chi nhánh.             | **Critical**   |
| **Nhân viên Tư vấn/Admin** | (Người dùng chính)     | 1. **Giao diện ghi danh nhanh**, dễ sử dụng.<br>2. Tự động nhắc nhở học viên (tái đăng ký, nợ phí).<br>3. Giảm bớt công việc nhập liệu thủ công. | **Critical**   |
| **Quản lý Học vụ**         | (Người dùng nghiệp vụ) | 1. **Tránh trùng lặp** lịch giáo viên/lịch phòng học.<br>2. Dễ dàng theo dõi tiến độ của lớp học.<br>3. Chất lượng chương trình giảng dạy.       | **Major**      |
| **Giáo viên**              | (Người dùng nghiệp vụ) | 1. **Điểm danh online** dễ dàng (trên điện thoại).<br>2. Xem lịch dạy rõ ràng.<br>3. Nền tảng để nhập nhận xét/điểm số.                          | **Major**      |
| **Phụ huynh/Học viên**     | (Người dùng bên ngoài) | 1. Tra cứu lịch học, kết quả.<br>2. **Thanh toán học phí online** tiện lợi.<br>3. Nhận thông báo từ trung tâm (nghỉ học, sự kiện).               | **Major**      |
| **Bộ phận IT (Nếu có)**    | (Hỗ trợ Kỹ thuật)      | 1. **Bảo mật** và sao lưu dữ liệu.<br>2. Hệ thống chạy ổn định, ít lỗi.<br>3. Chi phí bảo trì.                                                   | **Minor**      |

---

## 3. Nguồn Yêu cầu & Kỹ thuật Thu thập

Để đảm bảo thu thập đầy đủ và chính xác các yêu cầu, chúng tôi sẽ phối hợp các kỹ thuật sau:

1.  **Phân tích Tài liệu (Document Analysis):**
    - **Nguồn:** Các tệp Excel quản lý học viên, quản lý thu chi, các biểu mẫu ghi danh (bản giấy) và các quy trình (file Word) mà trung tâm đang sửn.
    - **Mục đích:** Hiểu quy trình "as-is" (hiện trạng), các trường dữ liệu bắt buộc và các quy tắc nghiệp vụ cơ bản.
2.  **Phỏng vấn (Interview):**
    - **Đối tượng:** Ban Giám đốc (để hiểu mục tiêu chiến lược, ROI), Quản lý Học vụ và Kế toán (để làm rõ các quy tắc nghiệp vụ phức tạp).
    - **Mục đích:** Thu thập thông tin chiều sâu, các yêu cầu nhạy cảm và lý do "Tại sao?" đằng sau các quy trình.
3.  **Quan sát (Observation):**
    - **Đối tượng:** Quan sát trực tiếp nhân viên Admin/Tư vấn làm việc tại quầy lễ tân vào giờ cao điểm.
    - **Mục đích:** Phát hiện các "nỗi đau" (pain points) và các bước "lách" (workarounds) mà họ phải thực hiện (ví dụ: dùng Zalo để xác nhận nhanh) mà phỏng vấn có thể không nói ra.
4.  **Khảo sát (Survey):**
    - **Đối tượng:** Gửi khảo sát nhanh cho toàn bộ Giáo viên và một nhóm Phụ huynh.
    - **Mục đích:** Lấy ý kiến số đông về các tính năng họ mong muốn nhất (ví dụ: "Bạn muốn xem gì nhất trên ứng dụng?").

---

## 4. Phân loại Yêu cầu Hệ thống (Sơ bộ)

Từ quá trình phân tích trên, chúng tôi phác thảo các yêu cầu chính của hệ thống TMS:

### Yêu cầu Chức năng (Functional Requirements)

- **FR1 (Quản lý Học viên):** Hệ thống phải cho phép tạo, lưu trữ, và tìm kiếm hồ sơ học viên (thông tin cá nhân, lịch sử học tập, lịch sử đóng phí).
- **FR2 (Quản lý Lớp học):** Hệ thống phải cho phép tạo lớp học mới, gán giáo viên, phòng học, và lịch học.
- **FR3 (Ghi danh & Xếp lớp):** Hệ thống phải cho phép Admin ghi danh học viên mới và xếp họ vào lớp học phù hợp (có kiểm tra sĩ số tối đa).
- **FR4 (Quản lý Học phí):** Hệ thống phải tự động tạo phiếu thu khi ghi danh, theo dõi trạng thái (đã thu, chưa thu, nợ) và hỗ trợ thanh toán online (tích hợp cổng thanh toán).
- **FR5 (Điểm danh):** Hệ thống phải cho phép Giáo viên điểm danh học viên (có mặt, vắng, xin phép) trên giao diện web hoặc di động.
- **FR6 (Gửi thông báo):** Hệ thống phải tự động gửi SMS/Email nhắc nợ học phí trước 3 ngày hoặc thông báo nghỉ học đột xuất.

### Yêu cầu Phi chức năng (Non-Functional Requirements)

- **NFR1 (Bảo mật - Security):** Toàn bộ thông tin cá nhân của học viên phải được mã hóa. Hệ thống phải phân quyền rõ ràng (Giáo viên không thể thấy thông tin học phí).
- **NFR2 (Hiệu năng - Performance):** Trang tra cứu lịch học và báo cáo doanh thu phải tải xong (load) trong vòng 3 giây, ngay cả khi có 5000+ học viên.
- **NFR3 (Tính khả dụng - Usability):** Giao diện cho Admin phải trực quan, một nhân viên mới có thể học cách ghi danh cho học viên trong vòng 30 phút đào tạo.
- **NFR4 (Tính tương thích - Compatibility):** Giao diện dành cho Phụ huynh/Học viên phải hiển thị tốt trên các trình duyệt di động (Responsive Design).
- **NFR5 (Độ tin cậy - Reliability):** Hệ thống phải đảm bảo hoạt động 99.9% thời gian (uptime) và có cơ chế sao lưu (backup) cơ sở dữ liệu hàng ngày.

---

## 5. Gợi ý Cấu trúc Tài liệu Mô tả Yêu cầu (SRS)

Để đảm bảo đội ngũ phát triển (Developers) và khách hàng (Trung tâm ABC) hiểu thống nhất về hệ thống, tôi đề xuất cấu trúc Tài liệu Đặc tả Yêu cầu Phần mềm (SRS) như sau:

1.  **Giới thiệu (Introduction)**
    - 1.1. Mục đích
    - 1.2. Phạm vi hệ thống (Scope)
    - 1.3. Định nghĩa, Từ viết tắt
    - 1.4. Tài liệu tham chiếu
    - 1.5. Tổng quan tài liệu
2.  **Mô tả Tổng quan (Overall Description)**
    - 2.1. Bối cảnh Sản phẩm (Tích hợp với hệ thống nào)
    - 2.2. Chức năng Sản phẩm (Tóm tắt các mô-đun chính)
    - 2.3. Đặc điểm Người dùng (Admin, Giáo viên, Phụ huynh...)
    - 2.4. Ràng buộc (Constraints) (Pháp lý, Kỹ thuật...)
    - 2.5. Giả định và Phụ thuộc
3.  **Yêu cầu Cụ thể (Specific Requirements)**
    - 3.1. Yêu cầu Giao diện Bên ngoài (Giao diện với cổng thanh toán, SMS)
    - 3.2. Yêu cầu Chức năng (Chi tiết hóa từng FR ở mục 4)
      - 3.2.1. Mô-đun Quản lý Học viên
      - 3.2.2. Mô-đun Quản lý Lớp học & Lịch học
      - 3.2.3. Mô-đun Thu phí & Kế toán
      - 3.2.4. Mô-đun Điểm danh & Báo cáo
    - 3.3. Yêu cầu Phi chức năng (Chi tiết hóa NFR: Hiệu năng, Bảo mật, Usability...)
    - 3.4. Yêu cầu về Cơ sở dữ liệu (Các thực thể chính, ràng buộc dữ liệu)
4.  **Phụ lục (Appendices)**
    - (Bao gồm các Sơ đồ Use Case, Sơ đồ Luồng dữ liệu (DFD), hoặc Mô hình ERD nếu cần làm rõ).

---

**Kết luận:** Hệ thống TMS là một khoản đầu tư cần thiết để giải quyết các vấn đề vận hành hiện tại và tạo nền tảng cho sự phát triển của trung tâm. Bước tiếp theo được đề xuất là tổ chức các buổi phỏng vấn chi tiết (Mục 3) để bắt đầu biên soạn tài liệu SRS (Mục 5).
