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
| **1. Thông tin hiển thị (Xem)** | • **Header Profile:** Mã KH nội bộ duy nhất (`customer_id`), Họ tên, SĐT liên hệ, Email, CCCD, Địa chỉ, Ngày tạo hồ sơ.<br>• **Thẻ Smember 4 Hạng & Nhóm Giáo dục:** Badge Hạng mức (`S-NULL`, `S-NEW`, `S-MEM`, `⭐ S-VIP`), Badge Giáo dục (`🎓 S-Student` / `👨‍🏫 S-Teacher` kèm trạng thái Xác minh & Hạn hưởng), Tổng chi tiêu tham chiếu (`total_spent`), Điểm thưởng (`reward_points`).<br>• **Tab 1 - Thiết bị sở hữu / Lịch sử mua hàng:** Danh sách máy đã mua lấy từ `Sales_Invoice_Reference` (Mã SKU, Tên máy, Số IMEI/Serial, Ngày mua, Showroom xuất bán, Hạn bảo hành theo mã).<br>• **Tab 2 - Lịch sử Phiếu hỗ trợ:** Danh sách toàn bộ Ticket đã mở (Mã TCK, Lỗi, Trạng thái 5 State, Kết quả xử lý, Đơn vị tiếp nhận).<br>• **Tab 3 - Dòng thời gian Tương tác (Timeline):** Lịch sử các cuộc gọi Hotline 1800, Chat Zalo, Showroom.<br>• **Tab 4 - Nhu cầu / Cơ hội mở:** Các cơ hội tư vấn đang mở và lịch chăm sóc. |
| **2. Dữ liệu thao tác (Nhập)** | • Form cập nhật thông tin liên hệ (Địa chỉ nhận hàng, Email, Tỉnh/Thành phố).<br>• Cập nhật thông tin nhóm giáo dục (Loại ưu đãi, trạng thái duyệt, thời hạn xác nhận - yêu cầu quyền quản trị).<br>• Ô nhập ghi chú đặc điểm CSKH. |
| **3. Hành động chính (Actions)** | • **[+ Tạo Phiếu Hỗ Trợ]:** Mở form tạo Ticket mới (tự động điền thông tin KH).<br>• **[+ Tạo Cơ Hội Tư Vấn]:** Mở form ghi nhận nhu cầu/sản phẩm quan tâm.<br>• **[+ Ghi Nhận Tương Tác]:** Mở cửa sổ nhật ký cuộc gọi/chat.<br>• **[Lưu Thay Đổi]:** Cập nhật dữ liệu vào cơ sở dữ liệu. |
| **4. Thông báo lỗi quan trọng** | • **Cảnh báo trùng SĐT:** *"Số điện thoại [0908123456] đã tồn tại trên hồ sơ CUST-2026-00001! Hệ thống cảnh báo để kiểm tra, không tự ý gộp hồ sơ."*<br>• **Lỗi quyền hạn:** *"Bạn không có quyền chỉnh sửa thông tin Hạng thành viên hoặc Xác minh nhóm Giáo dục!"* |

### 2. Wireframe Mockup UI (Frappe Desk Form View - Tabbed)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Khách hàng > CUST-2026-00002                  [⭐ S-MEM] [🎓 S-Student: Đã duyệt] [ Lưu ]    |
+--------------------------------------------------------------------------------------------------------------+
| LÊ THỊ BÍCH TRÂM                                                                [+ Tạo Ticket] [+ Tạo Cơ Hội] |
| SĐT: 0912345678  |  Email: tram.le@yahoo.com  |  CCCD: 079195005678             [+ Ghi Tương Tác]            |
| Địa chỉ: Showroom 125 Lê Văn Việt, P. Hiệp Phú, TP. Thủ Đức, TP.HCM                                          |
+--------------------------------------------------------------------------------------------------------------+
| [ THÔNG TIN HỘI VIÊN SMEMBER & ƯU ĐÃI GIÁO DỤC ]                                                             |
| Hạng Smember: ⭐ S-MEM (Chi tiêu năm: 18,500,000 đ)  | Nhóm giáo dục: 🎓 S-Student (Đã duyệt - Hạn: 30/06/2027) |
| Tổng tích lũy tham chiếu: 18,500,000 đ               | Người cập nhật: HuyTV4 (Admin)                         |
+--------------------------------------------------------------------------------------------------------------+
| [TAB 1: Thiết bị sở hữu / IMEI (1)] | [TAB 2: Cơ hội (1)] | [TAB 3: Phiếu hỗ trợ (1)] | [TAB 4: Tương tác]    |
+--------------------------------------------------------------------------------------------------------------+
| • Laptop Lenovo LOQ 15IAX9 83GS001RVN | SN: 83GS001RVNVN101 | HĐ: INV-2025-01124 | Mua: 15/01/2025 | BH: 24 Tháng |
+--------------------------------------------------------------------------------------------------------------+
| [ LỊCH SỬ PHIẾU HỖ TRỢ GẦN NHẤT ]                                                                            |
| - TCK-2026-00150: Lenovo LOQ quạt kêu to -> [🟡 In Progress] -> Đang kiểm tra tại DTV Lê Văn Việt              |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 2: QUẢN LÝ CƠ HỘI TƯ VẤN & NHU CẦU MUA SẮM (LEAD & OPPORTUNITY VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • Mã cơ hội (`lead_id`), Tên khách hàng, SĐT liên hệ, Email, Nguồn tiếp nhận.<br>• **Thông tin Tư vấn Chuyên sâu:** Sản phẩm/SKU quan tâm (ví dụ: `Lenovo LOQ 15IAX9 83GS001RVN`, `iPhone 15 Pro Max`), Ngân sách dự kiến (`budget`), Thiết bị đang dùng (`device_in_use`), Nhu cầu thu cũ (`trade_in_demand`), Yêu cầu cấu hình/tác vụ (`desired_specs` - đồ họa, RAM, cổng sạc GaN...), Ngày dự kiến mua (`expected_buy_date`).<br>• Lịch nhắc việc chăm sóc (`follow_up_due` = Tạo + 3 ngày làm việc). |
| **2. Dữ liệu thao tác (Nhập)** | • Form nhập thông tin nhu cầu và kết quả tư vấn: Trạng thái cơ hội (`Open` $\rightarrow$ `Contacted` $\rightarrow$ `Qualified` $\rightarrow$ `Converted` $\rightarrow$ `Lost`).<br>• Chọn Showroom tiếp nhận tư vấn và Nhân viên phụ trách.<br>• Ô ghi chú chi tiết nhu cầu quà tặng / tư vấn gói AppleCare+ (phân biệt rõ với CareS). |
| **3. Hành động chính (Actions)** | • **[Bắt đầu cuộc gọi Telesales]:** Quay số tới SĐT khách.<br>• **[⏰ Tạo Lịch Hẹn Chăm Sóc (3 Ngày)]:** Tự động đặt lịch follow-up sau 3 ngày làm việc.<br>• **[🎯 Chuyển Đổi Thành Công (Converted)]:** Chốt mua, tạo đơn tham chiếu hoặc liên kết khách hàng.<br>• **[Đóng Cơ Hội (Lost)]:** Đánh dấu thất bại kèm lý do cụ thể. |
| **4. Thông báo lỗi quan trọng** | • **Thiếu thông tin tư vấn:** *"Vui lòng nhập Ngân sách dự kiến hoặc Thiết bị quan tâm trước khi chuyển trạng thái Qualified!"*<br>• **Trùng lặp cơ hội:** *"Khách hàng đã có một cơ hội mở cho sản phẩm này tại Showroom 125 Lê Văn Việt!"* |

### 2. Wireframe Mockup UI (Frappe Desk Opportunity Form View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Cơ hội Tư vấn > LEAD-2026-08912            [ Trạng thái: 🟡 CONTACTED ] [ 📞 Gọi Telesales ] |
+--------------------------------------------------------------------------------------------------------------+
| KHÁCH HÀNG: Trần Thị Mai Loan (SĐT: 0938112233)              KÊNH TIẾP NHẬN: [ Showroom 125 Lê Văn Việt   ▼ ]|
| SẢN PHẨM QUAN TÂM: Laptop Lenovo LOQ 15IAX9 83GS001RVN      NGÂN SÁCH DỰ KIẾN: [ 20,000,000 - 25,000,000 đ ]|
| THIẾT BỊ ĐANG DÙNG: Asus Vivobook cũ                         NHU CẦU THU CŨ ĐỔI MỚI: [☑ Có nhu cầu trợ giá ]  |
| YÊU CẦU CẤU HÌNH / TÁC VỤ: Học CNTT, nâng cấp 16GB RAM, game nhẹ, màn hình chuẩn màu 100% sRGB               |
| NGÀY DỰ KIẾN MUA: [ 10/10/2026 📅 ]                          HẠN CHĂM SÓC (+3 Ngày LV): [ 06/10/2026 17:00 ⏰ ]|
+--------------------------------------------------------------------------------------------------------------+
| TIẾN TRÌNH TƯ VẤN (STATE):                                                                                   |
| ( ) 1. Mới tạo (Open)   (•) 2. Đã liên hệ (Contacted)   ( ) 3. Đủ điều kiện (Qualified)   ( ) 4. Chốt mua   |
+--------------------------------------------------------------------------------------------------------------+
| GHI CHÚ TƯ VẤN & TƯ VẤN GÓI BẢO HÀNH:                                                                        |
| +----------------------------------------------------------------------------------------------------------+ |
| | Đã tư vấn bản LOQ 83GS001RVN bảo hành 24 tháng chính hãng. Khách thuộc nhóm S-Student cần chuẩn bị CCCD  | |
| | + Thẻ SV để được hưởng chính sách giá ưu đãi giáo dục. Khách hẹn T7 ra Showroom trải nghiệm máy.         | |
| +----------------------------------------------------------------------------------------------------------+ |
+--------------------------------------------------------------------------------------------------------------+
| [ Hủy / Đóng Cơ Hội (Lost) ]    [ ⏰ Đặt Lịch Chăm Sóc (+3 Ngày) ]    [ 🎯 Đã Mua Hàng (Converted) ]   [ Lưu ]|
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 3: GHI NHẬN TƯƠNG TÁC ĐA KÊNH & NHẮC VIỆC (INTERACTION & TASK REMINDER)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • Mã tương tác (`interaction_id`), Thời gian phát sinh cuộc gọi/chat/tại quầy.<br>• Thông tin nhận diện người gọi: Tên khách hàng, Hạng Smember (`S-NULL`, `S-NEW`, `S-MEM`, `S-VIP`), Badge Giáo dục, Chi nhánh phụ trách.<br>• Nhân viên thực hiện (`staff_agent`). |
| **2. Dữ liệu thao tác (Nhập)** | • **Khách hàng:** Chọn khách hàng (`customer_id`).<br>• **SĐT liên hệ:** `contact_phone` (Bắt buộc, 10 chữ số).<br>• **Kênh tiếp nhận:** `Hotline_1800`, `Zalo_OA`, `Fanpage`, `Showroom_Store`.<br>• **Phân loại nhu cầu:** `Tu_van_mua_hang`, `Tra_cuu_don_hang`, `Tiep_nhan_doi_tra`, `Bao_hanh_khieu_nai`.<br>• **Tóm tắt nội dung:** Biên bản trao đổi ngắn gọn.<br>• **Tạo lịch nhắc việc (Follow-up Task):** Chọn ngày giờ hẹn và nội dung nhắc nhở. |
| **3. Hành động chính (Actions)** | • **[Lưu Tương Tác]:** Ghi nhận vào nhật ký hệ thống.<br>• **[⚡ Tạo Nhanh Ticket]:** Chuyển sang tạo `Support_Ticket`.<br>• **[⏰ Tạo Lịch Nhắc Việc]:** Thêm nhiệm vụ nhắc việc vào lịch cá nhân.<br>• **[Hủy Bỏ].** |
| **4. Thông báo lỗi quan trọng** | • **Thiếu nội dung:** *"Vui lòng nhập Tóm tắt nội dung trao đổi trước khi lưu!"*<br>• **SĐT không hợp lệ:** *"Số điện thoại phải gồm 10 chữ số!"* |

### 2. Wireframe Mockup UI (Interaction Pop-up & Follow-up Task)

```text
+--------------------------------------------------------------------------------------------------------------+
| [GHI NHẬN TƯƠNG TÁC KHÁCH HÀNG] - INT-2026-00542                             [ Hotline 1800.2097 ] [ Cuộc gọi]|
+--------------------------------------------------------------------------------------------------------------+
| SĐT người gọi (*): [ 0908123456             ]  Khách hàng: [ CUST-2026-00001 - Nguyễn Văn An (⭐ S-VIP)   ▼ ]|
| Tên người liên hệ: [ Nguyễn Văn An          ]  Kênh tiếp nhận (*): [ Hotline 1800                         ▼ ]|
+--------------------------------------------------------------------------------------------------------------+
| Phân loại tương tác (*):                                                                                     |
| ( ) Tư vấn mua hàng / Cấu hình    (•) Tiếp nhận bảo hành / Đổi trả    ( ) Tra cứu đơn hàng    ( ) Khiếu nại  |
+--------------------------------------------------------------------------------------------------------------+
| Tóm tắt nội dung trao đổi (*):                                                                               |
| +----------------------------------------------------------------------------------------------------------+ |
| | Khách báo iPhone 15 Pro Max 256GB bị sập nguồn khi sạc. Hướng dẫn khách mang máy và hóa đơn đến Showroom  | |
| | 125 Lê Văn Việt để kỹ thuật viên kiểm tra điều kiện chính sách hãng và gói dịch vụ AppleCare+.            | |
| +----------------------------------------------------------------------------------------------------------+ |
+--------------------------------------------------------------------------------------------------------------+
| [⏰ TẠO LỊCH NHẮC VIỆC GỌI LẠI (FOLLOW-UP TASK)]                                                             |
| Ngày giờ hẹn gọi: [ 04/10/2026 10:00 📅 ]   Nội dung: [ Gọi lại xác nhận khách đã gửi máy tại Showroom 125  ]|
+--------------------------------------------------------------------------------------------------------------+
| [ Hủy bỏ ]                                  [ Lưu tương tác ]  [ ⚡ Tạo nhanh Ticket ]  [ ⏰ Lưu Lịch Hẹn ]  |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 4: DANH SÁCH & KANBAN PHIẾU HỖ TRỢ (TICKET KANBAN & LIST VIEW)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Kanban Board 5 Cột chuẩn State Diagram C03 (Nhật):**<br>  1. `Open (Mới tạo)` $\rightarrow$ 2. `In Progress (Đang xử lý)` $\rightarrow$ 3. `Pending Vendor (Chờ Hãng/LK)` $\rightarrow$ 4. `Resolved (Đã giải quyết)` $\rightarrow$ 5. `Closed (Đóng phiếu)`.<br>• **Thông tin trên mỗi Card:** Mã Ticket, Tên khách, SKU Thiết bị + IMEI, Nhãn ưu tiên (`Critical` / `High` / `S-VIP`), Phương án kết quả nếu có, Nhân viên phụ trách.<br>• **Bộ lọc cửa hàng:** Phân quyền theo Showroom (Showroom 125 Lê Văn Việt, Showroom 213 Trần Quang Khải). |
| **2. Dữ liệu thao tác (Nhập)** | • Ô lọc nhanh theo Mã Ticket, IMEI, Tên khách, SĐT.<br>• Kéo thả Card giữa các cột trạng thái hợp lệ. |
| **3. Hành động chính (Actions)** | • **[+ Tạo Phiếu Hỗ Trợ Mới]:** Mở form tiếp nhận.<br>• **[Chuyển Chế Độ List / Kanban]:** Đổi cách hiển thị.<br>• **[Phân Công Xử Lý]:** Gán nhân viên CSKH/Kỹ thuật viên phụ trách. |
| **4. Thông báo lỗi quan trọng** | • **Vi phạm luồng trạng thái:** *"Không thể chuyển trực tiếp từ Open sang Resolved mà chưa qua bước xử lý/xác minh!"*<br>• **Quyền truy cập:** *"Bạn chỉ được thao tác trên các phiếu thuộc phạm vi Showroom của mình!"* |

### 2. Wireframe Mockup UI (Frappe Desk Kanban View - 5 Cột)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Phiếu hỗ trợ (Kanban)           [ Chi nhánh: Showroom 125 Lê Văn Việt ▼ ] [+ Tạo Ticket Mới]|
| Phân loại: [ Tất cả ▼ ]   Ưu tiên: [ Tất cả ▼ ]                               [ 🔍 Tìm mã ticket, IMEI, SĐT... ]|
+--------------------------------------------------------------------------------------------------------------+
| 1. MỚI TẠO (Open)   | 2. ĐANG XỬ LÝ (In Prog)| 3. CHỜ HÃNG (Pending)  | 4. ĐÃ XỬ LÝ (Resolved) | 5. ĐÓNG (Closed)   |
| (2 Phiếu)           | (3 Phiếu)              | (1 Phiếu)               | (2 Phiếu)              | (12 Phiếu)         |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
| [TCK-2026-00155] ⭐  | [TCK-2026-00150]        | [TCK-2026-00145]         | [TCK-2026-00142] ⭐     | [TCK-2026-00130]    |
| KH: Nguyễn Văn An   | KH: Lê Thị Bích Trâm   | KH: Đỗ Mỹ Linh          | KH: Hoàng Minh Quân    | KH: Bùi Anh Tuấn   |
| iPhone 15 Pro Max   | Lenovo LOQ 15IAX9      | Asus ROG Zephyrus G16   | Củ sạc GaN Anker 65W   | Loa Marshall       |
| IMEI: 3589410982... | SN: 83GS001RVNVN101    | SN: K9N0CV124982        | SN: AN2024GAN65001     | SN: MS20240912     |
| Lỗi: Nóng sập nguồn | Lỗi: Quạt kêu rè rè    | Lỗi: Lỗi màn hình       | Kết quả: Đổi theo CS   | Hoàn tất trả máy   |
| Phụ trách: Chưa gán | Phụ trách: KTV DTV-Tú  | Phụ trách: KTV DTV-Hải  | Đã báo khách qua ĐT    | Đã thông báo khách |
| Store: 125 LVV      | Store: 125 LVV         | Chuyển: Apple Service   | Phụ trách: CSKH-Thuận  | Phụ trách: CSKH-My |
+---------------------+------------------------+-------------------------+------------------------+--------------------+
```

---

## MÀN HÌNH 5: CHI TIẾT PHIẾU HỖ TRỢ & TIẾP NHẬN BẢO HÀNH (TICKET DETAIL & RESOLUTION)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **Header Status:** Mã Ticket (`TCK-YYYY-XXXXX`), Trạng thái 5 State, Badge Hội viên Smember / Học sinh - Sinh viên.<br>• **Thông tin Chung:** Khách hàng, SĐT, Thiết bị lỗi (SKU, Tên máy, Serial/IMEI), Hóa đơn mua tham chiếu, Hạn bảo hành theo mã.<br>• **Bảng con Linh kiện (`Ticket_Repair_Item`):** Bộ phận lỗi, Tình trạng ngoại quan, Phụ kiện giữ lại, Diện bảo hành.<br>• **Dòng thời gian Xử lý (`Ticket_Activity_Log`):** Lịch sử chuyển trạng thái, ghi chú xác minh kỹ thuật. |
| **2. Dữ liệu thao tác (Nhập)** | • Phân công xử lý: Chọn người phụ trách và Chi nhánh tiếp nhận.<br>• **Nghiệm thu & Kết quả giải quyết (Bắt buộc khi Resolve/Close):**<br>  - `resolution_result`: Chọn *Bảo hành chính hãng, Đổi theo chính sách, Sửa chữa có phí, Không đủ điều kiện, Khách rút yêu cầu*.<br>  - `resolution_notes`: Ghi chú nội dung đã xử lý / mã thân máy đổi mới.<br>• **Ghi nhận thông báo cho khách (MVP Rule):**<br>  - `notify_customer_status`: Chọn *Chưa thông báo, Đã thông báo qua điện thoại, Đã thông báo tại quầy*.<br>  - `customer_notified_at`: Ngày giờ nhân viên thực hiện thông báo. |
| **3. Hành động chính (Actions)** | • **[Nhận Xử Lý (In Progress)]:** Tiếp nhận máy vào bước chẩn đoán.<br>• **[Chuyển Hãng / Đơn vị Bảo hành (Pending Vendor)]:** Gửi thiết bị sang trung tâm ủy quyền.<br>• **[Hoàn Tất / Resolve]:** Ghi nhận kết quả xử lý được duyệt.<br>• **[Bàn Giao & Đóng Phiếu (Close)]:** Hoàn tất bàn giao và ghi nhận đã thông báo khách.<br>• **[In Biên Nhận Tiếp Nhận / Trả Máy]:** Xuất mẫu in bàn giao. |
| **4. Thông báo lỗi quan trọng** | • **Thiếu kết quả xử lý:** *"Bắt buộc phải chọn Kết quả giải quyết (resolution_result) và nhập Ghi chú xử lý trước khi đánh dấu Hoàn tất (Resolved)!"*<br>• **Chưa ghi nhận thông báo khách:** *"Vui lòng xác nhận đã thông báo cho khách hàng trước khi Đóng phiếu (Closed)!"*<br>• **Chính sách đổi trả:** *"Thời hạn 30 ngày không mặc định đổi mới nguyên seal cho mọi trường hợp. Vui lòng kiểm tra điều kiện ngoại quan và chính sách hãng!"* |

### 2. Wireframe Mockup UI (Frappe Desk Ticket Detail Form View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Phiếu hỗ trợ > TCK-2026-00155             [⭐ S-VIP] [ In Phiếu Tiếp Nhận ] [ Lưu Dữ Liệu ]  |
| Trạng thái: [🟡 IN PROGRESS]                                                [ Cập nhật ] [ HOÀN TẤT / RESOLVE ]|
+--------------------------------------------------------------------------------------------------------------+
| KHÁCH HÀNG: Nguyễn Văn An (SĐT: 0908123456)             CHI NHÁNH: [ Showroom 125 Lê Văn Việt             ▼ ]|
| THIẾT BỊ: iPhone 15 Pro Max 256GB Titan Tự Nhiên         NGƯỜI PHỤ TRÁCH: [ CSKH_NguyenVanThuan            ▼ ]|
| SỐ SERIAL / IMEI: 358941098234112                        HÓA ĐƠN THAM CHIẾU: INV-2024-08912                   |
| DỊCH VỤ / GÓI BẢO VỆ: AppleCare+ (Trang dịch vụ riêng)   ĐƠN VỊ PHỐI HỢP: [ Trung tâm CareS Q9             ▼ ]|
+--------------------------------------------------------------------------------------------------------------+
| [ BẢNG KIỂM TRA NGOẠI QUAN & LINH KIỆN TIẾP NHẬN ] (Ticket_Repair_Item)                                      |
+----+---------------------+----------------------+----------------------+------------------+---------------+
| No | Bộ phận phát hiện lỗi| Tình trạng ngoại quan| Phụ kiện giữ lại     | Diện xử lý       | Chi phí dự kiến|
+----+---------------------+----------------------+----------------------+------------------+---------------+
| 1  | [ Nguồn / Pin     ▼]| [ Máy không cấn móp ▼]| [ Thân máy, Không hộp] | [ Bảo hành hãng ▼]| [         0 đ ]|
+----+---------------------+----------------------+----------------------+------------------+---------------+
| [ THÔNG TIN KẾT QUẢ GIẢI QUYẾT & THÔNG BÁO KHÁCH HÀNG ]                                                      |
| Kết quả giải quyết (*): [ Đổi theo chính sách (Đổi thân máy bảo hành)                                      ▼ ]|
| Ghi chú xử lý (*):      [ Trung tâm bảo hành xác nhận đổi thân máy mới IMEI: 358941098999888.              ]|
| Trạng thái thông báo:   [☑ Đã thông báo qua điện thoại ]   Thời điểm: [ 03/10/2026 16:30 📅 ]                |
+--------------------------------------------------------------------------------------------------------------+
| [ DÒNG THỜI GIAN XỬ LÝ (TIMELINE) ]                                                                          |
| • 10:15 - CSKH_Thuan: Tiếp nhận máy tại Showroom 125 Lê Văn Việt -> Trạng thái: Open.                        |
| • 11:00 - CSKH_Thuan: Chuyển máy sang CareS kiểm định nguồn -> Trạng thái: In Progress.                     |
| • 15:30 - CSKH_Thuan: Nhận kết quả đổi thân máy, gọi điện thông báo khách hẹn ngày 04/10 nhận máy.           |
+--------------------------------------------------------------------------------------------------------------+
```

---

## MÀN HÌNH 6: DASHBOARD BÁO CÁO TỔNG HỢP & HIỆU SUẤT DỊCH VỤ (CSKH ANALYTICS DASHBOARD)

### 1. Bảng Đặc tả Thành phần Màn hình

| Thành phần | Chi tiết đặc tả nghiệp vụ & kỹ thuật |
| :--- | :--- |
| **1. Thông tin hiển thị (Xem)** | • **4 Thẻ chỉ số tổng quan (KPI Cards theo Mục 5.1 BA):**<br>  - Tỷ lệ Thắng cơ hội ($\text{Tỷ lệ thắng} = \frac{\text{Thắng}}{\text{Thắng} + \text{Thua}} \times 100\%$).<br>  - Số lượng Khách hàng mới trong kỳ (đếm theo ngày tạo hồ sơ).<br>  - Tổng số Ticket phát sinh & Đã giải quyết.<br>  - Số lượng Cơ hội mở cần chăm sóc (Quá hạn 3 ngày làm việc).<br>• **Biểu đồ 1 (Bar Chart):** Hiệu suất tư vấn cơ hội thắng/thua theo 2 Showroom mô phỏng.<br>• **Biểu đồ 2 (Donut Chart):** Phân bổ Ticket theo Kết quả xử lý (Bảo hành, Đổi theo chính sách, Sửa có phí...).<br>• **Bảng danh sách nhắc việc chăm sóc khách hàng đến hạn.** |
| **2. Dữ liệu thao tác (Nhập)** | • Bộ lọc khoảng thời gian: `Hôm nay`, `Tuần này`, `Tháng này`, `Tùy chọn ngày`.<br>• Bộ lọc Chi nhánh: `Tất cả chuỗi`, `Showroom 125 Lê Văn Việt`, `Showroom 213 Trần Quang Khải`.<br>• Bộ lọc Ngành hàng: `Điện thoại`, `Laptop`, `Phụ kiện`. |
| **3. Hành động chính (Actions)** | • **[Xuất Báo Cáo PDF / Excel]:** Xuất báo cáo tổng hợp.<br>• **[Làm Mới Dữ Liệu (Refresh)]:** Nạp lại dữ liệu mới nhất. |
| **4. Thông báo lỗi quan trọng** | • **Lỗi chọn khoảng ngày:** *"Ngày bắt đầu không được lớn hơn Ngày kết thúc trong bộ lọc thời gian!"*<br>• **Chưa có dữ liệu:** *"Chưa phát sinh cơ hội đóng trong kỳ để tính tỷ lệ thắng (Mẫu số = 0)."* |

### 2. Wireframe Mockup UI (Frappe Desk Dashboard View)

```text
+--------------------------------------------------------------------------------------------------------------+
| CellphoneS CRM > Dashboard Báo cáo Hiệu suất Dịch vụ                       [ Tháng này: 10/2026 ▼ ] [ Xuất Báo Cáo ]|
| Chi nhánh: [ Showroom 125 Lê Văn Việt ▼ ]   Ngành hàng: [ Tất cả ▼ ]                         [ 🔄 Làm mới ]  |
+--------------------------------------------------------------------------------------------------------------+
| +---------------------+ +---------------------+ +---------------------+ +---------------------+             |
| | TỶ LỆ THẮNG CƠ HỘI  | | KHÁCH HÀNG MỚI      | | TỔNG PHIẾU TICKET   | | NHẮC VIỆC CẦN CHĂM  |             |
| |       68.5%         | |      142 Khách      | |      85 Phiếu       | |       12 Cơ hội     |             |
| | (Thắng: 54/ Đóng:79)| | (Đã kiểm soát trùng)| | (Đã xử lý: 78/85)   | | (Quá hạn 3 ngày LV) |             |
| +---------------------+ +---------------------+ +---------------------+ +---------------------+             |
+--------------------------------------------------------------------------------------------------------------+
| [ BIỂU ĐỒ KẾT QUẢ XỬ LÝ PHIẾU HỖ TRỢ ]         | [ CƠ HỘI BÁN HÀNG THEO SHOWROOM ]                           |
|                                                |                                                             |
|   Đổi theo chính sách [====================]40%| Showroom 125 Lê Văn Việt [====================] 48 Đơn     |
|   Bảo hành chính hãng [==============      ]30%| Showroom 213 Trần Q. Khải [==============      ] 31 Đơn     |
|   Sửa chữa có phí     [=========           ]18%|                                                             |
|   Không đủ điều kiện  [====                ] 8%|                                                             |
|   Khách rút yêu cầu   [==                  ] 4%|                                                             |
+--------------------------------------------------------------------------------------------------------------+
| [ DANH SÁCH CƠ HỘI CẦN CHĂM SÓC GẤP (QUÁ HẠN 3 NGÀY LÀM VIỆC) ]                                             |
| • LEAD-2026-08912 | Trần Thị Mai Loan | Laptop Lenovo LOQ | Quá hạn 1 ngày | Phụ trách: CSKH_Thuận (125 LVV)   |
| • LEAD-2026-08905 | Hoàng Minh Quân   | Phụ kiện sạc GaN  | Đến hạn hôm nay| Phụ trách: CSKH_Linh  (213 TQK)   |
+--------------------------------------------------------------------------------------------------------------+
```
