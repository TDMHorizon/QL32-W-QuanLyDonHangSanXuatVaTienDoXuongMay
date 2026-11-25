# Yêu cầu phi chức năng (NFR)

Các yêu cầu phi chức năng dưới đây được định lượng rõ để làm căn cứ kiểm thử chất lượng (Bài 5 MAN104) và nghiệm thu UAT:

| Mã | Nhóm | Yêu cầu & Tiêu chí đo kiểm (Acceptance Criteria) |
|---|---|---|
| NFR-01 | Bảo mật & Xác thực | Mật khẩu băm một chiều (`bcrypt`/`argon2`); toàn bộ API nghiệp vụ yêu cầu xác thực hợp lệ; khóa `COMMAND_CODE_API_KEY` và chuỗi kết nối DB chỉ cấu hình qua biến môi trường (`env`), tuyệt đối không lưu trong mã nguồn GitHub. |
| NFR-02 | Phân quyền & Kiosk Auth | Kiểm tra RBAC tại tầng Backend cho mọi API: Tổ trưởng chỉ ghi nhận trên chuyền được gán; Công nhân chỉ đọc được SL & lương của chính `user_id` mình; màn hình TV xưởng xác thực bằng **Kiosk Token chỉ đọc** riêng biệt (không được truy cập API lương hay quản trị). |
| NFR-03 | Quyền riêng tư & Ẩn danh (PII) | Tuân thủ Mục 2 đề bài MAN104: mọi dữ liệu khảo sát và UAT từ xưởng thực tế khi nạp lên môi trường demo công khai Render phải **ẩn danh thông tin cá nhân/nhạy cảm** (mã hóa tên thật công nhân, SĐT, thông tin thương mại khách hàng). |
| NFR-04 | Hiệu năng phản hồi | Sau khi dịch vụ trên Render đã khởi động (warm state): các API CRUD và tra cứu tiến độ/TV phản hồi **&lt; 2 giây** ở tải sử dụng xưởng nhỏ (10–20 người dùng đồng thời); API Chatbot gọi LLM phản hồi **&lt; 15 giây**, có cơ chế timeout và trả thông báo dự phòng thân thiện khi LLM quá tải/hết quota. |
| NFR-05 | Khả dụng & Đa thiết bị | Giao diện **100% tiếng Việt**; màn hình Kế hoạch/Báo cáo tối ưu cho Desktop (`≥ 1280px`); màn hình Tổ trưởng nhập SL và trang Cá nhân Công nhân đạt chuẩn **Web Responsive trên trình duyệt điện thoại (`≥ 375px`)** với nút bấm rõ, thao tác chọn bậc thang tối đa 3 chạm. |
| NFR-06 | Hiển thị Dashboard TV | Bố cục Kiosk chuẩn màn hình ngang **16:9 Full HD (`1920×1080`)**, chữ lớn đọc rõ từ khoảng cách 3–5m tại chuyền, hiển thị trọn trong 1 khung nhìn (không cần cuộn trang), tự động làm mới định kỳ **30–60 giây** mà không rò rỉ bộ nhớ trình duyệt. |
| NFR-07 | Toàn vẹn dữ liệu & Audit | Bản ghi sản lượng đã `Chốt` trả mã lỗi `409 Conflict` nếu Tổ trưởng gọi API sửa/xóa; mọi thao tác Admin mở khóa sản lượng hoặc Kế hoạch sửa/hủy đơn `Đang sản xuất` phải bắt buộc có lý do và lưu vết thời gian + người thực hiện; dữ liệu thuộc kỳ lương `Đã chốt` bị khóa bất biến. |
| NFR-08 | Chuẩn API & CSDL | Backend cung cấp RESTful API kèm tài liệu tương tác **Swagger / OpenAPI** đầy đủ; lược đồ CSDL quan hệ chuẩn xác với **ERD ≥ 8 bảng** (thiết kế dự kiến ~14 bảng). |
| NFR-09 | An toàn & Kiểm thử AI | Chatbot chỉ trả lời dựa trên CSDL + tài liệu SOP nội bộ; write-tool duy nhất (`acknowledge_alert`) bắt buộc có bước xác nhận từ người dùng trước khi ghi DB; lưu 100% lượt hỏi đáp vào `chat_logs`; bộ kiểm thử chất lượng Chatbot đạt **≥ 30 kịch bản câu hỏi** (trong phạm vi, ngoài phạm vi, vượt quyền, đơn trễ, xác nhận tool). |
| NFR-10 | Triển khai & Khởi tạo dữ liệu | Hệ thống triển khai chạy thật qua URL HTTPS công khai trên **Render**; có script khởi tạo dữ liệu tự động (seed mô phỏng cho Sprint 1–2 và bộ dữ liệu mẫu UAT ẩn danh cho tuần 6–7) kèm tài liệu hướng dẫn cài đặt & vận hành. |
