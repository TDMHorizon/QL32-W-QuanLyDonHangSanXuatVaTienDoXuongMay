# Mô tả hệ thống

Nguồn: `00-ke-hoach/THONG-TIN-NGHIEP-VU.md`.

## 1. Tổng quan hệ thống

Hệ thống là ứng dụng Web quản lý xuyên suốt từ khâu nhận đơn đến theo dõi tiến độ chuyền may, cảnh báo sớm, tính năng suất, đối soát lương khoán và trợ lý hỏi đáp AI:

- **Admin:** Quản trị người dùng/vai trò, danh mục gốc, cấp Kiosk Token cho TV xưởng, mở khóa bản ghi sản lượng kèm lý do (khi chưa chốt lương) và đồng duyệt kỳ lương.
- **Phòng Kế hoạch:** Quản lý Danh mục Mã hàng dùng chung, quản lý Đơn hàng (bao gồm điều chỉnh khi đơn biến động), chia Kế hoạch sản xuất về từng chuyền theo ngày + ca.
- **Tổ trưởng:** Sử dụng trình duyệt máy tính hoặc điện thoại tại chuyền để nhập & **chốt** sản lượng theo giờ (cấp chuyền), phân bổ & **chốt** sản lượng đạt chuẩn cho từng công nhân vào cuối ca, theo dõi và phản hồi cảnh báo của chuyền mình.
- **Giám đốc:** Giám sát toàn xưởng qua Dashboard tổng quan, **Dashboard TV Kiosk**, cảnh báo trễ, báo cáo năng suất ngày/tuần và chốt kỳ lương khoán.
- **Công nhân:** Đăng nhập vào trang **Cá nhân** chỉ đọc để tự kiểm tra sản lượng đạt chuẩn theo ca, phiếu lương khoán kỳ và hỏi Chatbot về số liệu của bản thân.
- **Chatbot AI:** Hỗ trợ mọi vai trò đăng nhập hỏi đáp tiến độ & quy chế nghiệp vụ (SOP); thực thi tool calling đọc dữ liệu theo phân quyền và hỗ trợ Giám đốc/Kế hoạch/Admin cập nhật trạng thái xử lý cảnh báo (có bước xác nhận).

## 2. Các thành phần logic

| Thành phần | Vai trò chính |
|---|---|
| **Frontend (Web App)** | Giao diện phân tách theo vai trò: Desktop UI cho Kế hoạch/Giám đốc/Admin; **Responsive Mobile Web** cho Tổ trưởng & Công nhân; màn hình **TV Kiosk 16:9** chỉ đọc; khung Chat AI |
| **Backend REST API** | Xác thực, RBAC, nghiệp vụ đơn–kế hoạch–sản lượng–lương, bộ quy tắc cảnh báo, tổng hợp báo cáo năng suất, orchestrator gọi LLM & thực thi tools, tài liệu OpenAPI/Swagger |
| **Cơ sở dữ liệu** | Quan hệ (ERD ≥ 8 bảng, dự kiến ~14 bảng): người dùng, vai trò, công nhân, chuyền, mã hàng dùng chung, đơn hàng, chi tiết đơn, kế hoạch ca, SL giờ chuyền, SL ca công nhân, cảnh báo, đơn giá/kỳ lương, chat log & audit log |
| **Tri thức SOP nội bộ** | Tài liệu markdown/JSON chuẩn hóa quy tắc tính trễ, công thức năng suất, quy chế lương khoán, quy trình chốt ca được BE nạp làm ngữ cảnh chuẩn cho Chatbot |
| **Dịch vụ LLM ngoài** | **CommandCode Provider API** (`POST /chat/completions`) nhận prompt + schema tools từ BE qua HTTPS |
| **Hạ tầng triển khai** | **Render** cung cấp URL Web công khai phục vụ Sprint Review, UAT tại xưởng và bảo vệ đồ án |

*(Stack ngôn ngữ/framework chi tiết FE/BE/DB được chốt tại quyết định kỹ thuật Q10 trước Sprint 1).*

## 3. Luồng nghiệp vụ khép kín (End-to-End)

1. **Thiết lập nền tảng:** Admin khởi tạo tài khoản (Admin, Kế hoạch, Tổ trưởng, Giám đốc, Công nhân), danh mục Chuyền, khung Ca làm việc và Kiosk Token TV; Admin/Kế hoạch cập nhật **Danh mục Mã hàng** (kèm năng suất định mức & đơn giá khoán).
2. **Nhận đơn & lập kế hoạch:** Kế hoạch tạo Đơn hàng từ Danh mục Mã hàng → chia kế hoạch sản xuất cho một hoặc nhiều chuyền theo **ngày + ca** (nếu khách thay đổi số lượng/hạn giao khi đang may, hệ thống kiểm soát điều kiện biến động hợp lệ).
3. **Cập nhật sản lượng theo giờ (cấp Chuyền):** Trong ca, Tổ trưởng chọn giờ BĐ–KT, chọn bậc thang **Mã hàng → Đơn**, nhập `SL đạt chuẩn`, `SL lỗi tổng` và `Số công nhân đứng chuyền` → bấm **Chốt** để cập nhật tiến độ tức thời.
4. **Phân bổ sản lượng cuối ca (cấp Công nhân):** Cuối ca, Tổ trưởng phân bổ SL đạt chuẩn cho từng công nhân thuộc chuyền trong ca đó (hệ thống kiểm tra tổng khớp SL đạt chuẩn ca) → bấm **Chốt**.
5. **Giám sát tiến độ & sinh cảnh báo:** Hệ thống tự động đối chiếu hạn giao, kế hoạch ngày và nhịp nhập sản lượng theo giờ để phát cảnh báo lên Web, **Dashboard TV xưởng** và Chatbot.
6. **Phân tích năng suất:** Giám đốc và Kế hoạch xem báo cáo năng suất theo trục **chuyền × ngày → tuần** và theo đơn/mã hàng/khách dựa trên tổng giờ công thực tế.
7. **Tính & chốt lương khoán:** Cuối kỳ, hệ thống tổng hợp lương khoán từng công nhân từ SL đạt chuẩn đã chốt; Giám đốc/Admin rà soát và **Chốt kỳ lương** (khóa vĩnh viễn dữ liệu SL thuộc kỳ); Công nhân xem phiếu lương trên trang Cá nhân.
8. **Tương tác Chatbot AI:** Người dùng đặt câu hỏi tự nhiên → BE kiểm tra quyền, nạp SOP + gọi CommandCode LLM → LLM gọi read-tools lấy số liệu chính xác (hoặc đề xuất `acknowledge_alert` chờ người dùng bấm xác nhận) → ghi `chat_logs`.

## 4. Nguyên tắc thiết kế cốt lõi

- **Phân quyền tối thiểu (Least Privilege) & Đa vai trò:** Một tài khoản có thể giữ nhiều vai, nhưng Tổ trưởng chỉ thao tác trên chuyền phụ trách, Công nhân chỉ đọc dữ liệu của chính mình, và Dashboard TV dùng Kiosk Token chỉ đọc tách rời tài khoản Giám đốc.
- **Toàn vẹn số liệu sản xuất & lương:** Bản ghi sản lượng sau khi Chốt bị khóa với Tổ trưởng; mọi thao tác mở khóa của Admin hoặc điều chỉnh đơn đang sản xuất của Kế hoạch đều phải ghi lý do vào lịch sử kiểm toán (audit), và bị cấm hoàn toàn một khi kỳ lương đã chốt.
- **AI có kiểm soát (Human-in-the-loop):** Chatbot tuyệt đối không tự bịa số liệu ngoài CSDL/SOP, không có công cụ sửa đơn/sản lượng/lương; thao tác ghi duy nhất (cập nhật trạng thái cảnh báo) bắt buộc có xác nhận từ người dùng có thẩm quyền.
- **Bảo vệ dữ liệu thực tế:** Mọi dữ liệu khảo sát và UAT từ đơn vị xưởng may khi đưa lên môi trường công khai Render đều phải ẩn danh thông tin định danh cá nhân (PII).
