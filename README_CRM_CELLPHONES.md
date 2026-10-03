# DỰ ÁN: PHÂN TÍCH & THIẾT KẾ MODULE QUẢN TRỊ QUAN HỆ KHÁCH HÀNG (CRM)
# CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES TRÊN NỀN TẢNG FRAPPE FRAMEWORK

**Tác giả / Vai trò:** Chuyên viên Phân tích Thiết kế Hệ thống (System Analyst) & Quản lý Dự án (Project Manager)  
**Chủ nhiệm dự án (User):** Trần Quang Huy  
**Nền tảng kỹ thuật:** Frappe Framework v15.x / ERPNext v15.x  
**Cơ sở dữ liệu:** MariaDB 10.6+ / PostgreSQL 15+  
**Tài liệu tham chiếu:** [Nhiemvu.md](file:///d:/CRM_CellphoneS/Nhiemvu.md)

---

## 1. TỔNG QUAN DỰ ÁN (PROJECT OVERVIEW)
Dự án nhằm phân tích, mô hình hóa và thiết kế giải pháp phần mềm quản trị quan hệ khách hàng (CRM) chuyên biệt cho chuỗi bán lẻ công nghệ **CellphoneS** (hơn 100 cửa hàng trên toàn quốc, hệ thống website/app và chuỗi trung tâm sửa chữa - bảo hành **Điện Thoại Vui**).

### 1.1. Bối cảnh Nghiệp vụ Cốt lõi
* **Bán lẻ đa kênh (Omnichannel):** Tiếp nhận khách hàng từ Hotline tổng đài 1800.2097, Fanpage/Zalo Official Account, Website trực tuyến, và trực tiếp tại chuỗi Showroom CellphoneS.
* **Hệ thống Hội viên Smember:** Quản lý vòng đời khách hàng, chính sách tích lũy chi tiêu, nâng hạng hội viên (Smember Standard, Smember VIP - S-VIP), và áp dụng đặc quyền ưu đãi dịch vụ/SLA.
* **Tiếp nhận Khiếu nại & Bảo hành/Sửa chữa (Điện Thoại Vui):** Quản lý phiếu hỗ trợ (Ticket), kiểm tra số Serial/IMEI theo chuẩn GSMA/Luhn, luân chuyển bảo hành nội bộ hoặc hãng thứ ba (Apple Care, Samsung Service Center), cam kết SLA xử lý theo từng phân hạng khách hàng.
* **Chăm sóc Khách hàng Chủ động & Đánh giá Dịch vụ (CSAT):** Tự động gửi tin nhắn Zalo ZNS / SMS mời khách hàng đánh giá sau khi hoàn tất hỗ trợ; tự động phân luồng telesales chăm sóc các khách đặt trước (Pre-order) dòng sản phẩm chiến lược (iPhone, Galaxy S series).

---

## 2. CƠ CHẾ PHỐI HỢP & PHÂN VAI (ROLES & COLLABORATION)

| Vai trò | Phụ trách chính | Trách nhiệm then chốt |
| :--- | :--- | :--- |
| **System Analyst & Project Manager** | **Trần Quang Huy** | Chủ trì phân tích dữ liệu, thiết kế ERD, từ điển dữ liệu, wireframe, kiến trúc DocType Frappe, sequence diagrams, lập kế hoạch WBS và kiểm soát chất lượng bàn giao. |
| **Business Flow & Process Analyst** | **Nhật** | Xây dựng tài liệu Use Case, Biểu đồ luồng trạng thái (State Diagram C03) của Ticket và Lead. |
| **Domain & Retail Business Expert** | **Mạnh** | Cung cấp và thẩm định tính thực tế của các quy trình nghiệp vụ CellphoneS (Smember, IMEI, SLA, quy trình bảo hành Điện Thoại Vui). |

---

## 3. KIẾN TRÚC HỆ THỐNG TRÊN NỀN TẢNG FRAPPE FRAMEWORK

```
+-------------------------------------------------------------------------+
|                       TẦNG GIAO DIỆN (UI LAYER)                        |
|  [Desk UI / Form View]   [Kanban Board Ticket]   [CSKH Dashboard Charts]|
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                  TẦNG ĐIỀU KHIỂN & NGHIỆP VỤ (FRAPPE CORE)              |
|  - Workflow Engine (5 Trạng thái Ticket & Phân quyền Transition Rules)   |
|  - Role Permission Manager (Agent, Manager, Kỹ thuật Điện Thoại Vui)     |
|  - Server Scripts / Hooks (Auto-routing SLA, Pre-order to Order, Dedupe) |
|  - Notification Engine (Email, SMS API, Zalo ZNS)                       |
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                    TẦNG MÔ HÌNH DỮ LIỆU (DOCTYPE / DB)                  |
|  [Standard DocTypes]: Customer, Item, User, Lead, Branch                |
|  [Custom DocTypes]: Support Ticket, Smember Profile, Interaction...     |
|  [Child Tables]: Ticket Repair Item, Ticket Activity Log                |
|  [Database]: MariaDB / PostgreSQL                                       |
+-------------------------------------------------------------------------+
```

---

## 4. DANH MỤC SẢN PHẨM BÀN GIAO (DELIVERABLES MAP)

Tất cả các tài liệu được hoàn thiện chi tiết trong thư mục `docs/`:

* **[D01 - ERD & Thuyết minh Quan hệ](file:///d:/CRM_CellphoneS/docs/D01_ERD_ThuyetMinh.md):** Danh mục thực thể (Chi nhánh, Khách hàng, Smember, Lead, Tương tác, Ticket, Dữ liệu mua hàng, Sản phẩm, Nhân sự, Merge log), sơ đồ Mermaid ERD chuẩn hóa, giải pháp Khách vãng lai & Deduplication/Merge hồ sơ.
* **[D02 - Từ điển Dữ liệu & Validation Rules](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu.md):** Đặc tả 8 thuộc tính chuẩn cho 100% trường dữ liệu, biểu thức chính quy (Regex), thuật toán Luhn IMEI, ràng buộc SLA, đồng bộ State Diagram C03, bảng dữ liệu mẫu và quy định cột CSV cần nhập.
* **[D03 - Wireframe & Layout Màn hình Ưu tiên](file:///d:/CRM_CellphoneS/docs/D03_Wireframe_GiaoDien.md):** Wireframe 6 màn hình cốt lõi bóc tách 3 khối (Xem - Nhập - Hành động) kèm danh sách thông báo lỗi quan trọng.
* **[D04 - Bảng Ánh xạ Kỹ thuật sang Frappe Framework](file:///d:/CRM_CellphoneS/docs/D04_AnhXa_Frappe.md):** Ánh xạ DocType chuẩn/custom, Child Tables, cấu hình Workflow 5 trạng thái, ma trận phân quyền Role Permission, phối hợp A03 về hệ sinh thái ngoài và phương án mô phỏng.
* **[D05 - Sequence Diagrams Chọn lọc](file:///d:/CRM_CellphoneS/docs/D05_Sequence_Diagram.md):** 2 biểu đồ tuần tự UML: (1) Tạo và tự động phân công phiếu hỗ trợ theo điều kiện SLA/Smember; (2) Nghiệm thu kỹ thuật và đóng phiếu đổi mới 1-1 S-VIP kèm khảo sát CSAT qua Zalo ZNS.
* **[Ma trận Đối soát Chéo (Cross-check Traceability Matrix)](file:///d:/CRM_CellphoneS/docs/Traceability_Matrix.md):** Ma trận kiểm tra tính nhất quán 100% giữa Giao diện - Dữ liệu - Quy trình - Phân quyền.

---

## 5. NHẬT KÝ CÔNG VIỆC & TIẾN ĐỘ THỰC HIỆN (WORK LOG)

- [x] **2026-10-02:** Tiếp nhận yêu cầu dự án từ `Nhiemvu.md`, phỏng vấn làm rõ các quyết định thiết kế (Grill-me) cùng anh Trần Quang Huy.
- [x] **2026-10-02:** Lập kế hoạch thực thi chi tiết Action Plan & WBS, thống nhất cấu trúc tài liệu bàn giao.
- [x] **2026-10-02:** Khởi tạo tài liệu kiến trúc tổng quan hệ thống `README_CRM_CELLPHONES.md`.
- [x] **2026-10-02:** Hoàn thiện sản phẩm D01: Danh mục thực thể, ERD Logic & Thuyết minh 2 bài toán dữ liệu.
- [x] **2026-10-02:** Hoàn thiện sản phẩm D02: Từ điển dữ liệu chuẩn 8 cột & Validation Rules, đồng bộ C03, dữ liệu mẫu và quy định cột CSV.
- [x] **2026-10-02:** Hoàn thiện sản phẩm D03: Wireframe 6 màn hình ưu tiên (Xem - Nhập - Actions) kèm thông báo lỗi quan trọng.
- [x] **2026-10-02:** Hoàn thiện sản phẩm D04: Bảng ánh xạ DocType, Workflow, Role Permissions, phối hợp A03 và phương án mô phỏng.
- [x] **2026-10-02:** Hoàn thiện sản phẩm D05: Sequence Diagram (Tạo/phân công phiếu & Nghiệm thu/đóng phiếu CSAT có điều kiện).
- [x] **2026-10-02:** Lập Ma trận đối soát chéo (Traceability Matrix) chuẩn bị cho Mốc M3 và hoàn tất Mốc M4.
- [x] **2026-10-02:** Chốt bàn giao chính thức Mốc M4 (Final Sign-off).
- [x] **2026-10-03:** Rà soát và cập nhật chuẩn hóa tài liệu [D01_ERD_ThuyetMinh.md](file:///d:/CRM_CellphoneS/docs/D01_ERD_ThuyetMinh.md) bám sát các ranh giới nghiệp vụ: Khách hàng B2C cá nhân, 3 ngành hàng (Điện thoại, Laptop, Phụ kiện), 5 nhóm người dùng CRM, 5 nghiệp vụ chính, giới hạn hậu mãi (tiếp nhận & tiến độ, không quản lý sửa chữa/hoàn tiền) và giới hạn Smember (lưu trữ/tra cứu hạng, chưa tự tính điểm xét hạng).
