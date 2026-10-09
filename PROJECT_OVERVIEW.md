# TÀI LIỆU TỔNG QUAN DỰ ÁN CRM CELLPHONES (PROJECT OVERVIEW)

**Dự án:** Phân hệ Quản trị Mối quan hệ Khách hàng (CRM) Chuỗi Bán lẻ CellphoneS  
**Nền tảng công nghệ:** Frappe Framework v15 (Custom App độc lập `cellphones_crm` – MariaDB, Python, JavaScript)  
**Người thực hiện:** Trần Quang Huy (MSSV: 24520706 – TV4)  
**Vai trò:** Quản trị hệ thống, Phụ trách Mô hình Dữ liệu và Thiết kế Triển khai  
**Thời điểm hoàn thiện:** 05 tháng 10 năm 2026  
**Trạng thái:** Hoàn thiện Mốc Thiết kế Kỹ thuật (M4) – Gắn nhãn Thiết kế đề xuất (Proposed Design)

---

## 1. MỤC TIÊU VÀ MÔ TẢ DỰ ÁN

### 1.1. Bối cảnh & Phạm vi nghiên cứu
Dự án tập trung xây dựng phân hệ Quản trị Mối quan hệ Khách hàng (CRM) chuyên biệt phục vụ chuỗi bán lẻ thiết bị công nghệ **CellphoneS** trong khuôn khổ môn học Quản trị mối quan hệ khách hàng.

* **Quy mô triển khai mô phỏng:** 2 cửa hàng đại diện (Chi nhánh 1: CellphoneS 136 Nguyễn Thái Học, Chi nhánh 2: CellphoneS 21A Hàng Bài) và Trụ sở chuỗi (HQ).
* **Khách hàng trọng tâm:** Khách hàng cá nhân (B2C), trong đó chú trọng phân nhóm Học sinh - Sinh viên (`S-Student`), Giáo viên (`S-Teacher`) và khách hàng cá nhân thông thường.
* **3 Nhóm sản phẩm đại diện:**
  1. *Điện thoại thông minh:* Đại diện là iPhone 15 Pro Max (ngân sách, dung lượng, màu sắc, nhu cầu thu cũ đổi mới, AppleCare+).
  2. *Laptop:* Đại diện là Lenovo LOQ 15IAX9 83GS001RVN (nhu cầu học tập, gaming, cấu hình nâng cấp RAM/SSD).
  3. *Phụ kiện:* Đại diện là Củ sạc nhanh GaN 65W (chuẩn sạc tương thích thiết bị, công suất, số cổng).
* **Ranh giới giải pháp (Scope Boundary):**
  - *Thuộc phạm vi (In-scope):* Hồ sơ khách hàng 360°, Cơ hội bán hàng & tư vấn, Nhật ký tương tác & nhắc việc chăm sóc, Phiếu hỗ trợ hậu mãi (bảo hành/đổi trả/khiếu nại), Lịch sử mua hàng tham chiếu qua CSV, Báo cáo SLA quản lý.
  - *Ngoài phạm vi (Out-of-scope):* Không thanh toán, không quản lý tồn kho thật, không hạch toán kế toán, không định giá thu cũ tự động và không kết nối API trực tiếp ra các kênh ngoài thật.

---

## 2. NỀN TẢNG PHƯƠNG PHÁP LUẬN: 5 THỰC HÀNH CỐT LÕI CHƯƠNG 2 (KARL WIEGERS)

Toàn bộ các sản phẩm phân tích và thiết kế của TV4 được chuẩn hóa theo 5 thực hành cốt lõi trong cuốn sách kinh điển *"Software Requirements Essentials: Core Practices for Successful Business Analysis"* (Karl Wiegers & Candase Hokanson):

| Thực hành cốt lõi (Core Practice) | Nội dung áp dụng vào dự án CRM CellphoneS | Kết quả thể hiện trong bộ sản phẩm |
| :--- | :--- | :--- |
| **Practice #1:** Understand the problem before converging on a solution *(Hiểu vấn đề trước khi chọn giải pháp)* | Phân tích 3 điểm nghẽn cốt lõi: thông tin phân tán, bỏ sót chăm sóc, thiếu dấu vết trách nhiệm hậu mãi. Không thiết kế các thực thể dư thừa ngoài phạm vi. | Thể hiện trong cấu trúc tinh gọn của ERD [D01](file:///d:/CRM_CellphoneS/docs/D01_ERD_ThuyetMinh.md) và Từ điển dữ liệu [D02](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu.md). |
| **Practice #2:** Define business objectives *(Xác định mục tiêu kinh doanh & chỉ tiêu thành công)* | Cấu trúc dữ liệu và trường trạng thái hỗ trợ đo lường chính xác 3 chỉ tiêu thành công: Bàn giao tư vấn $\ge 18/20$ ca; Hoàn thành chăm sóc đúng hạn $\ge 90\%$; 100% phiếu hỗ trợ có người phụ trách và kết quả. | Thể hiện qua các trường `due_date`, `resolution_result`, `customer_notified` trong [D02](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu.md) và Dashboard [D03](file:///d:/CRM_CellphoneS/docs/D03_Wireframe_GiaoDien.md#25-màn-hình-ui-05-dashboard-báo-cáo-quản-lý--giám-sát-sla-management-report--sla-tracking). |
| **Practice #3:** Define the solution's boundaries *(Xác định ranh giới giải pháp & tự động hóa)* | Phân định ranh giới dữ liệu nội bộ CRM vs Dữ liệu tham chiếu ngoài (nạp qua CSV); xác định ranh giới tự động hóa (nhân viên ghi nhận thực tế, hệ thống hỗ trợ nhắc việc/kiểm tra quyền/báo cáo). | Thể hiện qua thực thể `Purchase_History_Ref` và ranh giới tích hợp đối tác bảo hành ngoài trong [D04](file:///d:/CRM_CellphoneS/docs/D04_AnhXa_Frappe.md). |
| **Practice #4:** Identify and characterize stakeholders *(Nhận diện & phân loại bên liên quan)* | Thiết kế dữ liệu và giao diện đáp ứng nhu cầu tác nghiệp chuyên biệt cho 6 vai trò: Store Sales, Store CSKH, Store Manager, Chain CSKH, Chain Manager, System Administrator. | Thể hiện qua Ma trận phân quyền Role Permission Manager trong [D04](file:///d:/CRM_CellphoneS/docs/D04_AnhXa_Frappe.md) và Wireframe [D03](file:///d:/CRM_CellphoneS/docs/D03_Wireframe_GiaoDien.md). |
| **Practice #5:** Identify empowered decision makers *(Người ra quyết định & quy tắc quyết định)* | Toàn bộ thiết kế mang nhãn `Proposed Design`; cơ chế gộp hồ sơ trùng (Merge Rule) có kiểm soát; quy tắc quản lý thay đổi có nhật ký theo dõi. | Thể hiện qua 2 bài toán dữ liệu trong [D01](file:///d:/CRM_CellphoneS/docs/D01_ERD_ThuyetMinh.md) và Biểu đồ tuần tự kiểm tra điều kiện [D05](file:///d:/CRM_CellphoneS/docs/D05_Sequence_Diagram.md). |

---

## 3. KIẾN TRÚC HỆ THỐNG VÀ CÔNG NGHỆ TRIỂN KHAI

* **Kiến trúc ứng dụng:** Standalone Custom App `cellphones_crm` chạy trên **Frappe Framework v15**.
* **Cơ sở dữ liệu:** MariaDB v10.6+ (Cấu trúc bảng chuẩn hóa, khóa chính tự sinh theo định dạng Auto-naming).
* **Backend:** Python v3.11+, RESTful API, Frappe Document Lifecycle Hooks.
* **Frontend:** Frappe Desk UI (JavaScript, Jinja, HTML/CSS, Desk Workspaces).
* **Bảo mật & Phân quyền:**
  * *Role-based Permissions:* Ma trận 6 vai trò kiểm soát quyền Read, Write, Create, Delete, Submit, Export.
  * *Branch-level Isolation (User Permission):* Nhân viên cửa hàng chỉ truy cập dữ liệu thuộc chi nhánh mình; Quản lý chuỗi và CSKH chuỗi xem toàn bộ dữ liệu 2 cửa hàng.

---

## 4. TỔNG HỢP 6 SẢN PHẨM BÀN GIAO KỸ THUẬT (TV4)

| Mã | Tên sản phẩm bàn giao | Nội dung tóm tắt | Đường dẫn tài liệu |
| :---: | :--- | :--- | :---: |
| **D01** | **Sơ đồ ERD Logic & Thuyết minh quan hệ** | Mô hình 8 thực thể (`Branch`, `Staff_User`, `Customer`, `Opportunity`, `Interaction`, `Care_Task`, `Support_Ticket`, `Purchase_History_Ref`), bội số quan hệ và giải pháp cho 2 bài toán dữ liệu đặc thù (Khách vãng lai & Quy tắc gộp hồ sơ trùng). | [D01_ERD_ThuyetMinh.md](file:///d:/CRM_CellphoneS/docs/D01_ERD_ThuyetMinh.md) |
| **D02** | **Từ điển dữ liệu & Quy tắc kiểm tra** | Đặc tả 8 thực thể theo bảng 6 cột chuẩn kỹ thuật, chuẩn hóa bộ trạng thái nghiệp vụ, định dạng Regex SĐT VN, điều kiện đóng phiếu nghiêm ngặt và cấu trúc tệp CSV mẫu `order_history.csv`. | [D02_TuDienDuLieu.md](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu.md) |
| **D03** | **Wireframe giao diện 5 màn hình chính** | Phác thảo layout ASCII Desk v15 cho 5 màn hình tác nghiệp (Hồ sơ 360°, Chi tiết cơ hội, Tương tác & Nhắc việc, Phiếu hỗ trợ hậu mãi, Dashboard SLA) bóc tách rõ 3 phần: Xem – Nhập – Hành động & Cảnh báo lỗi. | [D03_Wireframe_GiaoDien.md](file:///d:/CRM_CellphoneS/docs/D03_Wireframe_GiaoDien.md) |
| **D04** | **Bảng ánh xạ sang Frappe Framework** | Ánh xạ 8 thực thể sang DocType chuẩn/tùy biến, thiết kế Workflow transitions cho `Support Ticket` và `Opportunity`, Ma trận phân quyền 6 vai trò và phương án nạp dữ liệu lịch sử mua hàng qua Data Import Tool. | [D04_AnhXa_Frappe.md](file:///d:/CRM_CellphoneS/docs/D04_AnhXa_Frappe.md) |
| **D05** | **Biểu đồ tuần tự (Sequence Diagram)** | 2 biểu đồ UML (Proposed Design): (1) Tiếp nhận & Điều phối phiếu hỗ trợ liên chi nhánh; (2) Thẩm định điều kiện và Đóng phiếu hỗ trợ kèm kiểm tra kết quả và cờ xác nhận đã thông báo khách hàng. | [D05_Sequence_Diagram.md](file:///d:/CRM_CellphoneS/docs/D05_Sequence_Diagram.md) |
| **D06** | **Đặc tả Sự kiện & Phản ứng hệ thống (Event–Response & State Transition)** | Danh mục 19 sự kiện MVP chuẩn Karl Wiegers (13 Business, 2 Signal, 4 Temporal), Bảng phản ứng 6 cột bóc tách hệ thống và con người, Sơ đồ trạng thái Mermaid v2, Ma trận trạng thái và đối chiếu 9 ngoại lệ EX-01 đến EX-09. | [D06_Event_Response_Specification.md](file:///d:/CRM_CellphoneS/docs/D06_Event_Response_Specification.md) |

---

## 5. MA TRẬN TRUY VẾT TỔNG THỂ (TRACEABILITY MATRIX)

| Vấn đề kinh doanh (Practice #1) | Mục tiêu & Chỉ tiêu thành công (Practice #2) | Thực thể dữ liệu (D01 & D02) | Màn hình giao diện (D03) | Triển khai Frappe (D04 & D05) |
| :--- | :--- | :--- | :--- | :--- |
| Thông tin khách hàng và nhu cầu tư vấn bị phân tán giữa các lần liên hệ | Bàn giao tiếp nối tư vấn thuận lợi ($\ge 18/20$ ca) | `Customer`, `Opportunity`, `Interaction`, `Purchase_History_Ref` | `UI-01` (Hồ sơ 360°), `UI-02` (Chi tiết cơ hội) | Custom DocType `CellphoneS Customer`, `CellphoneS Opportunity`, User Permission theo `Branch`. |
| Bỏ sót các công việc chăm sóc và lịch hẹn liên hệ lại khách hàng | Tỷ lệ công việc chăm sóc hoàn thành đúng hạn $\ge 90\%$ | `Care_Task` (`due_date`, `status`, `assigned_to_user`, `completed_at`) | `UI-03` (Quản lý tương tác & nhắc việc) | Custom DocType `Customer Care Task`, Cảnh báo đỏ việc quá hạn trên Desk Workspace. |
| Mất dấu vết trách nhiệm và tiến độ trong xử lý bảo hành, đổi trả, khiếu nại | 100% phiếu hỗ trợ có người phụ trách, tiến độ và kết quả khi đóng | `Support_Ticket` (`assigned_to_user`, `status`, `resolution_result`, `customer_notified`) | `UI-04` (Phiếu hỗ trợ), `UI-05` (Dashboard SLA) | Custom DocType `Support Ticket`, Workflow closing gates (D05), Role Permission Manager. |

---

## 6. NHẬT KÝ QUẢN TRỊ DỰ ÁN & QUYẾT ĐỊNH KỸ THUẬT (CHANGELOG)

* **2026-10-05 (Phiên bản v1.0 - Hoàn thiện Mốc M4):**
  - Tiếp nhận tài liệu nền tảng `Nền Tảng CRM.md`, yêu cầu nhiệm vụ TV4 `Nhiemvu.md` và 5 thực hành Chương 2 sách Karl Wiegers.
  - Xây dựng hoàn chỉnh sơ đồ ERD Logic 8 thực thể kèm thuyết minh và giải pháp cho 2 bài toán dữ liệu đặc thù (`docs/D01_ERD_ThuyetMinh.md`).
  - Xây dựng Từ điển dữ liệu 6 cột kỹ thuật, đồng bộ trạng thái và cấu trúc CSV `order_history.csv` (`docs/D02_TuDienDuLieu.md`).
  - Thiết kế Wireframe ASCII 5 màn hình tác nghiệp Desk v15 bóc tách Xem – Nhập – Hành động (`docs/D03_Wireframe_GiaoDien.md`).
  - Hoàn thiện Bảng ánh xạ DocType, sơ đồ Mermaid State Diagram cho cả 2 Workflow (`Support Ticket` & `CellphoneS Opportunity`) và Ma trận phân quyền 6 vai trò trên Frappe Framework v15 độc lập (`docs/D04_AnhXa_Frappe.md`).
  - Thiết kế 2 Sequence Diagram UML cho luồng điều phối liên chi nhánh và luồng kiểm tra điều kiện đóng phiếu nghiêm ngặt (`docs/D05_Sequence_Diagram.md`).
  - Phối hợp bộ kỹ năng (`taste-design`, `enhance-prompt`, `manage-design-system`, `generate-design`) triển khai sinh 5 màn hình UI High-Fidelity trên **Google Stitch** chuẩn Frappe Desk v15 (Project ID: `1163140924608807106`):
    1. `UI-01`: Hồ sơ khách hàng 360° (Customer 360 View - CUST-2026-00128)
    2. `UI-02`: Chi tiết cơ hội bán hàng & Tư vấn sản phẩm (Opportunity - OPP-2026-045)
    3. `UI-03`: Quản lý tương tác & Nhắc việc chăm sóc (Care Tasks & Quick Log)
    4. `UI-04`: Phiếu hỗ trợ dịch vụ & Phối hợp hậu mãi (Support Ticket - TICK-2026-00089)
    5. `UI-05`: Dashboard báo cáo quản trị & Giám sát SLA (3 chỉ tiêu Wiegers Practice #2)
  - Đồng bộ và tải trọn bộ file HTML & ảnh render PNG độ phân giải cao vào thư mục `.stitch/designs/`, cập nhật `.stitch/metadata.json`.
  - Khởi tạo và xuất toàn bộ 8 bảng từ điển dữ liệu chi tiết Mục 2 sang định dạng tài liệu Microsoft Word chuẩn trắng đen [D02_TuDienDuLieu_v2.docx](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu_v2.docx).
  - Tổng hợp toàn diện tài liệu quản trị `PROJECT_OVERVIEW.md`.

* **2026-10-09 (Phiên bản v1.1 - Hoàn thiện Mốc Practice #7 theo Karl Wiegers):**
  - Nghiên cứu và tiếp nhận tài liệu baseline dùng chung của nhóm (`baseline_case_study_crm_cellphones.md`) về Case tiếp nhận & theo dõi bảo hành/đổi trả tại hai chi nhánh CH01 & CH02.
  - Phân tích và tham chiếu chuyên sâu tài liệu *Software Requirements Essentials* (Karl Wiegers & Candase Hokanson, Chapter 3, Practice #7: Identify Events and Responses), kết hợp chéo với phân tích Gaps Model SERVQUAL của Võ Duy Mạnh (`nội dung của mạnh.md`) và Use Cases Practice #6 của Nguyễn Đức Nhật (`nôi dung của nhật.md`).
  - Xây dựng hoàn chỉnh tài liệu [practice7_tran_quang_huy.md](file:///d:/CRM_CellphoneS/practice7_tran_quang_huy.md) bao gồm:
    1. Định vị bối cảnh mô phỏng 2 cửa hàng, kịch bản chuẩn Happy path và nguyên tắc phân loại thông tin A/B/C.
    2. Danh mục 19 sự kiện cốt lõi trong MVP (Event List) chuẩn hóa tinh gọn: 13 Business Events (`EVT-B01` đến `EVT-B13`), 2 Signal Events phần cứng quầy (`EVT-S01` quét Barcode IMEI và `EVT-S02` quẹt thẻ quầy) và 4 Temporal Events chạy ngầm (`EVT-T01` đến `EVT-T04`). Các Webhook đối tác ngoài được tách bạch rõ sang Mức C (Future-state).
    3. Bảng phản ứng với sự kiện toàn diện (Event–Response Table) chuẩn 6 cột kỹ thuật: Event ID, Tên sự kiện & Tác nhân, Phân loại, Điều kiện/Trạng thái kích hoạt, Phản ứng kỳ vọng (tách bạch hành vi CRM và trách nhiệm con người), Dữ liệu & Nhật ký bị tác động gắn liền Frappe v15 (`docstatus = 0/1`, `frappe.throw`, `scheduler_events`).
    4. Sơ đồ máy trạng thái Mermaid (State Transition Diagram) và Ma trận chuyển trạng thái (State Transition Matrix) trên đúng 5 trạng thái chính, chuẩn hóa `wait_reason` thành sub-state của `Processing`, bổ sung 2 nhánh Self-transition điều chuyển liên cửa hàng (`EVT-B03` tại `Verifying` và `Processing`) và mô hình hóa 2 vòng lặp ngoại lệ (EX-07 quay lại xử lý, EX-08 mở lại ticket đã đóng về xác minh).
    5. Ma trận truy vết 4 chiều (Traceability Matrix) kết nối Customer Expectations / Gaps Model $\leftrightarrow$ Use Cases $\leftrightarrow$ Events & Responses $\leftrightarrow$ Dữ liệu & Cổng kiểm soát Frappe CRM.
    6. Bảng đối chiếu toàn diện 9 tình huống ngoại lệ bắt buộc (EX-01 đến EX-09).
    7. Rà soát kiểm tra đạt 100% tiêu chí Checklist baseline mục 14.
  - Đóng gói chính thức hồ sơ bàn giao kỹ thuật [D06_Event_Response_Specification.md](file:///d:/CRM_CellphoneS/docs/D06_Event_Response_Specification.md) lưu trữ trong thư mục `docs/`, phục vụ báo cáo đồ án và trình bày nhóm.
