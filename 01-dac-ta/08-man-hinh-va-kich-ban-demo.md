# Danh sách màn hình dự kiến & Kịch bản Demo bảo vệ

Nguồn: `00-ke-hoach/THONG-TIN-NGHIEP-VU.md`.

## 1. Danh sách màn hình (14 màn hình / khung giao diện)

| # | Màn hình | Vai trò chính | Thiết bị ưu tiên & Ghi chú |
|---|---|---|---|
| 1 | Đăng nhập | Tất cả | Desktop & Mobile Web |
| 2 | Dashboard điều hành tổng quan | Giám đốc, Kế hoạch, Admin | Desktop — KPI tiến độ, đơn trễ, năng suất nhanh |
| 3 | Danh mục Mã hàng (SKU/Style) | Admin, Kế hoạch | Quản lý mã hàng dùng chung, NS định mức, đơn giá khoán |
| 4 | Quản lý Đơn hàng & Chi tiết đơn | Kế hoạch, Admin (GD/TT xem) | Tạo đơn từ danh mục mã hàng; hỗ trợ điều chỉnh biến động |
| 5 | Danh mục Chuyền & Ca làm việc | Admin, Kế hoạch | Gán tổ trưởng phụ trách, khung giờ ca chuẩn |
| 6 | Kế hoạch sản xuất theo chuyền (ngày + ca) | Kế hoạch (GD/TT xem) | Chia 1 mã hàng ra nhiều chuyền |
| 7 | Nhập & Chốt sản lượng giờ (cấp Chuyền) | Tổ trưởng | **Mobile Web Responsive** — chọn bậc thang mã→đơn, nhập SL đạt chuẩn + SL lỗi tổng + số CN, nút Chốt |
| 8 | Phân bổ & Chốt sản lượng công nhân cuối ca | Tổ trưởng (Admin mở khóa) | **Mobile Web Responsive** — đối soát tổng SL công nhân khớp SL đạt chuẩn ca; Admin có modal Mở khóa kèm lý do |
| 9 | Trung tâm Cảnh báo sản xuất | Giám đốc, Kế hoạch, Tổ trưởng | Trễ hạn giao, hụt KH ngày, chậm nhập SL giờ; cập nhật xử lý |
| 10 | Báo cáo Năng suất | Giám đốc, Kế hoạch | Biểu đồ & bảng trục chuyền×ngày → tuần; lọc đơn/mã hàng/khách |
| 11 | Dashboard TV Xưởng (Kiosk 16:9) | Xưởng sản xuất, Giám đốc | **Full HD `1920×1080`**, chỉ đọc, auto-refresh `30–60s`, mở qua Kiosk Token |
| 12 | Kỳ lương khoán & Bảng lương chi tiết | Giám đốc, Admin | Tính tự động theo kỳ, rà soát và Chốt kỳ lương |
| 13 | Trang Cá nhân Công nhân | Công nhân | **Mobile Web Responsive**, chỉ đọc — xem SL đạt chuẩn theo ca & phiếu lương kỳ của bản thân |
| 14 | Quản trị Người dùng, Vai trò & Chat Logs | Admin | Quản lý tài khoản/công nhân, cấp Kiosk Token, tra cứu log AI & audit |
| *(Widget)* | Khung trợ lý Chatbot AI | Tất cả đã đăng nhập | Khung chat nổi hoặc trang riêng; hỗ trợ thẻ xác nhận khi gọi `acknowledge_alert` |

## 2. Kịch bản Demo bảo vệ E2E (khớp MVP + Mức Tốt)

**Điều kiện chuẩn bị:** Dịch vụ đã chạy ổn định trên URL công khai **Render**; dữ liệu seed/UAT ẩn danh đã nạp sẵn; `COMMAND_CODE_API_KEY` và `TV_KIOSK_TOKEN` đã cấu hình trên env.

1. **Thiết lập & Danh mục (Admin / Kế hoạch):** Đăng nhập xem danh sách tài khoản đủ 5 vai, **Danh mục Mã hàng** (có định mức & đơn giá khoán) và danh mục Chuyền/Ca.
2. **Đơn hàng & Biến động (Kế hoạch):** Tạo đơn hàng mới từ Danh mục Mã hàng → chia kế hoạch 1 mã hàng chạy trên 2 chuyền theo ngày + ca; thử điều chỉnh tăng số lượng / dời hạn giao trên một đơn `Đang sản xuất` kèm lý do biến động.
3. **Sản lượng giờ & Phân bổ ca (Tổ trưởng trên khung Mobile Web):**
   - Nhập sản lượng khung giờ (chọn Mã hàng → Đơn, nhập `SL đạt chuẩn`, `SL lỗi tổng`, `Số công nhân`) → bấm **Chốt** → thử sửa lại (hệ thống chặn `409`).
   - Cuối ca phân bổ SL đạt chuẩn cho từng công nhân trong chuyền (hệ thống kiểm tra khớp tổng) → bấm **Chốt**. *(Minh họa nhanh quyền Admin mở khóa kèm lý do khi chưa chốt lương).*
4. **Cảnh báo & Dashboard TV Kiosk:** Mở trung tâm cảnh báo xem đủ 3 loại (*đơn quá hạn*, *hụt KH ngày*, *chậm nhập SL giờ*) → mở tab **Dashboard TV (`1920×1080` bằng Kiosk Token)** thấy bảng tiến độ chuyền, cảnh báo nổi bật và SL giờ mới nhất tự động cập nhật.
5. **Báo cáo năng suất & Lương khoán (Giám đốc + Công nhân):**
   - Giám đốc xem báo cáo năng suất chuyền theo ngày/tuần (% hiệu suất định mức) → vào Kỳ lương khoán kiểm tra và bấm **Chốt kỳ lương**.
   - Đăng nhập tài khoản **Công nhân** mở trang Cá nhân chỉ đọc để đối chiếu đúng sản lượng ca và phiếu lương của mình.
6. **Chatbot AI & Tool Calling:**
   - Hỏi tiến độ đơn hàng & hỏi giải thích công thức năng suất/quy tắc tính trễ (Bot trả lời đúng dữ liệu + SOP).
   - Với vai Giám đốc/Kế hoạch: yêu cầu Bot liệt kê cảnh báo mở và đánh dấu 1 cảnh báo sang `Đang xử lý` → Bot hiện yêu cầu xác nhận → bấm xác nhận → trạng thái cảnh báo cập nhật thật trên hệ thống.
   - Thử câu hỏi vượt quyền (Công nhân hỏi lương người khác) hoặc yêu cầu sửa sản lượng qua chat → Bot từ chối đúng Lớp 4.

## 3. Yêu cầu Video Demo nộp kèm

- Thời lượng **5 – 7 phút**, quay trực tiếp trên đường dẫn Render công khai, trình diễn trọn vẹn luồng MVP, Chatbot AI và cả 3 hạng mục mức Tốt như kịch bản trên.
