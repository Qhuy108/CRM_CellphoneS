# BÁO CÁO KẾT QUẢ THIẾT KẾ & TRIỂN KHAI 6 GIAO DIỆN CRM CELLPHONES
## HỆ THỐNG QUẢN TRỊ QUAN HỆ KHÁCH HÀNG & DỊCH VỤ SỬA CHỮA ĐIỆN THOẠI VUI (FRAPPE DESK V15)
### ĐỒNG BỘ 100% THEO ĐẶC TẢ D03 WIREFRAME & LAYOUT

- **Dự án:** CRM Chuỗi Bán lẻ Công nghệ CellphoneS & Hệ thống Sửa chữa Điện Thoại Vui
- **Tác giả:** Trần Quang Huy (System Analyst & Project Manager)
- **Nền tảng mục tiêu:** Frappe Desk UI v15 (Custom App `cellphones_crm` độc lập)
- **Công cụ thiết kế & sinh mã:** Stitch MCP (AI UI/UX Engine) & Frappe Design System
- **Trạng thái:** Hoàn thiện 100% (Final Production-Ready Prototype)
- **Ngày cập nhật:** 03/10/2026

---

## 1. TỔNG QUAN DỰ ÁN & KIẾN TRÚC GIAO DIỆN

Hệ thống giao diện CRM CellphoneS được xây dựng bám sát chuẩn kiến trúc **Frappe Desk v15**, phục vụ toàn diện 6 Use Case nghiệp vụ cốt lõi theo đặc tả tài liệu **D03**:
1. Quản trị vòng đời khách hàng và quyền lợi Smember 4 hạng & Nhóm giáo dục (S-Student / S-Teacher).
2. Quản lý cơ hội tư vấn bán hàng đa kênh & thu cũ đổi mới trợ giá.
3. Tiếp nhận tương tác tổng đài đa kênh (Hotline 1800) & lập lịch nhắc việc chăm sóc (+3 ngày làm việc).
4. Điều phối và xử lý phiếu hỗ trợ bảo hành - sửa chữa Điện Thoại Vui theo Kanban 5 Cột chuẩn State Diagram C03.
5. Khám nghiệm kỹ thuật, đối soát IMEI/AppleCare+ và bắt buộc giải trình nghiệm thu trước khi đóng phiếu.
6. Giám sát thời gian thực chỉ số cam kết chất lượng dịch vụ (Tỷ lệ thắng cơ hội, Khách hàng mới, Tổng Ticket, Danh sách quá hạn 3 ngày).

### 🎨 Bộ Quy Chuẩn Thiết Kế (Design Tokens)
- **Màu chủ đạo (Primary Brand):** `#D70018` (Đỏ CellphoneS) & `#B30012` (Đỏ sẫm).
- **Màu đặc quyền VIP & Cảnh báo (Accent Gold & Amber):** `#F59E0B` (Smember Crown Badge & S-VIP Priority).
- **Màu nhóm giáo dục (Purple):** `#7C3AED` (Badge 🎓 S-Student & 👨‍🏫 S-Teacher).
- **Màu trạng thái hoàn tất (Emerald Green):** `#10B981` (SLA Passed, Ticket Closed, Converted).
- **Màu vi phạm / Khẩn cấp (Crimson Red):** `#EF4444` (SLA Breached, Critical Priority).
- **Nền tảng hiển thị:** Desktop Enterprise 1440px / 1280px responsive, phông chữ `Inter` & `Public Sans` chuẩn hiển thị tiếng Việt, mã định danh IMEI/Serial/DocType dùng font `JetBrains Mono`.

---

## 2. DANH MỤC 6 MÀN HÌNH ĐÃ THIẾT KẾ & XUẤT BẢN

| STT | Mã Màn hình | Tên Giao diện & Nghiệp vụ Theo D03 | Tệp Mã Nguồn HTML | Đối tượng DocType |
| :--- | :--- | :--- | :--- | :--- |
| **01** | `SCREEN_01` | **Hồ sơ khách hàng 360° & Quản trị Smember** | [screen1_customer_360.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen1_customer_360.html) | `Customer` (CUST-2026-00002) |
| **02** | `SCREEN_02` | **Quản lý cơ hội tư vấn & Nhu cầu mua sắm** | [screen2_lead_preorder.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen2_lead_preorder.html) | `Lead & Opportunity` (LEAD-2026-08912) |
| **03** | `SCREEN_03` | **Ghi nhận tương tác đa kênh & Nhắc việc** | [screen3_interaction_reminder.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen3_interaction_reminder.html) | `Customer_Interaction` (INT-2026-00542) |
| **04** | `SCREEN_04` | **Kanban 5 Cột Phiếu hỗ trợ & Bảo hành DTV** | [screen4_ticket_kanban.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen4_ticket_kanban.html) | `Support_Ticket` (5 Cột Vòng Đời) |
| **05** | `SCREEN_05` | **Chi tiết Phiếu hỗ trợ & Nghiệm thu Kỹ thuật** | [screen5_ticket_detail.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen5_ticket_detail.html) | `Support_Ticket` (TCK-2026-00155) |
| **06** | `SCREEN_06` | **CSKH Analytics & Hiệu suất Dịch vụ Dashboard** | [screen6_cskh_dashboard.html](file:///d:/CRM_CellphoneS/.stitch/designs/screen6_cskh_dashboard.html) | `CSKH Analytics Dashboard` |

---

## 3. CHI TIẾT CẤU TẠO VÀ ĐẶC TẢ NGHIỆP VỤ 6 MÀN HÌNH (CHUẨN D03)

### 3.1. Màn hình 1: Hồ Sơ Khách Hàng 360° & Danh Sách (`SCREEN_01`)
- **Đối tượng dữ liệu:** `Customer` (CUST-2026-00002 - Lê Thị Bích Trâm).
- **1. Thông tin hiển thị (Xem):**
  - **Header Profile:** Mã KH `CUST-2026-00002`, Họ tên: `LÊ THỊ BÍCH TRÂM`, SĐT: `0912345678`, Email: `tram.le@yahoo.com`, CCCD: `079195005678`, Địa chỉ: `Showroom 125 Lê Văn Việt, P. Hiệp Phú, TP. Thủ Đức, TP.HCM`.
  - **Thẻ Smember 4 Hạng & Nhóm Giáo dục:** Badge Hạng mức `⭐ S-MEM` (Chi tiêu năm: 18,500,000 đ), Badge Giáo dục `🎓 S-Student (Đã duyệt - Hạn: 30/06/2027)`, Tổng chi tiêu tham chiếu `18,500,000 đ` (`total_spent`), Điểm thưởng `370 pts` (`reward_points`), Quản trị viên cập nhật: `HuyTV4 (Admin)`.
  - **4 Tabs Nghiệp vụ:**
    - *Tab 1 - Thiết bị sở hữu / Lịch sử mua hàng:* Laptop Lenovo LOQ 15IAX9 83GS001RVN (SN: `83GS001RVNVN101`, HĐ `INV-2025-01124`, Mua: 15/01/2025, BH 24 Tháng tại Showroom 125 Lê Văn Việt).
    - *Tab 2 - Cơ hội mở / Nhu cầu:* `LEAD-2026-08912` (Lenovo LOQ 20-25tr, Hạn chăm sóc 06/10/2026).
    - *Tab 3 - Lịch sử Phiếu hỗ trợ:* `TCK-2026-00150` (Quạt kêu to -> `[🟡 In Progress]` tại DTV Lê Văn Việt - KTV Tú).
    - *Tab 4 - Dòng thời gian tương tác:* Lịch sử cuộc gọi Hotline 1800.2097 và Zalo OA.
- **2. Dữ liệu thao tác (Nhập):** Form cập nhật địa chỉ, email, nhóm giáo dục và ô ghi chú đặc điểm CSKH.
- **3. Hành động chính (Actions):** `[+ Tạo Phiếu Hỗ Trợ]`, `[+ Tạo Cơ Hội Tư Vấn]`, `[+ Ghi Nhận Tương Tác]`, `[Lưu Thay Đổi]`.
- **4. Thông báo lỗi (Alerts):** Cảnh báo trùng SĐT với hồ sơ CUST-2026-00001, Cảnh báo phân quyền Admin khi cập nhật nhóm Giáo dục.

---

### 3.2. Màn hình 2: Quản Lý Cơ Hội Tư Vấn & Nhu Cầu Mua Sắm (`SCREEN_02`)
- **Đối tượng dữ liệu:** `Lead / Opportunity` (`LEAD-2026-08912` - Trần Thị Mai Loan).
- **1. Thông tin hiển thị (Xem):**
  - Khách hàng: `Trần Thị Mai Loan` (SĐT: `0938112233`, Email: `loan.tran@gmail.com`).
  - Kênh tiếp nhận: `Showroom 125 Lê Văn Việt`.
  - Sản phẩm quan tâm: `Laptop Lenovo LOQ 15IAX9 83GS001RVN` (Ngân sách: `20,000,000 - 25,000,000 đ`).
  - Thiết bị đang dùng: `Asus Vivobook cũ` | Nhu cầu thu cũ đổi mới: `[☑ Có nhu cầu trợ giá SV]`.
  - Yêu cầu cấu hình/tác vụ: `Học CNTT, nâng cấp 16GB RAM, game nhẹ, màn hình chuẩn màu 100% sRGB`.
  - Ngày dự kiến mua: `10/10/2026` | Hạn chăm sóc (+3 Ngày LV): `06/10/2026 17:00 ⏰`.
- **2. Dữ liệu thao tác (Nhập):**
  - Tiến trình tư vấn (State): `( ) 1. Mới tạo (Open)` $\rightarrow$ `(•) 2. Đã liên hệ (Contacted)` $\rightarrow$ `( ) 3. Đủ điều kiện (Qualified)` $\rightarrow$ `( ) 4. Chốt mua (Converted)`.
  - Ghi chú tư vấn: *Đã tư vấn bản LOQ 83GS001RVN BH 24T chính hãng, khách thuộc nhóm S-Student cần chuẩn bị CCCD + Thẻ SV.*
- **3. Hành động chính (Actions):** `[ 📞 Bắt đầu cuộc gọi Telesales ]`, `[ ⏰ Đặt Lịch Chăm Sóc (+3 Ngày) ]`, `[ 🎯 Đã Mua Hàng (Converted) ]`, `[ Hủy / Đóng Cơ Hội (Lost) ]`, `[ Lưu ]`.
- **4. Thông báo lỗi (Alerts):** Cảnh báo thiếu thông tin tư vấn trước khi Qualified, Cảnh báo trùng lặp cơ hội tại Showroom.

---

### 3.3. Màn hình 3: Ghi Nhận Tương Tác Đa Kênh & Nhắc Việc (`SCREEN_03`)
- **Đối tượng dữ liệu:** `Customer_Interaction` (`INT-2026-00542`).
- **1. Thông tin hiển thị (Xem):**
  - CTI Live Call Bar: Hotline 1800.2097 thời lượng `02:45`, phát sinh lúc `03/10/2026 10:15`.
  - Tự động nhận diện: `Nguyễn Văn An` (SĐT `0908123456`, liên kết `CUST-2026-00001 ⭐ S-VIP`).
  - Nhân viên tiếp nhận: `staff_agent` = `HuyTV4 (Trần Quang Huy)`.
- **2. Dữ liệu thao tác (Nhập):**
  - Phân loại tương tác: `(•) Tiếp nhận bảo hành / Đổi trả`.
  - Tóm tắt nội dung trao đổi (*): *Khách báo iPhone 15 Pro Max 256GB bị sập nguồn khi sạc. Hướng dẫn mang máy qua Showroom 125 Lê Văn Việt kiểm tra AppleCare+.*
  - Khối nhắc việc gọi lại (Follow-up Task): Hẹn gọi `04/10/2026 10:00` (Nội dung: *Gọi lại xác nhận khách đã gửi máy tại Showroom 125*).
- **3. Hành động chính (Actions):** `[ Hủy bỏ ]`, `[ Lưu tương tác ]`, `[ ⚡ Tạo nhanh Ticket ]`, `[ ⏰ Lưu Lịch Hẹn ]`.
- **4. Thông báo lỗi (Alerts):** Bắt buộc nhập tóm tắt trao đổi, Regex SĐT 10 chữ số.

---

### 3.4. Màn hình 4: Kanban 5 Cột Phiếu Hỗ Trợ & Bảo Hành DTV (`SCREEN_04`)
- **Đối tượng dữ liệu:** `Support_Ticket` (Kanban Board 5 Cột).
- **1. Thông tin hiển thị (Xem) - 5 Cột Chuẩn State Diagram C03:**
  1. `1. MỚI TẠO (Open - 2 Phiếu):` `[TCK-2026-00155] ⭐` (Nguyễn Văn An - iPhone 15 Pro Max nóng sập nguồn, SLA 4H), `[TCK-2026-00156]` (Trần Văn Nam - AirPods Pro 2 rè tai).
  2. `2. ĐANG XỬ LÝ (In Progress - 3 Phiếu):` `[TCK-2026-00150]` (Lê Thị Bích Trâm - Lenovo LOQ quạt rè, KTV Tú), `[TCK-2026-00151]`, `[TCK-2026-00152]`.
  3. `3. CHỜ HÃNG (Pending Vendor - 1 Phiếu):` `[TCK-2026-00145]` (Đỗ Mỹ Linh - Asus ROG Zephyrus G16 lỗi màn OLED, chuyển Asus Service).
  4. `4. ĐÃ XỬ LÝ (Resolved - 2 Phiếu):` `[TCK-2026-00142] ⭐` (Hoàng Minh Quân - Củ sạc GaN Anker 65W đổi theo CS, đã báo qua ĐT, CSKH-Thuận).
  5. `5. ĐÓNG (Closed - 12 Phiếu):` `[TCK-2026-00130]` (Bùi Anh Tuấn - Loa Marshall hoàn tất trả máy, CSAT 5 sao).
- **2. Dữ liệu thao tác (Nhập):** Bộ lọc Showroom 125 Lê Văn Việt, Phân loại, Mức độ ưu tiên, Ô tìm kiếm nhanh mã Ticket/IMEI/SĐT.
- **3. Hành động chính (Actions):** `[+ Tạo Ticket Mới]`, `[Chuyển Chế Độ List / Kanban]`, `[Phân Công Xử Lý]`.
- **4. Thông báo lỗi (Alerts):** Không cho phép chuyển thẳng từ Open sang Resolved, Ràng buộc quyền hạn chi nhánh.

---

### 3.5. Màn hình 5: Chi Tiết Phiếu Hỗ Trợ & Nghiệm Thu Kỹ Thuật (`SCREEN_05`)
- **Đối tượng dữ liệu:** `Support_Ticket` & Child Table `Ticket_Repair_Item` (`TCK-2026-00155`).
- **1. Thông tin hiển thị (Xem):**
  - Header: `TCK-2026-00155`, Trạng thái: `[🟡 IN PROGRESS]`, Badge: `⭐ S-VIP`.
  - Khách hàng: `Nguyễn Văn An` (0908123456), Showroom: `125 Lê Văn Việt`, Thiết bị: `iPhone 15 Pro Max 256GB Titan Tự Nhiên`, Phụ trách: `CSKH_NguyenVanThuan`, IMEI: `358941098234112`, HĐ: `INV-2024-08912`, Dịch vụ: `AppleCare+ CareS Q9`.
  - Bảng con Linh kiện (`Ticket_Repair_Item`): Dòng 1 | Nguồn / Pin | Máy không cấn móp | Thân máy, Không hộp | Bảo hành hãng | Chi phí: `0 đ`.
  - Timeline 3 mốc: 10:15 Tiếp nhận Open $\rightarrow$ 11:00 CareS kiểm định In Progress $\rightarrow$ 15:30 Đổi thân máy & hẹn ngày 04/10.
- **2. Dữ liệu thao tác (Nhập):**
  - `resolution_result`: `Đổi theo chính sách (Đổi thân máy bảo hành)`.
  - `resolution_notes`: `Trung tâm bảo hành xác nhận đổi thân máy mới IMEI: 358941098999888.`
  - `notify_customer_status`: `☑ Đã thông báo qua điện thoại` lúc `03/10/2026 16:30`.
- **3. Hành động chính (Actions):** `[ Nhận Xử Lý ]`, `[ Chuyển Hãng ]`, `[ HOÀN TẤT / RESOLVE ]`, `[ Bàn Giao & Đóng Phiếu (Close) ]`, `[ In Phiếu Tiếp Nhận ]`.
- **4. Thông báo lỗi (Alerts):** Bắt buộc chọn kết quả giải quyết & ghi chú xử lý trước khi Resolve, Bắt buộc xác nhận đã thông báo khách trước khi Close.

---

### 3.6. Màn hình 6: CSKH Analytics & Hiệu Suất Dịch Vụ Dashboard (`SCREEN_06`)
- **Đối tượng dữ liệu:** `CSKH Analytics Dashboard`.
- **1. Thông tin hiển thị (Xem) - 4 Thẻ KPI Mục 5.1 BA & D03:**
  1. `TỶ LỆ THẮNG CƠ HỘI`: `68.5%` (Thắng: 54 / Đóng: 79) - Công thức: $\frac{\text{Thắng}}{\text{Thắng} + \text{Thua}} \times 100\%$.
  2. `KHÁCH HÀNG MỚI`: `142 Khách` (Đã kiểm soát trùng SĐT).
  3. `TỔNG PHIẾU TICKET`: `85 Phiếu` (Đã xử lý hoàn tất: 78/85).
  4. `NHẮC VIỆC CẦN CHĂM`: `12 Cơ hội` (Quá hạn 3 ngày làm việc cần gọi Telesales ngay).
- **2 Biểu đồ Phân tích:**
  - *Biểu đồ 1:* Kết quả xử lý Ticket (Đổi theo CS 40%, BH chính hãng 30%, Sửa có phí 18%, Không đủ ĐK 8%, Khách rút 4%).
  - *Biểu đồ 2:* Cơ hội bán hàng theo Showroom (Showroom 125 Lê Văn Việt: 48 Đơn vs Showroom 213 Trần Quang Khải: 31 Đơn).
- **Bảng danh sách cơ hội quá hạn 3 ngày làm việc:**
  - `LEAD-2026-08912` | Trần Thị Mai Loan | Laptop Lenovo LOQ | Quá hạn 1 ngày | Phụ trách: CSKH_Thuận (125 LVV).
  - `LEAD-2026-08905` | Hoàng Minh Quân | Phụ kiện sạc GaN | Đến hạn hôm nay | Phụ trách: CSKH_Linh (213 TQK).
- **Hành động chính (Actions):** `[ Xuất Báo Cáo PDF / Excel ]`, `[ 🔄 Làm mới ]`, Bộ lọc thời gian (Tháng này: 10/2026), Chi nhánh (Showroom 125 Lê Văn Việt), Ngành hàng (Tất cả).

---

## 4. HƯỚNG DẪN TRẢI NGHIỆM PROTOTYPE SHOWCASE PORTAL

Toàn bộ 6 giao diện đã được đồng bộ và tích hợp vào trang Portal trung tâm tại:  
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
