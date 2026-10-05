# SẢN PHẨM BÀN GIAO D05: BIỂU ĐỒ TUẦN TỰ (SEQUENCE DIAGRAM)
## HỆ THỐNG CRM CHUỖI BÁN LẺ CELLPHONES (TRÊN NỀN TẢNG FRAPPE FRAMEWORK)

**Mã sản phẩm:** D05  
**Tên sản phẩm:** Biểu đồ tuần tự chọn lọc và ghi chú thiết kế (Sequence Diagrams – Proposed Design)  
**Người thực hiện:** Trần Quang Huy (MSSV: 24520706 – TV4)  
**Vai trò:** Quản trị hệ thống, Phụ trách Mô hình Dữ liệu và Thiết kế Triển khai  
**Nền tảng mục tiêu:** Frappe Framework v15 (Custom App độc lập `cellphones_crm`)  
**Tài liệu cơ sở:**
- `Nền Tảng CRM.md` (Nền tảng phân tích hệ thống CRM CellphoneS)
- `Nhiemvu.md` (Phân công nhiệm vụ và yêu cầu chuẩn đầu ra TV4)
- `docs/D01_ERD_ThuyetMinh.md`, `docs/D02_TuDienDuLieu.md`, `docs/D04_AnhXa_Frappe.md`  
**Trạng thái:** Hoàn thiện (Thiết kế đề xuất – Proposed Design)

---

## 1. NGUYÊN TẮC THIẾT KẾ BIỂU ĐỒ TUẦN TỰ

1. **Phân tầng kiến trúc rõ ràng (Multi-tier Separation):**
   * **Actor Layer:** Khách hàng (`Customer`), Nhân viên CSKH cửa hàng (`Store CSKH`), Nhân viên CSKH chuỗi (`Chain CSKH`).
   * **Presentation Layer (UI):** Giao diện Frappe Desk (`Support Ticket Form / Desk UI`).
   * **Business Logic Layer (Controller):** Frappe Server Controller (`support_ticket.py`, `Workflow Engine`).
   * **Persistence Layer (Database):** Cơ sở dữ liệu MariaDB (`Support_Ticket Table`, `Interaction Table`).
2. **Gắn nhãn chuẩn hóa:**
   * Toàn bộ các biểu đồ dưới đây được định danh là **"Thiết kế đề xuất" (Proposed Design)**, dùng để minh họa logic xử lý kỹ thuật trước khi xây dựng mã nguồn chính thức.

---

## 2. BIỂU ĐỒ TUẦN TỰ 1: TIẾP NHẬN VÀ ĐIỀU PHỐI PHIẾU HỖ TRỢ LIÊN CHI NHÁNH

* **Tình huống nghiệp vụ:** Khách hàng liên hệ CSKH chuỗi hoặc đến Cửa hàng 1 (CN01) yêu cầu bảo hành/hỗ trợ sản phẩm nhưng yêu cầu cần được chuyển giao cho nhân viên tại Cửa hàng 2 (CN02) xử lý tiếp.
* **Mục tiêu kỹ thuật:** Làm rõ luồng tiếp nhận, kiểm tra dữ liệu khách hàng, ghi nhận phiếu và cơ chế phân quyền điều phối theo chi nhánh.

```mermaid
sequenceDiagram
    autonumber
    actor Customer as Khách hàng
    actor Agent1 as Nhân viên CSKH (CN01)
    participant UI as Frappe Desk UI
    participant Ctrl as SupportTicket Controller
    participant DB as MariaDB Database
    actor Agent2 as Nhân viên CSKH (CN02)

    Note over Customer, Agent2: [THIẾT KẾ ĐỀ XUẤT] Luồng tiếp nhận & Điều phối Phiếu hỗ trợ liên chi nhánh

    Customer->>Agent1: Yêu cầu bảo hành / khiếu nại (SĐT, Sản phẩm, Tình trạng lỗi)
    Agent1->>UI: Mở Form Tạo Phiếu Hỗ Trợ (Nhập SĐT / Mã KH)
    UI->>Ctrl: get_customer_profile(phone_number)
    Ctrl->>DB: SELECT * FROM `tabCellphoneS Customer` WHERE phone_number = ?
    DB-->>Ctrl: Trả về thông tin khách hàng & lịch sử mua hàng
    Ctrl-->>UI: Hiển thị thông tin KH (Tên, Hạng thành viên, Lịch sử mua)

    Agent1->>UI: Nhập thông tin lỗi, chọn Phân loại: "Bảo hành", chọn Chi nhánh xử lý: "CN02", Phân công: "Agent2"
    Agent1->>UI: Bấm nút [Lưu & Chuyển tiếp nhận]
    UI->>Ctrl: submit_ticket(ticket_data)
    
    activate Ctrl
    Ctrl->>Ctrl: validate_ticket_data()
    Note right of Ctrl: Kiểm tra bắt buộc: customer_id, issue_description, assigned_to_user
    Ctrl->>DB: INSERT INTO `tabSupport Ticket` (status='Tiếp nhận', branch_id='CN02', assigned_to='Agent2', ...)
    Ctrl->>DB: INSERT INTO `tabCustomer Interaction` (summary='Tiếp nhận yêu cầu bảo hành tại CN01, chuyển CN02')
    DB-->>Ctrl: Ghi nhận thành công (ticket_id: TICK-2026-00089)
    
    Ctrl->>Ctrl: trigger_notification(assigned_to='Agent2')
    Ctrl-->>UI: Trả về thông báo thành công & Mã phiếu TICK-2026-00089
    deactivate Ctrl

    UI-->>Agent1: Hiển thị thông báo "Đã tạo và phân công phiếu thành công"
    Agent1-->>Customer: Cung cấp mã tiếp nhận & hẹn thời gian liên hệ lại

    Ctrl-)Agent2: Gửi thông báo trên Desk & Notification: "Bạn có phiếu hỗ trợ mới TICK-2026-00089 cần xử lý tại CN02"
    Agent2->>UI: Mở xem chi tiết phiếu TICK-2026-00089 trên Workspace CN02
```

---

## 3. BIỂU ĐỒ TUẦN TỰ 2: THẨM ĐỊNH ĐIỀU KIỆN VÀ ĐÓNG PHIẾU HỖ TRỢ (CLOSING GATES VALIDATION)

* **Tình huống nghiệp vụ:** Nhân viên CSKH sau khi nhận phản hồi kết quả từ đối tác bảo hành/hãng tiến hành nghiệm thu và đóng phiếu hỗ trợ trên hệ thống.
* **Mục tiêu kỹ thuật:** Ràng buộc logic kiểm tra điều kiện đóng phiếu nghiêm ngặt (Practice #2 – Đảm bảo 100% phiếu đóng có kết quả xử lý và xác nhận đã thông báo cho khách hàng).

```mermaid
sequenceDiagram
    autonumber
    actor Agent as Nhân viên CSKH
    participant UI as Form Chi Tiết Phiếu Hỗ Trợ
    participant Ctrl as SupportTicket Controller
    participant DB as MariaDB Database
    actor Customer as Khách hàng

    Note over Agent, Customer: [THIẾT KẾ ĐỀ XUẤT] Luồng thẩm định điều kiện & Đóng phiếu hỗ trợ (Closing Gates)

    Agent->>UI: Mở phiếu TICK-2026-00089 (Trạng thái hiện tại: 'Đang xử lý')
    Agent->>UI: Bấm nút hành động [Đóng phiếu hỗ trợ]
    
    UI->>Ctrl: validate_and_close_ticket(ticket_id)
    activate Ctrl

    Note over Ctrl: BƯỚC 1: Kiểm tra kết quả xử lý (Resolution Result Gate)
    alt resolution_result IS NULL hoặc rỗng
        Ctrl-->>UI: Throw ValidationError("Không thể đóng phiếu: Vui lòng nhập Kết quả xử lý chi tiết trước khi đóng!")
        UI-->>Agent: Hiển thị cảnh báo lỗi màu đỏ (Highlight ô 'Kết quả xử lý')
    else resolution_result ĐÃ NHẬP
        
        Note over Ctrl: BƯỚC 2: Kiểm tra cờ xác nhận đã thông báo khách (Customer Notification Gate)
        alt customer_notified == FALSE (Chưa báo khách)
            Ctrl-->>UI: Throw ValidationError("Không thể đóng phiếu: Bạn phải xác nhận đã liên hệ thông báo kết quả cho khách hàng!")
            UI-->>Agent: Hiển thị cảnh báo lỗi (Yêu cầu tích chọn 'Đã thông báo khách')
        else customer_notified == TRUE (Đã báo khách)
            
            Note over Ctrl: BƯỚC 3: Hợp lệ -> Tiến hành đóng phiếu
            Ctrl->>DB: UPDATE `tabSupport Ticket` SET status = 'Đóng phiếu', closed_at = NOW() WHERE name = 'TICK-2026-00089'
            Ctrl->>DB: UPDATE `tabCustomer Care Task` SET status = 'Hoàn thành', completed_at = NOW() WHERE ticket_id = 'TICK-2026-00089'
            DB-->>Ctrl: Cập nhật thành công
            
            Ctrl->>Ctrl: update_sla_metrics(ticket_id)
            Ctrl-->>UI: Trả về trạng thái mới 'Đóng phiếu' & Thời điểm closed_at
            deactivate Ctrl

            UI-->>Agent: Cập nhật giao diện: Trạng thái [ĐÃ ĐÓNG PHIẾU], Khóa các ô nhập liệu
            Agent->>Customer: Bàn giao thiết bị hoàn tất / Xác nhận kết thúc khiếu nại
        end
    end
```

---

## 4. GHI CHÚ THIẾT KẾ & ĐỒNG BỘ VỚI CÁC SẢN PHẨM KHÁC

1. **Đồng bộ với D01 (ERD) & D02 (Từ điển dữ liệu):**
   * Các thực thể và trường gọi trong Sequence Diagram (`Support_Ticket.status`, `assigned_to_user`, `resolution_result`, `customer_notified`, `closed_at`) khớp chính xác 100% với tên vật lý và kiểu dữ liệu trong D02.
2. **Đồng bộ với D03 (Wireframe):**
   * Các thông báo lỗi `ValidationError` trong biểu đồ 2 tương ứng chính xác với các cảnh báo đỏ được thiết kế trên giao diện `UI-04` (Support Ticket View).
3. **Đồng bộ với D04 (Ánh xạ Frappe):**
   * Các bước chuyển trạng thái (Transitions) tuân thủ đúng quy trình Workflow và Ma trận phân quyền của Frappe Framework v15.
