# SẢN PHẨM BÀN GIAO D03: WIREFRAME & LAYOUT CÁC MÀN HÌNH ƯU TIÊN
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D03  
**Thuộc nhiệm vụ:** Nhiệm vụ 4 (Phác thảo 4–6 màn hình ưu tiên theo Use Case)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Trạng thái:** Bản nháp Mốc M3 (Draft for Cross-check)

---

## TỔNG QUAN THIẾT KẾ GIAO DIỆN (DESK UI STANDARDS)

Toàn bộ 6 màn hình ưu tiên dưới đây được thiết kế bám sát chuẩn giao diện **Frappe Desk UI v15**, tối ưu trải nghiệm cho 3 nhóm người dùng: Nhân viên CSKH/Telesales, Quản lý CSKH và Kỹ thuật viên Điện Thoại Vui.

Mỗi màn hình được bóc tách chặt chẽ theo cấu trúc 3 phần chuẩn:
1. **Thông tin hiển thị (Xem):** Dữ liệu chỉ đọc và nguồn truy vấn.
2. **Dữ liệu thao tác (Nhập):** Các ô nhập liệu, bộ lọc và danh sách chọn.
3. **Hành động chính (Actions):** Các nút bấm chức năng kích hoạt quy trình nghiệp vụ.

---

## MÀN HÌNH 1: DANH SÁCH KHÁCH HÀNG (CUSTOMER LIST VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình (3 Khối)

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • Mã khách hàng (`customer_id` / `name`)<br>• Họ và tên (`customer_name`)<br>• Số điện thoại (`phone_number`)<br>• Hạng hội viên Smember Badge (`member_tier`: Smember Standard / S-VIP có màu nổi bật)<br>• Điểm thưởng tích lũy (`reward_points`)<br>• Tổng chi tiêu tích lũy (`total_spent`)<br>• Trạng thái hồ sơ (`status`: Active / Inactive / Merged)<br>• Ngày tương tác cuối cùng |
| **2. Dữ liệu nhập/thao tác (Nhập)** | • Ô tìm kiếm nhanh (Search by SĐT, Họ tên, Mã KH, CCCD)<br>• Bộ lọc Dropdown: Hạng Smember (`Tất cả`, `Smember`, `S-VIP`), Trạng thái (`Active`, `Merged`), Khu vực Tỉnh/TP |
| **3. Hành động chính (Actions)** | • **[+ Thêm Khách hàng]:** Mở form tạo nhanh khách hàng mới.<br>• **[Gộp hồ sơ (Merge)]:** Mở Modal gộp 2 khách hàng trùng lặp (chỉ CSKH Manager).<br>• **[Xuất Excel / CSV]:** Xuất danh sách phân khúc phục vụ chiến dịch Marketing.<br>• **[Click vào dòng]:** Chuyển hướng sang Màn hình 2 (Hồ sơ 360°). |

### 2. Wireframe Mockup UI (Frappe Desk List View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Khách hàng                                              [🔔 3] [🔍 Search Ctrl+K] [👤 Huy.TQ] |
+--------------------------------------------------------------------------------------------------------------+
| [ 🔍 Tìm kiếm SĐT, Tên, CCCD...         ] [ Hạng Smember: Tất cả ▼ ] [ Khu vực: TP.HCM ▼ ] [+ Thêm Khách hàng] |
|                                                                                           [ ⚡ Gộp hồ sơ (Merge)] |
+--------------------------------------------------------------------------------------------------------------+
| [ ] | Mã KH       | Họ và tên         | Số điện thoại | Hạng Smember | Điểm tích lũy | Tổng chi tiêu | Trạng thái |
+-----+-------------+-------------------+---------------+--------------+---------------+---------------+------------+
| [ ] | CUST-2026-001| Nguyễn Văn An     | 0908123456    | [⭐ S-VIP]   | 1,250 điểm    | 85,400,000 đ  | [🟢 Active]|
| [ ] | CUST-2026-002| Lê Thị Bích Trâm  | 0912345678    | [  Smember]  |   320 điểm    | 12,800,000 đ  | [🟢 Active]|
| [ ] | CUST-2026-003| Trần Đình Trọng   | 0987654321    | [  Smember]  |    80 điểm    |  4,500,000 đ  | [🟢 Active]|
| [ ] | CUST-2026-004| Hoàng Minh Quân   | 0933112233    | [⭐ S-VIP]   | 2,100 điểm    |142,000,000 đ  | [🟢 Active]|
| [ ] | CUST-2026-005| Phạm Thu Hà (Cũ)  | 0977889900    | [  Smember]  |     0 điểm    |            0 đ| [⚪ Merged]|
+-----+-------------+-------------------+---------------+--------------+---------------+---------------+------------+
| Hiển thị 1 - 5 của 24,580 khách hàng                                     [<< Trang trước] [1] [2] [3] [Trang sau >>]|
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 2: HỒ SƠ KHÁCH HÀNG 360° (CUSTOMER 360° PROFILE VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình (3 Khối)

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Header Profile:** Avatar, Tên, SĐT, CCCD, Email, Địa chỉ, Ngày tham gia.<br>• **Thẻ Hội viên Smember:** Hạng mức hiện tại, Điểm thưởng khả dụng, Tổng chi tiêu, Hạn duy trì hạng VIP.<br>• **Tab 1 - Lịch sử thiết bị / Mua hàng:** Danh sách máy đã mua, Mã SKU, Tên máy, Số Serial/IMEI, Ngày kích hoạt, Hạn bảo hành gốc.<br>• **Tab 2 - Lịch sử Phiếu hỗ trợ:** Danh sách toàn bộ Ticket (Mã, Lỗi, Trạng thái, Đơn vị Điện Thoại Vui xử lý).<br>• **Tab 3 - Dòng thời gian Tương tác (Timeline):** Lịch sử các cuộc gọi Hotline, Chat Zalo OA, Showroom kèm ghi chú. |
| **2. Dữ liệu nhập/thao tác (Nhập)** | • Form chỉnh sửa thông tin liên lạc (Địa chỉ, Email, SĐT phụ).<br>• Ô nhập ghi chú nhanh CSKH (Ghi nhận đặc điểm khách hàng: khó tính, ưu tiên bảo hành nhanh...). |
| **3. Hành động chính (Actions)** | • **[+ Tạo Ticket mới]:** Mở form tạo phiếu hỗ trợ (tự động điền thông tin khách).<br>• **[+ Ghi nhận tương tác]:** Mở cửa sổ ghi nhật ký cuộc gọi/chat.<br>• **[⭐ Điều chỉnh điểm Smember]:** Mở Modal thưởng/phạt điểm (Cần quyền Manager).<br>• **[Lưu thay đổi]:** Cập nhật dữ liệu khách hàng. |

### 2. Wireframe Mockup UI (Frappe Desk Form View - Tabbed)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Khách hàng > CUST-2026-001                                      [⭐ S-VIP] [ Lưu thông tin ] |
+--------------------------------------------------------------------------------------------------------------+
| NGUYỄN VĂN AN                                                                   [+ Tạo Ticket] [+ Ghi tương tác]|
| SĐT: 0908123456  |  Email: an.nguyen@gmail.com  |  CCCD: 079098001234           [⭐ Điều chỉnh điểm Smember]    |
| Địa chỉ: 125 Lê Văn Việt, Tăng Nhơn Phú B, TP. Thủ Đức, TP.HCM                                               |
+--------------------------------------------------------------------------------------------------------------+
| [ THÔNG TIN HỘI VIÊN SMEMBER ]                                                                               |
| Hạng: ⭐ S-VIP (Ưu tiên SLA 4H & Đổi mới 30 ngày) | Điểm khả dụng: 1,250 pts | Tích lũy: 85,400,000 đ        |
| Hạn duy trì hạng: 31/12/2026                      | Ngày thăng hạng gần nhất: 15/01/2026                     |
+--------------------------------------------------------------------------------------------------------------+
| [TAB 1: Thiết bị sở hữu / IMEI] | [TAB 2: Phiếu hỗ trợ (2)] | [TAB 3: Nhật ký tương tác] | [TAB 4: Ghi chú] |
+--------------------------------------------------------------------------------------------------------------+
| • iPhone 15 Pro Max 256GB Titan Tự Nhiên | IMEI: 358941098234112 | Mua: 20/09/2024 | BH: 19/09/2025 (Chính hãng) |
| • Apple Watch Ultra 2 GPS + Cellular     | IMEI: 356712098451201 | Mua: 15/11/2024 | BH: 14/11/2025 (Chính hãng) |
| • Củ sạc Apple 20W Type-C Chính hãng     | SKU: SP-APP-20W       | Mua: 20/09/2024 | BH: 19/09/2025             |
+--------------------------------------------------------------------------------------------------------------+
| [ LỊCH SỬ PHIẾU HỖ TRỢ GẦN NHẤT ]                                                                            |
| - TCK-2026-0089: iPhone 15 Pro Max lỗi sọc màn hình -> [🟢 Resolved] -> Xử lý: Đổi cụm màn hình mới tại DTV |
| - TCK-2026-0142: Yêu cầu xuất lại hóa đơn VAT điện tử -> [⚪ Closed]                                         |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 3: GHI NHẬN TƯƠNG TÁC ĐA KÊNH (CUSTOMER INTERACTION LOG)

### 1. Bảng Đặc tả Thành phần Màn hình (3 Khối)

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • Mã tương tác tự sinh (`INT-YYYY-XXXXX`)<br>• Thời điểm phát sinh (`interaction_time`)<br>• Thông tin Agent trực máy (`staff_agent`)<br>• Pop-up CTI Caller-ID: Hiển thị tên khách và lịch sử 3 lần liên hệ gần nhất nếu nhận diện được SĐT gọi đến. |
| **2. Dữ liệu nhập/thao tác (Nhập)** | • **Khách hàng:** Chọn khách hàng liên kết (`customer_id` - Để trống nếu là Khách vãng lai)<br>• **SĐT liên hệ:** `contact_phone` (Bắt buộc, tự điền từ CTI Tổng đài)<br>• **Tên người gọi:** `contact_name` (Nếu khách cung cấp)<br>• **Kênh tiếp nhận:** `Hotline_1800`, `Zalo_OA`, `Fanpage`, `Showroom_Store`<br>• **Phân loại nhu cầu:** `Tu_van_mua_hang`, `Tra_cuu_don_hang`, `Bao_hanh_sua_chua`, `Khieu_nai_dich_vu`<br>• **Nội dung tóm tắt:** Ô soạn thảo ghi lại ngắn gọn yêu cầu và hướng tư vấn.<br>• **Điểm hài lòng CSAT:** 1-5 Sao (Nếu khách đánh giá ngay). |
| **3. Hành động chính (Actions)** | • **[Lưu tương tác]:** Ghi vào nhật ký hệ thống.<br>• **[⚡ Tạo nhanh Ticket]:** Lưu tương tác và mở ngay form tạo `Support Ticket` (tự sao chép thông tin khách/SĐT sang).<br>• **[🎯 Chuyển Telesales (Lead)]:** Tạo Lead tư vấn bán hàng trả góp/Pre-order.<br>• **[Hủy / Đóng].** |

### 2. Wireframe Mockup UI (Interaction Pop-up / Quick Form)

```text
+--------------------------------------------------------------------------------------------------------------+
| [GHI NHẬN TƯƠNG TÁC KHÁCH HÀNG] - INT-2026-00542                             [ Hotline 1800.2097 ] [ Đang gọi ]|
+--------------------------------------------------------------------------------------------------------------+
| SĐT người gọi (*): [ 0908123456             ]  Khách hàng: [ CUST-2026-001 - Nguyễn Văn An (⭐ S-VIP)      ▼ ]|
| Tên người liên hệ: [ Nguyễn Văn An          ]  Kênh tiếp nhận (*): [ Hotline 1800                        ▼ ]|
+--------------------------------------------------------------------------------------------------------------+
| Phân loại tương tác (*):                                                                                     |
| ( ) Tư vấn mua hàng / Pre-order    (•) Khiếu nại bảo hành máy    ( ) Tra cứu giao hàng    ( ) Hỏi giá phụ kiện|
+--------------------------------------------------------------------------------------------------------------+
| Tóm tắt nội dung trao đổi (*):                                                                               |
| +----------------------------------------------------------------------------------------------------------+ |
| | Khách báo iPhone 15 Pro Max mua tháng 9/2024 bị nóng máy bất thường và sập nguồn khi sạc.               | |
| | Đã hướng dẫn khách mang máy ra Showroom CellphoneS 125 Lê Văn Việt để Kỹ thuật Điện Thoại Vui thẩm định.| |
| | Khách là hội viên S-VIP, yêu cầu hỗ trợ kiểm tra nhanh trong vòng 4 tiếng.                              | |
| +----------------------------------------------------------------------------------------------------------+ |
+--------------------------------------------------------------------------------------------------------------+
| Đánh giá CSAT tức thời: [ ⭐⭐⭐⭐⭐ (5 Sao) ▼ ]     Nhân viên tiếp nhận: Lê Minh Duy (Agent-012)             |
+--------------------------------------------------------------------------------------------------------------+
| [ Hủy bỏ ]                                       [ Lưu tương tác ]  [ ⚡ Tạo nhanh Ticket ]  [ 🎯 Tạo Lead ] |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 4: DANH SÁCH PHIẾU HỖ TRỢ (TICKET KANBAN & LIST VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình (3 Khối)

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Chế độ Kanban Board (5 Cột chuẩn State của Nhật):**<br>  1. `Open (Mới tạo)` $\rightarrow$ 2. `In Progress (Đang xử lý)` $\rightarrow$ 3. `Pending Vendor (Chờ linh kiện/Hãng)` $\rightarrow$ 4. `Resolved (Đã giải quyết)` $\rightarrow$ 5. `Closed (Đóng phiếu)`.<br>• **Thông tin trên mỗi Card:** Mã Ticket, Tên khách, Model máy + IMEI, Tag SLA Timer đếm ngược (Màu đỏ nếu sắp trễ hạn), Nhãn ưu tiên (Critical / High / VIP), Avatar nhân viên phụ trách.<br>• **Bộ đếm tổng quan:** Số ticket quá hạn (SLA Breach), Số ticket S-VIP cần ưu tiên. |
| **2. Dữ liệu nhập/thao tác (Nhập)** | • Ô lọc nhanh theo Mã Ticket, IMEI, Tên khách.<br>• Bộ lọc phân loại: Theo Phòng ban xử lý (`CSKH Showroom`, `Điện Thoại Vui`, `Hãng`), Theo Độ ưu tiên (`Critical`, `High`, `Medium`, `Low`).<br>• Kéo thả Card giữa các cột trạng thái (Trigger Transition Rule). |
| **3. Hành động chính (Actions)** | • **[+ Mở Phiếu hỗ trợ mới]:** Mở form tạo Ticket.<br>• **[Chuyển chế độ List / Kanban]:** Thay đổi dạng xem.<br>• **[Gán việc nhanh (Quick Assign)]:** Phân công kỹ thuật viên trực tiếp trên Card.<br>• **[Lọc vé của tôi (My Tickets)].** |

### 2. Wireframe Mockup UI (Frappe Desk Kanban View - 5 Cột)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Phiếu hỗ trợ (Kanban)                     [ Bộ lọc: Tất cả ▼ ] [ Chế độ: Kanban ▤ ] [+ Tạo Ticket]|
| SLA Cảnh báo: [ 🔴 2 Phiếu sắp quá hạn ] [ ⭐ 4 Phiếu S-VIP ]                 [ 🔍 Tìm mã ticket, IMEI, SĐT... ]|
+--------------------------------------------------------------------------------------------------------------+
| 1. MỚI TẠO (Open)   | 2. ĐANG XỬ LÝ (In Prog)| 3. CHỜ HÃNG/LK (Pending)| 4. ĐÃ XỬ LÝ (Resolved) | 5. ĐÓNG (Closed)   |
| (3 Phiếu)           | (5 Phiếu)              | (2 Phiếu)               | (4 Phiếu)              | (18 Phiếu)         |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
| [TCK-2026-0155] ⭐  | [TCK-2026-0150]        | [TCK-2026-0145]         | [TCK-2026-0142] ⭐     | [TCK-2026-0130]    |
| KH: Nguyễn Văn An   | KH: Lê Hoàng Nam       | KH: Đỗ Mỹ Linh          | KH: Hoàng Minh Quân    | KH: Bùi Anh Tuấn   |
| IP 15 Pro Max       | Galaxy S24 Ultra       | Asus ROG Zephyrus G16   | iPad Pro M4            | Loa Marshall       |
| IMEI: 3589410982... | IMEI: 3521098451...    | SN: K9N0CV124982        | IMEI: 3599810234...    | SN: MS20240912     |
| Lỗi: Nóng sập nguồn | Lỗi: Sọc màn hình      | Lỗi: Quạt kêu rè rè     | Xử lý: Đổi máy mới 1-1 | Hoàn tất bảo hành  |
| SLA: [⏱️ Còn 3h15p] | SLA: [⏱️ Còn 18h]      | Điểm tiếp nhận: Apple   | Chờ khách đến nhận máy | CSAT: ⭐⭐⭐⭐⭐     |
| Phụ trách: Chưa gán | Phụ trách: KTV DTV-Tú  | Phụ trách: KTV DTV-Hải  | Phụ trách: CSKH-Thuận  | Phụ trách: CSKH-My |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
| [TCK-2026-0156]     | [TCK-2026-0152] 🔴     |                         | [TCK-2026-0144]        |                    |
| KH: Phạm Minh Tâm   | KH: Vũ Đức Đam         |                         | KH: Trương Vĩnh Ký     |                    |
| Laptop Acer Nitro 5 | Xiaomi 14 Ultra        |                         | AirPods Pro 2          |                    |
| Lỗi: Treo logo      | Lỗi: Không nhận sạc    |                         | Xử lý: Đổi tai nghe R  |                    |
| SLA: [⏱️ Còn 22h]   | SLA: [🔴 QUÁ HẠN 45P]  |                         |                        |                    |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
```

---

## MÀN HÌNH 5: CHI TIẾT PHIẾU HỖ TRỢ (TICKET DETAIL & REPAIR VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình (3 Khối)

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Header Status Bar:** Mã Ticket, Trạng thái (5 State), Đồng hồ đếm ngược SLA, Badge Hội viên Smember.<br>• **Khối Thông tin Chung:** Khách hàng, SĐT, Sản phẩm lỗi (SKU, Tên, Số Serial/IMEI), Ngày mua, Hạn bảo hành.<br>• **Khối Bảng con Linh kiện (`Ticket_Repair_Item`):** Bộ phận lỗi, Ngoại quan máy khi nhận, Phụ kiện giữ lại, Tình trạng bảo hành hãng.<br>• **Khối Lịch sử Xử lý (`Ticket_Activity_Log` / Timeline):** Toàn bộ thao tác chuyển trạng thái, ghi chú kỹ thuật DTV, tin nhắn SMS/Zalo gửi cho khách. |
| **2. Dữ liệu nhập/thao tác (Nhập)** | • **Phân công xử lý:** Chọn Nhân viên tiếp nhận và Đơn vị chịu trách nhiệm (`Trung_tam_Dien_Thoai_Vui`, `Showroom`, `Hang`).<br>• **Bảng kiểm định linh kiện:** Thêm dòng lỗi linh kiện, chọn mức độ trầy xước, phụ kiện đi kèm.<br>• **Form Nghiệm thu & Khắc phục (Bắt buộc khi Resolve/Close):**<br>  - `resolution_type`: Chọn Đổi mới 100%, Sửa chữa thay linh kiện, Bảo hành hãng, Từ chối.<br>  - `root_cause`: Nhập nguyên nhân lỗi kỹ thuật chẩn đoán.<br>  - `resolution_notes`: Ghi chú nội dung đã xử lý thực tế. |
| **3. Hành động chính (Actions)** | • **[Nhận xử lý (In Progress)]:** Kỹ thuật viên DTV tiếp nhận máy vào quy trình.<br>• **[Chuyển Hãng (Pending Vendor)]:** Đánh dấu gửi Apple Care / Samsung.<br>• **[Hoàn tất xử lý (Resolve)]:** Kích hoạt nghiệm thu kỹ thuật.<br>• **[Bàn giao & Đóng phiếu (Close)]:** Ký biên bản giao máy, tự động kích hoạt Zalo ZNS khảo sát CSAT.<br>• **[In Phiếu tiếp nhận / Biên nhận bảo hành]:** Xuất file PDF giao khách. |

### 2. Wireframe Mockup UI (Frappe Desk Form View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Phiếu hỗ trợ > TCK-2026-0155           [⭐ S-VIP] [⏱️ SLA Còn: 03h 45m] [ In Phiếu Tiếp Nhận ]|
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
| Phương án giải quyết (*): [ Đổi máy mới 100% (Chính sách S-VIP)                                            ▼ ]|
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

## MÀN HÌNH 6: DASHBOARD CSKH & QUẢN TRỊ DỊCH VỤ (CSKH ANALYTICS DASHBOARD)

### 1. Bảng Đặc tả Thành phần Màn hình (3 Khối)

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **4 Thẻ chỉ số tổng quan (KPI Cards):**<br>  - Tỷ lệ hoàn thành đúng hạn SLA (% On-time SLA, Mục tiêu $\ge 95\%$).<br>  - Điểm hài lòng trung bình (CSAT Score, Mục tiêu $\ge 4.8/5.0$).<br>  - Tổng số Ticket phát sinh trong kỳ & Tỷ lệ theo kênh (Hotline, Zalo, Store).<br>  - Số lượng máy đổi mới 1-đổi-1 theo chính sách Smember VIP.<br>• **Biểu đồ 1 (Donut Chart):** Phân bổ Ticket theo Nhóm vấn đề (Lỗi nguồn, Màn hình, Đổi trả VIP, Dịch vụ).<br>• **Biểu đồ 2 (Bar Chart):** Top 5 Sản phẩm/Dòng máy phát sinh lỗi nhiều nhất (iPhone 15, S24, ROG Phone...).<br>• **Bảng danh sách Top Chi nhánh / Cửa hàng giải quyết khiếu nại nhanh nhất.** |
| **2. Dữ liệu nhập/thao tác (Nhập)** | • Bộ lọc khoảng thời gian: `Hôm nay`, `Tuần này`, `Tháng này`, `Tùy chọn ngày`.<br>• Bộ lọc theo Chi nhánh Showroom hoặc Trung tâm Điện Thoại Vui.<br>• Bộ lọc theo Hãng thiết bị: `Apple`, `Samsung`, `Xiaomi`, `Laptop`. |
| **3. Hành động chính (Actions)** | • **[Xuất báo cáo PDF / Excel]:** Xuất báo cáo hiệu suất phục vụ cuộc họp giao ban CSKH.<br>• **[Chi tiết SLA Breaches]:** Xem danh sách tất cả các Ticket bị trễ hạn SLA.<br>• **[Làm mới dữ liệu (Refresh)].** |

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
| • TCK-2026-0152 | Vũ Đức Đam | Xiaomi 14 Ultra | Trễ 45 phút | ĐTV Quận 10 | Phụ trách: KTV DTV-Hải           |
| • TCK-2026-0118 | Lê Thị Hoa | iPad Air M2     | Trễ 1h 20m  | Apple Care  | Phụ trách: CSKH-Hoàng            |
+--------------------------------------------------------------------------------------------------------------+
```

---

## TÍNH KHỚP NỐI VÀ KIỂM ĐỊNH (CROSS-CHECK)
- **100% Khớp với Từ điển Dữ liệu (D02):** Mọi trường trên 6 giao diện đều có tên vật lý và kiểu dữ liệu tương ứng trong bảng từ điển (không có thuộc tính mồ côi).
- **100% Khớp với State Diagram của Nhật:** Màn hình 4 (Kanban) và Màn hình 5 (Chi tiết phiếu) hỗ trợ đầy đủ 5 trạng thái: `Open`, `In_Progress`, `Pending_Vendor`, `Resolved`, `Closed`.
- **100% Phù hợp Nghiệp vụ Bán lẻ CellphoneS:** Tích hợp nhận diện Smember VIP, tra cứu bảo hành qua IMEI, và phân luồng tiếp nhận Điện Thoại Vui.
