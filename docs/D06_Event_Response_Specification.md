# ĐẶC TẢ SỰ KIỆN VÀ PHẢN ỨNG HỆ THỐNG (EVENTS & RESPONSES)
## PHÂN HỆ QUẢN TRỊ MỐI QUAN HỆ KHÁCH HÀNG (CRM) – HỆ THỐNG BÁN LẺ CELLPHONES
### Case Study: Tiếp nhận và theo dõi yêu cầu bảo hành / đổi trả tại hai cửa hàng

- **Người thực hiện:** Trần Quang Huy (MSSV: 24520706 – TV4)
- **Học phần:** Quản trị mối quan hệ khách hàng (CRM) – EC337
- **Nền tảng công nghệ:** Frappe Framework v15 (Custom App độc lập `cellphones_crm` – MariaDB)
- **Phương pháp luận:** Karl Wiegers & Candase Hokanson (*Software Requirements Essentials*, Chapter 3, Practice #7: *Identify Events and Responses*)
- **Tài liệu căn cứ tích hợp:**
  - `baseline_case_study_crm_cellphones.md` (Baseline dùng chung của nhóm)
  - `nội dung của mạnh.md` (Mô hình Gaps Model SERVQUAL – Võ Duy Mạnh)
  - `nôi dung của nhật.md` (User Requirements & Use Cases UC-01 đến UC-06 – Nguyễn Đức Nhật)
  - `PROJECT_OVERVIEW.md` & Bộ hồ sơ kỹ thuật D01–D05 (Trần Quang Huy)

---

## 1. BỐI CẢNH VÀ QUY ƯỚC NGHIỆP VỤ

### 1.1. Bối cảnh hai cửa hàng và luồng nghiệp vụ chuẩn (Happy Path)
Hệ thống CRM mô phỏng quy trình xử lý yêu cầu hậu mãi (bảo hành, đổi trả, khiếu nại) tại hai chi nhánh:
- **Cửa hàng 01 (CH01):** CellphoneS 136 Nguyễn Thái Học, TP.HCM (Nơi phát sinh giao dịch mua hàng ban đầu).
- **Cửa hàng 02 (CH02):** CellphoneS 21A Hàng Bài, Hà Nội (Nơi khách hàng mang thiết bị đến yêu cầu hỗ trợ).

**Kịch bản nghiệp vụ chuẩn (Happy Path):**
> Khách hàng đã mua thiết bị tại CH01, sau đó mang thiết bị đến CH02 yêu cầu bảo hành/đổi trả. Nhân viên tại CH02 tra cứu thông tin tham chiếu, lập ticket, ghi nhận hiện trạng. Ticket được phân công đích danh Owner và chuyển sang giai đoạn xác minh chính sách. Sau đó, CH02 chuyển ticket sang trạng thái xử lý/phối hợp với đối tác bảo hành ngoài (có thể kích hoạt chế độ chờ có hẹn ngày theo dõi). Khi nhận được kết quả thẩm định chuyên môn, ticket chuyển sang "Đã giải quyết". Nhân viên liên hệ thông báo cho khách hàng, thực hiện bàn giao thiết bị thực tế, đối chiếu đủ điều kiện đóng (Closing Gates) và đóng ticket. Toàn bộ lịch sử xử lý, phân công và tương tác được lưu trữ nguyên vẹn trên hệ thống.

### 1.2. Phân định mức độ thông tin (A / B / C)
- **Mức A (Căn cứ thực tế có sẵn):** Mô hình hai cửa hàng (CH01, CH02); quy trình hậu mãi 6 giai đoạn; 5 trạng thái chính của ticket; 9 tình huống ngoại lệ EX-01 đến EX-09; các vai trò nhân sự xác định.
- **Mức B (Đề xuất giải pháp nghiệp vụ):** Người tạo tạm chịu trách nhiệm (`temporary_owner`); kiểm soát điều kiện đóng nghiêm ngặt (`Closing Gates`); tách biệt trạng thái chính và lý do chờ (`wait_reason`); quy tắc điều phối liên cửa hàng giữ nguyên mã `ticket_id`.
- **Mức C (Định hướng mở rộng tương lai - Future-state, ngoài phạm vi MVP):** Cổng tra cứu trực tuyến cho khách (Customer Portal); kết nối API thời gian thực với hãng/Điện Thoại Vui; Webhook tự động từ đối tác giao vận; gửi SMS/Zalo Brandname tự động; khảo sát CSAT tự động.

---

## 2. DANH MỤC SỰ KIỆN VÀ PHÂN LOẠI (EVENT LIST & CLASSIFICATION)

### 2.1. Phân loại sự kiện theo Karl Wiegers
Theo phương pháp luận Practice #7 của Karl Wiegers (*Software Requirements Essentials*), Use Cases rất hiệu quả khi mô tả tương tác trực tiếp giữa người dùng và phần mềm, nhưng dễ bỏ sót các tương tác từ thiết bị phần cứng hoặc các tiến trình chạy ngầm theo thời gian. Hệ thống phân chia toàn bộ sự kiện thành **3 nhóm chuẩn mực**:

1. **Business Events (Sự kiện nghiệp vụ):** 
   - Khởi phát do con người chủ động thao tác (Khách hàng, Nhân viên quầy, Kỹ thuật viên/Owner, Quản lý cửa hàng, CSKH chuỗi hoặc Đơn vị bảo hành ngoài).
   - Mục đích nhằm thực hiện một giao dịch trong luồng nghiệp vụ (tiếp nhận lập ticket, phân công owner, xác minh chính sách, chuyển xử lý, đóng ticket, khiếu nại, mở lại ticket...).
2. **Signal Events (Sự kiện tín hiệu phần cứng quầy):**
   - Khởi phát trực tiếp từ các thiết bị phần cứng, đầu đọc ngoại vi tại quầy tiếp nhận (máy quét Barcode/QR quét mã IMEI/Serial máy, đầu đọc thẻ thông minh quẹt thẻ hội viên Smember).
   - Tín hiệu ngắt truyền trực tiếp vào giao diện nhập liệu để tự động hóa thao tác tra cứu, giảm thời gian nhập tay.
3. **Temporal Events (Sự kiện thời gian chạy ngầm):**
   - Khởi phát tự động bởi bộ lập lịch hệ thống (Frappe Scheduled Hook / Cron Scheduler) chạy ngầm định kỳ vào đầu ngày hoặc theo chu kỳ quét cố định (đến hạn nhắc việc trong ngày, quá hạn SLA theo dõi EX-06, quét ticket chờ lâu, quét ticket mất chủ).
   - *Lưu ý quan trọng:* Đây là tiến trình quét định kỳ của hệ thống, không phải hệ thống tự nhận biết theo thời gian thực.

> **Quy chuẩn phân định ranh giới (Guard Conditions vs Signal Events):**
> Các thao tác kiểm tra tính hợp lệ dữ liệu (Validation Gate), kiểm tra phân quyền (Permission Check) hay kiểm tra điều kiện đóng ticket (Closing Gates BR-06) là **Điều kiện kiểm tra (Guard Conditions)** và **Phản ứng phần mềm (System Responses)** nằm trong quá trình xử lý một *Business Event*, không phải là một *Signal Event* độc lập.

---

### 2.2. Danh mục 19 sự kiện trong phạm vi MVP

Toàn bộ hệ thống quản lý bảo hành/đổi trả bao gồm **19 sự kiện cốt lõi**, mã hóa theo chuẩn:
- **EVT-B01 đến EVT-B13:** 13 Business Events.
- **EVT-S01 và EVT-S02:** 2 Signal Events phần cứng tại quầy.
- **EVT-T01 đến EVT-T04:** 4 Temporal Events chạy ngầm định kỳ.

#### Bảng tổng hợp Danh mục sự kiện (Event List)

| Nhóm sự kiện | Mã Event | Tên sự kiện | Tác nhân / Nguồn kích hoạt | Mục đích & Ngữ cảnh xuất hiện |
| :--- | :---: | :--- | :--- | :--- |
| **Business Events**<br>*(13 sự kiện)* | **EVT-B01** | Tiếp nhận và gửi yêu cầu tạo ticket tại quầy CH02 | Khách hàng & Nhân viên tiếp nhận | Khách hàng mang máy đến CH02; nhân viên đối chiếu thông tin và gửi yêu cầu lưu ticket lên CRM. |
| | **EVT-B02** | Phân công Owner chính và thiết lập hạn theo dõi | Quản lý / CSKH chuỗi | Chỉ định đích danh nhân sự phụ trách và ngày hẹn theo dõi (`follow_up_date`) để chuyển sang xác minh. |
| | **EVT-B03** | Điều phối chuyển ticket giữa hai cửa hàng (EX-04) | CSKH cấp chuỗi | Chuyển giao trách nhiệm xử lý từ CH02 sang CH01; giữ nguyên mã `ticket_id` và toàn bộ lịch sử. |
| | **EVT-B04** | Hoàn tất xác minh và xác định hướng xử lý | Ticket Owner | Ghi nhận căn cứ chính sách bảo hành, chọn hướng xử lý để chuyển ticket sang trạng thái Đang xử lý. |
| | **EVT-B05** | Kích hoạt chế độ chờ xử lý bên ngoài (EX-05) | Ticket Owner | Ghi nhận nhu cầu chờ linh kiện, chờ thẩm định hãng hoặc chờ khách; gán lý do chờ và ngày hẹn theo dõi mới. |
| | **EVT-B06** | Nhận phản hồi kết quả từ đơn vị bảo hành ngoài | Đối tác bảo hành / Owner ghi nhận | Đối tác gửi biên bản kiểm định/báo giá; Owner ghi nhận thông tin và đính kèm biên bản vào hồ sơ CRM. |
| | **EVT-B07** | Xác nhận kết thúc thời gian chờ xử lý | Ticket Owner | Nhận đủ kết quả từ khách hoặc đối tác; giải phóng cờ chờ để tiếp tục xử lý kỹ thuật. |
| | **EVT-B08** | Báo trở ngại đủ điều kiện đổi máy nhưng kho hết hàng (EX-09) | Nhân viên / Quản lý kho | Xác nhận đủ điều kiện đổi mới 1-1 nhưng hết hàng tại chi nhánh; báo chuyển Quản lý tìm phương án giải quyết. |
| | **EVT-B09** | Khách hàng chủ động rút yêu cầu hỗ trợ | Khách hàng | Khách đổi ý, không muốn tiếp tục gửi máy; chuyển luồng hoàn trả thiết bị nguyên trạng, không xóa ticket. |
| | **EVT-B10** | Chốt kết quả xử lý thực tế và chuyển Đã giải quyết | Ticket Owner | Cập nhật kết luận giải quyết thực tế, căn cứ đính kèm để chuyển ticket sang Đã giải quyết. |
| | **EVT-B11** | Ghi nhận phản hồi của khách hàng sau thông báo | Khách hàng & Nhân viên liên hệ | Khách hàng đồng ý phương án (Happy path) hoặc khách hàng từ chối/khiếu nại (EX-07 quay lại xử lý). |
| | **EVT-B12** | Gửi yêu cầu đóng ticket và hoàn tất bàn giao | Nhân viên / Owner / Quản lý | Yêu cầu đóng ca sau khi đã bàn giao máy cho khách; kiểm tra nghiêm ngặt điều kiện Closing Gates BR-06. |
| | **EVT-B13** | Yêu cầu mở lại (Reopen) ticket đã đóng (EX-08) | Khách hàng & Quản lý / CSKH chuỗi | Khách mang máy quay lại phản ánh cùng lỗi cũ; cấp có thẩm quyền phê duyệt mở lại ticket đã đóng. |
| **Signal Events**<br>*(2 sự kiện)* | **EVT-S01** | Quét Barcode/QR IMEI/Serial thiết bị thành công | Máy quét mã vạch 2D tại quầy | Máy quét bắn tín hiệu qua cổng HID; CRM tự động nạp IMEI lên form và tìm kiếm giao dịch mua CH01. |
| | **EVT-S02** | Quẹt thẻ thành viên Smember qua cảm biến quầy | Đầu đọc thẻ thông minh tại quầy | Cảm biến nhận diện mã thẻ; CRM tự động truy vấn và hiển thị thông tin khách hàng lên màn hình tiếp nhận. |
| **Temporal Events**<br>*(4 sự kiện)* | **EVT-T01** | Đến hạn theo dõi ticket trong ngày | Tiến trình chạy ngầm hàng ngày (00:00 / 08:00) | Quét các ticket chưa đóng có ngày hẹn đúng ngày hiện tại; phát thông báo nhắc việc cho Owner. |
| | **EVT-T02** | Quá hạn theo dõi mà chưa có cập nhật (EX-06) | Tiến trình chạy ngầm hàng ngày (00:00 / 08:00) | Quét ticket quá hạn theo dõi mà chưa có log cập nhật mới; gắn cờ báo đỏ và gửi cảnh báo đến Quản lý. |
| | **EVT-T03** | Định kỳ quét rà soát ticket chờ kéo dài | Tiến trình Cron chạy ngầm mỗi 24h | Quét các ticket đang ở chế độ chờ đối tác hoặc chờ khách quá 7 ngày; tổng hợp báo cáo cảnh báo SLA chuỗi. |
| | **EVT-T04** | Định kỳ quét phát hiện ticket mất chủ | Tiến trình Cron chạy ngầm mỗi 6h | Quét các ticket chưa đóng nhưng bị trống người chịu trách nhiệm; đưa vào danh sách khẩn cấp để Quản lý gán lại. |

---

## 3. BẢNG PHẢN ỨNG VỚI SỰ KIỆN (EVENT–RESPONSE TABLE)

Bảng phân tích 6 cột chuẩn kỹ thuật thể hiện rõ: Mã sự kiện, Tên & tác nhân, Phân loại, Điều kiện kích hoạt, Phản ứng kỳ vọng (tách bạch rõ ràng giữa hành vi hệ thống CRM và trách nhiệm con người ngoài thực tế), Dữ liệu & Nhật ký bị tác động.

| Mã Event | Tên sự kiện & Tác nhân | Phân loại | Điều kiện / Trạng thái kích hoạt | Phản ứng kỳ vọng của hệ thống & Trách nhiệm con người | Dữ liệu & Nhật ký bị tác động |
| :---: | :--- | :---: | :--- | :--- | :--- |
| **EVT-B01** | Tiếp nhận và gửi yêu cầu tạo ticket tại quầy CH02<br>*(Khách hàng & Nhân viên quầy)* | **Business** | Khách mang máy tới quầy CH02; Nhân viên điền form và nhấn Lưu. *(Hỗ trợ nạp nhanh qua EVT-S01 quét mã hoặc EVT-S02 quẹt thẻ)*. | **1. Hệ thống CRM:**<br>- Kiểm tra đủ thông tin bắt buộc $\rightarrow$ Tạo ticket mới ở trạng thái `New`, tự cấp mã ticket, gán người tạo tạm chịu trách nhiệm (`temporary_owner`).<br>- Nếu thiếu thông tin bắt buộc $\rightarrow$ Báo đỏ ô bị thiếu và chặn lưu (không tạo rác dữ liệu).<br>- Nếu nghi trùng SĐT $\rightarrow$ Hiện cảnh báo đối chiếu (tuyệt đối không tự gộp hồ sơ).<br>- Ghi nhận cờ ngoại lệ nếu có: thiếu chứng từ (EX-01), tranh chấp rơi vỡ/vào nước (EX-02), lỗi chập chờn (EX-03).<br>**2. Nhân viên quầy:** Đối chiếu dữ liệu, kiểm tra ngoại quan máy thực tế, in mã ticket đưa cho khách và hẹn các bước tiếp theo. | Tạo mới `Support Ticket` (trạng thái `New`), tạo `Customer` (nếu khách mới), ghi log `CREATE_TICKET`. |
| **EVT-S01** | Quét Barcode/QR IMEI/Serial thiết bị thành công<br>*(Máy quét Barcode tại quầy)* | **Signal** | Đang mở Form tiếp nhận; nhân viên quét tem mã trên thân máy hoặc vỏ hộp. | **1. Hệ thống CRM:** Bắt tín hiệu từ máy quét, tự động điền mã IMEI vào form và kích hoạt tìm kiếm lịch sử mua hàng tham chiếu từ CH01.<br>**2. Nhân viên quầy:** So khớp mã quét được với tem máy vật lý. | Điền tự động trường `imei_serial` trên form tiếp nhận. |
| **EVT-S02** | Quẹt thẻ thành viên Smember qua cảm biến quầy<br>*(Đầu đọc thẻ tại quầy)* | **Signal** | Đang ở màn hình tra cứu; khách chạm thẻ hội viên vào đầu đọc. | **1. Hệ thống CRM:** Nhận mã thẻ từ đầu đọc, tự động tìm kiếm và nạp thông tin khách hàng lên màn hình tiếp nhận.<br>**2. Nhân viên quầy:** Xác nhận lại họ tên với khách hàng để đảm bảo chính chủ thẻ. | Nạp thông tin khách hàng (`customer_id`, tên, SĐT, hạng thẻ) lên form tiếp nhận. |
| **EVT-B02** | Phân công Owner chính và thiết lập hạn theo dõi<br>*(Quản lý / CSKH chuỗi)* | **Business** | Ticket ở trạng thái `New`; Quản lý chọn nhân sự xử lý và ngày hẹn theo dõi (`follow_up_date`). | **1. Hệ thống CRM:**<br>- Kiểm tra nhân sự được chọn có quyền tại chi nhánh $\rightarrow$ Gán chính thức `Owner`, xóa người tạo tạm, chuyển ticket sang `Verifying`, lưu ngày hẹn theo dõi, tự động tạo việc nhắc nhở (`Care Task`) và gửi thông báo công việc cho Owner mới.<br>- Nếu chọn nhân sự sai chi nhánh $\rightarrow$ Chặn thao tác, giữ nguyên trạng thái cũ.<br>**2. Owner mới:** Nhận thông báo việc, mở hồ sơ bắt đầu đối chiếu chính sách bảo hành. | Cập nhật `Support Ticket` (`Verifying`, `assigned_to_user`, `follow_up_date`), tạo mới `Care Task`, ghi log `ASSIGN_OWNER`. |
| **EVT-B03** | Điều phối chuyển ticket giữa hai cửa hàng (EX-04)<br>*(CSKH cấp chuỗi)* | **Business** | Trạng thái `Verifying` hoặc `Processing`; Cần chuyển giao trách nhiệm từ CH02 sang CH01 (hoặc ngược lại). | **1. Hệ thống CRM:** Giữ nguyên mã `ticket_id` và toàn bộ lịch sử ca; đổi chi nhánh xử lý sang CH01, gán Owner mới tại CH01; tự động chuyển quyền chỉnh sửa từ CH02 sang CH01 (CH02 chỉ còn quyền xem).<br>**2. CSKH chuỗi & Nhân viên:** CSKH chuỗi ghi rõ lý do chuyển; làm thủ tục điều chuyển thiết bị vật lý; nhân viên CH01 kiểm tra tiếp nhận hồ sơ. | Cập nhật `Support Ticket` (`processing_store = 'CH01'`, Owner mới, lý do chuyển), đổi quyền truy cập chi nhánh, ghi log `TRANSFER_STORE_EX04`. |
| **EVT-B04** | Hoàn tất xác minh và xác định hướng xử lý<br>*(Ticket Owner)* | **Business** | Trạng thái `Verifying`; Owner đã điền căn cứ chính sách và chọn hướng giải quyết. | **1. Hệ thống CRM:** Kiểm tra đã điền đủ căn cứ chính sách $\rightarrow$ Chuyển trạng thái sang `Processing`, lưu vết người xác minh, thời gian xác minh và ngày hẹn theo dõi tiếp theo.<br>**2. Owner:** Tiến hành xử lý kỹ thuật nội bộ hoặc làm thủ tục gửi máy sang trung tâm bảo hành hãng. | Cập nhật `Support Ticket` (`Processing`, phương án xử lý, người xác minh), ghi log `TRANSITION_PROCESSING`. |
| **EVT-B05** | Kích hoạt chế độ chờ xử lý bên ngoài (EX-05)<br>*(Ticket Owner)* | **Business** | Trạng thái chính là `Processing`; phát sinh nhu cầu chờ khách hoặc chờ hãng thẩm định. | **1. Hệ thống CRM:** Bắt buộc chọn lý do chờ (chờ khách hoặc chờ hãng), nhập tên đơn vị, mã phiếu gửi và ngày hẹn theo dõi mới; giữ nguyên trạng thái chính là `Processing`; tự động tạo việc nhắc nhở `Care Task`.<br>**2. Owner:** Điền đầy đủ thông tin gửi máy cho hãng hoặc hẹn khách; theo dõi sát ngày hẹn. | Cập nhật `Support Ticket` (`wait_reason`, đối tác, mã tham chiếu, ngày hẹn theo dõi), tạo mới `Care Task`, ghi log `SET_WAIT_REASON`. |
| **EVT-B06** | Nhận phản hồi kết quả từ đơn vị bảo hành ngoài<br>*(Đối tác bảo hành / Owner ghi nhận)* | **Business** | Trạng thái `Processing`, đang ở chế độ chờ hãng (`CHO_DON_VI_BAO_HANH`). | **1. Hệ thống CRM:** Cho phép nhập kết quả thẩm định chuyên môn của hãng, đính kèm biên bản kiểm định và ghi nhận chi phí (nếu có).<br>**2. Owner:** Tiếp nhận thông tin từ hãng qua email/điện thoại; nhập đầy đủ kết quả vào CRM; đính kèm biên bản; chuẩn bị phương án trả lời khách. | Tạo bản ghi `Interaction` (loại phản hồi đối tác), đính kèm file biên bản, ghi log `PARTNER_RESPONSE_RECORDED`. |
| **EVT-B07** | Xác nhận kết thúc thời gian chờ xử lý<br>*(Ticket Owner)* | **Business** | Đang có lý do chờ; nhận đủ dữ liệu phản hồi từ đối tác hoặc khách hàng. | **1. Hệ thống CRM:** Xóa cờ chờ (`wait_reason = NULL`), đóng hoàn tất việc nhắc nhở `Care Task` chờ, giữ nguyên trạng thái chính `Processing`.<br>**2. Owner:** Tiếp tục thực hiện các bước xử lý kỹ thuật tiếp theo cho ticket. | Cập nhật `Support Ticket` (xóa `wait_reason`), đóng `Care Task`, ghi log `WAIT_ENDED`. |
| **EVT-B08** | Báo trở ngại đủ điều kiện đổi máy nhưng kho hết hàng (EX-09)<br>*(Nhân viên / Quản lý kho)* | **Business** | Trạng thái `Processing`; xác nhận đủ điều kiện đổi mới 1-1 nhưng hết máy tại cửa hàng. | **1. Hệ thống CRM:** Ghi nhận trở ngại "Kho hết hàng", cấm hệ thống tự động hoàn tiền/cấp voucher, tự động tạo việc khẩn gửi Quản lý cửa hàng.<br>**2. Owner & Quản lý:** Owner báo cáo Quản lý; Quản lý liên hệ điều chuyển hàng từ chi nhánh khác hoặc tư vấn phương án thỏa thuận với khách. | Cập nhật cờ hết hàng trên `Support Ticket`, tạo mới `Care Task` gửi Quản lý, ghi log `OUT_OF_STOCK_EX09`. |
| **EVT-B09** | Khách hàng chủ động rút yêu cầu hỗ trợ<br>*(Khách hàng)* | **Business** | Trạng thái `Verifying` hoặc `Processing`. | **1. Hệ thống CRM:** Cho phép ghi nhận lý do khách rút ca; chuyển thẳng sang `Resolved` với kết quả "Khách rút yêu cầu"; tuyệt đối không xóa ticket; tự động tạo việc bàn giao trả máy.<br>**2. Owner:** Ghi rõ lý do khách rút ca; thực hiện thủ tục hoàn trả thiết bị nguyên trạng cho khách; ký biên bản bàn giao. | Cập nhật `Support Ticket` (`Resolved`, kết quả rút ca), tạo mới `Care Task` trả máy, ghi log `CUSTOMER_WITHDREW`. |
| **EVT-B10** | Chốt kết quả xử lý thực tế và chuyển Đã giải quyết<br>*(Ticket Owner)* | **Business** | Trạng thái `Processing`; đã có kết quả xử lý thực tế; cờ chờ đã kết thúc. | **1. Hệ thống CRM:** Bắt buộc nhập kết luận thực tế và căn cứ giải quyết; chuyển trạng thái sang `Resolved`, lưu người chốt và thời gian chốt; tự động sinh 2 việc bắt buộc: "Thông báo khách" và "Bàn giao máy".<br>**2. Owner:** Kiểm tra lại toàn bộ hồ sơ; chuẩn bị nội dung giải thích rõ ràng cho khách; mở tác vụ liên hệ. | Cập nhật `Support Ticket` (`Resolved`, kết quả giải quyết, người chốt), tạo mới 2 `Care Task`, ghi log `TRANSITION_RESOLVED`. |
| **EVT-B11** | Ghi nhận phản hồi của khách sau thông báo kết quả<br>*(Khách hàng & Nhân viên liên hệ)* | **Business** | Trạng thái `Resolved`; nhân viên đã liên hệ giải thích phương án xử lý cho khách. | **1. Hệ thống CRM:**<br>- Khách đồng ý $\rightarrow$ Đánh dấu "Đã thông báo khách", hoàn tất việc thông báo.<br>- Khách khiếu nại / từ chối (EX-07) $\rightarrow$ Chuyển ngược ticket về `Processing`, bắt buộc nhập lý do khiếu nại, hẹn ngày xử lý mới; bảo toàn toàn bộ kết quả cũ trong lịch sử.<br>**2. Nhân viên & Quản lý:** Nếu khách khiếu nại, lắng nghe trung thực, báo cáo Quản lý để hội chẩn lại phương án thỏa đáng hơn. | Tạo bản ghi `Interaction` (cuộc gọi/gặp khách), cập nhật `Support Ticket` (cờ đã báo hoặc quay về `Processing`), ghi log `CUSTOMER_RESPONSE`. |
| **EVT-B12** | Gửi yêu cầu đóng ticket và hoàn tất bàn giao<br>*(Nhân viên / Owner / Quản lý)* | **Business** | Trạng thái `Resolved`; người dùng thao tác nhấn nút "Đóng Ticket". | **1. Hệ thống CRM:** Kiểm tra cổng đóng nghiêm ngặt (Closing Gates): Nếu ĐÃ thông báo khách VÀ ĐÃ bàn giao máy VÀ ĐÃ xong hết các việc mở $\rightarrow$ Cho phép đóng ticket (`Closed`) và khóa toàn bộ dữ liệu chỉ đọc. Nếu vi phạm bất kỳ điều kiện nào $\rightarrow$ Chặn đóng, báo rõ điều kiện còn thiếu.<br>**2. Nhân viên & Quản lý:** Cho khách kiểm tra máy thực tế, ký biên bản bàn giao, tải ảnh biên bản lên CRM trước khi bấm đóng ca. | Cập nhật `Support Ticket` sang `Closed` (khóa chỉnh sửa), lưu người đóng và ngày đóng, ghi log `CLOSE_TICKET_SUCCESS`. |
| **EVT-B13** | Yêu cầu mở lại (Reopen) ticket đã đóng (EX-08)<br>*(Khách hàng & Quản lý / CSKH chuỗi)* | **Business** | Ticket đã ở trạng thái `Closed`; khách mang máy quay lại phản ánh đúng lỗi cũ tái phát. | **1. Hệ thống CRM:** Nếu là Quản lý/CSKH chuỗi $\rightarrow$ Cho phép mở lại ticket cũ về `Verifying`, giữ nguyên mã ticket và toàn bộ lịch sử trước, tăng số lần mở lại (`reopen_count + 1`), bắt buộc gán Owner mới và hạn theo dõi mới. Nếu là nhân viên thường $\rightarrow$ Chặn thực hiện.<br>**2. Quản lý / CSKH chuỗi:** Thẩm định thực tế đúng lỗi cũ tái phát; phê duyệt mở lại hồ sơ; chỉ định nhân sự chuyên môn tiếp nhận lại từ đầu. | Cập nhật `Support Ticket` (`Verifying`, mở khóa chỉnh sửa, tăng biến đếm `reopen_count`, lý do mở lại), ghi log `REOPEN_TICKET_EX08`. |
| **EVT-T01** | Đến hạn theo dõi ticket trong ngày<br>*(Tiến trình chạy ngầm hàng ngày)* | **Temporal** | Kích hoạt tự động đầu ca sáng (08:00); quét các ticket chưa đóng có ngày hẹn đúng ngày hôm nay. | **1. Hệ thống CRM:** Tự động phát hiện các ticket có ngày hẹn theo dõi trùng ngày hôm nay $\rightarrow$ Gửi thông báo nhắc việc và đánh dấu "Đến hạn hôm nay" trên danh sách công việc của Owner.<br>**2. Owner:** Mở danh sách công việc đầu ngày, thực hiện liên hệ đối tác hoặc kiểm tra tiến độ đã hẹn. | Cập nhật trạng thái `Care Task` thành "Đến hạn hôm nay", tạo thông báo trên hệ thống. |
| **EVT-T02** | Quá hạn theo dõi mà chưa có cập nhật (EX-06)<br>*(Tiến trình chạy ngầm hàng ngày)* | **Temporal** | Kích hoạt tự động đầu ca sáng; quét các ticket chưa đóng có ngày hẹn nhỏ hơn ngày hiện tại và không có cập nhật mới. | **1. Hệ thống CRM:** Tự động phát hiện ticket quá hạn theo dõi $\rightarrow$ Bật cờ báo đỏ quá hạn và gửi cảnh báo khẩn cấp lên màn hình Quản lý cửa hàng (tuyệt đối không tự đóng ca hay tự lùi ngày).<br>**2. Owner & Quản lý:** Owner khẩn trương liên hệ xử lý; Quản lý can thiệp đôn đốc hoặc điều chuyển người phụ trách nếu cần. | Cập nhật cờ quá hạn (`is_overdue = 1`) trên `Support Ticket`, ghi log `TEMPORAL_OVERDUE_ALERT_EX06`. |
| **EVT-T03** | Định kỳ quét rà soát ticket chờ kéo dài<br>*(Tiến trình Cron chạy ngầm mỗi 24h)* | **Temporal** | Kích hoạt lúc 00:00 hàng ngày; quét các ticket có cờ chờ kéo dài quá 7 ngày. | **1. Hệ thống CRM:** Tự động lọc các ticket đang ở chế độ chờ (chờ khách hoặc chờ hãng) kéo dài quá 7 ngày $\rightarrow$ Tổng hợp danh sách cảnh báo trên báo cáo quản trị SLA cho CSKH cấp chuỗi.<br>**2. Quản lý chuỗi:** Rà soát các ca nghẽn đối tác bảo hành để liên hệ cấp quản lý của trung tâm hãng can thiệp xử lý. | Tạo bản ghi thống kê báo cáo SLA hàng ngày. |
| **EVT-T04** | Định kỳ quét phát hiện ticket mất chủ<br>*(Tiến trình Cron chạy ngầm mỗi 6h)* | **Temporal** | Kích hoạt định kỳ mỗi 6 giờ; quét các ticket chưa đóng nhưng bị trống người chịu trách nhiệm. | **1. Hệ thống CRM:** Tự động lọc các ticket chưa đóng bị trống người chịu trách nhiệm $\rightarrow$ Đưa vào danh sách khẩn "Cần phân công lại" trên màn hình Quản lý cửa hàng.<br>**2. Quản lý cửa hàng:** Truy cập ngay danh sách và chỉ định Owner mới để nối lại trách nhiệm phục vụ. | Cập nhật hiển thị trên Dashboard Quản lý, ghi log `UNASSIGNED_SWEEP_ALERT`. |

---

## 4. CHUYỂN ĐỔI TRẠNG THÁI (STATE TRANSITION)

Theo phương pháp luận Practice #7 của Karl Wiegers, biểu đồ chuyển đổi trạng thái (State Transition Diagram) và ma trận trạng thái (State Transition Matrix) là công cụ trực quan tốt nhất để kiểm tra tính toàn vẹn của vòng đời thực thể, giúp phát hiện sớm các trạng thái ngõ cụt, các bước chuyển đổi thiếu hoặc các vòng lặp sai lệch.

### 4.1. Vòng đời chuẩn 5 trạng thái của Ticket
Vòng đời của phiếu hỗ trợ (`Support Ticket`) vận hành qua đúng **5 trạng thái chính** theo quy chuẩn baseline:
1. **`New` (Mới tiếp nhận):** Phiếu vừa được tạo tại quầy, người tạo tạm thời chịu trách nhiệm (`temporary_owner`).
2. **`Verifying` (Đang xác minh):** Đã phân công đích danh Owner; Owner đang đối chiếu thông tin mua hàng, tình trạng thiết bị và chính sách bảo hành.
3. **`Processing` (Đang xử lý):** Đã xác định hướng xử lý; thực hiện can thiệp kỹ thuật nội bộ hoặc gửi sang trung tâm bảo hành của hãng.
   - *Chế độ con (Sub-state / Wait Reason):* Chờ khách phản hồi (`CHO_KHACH`) hoặc Chờ đối tác bảo hành thẩm định (`CHO_DON_VI_BAO_HANH`). Trạng thái chính vẫn là `Processing`.
4. **`Resolved` (Đã giải quyết):** Đã có kết quả chuyên môn thực tế; phát sinh các tác vụ liên hệ thông báo cho khách và chuẩn bị bàn giao máy.
5. **`Closed` (Đóng):** Đã hoàn tất thông báo và bàn giao máy thực tế, thỏa mãn toàn bộ điều kiện Closing Gates (BR-06); hệ thống khóa sổ dữ liệu chỉ đọc (`docstatus = 1`).

---

### 4.2. Sơ đồ chuyển đổi trạng thái (State Transition Diagram)

Biểu đồ Mermaid State Machine dưới đây mô hình hóa toàn diện vòng đời của ticket, bao gồm các chế độ con, nhánh tự chuyển đổi nội bộ khi điều chuyển cửa hàng (Self-transition), vòng lặp khiếu nại và mở lại ticket sau khi đã đóng:

```mermaid
stateDiagram-v2
    [*] --> New: EVT-B01 [Đủ dữ liệu tối thiểu BR-02]\n(Tiếp nhận tại CH02, docstatus=0)

    state New {
        [*] --> GhiNhanTam
        GhiNhanTam: Người tạo tạm chịu trách nhiệm (temporary_owner)
    }

    New --> Verifying: EVT-B02\n(Quản lý chỉ định Owner & Ngày hẹn theo dõi)

    state Verifying {
        [*] --> DoiChieuChinhSach
        DoiChieuChinhSach: Đối chiếu Purchase Reference,\nNgoại quan, IMEI & Chính sách
    }

    Verifying --> Verifying: EVT-B03 [EX-04]\n(CSKH chuỗi chuyển CH02 -> CH01, đổi quyền ghi, giữ Ticket ID)

    Verifying --> Processing: EVT-B04\n(Owner xác định hướng xử lý nghiệp vụ)

    state Processing {
        [*] --> DangXuLyNoiBo

        DangXuLyNoiBo --> CheDoCho: EVT-B05 [Cần linh kiện / thẩm định hãng]\n(Gán wait_reason & Ngày hẹn theo dõi)
        
        state CheDoCho {
            ChoKhach: Chờ khách bổ sung chứng từ / quyết định (EX-01, EX-05)
            ChoHangs: Chờ đối tác hãng thẩm định / sửa chữa (EX-05)
            --
            [*] --> ChoKhach
            [*] --> ChoHangs
        }

        CheDoCho --> DangXuLyNoiBo: EVT-B06 / EVT-B07 [Nhận kết quả / Hết chờ]\n(Xóa wait_reason, tiếp tục xử lý)
    }

    Processing --> Processing: EVT-B03 [EX-04]\n(CSKH chuỗi chuyển CH02 -> CH01, đổi quyền ghi, giữ Ticket ID)

    Processing --> Resolved: EVT-B10\n(Đã có kết quả thực tế, sinh task thông báo & bàn giao)

    state Resolved {
        [*] --> ThongBaoVaBanGiao
        ThongBaoVaBanGiao: Liên hệ khách (EVT-B11)\n& Bàn giao máy tại quầy
    }

    Resolved --> Processing: EVT-B11 [Khách KHÔNG đồng ý] (EX-07)\n(Ghi lý do khiếu nại, giữ nguyên kết quả cũ trong log)

    Resolved --> Closed: EVT-B12 [Thỏa mãn Closing Gates BR-06]\n(doc.submit() -> Khóa chỉ đọc docstatus=1)

    state Closed {
        [*] --> HoanTatLuuTru
        HoanTatLuuTru: Khóa chỉnh sửa dữ liệu ở tầng MariaDB
    }

    Closed --> Verifying: EVT-B13 [Lỗi cũ tái phát sau đóng] (EX-08)\n(Quản lý/CSKH chuỗi duyệt Reopen, giữ nguyên Ticket ID)

    Closed --> [*]
```

---

### 4.3. Ma trận chuyển đổi trạng thái (State Transition Matrix)

Bảng đối chiếu kiểm tra tính toàn vẹn (Completeness & Reachability Matrix) giữa 5 trạng thái chính gắn liền với cơ chế DocStatus của Frappe v15:

| Trạng thái nguồn (From State) | Trạng thái đích (To State) | Sự kiện kích hoạt (Triggering Event) | Điều kiện kiểm tra (Guard Condition) | Hành động hệ thống (System Action) |
| :--- | :--- | :--- | :--- | :--- |
| **`[*]` Bắt đầu** | `New` | **EVT-B01** | Thỏa mãn bộ dữ liệu tối thiểu (Khách hàng, Chi nhánh, Thiết bị) theo BR-02 | Tạo ticket với `docstatus = 0`, cấp mã `ticket_id`, gán `temporary_owner = session.user`. |
| `New` | `Verifying` | **EVT-B02** | Owner mới có tài khoản hoạt động và thuộc chi nhánh xử lý; có `follow_up_date` | Cập nhật `assigned_to_user`, xóa `temporary_owner = NULL`, chuyển trạng thái `Verifying`, tạo `Care Task`. |
| `Verifying` | `Verifying`<br>*(Self-transition)* | **EVT-B03 (EX-04)** | CSKH chuỗi có thẩm quyền; chọn chi nhánh đích và ghi rõ lý do chuyển | Đổi `processing_store = 'CH01'`, gán Owner mới, chuyển quyền `User Permission`, giữ nguyên `ticket_id`. |
| `Verifying` | `Processing` | **EVT-B04** | Đã ghi nhận căn cứ xác minh và chọn hướng xử lý (`resolution_path`) | Cập nhật trạng thái `Processing`, lưu vết `verified_by` và `verified_at`, cập nhật hạn theo dõi tiếp theo. |
| `Processing` | `Processing`<br>*(Self-transition)* | **EVT-B03 (EX-04)** | CSKH chuỗi có thẩm quyền; chọn chi nhánh đích và ghi rõ lý do chuyển | Đổi `processing_store = 'CH01'`, gán Owner mới, chuyển quyền `User Permission`, giữ nguyên `ticket_id`. |
| `Processing` | `Processing`<br>*(Chế độ Chờ)* | **EVT-B05** | Bắt buộc chọn `wait_reason`, nhập đơn vị phối hợp và ngày hẹn theo dõi | Gán `wait_reason`, tạo `Care Task` theo dõi đối tác ngoài, trạng thái chính vẫn là `Processing`. |
| `Processing` | `Processing`<br>*(Kết thúc Chờ)* | **EVT-B06 / EVT-B07** | Đã có phản hồi của đối tác/khách hoặc Owner xác nhận kết thúc chờ | Xóa cờ `wait_reason = NULL`, đóng `Care Task` chờ, tiếp tục tiến trình kỹ thuật nội bộ. |
| `Processing` | `Resolved` | **EVT-B10** | Đã nhập `resolution_result` thực tế; cờ chờ `wait_reason == NULL` | Cập nhật trạng thái `Resolved`, lưu vết `resolved_by`, tự động sinh 2 `Care Task` thông báo và bàn giao. |
| `Resolved` | `Processing`<br>*(Quay lại - Loop)* | **EVT-B11 (EX-07)** | Khách hàng từ chối phương án; người dùng có quyền cập nhật hồ sơ | Chuyển ngược `Processing`, bắt buộc ghi `dispute_reason`, tạo task khiếu nại, giữ nguyên kết quả cũ. |
| `Resolved` | `Closed` | **EVT-B12 (BR-06)** | Thỏa mãn đồng thời: `customer_notified == 1` VÀ (`device_returned == 1` nếu nhận máy) VÀ không còn task mở | Gọi `doc.submit()`, chuyển `docstatus = 1` (Khóa chỉ đọc MariaDB), cập nhật trạng thái `Closed`, lưu `closed_by`. |
| `Closed` | `Verifying`<br>*(Mở lại - Reopen)* | **EVT-B13 (EX-08)** | Người thực hiện là Quản lý hoặc CSKH chuỗi; có lý do mở lại và Owner mới | Giữ nguyên `ticket_id`, chuyển `docstatus = 0`, chuyển trạng thái `Verifying`, tăng `reopen_count += 1`. |

---

## 5. BẢNG ĐỐI CHIẾU 9 TÌNH HUỐNG NGOẠI LỆ (EX-01 ĐẾN EX-09)

Bảng đối chiếu cụ thể phản ứng của phần mềm CRM và trách nhiệm con người cho toàn bộ 9 tình huống bắt buộc theo tài liệu baseline:

| Mã | Tình huống ngoại lệ | Event tương ứng | Trạng thái kích hoạt | Phản ứng nghiệp vụ của CRM (Frappe v15) | Trách nhiệm con người |
| :---: | :--- | :---: | :---: | :--- | :--- |
| **EX-01** | Thiếu chứng từ / dữ liệu mua hàng tham chiếu | **EVT-B01** | `New` $\rightarrow$ `Verifying` | Cho phép lưu ticket ở chế độ `purchase_reference_status = 'Unverified'`; tạo task bổ sung thông tin; không tự cam kết quyền lợi. | Nhân viên tiếp nhận máy bình thường, ghi rõ phần thiếu, giải thích khách và không hứa hẹn miễn phí. |
| **EX-02** | Sản phẩm rơi vỡ, vào nước hoặc tranh chấp ngoại quan | **EVT-B01** | `New` $\rightarrow$ `Verifying` | Bắt buộc đính kèm tệp ảnh bằng chứng; mở trường ghi nhận ý kiến hai chiều; lưu cờ `has_damage_dispute = 1`. | Nhân viên chụp ảnh chi tiết các góc máy, lập biên nhận hiện trạng khách ký xác nhận, chuyển cấp chuyên môn thẩm định. |
| **EX-03** | Lỗi chập chờn, không tái hiện tại quầy | **EVT-B01** | `Verifying` $\rightarrow$ `Processing` | Ghi nhận cờ `issue_type = 'Intermittent'`, `reproducible = 0`; lưu mô tả điều kiện xuất hiện lỗi; gia hạn thời gian kiểm tra tăng cường. | Owner khai thác kỹ hoàn cảnh lỗi từ khách, gửi đơn vị bảo hành kiểm tra chuyên sâu, không vội kết luận máy bình thường. |
| **EX-04** | Cần chuyển ticket sang cửa hàng khác xử lý | **EVT-B03** | `Verifying` / `Processing` (Self-transition) | Giữ nguyên `ticket_id`, lịch sử và tệp đính kèm; cập nhật `processing_store`, điều chuyển `User Permission` theo chi nhánh. | CSKH chuỗi ghi rõ lý do chuyển, bàn giao đích danh cho nhân sự chi nhánh nhận, thông báo cho khách nếu cần. |
| **EX-05** | Đang chờ khách hoặc đơn vị bảo hành | **EVT-B05** | `Processing` (Chế độ chờ) | Ghi nhận `wait_reason`, ngày liên hệ gần nhất, ngày hẹn theo dõi; tạo `Care Task` nhắc việc tự động cho Owner. | Owner theo dõi sát mốc hẹn, chủ động liên hệ đối tác hoặc khách hàng khi sắp đến hạn. |
| **EX-06** | Đến hạn theo dõi nhưng chưa có phản hồi | **EVT-T01** & **EVT-T02** | `Processing` (Đến/Quá hạn) | Kích hoạt cảnh báo vàng/đỏ trên Desk UI; gửi cảnh báo Escalation cho Quản lý; cấm tự ý đóng hoặc tự gia hạn ngày. | Owner khẩn trương liên hệ đôn đốc; Quản lý can thiệp hỗ trợ nếu đối tác hãng chậm trễ kéo dài. |
| **EX-07** | Khách chưa đồng ý kết quả giải quyết | **EVT-B11** | `Resolved` $\rightarrow$ `Processing` | Chuyển ngược ticket về `Processing`; bắt buộc nhập `dispute_reason`; bảo toàn nguyên vẹn kết quả cũ trong lịch sử. | Owner lắng nghe thắc mắc, ghi nhận trung thực, báo cáo Quản lý để hội chẩn lại phương án thỏa đáng hơn. |
| **EX-08** | Vấn đề phát sinh lại sau khi đã đóng ticket | **EVT-B13** | `Closed` $\rightarrow$ `Verifying` | Quản lý/CSKH chuỗi duyệt Reopen; giữ nguyên `ticket_id`; chuyển về `Verifying`; `docstatus = 0`; gán owner và hạn mới. | Quản lý thẩm định đúng lỗi cũ, chỉ định nhân sự chuyên môn thụ lý lại, giải thích rõ quy trình cho khách. |
| **EX-09** | Đủ điều kiện đổi nhưng kho chi nhánh hết hàng | **EVT-B08** | `Processing` | Ghi nhận `exchange_issue = 'Out of Stock'`; cấm CRM tự động hoàn tiền/cấp voucher; kích hoạt Care Task cho Quản lý cửa hàng. | Owner báo cáo Quản lý; Quản lý liên hệ điều chuyển hàng liên kho hoặc tư vấn khách phương án thỏa thuận. |

---

## 6. MA TRẬN TRUY VẾT TỔNG HỢP (TRACEABILITY MATRIX)

Ma trận truy vết 4 chiều chứng minh tính liên kết logic chặt chẽ giữa kỳ vọng khách hàng (SERVQUAL Gap Model), yêu cầu người dùng (Use Cases), sự kiện/phản ứng hệ thống và mô hình dữ liệu kiểm soát của CRM:

| Kỳ vọng khách hàng & Khoảng lệch Gap (Võ Duy Mạnh) | Use Case người dùng (Nguyễn Đức Nhật) | Sự kiện & Phản ứng đối ứng (Trần Quang Huy) | Dữ liệu & Cổng kiểm soát Frappe CRM (TV4 Technical Design) |
| :--- | :--- | :--- | :--- |
| **G1.1 & G5.1 – Tiếp nhận thuận tiện:** Khách không muốn khai lại thông tin khi đến CH02; tiếp nhận ít bước. | **UC-01:** Tiếp nhận yêu cầu bảo hành hoặc đổi trả tại CH02 | **EVT-B01, EVT-S01 & EVT-S02:** Tra cứu liên chi nhánh; quét Barcode/NFC nạp nhanh dữ liệu; lưu tạo ticket. | Tra cứu `Customer` & `Purchase_History_Ref`; tạo `Support Ticket` (`receiving_store = 'CH02'`). |
| **G1.2, G2.5 & G5.2 – Thông tin chính xác:** Đúng người, đúng thiết bị, đúng lịch sử; tránh nhầm hồ sơ. | **UC-01-A1, UC-01-E3:** Xác thực khách và dữ liệu tối thiểu | **EVT-B01 (Validation & BR-01):** Validation Gate chặn lưu thiếu; cảnh báo nghi trùng SĐT không tự gộp. | Regex SĐT VN, `customer_id` ổn định, kiểm tra trường bắt buộc trên Form Desk v15 qua Controller `validate`. |
| **G1.3, G2.1, G3.2 & G5.4 – Trách nhiệm rõ:** Luôn có người chịu trách nhiệm; không bị "mất chủ" khi bàn giao. | **UC-02 (Bước 1):** Bàn giao trách nhiệm và xác nhận Owner | **EVT-B02 & EVT-T04:** Chặn gán owner sai quyền; chuyển giao có log, giải phóng `temporary_owner`; Cron quét mất chủ (6h). | Trường `assigned_to_user` bắt buộc; giải phóng `temporary_owner = NULL`; Scheduled task quét `unassigned`. |
| **G1.4, G2.3, G3.4 & G5.5 – Tiến độ minh bạch:** Khách biết đang ở bước nào; không bị bỏ quên khi chờ. | **UC-03:** Theo dõi tiến độ và xử lý trường hợp chờ | **EVT-B05, EVT-T01 & EVT-T02:** Gán `wait_reason` kèm hạn; nhắc việc đến hạn đầu ngày; cảnh báo đỏ quá hạn. | `wait_reason` (`CHO_KHACH`/`CHO_DON_VI_BAO_HANH`), `follow_up_date`, DocType `Care Task`, Workspace Alert. |
| **G1.5, G2.4 & G5.6 – Liên tục giữa hai cửa hàng:** CH01 và CH02 là một thể thống nhất; không làm lại từ đầu. | **UC-06 & UC-02-A4:** Chuyển giao ticket giữa hai cửa hàng | **EVT-B03 (EX-04):** Giữ nguyên `ticket_id` và lịch sử; đổi `processing_store`; điều chuyển User Permission. | Cập nhật `processing_store`, điều chỉnh `User Permission` theo chi nhánh, không tạo ticket mới. |
| **G1.6, G2.6, G3.6 & G5.7 – Kết quả & Đóng hoàn tất:** Kết quả minh bạch; chỉ đóng khi khách được thông báo và nhận máy. | **UC-04:** Thông báo và hoàn tất yêu cầu; đóng ticket | **EVT-B11 & EVT-B12:** Kiểm tra Closing Gates BR-06 chặn đóng thiếu; lưu bằng chứng bàn giao; gọi `doc.submit()`. | Cờ `customer_notified`, `device_returned`, kiểm tra hoàn thành `Care Task`, `docstatus = 1` Read-only lock. |
| **G2.7 & G4.5 – Xử lý ngoại lệ chuẩn:** Khách chưa đồng ý kết quả hoặc vấn đề tái phát sau đóng. | **UC-04-A1 & UC-05:** Khách từ chối kết quả; Mở lại ticket đã đóng | **EVT-B11 (EX-07) & EVT-B13 (EX-08):** Quay lại `Processing` hoặc mở lại về `Verifying`, bảo toàn lịch sử. | Chuyển ngược workflow có lưu lý do tranh chấp; tăng `reopen_count`; bảo toàn toàn bộ `Audit Trail Log` cũ. |

---

## 7. RÀ SOÁT CHECKLIST NGHIỆM THU (BASELINE CHECKLIST)

Bảng đối chiếu rà soát đảm bảo tuân thủ đầy đủ 9 tiêu chí của tài liệu baseline dùng chung:

- [x] **1. Dùng đúng case và hai cửa hàng:** Đúng mô hình CH01 (136 Nguyễn Thái Học) và CH02 (21A Hàng Bài); không phát sinh cửa hàng ngoài phạm vi.
- [x] **2. Không dùng số điện thoại làm khóa chính duy nhất:** `customer_id` ổn định độc lập; SĐT chỉ dùng để tra cứu và cảnh báo nghi trùng (BR-01 trong EVT-B01).
- [x] **3. Không áp đặt chính sách tùy tiện (1 đổi 1 30 ngày / SLA cố định):** Toàn bộ căn cứ thời gian và chính sách dựa trên thẩm định thực tế từng ca; hạn theo dõi `follow_up_date` là mốc liên hệ nội bộ.
- [x] **4. Khách hàng và đối tác không đăng nhập CRM trong MVP:** Mọi tương tác của khách hàng và đối tác ngoài đều được nhân viên/quản lý tiếp nhận và ghi nhận qua biểu mẫu CRM.
- [x] **5. Không biến tính năng Future-state thành hiện thực MVP:** Portal khách hàng, SMS/Zalo Brandname tự động, CSAT tự động và API real-time với đối tác được gắn nhãn Mức C (Future-state).
- [x] **6. Dùng đúng 5 trạng thái chính chuẩn mực:** `New` $\rightarrow$ `Verifying` $\rightarrow$ `Processing` $\rightarrow$ `Resolved` $\rightarrow$ `Closed`. Chờ khách và chờ đối tác là `wait_reason` (sub-status). `Closed` được bảo vệ bằng MariaDB Read-only lock (`docstatus = 1`).
- [x] **7. CRM không tự động thực hiện các tác vụ phi thực tế:** CRM không tự sửa chữa, không tự hoàn tiền, không tự xuất kho, không tự kết luận đủ điều kiện bảo hành chỉ dựa vào ngày mua.
- [x] **8. Mọi ngoại lệ (EX-01 đến EX-09) đều nêu rõ phản ứng CRM và trách nhiệm con người:** Được phân tích chi tiết trong Bảng Event–Response Table và Bảng đối chiếu Mục 5.
- [x] **9. Phân định rõ ràng mức độ thông tin (A / B / C):** Được nêu rõ ràng tại Mục 1.2 và duy trì xuyên suốt tài liệu.

---
*Tài liệu hoàn thành ngày 09 tháng 10 năm 2026 bởi Trần Quang Huy (MSSV: 24520706 – TV4).*
