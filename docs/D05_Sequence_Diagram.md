# SẢN PHẨM BÀN GIAO D05: SEQUENCE DIAGRAMS CHỌN LỌC & GHI CHÚ THIẾT KẾ
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D05  
**Thuộc nhiệm vụ:** Nhiệm vụ 6 (Vẽ 1–2 Sequence Diagram cho luồng xử lý chính có kiểm tra điều kiện)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Nền tảng mục tiêu:** Frappe Framework v15 / ERPNext v15  
**Trạng thái:** Hoàn thiện Mốc M4 (Final Deliverable)  
**Nhãn xác nhận:** **THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN)**

---

## TỔNG QUAN CẤU TRÚC BIỂU ĐỒ TUẦN TỰ (UML SEQUENCE ARCHITECTURE)

Các biểu đồ tuần tự được thiết kế theo mô hình 4 tầng phân lập rõ ràng, đồng bộ 100% với Use Case, Từ điển dữ liệu (D02), Wireframe (D03) và Ma trận phân quyền Frappe (D04):
1. **Tác nhân (Actor / User):** Khách hàng Smember, Nhân viên CSKH Showroom, Kỹ thuật viên Điện Thoại Vui, Quản lý CSKH.
2. **Tầng Giao diện (UI Layer):** Form Tiếp nhận Ticket (Desk Form View), Pop-up Nghiệm thu Kỹ thuật.
3. **Tầng Điều khiển Nghiệp vụ (Controller / Backend):** Frappe Server Script Controller, Workflow Engine, Routing Service.
4. **Tầng Cơ sở Dữ liệu & Dịch vụ Ngoài (Database & External Services):** MariaDB, Cổng tra cứu Apple Care API, Cổng gửi tin nhắn Zalo ZNS.

---

## LUỒNG 1: TIẾP NHẬN, THẨM ĐỊNH SLA & TỰ ĐỘNG ĐỊNH TUYẾN PHÂN CÔNG PHIẾU HỖ TRỢ (TẠO & PHÂN CÔNG CÓ ĐIỀU KIỆN)

### 1. Bối cảnh Nghiệp vụ CellphoneS
Khi khách hàng mang máy đến Showroom CellphoneS khiếu nại lỗi thiết bị (hoặc gọi điện qua Hotline 1800), nhân viên CSKH mở phiếu tiếp nhận. Hệ thống tự động kiểm tra hạng hội viên Smember của khách, tính toán thời hạn cam kết SLA và tự động định tuyến phân công (Auto-routing) cho Kỹ thuật viên Điện Thoại Vui (nếu là lỗi phần cứng) hoặc Quản lý Showroom (nếu là khiếu nại thái độ dịch vụ).

### 2. Biểu đồ Sequence Diagram (Proposed Design)

```mermaid
sequenceDiagram
    autonumber
    actor StoreAgent as 👔 CSKH Showroom
    participant UI as 🖥️ Ticket Form (Desk UI)
    participant Ctrl as ⚙️ Ticket Controller (Frappe Backend)
    participant DB as 🗄️ Cơ sở Dữ liệu (MariaDB)
    actor TechAgent as 🛠️ KTV Điện Thoại Vui

    Note over StoreAgent,TechAgent: THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN) - LUỒNG TẠO & PHÂN CÔNG PHIẾU HỖ TRỢ

    StoreAgent->>UI: Nhập thông tin phiếu: SĐT (0908123456), IMEI (358941098234112), Loại vấn đề (Lỗi nguồn)
    StoreAgent->>UI: Nhấn nút [Tiếp nhận & Tạo Phiếu]
    
    UI->>Ctrl: submit_new_ticket(ticket_data)
    
    activate Ctrl
    Ctrl->>DB: 1. Kiểm tra SĐT trong tabCustomer & tabSmember Profile
    DB-->>Ctrl: Khách: Nguyễn Văn An | Hạng: ⭐ S-VIP | Chi tiêu: 85.4 triệu
    
    Ctrl->>Ctrl: 2. Tính toán hạn chót SLA theo chính sách Smember
    alt Khách hàng là S-VIP
        Ctrl->>Ctrl: Set priority = 'Critical', sla_deadline = now() + 4 Giờ
    else Khách hàng Standard / Vãng lai
        Ctrl->>Ctrl: Set priority = 'Medium', sla_deadline = now() + 24 Giờ
    end

    Ctrl->>Ctrl: 3. Kiểm tra logic định tuyến phân công (Auto-routing Engine)
    alt issue_category IN ('Loi_phan_cung_NSX', 'Doi_tra_30_ngay_VIP')
        Ctrl->>DB: Truy vấn KTV DTV đang trực tại Trung tâm DTV gần nhất
        DB-->>Ctrl: KTV khả dụng: TranVanTu (DTV Q9)
        Ctrl->>Ctrl: Set assigned_dept = 'Trung_tam_Dien_Thoai_Vui', allocated_to = 'TranVanTu'
        Ctrl->>Ctrl: Set workflow_state = 'In_Progress' (Bắt đầu xử lý)
    else issue_category == 'Khieu_nai_thai_do_dich_vu'
        Ctrl->>DB: Truy vấn Quản lý Showroom tiếp nhận
        DB-->>Ctrl: Quản lý: StoreManager_Linh
        Ctrl->>Ctrl: Set assigned_dept = 'CSKH_Showroom', allocated_to = 'StoreManager_Linh'
        Ctrl->>Ctrl: Set workflow_state = 'Open'
    end

    Ctrl->>DB: 4. Lưu bản ghi `Support_Ticket` (TCK-2026-00155)
    Ctrl->>DB: 5. Ghi nhật ký vào bảng con `Ticket_Activity_Log` (Action: Tao_moi_va_Phan_cong)
    
    Ctrl-->>TechAgent: Gửi thông báo Push Notification: "Phiếu S-VIP TCK-0155 (SLA 4h) đã được giao cho bạn!"
    deactivate Ctrl

    Ctrl-->>UI: Trả về kết quả thành công kèm Mã Ticket & Hạn SLA
    UI-->>StoreAgent: Hiển thị thông báo: "Tạo phiếu thành công! Đã điều phối KTV DTV-Tú thẩm định (SLA: 4h)."
```

### 3. Ghi chú Thiết kế & Xử lý Luồng Ngoại lệ (Exception Handling)
* **Khách hàng vãng lai không cung cấp SĐT:** Hệ thống tự động gán `customer_id = 'CUST-GUEST'`, `contact_phone = '0000000000'`, đặt mức ưu tiên `priority = 'Low'` và thời hạn SLA mặc định là 48 giờ.
* **Không có Kỹ thuật viên trực ca tại địa bàn:** Hệ thống đưa Ticket vào hàng đợi chung (`Department Queue: Trung_tam_Dien_Thoai_Vui`) với trạng thái `Open`, đồng thời gửi cảnh báo Email khẩn cấp cho Quản lý Trung tâm DTV để gán việc thủ công.

---

## LUỒNG 2: NGHIỆM THU KỸ THUẬT & ĐÓNG PHIẾU HỖ TRỢ CÓ KIỂM TRA ĐIỀU KIỆN (ĐỔI TRẢ 1-ĐỔI-1 S-VIP & KHẢO SÁT CSAT)

### 1. Bối cảnh Nghiệp vụ CellphoneS
Sau khi Kỹ thuật viên Điện Thoại Vui kiểm định lỗi phần cứng do nhà sản xuất (IC nguồn hỏng) trên máy iPhone của hội viên S-VIP trong 30 ngày đầu, hệ thống thực hiện quy trình nghiệm thu, đối soát điều kiện bảo mật (iCloud), duyệt xuất đổi máy mới và đóng phiếu kèm kích hoạt khảo sát CSAT qua Zalo ZNS.

### 2. Biểu đồ Sequence Diagram (Proposed Design)

```mermaid
sequenceDiagram
    autonumber
    actor TechAgent as 🛠️ KTV Điện Thoại Vui
    actor StoreAgent as 👔 CSKH Showroom
    participant UI as 🖥️ Ticket Detail Form (Desk UI)
    participant Ctrl as ⚙️ Ticket Workflow Engine
    participant AppleAPI as 🌐 Cổng Apple Care / GSMA API
    participant DB as 🗄️ Cơ sở Dữ liệu (MariaDB)
    participant ZNS as 📲 Cổng Zalo ZNS Gateway
    actor Customer as 👤 Khách hàng S-VIP

    Note over TechAgent,Customer: THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN) - LUỒNG NGHIỆM THU & ĐÓNG PHIẾU CÓ ĐIỀU KIỆN

    TechAgent->>UI: Nhập Bảng linh kiện `Ticket_Repair_Item` (Lỗi: Mainboard Nguồn)
    TechAgent->>UI: Chọn Resolution Type: "Đổi máy mới 100%" & Nhập `root_cause`
    TechAgent->>UI: Nhấn nút [Hoàn tất / Resolve]

    UI->>Ctrl: trigger_workflow_action("Mark Resolved")
    
    activate Ctrl
    Note over Ctrl,AppleAPI: KIỂM TRA ĐIỀU KIỆN NGHIỆM THU (VALIDATION 1 & 2)
    alt Thiếu phương án hoặc nguyên nhân lỗi
        Ctrl-->>UI: Báo lỗi: "Bắt buộc nhập Phương án giải quyết và Báo cáo nguyên nhân!"
    else Đủ thông tin chẩn đoán
        Ctrl->>AppleAPI: Tra cứu trạng thái Khóa kích hoạt iCloud / Find My
        AppleAPI-->>Ctrl: Status: iCloud OFF (Hợp lệ) | Apple Care: Active
        
        Ctrl->>DB: Cập nhật status = 'Resolved', resolved_time = now()
        Ctrl->>DB: Ghi log `Ticket_Activity_Log` (Trạng thái: In_Progress -> Resolved)
        Ctrl-->>StoreAgent: Thông báo: "Máy đã duyệt đổi mới! Vui lòng xuất thân máy mới bàn giao cho khách."
    end
    deactivate Ctrl

    Note over StoreAgent,Customer: CSKH Showroom xuất thân máy mới (IMEI mới: 358941098999888) giao khách ký biên nhận

    StoreAgent->>UI: Nhập `resolution_notes` (Đã giao máy mới kèm IMEI mới) -> Nhấn [Bàn giao & Đóng phiếu]
    UI->>Ctrl: trigger_workflow_action("Customer Accept & Close")

    activate Ctrl
    Note over Ctrl,DB: KIỂM TRA ĐIỀU KIỆN ĐÓNG PHIẾU (VALIDATION 3)
    alt resolution_notes để trống
        Ctrl-->>UI: Báo lỗi: "Bắt buộc nhập Ghi chú khắc phục xác nhận bàn giao trước khi đóng phiếu!"
    else Đã nhập đầy đủ biên bản bàn giao
        Ctrl->>DB: 1. Cập nhật status = 'Closed', closed_time = now()
        Ctrl->>DB: 2. Cập nhật thiết bị sở hữu trong `Customer 360` (Bổ sung IMEI mới xuất kho)
        Ctrl->>DB: 3. Ghi log `Ticket_Activity_Log` (Trạng thái: Resolved -> Closed)
        
        Ctrl->>ZNS: 4. Gọi API gửi tin nhắn Zalo ZNS CSAT Survey (Đính kèm link chấm 1-5 Sao)
        ZNS-->>Customer: Gửi tin nhắn Zalo: "CellphoneS cảm ơn quý khách. Xin đánh giá dịch vụ hỗ trợ TCK-0155..."
        
        Customer->>ZNS: Chấm điểm 5 Sao ⭐⭐⭐⭐⭐ kèm phản hồi "Đổi máy rất nhanh, dịch vụ tuyệt vời"
        ZNS->>Ctrl: Webhook Callback (ticket_id: TCK-0155, score: 5_Sao)
        Ctrl->>DB: Cập nhật `Support_Ticket.csat_score = '5_Sao'`
        Ctrl-->>UI: Cập nhật Dashboard CSKH Real-time
    end
    deactivate Ctrl
```

### 3. Ghi chú Thiết kế & Xử lý Luồng Ngoại lệ (Exception Handling)
* **Khách chưa thoát tài khoản iCloud:** Nếu Apple API trả về `iCloud = ON`, hệ thống lập tức chặn hành động `Mark Resolved`, hiển thị cảnh báo đỏ trên giao diện và hướng dẫn nhân viên hỗ trợ khách hàng đăng nhập `icloud.com/find` để gỡ bỏ thiết bị khỏi tài khoản từ xa trước khi tiến hành đổi máy.
* **Máy bị lỗi do người dùng (Rơi vỡ / Vào nước):** Kỹ thuật viên DTV chọn `resolution_type = 'Tu_choi_do_roi_vo'`, đính kèm ảnh chụp quỳ tím đổi màu vào bảng con `Ticket_Repair_Item`. Hệ thống tự động chuyển diện sang "Sửa chữa có phí", lập báo giá `estimated_cost` và gửi SMS xin ý kiến duyệt giá từ khách hàng.
* **Webhook Zalo ZNS Timeout:** Nếu cổng Zalo không phản hồi trong 5 giây, giao diện vẫn cho phép hoàn tất đóng phiếu và đưa tác vụ gửi tin nhắn CSAT vào hàng đợi nền (`frappe.enqueue`) để thử lại tự động theo cơ chế Exponential Backoff.
