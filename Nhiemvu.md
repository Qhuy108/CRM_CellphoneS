Dưới đây là bản hướng dẫn chi tiết từng nhiệm vụ, bám sát nghiệp vụ thực tế của chuỗi bán lẻ công nghệ **CellphoneS** (bán lẻ đa kênh, hệ thống Smember, dịch vụ bảo hành/sửa chữa) và môi trường triển khai **Frappe Framework**.

---

### Nhiệm vụ 1: Rút ra các thực thể từ yêu cầu và quy trình

Nhiệm vụ này nhằm xác định **phạm vi lưu trữ dữ liệu** của hệ thống CRM. Bạn cần sàng lọc danh sách ứng viên, quyết định giữ hay bỏ thực thể nào dựa trên bài toán CellphoneS:

* **Khách hàng (Customer):** **Chắc chắn giữ.** Quản lý thông tin định danh, hạng thành viên Smember (Smember, S-VIP...), lịch sử chi tiêu, điểm tích lũy.
* **Nhu cầu tư vấn / Khách tiềm năng (Lead/Opportunity):** **Nên giữ nếu có nghiệp vụ Telesales/Bán hàng đa kênh.** Dùng để theo dõi khách để lại thông tin đặt trước iPhone/sản phẩm mới ra mắt hoặc cần tư vấn trả góp nhưng chưa mua ngay.
* **Tương tác (Interaction/Activity):** **Chắc chắn giữ.** Ghi lại nhật ký mỗi lần tiếp xúc qua Hotline, Chat Zalo/Fanpage, Web form hoặc trực tiếp tại cửa hàng.
* **Phiếu hỗ trợ (Ticket/Issue):** **Chắc chắn giữ.** Nghiệp vụ cốt lõi để tiếp nhận khiếu nại, yêu cầu đổi trả, thắc mắc đơn hàng hoặc chuyển bảo hành.
* **Lịch sử xử lý (Ticket Log/Audit Trail):** **Cân nhắc:** Trong Frappe đã tích hợp sẵn tính năng *Activity Log/Timeline/Version* cho mọi bản ghi. Nếu cần lưu bước duyệt hoặc phân công chuyên biệt, bạn có thể tạo một bảng con (Child Table) gắn vào Phiếu hỗ trợ thay vì một thực thể độc lập lớn.
* **Người dùng (User/Staff):** **Chắc chắn giữ.** Đại diện cho nhân viên trực tổng đài, nhân viên CSKH, quản lý hoặc nhân viên kỹ thuật tiếp nhận xử lý.
* **Sản phẩm tham chiếu (Item/Product Reference):** **Chỉ giữ ở mức tham chiếu.** Không quản lý tồn kho hay giá vốn phức tạp; chỉ lưu `Mã sản phẩm (SKU)`, `Tên sản phẩm`, `Dòng sản phẩm` (Apple, Samsung, Laptop...) và tùy chọn `Số Serial/IMEI` để phục vụ tra cứu bảo hành.

> **Đầu ra cần nộp:** Bảng danh mục thực thể gồm: *Tên thực thể*, *Mục đích sử dụng*, *Quyết định (Chọn/Loại bỏ)* và *Lý do biện luận*.

---

### Nhiệm vụ 2: Xây dựng ERD Logic & giải quyết 2 bài toán dữ liệu

Xây dựng sơ đồ quan hệ thực thể mức logic, thể hiện: Khóa chính (PK), Khóa ngoại (FK), các thuộc tính quan trọng, bản số quan hệ (Cardinality) và tính bắt buộc (Optionality).

```
[Khách hàng] 1 ---- 0..N ---- [Tương tác]
     1
     |
   0..N
     |
[Phiếu hỗ trợ] 0..N ---- 1 [Người dùng (Agent xử lý)]
     |
    0..N
     |
[Sản phẩm tham chiếu]

```

#### Xử lý 2 bài toán nghiệp vụ đặc thù:

1. **Khách hàng chưa xác định (Khách vãng lai, ẩn danh):**
* *Bối cảnh CellphoneS:* Khách gọi tổng đài chỉ hỏi giá, chat Fanpage bằng nick phụ, không cung cấp số điện thoại.
* *Giải pháp thiết kế:*
* Trong thực thể `Tương tác` hoặc `Phiếu hỗ trợ`, trường tham chiếu `customer_id` phải để ở chế độ **Tùy chọn (Nullable)**.
* Hoặc tạo một bản ghi mặc định trong bảng Khách hàng mang tên **"Khách vãng lai"** (`customer_id = CUST-GUEST`) để gom các tương tác không định danh, tránh vi phạm ràng buộc toàn vẹn dữ liệu.




2. **Xử lý hồ sơ trùng lặp (Deduplication):**
* *Bối cảnh CellphoneS:* Khách dùng 2 số điện thoại khác nhau hoặc khai báo tên khác nhau khi mua tại cửa hàng và trên website.
* *Giải pháp:*
* Chọn **Số điện thoại** làm định danh duy nhất (Unique Key) cho khách hàng cá nhân.
* Thiết kế quy trình/cờ **Merge Customer**: Khi phát hiện 2 hồ sơ thuộc về 1 người (cùng CCCD, cùng email), hệ thống chuyển toàn bộ `Ticket` và `Interaction` của hồ sơ phụ sang hồ sơ chính, sau đó đánh dấu hồ sơ phụ là `Archived/Merged`.





---

### Nhiệm vụ 3: Lập từ điển dữ liệu (Data Dictionary)

Lập bảng đặc tả chi tiết cho từng trường dữ liệu trong các thực thể đã chọn.

* **Các cột bắt buộc trong bảng từ điển:**
1. Tên trường (Logical Name & Physical Name, ví dụ: `Trạng thái phiếu` - `ticket_status`)
2. Ý nghĩa nghiệp vụ
3. Kiểu dữ liệu (Data Type: Data, Select/Enum, Link, Int, Currency, Text, Datetime...)
4. Bắt buộc (Mandatory: Yes/No)
5. Duy nhất (Unique: Yes/No)
6. Tập giá trị hợp lệ (Allowed Values / Enum options)
7. Nguồn dữ liệu (Người dùng nhập, Hệ thống tự sinh, Tính toán từ công thức)
8. Quy tắc kiểm tra (Validation Rules: Regex SĐT Việt Nam 10 chữ số, ngày đóng phiếu $\ge$ ngày tạo...).


* **Thống nhất với State Diagram của Nhật:** Trường `status` của `Phiếu hỗ trợ` và `Nhu cầu tư vấn` phải khớp chính xác các trạng thái trong biểu đồ của Nhật, ví dụ:
* `Mới tạo (Open)` $\rightarrow$ `Đang xử lý (In Progress)` $\rightarrow$ `Chờ linh kiện/Hãng kiểm tra (Pending Vendor)` $\rightarrow$ `Đã giải quyết (Resolved)` $\rightarrow$ `Đóng phiếu (Closed)`.



---

### Nhiệm vụ 4: Phác thảo 4–6 màn hình ưu tiên theo Use Case

Mô tả layout và cấu trúc cho các màn hình then chốt (kèm wireframe khung hộp đơn giản hoặc giải thích dạng văn bản):

| Màn hình | Thông tin hiển thị (Xem) | Dữ liệu nhập/thao tác (Nhập) | Hành động chính (Actions) |
| --- | --- | --- | --- |
| **1. Danh sách khách hàng** | Mã KH, Họ tên, SĐT, Hạng Smember, Điểm thưởng, Ngày tương tác cuối | Bộ lọc (theo hạng, khu vực), Ô tìm kiếm SĐT/Tên | Thêm mới KH, Xuất danh sách, Xem chi tiết |
| **2. Hồ sơ khách hàng 360°** | Thông tin cá nhân, Lịch sử mua hàng (IMEI/Sản phẩm đã mua), Tab Lịch sử Ticket, Tab Nhật ký tương tác | Ghi chú CSKH, Cập nhật thông tin liên hệ | Tạo phiếu hỗ trợ mới, Ghi nhận tương tác, Cộng/trừ điểm Smember |
| **3. Ghi nhận tương tác** | Tên KH, Kênh (Hotline, Zalo, Showroom), Lịch sử 3 lần liên hệ gần nhất | Kênh tiếp xúc, Mục đích (Hỏi giá/Bảo hành/Khiếu nại), Nội dung tóm tắt, Mức độ hài lòng | Lưu tương tác, Tạo nhanh Ticket từ tương tác |
| **4. Danh sách phiếu hỗ trợ** | Mã Ticket, Khách hàng, Sản phẩm liên quan, Độ ưu tiên (Gấp/Thường), Trạng thái, SLA còn lại, Người phụ trách | Bộ lọc trạng thái (Kanban hoặc List), Lọc theo nhân viên/chi nhánh | Tạo ticket, Gán việc nhanh, Đổi trạng thái |
| **5. Chi tiết phiếu hỗ trợ** | Thông tin lỗi/khiếu nại, Tiến trình xử lý (Timeline), Hạn xử lý (SLA), Thông tin bảo hành hãng | Cập nhật kết quả xử lý, Thêm nhân viên hỗ trợ, Đính kèm ảnh lỗi/biên lai | Chuyển trạng thái, Phân công lại, Đóng phiếu, Gửi SMS/Zalo cho khách |
| **6. Dashboard CSKH** | Biểu đồ tỉ lệ phiếu xử lý đúng hạn (SLA), Top 5 sản phẩm bị lỗi nhiều nhất, Số lượng ticket theo kênh | Chọn khoảng thời gian, Chọn chi nhánh/kênh tiếp nhận | Xuất báo cáo, Xem chi tiết từng chỉ số |

---

### Nhiệm vụ 5: Bảng ánh xạ mô hình sang Frappe Framework

Frappe quản lý mọi thứ dưới dạng **DocType**. Bạn cần lập bảng chuyển đổi từ thiết kế logic sang các thành phần kỹ thuật của Frappe:

* **DocType Mapping:**
* *Khách hàng* $\rightarrow$ Dùng chuẩn `Customer` của Frappe/ERPNext (thêm Custom Fields: `custom_smember_tier`, `custom_points`).
* *Phiếu hỗ trợ* $\rightarrow$ Dùng chuẩn `Issue` hoặc tạo mới `Custom DocType: Support Ticket`.
* *Tương tác* $\rightarrow$ Dùng `Communication` hoặc tạo mới `Customer Interaction`.
* *Sản phẩm* $\rightarrow$ Dùng chuẩn `Item`.


* **Quan hệ trong Frappe:**
* Quan hệ 1-N dùng kiểu trường `Link` (Ví dụ trong `Issue` có trường `customer` kiểu Link tới `Customer`).
* Bảng con (Lịch sử xử lý chi tiết, Danh sách linh kiện kiểm tra) dùng `Table` field liên kết tới `Child DocType`.


* **Workflow:** Định nghĩa các State (`Draft`, `Open`, `Working`, `Pending`, `Closed`) và các Transition Rule (Ai được quyền chuyển trạng thái nào).
* **Phân quyền (Role Permission Manager):**
* *CSKH Agent:* Tạo, đọc, sửa Ticket của chính mình/nhóm mình.
* *CSKH Manager:* Đọc tất cả, xóa, ghi đè trạng thái, đóng ticket quá hạn.
* *Kỹ thuật viên:* Chỉ sửa các trường ghi chú xử lý kỹ thuật.



---

### Nhiệm vụ 6: Vẽ 1–2 Sequence Diagram cho luồng xử lý chính

Chọn các luồng có logic nghiệp vụ cần kiểm tra điều kiện.

#### Gợi ý luồng 1: Tiếp nhận và Phân công Phiếu hỗ trợ

* **Tác nhân (Actors/Objects):** `CSKH Agent`, `Giao diện Ticket (UI)`, `Bộ điều khiển (Ticket Controller)`, `Cơ sở dữ liệu (Database)`, `Hệ thống thông báo (Notification)`.
* **Tiến trình:**
1. Agent gửi form tạo phiếu kèm loại lỗi (ví dụ: "Lỗi nguồn iPhone 15 Pro").
2. Hệ thống kiểm tra điều kiện: Khách còn hạn bảo hành / SLA cho dòng máy cao cấp.
3. Controller tự động định tuyến (Routing): Nếu là lỗi phần cứng $\rightarrow$ Gán sang nhóm Kỹ thuật/Điện Thoại Vui; nếu khiếu nại dịch vụ $\rightarrow$ Gán cho Trưởng ca.
4. Ghi bản ghi vào Database $\rightarrow$ Trả về thông báo thành công cho Agent và gửi Email/Notification cho người được gán.



#### Gợi ý luồng 2: Đóng phiếu hỗ trợ kèm kiểm tra điều kiện

* **Tiến trình:**
1. Nhân viên nhấn "Hoàn thành / Đóng phiếu".
2. Hệ thống kiểm tra điều kiện:
* *Đã nhập nguyên nhân và hướng khắc phục chưa?* (Nếu chưa $\rightarrow$ Báo lỗi bắt buộc nhập).
* *Biên bản bàn giao/xác nhận từ khách đã được tải lên chưa?*


3. Cập nhật trạng thái phiếu thành `Resolved/Closed`, tính toán thời gian xử lý thực tế so với SLA.
4. Kích hoạt gửi tin nhắn Zalo ZNS / SMS tự động mời khách đánh giá mức độ hài lòng (CSAT).



---

### Nhiệm vụ 7: Rà soát chéo (Cross-check với Nhật)

Lập ma trận đối soát (Traceability Matrix) để đảm bảo tính nhất quán giữa dữ liệu, giao diện và quy trình:

1. **Giao diện vs. Dữ liệu:**
* Mọi ô nhập hoặc hiển thị trên 6 màn hình (Nhiệm vụ 4) đều phải có một trường tương ứng trong Từ điển dữ liệu (Nhiệm vụ 3).
* Không để xuất hiện "thuộc tính mồ côi" (lưu trong cơ sở dữ liệu nhưng không màn hình nào dùng, hoặc màn hình yêu cầu nhập nhưng cơ sở dữ liệu không có chỗ lưu).


2. **Quy trình vs. Trạng thái:**
* So sánh danh sách trạng thái của Phiếu hỗ trợ trong ERD/Frappe với **State Diagram** của Nhật. Các sự kiện chuyển trạng thái (Transitions) của Nhật phải được hỗ trợ bởi các nút bấm (Buttons/Actions) trên màn hình chi tiết phiếu.


3. **Phân quyền vs. Nghiệp vụ:**
* Kiểm tra xem các Role trong Frappe có bao phủ hết các vai trò tham gia vào quy trình mà Nhật đã mô tả hay chưa.


-----------Yêu cầu đầu ra------
Các tiêu chí cụ thể và yêu cầu thực hiện cho từng sản phẩm bàn giao (D01 – D05) cùng các tiêu chí hoàn thành, phối hợp trong dự án:

---

### 1. Chi tiết tiêu chí cho từng sản phẩm bàn giao (D01 – D05)



| Mã

 | Sản phẩm bàn giao

 | Tiêu chí nội dung & Yêu cầu chất lượng cần đạt |
| --- | --- | --- |
| **D01**<br> | **ERD và giải thích các quan hệ**<br> | * **Cấu trúc kỹ thuật:** Sơ đồ ERD logic thể hiện rõ Khóa chính (PK), Khóa ngoại (FK), các thuộc tính cốt lõi, lực lượng quan hệ (1-1, 1-N, N-N) và tính bắt buộc/tùy chọn (Mandatory/Optional).

<br>

<br>* **Không trùng khái niệm:** Phân rã chuẩn hóa, không tạo các thực thể dư thừa hoặc trùng lắp bản chất.

<br>

<br>* **Thuyết minh kèm theo:** Giải thích logic nghiệp vụ của các liên kết; mô tả rõ cơ chế lưu trữ khách vãng lai/chưa định danh (trường `customer_id` Nullable hoặc bản ghi Guest mặc định) và phương án xử lý hồ sơ trùng lặp (Merge rule dựa trên SĐT/CCCD). |
| **D02**<br> | **Từ điển dữ liệu và quy tắc kiểm tra**<br> | * **Đầy đủ 8 trường thông tin chuẩn:** Tên logic, Tên vật lý (field name), Ý nghĩa, Kiểu dữ liệu dự kiến (Frappe/SQL), Bắt buộc (Yes/No), Duy nhất (Yes/No), Giá trị hợp lệ (Enum/List), Nguồn dữ liệu.<br>

<br>* **Quy tắc kiểm tra (Validation):** Nêu rõ các ràng buộc logic (Regex số điện thoại Việt Nam, ngày đóng phiếu $\ge$ ngày tạo, điều kiện trạng thái).<br>

<br>* **Khớp nối State Diagram:** Các giá trị trong trường `status` (phiếu hỗ trợ, yêu cầu tư vấn) phải đồng nhất 100% với biểu đồ trạng thái của Nhật.

 |
| **D03**<br> | **Wireframe các màn hình ưu tiên**<br> | * **Số lượng:** 4–6 màn hình trọng tâm (Danh sách KH, Hồ sơ 360°, Ghi nhận tương tác, Danh sách phiếu hỗ trợ, Chi tiết phiếu, Dashboard/Báo cáo).<br>

<br>* **Cấu trúc mỗi màn hình phải bóc tách 3 phần:**<br>

<br>  1. *Thông tin hiển thị (Xem):* Dữ liệu chỉ đọc lấy từ đâu.<br>

<br>  2. *Thông tin nhập/chỉnh sửa (Nhập):* Các trường dữ liệu form thao tác.<br>

<br>  3. *Hành động (Actions):* Nút bấm thực thi (Lưu, Chuyển trạng thái, Gán việc, Đóng phiếu, In biên nhận). |
| **D04**<br> | **Bảng ánh xạ thiết kế sang Frappe**<br> | * **DocType Mapping:** Xác định thực thể nào dùng DocType chuẩn (`Customer`, `Issue`, `Communication`) và thực thể nào tạo `Custom DocType`.<br>

<br>* **Loại trường liên kết:** Phân định rõ quan hệ 1-N dùng trường kiểu `Link`, danh sách con (lịch sử xử lý, bảng linh kiện) dùng trường kiểu `Table` (Child DocType).<br>

<br>* **Workflow & Quyền:** Định nghĩa các bước chuyển trạng thái (States & Transitions) và ma trận phân quyền (Role Permission Manager: CSKH Agent, Quản lý, Kỹ thuật).<br>

<br>* **Lưu ý:** Ghi chú rõ đây là mô hình ánh xạ dự kiến, chưa phải cấu trúc lưu trữ chính thức đã cố định trên database. |
| **D05**<br> | **Sequence Diagram chọn lọc và ghi chú thiết kế**<br> | * **Số lượng & Phạm vi:** 1–2 biểu đồ UML cho luồng nghiệp vụ có logic rẽ nhánh phức tạp (ví dụ: *Tạo & Tự động phân công phiếu theo nhóm lỗi/SLA*, hoặc *Đóng phiếu kèm kiểm tra điều kiện nghiệm thu/khảo sát CSAT*).<br>

<br>* **Đối tượng tham gia:** Phân tách rõ tầng `Actor/User`, `Giao diện (UI)`, `Bộ điều khiển (Controller/Backend)`, `Cơ sở dữ liệu (Database)`.<br>

<br>* **Đánh dấu:** Phải ghi chú rõ nhãn **"Thiết kế đề xuất" (Proposed Design)**.

 |

---

### 2. Tiêu chí hoàn thành & Cơ chế phối hợp



**Về mặt kỹ thuật mô hình hóa:**

* **Tính rõ ràng và chính xác:** ERD bắt buộc phải thể hiện chuẩn xác quan hệ và bản số (bội số), không để quan hệ lơ lửng hoặc quan hệ N-N trực tiếp chưa qua bảng trung gian.


* **Phạm vi dữ liệu:** Dữ liệu thiết kế phải bao phủ đầy đủ các trường thông tin cần thiết phục vụ toàn bộ yêu cầu nghiệp vụ đã thống nhất của CellphoneS.


* **Phân định ERD và Class Diagram:** ERD chỉ tập trung mô hình hóa dữ liệu tĩnh lưu trữ trong cơ sở dữ liệu, không dùng để thay thế cho Class Diagram. Chỉ bổ sung thêm Class Diagram khi có các xử lý hướng đối tượng hoặc phương thức logic phức tạp đòi hỏi phải đặc tả cấu trúc lớp.



**Về mặt phối hợp và kiểm soát chất lượng (Cross-check):**

* **Với Nhật (Kiểm tra quy trình & luồng tương tác):**

* Đọc chéo dữ liệu thiết kế với tài liệu **Use Case** và **State Diagram** của Nhật.


* Đảm bảo mọi trạng thái quy trình đều có nút bấm kích hoạt trên wireframe (D03) và có trường lưu trữ tương ứng trong từ điển dữ liệu (D02).




* **Với Mạnh (Kiểm tra nghiệp vụ thực tế):**

* Rà soát tính thực tế của dữ liệu theo nghiệp vụ bán lẻ công nghệ CellphoneS (chính sách phân hạng Smember, quy trình bảo hành/sửa chữa liên kết Điện Thoại Vui, quản lý số IMEI/Serial, cam kết SLA giải quyết khiếu nại).





**Về tiến độ mốc kiểm soát:**

* **Mốc M3 (Milestone 3):** Hoàn thành **Bản nháp (Draft)** của toàn bộ 5 sản phẩm (D01 – D05) để chuyển cho Nhật và Mạnh đối soát chéo.


* **Mốc M4 (Milestone 4):** Xử lý toàn bộ các điểm phản biện, chuẩn hóa dữ liệu và **Chốt bàn giao chính thức (Final)**.