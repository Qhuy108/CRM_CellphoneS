# SẢN PHẨM BÀN GIAO D05: SEQUENCE DIAGRAMS CHỌN LỌC & GHI CHÚ THIẾT KẾ
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D05  
**Thuộc nhiệm vụ:** Nhiệm vụ 6 (Vẽ 1–2 Sequence Diagram cho luồng xử lý chính)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Trạng thái:** Bản nháp Mốc M3 (Draft for Cross-check)  
**Nhãn xác nhận:** **Thiết kế đề xuất (Proposed Design)**

---

## TỔNG QUAN CẤU TRÚC BIỂU ĐỒ TUẦN TỰ (UML SEQUENCE ARCHITECTURE)

Các biểu đồ tuần tự được thiết kế theo mô hình 4 tầng phân lập rõ ràng:
1. **Tác nhân (Actor / User):** Khách hàng, Nhân viên Telesales, Kỹ thuật viên Điện Thoại Vui, Quản lý CSKH.
2. **Tầng Giao diện (UI Layer):** Desk Form View, POS Checkout, Call Pop-up.
3. **Tầng Điều khiển Nghiệp vụ (Controller / Backend):** Frappe Controller, Server Scripts, Workflow Engine.
4. **Tầng Cơ sở Dữ liệu & Dịch vụ Ngoài (Database & External Services):** MariaDB, Cổng thanh toán VnPay/MoMo, Zalo ZNS Gateway, Hệ thống Tra cứu Apple Care.

---

## LUỒNG 1: QUY TRÌNH ĐẶT TRƯỚC (PRE-ORDER) IPHONE FLAGSHIP TỪ TELESALES SANG ĐƠN HÀNG

### 1. Bối cảnh Nghiệp vụ CellphoneS
Vào mùa ra mắt iPhone mới (hoặc Galaxy S Flagship), hàng chục nghìn khách hàng để lại thông tin đặt trước trên Website. Nhân viên Telesales tiếp nhận danh sách Lead từ hệ thống CRM, gọi điện tư vấn phiên bản/màu sắc, chốt đặt cọc và chuyển đổi Lead thành Khách hàng chính thức cùng Đơn hàng bán (Sales Order) để giữ suất nhận máy đợt 1.

### 2. Biểu đồ Sequence Diagram (Proposed Design)

```mermaid
sequenceDiagram
    autonumber
    actor Telesales as 👤 Nhân viên Telesales
    participant UI as 🖥️ Lead Form (Desk UI)
    participant Ctrl as ⚙️ Lead Controller (Frappe Backend)
    participant DB as 🗄️ Cơ sở Dữ liệu (MariaDB)
    participant Pay as 💳 Cổng Cọc / POS Gateway
    participant Notif as 📲 Zalo ZNS / SMS Service

    Note over Telesales,Notif: THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN) - LUỒNG PRE-ORDER IPHONE FLAGSHIP

    Telesales->>UI: Mở danh sách Lead "iPhone 16 Pro Max Pre-order"
    UI->>Ctrl: Lấy danh sách Lead được phân công (Owner = Current User)
    Ctrl->>DB: Query tabLead WHERE status='Open' AND lead_type='Pre_Order'
    DB-->>Ctrl: Danh sách 20 Leads
    Ctrl-->>UI: Hiển thị giao diện Telesales Call Queue

    Telesales->>UI: Nhấn "Bắt đầu gọi" & Cập nhật kết quả tư vấn (Chốt bản 256GB Titan Sa Mạc)
    Telesales->>UI: Nhấn nút [⚡ Chốt cọc & Chuyển đổi Đơn hàng]
    
    UI->>Ctrl: Trigger convert_preorder_lead(lead_id, item_sku, deposit_amount, store_id)
    
    Ctrl->>DB: Kiểm tra Hạn mức Suất đặt trước (Check Pre-order Quota per Store)
    alt Hết suất nhận máy đợt 1
        DB-->>Ctrl: Quota = 0 (Đã hết suất đợt 1)
        Ctrl-->>UI: Cảnh báo: "Đã hết suất đợt 1. Khách cần chuyển sang đợt nhận 2!"
        Telesales->>UI: Xác nhận đồng ý nhận đợt 2 với khách
    else Còn suất nhận máy đợt 1
        DB-->>Ctrl: Quota > 0 (Hợp lệ)
    end

    Ctrl->>Pay: Tạo liên kết thanh toán đặt cọc 1,000,000đ (VnPay/VietQR Link)
    Pay-->>Ctrl: Trả về QR Code / Payment URL
    Ctrl-->>UI: Hiển thị QR Code cọc & Gửi link SMS cho khách

    Note over Telesales,Pay: Khách hàng quét mã QR chuyển khoản tiền cọc 1.000.000đ

    Pay->>Ctrl: Webhook Callback: Đã nhận cọc thành công (Transaction_ID: VNP-998811)
    
    activate Ctrl
    Ctrl->>DB: 1. Kiểm tra SĐT khách (Query tabCustomer WHERE phone_number = lead.mobile_no)
    alt Khách hàng chưa có hồ sơ
        Ctrl->>DB: Tạo mới `Customer` (CUST-2026-XXXXX) & Khởi tạo `Smember_Profile`
    else Khách hàng đã tồn tại
        Ctrl->>DB: Lấy `customer_id` hiện hữu
    end

    Ctrl->>DB: 2. Tạo Đơn hàng đặt trước (`Sales Order` - Trạng thái: Pre-order Paid)
    Ctrl->>DB: 3. Cập nhật `Lead`: status = 'Converted', converted_customer = CUST_ID
    Ctrl->>DB: 4. Giảm số lượng Quota đợt 1 của Showroom tiếp nhận (-1 suất)
    deactivate Ctrl

    Ctrl->>Notif: Kích hoạt gửi Zalo ZNS: "Xác nhận đặt cọc thành công suất iPhone 16 Pro Max đợt 1"
    Notif-->>Telesales: Báo trạng thái gửi ZNS thành công

    Ctrl-->>UI: Thông báo: "Chuyển đổi thành công! Đơn hàng: SO-2026-09823"
    UI-->>Telesales: Đổi giao diện sang chi tiết Đơn hàng đã chốt cọc
```

### 3. Ghi chú Thiết kế & Luồng Ngoại lệ (Exception Handling)
- **Kiểm soát Hạn mức (Allotment / Quota Control):** Hệ thống khóa transaction bằng `SELECT FOR UPDATE` khi kiểm tra hạn mức đặt trước tại từng Showroom để tránh tình trạng bán vượt quá số lượng Apple phân bổ (Over-booking).
- **Timeout Thanh toán cọc:** Nếu sau 15 phút khách chưa thanh toán cọc qua QR Code, suất đặt giữ tạm tự động được giải phóng lại vào kho chung.

---

## LUỒNG 2: XỬ LÝ ĐỔI TRẢ 1-ĐỔI-1 TRONG 30 NGÀY CHO HỘI VIÊN S-VIP & ĐÓNG PHIẾU CSAT

### 1. Bối cảnh Nghiệp vụ CellphoneS
Đặc quyền hội viên **Smember VIP (S-VIP)** tại CellphoneS là chính sách "Lỗi là đổi mới 100% trong vòng 30 ngày đầu" kèm cam kết giải quyết khiếu nại trong 4 giờ làm việc. Quy trình này đòi hỏi sự phối hợp chặt chẽ giữa CSKH Showroom tiếp nhận, Kỹ thuật viên Điện Thoại Vui thẩm định phần cứng và tự động gửi tin nhắn Zalo ZNS mời đánh giá dịch vụ.

### 2. Biểu đồ Sequence Diagram (Proposed Design)

```mermaid
sequenceDiagram
    autonumber
    actor Customer as 👤 Khách hàng S-VIP
    actor StoreAgent as 👔 CSKH Showroom
    actor TechAgent as 🛠️ KTV Điện Thoại Vui
    participant UI as 🖥️ Ticket Form (Desk UI)
    participant Ctrl as ⚙️ Ticket Workflow Engine
    participant AppleAPI as 🌐 Tra cứu Apple Care / IMEI DB
    participant DB as 🗄️ Cơ sở Dữ liệu (MariaDB)
    participant ZNS as 📲 Zalo ZNS Service

    Note over Customer,ZNS: THIẾT KẾ ĐỀ XUẤT (PROPOSED DESIGN) - LUỒNG ĐỔI TRẢ 1-ĐỔI-1 VIP 30 NGÀY & CSAT

    Customer->>StoreAgent: Mang máy lỗi (iPhone 15 Pro Max) yêu cầu đổi mới theo quyền S-VIP
    StoreAgent->>UI: Mở Form [Tạo Phiếu Hỗ Trợ] & Quét IMEI: 358941098234112
    
    UI->>Ctrl: validate_and_fetch_vip_policy(imei, customer_id)
    Ctrl->>DB: Query thông tin mua hàng & Hạng Smember
    DB-->>Ctrl: Hạng S-VIP | Ngày mua: 10 ngày trước (< 30 ngày)
    
    Ctrl->>AppleAPI: Tra cứu trạng thái kích hoạt & Khóa iCloud/FindMy
    AppleAPI-->>Ctrl: Apple Care Active | iCloud: Đã tắt (Sẵn sàng đổi)
    
    Ctrl-->>UI: Xác nhận: "Đủ điều kiện chính sách Đổi mới 1-1 S-VIP | SLA cam kết: 4 Giờ"
    StoreAgent->>UI: Nhập mô tả lỗi, đính kèm ảnh ngoại quan -> Nhấn [Tiếp nhận & Chuyển DTV]

    UI->>Ctrl: Trigger Workflow: Open -> In_Progress
    Ctrl->>DB: Tạo Ticket (TCK-2026-0155), set priority='Critical', sla_deadline = now() + 4h
    Ctrl->>DB: Ghi log vào `Ticket_Activity_Log`
    Ctrl-->>TechAgent: Gửi thông báo Desktop/Mobile: "Phiếu S-VIP khẩn cấp cần thẩm định!"

    TechAgent->>UI: Mở Ticket -> Thao tác kiểm tra phần cứng -> Nhập bảng `Ticket_Repair_Item`
    Note over TechAgent,UI: Xác định: Lỗi chập IC nguồn trên bo mạch (Lỗi phần cứng NSX)
    
    TechAgent->>UI: Chọn Resolution Type: "Đổi máy mới 100%" & Nhập `root_cause` -> Nhấn [Hoàn tất / Resolve]
    UI->>Ctrl: validate_resolution_and_resolve()
    
    Ctrl->>DB: Kiểm tra: root_cause NOT NULL & resolution_type NOT NULL
    Ctrl->>DB: Cập nhật status = 'Resolved', resolved_time = now()
    Ctrl-->>StoreAgent: Báo hoàn tất thẩm định: Sẵn sàng xuất máy mới tại kho Showroom

    StoreAgent->>UI: Xuất thân máy mới nguyên seal (IMEI mới: 358941098999888) -> Bàn giao cho khách
    Customer->>StoreAgent: Kiểm tra máy mới, ký nhận biên bản bàn giao
    
    StoreAgent->>UI: Nhập `resolution_notes` (Đã giao máy mới kèm IMEI) -> Nhấn [Bàn giao & Đóng phiếu]
    UI->>Ctrl: Trigger Workflow: Resolved -> Closed
    
    activate Ctrl
    Ctrl->>DB: Cập nhật status = 'Closed', closed_time = now()
    Ctrl->>DB: Cập nhật thông tin thiết bị sở hữu trong `Customer 360` (Bổ sung IMEI mới)
    
    Ctrl->>ZNS: Trigger Webhook gửi Zalo ZNS CSAT Survey (Đính kèm link chấm 1-5 Sao)
    deactivate Ctrl

    ZNS->>Customer: Gửi tin nhắn Zalo: "CellphoneS cảm ơn quý khách. Xin vui lòng đánh giá dịch vụ hỗ trợ TCK-0155..."
    Customer->>ZNS: Chấm điểm 5 Sao ⭐⭐⭐⭐⭐ kèm lời khen "Xử lý rất nhanh và chuyên nghiệp"
    ZNS->>Ctrl: Webhook Feedback Received (Score: 5)
    Ctrl->>DB: Cập nhật `Support_Ticket.csat_score = '5_Sao'`
    Ctrl-->>UI: Cập nhật Dashboard CSKH Real-time
```

### 3. Ghi chú Thiết kế & Luồng Ngoại lệ (Exception Handling)
- **Xử lý Điều kiện Ràng buộc iCloud / Find My:** Trước khi duyệt đổi máy Apple, hệ thống tích hợp API kiểm tra trạng thái Khóa kích hoạt. Nếu khách chưa thoát iCloud, giao diện chặn hành động `Resolve` và yêu cầu nhân viên hướng dẫn khách tắt từ xa qua icloud.com.
- **Xử lý Ngoại quan Không đạt chuẩn (Cấn móp / Vào nước):** Nếu kỹ thuật viên phát hiện máy bị cấn móp nặng làm biến dạng khung sườn hoặc quỳ tím đổi màu (vào nước), hệ thống cho phép KTV chuyển phương án sang `Tu_choi_do_roi_vo`, ghi rõ lý do kèm ảnh chụp và tự động gửi SMS thông báo từ chối đổi 1-1 cho khách hàng.
- **Xử lý Timeout Webhook Zalo ZNS:** Nếu cổng ZNS không phản hồi sau 5 giây, giao diện vẫn hoàn tất đóng phiếu và đưa tin nhắn CSAT vào hàng đợi ngầm (Background Queue: `frappe.enqueue`) để tự động thử lại sau 3 phút.
