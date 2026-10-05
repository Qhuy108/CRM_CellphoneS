# SẢN PHẨM BÀN GIAO D03: PHÁC THẢO WIREFRAME GIAO DIỆN HỆ THỐNG
## HỆ THỐNG CRM CHUỖI BÁN LẺ CELLPHONES (TRÊN NỀN TẢNG FRAPPE FRAMEWORK)

**Mã sản phẩm:** D03  
**Tên sản phẩm:** Phác thảo Wireframe các màn hình ưu tiên (UI Specifications)  
**Người thực hiện:** Trần Quang Huy (MSSV: 24520706 – TV4)  
**Vai trò:** Quản trị hệ thống, Phụ trách Mô hình Dữ liệu và Thiết kế Triển khai  
**Nền tảng mục tiêu:** Frappe Framework v15 (Custom App độc lập `cellphones_crm`)  
**Tài liệu cơ sở:**
- `Nền Tảng CRM.md` (Nền tảng phân tích hệ thống CRM CellphoneS)
- `Nhiemvu.md` (Phân công nhiệm vụ và yêu cầu chuẩn đầu ra TV4)
- `docs/D01_ERD_ThuyetMinh.md` & `docs/D02_TuDienDuLieu.md` (Mô hình ERD và Từ điển dữ liệu)  
**Trạng thái:** Hoàn thiện (Thiết kế đề xuất – Proposed Design)

---

## 1. NGUYÊN TẮC THIẾT KẾ WIREFRAME

1. **Chuẩn hóa bố cục Frappe Desk v15:**
   * Mỗi màn hình tuân thủ cấu trúc giao diện chuẩn của Frappe Framework: Thanh điều hướng (Navbar) $\rightarrow$ Tiêu đề trang & Thanh nút hành động (Page Header & Actions) $\rightarrow$ Nội dung chi tiết bố trí theo các Section / Tab $\rightarrow$ Bảng con (Child Table) và Dòng thời gian hoạt động (Activity Timeline).
2. **Cấu trúc đặc tả 3 phần bắt buộc cho mỗi màn hình:**
   * **Phần 1: Thông tin hiển thị (Xem - Read-only):** Dữ liệu chỉ đọc và nguồn gốc trích xuất từ các trường trong Từ điển dữ liệu D02.
   * **Phần 2: Thông tin nhập / chỉnh sửa (Nhập - Input):** Các ô nhập liệu, danh sách chọn (Select), ô đánh dấu (Check) cùng quy tắc bắt buộc/tùy chọn.
   * **Phần 3: Hành động (Actions) & Cảnh báo / Thông báo lỗi:** Các nút bấm thao tác (Lưu, Phân công, Chuyển bước...) và logic kiểm tra điều kiện kèm thông báo phản hồi người dùng.

---

## 2. CHI TIẾT 5 MÀN HÌNH TÁC NGHIỆP TRỌNG TÂM

---

### 2.1. MÀN HÌNH UI-01: HỒ SƠ KHÁCH HÀNG 360° (CUSTOMER 360 VIEW)

#### A. Sơ đồ Wireframe (ASCII Layout)
```text
+----------------------------------------------------------------------------------------------------+
| [CellphoneS CRM]  Tìm kiếm khách hàng (SĐT / Họ tên / Mã KH)...           [Chi nhánh: CN01] [huy.tq v]|
+----------------------------------------------------------------------------------------------------+
| < Trở lại Danh sách KH     HỒ SƠ KHÁCH HÀNG: CUST-2026-00128               [Chỉnh sửa] [Gộp hồ sơ] [...] |
+----------------------------------------------------------------------------------------------------+
| THÔNG TIN ĐỊNH DANH & PHÂN NHÓM                                                                    |
| Họ và tên: [Nguyễn Văn An                 ]   Mã KH: CUST-2026-00128   Chi nhánh gốc: CN01-Nguyễn T.Học|
| Số điện thoại: [0901234567               ]   Nhóm khách: [S-Student (HSSV)   v]                    |
| Email: [an.nguyen@gmail.com              ]   Hạng hội viên: [S-Mem (Thành viên mẫu) v]             |
| Địa chỉ: [123 Cầu Giấy, Hà Nội           ]   Quyền riêng tư: [ ] Ngừng nhận liên hệ chăm sóc (Opt-out)|
+----------------------------------------------------------------------------------------------------+
| [TAB: Nhu cầu & Cơ hội (2)]  [TAB: Nhật ký tương tác (5)]  [TAB: Phiếu hỗ trợ (1)]  [TAB: Lịch sử mua hàng (3)] |
+----------------------------------------------------------------------------------------------------+
| DANH SÁCH CƠ HỘI MUA SẮM (OPPORTUNITIES)                                          [+ Tạo cơ hội mới] |
| Mã cơ hội    | Nhóm SP     | SP quan tâm         | Ngân sách dự kiến | Giai đoạn     | Người phụ trách |
| OPP-2026-045 | Điện thoại  | iPhone 15 Pro Max   | 30.000.000 đ      | Đang tư vấn   | Võ Duy Mạnh     |
| OPP-2026-012 | Phụ kiện    | Củ sạc nhanh GaN 65W| 700.000 đ         | Thành công    | Võ Duy Mạnh     |
+----------------------------------------------------------------------------------------------------+
| LỊCH SỬ MUA HÀNG THAM CHIẾU (TỪ DỮ LIỆU MẪU CSV)                                                   |
| Mã đơn hàng  | Ngày mua   | Sản phẩm                   | IMEI / Serial   | Giá trị (VNĐ)| Chi nhánh bán    |
| DH20260915-01| 15/09/2026 | iPhone 15 Pro Max 256GB    | 354890123456789 | 29.990.000   | CN01-Nguyễn T.Học|
| DH20260925-04| 25/09/2026 | Ốp lưng MagSafe IP15PM     | N/A             |    450.000   | CN01-Nguyễn T.Học|
+----------------------------------------------------------------------------------------------------+
| TIMELINE & NHẮC VIỆC GẦN NHẤT                                                    [+ Ghi nhận tương tác] |
| * 02/10/2026 14:30 [Gọi điện]: Khách hỏi tư vấn nâng cấp củ sạc nhanh cho máy mới mua (Võ Duy Mạnh)  |
| * LỊCH HẸN: 06/10/2026 10:00 - Gọi lại kiểm tra sự hài lòng sau mua sắm (Nguyễn Đức Nhật)        |
+----------------------------------------------------------------------------------------------------+
```

#### B. Đặc tả 3 thành phần giao diện
1. **Thông tin hiển thị (Xem - Read-only):**
   * Mã khách hàng (`Customer.customer_id`), Chi nhánh gốc (`Customer.branch_id`), Thời điểm tạo (`Customer.created_at`).
   * Bảng danh sách cơ hội (`Opportunity` theo `customer_id`).
   * Bảng lịch sử mua hàng tham chiếu (`Purchase_History_Ref` khớp qua `customer_phone` hoặc `customer_id`).
   * Lịch sử tương tác và công việc chăm sóc (`Interaction`, `Care_Task`).
2. **Thông tin nhập / chỉnh sửa (Nhập - Input):**
   * Họ và tên (`Customer.full_name` - Bắt buộc).
   * Số điện thoại (`Customer.phone_number` - Bắt buộc, đúng Regex).
   * Email (`Customer.email` - Tùy chọn), Địa chỉ (`Customer.address` - Tùy chọn).
   * Nhóm khách (`Customer.customer_group` - Select: Cá nhân, S-Student, S-Teacher).
   * Hạng thành viên mẫu (`Customer.membership_tier` - Select: S-New, S-Mem, S-Vip).
   * Ngừng nhận chăm sóc (`Customer.opt_out_care` - Checkbox).
3. **Hành động (Actions) & Cảnh báo lỗi:**
   * `[Lưu]`: Lưu thông tin khách hàng. Hệ thống kiểm tra Regex SĐT, nếu sai định dạng hiển thị: *"Số điện thoại không hợp lệ (yêu cầu 10 chữ số đầu 03/05/07/08/09)"*.
   * *Cảnh báo trùng*: Nếu SĐT đã tồn tại, hiển thị popup: *"Phát hiện số điện thoại đã tồn tại ở hồ sơ CUST-xxxxx. Vui lòng kiểm tra lại trước khi tạo mới!"*.
   * `[Gộp hồ sơ]`: Chỉ hiển thị với `System Administrator` hoặc `Chain Manager`. Mở hộp thoại chọn hồ sơ đích để gộp theo quy tắc đã chốt.
   * `[+ Tạo cơ hội mới]`: Mở form UI-02 tự động điền sẵn thông tin khách hàng.

---

### 2.2. MÀN HÌNH UI-02: CHI TIẾT CƠ HỘI BÁN HÀNG & TƯ VẤN SẢN PHẨM (OPPORTUNITY & CONSULTATION)

#### A. Sơ đồ Wireframe (ASCII Layout)
```text
+----------------------------------------------------------------------------------------------------+
| [CellphoneS CRM]  Cơ hội bán hàng: OPP-2026-045                             [Chi nhánh: CN01]       |
+----------------------------------------------------------------------------------------------------+
| < Trở lại Danh sách     CƠ HỘI TƯ VẤN: OPP-2026-045        Trạng thái: [ĐANG TƯ VẤN v]   [LƯU THAY ĐỔI] |
+----------------------------------------------------------------------------------------------------+
| THÔNG TIN KHÁCH HÀNG & PHÂN CÔNG                                                                   |
| Khách hàng: [CUST-2026-00128 - Nguyễn Văn An        v]    [+ Tạo nhanh KH nếu khách vãng lai]     |
| Số điện thoại: 0901234567                                  Chi nhánh phụ trách: CN01-Nguyễn T.Học  |
| Nhóm khách hàng: S-Student                                 Nhân viên phụ trách: [Võ Duy Mạnh    v] |
+----------------------------------------------------------------------------------------------------+
| CHI TIẾT NHU CẦU TƯ VẤN SẢN PHẨM                                                                   |
| Nhóm sản phẩm: (*) [Điện thoại           v]    Sản phẩm quan tâm: (*) [iPhone 15 Pro Max 256GB    ]|
| Ngân sách dự kiến: [30.000.000        ] VNĐ    Thời điểm dự kiến mua: [Trong tuần này            v]|
| Mục đích sử dụng:                                                                                  |
| [Nhu cầu chụp ảnh sự kiện, quay video học tập, nâng cấp từ iPhone 11 cũ                         ]  |
| Yêu cầu riêng / Cấu hình / Phụ kiện kèm theo:                                                      |
| [Quan tâm màu Titan Tự Nhiên; hỏi về chính sách Thu cũ đổi mới và gói bảo hành AppleCare+       ]  |
+----------------------------------------------------------------------------------------------------+
| TIẾN ĐỘ TƯ VẤN & HÀNH ĐỘNG TIẾP THEO                                                               |
| Giai đoạn: [Đang tư vấn            v]     Lý do thất bại/hủy (nếu có): [                         ] |
| [*] Nút chuyển nhanh: [Chờ phản hồi]  [Chốt thành công (Won)]  [Thất bại (Lost)]  [Hủy cơ hội]     |
+----------------------------------------------------------------------------------------------------+
| CÔNG VIỆC CHĂM SÓC LIÊN QUAN                                                      [+ Tạo lịch hẹn mới]|
| Tiêu đề việc                 | Hạn chót          | Trạng thái     | Người thực hiện | Hành động    |
| Gọi báo giá AppleCare+ kèm máy| 06/10/2026 15:00  | Chưa thực hiện | Võ Duy Mạnh     | [Hoàn thành] |
+----------------------------------------------------------------------------------------------------+
```

#### B. Đặc tả 3 thành phần giao diện
1. **Thông tin hiển thị (Xem - Read-only):**
   * Mã cơ hội (`Opportunity.opportunity_id`), Chi nhánh phụ trách (`Opportunity.branch_id`), Thời gian tạo (`Opportunity.created_at`).
   * Thông tin định danh của khách hàng liên kết (`Customer.full_name`, `Customer.phone_number`, `Customer.customer_group`).
2. **Thông tin nhập / chỉnh sửa (Nhập - Input):**
   * Khách hàng liên kết (`Opportunity.customer_id` - Tùy chọn nếu khách vãng lai).
   * Tên tạm, SĐT tạm (`temp_customer_name`, `temp_phone_number` nếu chưa chọn `customer_id`).
   * Nhóm sản phẩm (`Opportunity.product_category` - Bắt buộc: Điện thoại, Laptop, Phụ kiện).
   * Sản phẩm quan tâm (`Opportunity.product_name` - Bắt buộc).
   * Ngân sách dự kiến (`Opportunity.expected_budget` - Số tiền $\ge 0$).
   * Mục đích sử dụng & Yêu cầu riêng (`usage_purpose`, `specific_requirement`).
   * Thời điểm dự kiến mua (`expected_buy_time`).
   * Nhân viên phụ trách (`Opportunity.assigned_to_user` - Bắt buộc).
   * Giai đoạn (`Opportunity.stage`).
   * Lý do thất bại/hủy (`Opportunity.lost_reason` - Bắt buộc nếu chuyển sang Thất bại/Hủy).
3. **Hành động (Actions) & Cảnh báo lỗi:**
   * `[Lưu]`: Lưu thông tin cơ hội.
   * `[Thất bại (Lost)]` / `[Hủy cơ hội]`: Chuyển trạng thái; nếu `lost_reason` để trống, hệ thống chặn và báo lỗi: *"Vui lòng nhập lý do thất bại hoặc lý do hủy cơ hội để hoàn tất!"*.
   * `[+ Tạo lịch hẹn mới]`: Tự động tạo `Care_Task` liên kết với cơ hội này để nhắc việc đúng hẹn.

---

### 2.3. MÀN HÌNH UI-03: GHI NHẬN TƯƠNG TÁC & QUẢN LÝ CÔNG VIỆC CHĂM SÓC (INTERACTION & CARE TASKS)

#### A. Sơ đồ Wireframe (ASCII Layout)
```text
+----------------------------------------------------------------------------------------------------+
| [CellphoneS CRM]  Quản lý Tương tác & Nhắc việc Chăm sóc                    [Chi nhánh: CN01]       |
+----------------------------------------------------------------------------------------------------+
| DANH SÁCH VIỆC CẦN LÀM HÔM NAY (CARE TASKS DUE TODAY)                 [Bộ lọc: Việc của tôi v]     |
| [!] 2 việc quá hạn   [*] 3 việc đến hạn hôm nay   [v] 5 việc đã hoàn thành                         |
+----------------------------------------------------------------------------------------------------+
| Hạn chót       | Ưu tiên   | Khách hàng       | Tiêu đề công việc            | Trạng thái     | Xử lý   |
| 05/10 11:00 (QH)| [CAO]    | Nguyễn Văn An    | Báo kết quả bảo hành sạc GaN | Chưa thực hiện | [Xem][V]|
| 05/10 16:30    | [TR.BÌNH] | Trần Thị Mai     | Tư vấn nâng cấp RAM Lenovo LOQ| Đang thực hiện | [Xem][V]|
+----------------------------------------------------------------------------------------------------+
| FORM GHI NHẬN TƯƠNG TÁC NHANH (QUICK LOG INTERACTION)                                             |
| Khách hàng: (*) [CUST-2026-00128 - Nguyễn Văn An     v]   Gắn với Cơ hội: [OPP-2026-045         v] |
| Kênh liên hệ: (*) [Gọi điện thoại        v]               Gắn với Phiếu HT: [TICK-2026-00089     v] |
| Chiều liên hệ: (*) [Đến (Inbound)        v]               Thời điểm: [05/10/2026 11:15            ] |
| Người ghi nhận: [Võ Duy Mạnh (Store Sales)          v]                                             |
| Nội dung trao đổi: (*)                                                                             |
| [Khách gọi hỏi củ sạc Ugreen 65W gửi bảo hành hôm trước đã có hàng đổi mới về cửa hàng chưa.       ] |
| [Nhân viên đã kiểm tra hệ thống và thông báo hàng đã về, mời khách ghé lấy chiều nay.              ] |
| Kết quả tương tác: [Khách đồng ý chiều ghé]    [X] Tự động đánh dấu hoàn thành việc liên quan      |
|                                                     [LƯU TƯƠNG TÁC] [LƯU & TẠO VIỆC TIẾP THEO]     |
+----------------------------------------------------------------------------------------------------+
```

#### B. Đặc tả 3 thành phần giao diện
1. **Thông tin hiển thị (Xem - Read-only):**
   * Danh sách công việc cần làm (`Care_Task`) được phân loại trực quan theo màu: Đỏ (Quá hạn), Vàng (Đến hạn hôm nay), Xanh (Hoàn thành).
   * Thông tin nhân viên đăng nhập (`performed_by_user`).
2. **Thông tin nhập / chỉnh sửa (Nhập - Input):**
   * Khách hàng (`Interaction.customer_id` - Bắt buộc).
   * Kênh liên hệ (`channel` - Select: Gọi điện, Trực tiếp tại quầy, Tin nhắn).
   * Chiều liên hệ (`direction` - Select: Đến, Đi).
   * Thời điểm tương tác (`interaction_time` - Datetime $\le NOW()$).
   * Liên kết nghiệp vụ (`opportunity_id` hoặc `ticket_id` - Tùy chọn).
   * Nội dung trao đổi (`summary` - Long Text, Bắt buộc).
3. **Hành động (Actions) & Cảnh báo lỗi:**
   * `[LƯU TƯƠNG TÁC]`: Lưu bản ghi vào bảng `Interaction`. Nếu khách hàng đã `opt_out_care = 1`, hệ thống hiển thị icon cảnh báo: *"Khách hàng đã yêu cầu ngừng nhận liên hệ chăm sóc tiếp thị"*.
   * `[LƯU & TẠO VIỆC TIẾP THEO]`: Lưu tương tác và tự động mở popup tạo `Care_Task` mới cho bước chăm sóc tiếp theo.
   * `[Hoàn thành việc]`: Nút tích chọn hoàn thành nhanh trên danh sách `Care_Task`, tự động cập nhật `status = 'Hoàn thành'` và `completed_at = NOW()`.

---

### 2.4. MÀN HÌNH UI-04: PHIẾU HỖ TRỢ DỊCH VỤ & PHỐI HỢP HẬU MÃI (SUPPORT TICKET & AFTER-SALES)

#### A. Sơ đồ Wireframe (ASCII Layout)
```text
+----------------------------------------------------------------------------------------------------+
| [CellphoneS CRM]  Phiếu hỗ trợ: TICK-2026-00089                             [Chi nhánh: CN01]       |
+----------------------------------------------------------------------------------------------------+
| < Trở lại Danh sách     PHIẾU HỖ TRỢ: TICK-2026-00089        Trạng thái: [ĐÃ CÓ KẾT QUẢ v] [LƯU PHIẾU]|
+----------------------------------------------------------------------------------------------------+
| THÔNG TIN TIẾP NHẬN YÊU CẦU HẬU MÃI                                                               |
| Khách hàng: (*) [CUST-2026-00128 - Nguyễn Văn An     v]   Chi nhánh tiếp nhận: CN01-Nguyễn Thái Học|
| Số điện thoại: 0901234567                                  Thời điểm tiếp nhận: 01/10/2026 09:30   |
| Phân loại: (*) [Bảo hành thiết bị                   v]   Người chịu trách nhiệm: (*) [Nguyễn Đ.Nhật]|
| Sản phẩm liên quan: (*) [Củ sạc nhanh GaN 65W Ugreen  ]   Số IMEI/Serial: [UG65W-2026             ]|
| Mô tả lỗi / Phản ánh của khách: (*)                                                                |
| [Củ sạc cắm không vào điện, đã test với nhiều cáp khác nhau tại quầy, đèn led không sáng.          ] |
+----------------------------------------------------------------------------------------------------+
| TIẾN ĐỘ PHỐI HỢP & KẾT QUẢ XỬ LÝ                                                                   |
| Trạng thái xử lý: [Đã có kết quả                     v]   (Quy trình: Tiếp nhận -> Xử lý -> Kết quả)|
| Tiến độ đối tác ngoài (Hãng/CareS):                                                               |
| [Đã gửi TTBH Ugreen ngày 02/10; Hãng xác nhận lỗi mạch nguồn, đã đổi củ sạc mới mã mới về shop.    ] |
| Kết quả xử lý chi tiết: (*) [BẮT BUỘC KHI ĐÓNG PHIẾU]                                              |
| [Đổi mới 100% sản phẩm theo chính sách 1 đổi 1 trong 12 tháng. Củ sạc mới đã sẵn sàng tại quầy.    ] |
+----------------------------------------------------------------------------------------------------+
| ĐIỀU KIỆN ĐÓNG PHIẾU (CLOSING GATES)                                                               |
| [X] ĐÃ THÔNG BÁO CHO KHÁCH HÀNG (Customer Notified) (*)                                            |
| Phương thức thông báo: [Gọi điện thoại thông báo lúc 05/10 11:15 - Khách hẹn ghé lấy chiều nay     ] |
| [CHUYỂN TIẾP NHẬN]   [CHUYỂN ĐANG XỬ LÝ]   [CẬP NHẬT KẾT QUẢ]   >>> [ĐÓNG PHIẾU HỖ TRỢ] <<<        |
+----------------------------------------------------------------------------------------------------+
```

#### B. Đặc tả 3 thành phần giao diện
1. **Thông tin hiển thị (Xem - Read-only):**
   * Mã phiếu (`ticket_id`), Chi nhánh tiếp nhận (`branch_id`), Ngày giờ lập phiếu (`created_at`), Ngày đóng phiếu (`closed_at`).
   * Lịch sử tương tác và ghi chú điều chuyển giữa 2 chi nhánh.
2. **Thông tin nhập / chỉnh sửa (Nhập - Input):**
   * Khách hàng liên kết (`customer_id` - Bắt buộc).
   * Phân loại phiếu (`ticket_type` - Select: Bảo hành, Đổi trả, Khiếu nại).
   * Sản phẩm & IMEI/Serial (`related_product`, `imei_serial`).
   * Mô tả lỗi (`issue_description` - Long Text, Bắt buộc).
   * Nhân viên chịu trách nhiệm (`assigned_to_user` - Link, Bắt buộc).
   * Tiến độ đối tác ngoài (`external_partner_status`).
   * Kết quả xử lý (`resolution_result` - Long Text).
   * Cờ xác nhận đã thông báo khách (`customer_notified` - Checkbox).
3. **Hành động (Actions) & Cảnh báo lỗi (Hard Gate Validation):**
   * `[LƯU PHIẾU]`: Lưu thông tin cập nhật.
   * `[ĐÓNG PHIẾU HỖ TRỢ]`: Chuyển `status = 'Đóng phiếu'`.
   * *Kiểm tra điều kiện đóng phiếu nghiêm ngặt*:
     * Nếu `resolution_result` để trống: Báo lỗi *"Không thể đóng phiếu: Vui lòng nhập Kết quả xử lý chi tiết trước khi đóng!"*.
     * Nếu `customer_notified = 0` (chưa tích chọn đã thông báo): Báo lỗi *"Không thể đóng phiếu: Bạn phải xác nhận đã liên hệ thông báo kết quả cho khách hàng!"*.
     * Đóng thành công: Hệ thống tự động ghi nhận `closed_at = NOW()`.

---

### 2.5. MÀN HÌNH UI-05: DASHBOARD BÁO CÁO QUẢN LÝ & GIÁM SÁT SLA (MANAGEMENT REPORT & SLA TRACKING)

#### A. Sơ đồ Wireframe (ASCII Layout)
```text
+----------------------------------------------------------------------------------------------------+
| [CellphoneS CRM]  Dashboard Báo cáo Quản trị Toàn chuỗi                   [Phạm vi: Cả 2 cửa hàng v]|
+----------------------------------------------------------------------------------------------------+
| BỘ LỌC BÁO CÁO:  Từ ngày: [01/10/2026]  Đến ngày: [15/10/2026]  Chi nhánh: [Tất cả (CN01, CN02) v] |
+----------------------------------------------------------------------------------------------------+
| CÁC CHỈ TIÊU NGHIỆM THU CỐT LÕI (SUCCESS METRICS - KARL WIEGERS PRACTICE #2)                       |
| +-------------------------+ +-------------------------+ +-------------------------+                |
| | TIẾP NỐI TƯ VẤN THUẬN LỢI| | CHĂM SÓC ĐÚNG HẸN (SLA) | | TRÁCH NHIỆM HẬU MÃI   |                |
| |       19 / 20 ca        | |         94.2 %          | |         100 %           |                |
| | [ĐẠT CHỈ TIÊU >=18/20]  | | [ĐẠT CHỈ TIÊU >= 90%]   | | [100% Phiếu có người XL]|                |
| +-------------------------+ +-------------------------+ +-------------------------+                |
+----------------------------------------------------------------------------------------------------+
| BÁO CÁO TIẾN ĐỘ CƠ HỘI BÁN HÀNG                         | BÁO CÁO XỬ LÝ PHIẾU HỖ TRỢ DỊCH VỤ       |
| Phân loại theo giai đoạn:                               | Tổng số phiếu: 45                        |
| - Mới tiếp nhận : 12 ca (24%)                           | - Tiếp nhận     : 5 phiếu (11.1%)        |
| - Đang tư vấn   : 18 ca (36%)                           | - Đang xử lý    : 10 phiếu (22.2%)       |
| - Chờ phản hồi  :  8 ca (16%)                           | - Đã có kết quả : 8 phiếu (17.8%)        |
| - Thành công (V): 10 ca (20%)                           | - Đã thông báo  : 7 phiếu (15.6%)        |
| - Thất bại / Hủy:  2 ca ( 4%)                           | - Đã đóng phiếu : 15 phiếu (33.3%)       |
+----------------------------------------------------------------------------------------------------+
| THEO DÕI NĂNG SUẤT VÀ TỶ LỆ HOÀN THÀNH THEO CHI NHÁNH                                              |
| Chi nhánh          | Tổng việc giao | Hoàn thành đúng hạn | Quá hạn | Tỷ lệ SLA  | Đánh giá        |
| CN01 - Nguyễn T.Học|       65       |          62         |    3    |   95.4 %   | Xuất sắc        |
| CN02 - Hàng Bài    |       40       |          37         |    3    |   92.5 %   | Đạt yêu cầu     |
| Toàn hệ thống      |      105       |          99         |    6    |   94.2 %   | ĐẠT MỤC TIÊU    |
+----------------------------------------------------------------------------------------------------+
```

#### B. Đặc tả 3 thành phần giao diện
1. **Thông tin hiển thị (Xem - Read-only):**
   * Các thẻ KPI tổng hợp tương ứng 3 mục tiêu thành công trong `Nền Tảng CRM.md`.
   * Biểu đồ tỷ lệ chuyển đổi cơ hội tư vấn (`Opportunity.stage`).
   * Phân bổ trạng thái phiếu hỗ trợ (`Support_Ticket.status`).
   * Bảng đối soát SLA chăm sóc đúng hạn giữa 2 chi nhánh.
2. **Thông tin nhập / bộ lọc (Nhập - Input):**
   * Khoảng thời gian báo cáo (`From Date`, `To Date`).
   * Phạm vi chi nhánh (`Branch Filter`: CN01, CN02, Toàn chuỗi).
   * Lọc theo nhân viên phụ trách (`Assigned User Filter`).
3. **Hành động (Actions) & Phân quyền:**
   * `[Xuất báo cáo Excel / PDF]`: Xuất dữ liệu thống kê ra file báo cáo.
   * `[Xem danh sách chi tiết]`: Nhấp vào từng chỉ số để khoan sâu (drill-down) vào danh sách các bản ghi cụ thể.
   * *Giới hạn quyền truy cập*:
     * `Store Manager` chỉ xem được dữ liệu của chi nhánh mình.
     * `Chain Manager` và `System Administrator` có toàn quyền xem số liệu so sánh giữa 2 chi nhánh.
