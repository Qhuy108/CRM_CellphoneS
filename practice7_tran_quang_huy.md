# PRACTICE 7: XÁC ĐỊNH SỰ KIỆN VÀ PHẢN ỨNG HỆ THỐNG (EVENTS & RESPONSES)
## QUẢN TRỊ MỐI QUAN HỆ KHÁCH HÀNG (CRM) – HỆ THỐNG BÁN LẺ CELLPHONES
### Case Study: Tiếp nhận và theo dõi yêu cầu bảo hành / đổi trả tại hai cửa hàng

- **Thành viên thực hiện:** Trần Quang Huy (MSSV: 24520706 – TV4)
- **Học phần:** Quản trị mối quan hệ khách hàng (CRM)
- **Nền tảng công nghệ đối ứng:** Frappe Framework v15 (Custom App `cellphones_crm` – MariaDB)
- **Phương pháp luận:** *Software Requirements Essentials: Core Practices for Successful Business Analysis* (Karl Wiegers & Candase Hokanson) – Chapter 3, Practice #7.
- **Tài liệu nền tảng tích hợp:**
  - `baseline_case_study_crm_cellphones.md` (Baseline dùng chung)
  - `nội dung của mạnh.md` (Gaps Model SERVQUAL – Võ Duy Mạnh)
  - `nôi dung của nhật.md` (Practice 6 User Requirements – Nguyễn Đức Nhật)
  - `PROJECT_OVERVIEW.md` & Bộ hồ sơ kỹ thuật D01–D05 (Trần Quang Huy)

---

## 1. PHẠM VI VÀ CƠ SỞ MÔ HÌNH HÓA

### 1.1. Bối cảnh và kịch bản chuẩn (Standard Workflow)
Hệ thống CRM mô phỏng quy trình hậu mãi tại **hai chi nhánh cửa hàng**:
- **Cửa hàng 01 (CH01):** CellphoneS 136 Nguyễn Thái Học, TP.HCM (Nơi phát sinh giao dịch mua hàng ban đầu).
- **Cửa hàng 02 (CH02):** CellphoneS 21A Hàng Bài, Hà Nội (Nơi khách hàng mang thiết bị đến yêu cầu hỗ trợ).

**Kịch bản nghiệp vụ xuyên suốt (Happy Path):**
> *Khách hàng đã mua thiết bị tại CH01, sau đó mang thiết bị đến CH02 yêu cầu bảo hành/đổi trả. Nhân viên tại CH02 tra cứu dữ liệu mua hàng tham chiếu, lập ticket, ghi nhận tình trạng thiết bị. Ticket được phân công Owner và chuyển xác minh chính sách. Sau đó, CH02 chuyển ticket sang trạng thái xử lý/phối hợp với đơn vị bảo hành ngoài (có thể kích hoạt chế độ chờ có hạn theo dõi). Khi nhận được kết quả chuyên môn, ticket chuyển sang "Đã giải quyết". Nhân viên liên hệ thông báo cho khách hàng, thực hiện bàn giao thiết bị thực tế, đối chiếu điều kiện đóng và đóng ticket. Toàn bộ lịch sử xử lý, phân công và tương tác được lưu trữ nguyên vẹn.*

### 1.2. Nguyên tắc phân định mức độ thông tin (A / B / C)
Tuân thủ tuyệt đối quy tắc của baseline:
- **Mức A (Có căn cứ):** Mô hình hai cửa hàng; quy trình hậu mãi 6 giai đoạn; 5 trạng thái chính của ticket; 9 tình huống ngoại lệ EX-01 đến EX-09; các nhóm actor xác định.
- **Mức B (Mô hình hóa / Đề xuất nghiệp vụ):** Cơ chế người tạo tạm chịu trách nhiệm (`temporary_owner`); kiểm soát điều kiện đóng cổng nghiêm ngặt (`Closing Gates`); tách biệt trạng thái chính và lý do chờ (`wait_reason`); quy tắc chuyển giao liên cửa hàng giữ nguyên mã `ticket_id`.
- **Mức C (Future-state – Không thuộc MVP):** Cổng tra cứu trực tuyến dành cho khách hàng (Customer Portal); kết nối API real-time với hệ thống hãng/Điện Thoại Vui; Webhook tự động từ đối tác giao vận; gửi SMS/Zalo Brandname tự động; khảo sát CSAT tự động. *(Chỉ nêu ở hướng mở rộng kiến trúc, không đưa vào xử lý lõi của MVP).*

### 1.3. Cơ sở lý thuyết Practice #7 (Karl Wiegers & Candase Hokanson) & Kiến trúc Frappe v15
Theo Karl Wiegers (*Software Requirements Essentials* – Chapter 3, Practice #7: *Identify Events and Responses*), Use Cases (Practice #6) rất hiệu quả cho tương tác người – máy trực tiếp, nhưng không bao quát hết các hành vi kích hoạt ngầm, cảm biến phần cứng, tín hiệu ngắt kỹ thuật và cảnh báo thời gian. Phương pháp luận phân loại sự kiện kích hoạt phản ứng của hệ thống thành **3 nhóm chuẩn mực**:
1. **Business Event (Sự kiện nghiệp vụ):** Do con người (khách hàng, nhân viên tiếp nhận, kỹ thuật viên/owner, quản lý cửa hàng, CSKH chuỗi) hoặc đối tác nghiệp vụ bên ngoài chủ động khởi xướng nhằm thực hiện một giao dịch nghiệp vụ trong domain (tiếp nhận lập ticket, phân công owner, chuyển trạng thái xử lý, đóng ticket, khiếu nại, mở lại ticket...).
2. **Signal Event (Sự kiện tín hiệu ngoại vi):** Xuất phát từ **thiết bị phần cứng, cảm biến ngoại vi tại quầy** (máy quét Barcode/QR quét mã IMEI/Serial, đầu đọc thẻ thông minh quẹt thẻ hội viên) gửi tín hiệu ngắt trực tiếp vào giao diện nhập liệu.
3. **Temporal Event (Sự kiện thời gian):** Do bộ lập lịch hệ thống (Frappe Daily Scheduled Hook / Cron Scheduler) kích hoạt tự động theo mốc thời gian chạy ngầm định kỳ đầu ngày hoặc theo chu kỳ quét (đến hạn nhắc việc trong ngày, quá hạn SLA theo dõi EX-06, tác vụ quét định kỳ ticket chờ lâu, tác vụ quét định kỳ ticket mất chủ) — **chứ không phải hệ thống tự nhận biết theo thời gian thực**.

> [!IMPORTANT]
> **Quy chuẩn phân định ranh giới (Guard Conditions vs Signal Events):**
> Các tác vụ kiểm tra dữ liệu bắt buộc (Validation Gate), kiểm tra quyền hạn (Permission Check) hoặc kiểm tra điều kiện đóng cổng (Closing Gates BR-06) là **Điều kiện kiểm tra (Guard Conditions)** và **Phản ứng hệ thống (System Responses)** nằm trong quá trình xử lý một *Business Event*, không phải là một *Signal Event* độc lập.
> 
> **Hiện thực hóa trên Frappe Framework v15:**
> - **DocType & DocStatus:** DocType `Support Ticket` duy trì `docstatus = 0` (Draft) trong suốt quá trình xử lý; khi chuyển `Closed` thỏa mãn BR-06, hệ thống gọi `doc.submit()` chuyển `docstatus = 1` (Submitted - Read-only Lock ở tầng MariaDB).
> - **Controller Hooks (Python):** Phương thức `validate` / `before_save` thực hiện kiểm tra nghiệp vụ; ném `frappe.throw()` để rollback transaction khi vi phạm điều kiện cổng; dùng `frappe.msgprint()` để cảnh báo không chặn (BR-01).
> - **Scheduler Events (`hooks.py`):** Cấu hình cron định kỳ gọi các background tasks (`daily_sla_sweep`, `unassigned_ticket_sweep`).
> - **Desk Client Script (JavaScript):** Bắt trực tiếp sự kiện bàn phím/HID từ máy quét mã vạch 2D và đầu đọc thẻ tại quầy tiếp nhận.

---

## 2. DANH MỤC SỰ KIỆN CHI TIẾT (EVENT LIST)

Hệ thống sự kiện trong phạm vi MVP bao gồm **19 sự kiện cốt lõi** được chuẩn hóa tinh gọn, không phân mảnh, mã hóa có hệ thống: **EVT-Bxx** (13 Business Events), **EVT-Sxx** (2 Signal Events phần cứng quầy), **EVT-Txx** (4 Temporal Events chạy ngầm), gắn kết trực tiếp với Use Cases (UC-01 đến UC-06 của Nhật) và 9 ngoại lệ (EX-01 đến EX-09 của baseline).

```
                              ┌──────────────────────────────────────────────┐
                              │   HỆ THỐNG SỰ KIỆN CRM CELLPHONES (19 EVT MVP)│
                              └──────────────────────┬───────────────────────┘
                                                     │
            ┌────────────────────────────────────────┼────────────────────────────────────────┐
            │                                        │                                        │
┌───────────▼────────────┐              ┌────────────▼───────────┐              ┌─────────────▼──────────┐
│    BUSINESS EVENTS     │              │  SIGNAL EVENTS (MVP)   │              │    TEMPORAL EVENTS     │
│  (13 Sự kiện nghiệp vụ)│              │  (2 Sự kiện phần cứng) │              │  (4 Sự kiện thời gian) │
├────────────────────────┤              ├────────────────────────┤              ├────────────────────────┤
│ EVT-B01: Tiếp nhận quầy│              │ EVT-S01: Quét Barcode  │              │ EVT-T01: Đến hạn nhắc  │
│ EVT-B02: Gán Owner     │              │          IMEI/Serial   │              │          (Due Today)   │
│ EVT-B03: Chuyển CH     │              │ EVT-S02: Quẹt thẻ quầy │              │ EVT-T02: Quá hạn SLA   │
│          (EX-04)       │              │          hội viên      │              │          (Overdue EX06)│
│ EVT-B04: Hướng xử lý   │              └────────────────────────┘              │ EVT-T03: Cron quét     │
│ EVT-B05: Vào chế độ chờ│                                                      │          chờ lâu (24h) │
│ EVT-B06: Nhận KQ hãng  │                                                      │ EVT-T04: Cron quét mất │
│ EVT-B07: Hết thời gian │                                                      │          chủ (6h)      │
│          chờ           │                                                      └────────────────────────┘
│ EVT-B08: Thiếu máy đổi │
│          (EX-09)       │
│ EVT-B09: Khách rút ca  │
│ EVT-B10: Chốt kết quả  │
│ EVT-B11: Khách phản hồi│
│          (Đồng ý/EX-07)│
│ EVT-B12: Đóng ticket   │
│          (BR-06)       │
│ EVT-B13: Reopen ticket │
│          (EX-08)       │
└────────────────────────┘
```

### 2.1. Nhóm Sự kiện Nghiệp vụ (Business Events)
Các sự kiện giao dịch do tác nhân con người chủ động kích hoạt (Khách hàng, Nhân viên tiếp nhận, Kỹ thuật viên/Owner, Quản lý cửa hàng, CSKH cấp chuỗi hoặc Đơn vị bảo hành ngoài):

| Mã Event | Tên sự kiện & Tác nhân kích hoạt | Mô tả bản chất nghiệp vụ & Ngữ cảnh | Liên kết UC & Baseline |
| :---: | :--- | :--- | :---: |
| **EVT-B01** | Tiếp nhận và gửi yêu cầu tạo Ticket tại quầy CH02<br>*(Tác nhân: Khách hàng & Nhân viên tiếp nhận)* | Khách hàng mang thiết bị đến CH02 (mua tại CH01), nhân viên tra cứu lịch sử, thu thập thông tin, kiểm tra ngoại quan/chứng từ và gửi yêu cầu lưu ticket trên phần mềm CRM. | UC-01; EX-01; EX-02; EX-03; BR-01; BR-02 |
| **EVT-B02** | Phân công Owner chịu trách nhiệm chính và thiết lập hạn theo dõi<br>*(Tác nhân: Quản lý / CSKH chuỗi)* | Người có thẩm quyền lựa chọn đích danh nhân sự xử lý và ấn định ngày hẹn theo dõi (`follow_up_date`) để chuyển giao ca từ Mới tiếp nhận sang Đang xác minh. | UC-02 (bước 1-2); UC-02-E4; BR-02 |
| **EVT-B03** | Điều phối chuyển ticket xử lý giữa hai cửa hàng (EX-04 / UC-06)<br>*(Tác nhân: CSKH cấp chuỗi)* | Thiết bị hoặc hồ sơ cần được chuyển giao trách nhiệm xử lý từ CH02 về CH01 hoặc ngược lại để thuận tiện nghiệp vụ; giữ nguyên mã ticket và bảo toàn lịch sử. | EX-04; UC-06; UC-02-A4 |
| **EVT-B04** | Owner hoàn tất xác minh chính sách và xác định hướng xử lý nghiệp vụ<br>*(Tác nhân: Ticket Owner)* | Owner hoàn tất đối chiếu chính sách, ghi nhận căn cứ và chọn chuyển ticket từ giai đoạn xác minh sang thực thi/phối hợp (`Processing`). | UC-02 (bước 4) |
| **EVT-B05** | Owner kích hoạt chế độ chờ xử lý bên ngoài (EX-05)<br>*(Tác nhân: Ticket Owner)* | Phát sinh nhu cầu chờ linh kiện, chờ thẩm định hãng hoặc chờ khách bổ sung thông tin; Owner cập nhật mã lý do chờ (`wait_reason`) và ngày hẹn theo dõi mới. | EX-05; UC-03 (bước 3); BR-03 |
| **EVT-B06** | Đơn vị bảo hành ngoài / Hãng phản hồi kết quả kiểm định qua kênh liên lạc<br>*(Tác nhân: Đối tác bảo hành ngoài / Owner ghi nhận)* | Trung tâm bảo hành đối tác gửi biên bản kết quả thẩm định, phương án sửa chữa hoặc từ chối bảo hành qua email/điện thoại; Owner ghi nhận thủ công vào CRM. | Giai đoạn 4; UC-02 (bước 6); UC-03 |
| **EVT-B07** | Owner xác nhận kết thúc thời gian chờ xử lý<br>*(Tác nhân: Ticket Owner)* | Nhận được đầy đủ thông tin phản hồi từ khách hoặc đối tác, Owner giải phóng cờ chờ (`wait_reason = NULL`) để tiếp tục xử lý nghiệp vụ. | UC-03 (bước 6); BR-03 |
| **EVT-B08** | Báo trở ngại đủ điều kiện đổi sản phẩm nhưng kho chi nhánh hết hàng (EX-09)<br>*(Tác nhân: Nhân viên / Quản lý kho)* | Kết quả xác minh xác định đủ điều kiện đổi mới 1-1, nhưng model máy tương đương đang tạm hết hàng tại chi nhánh; mở luồng chuyển Quản lý duyệt phương án. | EX-09; UC-02-E3 |
| **EVT-B09** | Khách hàng chủ động yêu cầu rút lại yêu cầu hỗ trợ<br>*(Tác nhân: Khách hàng)* | Khách hàng thay đổi ý định, không muốn tiếp tục gửi máy hoặc tự xử lý bên ngoài; CRM chuyển hướng luồng hoàn trả thiết bị nguyên trạng, không xóa ticket. | UC-02-A3 |
| **EVT-B10** | Owner chốt kết quả xử lý thực tế và chuyển sang Đã giải quyết<br>*(Tác nhân: Ticket Owner)* | Owner cập nhật kết luận xử lý thực tế (`resolution_result`), căn cứ đính kèm và người phê duyệt để chuyển trạng thái từ `Processing` sang `Resolved`. | UC-02 (bước 7); BR-06 |
| **EVT-B11** | Ghi nhận kết quả thông báo và phản hồi của khách hàng<br>*(Tác nhân: Khách hàng & Nhân viên liên hệ)* | Nhân viên liên hệ giải thích phương án xử lý: Khách hàng xác nhận đồng ý (Happy path) hoặc Khách hàng khiếu nại từ chối (EX-07 quay lại `Processing`). | Giai đoạn 5; UC-04 (bước 3); EX-07; UC-04-A1 |
| **EVT-B12** | Gửi yêu cầu Đóng ticket và hoàn tất bàn giao máy<br>*(Tác nhân: Nhân viên / Owner / Quản lý)* | Người phụ trách yêu cầu đóng ca phục vụ sau khi khách đã nhận máy và được thông báo; hệ thống kiểm tra toàn bộ điều kiện Closing Gates BR-06 trước khi khóa sổ. | UC-04 (bước 4-6); UC-04-E1; BR-06; D05 |
| **EVT-B13** | Yêu cầu Mở lại (Reopen) ticket đã đóng do lỗi cũ tái phát (EX-08 / UC-05)<br>*(Tác nhân: Khách hàng & Quản lý cửa hàng / CSKH chuỗi)* | Khách hàng mang máy quay lại phản ánh cùng lỗi cũ; cấp có thẩm quyền phê duyệt mở lại ticket đã đóng, bảo toàn toàn bộ lịch sử ca cũ. | EX-08; UC-05; UC-05-E1; BR-07 |

### 2.2. Nhóm Sự kiện Tín hiệu Phần cứng Quầy (Signal Events - Phạm vi MVP)
Các sự kiện xuất phát từ thiết bị phần cứng, đầu đọc ngoại vi tại quầy tiếp nhận cửa hàng:

| Mã Event | Tên sự kiện & Nguồn phát tín hiệu | Mô tả cơ chế ngắt tín hiệu ngoại vi | Hiện thực hóa Frappe v15 |
| :---: | :--- | :--- | :--- |
| **EVT-S01** | Quét Barcode/QR IMEI/Serial thiết bị thành công<br>*(Nguồn: Máy quét mã vạch 2D tại quầy)* | Đầu đọc barcode bắn tín hiệu ngắt qua cổng HID/USB/COM chứa chuỗi ký tự mã định danh máy khi nhân viên quét vỏ hộp/thân máy. | Desk Client Script bắt sự kiện scanner, tự động điền trường `imei_serial` và gọi API tìm kiếm đơn hàng tham chiếu. |
| **EVT-S02** | Quẹt thẻ thành viên Smember qua cảm biến đầu đọc quầy<br>*(Nguồn: Đầu đọc thẻ thông minh tại quầy)* | Cảm biến đầu đọc nhận diện mã thẻ khách hàng, truyền mã thẻ về giao diện Desk để truy xuất thông tin hội viên. | Desk Client Script nhận mã thẻ, tự động truy vấn `Customer` và nạp dữ liệu khách hàng lên màn hình quầy. |

> [!NOTE]
> **Kiến trúc Mở rộng Tương lai (Future-state Extension – Mức C):**
> Các sự kiện tích hợp Webhook thời gian thực từ Cổng bảo hành Hãng (`EVT-S03: Apple GSX / Samsung Webhook`) và Hệ thống Logistics đối tác (`EVT-S04: Logistics Webhook`) được định vị là các module mở rộng giai đoạn sau (Mức C), hoàn toàn độc lập và không bắt buộc trong phiên bản MVP theo đúng Baseline Mục 4.2.

### 2.3. Nhóm Sự kiện Thời gian (Temporal Events)
Các sự kiện kích hoạt tự động theo tiến trình chạy ngầm định kỳ (Frappe Scheduled Tasks / Cron Hooks) chứ **không phải hệ thống tự nhận biết theo thời gian thực**:

| Mã Event | Tên sự kiện & Điều kiện thời gian | Cơ chế kích hoạt hệ thống (Trigger Mechanism) | Hiện thực hóa Frappe v15 |
| :---: | :--- | :--- | :--- |
| **EVT-T01** | Đến hạn theo dõi ticket (`follow_up_date` đến hạn trong ngày) | **Kích hoạt bởi tiến trình chạy ngầm hàng ngày (Frappe Daily Scheduled Hook)** vào đầu ngày (00:00 hoặc 08:00 sáng), quét các ticket đang mở có `follow_up_date == current_date`. *(Không phải hệ thống tự nhận biết theo thời gian thực).* | `hooks.py` $\rightarrow$ `scheduler_events["daily"]`: chạy hàm `cellphones_crm.tasks.check_due_tickets()`, gắn cờ `Due Today` trên `Care Task`, phát Desk Notification cho Owner. |
| **EVT-T02** | Quá hạn theo dõi mà ticket chưa được cập nhật (`follow_up_date < current_date`) | **Kích hoạt bởi tiến trình chạy ngầm hàng ngày (Frappe Daily Scheduled Hook)** vào đầu ngày (00:00 hoặc 08:00 sáng), quét các ticket chưa đóng có `follow_up_date < current_date` và không có log cập nhật mới (EX-06). *(Không phải hệ thống tự nhận biết theo thời gian thực).* | `hooks.py` $\rightarrow$ `scheduler_events["daily"]`: chạy hàm `cellphones_crm.tasks.escalate_overdue_tickets()`, bật cờ `is_overdue = 1`, gửi Escalation Alert lên Dashboard Quản lý. |
| **EVT-T03** | Định kỳ quét rà soát các ticket có trạng thái chờ kéo dài (Long-waiting Audit Sweep) | **Kích hoạt bởi tiến trình Cron chạy ngầm mỗi 24 giờ** (lúc 00:00 hàng ngày), quét các ticket đang ở chế độ chờ đối tác hoặc chờ khách có `wait_reason != NULL` kéo dài quá 7 ngày. | `hooks.py` $\rightarrow$ `scheduler_events["cron"] = {"0 0 * * *"}`: xuất danh sách cảnh báo trên báo cáo quản trị SLA cho CSKH cấp chuỗi. |
| **EVT-T04** | Định kỳ quét phát hiện ticket không có Owner phụ trách (Unassigned Sweep) | **Kích hoạt bởi tiến trình Cron chạy ngầm mỗi 6 giờ** (`0 */6 * * *`), rà soát các ticket ở trạng thái `New` hoặc `Verifying` nhưng bị trống người chịu trách nhiệm (`assigned_to_user IS NULL`). | `hooks.py` $\rightarrow$ `scheduler_events["cron"] = {"0 */6 * * *"}`: gom ticket vào danh mục khẩn cấp "Ticket cần phân công lại" của Quản lý cửa hàng. |

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

## 4. BIỂU ĐỒ CHUYỂN ĐỔI TRẠNG THÁI (STATE TRANSITION DIAGRAM)

Theo Karl Wiegers (Practice #7), State Transition Diagram là mô hình trực quan bổ trợ đắc lực nhất cho Event–Response Table, giúp phát hiện các trạng thái "chết", các nhánh chuyển đổi thiếu hoặc các vòng lặp sai lệch.

### 4.1. Sơ đồ trạng thái chuẩn (Mermaid State Machine)
Biểu đồ dưới đây biểu diễn chính xác vòng đời ticket qua đúng **5 trạng thái chính** theo baseline:
`New` (Mới tiếp nhận) $\rightarrow$ `Verifying` (Đang xác minh) $\rightarrow$ `Processing` (Đang xử lý) $\rightarrow$ `Resolved` (Đã giải quyết) $\rightarrow$ `Closed` (Đóng - Read-only Lock `docstatus = 1`).
Trong đó:
- **Chờ khách** và **Chờ đối tác bảo hành** được mô hình hóa chuẩn mực dưới dạng **Chế độ con (Sub-state / Wait Reason)** nằm trong trạng thái `Processing`.
- **Nhánh điều chuyển liên cửa hàng (EVT-B03 / EX-04)** được biểu diễn trực quan qua **Self-transition (Tự chuyển đổi nội trạng thái)** tại `Verifying` và `Processing`.

```mermaid
stateDiagram-v2
    [*] --> New: EVT-B01 [Lưu hợp lệ BR-02]\n(Tiếp nhận quầy CH02, docstatus=0)

    state New {
        [*] --> GhiNhanTam
        GhiNhanTam: Người tạo tạm chịu trách nhiệm (temporary_owner)
    }

    New --> Verifying: EVT-B02\n(Quản lý gán Owner hợp lệ & Hạn theo dõi)

    state Verifying {
        [*] --> KiemTraChinhSach
        KiemTraChinhSach: Đối chiếu Purchase Reference,\nNgoại quan, IMEI & Chính sách
    }

    Verifying --> Verifying: EVT-B03 (EX-04: CSKH chuỗi chuyển CH02 -> CH01,\nđổi User Permission, giữ nguyên Ticket ID)

    Verifying --> Processing: EVT-B04\n(Owner xác định hướng xử lý nghiệp vụ)

    state Processing {
        [*] --> DangThucHienNoiBo

        DangThucHienNoiBo --> CheDoCho: EVT-B05 [Cần thông tin/kiểm định]\n(Gán wait_reason & hạn theo dõi)
        
        state CheDoCho {
            ChoKhach: Chờ khách bổ sung (EX-01, EX-05)
            ChoDoiTac: Chờ hãng kiểm định (EX-05)
            --
            [*] --> ChoKhach
            [*] --> ChoDoiTac
        }

        CheDoCho --> DangThucHienNoiBo: EVT-B06 / EVT-B07 [Nhận phản hồi/Kết thúc chờ]\n(Xóa wait_reason, tiếp tục xử lý)
    }

    Processing --> Processing: EVT-B03 (EX-04: CSKH chuỗi chuyển CH02 -> CH01,\nđổi User Permission, giữ nguyên Ticket ID)

    Processing --> Resolved: EVT-B10\n(Có kết quả thực tế, sinh task thông báo & bàn giao)

    state Resolved {
        [*] --> ThongBaoVaBanGiao
        ThongBaoVaBanGiao: Liên hệ khách (EVT-B11)\n& Bàn giao máy (EVT-B12)
    }

    Resolved --> Processing: EVT-B11 [Khách KHÔNG đồng ý] (EX-07)\n(Ghi lý do khiếu nại, giữ nguyên kết quả cũ)

    Resolved --> Closed: EVT-B12 [Thỏa mãn Closing Gates BR-06]\n(doc.submit() -> docstatus=1 Read-only Lock)

    state Closed {
        [*] --> HoanTatLuuTru
        HoanTatLuuTru: Khóa chỉnh sửa dữ liệu ở tầng MariaDB
    }

    Closed --> Verifying: EVT-B13 [Lỗi tái phát sau đóng] (EX-08)\n(Quản lý/CSKH chuỗi duyệt Reopen, giữ nguyên Ticket ID)

    Closed --> [*]
```

### 4.2. Ma trận chuyển đổi trạng thái (State Transition Matrix)
Bảng kiểm tra tính toàn vẹn (Completeness & Reachability Matrix) giữa 5 trạng thái chính gắn liền với cơ chế DocStatus của Frappe v15:

| Từ trạng thái (From State) | Đến trạng thái (To State) | Sự kiện kích hoạt (Triggering Event) | Điều kiện kiểm tra (Guard Condition) | Hành động hệ thống (Frappe System Action) |
| :--- | :--- | :--- | :--- | :--- |
| **`[*]` Khởi tạo** | `New` | **EVT-B01** | Thỏa mãn minimum dataset (Customer, Store, Device) theo BR-02 | `docstatus = 0`, cấp mã `ticket_id`, gán `temporary_owner = session.user`, ghi audit log. |
| `New` | `Verifying` | **EVT-B02** | Owner mới có tài khoản hoạt động và có quyền tại chi nhánh; có `follow_up_date` | Cập nhật `assigned_to_user`, giải phóng `temporary_owner = NULL`, chuyển `workflow_state = 'Verifying'`, insert `Care Task`. |
| `Verifying` | `Verifying` *(Self-transition)* | **EVT-B03 (EX-04)** | CSKH chuỗi có thẩm quyền điều phối; chọn chi nhánh đích và lý do chuyển | Cập nhật `processing_store = 'CH01'`, chuyển giao `User Permission`, giữ nguyên `ticket_id`. |
| `Verifying` | `Processing` | **EVT-B04** | Đã ghi nhận căn cứ xác minh và chọn hướng xử lý (`resolution_path`) | Cập nhật `workflow_state = 'Processing'`, lưu `verified_by` và `verified_at`. |
| `Processing` | `Processing` *(Self-transition)* | **EVT-B03 (EX-04)** | CSKH chuỗi có thẩm quyền điều phối; chọn chi nhánh đích và lý do chuyển | Cập nhật `processing_store = 'CH01'`, chuyển giao `User Permission`, giữ nguyên `ticket_id`. |
| `Processing` | `Processing` *(Sub-state Chờ)* | **EVT-B05** | Bắt buộc chọn `wait_reason`, nhập đơn vị phối hợp và ngày hẹn | Gán `wait_reason`, tạo `Care Task` theo dõi tiến độ ngoài. |
| `Processing` | `Resolved` | **EVT-B10** | Đã nhập `resolution_result` thực tế; `wait_reason == NULL` | Chuyển `workflow_state = 'Resolved'`, tự động sinh 2 `Care Task` thông báo và bàn giao. |
| `Resolved` | `Processing` *(Quay lại - Loop)* | **EVT-B11 (EX-07)** | Khách hàng từ chối phương án; người dùng có quyền cập nhật | Chuyển ngược `Processing`, bắt buộc ghi `dispute_reason`, tạo task khiếu nại, giữ kết quả cũ. |
| `Resolved` | `Closed` | **EVT-B12 (BR-06)** | `customer_notified == 1` VÀ (`device_returned == 1` nếu nhận máy) VÀ không còn task mở | Gọi `doc.submit()`, chuyển `docstatus = 1` (Read-only), `workflow_state = 'Closed'`, lưu `closed_by`. |
| `Closed` | `Verifying` *(Mở lại - Reopen)* | **EVT-B13 (EX-08)** | Người thực hiện là Quản lý hoặc CSKH chuỗi; có lý do mở lại và Owner mới | Giữ `ticket_id`, chuyển `docstatus = 0`, `workflow_state = 'Verifying'`, tăng `reopen_count`. |

---

## 5. MA TRẬN TRUY VẾT TOÀN DIỆN (TRACEABILITY MATRIX)

Ma trận truy vết 4 chiều chứng minh tính liên kết chặt chẽ giữa:
$$\text{Customer Expectations \& Gaps Model (Mạnh)} \longleftrightarrow \text{User Use Cases (Nhật)} \longleftrightarrow \text{Events \& Responses (Huy)} \longleftrightarrow \text{Dữ liệu \& Cổng kiểm soát Frappe CRM}$$

| Kỳ vọng khách hàng & Khoảng lệch Gap (Võ Duy Mạnh) | Use Case người dùng (Nguyễn Đức Nhật) | Sự kiện & Phản ứng đối ứng (Trần Quang Huy) | Dữ liệu & Cổng kiểm soát Frappe CRM (TV4 Technical Design) |
| :--- | :--- | :--- | :--- |
| **G1.1 & G5.1 – Tiếp nhận thuận tiện:** Khách không muốn khai lại thông tin khi đến CH02; tiếp nhận ít bước. | **UC-01:** Tiếp nhận yêu cầu bảo hành hoặc đổi trả tại CH02 | **EVT-B01, EVT-S01 & EVT-S02:** Tìm kiếm liên chi nhánh; quét Barcode/NFC nạp nhanh dữ liệu; lưu tạo ticket. | Tra cứu `Customer` & `Purchase_History_Ref`; tạo `Support Ticket` (`receiving_store = 'CH02'`). |
| **G1.2, G2.5 & G5.2 – Thông tin chính xác:** Đúng người, đúng thiết bị, đúng lịch sử; tránh nhầm hồ sơ. | **UC-01-A1, UC-01-E3:** Xác thực khách và dữ liệu tối thiểu | **EVT-B01 (Validation & BR-01):** Validation Gate `frappe.throw` chặn lưu thiếu; cảnh báo nghi trùng SĐT không tự gộp. | Regex SĐT VN, `customer_id` ổn định, kiểm tra trường bắt buộc trên Form Desk v15 qua Controller `validate`. |
| **G1.3, G2.1, G3.2 & G5.4 – Trách nhiệm rõ:** Luôn có người chịu trách nhiệm; không bị "mất chủ" khi bàn giao. | **UC-02 (Bước 1):** Bàn giao trách nhiệm và xác nhận Owner | **EVT-B02 & EVT-T04:** Chặn gán owner sai quyền; chuyển giao có log, giải phóng `temporary_owner`; Cron quét định kỳ ticket mất chủ (6h). | Trường `assigned_to_user` bắt buộc; giải phóng `temporary_owner = NULL`; Scheduled task quét `unassigned`. |
| **G1.4, G2.3, G3.4 & G5.5 – Tiến độ minh bạch:** Khách biết đang ở bước nào; không bị bỏ quên khi chờ. | **UC-03:** Theo dõi tiến độ và xử lý trường hợp chờ | **EVT-B05, EVT-T01 & EVT-T02:** Gán `wait_reason` kèm hạn; nhắc việc đến hạn đầu ngày; cảnh báo đỏ quá hạn. | `wait_reason` (`CHO_KHACH`/`CHO_DON_VI_BAO_HANH`), `follow_up_date`, DocType `Care Task`, Workspace Alert. |
| **G1.5, G2.4 & G5.6 – Liên tục giữa hai cửa hàng:** CH01 và CH02 là một thể thống nhất; không làm lại từ đầu. | **UC-06 & UC-02-A4:** Chuyển giao ticket giữa hai cửa hàng | **EVT-B03 (EX-04):** Giữ nguyên `ticket_id` và lịch sử; đổi `processing_store`; điều chuyển User Permission. | Cập nhật `processing_store`, điều chỉnh `User Permission` theo chi nhánh, không tạo ticket mới. |
| **G1.6, G2.6, G3.6 & G5.7 – Kết quả & Đóng hoàn tất:** Kết quả minh bạch; chỉ đóng khi khách được thông báo và nhận máy. | **UC-04:** Thông báo và hoàn tất yêu cầu; đóng ticket | **EVT-B11 & EVT-B12:** Kiểm tra Closing Gates BR-06 chặn đóng thiếu; lưu bằng chứng bàn giao; gọi `doc.submit()`. | Cờ `customer_notified`, `device_returned`, kiểm tra hoàn thành `Care Task`, `docstatus = 1` Read-only lock. |
| **G2.7 & G4.5 – Xử lý ngoại lệ chuẩn:** Khách chưa đồng ý kết quả hoặc vấn đề tái phát sau đóng. | **UC-04-A1 & UC-05:** Khách từ chối kết quả; Mở lại ticket đã đóng | **EVT-B11 (EX-07) & EVT-B13 (EX-08):** Quay lại `Processing` hoặc mở lại về `Verifying`, bảo toàn lịch sử. | Chuyển ngược workflow có lưu lý do tranh chấp; tăng `reopen_count`; bảo toàn toàn bộ `Audit Trail Log` cũ. |

---

## 6. ĐỐI CHIẾU TOÀN DIỆN 9 TÌNH HUỐNG NGOẠI LỆ (EX-01 ĐẾN EX-09)

Bảng đối chiếu cụ thể phản ứng của CRM và trách nhiệm con người cho toàn bộ 9 tình huống bắt buộc theo baseline:

| Mã | Tình huống ngoại lệ | Event tương ứng | Trạng thái kích hoạt | Phản ứng nghiệp vụ của CRM (Frappe v15) | Trách nhiệm con người |
| :---: | :--- | :---: | :---: | :--- | :--- |
| **EX-01** | Thiếu chứng từ / dữ liệu mua hàng tham chiếu | **EVT-B01** | `New` $\rightarrow$ `Verifying` | Cho phép lưu ticket ở chế độ `purchase_reference_status = 'Unverified'`; tạo task bổ sung thông tin; không tự cam kết quyền lợi. | Nhân viên tiếp nhận máy bình thường, ghi rõ phần thiếu, giải thích khách và không hứa hẹn miễn phí. |
| **EX-02** | Sản phẩm rơi vỡ, vào nước hoặc tranh chấp ngoại quan | **EVT-B01** | `New` $\rightarrow$ `Verifying` | Bắt buộc đính kèm tệp ảnh bằng chứng; mở trường ghi nhận ý kiến hai chiều; lưu cờ `has_damage_dispute = 1`. | Nhân viên chụp ảnh chi tiết, lập biên nhận hiện trạng khách ký xác nhận, chuyển cấp chuyên môn thẩm định. |
| **EX-03** | Lỗi chập chờn, không tái hiện tại quầy | **EVT-B01** | `Verifying` $\rightarrow$ `Processing` | Ghi nhận cờ `issue_type = 'Intermittent'`, `reproducible = 0`; lưu mô tả điều kiện xuất hiện lỗi; gia hạn thời gian kiểm tra tăng cường. | Owner khai thác kỹ hoàn cảnh lỗi, gửi đơn vị bảo hành kiểm tra chuyên sâu, không vội kết luận bình thường. |
| **EX-04** | Cần chuyển ticket sang cửa hàng khác xử lý | **EVT-B03** | `Verifying` / `Processing` (Self-transition) | Giữ nguyên `ticket_id`, lịch sử và tệp đính kèm; cập nhật `processing_store`, điều chuyển `User Permission` theo chi nhánh. | CSKH chuỗi ghi rõ lý do chuyển, bàn giao đích danh cho nhân sự chi nhánh nhận, thông báo cho khách nếu cần. |
| **EX-05** | Đang chờ khách hoặc đơn vị bảo hành | **EVT-B05** | `Processing` (Chế độ chờ) | Ghi nhận `wait_reason`, ngày liên hệ gần nhất, ngày hẹn theo dõi; tạo DocType `Care Task` nhắc việc tự động. | Owner theo dõi sát mốc hẹn, chủ động liên hệ đối tác hoặc khách hàng khi sắp đến hạn. |
| **EX-06** | Đến hạn theo dõi nhưng chưa có phản hồi | **EVT-T01** & **EVT-T02** | `Processing` (Đến/Quá hạn) | Kích hoạt cảnh báo vàng/đỏ trên Desk UI; gửi cảnh báo Escalation cho Quản lý; cấm tự ý đóng hoặc tự gia hạn ngày. | Owner khẩn trương liên hệ đôn đốc; Quản lý can thiệp hỗ trợ nếu đối tác chậm trễ. |
| **EX-07** | Khách chưa đồng ý kết quả giải quyết | **EVT-B11** | `Resolved` $\rightarrow$ `Processing` | Chuyển ngược ticket về `Processing`; bắt buộc nhập `dispute_reason`; bảo toàn nguyên vẹn kết quả cũ trong lịch sử. | Owner lắng nghe thắc mắc, ghi nhận trung thực, báo cáo Quản lý để hội chẩn lại phương án thỏa đáng hơn. |
| **EX-08** | Vấn đề phát sinh lại sau khi đã đóng ticket | **EVT-B13** | `Closed` $\rightarrow$ `Verifying` | Quản lý/CSKH chuỗi duyệt Reopen; giữ nguyên `ticket_id`; chuyển về `Verifying`; `docstatus = 0`; gán owner và hạn mới. | Quản lý thẩm định đúng lỗi cũ, chỉ định nhân sự chuyên môn thụ lý lại, giải thích rõ quy trình cho khách. |
| **EX-09** | Đủ điều kiện đổi nhưng kho chi nhánh hết hàng | **EVT-B08** | `Processing` | Ghi nhận `exchange_issue = 'Out of Stock'`; cấm CRM tự động hoàn tiền/cấp voucher; kích hoạt Care Task cho Quản lý cửa hàng. | Owner báo cáo Quản lý; Quản lý liên hệ điều chuyển hàng liên kho hoặc tư vấn khách phương án thỏa thuận. |

---

## 7. RÀ SOÁT CHECKLIST NGHIỆM THU (BASELINE CHECKLIST)

Bảng đối chiếu kiểm tra 100% tiêu chí tuân thủ theo mục 14 của tài liệu baseline dùng chung:

- [x] **1. Dùng đúng case và hai cửa hàng:** Đúng mô hình CH01 (136 Nguyễn Thái Học) và CH02 (21A Hàng Bài); không phát sinh cửa hàng thứ ba ngoài phạm vi.
- [x] **2. Không dùng số điện thoại làm khóa chính duy nhất:** `customer_id` ổn định độc lập; SĐT chỉ dùng để tra cứu và kích hoạt cảnh báo nghi trùng (BR-01 trong EVT-B01).
- [x] **3. Không áp đặt chính sách tùy tiện (1 đổi 1 30 ngày / SLA cố định):** Toàn bộ căn cứ thời gian và chính sách dựa trên thẩm định thực tế từng ca; hạn theo dõi `follow_up_date` là mốc liên hệ nội bộ, không cam kết bừa bãi.
- [x] **4. Khách hàng và đối tác không đăng nhập CRM trong MVP:** Mọi tương tác của khách hàng và đối tác ngoài đều được nhân viên/quản lý tiếp nhận và ghi nhận qua biểu mẫu CRM.
- [x] **5. Không biến tính năng Future-state thành hiện thực MVP:** Portal khách hàng, SMS/Zalo Brandname tự động, CSAT tự động và API real-time với đối tác được gắn nhãn Mức C (Future-state).
- [x] **6. Dùng đúng 5 trạng thái chính chuẩn mực:** `New` $\rightarrow$ `Verifying` $\rightarrow$ `Processing` $\rightarrow$ `Resolved` $\rightarrow$ `Closed`. Chờ khách và chờ đối tác là `wait_reason` (sub-status). `Closed` được bảo vệ bằng MariaDB Read-only lock (`docstatus = 1`).
- [x] **7. CRM không tự động thực hiện các tác vụ phi thực tế:** CRM không tự sửa chữa, không tự hoàn tiền, không tự xuất kho, không tự kết luận đủ điều kiện bảo hành chỉ dựa vào ngày mua.
- [x] **8. Mọi ngoại lệ (EX-01 đến EX-09) đều nêu rõ phản ứng CRM và trách nhiệm con người:** Được phân tích chi tiết trong Bảng Event–Response Table và Bảng đối chiếu Mục 6.
- [x] **9. Phân định rõ ràng mức độ thông tin (A / B / C):** Được công bố minh bạch tại Mục 1.2 và duy trì xuyên suốt tài liệu.

---
*— HẾT TÀI LIỆU PRACTICE 7 (TRẦN QUANG HUY) —*
