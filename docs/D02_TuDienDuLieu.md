# SẢN PHẨM BÀN GIAO D02: TỪ ĐIỂN DỮ LIỆU & QUY TẮC KIỂM TRA
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D02  
**Thuộc nhiệm vụ:** Nhiệm vụ 3 (Lập Từ điển Dữ liệu & Quy tắc kiểm tra tính hợp lệ)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Trạng thái:** Bản nháp Mốc M3 (Draft for Cross-check)

---

## PHẦN 1: BẢNG TỪ ĐIỂN DỮ LIỆU CHI TIẾT (8 CỘT CHUẨN)

### 1. Bảng Khách hàng (`Customer` / `tabCustomer`)
Quản lý thông tin định danh và liên hệ của khách hàng cá nhân hoặc doanh nghiệp tại chuỗi CellphoneS.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã khách hàng | `name` / `customer_id` | Mã định danh khách hàng | Data | Yes | Yes | Format: `CUST-.YYYY.-.#####` | Hệ thống tự sinh |
| Số điện thoại | `phone_number` | Số điện thoại chính | Data | Yes | Yes | Chuỗi 10 chữ số đầu 03,05,07,08,09 | Người dùng nhập |
| Họ và tên | `customer_name` | Họ tên khách hàng | Data | Yes | No | Độ dài tối đa 140 ký tự | Người dùng nhập |
| Email | `email_id` | Thư điện tử | Data | No | No | Định dạng RFC 5322 email | Người dùng nhập |
| Số CCCD / CMND | `identity_card` | Căn cước công dân | Data | No | No | 9 hoặc 12 chữ số | Người dùng nhập |
| Ngày sinh | `date_of_birth` | Ngày tháng năm sinh | Date | No | No | $\le$ Ngày hiện tại | Người dùng nhập |
| Giới tính | `gender` | Giới tính | Select | No | No | `Nam`, `Nữ`, `Khác` | Người dùng chọn |
| Địa chỉ | `primary_address` | Địa chỉ nhà / giao hàng | Small Text | No | No | Văn bản tự do | Người dùng nhập |
| Tỉnh / Thành phố | `province_city` | Tỉnh thành phố cư trú | Select / Link | No | No | 63 tỉnh thành Việt Nam | Người dùng chọn |
| Phân loại KH | `customer_type` | Loại đối tượng | Select | Yes | No | `Individual` (Cá nhân), `Corporate` (Doanh nghiệp), `Anonymous` (Vãng lai) | Mặc định: `Individual` |
| Trạng thái hồ sơ | `status` | Tình trạng tài khoản | Select | Yes | No | `Active` (Hoạt động), `Inactive` (Ngừng), `Merged` (Đã gộp) | Mặc định: `Active` |
| Ngày khởi tạo | `creation` | Thời điểm tạo hồ sơ | Datetime | Yes | No | Timestamp hệ thống | Hệ thống tự ghi |

---

### 2. Bảng Hồ sơ Hội viên Smember (`Smember_Profile` / `tabSmember Profile`)
Quản lý hạng mức thành viên, tổng tích lũy chi tiêu và điểm thưởng ưu đãi tại CellphoneS.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã hồ sơ | `name` / `profile_id` | Mã hồ sơ Smember | Data | Yes | Yes | Format: `SMB-.#####` | Hệ thống tự sinh |
| Khách hàng | `customer` | Liên kết sang Khách hàng | Link | Yes | Yes | Link tới `Customer` (1-1) | Hệ thống / Form |
| Hạng hội viên | `member_tier` | Hạng mức thẻ Smember | Select | Yes | No | `Smember` (Chuẩn), `S-VIP` (Hạng VIP) | Hệ thống tự cập nhật |
| Tổng chi tiêu tích lũy | `total_spent` | Tổng tiền đã mua hàng (VND) | Currency | Yes | No | Số dương $\ge 0$ | Tính từ đơn hàng |
| Điểm thưởng khả dụng | `reward_points` | Điểm tích lũy đổi quà/chiết khấu | Int | Yes | No | Số nguyên $\ge 0$ | Tính từ đơn hàng & CSKH |
| Hạn duy trì hạng | `tier_expiry_date` | Ngày hết hạn hạng VIP | Date | No | No | Ngày trong tương lai | Hệ thống tính toán |
| Ngày thăng hạng gần nhất | `last_upgrade_date` | Thời điểm lên hạng S-VIP | Datetime | No | No | Timestamp | Hệ thống tự ghi |

---

### 3. Bảng Nhu cầu Tư vấn / Khách tiềm năng (`Lead_Opportunity` / `tabLead`)
Quản lý thông tin khách đăng ký đặt trước máy (Pre-order) hoặc cần tư vấn trả góp từ các chiến dịch Marketing/Telesales.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã nhu cầu | `name` / `lead_id` | Mã Lead tiềm năng | Data | Yes | Yes | Format: `LEAD-.YYYY.-.#####` | Hệ thống tự sinh |
| Họ tên khách | `lead_name` | Tên người để lại thông tin | Data | Yes | No | Tối đa 140 ký tự | Người dùng nhập / Form |
| Số điện thoại | `mobile_no` | SĐT liên hệ tư vấn | Data | Yes | No | Chuỗi 10 chữ số | Web Landing / Khách |
| Email | `email_id` | Email nhận báo giá | Data | No | No | Email hợp lệ | Người dùng nhập |
| Kênh tiếp nhận | `source_channel` | Nguồn phát sinh Lead | Select | Yes | No | `Website_Preorder`, `Facebook_Ads`, `Zalo`, `Hotline`, `Direct_Store` | Web API / Agent chọn |
| Sản phẩm quan tâm | `interested_item` | Dòng sản phẩm cần mua | Link | Yes | No | Link tới `Item_Reference` (VD: iPhone 16 Pro Max) | Khách chọn / Agent |
| Loại nhu cầu | `lead_type` | Mục đích của khách | Select | Yes | No | `Pre_Order` (Đặt trước), `Tra_Gop` (Tư vấn góp), `Gia_Si` (Khách B2B) | Mặc định: `Pre_Order` |
| Trạng thái Lead | `status` | Tiến trình xử lý Telesales | Select | Yes | No | `Open`, `Contacted`, `Qualified`, `Converted`, `Lost` | Agent cập nhật |
| Nhân viên phụ trách | `lead_owner` | Telesales được phân công | Link | No | No | Link tới `User` | Hệ thống Auto-routing |
| Khách hàng chuyển đổi | `converted_customer` | Mã KH sau khi mua | Link | No | No | Link tới `Customer` (Nullable) | Hệ thống tự điền |
| Ghi chú nhu cầu | `notes` | Chi tiết phiên bản/màu sắc | Text | No | No | Văn bản tự do | Người dùng nhập |

---

### 4. Bảng Tương tác Khách hàng (`Customer_Interaction` / `tabCustomer Interaction`)
Nhật ký tiếp xúc đa kênh qua Hotline, Zalo OA, Fanpage, Showroom CellphoneS.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã tương tác | `name` / `interaction_id` | Mã định danh tương tác | Data | Yes | Yes | Format: `INT-.YYYY.-.#####` | Hệ thống tự sinh |
| Khách hàng | `customer` | Khách hàng liên quan | Link | No | No | Link tới `Customer` (Nullable) | Chọn hoặc để trống |
| SĐT liên hệ | `contact_phone` | Số máy gọi đến / chat | Data | Yes | No | Chuỗi 10 số | Tổng đài CTI / Nhập |
| Tên người liên hệ | `contact_name` | Tên người gọi / chat | Data | No | No | Tối đa 140 ký tự | Khách khai báo |
| Kênh tiếp xúc | `channel` | Phương thức giao tiếp | Select | Yes | No | `Hotline_1800`, `Zalo_OA`, `Fanpage`, `Showroom_Store`, `Website_Chat` | Tự động / Agent |
| Phân loại tương tác | `interaction_type` | Mục đích giao dịch | Select | Yes | No | `Tu_van_mua_hang`, `Tra_cuu_don_hang`, `Bao_hanh_sua_chua`, `Khieu_nai_dich_vu` | Agent chọn |
| Tóm tắt nội dung | `summary` | Biên bản tóm tắt trao đổi | Small Text | Yes | No | Văn bản mô tả | Agent nhập |
| Điểm đánh giá CSAT | `satisfaction_rating` | Mức độ hài lòng | Select | No | No | `1_Sao`, `2_Sao`, `3_Sao`, `4_Sao`, `5_Sao` | Khách chấm qua IVR/ZNS |
| Nhân viên tiếp nhận | `staff_agent` | Agent nghe máy / trực chat | Link | Yes | No | Link tới `User` | User đăng nhập |
| Thời điểm tương tác | `interaction_time` | Giờ phát sinh cuộc gọi/chat | Datetime | Yes | No | Timestamp | Hệ thống tự ghi |
| Phiếu hỗ trợ tạo nhanh | `escalated_ticket` | Ticket phát sinh từ cuộc gọi | Link | No | No | Link tới `Support_Ticket` | Hệ thống tự liên kết |

---

### 5. Bảng Phiếu hỗ trợ (`Support_Ticket` / `tabSupport Ticket` hoặc `tabIssue`)
Quản lý toàn bộ yêu cầu bảo hành, đổi trả, khiếu nại chất lượng sản phẩm/dịch vụ tại CellphoneS.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã phiếu hỗ trợ | `name` / `ticket_id` | Mã số Ticket | Data | Yes | Yes | Format: `TCK-.YYYY.-.#####` | Hệ thống tự sinh |
| Khách hàng | `customer` | Khách hàng mở yêu cầu | Link | No | No | Link tới `Customer` (Nullable) | Chọn hoặc `CUST-GUEST` |
| SĐT liên hệ | `contact_phone` | SĐT tiếp nhận phản hồi | Data | Yes | No | Chuỗi 10 chữ số | Lấy từ KH hoặc nhập |
| Tên người liên hệ | `contact_name` | Tên người nhận thông tin | Data | Yes | No | Tối đa 140 ký tự | Lấy từ KH hoặc nhập |
| Kênh tiếp nhận | `channel` | Nguồn mở Ticket | Select | Yes | No | `Hotline`, `Showroom`, `Zalo_OA`, `Website` | Mặc định: `Showroom` |
| Sản phẩm liên quan | `item_code` | Mã SKU thiết bị lỗi | Link | Yes | No | Link tới `Item_Reference` | Agent chọn |
| Số Serial / IMEI | `serial_imei` | Mã định danh thiết bị | Data | Yes | No | 15 số (IMEI) hoặc Serial chuẩn | Agent quét / nhập |
| Nhóm vấn đề | `issue_category` | Danh mục khiếu nại | Select | Yes | No | `Loi_phan_cung_NSX`, `Doi_tra_30_ngay_VIP`, `Khi_khieu_nai_thai_do`, `Ho_tro_phan_mem` | Agent phân loại |
| Mức độ ưu tiên | `priority` | Độ khẩn cấp xử lý | Select | Yes | No | `Low`, `Medium`, `High`, `Critical` | Hệ thống tính theo Smember |
| Trạng thái phiếu | `status` | Tiến trình xử lý (5 States) | Select | Yes | No | `Open`, `In_Progress`, `Pending_Vendor`, `Resolved`, `Closed` | Workflow điều khiển |
| Nhân viên phụ trách | `allocated_to` | Agent / Kỹ thuật viên chính | Link | No | No | Link tới `User` | Auto-routing / Gán tay |
| Đơn vị xử lý | `assigned_dept` | Phòng ban chịu trách nhiệm | Select | Yes | No | `CSKH_Showroom`, `Trung_tam_Dien_Thoai_Vui`, `Hang_Apple_Care`, `Hang_Samsung` | Mặc định: `CSKH_Showroom` |
| Hạn chót cam kết SLA | `sla_deadline` | Thời hạn tối đa giải quyết | Datetime | Yes | No | $>\text{Thời gian tạo}$ | Hệ thống tự tính theo Matrix |
| Thời điểm giải quyết | `resolved_time` | Lúc hoàn thành sửa/đổi máy | Datetime | No | No | Timestamp | Hệ thống tự ghi |
| Thời điểm đóng phiếu | `closed_time` | Lúc khách ký nhận hoàn tất | Datetime | No | No | $\ge \text{resolved\_time}$ | Hệ thống tự ghi |
| Phương án giải quyết | `resolution_type` | Kết quả xử lý | Select | No | No | `Doi_may_moi_100`, `Sua_chua_thay_linh_kien`, `Bao_hanh_hang`, `Hoan_tien`, `Tu_choi_do_roi_vo` | Kỹ thuật / Manager chọn |
| Nguyên nhân lỗi | `root_cause` | Báo cáo chẩn đoán lỗi | Small Text | No | No | Bắt buộc nhập khi Resolved | Kỹ thuật viên DTV ghi |
| Ghi chú khắc phục | `resolution_notes` | Nội dung đã sửa chữa | Small Text | No | No | Bắt buộc nhập khi Closed | Kỹ thuật viên DTV ghi |
| Đánh giá CSAT | `csat_score` | Điểm hài lòng sau đóng phiếu | Select | No | No | `1_Sao`, `2_Sao`, `3_Sao`, `4_Sao`, `5_Sao` | Khách chấm qua Zalo ZNS |

---

### 6. Bảng Chi tiết Linh kiện & Ngoại quan (`Ticket_Repair_Item` - Child Table)
Bảng con nhúng trong `Support_Ticket` để ghi nhận tình trạng máy tiếp nhận gửi qua Điện Thoại Vui.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã dòng chi tiết | `name` | ID dòng con | Data | Yes | Yes | Frappe Hash ID | Hệ thống tự sinh |
| Phiếu cha | `parent` | Liên kết phiếu hỗ trợ | Data | Yes | No | Link tới `Support_Ticket` | Hệ thống tự gán |
| Hạng mục / Linh kiện | `fault_component` | Bộ phận phát hiện lỗi | Select | Yes | No | `Man_hinh_OLED`, `Pin_Chai_Phong`, `Mainboard_Nguon`, `Camera_Loi_Net`, `Vo_May_Suon` | Kỹ thuật viên chọn |
| Tình trạng ngoại quan | `initial_condition` | Vết trầy xước lúc nhận | Select | Yes | No | `May_dep_nhu_moi`, `Tray_xuoc_nhe`, `Can_mop_goc`, `Vo_kinh_lung` | Kỹ thuật viên chọn |
| Phụ kiện giữ lại | `accessories` | Đồ đi kèm khách gửi | Data | No | No | VD: "Hộp máy, Củ sạc 20W, Không có cáp" | Ghi nhận lúc nhận máy |
| Chi phí phát sinh (VND)| `estimated_cost` | Báo giá ngoài bảo hành | Currency | No | No | $\ge 0$ (0đ nếu bảo hành miễn phí) | Kỹ thuật DTV báo giá |
| Tình trạng duyệt giá | `warranty_status` | Diện bảo hành | Select | Yes | No | `Bao_hanh_Chinh_hang`, `Bao_hanh_VIP_1_doi_1`, `Sua_chua_Co_phi` | Kỹ thuật viên chọn |

---

### 7. Bảng Nhật ký Xử lý Phiếu (`Ticket_Activity_Log` - Child Table / Audit)
Bảng con ghi nhận chi tiết lịch sử chuyển giao và ghi chú nội bộ giữa CSKH Showroom và Điện Thoại Vui.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã nhật ký | `name` | ID dòng nhật ký | Data | Yes | Yes | Frappe Hash ID | Hệ thống tự sinh |
| Phiếu cha | `parent` | Liên kết phiếu hỗ trợ | Data | Yes | No | Link tới `Support_Ticket` | Hệ thống tự gán |
| Nhân viên thực hiện | `staff_user` | Người tác động | Link | Yes | No | Link tới `User` | User đăng nhập |
| Loại thao tác | `action_type` | Loại hành động | Select | Yes | No | `Chuyen_trang_thai`, `Dieu_phoi_Dien_Thoai_Vui`, `Cap_nhat_ghi_chu`, `Gui_thong_bao_khach` | Tự động ghi nhận |
| Trạng thái trước | `from_status` | Trạng thái cũ | Data | No | No | Trạng thái trước đổi | Hệ thống lấy tự động |
| Trạng thái sau | `to_status` | Trạng thái mới | Data | No | No | Trạng thái sau đổi | Hệ thống lấy tự động |
| Nội dung trao đổi | `comments` | Ghi chú kỹ thuật nội bộ | Small Text | Yes | No | Văn bản ghi chú | Nhân viên nhập |
| Thời điểm ghi nhận | `logged_at` | Giờ thực hiện thao tác | Datetime | Yes | No | Timestamp | Hệ thống tự ghi |

---

### 8. Bảng Sản phẩm Tham chiếu (`Item_Reference` / `tabItem`)
Quản lý danh mục hàng hóa CellphoneS phục vụ đối chiếu bảo hành và giá bán.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã sản phẩm (SKU) | `item_code` | Mã SKU sản phẩm | Data | Yes | Yes | Format: `SP-XXXXX` | Master Data đồng bộ |
| Tên sản phẩm | `item_name` | Tên thương mại sản phẩm | Data | Yes | No | Tối đa 250 ký tự | Master Data đồng bộ |
| Nhãn hàng | `brand` | Hãng sản xuất | Link / Data | Yes | No | `Apple`, `Samsung`, `Xiaomi`, `Asus`, `Sony` | Master Data |
| Nhóm ngành hàng | `item_group` | Phân loại thiết bị | Link | Yes | No | `Điện thoại`, `Máy tính bảng`, `Laptop`, `Phụ kiện` | Master Data |
| Thời gian bảo hành gốc| `standard_warranty_months` | Số tháng bảo hành tiêu chuẩn | Int | Yes | No | Giá trị: $12, 24, 36$ (Tháng) | Master Data |
| Tình trạng kinh doanh | `is_active` | Trạng thái mở bán | Check | Yes | No | `1` (Active), `0` (Discontinued) | Master Data |

---

### 9. Bảng Lịch sử Gộp Hồ sơ Khách hàng (`Customer_Merge_Log`)
Lưu vết kiểm toán khi sáp nhập 2 hồ sơ trùng lặp tại CellphoneS.

| Tên logic | Tên vật lý | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| Mã phiên gộp | `name` / `merge_id` | Mã định danh phiên gộp | Data | Yes | Yes | Format: `MRG-.YYYY.-.#####` | Hệ thống tự sinh |
| Hồ sơ nguồn (Bị xóa/gộp)| `source_customer` | Mã khách hàng phụ | Link | Yes | No | Link tới `Customer` (Status Merged) | Manager chọn |
| Hồ sơ đích (Giữ lại) | `target_customer` | Mã khách hàng chính | Link | Yes | No | Link tới `Customer` (Status Active) | Manager chọn |
| Lý do gộp hồ sơ | `merge_reason` | Biện luận nghiệp vụ | Select | Yes | No | `Trung_So_Dien_Thoai`, `Cung_So_CCCD`, `Yeu_cau_xac_minh_khach` | Manager chọn |
| Số Ticket đã chuyển | `migrated_tickets` | Số lượng ticket dời sang | Int | Yes | No | $\ge 0$ | Hệ thống tự đếm |
| Số tương tác đã chuyển | `migrated_interactions`| Số lượng cuộc gọi/chat dời sang | Int | Yes | No | $\ge 0$ | Hệ thống tự đếm |
| Điểm Smember đã dồn | `transferred_points` | Số điểm tích lũy cộng dồn | Int | Yes | No | $\ge 0$ | Hệ thống tự tính |
| Người thực hiện gộp | `executed_by` | User Quản lý CSKH | Link | Yes | No | Link tới `User` (Role CSKH Manager) | User đăng nhập |
| Thời gian thực hiện | `executed_at` | Giờ thực hiện gộp | Datetime | Yes | No | Timestamp | Hệ thống tự ghi |

---

## PHẦN 2: CÁC QUY TẮC KIỂM TRA TÍNH HỢP LỆ (VALIDATION RULES)

### 1. Quy tắc Định dạng Số điện thoại Việt Nam (Regex Validation)
* **Áp dụng:** Trường `phone_number` (`Customer`), `mobile_no` (`Lead`), `contact_phone` (`Support_Ticket`, `Customer_Interaction`).
* **Mẫu Regex:** `^(0[3|5|7|8|9])[0-9]{8}$`
* **Diễn giải:** Bắt đầu bằng chữ số 0, theo sau bởi các đầu mạng viễn thông Việt Nam (Viettel: 03, 086, 096...; Mobifone: 07, 089, 090...; Vinaphone: 081-085, 088, 091...), tổng chiều dài chính xác 10 ký tự số.

### 2. Quy tắc Định dạng Số Serial / IMEI thiết bị di động
* **Áp dụng:** Trường `serial_imei` trong `Support_Ticket`.
* **Mẫu Regex IMEI:** `^[0-9]{15}$` (Đúng 15 chữ số theo chuẩn quốc tế GSMA).
* **Kiểm tra bổ sung (Luhn Algorithm):** Chữ số thứ 15 là Check Digit kiểm tra tính hợp lệ của dãy số IMEI trước khi lưu Ticket.

### 3. Quy tắc Ràng buộc Logic Thời gian & Trạng thái Ticket
* **Quy tắc ngày đóng phiếu:** `closed_time >= creation` và `closed_time >= resolved_time`. Hệ thống không cho phép ngày đóng phiếu xảy ra trước ngày tạo hoặc trước ngày hoàn tất sửa chữa.
* **Quy tắc bắt buộc giải trình lỗi khi nghiệm thu:** Khi chuyển trạng thái sang `Resolved` hoặc `Closed`, 2 trường `root_cause` và `resolution_notes` **bắt buộc không được để trống**.
* **Quy tắc tính toán SLA tự động theo hạng Smember:**
  * *Hạng S-VIP:* Cam kết xử lý khiếu nại trong vòng **4 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 4\text{h}$).
  * *Hạng Smember Standard:* Cam kết xử lý trong vòng **24 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 24\text{h}$).
  * *Khách vãng lai:* Cam kết trong vòng **48 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 48\text{h}$).

### 4. Quy tắc Ràng buộc Chuyển đổi Khách tiềm năng (Lead Conversion)
* Khi `Lead_Opportunity.status` chuyển thành `'Converted'`, hệ thống kích hoạt kiểm tra:
  * Trường `converted_customer` **phải có giá trị** (đã tạo mới Customer hoặc liên kết Customer cũ).
  * Khách hàng được tạo phải có số điện thoại trùng khớp với `Lead.mobile_no`.

---

## PHẦN 3: BẢNG KHỚP NỐI TRẠNG THÁI VỚI STATE DIAGRAM CỦA NHẬT (ALIGNMENT)

| Trạng thái trong Từ điển CRM | Tên tiếng Việt trên Giao diện | Khớp nối State Diagram của Nhật | Mô tả ý nghĩa nghiệp vụ tại CellphoneS |
| :--- | :--- | :--- | :--- |
| `Open` | **Mới tạo** | `State: Open (Initial)` | Phiếu hỗ trợ vừa được tiếp nhận từ Hotline/Showroom, đang chờ phân công kỹ thuật. |
| `In_Progress` | **Đang xử lý** | `State: In Progress` | Nhân viên CSKH hoặc Kỹ thuật viên Điện Thoại Vui đang trực tiếp kiểm tra/thao tác trên máy. |
| `Pending_Vendor` | **Chờ Hãng / Linh kiện** | `State: Pending Vendor` | Máy đã được gửi sang Trung tâm bảo hành Apple/Samsung hoặc đang đợi điều phối linh kiện về trung tâm DTV. |
| `Resolved` | **Đã giải quyết** | `State: Resolved` | Máy đã sửa xong, hoặc chính sách đổi mới 1-đổi-1 đã được duyệt hoàn tất; sẵn sàng bàn giao cho khách. |
| `Closed` | **Đóng phiếu** | `State: Closed (Final)` | Khách đã kiểm tra, ký nhận bàn giao thiết bị và hệ thống kích hoạt tin nhắn khảo sát CSAT. |
