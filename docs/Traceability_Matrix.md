# MA TRẬN ĐỐI SOÁT CHÉO TOÀN DIỆN (CROSS-CHECK TRACEABILITY MATRIX)
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã tài liệu:** TRACE-MATRIX  
**Thuộc nhiệm vụ:** Nhiệm vụ 7 (Rà soát chéo và Đảm bảo tính nhất quán hệ thống)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Đối soát cùng:** Nhật (Process & State Analyst) & Mạnh (Domain Retail Expert)  
**Trạng thái:** Hoàn tất Mốc M3 (Sẵn sàng Bàn giao Mốc M4)

---

## 1. MA TRẬN ĐỐI SOÁT 1: GIAO DIỆN (D03) $\longleftrightarrow$ TỪ ĐIỂN DỮ LIỆU (D02)

Đảm bảo 100% phần tử hiển thị/nhập liệu trên 6 Màn hình cốt lõi đều được định nghĩa trong Từ điển dữ liệu, tuyệt đối không xuất hiện "thuộc tính mồ côi".

| Màn hình (D03) | Thành phần trên Giao diện | Tên trường tương ứng trong Data Dictionary (D02) | Bảng / DocType | Kiểu dữ liệu | Kiểm tra tồn tại |
| :--- | :--- | :--- | :--- | :---: | :---: |
| **MH1: Danh sách KH** | Mã khách hàng | `customer_id` / `name` | `Customer` | Data | ✅ Khớp 100% |
| | Họ và tên khách hàng | `customer_name` | `Customer` | Data | ✅ Khớp 100% |
| | Số điện thoại | `phone_number` | `Customer` | Data | ✅ Khớp 100% |
| | Badge Hạng Smember | `member_tier` | `Smember_Profile` | Select | ✅ Khớp 100% |
| | Điểm tích lũy | `reward_points` | `Smember_Profile` | Int | ✅ Khớp 100% |
| | Tổng chi tiêu tích lũy | `total_spent` | `Smember_Profile` | Currency | ✅ Khớp 100% |
| | Trạng thái hồ sơ | `status` | `Customer` | Select | ✅ Khớp 100% |
| **MH2: Hồ sơ 360°** | Thông tin CCCD, Ngày sinh, Địa chỉ | `identity_card`, `date_of_birth`, `primary_address` | `Customer` | Data/Date/Text | ✅ Khớp 100% |
| | Tab Thiết bị & IMEI sở hữu | `serial_imei`, `item_code`, `warranty_months` | `Item_Reference` / Ticket | Data/Link | ✅ Khớp 100% |
| | Hạn duy trì hạng VIP | `tier_expiry_date`, `last_upgrade_date` | `Smember_Profile` | Date/Datetime | ✅ Khớp 100% |
| **MH3: Ghi nhận tương tác** | SĐT người gọi & Tên người gọi | `contact_phone`, `contact_name` | `Customer_Interaction` | Data | ✅ Khớp 100% |
| | Kênh tiếp xúc (Hotline, Zalo, Showroom)| `channel` | `Customer_Interaction` | Select | ✅ Khớp 100% |
| | Phân loại nhu cầu giao dịch | `interaction_type` | `Customer_Interaction` | Select | ✅ Khớp 100% |
| | Tóm tắt trao đổi | `summary` | `Customer_Interaction` | Small Text | ✅ Khớp 100% |
| | Đánh giá hài lòng CSAT tức thời | `satisfaction_rating` | `Customer_Interaction` | Select | ✅ Khớp 100% |
| **MH4: Kanban Ticket** | Thẻ Ticket & 5 Cột Kanban | `ticket_id`, `status` | `Support_Ticket` | Data / Select | ✅ Khớp 100% |
| | Tag SLA đếm ngược | `sla_deadline` | `Support_Ticket` | Datetime | ✅ Khớp 100% |
| | Nhãn ưu tiên (VIP, Critical) | `priority` | `Support_Ticket` | Select | ✅ Khớp 100% |
| | Kỹ thuật viên phụ trách | `allocated_to` | `Support_Ticket` | Link User | ✅ Khớp 100% |
| **MH5: Chi tiết Ticket** | Bảng linh kiện lỗi & Ngoại quan | `fault_component`, `initial_condition`, `accessories` | `Ticket_Repair_Item` | Select / Data | ✅ Khớp 100% |
| | Phương án giải quyết | `resolution_type` | `Support_Ticket` | Select | ✅ Khớp 100% |
| | Báo cáo nguyên nhân & Khắc phục | `root_cause`, `resolution_notes` | `Support_Ticket` | Small Text | ✅ Khớp 100% |
| | Dòng thời gian trao đổi kỹ thuật | `comments`, `action_type`, `logged_at` | `Ticket_Activity_Log` | Text / Datetime | ✅ Khớp 100% |
| **MH6: CSKH Dashboard** | Tỷ lệ SLA, CSAT, Tổng phiếu | Tổng hợp từ `sla_deadline`, `csat_score`, `creation` | `Support_Ticket` | Aggregated | ✅ Khớp 100% |
| | Top sản phẩm khiếu nại | Tổng hợp từ `item_code` | `Support_Ticket` | Aggregated | ✅ Khớp 100% |

---

## 2. MA TRẬN ĐỐI SOÁT 2: QUY TRÌNH & NÚT BẤM (D03/D04) $\longleftrightarrow$ STATE DIAGRAM CỦA NHẬT

Đảm bảo mọi bước chuyển trạng thái trong biểu đồ của Nhật đều có nút bấm kích hoạt trên giao diện và điều kiện Workflow tương ứng trên Frappe.

| Trạng thái nguồn | Sự kiện chuyển trạng thái (Nhật) | Trạng thái đích | Nút bấm thao tác trên Giao diện (D03) | Workflow Action trên Frappe (D04) | Điều kiện kiểm tra ràng buộc |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Open** | Phân công & Tiếp nhận | **In_Progress** | `[Nhận xử lý]` / `[Gán việc]` | `Assign / Start Work` | Bắt buộc đã chọn nhân viên `allocated_to`. |
| **In_Progress** | Gửi Hãng / Điều phối linh kiện | **Pending_Vendor** | `[Chuyển Hãng Apple/Samsung]` | `Send to Vendor` | Bắt buộc có dữ liệu trong bảng `Ticket_Repair_Item`. |
| **Pending_Vendor**| Nhận linh kiện / Nhận máy trả về | **In_Progress** | `[Linh kiện đã về / Nhận lại máy]`| `Receive from Vendor` | Ghi nhận thời điểm nhận máy tại trung tâm DTV. |
| **In_Progress** | Kỹ thuật hoàn tất sửa/đổi | **Resolved** | `[Hoàn tất / Resolve]` | `Mark Resolved` | Bắt buộc nhập `resolution_type` và `root_cause`. |
| **Resolved** | Khách nhận máy & Ký biên bản | **Closed** | `[Bàn giao & Đóng phiếu]` | `Customer Accept & Close` | Bắt buộc nhập `resolution_notes`. Kích hoạt Zalo ZNS. |
| **Open / In_Prog**| Khách hủy yêu cầu / Sai sót | **Closed** | `[Hủy phiếu hỗ trợ]` | `Cancel Ticket` | Chỉ Quản lý (CSKH Manager), bắt buộc nhập lý do. |

---

## 3. MA TRẬN ĐỐI SOÁT 3: PHÂN QUYỀN (D04) $\longleftrightarrow$ VAI TRÒ THỰC TẾ TRONG USE CASE

| Vai trò nghiệp vụ | Quyền trên Khách hàng (`Customer`) | Quyền trên Phiếu (`Support Ticket`) | Quyền trên Smember Profile | Quyền trên Nhật ký Tương tác | Đánh giá độ phủ Use Case |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **CSKH Agent (Hotline/Online)** | Đọc, Tạo, Sửa | Đọc, Tạo, Sửa (Tiếp nhận & Đóng phiếu) | Chỉ đọc | Toàn quyền tạo và ghi nhận | ✅ Bao phủ 100% |
| **CSKH Manager (Trưởng phòng)** | Toàn quyền + Gộp hồ sơ (Merge) | Toàn quyền + Duyệt đổi mới 1-1 | Toàn quyền + Điều chỉnh điểm | Toàn quyền giám sát CSAT | ✅ Bao phủ 100% |
| **Kỹ thuật viên Điện Thoại Vui** | Chỉ đọc thông tin liên hệ | Sửa kết quả kiểm định & Bảng linh kiện | Chỉ đọc (Nhận diện VIP) | Xem lịch sử trao đổi | ✅ Bao phủ 100% |
| **Nhân viên Showroom CellphoneS**| Đọc, Tạo nhanh lúc bán | Tạo phiếu tiếp nhận bảo hành tại quầy | Chỉ đọc (Chiết khấu) | Ghi nhận khách ghé quầy | ✅ Bao phủ 100% |

---

## 4. MA TRẬN ĐỐI SOÁT 4: NGHIỆP VỤ BÁN LẺ THỰC TẾ (PHẢN BIỆN CÙNG MẠNH)

| Điểm nghiệp vụ đặc thù CellphoneS | Yêu cầu thực tế (Mạnh đề xuất) | Giải pháp thiết kế trong hệ thống CRM | Vị trí tài liệu |
| :--- | :--- | :--- | :--- |
| **Chính sách Hội viên Smember** | Phân cấp rõ rệt giữa khách thường và S-VIP; tích lũy chi tiêu đa kênh. | Thực thể riêng `Smember_Profile` quan hệ 1-1, tự động tính tổng chi tiêu lũy kế và cấp hạn duy trì. | [D01_ERD_ThuyetMinh.md](file:///d:/CRM_CellphoneS/docs/D01_ERD_ThuyetMinh.md) & [D02_TuDienDuLieu.md](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu.md) |
| **Cam kết Thời gian SLA** | Khách S-VIP cam kết xử lý trong 4h; khách thường 24h - 48h. | Tự động tính toán `sla_deadline` khi mở phiếu; gắn thẻ đồng hồ đếm ngược trên Kanban và cảnh báo trễ hạn. | [D02_TuDienDuLieu.md](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu.md) & [D03_Wireframe_GiaoDien.md](file:///d:/CRM_CellphoneS/docs/D03_Wireframe_GiaoDien.md) |
| **Tra cứu Số Serial / IMEI** | Chuẩn 15 số quốc tế, kiểm tra chính xác để xác thực bảo hành Apple Care/Hãng. | Áp dụng Regex 15 số, thuật toán Luhn, tích hợp nút tra cứu nhanh trạng thái bảo hành và khóa iCloud. | [D02_TuDienDuLieu.md](file:///d:/CRM_CellphoneS/docs/D02_TuDienDuLieu.md) & [D05_Sequence_Diagram.md](file:///d:/CRM_CellphoneS/docs/D05_Sequence_Diagram.md) |
| **Hệ sinh thái Điện Thoại Vui** | Phối hợp sửa chữa phần cứng chuyên sâu giữa cửa hàng bán lẻ và trung tâm DTV. | Thiết kế Bảng con `Ticket_Repair_Item` và `Ticket_Activity_Log` hỗ trợ phân quyền riêng cho KTV DTV. | [D03_Wireframe_GiaoDien.md](file:///d:/CRM_CellphoneS/docs/D03_Wireframe_GiaoDien.md) & [D04_AnhXa_Frappe.md](file:///d:/CRM_CellphoneS/docs/D04_AnhXa_Frappe.md) |
| **Đổi trả 1-đổi-1 30 ngày VIP** | Đổi thân máy mới nguyên seal tại quầy nếu lỗi nhà sản xuất trong tháng đầu. | Sequence Diagram Luồng 2 đặc tả chi tiết kiểm định linh kiện, duyệt đổi máy và thu hồi IMEI lỗi. | [D05_Sequence_Diagram.md](file:///d:/CRM_CellphoneS/docs/D05_Sequence_Diagram.md) |

---

## KẾT LUẬN & ĐÁNH GIÁ MỐC M3
Hệ thống tài liệu thiết kế (D01 – D05) đạt mức độ nhất quán **100%**, đáp ứng toàn bộ các tiêu chí kỹ thuật và nghiệp vụ đề ra trong `Nhiemvu.md`, sẵn sàng bước vào mốc thẩm định chính thức **Mốc M4**.
