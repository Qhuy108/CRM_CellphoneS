# SẢN PHẨM BÀN GIAO D02: TỪ ĐIỂN DỮ LIỆU & QUY TẮC KIỂM TRA
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D02  
**Thuộc nhiệm vụ:** Nhiệm vụ 3 (Lập Từ điển Dữ liệu & Quy tắc kiểm tra tính hợp lệ)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Nền tảng mục tiêu:** Frappe Framework v15 / ERPNext v15 (MariaDB / PostgreSQL)  
**Trạng thái:** Hoàn thiện Mốc M4 (Final Deliverable)

---

## PHẦN 1: BẢNG TỪ ĐIỂN DỮ LIỆU CHI TIẾT (8 CỘT CHUẨN ĐẦU RA)

### 1. Bảng Khách hàng (`Customer` / `tabCustomer`)
Quản lý thông tin định danh cá nhân, phân loại và trạng thái tài khoản khách hàng CellphoneS.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã khách hàng** | `name` / `customer_id` | Mã định danh duy nhất của khách | Data / VARCHAR(140) | Yes | Yes | Format: `CUST-.YYYY.-.#####` | Hệ thống tự sinh tự động tăng theo năm |
| **Số điện thoại** | `phone_number` | SĐT chính liên hệ và tích điểm | Data / VARCHAR(20) | Yes | Yes | Chuỗi 10 số (03x, 05x, 07x, 08x, 09x) | Người dùng nhập; Regex VN 10 chữ số |
| **Họ và tên** | `customer_name` | Họ tên khách hàng | Data / VARCHAR(140) | Yes | No | Chuỗi ký tự, tối đa 140 ký tự | Người dùng nhập; Không chứa ký tự số |
| **Email** | `email_id` | Thư điện tử nhận hóa đơn VAT | Data / VARCHAR(140) | No | No | Chuỗi định dạng email RFC 5322 | Người dùng nhập; Validate email format |
| **Số CCCD / CMND** | `identity_card` | Căn cước công dân | Data / VARCHAR(20) | No | No | Chuỗi 9 hoặc 12 chữ số | Người dùng nhập; Regex: `^[0-9]{9,12}$` |
| **Ngày sinh** | `date_of_birth` | Ngày sinh để gửi quà sinh nhật | Date / DATE | No | No | Ngày hợp lệ trong quá khứ | Người dùng chọn; $\text{date\_of\_birth} \le \text{today}$ |
| **Giới tính** | `gender` | Giới tính xưng hô | Select / VARCHAR(20) | No | No | `Nam`, `Nữ`, `Khác` | Người dùng chọn từ Dropdown |
| **Địa chỉ** | `primary_address` | Địa chỉ nhà / giao hàng | Small Text / TEXT | No | No | Văn bản tự do tối đa 500 ký tự | Người dùng nhập |
| **Tỉnh / Thành phố** | `province_city` | Tỉnh thành phố cư trú | Select / VARCHAR(100) | No | No | 63 tỉnh thành Việt Nam | Người dùng chọn danh mục chuẩn |
| **Phân loại KH** | `customer_type` | Loại đối tượng khách hàng | Select / VARCHAR(50) | Yes | No | `Individual`, `Corporate`, `Anonymous` | Mặc định: `Individual` (Vãng lai: `Anonymous`) |
| **Trạng thái hồ sơ** | `status` | Tình trạng hoạt động hồ sơ | Select / VARCHAR(50) | Yes | No | `Active`, `Inactive`, `Merged` | Mặc định: `Active` (`Merged` khi đã gộp) |
| **Ngày khởi tạo** | `creation` | Thời điểm tạo hồ sơ vào CRM | Datetime / DATETIME(6) | Yes | No | Timestamp hệ thống | Hệ thống tự ghi nhận lúc Insert |

---

### 2. Bảng Hồ sơ Hội viên Smember (`Smember_Profile` / `tabSmember Profile`)
Quản lý chính sách thăng hạng, tích lũy doanh số mua hàng và điểm thưởng chiết khấu của CellphoneS.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã hồ sơ Smember**| `name` / `profile_id` | Mã hồ sơ hội viên | Data / VARCHAR(140) | Yes | Yes | Format: `SMB-.#####` | Hệ thống tự sinh tự động tăng |
| **Khách hàng** | `customer` | Khách hàng sở hữu hồ sơ | Link / VARCHAR(140) | Yes | Yes | Link tới `Customer` (Quan hệ 1-1) | Hệ thống gán; Ràng buộc Unique Foreign Key |
| **Hạng hội viên** | `member_tier` | Hạng mức thẻ Smember | Select / VARCHAR(50) | Yes | No | `Smember` (Chuẩn), `S-VIP` (Hạng VIP) | Hệ thống tự thăng hạng khi tích lũy $\ge 50$tr |
| **Tổng tích lũy chi tiêu**| `total_spent` | Tổng tiền mua hàng lũy kế | Currency / DECIMAL(18,2) | Yes | No | Số dương $\ge 0$ (VND) | Tự động cộng dồn từ Hóa đơn bán hàng |
| **Điểm thưởng khả dụng**| `reward_points` | Điểm tích lũy đổi voucher/quà | Int / INT | Yes | No | Số nguyên $\ge 0$ | Tích $1\%$ giá trị đơn hàng; trừ khi đổi quà |
| **Hạn duy trì hạng** | `tier_expiry_date` | Hạn chót giữ quyền lợi VIP | Date / DATE | No | No | Ngày trong tương lai | Tự động gia hạn 12 tháng kể từ ngày nâng hạng |
| **Ngày thăng hạng gần nhất**| `last_upgrade_date`| Thời điểm lên hạng S-VIP | Datetime / DATETIME | No | No | Timestamp | Hệ thống tự ghi nhận khi vượt ngưỡng chi tiêu |

---

### 3. Bảng Chi nhánh / Cửa hàng (`Branch_Store` / `tabBranch Store`)
Quản lý mạng lưới Showroom CellphoneS và Trung tâm sửa chữa - bảo hành Điện Thoại Vui toàn quốc.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã chi nhánh** | `name` / `branch_id` | Mã định danh cửa hàng | Data / VARCHAR(140) | Yes | Yes | Format: `BR-.#####` (VD: `BR-00101`) | Hệ thống tự sinh / Master Data nhập ban đầu |
| **Tên chi nhánh** | `branch_name` | Tên Showroom / Trung tâm DTV | Data / VARCHAR(255) | Yes | No | Tối đa 255 ký tự | Master Data; VD: "Showroom 125 Lê Văn Việt" |
| **Loại cơ sở** | `branch_type` | Loại hình địa điểm kinh doanh | Select / VARCHAR(50) | Yes | No | `Showroom_Store`, `Dien_Thoai_Vui_Center` | Người dùng chọn; Phân biệt bán lẻ hay sửa chữa |
| **Địa chỉ chi tiết** | `address` | Số nhà, tên đường, phường xã | Small Text / TEXT | Yes | No | Văn bản địa chỉ đầy đủ | Người dùng nhập |
| **Tỉnh / Thành phố** | `province_city` | Tỉnh thành trực thuộc | Select / VARCHAR(100) | Yes | No | `TP. Hồ Chí Minh`, `Hà Nội`, `Đà Nẵng`... | Người dùng chọn từ danh mục tỉnh thành |
| **Hotline chi nhánh**| `hotline` | Số điện thoại liên hệ cửa hàng | Data / VARCHAR(20) | No | No | Đầu số cố định hoặc di động | Regex số điện thoại hợp lệ |
| **Quản lý chi nhánh**| `manager_user` | Nhân sự chịu trách nhiệm ca/shop | Link / VARCHAR(140) | No | No | Link tới `User` (Role Store Manager) | Chọn từ danh sách User nội bộ |
| **Đang hoạt động** | `is_active` | Trạng thái mở cửa hoạt động | Check / INT(1) | Yes | No | `1` (Active), `0` (Closed) | Mặc định: `1` |

---

### 4. Bảng Nhu cầu Tư vấn / Cơ hội (`Lead_Opportunity` / `tabLead`)
Quản lý khách đăng ký đặt trước máy (Pre-order iPhone/Samsung Flagship) hoặc tư vấn trả góp từ Telesales.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã cơ hội tư vấn**| `name` / `lead_id` | Mã định danh Lead | Data / VARCHAR(140) | Yes | Yes | Format: `LEAD-.YYYY.-.#####` | Hệ thống tự sinh tự động tăng |
| **Họ tên khách** | `lead_name` | Tên khách để lại thông tin | Data / VARCHAR(140) | Yes | No | Tối đa 140 ký tự | Khách đăng ký Web Landing / Agent nhập |
| **Số điện thoại** | `mobile_no` | SĐT nhận cuộc gọi tư vấn | Data / VARCHAR(20) | Yes | No | Chuỗi 10 số (Regex VN) | Bắt buộc; Validate 10 chữ số |
| **Email** | `email_id` | Thư điện tử nhận báo giá cọc | Data / VARCHAR(140) | No | No | Email RFC 5322 hợp lệ | Web Landing Page / Form tư vấn |
| **Kênh tiếp nhận** | `source_channel` | Nguồn phát sinh nhu cầu | Select / VARCHAR(50) | Yes | No | `Website_Preorder`, `Facebook_Ads`, `Zalo`, `Hotline`, `Direct_Store` | Web API / Agent chọn |
| **Sản phẩm quan tâm**| `interested_item` | Thiết bị khách muốn mua | Link / VARCHAR(140) | Yes | No | Link tới `Item_Reference` | Chọn từ danh mục SKU sản phẩm |
| **Chi nhánh nhận máy**| `preferred_branch` | Showroom khách muốn ghé nhận | Link / VARCHAR(140) | No | No | Link tới `Branch_Store` | Khách chọn khi Pre-order |
| **Loại nhu cầu** | `lead_type` | Phân loại mục đích của khách | Select / VARCHAR(50) | Yes | No | `Pre_Order`, `Tra_Gop`, `Tu_Van_Ky_Thuat` | Mặc định: `Pre_Order` |
| **Trạng thái Lead** | `status` | Tiến trình Telesales (C03) | Select / VARCHAR(50) | Yes | No | `Open`, `Contacted`, `Qualified`, `Converted`, `Lost` | Đồng bộ 100% với State Diagram của Nhật |
| **Nhân viên phụ trách**| `lead_owner` | Telesales Agent được phân công | Link / VARCHAR(140) | No | No | Link tới `User` (Role CSKH/Telesales) | Auto-routing tự động chia đều Lead |
| **Khách hàng sau chuyển**| `converted_customer`| Khách hàng tạo sau khi cọc | Link / VARCHAR(140) | No | No | Link tới `Customer` (Nullable) | Bắt buộc có giá trị khi `status = 'Converted'` |
| **Ghi chú tư vấn** | `notes` | Màu sắc, dung lượng, lưu ý cọc | Text / TEXT | No | No | Văn bản ghi chú tự do | Telesales cập nhật sau mỗi cuộc gọi |

---

### 5. Bảng Tương tác Khách hàng (`Customer_Interaction` / `tabCustomer Interaction`)
Ghi nhận nhật ký tiếp xúc đa kênh qua Hotline 1800, Zalo OA, Fanpage, Web Chat hoặc Showroom.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã tương tác** | `name` / `interaction_id`| Mã định danh tương tác | Data / VARCHAR(140) | Yes | Yes | Format: `INT-.YYYY.-.#####` | Hệ thống tự sinh |
| **Khách hàng** | `customer` | Khách hàng liên quan | Link / VARCHAR(140) | No | No | Link tới `Customer` (Nullable) | Nullable (Hỗ trợ Khách vãng lai) |
| **SĐT người liên hệ**| `contact_phone` | Số điện thoại gọi đến / chat | Data / VARCHAR(20) | Yes | No | Chuỗi 10 số (Regex VN) | Tự động lấy từ CTI Tổng đài hoặc nhập |
| **Tên người liên hệ**| `contact_name` | Tên người gọi / chat | Data / VARCHAR(140) | No | No | Tối đa 140 ký tự | Lấy từ hồ sơ khách hoặc khách xưng tên |
| **Kênh tiếp xúc** | `channel` | Kênh diễn ra trao đổi | Select / VARCHAR(50) | Yes | No | `Hotline_1800`, `Zalo_OA`, `Fanpage`, `Showroom_Store`, `Website_Chat` | Tự động nhận diện từ Webhook / Chọn |
| **Chi nhánh tiếp nhận**| `branch` | Cửa hàng diễn ra giao tiếp | Link / VARCHAR(140) | No | No | Link tới `Branch_Store` | Nullable nếu gọi lên tổng đài trung tâm |
| **Phân loại giao dịch**| `interaction_type` | Mục đích của khách hàng | Select / VARCHAR(50) | Yes | No | `Tu_van_mua_hang`, `Tra_cuu_don_hang`, `Bao_hanh_sua_chua`, `Khieu_nai_dich_vu` | Agent phân loại |
| **Tóm tắt nội dung** | `summary` | Biên bản tóm tắt trao đổi | Small Text / TEXT | Yes | No | Văn bản mô tả tối đa 1000 ký tự | Agent nhập sau cuộc tiếp xúc |
| **Điểm hài lòng CSAT**| `satisfaction_rating`| Mức độ hài lòng tức thời | Select / VARCHAR(20) | No | No | `1_Sao`, `2_Sao`, `3_Sao`, `4_Sao`, `5_Sao` | Khách chấm qua IVR/ZNS |
| **Nhân viên tiếp nhận**| `staff_agent` | Agent trực tiếp xử lý | Link / VARCHAR(140) | Yes | No | Link tới `User` | Mặc định lấy User đang đăng nhập hệ thống |
| **Thời điểm tương tác**| `interaction_time` | Giờ phát sinh cuộc gọi/chat | Datetime / DATETIME | Yes | No | Timestamp | Hệ thống tự ghi |
| **Ticket phát sinh** | `escalated_ticket` | Mã phiếu hỗ trợ nếu có khiếu nại| Link / VARCHAR(140) | No | No | Link tới `Support_Ticket` (Nullable) | Tự động điền khi bấm nút "Tạo nhanh Ticket" |

---

### 6. Bảng Phiếu Hỗ Trợ (`Support_Ticket` / `tabSupport Ticket`)
Quản lý vòng đời khiếu nại, bảo hành, đổi trả 1-đổi-1 30 ngày và sửa chữa tại Điện Thoại Vui.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã phiếu hỗ trợ** | `name` / `ticket_id` | Mã định danh Ticket | Data / VARCHAR(140) | Yes | Yes | Format: `TCK-.YYYY.-.#####` | Hệ thống tự sinh tự động tăng |
| **Khách hàng** | `customer` | Khách hàng mở yêu cầu | Link / VARCHAR(140) | No | No | Link tới `Customer` (Nullable) | Để trống nếu vãng lai hoặc trỏ `CUST-GUEST` |
| **SĐT liên hệ** | `contact_phone` | Số nhận thông báo xử lý | Data / VARCHAR(20) | Yes | No | Chuỗi 10 số (Regex VN) | Bắt buộc; Validate 10 chữ số |
| **Tên người liên hệ** | `contact_name` | Tên người nhận kết quả | Data / VARCHAR(140) | Yes | No | Tối đa 140 ký tự | Lấy từ KH hoặc người mang máy tới quầy |
| **Kênh tiếp nhận** | `channel` | Nguồn mở yêu cầu | Select / VARCHAR(50) | Yes | No | `Hotline`, `Showroom`, `Zalo_OA`, `Website` | Mặc định: `Showroom` |
| **Chi nhánh tiếp nhận**| `branch` | Cửa hàng nhận máy ban đầu | Link / VARCHAR(140) | Yes | No | Link tới `Branch_Store` | Bắt buộc chọn cửa hàng tiếp nhận |
| **Mã sản phẩm lỗi** | `item_code` | SKU thiết bị bảo hành | Link / VARCHAR(140) | Yes | No | Link tới `Item_Reference` | Agent chọn từ danh mục sản phẩm |
| **Số Serial / IMEI** | `serial_imei` | Mã định danh phần cứng máy | Data / VARCHAR(50) | Yes | No | 15 số (IMEI GSMA) hoặc Serial chuẩn | Regex 15 số; Kiểm tra thuật toán Luhn |
| **Hóa đơn mua cũ** | `sales_invoice` | Hóa đơn xuất bán thiết bị | Link / VARCHAR(140) | No | No | Link tới `Sales_Invoice_Reference` | Dùng đối soát thời hạn 30 ngày đổi 1-1 |
| **Nhóm vấn đề** | `issue_category` | Phân loại lỗi khiếu nại | Select / VARCHAR(50) | Yes | No | `Loi_phan_cung_NSX`, `Doi_tra_30_ngay_VIP`, `Khieu_nai_thai_do`, `Ho_tro_phan_mem` | Quyết định điều phối phòng ban xử lý |
| **Mức độ ưu tiên** | `priority` | Mức độ khẩn cấp xử lý | Select / VARCHAR(20) | Yes | No | `Low`, `Medium`, `High`, `Critical` | Tự động set `Critical` nếu khách là S-VIP |
| **Trạng thái phiếu** | `status` | Tiến trình xử lý (C03 State) | Select / VARCHAR(50) | Yes | No | `Open`, `In_Progress`, `Pending_Vendor`, `Resolved`, `Closed` | Điều khiển bởi Workflow Engine 5 trạng thái |
| **Nhân viên phụ trách**| `allocated_to` | Agent / Kỹ thuật viên chính | Link / VARCHAR(140) | No | No | Link tới `User` | Auto-routing hoặc phân công thủ công |
| **Đơn vị xử lý** | `assigned_dept` | Phòng ban chịu trách nhiệm | Select / VARCHAR(50) | Yes | No | `CSKH_Showroom`, `Trung_tam_Dien_Thoai_Vui`, `Hang_Apple_Care`, `Hang_Samsung` | Mặc định: `CSKH_Showroom` |
| **Hạn chót SLA** | `sla_deadline` | Thời hạn tối đa giải quyết | Datetime / DATETIME | Yes | No | Ngày giờ tương lai $> \text{creation}$ | Tự động tính: S-VIP (4h), Thường (24h) |
| **Thời điểm giải quyết**| `resolved_time` | Lúc xong sửa chữa / duyệt đổi | Datetime / DATETIME | No | No | Timestamp | Ghi tự động khi chuyển `Resolved` |
| **Thời điểm đóng phiếu**| `closed_time` | Lúc khách nhận máy & CSAT | Datetime / DATETIME | No | No | $\text{closed\_time} \ge \text{resolved\_time}$ | Ghi tự động khi chuyển `Closed` |
| **Phương án xử lý** | `resolution_type` | Kết luận phương án khắc phục | Select / VARCHAR(50) | No | No | `Doi_may_moi_100`, `Sua_chua_thay_linh_kien`, `Bao_hanh_hang`, `Hoan_tien`, `Tu_choi_do_roi_vo` | Bắt buộc nhập khi trạng thái là `Resolved` |
| **Nguyên nhân lỗi** | `root_cause` | Chẩn đoán nguyên nhân kỹ thuật | Small Text / TEXT | No | No | Văn bản giải trình kỹ thuật | Bắt buộc nhập khi chuyển `Resolved` |
| **Ghi chú khắc phục** | `resolution_notes` | Nội dung chi tiết đã xử lý | Small Text / TEXT | No | No | Văn bản bàn giao giao nhận | Bắt buộc nhập khi chuyển `Closed` |
| **Đánh giá CSAT** | `csat_score` | Điểm hài lòng sau đóng phiếu | Select / VARCHAR(20) | No | No | `1_Sao`, `2_Sao`, `3_Sao`, `4_Sao`, `5_Sao` | Ghi nhận từ Webhook Zalo ZNS phản hồi |

---

### 7. Bảng Dữ Liệu Mua Hàng / Hóa Đơn (`Sales_Invoice_Reference` / `tabSales Invoice Reference`)
Lưu trữ thông tin tham chiếu lịch sử đơn hàng bán ra phục vụ kiểm tra bảo hành và chiết khấu Smember.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã hóa đơn** | `name` / `invoice_id` | Số hóa đơn VAT bán hàng | Data / VARCHAR(140) | Yes | Yes | Format: `INV-.YYYY.-.#####` | Master Data POS đồng bộ sang CRM |
| **Khách hàng mua** | `customer` | Khách hàng đứng tên mua | Link / VARCHAR(140) | Yes | No | Link tới `Customer` | POS đồng bộ |
| **Chi nhánh xuất bán**| `branch` | Cửa hàng xuất hóa đơn | Link / VARCHAR(140) | Yes | No | Link tới `Branch_Store` | POS đồng bộ |
| **Mã sản phẩm** | `item_code` | SKU thiết bị đã bán | Link / VARCHAR(140) | Yes | No | Link tới `Item_Reference` | POS đồng bộ |
| **Số Serial / IMEI** | `serial_imei` | Serial / IMEI thiết bị xuất bán | Data / VARCHAR(50) | Yes | No | 15 số (IMEI) hoặc Serial chuẩn | POS quét xuất kho |
| **Ngày giờ mua hàng**| `purchase_date` | Thời điểm thanh toán đơn hàng | Datetime / DATETIME | Yes | No | Timestamp quá khứ $\le \text{today}$ | POS ghi nhận |
| **Tổng tiền thanh toán**| `grand_total` | Giá trị thực trả sau giảm giá | Currency / DECIMAL(18,2) | Yes | No | Số dương $> 0$ (VND) | POS tính toán; Cộng dồn vào `total_spent` |
| **Hạn bảo hành gốc** | `warranty_expiry_date`| Ngày hết hạn bảo hành chính hãng| Date / DATE | Yes | No | $\text{purchase\_date} + \text{warranty\_months}$ | Hệ thống tự tính toán theo chính sách hãng |
| **Phương thức trả tiền**| `payment_method` | Hình thức giao dịch | Select / VARCHAR(50) | Yes | No | `Tien_mat`, `Chuyen_khoan`, `Tra_gop_0%`, `The_tin_dung` | POS đồng bộ |
| **Trạng thái hóa đơn**| `invoice_status` | Tình trạng hiệu lực hóa đơn | Select / VARCHAR(50) | Yes | No | `Paid`, `Returned`, `Cancelled` | Mặc định: `Paid` (`Returned` khi đã đổi trả) |

---

### 8. Bảng Chi Tiết Linh Kiện & Ngoại Quan (`Ticket_Repair_Item` - Child Table)
Bảng con nhúng trong `Support_Ticket` để ghi nhận tình trạng máy khi gửi thẩm định/sửa chữa tại Điện Thoại Vui.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã dòng chi tiết** | `name` | ID dòng con | Data / VARCHAR(140) | Yes | Yes | Frappe Hash ID tự sinh | Hệ thống tự sinh |
| **Phiếu hỗ trợ cha** | `parent` | Liên kết phiếu cha | Data / VARCHAR(140) | Yes | No | Link tới `Support_Ticket` | Hệ thống tự động gán Cascade |
| **Linh kiện phát hiện lỗi**| `fault_component`| Bộ phận phần cứng lỗi | Select / VARCHAR(50) | Yes | No | `Man_hinh_OLED`, `Pin_Chai_Phong`, `Mainboard_Nguon`, `Camera_Loi_Net`, `Vo_May_Suon` | Kỹ thuật viên DTV chọn |
| **Tình trạng ngoại quan**| `initial_condition` | Ngoại quan máy lúc nhận | Select / VARCHAR(50) | Yes | No | `May_dep_nhu_moi`, `Tray_xuoc_nhe`, `Can_mop_goc`, `Vo_kinh_lung` | Kỹ thuật viên DTV chọn |
| **Phụ kiện giữ lại** | `accessories` | Đồ đính kèm khách bàn giao | Data / VARCHAR(255) | No | No | Chuỗi mô tả (VD: Củ sạc 20W, Hộp máy) | Agent / KTV ghi nhận lúc nhận máy |
| **Chi phí phát sinh** | `estimated_cost` | Báo giá ngoài bảo hành | Currency / DECIMAL(18,2) | No | No | Số dương $\ge 0$ (0đ nếu bảo hành miễn phí) | Kỹ thuật DTV báo giá cho khách duyệt |
| **Diện bảo hành** | `warranty_status` | Chế độ bảo hành áp dụng | Select / VARCHAR(50) | Yes | No | `Bao_hanh_Chinh_hang`, `Bao_hanh_VIP_1_doi_1`, `Sua_chua_Co_phi` | Kỹ thuật viên chọn theo chính sách |

---

### 9. Bảng Nhật Ký Xử Lý Phiếu (`Ticket_Activity_Log` - Child Table)
Bảng con lưu vết toàn bộ trao đổi nội bộ, ghi chú kỹ thuật và chuyển trạng thái giữa Showroom và Điện Thoại Vui.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã nhật ký** | `name` | ID dòng nhật ký | Data / VARCHAR(140) | Yes | Yes | Frappe Hash ID tự sinh | Hệ thống tự sinh |
| **Phiếu hỗ trợ cha** | `parent` | Liên kết phiếu cha | Data / VARCHAR(140) | Yes | No | Link tới `Support_Ticket` | Hệ thống tự động gán Cascade |
| **Người thực hiện** | `staff_user` | Nhân sự tác động bản ghi | Link / VARCHAR(140) | Yes | No | Link tới `User` | Hệ thống lấy User đang thao tác |
| **Loại hành động** | `action_type` | Bản chất thao tác | Select / VARCHAR(50) | Yes | No | `Chuyen_trang_thai`, `Dieu_phoi_DTV`, `Cap_nhat_ghi_chu`, `Gui_thong_bao_khach` | Tự động nhận diện |
| **Trạng thái trước** | `from_status` | Trạng thái cũ | Data / VARCHAR(50) | No | No | Trạng thái trước khi đổi | Hệ thống tự điền |
| **Trạng thái sau** | `to_status` | Trạng thái mới | Data / VARCHAR(50) | No | No | Trạng thái sau khi đổi | Hệ thống tự điền |
| **Ghi chú nội bộ** | `comments` | Nội dung trao đổi kỹ thuật | Small Text / TEXT | Yes | No | Văn bản ghi chú chi tiết | Nhân sự nhập |
| **Thời điểm ghi nhận**| `logged_at` | Giờ thao tác | Datetime / DATETIME | Yes | No | Timestamp | Hệ thống tự ghi |

---

### 10. Bảng Sản Phẩm Tham Chiếu (`Item_Reference` / `tabItem`)
Danh mục sản phẩm điện thoại, máy tính bảng, phụ kiện CellphoneS kinh doanh phục vụ tra cứu bảo hành.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã sản phẩm (SKU)** | `item_code` | Mã SKU sản phẩm | Data / VARCHAR(140) | Yes | Yes | Format: `SP-XXXXX` | Master Data đồng bộ |
| **Tên sản phẩm** | `item_name` | Tên thương mại sản phẩm | Data / VARCHAR(255) | Yes | No | Tối đa 255 ký tự | Master Data đồng bộ |
| **Thương hiệu** | `brand` | Hãng sản xuất | Select / VARCHAR(100) | Yes | No | `Apple`, `Samsung`, `Xiaomi`, `Asus`, `Sony` | Master Data |
| **Nhóm ngành hàng** | `item_group` | Phân loại thiết bị | Select / VARCHAR(100) | Yes | No | `Điện thoại`, `Máy tính bảng`, `Laptop`, `Phụ kiện` | Master Data |
| **Thời gian bảo hành** | `standard_warranty_months`| Số tháng bảo hành tiêu chuẩn | Int / INT | Yes | No | Giá trị: $12, 24, 36$ (Tháng) | Master Data |
| **Đang kinh doanh** | `is_active` | Trạng thái mở bán sản phẩm | Check / INT(1) | Yes | No | `1` (Active), `0` (Discontinued) | Mặc định: `1` |

---

### 11. Bảng Lịch Sử Gộp Hồ Sơ Khách Hàng (`Customer_Merge_Log`)
Lưu vết kiểm toán khi Quản lý CSKH thực thi gộp 2 hồ sơ khách hàng trùng lặp.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã phiên gộp** | `name` / `merge_id` | Mã định danh phiên gộp | Data / VARCHAR(140) | Yes | Yes | Format: `MRG-.YYYY.-.#####` | Hệ thống tự sinh |
| **Hồ sơ nguồn (Bị gộp)**| `source_customer` | Mã khách hàng phụ bị dời | Link / VARCHAR(140) | Yes | No | Link tới `Customer` (Status Merged) | Manager chọn; Chuyển status = Merged |
| **Hồ sơ đích (Giữ lại)**| `target_customer` | Mã khách hàng chính giữ lại | Link / VARCHAR(140) | Yes | No | Link tới `Customer` (Status Active) | Manager chọn; Giữ nguyên hoạt động |
| **Lý do gộp hồ sơ** | `merge_reason` | Biện luận nghiệp vụ | Select / VARCHAR(50) | Yes | No | `Trung_So_Dien_Thoai`, `Cung_So_CCCD`, `Yeu_cau_xac_minh_khach` | Manager chọn |
| **Số Ticket đã chuyển**| `migrated_tickets` | Số lượng Ticket dời sang đích | Int / INT | Yes | No | Số nguyên $\ge 0$ | Hệ thống tự động đếm sau update FK |
| **Số tương tác đã dời**| `migrated_interactions`| Số lượng cuộc gọi/chat dời sang| Int / INT | Yes | No | Số nguyên $\ge 0$ | Hệ thống tự động đếm sau update FK |
| **Điểm Smember đã dồn**| `transferred_points` | Số điểm tích lũy cộng dồn | Int / INT | Yes | No | Số nguyên $\ge 0$ | Hệ thống tự tính và cộng dồn |
| **Người thực hiện gộp** | `executed_by` | User Quản lý CSKH duyệt gộp | Link / VARCHAR(140) | Yes | No | Link tới `User` (Role CSKH Manager) | Mặc định User quản lý đang đăng nhập |
| **Thời gian thực hiện** | `executed_at` | Giờ hoàn tất transaction | Datetime / DATETIME | Yes | No | Timestamp | Hệ thống tự ghi |

---

## PHẦN 2: CÁC QUY TẮC KIỂM TRA TÍNH HỢP LỆ CHI TIẾT (VALIDATION RULES)

### 1. Quy tắc Định dạng Số điện thoại Việt Nam (Regex Validation)
* **Trường áp dụng:** `Customer.phone_number`, `Lead.mobile_no`, `Support_Ticket.contact_phone`, `Customer_Interaction.contact_phone`.
* **Biểu thức chính quy (Regex):** `^(0[3|5|7|8|9])[0-9]{8}$`
* **Diễn giải:** Bắt đầu bằng chữ số `0`, theo sau bởi một trong các chữ số mạng viễn thông Việt Nam (`3`, `5`, `7`, `8`, `9`), và đúng 8 chữ số tiếp theo. Tổng chiều dài đúng 10 ký tự số.
* **Thông báo lỗi khi vi phạm:** *"Số điện thoại không hợp lệ! Vui lòng nhập đúng 10 chữ số thuộc các đầu mạng Việt Nam (03x, 05x, 07x, 08x, 09x)."*

### 2. Quy tắc Định dạng Số Serial / IMEI thiết bị di động
* **Trường áp dụng:** `Support_Ticket.serial_imei`, `Sales_Invoice_Reference.serial_imei`.
* **Biểu thức Regex:** `^[0-9]{15}$` (Đúng 15 chữ số theo chuẩn GSMA).
* **Kiểm tra thuật toán Luhn (Check Digit):** Chữ số thứ 15 được xác thực bằng thuật toán Modulo 10 của GSMA để loại bỏ số IMEI giả mạo.
* **Thông báo lỗi khi vi phạm:** *"Số IMEI không hợp lệ! Vui lòng nhập chính xác dãy 15 chữ số chuẩn GSMA hoặc quét mã vạch trên thân máy."*

### 3. Quy tắc Ràng buộc Logic Thời gian & Trạng thái Ticket
* **Quy tắc thời gian đóng phiếu:** `closed_time >= creation` và `closed_time >= resolved_time`.
* **Quy tắc giải trình bắt buộc khi hoàn tất:** Khi chuyển `status` sang `Resolved`, 2 trường `resolution_type` và `root_cause` **bắt buộc không được để trống**.
* **Quy tắc nghiệm thu đóng phiếu:** Khi chuyển `status` sang `Closed`, trường `resolution_notes` **bắt buộc không được để trống**.
* **Quy tắc tính toán SLA tự động theo hạng Smember:**
  * *Hạng S-VIP:* Cam kết xử lý trong vòng **4 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 4\text{h}$).
  * *Hạng Smember Standard:* Cam kết xử lý trong vòng **24 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 24\text{h}$).
  * *Khách vãng lai:* Cam kết xử lý trong vòng **48 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 48\text{h}$).

### 4. Quy tắc Ràng buộc Chuyển đổi Cơ hội (Lead Conversion Validation)
* Khi `Lead_Opportunity.status` chuyển thành `'Converted'`:
  * Trường `converted_customer` **bắt buộc phải có giá trị**.
  * Khách hàng được liên kết phải có `phone_number` trùng khớp với `Lead.mobile_no`.

---

## PHẦN 3: BẢNG ĐỒNG BỘ TRẠNG THÁI VỚI STATE DIAGRAM CỦA NHẬT (C03 ALIGNMENT)

### 1. Đồng bộ Trạng thái Phiếu Hỗ Trợ (`Support_Ticket.status`)

| Trạng thái trong Từ điển CRM | Tên tiếng Việt hiển thị UI | Trạng thái State Diagram C03 (Nhật) | Ý nghĩa nghiệp vụ chi tiết tại CellphoneS |
| :--- | :--- | :--- | :--- |
| `Open` | **Mới tạo** | `State: Open (Initial)` | Phiếu hỗ trợ vừa được tiếp nhận từ Hotline/Showroom, đang chờ phân công kỹ thuật. |
| `In_Progress` | **Đang xử lý** | `State: In Progress` | Nhân viên CSKH hoặc Kỹ thuật viên Điện Thoại Vui đang trực tiếp kiểm tra/thao tác trên máy. |
| `Pending_Vendor` | **Chờ Hãng / Linh kiện** | `State: Pending Vendor` | Máy đã gửi sang Trung tâm bảo hành Apple/Samsung hoặc đang đợi điều phối linh kiện về trung tâm DTV. |
| `Resolved` | **Đã giải quyết** | `State: Resolved` | Máy đã sửa xong, hoặc chính sách đổi mới 1-đổi-1 đã được duyệt hoàn tất; sẵn sàng giao khách. |
| `Closed` | **Đóng phiếu** | `State: Closed (Final)` | Khách đã kiểm tra, ký nhận bàn giao thiết bị và hệ thống kích hoạt tin nhắn Zalo ZNS khảo sát CSAT. |

### 2. Đồng bộ Trạng thái Cơ hội Tư vấn / Pre-order (`Lead_Opportunity.status`)

| Trạng thái trong Từ điển CRM | Tên tiếng Việt hiển thị UI | Trạng thái State Diagram C03 (Nhật) | Ý nghĩa nghiệp vụ chi tiết tại CellphoneS |
| :--- | :--- | :--- | :--- |
| `Open` | **Mới tiếp nhận** | `Lead: Open (New)` | Khách vừa đăng ký để lại thông tin đặt trước trên Website hoặc Fanpage. |
| `Contacted` | **Đã liên hệ** | `Lead: Contacted` | Nhân viên Telesales đã gọi điện tư vấn sản phẩm/chương trình ưu đãi. |
| `Qualified` | **Đủ điều kiện** | `Lead: Qualified` | Khách xác nhận chốt phiên bản/màu sắc và đồng ý nhận thông tin cọc. |
| `Converted` | **Đã chốt cọc / Mua** | `Lead: Converted (Success)` | Khách đã thanh toán cọc thành công, tạo Đơn hàng và hồ sơ `Customer`. |
| `Lost` | **Hủy / Không mua** | `Lead: Lost (Closed)` | Khách từ chối mua, đổi ý hoặc không liên lạc được sau 3 lần gọi. |

---

## PHẦN 4: QUY ĐỊNH DỮ LIỆU MẪU (SAMPLE DATA SPECIFICATION)

### 1. Dữ liệu Mẫu Bảng Khách hàng & Smember Profile

| `customer_id` | `phone_number` | `customer_name` | `email_id` | `identity_card` | `province_city` | `customer_type` | `status` | `member_tier` | `total_spent` | `reward_points` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `CUST-2026-00001` | `0908123456` | Nguyễn Văn An | an.nguyen@gmail.com | 079098001234 | TP. Hồ Chí Minh | Individual | Active | **S-VIP** | 85,400,000 đ | 1,250 pts |
| `CUST-2026-00002` | `0912345678` | Lê Thị Bích Trâm | tram.le@yahoo.com | 079195005678 | TP. Hồ Chí Minh | Individual | Active | **Smember** | 12,800,000 đ | 320 pts |
| `CUST-2026-00003` | `0987654321` | Hoàng Minh Quân | quan.hoang@fpt.com | 001099004321 | Hà Nội | Individual | Active | **S-VIP** | 142,000,000 đ | 2,100 pts |
| `CUST-GUEST` | `0000000000` | Khách vãng lai | guest@cellphones.com.vn| — | TP. Hồ Chí Minh | Anonymous | Active | **Smember** | 0 đ | 0 pts |

### 2. Dữ liệu Mẫu Bảng Chi nhánh (`Branch_Store`)

| `branch_id` | `branch_name` | `branch_type` | `address` | `province_city` | `hotline` | `manager_user` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `BR-00101` | Showroom 125 Lê Văn Việt | `Showroom_Store` | 125 Lê Văn Việt, P. Hiệp Phú, TP. Thủ Đức | TP. Hồ Chí Minh | 02871088125 | `thuan.cskh@cellphones.com.vn` |
| `BR-00102` | Showroom 213 Trần Quang Khải | `Showroom_Store` | 213 Trần Quang Khải, P. Tân Định, Quận 1 | TP. Hồ Chí Minh | 02871088213 | `linh.manager@cellphones.com.vn` |
| `BR-00201` | Trung tâm Điện Thoại Vui Q9 | `Dien_Thoai_Vui_Center` | 129 Lê Văn Việt, P. Hiệp Phú, TP. Thủ Đức | TP. Hồ Chí Minh | 02871010129 | `tu.dtv@cellphones.com.vn` |

### 3. Dữ liệu Mẫu Bảng Phiếu Hỗ Trợ (`Support_Ticket`)

| `ticket_id` | `customer` | `contact_phone` | `contact_name` | `item_code` | `serial_imei` | `issue_category` | `priority` | `status` | `assigned_dept` | `resolution_type` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `TCK-2026-00155` | `CUST-2026-00001` | `0908123456` | Nguyễn Văn An | `SP-IP15PM-256` | `358941098234112` | `Doi_tra_30_ngay_VIP`| Critical | `In_Progress` | `Trung_tam_Dien_Thoai_Vui` | `Doi_may_moi_100` |
| `TCK-2026-00150` | `CUST-2026-00002` | `0912345678` | Lê Thị Bích Trâm| `SP-SS-S24U` | `352109845120993` | `Loi_phan_cung_NSX` | High | `In_Progress` | `Trung_tam_Dien_Thoai_Vui` | `Sua_chua_thay_linh_kien` |
| `TCK-2026-00142` | `CUST-2026-00003` | `0987654321` | Hoàng Minh Quân | `SP-IPAD-M4` | `359981023455102` | `Doi_tra_30_ngay_VIP`| Critical | `Resolved` | `CSKH_Showroom` | `Doi_may_moi_100` |

---

## PHẦN 5: QUY ĐỊNH CỘT CSV CẦN NHẬP (CSV IMPORT SPECIFICATIONS)

Dưới đây là đặc tả chi tiết danh sách cột trong các file mẫu CSV phục vụ Import nạp dữ liệu ban đầu vào Frappe Framework:

### 1. File CSV Import Khách hàng (`Customer_Data_Import.csv`)
* **Mục đích:** Nạp danh sách khách hàng ban đầu từ hệ thống POS cũ sang CRM Frappe.
* **Quy định cột:**

| Header Cột CSV | Fieldname Frappe tương ứng | Bắt buộc | Kiểu dữ liệu | Ví dụ giá trị | Ghi chú kiểm tra |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `Phone Number` | `phone_number` | **BẮT BUỘC** | Text | `0908123456` | Khóa duy nhất (Unique), đúng 10 chữ số |
| `Full Name` | `customer_name` | **BẮT BUỘC** | Text | `Nguyễn Văn An` | Tên đầy đủ, không để trống |
| `Email` | `email_id` | Tùy chọn | Text | `an.nguyen@gmail.com`| Validate email format |
| `Identity Card` | `identity_card` | Tùy chọn | Text | `079098001234` | 9 hoặc 12 số CCCD |
| `Gender` | `gender` | Tùy chọn | Enum | `Nam` | Chọn: `Nam`, `Nữ`, `Khác` |
| `Province City` | `province_city` | Tùy chọn | Text | `TP. Hồ Chí Minh` | Tên tỉnh thành phố |
| `Customer Type` | `customer_type` | **BẮT BUỘC** | Enum | `Individual` | `Individual`, `Corporate`, `Anonymous` |
| `Smember Tier` | `custom_smember_tier` | **BẮT BUỘC** | Enum | `S-VIP` | `Smember`, `S-VIP` |
| `Total Spent` | `custom_total_spent` | Tùy chọn | Number | `85400000` | Số tiền tích lũy cũ (VND) |
| `Reward Points` | `custom_reward_points`| Tùy chọn | Number | `1250` | Điểm tích lũy cũ |

---

### 2. File CSV Import Chi nhánh Cửa hàng (`Branch_Store_Import.csv`)
* **Mục đích:** Nạp danh mục toàn bộ mạng lưới cửa hàng và trung tâm sửa chữa CellphoneS.
* **Quy định cột:**

| Header Cột CSV | Fieldname Frappe tương ứng | Bắt buộc | Kiểu dữ liệu | Ví dụ giá trị | Ghi chú kiểm tra |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `Branch ID` | `name` | **BẮT BUỘC** | Text | `BR-00101` | Mã chi nhánh duy nhất |
| `Branch Name` | `branch_name` | **BẮT BUỘC** | Text | `Showroom 125 Lê Văn Việt` | Tên hiển thị cửa hàng |
| `Branch Type` | `branch_type` | **BẮT BUỘC** | Enum | `Showroom_Store` | `Showroom_Store` hoặc `Dien_Thoai_Vui_Center` |
| `Address` | `address` | **BẮT BUỘC** | Text | `125 Lê Văn Việt, P. Hiệp Phú, Thủ Đức` | Địa chỉ cụ thể |
| `Province City` | `province_city` | **BẮT BUỘC** | Text | `TP. Hồ Chí Minh` | Tỉnh/TP |
| `Hotline` | `hotline` | Tùy chọn | Text | `02871088125` | Số điện thoại bàn/di động |
| `Is Active` | `is_active` | **BẮT BUỘC** | Int | `1` | `1` (Đang mở), `0` (Đóng cửa) |

---

### 3. File CSV Import Sản phẩm Tham chiếu (`Item_Reference_Import.csv`)
* **Mục đích:** Đồng bộ danh mục SKU thiết bị từ hệ thống ERP/Kho sang CRM.
* **Quy định cột:**

| Header Cột CSV | Fieldname Frappe tương ứng | Bắt buộc | Kiểu dữ liệu | Ví dụ giá trị | Ghi chú kiểm tra |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `Item SKU` | `item_code` | **BẮT BUỘC** | Text | `SP-IP15PM-256` | Mã SKU sản phẩm duy nhất |
| `Item Name` | `item_name` | **BẮT BUỘC** | Text | `iPhone 15 Pro Max 256GB Titan Tự Nhiên` | Tên thương mại |
| `Brand` | `brand` | **BẮT BUỘC** | Enum | `Apple` | `Apple`, `Samsung`, `Xiaomi`, `Asus`... |
| `Item Group` | `item_group` | **BẮT BUỘC** | Enum | `Điện thoại` | `Điện thoại`, `Laptop`, `Phụ kiện`... |
| `Warranty Months`| `standard_warranty_months`| **BẮT BUỘC** | Int | `12` | Số tháng bảo hành ($12, 24, 36$) |
| `Is Active` | `is_active` | **BẮT BUỘC** | Int | `1` | `1` (Mở bán), `0` (Ngừng kinh doanh) |
