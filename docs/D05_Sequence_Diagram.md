# SẢN PHẨM BÀN GIAO D05: SEQUENCE DIAGRAMS CHỌN LỌC & GHI CHÚ THIẾT KẾ
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D05  
**Thuộc nhiệm vụ:** Nhiệm vụ 6 (Vẽ 1–2 Sequence Diagram cho luồng xử lý chính có kiểm tra điều kiện)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Nền tảng mục tiêu:** Frappe Framework v15.x (Custom App `cellphones_crm` độc lập)  
**Trạng thái:** Hoàn thiện Mốc M4 (Final Deliverable)  
**Nhãn xác nhận:** **THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN)**

---

## TỔNG QUAN CẤU TRÚC BIỂU ĐỒ TUẦN TỰ (UML SEQUENCE ARCHITECTURE)

Các biểu đồ tuần tự được thiết kế theo mô hình 4 tầng phân lập rõ ràng, đồng bộ 100% với Use Case, Từ điển dữ liệu (D02), Wireframe (D03) và Ma trận phân quyền Frappe (D04):
1. **Tác nhân (Actor / User):** Khách hàng Smember, Bán hàng & CSKH cửa hàng, Kỹ thuật viên đối tác / CareS, Quản lý cửa hàng.
2. **Tầng Giao diện (UI Layer):** Form Tiếp nhận Ticket (Desk Form View), Pop-up Nghiệm thu & Kết quả giải quyết.
3. **Tầng Điều khiển Nghiệp vụ (Controller / Backend):** Frappe Server Script Controller, Workflow Engine, Routing Service.
4. **Tầng Cơ sở Dữ liệu & Dịch vụ Ngoài (Database & External Services):** MariaDB, Cổng tra cứu Apple Care API, Dữ liệu đơn hàng tham chiếu.

---

## LUỒNG 1: TIẾP NHẬN & PHÂN CÔNG PHIẾU HỖ TRỢ CÓ ĐIỀU KIỆN (TẠO & ĐIỀU PHỐI THEO SHOWROOM)

### 1. Bối cảnh Nghiệp vụ CellphoneS
Khi khách hàng mang thiết bị đến Showroom CellphoneS yêu cầu bảo hành/đổi trả (hoặc liên hệ Hotline), nhân viên Bán hàng & CSKH cửa hàng mở phiếu tiếp nhận. Hệ thống kiểm tra thông tin khách hàng, hạng Smember (`S-NULL`, `S-NEW`, `S-MEM`, `S-VIP`) và nhóm giáo dục (`S-Student`/`S-Teacher`), tính toán mức độ ưu tiên và điều phối phân công cho nhân viên xử lý hoặc chuyển đơn vị bảo hành/CareS.

### 2. Biểu đồ Sequence Diagram (Proposed Design)

```mermaid
sequenceDiagram
    autonumber
    actor StoreAgent as 👔 Bán hàng & CSKH cửa hàng
    participant UI as 🖥️ Ticket Form (Desk UI)
    participant Ctrl as ⚙️ Ticket Controller (Frappe Backend)
    participant DB as 🗄️ Cơ sở Dữ liệu (MariaDB)
    actor TechAgent as 🛠️ KTV / CSKH Phụ trách

    Note over StoreAgent,TechAgent: THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN) - LUỒNG TIẾP NHẬN & PHÂN CÔNG PHIẾU HỖ TRỢ

    StoreAgent->>UI: Nhập thông tin phiếu: SĐT (0908123456), SKU (SP-IP15PM-256), IMEI (358941098234112), Loại (Lỗi nguồn)
    StoreAgent->>UI: Nhấn nút [Tiếp nhận & Tạo Phiếu]
    
    UI->>Ctrl: submit_new_ticket(ticket_data)
    
    activate Ctrl
    Ctrl->>DB: 1. Kiểm tra SĐT trong tabCustomer & tabSmember Profile
    DB-->>Ctrl: Khách: Nguyễn Văn An | Hạng: ⭐ S-VIP | Chi tiêu: 85.4 triệu
    
    Ctrl->>Ctrl: 2. Thiết lập mức ưu tiên và thời hạn xử lý
    alt Khách hàng là S-VIP
        Ctrl->>Ctrl: Set priority = 'Critical', Phân luồng tiếp nhận ưu tiên
    else Khách hàng S-MEM / S-NEW / S-NULL
        Ctrl->>Ctrl: Set priority = 'Medium'
    end

    Ctrl->>Ctrl: 3. Kiểm tra phân loại vấn đề & Điều phối đơn vị phối hợp
    alt issue_category IN ('Tiep_nhan_bao_hanh', 'Doi_tra_theo_chinh_sach')
        Ctrl->>DB: Truy vấn nhân viên tiếp nhận kỹ thuật thuộc Showroom/CareS
        DB-->>Ctrl: KTV khả dụng: TranVanTu (DTV/CareS Q9)
        Ctrl->>Ctrl: Set partner_unit = 'Trung_tam_CareS', allocated_to = 'TranVanTu'
        Ctrl->>Ctrl: Set status = 'In_Progress' (Bắt đầu kiểm tra)
    else issue_category == 'Khieu_nai_dich_vu'
        Ctrl->>DB: Truy vấn Quản lý Showroom tiếp nhận
        DB-->>Ctrl: Quản lý: StoreManager_Linh
        Ctrl->>Ctrl: Set partner_unit = 'CSKH_Showroom', allocated_to = 'StoreManager_Linh'
        Ctrl->>Ctrl: Set status = 'Open'
    end

    Ctrl->>DB: 4. Lưu bản ghi `Support_Ticket` (TCK-2026-00155)
    Ctrl->>DB: 5. Ghi nhật ký vào bảng con `Ticket_Activity_Log` (Action: Tiep_nhan_va_phan_cong)
    
    Ctrl-->>TechAgent: Gửi thông báo hệ thống: "Phiếu TCK-2026-00155 đã được phân công xử lý!"
    deactivate Ctrl

    Ctrl-->>UI: Trả về kết quả thành công kèm Mã Ticket & Trạng thái
    UI-->>StoreAgent: Hiển thị thông báo: "Tạo phiếu thành công! Đã chuyển tiếp nhận kiểm tra tại Showroom."
```

### 3. Ghi chú Thiết kế & Xử lý Luồng Ngoại lệ (Exception Handling)
* **Khách hàng vãng lai không cung cấp SĐT:** Hệ thống tự động gán `customer_id = 'CUST-GUEST'`, `contact_phone = '0000000000'`, đặt mức ưu tiên `priority = 'Low'`.
* **Khách hàng đến từ cửa hàng khác (O03):** Giữ nguyên mã khách hàng duy nhất; nhân viên Showroom B chỉ được xem thông tin hồ sơ và các ticket cần thiết được phân công, không hiển thị toàn bộ lịch sử nội bộ ngoài phạm vi cửa hàng.

---

## LUỒNG 2: NGHIỆM THU KỸ THUẬT, ĐỐI CHIẾU CHÍNH SÁCH & ĐÓNG PHIẾU HỖ TRỢ CÓ ĐIỀU KIỆN

### 1. Bối cảnh Nghiệp vụ CellphoneS
Sau khi thiết bị được kiểm định lỗi phần cứng do nhà sản xuất (IC nguồn hỏng) trên máy iPhone của hội viên trong thời hạn chính sách, hệ thống thực hiện đối chiếu điều kiện (chính sách hãng và gói AppleCare+), cập nhật kết quả giải quyết (`resolution_result`), ghi nhận thông báo cho khách và đóng phiếu.

### 2. Biểu đồ Sequence Diagram (Proposed Design)

```mermaid
sequenceDiagram
    autonumber
    actor TechAgent as 🛠️ KTV / CSKH Phụ trách
    actor StoreAgent as 👔 Bán hàng & CSKH cửa hàng
    participant UI as 🖥️ Ticket Detail Form (Desk UI)
    participant Ctrl as ⚙️ Ticket Workflow Engine
    participant AppleAPI as 🌐 Tra cứu Apple Care / IMEI
    participant DB as 🗄️ Cơ sở Dữ liệu (MariaDB)
    actor Customer as 👤 Khách hàng

    Note over TechAgent,Customer: THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN) - LUỒNG NGHIỆM THU & ĐÓNG PHIẾU CÓ ĐIỀU KIỆN

    TechAgent->>UI: Nhập Bảng linh kiện `Ticket_Repair_Item` (Lỗi: Mainboard Nguồn)
    TechAgent->>UI: Chọn Kết quả giải quyết: "Đổi theo chính sách" & Nhập `resolution_notes`
    TechAgent->>UI: Nhấn nút [Hoàn tất / Resolve]

    UI->>Ctrl: trigger_workflow_action("Mark Resolved")
    
    activate Ctrl
    Note over Ctrl,AppleAPI: KIỂM TRA ĐIỀU KIỆN NGHIỆM THU (VALIDATION 1 & 2)
    alt Thiếu kết quả giải quyết hoặc ghi chú xử lý
        Ctrl-->>UI: Báo lỗi: "Bắt buộc chọn Kết quả giải quyết (resolution_result) và nhập Ghi chú xử lý!"
    else Đủ thông tin chẩn đoán
        Ctrl->>AppleAPI: Tra cứu trạng thái bảo hành chính hãng / AppleCare+
        AppleAPI-->>Ctrl: Status: AppleCare+ Active (Hợp lệ)
        
        Ctrl->>DB: Cập nhật status = 'Resolved', resolution_result = 'Doi_theo_chinh_sach'
        Ctrl->>DB: Ghi log `Ticket_Activity_Log` (Trạng thái: In_Progress -> Resolved)
        Ctrl-->>StoreAgent: Thông báo: "Đã duyệt phương án đổi theo chính sách! Vui lòng thông báo cho khách."
    end
    deactivate Ctrl

    Note over StoreAgent,Customer: Nhân viên CSKH Showroom gọi điện thông báo kết quả cho khách hàng hẹn ngày nhận máy

    StoreAgent->>UI: Cập nhật `notify_customer_status = 'Da_thong_bao_qua_dien_thoai'` & `customer_notified_at = now()`
    StoreAgent->>UI: Bàn giao máy cho khách -> Nhấn [Bàn giao & Đóng phiếu]
    UI->>Ctrl: trigger_workflow_action("Notify & Close")

    activate Ctrl
    Note over Ctrl,DB: KIỂM TRA ĐIỀU KIỆN ĐÓNG PHIẾU (VALIDATION 3)
    alt notify_customer_status == 'Chua_thong_bao'
        Ctrl-->>UI: Báo lỗi: "Bắt buộc ghi nhận đã thông báo cho khách trước khi Đóng phiếu!"
    else Đã ghi nhận thông báo khách
        Ctrl->>DB: 1. Cập nhật status = 'Closed', closed_time = now()
        Ctrl->>DB: 2. Ghi log `Ticket_Activity_Log` (Trạng thái: Resolved -> Closed)
        Ctrl-->>UI: Cập nhật trạng thái phiếu thành công [Closed]
    end
    deactivate Ctrl
```

### 3. Ghi chú Thiết kế & Xử lý Luồng Ngoại lệ (Exception Handling)
* **Khách chưa thoát tài khoản iCloud:** Nếu thiết bị kích hoạt khóa Find My / iCloud, nhân viên hướng dẫn khách hỗ trợ mở khóa trước khi thực hiện các thủ tục đổi máy/bảo hành.
* **Thời hạn 30 ngày và tình trạng máy:** Thời hạn 30 ngày không đồng nghĩa mọi yêu cầu đều được đổi miễn phí hoặc đổi máy nguyên seal. Nếu máy bị cấn móp, rơi vỡ hoặc không đạt điều kiện chính sách, nhân viên ghi nhận `resolution_result = 'Khong_du_dieu_kien'` hoặc chuyển diện sửa chữa có phí sau khi thỏa thuận với khách hàng.
* **Quy tắc Thông báo Khách hàng (MVP):** Trong giai đoạn MVP, CRM hỗ trợ nhân viên ghi nhận kết quả liên lạc trực tiếp qua điện thoại hoặc tại quầy thông qua trường `notify_customer_status`, đảm bảo mọi tương tác đều được lưu vết đầy đủ trước khi đóng phiếu.

