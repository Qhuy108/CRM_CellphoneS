# CASE STUDY BASELINE
## TIẾP NHẬN VÀ THEO DÕI YÊU CẦU BẢO HÀNH / ĐỔI TRẢ – CRM CELLPHONES

- **Tài liệu dùng chung cho bài tập:** Gaps Model + Practice 6 + Practice 7
- **Môn học:** Quản trị mối quan hệ khách hàng (CRM)
- **Nền tảng triển khai:** Frappe Framework
- **Phạm vi mô phỏng:** B2C – hai cửa hàng
- **Mục đích:** Làm chuẩn chung để 4 thành viên phân tích cùng một case, cùng một phạm vi và cùng một thuật ngữ

---

## 0. Cách sử dụng tài liệu này

Tài liệu này là “baseline dùng chung” cho bài tập tuần này. Mọi thành viên dùng cùng case, cùng phạm vi, cùng workflow và cùng bộ trạng thái trước khi viết phần riêng. Nếu có thay đổi, nhóm phải chốt trước rồi cập nhật đồng bộ Gaps Model, Practice 6 và Practice 7.

| Thành viên | Phần chính tuần này | Đầu ra dùng baseline này |
| :--- | :--- | :--- |
| **Nguyễn Vũ Quang Huy** | Nền tảng case + tích hợp bài | Chốt case, scope, thuật ngữ; kiểm tra liên kết Gaps ↔ Use Case ↔ Event; ghép bài cuối. |
| **Võ Duy Mạnh** | Gaps Model – SERVQUAL | Phân tích Gap 1–5 trên đúng case; mỗi Gap có biểu hiện, nguyên nhân, ảnh hưởng và giải pháp. |
| **Nguyễn Đức Nhật** | Practice 6 – User Requirements | Xác định người dùng cần làm gì; lập Use Case; viết normal/alternative/exception flow cho các Use Case trọng tâm. |
| **Trần Quang Huy** | Practice 7 – Events & Responses | Lập event list, phân loại Business/Signal/Temporal, Event–Response Table; đề xuất State Transition nếu cần. |

> **Nguyên tắc quan trọng:** Nhóm hướng tới mô hình hóa sát bối cảnh CellphoneS nhất có thể, nhưng không giả định đã biết toàn bộ CRM nội bộ thật. Mỗi chi tiết cần được phân loại là:
> - **(A)** có căn cứ;
> - **(B)** mô hình hóa/đề xuất hợp lý; hoặc
> - **(C)** future-state vượt MVP sinh viên.

---

## 1. Bài tập tuần này và cách nối ba phần

Bài tập gồm hai nhánh nhưng nên làm thành một mạch thống nhất:

1. **Gaps Model trong sách CRM:** Lấy case sản phẩm/dịch vụ của đồ án, phân tích từng Gap và đưa giải pháp.
2. **Practice 6 – Chapter 3, Software Requirements Essentials:** Tập trung vào “người dùng cần làm gì với giải pháp”, từ đó xác định scenario/use case, luồng chuẩn, luồng thay thế và ngoại lệ.
3. **Practice 7 – Chapter 3:** Nhận diện các sự kiện mà nghiệp vụ/hệ thống phải phản ứng, phân loại Business / Signal / Temporal và lập Event–Response Table.

**Chuỗi logic đề xuất:**  
$$\text{Case bảo hành/đổi trả} \longrightarrow \text{Customer Expectations} \longrightarrow \text{Gaps} \longrightarrow \text{User Goals / Use Cases} \longrightarrow \text{Events / Responses} \longrightarrow \text{Yêu cầu hệ thống và quy trình cần hoàn thiện}$$

---

## 2. Case Study được chốt

- **Case:** Tiếp nhận và theo dõi yêu cầu bảo hành/đổi trả của khách hàng tại hệ thống cửa hàng CellphoneS.
- **Mô tả:** Case tập trung vào hành trình hậu mãi từ khi khách phát sinh yêu cầu đến khi có kết quả, thông báo/bàn giao và đóng ticket. Trọng tâm CRM là duy trì lịch sử phục vụ, trách nhiệm xử lý, tiến độ, phối hợp giữa cửa hàng và đơn vị bảo hành, đồng thời hỗ trợ trải nghiệm khách hàng nhất quán.

### 2.1. Lý do lựa chọn
- Đây là điểm chạm nhạy cảm trong hành trình sau mua: khách đang gặp sự cố nên tính minh bạch, trách nhiệm và tốc độ phản hồi ảnh hưởng mạnh đến trải nghiệm.
- Quy trình có nhiều bên tham gia: khách hàng, nhân viên cửa hàng, CSKH, quản lý, đơn vị bảo hành/đối tác; do đó phù hợp để phân tích Gaps Model.
- Yêu cầu tính liên tục dữ liệu cao: một yêu cầu có thể được tiếp nhận ở cửa hàng khác nơi mua, chuyển người xử lý hoặc chờ phản hồi bên ngoài.
- Case có đủ tương tác người–hệ thống và sự kiện theo thời gian để làm Practice 6 và Practice 7 một cách rõ ràng.

---

## 3. Mức độ mô phỏng và quy tắc sử dụng thông tin

| Mức | Cách hiểu | Ví dụ trong bài |
| :--- | :--- | :--- |
| **A – Có căn cứ** | Thông tin từ sách, tài liệu nhóm hoặc nguồn công khai có thể kiểm tra. | CellphoneS có dịch vụ hậu mãi; đồ án dùng hai cửa hàng; CRM nhóm theo dõi ticket, người phụ trách và tiến độ. |
| **B – Mô hình hóa / Đề xuất** | Nhóm thiết kế cách vận hành hợp lý để hệ thống sát thực tế và giải quyết vấn đề. | Người tạo tạm chịu trách nhiệm ticket trước khi bàn giao; bộ quy tắc ngoại lệ; cách ghi trạng thái chờ. |
| **C – Future-state** | Ý tưởng phù hợp CRM thực tế nhưng vượt MVP hoặc chưa có căn cứ triển khai hiện tại. | API với đối tác bảo hành, portal khách hàng, SMS/Zalo tự động, CSAT tự động, đồng bộ real-time. |

> **Cách diễn đạt khuyến nghị:**  
> *“Case được xây dựng dựa trên thông tin có thể nghiên cứu từ CellphoneS, kết hợp kiến thức CRM và yêu cầu hệ thống của đồ án. Nhóm hướng tới mô hình hóa sát bối cảnh thực tế nhất có thể; một số bước được đề xuất hoặc giản lược để phù hợp phạm vi và năng lực triển khai của đồ án sinh viên.”*

---

## 4. Phạm vi Case Study

### 4.1. In-scope – Dùng cho bài tuần này
- Khách mua sản phẩm tại một cửa hàng nhưng có thể yêu cầu bảo hành/đổi trả tại cửa hàng còn lại trong mô hình hai cửa hàng.
- Xác định khách hàng, tra cứu dữ liệu mua hàng tham chiếu và ghi nhận sản phẩm/thiết bị liên quan.
- Tiếp nhận thiết bị nếu có: ghi ngoại quan, phụ kiện, IMEI/Serial phù hợp loại thiết bị và bằng chứng hình ảnh khi cần.
- Tạo ticket, phân công trách nhiệm, lưu cửa hàng tiếp nhận và cửa hàng đang xử lý.
- Xác minh thông tin, chính sách áp dụng và phân loại hướng xử lý.
- Phối hợp đơn vị bảo hành/đối tác; ghi nội dung gửi, mã tham chiếu nếu có, phản hồi và hạn theo dõi.
- Theo dõi trạng thái, lý do chờ, công việc đến hạn/quá hạn, lịch sử tương tác và chuyển giao.
- Ghi nhận kết quả, thông báo khách, bàn giao thiết bị/phương án bàn giao, đóng hoặc mở lại ticket khi cần.
- Audit/log các thay đổi quan trọng để truy được vòng đời ticket.

### 4.2. Out-of-scope – Không biến thành chức năng lõi của MVP
- Sửa chữa kỹ thuật chi tiết, thay linh kiện, quy trình kiểm định chuyên sâu của hãng/đối tác.
- Thanh toán, kế toán, hoàn tiền, xuất/nhập kho, định giá hoặc quyết toán chi phí.
- Logistics vật lý chi tiết giữa cửa hàng – trung tâm bảo hành – hãng.
- Customer portal, tài khoản khách hàng hoặc đối tác bảo hành đăng nhập CRM.
- Tích hợp real-time với POS/ERP, Zalo/Facebook/SMS, hệ thống SMember hoặc hệ thống đối tác trong MVP.
- Tự động quyết định khách đủ/không đủ điều kiện bảo hành hoặc đổi trả chỉ dựa trên thời gian.

> **Lưu ý:** Không dùng như quy tắc chung: *“1 đổi 1 trong 30 ngày”*, *“SLA 15 ngày”*, *“máy dùng tạm”*, *“tặng voucher khi quá hạn”* hoặc một chính sách cụ thể cho mọi sản phẩm, trừ khi nhóm có nguồn và xác định đúng phạm vi sản phẩm/thời điểm.

---

## 5. Actors & Stakeholders dùng chung

| Bên liên quan | Vai trò trong case | Quan hệ với CRM MVP |
| :--- | :--- | :--- |
| **Khách hàng** | Phát sinh yêu cầu, cung cấp thông tin, nhận cập nhật và kết quả, nhận/bàn giao thiết bị. | Không đăng nhập CRM; giao tiếp qua nhân viên/kênh ngoài hệ thống. |
| **Nhân viên tiếp nhận / CSKH cửa hàng** | Tra cứu khách, ghi tình trạng thiết bị, tạo/cập nhật ticket, phối hợp và thông báo khách. | User trực tiếp. |
| **CSKH cấp chuỗi** | Điều phối trường hợp liên cửa hàng, hỗ trợ truy cập chi tiết trong phạm vi được cấp. | User trực tiếp theo quyền. |
| **Quản lý cửa hàng** | Theo dõi ticket, hỗ trợ phân công/escalation và xử lý ngoại lệ theo thẩm quyền. | User trực tiếp. |
| **Đơn vị bảo hành / Đối tác / Hãng** | Kiểm tra hoặc xử lý chuyên môn và phản hồi kết quả. | Bên ngoài CRM; MVP ghi nhận trao đổi qua nhân viên. |
| **Quản trị hệ thống** | Quản lý tài khoản, quyền, cấu hình và dữ liệu hệ thống. | User kỹ thuật; không tự quyết định chính sách kinh doanh. |

> **Lưu ý về “Kỹ thuật viên tại cửa hàng”:** Chỉ tách thành user class riêng nếu nhóm quyết định bổ sung vai trò, quyền và dữ liệu tương ứng. Trong baseline hiện tại có thể mô hình hóa chức năng kiểm tra ban đầu dưới *“Nhân viên tiếp nhận/CSKH cửa hàng”*.

---

## 6. Standard Workflow – Happy Path dùng chung

**Scenario chuẩn để tất cả thành viên cùng phân tích:**  
> *Khách mua sản phẩm tại CH01, sau đó mang thiết bị đến CH02 yêu cầu bảo hành. CH02 tiếp nhận, tra cứu dữ liệu tham chiếu, tạo ticket, xác minh, phối hợp đơn vị bảo hành nếu cần, theo dõi tiến độ, nhận kết quả, thông báo khách, bàn giao và đóng ticket.*

| Giai đoạn | Hoạt động nghiệp vụ chính | CRM cần hỗ trợ |
| :--- | :--- | :--- |
| **1. Tiếp nhận** | Xác định khách; ghi loại yêu cầu, mô tả, cửa hàng tiếp nhận; nếu nhận máy thì ghi tình trạng, phụ kiện, IMEI/Serial phù hợp và bằng chứng. | Tìm khách/dữ liệu mua tham chiếu; tạo Ticket ID; lưu người tạo, thời điểm, cửa hàng và dữ liệu thiết bị. |
| **2. Xác minh** | Kiểm tra thông tin khách, thiết bị, chứng từ và chính sách áp dụng; phân loại hướng xử lý. | Chuyển “Đang xác minh”; gắn owner; đặt hạn theo dõi; lưu kết quả xác minh. |
| **3. Xử lý / Phối hợp** | Xử lý nội bộ hoặc gửi thông tin/thiết bị cho đơn vị bảo hành; có thể chờ khách hoặc chờ đối tác. | Chuyển “Đang xử lý”; lưu `wait_reason`, đơn vị phối hợp, nội dung gửi/nhận, mã tham chiếu và lịch theo dõi. |
| **4. Có kết quả** | Nhận kết quả thực tế; ghi kết luận, căn cứ và phương án tiếp theo. | Chuyển “Đã giải quyết”; tạo việc thông báo/bàn giao nếu chưa hoàn tất. |
| **5. Thông báo / Bàn giao** | Thông báo kết quả; nếu có thiết bị thì bàn giao hoặc xác nhận phương án bàn giao. | Lưu kênh, thời điểm, nội dung, kết quả thông báo; lưu bằng chứng bàn giao nếu áp dụng. |
| **6. Đóng / Mở lại** | Đóng khi đủ điều kiện; nếu khách chưa đồng ý hoặc vấn đề tái phát thì quay lại xử lý/xác minh. | Kiểm điều kiện đóng; hoàn thành/hủy việc còn mở có lý do; log; hỗ trợ mở lại mà không mất lịch sử cũ. |

---

## 7. Bộ trạng thái ticket thống nhất

| Trạng thái chính | Ý nghĩa | Ghi chú |
| :--- | :--- | :--- |
| **Mới tiếp nhận** | Ticket vừa được tạo, đã có dữ liệu tối thiểu. | Đề xuất: nếu chưa có owner chính thức, người tạo chịu trách nhiệm tạm thời đến khi bàn giao hợp lệ. |
| **Đang xác minh** | Đang kiểm tra khách/thiết bị/chứng từ/chính sách. | Phải có owner hợp lệ và hạn theo dõi. |
| **Đang xử lý** | Đã có hướng xử lý, đang thực hiện/phối hợp. | *“Chờ khách”* và *“Chờ đơn vị bảo hành”* là lý do chờ (`wait_reason` / sub-status), không phải trạng thái chính độc lập. |
| **Đã giải quyết** | Đã có kết quả nhưng chưa chắc đã đủ điều kiện đóng. | Có thể còn việc thông báo hoặc bàn giao; nếu khách chưa đồng ý có thể quay lại xử lý. |
| **Đóng** | Đã hoàn tất điều kiện đóng. | Giữ toàn bộ lịch sử; có thể mở lại khi phát sinh lại vấn đề theo quyền. |

> **Quy ước:** Không dùng bộ trạng thái kiểu *“Đã gửi hãng → Đang sửa → Chờ linh kiện → Đang vận chuyển → Chờ khách nhận”* làm vòng đời chính. Các chi tiết này lưu dưới dạng tiến độ phối hợp, sub-status hoặc tương tác (interaction).

---

## 8. Customer Expectations – Đầu vào cho Gaps Model

| Kỳ vọng | Diễn giải trong case |
| :--- | :--- |
| **Tiếp nhận thuận tiện** | Khách không phải trải qua quá nhiều bước chỉ để ghi nhận yêu cầu. |
| **Thông tin chính xác** | Nhân viên xác định đúng khách, sản phẩm và lịch sử liên quan; không nhầm hoặc tự gộp hồ sơ. |
| **Quy trình rõ ràng** | Khách hiểu yêu cầu đang ở bước nào và bước tiếp theo là gì. |
| **Trách nhiệm rõ** | Ticket có người chịu trách nhiệm xuyên suốt; chuyển giao không làm “mất chủ”. |
| **Tiến độ minh bạch** | Nhân viên có thể tra cứu trạng thái hiện tại và chủ động cập nhật cho khách khi có thay đổi/đến hạn. |
| **Liên tục giữa cửa hàng** | Khách không phải kể lại toàn bộ lịch sử chỉ vì đến cửa hàng khác hoặc gặp nhân viên khác. |
| **Kết quả được thông báo** | Có kết quả rõ ràng, ghi nhận lần thông báo và hoàn tất bàn giao khi có thiết bị. |

*(Lưu ý: Các kỳ vọng trên là cơ sở mô hình hóa; không ghi nhận là số liệu khảo sát thực tế nếu chưa triển khai khảo sát).*

---

## 9. Exceptions & Edge Cases – Bắt buộc dùng khi làm Practice 6/7

| Mã | Tình huống | Hướng xử lý ở mức CRM |
| :--- | :--- | :--- |
| **EX-01** | Không tìm thấy hoặc thiếu chứng từ/dữ liệu mua hàng. | Vẫn tiếp nhận ticket; ghi thông tin thiếu; chuyển xác minh/chờ phù hợp; không tự cam kết miễn phí. |
| **EX-02** | Sản phẩm có dấu hiệu rơi vỡ/ngấm nước hoặc tranh chấp ngoại quan. | Ghi tình trạng, bằng chứng hình ảnh khi cần; chuyển xác minh/chuyên môn; CRM lưu căn cứ, không tự kết luận. |
| **EX-03** | Lỗi chập chờn, không tái hiện tại quầy. | Ghi mô tả lỗi và bằng chứng; chuyển kiểm tra chuyên sâu/theo dõi; giữ ticket mở và hạn theo dõi. |
| **EX-04** | Cần chuyển ticket sang cửa hàng khác. | Giữ nguyên Ticket ID và lịch sử; cập nhật cửa hàng xử lý/quyền; không tạo ticket mới chỉ vì chuyển cửa hàng. |
| **EX-05** | Đang chờ khách hoặc đơn vị bảo hành. | Ghi lý do chờ, lần liên hệ/gửi gần nhất, hạn theo dõi và công việc nhắc đang mở. |
| **EX-06** | Đến hạn theo dõi nhưng chưa có phản hồi/kết quả. | Temporal event: đánh dấu việc đến hạn/quá hạn, nhắc owner và hiển thị cho quản lý theo quyền. |
| **EX-07** | Khách chưa đồng ý kết quả. | Từ “Đã giải quyết” quay lại “Đang xử lý”; ghi lý do, owner và hạn mới; không mất kết quả cũ. |
| **EX-08** | Vấn đề phát sinh lại sau khi đã đóng. | Mở lại về “Đang xác minh”; ghi lý do và hạn mới; giữ lịch sử lần đóng trước. |
| **EX-09** | Đủ điều kiện đổi nhưng phương án đổi trực tiếp chưa thể thực hiện (ví dụ thiếu hàng). | CRM ghi tình trạng và chuyển người có thẩm quyền xử lý ngoại lệ; không tự hoàn tiền/đổi model/tặng voucher. |

---

## 10. Core Data – Dữ liệu cả nhóm phải dùng nhất quán

| Nhóm dữ liệu | Trường / Ý nghĩa chính |
| :--- | :--- |
| **Customer** | `customer_id` ổn định; họ tên; số điện thoại/thông tin liên hệ. *(Số điện thoại dùng tìm kiếm/cảnh báo trùng, không phải khóa chính duy nhất).* |
| **Product / Device** | Model; IMEI hoặc Serial Number nếu áp dụng; thiết bị thực nhận hay không. |
| **Purchase Reference** | Mã giao dịch/hóa đơn tham chiếu, ngày mua, nguồn và thời điểm cập nhật. *(Dữ liệu tham chiếu không phải chứng từ kế toán do CRM phát hành).* |
| **Ticket** | `ticket_id`, loại yêu cầu, mô tả, mức ưu tiên, cửa hàng tiếp nhận, cửa hàng xử lý. |
| **Device Condition** | Ngoại quan, phụ kiện, ghi chú và tệp/hình ảnh bằng chứng khi cần. |
| **Responsibility** | Owner hiện tại, người tạo, lịch sử phân công/chuyển giao. |
| **Progress** | `status`, `wait_reason`, hạn theo dõi, thời điểm cập nhật, việc đến hạn/quá hạn. |
| **Partner Coordination** | Đơn vị phối hợp, nội dung gửi, thời điểm gửi/nhận, mã tham chiếu bên ngoài nếu có. |
| **Interaction** | Lịch sử trao đổi với khách/đối tác; kênh, thời điểm, nội dung, người ghi. |
| **Resolution / Notification** | Kết quả, căn cứ, lần thông báo khách, tình trạng xác nhận/bàn giao, thời điểm đóng/mở lại. |
| **Audit** | Log thay đổi quan trọng, chuyển trạng thái, phân công, mở lại và thay đổi dữ liệu nhạy cảm. |

---

## 11. Phân biệt MVP và Future-state

| MVP dùng cho bài/đồ án hiện tại | Future-state – Có thể đưa vào giải pháp mở rộng |
| :--- | :--- |
| Nhân viên cập nhật phản hồi đối tác vào CRM. | API real-time với hệ thống bảo hành/Điện Thoại Vui/hãng. |
| Khách nhận thông tin qua nhân viên/kênh ngoài CRM. | Portal/link tự tra cứu trạng thái cho khách. |
| Nhắc việc và cảnh báo nội bộ. | SMS/Zalo tự động theo trạng thái. |
| Báo cáo ticket/tiến độ dựa trên dữ liệu đã nhập. | CSAT tự động sau khi đóng ticket; phân tích CX nâng cao. |
| Dữ liệu mua hàng mẫu/tham chiếu. | Đồng bộ POS/ERP/SMember thật. |

---

## 12. Khung đầu ra cho từng thành viên

### 12.1. Võ Duy Mạnh – Gaps Model
- Phân tích toàn bộ 5 Gap dựa trên Customer Expectations và workflow chung (không chia lẻ Gap cho người khác).
- Cấu trúc từng Gap:
  1. Ý nghĩa Gap theo sách CRM.
  2. Biểu hiện có thể xảy ra trong case bảo hành/đổi trả.
  3. Nguyên nhân: con người / quy trình / tiêu chuẩn / công nghệ / truyền thông.
  4. Ảnh hưởng đến trải nghiệm khách hàng và hoạt động CRM.
  5. Giải pháp: kết hợp quy trình + tiêu chuẩn + dữ liệu + CRM.
- *Gợi ý:* Gap 5 là khoảng cách giữa Expected Service và Perceived Service (chịu tác động bởi Gap 1–4).

### 12.2. Nguyễn Đức Nhật – Practice 6
- Bắt đầu từ câu hỏi: *“Người dùng cần làm gì với giải pháp?”*
- Đầu ra:
  - Danh sách user classes liên quan.
  - Danh sách Use Case theo mục tiêu người dùng (tên bắt đầu bằng động từ).
  - Đặc tả 3–5 Use Case trọng tâm: actor, goal, trigger, precondition, normal flow, alternative flow, exception, postcondition.
  - Bao phủ: tiếp nhận ticket, xác minh/xử lý, theo dõi chờ, thông báo/bàn giao, đóng/mở lại hoặc chuyển cửa hàng.

### 12.3. Trần Quang Huy – Practice 7
- Đầu ra:
  - Event List: phân loại Business Event / Signal Event / Temporal Event.
  - Event–Response Table: `Event ID`, `Event`, `Type`, `Condition/Current State`, `Expected Response`, `Data/Log affected`.
  - Ưu tiên: khách yêu cầu, chuyển ticket, nhận phản hồi đối tác, khách chưa đồng ý, đến hạn theo dõi, mở lại ticket.
  - Đề xuất State Transition Diagram tương ứng 5 trạng thái chính.

### 12.4. Nguyễn Vũ Quang Huy – Tích hợp và kiểm tra
- Duy trì tính thống nhất của baseline; không để lệch scope hay trạng thái.
- Rà soát tính logic: Gaps Model ↔ Practice 6 ↔ Practice 7.
- Đảm bảo Future-state không bị đánh đồng với MVP.
- Lập bảng ma trận truy vết (Traceability Matrix) và ghép bài hoàn chỉnh.

---

## 13. Bảng truy vết mẫu để ghép bài cuối

| Vấn đề / Kỳ vọng | Gaps Model có thể phân tích | Practice 6 – User task / Use case | Practice 7 – Event / Response |
| :--- | :--- | :--- | :--- |
| **Khách không biết tiến độ** | Gap liên quan hiểu kỳ vọng, tiêu chuẩn, thực thi hoặc truyền thông. | Theo dõi/Cập nhật ticket; Thông báo khách. | Đến hạn theo dõi; nhận phản hồi đối tác → nhắc owner/cập nhật tiến độ. |
| **Khách phải kể lại khi đổi cửa hàng** | Gap liên quan đến tiêu chuẩn và thực thi dịch vụ. | Tra cứu lịch sử; Chuyển ticket giữa cửa hàng. | Event chuyển cửa hàng → giữ Ticket ID, lịch sử và quyền phù hợp. |
| **Ticket bị bỏ quên khi chờ** | Gap thực thi dịch vụ. | Quản lý việc theo dõi; Điều phối ticket. | Temporal event đến hạn → cảnh báo/nhắc việc. |
| **Khách chưa đồng ý kết quả** | Gap giữa dịch vụ kỳ vọng và cảm nhận. | Ghi phản hồi; Tiếp tục xử lý / mở lại. | Business event khách phản hồi không đồng ý → RESOLVED → PROCESSING. |

---

## 14. Checklist trước khi mỗi thành viên gửi phần của mình

- [ ] Dùng đúng case và hai cửa hàng; không tự thêm cửa hàng thứ ba.
- [ ] Không dùng số điện thoại làm khóa chính duy nhất của Customer.
- [ ] Không áp “1 đổi 1 trong 30 ngày” hoặc SLA cụ thể cho toàn bộ sản phẩm nếu chưa có nguồn/phạm vi.
- [ ] Không để đối tác bảo hành hoặc khách hàng đăng nhập CRM trong MVP.
- [ ] Không biến API real-time, portal, SMS/Zalo tự động, CSAT tự động thành chức năng đã có của MVP.
- [ ] Dùng đúng 5 trạng thái chính: **Mới tiếp nhận – Đang xác minh – Đang xử lý – Đã giải quyết – Đóng**; chờ khách/chờ bảo hành là lý do chờ (`wait_reason`).
- [ ] Không để CRM tự sửa chữa, hoàn tiền, xuất kho hoặc tự kết luận đủ điều kiện bảo hành.
- [ ] Mọi exception phải nêu phản ứng của CRM và trách nhiệm con người, không chỉ mô tả tình huống.
- [ ] Phân biệt rõ: thông tin có căn cứ (A), đề xuất mô hình hóa (B) và future-state (C).

---

## 15. Tài liệu tham chiếu dùng để lập baseline

1. **Karl Wiegers & Candase Hokanson**, *Software Requirements Essentials: Core Practices for Successful Business Analysis* – Chapter 3, Practice 6 và Practice 7.
2. **Daniel D. Prior, Francis Buttle & Stan Maklan**, *Customer Relationship Management: Concepts, Applications, and Technologies*, 5th Edition (2024) – SERVQUAL / Gaps Model.
3. `NguyenHuy.docx` – Nền tảng phân tích, phạm vi, mục tiêu, người dùng và nguyên tắc chung của nhóm.
4. `Manh (2).docx` – Nghiệp vụ, hành trình khách hàng, hỏi đáp mô phỏng, Problem Statement và Root Cause Analysis.
5. `Nhat (2).docx` – System Requirements, Use Case, trạng thái ticket, quyền, tiêu chí kiểm tra và truy vết.
6. `TranHuy.docx` – ERD, từ điển dữ liệu và thiết kế kỹ thuật hiện tại.

---
*— HẾT TÀI LIỆU BASELINE DÙNG CHUNG —*