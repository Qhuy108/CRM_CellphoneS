# SẢN PHẨM BÀN GIAO D02: TỪ ĐIỂN DỮ LIỆU & QUY TẮC KIỂM TRA
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D02  
**Thuộc nhiệm vụ:** Nhiệm vụ 3 (Lập Từ điển Dữ liệu & Quy tắc kiểm tra tính hợp lệ)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Nền tảng mục tiêu:** Frappe Framework v15.x (Custom App `cellphones_crm` độc lập) / MariaDB 10.6+ hoặc PostgreSQL 15+  
**Trạng thái:** Hoàn thiện Mốc M4 (Final Deliverable)

---

## PHẦN 1: BẢNG TỪ ĐIỂN DỮ LIỆU CHI TIẾT (8 CỘT CHUẨN ĐẦU RA)

### 1. Bảng Khách hàng (`Customer` / `tabCustomer`)
Quản lý mã định danh duy nhất, thông tin liên hệ và nhóm ưu đãi giáo dục của khách hàng CellphoneS.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã khách hàng** | `name` / `customer_id` | Mã định danh duy nhất của khách | Data / VARCHAR(140) | Yes | Yes | Format: `CUST-.YYYY.-.#####` | Hệ thống tự sinh tự động tăng theo năm |
| **Số điện thoại** | `phone_number` | SĐT chính liên hệ và tìm kiếm | Data / VARCHAR(20) | Yes | Yes | Chuỗi 10 số (03x, 05x, 07x, 08x, 09x) | Người dùng nhập; Cảnh báo trùng, không tự gộp |
| **Họ và tên** | `customer_name` | Họ tên khách hàng cá nhân | Data / VARCHAR(140) | Yes | No | Chuỗi ký tự, tối đa 140 ký tự | Người dùng nhập; Không chứa ký tự số |
| **Email** | `email_id` | Thư điện tử nhận thông tin/hóa đơn | Data / VARCHAR(140) | No | No | Chuỗi định dạng email RFC 5322 | Người dùng nhập; Validate email format |
| **Số CCCD / CMND** | `identity_card` | Căn cước công dân | Data / VARCHAR(20) | No | No | Chuỗi 9 hoặc 12 chữ số | Người dùng nhập; Regex: `^[0-9]{9,12}$` |
| **Ngày sinh** | `date_of_birth` | Ngày sinh để chăm sóc khách hàng | Date / DATE | No | No | Ngày hợp lệ trong quá khứ | Người dùng chọn; $\text{date\_of\_birth} \le \text{today}$ |
| **Giới tính** | `gender` | Giới tính xưng hô | Select / VARCHAR(20) | No | No | `Nam`, `Nữ`, `Khác` | Người dùng chọn từ Dropdown |
| **Địa chỉ** | `primary_address` | Địa chỉ nhà / liên hệ | Small Text / TEXT | No | No | Văn bản tự do tối đa 500 ký tự | Người dùng nhập |
| **Tỉnh / Thành phố** | `province_city` | Tỉnh thành phố cư trú | Select / VARCHAR(100) | No | No | 63 tỉnh thành Việt Nam | Người dùng chọn danh mục chuẩn |
| **Phân loại KH** | `customer_type` | Loại đối tượng khách hàng | Select / VARCHAR(50) | Yes | No | `Individual`, `Anonymous` | Mặc định: `Individual` (Vãng lai: `Anonymous`) |
| **Trạng thái hồ sơ** | `status` | Tình trạng hoạt động hồ sơ | Select / VARCHAR(50) | Yes | No | `Active`, `Inactive`, `Merged` | Mặc định: `Active` (`Merged` khi đã duyệt gộp) |
| **Ngày khởi tạo** | `creation` | Thời điểm tạo hồ sơ vào CRM | Datetime / DATETIME(6) | Yes | No | Timestamp hệ thống | Hệ thống tự ghi nhận lúc Insert |

---

### 2. Bảng Hồ sơ Hội viên Smember (`Smember_Profile` / `tabSmember Profile`)
Lưu trữ 4 hạng hội viên mẫu và nhóm ưu đãi giáo dục (nhập tay/CSV bởi người có quyền, chưa tính chi tiêu thăng hạng tự động trong MVP).

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã hồ sơ Smember**| `name` / `profile_id` | Mã hồ sơ hội viên | Data / VARCHAR(140) | Yes | Yes | Format: `SMB-.#####` | Hệ thống tự sinh tự động tăng |
| **Khách hàng** | `customer` | Khách hàng sở hữu hồ sơ | Link / VARCHAR(140) | Yes | Yes | Link tới `Customer` (Quan hệ 1-1) | Hệ thống gán; Ràng buộc Unique Foreign Key |
| **Hạng hội viên mẫu**| `member_tier` | 4 Hạng mẫu CellphoneS | Select / VARCHAR(50) | Yes | No | `S-NULL`, `S-NEW`, `S-MEM`, `S-VIP` | Nhập tay/CSV bởi người có quyền (chưa tự tính) |
| **Nhóm giáo dục** | `edu_type` | Ưu đãi Học sinh-SV / Giáo viên | Select / VARCHAR(50) | Yes | No | `None`, `S-Student`, `S-Teacher` | Mặc định: `None` |
| **Trạng thái xác minh GD**| `edu_status` | Tiến độ duyệt hồ sơ giáo dục | Select / VARCHAR(50) | Yes | No | `Chua_xac_minh`, `Da_xac_minh`, `Tu_choi` | Cập nhật bởi nhân viên có thẩm quyền |
| **Thời hạn nhóm GD** | `edu_expiry_date` | Thời hạn ưu đãi giáo dục | Date / DATE | No | No | Ngày trong tương lai | Ghi nhận khi đã xác minh thành công |
| **Người duyệt xác minh**| `edu_verified_by` | Nhân viên xác nhận hồ sơ GD | Link / VARCHAR(140) | No | No | Link tới `User` | Ghi nhận User thực hiện duyệt |
| **Ghi chú quyền lợi** | `tier_note` | Ghi chú chính sách áp dụng | Small Text / TEXT | No | No | Văn bản ghi chú chính sách | Người dùng nhập |

---

### 3. Bảng Chi nhánh / Cửa hàng (`Branch_Store` / `tabBranch Store`)
Quản lý mạng lưới Showroom CellphoneS (mô phỏng thử nghiệm cấp chuỗi bằng 2 cửa hàng).

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã chi nhánh** | `name` / `branch_id` | Mã định danh cửa hàng | Data / VARCHAR(140) | Yes | Yes | Format: `BR-.#####` (VD: `BR-00101`, `BR-00102`) | Master Data nhập ban đầu |
| **Tên chi nhánh** | `branch_name` | Tên Showroom cửa hàng | Data / VARCHAR(255) | Yes | No | Tối đa 255 ký tự | Master Data; VD: "Showroom 125 Lê Văn Việt" |
| **Loại cơ sở** | `branch_type` | Phân loại cơ sở | Select / VARCHAR(50) | Yes | No | `Showroom_Store`, `Dien_Thoai_Vui_Center` | Người dùng chọn; Showroom bán lẻ hoặc TT bảo hành |
| **Địa chỉ chi tiết** | `address` | Số nhà, tên đường, phường xã | Small Text / TEXT | Yes | No | Văn bản địa chỉ đầy đủ | Người dùng nhập |
| **Tỉnh / Thành phố** | `province_city` | Tỉnh thành trực thuộc | Select / VARCHAR(100) | Yes | No | `TP. Hồ Chí Minh`, `Hà Nội`... | Người dùng chọn từ danh mục tỉnh thành |
| **Hotline chi nhánh**| `hotline` | Số điện thoại liên hệ cửa hàng | Data / VARCHAR(20) | No | No | Đầu số cố định hoặc di động | Regex số điện thoại hợp lệ |
| **Quản lý chi nhánh**| `manager_user` | Nhân sự chịu trách nhiệm Shop | Link / VARCHAR(140) | No | No | Link tới `User` (Role Store Manager) | Chọn từ danh sách User nội bộ |
| **Đang hoạt động** | `is_active` | Trạng thái mở cửa hoạt động | Check / INT(1) | Yes | No | `1` (Active), `0` (Closed) | Mặc định: `1` |

---

### 4. Bảng Nhu cầu Tư vấn / Cơ hội (`Lead_Opportunity` / `tabLead`)
Quản lý nhu cầu tư vấn khách hàng: điện thoại (iPhone 15 Pro Max), laptop (Lenovo LOQ 83GS001RVN), phụ kiện (Sạc GaN), đặt trước (Pre-order) và lịch chăm sóc 3 ngày.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã cơ hội tư vấn**| `name` / `lead_id` | Mã định danh cơ hội | Data / VARCHAR(140) | Yes | Yes | Format: `LEAD-.YYYY.-.#####` | Hệ thống tự sinh tự động tăng |
| **Họ tên khách** | `lead_name` | Tên khách để lại thông tin | Data / VARCHAR(140) | Yes | No | Tối đa 140 ký tự | Khách đăng ký Web Landing / Agent nhập |
| **Số điện thoại** | `mobile_no` | SĐT nhận cuộc gọi tư vấn | Data / VARCHAR(20) | Yes | No | Chuỗi 10 số (Regex VN) | Bắt buộc; Validate 10 chữ số |
| **Email** | `email_id` | Thư điện tử nhận báo giá cọc | Data / VARCHAR(140) | No | No | Email RFC 5322 hợp lệ | Web Landing Page / Form tư vấn |
| **Kênh tiếp nhận** | `source_channel` | Nguồn phát sinh nhu cầu | Select / VARCHAR(50) | Yes | No | `Website_Preorder`, `Facebook_Ads`, `Zalo`, `Hotline`, `Direct_Store` | Web API / Agent chọn |
| **Sản phẩm quan tâm**| `interested_item` | Thiết bị khách muốn mua | Link / VARCHAR(140) | Yes | No | Link tới `Item_Reference` | Chọn từ danh mục SKU (iPhone 15 PM, LOQ...) |
| **Ngân sách dự kiến**| `budget` | Khoảng tiền khách dự định chi | Currency / DECIMAL(18,2)| No | No | Số dương $\ge 0$ (VND) | Người dùng nhập sau phỏng vấn tư vấn |
| **Thời điểm dự kiến mua**| `expected_buy_date`| Ngày dự định mua máy | Date / DATE | No | No | Ngày trong tương lai | Người dùng chọn |
| **Thiết bị đang dùng**| `device_in_use` | Dòng máy khách đang sử dụng | Data / VARCHAR(140) | No | No | Tối đa 140 ký tự | Ghi nhận máy hiện tại để tư vấn lên đời |
| **Nhu cầu thu cũ** | `trade_in_demand` | Khách có muốn bán máy cũ | Select / VARCHAR(20) | Yes | No | `Khong`, `Co_Nhu_Cau` | Ghi nhận nhu cầu (định giá ngoài CRM) |
| **Cấu hình mong muốn**| `desired_specs` | RAM, SSD, Màu sắc, Dung lượng| Data / VARCHAR(255) | No | No | Tối đa 255 ký tự | Ghi nhận cấu hình/nhu cầu phần mềm |
| **Cửa hàng tư vấn** | `preferred_branch` | Cửa hàng khách ghé trải nghiệm | Link / VARCHAR(140) | No | No | Link tới `Branch_Store` | Phân bổ cơ hội theo chi nhánh |
| **Loại nhu cầu** | `lead_type` | Phân loại mục đích của khách | Select / VARCHAR(50) | Yes | No | `Pre_Order`, `Tra_Gop`, `Tu_Van_Ky_Thuat` | Mặc định: `Tu_Van_Ky_Thuat` |
| **Trạng thái cơ hội** | `status` | Tiến trình tư vấn | Select / VARCHAR(50) | Yes | No | `Open`, `Contacted`, `Qualified`, `Converted`, `Lost` | Đồng bộ 100% với State Diagram của Nhật |
| **Lịch nhắc chăm sóc**| `follow_up_due` | Hạn nhắc việc gọi lại | Datetime / DATETIME | No | No | $\text{creation} + 3\text{ ngày làm việc}$ | Tự động đặt lịch nhắc việc 3 ngày |
| **Nhân viên phụ trách**| `lead_owner` | Nhân viên tư vấn được phân công | Link / VARCHAR(140) | No | No | Link tới `User` | Phân công theo cửa hàng hoặc điều chuyển |
| **Khách sau chuyển** | `converted_customer`| Khách hàng tạo sau khi chốt mua | Link / VARCHAR(140) | No | No | Link tới `Customer` (Nullable) | Bắt buộc có giá trị khi `status = 'Converted'` |
| **Ghi chú tư vấn** | `notes` | Lưu ý chi tiết trao đổi | Text / TEXT | No | No | Văn bản ghi chú tự do | Nhân viên cập nhật sau mỗi lần liên hệ |

---

### 5. Bảng Tương tác Khách hàng (`Customer_Interaction` / `tabCustomer Interaction`)
Ghi nhận nhật ký tiếp xúc đa kênh qua Hotline 1800, Zalo OA, Fanpage, Web Chat hoặc tại Cửa hàng (thời gian, kênh, người thực hiện, kết quả).

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã tương tác** | `name` / `interaction_id`| Mã định danh tương tác | Data / VARCHAR(140) | Yes | Yes | Format: `INT-.YYYY.-.#####` | Hệ thống tự sinh |
| **Khách hàng** | `customer` | Khách hàng liên quan | Link / VARCHAR(140) | No | No | Link tới `Customer` (Nullable) | Nullable (Hỗ trợ Khách vãng lai) |
| **SĐT người liên hệ**| `contact_phone` | Số điện thoại gọi đến / chat | Data / VARCHAR(20) | Yes | No | Chuỗi 10 số (Regex VN) | Tự động lấy từ CTI Tổng đài hoặc nhập |
| **Tên người liên hệ**| `contact_name` | Tên người gọi / chat | Data / VARCHAR(140) | No | No | Tối đa 140 ký tự | Lấy từ hồ sơ khách hoặc khách xưng tên |
| **Kênh tiếp xúc** | `channel` | Kênh diễn ra trao đổi | Select / VARCHAR(50) | Yes | No | `Hotline_1800`, `Zalo_OA`, `Fanpage`, `Showroom_Store`, `Website_Chat` | Tự động nhận diện từ Webhook / Chọn |
| **Cửa hàng tiếp nhận**| `branch` | Cửa hàng diễn ra giao tiếp | Link / VARCHAR(140) | No | No | Link tới `Branch_Store` | Nullable nếu gọi lên tổng đài trung tâm |
| **Phân loại giao dịch**| `interaction_type` | Mục đích của khách hàng | Select / VARCHAR(50) | Yes | No | `Tu_van_mua_hang`, `Tra_cuu_don_hang`, `Bao_hanh_sua_chua`, `Khieu_nai_dich_vu` | Nhân viên phân loại |
| **Tóm tắt nội dung** | `summary` | Biên bản tóm tắt trao đổi | Small Text / TEXT | Yes | No | Văn bản mô tả tối đa 1000 ký tự | Nhân viên nhập sau cuộc tiếp xúc |
| **Kết quả tương tác** | `interaction_result` | Kết quả cuộc liên hệ | Select / VARCHAR(50) | Yes | No | `Thanh_cong`, `Hen_goi_lai`, `Khong_nghe_may` | Nhân viên chọn |
| **Nhân viên tiếp nhận**| `staff_agent` | Nhân viên trực tiếp xử lý | Link / VARCHAR(140) | Yes | No | Link tới `User` | Mặc định lấy User đang đăng nhập |
| **Thời điểm tương tác**| `interaction_time` | Giờ phát sinh cuộc gọi/chat | Datetime / DATETIME | Yes | No | Timestamp | Hệ thống tự ghi |
| **Ticket phát sinh** | `escalated_ticket` | Mã phiếu hỗ trợ nếu có khiếu nại| Link / VARCHAR(140) | No | No | Link tới `Support_Ticket` (Nullable) | Tự động điền khi bấm nút "Tạo nhanh Ticket" |

---

### 6. Bảng Phiếu Hỗ Trợ (`Support_Ticket` / `tabSupport Ticket`)
Quản lý vòng đời tiếp nhận, xác minh, xử lý bảo hành, đổi trả theo chính sách và theo dõi tiến độ SLA.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã phiếu hỗ trợ** | `name` / `ticket_id` | Mã định danh Ticket | Data / VARCHAR(140) | Yes | Yes | Format: `TCK-.YYYY.-.#####` | Hệ thống tự sinh tự động tăng |
| **Khách hàng** | `customer` | Khách hàng mở yêu cầu | Link / VARCHAR(140) | No | No | Link tới `Customer` (Nullable) | Để trống nếu vãng lai hoặc trỏ `CUST-GUEST` |
| **SĐT liên hệ** | `contact_phone` | Số nhận thông báo xử lý | Data / VARCHAR(20) | Yes | No | Chuỗi 10 số (Regex VN) | Bắt buộc; Validate 10 chữ số |
| **Tên người liên hệ** | `contact_name` | Tên người nhận kết quả | Data / VARCHAR(140) | Yes | No | Tối đa 140 ký tự | Lấy từ KH hoặc người mang máy tới quầy |
| **Kênh tiếp nhận** | `channel` | Nguồn mở yêu cầu | Select / VARCHAR(50) | Yes | No | `Hotline`, `Showroom`, `Zalo_OA`, `Website` | Mặc định: `Showroom` |
| **Cửa hàng tiếp nhận**| `branch` | Cửa hàng nhận máy ban đầu | Link / VARCHAR(140) | Yes | No | Link tới `Branch_Store` | Bắt buộc chọn cửa hàng tiếp nhận |
| **Mã sản phẩm tiếp nhận**| `item_code` | SKU thiết bị bảo hành/đổi trả | Link / VARCHAR(140) | Yes | No | Link tới `Item_Reference` | Chọn từ danh mục SKU sản phẩm |
| **Số Serial / IMEI** | `serial_imei` | Mã định danh phần cứng máy | Data / VARCHAR(50) | Yes | No | 15 số (IMEI GSMA) hoặc Serial chuẩn | Regex 15 số; Kiểm tra thuật toán Luhn |
| **Đơn mua tham chiếu**| `sales_invoice` | Đơn hàng mua cũ đối soát | Link / VARCHAR(140) | No | No | Link tới `Sales_Invoice_Reference` | Dùng đối soát thời hạn chính sách đổi/bảo hành |
| **Loại yêu cầu** | `issue_type` | Phân loại yêu cầu hậu mãi | Select / VARCHAR(50) | Yes | No | `Tiep_nhan_bao_hanh`, `Doi_tra_theo_chinh_sach`, `Khieu_nai_dich_vu` | Theo chính sách từng dòng sản phẩm |
| **Mức độ ưu tiên** | `priority` | Mức độ khẩn cấp xử lý | Select / VARCHAR(20) | Yes | No | `Low`, `Medium`, `High`, `Critical` | Tự động nâng `Critical` nếu khách là S-VIP |
| **Trạng thái phiếu** | `status` | 5 Trạng thái vòng đời cốt lõi | Select / VARCHAR(50) | Yes | No | `Open`, `In_Progress`, `Pending_Vendor`, `Resolved`, `Closed` | Điều khiển bởi Workflow Engine 5 trạng thái |
| **Nhân viên phụ trách**| `allocated_to` | Nhân viên xử lý tiến độ | Link / VARCHAR(140) | No | No | Link tới `User` | Phân công hoặc điều chuyển theo quyền |
| **Đơn vị phối hợp** | `assigned_dept` | Bộ phận chịu trách nhiệm | Select / VARCHAR(50) | Yes | No | `CSKH_Cua_Hang`, `Trung_Tam_Bao_Hanh`, `Doi_Tac_Hang` | Mặc định: `CSKH_Cua_Hang` |
| **Hạn chót SLA** | `sla_deadline` | Thời hạn tối đa giải quyết | Datetime / DATETIME | Yes | No | Ngày giờ tương lai $> \text{creation}$ | Tự động tính: S-VIP (4h), Thường (24h) |
| **Thời điểm giải quyết**| `resolved_time` | Lúc hoàn tất phương án | Datetime / DATETIME | No | No | Timestamp | Ghi tự động khi chuyển `Resolved` |
| **Thời điểm đóng phiếu**| `closed_time` | Lúc bàn giao và hoàn tất | Datetime / DATETIME | No | No | $\text{closed\_time} \ge \text{resolved\_time}$ | Ghi tự động khi chuyển `Closed` |
| **Kết quả xử lý** | `resolution_result` | Kết luận phương án giải quyết | Select / VARCHAR(50) | No | No | `Bao_hanh_chinh_hang`, `Doi_theo_chinh_sach`, `Sua_chua_co_phi`, `Khong_du_dieu_kien`, `Khach_rut_yeu_cau` | Bắt buộc nhập khi trạng thái là `Resolved` |
| **Báo cáo nguyên nhân** | `root_cause` | Chẩn đoán nguyên nhân kỹ thuật | Small Text / TEXT | No | No | Văn bản giải trình kỹ thuật | Bắt buộc nhập khi chuyển `Resolved` |
| **Ghi chú bàn giao** | `resolution_notes` | Nội dung bàn giao kết quả | Small Text / TEXT | No | No | Văn bản bàn giao giao nhận | Bắt buộc nhập khi chuyển `Closed` |
| **Ghi nhận thông báo** | `notify_customer_status`| Trạng thái đã báo cho khách | Select / VARCHAR(50) | Yes | No | `Chua_thong_bao`, `Da_thong_bao_qua_dien_thoai`, `Da_thong_bao_tai_quay` | Mặc định: `Chua_thong_bao` (nhân viên ghi nhận) |
| **Đánh giá CSAT** | `csat_score` | Điểm hài lòng sau đóng phiếu | Select / VARCHAR(20) | No | No | `1_Sao`, `2_Sao`, `3_Sao`, `4_Sao`, `5_Sao` | Ghi nhận phản hồi của khách hàng |

---

### 7. Bảng Đơn Hàng Mua Tham Chiếu (`Sales_Invoice_Reference` / `tabSales Invoice Reference`)
Lưu trữ thông tin tham chiếu lịch sử mua hàng từ tệp CSV nạp vào phục vụ kiểm tra điều kiện bảo hành và đổi trả.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã đơn tham chiếu**| `name` / `invoice_id` | Mã đơn hàng / hóa đơn mua | Data / VARCHAR(140) | Yes | Yes | Format: `INV-.YYYY.-.#####` | Dữ liệu CSV nạp vào CRM |
| **Khách hàng mua** | `customer` | Khách hàng đứng tên mua | Link / VARCHAR(140) | Yes | No | Link tới `Customer` | Dữ liệu CSV nạp vào |
| **Cửa hàng xuất bán**| `branch` | Cửa hàng xuất bán | Link / VARCHAR(140) | Yes | No | Link tới `Branch_Store` | Dữ liệu CSV nạp vào |
| **Mã sản phẩm** | `item_code` | SKU thiết bị đã bán | Link / VARCHAR(140) | Yes | No | Link tới `Item_Reference` | Dữ liệu CSV nạp vào |
| **Số Serial / IMEI** | `serial_imei` | Serial / IMEI thiết bị xuất bán | Data / VARCHAR(50) | No | No | 15 số (IMEI) hoặc Serial chuẩn | Dữ liệu CSV nạp vào (nếu có) |
| **Ngày mua / nhận hàng**| `purchase_date` | Thời điểm giao dịch mua | Datetime / DATETIME | Yes | No | Timestamp quá khứ $\le \text{today}$ | Dữ liệu CSV nạp vào |
| **Tổng tiền thanh toán**| `grand_total` | Giá trị thực trả | Currency / DECIMAL(18,2) | Yes | No | Số dương $> 0$ (VND) | Dữ liệu CSV nạp vào |
| **Hạn bảo hành theo mã**| `warranty_expiry_date`| Ngày hết hạn bảo hành | Date / DATE | Yes | No | $\text{purchase\_date} + \text{warranty\_months}$ | Tính theo chính sách từng mã hàng |
| **Trạng thái đơn** | `invoice_status` | Tình trạng hiệu lực đơn | Select / VARCHAR(50) | Yes | No | `Paid`, `Exchanged`, `Cancelled` | Mặc định: `Paid` |

---

### 8. Bảng Chi Tiết Ngoại Quan & Phụ Kiện Tiếp Nhận (`Ticket_Item_Condition` - Child Table)
Bảng con nhúng trong `Support_Ticket` để ghi nhận tình trạng máy lúc tiếp nhận (tránh tự cam kết đổi máy nguyên seal khi chưa thẩm định).

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã dòng chi tiết** | `name` | ID dòng con | Data / VARCHAR(140) | Yes | Yes | Frappe Hash ID tự sinh | Hệ thống tự sinh |
| **Phiếu hỗ trợ cha** | `parent` | Liên kết phiếu cha | Data / VARCHAR(140) | Yes | No | Link tới `Support_Ticket` | Hệ thống tự động gán Cascade |
| **Hiện tượng lỗi mô tả**| `reported_issue` | Mô tả lỗi khách phản ánh | Small Text / TEXT | Yes | No | Văn bản mô tả hiện tượng | Nhân viên tiếp nhận nhập |
| **Hiện trạng ngoại quan**| `physical_condition` | Tình trạng thân vỏ máy | Select / VARCHAR(50) | Yes | No | `May_dep_nhu_moi`, `Tray_xuoc_nhe`, `Can_mop_goc`, `Nut_kinh_lung` | Nhân viên tiếp nhận kiểm tra |
| **Phụ kiện giữ lại** | `accessories_included` | Đồ đính kèm khách bàn giao | Data / VARCHAR(255) | No | No | Chuỗi mô tả (VD: Củ sạc, Cáp, Hộp) | Nhân viên ghi nhận lúc nhận máy |
| **Điều kiện tiếp nhận** | `warranty_eligibility` | Đánh giá sơ bộ ban đầu | Select / VARCHAR(50) | Yes | No | `Du_dieu_kien_kiem_dinh`, `Nghi_ngo_roi_nuoc`, `Can_chuyen_hang_kiem_tra`| Nhân viên chọn theo chính sách |

---

### 9. Bảng Nhật Ký Xử Lý Phiếu (`Ticket_Activity_Log` - Child Table)
Bảng con lưu vết toàn bộ trao đổi nội bộ, ghi chú kỹ thuật, chuyển trạng thái trước/sau, bước xác minh, chờ khách và ghi nhận thông báo.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã nhật ký** | `name` | ID dòng nhật ký | Data / VARCHAR(140) | Yes | Yes | Frappe Hash ID tự sinh | Hệ thống tự sinh |
| **Phiếu hỗ trợ cha** | `parent` | Liên kết phiếu cha | Data / VARCHAR(140) | Yes | No | Link tới `Support_Ticket` | Hệ thống tự động gán Cascade |
| **Người thực hiện** | `staff_user` | Nhân sự tác động bản ghi | Link / VARCHAR(140) | Yes | No | Link tới `User` | Hệ thống lấy User đang thao tác |
| **Loại hành động** | `action_type` | Bản chất thao tác | Select / VARCHAR(50) | Yes | No | `Cap_nhat_tien_do`, `Chuyen_trang_thai`, `Xac_minh_dieu_kien`, `Cho_phan_hoi_khach`, `Cho_don_vi_bao_hanh`, `Ghi_nhan_thong_bao` | Nhân sự chọn / Tự động |
| **Trạng thái trước** | `from_status` | Trạng thái cũ | Data / VARCHAR(50) | No | No | Trạng thái trước khi đổi | Hệ thống tự điền |
| **Trạng thái sau** | `to_status` | Trạng thái mới | Data / VARCHAR(50) | No | No | Trạng thái sau khi đổi | Hệ thống tự điền |
| **Ghi chú tiến độ** | `progress_notes` | Nội dung chi tiết | Small Text / TEXT | Yes | No | Văn bản ghi chú chi tiết | Nhân sự nhập |
| **Thời điểm ghi nhận**| `logged_at` | Giờ thao tác | Datetime / DATETIME | Yes | No | Timestamp | Hệ thống tự ghi |

---

### 10. Bảng Sản Phẩm Tham Chiếu (`Item_Reference` / `tabItem`)
Danh mục sản phẩm tham chiếu 3 ngành hàng (iPhone 15 Pro Max, Lenovo LOQ 15IAX9 83GS001RVN, Củ sạc GaN) kèm chính sách bảo hành đúng theo từng mã hàng.

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Mã sản phẩm (SKU)** | `item_code` | Mã SKU sản phẩm chính xác | Data / VARCHAR(140) | Yes | Yes | `SP-IP15PM`, `SP-LOQ-83GS001RVN`, `SP-GAN-65W` | Master Data đồng bộ |
| **Tên sản phẩm** | `item_name` | Tên thương mại sản phẩm | Data / VARCHAR(255) | Yes | No | Tối đa 255 ký tự (Lenovo LOQ 15IAX9 83GS001RVN...) | Master Data đồng bộ |
| **Thương hiệu** | `brand` | Hãng sản xuất | Select / VARCHAR(100) | Yes | No | `Apple`, `Lenovo`, `Anker`, `Samsung`, `Asus` | Master Data |
| **Nhóm ngành hàng** | `item_group` | Phân loại thiết bị | Select / VARCHAR(100) | Yes | No | `Điện thoại`, `Laptop`, `Phụ kiện` | Master Data (3 ngành hàng nghiên cứu) |
| **Thời gian bảo hành** | `standard_warranty_months`| Số tháng bảo hành chính hãng | Int / INT | Yes | No | Giá trị: $12, 24$ (LOQ: 24 tháng theo S06) | Master Data theo từng SKU |
| **Ghi chú gói dịch vụ**| `warranty_policy_note`| Phân định AppleCare+/Gói mở rộng| Small Text / TEXT | No | No | Phân định AppleCare+ và CareS là riêng biệt | Master Data ghi chú chính sách |
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

### 12. Bảng Người Dùng Hệ Thống (`Staff_User` / DocType `User` / `tabUser`)
Đại diện cho 5 vai trò nhân sự nội bộ (Bán hàng & CSKH cửa hàng, Quản lý cửa hàng, CSKH chuỗi, Quản lý chuỗi, Quản trị hệ thống) và phân quyền chi nhánh (User Permissions).

| Tên logic | Tên vật lý (Fieldname) | Ý nghĩa nghiệp vụ | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc | Duy nhất | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Tên đăng nhập / Email** | `name` / `email` | Mã định danh duy nhất / Email | Data / VARCHAR(140) | Yes | Yes | Format email: `user@cellphones.com.vn` | Quản trị viên khởi tạo; Bắt buộc duy nhất |
| **Họ và tên nhân viên** | `full_name` / `first_name` | Tên đầy đủ của nhân sự | Data / VARCHAR(140) | Yes | No | Chuỗi ký tự, tối đa 140 ký tự | Quản trị viên nhập |
| **Số điện thoại nội bộ** | `phone` / `mobile_no` | SĐT liên hệ công việc | Data / VARCHAR(20) | No | No | Chuỗi 10 số (Regex VN) | Quản trị viên nhập |
| **Vai trò người dùng** | `role_profile_name` | 5 Nhóm vai trò chuẩn theo BA | Select / VARCHAR(100) | Yes | No | `Ban_hang_CSKH_cua_hang`, `Quan_ly_cua_hang`, `CSKH_chuoi`, `Quan_ly_chuoi`, `Quan_tri_he_thong` | Gán Role Profile chuẩn hóa |
| **Chi nhánh trực thuộc** | `default_branch` | Cửa hàng nhân viên công tác | Link / VARCHAR(140) | No | No | Link tới `Branch_Store` | Căn cứ thiết lập User Permission phân vùng dữ liệu |
| **Phân loại người dùng** | `user_type` | Loại tài khoản hệ thống | Select / VARCHAR(50) | Yes | No | `System User` | Mặc định: `System User` |
| **Đang hoạt động** | `enabled` | Trạng thái tài khoản | Check / INT(1) | Yes | No | `1` (Active), `0` (Disabled) | Mặc định: `1` |

---

## PHẦN 2: CÁC QUY TẮC KIỂM TRA TÍNH HỢP LỆ CHI TIẾT (VALIDATION RULES)

### 1. Quy tắc Định dạng Số điện thoại Việt Nam (Regex Validation)
* **Trường áp dụng:** `Customer.phone_number`, `Lead.mobile_no`, `Support_Ticket.contact_phone`, `Customer_Interaction.contact_phone`.
* **Biểu thức chính quy (Regex):** `^(0[3|5|7|8|9])[0-9]{8}$`
* **Diễn giải:** Bắt đầu bằng chữ số `0`, theo sau bởi một trong các chữ số mạng viễn thông Việt Nam (`3`, `5`, `7`, `8`, `9`), và đúng 8 chữ số tiếp theo. Tổng chiều dài đúng 10 ký tự số.
* **Cảnh báo trùng số:** Khi trùng số điện thoại trong hệ thống, CRM tạo cảnh báo để nhân viên kiểm tra xác minh, tuyệt đối **không tự động gộp hồ sơ**.
* **Thông báo lỗi khi vi phạm:** *"Số điện thoại không hợp lệ! Vui lòng nhập đúng 10 chữ số thuộc các đầu mạng Việt Nam (03x, 05x, 07x, 08x, 09x)."*

### 2. Quy tắc Định dạng Số Serial / IMEI thiết bị di động
* **Trường áp dụng:** `Support_Ticket.serial_imei`, `Sales_Invoice_Reference.serial_imei`.
* **Biểu thức Regex:** `^[0-9]{15}$` (Đúng 15 chữ số theo chuẩn GSMA) hoặc chuỗi Serial theo chuẩn nhà sản xuất.
* **Kiểm tra thuật toán Luhn (Check Digit):** Chữ số thứ 15 được xác thực bằng thuật toán Modulo 10 của GSMA để loại bỏ số IMEI giả mạo.

### 3. Quy tắc Ràng buộc Logic Thời gian & Trạng thái Ticket
* **Quy tắc thời gian đóng phiếu:** $\text{closed\_time} \ge \text{creation}$ và $\text{closed\_time} \ge \text{resolved\_time}$.
* **Quy tắc giải trình bắt buộc khi hoàn tất:** Khi chuyển `status` sang `Resolved`, 2 trường `resolution_result` và `root_cause` **bắt buộc không được để trống**. Không tự xác nhận sửa xong khi hết thời gian dự kiến.
* **Quy tắc nghiệm thu đóng phiếu:** Khi chuyển `status` sang `Closed`, trường `resolution_notes` **bắt buộc không được để trống**.
* **Quy tắc ghi nhận thông báo:** Trong MVP, nhân viên cập nhật `notify_customer_status = 'Da_thong_bao_qua_dien_thoai'` khi đã liên hệ báo khách.
* **Quy tắc tính toán SLA theo hạng Smember:**
  * *Hạng S-VIP:* Cam kết xử lý trong vòng **4 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 4\text{h}$).
  * *Hạng S-MEM / S-NEW / S-NULL:* Cam kết xử lý trong vòng **24 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 24\text{h}$).
  * *Khách vãng lai:* Cam kết xử lý trong vòng **48 giờ làm việc** ($\text{sla\_deadline} = \text{creation} + 48\text{h}$).

### 4. Quy tắc Ràng buộc Chuyển đổi Cơ hội (Lead Conversion Validation)
* Khi `Lead_Opportunity.status` chuyển thành `'Converted'`:
  * Trường `converted_customer` **bắt buộc phải có giá trị**.
  * Khách hàng được liên kết phải có `phone_number` trùng khớp với `Lead.mobile_no`.

### 5. Quy tắc Tính Báo cáo Tỷ lệ Thắng & Nhắc việc (Mục 5.1 Tài liệu BA)
* **Công thức Tỷ lệ Thắng cơ hội:**
  $$\text{Tỷ lệ thắng} = \frac{\text{Số cơ hội Thắng (Converted)}}{\text{Thắng (Converted)} + \text{Thua (Lost)}} \times 100\%$$
  * *Điều kiện lọc:* Chỉ tính các cơ hội đã đóng trong kỳ (`Converted` hoặc `Lost`), lọc theo ngày đóng và gán cho nhân viên tại lúc đóng.
  * *Xử lý mẫu số = 0:* Khi chưa phát sinh cơ hội đóng, hệ thống hiển thị: **"Chưa có dữ liệu"**.
* **Quy tắc Nhắc việc chăm sóc (Task Reminder):**
  * Tự động tính hạn nhắc việc $\text{follow\_up\_due} = \text{creation} + 3\text{ ngày làm việc}$ đối với các cơ hội đang mở (`Open`, `Contacted`).

---

## PHẦN 3: BẢNG ĐỒNG BỘ TRẠNG THÁI VỚI STATE DIAGRAM CỦA NHẬT (C03 ALIGNMENT)

### 1. Đồng bộ Trạng thái Phiếu Hỗ Trợ (`Support_Ticket.status`)

| Trạng thái trong Từ điển CRM | Tên tiếng Việt hiển thị UI | Trạng thái State Diagram C03 (Nhật) | Ý nghĩa nghiệp vụ chi tiết tại CellphoneS |
| :--- | :--- | :--- | :--- |
| `Open` | **Mới tạo** | `State: Open (Initial)` | Phiếu hỗ trợ vừa tiếp nhận từ Hotline/Showroom; lưu bước tiếp nhận ban đầu. |
| `In_Progress` | **Đang xử lý** | `State: In Progress` | Nhân viên CSKH cửa hàng hoặc kỹ thuật viên đang trực tiếp xác minh/kiểm tra máy. |
| `Pending_Vendor` | **Chờ Hãng / Đối tác** | `State: Pending Vendor` | Thiết bị đã gửi sang đơn vị bảo hành (Apple Care, Samsung, CareS) hoặc chờ linh kiện. |
| `Resolved` | **Đã giải quyết** | `State: Resolved` | Đã có phương án/kết quả giải quyết xác nhận do nhân viên nhập; sẵn sàng bàn giao. |
| `Closed` | **Đóng phiếu** | `State: Closed (Final)` | Khách đã nhận bàn giao thiết bị, nhân viên ghi nhận đã thông báo và đóng phiếu. |

*(Lưu ý: Các bước "Đang xác minh" và "Chờ khách" được ghi nhận linh hoạt thông qua trường `action_type` trong bảng con `Ticket_Activity_Log`).*

### 2. Đồng bộ Trạng thái Cơ hội Tư vấn / Pre-order (`Lead_Opportunity.status`)

| Trạng thái trong Từ điển CRM | Tên tiếng Việt hiển thị UI | Trạng thái State Diagram C03 (Nhật) | Ý nghĩa nghiệp vụ chi tiết tại CellphoneS |
| :--- | :--- | :--- | :--- |
| `Open` | **Mới tiếp nhận** | `Lead: Open (New)` | Khách vừa đăng ký để lại thông tin đặt trước hoặc cần tư vấn qua Web/Hotline. |
| `Contacted` | **Đã liên hệ** | `Lead: Contacted` | Nhân viên tư vấn đã gọi điện hỏi nhu cầu, ngân sách, thiết bị đang dùng. |
| `Qualified` | **Đủ điều kiện** | `Lead: Qualified` | Khách xác nhận chốt phiên bản/cấu hình/màu sắc mong muốn. |
| `Converted` | **Đã chốt mua / Cọc** | `Lead: Converted (Success)` | Khách đã mua hàng/chốt cọc thành công, tạo đơn và hồ sơ `Customer`. |
| `Lost` | **Hủy / Không mua** | `Lead: Lost (Closed)` | Khách từ chối mua, đổi ý hoặc không liên lạc được sau thời gian chăm sóc. |

---

## PHẦN 4: QUY ĐỊNH DỮ LIỆU MẪU (SAMPLE DATA SPECIFICATION)

### 1. Dữ liệu Mẫu Bảng Khách hàng & Smember Profile (4 Hạng chuẩn & Nhóm Giáo dục)

| `customer_id` | `phone_number` | `customer_name` | `email_id` | `identity_card` | `province_city` | `status` | `member_tier` | `edu_type` | `edu_status` | `edu_expiry_date` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `CUST-2026-00001` | `0908123456` | Nguyễn Văn An | an.nguyen@gmail.com | 079098001234 | TP. Hồ Chí Minh | Active | **S-VIP** | `None` | `None` | — |
| `CUST-2026-00002` | `0912345678` | Lê Thị Bích Trâm | tram.le@yahoo.com | 079195005678 | TP. Hồ Chí Minh | Active | **S-MEM** | `S-Student` | `Da_xac_minh` | 2027-06-30 |
| `CUST-2026-00003` | `0987654321` | Hoàng Minh Quân | quan.hoang@fpt.com | 001099004321 | Hà Nội | Active | **S-NEW** | `S-Teacher` | `Da_xac_minh` | 2027-10-31 |
| `CUST-2026-00004` | `0933445566` | Phạm Thu Trang | trang.pham@gmail.com| 079201009876 | TP. Hồ Chí Minh | Active | **S-NULL** | `None` | `Chua_xac_minh` | — |
| `CUST-GUEST` | `0000000000` | Khách vãng lai | guest@cellphones.com.vn| — | TP. Hồ Chí Minh | Active | **S-NULL** | `None` | `None` | — |

### 2. Dữ liệu Mẫu Bảng Chi nhánh (`Branch_Store` - Mô phỏng 2 Showroom)

| `branch_id` | `branch_name` | `branch_type` | `address` | `province_city` | `hotline` | `manager_user` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `BR-00101` | Showroom 125 Lê Văn Việt | `Showroom_Store` | 125 Lê Văn Việt, P. Hiệp Phú, TP. Thủ Đức | TP. Hồ Chí Minh | 02871088125 | `thuan.cskh@cellphones.com.vn` |
| `BR-00102` | Showroom 213 Trần Quang Khải | `Showroom_Store` | 213 Trần Quang Khải, P. Tân Định, Quận 1 | TP. Hồ Chí Minh | 02871088213 | `linh.manager@cellphones.com.vn` |
| `BR-00201` | Trung tâm Tiếp nhận Bảo hành Q9 | `Dien_Thoai_Vui_Center` | 129 Lê Văn Việt, P. Hiệp Phú, TP. Thủ Đức | TP. Hồ Chí Minh | 02871010129 | `tu.dtv@cellphones.com.vn` |

### 3. Dữ liệu Mẫu Bảng Phiếu Hỗ Trợ (`Support_Ticket`)

| `ticket_id` | `customer` | `contact_phone` | `contact_name` | `item_code` | `serial_imei` | `issue_type` | `priority` | `status` | `resolution_result` | `notify_customer_status` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `TCK-2026-00155` | `CUST-2026-00001` | `0908123456` | Nguyễn Văn An | `SP-IP15PM-256` | `358941098234112` | `Doi_tra_theo_chinh_sach`| Critical | `In_Progress` | — | `Chua_thong_bao` |
| `TCK-2026-00150` | `CUST-2026-00002` | `0912345678` | Lê Thị Bích Trâm| `SP-LOQ-83GS001RVN` | `83GS001RVNVN101` | `Tiep_nhan_bao_hanh` | High | `In_Progress` | — | `Chua_thong_bao` |
| `TCK-2026-00142` | `CUST-2026-00003` | `0987654321` | Hoàng Minh Quân | `SP-GAN-65W` | `AN2024GAN65001` | `Doi_tra_theo_chinh_sach`| Medium | `Resolved` | `Doi_theo_chinh_sach` | `Da_thong_bao_qua_dien_thoai` |

### 4. Dữ liệu Mẫu Bảng Người Dùng Hệ Thống (`Staff_User` - 5 Vai trò chuẩn)

| `email` (`name`) | `full_name` | `phone` | `role_profile_name` | `default_branch` | `enabled` |
| :--- | :--- | :--- | :--- | :--- | :---: |
| `admin@cellphones.com.vn` | Trần Quang Huy | `0909000001` | `Quan_tri_he_thong` (System Administrator) | `BR-00101` | 1 |
| `linh.manager@cellphones.com.vn` | Đỗ Mỹ Linh | `0909000002` | `Quan_ly_cua_hang` (Store Manager) | `BR-00102` | 1 |
| `thuan.cskh@cellphones.com.vn` | Nguyễn Văn Thuận | `0909000003` | `Ban_hang_CSKH_cua_hang` (Store Sales & CSKH) | `BR-00101` | 1 |
| `my.chain@cellphones.com.vn` | Bùi Trà My | `0909000004` | `CSKH_chuoi` (Chain CSKH Agent) | `BR-00101` | 1 |
| `phuc.head@cellphones.com.vn` | Lê Hoàng Phúc | `0909000005` | `Quan_ly_chuoi` (Chain Manager / Head CSKH)| `BR-00101` | 1 |

---

## PHẦN 5: QUY ĐỊNH CỘT CSV CẦN NHẬP (CSV IMPORT SPECIFICATIONS)

Dưới đây là đặc tả chi tiết danh sách cột trong các file mẫu CSV phục vụ Import nạp dữ liệu ban đầu vào Frappe Framework:

### 1. File CSV Import Khách hàng (`Customer_Data_Import.csv`)
* **Mục đích:** Nạp danh sách khách hàng ban đầu từ hệ thống POS cũ/bộ mẫu thử nghiệm sang CRM Frappe.
* **Quy định cột:**

| Header Cột CSV | Fieldname Frappe tương ứng | Bắt buộc | Kiểu dữ liệu | Ví dụ giá trị | Ghi chú kiểm tra |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `Customer ID` | `name` | Tùy chọn | Text | `CUST-2026-00001` | Mã định danh nội bộ ổn định |
| `Phone Number` | `phone_number` | **BẮT BUỘC** | Text | `0908123456` | Số điện thoại liên hệ & tìm kiếm (10 chữ số) |
| `Full Name` | `customer_name` | **BẮT BUỘC** | Text | `Nguyễn Văn An` | Tên đầy đủ, không để trống |
| `Email` | `email_id` | Tùy chọn | Text | `an.nguyen@gmail.com`| Validate email format |
| `Identity Card` | `identity_card` | Tùy chọn | Text | `079098001234` | 9 hoặc 12 số CCCD |
| `Gender` | `gender` | Tùy chọn | Enum | `Nam` | Chọn: `Nam`, `Nữ`, `Khác` |
| `Province City` | `province_city` | Tùy chọn | Text | `TP. Hồ Chí Minh` | Tên tỉnh thành phố |
| `Customer Type` | `customer_type` | **BẮT BUỘC** | Enum | `Individual` | `Individual`, `Educational`, `Corporate` |
| `Smember Tier` | `custom_smember_tier` | **BẮT BUỘC** | Enum | `S-VIP` | `S-NULL`, `S-NEW`, `S-MEM`, `S-VIP` |
| `Edu Type` | `custom_edu_type` | Tùy chọn | Enum | `S-Student` | `None`, `S-Student`, `S-Teacher` |
| `Edu Status` | `custom_edu_status` | Tùy chọn | Enum | `Da_xac_minh` | `Chua_xac_minh`, `Da_xac_minh`, `Tu_choi` |
| `Edu Expiry Date` | `custom_edu_expiry_date` | Tùy chọn | Date | `2027-06-30` | Thời hạn ưu đãi giáo dục (nếu đã duyệt) |
| `Total Spent` | `custom_total_spent` | Tùy chọn | Number | `85400000` | Số tiền tích lũy tham chiếu (VND) |

---

### 2. File CSV Import Chi nhánh Cửa hàng (`Branch_Store_Import.csv`)
* **Mục đích:** Nạp danh mục toàn bộ mạng lưới 2 Showroom mô phỏng và Trung tâm bảo hành/CareS.
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
* **Mục đích:** Đồng bộ danh mục SKU thiết bị đại diện nghiên cứu sang CRM.
* **Quy định cột:**

| Header Cột CSV | Fieldname Frappe tương ứng | Bắt buộc | Kiểu dữ liệu | Ví dụ giá trị | Ghi chú kiểm tra |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `Item SKU` | `item_code` | **BẮT BUỘC** | Text | `SP-LOQ-83GS001RVN` | Mã SKU sản phẩm duy nhất |
| `Item Name` | `item_name` | **BẮT BUỘC** | Text | `Laptop Lenovo LOQ 15IAX9 83GS001RVN` | Tên thương mại chính xác |
| `Brand` | `brand` | **BẮT BUỘC** | Enum | `Lenovo` | `Apple`, `Lenovo`, `Anker`, `Samsung`... |
| `Item Group` | `item_group` | **BẮT BUỘC** | Enum | `Laptop` | `Điện thoại`, `Laptop`, `Phụ kiện` |
| `Warranty Months`| `standard_warranty_months`| **BẮT BUỘC** | Int | `24` | Số tháng bảo hành chính hãng (24 tháng cho Lenovo LOQ) |
| `Is Active` | `is_active` | **BẮT BUỘC** | Int | `1` | `1` (Mở bán), `0` (Ngừng kinh doanh) |

---

### 4. File CSV Import Nhân sự & Người dùng (`Staff_User_Import.csv`)
* **Mục đích:** Khởi tạo danh sách tài khoản nội bộ và phân quyền 5 nhóm vai trò theo chi nhánh.
* **Quy định cột:**

| Header Cột CSV | Fieldname Frappe tương ứng | Bắt buộc | Kiểu dữ liệu | Ví dụ giá trị | Ghi chú kiểm tra |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `Email / User ID` | `name` / `email` | **BẮT BUỘC** | Text | `thuan.cskh@cellphones.com.vn` | Email đăng nhập duy nhất |
| `Full Name` | `first_name` | **BẮT BUỘC** | Text | `Nguyễn Văn Thuận` | Họ và tên đầy đủ |
| `Phone` | `phone` | Tùy chọn | Text | `0909000003` | Số điện thoại nội bộ |
| `Role Profile` | `role_profile_name` | **BẮT BUỘC** | Enum | `Ban_hang_CSKH_cua_hang` | 5 Vai trò chuẩn theo BA |
| `Default Branch` | `default_branch` | Tùy chọn | Text | `BR-00101` | Mã chi nhánh cửa hàng |
| `Is Active` | `enabled` | **BẮT BUỘC** | Int | `1` | `1` (Hoạt động), `0` (Khóa tài khoản) |
