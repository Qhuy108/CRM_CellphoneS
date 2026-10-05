Kế hoạch phân tích nghiệp vụ và thiết kế hệ thống trên Frappe Framework. Bản này bổ sung các đầu ra theo 5 thực hành Chương 2 và giữ các phần yêu cầu, quy trình, dữ liệu phục vụ xây dựng hệ thống.
TV4 • MSSV 24520706 • Phụ trách dữ liệu và thiết kế triển khai
Mã
Đầu ra và nội dung tối thiểu
D01
ERD logic
 Xác định thực thể, định danh, khóa tham chiếu, quan hệ và bội số. Thể hiện chi nhánh, khách hàng, cơ hội tư vấn, tương tác, phiếu hỗ trợ và dữ liệu mua hàng theo yêu cầu; xử lý hồ sơ trùng theo quy tắc đã chốt.
D02
Từ điển dữ liệu
 Mỗi trường có tên, ý nghĩa, kiểu, bắt buộc/duy nhất, giá trị hợp lệ, nguồn và quy tắc kiểm tra. Đồng bộ trạng thái với C03; quy định dữ liệu mẫu và cột CSV cần nhập.
D03
Wireframe
 Phác thảo 4-6 màn hình chính theo Use Case: hồ sơ khách, cơ hội, tương tác/nhắc việc, phiếu hỗ trợ và báo cáo. Chỉ rõ thông tin nhập/xem, hành động và thông báo lỗi quan trọng.
D04
Ánh xạ sang Frappe
 Lập bảng thực thể → DocType dự kiến → quan hệ → workflow → quyền. Chốt phiên bản và ứng dụng sử dụng; phân biệt tự xây với thành phần sẵn có. Phối hợp A03 về hệ sinh thái, dữ liệu ngoài và phương án mô phỏng.
D05
Sequence Diagram
 Vẽ 1-2 luồng cần làm rõ xử lý, ví dụ tạo/phân công phiếu và đóng phiếu có kiểm tra điều kiện. Đồng bộ với Use Case, dữ liệu và quyền; ghi là thiết kế đề xuất.

Có thể chuẩn bị công cụ và khung dữ liệu từ M1; dựng ERD nháp sau M2. Phối hợp liên tục với TV3 tại M3, chốt tại M4. Mỗi dữ liệu trên giao diện phải có nơi lưu/nguồn đọc; mỗi thực thể phải phục vụ yêu cầu cụ thể.
