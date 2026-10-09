**Practice 6**  
**Yêu cầu người dùng**

*Tiếp nhận và theo dõi yêu cầu bảo hành đổi trả*  
*CRM CellphoneS*

Người dùng cần tiếp nhận một yêu cầu hậu mãi, xác minh thông tin, duy trì trách nhiệm xử lý và hoàn tất phục vụ khách trên cùng một ticket. Bài làm xác định các mục tiêu đó và đặc tả cách người dùng tương tác với CRM trong mô hình hai cửa hàng.

**1 Phạm vi và cơ sở mô hình hóa**

Tình huống chuẩn: khách mua thiết bị tại CH01, sau đó mang thiết bị đến CH02 yêu cầu bảo hành. CH02 tiếp nhận, xác minh, phối hợp đơn vị bảo hành khi cần, theo dõi kết quả, thông báo và bàn giao trước khi đóng ticket. Yêu cầu đổi trả dùng cùng khung theo dõi nhưng căn cứ xử lý phải phù hợp từng trường hợp. 

CRM hỗ trợ hồ sơ khách, dữ liệu mua hàng tham chiếu, tình trạng thiết bị, ticket, người phụ trách, lịch theo dõi, tương tác và nhật ký thay đổi. Sửa chữa, vận chuyển vật lý, thanh toán, hoàn tiền và xuất nhập kho được thực hiện ngoài phạm vi CRM. Khách hàng và đối tác không đăng nhập hệ thống trong MVP. 

Thuật ngữ: ticket là phiếu yêu cầu hỗ trợ; owner là người đang chịu trách nhiệm xử lý; wait\_reason là lý do chờ; MVP là phạm vi triển khai tối thiểu của đồ án.

Phân loại thông tin:

| Mức | Áp dụng trong bài |
| :---: | :---- |
| A | Case, phạm vi hai cửa hàng, nhóm người dùng, năm trạng thái chính và các tình huống EX-01 đến EX-09 được lấy từ baseline của nhóm. |
| B | Chi tiết các bước, kiểm tra trước khi lưu, quyền mở lại và cách bàn giao trách nhiệm là mô hình đề xuất cho đồ án; không khẳng định đây là quy trình nội bộ thật của CellphoneS. |
| C | Portal khách hàng, kết nối đối tác thời gian thực, SMS/Zalo tự động và CSAT tự động là hướng mở rộng; không dùng làm bước bắt buộc trong các use case dưới đây. |

Các mô tả use case chi tiết thuộc mức B và tuân theo dữ kiện mức A. Mã UC trong bài được đánh riêng cho Practice 6 của case bảo hành/đổi trả. Nội dung tập trung vào nhiệm vụ và kết quả người dùng cần đạt. 

**2 Nhóm người dùng và mục tiêu sử dụng**

**Người dùng trực tiếp**

| Nhóm người dùng | Mục tiêu khi sử dụng CRM | Phạm vi trách nhiệm |
| :---- | :---- | :---- |
| Nhân viên tiếp nhận hoặc CSKH cửa hàng | Ghi nhận yêu cầu, theo dõi việc cần làm và cập nhật kết quả cho khách. | Tác nghiệp trong phạm vi được cấp; người tạo tạm chịu trách nhiệm đến khi bàn giao hợp lệ. |
| CSKH cấp chuỗi | Duy trì phục vụ liên tục khi khách đến cửa hàng khác hoặc cần chuyển ticket. | Hỗ trợ truy cập thông tin và điều phối CH01-CH02 theo quyền. |
| Quản lý cửa hàng | Biết ticket nào chưa được xử lý, đang chờ hoặc quá hạn để phân công và can thiệp. | Quản lý trong cửa hàng; xử lý ngoại lệ theo thẩm quyền và hỗ trợ mở lại ticket. |
| Quản trị hệ thống | Bảo đảm nhân viên có tài khoản, quyền và cấu hình phù hợp để thực hiện công việc. | Hỗ trợ kỹ thuật; không tự quyết định điều kiện bảo hành, đổi trả hoặc kết quả nghiệp vụ. |

Nhóm tư vấn/bán hàng tham gia case khi làm nhiệm vụ tiếp nhận hoặc CSKH; không mở thêm vai trò kỹ thuật viên trong phạm vi hiện tại. Quản trị hệ thống là người dùng hỗ trợ, không là tác nhân chính của năm use case hậu mãi. 

**Bên liên quan bên ngoài**

| Bên liên quan | Nhu cầu và cách tham gia |
| :---- | :---- |
| Khách hàng | Cung cấp yêu cầu/thông tin/thiết bị, nhận cập nhật và kết quả, phản hồi hoặc nhận bàn giao. Trao đổi qua nhân viên; không thao tác trực tiếp trên CRM. |
| Đơn vị bảo hành hoặc đối tác | Nhận thông tin cần kiểm tra, thực hiện chuyên môn bên ngoài và phản hồi kết quả. Nhân viên ghi lại trao đổi vào CRM; đối tác không có tài khoản MVP. |

**Yêu cầu người dùng theo mục tiêu**

| Mã | Người dùng cần làm gì | Use case |
| :---: | :---- | :---- |
| UR-01 | Nhân viên cần tiếp nhận cả yêu cầu thiếu chứng từ, có mã theo dõi và người phụ trách. | UC-01 |
| UR-02 | Owner cần đối chiếu khách, thiết bị, chứng từ và chính sách để chọn hướng xử lý có căn cứ. | UC-02 |
| UR-03 | Owner và quản lý cần biết đang chờ ai, chờ gì và hạn liên hệ tiếp để tránh bỏ quên ticket. | UC-03 |
| UR-04 | Nhân viên cần thông báo đúng kết quả và hoàn tất bàn giao trước khi đóng ticket. | UC-04 |
| UR-05 | Quản lý hoặc CSKH chuỗi cần mở lại đúng ticket khi vấn đề cũ tái phát để giữ lịch sử. | UC-05 |
| UR-06 | CSKH chuỗi cần chuyển người/cửa hàng xử lý mà vẫn giữ mã ticket và lịch sử. | UC-06 |

**3 Danh mục use case và quy tắc dùng chung**

| Mã và tên use case | Tác nhân chính | Kết quả người dùng đạt được |
| :---- | :---- | :---- |
| UC-01 Tiếp nhận yêu cầu bảo hành hoặc đổi trả | Nhân viên tiếp nhận/CSKH cửa hàng | Yêu cầu có Ticket ID, thông tin tiếp nhận và người tạm chịu trách nhiệm. |
| UC-02 Xác minh và xử lý yêu cầu | Owner; quản lý/CSKH chuỗi phối hợp theo quyền | Có hướng xử lý và kết quả thực tế được ghi nhận cùng căn cứ. |
| UC-03 Theo dõi tiến độ và xử lý trường hợp chờ | Owner; quản lý theo dõi | Tiến độ, lý do chờ và việc tiếp theo rõ ràng; các lần liên hệ có lịch sử. |
| UC-04 Thông báo và hoàn tất yêu cầu | Owner hoặc nhân viên được giao | Khách nhận thông tin; hoàn tất điều kiện bàn giao và đóng ticket. |
| UC-05 Mở lại ticket đã đóng | Quản lý cửa hàng/CSKH chuỗi có quyền | Vấn đề cũ được xác minh lại, có owner/hạn mới và giữ lịch sử trước đó. |
| UC-06 Chuyển giao ticket giữa cửa hàng | CSKH cấp chuỗi có quyền điều phối | Ticket giữ nguyên mã và trạng thái, đổi cửa hàng xử lý/owner/quyền hợp lệ. |

UC-01 đến UC-05 được đặc tả đầy đủ tại mục 4\. UC-06 là use case bổ trợ; luồng chuyển giao được diễn giải tại UC-02-A4 và đối chiếu với EX-04. Tra cứu khách, ghi nhận tương tác và phân công là các bước hỗ trợ trong mục tiêu nghiệp vụ tương ứng.

**Quy tắc nghiệp vụ dùng trong các luồng**

**BR-01** Customer ID ổn định; số điện thoại dùng tra cứu/cảnh báo nghi trùng, không là khóa chính duy nhất và không làm căn cứ tự gộp khách. 

**BR-02** Mới tiếp nhận phải có người chịu trách nhiệm tạm thời là người tạo. Trước khi bắt đầu xác minh phải xác nhận owner hợp lệ và hạn theo dõi; việc chuyển giao chỉ hoàn tất khi người nhận đủ quyền và đã được xác nhận nhận việc. 

**BR-03** Giữ đúng năm trạng thái chính: Mới tiếp nhận, Đang xác minh, Đang xử lý, Đã giải quyết, Đóng. Chờ khách và Chờ đơn vị bảo hành là wait\_reason dưới Đang xử lý; khi hết chờ, xóa lý do chờ hiện tại và giữ lịch sử khoảng chờ. 

**BR-04** Dữ liệu mua hàng chỉ là tham chiếu. Người có thẩm quyền xác minh chính sách và ghi căn cứ; CRM không tự kết luận miễn phí/đủ điều kiện chỉ dựa vào số ngày mua. 

**BR-05** Hạn theo dõi là thời điểm phải kiểm tra hoặc liên hệ tiếp, không mặc nhiên là thời hạn cam kết sửa xong. Quá hạn không tự giải quyết, đóng hoặc gia hạn ticket. 

**BR-06** Đóng khi có kết quả, ghi nhận thông báo và phản hồi của khách; nếu đã nhận thiết bị thì có bằng chứng bàn giao hoặc phương án bàn giao được khách xác nhận. Các việc còn mở phải được hoàn thành/hủy có lý do, không được hủy để bỏ qua điều kiện thông báo/bàn giao. 

**BR-07** Chuyển cửa hàng và mở lại đều giữ Ticket ID, lịch sử và dấu vết trách nhiệm. Truy cập liên cửa hàng chỉ cấp dữ liệu cần thiết cho công việc, không mặc định mở toàn bộ lịch sử khách.

**Kịch bản xuyên suốt**

Khách mua tại CH01 và mang thiết bị đến CH02. Nhân viên CH02 thực hiện UC-01, sau đó owner dùng UC-02 để xác minh và xử lý. Nếu cần phản hồi bên ngoài, owner dùng UC-03 để theo dõi chờ; có phản hồi thì tiếp tục UC-02. Khi có kết quả thực tế, nhân viên dùng UC-04 để thông báo, bàn giao và đóng. UC-05 chỉ dùng nếu vấn đề phát sinh lại sau đóng; UC-06 chỉ phát sinh khi cần chuyển cửa hàng xử lý.

Cửa hàng mua hàng có thể là CH01, cửa hàng tiếp nhận là CH02 và cửa hàng xử lý hiện tại ban đầu là CH02. Khách đến cửa hàng khác nơi mua không tự tạo ra một lần chuyển ticket, vì ticket chưa tồn tại trước lúc tiếp nhận.

**4 Đặc tả năm use case trọng tâm**

Tiền điều kiện chung: tài khoản đang hoạt động và có quyền thực hiện thao tác trên cửa hàng/ticket liên quan. Mỗi thao tác thành công phải lưu được thay đổi và log tương ứng; lỗi lưu không được tạo trạng thái hoặc chuyển giao dở dang. Các luồng thay thế là nhánh hợp lệ; ngoại lệ nêu tình huống cần can thiệp hoặc không thể hoàn tất bước đang thực hiện.

**UC 01 Tiếp nhận yêu cầu bảo hành hoặc đổi trả**

**Tác nhân:** Nhân viên tiếp nhận/CSKH cửa hàng; CSKH cấp chuỗi hỗ trợ tra cứu liên cửa hàng khi cần.

**Mục tiêu:** Ghi nhận đúng khách và yêu cầu, tạo một ticket có thông tin ban đầu và người chịu trách nhiệm.

**Kích hoạt:** Khách liên hệ hoặc mang thiết bị đến CH02 yêu cầu bảo hành/đổi trả.

**Tiền điều kiện:** Nhân viên có quyền tạo ticket tại CH02. Khách chưa cần có hóa đơn hoặc kết luận đủ điều kiện bảo hành. Có đủ thông tin tối thiểu để xác định/liên hệ khách trước khi lưu.

**Luồng chuẩn**

| Bước | Người dùng thực hiện | CRM đáp ứng |
| :---: | :---- | :---- |
| 1 | Hỏi nhu cầu và tìm khách theo mã hoặc thông tin liên hệ. | Trả kết quả trong phạm vi; cảnh báo nghi trùng mà không tự gộp hồ sơ. |
| 2 | Chọn đúng khách, đối chiếu dữ liệu mua hàng tham chiếu có thể truy cập. | Liên kết Customer ID và dữ liệu mua phù hợp; hiển thị nguồn/thời điểm cập nhật. |
| 3 | Ghi loại yêu cầu, mô tả vấn đề, mức ưu tiên và cửa hàng tiếp nhận. | Chuẩn bị ticket với cửa hàng tiếp nhận và xử lý ban đầu là CH02. |
| 4 | Nếu nhận máy, ghi model, IMEI/Serial phù hợp, ngoại quan, phụ kiện và ảnh khi cần. | Lưu các dòng tình trạng và bằng chứng gắn với ticket; ghi rõ có nhận thiết bị hay không. |
| 5 | Kiểm tra lại thông tin với khách và chọn lưu. | Kiểm tra quyền, dữ liệu tối thiểu và quan hệ khách-thiết bị-tham chiếu; báo rõ trường cần sửa. |
| 6 | Xác nhận tiếp nhận, ghi nhận cách liên hệ khách. | Cấp một Ticket ID, trạng thái Mới tiếp nhận; lưu người/lúc tạo và người tạo tạm chịu trách nhiệm; ghi log. |
| 7 | Thông báo mã theo dõi và bước xác minh tiếp theo cho khách. | Lưu ghi nhận đã tiếp nhận; cho người có quyền mở ticket để thực hiện UC-02. |

**Luồng thay thế**

**UC-01-A1 · Chưa có hồ sơ khách tại bước 1\.** Nhân viên kiểm tra nghi trùng, tạo hồ sơ đủ thông tin liên hệ, nhận Customer ID rồi quay lại bước 2\. Không tạo hồ sơ khác chỉ vì khách mua tại CH01.

**UC-01-A2 · Thiếu chứng từ tại bước 2, EX-01.** Nhân viên ghi thông tin đang thiếu và yêu cầu khách bổ sung. Tiếp tục bước 3-7, để dữ liệu mua ở tình trạng chưa xác minh; không tự cam kết bảo hành miễn phí.

**UC-01-A3 · Không nhận thiết bị tại bước 4\.** Nhân viên ghi chỉ tiếp nhận thông tin; bỏ phần biên bản nhận máy. Nếu thiết bị không có/không đọc được serial, ghi lý do; không nhập serial giả để lưu.

**Ngoại lệ**

**UC-01-E1 · Hồ sơ CH01 ngoài phạm vi.** Nhân viên tiếp nhận thông tin từ khách và nhờ CSKH chuỗi xác minh/cấp dữ liệu cần thiết. Hoàn tất liên kết và lưu sau khi xác định đúng hồ sơ được phép dùng; không mở lịch sử trái phép hoặc tạo khách trùng để bỏ qua quyền.

**UC-01-E2 · Tranh chấp ngoại quan, EX-02.** Nhân viên ghi nhận cả phản ánh của khách và quan sát thực tế, ảnh khi cần; đánh dấu nội dung cần xác minh và chuyển quản lý/chuyên môn. Có thể lưu ticket nhưng chưa kết luận điều kiện bảo hành.

**UC-01-E3 · Thiếu dữ liệu tối thiểu hoặc lưu lỗi.** CRM chỉ rõ thông tin thiếu/lỗi và không xác nhận tạo thành công. Nhân viên bổ sung hoặc kiểm tra đã có Ticket ID trước khi gửi lại để tránh nhân đôi.

**Hậu điều kiện**

Thành công: một ticket Mới tiếp nhận có Customer ID, thông tin thiết bị nếu thực nhận, cửa hàng và người tạm chịu trách nhiệm. Chứng từ chưa đủ vẫn được ghi là chưa xác minh. Thất bại: không tạo ticket hoặc dữ liệu thiết bị mồ côi; nhân viên tiếp tục bổ sung/kiểm tra.

**UC 02 Xác minh và xử lý yêu cầu**

**Tác nhân:** Owner của ticket; quản lý cửa hàng phân công/xử lý ngoại lệ; CSKH chuỗi điều phối theo quyền.

**Mục tiêu:** Xác định hướng xử lý có căn cứ và ghi nhận kết quả thực tế của yêu cầu.

**Kích hoạt:** Ticket mới cần xác minh, ticket đã mở lại hoặc công việc đang xử lý nhận thêm thông tin.

**Tiền điều kiện:** Ticket chưa Đóng. Trước khi chuyển Đang xác minh phải có owner hoạt động, có quyền tại cửa hàng xử lý và hạn theo dõi. Các công việc chuyên môn thực hiện bên ngoài CRM.

**Luồng chuẩn**

| Bước | Người dùng thực hiện | CRM đáp ứng |
| :---: | :---- | :---- |
| 1 | Người có quyền xác nhận owner, hạn theo dõi và nhận bàn giao từ người tạo. | Lưu trách nhiệm hiện tại và log; không để ticket mất người chịu trách nhiệm khi đổi owner. |
| 2 | Owner bắt đầu kiểm tra ticket Mới tiếp nhận. | Chuyển Đang xác minh; ticket đã ở trạng thái này không tạo một lần chuyển trùng. |
| 3 | Đối chiếu khách, model/serial, tình trạng thiết bị, chứng từ và chính sách đúng trường hợp. | Cho xem dữ liệu được cấp, ghi thông tin đã kiểm tra/còn thiếu và căn cứ; không tự phán quyết chính sách. |
| 4 | Ghi kết quả xác minh, hướng xử lý và người/đơn vị cần phối hợp. | Chuyển Đang xử lý khi có hướng thực hiện; lưu owner và hạn theo dõi. |
| 5 | Thực hiện hỗ trợ nội bộ hoặc liên hệ đơn vị bảo hành bằng kênh ngoài CRM. | Lưu nội dung gửi, thời điểm, đơn vị và mã tham chiếu nếu có. Nếu phải chờ, tiếp tục UC-03. |
| 6 | Khi có phản hồi và kết quả thực tế, kiểm tra căn cứ rồi ghi kết quả xử lý. | Lưu kết quả, tài liệu/ghi nhận liên quan và người xác nhận; giữ các lần xử lý trước. |
| 7 | Xác nhận công việc xử lý đã có kết quả và chuyển sang hoàn tất phục vụ. | Chuyển Đã giải quyết; tạo/duy trì việc thông báo, bàn giao còn thiếu để thực hiện UC-04. |

**Luồng thay thế**

**UC-02-A1 · Đã xác minh hoặc quay lại xử lý.** Với ticket đang Đang xử lý, owner tiếp tục từ bước 5 theo hướng đã ghi. Nếu khách chưa đồng ý kết quả, UC-04-A1 đưa ticket về Đang xử lý; không chuyển ngược về Mới tiếp nhận.

**UC-02-A2 · Cần thêm chứng từ, EX-01.** Ở bước 3, owner ghi phần thiếu và hướng thu thập bổ sung. Khi đã xác định việc tiếp theo, chuyển Đang xử lý rồi dùng UC-03 với lý do Chờ khách. Không coi hồ sơ chưa đủ là kết luận không đủ điều kiện.

**UC-02-A3 · Không đủ điều kiện hoặc khách rút yêu cầu.** Người có thẩm quyền xác nhận căn cứ; owner ghi đúng kết quả thực tế, tiếp tục bước 6-7 và UC-04. Ticket không bị xóa và vẫn phải giải quyết việc thông báo/bàn giao nếu đã nhận máy.

**UC-02-A4 · Chuyển cửa hàng, UC-06 và EX-04.** Với ticket chưa Đóng, quản lý/owner đề nghị CSKH chuỗi chuyển. CSKH chuỗi chọn cửa hàng nhận và người nhận có quyền, xác nhận nhận việc, ghi lý do và hạn. CRM giữ Ticket ID/trạng thái/cửa hàng tiếp nhận; cập nhật cửa hàng xử lý, owner, việc mở và quyền cùng log. Cửa hàng nhận xem dữ liệu cần cho chính ticket; bên gửi không tiếp tục cập nhật, chỉ xem thông tin chuyển giao theo quyền. Nếu chưa đủ điều kiện bàn giao, giữ owner/cửa hàng cũ. Người nhận tiếp tục tại bước phù hợp trạng thái hiện tại.

**Ngoại lệ**

**UC-02-E1 · Rơi vỡ, ngấm nước hoặc ngoại quan chưa thống nhất, EX-02.** Owner ghi bằng chứng, yêu cầu quản lý/chuyên môn xác minh; chưa có căn cứ thì chưa xác nhận miễn phí hay từ chối tự động. Duy trì owner và hạn theo dõi.

**UC-02-E2 · Không tái hiện lỗi tại quầy, EX-03.** Ghi hoàn cảnh xuất hiện lỗi và bằng chứng khách cung cấp; xin kiểm tra chuyên sâu/theo dõi. Giữ Đang xác minh khi chưa có hướng, hoặc Đang xử lý khi đã có hướng kiểm tra; không giải quyết chỉ vì không thấy lỗi ngay.

**UC-02-E3 · Phương án đổi chưa thực hiện được, EX-09.** Owner ghi lý do thực tế, báo quản lý/người có thẩm quyền và đặt hạn theo dõi. Giữ Đang xử lý; nếu chỉ chờ phương án nội bộ thì ghi trong tiến độ, không gán sai lý do Chờ đơn vị bảo hành. Không tự đổi model, hoàn tiền, cấp voucher hoặc cập nhật tồn kho.

**UC-02-E4 · Người nhận không hợp lệ hoặc thay đổi đồng thời.** CRM từ chối cập nhật và giữ trạng thái/phân công trước đó; người có quyền chọn lại owner hoặc tải phiên bản mới để đối chiếu, không ghi đè âm thầm.

**Hậu điều kiện**

Nếu đã có kết quả thực tế: ticket Đã giải quyết, có căn cứ, người xác nhận và việc hoàn tất phục vụ. Nếu còn xác minh/xử lý/chờ: ticket vẫn mở, có owner và hạn theo dõi. Chuyển cửa hàng không làm mất lịch sử hoặc tạo Ticket ID mới.

**UC 03 Theo dõi tiến độ và xử lý trường hợp chờ**

**Tác nhân:** Owner của ticket; quản lý cửa hàng theo dõi/can thiệp; CSKH chuỗi phối hợp theo quyền.

**Mục tiêu:** Biết tiến độ hiện tại, đang chờ ai và việc cần thực hiện tiếp theo để ticket không bị bỏ quên.

**Kích hoạt:** Owner cần kiểm tra tiến độ, phải chờ thông tin, nhận phản hồi hoặc thấy việc đến hạn/quá hạn.

**Tiền điều kiện:** Ticket chưa Đóng, có owner và hạn theo dõi. Chỉ ghi wait\_reason Chờ khách/Chờ đơn vị bảo hành khi trạng thái chính là Đang xử lý.

**Luồng chuẩn**

| Bước | Người dùng thực hiện | CRM đáp ứng |
| :---: | :---- | :---- |
| 1 | Mở danh sách công việc của mình hoặc ticket cần theo dõi. | Hiển thị trạng thái, owner, lần cập nhật gần nhất, hạn và việc đến hạn/quá hạn theo quyền. |
| 2 | Xem lịch sử, xác định thông tin cần chờ và liên hệ khách/đối tác bên ngoài CRM. | Cung cấp lịch sử được phép xem, tránh yêu cầu khách kể lại thông tin đã ghi. |
| 3 | Với ticket Đang xử lý, ghi lý do chờ, nội dung cần phản hồi, lần liên hệ/gửi và hạn tiếp theo. | Kiểm tra dữ liệu; lưu wait\_reason và công việc nhắc đang mở. Nếu chờ đối tác, lưu đơn vị/mã tham chiếu nếu có. |
| 4 | Theo dõi danh sách và thực hiện liên hệ vào thời điểm cần thiết. | Đánh dấu việc đến/quá hạn và nhắc nội bộ cho owner; quản lý thấy các việc thuộc phạm vi. Trạng thái ticket không tự đổi. |
| 5 | Khi nhận phản hồi, ghi nội dung, thời gian, kênh và căn cứ đi kèm. | Lưu tương tác và người ghi, nối vào đúng ticket; không coi đây là dữ liệu API tự đồng bộ. |
| 6 | Đánh giá thông tin đã đủ để tiếp tục và ghi việc tiếp theo. | Nếu đủ: kết thúc lý do chờ, giữ lịch sử, hoàn thành việc chờ có kết quả; trạng thái vẫn Đang xử lý để tiếp tục UC-02. |

**Luồng thay thế**

**UC-03-A1 · Chờ khách hoặc chờ đối tác, EX-05.** Chờ khách phải nêu khách cần bổ sung gì; chờ đối tác phải nêu đơn vị/nội dung đã gửi. Cả hai đều có lần liên hệ gần nhất, owner và hạn theo dõi; không tạo trạng thái chính mới.

**UC-03-A2 · Đến hạn nhưng chưa có phản hồi, EX-06.** Owner liên hệ lại, ghi kết quả chưa có phản hồi và đề nghị quản lý hỗ trợ nếu cần. Chỉ thay hạn khi có lý do; CRM giữ hạn cũ trong lịch sử và không tự kéo dài thời hạn.

**UC-03-A3 · Phản hồi còn thiếu hoặc cần đổi bên đang chờ.** Nếu vẫn chờ cùng bên, giữ lý do chờ và ghi nội dung/lịch mới. Nếu đổi bên, kết thúc khoảng chờ cũ, ghi bước xử lý rồi tạo lý do chờ mới; không đặt đồng thời hai wait\_reason.

**UC-03-A4 · Theo dõi khi đang xác minh hoặc đã giải quyết.** Owner vẫn xem/cập nhật việc theo dõi ở trạng thái hiện tại. Với Đã giải quyết, công việc thường là thông báo/bàn giao trong UC-04; không gán trạng thái con chờ của Đang xử lý vào Đã giải quyết.

**Ngoại lệ**

**UC-03-E1 · Thiếu nội dung chờ hoặc hạn không hợp lệ.** CRM từ chối lưu lý do chờ mới, chỉ rõ trường cần sửa; owner bổ sung, không để việc nhắc thiếu người/hạn.

**UC-03-E2 · Owner không còn hoạt động.** Quản lý tiếp nhận trách nhiệm tạm thời và phân công lại người có quyền; CRM ghi bàn giao, chuyển việc mở và giữ lịch sử. Không tự đóng ticket vì người xử lý nghỉ/vô hiệu tài khoản.

**UC-03-E3 · Không có quyền hoặc lưu phản hồi lỗi.** CRM không trả thông tin ngoài phạm vi và không xác nhận cập nhật thành công. Người dùng nhờ người có quyền hoặc kiểm tra lại dữ liệu trước khi gửi lại; không tạo phản hồi trùng.

**Hậu điều kiện**

Ticket có tiến độ và lịch sử liên hệ cập nhật. Nếu còn chờ, có lý do, owner, hạn và việc nhắc đang mở; nếu hết chờ, lý do hiện tại được xóa, lịch sử chờ được giữ và công việc tiếp tục ở UC-02. Đến hạn không đồng nghĩa đã có kết quả xử lý.

**UC 04 Thông báo và hoàn tất yêu cầu**

**Tác nhân:** Owner hoặc nhân viên được phân công và có quyền; quản lý cửa hàng hỗ trợ trường hợp chưa thể hoàn tất.

**Mục tiêu:** Bảo đảm khách được thông báo đúng kết quả và hoàn tất việc bàn giao trước khi đóng ticket.

**Kích hoạt:** Ticket đã có kết quả thực tế và được chuyển Đã giải quyết.

**Tiền điều kiện:** Ticket ở Đã giải quyết, có kết quả/căn cứ/người xác nhận. Nhân viên có quyền ghi thông báo, bàn giao và đề nghị đóng trong phạm vi được giao.

**Luồng chuẩn**

| Bước | Người dùng thực hiện | CRM đáp ứng |
| :---: | :---- | :---- |
| 1 | Mở ticket, kiểm tra kết quả, căn cứ, thiết bị đã nhận và công việc còn lại. | Hiển thị thông tin phục vụ kết thúc yêu cầu; chỉ rõ việc thông báo/bàn giao còn thiếu. |
| 2 | Liên hệ khách qua kênh bên ngoài CRM, giải thích kết quả và bước tiếp theo. | Cho ghi kênh, thời điểm, nội dung thông báo và người thực hiện. |
| 3 | Ghi nhận đã liên hệ thành công và phản hồi/xác nhận của khách. | Lưu đúng kết quả liên hệ; không tự đánh dấu khách đã nhận/đồng ý chỉ vì nhân viên bấm gửi. |
| 4 | Nếu đã nhận thiết bị, hoàn tất bàn giao hoặc thống nhất phương án bàn giao được khách xác nhận. | Lưu thời điểm, người thực hiện và bằng chứng/xác nhận phù hợp; không tạo phiếu xuất kho. |
| 5 | Rà soát các việc mở, ghi kết quả hoàn thành hoặc lý do hủy việc không còn cần thiết. | Giữ dấu vết các việc; không cho dùng thao tác hủy để bỏ qua thông báo hoặc bàn giao bắt buộc. |
| 6 | Yêu cầu đóng ticket sau khi đủ điều kiện. | Kiểm tra quyền và các điều kiện BR-06; chuyển Đóng, lưu người/thời điểm/log, giữ lịch sử và kết quả các lần xử lý. |

**Luồng thay thế**

**UC-04-A1 · Khách chưa đồng ý kết quả, EX-07.** Tại bước 3, nhân viên ghi phản hồi và lý do; người có quyền chuyển Đã giải quyết về Đang xử lý, xác nhận owner và hạn mới. Giữ kết quả cũ trong lịch sử, tiếp tục UC-02; chưa dùng UC-05 vì ticket chưa đóng.

**UC-04-A2 · Chưa liên lạc được khách.** Ghi từng lần liên hệ chưa thành công, giữ Đã giải quyết và tạo/duy trì lịch liên hệ tiếp. Không tự đóng sau một số ngày hoặc tự ghi khách đã xác nhận.

**UC-04-A3 · Không nhận thiết bị.** Bỏ bước 4, ghi bàn giao không áp dụng; vẫn hoàn tất thông báo, phản hồi và công việc liên quan trước bước 6\.

**UC-04-A4 · Khách chọn phương án bàn giao khác.** Nhân viên ghi phương án đã được thống nhất và bằng chứng khách xác nhận; quản lý kiểm tra thẩm quyền khi cần. Chi tiết vận chuyển thực tế ở ngoài CRM. Chỉ đóng khi điều kiện của phương án được xác nhận đầy đủ.

**Ngoại lệ**

**UC-04-E1 · Thiếu căn cứ kết quả hoặc bằng chứng bàn giao.** CRM chặn đóng và chỉ rõ điều kiện thiếu. Nhân viên bổ sung hoặc nhờ quản lý xử lý; ticket giữ nguyên trạng thái, không ghi log Đóng thành công.

**UC-04-E2 · Tranh chấp tình trạng lúc trả thiết bị.** Nhân viên ghi phản ánh, đối chiếu tình trạng tiếp nhận và chuyển quản lý xử lý. Nếu kết quả chưa đạt, thực hiện UC-04-A1; không xác nhận bàn giao thay khách.

**UC-04-E3 · Ticket vừa bị đổi trạng thái hoặc người dùng mất quyền.** CRM từ chối yêu cầu đóng dựa trên dữ liệu cũ; nhân viên tải lại và kiểm tra với owner hiện tại. Không ghi đè thay đổi đã được lưu.

**Hậu điều kiện**

Thành công: ticket Đóng, có kết quả, ghi nhận thông báo/phản hồi và bằng chứng bàn giao hoặc phương án được xác nhận nếu áp dụng; không còn việc mở chưa xử lý. Chưa đủ điều kiện: giữ Đã giải quyết với lịch tiếp theo, hoặc trở về Đang xử lý khi khách chưa đồng ý.

**UC 05 Mở lại ticket đã đóng**

**Tác nhân:** Quản lý cửa hàng hoặc CSKH cấp chuỗi có quyền; nhân viên tiếp nhận ghi yêu cầu mở lại và cung cấp thông tin.

**Mục tiêu:** Tiếp tục xác minh vấn đề cũ trên cùng ticket, giữ lịch sử và khôi phục trách nhiệm theo dõi.

**Kích hoạt:** Khách báo vấn đề phát sinh lại sau khi ticket đã Đóng, tương ứng EX-08.

**Tiền điều kiện:** Ticket đang Đóng và người quyết định mở lại có quyền trên cửa hàng xử lý hiện tại. Có thông tin phản ánh mới để xác định liên quan đến yêu cầu trước.

**Luồng chuẩn**

| Bước | Người dùng thực hiện | CRM đáp ứng |
| :---: | :---- | :---- |
| 1 | Tra cứu ticket cũ, xác nhận khách/thiết bị và ghi phản ánh phát sinh lại. | Hiển thị lịch sử được phép xem, kết quả cũ và thời điểm đóng để người dùng đối chiếu. |
| 2 | Quản lý hoặc CSKH chuỗi đánh giá phản ánh có thuộc yêu cầu cũ và quyết định mở lại. | Chỉ cho người có quyền thực hiện; lưu căn cứ và lý do, không tự mở lại theo từ khóa phản ánh. |
| 3 | Chọn owner hoạt động, xác nhận nhận việc và đặt hạn theo dõi mới. | Kiểm tra người nhận/phạm vi, dữ liệu bắt buộc và trạng thái hiện tại của ticket. |
| 4 | Xác nhận mở lại. | Giữ Ticket ID; chuyển Đóng về Đang xác minh, không có wait\_reason; lưu người/thời điểm/lý do mở lại và tạo việc theo dõi mới. |
| 5 | Owner tiếp nhận và bắt đầu xác minh lại theo UC-02. | Cho tiếp tục xử lý trên lịch sử đã có; kết quả lần trước vẫn được lưu, kết quả hiện tại cần được xác minh lại. |

**Luồng thay thế**

**UC-05-A1 · Phản ánh là yêu cầu mới độc lập.** Người có thẩm quyền ghi kết luận phân loại, không mở lại ticket cũ; nhân viên thực hiện UC-01 cho yêu cầu mới và ghi mã tham chiếu ticket trước nếu có liên quan.

**UC-05-A2 · Khách liên hệ ở cửa hàng khác.** Nhân viên nhờ CSKH chuỗi xác minh và truy cập theo quyền. CSKH chuỗi mở lại với owner hợp lệ ở cửa hàng xử lý hiện tại; nếu cần đổi cửa hàng, thực hiện tiếp UC-06, không tạo ticket trùng để né quyền.

**UC-05-A3 · Khách phản hồi khi ticket chưa Đóng.** Không chạy thao tác mở lại. Nếu đang Đã giải quyết, dùng UC-04-A1; nếu đang xác minh/xử lý thì owner cập nhật qua UC-02 hoặc UC-03.

**Ngoại lệ**

**UC-05-E1 · Nhân viên tiếp nhận tự mở lại ngoài thẩm quyền.** CRM từ chối; nhân viên chuyển yêu cầu và căn cứ cho quản lý/CSKH chuỗi có quyền. Ticket giữ Đóng cho đến khi quyết định được ghi hợp lệ.

**UC-05-E2 · Thiếu lý do, hạn hoặc owner hợp lệ.** CRM chỉ rõ dữ liệu cần bổ sung và không mở lại một phần. Người có quyền chọn người nhận khác hoặc hoàn thiện thông tin.

**UC-05-E3 · Ticket đã được người khác mở lại.** CRM thông báo trạng thái mới và yêu cầu tải lại; không tạo thêm lần mở lại hoặc công việc theo dõi trùng. Người dùng bổ sung phản ánh vào ticket đang mở.

**Hậu điều kiện**

Thành công: cùng Ticket ID ở Đang xác minh, có owner và hạn mới; toàn bộ kết quả, thông báo, bàn giao và lần đóng trước vẫn còn. Nếu không mở lại: ticket giữ trạng thái hiện tại; yêu cầu mới độc lập được tiếp nhận bằng UC-01 khi đã được phân loại.

**5 Đối chiếu các tình huống bắt buộc**

Mã EX giữ nguyên theo baseline để các thành viên đối chiếu khi ghép bài. Bảng này xác định nơi tình huống được xử lý trong Practice 6; việc phân loại event và lập Event-Response Table thuộc phần Practice 7\.

| Mã | Tình huống | Luồng xử lý trong bài | Trách nhiệm và kết quả |
| :---: | :---- | :---- | :---- |
| EX-01 | Thiếu dữ liệu mua hoặc chứng từ. | UC-01-A2; UC-02-A2 | Nhân viên vẫn tiếp nhận; owner ghi phần thiếu và lịch bổ sung; chưa cam kết miễn phí. |
| EX-02 | Rơi vỡ/ngấm nước hoặc tranh chấp ngoại quan. | UC-01-E2; UC-02-E1 | Ghi hiện trạng/bằng chứng; quản lý hoặc chuyên môn xác minh; CRM không tự kết luận. |
| EX-03 | Lỗi không tái hiện tại quầy. | UC-02-E2 | Owner ghi mô tả và chuyển kiểm tra/theo dõi; ticket mở, có hạn tiếp theo. |
| EX-04 | Cần chuyển sang cửa hàng khác. | UC-06 qua UC-02-A4 | CSKH chuỗi bàn giao cho người đủ quyền; giữ Ticket ID, lịch sử và trạng thái, cập nhật phạm vi. |
| EX-05 | Chờ khách hoặc đơn vị bảo hành. | UC-03, bước 3-6; UC-03-A1 | Owner ghi wait\_reason, lần liên hệ/gửi, nội dung chờ, hạn và việc nhắc đang mở. |
| EX-06 | Đến hạn nhưng chưa có phản hồi. | UC-03, bước 4; UC-03-A2 | CRM nhắc nội bộ; owner liên hệ lại; quản lý thấy quá hạn; không tự đổi kết quả hay đóng ticket. |
| EX-07 | Khách chưa đồng ý kết quả. | UC-04-A1 | Ghi phản hồi; chuyển Đã giải quyết về Đang xử lý, có owner và hạn mới; giữ kết quả cũ. |
| EX-08 | Vấn đề phát sinh lại sau đóng. | UC-05 | Quản lý/CSKH chuỗi mở về Đang xác minh theo quyền; giữ lịch sử và giao việc mới. |
| EX-09 | Đủ điều kiện đổi nhưng chưa thực hiện được phương án. | UC-02-E3 | Owner báo quản lý/người có thẩm quyền; giữ Đang xử lý và hạn; CRM không tự hoàn tiền/đổi model/voucher. |

