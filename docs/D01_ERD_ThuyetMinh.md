# SẢN PHẨM BÀN GIAO D01: SƠ ĐỒ ERD LOGIC & THUYẾT MINH QUAN HỆ
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã sản phẩm:** D01  
**Thuộc nhiệm vụ:** Nhiệm vụ 1 (Sàng lọc & Xác định thực thể) & Nhiệm vụ 2 (Xây dựng ERD Logic & Giải quyết 2 bài toán dữ liệu)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Nền tảng mục tiêu:** Frappe Framework v15 / ERPNext v15  
**Trạng thái:** Hoàn thiện Mốc M4 (Final Deliverable)

---

## TỔNG QUAN PHẠM VI & RANH GIỚI NGHIỆP VỤ CRM CELLPHONES

Dự án tập trung xây dựng phân hệ Quản lý Quan hệ Khách hàng (**CRM**) chuyên biệt cho chuỗi bán lẻ công nghệ **CellphoneS** với các phạm vi và ranh giới đã được xác định cụ thể:

1. **Phạm vi khách hàng & mô hình kinh doanh:**
   * **Mô hình:** B2C (Business to Customer) thuần túy.
   * **Đối tượng:** Toàn bộ là **Khách hàng cá nhân**, không phục vụ khách hàng doanh nghiệp (B2B/Corporate).

2. **Ngành hàng và Sản phẩm nghiên cứu:**
   * Tập trung 3 nhóm ngành hàng bán lẻ cốt lõi của CellphoneS:
     * **Điện thoại** (*Smartphones* - Apple iPhone3. **Nhóm người dùng hệ thống CRM (5 Vai trò chuẩn hóa theo tài liệu BA):**
   * **Bán hàng và CSKH cửa hàng (Store Sales & CSKH Agent):** Xem công việc, khách hàng và giao dịch trong phạm vi cửa hàng được cấp; tiếp nhận khách tiềm năng (Lead/Cơ hội), tư vấn bán hàng, tiếp nhận tương tác tại quầy và mở phiếu hỗ trợ bước đầu.
   * **Quản lý cửa hàng (Store Manager):** Xem toàn bộ dữ liệu thuộc cửa hàng quản lý; phân công và điều chuyển cơ hội/phiếu hỗ trợ giữa các nhân viên trong cửa hàng; giám sát SLA tại chi nhánh.
   * **CSKH cấp chuỗi (Chain CSKH Agent):** Tiếp nhận tương tác đa kênh tập trung (Hotline 1800, Fanpage, Zalo OA); điều chuyển cơ hội và ticket giữa các cửa hàng; phối hợp điều phối dịch vụ.
   * **Quản lý chuỗi (Chain Manager / Head of CSKH):** Xem báo cáo tổng hợp toàn chuỗi (mô phỏng hai cửa hàng); giám sát KPI tỷ lệ thắng, SLA và chất lượng phục vụ.
   * **Quản trị hệ thống (System Administrator):** Quản lý tài khoản, phân quyền vai trò (Role Permission), cấu hình hệ thống, cập nhật hạng hội viên mẫu và thực hiện gộp hồ sơ khách trùng.
   *(Lưu ý: Khách hàng và đối tác bảo hành/hãng là stakeholder bên ngoài, không đăng nhập CRM trong MVP; vai trò kỹ thuật sửa chữa Điện Thoại Vui/CareS không gộp chung vào CSKH nội bộ).*

4. **5 Nhóm nghiệp vụ chính:**
   * **Nghiệp vụ 1: Tư vấn trước mua & Quản lý cơ hội (Lead/Opportunity):** Ghi nhận đầu mối, sản phẩm quan tâm (iPhone 15 Pro Max, Lenovo LOQ 15IAX9 83GS001RVN, Sạc GaN), ngân sách, thiết bị đang dùng, nhu cầu thu cũ, cấu hình mong muốn và lịch chăm sóc (nhắc việc 3 ngày).
   * **Nghiệp vụ 2: Hỗ trợ đơn hàng tham chiếu:** Tra cứu đơn hàng/hóa đơn CSV, thời điểm cập nhật; giải đáp hoặc lập ticket liên kết.
   * **Nghiệp vụ 3: Tiếp nhận đổi trả & Hậu mãi:** Lập ticket theo chính sách từng dòng sản phẩm, ngày mua/nhận hàng, hiện trạng ngoại quan; theo dõi tiến độ giải quyết (không tự cam kết đổi máy nguyên seal khi chưa thẩm định).
   * **Nghiệp vụ 4: Bảo hành và khiếu nại:** Tiếp nhận thiết bị, mô tả lỗi/phản ánh, giao dịch mua, ảnh chứng từ; theo dõi tiến độ xử lý và phối hợp kỹ thuật/hãng.
   * **Nghiệp vụ 5: Quản lý hồ sơ & Báo cáo:** Hồ sơ 360°, hạng thành viên mẫu (S-NULL, S-NEW, S-MEM, S-VIP), nhóm ưu đãi giáo dục (S-Student, S-Teacher), báo cáo tỷ lệ thắng và SLA.

5. **Ranh giới nghiệp vụ (Scope Boundaries):**
   * **Về Bảo hành / Đổi trả:** CRM tập trung **quản lý yêu cầu, ghi nhận tình trạng máy lúc tiếp nhận, lưu lịch sử và theo dõi tiến độ xử lý**; **CHƯA** quản lý chi tiết sửa chữa kỹ thuật/kho linh kiện, **CHƯA** tạo Credit Note, hoàn tiền hay xuất/nhập kho.
   * **Về Hội viên SMember & Ưu đãi Giáo dục:** CRM lưu trữ và tra cứu **hạng mẫu (S-NULL, S-NEW, S-MEM, S-VIP)** được nhập tay/CSV bởi người có quyền; lưu riêng nhóm giáo dục (**S-Student, S-Teacher**) kèm trạng thái xác minh và thời hạn; **CHƯA** tự động chạy thuật toán tính chi tiêu tích lũy, tự động nâng/hạ hạng hay tự động cộng dồn voucher.
   * **Về Thông báo:** Trong MVP **CHƯA** gửi tin nhắn tự động qua Zalo/SMS; nhân viên ghi nhận thao tác thông báo trên hệ thống.

---

## PHẦN 1: BẢNG DANH MỤC THỰC THỂ & BIỆN LUẬN PHẠM VI (NHIỆM VỤ 1)

Dưới đây là bảng sàng lọc toàn bộ các thực thể dữ liệu được thiết kế tối ưu cho bài toán CRM bán lẻ công nghệ CellphoneS:

| STT | Tên thực thể (Logical / Physical) | Mục đích sử dụng trong CRM CellphoneS | Quyết định | Lý do biện luận nghiệp vụ & kỹ thuật Frappe |
| :---: | :--- | :--- | :---: | :--- |
| **1** | **Chi nhánh / Cửa hàng**<br>`BRANCH_STORE` | Quản lý hệ thống Showroom CellphoneS (thử nghiệm mô phỏng 2 cửa hàng). | **CHỌN (Cơ sở phân vùng)** | Đóng vai trò phân vùng dữ liệu theo cửa hàng, làm căn cứ phân quyền cho nhân viên và phân bổ cơ hội/ticket theo O03. |
| **2** | **Khách hàng**<br>`CUSTOMER` | Quản lý mã định danh khách duy nhất, ổn định; SĐT liên hệ/tìm kiếm; họ tên, email, CCCD, nhóm giáo dục. | **CHỌN (Cốt lõi)** | Thực thể trung tâm của CRM. Ánh xạ sang DocType `Customer`. Cảnh báo trùng SĐT, không tự ý gộp hồ sơ. |
| **3** | **Hồ sơ Hội viên Smember**<br>`SMEMBER_PROFILE` | Lưu trữ 4 hạng mẫu (`S-NULL`, `S-NEW`, `S-MEM`, `S-VIP`) và nhóm giáo dục (`S-Student`, `S-Teacher`) kèm trạng thái xác minh. | **CHỌN (Tra cứu / Lưu trữ)** | Tách biệt với thông tin cá nhân cơ bản để phục vụ tra cứu chính sách ưu đãi khách hàng mà không phá vỡ cấu trúc DocType `Customer`. Quan hệ 1-1 với `Customer`. |
| **4** | **Khách tiềm năng & Cơ hội**<br>`LEAD_OPPORTUNITY` | Quản lý nhu cầu tư vấn: sản phẩm quan tâm, ngân sách, thời điểm mua, thiết bị đang dùng, nhu cầu thu cũ, cấu hình mong muốn, lịch chăm sóc 3 ngày. | **CHỌN (Cốt lõi Tư vấn)** | Một khách có thể có nhiều cơ hội. Quản lý tiến trình tư vấn và tính tỷ lệ thắng cơ hội theo quy tắc đã duyệt. |
| **5** | **Tương tác đa kênh**<br>`CUSTOMER_INTERACTION` | Ghi nhận nhật ký mỗi lần tiếp xúc qua Hotline, Chat Zalo/Fanpage, Web form hoặc trực tiếp tại cửa hàng (thời gian, kênh, người thực hiện, kết quả). | **CHỌN (Cốt lõi CSKH)** | Giúp nhân viên có cái nhìn 360 độ về lịch sử trao đổi của khách. Đóng vai trò làm nguồn gốc phát sinh Phiếu hỗ trợ khi có thắc mắc/khiếu nại. |
| **6** | **Phiếu hỗ trợ / Hậu mãi**<br>`SUPPORT_TICKET` | Tiếp nhận và theo dõi tiến độ đổi trả, bảo hành, khiếu nại dịch vụ (kết quả: bảo hành, đổi theo chính sách, sửa có phí, từ chối, rút yêu cầu). | **CHỌN (Cốt lõi Hậu mãi)** | Quản lý vòng đời tiếp nhận & tiến độ xử lý hậu mãi. Ánh xạ sang Custom DocType `Support Ticket` với Workflow chuẩn hóa, không quản lý chi tiết sửa chữa hay hoàn tiền. |
| **7** | **Chi tiết thiết bị tiếp nhận**<br>`TICKET_ITEM_CONDITION` | Ghi nhận tình trạng ngoại quan lúc nhận máy (máy đẹp, trầy xước, cấn móp, nứt vỡ) và phụ kiện đi kèm khi lập phiếu hỗ trợ. | **CHỌN (Child Table)** | Thiết kế dưới dạng Bảng con (Child Table) gắn trực tiếp vào `SUPPORT_TICKET` nhằm lưu chứng cứ biên bản bàn giao máy, không quản lý kho linh kiện kỹ thuật. |
| **8** | **Nhật ký tiến độ xử lý**<br>`TICKET_ACTIVITY_LOG` | Ghi nhận các bước xử lý nội bộ, chuyển trạng thái trước/sau, phân công, ghi nhận thông báo khách và bước xác minh/chờ khách. | **CHỌN (Child Table / Timeline)** | Lưu tiến trình phân công và xử lý công việc giữa các bộ phận, phục vụ đo lường thời gian thực hiện theo cam kết SLA. |
| **9** | **Dữ liệu đơn tham chiếu**<br>`SALES_INVOICE_REFERENCE` | Lưu đơn mua hàng tham chiếu (nhập CSV): Mã đơn, ngày mua, cửa hàng xuất, sản phẩm, IMEI/Serial, hạn bảo hành. | **CHỌN (Tham chiếu Mua hàng)** | Đơn hàng tham chiếu phục vụ tra cứu đối soát điều kiện bảo hành/đổi trả; dữ liệu không đồng bộ trực tiếp từ doanh nghiệp. |
| **10** | **Sản phẩm tham chiếu**<br>`ITEM_REFERENCE` | Danh mục mẫu hàng 3 nhóm (Điện thoại iPhone 15 Pro Max, Laptop Lenovo LOQ 83GS001RVN, Sạc GaN) kèm thông số và thời hạn bảo hành chuẩn. | **CHỌN (Mức tham chiếu)** | Tách biệt mẫu hàng với thiết bị cụ thể có IMEI/Serial; lưu chính sách bảo hành đúng theo từng mã hàng. |
| **11** | **Người dùng hệ thống**<br>`STAFF_USER` | Đại diện 5 vai trò nội bộ: Bán hàng & CSKH cửa hàng, Quản lý cửa hàng, CSKH chuỗi, Quản lý chuỗi, Quản trị hệ thống. | **CHỌN (Hệ thống)** | Sử dụng DocType `User` chuẩn của Frappe kết hợp phân quyền theo cửa hàng (User Permissions) để kiểm soát phạm vi truy cập dữ liệu. |
| **12** | **Nhật ký gộp hồ sơ**<br>`CUSTOMER_MERGE_LOG` | Lưu vết kiểm toán khi Quản trị viên/Quản lý thực hiện gộp 2 hồ sơ khách hàng trùng lặp sau khi đã xác minh cảnh báo. | **CHỌN (Kiểm toán)** | Đảm bảo tính toàn vẹn dữ liệu, ghi nhận rõ ai thực hiện, hồ sơ bị gộp, hồ sơ giữ lại và số lượng Ticket/Tương tác đã di dời. |CHỌN (Child Table / Timeline)** | Lưu tiến trình phân công và xử lý công việc giữa các bộ phận, phục vụ đo lường thời gian thực hiện theo cam kết SLA. |
| **9** | **Dữ liệu mua hàng tham chiếu**<br>`SALES_INVOICE_REFERENCE` | Lưu thông tin tham chiếu hóa đơn mua hàng: Mã hóa đơn, Ngày mua, Chi nhánh xuất bán, Sản phẩm kèm số IMEI/Serial, Hạn bảo hành gốc. | **CHỌN (Tham chiếu Mua hàng)** | Phục vụ tra cứu lịch sử mua hàng, xác thực điều kiện áp dụng chính sách đổi mới 1-đổi-1 trong 30 ngày và đối soát bảo hành chính hãng. |
| **10** | **Sản phẩm tham chiếu**<br>`ITEM_REFERENCE` | Danh mục sản phẩm kinh doanh thuộc 3 ngành hàng: Điện thoại, Laptop, Phụ kiện (Mã SKU, Tên sản phẩm, Thương hiệu, Thời gian bảo hành). | **CHỌN (Mức tham chiếu)** | Không quản lý kế toán/tồn kho phức tạp trong CRM; chỉ lưu dữ liệu tham chiếu để chọn sản phẩm khi tư vấn cơ hội và kiểm tra điều kiện tiếp nhận bảo hành. |
| **11** | **Người dùng hệ thống**<br>`STAFF_USER` | Đại diện cho 5 nhóm nhân sự: Nhân viên tư vấn, Nhân viên CSKH, Quản lý cửa hàng, Quản trị viên, Nhân viên phân tích. | **CHỌN (Hệ thống)** | Sử dụng DocType `User` chuẩn của Frappe Framework kết hợp phân quyền Role Permission Manager để kiểm soát quyền hạn dữ liệu. |
| **12** | **Nhật ký gộp hồ sơ**<br>`CUSTOMER_MERGE_LOG` | Lưu vết kiểm toán (Audit Trail) khi tiến hành gộp 2 hồ sơ khách hàng cá nhân bị trùng lặp số điện thoại hoặc thông tin định danh. | **CHỌN (Kiểm toán)** | Đảm bảo tính toàn vẹn dữ liệu, ghi nhận rõ ai thực hiện, hồ sơ bị gộp, hồ sơ giữ lại và số lượng Ticket/Tương tác đã di dời. |

---

## PHẦN 2: SƠ ĐỒ ERD LOGIC HỆ THỐNG CRM CELLPHONES (NHIỆM VỤ 2)

Sơ đồ ERD Logic thể hiện cấu trúc các thực thể, Khóa chính (PK), Khóa ngoại (FK), kiểu dữ liệu thuộc tính và bản số quan hệ (Cardinality & Optionality):

```mermaid
erDiagram
    BRANCH_STORE ||--o{ STAFF_USER : "employs (1-N)"
    BRANCH_STORE ||--o{ LEAD_OPPORTUNITY : "assigned to (1-N)"
    BRANCH_STORE ||--o{ CUSTOMER_INTERACTION : "hosts walk-in (1-N)"
    BRANCH_STORE ||--o{ SUPPORT_TICKET : "receives at (1-N)"
    BRANCH_STORE ||--o{ SALES_INVOICE_REFERENCE : "issues (1-N)"

    CUSTOMER ||--o| SMEMBER_PROFILE : "has membership (1-1)"
    CUSTOMER ||--o{ CUSTOMER_INTERACTION : "participates (1-N)"
    CUSTOMER ||--o{ SUPPORT_TICKET : "requests (1-N)"
    CUSTOMER ||--o{ LEAD_OPPORTUNITY : "converted from (0..1-N)"
    CUSTOMER ||--o{ SALES_INVOICE_REFERENCE : "owns (1-N)"
    CUSTOMER ||--o{ CUSTOMER_MERGE_LOG : "source or target (1-N)"

    STAFF_USER ||--o{ CUSTOMER_INTERACTION : "handled by (1-N)"
    STAFF_USER ||--o{ SUPPORT_TICKET : "assigned to (1-N)"
    STAFF_USER ||--o{ LEAD_OPPORTUNITY : "consults (1-N)"
    STAFF_USER ||--o{ CUSTOMER_MERGE_LOG : "executed by (1-N)"
    STAFF_USER ||--o{ TICKET_ACTIVITY_LOG : "logs action (1-N)"

    ITEM_REFERENCE ||--o{ SUPPORT_TICKET : "referenced in (1-N)"
    ITEM_REFERENCE ||--o{ LEAD_OPPORTUNITY : "interested in (1-N)"
    ITEM_REFERENCE ||--o{ SALES_INVOICE_REFERENCE : "sold item (1-N)"

    SALES_INVOICE_REFERENCE ||--o{ SUPPORT_TICKET : "verified against (0..1-N)"

    SUPPORT_TICKET ||--o{ TICKET_ITEM_CONDITION : "checks condition (1-N)"
    SUPPORT_TICKET ||--o{ TICKET_ACTIVITY_LOG : "records progress (1-N)"
    CUSTOMER_INTERACTION ||--o| SUPPORT_TICKET : "escalated into (0..1-1)"

    BRANCH_STORE {
        string branch_id PK "Mã chi nhánh (BR-XXXXX)"
        string branch_name "Tên chi nhánh Showroom CellphoneS"
        string address "Địa chỉ chi tiết cửa hàng"
        string province_city "Tỉnh / Thành phố"
        string hotline "Số điện thoại hotline chi nhánh"
        string manager_user FK "Quản lý cửa hàng (Link Staff_User)"
        boolean is_active "Đang hoạt động (Yes/No)"
    }

    CUSTOMER {
        string customer_id PK "Mã định danh KH duy nhất (CUST-YYYY-XXXXX)"
        string phone_number UK "Số điện thoại liên hệ/tìm kiếm (10 chữ số)"
        string full_name "Họ và tên khách hàng cá nhân"
        string email "Email liên hệ cá nhân"
        string identity_card "Số CCCD / CMND (9 hoặc 12 số)"
        date date_of_birth "Ngày sinh"
        string gender "Giới tính (Nam / Nu / Khac)"
        string address "Địa chỉ liên hệ"
        string province_city "Tỉnh / Thành phố"
        string status "Trạng thái (Active / Inactive / Merged)"
        datetime created_at "Thời gian tạo"
    }

    SMEMBER_PROFILE {
        string profile_id PK "Mã hồ sơ Smember (SMB-XXXXX)"
        string customer_id FK "Liên kết Khách hàng cá nhân (1-1 Unique)"
        string member_tier "Hạng hội viên mẫu (S-NULL / S-NEW / S-MEM / S-VIP)"
        string edu_type "Nhóm giáo dục (None / S-Student / S-Teacher)"
        string edu_status "Trạng thái xác minh GD (Chua_xac_minh / Da_xac_minh / Tu_choi)"
        date edu_expiry_date "Thời hạn nhóm giáo dục nếu có"
        string edu_verified_by FK "Người cập nhật xác minh (Link Staff_User)"
        date join_date "Ngày tham gia hội viên"
        date tier_expiry_date "Hạn duy trì hạng hiện tại"
        string tier_note "Ghi chú quyền lợi chính sách"
    }

    LEAD_OPPORTUNITY {
        string lead_id PK "Mã cơ hội (LEAD-YYYY-XXXXX)"
        string lead_name "Tên khách tiềm năng"
        string phone_number "Số điện thoại liên hệ"
        string email "Email liên hệ"
        string channel "Kênh tiếp nhận (Website / Facebook / Zalo / Showroom / Hotline)"
        string product_category "Ngành hàng quan tâm (Dien_thoai / Laptop / Phu_kien)"
        string item_sku FK "Sản phẩm quan tâm (Link Item_Reference)"
        currency budget "Ngân sách dự kiến của khách (VND)"
        date expected_buy_date "Thời điểm dự kiến mua"
        string device_in_use "Thiết bị đang dùng hiện tại"
        string trade_in_demand "Nhu cầu thu cũ đổi mới (Co / Khong)"
        string desired_specs "Cấu hình / Màu sắc / Dung lượng mong muốn"
        string branch_id FK "Cửa hàng tư vấn / nhận máy (Link Branch_Store)"
        string consultation_type "Loại nhu cầu (Tu_van / Dat_truoc_PreOrder / Tra_gop)"
        string status "Trạng thái (Open / Contacted / Qualified / Converted / Lost)"
        datetime follow_up_due "Lịch nhắc chăm sóc (3 ngày làm việc)"
        string assigned_to FK "Nhân viên phụ trách tư vấn (Link Staff_User)"
        string converted_customer_id FK "Mã KH sau chuyển đổi (Link Customer - Nullable)"
        text notes "Nội dung ghi chú tư vấn chi tiết"
    }

    CUSTOMER_INTERACTION {
        string interaction_id PK "Mã tương tác (INT-YYYY-XXXXX)"
        string customer_id FK "Mã khách hàng (Link Customer - Nullable)"
        string contact_phone "SĐT người liên hệ"
        string contact_name "Họ tên người liên hệ"
        string channel "Kênh (Hotline_1800 / Zalo_OA / Fanpage / Showroom / Web)"
        string branch_id FK "Cửa hàng tiếp nhận (Link Branch_Store)"
        string interaction_purpose "Mục đích (Tu_van / Don_hang / Ho_tro_ky_thuat / Khieu_nai)"
        text summary "Tóm tắt nội dung trao đổi"
        string staff_id FK "Nhân viên thực hiện liên hệ (Link Staff_User)"
        datetime interaction_time "Thời điểm phát sinh tương tác"
        string interaction_result "Kết quả tương tác (Thanh_cong / Hen_lai / Khong_nghe_may)"
        string escalated_ticket_id FK "Phiếu hỗ trợ phát sinh (Link Support_Ticket - Nullable)"
    }

    SUPPORT_TICKET {
        string ticket_id PK "Mã phiếu hỗ trợ (TCK-YYYY-XXXXX)"
        string customer_id FK "Mã khách hàng (Link Customer - Nullable)"
        string contact_phone "Số điện thoại liên hệ"
        string contact_name "Họ tên người yêu cầu"
        string channel "Kênh tiếp nhận (Hotline / Showroom / Zalo / Web)"
        string branch_id FK "Cửa hàng tiếp nhận ban đầu (Link Branch_Store)"
        string item_sku FK "Mã sản phẩm tiếp nhận (Link Item_Reference)"
        string serial_imei "Số Serial / IMEI thiết bị (15 số GSMA)"
        string sales_invoice_id FK "Đơn mua hàng tham chiếu đối soát (Link Sales_Invoice - Nullable)"
        string issue_type "Loại yêu cầu (Tiep_nhan_bao_hanh / Doi_tra_theo_chinh_sach / Khieu_nai_dich_vu)"
        string priority "Độ ưu tiên (Low / Medium / High / Urgent)"
        string status "Trạng thái (Open / In_Progress / Pending_Vendor / Resolved / Closed)"
        string assigned_staff FK "Nhân viên xử lý tiến độ (Link Staff_User)"
        datetime sla_deadline "Hạn chót giải quyết theo SLA"
        datetime resolved_time "Thời điểm giải quyết xong"
        datetime closed_time "Thời điểm đóng phiếu"
        string resolution_result "Kết quả xử lý (Bao_hanh / Doi_theo_chinh_sach / Sua_co_phi / Tu_choi / Khach_rut)"
        string notify_customer_status "Trạng thái thông báo khách (Chua_thong_bao / Da_thong_bao_qua_dien_thoai)"
        text customer_feedback "Ý kiến và phản hồi của khách hàng"
        string csat_score "Điểm hài lòng CSAT ghi nhận (1-5 Sao)"
    }

    TICKET_ITEM_CONDITION {
        string row_id PK "Mã dòng chi tiết"
        string ticket_id FK "Mã phiếu hỗ trợ cha (Link Support_Ticket)"
        string reported_issue "Mô tả hiện tượng lỗi từ khách hàng"
        string physical_condition "Hiện trạng ngoại quan (May_dep / Tray_xuoc / Can_mop / Nut_kinh)"
        string accessories_included "Phụ kiện kèm theo (Hop, Cu_sac, Day_cap, Khong)"
        string warranty_eligibility "Điều kiện tiếp nhận ban đầu (Hop_le / Nghi_ngo_roi_nuoc / Can_kiem_dinh)"
    }

    TICKET_ACTIVITY_LOG {
        string log_id PK "Mã nhật ký xử lý"
        string ticket_id FK "Mã phiếu hỗ trợ cha (Link Support_Ticket)"
        string staff_id FK "Nhân viên cập nhật (Link Staff_User)"
        string action_type "Hành động (Cap_nhat_tien_do / Chuyen_trang_thai / Phan_cong / Xac_minh / Cho_khach / Ghi_nhan_thong_bao)"
        string from_status "Trạng thái trước"
        string to_status "Trạng thái sau"
        text progress_notes "Ghi chú tiến độ chi tiết"
        datetime logged_at "Thời gian ghi nhận"
    }

    SALES_INVOICE_REFERENCE {
        string invoice_id PK "Mã đơn hàng / hóa đơn tham chiếu (INV-YYYY-XXXXX)"
        string customer_id FK "Mã khách hàng mua (Link Customer)"
        string branch_id FK "Cửa hàng xuất bán (Link Branch_Store)"
        string item_sku FK "Mã sản phẩm mua (Link Item_Reference)"
        string serial_imei "Số Serial / IMEI xuất kho nếu có"
        datetime purchase_date "Ngày giờ mua / nhận hàng"
        currency grand_total "Tổng giá trị đơn hàng (VND)"
        date warranty_expiry_date "Ngày hết hạn bảo hành theo mã hàng"
        string invoice_status "Trạng thái đơn tham chiếu (Paid / Exchanged / Cancelled)"
    }

    ITEM_REFERENCE {
        string item_sku PK "Mã sản phẩm / SKU (SP-XXXXX / VD: SP-LOQ-83GS001RVN)"
        string item_name "Tên sản phẩm thương mại"
        string brand "Thương hiệu (Apple / Lenovo / Anker / Samsung...)"
        string category "Ngành hàng (Dien_thoai / Laptop / Phu_kien)"
        int warranty_months "Thời gian bảo hành chính hãng (Tháng: 12, 24...)"
        string warranty_policy_note "Ghi chú chính sách gói mở rộng (AppleCare+, Bảo hành hãng)"
        boolean is_active "Đang kinh doanh (Yes/No)"
    }

    STAFF_USER {
        string user_id PK "Tên đăng nhập / Email (user@cellphones.com.vn)"
        string full_name "Họ và tên nhân viên"
        string role "5 Vai trò chuẩn (Ban_hang_CSKH_cua_hang / Quan_ly_cua_hang / CSKH_chuoi / Quan_ly_chuoi / Quan_tri_he_thong)"
        string branch_id FK "Cửa hàng trực thuộc (Link Branch_Store)"
        boolean is_active "Đang hoạt động (Yes/No)"
    }

    CUSTOMER_MERGE_LOG {
        string merge_id PK "Mã phiên gộp (MRG-YYYY-XXXXX)"
        string source_customer_id FK "Mã hồ sơ phụ bị gộp (Link Customer)"
        string target_customer_id FK "Mã hồ sơ chính giữ lại (Link Customer)"
        string merge_reason "Lý do gộp (Xac_minh_trung_SDT / Cung_CCCD / Yeu_cau_xac_thuc)"
        int migrated_tickets_count "Số lượng Ticket đã chuyển giao"
        int migrated_interactions_count "Số lượng Tương tác đã chuyển giao"
        string executed_by FK "Quản trị viên thực hiện (Link Staff_User)"
        datetime executed_at "Thời gian thực hiện"
    }
```

---

## PHẦN 3: ĐẶC TẢ QUAN HỆ & BỘI SỐ CHI TIẾT (CARDINALITY & MULTIPLICITY)

| Cặp thực thể (Cha $\rightarrow$ Con) | Bội số (Cardinality) | Tính bắt buộc (Optionality) | Giải thích logic nghiệp vụ CRM CellphoneS |
| :--- | :---: | :---: | :--- |
| `BRANCH_STORE` $\rightarrow$ `STAFF_USER` | $1 - N$ | Mandatory $\rightarrow$ Optional | Một chi nhánh có nhiều nhân sự làm việc (Tư vấn, CSKH, Quản lý); mỗi nhân sự thuộc một chi nhánh chính. |
| `BRANCH_STORE` $\rightarrow$ `SALES_INVOICE_REFERENCE` | $1 - N$ | Mandatory $\rightarrow$ Optional | Một cửa hàng xuất nhiều hóa đơn bán hàng cho khách; hóa đơn lưu vết chi nhánh xuất hàng. |
| `BRANCH_STORE` $\rightarrow$ `SUPPORT_TICKET` | $1 - N$ | Mandatory $\rightarrow$ Optional | Một chi nhánh tiếp nhận nhiều phiếu hỗ trợ/bảo hành từ khách hàng mang máy đến. |
| `CUSTOMER` $\rightarrow$ `SMEMBER_PROFILE` | $1 - 1$ | Mandatory $\rightarrow$ Mandatory | Mỗi khách hàng cá nhân có duy nhất 1 hồ sơ Smember phục vụ tra cứu hạng thành viên và quyền lợi ưu đãi. |
| `CUSTOMER` $\rightarrow$ `SALES_INVOICE_REFERENCE` | $1 - N$ | Mandatory $\rightarrow$ Optional | Một khách hàng cá nhân có thể mua nhiều sản phẩm qua nhiều đơn hàng theo thời gian. |
| `CUSTOMER` $\rightarrow$ `SUPPORT_TICKET` | $1 - N$ | Optional $\rightarrow$ Optional | Một khách hàng có thể mở nhiều phiếu hỗ trợ. Khóa ngoại `customer_id` là Nullable để hỗ trợ Khách vãng lai. |
| `CUSTOMER` $\rightarrow$ `CUSTOMER_INTERACTION` | $1 - N$ | Optional $\rightarrow$ Optional | Một khách hàng có thể gọi Hotline hoặc Chat nhiều lần. Nullable để tiếp nhận cuộc gọi hỏi giá/tư vấn ẩn danh. |
| `CUSTOMER` $\rightarrow$ `LEAD_OPPORTUNITY` | $1 - N$ | Optional $\rightarrow$ Optional | Khách tiềm năng sau khi chốt đặt trước/tư vấn thành công sẽ được chuyển đổi liên kết với 1 bản ghi `CUSTOMER`. |
| `ITEM_REFERENCE` $\rightarrow$ `SALES_INVOICE_REFERENCE` | $1 - N$ | Mandatory $\rightarrow$ Optional | Mỗi dòng hóa đơn tham chiếu đến 1 mã SKU sản phẩm (Điện thoại/Laptop/Phụ kiện) cụ thể. |
| `ITEM_REFERENCE` $\rightarrow$ `SUPPORT_TICKET` | $1 - N$ | Mandatory $\rightarrow$ Optional | Mỗi phiếu hỗ trợ ghi nhận việc tiếp nhận yêu cầu bảo hành/đổi trả cho 1 dòng sản phẩm cụ thể. |
| `SALES_INVOICE_REFERENCE` $\rightarrow$ `SUPPORT_TICKET` | $1 - N$ | Optional $\rightarrow$ Optional | Khi tiếp nhận máy, nhân viên tra cứu đối soát hóa đơn mua cũ để kiểm tra hạn 30 ngày đổi mới hoặc bảo hành. |
| `SUPPORT_TICKET` $\rightarrow$ `TICKET_ITEM_CONDITION` | $1 - N$ | Mandatory $\rightarrow$ Optional | Một phiếu hỗ trợ có bảng con ghi nhận tình trạng ngoại quan, phụ kiện kèm theo và hiện tượng lỗi của máy. |
| `SUPPORT_TICKET` $\rightarrow$ `TICKET_ACTIVITY_LOG` | $1 - N$ | Mandatory $\rightarrow$ Optional | Một phiếu hỗ trợ lưu lại toàn bộ tiến trình cập nhật tiến độ, trao đổi nội bộ và chuyển trạng thái theo SLA. |
| `CUSTOMER_INTERACTION` $\rightarrow$ `SUPPORT_TICKET` | $1 - 1$ | Optional $\rightarrow$ Optional | Một cuộc gọi Hotline hoặc cuộc Chat phản ánh khiếu nại có thể được tạo nhanh (escalate) thành 1 Phiếu hỗ trợ. |
| `CUSTOMER` $\rightarrow$ `CUSTOMER_MERGE_LOG` | $1 - N$ | Mandatory $\rightarrow$ Optional | Một khách hàng đóng vai trò là hồ sơ nguồn (bị gộp) hoặc hồ sơ đích (giữ lại) trong nhật ký kiểm toán gộp dữ liệu. |

---

## PHẦN 4: THUYẾT MINH GIẢI PHÁP 2 BÀI TOÁN DỮ LIỆU ĐẶC THÙ CELLPHONES

### 4.1. Bài toán 1: Xử lý Khách hàng chưa xác định (Khách vãng lai / Ẩn danh)

#### Bối cảnh thực tế tại CellphoneS:
* Khách gọi lên tổng đài CSKH Hotline 1800.2097 chỉ hỏi giá iPhone mới ra mắt, cấu hình Laptop gaming hoặc tình trạng tồn kho phụ kiện tại chi nhánh gần nhất rồi ngắt máy.
* Khách nhắn tin qua Fanpage/Zalo bằng tài khoản mạng xã hội ẩn danh, không để lại số điện thoại cá nhân.
* Khách ghé trực tiếp Showroom hỏi tham khảo trải nghiệm máy mà chưa có nhu cầu để lại thông tin thành viên.

#### Giải pháp Thiết kế Cơ sở Dữ liệu:
Hệ thống CRM CellphoneS áp dụng giải pháp kết hợp **"Nullable Foreign Key & Bản ghi Sentinel Dự phòng"**:
1. **Ràng buộc trường `customer_id`:** Trong thực thể `CUSTOMER_INTERACTION` và `SUPPORT_TICKET`, khóa ngoại `customer_id` được thiết lập ở chế độ **Nullable (Không bắt buộc nhập)**. Đồng thời bổ sung 2 trường lưu tạm độc lập: `contact_phone` và `contact_name`.
2. **Bản ghi mặc định (Sentinel Record):** Hệ thống khởi tạo sẵn một bản ghi Khách hàng hệ thống:
   * `customer_id` = `CUST-GUEST`
   * `full_name` = `Khách vãng lai CellphoneS`
   * `phone_number` = `0000000000`
   * `status` = `Active`
3. **Quy trình định danh bổ sung (Late Identification Workflow):**
   * Khi khách hàng đồng ý cung cấp Số điện thoại: Nhân viên bấm nút *"Liên kết / Tạo mới Khách hàng"* trên form thao tác.
   * Hệ thống tự động truy vấn tìm `phone_number` trong bảng `CUSTOMER`:
     * **Nếu đã có trên hệ thống:** Gán lại `customer_id` của tương tác/phiếu hỗ trợ trỏ về khách hàng tìm thấy.
     * **Nếu chưa có:** Mở form tạo nhanh `CUSTOMER`, sinh mã `CUST-YYYY-XXXXX`, tự động tạo `SMEMBER_PROFILE` ở hạng khởi điểm (`Smember`) và liên kết ngược lại tương tác.

---

### 4.2. Bài toán 2: Xử lý Hồ sơ Trùng lặp (Customer Deduplication & Merge)

#### Bối cảnh thực tế tại CellphoneS:
* Khách hàng cá nhân khi mua điện thoại tại Showroom cung cấp SĐT 1, khi đặt hàng online laptop trên website lại nhập SĐT 2 nhưng trùng số CCCD hoặc Email.
* Nhân viên Showroom nhập sai chính tả họ tên hoặc gõ nhầm 1 chữ số điện thoại khi tạo nhanh, dẫn đến xuất hiện 2 hồ sơ của cùng một khách hàng.

#### Giải pháp Thiết kế Cơ sở Dữ liệu & Quy trình Gộp (Merge Strategy):
1. **Định danh duy nhất (Unique Constraints):**
   * Trường `phone_number` trong bảng `CUSTOMER` được đánh chỉ mục **UNIQUE**. Hệ thống ngăn chặn việc tạo 2 khách hàng cá nhân có cùng số điện thoại.
   * Hệ thống quét tự động tìm các cặp hồ sơ nghi trùng lặp dựa trên: `identity_card` (CCCD/CMND) hoặc `email`.
2. **Cơ chế Gộp hồ sơ (Merge Customer Operation):**
   Khi Quản trị viên (System Admin) hoặc Quản lý xác nhận 2 hồ sơ thuộc về cùng một người:
   * **Xác định Hồ sơ Chính (Target Profile):** Hồ sơ có thời gian tạo sớm hơn hoặc có hạng Smember cao hơn.
   * **Xác định Hồ sơ Phụ (Source Profile):** Hồ sơ trùng lặp cần gộp vào.
   * **Thực thi chuyển dịch dữ liệu (Foreign Key Re-pointing):**
     $$\text{UPDATE } \text{SUPPORT\_TICKET SET } customer\_id = \text{Target\_ID WHERE } customer\_id = \text{Source\_ID}$$
     $$\text{UPDATE } \text{CUSTOMER\_INTERACTION SET } customer\_id = \text{Target\_ID WHERE } customer\_id = \text{Source\_ID}$$
     $$\text{UPDATE } \text{LEAD\_OPPORTUNITY SET } converted\_customer\_id = \text{Target\_ID WHERE } converted\_customer\_id = \text{Source\_ID}$$
     $$\text{UPDATE } \text{SALES\_INVOICE\_REFERENCE SET } customer\_id = \text{Target\_ID WHERE } customer\_id = \text{Source\_ID}$$
   * **Đánh dấu lưu trữ hồ sơ phụ:** Cập nhật `Source.status = 'Merged'` và đổi số điện thoại thành `Source.phone_number + '_MERGED_' + timestamp` để giải phóng ràng buộc Unique.
   * **Ghi nhật ký kiểm toán:** Khởi tạo bản ghi trong bảng `CUSTOMER_MERGE_LOG` lưu chi tiết: Quản trị viên thực hiện, ID nguồn, ID đích, số lượng Ticket và Tương tác đã chuyển dời.

---

## PHẦN 5: TÍNH TOÀN VẸN & KHÔNG TRÙNG LẮP KHÁI NIỆM (COMPLIANCE CHECK)

1. **Chuẩn hóa bậc 3 (3NF):**
   * Không có thuộc tính lặp hoặc phụ thuộc bắc cầu trong các thực thể.
   * Thông tin hạng hội viên Smember được tách riêng (`SMEMBER_PROFILE`) giúp đảm bảo hiệu năng và giữ vai trò tra cứu quyền lợi rõ ràng.
   * Thông tin tình trạng máy tiếp nhận được tách thành bảng con `TICKET_ITEM_CONDITION` gắn với từng phiếu hỗ trợ, đảm bảo tính trực quan và đúng ranh giới tiếp nhận.
   * Toàn bộ các thực thể phân bổ đúng 3 nhóm sản phẩm (Điện thoại, Laptop, Phụ kiện) và 5 nhóm người dùng hệ thống.
2. **Toàn vẹn tham chiếu (Referential Integrity):**
   * 100% Khóa ngoại (FK) đều có ràng buộc rõ ràng với bảng cha tương ứng.
   * Áp dụng quy tắc `ON DELETE RESTRICT` đối với các bảng giao dịch (`SUPPORT_TICKET`, `CUSTOMER_INTERACTION`, `SALES_INVOICE_REFERENCE`) nhằm ngăn chặn việc vô tình xóa mất lịch sử chăm sóc và bằng chứng dịch vụ của khách hàng.
