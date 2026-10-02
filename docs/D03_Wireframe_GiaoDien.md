# SẢN PHẨM BÀN GIAO D03: WIREFRAME & LAYOUT CÁC MÀN HÌNH ƯU TIÊN
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D03  
**Thuộc nhiệm vụ:** Nhiệm vụ 4 (Phác thảo 4–6 màn hình ưu tiên theo Use Case)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Nền tảng mục tiêu:** Frappe Desk UI v15 / ERPNext Web Client  
**Trạng thái:** Hoàn thiện Mốc M4 (Final Deliverable)

---

## TỔNG QUAN THIẾT KẾ GIAO DIỆN (DESK UI STANDARDS)

Toàn bộ 6 màn hình trọng tâm dưới đây được thiết kế bám sát chuẩn giao diện **Frappe Desk UI v15**, phục vụ các Use Case chính của CellphoneS:
1. Quản trị vòng đời khách hàng và quyền lợi Smember VIP.
2. Quản lý cơ hội tư vấn bán hàng đa kênh & đặt trước (Pre-order) Flagship.
3. Tiếp nhận tương tác tổng đài đa kênh & nhắc việc chăm sóc (Task Reminder).
4. Điều phối và xử lý phiếu hỗ trợ bảo hành - sửa chữa Điện Thoại Vui.
5. Nghiệm thu kỹ thuật, đối soát IMEI/Apple Care và giải trình đóng phiếu.
6. Giám sát thời gian thực chỉ số cam kết chất lượng dịch vụ (SLA & CSAT).

Mỗi màn hình được bóc tách chặt chẽ theo 4 cấu phần:
* **1. Thông tin hiển thị (Xem):** Dữ liệu chỉ đọc và nguồn gốc truy vấn.
* **2. Dữ liệu thao tác (Nhập):** Các ô nhập liệu, bộ lọc, danh sách chọn và bảng con.
* **3. Hành động chính (Actions):** Các nút bấm chức năng kích hoạt quy trình nghiệp vụ.
* **4. Thông báo lỗi quan trọng (Important Validation & Error Alerts):** Cảnh báo khi người dùng thao tác sai hoặc vi phạm ràng buộc dữ liệu.

---

## MÀN HÌNH 1: HỒ SƠ KHÁCH HÀNG 360° & DANH SÁCH (CUSTOMER 360° PROFILE VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Header Profile:** Mã KH (`customer_id`), Họ tên, SĐT, Email, CCCD, Địa chỉ, Ngày tham gia.<br>• **Thẻ Smember VIP:** Badge Hạng mức (`Smember` / `⭐ S-VIP`), Điểm tích lũy khả dụng (`reward_points`), Tổng chi tiêu lũy kế (`total_spent`), Hạn duy trì hạng VIP.<br>• **Tab 1 - Thiết bị sở hữu / Lịch sử mua hàng:** Danh sách máy đã mua lấy từ `Sales_Invoice_Reference` (Mã SKU, Tên máy, Số IMEI/Serial, Ngày mua, Showroom xuất bán, Hạn bảo hành gốc).<br>• **Tab 2 - Lịch sử Phiếu hỗ trợ:** Danh sách toàn bộ Ticket đã mở (Mã TCK, Lỗi phần cứng, Trạng thái 5 State, Đơn vị Điện Thoại Vui tiếp nhận).<br>• **Tab 3 - Dòng thời gian Tương tác (Timeline):** Lịch sử các cuộc gọi Hotline 1800, Chat Zalo OA, Showroom. |
| **2. Dữ liệu thao tác (Nhập)** | • Form cập nhật thông tin liên hệ (Địa chỉ nhận hàng, Email, Tỉnh/Thành phố).<br>• Ô nhập ghi chú đặc điểm CSKH (Khách hàng VIP khó tính, ưu tiên gọi buổi tối...). |
| **3. Hành động chính (Actions)** | • **[+ Tạo Phiếu Hỗ Trợ]:** Mở form tạo Ticket mới (tự động điền thông tin KH).<br>• **[+ Ghi Nhận Tương Tác]:** Mở cửa sổ nhật ký cuộc gọi/chat.<br>• **[⚡ Gộp Hồ Sơ (Merge)]:** Mở Modal gộp hồ sơ trùng lặp (chỉ CSKH Manager).<br>• **[⭐ Điều Chỉnh Điểm Smember]:** Modal thưởng/phạt điểm (Cần quyền Manager).<br>• **[Lưu Thay Đổi]:** Cập nhật dữ liệu vào cơ sở dữ liệu. |
| **4. Thông báo lỗi quan trọng** | • **Cảnh báo trùng SĐT:** *"Số điện thoại [0908123456] đã tồn tại trên hồ sơ CUST-2026-00001! Không thể tạo trùng."*<br>• **Lỗi gộp hồ sơ:** *"Không thể gộp hồ sơ vào chính nó hoặc hồ sơ phụ đã ở trạng thái Merged!"*<br>• **Lỗi quyền hạn:** *"Bạn không có quyền điều chỉnh điểm Smember (Yêu cầu quyền CSKH Manager)!"* |

### 2. Wireframe Mockup UI (Frappe Desk Form View - Tabbed)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Khách hàng > CUST-2026-00001                                    [⭐ S-VIP] [ Lưu thông tin ] |
+--------------------------------------------------------------------------------------------------------------+
| NGUYỄN VĂN AN                                                                   [+ Tạo Ticket] [+ Ghi tương tác]|
| SĐT: 0908123456  |  Email: an.nguyen@gmail.com  |  CCCD: 079098001234           [⚡ Gộp hồ sơ] [⭐ Chỉnh điểm]  |
| Địa chỉ: 125 Lê Văn Việt, P. Hiệp Phú, TP. Thủ Đức, TP.HCM                                                    |
+--------------------------------------------------------------------------------------------------------------+
| [ THÔNG TIN HỘI VIÊN SMEMBER ]                                                                               |
| Hạng thẻ: ⭐ S-VIP (Đặc quyền SLA 4H & Đổi mới 1-1 30 ngày) | Điểm khả dụng: 1,250 pts | Chi tiêu: 85,400,000 đ |
| Hạn duy trì hạng: 31/12/2026                                 | Ngày thăng hạng gần nhất: 15/01/2026           |
+--------------------------------------------------------------------------------------------------------------+
| [TAB 1: Thiết bị sở hữu / IMEI (3)] | [TAB 2: Phiếu hỗ trợ (2)] | [TAB 3: Nhật ký tương tác] | [TAB 4: Ghi chú] |
+--------------------------------------------------------------------------------------------------------------+
| • iPhone 15 Pro Max 256GB Titan Tự Nhiên | IMEI: 358941098234112 | HĐ: INV-2024-08912 | Mua: 20/09/2024 | BH: Còn 3 tháng|
| • Apple Watch Ultra 2 GPS + Cellular     | IMEI: 356712098451201 | HĐ: INV-2024-11024 | Mua: 15/11/2024 | BH: Còn 5 tháng|
| • Củ sạc Apple 20W Type-C Chính hãng     | SKU: SP-APP-20W       | HĐ: INV-2024-08912 | Mua: 20/09/2024 | BH: Hết hạn    |
+--------------------------------------------------------------------------------------------------------------+
| [ LỊCH SỬ PHIẾU HỖ TRỢ GẦN NHẤT ]                                                                            |
| - TCK-2026-00155: iPhone 15 Pro Max lỗi nguồn -> [🟡 In Progress] -> Đang kiểm định tại Điện Thoại Vui Q9     |
| - TCK-2026-00089: Củ sạc Apple 20W không vào điện -> [🟢 Closed] -> Đã đổi mới tại quầy Showroom             |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 2: QUẢN LÝ CƠ HỘI TƯ VẤN & ĐẶT TRƯỚC PRE-ORDER (LEAD & PRE-ORDER VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • Mã Lead (`lead_id`), Tên khách hàng, SĐT liên hệ, Email.<br>• Nguồn tiếp nhận (`Website_Preorder`, `Facebook_Ads`, `Zalo OA`).<br>• Thiết bị khách quan tâm (Mã SKU, Phiên bản dung lượng, Màu sắc mong muốn).<br>• Showroom nhận máy mong muốn và số lượng suất cọc đợt 1 còn khả dụng (Pre-order Quota).<br>• Lịch sử các lần gọi tư vấn Telesales trước đó. |
| **2. Dữ liệu thao tác (Nhập)** | • Form nhập kết quả tư vấn cuộc gọi: Trạng thái Lead (`Open` $\rightarrow$ `Contacted` $\rightarrow$ `Qualified` $\rightarrow$ `Converted` $\rightarrow$ `Lost`).<br>• Chọn phiên bản máy chốt cọc và số tiền cọc (Mặc định: 1,000,000đ).<br>• Ô ghi chú chi tiết nhu cầu quà tặng / thu cũ đổi mới (Trade-in). |
| **3. Hành động chính (Actions)** | • **[Bắt đầu cuộc gọi Telesales]:** Tích hợp CTI tự động quay số tới SĐT khách.<br>• **[⚡ Tạo QR Thanh Toán Cọc]:** Sinh mã VietQR / VnPay gửi qua SMS/Zalo cho khách.<br>• **[🎯 Chốt Cọc & Chuyển Đổi Đơn Hàng]:** Chuyển Lead thành Khách hàng (`Customer`) và Đơn hàng đặt trước (`Sales Order`).<br>• **[Đóng Lead (Lost)]:** Đánh dấu thất bại kèm lý do (Giá cao, Đổi ý, Không liên lạc được). |
| **4. Thông báo lỗi quan trọng** | • **Hết suất nhận máy:** *"Chi nhánh 125 Lê Văn Việt đã hết suất nhận máy đợt 1! Vui lòng tư vấn khách nhận đợt 2 hoặc chuyển Showroom khác."*<br>• **Chưa nhận tiền cọc:** *"Chưa nhận được xác nhận thanh toán cọc 1,000,000đ từ cổng thanh toán! Không thể chuyển đổi đơn hàng."*<br>• **Lỗi liên kết KH:** *"Bắt buộc phải tạo hoặc liên kết hồ sơ Khách hàng có cùng số điện thoại trước khi hoàn tất chuyển đổi!"* |

### 2. Wireframe Mockup UI (Frappe Desk Lead Form View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Nhu cầu Tư vấn > LEAD-2026-08912           [ Trang thái: 🟡 CONTACTED ] [ 📞 Gọi Telesales ] |
+--------------------------------------------------------------------------------------------------------------+
| KHÁCH HÀNG: Trần Thị Mai Loan (SĐT: 0938112233)              KÊNH TIẾP NHẬN: [ Website Pre-order (Form)   ▼ ]|
| SẢN PHẨM: iPhone 16 Pro Max 256GB Sa Mạc (SP-IP16PM-256-DES) SHOWROOM NHẬN: [ Showroom 125 Lê Văn Việt   ▼ ]|
| SUẤT ĐẶT TRƯỚC ĐỢT 1 TẠI CHI NHÁNH: [ 🟢 Còn 12 / 50 Suất ]  TIỀN CỌC QUY ĐỊNH: 1,000,000 đ                 |
+--------------------------------------------------------------------------------------------------------------+
| TIẾN TRÌNH TELESALES (STATE):                                                                                |
| ( ) 1. Mới tạo (Open)   (•) 2. Đã liên hệ (Contacted)   ( ) 3. Đủ điều kiện (Qualified)   ( ) 4. Chốt cọc   |
+--------------------------------------------------------------------------------------------------------------+
| KẾT QUẢ TƯ VẤN CUỘC GỌI:                                                                                     |
| +----------------------------------------------------------------------------------------------------------+ |
| | Khách quan tâm màu Sa Mạc 256GB. Đã tư vấn chương trình trợ giá Thu cũ đổi mới iPhone 13 Pro Max lên đời.| |
| | Khách đồng ý đặt cọc giữ suất nhận máy ngày mở bán đầu tiên (Đợt 1). Yêu cầu gửi link VietQR cọc 1tr.    | |
| +----------------------------------------------------------------------------------------------------------+ |
+--------------------------------------------------------------------------------------------------------------+
| [ Hủy / Đóng Lead ]                [ ⚡ Tạo QR Code Cọc ]  [ 🎯 Chốt Cọc & Tạo Đơn Hàng ]  [ Lưu Ghi Chú ]  |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 3: GHI NHẬN TƯƠNG TÁC ĐA KÊNH & NHẮC VIỆC (INTERACTION & TASK REMINDER)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • Mã tương tác (`interaction_id`), Thời gian phát sinh cuộc gọi/chat.<br>• Pop-up CTI Caller-ID: Tự động hiển thị tên khách hàng, hạng Smember và lịch sử 3 lần liên hệ gần nhất nếu nhận diện được số điện thoại gọi đến.<br>• Thông tin nhân viên trực máy (`staff_agent`). |
| **2. Dữ liệu thao tác (Nhập)** | • **Khách hàng:** Chọn khách hàng (`customer_id` - Nullable nếu là khách vãng lai).<br>• **SĐT liên hệ:** `contact_phone` (Bắt buộc, tự điền từ CTI tổng đài).<br>• **Kênh tiếp nhận:** `Hotline_1800`, `Zalo_OA`, `Fanpage`, `Showroom_Store`.<br>• **Phân loại nhu cầu:** `Tu_van_mua_hang`, `Tra_cuu_don_hang`, `Bao_hanh_sua_chua`, `Khieu_nai_dich_vu`.<br>• **Nội dung tóm tắt:** Biên bản trao đổi ngắn gọn.<br>• **Tạo lịch nhắc việc (Follow-up Task):** Chọn ngày giờ hẹn gọi lại và nội dung công việc. |
| **3. Hành động chính (Actions)** | • **[Lưu Tương Tác]:** Ghi nhận vào nhật ký hệ thống.<br>• **[⚡ Tạo Nhanh Ticket]:** Mở ngay form tạo `Support_Ticket` và tự động sao chép thông tin khách/SĐT/nội dung.<br>• **[⏰ Tạo Lịch Nhắc Việc]:** Thêm nhiệm vụ vào lịch cá nhân của nhân viên.<br>• **[Hủy Bỏ].** |
| **4. Thông báo lỗi quan trọng** | • **Thiếu nội dung:** *"Vui lòng nhập Tóm tắt nội dung trao đổi trước khi lưu tương tác!"*<br>• **SĐT không hợp lệ:** *"Số điện thoại người gọi không đúng định dạng 10 chữ số chuẩn Việt Nam!"*<br>• **Lỗi lịch hẹn:** *"Thời gian hẹn gọi lại phải lớn hơn thời điểm hiện tại!"* |

### 2. Wireframe Mockup UI (Interaction Pop-up & Follow-up Task)

```text
+--------------------------------------------------------------------------------------------------------------+
| [GHI NHẬN TƯƠNG TÁC KHÁCH HÀNG] - INT-2026-00542                             [ Hotline 1800.2097 ] [ Đang gọi ]|
+--------------------------------------------------------------------------------------------------------------+
| SĐT người gọi (*): [ 0908123456             ]  Khách hàng: [ CUST-2026-00001 - Nguyễn Văn An (⭐ S-VIP)   ▼ ]|
| Tên người liên hệ: [ Nguyễn Văn An          ]  Kênh tiếp nhận (*): [ Hotline 1800                         ▼ ]|
+--------------------------------------------------------------------------------------------------------------+
| Phân loại tương tác (*):                                                                                     |
| ( ) Tư vấn mua hàng / Pre-order    (•) Khiếu nại bảo hành máy    ( ) Tra cứu giao hàng    ( ) Hỏi giá phụ kiện|
+--------------------------------------------------------------------------------------------------------------+
| Tóm tắt nội dung trao đổi (*):                                                                               |
| +----------------------------------------------------------------------------------------------------------+ |
| | Khách báo iPhone 15 Pro Max bị sập nguồn khi đang sạc, máy rất nóng. Đã hướng dẫn khách mang ra Showroom  | |
| | 125 Lê Văn Việt để Kỹ thuật Điện Thoại Vui kiểm tra theo diện S-VIP ưu tiên SLA 4 giờ.                   | |
| +----------------------------------------------------------------------------------------------------------+ |
+--------------------------------------------------------------------------------------------------------------+
| [⏰ TẠO LỊCH NHẮC VIỆC GỌI LẠI (FOLLOW-UP TASK)]                                                             |
| Ngày giờ hẹn gọi: [ 03/10/2026 14:00 📅 ]   Nội dung: [ Gọi lại hỏi thăm khách sau khi DTV thẩm định máy  ]|
+--------------------------------------------------------------------------------------------------------------+
| [ Hủy bỏ ]                                  [ Lưu tương tác ]  [ ⚡ Tạo nhanh Ticket ]  [ ⏰ Lưu Lịch Hẹn ]  |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 4: DANH SÁCH & KANBAN PHIẾU HỖ TRỢ (TICKET KANBAN & LIST VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Kanban Board 5 Cột chuẩn State Diagram C03 của Nhật:**<br>  1. `Open (Mới tạo)` $\rightarrow$ 2. `In Progress (Đang xử lý)` $\rightarrow$ 3. `Pending Vendor (Chờ LK/Hãng)` $\rightarrow$ 4. `Resolved (Đã giải quyết)` $\rightarrow$ 5. `Closed (Đóng phiếu)`.<br>• **Thông tin trên mỗi Card:** Mã Ticket, Tên khách, Model máy + IMEI, Tag SLA Timer đếm ngược (Màu đỏ nếu sắp trễ hạn), Nhãn ưu tiên (`Critical` / `High` / `S-VIP`), Avatar nhân viên phụ trách.<br>• **Thống kê nhanh:** Số phiếu vi phạm SLA, Số phiếu VIP đang xử lý. |
| **2. Dữ liệu thao tác (Nhập)** | • Ô lọc nhanh theo Mã Ticket, IMEI, Tên khách, SĐT.<br>• Bộ lọc phân loại: Phòng ban xử lý (`CSKH Showroom`, `Điện Thoại Vui`, `Hãng Apple/Samsung`), Mức độ ưu tiên (`Critical`, `High`, `Medium`, `Low`).<br>• Thao tác kéo thả Card giữa các cột trạng thái. |
| **3. Hành động chính (Actions)** | • **[+ Tạo Phiếu Hỗ Trợ Mới]:** Mở form tạo Ticket.<br>• **[Chuyển Chế Độ List / Kanban]:** Thay đổi giao diện xem.<br>• **[Gán Việc Nhanh (Quick Assign)]:** Phân công kỹ thuật viên trực tiếp trên Card.<br>• **[Lọc Phiếu Của Tôi (My Tickets)].** |
| **4. Thông báo lỗi quan trọng** | • **Vi phạm quy trình chuyển trạng thái:** *"Không thể kéo thẳng từ 'Open' sang 'Resolved' mà chưa qua bước tiếp nhận 'In Progress'!"*<br>• **Chưa phân công nhân viên:** *"Không thể chuyển sang 'In Progress' khi chưa chọn Kỹ thuật viên phụ trách!"* |

### 2. Wireframe Mockup UI (Frappe Desk Kanban View - 5 Cột)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Phiếu hỗ trợ (Kanban)                     [ Bộ lọc: Tất cả ▼ ] [ Chế độ: Kanban ▤ ] [+ Tạo Ticket]|
| SLA Cảnh báo: [ 🔴 2 Phiếu sắp quá hạn ] [ ⭐ 4 Phiếu S-VIP ]                 [ 🔍 Tìm mã ticket, IMEI, SĐT... ]|
+--------------------------------------------------------------------------------------------------------------+
| 1. MỚI TẠO (Open)   | 2. ĐANG XỬ LÝ (In Prog)| 3. CHỜ HÃNG/LK (Pending)| 4. ĐÃ XỬ LÝ (Resolved) | 5. ĐÓNG (Closed)   |
| (3 Phiếu)           | (5 Phiếu)              | (2 Phiếu)               | (4 Phiếu)              | (18 Phiếu)         |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
| [TCK-2026-00155] ⭐  | [TCK-2026-00150]        | [TCK-2026-00145]         | [TCK-2026-00142] ⭐     | [TCK-2026-00130]    |
| KH: Nguyễn Văn An   | KH: Lê Thị Bích Trâm   | KH: Đỗ Mỹ Linh          | KH: Hoàng Minh Quân    | KH: Bùi Anh Tuấn   |
| IP 15 Pro Max       | Galaxy S24 Ultra       | Asus ROG Zephyrus G16   | iPad Pro M4            | Loa Marshall       |
| IMEI: 3589410982... | IMEI: 3521098451...    | SN: K9N0CV124982        | IMEI: 3599810234...    | SN: MS20240912     |
| Lỗi: Nóng sập nguồn | Lỗi: Sọc màn hình      | Lỗi: Quạt kêu rè rè     | Xử lý: Đổi máy mới 1-1 | Hoàn tất bảo hành  |
| SLA: [⏱️ Còn 3h15p] | SLA: [⏱️ Còn 18h]      | Điểm tiếp nhận: Apple   | Chờ khách đến nhận máy | CSAT: ⭐⭐⭐⭐⭐     |
| Phụ trách: Chưa gán | Phụ trách: KTV DTV-Tú  | Phụ trách: KTV DTV-Hải  | Phụ trách: CSKH-Thuận  | Phụ trách: CSKH-My |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
| [TCK-2026-00156]     | [TCK-2026-00152] 🔴     |                         | [TCK-2026-00144]        |                    |
| KH: Phạm Minh Tâm   | KH: Vũ Đức Đam         |                         | KH: Trương Vĩnh Ký     |                    |
| Laptop Acer Nitro 5 | Xiaomi 14 Ultra        |                         | AirPods Pro 2          |                    |
| Lỗi: Treo logo      | Lỗi: Không nhận sạc    |                         | Xử lý: Đổi tai nghe R  |                    |
| SLA: [⏱️ Còn 22h]   | SLA: [🔴 QUÁ HẠN 45P]  |                         |                        |                    |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
```

---

## MÀN HÌNH 5: CHI TIẾT PHIẾU HỖ TRỢ & NGHIỆM THU KỸ THUẬT (TICKET DETAIL & REPAIR)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Header Status:** Mã Ticket (`TCK-YYYY-XXXXX`), Trạng thái hiện tại, Đồng hồ đếm ngược SLA, Badge Hội viên Smember.<br>• **Khối Thông tin Chung:** Khách hàng, SĐT, Sản phẩm lỗi (SKU, Tên máy, Số Serial/IMEI), Hóa đơn mua cũ, Hạn bảo hành Apple Care/Hãng.<br>• **Bảng con Linh kiện (`Ticket_Repair_Item`):** Bộ phận phát hiện lỗi, Tình trạng ngoại quan máy lúc nhận, Phụ kiện giữ lại, Diện bảo hành.<br>• **Dòng thời gian Xử lý (`Ticket_Activity_Log`):** Lịch sử chuyển trạng thái, ghi chú kỹ thuật DTV, tin nhắn gửi khách. |
| **2. Dữ liệu thao tác (Nhập)** | • Phân công xử lý: Chọn Kỹ thuật viên Điện Thoại Vui và Đơn vị chịu trách nhiệm.<br>• Thêm/sửa dòng kiểm tra linh kiện và báo giá ngoài bảo hành nếu có.<br>• **Form Nghiệm thu & Khắc phục (Bắt buộc khi Resolve/Close):**<br>  - `resolution_type`: Chọn Đổi mới 100%, Sửa chữa thay linh kiện, Bảo hành hãng, Từ chối.<br>  - `root_cause`: Nhập báo cáo nguyên nhân lỗi kỹ thuật.<br>  - `resolution_notes`: Ghi chú nội dung đã sửa chữa / IMEI thân máy đổi mới. |
| **3. Hành động chính (Actions)** | • **[Nhận Xử Lý (In Progress)]:** Kỹ thuật viên tiếp nhận máy vào quy trình chẩn đoán.<br>• **[Chuyển Hãng (Pending Vendor)]:** Gửi máy sang Apple Care / Samsung Service.<br>• **[Hoàn Tất / Resolve]:** Đánh dấu xong sửa chữa / duyệt đổi máy mới 1-1.<br>• **[Bàn Giao & Đóng Phiếu (Close)]:** Ký biên nhận bàn giao, kích hoạt Webhook gửi Zalo ZNS khảo sát CSAT.<br>• **[In Biên Nhận Tiếp Nhận / Trả Máy]:** Xuất file PDF giao khách. |
| **4. Thông báo lỗi quan trọng** | • **Thiếu giải trình nghiệm thu:** *"Bắt buộc phải chọn Phương án xử lý (resolution_type) và nhập Báo cáo nguyên nhân lỗi (root_cause) trước khi đánh dấu Hoàn tất (Resolved)!"*<br>• **Thiếu ghi chú bàn giao:** *"Bắt buộc phải nhập Ghi chú khắc phục (resolution_notes) xác nhận biên bản bàn giao trước khi Đóng phiếu (Closed)!"*<br>• **Chưa thoát tài khoản iCloud:** *"Thiết bị chưa thoát tài khoản iCloud / Find My! Không thể thực hiện thủ tục đổi máy mới."*<br>• **Máy bị vào nước/rơi vỡ:** *"Máy bị cấn móp góc/vào nước vi phạm điều kiện đổi mới 1-1! Vui lòng chuyển diện Sửa chữa có phí."* |

### 2. Wireframe Mockup UI (Frappe Desk Form View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Phiếu hỗ trợ > TCK-2026-00155           [⭐ S-VIP] [⏱️ SLA Còn: 03h 45m] [ In Phiếu Tiếp Nhận ]|
| Trạng thái: [🟡 IN PROGRESS]                                                [ Cập nhật ] [ HOÀN TẤT / RESOLVE ]|
+--------------------------------------------------------------------------------------------------------------+
| KHÁCH HÀNG: Nguyễn Văn An (SĐT: 0908123456)             ĐƠN VỊ XỬ LÝ: [ Trung tâm Điện Thoại Vui - Q9     ▼ ]|
| THIẾT BỊ: iPhone 15 Pro Max 256GB Titan Tự Nhiên         KỸ THUẬT PHỤ TRÁCH: [ DTV_TranVanTu (KTV Bậc 3)   ▼ ]|
| SỐ SERIAL / IMEI: 358941098234112  [🔍 Tra cứu Apple Care: Hạn BH đến 19/09/2025 - Chính hãng VN/A]         |
+--------------------------------------------------------------------------------------------------------------+
| [ BẢNG KIỂM TRA LINH KIỆN & NGOẠI QUAN MÁY ] (Child Table: Ticket_Repair_Item)                               |
+----+---------------------+----------------------+----------------------+------------------+---------------+
| No | Bộ phận phát hiện lỗi| Tình trạng ngoại quan| Phụ kiện giữ lại     | Diện bảo hành    | Chi phí phát sinh|
+----+---------------------+----------------------+----------------------+------------------+---------------+
| 1  | [ Mainboard Nguồn ▼]| [ Máy đẹp như mới  ▼]| [ Máy trần, Không sạc] | [ VIP 1-Đổi-1 30N▼]| [         0 đ ]|
+----+---------------------+----------------------+----------------------+------------------+---------------+
| [+ Thêm linh kiện kiểm tra]                                                                                  |
+--------------------------------------------------------------------------------------------------------------+
| [ THÔNG TIN NGHIỆM THU & GIẢI TRÌNH KHẮC PHỤC ]                                                               |
| Phương án giải quyết (*): [ Đổi máy mới 100% (Chính sách S-VIP 30 ngày)                                     ▼ ]|
| Báo cáo nguyên nhân (*): [ Chập IC nguồn sạc nhanh Type-C trên bo mạch chính do lỗi linh kiện nhà sản xuất.  ]|
| Ghi chú khắc phục (*):   [ Đã làm thủ tục xuất đổi thân máy mới nguyên seal IMEI: 358941098999888.           ]|
+--------------------------------------------------------------------------------------------------------------+
| [ DÒNG THỜI GIAN XỬ LÝ & NHẬT KÝ TRAO ĐỔI (TIMELINE) ]                                                      |
| • 10:15 - CSKH_Thuận: Tiếp nhận máy tại Showroom CellphoneS 125 Lê Văn Việt -> Trạng thái: Open.             |
| • 10:30 - Auto-routing: Nhận diện khách S-VIP -> Phân luồng ưu tiên sang KTV DTV-Tú (SLA 4h).                |
| • 11:00 - KTV_Tu: Bắt đầu chẩn đoán phần cứng -> Trạng thái: In Progress.                                    |
| [ Gửi tin nhắn SMS / Zalo ZNS thông báo tiến độ cho khách hàng: "Máy của anh/chị đang được kiểm tra..."    ] |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 6: DASHBOARD CSKH & BÁO CÁO HIỆU SUẤT DỊCH VỤ (CSKH ANALYTICS DASHBOARD)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **4 Thẻ chỉ số tổng quan (KPI Cards):**<br>  - Tỷ lệ hoàn thành đúng hạn SLA (% On-time SLA, Mục tiêu $\ge 95\%$).<br>  - Điểm hài lòng trung bình (CSAT Score, Mục tiêu $\ge 4.8/5.0$).<br>  - Tổng số Ticket phát sinh trong kỳ & Tỷ lệ theo kênh (Hotline, Zalo, Showroom).<br>  - Số lượng máy đổi mới 1-đổi-1 theo chính sách Smember VIP.<br>• **Biểu đồ 1 (Donut Chart):** Phân bổ Ticket theo Nhóm vấn đề (Lỗi nguồn, Màn hình, Đổi trả VIP, Dịch vụ).<br>• **Biểu đồ 2 (Bar Chart):** Top 5 Sản phẩm phát sinh khiếu nại nhiều nhất.<br>• **Bảng danh sách các Ticket quá hạn SLA cần cứu vãn gấp.** |
| **2. Dữ liệu thao tác (Nhập)** | • Bộ lọc khoảng thời gian: `Hôm nay`, `Tuần này`, `Tháng này`, `Tùy chọn ngày`.<br>• Bộ lọc theo Chi nhánh Showroom hoặc Trung tâm Điện Thoại Vui.<br>• Bộ lọc theo Hãng thiết bị: `Apple`, `Samsung`, `Xiaomi`, `Laptop`. |
| **3. Hành động chính (Actions)** | • **[Xuất Báo Cáo PDF / Excel]:** Xuất dữ liệu phục vụ báo cáo ban giám đốc.<br>• **[Xem Chi Tiết SLA Breaches]:** Danh sách các phiếu trễ hạn.<br>• **[Làm Mới Dữ Liệu (Refresh)].** |
| **4. Thông báo lỗi quan trọng** | • **Lỗi chọn khoảng ngày:** *"Ngày bắt đầu không được lớn hơn Ngày kết thúc trong bộ lọc thời gian!"*<br>• **Lỗi vượt ngưỡng xuất file:** *"Dữ liệu truy vấn vượt quá 50,000 dòng. Vui lòng thu hẹp khoảng thời gian hoặc lọc theo Chi nhánh!"* |

### 2. Wireframe Mockup UI (Frappe Desk Dashboard View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Dashboard CSKH & Chất lượng Dịch vụ                       [ Tháng này: 10/2026 ▼ ] [ Xuất Báo Cáo ]|
| Bộ lọc: [ Toàn quốc ▼ ] [ Ngành hàng: Điện thoại ▼ ]                                         [ 🔄 Làm mới ]  |
+--------------------------------------------------------------------------------------------------------------+
| +---------------------+ +---------------------+ +---------------------+ +---------------------+             |
| | TỶ LỆ ĐÚNG HẠN SLA  | | ĐIỂM HÀI LÒNG CSAT  | | TỔNG PHIẾU TIẾP NHẬN| | ĐỔI MÁY MỚI VIP 30N |             |
| |       96.4%         | |    4.85 / 5.0 ⭐    | |     1,420 Phiếu     | |       68 Máy        |             |
| | [🟢 Đạt mục tiêu]   | | [🟢 Tăng 0.12 pts]  | | (Hotline 45% - Store) | | (100% S-VIP duyệt)  |             |
| +---------------------+ +---------------------+ +---------------------+ +---------------------+             |
+--------------------------------------------------------------------------------------------------------------+
| [ BIỂU ĐỒ PHÂN BỔ THEO KÊNH TIẾP NHẬN ]       | [ TOP 5 SẢN PHẨM PHÁT SINH KHIẾU NẠI / LỖI ]                 |
|                                                |                                                             |
|   Hotline 1800  [====================] 45%     | 1. iPhone 15 Pro Max (Lỗi nóng/sập nguồn) [========] 120 vé |
|   Tại Showroom  [==============      ] 32%     | 2. Galaxy S24 Ultra (Sọc màn hình)        [======  ]  85 vé |
|   Zalo OA Chat  [=========           ] 18%     | 3. Acer Nitro 5 (Lỗi quạt tản nhiệt)      [====    ]  48 vé |
|   Website Form  [==                  ]  5%     | 4. AirPods Pro 2 (Lỗi rè chống ồn)        [===     ]  32 vé |
|                                                | 5. Xiaomi 14 Ultra (Lỗi lấy nét camera)   [==      ]  21 vé |
+--------------------------------------------------------------------------------------------------------------+
| [ DANH SÁCH 5 PHIẾU QUÁ HẠN SLA CẦN XỬ LÝ GẤP ]                                                              |
| • TCK-2026-00152 | Vũ Đức Đam | Xiaomi 14 Ultra | Trễ 45 phút | ĐTV Quận 10 | Phụ trách: KTV DTV-Hải           |
| • TCK-2026-00118 | Lê Thị Hoa | iPad Air M2     | Trễ 1h 20m  | Apple Care  | Phụ trách: CSKH-Hoàng            |
+--------------------------------------------------------------------------------------------------------------+
```
