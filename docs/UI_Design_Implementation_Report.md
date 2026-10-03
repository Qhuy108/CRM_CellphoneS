# BÁO CÁO KẾT QUẢ THIẾT KẾ & TRIỂN KHAI 6 GIAO DIỆN CRM CELLPHONES
## HỆ THỐNG QUẢN TRỊ QUAN HỆ KHÁCH HÀNG & DỊCH VỤ SỬA CHỮA ĐIỆN THOẠI VUI (FRAPPE DESK V15)

- **Dự án:** CRM Chuỗi Bán lẻ Công nghệ CellphoneS & Hệ thống Sửa chữa Điện Thoại Vui
- **Tác giả:** Trần Quang Huy (System Analyst & Project Manager)
- **Nền tảng mục tiêu:** Frappe Desk UI v15 / ERPNext Web Client
- **Công cụ thiết kế & sinh mã:** Stitch MCP (AI UI/UX Engine) & Frappe Design System
- **Trạng thái:** Hoàn thiện 100% (Final Production-Ready Prototype)
- **Ngày hoàn thành:** 03/10/2026

---

## 1. TỔNG QUAN DỰ ÁN & KIẾN TRÚC GIAO DIỆN

Hệ thống giao diện CRM CellphoneS được xây dựng bám sát chuẩn kiến trúc **Frappe Desk v15**, phục vụ toàn diện các quy trình bán lẻ đa kênh, chăm sóc khách hàng hội viên Smember VIP và điều phối bảo hành - sửa chữa liên kết với Trung tâm Điện Thoại Vui.

### 🎨 Bộ Quy Chuẩn Thiết Kế (Design System & Tokens)
- **Màu chủ đạo (Primary Brand):** `#D70018` (Đỏ CellphoneS) & `#B30012` (Đỏ sẫm).
- **Màu đặc quyền VIP (Accent Gold):** `#F59E0B` (Smember Crown Badge & S-VIP Priority).
- **Màu trạng thái nghiệp vụ:**
  - `Thành công / Hoàn tất SLA:` `#10B981` (Emerald Green)
  - `Đang xử lý / Cảnh báo:` `#F59E0B` (Amber Orange)
  - `Quá hạn SLA / Khẩn cấp:` `#EF4444` (Crimson Red)
- **Nền tảng hiển thị:** Desktop Enterprise 1440px / 1280px responsive, phông chữ `Inter` & `Roboto` chuẩn hiển thị tiếng Việt, mã định danh IMEI/Serial/DocType dùng font `JetBrains Mono`.
- **Thành phần khung Frappe Desk:**
  - **Awesome Bar toàn cục (`Ctrl + K`):** Hỗ trợ tìm nhanh số điện thoại, mã IMEI 15 số, mã phiếu hỗ trợ `TCK-2026-XXXXX` và mã đơn hàng.
  - **Dải cảnh báo SLA Live Strip:** Hiển thị tức thời số ca vi phạm cam kết chất lượng dịch vụ.
  - **Thanh điều hướng Sidebar trái:** Tích hợp đầy đủ các DocType: *Khách hàng*, *Nhu cầu tư vấn*, *Tương tác đa kênh*, *Phiếu hỗ trợ*, *CSKH Dashboard*, *Điện Thoại Vui Service*.

---

## 2. DANH MỤC 6 MÀN HÌNH ĐÃ THIẾT KẾ & XUẤT BẢN

| STT | Mã Màn hình | Tên Giao diện & Nghiệp vụ | Tệp Mã Nguồn HTML | Stitch Screen ID |
| :--- | :--- | :--- | :--- | :--- |
| **01** | `SCREEN_01` | **Hồ sơ khách hàng 360° & Quản trị Smember VIP** | [screen1_customer_360.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen1_customer_360.html) | `9f072f11b61049fcb0e8c2c2b26e5124` |
| **02** | `SCREEN_02` | **Quản lý cơ hội tư vấn & Pre-order Flagship** | [screen2_lead_preorder.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen2_lead_preorder.html) | `70229c33bad74a26ba8c10094c6158d1` |
| **03** | `SCREEN_03` | **Tiếp nhận tương tác đa kênh & Nhắc việc (CTI Caller-ID)** | [screen3_interaction_reminder.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen3_interaction_reminder.html) | `e9010cd6f6444ae6a7fb2e0e30049907` |
| **04** | `SCREEN_04` | **Kanban 5 Cột Phiếu hỗ trợ & Bảo hành Điện Thoại Vui** | [screen4_ticket_kanban.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen4_ticket_kanban.html) | `c8e116b0d8804be2adf4906b748542d7` |
| **05** | `SCREEN_05` | **Chi tiết Phiếu hỗ trợ & Nghiệm thu Kỹ thuật** | [screen5_ticket_detail.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen5_ticket_detail.html) | `bb761b9200ab48568e7dd0ad553465ee` |
| **06** | `SCREEN_06` | **CSKH Analytics & Real-Time SLA Dashboard** | [screen6_cskh_dashboard.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen6_cskh_dashboard.html) | `cde3c109dbd345c1afce99ff0dd0b6b2` |

---

## 3. CHI TIẾT CẤU TẠO VÀ ĐẶC TẢ NGHIỆP VỤ TỪNG MÀN HÌNH

### 3.1. Màn hình 1: Hồ Sơ Khách Hàng 360° & Quản Trị Smember VIP
- **Đối tượng dữ liệu:** `Customer` (CUST-2026-00001 - Nguyễn Văn An).
- **Cấu phần hiển thị:**
  - **Header Profile:** Tên khách hàng, SĐT (`0908123456`), Email, CCCD, Địa chỉ tại Showroom 125 Lê Văn Việt.
  - **4 Thẻ Smember VIP:** Hạng `⭐ S-VIP` (Đặc quyền SLA 4H & Đổi mới 1-1 trong 30 ngày), Điểm khả dụng `1,250 pts` (1,250,000 đ), Chi tiêu lũy kế `85,400,000 đ`, Hạn duy trì đến `31/12/2026`.
  - **Hệ thống 4 Tabs:**
    - *Tab 1 - Thiết bị sở hữu / IMEI:* iPhone 15 Pro Max 256GB Titan (IMEI: 358941098234112), Apple Watch Ultra 2, Củ sạc 20W Type-C.
    - *Tab 2 - Lịch sử Phiếu hỗ trợ:* Phiếu TCK-2026-00155 (Đang xử lý tại DTV Q9) và TCK-2026-00089 (Đã đóng & đổi mới).
    - *Tab 3 - Dòng thời gian tương tác:* Lịch sử cuộc gọi 1800 và chat Zalo OA.
    - *Tab 4 - Ghi chú CSKH:* Lưu ý đặc quyền phục vụ VIP ngoài giờ.
  - **Hành động nghiệp vụ:** `[+ Tạo Phiếu Hỗ Trợ]`, `[+ Ghi Nhận Tương Tác]`, `[⚡ Gộp Hồ Sơ (Merge)]`, `[⭐ Điều Chỉnh Điểm Smember]`, `[Lưu Thay Đổi]`.

---

### 3.2. Màn hình 2: Quản Lý Cơ Hội Tư Vấn & Pre-Order Flagship
- **Đối tượng dữ liệu:** `Lead / Opportunity` (LEAD-2026-08912 - Trần Thị Mai Loan).
- **Cấu phần hiển thị:**
  - **Lead Header Card:** Sản phẩm iPhone 16 Pro Max 256GB Sa Mạc (`SP-IP16PM-256-DES`), Showroom nhận máy 125 Lê Văn Việt, Thanh đếm Quota (*Còn 12 / 50 Suất Đợt 1*).
  - **Telesales Workflow Stepper:** `1. Mới tạo` $\rightarrow$ `2. Đã liên hệ (Contacted - Đang xử lý)` $\rightarrow$ `3. Đủ điều kiện` $\rightarrow$ `4. Chốt cọc (Converted)`.
  - **Phân hệ Thu Cũ Đổi Mới (Trade-in):** Thẩm định iPhone 13 Pro Max 128GB loại 1, voucher trợ giá Smember +1,000,000 đ.
  - **Cổng Thanh Toán VietQR Tự Động:** Mã VietQR Napas247, MBBank, STK `0908123456`, tiền cọc `1,000,000 đ`, cú pháp `COC IP16PM LEAD-2026-08912` kèm cơ chế IPN kích hoạt đơn hàng tức thì.
  - **Hành động nghiệp vụ:** `[📞 Gọi Telesales (CTI)]`, `[⚡ Tạo VietQR Cọc]`, `[🎯 Chốt Cọc & Tạo Sales Order]`, `[Hủy / Đóng Lead]`.

---

### 3.3. Màn hình 3: Tiếp Nhận Tương Tác Đa Kênh & Nhắc Việc (CTI Caller-ID)
- **Đối tượng dữ liệu:** `Customer Interaction` (INT-2026-00542).
- **Cấu phần hiển thị:**
  - **CTI Live Call Banner:** Cuộc gọi Hotline 1800.2097 thời lượng `02:45`, hoạt họa sóng âm, nhận diện tự động khách S-VIP `Nguyễn Văn An`.
  - **Form Ghi Nhận Tương Tác:** SĐT người gọi (có badge đã xác thực), Kênh tiếp nhận Hotline 1800, Phân loại *Khiếu nại bảo hành máy*.
  - **Lịch sử 3 lần liên hệ gần nhất (Quick Peek):** Đối soát nhanh lịch sử mua sắm và ticket cũ.
  - **Khối Nhắc Việc Chăm Sóc (Follow-up Task):** Lịch hẹn gọi lại lúc `14:00 ngày 03/10/2026` sau khi Điện Thoại Vui kiểm định máy xong.
  - **Hành động nghiệp vụ:** `[Lưu Tương Tác]`, `[⚡ Tạo Nhanh Ticket]`, `[⏰ Lưu Lịch Hẹn]`.

---

### 3.4. Màn hình 4: Kanban 5 Cột Phiếu Hỗ Trợ & Tiếp Nhận Điện Thoại Vui
- **Đối tượng dữ liệu:** `Support Ticket` (Kanban Board 5 Cột).
- **Cấu phần hiển thị:**
  - **5 Cột Trạng Thái Chuẩn Vòng Đời:**
    1. `1. MỚI TẠO (Open - 3 Phiếu):` TCK-2026-00155 (S-VIP, còn 3h15p SLA).
    2. `2. ĐANG XỬ LÝ (In Progress - 5 Phiếu):` TCK-2026-00152 (🔴 Quá hạn 45p tại DTV Q10), TCK-2026-00150 (Galaxy S24 Ultra sọc màn).
    3. `3. CHỜ HÃNG / LINH KIỆN (Pending Vendor - 2 Phiếu):` TCK-2026-00145 (Asus ROG Zephyrus G16 chờ cụm quạt tản nhiệt).
    4. `4. ĐÃ XỬ LÝ (Resolved - 4 Phiếu):` TCK-2026-00142 (iPad Pro M4 đổi máy mới 1-1, chờ khách nhận).
    5. `5. ĐÓNG PHIẾU (Closed - 18 Phiếu):` TCK-2026-00130 (Loa Marshall hoàn tất, CSAT 5 sao).
  - **Dải Cảnh Báo SLA:** 2 Phiếu trễ hạn + 4 Phiếu S-VIP cam kết xử lý trong 4 giờ.
  - **Bộ lọc & Action:** Chuyển đổi Kanban/List View, lọc theo Trạm Điện Thoại Vui / Apple Care / CSKH Showroom.

---

### 3.5. Màn hình 5: Chi Tiết Phiếu Hỗ Trợ & Nghiệm Thu Kỹ Thuật
- **Đối tượng dữ liệu:** `Support Ticket` & Child Table `Ticket_Repair_Item` (TCK-2026-00155).
- **Cấu phần hiển thị:**
  - **Header & SLA Clock:** Mã TCK-2026-00155, Trạng thái `[🟡 IN PROGRESS]`, Đồng hồ đếm ngược `⏱️ SLA Còn: 03h 45m`, Tra cứu Apple Care VN/A đến 19/09/2025.
  - **Bảng Con Linh Kiện (`Ticket_Repair_Item`):**
    - Dòng 1: Mainboard IC Nguồn | Ngoại quan 99% không trầy xước | Diện VIP 1-Đổi-1 30N | Chi phí: 0 đ.
    - Dòng 2: Màn hình OLED Super Retina | Đã dán cường lực KingKong | Diện Apple VN/A | Chi phí: 0 đ.
  - **Form Nghiệm Thu Bắt Buộc (Resolution Form):**
    - Phương án giải quyết: *Đổi máy mới 100% (Chính sách S-VIP 30 ngày)*.
    - Báo cáo nguyên nhân: *Chập IC nguồn Type-C do linh kiện nhà sản xuất*.
    - Ghi chú khắc phục & Bàn giao: *Cấp thân máy mới nguyên seal VN/A IMEI: 358941098999888*.
  - **Timeline & Box Gửi Zalo ZNS:** Gửi tin nhắn tự động thông báo khách hàng tới Showroom nhận máy.
  - **Hành động nghiệp vụ:** `[HOÀN TẤT / RESOLVE]` (nút đỏ chính), `[Bàn Giao & Đóng Phiếu]`, `[In Phiếu Tiếp Nhận (PDF)]`.

---

### 3.6. Màn hình 6: CSKH Analytics & Real-Time SLA Dashboard
- **Đối tượng dữ liệu:** `CSKH Analytics Dashboard`.
- **Cấu phần hiển thị:**
  - **4 Thẻ Chỉ Số KPI Tổng Quan:**
    - Tỷ lệ đúng hạn SLA: `96.4%` (Mục tiêu $\ge 95\%$, Đạt 1,369/1,420 phiếu).
    - Điểm hài lòng CSAT: `4.85 / 5.0 ⭐` (98.2% đánh giá 5 sao từ khảo sát Zalo ZNS).
    - Tổng phiếu tiếp nhận: `1,420 Phiếu` (Hotline 45%, Store 32%, Zalo 18%, Web 5%).
    - Đổi mới 1-Đổi-1 S-VIP 30 ngày: `68 Máy` (100% duyệt trong 4H).
  - **Biểu Đồ Phân Bổ Kênh Tiếp Nhận & Top 5 Thiết Bị Báo Lỗi Nhiều Nhất:** Thống kê chi tiết iPhone 15 Pro Max, Galaxy S24 Ultra, Nitro 5, AirPods Pro 2, Xiaomi 14 Ultra.
  - **Bảng Cứu Vãn 5 Ca Vi Phạm SLA Khẩn Cấp:** Tích hợp các nút hành động phân quyền: `⚡ Điều Phối Gấp`, `⚡ Đôn Đốc Hãng`, `⚡ Can Thiệp Quản Lý`, `⚡ Xuất Đổi Máy`, `⚡ Gọi Báo Khách`.
  - **Live Footer:** Chu kỳ cập nhật số liệu tự động mỗi 30 giây từ Frappe Engine và POS Điện Thoại Vui.

---

## 4. HƯỚNG DẪN TRẢI NGHIỆM PROTOTYPE SHOWCASE PORTAL

Toàn bộ 6 giao diện đã được đóng gói và tích hợp vào trang Portal điều khiển trung tâm tại:
👉 **[Portal Trải Nghiệm Prototype: .stitch/designs/index.html](file:///d:/CRM_CellphoneS/.stitch/designs/index.html)**

### ✨ Tính năng của Portal Trải nghiệm:
1. **Chuyển đổi 6 màn hình tức thì:** Bấm các Tab `01` $\rightarrow$ `06` ở thanh tiêu đề trên cùng.
2. **Ngăn xem Đặc tả nghiệp vụ D03:** Bấm nút `📋 Đặc tả D03` ở góc phải để mở Drawer hiển thị chi tiết Thông tin hiển thị (Xem), Dữ liệu thao tác (Nhập), Hành động (Actions) và Ràng buộc lỗi của màn hình đang chọn.
3. **Chế độ xem đa khung hình (Viewport Selector):** Chuyển đổi giữa `Full`, `1440px` (Desktop Chuẩn), và `1280px` (Laptop).
4. **Mở trực tiếp file đơn lẻ:** Bấm nút `↗ Mở Tab Mới` để mở riêng lẻ từng trang HTML toàn màn hình.

---

## 5. MA TRẬN ĐỐI SOÁT VỚI BỘ TÀI LIỆU D01 – D05

| Mã Tài Liệu | Tên Sản Phẩm | Mức Độ Khớp Nối Với Bộ Giao Diện |
| :--- | :--- | :--- |
| **D01** | ERD & Thuyết minh | 100% khớp các thực thể: `Customer`, `Lead`, `Interaction`, `Ticket`, `Ticket_Repair_Item`, `User`. |
| **D02** | Từ điển dữ liệu | 100% các ô nhập liệu, định dạng mã số (`CUST-`, `LEAD-`, `INT-`, `TCK-`), IMEI 15 số và regex SĐT Việt Nam đều đồng nhất. |
| **D03** | Wireframe & Layout | Toàn bộ cấu trúc 4 phần (Xem - Nhập - Actions - Alerts) được chuyển hóa trọn vẹn sang giao diện Frappe Desk. |
| **D04** | Ánh xạ Frappe | Cấu trúc DocType, Child Table, Workflow State và Phân quyền Role Permission Manager được áp dụng đầy đủ. |
| **D05** | Sequence Diagram | Thể hiện đúng các luồng tự động định tuyến SLA 4H của S-VIP và điều kiện bắt buộc giải trình trước khi Resolve/Close. |

---

*Báo cáo được lập và bàn giao chính thức bởi **Trần Quang Huy**.*
