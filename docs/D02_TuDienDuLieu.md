# SẢN PHẨM BÀN GIAO D02: TỪ ĐIỂN DỮ LIỆU & QUY TẮC KIỂM TRA
## HỆ THỐNG CRM CHUỖI BÁN LẺ CELLPHONES (TRÊN NỀN TẢNG FRAPPE FRAMEWORK)

**Mã sản phẩm:** D02  
**Tên sản phẩm:** Từ điển dữ liệu & Quy tắc kiểm tra (Data Dictionary & Validation Rules)  
**Người thực hiện:** Trần Quang Huy (MSSV: 24520706 – TV4)  
**Vai trò:** Quản trị hệ thống, Phụ trách Mô hình Dữ liệu và Thiết kế Triển khai  
**Nền tảng mục tiêu:** Frappe Framework v15 (Custom App độc lập `cellphones_crm`)  
**Tài liệu cơ sở:**
- `Nền Tảng CRM.md` (Nền tảng phân tích hệ thống CRM CellphoneS)
- `Nhiemvu.md` (Phân công nhiệm vụ và yêu cầu chuẩn đầu ra TV4)
- `docs/D01_ERD_ThuyetMinh.md` (Sơ đồ ERD Logic và quan hệ thực thể)  
**Trạng thái:** Hoàn thiện (Thiết kế đề xuất – Proposed Design)

---

## 1. NGUYÊN TẮC THIẾT KẾ VÀ ĐỒNG BỘ DỮ LIỆU

1. **Chuẩn hóa 6 cột thông tin kỹ thuật:**
   Mỗi trường dữ liệu được mô tả chi tiết qua 6 thuộc tính:
   * **Tên vật lý (Field name):** Tên trường chuẩn hóa dạng `snake_case` dùng trong cơ sở dữ liệu MariaDB và Frappe DocType.
   * **Kiểu dữ liệu (Frappe/SQL):** Kiểu dữ liệu tương thích giữa Frappe FieldType (Data, Select, Link, Check, Currency, Date, Datetime, Small Text, Long Text...) và kiểu SQL tương ứng (VARCHAR, INT, DATETIME, DECIMAL, TINYINT...).
   * **Bắt buộc (Req):** Quy định trường có bắt buộc nhập hay không (Yes/No).
   * **Duy nhất (Unique):** Quy định trường có bắt buộc duy nhất trên toàn hệ thống hay không (Yes/No).
   * **Giá trị hợp lệ / Enum:** Tập giá trị cho phép của trường (danh sách lựa chọn Select, định dạng Regex, khoảng giá trị).
   * **Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule):** Nguồn nhập liệu (Người dùng nhập, Hệ thống tự sinh, Tính toán tự động, CSV Import) và các logic kiểm tra ràng buộc nghiệp vụ (Regex SĐT, so sánh ngày tháng, kiểm tra điều kiện chuyển trạng thái).

2. **Chuẩn hóa bộ trạng thái nghiệp vụ:**
   * **Cơ hội tư vấn (`Opportunity.stage`):** `Mới tiếp nhận` $\rightarrow$ `Đang tư vấn` $\rightarrow$ `Chờ phản hồi` $\rightarrow$ `Thành công` | `Thất bại` | `Hủy`.
   * **Phiếu hỗ trợ (`Support_Ticket.status`):** `Tiếp nhận` $\rightarrow$ `Đang xử lý` $\rightarrow$ `Đã có kết quả` $\rightarrow$ `Đã thông báo khách` $\rightarrow$ `Đóng phiếu`.
   * **Việc chăm sóc (`Care_Task.status`):** `Chưa thực hiện` $\rightarrow$ `Đang thực hiện` $\rightarrow$ `Hoàn thành` | `Quá hạn` | `Hủy`.

---

## 2. BẢNG TỪ ĐIỂN DỮ LIỆU CHI TIẾT TỪNG THỰC THỂ

### 2.1. Thực thể `Branch` (Chi nhánh / Cửa hàng)
* **Mục đích:** Lưu trữ thông tin 2 cửa hàng mô phỏng để quản lý phân quyền và dữ liệu theo chi nhánh.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `branch_id` | Data / VARCHAR(20) | Yes | Yes | Định dạng: `CN01`, `CN02`, `HQ` | Khóa chính (PK). Nhập thủ công khi cấu hình hệ thống. |
| `branch_name` | Data / VARCHAR(140) | Yes | No | Chuỗi văn bản $\le 140$ ký tự | Tên cửa hàng (ví dụ: CellphoneS 136 Nguyễn Thái Học). |
| `address` | Small Text / TEXT | Yes | No | Chuỗi văn bản địa chỉ | Địa chỉ thực tế của cửa hàng. |
| `phone_number` | Data / VARCHAR(20) | Yes | No | Regex: `^(0[3\|5\|7\|8\|9])[0-9]{8}$` | Số điện thoại liên hệ của cửa hàng, đúng chuẩn SĐT VN. |
| `is_active` | Check / TINYINT(1) | Yes | No | `0` (Không hoạt động), `1` (Hoạt động) | Mặc định là `1`. Khi ngừng hoạt động chuyển về `0`. |

---

### 2.2. Thực thể `Staff_User` (Nhân viên / Người dùng hệ thống)
* **Mục đích:** Quản lý tài khoản người dùng, vai trò và phân quyền theo chi nhánh.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `user_id` | Data / VARCHAR(50) | Yes | Yes | Mã nhân viên hoặc Username | Khóa chính (PK). Chuỗi không dấu, không khoảng trắng (VD: `huy.tq`). |
| `full_name` | Data / VARCHAR(140) | Yes | No | Chuỗi văn bản họ tên | Tên hiển thị của nhân viên. |
| `email` | Data / VARCHAR(140) | Yes | Yes | Định dạng Email chuẩn | Email công việc, dùng để đăng nhập và nhận thông báo. |
| `role` | Select / VARCHAR(50) | Yes | No | `Store Sales`, `Store CSKH`, `Store Manager`, `Chain CSKH`, `Chain Manager`, `System Administrator` | Chọn 1 trong 6 vai trò chuẩn hóa của hệ thống. |
| `branch_id` | Link:Branch / VARCHAR(20) | Yes | No | Phải tồn tại trong bảng `Branch` | Khóa ngoại (FK). Dùng thiết lập User Permission giới hạn chi nhánh. |
| `is_active` | Check / TINYINT(1) | Yes | No | `0` (Khóa), `1` (Kích hoạt) | Mặc định là `1`. Khi nhân viên nghỉ việc chuyển về `0`. |

---

### 2.3. Thực thể `Customer` (Hồ sơ khách hàng)
* **Mục đích:** Lưu trữ hồ sơ định danh khách hàng cá nhân B2C, phân nhóm và theo dõi hạng thành viên mẫu.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `customer_id` | Data / VARCHAR(30) | Yes | Yes | Auto-naming: `CUST-.YYYY.-.#####` | Khóa chính (PK). Hệ thống tự sinh mã nội bộ duy nhất cố định. |
| `full_name` | Data / VARCHAR(140) | Yes | No | Chuỗi văn bản $\le 140$ ký tự | Họ tên khách hàng cá nhân. |
| `phone_number` | Data / VARCHAR(20) | Yes | No | Regex: `^(0[3\|5\|7\|8\|9])[0-9]{8}$` | SĐT 10 chữ số chuẩn VN. Kiểm tra cảnh báo trùng nhưng không tự động gộp. |
| `email` | Data / VARCHAR(140) | No | No | Định dạng Email chuẩn | Email liên hệ nhận hóa đơn, thông tin bảo hành. |
| `address` | Small Text / TEXT | No | No | Chuỗi văn bản | Địa chỉ liên hệ của khách hàng. |
| `customer_group` | Select / VARCHAR(50) | Yes | No | `Khách hàng cá nhân`, `S-Student`, `S-Teacher` | Phân loại nhóm khách hàng và đối tượng ưu đãi. Mặc định: `Khách hàng cá nhân`. |
| `membership_tier` | Select / VARCHAR(50) | Yes | No | `S-New`, `S-Mem`, `S-Vip` | Hạng thành viên Smember mẫu. Phiên bản đầu không tự tính điểm, lưu mẫu để tư vấn. |
| `opt_out_care` | Check / TINYINT(1) | Yes | No | `0` (Nhận chăm sóc), `1` (Ngừng nhận) | Mặc định là `0`. Nếu là `1`, hệ thống chặn tạo `Care_Task` mới dạng tiếp thị/tư vấn. |
| `is_merged` | Check / TINYINT(1) | Yes | No | `0` (Hồ sơ gốc), `1` (Đã bị gộp) | Mặc định là `0`. Chuyển thành `1` khi thực hiện gộp vào hồ sơ khác. |
| `merged_into_customer_id` | Link:Customer / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Customer.customer_id` | Chỉ có giá trị khi `is_merged = 1`. Lưu mã hồ sơ chính sau gộp. |
| `branch_id` | Link:Branch / VARCHAR(20) | Yes | No | Khóa ngoại trỏ về `Branch.branch_id` | Chi nhánh tạo hoặc tiếp nhận hồ sơ ban đầu. |
| `created_at` | Datetime / DATETIME | Yes | No | Ngày giờ tạo bản ghi | Mặc định: `NOW()`, hệ thống tự sinh không cho sửa. |

---

### 2.4. Thực thể `Opportunity` (Cơ hội bán hàng & Nhu cầu tư vấn)
* **Mục đích:** Ghi nhận nhu cầu mua sắm cụ thể của khách hàng theo từng nhóm sản phẩm, theo dõi tiến độ tư vấn.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `opportunity_id` | Data / VARCHAR(30) | Yes | Yes | Auto-naming: `OPP-.YYYY.-.#####` | Khóa chính (PK). Hệ thống tự sinh mã duy nhất. |
| `customer_id` | Link:Customer / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Customer.customer_id` | Cho phép Null nếu là khách vãng lai chưa tạo hồ sơ. |
| `temp_customer_name` | Data / VARCHAR(140) | No | No | Chuỗi văn bản | Tên tạm thời của khách khi chưa có `customer_id`. |
| `temp_phone_number` | Data / VARCHAR(20) | No | No | SĐT 10 chữ số | Số điện thoại tạm thời của khách khi chưa có `customer_id`. |
| `product_category` | Select / VARCHAR(50) | Yes | No | `Điện thoại`, `Laptop`, `Phụ kiện` | 3 nhóm sản phẩm trong phạm vi nghiên cứu. |
| `product_name` | Data / VARCHAR(140) | Yes | No | Ví dụ: `iPhone 15 Pro Max`, `Lenovo LOQ 15IAX9`, `Củ sạc GaN 65W` | Tên sản phẩm cụ thể khách hàng đang quan tâm. |
| `expected_budget` | Currency / DECIMAL(18,2) | No | No | Giá trị $\ge 0$ | Ngân sách dự kiến mà khách hàng sẵn sàng chi trả. |
| `usage_purpose` | Small Text / TEXT | No | No | Chuỗi văn bản mô tả | Mục đích sử dụng (học tập, văn phòng, gaming, đồ họa...). |
| `specific_requirement` | Small Text / TEXT | No | No | Chuỗi văn bản mô tả | Yêu cầu riêng: dung lượng, màu sắc, cổng sạc, thu cũ đổi mới... |
| `expected_buy_time` | Select / VARCHAR(50) | No | No | `Mua ngay trong ngày`, `Trong tuần này`, `Trong tháng này`, `Đang tham khảo thêm` | Thời điểm dự kiến mua hàng để xếp thứ tự ưu tiên chăm sóc. |
| `stage` | Select / VARCHAR(50) | Yes | No | `Mới tiếp nhận`, `Đang tư vấn`, `Chờ phản hồi`, `Thành công`, `Thất bại`, `Hủy` | Mặc định là `Mới tiếp nhận`. Quản lý theo Workflow. |
| `lost_reason` | Small Text / TEXT | No | No | Chuỗi văn bản lý do | Bắt buộc nhập khi chuyển `stage` sang `Thất bại` hoặc `Hủy`. |
| `assigned_to_user` | Link:Staff_User / VARCHAR(50) | Yes | No | Khóa ngoại trỏ về `Staff_User.user_id` | Nhân viên chịu trách nhiệm tư vấn và chăm sóc cơ hội này. |
| `branch_id` | Link:Branch / VARCHAR(20) | Yes | No | Khóa ngoại trỏ về `Branch.branch_id` | Chi nhánh phát sinh và quản lý cơ hội. |
| `created_at` | Datetime / DATETIME | Yes | No | Ngày giờ tạo | Mặc định: `NOW()`. |
| `closed_at` | Datetime / DATETIME | No | No | Ngày giờ kết thúc | Tự động ghi nhận khi `stage` chuyển sang `Thành công`, `Thất bại`, `Hủy`. |

---

### 2.5. Thực thể `Interaction` (Lịch sử tương tác)
* **Mục đích:** Ghi nhận nhật ký mỗi lần trao đổi giữa nhân viên và khách hàng qua các kênh.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `interaction_id` | Data / VARCHAR(30) | Yes | Yes | Auto-naming: `INT-.YYYY.-.#####` | Khóa chính (PK). Hệ thống tự sinh mã. |
| `customer_id` | Link:Customer / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Customer.customer_id` | Mã khách hàng tương tác (Nullable với khách vãng lai). |
| `opportunity_id` | Link:Opportunity / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Opportunity` | Tùy chọn. Gắn với cơ hội bán hàng nếu trao đổi về nhu cầu mua. |
| `ticket_id` | Link:Support_Ticket / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Support_Ticket` | Tùy chọn. Gắn với phiếu hỗ trợ nếu trao đổi về bảo hành/khiếu nại. |
| `channel` | Select / VARCHAR(50) | Yes | No | `Gọi điện thoại`, `Trực tiếp tại quầy`, `Tin nhắn / Zalo / Chat` | Kênh diễn ra cuộc trao đổi. |
| `direction` | Select / VARCHAR(20) | Yes | No | `Đến (Inbound)`, `Đi (Outbound)` | Mặc định: `Đến (Inbound)`. |
| `interaction_time` | Datetime / DATETIME | Yes | No | $\le$ Thời điểm hiện tại (`NOW()`) | Thời điểm cuộc trao đổi diễn ra, không được chọn ngày tương lai. |
| `summary` | Long Text / TEXT | Yes | No | Chuỗi văn bản mô tả chi tiết | Nội dung chính cuộc tư vấn/trao đổi, phản hồi của khách. |
| `performed_by_user` | Link:Staff_User / VARCHAR(50) | Yes | No | Khóa ngoại trỏ về `Staff_User.user_id` | Nhân viên trực tiếp thực hiện ghi nhận tương tác. |

---

### 2.6. Thực thể `Care_Task` (Công việc chăm sóc & Nhắc việc)
* **Mục đích:** Quản lý lịch hẹn, công việc cần làm của nhân viên đối với khách hàng có thời hạn và trạng thái theo dõi.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `task_id` | Data / VARCHAR(30) | Yes | Yes | Auto-naming: `TASK-.YYYY.-.#####` | Khóa chính (PK). Hệ thống tự sinh mã. |
| `customer_id` | Link:Customer / VARCHAR(30) | Yes | No | Khóa ngoại trỏ về `Customer.customer_id` | Bắt buộc phải có khách hàng được chăm sóc. |
| `opportunity_id` | Link:Opportunity / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Opportunity` | Cơ hội tư vấn liên quan (nếu có). |
| `ticket_id` | Link:Support_Ticket / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Support_Ticket` | Phiếu hỗ trợ liên quan (nếu có). |
| `task_title` | Data / VARCHAR(140) | Yes | No | Chuỗi văn bản $\le 140$ ký tự | Tiêu đề công việc (ví dụ: "Gọi lại tư vấn màu iPhone 15 Pro Max"). |
| `task_description` | Small Text / TEXT | No | No | Chuỗi văn bản | Hướng dẫn hoặc chi tiết nội dung cần chăm sóc. |
| `due_date` | Datetime / DATETIME | Yes | No | Ngày giờ hợp lệ | Hạn chót hoàn thành việc / Lịch hẹn với khách hàng. |
| `priority` | Select / VARCHAR(20) | Yes | No | `Thấp`, `Trung bình`, `Cao` | Mức độ ưu tiên. Mặc định: `Trung bình`. |
| `status` | Select / VARCHAR(50) | Yes | No | `Chưa thực hiện`, `Đang thực hiện`, `Hoàn thành`, `Quá hạn`, `Hủy` | Mặc định là `Chưa thực hiện`. Hệ thống tự đánh dấu `Quá hạn` nếu `NOW() > due_date` và chưa hoàn thành. |
| `assigned_to_user` | Link:Staff_User / VARCHAR(50) | Yes | No | Khóa ngoại trỏ về `Staff_User.user_id` | Bắt buộc có nhân viên được giao việc (phục vụ KPI SLA). |
| `completed_at` | Datetime / DATETIME | No | No | $\ge$ Thời điểm tạo việc | Tự động ghi nhận thời điểm khi chuyển sang `Hoàn thành`. |
| `cancellation_reason` | Small Text / TEXT | No | No | Chuỗi văn bản | Bắt buộc nhập khi chuyển `status` sang `Hủy`. |

---

### 2.7. Thực thể `Support_Ticket` (Phiếu hỗ trợ dịch vụ & Hậu mãi)
* **Mục đích:** Tiếp nhận, theo dõi tiến độ và ghi nhận kết quả xử lý các khiếu nại, yêu cầu bảo hành, đổi trả của khách hàng.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `ticket_id` | Data / VARCHAR(30) | Yes | Yes | Auto-naming: `TICK-.YYYY.-.#####` | Khóa chính (PK). Hệ thống tự sinh mã. |
| `customer_id` | Link:Customer / VARCHAR(30) | Yes | No | Khóa ngoại trỏ về `Customer.customer_id` | Khách hàng phát sinh yêu cầu hỗ trợ. |
| `ticket_type` | Select / VARCHAR(50) | Yes | No | `Bảo hành thiết bị`, `Đề nghị đổi trả`, `Khiếu nại dịch vụ / Phản ánh` | 3 nhóm yêu cầu hậu mãi trong phạm vi nghiên cứu. |
| `related_product` | Data / VARCHAR(140) | Yes | No | Tên sản phẩm / thiết bị | Sản phẩm khách hàng mang đến bảo hành / khiếu nại. |
| `imei_serial` | Data / VARCHAR(50) | No | No | Chuỗi ký tự IMEI (15 số) hoặc Serial | Số nhận dạng thiết bị để tra cứu điều kiện bảo hành. |
| `issue_description` | Long Text / TEXT | Yes | No | Chuỗi văn bản chi tiết | Tình trạng lỗi, phản ánh của khách khi tiếp nhận. |
| `status` | Select / VARCHAR(50) | Yes | No | `Tiếp nhận`, `Đang xử lý`, `Đã có kết quả`, `Đã thông báo khách`, `Đóng phiếu` | Mặc định: `Tiếp nhận`. Quản lý chuyển trạng thái nghiêm ngặt. |
| `assigned_to_user` | Link:Staff_User / VARCHAR(50) | Yes | No | Khóa ngoại trỏ về `Staff_User.user_id` | Bắt buộc phải có nhân viên phụ trách (đáp ứng 100% phiếu có trách nhiệm). |
| `branch_id` | Link:Branch / VARCHAR(20) | Yes | No | Khóa ngoại trỏ về `Branch.branch_id` | Chi nhánh tiếp nhận xử lý phiếu. |
| `external_partner_status` | Small Text / TEXT | No | No | Chuỗi văn bản ghi nhận | Ghi nhận tiến độ phản hồi từ trung tâm bảo hành hãng / đối tác ngoài. |
| `resolution_result` | Long Text / TEXT | No | No | Chuỗi văn bản kết luận | Bắt buộc nhập khi chuyển sang `Đã có kết quả` hoặc `Đóng phiếu`. |
| `customer_notified` | Check / TINYINT(1) | Yes | No | `0` (Chưa báo), `1` (Đã báo) | Mặc định `0`. **Bắt buộc = 1 mới được chuyển trạng thái sang `Đóng phiếu`**. |
| `created_at` | Datetime / DATETIME | Yes | No | Ngày giờ tạo phiếu | Mặc định: `NOW()`. |
| `closed_at` | Datetime / DATETIME | No | No | Ngày giờ đóng phiếu | Ghi nhận tự động khi chuyển sang `Đóng phiếu`. |

---

### 2.8. Thực thể `Purchase_History_Ref` (Lịch sử mua hàng tham chiếu từ CSV)
* **Mục đích:** Lưu trữ dữ liệu giao dịch mẫu nhập từ tệp CSV để nhân viên tra cứu nhanh lịch sử mua sắm khi phục vụ.

| Tên vật lý (Field name) | Kiểu dữ liệu (Frappe/SQL) | Bắt buộc (Req) | Duy nhất (Unique) | Giá trị hợp lệ / Enum | Nguồn dữ liệu & Quy tắc kiểm tra (Validation Rule) |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `order_ref_id` | Data / VARCHAR(50) | Yes | Yes | Mã đơn hàng trên hóa đơn (VD: `DH20261001-01`) | Khóa chính (PK). Lấy từ tệp CSV import. |
| `customer_id` | Link:Customer / VARCHAR(30) | No | No | Khóa ngoại trỏ về `Customer` | Tự động liên kết qua SĐT khi import hoặc để trống nếu chưa khớp. |
| `customer_phone` | Data / VARCHAR(20) | Yes | No | SĐT người mua | Căn cứ tra cứu nhanh giao dịch lịch sử khi khách đến cửa hàng. |
| `customer_name` | Data / VARCHAR(140) | Yes | No | Họ tên người mua | Tên khách hàng in trên phiếu mua hàng. |
| `order_date` | Date / DATE | Yes | No | Định dạng `YYYY-MM-DD` | Ngày phát sinh đơn hàng tại cửa hàng. |
| `product_name` | Data / VARCHAR(140) | Yes | No | Tên sản phẩm đã mua | Tên thiết bị công nghệ (iPhone, Laptop, Phụ kiện...). |
| `imei_serial` | Data / VARCHAR(50) | No | No | IMEI / Serial máy | Dùng đối chiếu với `imei_serial` trong phiếu bảo hành. |
| `total_amount` | Currency / DECIMAL(18,2) | Yes | No | Giá trị $\ge 0$ | Tổng số tiền thanh toán của đơn hàng. |
| `purchase_branch` | Data / VARCHAR(140) | Yes | No | Tên hoặc mã cửa hàng bán | Chi nhánh đã xuất bán sản phẩm. |
| `imported_at` | Datetime / DATETIME | Yes | No | Ngày giờ import | Tự động ghi thời điểm chạy lệnh import dữ liệu CSV vào hệ thống. |

---

## 3. QUY ĐỊNH CẤU TRÚC VÀ DỮ LIỆU MẪU TỆP CSV LỊCH SỬ MUA HÀNG (`order_history.csv`)

Tệp dữ liệu CSV dùng để nạp dữ liệu lịch sử mua sắm mẫu vào CRM có cấu trúc các cột như sau:

```csv
order_ref_id,customer_phone,customer_name,order_date,product_name,imei_serial,total_amount,purchase_branch
DH20260915-01,0901234567,Nguyễn Văn An,2026-09-15,iPhone 15 Pro Max 256GB,354890123456789,29990000.00,CellphoneS 136 Nguyễn Thái Học
DH20260918-02,0987654321,Trần Thị Mai,2026-09-18,Laptop Lenovo LOQ 15IAX9 83GS001RVN,PF4X89A1,21490000.00,CellphoneS 21A Hàng Bài
DH20260920-03,0912345678,Lê Hoàng Nam,2026-09-20,Củ sạc nhanh GaN 65W Ugreen,UG65W-2026,690000.00,CellphoneS 136 Nguyễn Thái Học
DH20260925-04,0901234567,Nguyễn Văn An,2026-09-25,Ốp lưng iPhone 15 Pro Max MagSafe,OL-IP15PM,450000.00,CellphoneS 136 Nguyễn Thái Học
DH20260928-05,0934567890,Phạm Đức Minh,2026-09-28,iPhone 15 Pro Max 512GB,356712349876123,34990000.00,CellphoneS 21A Hàng Bài
```

---

## 4. QUY TẮC RÀNG BUỘC TOÀN VẸN & LOGIC NGHIỆP VỤ (VALIDATION LOGIC)

1. **Ràng buộc Định dạng Số điện thoại (Phone Validation):**
   * Mọi trường SĐT (`phone_number`, `temp_phone_number`, `customer_phone`) phải bắt đầu bằng đầu số di động hợp lệ tại Việt Nam (`03`, `05`, `07`, `08`, `09`) và có đúng 10 chữ số.
2. **Ràng buộc Ngày tháng Logic (Date Range Validation):**
   * `Interaction.interaction_time` $\le$ `NOW()` (Không được ghi nhận tương tác ở thời điểm tương lai).
   * `Care_Task.completed_at` $\ge$ `Care_Task.created_at` (Thời điểm hoàn thành không được trước thời điểm giao việc).
   * `Support_Ticket.closed_at` $\ge$ `Support_Ticket.created_at` (Thời điểm đóng phiếu không được trước thời điểm mở phiếu).
3. **Ràng buộc Điều kiện Đóng phiếu Hỗ trợ (Ticket Closing Gate):**
   * Nhân viên **chỉ được phép** chuyển trạng thái `Support_Ticket.status` sang `Đóng phiếu` khi thỏa mãn đồng thời 2 điều kiện:
     1. Trường `resolution_result` không được để trống (`NOT NULL`).
     2. Trường `customer_notified` phải có giá trị là `1` (True - Đã liên hệ thông báo kết quả cho khách hàng).
4. **Ràng buộc Quyền riêng tư Chăm sóc (Care Opt-out Protection):**
   * Nếu `Customer.opt_out_care = 1`, hệ thống hiển thị cảnh báo đỏ và ngăn chặn nhân viên tạo các công việc chăm sóc (`Care_Task`) mới dạng tiếp thị/tư vấn bán hàng.
