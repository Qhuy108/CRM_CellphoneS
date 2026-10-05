# Phân Hệ Quản Trị Mối Quan Hệ Khách Hàng (CRM) Chuỗi Bán Lẻ CellphoneS

Dự án phân tích nghiệp vụ và thiết kế triển khai hệ thống CRM chuyên biệt cho chuỗi bán lẻ công nghệ **CellphoneS** trên nền tảng **Frappe Framework v15** (Custom App độc lập `cellphones_crm`).

## Thành viên thực hiện
* **Người thực hiện:** Trần Quang Huy (MSSV: 24520706 – TV4)
* **Vai trò:** Quản trị hệ thống, Phụ trách Mô hình Dữ liệu và Thiết kế Triển khai
* **Môn học:** Quản trị mối quan hệ khách hàng (CRM)

---

## Danh mục tài liệu và sản phẩm bàn giao (Deliverables)

| Mã | Tên sản phẩm | Mô tả tóm tắt | Liên kết |
| :---: | :--- | :--- | :---: |
| **Tổng quan** | **Tài liệu quản trị dự án** | Tổng quan mục tiêu, kiến trúc công nghệ, 5 thực hành Chương 2 và Ma trận truy vết. | [PROJECT_OVERVIEW.md](PROJECT_OVERVIEW.md) |
| **D01** | **Sơ đồ ERD Logic & Thuyết minh** | ERD 8 thực thể, phân tích bội số, giải pháp khách vãng lai và quy tắc gộp hồ sơ trùng. | [docs/D01_ERD_ThuyetMinh.md](docs/D01_ERD_ThuyetMinh.md) |
| **D02** | **Từ điển dữ liệu & Quy tắc kiểm tra** | Đặc tả 8 thực thể theo bảng 6 cột chuẩn kỹ thuật, đồng bộ trạng thái và cấu trúc file CSV mẫu. | [docs/D02_TuDienDuLieu.md](docs/D02_TuDienDuLieu.md) |
| **D03** | **Wireframe giao diện 5 màn hình** | Phác thảo layout ASCII Desk v15 bóc tách 3 phần: Xem – Nhập – Hành động & Cảnh báo lỗi. | [docs/D03_Wireframe_GiaoDien.md](docs/D03_Wireframe_GiaoDien.md) |
| **D04** | **Bảng ánh xạ sang Frappe Framework** | Kiến trúc Standalone Custom App, ánh xạ DocType, Workflow và Ma trận phân quyền 6 vai trò. | [docs/D04_AnhXa_Frappe.md](docs/D04_AnhXa_Frappe.md) |
| **D05** | **Biểu đồ tuần tự (Sequence Diagram)** | 2 biểu đồ UML: Luồng điều phối liên chi nhánh và Luồng thẩm định đóng phiếu nghiêm ngặt. | [docs/D05_Sequence_Diagram.md](docs/D05_Sequence_Diagram.md) |

---

## Nền tảng phương pháp luận
Hệ thống tuân thủ 5 thực hành cốt lõi trong Chương 2 (*Laying the Foundation*) cuốn sách **"Software Requirements Essentials: Core Practices for Successful Business Analysis"** (Karl Wiegers, Candase Hokanson):
1. **Practice #1:** Understand the problem before converging on a solution.
2. **Practice #2:** Define business objectives.
3. **Practice #3:** Define the solution's boundaries.
4. **Practice #4:** Identify and characterize stakeholders.
5. **Practice #5:** Identify empowered decision makers.
