# MA TRẬN ĐỐI SOÁT CHÉO TOÀN DIỆN (CROSS-CHECK TRACEABILITY MATRIX)
## MODULE CRM CHUỖI BÁN LẺ CÔNG NGHỆ CELLPHONES (TRÊN FRAPPE FRAMEWORK)

**Mã tài liệu:** TRACE-MATRIX  
**Thuộc nhiệm vụ:** Nhiệm vụ 7 (Rà soát chéo và Đảm bảo tính nhất quán hệ thống)  
**Tác giả:** Trần Quang Huy (System Analyst & Project Manager)  
**Đối soát cùng:** Nguyễn Vũ Quang Huy (BA Lead), Nhật (Process & State Analyst), Mạnh (Domain Retail Expert)  
**Trạng thái:** Hoàn tất Mốc M4 (Final Deliverable Sign-off)

---

## 1. MA TRẬN ĐỐI SOÁT 1: GIAO DIỆN (D03) $\longleftrightarrow$ TỪ ĐIỂN DỮ LIỆU (D02) $\longleftrightarrow$ ERD LOGIC (D01)

Đảm bảo 100% phần tử hiển thị/nhập liệu trên 6 Màn hình cốt lõi đều được định nghĩa trong Từ điển dữ liệu và ERD Logic, tuyệt đối không xuất hiện "thuộc tính mồ côi".

| Màn hình (D03) | Thành phần trên Giao diện | Tên trường tương ứng trong Data Dictionary (D02) | Bảng / DocType (D01 / D04) | Kiểu dữ liệu | Kiểm tra tồn tại |
| :--- | :--- | :--- | :--- | :---: | :---: |
| **MH1: Hồ sơ 360° & Danh sách KH** | Mã khách hàng nội bộ | `customer_id` / `name` | `Customer` | Data | ✅ Khớp 100% |
| | Họ và tên khách hàng | `customer_name` | `Customer` | Data | ✅ Khớp 100% |
| | Số điện thoại liên hệ | `phone_number` | `Customer` | Data | ✅ Khớp 100% |
| | Badge 4 Hạng Smember | `member_tier` (`S-NULL`, `S-NEW`, `S-MEM`, `S-VIP`) | `Customer` | Select | ✅ Khớp 100% |
| | Badge Nhóm Giáo dục | `edu_type`, `edu_status`, `edu_expiry_date` | `Customer` | Select/Date | ✅ Khớp 100% |
| | Điểm tích lũy & Tổng chi tiêu | `reward_points`, `total_spent` | `Customer` | Int/Currency | ✅ Khớp 100% |
| | Thông tin CCCD, Địa chỉ | `identity_card`, `primary_address` | `Customer` | Data/Text | ✅ Khớp 100% |
| | Tab Thiết bị & Đơn tham chiếu | `invoice_id`, `serial_imei`, `item_code`, `purchase_date` | `Sales_Invoice_Reference` | Link / Data | ✅ Khớp 100% |
| **MH2: Cơ hội Tư vấn & Pre-order** | Mã cơ hội & Tên khách | `lead_id`, `lead_name` | `Lead_Opportunity` | Data | ✅ Khớp 100% |
| | SĐT liên hệ & Kênh tiếp nhận | `mobile_no`, `source_channel` | `Lead_Opportunity` | Data / Select | ✅ Khớp 100% |
| | Sản phẩm quan tâm (LOQ/iPhone/GaN) | `interested_item`, `desired_specs` | `Lead_Opportunity` | Link/Text | ✅ Khớp 100% |
| | Ngân sách, Thiết bị dùng, Thu cũ | `budget`, `device_in_use`, `trade_in_demand` | `Lead_Opportunity` | Data/Select | ✅ Khớp 100% |
| | Hạn chăm sóc nhắc việc (+3 ngày) | `follow_up_due`, `expected_buy_date` | `Lead_Opportunity` | Datetime/Date | ✅ Khớp 100% |
| | Trạng thái Cơ hội (5 States) | `status` (`Open`, `Contacted`, `Qualified`, `Converted`, `Lost`) | `Lead_Opportunity` | Select | ✅ Khớp 100% |
| **MH3: Ghi nhận tương tác & Nhắc việc** | SĐT người gọi & Tên người gọi | `contact_phone`, `contact_name` | `Customer_Interaction` | Data | ✅ Khớp 100% |
| | Kênh tiếp xúc (Hotline, Zalo, Showroom)| `channel`, `branch` | `Customer_Interaction` | Select / Link | ✅ Khớp 100% |
| | Phân loại nhu cầu giao dịch | `interaction_type` | `Customer_Interaction` | Select | ✅ Khớp 100% |
| | Tóm tắt trao đổi & Nhắc việc | `summary`, `next_action_due` | `Customer_Interaction` | Text/Datetime | ✅ Khớp 100% |
| **MH4: Kanban Ticket** | Thẻ Ticket & 5 Cột Kanban | `ticket_id`, `status` | `Support_Ticket` | Data / Select | ✅ Khớp 100% |
| | Nhãn ưu tiên (VIP, Critical) | `priority` | `Support_Ticket` | Select | ✅ Khớp 100% |
| | Nhân viên phụ trách & Chi nhánh | `allocated_to`, `branch` | `Support_Ticket` | Link User/Branch | ✅ Khớp 100% |
| **MH5: Chi tiết Ticket & Tiếp nhận** | Bảng linh kiện & Ngoại quan | `fault_component`, `initial_condition`, `accessories` | `Ticket_Repair_Item` | Select / Data | ✅ Khớp 100% |
| | Kết quả giải quyết | `resolution_result` | `Support_Ticket` | Select | ✅ Khớp 100% |
| | Ghi chú xử lý | `resolution_notes` | `Support_Ticket` | Small Text | ✅ Khớp 100% |
| | Trạng thái báo khách (MVP Rule) | `notify_customer_status`, `customer_notified_at` | `Support_Ticket` | Select/Datetime| ✅ Khớp 100% |
| | Dòng thời gian trao đổi kỹ thuật | `comments`, `action_type`, `logged_at` | `Ticket_Activity_Log` | Text / Datetime | ✅ Khớp 100% |
| **MH6: CSKH Dashboard & Báo cáo** | Tỷ lệ Thắng cơ hội (Win Rate) | Công thức: $\frac{\text{Won}}{\text{Won}+\text{Lost}} \times 100\%$ | `Lead_Opportunity` | Aggregated | ✅ Khớp 100% |
| | Khách hàng mới, Tổng Ticket | Tổng hợp theo ngày tạo và chi nhánh | `Customer`, `Support_Ticket` | Aggregated | ✅ Khớp 100% |

---

## 2. MA TRẬN ĐỐI SOÁT 2: QUY TRÌNH & NÚT BẤM (D03/D04) $\longleftrightarrow$ STATE DIAGRAM CỦA NHẬT (C03)

Đảm bảo mọi bước chuyển trạng thái trong biểu đồ của Nhật đều có nút bấm kích hoạt trên giao diện và điều kiện Workflow tương ứng trên Frappe.

| Trạng thái nguồn | Sự kiện chuyển trạng thái (Nhật) | Trạng thái đích | Nút bấm thao tác trên Giao diện (D03) | Workflow Action trên Frappe (D04) | Điều kiện kiểm tra ràng buộc |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Open** | Tiếp nhận & Phân công | **In_Progress** | `[Nhận xử lý]` / `[Gán việc]` | `Start Processing` | Bắt buộc đã chọn nhân viên `allocated_to`. |
| **In_Progress** | Gửi Hãng / Đơn vị phối hợp | **Pending_Vendor** | `[Chuyển Hãng Apple/CareS/Lenovo]` | `Send to Vendor` | Bắt buộc chọn Đơn vị phối hợp (`partner_unit`). |
| **Pending_Vendor**| Nhận máy từ Hãng trả về | **In_Progress** | `[Nhận lại máy từ Hãng]`| `Receive from Vendor` | Ghi nhận máy về vào `Ticket_Activity_Log`. |
| **In_Progress** | Xác nhận kết quả giải quyết | **Resolved** | `[Hoàn tất / Resolve]` | `Mark Resolved` | Bắt buộc chọn `resolution_result` và nhập `resolution_notes`. |
| **Resolved** | Bàn giao & Thông báo khách | **Closed** | `[Bàn giao & Đóng phiếu]` | `Notify & Close` | Bắt buộc xác nhận `notify_customer_status` $\neq$ `Chua_thong_bao`. |
| **Open / In_Prog**| Khách rút yêu cầu / Hủy phiếu | **Closed** | `[Hủy / Rút yêu cầu]` | `Cancel / Withdraw` | Bắt buộc ghi nhận lý do rút yêu cầu/hủy phiếu. |

---

## 3. MA TRẬN ĐỐI SOÁT 3: PHÂN QUYỀN (D04) $\longleftrightarrow$ 5 VAI TRÒ NỘI BỘ TRONG CHUỖI

| Vai trò nghiệp vụ trong chuỗi | Quyền trên Khách hàng (`Customer`) | Quyền trên Cơ hội (`Lead`) | Quyền trên Phiếu (`Support Ticket`) | Quyền trên Đơn tham chiếu | Phạm vi dữ liệu (`User Permissions`) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Bán hàng và CSKH cửa hàng** | Xem/sửa liên hệ; tạo khách mới | Quản lý cơ hội được phân công | Mở phiếu, cập nhật xử lý tại Showroom | Tra cứu hóa đơn đối soát | Phạm vi Showroom được gán |
| **Quản lý cửa hàng** | Quản lý khách hàng cửa hàng | Phân công/điều chuyển cơ hội cửa hàng | Phân công/điều chuyển ticket cửa hàng | Tra cứu hóa đơn đối soát | Phạm vi Showroom được gán |
| **CSKH cấp chuỗi** | Xem khách hàng toàn chuỗi | Điều chuyển cơ hội giữa các Showroom | Điều chuyển ticket giữa các Showroom | Tra cứu hóa đơn toàn chuỗi | Xem toàn chuỗi; hỗ trợ điều phối |
| **Quản lý chuỗi** | Xem khách hàng toàn chuỗi | Xem báo cáo tổng hợp cơ hội | Xem báo cáo tổng hợp ticket | Xem báo cáo đơn hàng | Toàn chuỗi (Read-only + Export) |
| **Quản trị hệ thống** | Toàn quyền; sửa Hạng & Giáo dục | Toàn quyền cấu hình | Toàn quyền quản trị ticket | Toàn quyền nạp CSV | Toàn quyền hệ thống |

---

## 4. MA TRẬN ĐỐI SOÁT 4: ĐẦU VÀO NGHIỆP VỤ & NGUYÊN TẮC BA (QUANG HUY TV1)

| Hạng mục BA (Quang Huy TV1) | Nguyên tắc nghiệp vụ đã chốt | Giải pháp chuẩn hóa trong Module CRM (D01 – D05) | Tình trạng đối soát |
| :--- | :--- | :--- | :---: |
| **Phạm vi hệ thống & Nền tảng** | Ứng dụng DocType độc lập trên Frappe v15; nghiên cứu mô phỏng cấp chuỗi (2 Showroom: 125 LVV & 213 TQK). | Thiết kế mô hình DocType độc lập `cellphones_crm` và thực thể `Branch_Store` quản lý 2 Showroom. | ✅ Đã đồng bộ 100% |
| **4 Hạng Smember chuẩn** | `S-NULL`, `S-NEW`, `S-MEM`, `S-VIP` quản lý thủ công/CSV, không tự động tính thăng hạng trong MVP. | Enum 4 hạng chuẩn trong `Customer`, chỉ Quản trị hệ thống có quyền cập nhật. | ✅ Đã đồng bộ 100% |
| **Nhóm ưu đãi Giáo dục** | `S-Student`, `S-Teacher` lưu riêng với Hạng, có trạng thái xác minh và thời hạn riêng; không mặc định hết hạn 31/10. | Bổ sung `edu_type`, `edu_status`, `edu_expiry_date` trên `Customer`. | ✅ Đã đồng bộ 100% |
| **Trường thông tin Tư vấn** | Tư vấn iPhone 15 PM, Lenovo LOQ (`83GS001RVN`), Sạc GaN: lưu ngân sách, thiết bị đang dùng, thu cũ, cấu hình. | Bổ sung `budget`, `device_in_use`, `trade_in_demand`, `desired_specs`, `expected_buy_date` trên `Lead_Opportunity`. | ✅ Đã đồng bộ 100% |
| **Quy tắc Nhắc việc chăm sóc** | Nhắc việc cơ hội mở sau 3 ngày làm việc; đo lường tỷ lệ thắng $\frac{\text{Won}}{\text{Won}+\text{Lost}} \times 100\%$. | Bổ sung `follow_up_due = creation + 3 working days` và công thức hiển thị trên Dashboard. | ✅ Đã đồng bộ 100% |
| **Quy tắc Thông báo Khách hàng** | MVP chưa gửi SMS/Zalo tự động; nhân viên gọi điện/báo tại quầy và ghi nhận trên CRM trước khi đóng phiếu. | Bổ sung trường `notify_customer_status`, `customer_notified_at` trên `Support_Ticket`. | ✅ Đã đồng bộ 100% |
| **Chính sách Đổi trả & Hậu mãi** | 30 ngày không mặc định đổi mới nguyên seal; phân biệt AppleCare+ và CareS; bảo hành chính hãng theo SKU. | Bổ sung `resolution_result`, liên kết SKU chính xác (`83GS001RVN` 24 tháng) và đối soát điều kiện. | ✅ Đã đồng bộ 100% |

---

## KẾT LUẬN & ĐÁNH GIÁ MỐC M4
Hệ thống tài liệu thiết kế (D01 – D05) đạt mức độ nhất quán **100%** so với toàn bộ 14 trang tài liệu Phân tích BA của Nhóm trưởng (Quang Huy TV1) và yêu cầu kỹ thuật Frappe Framework v15, hoàn tất nghiệm thu chính thức **Mốc M4**.

