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
- **Mức C (Future-state – Không thuộc MVP):** Cổng tra cứu trực tuyến dành cho khách hàng (Customer Portal); kết nối API thời gian thực với hệ thống hãng/Điện Thoại Vui; gửi SMS/Zalo Brandname tự động; khảo sát CSAT tự động. *(Chỉ nêu ở hướng mở rộng, không đưa vào xử lý của hệ thống hiện tại).*

### 1.3. Cơ sở lý thuyết Practice #7 (Karl Wiegers & Candase Hokanson)
Theo Karl Wiegers, Use Cases (Practice #6) rất hiệu quả cho các tương tác người – máy trực tiếp, nhưng chưa đủ để bao quát toàn diện các hệ thống vận hành đa chiều, đặc biệt là các hành vi kích hoạt ngầm, cảnh báo tiến độ và kiểm tra điều kiện. **Practice #7 (Identify Events and Responses)** bổ trợ trực tiếp bằng cách xác định toàn bộ các thay đổi trong môi trường vận hành kích hoạt phản ứng của hệ thống, phân loại thành 3 nhóm:
1. **Business Event (Sự kiện nghiệp vụ):** Xuất phát từ bên ngoài ranh giới domain doanh nghiệp (khách hàng yêu cầu, đối tác gửi kết quả kiểm định...).
2. **Signal Event (Sự kiện tín hiệu / Dữ liệu):** Xuất phát từ thao tác cập nhật dữ liệu, tín hiệu kiểm tra điều kiện nội bộ hoặc hoàn tất một bước xử lý (lưu ticket hợp lệ, vi phạm điều kiện đóng cổng, phân công owner không hợp lệ...).
3. **Temporal Event (Sự kiện thời gian):** Kích hoạt khi đến một mốc thời gian định trước hoặc khi một khoảng thời gian trôi qua kể từ một trạng thái cụ thể (đến hạn theo dõi, quá hạn phản hồi SLA, quét định kỳ ticket bị treo...).

---

## 2. DANH MỤC SỰ KIỆN CHI TIẾT (EVENT LIST)

Toàn bộ các sự kiện được mã hóa có hệ thống: **EVT-Bxx** (Business Events), **EVT-Sxx** (Signal Events), **EVT-Txx** (Temporal Events), gắn kết trực tiếp với các Use Case (UC-01 đến UC-06 của Nhật) và các tình huống ngoại lệ (EX-01 đến EX-09 của baseline).

```
                              ┌──────────────────────────────────────────────┐
                              │     HỆ THỐNG SỰ KIỆN CRM CELLPHONES          │
                              └──────────────────────┬───────────────────────┘
                                                     │
            ┌────────────────────────────────────────┼────────────────────────────────────────┐
            │                                        │                                        │
┌───────────▼────────────┐              ┌────────────▼───────────┐              ┌─────────────▼──────────┐
│    BUSINESS EVENTS     │              │     SIGNAL EVENTS      │              │    TEMPORAL EVENTS     │
│   (12 Sự kiện nghiệp vụ)│              │  (12 Sự kiện tín hiệu) │              │  (4 Sự kiện thời gian) │
├────────────────────────┤              ├────────────────────────┤              ├────────────────────────┤
│ EVT-B01: Tiếp nhận quầy│              │ EVT-S01: Lưu hợp lệ    │              │ EVT-T01: Đến hạn nhắc  │
│ EVT-B02: Thiếu chứng từ│              │ EVT-S02: Lỗi validation│              │ EVT-T02: Quá hạn theo  │
│ EVT-B03: Tranh chấp máy│              │ EVT-S03: Lỗi gán owner │              │          dõi (SLA)     │
│ EVT-B04: Lỗi ẩn quầy   │              │ EVT-S04: Gán owner OK  │              │ EVT-T03: Quét ticket   │
│ EVT-B05: Điều phối kho │              │ EVT-S05: Chọn hướng XL │              │          chờ lâu       │
│ EVT-B06: Nhận KQ hãng  │              │ EVT-S06: Vào chế độ chờ│              │ EVT-T04: Quét ticket   │
│ EVT-B07: Khách đồng ý  │              │ EVT-S07: Hết thời gian │              │          mất chủ       │
│ EVT-B08: Khách từ chối │              │          chờ           │              └────────────────────────┘
│ EVT-B09: Khách nhận máy│              │ EVT-S08: Ghi nhận KQ   │
│ EVT-B10: Lỗi phát sinh │              │ EVT-S09: Chặn đóng     │
│          lại (Reopen)  │              │          (Vi phạm cổng)│
│ EVT-B11: Thiếu máy đổi │              │ EVT-S10: Đóng hợp lệ   │
│ EVT-B12: Khách hủy ca  │              │ EVT-S11: Cảnh báo trùng│
└────────────────────────┘              │ EVT-S12: Chặn mở trái  │
                                        │          phép          │
                                        └────────────────────────┘
```

### 2.1. Nhóm Sự kiện Nghiệp vụ (Business Events)
Các sự kiện do tác nhân bên ngoài domain (Khách hàng, Đơn vị bảo hành ngoài) khởi xướng hoặc kích hoạt:

| Mã Event | Tên sự kiện & Tác nhân kích hoạt | Mô tả bản chất nghiệp vụ | Liên kết UC & Baseline |
| :---: | :--- | :--- | :---: |
| **EVT-B01** | Khách hàng mang thiết bị đến CH02 yêu cầu bảo hành/đổi trả (Khách mua tại CH01) | Khách hàng xuất hiện tại quầy, cung cấp SĐT/thông tin liên hệ và mô tả lỗi thiết bị. | Kích hoạt UC-01 |
| **EVT-B02** | Khách hàng yêu cầu hỗ trợ nhưng thiếu chứng từ mua hàng hoặc mất hóa đơn | Khách hàng không cung cấp được hóa đơn giấy hoặc thông tin đơn hàng bị thiếu trong hệ thống tham chiếu. | EX-01; UC-01-A2; UC-02-A2 |
| **EVT-B03** | Khách hàng bàn giao thiết bị có dấu hiệu rơi vỡ, vào nước hoặc tranh chấp ngoại quan | Thiết bị thực nhận có dấu hiệu biến dạng vật lý, cong vênh, trầy xước nặng hoặc chỉ báo ngấm nước chuyển màu. | EX-02; UC-01-E2; UC-02-E1 |
| **EVT-B04** | Khách hàng mô tả lỗi chập chờn nhưng không thể tái hiện trực tiếp tại quầy | Lỗi xuất hiện gián đoạn trong quá trình sử dụng thực tế của khách hàng nhưng khi nhân viên kiểm tra quầy thì máy hoạt động bình thường. | EX-03; UC-02-E2 |
| **EVT-B05** | CSKH cấp chuỗi yêu cầu điều phối / chuyển ticket xử lý giữa hai cửa hàng | Thiết bị hoặc hồ sơ cần được chuyển giao trách nhiệm xử lý từ CH02 về CH01 hoặc ngược lại để thuận tiện nghiệp vụ. | EX-04; UC-06; UC-02-A4 |
| **EVT-B06** | Đơn vị bảo hành ngoài / Hãng phản hồi kết quả kiểm định chuyên môn | Trung tâm bảo hành đối tác gửi văn bản/biên bản kết quả thẩm định, phương án sửa chữa hoặc từ chối bảo hành qua kênh ngoài CRM. | Giai đoạn 4; UC-02 (bước 6); UC-03 |
| **EVT-B07** | Khách hàng xác nhận đồng ý với phương án giải quyết đã thông báo | Sau khi nhân viên liên hệ giải thích phương án, khách hàng xác nhận hài lòng và đồng ý tiếp nhận bàn giao. | Giai đoạn 5; UC-04 (bước 3) |
| **EVT-B08** | Khách hàng phản hồi KHÔNG đồng ý với kết quả giải quyết | Khách hàng khiếu nại, không chấp nhận kết quả từ chối bảo hành, chi phí báo giá hoặc tình trạng sửa chữa của hãng. | EX-07; UC-04-A1 |
| **EVT-B09** | Khách hàng đến quầy nhận lại thiết bị hoặc xác nhận hoàn tất bàn giao | Khách hàng kiểm tra máy thực tế, ký nhận biên bản bàn giao hoặc xác nhận phương án nhận hàng thay thế. | Giai đoạn 5; UC-04 (bước 4) |
| **EVT-B10** | Khách hàng phản ánh vấn đề cũ tái phát sau khi ticket đã Đóng | Sau khi nhận thiết bị về sử dụng, khách hàng gặp lại lỗi kỹ thuật y hệt lỗi của ticket đã đóng trước đó. | EX-08; UC-05 |
| **EVT-B11** | Đủ điều kiện đổi sản phẩm mới nhưng kho cửa hàng không có sẵn máy tương đương | Kết quả xác minh xác định đủ điều kiện đổi mới 1-1, nhưng model máy tương đương đang hết hàng tại chi nhánh. | EX-09; UC-02-E3 |
| **EVT-B12** | Khách hàng chủ động yêu cầu rút lại yêu cầu hỗ trợ | Khách hàng thay đổi ý định, không muốn tiếp tục gửi máy hoặc tự xử lý bên ngoài. | UC-02-A3 |

### 2.2. Nhóm Sự kiện Tín hiệu (Signal Events)
Các sự kiện phát sinh từ bên trong hệ thống phần mềm khi người dùng thao tác dữ liệu, kích hoạt workflow hoặc vi phạm các quy tắc nghiệp vụ (Business Rules):

| Mã Event | Tên sự kiện & Điều kiện tín hiệu | Mô tả bản chất kiểm tra của CRM | Liên kết UC & Baseline |
| :---: | :--- | :--- | :---: |
| **EVT-S01** | Tín hiệu lưu tạo ticket tiếp nhận thành công | Dữ liệu đầu vào thỏa mãn minimum dataset: có Customer hợp lệ, mô tả lỗi, chi nhánh và thông tin thiết bị (nếu nhận máy). | UC-01 (bước 6); BR-02 |
| **EVT-S02** | Tín hiệu lỗi kiểm tra dữ liệu bắt buộc khi tiếp nhận (Validation Failure) | Thiếu thông tin liên hệ tối thiểu của khách hàng, hoặc nhận máy nhưng không ghi ngoại quan/phụ kiện. | UC-01-E3; G2.5 |
| **EVT-S03** | Tín hiệu chỉ định Owner mới không hợp lệ hoặc thiếu quyền chi nhánh | Người được gán không có tài khoản hoạt động, hoặc không thuộc danh sách nhân viên được phân quyền tại chi nhánh xử lý. | UC-02-E4; BR-02; G2.1 |
| **EVT-S04** | Tín hiệu xác nhận gán Owner chính thức và thiết lập hạn theo dõi | Người có thẩm quyền xác nhận tiếp nhận trách nhiệm xử lý, chỉ định đích danh User và ngày hẹn theo dõi (`follow_up_date`). | UC-02 (bước 1-2); BR-02 |
| **EVT-S05** | Tín hiệu xác định xong hướng xử lý nghiệp vụ | Owner hoàn tất đối chiếu chính sách, ghi nhận căn cứ và chọn chuyển ticket sang giai đoạn thực thi/phối hợp. | UC-02 (bước 4) |
| **EVT-S06** | Tín hiệu kích hoạt chế độ chờ xử lý bên ngoài | Owner cập nhật ticket vào chế độ chờ, bắt buộc nhập lý do chờ (`wait_reason`) và ngày hẹn liên hệ lại. | EX-05; UC-03 (bước 3); BR-03 |
| **EVT-S07** | Tín hiệu kết thúc thời gian chờ xử lý | Nhận được thông tin phản hồi từ khách hoặc đối tác, hệ thống giải phóng cờ chờ (`wait_reason = NULL`). | UC-03 (bước 6); BR-03 |
| **EVT-S08** | Tín hiệu ghi nhận kết quả xử lý thực tế và chuyển sang Đã giải quyết | Owner cập nhật kết luận xử lý thực tế (`resolution_result`), căn cứ đính kèm và người phê duyệt. | UC-02 (bước 7); BR-06 |
| **EVT-S09** | Tín hiệu vi phạm điều kiện cổng đóng ticket (Closing Gates Failure) | Người dùng yêu cầu đóng ticket khi chưa ghi nhận thông báo khách, hoặc chưa có biên bản bàn giao máy, hoặc còn task mở. | UC-04-E1; BR-06; D05 |
| **EVT-S10** | Tín hiệu thỏa mãn toàn bộ điều kiện cổng đóng ticket (Closing Gates Passed) | Kiểm tra thành công 100% điều kiện: đã có kết quả, đã log thông báo, đã có biên lai bàn giao (nếu có máy), không còn task treo. | UC-04 (bước 6); BR-06 |
| **EVT-S11** | Tín hiệu cảnh báo hồ sơ khách hàng nghi trùng số điện thoại | Nhân viên tra cứu hoặc tạo mới khách hàng với SĐT đã tồn tại trên một hồ sơ khác trong hệ thống. | BR-01; G1.2; G3.1 |
| **EVT-S12** | Tín hiệu chặn yêu cầu mở lại ticket do người dùng không đủ quyền | Nhân viên tiếp nhận tự ý bấm mở lại ticket đã đóng mà không qua phê duyệt của Quản lý hoặc CSKH chuỗi. | UC-05-E1; BR-07 |

### 2.3. Nhóm Sự kiện Thời gian (Temporal Events)
Các sự kiện kích hoạt tự động theo thời gian chạy ngầm (Scheduled Tasks / Hooks) của hệ thống:

| Mã Event | Tên sự kiện & Điều kiện thời gian | Mô tả cơ chế kích hoạt thời gian | Liên kết UC & Baseline |
| :---: | :--- | :--- | :---: |
| **EVT-T01** | Đến hạn theo dõi ticket (`follow_up_date` đến hạn trong ngày) | Mốc thời gian hệ thống chạm đến ngày hẹn liên hệ/kiểm tra tiến độ đã ghi trên ticket (`current_date == follow_up_date`). | EX-06; UC-03 (bước 4); BR-05 |
| **EVT-T02** | Quá hạn theo dõi mà ticket chưa được cập nhật (`follow_up_date < current_date`) | Ticket chưa đóng nhưng ngày hẹn theo dõi đã trôi qua mà không có bất kỳ log cập nhật hay gia hạn hợp lệ nào. | EX-06; G3.4; G5.5; D02 |
| **EVT-T03** | Định kỳ quét rà soát các ticket có trạng thái chờ kéo dài (Long-waiting Audit Sweep) | Tác vụ chạy định kỳ mỗi 24 giờ, kiểm tra các ticket đang ở chế độ chờ đối tác hoặc chờ khách quá thời hạn tiêu chuẩn. | G2.3; G3.3 |
| **EVT-T04** | Định kỳ quét phát hiện ticket không có Owner phụ trách (Unassigned Sweep) | Tác vụ chạy định kỳ rà soát các ticket ở trạng thái Mới tiếp nhận hoặc Đang xác minh nhưng bị trống người chịu trách nhiệm. | G1.3; G2.1; G3.2; UC-03-E2 |

---

## 3. BẢNG PHẢN ỨNG VỚI SỰ KIỆN (EVENT–RESPONSE TABLE)

Bảng phân tích toàn diện 6 cột chuẩn mực theo quy chuẩn của Karl Wiegers và yêu cầu baseline mục 12.3:
- **Event ID:** Mã định danh sự kiện duy nhất.
- **Tên sự kiện & Tác nhân:** Tên sự kiện, tác nhân gây ra và ngữ cảnh xuất hiện.
- **Phân loại (Type):** Business Event, Signal Event hoặc Temporal Event.
- **Điều kiện / Trạng thái kích hoạt (Current State / Condition):** Trạng thái hiện tại của ticket và các tiền điều kiện kèm theo.
- **Phản ứng kỳ vọng (Expected Response):** Tách bạch rõ hành vi đáp ứng của phần mềm CRM và trách nhiệm thực thi của con người.
- **Dữ liệu & Nhật ký bị tác động (Data & Audit Log Affected):** Các thực thể, trường thông tin và bản ghi audit trail được cập nhật.

*(Ghi chú thuật ngữ trạng thái chuẩn: `NEW` = Mới tiếp nhận, `VERIFYING` = Đang xác minh, `PROCESSING` = Đang xử lý, `RESOLVED` = Đã giải quyết, `CLOSED` = Đóng).*

| Event ID | Tên sự kiện & Tác nhân | Phân loại | Điều kiện / Trạng thái kích hoạt | Phản ứng kỳ vọng của hệ thống & con người | Dữ liệu & Nhật ký bị tác động |
| :---: | :--- | :---: | :--- | :--- | :--- |
| **EVT-B01** | Khách hàng mang thiết bị đến CH02 yêu cầu bảo hành/đổi trả (Khách mua tại CH01)<br>*(Tác nhân: Khách hàng)* | **Business** | Khách hàng xuất hiện tại quầy CH02; chưa có ticket cho yêu cầu này. | **Hệ thống (CRM):** Cung cấp giao diện tra cứu khách hàng liên chi nhánh qua SĐT/mã khách; cho phép liên kết dữ liệu mua hàng tham chiếu từ CH01; mở form tiếp nhận ticket.<br>**Con người (Nhân viên CH02):** Tra cứu hồ sơ; đối chiếu thông tin; kiểm tra ngoại quan máy; ghi biên bản nhận thiết bị (IMEI/Serial, phụ kiện, vết trầy); chọn Lưu. | Tạo mới `Support_Ticket` (trạng thái `NEW`); tạo `Customer` (nếu khách mới); liên kết `Purchase_Reference`. Audit log ghi nhận hành động tạo và người tạo. |
| **EVT-S01** | Tín hiệu lưu tạo ticket hợp lệ thành công<br>*(Tác nhân: Hệ thống CRM)* | **Signal** | Form tiếp nhận có đủ dữ liệu tối thiểu; nhân viên bấm "Lưu". | **Hệ thống (CRM):** Cấp mã `ticket_id` tự động; đặt trạng thái ban đầu là `NEW`; tự động gán `temporary_owner = Creator`; đặt `receiving_store = CH02` và `processing_store = CH02`; gửi tín hiệu mở màn hình chi tiết ticket.<br>**Con người (Nhân viên CH02):** In/cung cấp mã `ticket_id` cho khách hàng; giải thích các bước xác minh tiếp theo. | Lưu bản ghi `Support_Ticket` với `status = 'NEW'`, `owner = session.user`. Sinh bản ghi `Audit_Log` loại "CREATE_TICKET". |
| **EVT-S02** | Vi phạm kiểm tra dữ liệu bắt buộc khi tiếp nhận<br>*(Tác nhân: Hệ thống CRM)* | **Signal** | Thiếu thông tin liên hệ khách hoặc thiếu thông tin tình trạng máy khi có nhận thiết bị. | **Hệ thống (CRM):** Chặn lưu bản ghi; hiển thị thông báo lỗi Validation màu đỏ chỉ rõ các trường thiếu; không sinh `ticket_id` rác.<br>**Con người (Nhân viên):** Thu thập bổ sung thông tin từ khách hàng; hoàn thiện dữ liệu trước khi gửi lại. | Không ghi vào cơ sở dữ liệu. Không phát sinh log nghiệp vụ. |
| **EVT-S11** | Cảnh báo phát hiện số điện thoại nghi trùng<br>*(Tác nhân: Hệ thống CRM)* | **Signal** | Nhân viên nhập SĐT khách hàng đã tồn tại trên một hồ sơ khách hàng khác. | **Hệ thống (CRM):** Hiển thị hộp thoại cảnh báo nghi trùng hồ sơ (BR-01); hiển thị danh sách khách hàng trùng SĐT để nhân viên đối chiếu; TUYỆT ĐỐI KHÔNG tự động gộp dữ liệu.<br>**Con người (Nhân viên):** Kiểm tra đối chiếu họ tên, địa chỉ với khách hàng để chọn đúng hồ sơ hoặc xác nhận tạo hồ sơ riêng biệt. | Giữ nguyên dữ liệu nếu chọn đúng khách; tạo hồ sơ mới với `customer_id` mới nếu xác nhận là hai cá nhân khác nhau. |
| **EVT-B02** | Khách yêu cầu hỗ trợ nhưng thiếu chứng từ mua hàng (EX-01)<br>*(Tác nhân: Khách hàng)* | **Business** | Trạng thái ticket `NEW` hoặc chuẩn bị chuyển `VERIFYING`. | **Hệ thống (CRM):** Cho phép lưu ticket ở chế độ dữ liệu tham chiếu "Chưa xác minh" (`verification_status = 'Pending'`); mở ô ghi chú chứng từ còn thiếu.<br>**Con người (Nhân viên / Owner):** Vẫn tiếp nhận thiết bị; ghi nhận rõ thông tin chứng từ đang thiếu; hướng dẫn khách cách bổ sung; TUYỆT ĐỐI KHÔNG cam kết hỗ trợ miễn phí tại quầy. | Cập nhật `Support_Ticket`: trường `purchase_reference_status = 'Unverified'`, ghi nhận chi tiết thiếu vào `notes`. Audit log ghi nhận ngoại lệ EX-01. |
| **EVT-B03** | Thiết bị có dấu hiệu rơi vỡ, vào nước hoặc tranh chấp ngoại quan (EX-02)<br>*(Tác nhân: Khách hàng / Nhân viên)* | **Business** | Quá trình tiếp nhận tại quầy hoặc lúc bắt đầu xác minh. | **Hệ thống (CRM):** Bắt buộc đính kèm tối thiểu 01 tệp hình ảnh/video bằng chứng ngoại quan; mở trường ghi nhận ý kiến hai chiều (Ý kiến khách hàng vs Quan sát nhân viên).<br>**Con người (Nhân viên):** Chụp ảnh các điểm trầy xước/cấn móp/chỉ báo ngấm nước; ghi nhận trung thực ý kiến khách; chuyển Quản lý/Bộ phận chuyên môn xác định; không tự kết luận từ chối bảo hành. | `Support_Ticket`: trường `has_damage_dispute = TRUE`, lưu danh sách URL tệp hình ảnh vào `evidence_attachments`. Ghi log tranh chấp ngoại quan. |
| **EVT-B04** | Khách mô tả lỗi chập chờn không tái hiện tại quầy (EX-03)<br>*(Tác nhân: Khách hàng)* | **Business** | Trạng thái ticket `NEW` hoặc `VERIFYING`. | **Hệ thống (CRM):** Cho phép chọn phân loại lỗi "Lỗi gián đoạn / Không tái hiện"; mở trường mô tả ngữ cảnh phát sinh lỗi (nhiệt độ, ứng dụng đang chạy, tần suất...).<br>**Con người (Owner):** Khai thác chi tiết điều kiện xuất hiện lỗi từ khách hàng; đề xuất chuyển thẩm định sâu; thiết lập hạn theo dõi kiểm tra tăng cường; không đóng ticket vội vã. | `Support_Ticket`: trường `issue_type = 'Intermittent'`, `reproducible = FALSE`, lưu chi tiết vào `issue_description`. Thiết lập `follow_up_date` mới. |
| **EVT-S03** | Chỉ định Owner mới không hợp lệ hoặc thiếu quyền<br>*(Tác nhân: Hệ thống CRM)* | **Signal** | Người dùng chọn User bị khóa hoặc không có quyền tại `processing_store`. | **Hệ thống (CRM):** Từ chối thao tác phân công; hiển thị cảnh báo lỗi phân quyền; giữ nguyên Owner và quyền hạn trước đó.<br>**Con người (Quản lý):** Kiểm tra danh sách nhân sự trực thuộc chi nhánh xử lý và chọn lại nhân sự đủ thẩm quyền. | Giữ nguyên trường `assigned_to_user` và `processing_store`. Ghi log cảnh báo phân quyền thất bại. |
| **EVT-S04** | Xác nhận phân công Owner hợp lệ và thiết lập hạn theo dõi<br>*(Tác nhân: Quản lý / CSKH chuỗi)* | **Signal** | Trạng thái `NEW`; Người được gán có quyền hoạt động hợp lệ; có chọn `follow_up_date`. | **Hệ thống (CRM):** Cập nhật `assigned_to_user = New_Owner`; chuyển trạng thái ticket từ `NEW` sang `VERIFYING`; tạo một Care Task loại "Xác minh chính sách" cho Owner mới; ghi nhận thời điểm bàn giao.<br>**Con người (Owner mới):** Tiếp nhận thông báo công việc; mở hồ sơ bắt đầu kiểm tra đối chiếu chính sách. | Cập nhật `Support_Ticket`: `status = 'VERIFYING'`, `assigned_to_user`, `follow_up_date`. Tạo bản ghi `Care_Task`. Ghi log "ASSIGN_OWNER_TRANSITION_VERIFYING". |
| **EVT-S05** | Xác định xong hướng xử lý nghiệp vụ<br>*(Tác nhân: Owner)* | **Signal** | Trạng thái `VERIFYING`; Owner đã điền căn cứ chính sách và hướng xử lý (`resolution_path`). | **Hệ thống (CRM):** Kiểm tra điều kiện hợp lệ; chuyển trạng thái từ `VERIFYING` sang `PROCESSING`; kích hoạt các trường theo dõi phối hợp đối tác; cập nhật hạn theo dõi tiếp theo.<br>**Con người (Owner):** Thực hiện xử lý nội bộ hoặc liên hệ bàn giao thiết bị cho trung tâm bảo hành ngoài theo quy trình vận hành. | Cập nhật `Support_Ticket`: `status = 'PROCESSING'`, `resolution_path`, `verified_by = session.user`, `verified_at = NOW()`. Ghi log "TRANSITION_PROCESSING". |
| **EVT-B05** | Điều phối chuyển ticket giữa hai cửa hàng (EX-04 / UC-06)<br>*(Tác nhân: CSKH cấp chuỗi)* | **Business** | Trạng thái `VERIFYING` hoặc `PROCESSING`; Cần chuyển trách nhiệm từ CH02 sang CH01. | **Hệ thống (CRM):** Giữ NGUYÊN `ticket_id`, toàn bộ lịch sử tương tác và đính kèm; cập nhật `processing_store = CH01`; tự động gán Owner mới thuộc CH01; cập nhật User Permission theo chi nhánh; thu hồi quyền chỉnh sửa của CH02 (chuyển sang Read-only).<br>**Con người (CSKH chuỗi & Owner mới):** CSKH chuỗi ghi rõ lý do chuyển; thông báo bàn giao; Owner tại CH01 kiểm tra tiếp nhận hồ sơ. | Cập nhật `Support_Ticket`: `processing_store = 'CH01'`, `assigned_to_user = New_Staff_CH01`, ghi nhận `transfer_reason`. Ghi log "TRANSFER_STORE_EX04". |
| **EVT-S06** | Kích hoạt chế độ chờ xử lý bên ngoài (EX-05)<br>*(Tác nhân: Owner)* | **Signal** | Trạng thái chính `PROCESSING`; phát sinh nhu cầu chờ khách hoặc chờ đối tác hãng. | **Hệ thống (CRM):** Bắt buộc nhập `wait_reason` (`CHO_KHACH` hoặc `CHO_DON_VI_BAO_HANH`), ghi nhận đơn vị phối hợp, mã tham chiếu ngoài (nếu có) và ngày hẹn `follow_up_date`; giữ NGUYÊN trạng thái chính là `PROCESSING`; tạo task nhắc việc.<br>**Con người (Owner):** Nhập đầy đủ thông tin gửi đối tác; thiết lập ngày hẹn theo dõi sát sao. | Cập nhật `Support_Ticket`: `wait_reason = 'CHO_DON_VI_BAO_HANH'`, `partner_name`, `partner_ref_code`, `follow_up_date`. Ghi log "SET_WAIT_REASON". |
| **EVT-B06** | Đơn vị bảo hành ngoài phản hồi kết quả kiểm định<br>*(Tác nhân: Đối tác bảo hành)* | **Business** | Trạng thái `PROCESSING`, `wait_reason = 'CHO_DON_VI_BAO_HANH'`. | **Hệ thống (CRM):** Cho phép nhập nội dung phản hồi, đính kèm biên bản thẩm định của hãng, cập nhật chi phí (nếu có); cho phép kết thúc chế độ chờ.<br>**Con người (Owner):** Tiếp nhận kết quả qua email/điện thoại; nhập đầy đủ thông tin vào CRM; xóa `wait_reason`; đánh giá để chuẩn bị chuyển kết luận. | Tạo bản ghi `Interaction` (loại Partner); đính kèm biên bản; cập nhật `Support_Ticket`: giải phóng `wait_reason = NULL`. Ghi log "PARTNER_RESPONSE_RECEIVED". |
| **EVT-S07** | Kết thúc thời gian chờ xử lý<br>*(Tác nhân: Owner / Hệ thống)* | **Signal** | Đang có `wait_reason`; nhận đủ dữ liệu cần thiết để tiếp tục xử lý. | **Hệ thống (CRM):** Xóa trắng trường `wait_reason`; đóng Care Task chờ; lưu vết khoảng thời gian chờ vào lịch sử; giữ trạng thái chính là `PROCESSING`.<br>**Con người (Owner):** Tiếp tục thực hiện các bước xử lý nghiệp vụ tiếp theo trên ticket. | Cập nhật `Support_Ticket`: `wait_reason = NULL`. Cập nhật `Care_Task`: `status = 'Completed'`. Ghi log "WAIT_ENDED". |
| **EVT-B11** | Đủ điều kiện đổi máy nhưng kho chi nhánh hết hàng (EX-09)<br>*(Tác nhân: Nhân viên / Kho)* | **Business** | Trạng thái `PROCESSING`; đã xác nhận đủ điều kiện đổi mới 1-1. | **Hệ thống (CRM):** Cho phép ghi nhận trở ngại "Thiếu hàng đổi tại chi nhánh"; giữ nguyên trạng thái `PROCESSING`; cấm nhân viên tự ý hoàn tiền hoặc tặng voucher tùy tiện; mở luồng chuyển Quản lý duyệt phương án.<br>**Con người (Owner & Quản lý):** Owner báo cáo Quản lý; Quản lý phối hợp chuỗi điều chuyển máy hoặc đề xuất phương án giải quyết theo thẩm quyền. | `Support_Ticket`: ghi nhận `exchange_issue = 'Out of Stock'`, tạo task escalated cho Quản lý cửa hàng. Ghi log "OUT_OF_STOCK_EXCEPTION_EX09". |
| **EVT-B12** | Khách hàng chủ động rút yêu cầu hỗ trợ<br>*(Tác nhân: Khách hàng)* | **Business** | Trạng thái `VERIFYING` hoặc `PROCESSING`. | **Hệ thống (CRM):** Mở form ghi nhận lý do rút yêu cầu; chuyển hướng ticket sang luồng kết thúc đặc thù; TUYỆT ĐỐI KHÔNG xóa ticket; yêu cầu bàn giao lại máy nếu đã nhận.<br>**Con người (Owner):** Ghi rõ lý do khách rút yêu cầu; thực hiện bàn giao lại thiết bị nguyên trạng cho khách; hoàn tất biên bản bàn giao. | `Support_Ticket`: cập nhật `resolution_result = 'Customer Withdrew'`, chuyển `status = 'RESOLVED'`. Tạo task bàn giao thiết bị hoàn trả. Ghi log "CUSTOMER_WITHDREW". |
| **EVT-S08** | Ghi nhận kết quả xử lý thực tế và chuyển Đã giải quyết<br>*(Tác nhân: Owner)* | **Signal** | Trạng thái `PROCESSING`; đã có kết quả nghiệp vụ thực tế; không còn `wait_reason`. | **Hệ thống (CRM):** Bắt buộc nhập `resolution_result` và căn cứ; chuyển trạng thái từ `PROCESSING` sang `RESOLVED`; tự động sinh Care Task bắt buộc: "Thông báo kết quả cho khách hàng" và "Bàn giao thiết bị".<br>**Con người (Owner):** Rà soát lại hồ sơ; chuẩn bị phương án giải thích rõ ràng cho khách hàng; mở tác vụ liên hệ. | Cập nhật `Support_Ticket`: `status = 'RESOLVED'`, `resolved_at = NOW()`, `resolution_result`. Sinh `Care_Task` liên hệ khách. Ghi log "TRANSITION_RESOLVED". |
| **EVT-B07** | Khách hàng đồng ý với kết quả thông báo<br>*(Tác nhân: Khách hàng)* | **Business** | Trạng thái `RESOLVED`; nhân viên đã liên hệ giải thích kết quả. | **Hệ thống (CRM):** Cung cấp form Quick Log tương tác; đánh dấu cờ `customer_notified = TRUE`; lưu kênh liên lạc, thời điểm và nội dung phản hồi đồng ý của khách.<br>**Con người (Nhân viên):** Gọi điện/gặp trực tiếp khách hàng; giải thích kết quả; ghi nhận sự đồng thuận; hẹn ngày giờ bàn giao máy. | Tạo bản ghi `Interaction` (loại Customer Call/Meeting); cập nhật `Support_Ticket`: `customer_notified = TRUE`, `customer_agreement_status = 'Agreed'`. Ghi log "CUSTOMER_AGREED". |
| **EVT-B08** | Khách hàng KHÔNG đồng ý với kết quả giải quyết (EX-07)<br>*(Tác nhân: Khách hàng)* | **Business** | Trạng thái `RESOLVED`; khách phản hồi khiếu nại hoặc từ chối phương án. | **Hệ thống (CRM):** Chuyển ngược trạng thái từ `RESOLVED` quay lại `PROCESSING`; bắt buộc nhập lý do khách không đồng ý; gán hạn theo dõi mới; giữ nguyên kết quả cũ trong lịch sử; KHÔNG chuyển về `NEW`.<br>**Con người (Owner & Quản lý):** Lắng nghe khiếu nại; ghi nhận trung thực vào CRM; báo cáo Quản lý để hội chẩn lại với trung tâm bảo hành hoặc cấp thẩm quyền cao hơn. | Cập nhật `Support_Ticket`: `status = 'PROCESSING'`, `dispute_reason`, `follow_up_date = New_Date`. Tạo `Care_Task` xử lý khiếu nại. Ghi log "REJECT_RESOLUTION_EX07". |
| **EVT-B09** | Khách hàng đến nhận lại máy hoặc xác nhận bàn giao<br>*(Tác nhân: Khách hàng / Nhân viên)* | **Business** | Trạng thái `RESOLVED`; có thiết bị nhận tại quầy. | **Hệ thống (CRM):** Mở form ghi nhận bàn giao thiết bị; bắt buộc đánh dấu cờ `device_returned = TRUE`; cho phép đính kèm chữ ký/hình ảnh biên lai bàn giao.<br>**Con người (Nhân viên bàn giao):** Cho khách hàng kiểm tra lại tình trạng thiết bị; cùng ký biên bản bàn giao; tải ảnh biên bản lên CRM. | Cập nhật `Support_Ticket`: `device_returned = TRUE`, `handover_by = session.user`, `handover_at = NOW()`. Lưu ảnh vào `handover_evidence`. Ghi log "DEVICE_HANDED_OVER". |
| **EVT-S09** | Vi phạm điều kiện cổng đóng ticket (Closing Gates Failure)<br>*(Tác nhân: Hệ thống CRM)* | **Signal** | Người dùng bấm "Đóng Ticket" khi chưa thỏa mãn BR-06: `customer_notified == FALSE`, hoặc chưa bàn giao máy khi có nhận thiết bị, hoặc còn task mở. | **Hệ thống (CRM):** CHẶN ĐỨNG thao tác đóng; hiển thị pop-up cảnh báo màu đỏ liệt kê chính xác các điều kiện chưa hoàn tất; giữ nguyên trạng thái `RESOLVED`; không ghi log đóng thành công.<br>**Con người (Nhân viên):** Kiểm tra danh sách thiếu sót; thực hiện gọi thông báo khách hoặc hoàn tất bàn giao máy trước khi thử lại. | Chặn giao dịch cơ sở dữ liệu. Tạo bản ghi `Audit_Log` loại "CLOSING_GATE_REJECTED". |
| **EVT-S10** | Thỏa mãn toàn bộ điều kiện cổng đóng ticket (Closing Gates Passed)<br>*(Tác nhân: Hệ thống CRM)* | **Signal** | Người dùng có thẩm quyền yêu cầu đóng; thỏa mãn 100% điều kiện cổng kiểm soát BR-06. | **Hệ thống (CRM):** Chuyển trạng thái ticket từ `RESOLVED` sang `CLOSED`; đóng toàn bộ các task phụ liên quan; lưu `closed_by = session.user`, `closed_at = NOW()`; khóa chỉnh sửa dữ liệu chính của ticket (Read-only lock).<br>**Con người (Owner / Quản lý):** Xác nhận hoàn tất trọn vẹn ca phục vụ. | Cập nhật `Support_Ticket`: `status = 'CLOSED'`, `closed_at = NOW()`, `closed_by`. Sinh bản ghi `Audit_Log` "CLOSE_TICKET_SUCCESS". |
| **EVT-B10** | Khách phản ánh vấn đề cũ tái phát sau khi đã Đóng (EX-08 / UC-05)<br>*(Tác nhân: Khách hàng)* | **Business** | Trạng thái ticket `CLOSED`; khách mang máy quay lại phản ánh cùng một lỗi kỹ thuật. | **Hệ thống (CRM):** Cho phép Quản lý hoặc CSKH chuỗi mở lại ticket; giữ NGUYÊN `ticket_id` và toàn bộ lịch sử trước; chuyển trạng thái từ `CLOSED` về `VERIFYING`; xóa cờ `wait_reason`; bắt buộc gán Owner mới và hạn theo dõi mới.<br>**Con người (Quản lý / CSKH chuỗi):** Đánh giá lỗi có trùng khớp lỗi cũ; phê duyệt mở lại; phân công nhân viên xác minh lại từ đầu. | Cập nhật `Support_Ticket`: `status = 'VERIFYING'`, `reopen_count += 1`, `reopen_reason`, `reopened_by`, `reopened_at = NOW()`. Ghi log "REOPEN_TICKET_EX08". |
| **EVT-S12** | Chặn thao tác mở lại ticket ngoài thẩm quyền<br>*(Tác nhân: Hệ thống CRM)* | **Signal** | Nhân viên quầy thông thường cố gắng bấm nút mở lại ticket đã `CLOSED`. | **Hệ thống (CRM):** Chặn thao tác; hiển thị thông báo lỗi quyền hạn: "Chỉ Quản lý cửa hàng hoặc CSKH chuỗi mới có thẩm quyền mở lại ticket đã đóng".<br>**Con người (Nhân viên quầy):** Hướng dẫn khách hàng và trình hồ sơ lên Quản lý cửa hàng xem xét phê duyệt. | Chặn giao dịch. Ghi log "UNAUTHORIZED_REOPEN_ATTEMPT". |
| **EVT-T01** | Đến hạn theo dõi ticket (`follow_up_date` đến hạn)<br>*(Tác nhân: Bộ lập lịch hệ thống)* | **Temporal** | Đến ngày `follow_up_date` mà ticket chưa `CLOSED`; `current_date == follow_up_date`. | **Hệ thống (CRM):** Tự động phát sinh thông báo đẩy (Desk Notification) và đánh dấu màu vàng cảnh báo "Công việc đến hạn hôm nay" trên Dashboard của Owner.<br>**Con người (Owner):** Mở danh sách công việc; thực hiện hành động liên hệ đối tác hoặc kiểm tra tiến độ đã cam kết. | Cập nhật trạng thái `Care_Task` liên quan thành `Due Today`. Tạo bản ghi thông báo hệ thống. |
| **EVT-T02** | Quá hạn theo dõi mà ticket chưa được cập nhật (EX-06)<br>*(Tác nhân: Bộ lập lịch hệ thống)* | **Temporal** | Ticket chưa `CLOSED`, `follow_up_date < current_date`, không có log cập nhật mới. | **Hệ thống (CRM):** Tự động gắn nhãn cảnh báo đỏ "QUÁ HẠN THEO DÕI" trên giao diện ticket và Workspace; gửi cảnh báo Escalation lên Dashboard của Quản lý cửa hàng; TUYỆT ĐỐI KHÔNG tự động đóng hay gia hạn ngày.<br>**Con người (Owner & Quản lý):** Owner khẩn trương liên hệ xử lý; Quản lý can thiệp đôn đốc hoặc điều chuyển người phụ trách nếu cần. | Cập nhật cờ `is_overdue = TRUE` trên `Support_Ticket`. Ghi log "TEMPORAL_OVERDUE_ALERT_EX06". |
| **EVT-T03** | Định kỳ quét rà soát ticket chờ kéo dài<br>*(Tác nhân: Bộ lập lịch hệ thống)* | **Temporal** | Tác vụ chạy định kỳ 00:00 hàng ngày; quét các ticket có `wait_reason != NULL` kéo dài $> 7$ ngày. | **Hệ thống (CRM):** Tổng hợp danh sách ticket chờ lâu; xuất danh sách cảnh báo trên báo cáo quản trị SLA cho CSKH cấp chuỗi và Quản lý.<br>**Con người (Quản lý chuỗi):** Rà soát các ca nghẽn đối tác bảo hành để liên hệ cấp quản lý của đơn vị bảo hành can thiệp xử lý. | Sinh bản ghi thống kê báo cáo SLA hàng ngày. |
| **EVT-T04** | Định kỳ quét ticket mất chủ (Unassigned Ticket Sweep)<br>*(Tác nhân: Bộ lập lịch hệ thống)* | **Temporal** | Chạy định kỳ mỗi 6 giờ; quét ticket có `assigned_to_user IS NULL` hoặc User đã bị Disable. | **Hệ thống (CRM):** Đưa ticket vào danh mục khẩn cấp "Ticket cần phân công lại"; phát chuông cảnh báo trên bàn làm việc của Quản lý cửa hàng tương ứng.<br>**Con người (Quản lý cửa hàng):** Truy cập ngay danh mục và chỉ định Owner mới có thẩm quyền để nối lại trách nhiệm phục vụ. | Cập nhật trạng thái hiển thị trên Dashboard. Ghi log "UNASSIGNED_SWEEP_ALERT". |

---

## 4. BIỂU ĐỒ CHUYỂN ĐỔI TRẠNG THÁI (STATE TRANSITION DIAGRAM)

Theo Karl Wiegers (Practice #7), State Transition Diagram là mô hình trực quan bổ trợ đắc lực nhất cho Event–Response Table, giúp phát hiện các trạng thái "chết", các nhánh chuyển đổi thiếu hoặc các vòng lặp sai lệch.

### 4.1. Sơ đồ trạng thái chuẩn (Mermaid State Machine)
Biểu đồ dưới đây biểu diễn chính xác vòng đời ticket qua đúng **5 trạng thái chính** theo baseline:
`Mới tiếp nhận` $\rightarrow$ `Đang xác minh` $\rightarrow$ `Đang xử lý` $\rightarrow$ `Đã giải quyết` $\rightarrow$ `Đóng`.
Trong đó, **Chờ khách** và **Chờ đối tác bảo hành** được mô hình hóa chuẩn mực dưới dạng **Chế độ con (Sub-state / Wait Reason)** nằm trong trạng thái `Đang xử lý`, không phá vỡ 5 trạng thái chính.

```mermaid
stateDiagram-v2
    [*] --> MOI_TIEP_NHAN: EVT-B01 / EVT-S01\n(Tiếp nhận tại quầy CH02,\nLưu đủ dữ liệu ban đầu)

    state MOI_TIEP_NHAN {
        [*] --> GhiNhanTam
        GhiNhanTam: Người tạo tạm chịu trách nhiệm
    }

    MOI_TIEP_NHAN --> DANG_XAC_MINH: EVT-S04\n(Xác nhận Owner hợp lệ & Hạn theo dõi)

    state DANG_XAC_MINH {
        [*] --> KiemTraChinhSach
        KiemTraChinhSach: Đối chiếu Purchase Reference,\nNgoại quan, IMEI & Chính sách
    }

    DANG_XAC_MINH --> DANG_XU_LY: EVT-S05\n(Xác định hướng xử lý nghiệp vụ)

    state DANG_XU_LY {
        [*] --> DangThucHienNoiBo

        DangThucHienNoiBo --> CheDoCho: EVT-S06 [Cần thông tin/kiểm định]\n(Gán wait_reason & hạn theo dõi)
        
        state CheDoCho {
            ChoKhach: Chờ khách bổ sung (EX-01, EX-05)
            ChoDoiTac: Chờ hãng/Tập đoàn kiểm định (EX-05)
            --
            [*] --> ChoKhach
            [*] --> ChoDoiTac
        }

        CheDoCho --> DangThucHienNoiBo: EVT-B06 / EVT-S07 [Nhận phản hồi]\n(Xóa wait_reason, tiếp tục xử lý)
    }

    DANG_XU_LY --> DA_GIAI_QUYET: EVT-S08\n(Có kết quả thực tế & Căn cứ xử lý)

    state DA_GIAI_QUYET {
        [*] --> ThongBaoVaBanGiao
        ThongBaoVaBanGiao: Liên hệ khách (EVT-B07)\n& Bàn giao thiết bị (EVT-B09)
    }

    DA_GIAI_QUYET --> DANG_XU_LY: EVT-B08 [Khách KHÔNG đồng ý] (EX-07)\n(Ghi lý do khiếu nại, giữ nguyên kết quả cũ)

    DA_GIAI_QUYET --> DONG: EVT-S10 [Thỏa mãn Closing Gates BR-06]\n(Đã thông báo + Đã bàn giao + Không còn task mở)

    state DONG {
        [*] --> HoanTatLuuTru
        HoanTatLuuTru: Khóa chỉnh sửa dữ liệu (Read-only)
    }

    DONG --> DANG_XAC_MINH: EVT-B10 [Lỗi tái phát sau đóng] (EX-08)\n(Quản lý/CSKH chuỗi duyệt Reopen, giữ nguyên Ticket ID)

    DONG --> [*]
```

### 4.2. Ma trận chuyển đổi trạng thái (State Transition Matrix)
Bảng kiểm tra tính toàn vẹn (Completeness & Reachability Matrix) giữa 5 trạng thái chính:

| Từ trạng thái (From State) | Đến trạng thái (To State) | Sự kiện kích hoạt (Triggering Event) | Điều kiện kiểm tra (Guard Condition) | Hành động hệ thống (System Action) |
| :--- | :--- | :--- | :--- | :--- |
| **`[*]` Khởi tạo** | `MOI_TIEP_NHAN` | EVT-B01 / EVT-S01 | Thỏa mãn minimum dataset (Customer, Store, Device) | Cấp `ticket_id`, gán `temporary_owner = Creator`, log tạo mới. |
| `MOI_TIEP_NHAN` | `DANG_XAC_MINH` | EVT-S04 | Owner mới có tài khoản hoạt động và có quyền tại chi nhánh; có `follow_up_date` | Gán `assigned_to_user`, chuyển trạng thái, tạo task xác minh. |
| `DANG_XAC_MINH` | `DANG_XU_LY` | EVT-S05 | Đã ghi nhận căn cứ xác minh và chọn hướng xử lý (`resolution_path`) | Cập nhật `status = 'PROCESSING'`, lưu thời điểm xác minh. |
| `DANG_XU_LY` | `DANG_XU_LY` *(Tự chuyển/Sub-state)* | EVT-S06 | Bắt buộc chọn `wait_reason`, nhập đơn vị phối hợp và ngày hẹn | Kích hoạt chế độ chờ, tạo task theo dõi định kỳ. |
| `DANG_XU_LY` | `DA_GIAI_QUYET` | EVT-S08 | Đã nhập `resolution_result` thực tế; `wait_reason == NULL` | Chuyển `RESOLVED`, sinh task bắt buộc thông báo và bàn giao. |
| `DA_GIAI_QUYET` | `DANG_XU_LY` *(Quay lại - Loop)* | **EVT-B08 (EX-07)** | Khách hàng từ chối phương án; người dùng có quyền cập nhật | Chuyển ngược `PROCESSING`, bắt buộc ghi lý do tranh chấp, tạo task khiếu nại, giữ kết quả cũ. |
| `DA_GIAI_QUYET` | `DONG` | **EVT-S10 (BR-06)** | `customer_notified == TRUE` VÀ (`device_returned == TRUE` nếu nhận máy) VÀ không còn task mở | Khóa dữ liệu, chuyển `CLOSED`, ghi nhận `closed_by` và `closed_at`. |
| `DONG` | `DANG_XAC_MINH` *(Mở lại - Reopen)* | **EVT-B10 (EX-08)** | Người thực hiện là Quản lý hoặc CSKH chuỗi; có lý do mở lại và Owner mới | Giữ `ticket_id`, chuyển `VERIFYING`, tăng `reopen_count`, tạo task xác minh mới. |

---

## 5. MA TRẬN TRUY VẾT TOÀN DIỆN (TRACEABILITY MATRIX)

Ma trận truy vết 4 chiều chứng minh tính liên kết chặt chẽ giữa:
$$\text{Customer Expectations \& Gaps Model (Mạnh)} \longleftrightarrow \text{User Use Cases (Nhật)} \longleftrightarrow \text{Events \& Responses (Huy)} \longleftrightarrow \text{Dữ liệu \& Cổng kiểm soát Frappe CRM}$$

| Kỳ vọng khách hàng & Khoảng lệch Gap (Võ Duy Mạnh) | Use Case người dùng (Nguyễn Đức Nhật) | Sự kiện & Phản ứng đối ứng (Trần Quang Huy) | Dữ liệu & Cổng kiểm soát Frappe CRM (TV4 Technical Design) |
| :--- | :--- | :--- | :--- |
| **G1.1 & G5.1 – Tiếp nhận thuận tiện:** Khách không muốn khai lại thông tin khi đến CH02; tiếp nhận ít bước. | **UC-01:** Tiếp nhận yêu cầu bảo hành hoặc đổi trả tại CH02 | **EVT-B01 & EVT-S01:** Tìm kiếm liên chi nhánh; nạp dữ liệu mua tham chiếu; lưu tạo ticket nhanh. | Tra cứu `Customer` & `Purchase_History_Ref`; tạo `Support_Ticket` với `receiving_store = 'CH02'`. |
| **G1.2, G2.5 & G5.2 – Thông tin chính xác:** Đúng người, đúng thiết bị, đúng lịch sử; tránh nhầm hồ sơ. | **UC-01-A1, UC-01-E3:** Xác thực khách và dữ liệu tối thiểu | **EVT-S02 & EVT-S11:** Validation Gate chặn lưu thiếu; cảnh báo nghi trùng SĐT không tự gộp. | Regex SĐT VN, `customer_id` ổn định, kiểm tra trường bắt buộc trên Form Desk v15. |
| **G1.3, G2.1, G3.2 & G5.4 – Trách nhiệm rõ:** Luôn có người chịu trách nhiệm; không bị "mất chủ" khi bàn giao. | **UC-02 (Bước 1):** Bàn giao trách nhiệm và xác nhận Owner | **EVT-S03, EVT-S04 & EVT-T04:** Chặn gán owner sai; chuyển giao có log; quét định kỳ ticket mất chủ. | Trường `assigned_to_user` bắt buộc; lưu `temporary_owner = Creator`; Scheduled task quét `unassigned`. |
| **G1.4, G2.3, G3.4 & G5.5 – Tiến độ minh bạch:** Khách biết đang ở bước nào; không bị bỏ quên khi chờ. | **UC-03:** Theo dõi tiến độ và xử lý trường hợp chờ | **EVT-S06, EVT-T01 & EVT-T02:** Gán `wait_reason` kèm hạn; nhắc việc đến hạn; cảnh báo đỏ quá hạn. | `wait_reason` (`CHO_KHACH`/`CHO_DOI_TAC`), `follow_up_date`, `Care_Task`, Workspace Alert. |
| **G1.5, G2.4 & G5.6 – Liên tục giữa hai cửa hàng:** CH01 và CH02 là một thể thống nhất; không làm lại từ đầu. | **UC-06 & UC-02-A4:** Chuyển giao ticket giữa hai cửa hàng | **EVT-B05 (EX-04):** Giữ nguyên `ticket_id` và lịch sử; đổi `processing_store` và User Permission. | Cập nhật `processing_store`, điều chỉnh `User Permission` theo chi nhánh, không tạo ticket mới. |
| **G1.6, G2.6, G3.6 & G5.7 – Kết quả & Đóng hoàn tất:** Kết quả minh bạch; chỉ đóng khi khách được thông báo và nhận máy. | **UC-04:** Thông báo và hoàn tất yêu cầu; đóng ticket | **EVT-B07, EVT-B09, EVT-S09 & EVT-S10:** Kiểm tra Closing Gates BR-06 chặn đóng thiếu; lưu bằng chứng. | Cờ `customer_notified`, `device_returned`, kiểm tra hoàn thành `Care_Task`, Workflow Closing Gate (D05). |
| **G2.7 & G4.5 – Xử lý ngoại lệ chuẩn:** Khách chưa đồng ý kết quả hoặc vấn đề tái phát sau đóng. | **UC-04-A1 & UC-05:** Khách từ chối kết quả; Mở lại ticket đã đóng | **EVT-B08 (EX-07) & EVT-B10 (EX-08):** Quay lại `PROCESSING` hoặc mở lại về `VERIFYING`, bảo toàn lịch sử. | Chuyển ngược workflow có lưu lý do tranh chấp; tăng `reopen_count`; bảo toàn toàn bộ `Audit_Log` cũ. |

---

## 6. ĐỐI CHIẾU TOÀN DIỆN 9 TÌNH HUỐNG NGOẠI LỆ (EX-01 ĐẾN EX-09)

Bảng đối chiếu cụ thể phản ứng của CRM và trách nhiệm con người cho toàn bộ 9 tình huống bắt buộc theo baseline:

| Mã | Tình huống ngoại lệ | Event tương ứng | Trạng thái kích hoạt | Phản ứng nghiệp vụ của CRM | Trách nhiệm con người |
| :---: | :--- | :---: | :---: | :--- | :--- |
| **EX-01** | Thiếu chứng từ / dữ liệu mua hàng tham chiếu | **EVT-B02** | `NEW` $\rightarrow$ `VERIFYING` | Cho phép lưu ticket ở chế độ "Chưa xác minh chứng từ"; tạo task bổ sung thông tin; không tự cam kết quyền lợi. | Nhân viên tiếp nhận máy bình thường, ghi rõ phần thiếu, giải thích khách và không hứa hẹn miễn phí. |
| **EX-02** | Sản phẩm rơi vỡ, vào nước hoặc tranh chấp ngoại quan | **EVT-B03** | `NEW` $\rightarrow$ `VERIFYING` | Bắt buộc đính kèm tệp ảnh bằng chứng; mở trường ghi nhận ý kiến hai chiều; lưu cờ tranh chấp ngoại quan. | Nhân viên chụp ảnh chi tiết, lập biên nhận hiện trạng khách ký xác nhận, chuyển cấp chuyên môn thẩm định. |
| **EX-03** | Lỗi chập chờn, không tái hiện tại quầy | **EVT-B04** | `VERIFYING` $\rightarrow$ `PROCESSING` | Ghi nhận cờ `Intermittent`; lưu mô tả điều kiện xuất hiện lỗi; gia hạn thời gian kiểm tra tăng cường. | Owner khai thác kỹ hoàn cảnh lỗi, gửi đơn vị bảo hành kiểm tra chuyên sâu, không vội kết luận bình thường. |
| **EX-04** | Cần chuyển ticket sang cửa hàng khác xử lý | **EVT-B05** | `VERIFYING` / `PROCESSING` | Giữ nguyên `ticket_id`, lịch sử và tệp đính kèm; cập nhật `processing_store`, điều chuyển quyền truy cập. | CSKH chuỗi ghi rõ lý do chuyển, bàn giao đích danh cho nhân sự chi nhánh nhận, thông báo cho khách nếu cần. |
| **EX-05** | Đang chờ khách hoặc đơn vị bảo hành | **EVT-S06** | `PROCESSING` (Chế độ chờ) | Ghi nhận `wait_reason`, ngày liên hệ gần nhất, ngày hẹn theo dõi; tạo Care Task nhắc việc tự động. | Owner theo dõi sát mốc hẹn, chủ động liên hệ đối tác hoặc khách hàng khi sắp đến hạn. |
| **EX-06** | Đến hạn theo dõi nhưng chưa có phản hồi | **EVT-T01** & **EVT-T02** | `PROCESSING` (Đến/Quá hạn) | Kích hoạt cảnh báo vàng/đỏ trên Desk UI; gửi cảnh báo Escalation cho Quản lý; cấm tự ý đóng hoặc tự gia hạn. | Owner khẩn trương liên hệ đôn đốc; Quản lý can thiệp hỗ trợ nếu đối tác chậm trễ. |
| **EX-07** | Khách chưa đồng ý kết quả giải quyết | **EVT-B08** | `RESOLVED` $\rightarrow$ `PROCESSING` | Chuyển ngược ticket về `PROCESSING`; bắt buộc nhập lý do khách không đồng ý; bảo toàn nguyên vẹn kết quả cũ. | Owner lắng nghe thắc mắc, ghi nhận trung thực, báo cáo Quản lý để hội chẩn lại phương án thỏa đáng hơn. |
| **EX-08** | Vấn đề phát sinh lại sau khi đã đóng ticket | **EVT-B10** | `CLOSED` $\rightarrow$ `VERIFYING` | Quản lý/CSKH chuỗi duyệt Reopen; giữ nguyên `ticket_id`; chuyển về `VERIFYING`; gán owner và hạn mới. | Quản lý thẩm định đúng lỗi cũ, chỉ định nhân sự chuyên môn thụ lý lại, giải thích rõ quy trình cho khách. |
| **EX-09** | Đủ điều kiện đổi nhưng kho chi nhánh hết hàng | **EVT-B11** | `PROCESSING` | Ghi nhận trở ngại thiếu hàng; cấm CRM tự động hoàn tiền/cấp voucher; kích hoạt task điều phối quản lý. | Owner báo cáo Quản lý; Quản lý liên hệ điều chuyển hàng liên kho hoặc tư vấn khách phương án thỏa thuận. |

---

## 7. RÀ SOÁT CHECKLIST NGHIỆM THU (BASELINE CHECKLIST)

Bảng đối chiếu kiểm tra 100% tiêu chí tuân thủ theo mục 14 của tài liệu baseline dùng chung:

- [x] **1. Dùng đúng case và hai cửa hàng:** Đúng mô hình CH01 (136 Nguyễn Thái Học) và CH02 (21A Hàng Bài); không phát sinh cửa hàng thứ ba ngoài phạm vi.
- [x] **2. Không dùng số điện thoại làm khóa chính duy nhất:** `customer_id` ổn định độc lập; SĐT chỉ dùng để tra cứu và kích hoạt cảnh báo nghi trùng (EVT-S11).
- [x] **3. Không áp đặt chính sách tùy tiện (1 đổi 1 30 ngày / SLA cố định):** Toàn bộ căn cứ thời gian và chính sách dựa trên thẩm định thực tế từng ca; hạn theo dõi `follow_up_date` là mốc liên hệ nội bộ, không cam kết bừa bãi.
- [x] **4. Khách hàng và đối tác không đăng nhập CRM trong MVP:** Mọi tương tác của khách hàng và đối tác ngoài đều được nhân viên/quản lý tiếp nhận và ghi nhận qua biểu mẫu CRM.
- [x] **5. Không biến tính năng Future-state thành hiện thực MVP:** Portal khách hàng, SMS/Zalo Brandname tự động, CSAT tự động và API real-time được gắn nhãn Mức C (Future-state).
- [x] **6. Dùng đúng 5 trạng thái chính chuẩn mực:** `Mới tiếp nhận` $\rightarrow$ `Đang xác minh` $\rightarrow$ `Đang xử lý` $\rightarrow$ `Đã giải quyết` $\rightarrow$ `Đóng`. Chờ khách và chờ đối tác là `wait_reason` (sub-status).
- [x] **7. CRM không tự động thực hiện các tác vụ phi thực tế:** CRM không tự sửa chữa, không tự hoàn tiền, không tự xuất kho, không tự kết luận đủ điều kiện bảo hành chỉ dựa vào ngày mua.
- [x] **8. Mọi ngoại lệ (EX-01 đến EX-09) đều nêu rõ phản ứng CRM và trách nhiệm con người:** Được phân tích chi tiết trong Bảng Event–Response Table và Bảng đối chiếu Mục 6.
- [x] **9. Phân định rõ ràng mức độ thông tin (A / B / C):** Được công bố minh bạch tại Mục 1.2 và duy trì xuyên suốt tài liệu.

---
*— HẾT TÀI LIỆU PRACTICE 7 (TRẦN QUANG HUY) —*
