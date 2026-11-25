# Phạm vi sản phẩm (MVP + Mức Tốt)

Nguồn: `00-ke-hoach/THONG-TIN-NGHIEP-VU.md` & Đề tài `QL32-W` (`BM01a - De bai do an QLDA CNTT (MAN104) v1.pdf`).

## 1. Trong phạm vi — Phần bắt buộc (MVP)

1. **Xác thực & phân quyền RBAC:** 4 vai nghiệp vụ/quản trị (`Admin`, `Kế hoạch`, `Tổ trưởng`, `Giám đốc`) + cổng **Cá nhân chỉ đọc** cho `Công nhân` (đáp ứng và vượt yêu cầu `≥ 3 vai trò`).
2. **Danh mục Mã hàng dùng chung & Quản lý Đơn hàng:** Quản lý SKU/Style (kèm NS định mức, đơn giá khoán chuẩn); CRUD đơn hàng & chi tiết đơn; kiểm soát biến động đơn hàng khi đang sản xuất (điều chỉnh hạn giao, tăng/giảm SL hợp lệ, hủy đơn vẫn bảo lưu SL tính lương).
3. **Kế hoạch sản xuất theo chuyền:** Quản lý danh mục chuyền & khung ca làm việc; phân bổ mã hàng vào chuyền theo **ngày + ca** (1 mã hàng chia được nhiều chuyền).
4. **Cập nhật sản lượng 2 tầng & Chốt khóa:**
   - *Theo giờ (cấp Chuyền):* Nhập trên Web/Mobile Web theo bậc thang **Mã hàng → Đơn** gồm `SL đạt chuẩn`, `SL lỗi tổng` và `Số công nhân đứng chuyền`.
   - *Cuối ca (cấp Công nhân):* Phân bổ & đối soát `SL đạt chuẩn` cho từng công nhân trong chuyền.
   - *Khóa dữ liệu:* Chốt xong khóa với Tổ trưởng; chỉ Admin mở khóa kèm lý do audit khi kỳ lương chưa chốt.
5. **Tiến độ & 3 nhóm cảnh báo sớm:** Theo dõi % hoàn thành thực tế; tự động cảnh báo khi **(a) quá hạn giao**, **(b) hụt kế hoạch ngày**, hoặc **(c) chậm nhập sản lượng giờ** trong ca chạy; quản lý trạng thái xử lý cảnh báo in-app.
6. **Báo cáo năng suất:** Tổng hợp theo trục **chuyền × ngày → tuần** và phân tích theo **đơn / mã hàng / khách**; tính NS chuyền, NS đơn và % hiệu suất định mức dựa trên giờ công thực tế (nguồn công thức nền: [VTI Solutions](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/)).
7. **Chatbot AI hỏi đáp (MVP):** Tích hợp **CommandCode Provider API**, trả lời tiến độ đơn/chuyền và giải đáp quy chế dựa trên dữ liệu CSDL **kèm tài liệu SOP nghiệp vụ nội bộ**; giới hạn phạm vi trả lời; ghi `chat_logs`.

## 2. Trong phạm vi — Phần nâng cao để đạt mức Tốt

8. **Dashboard trên TV tại xưởng:** Giao diện Kiosk 16:9 (`1920×1080`) chỉ đọc, auto-refresh `30–60s`, mở an toàn bằng **Kiosk Token chỉ đọc**; hiển thị tiến độ chuyền hôm nay, cảnh báo nóng và SL giờ gần nhất.
9. **Tính lương khoán theo kỳ:** Tự động tính `Σ(Đơn giá khoán × SL đạt chuẩn công nhân)` theo kỳ; Giám đốc/Admin rà soát và chốt kỳ lương (khóa cứng dữ liệu kỳ); Công nhân xem phiếu lương cá nhân.
10. **Chatbot thao tác nghiệp vụ qua API (Tool Calling):**
    - Bộ **Read-tools** truy vấn trực tiếp API nội bộ theo phân quyền từng vai (bao gồm tool cá nhân cho Công nhân).
    - **1 Write-tool an toàn có xác nhận** (`acknowledge_alert`): cho phép Giám đốc / Kế hoạch / Admin cập nhật trạng thái xử lý cảnh báo qua hội thoại sau khi bấm/xác nhận đồng ý.

## 3. Ngoài phạm vi kỳ này & Ranh giới phân biệt (Out-of-Scope Boundaries)

Để tránh trượt phạm vi (scope creep) hoặc chồng lấn sang đề tài khác, nhóm khoanh rõ các điểm cắt sau:

| Hạng mục ngoài phạm vi (Không làm) | Ranh giới giữ lại trong hệ thống (Có làm) |
|---|---|
| Quản lý kho, định mức nguyên phụ liệu (BOM), mua sắm vật tư | Chỉ quản lý danh mục Mã hàng thành phẩm và số lượng đặt/sản xuất |
| Hệ thống QC chuyên sâu: phân loại mã lỗi may, phiếu kiểm lô, biểu đồ Pareto / Control Chart *(thuộc đề QL32-D)* | Chỉ nhập **1 con số `SL lỗi tổng`** trên bản ghi sản lượng giờ để tách bạch `SL đạt chuẩn` |
| Kết nối phần cứng IoT, cảm biến đếm mũi kim / máy may tự động, bảo trì máy MTBF/MTTR *(thuộc đề QL32-M)* | Tổ trưởng nhập số liệu qua form Web / Mobile Web và chốt ca |
| Kế toán tổng hợp, chấm công vân tay, tính tăng ca hệ số phức tạp, thuế TNCN, BHXH | Chỉ tính **lương khoán sản phẩm thuần** `Σ(đơn giá × SL đạt chuẩn)` theo kỳ và chấm số lượng công nhân đứng chuyền theo giờ/ca |
| Ứng dụng di động cài đặt riêng (Mobile App Native APK/iOS) | Thiết kế **Web Responsive (`≥ 375px`)** chạy trực tiếp trên trình duyệt điện thoại cho Tổ trưởng & Công nhân |
| Gửi thông báo qua Email, SMS, Zalo OA hoặc Push Notification nền | Cảnh báo hiển thị trực tiếp **in-app**, trên **Dashboard TV xưởng** và qua **Chatbot** |
| Tự huấn luyện / fine-tune mô hình học máy hoặc cho AI tự ý sửa đơn hàng / sản lượng / bảng lương | Dùng LLM thương mại qua API (CommandCode); chỉ mở Read-tools và **1 Write-tool cập nhật cảnh báo có xác nhận** |
| Đấu nối đồng bộ trực tiếp vào phần mềm nội bộ / ERP sẵn có của doanh nghiệp | Khảo sát thực tế tuần 1; dùng seed mô phỏng khi phát triển và **nạp bộ dữ liệu mẫu thực tế đã ẩn danh PII lên Render để người dùng thật UAT ở tuần 6–7** |

## 4. Ràng buộc triển khai & Mua sắm (Make-or-Buy)

- **Triển khai công khai:** Ứng dụng Web chạy thật trên **Render** kèm tài liệu API Swagger, phục vụ demo từng Sprint Review, UAT tại đơn vị và buổi bảo vệ cuối kỳ (kèm video demo 5–7 phút).
- **Quyết định Làm hay Mua (Make-or-Buy):**
  - **Mua / Thuê dịch vụ sẵn có:** Dịch vụ LLM (**CommandCode Provider API**) và hạ tầng Cloud Hosting (**Render**).
  - **Tự phát triển:** Toàn bộ Frontend, Backend REST API, CSDL, bộ quy tắc tiến độ–cảnh báo–năng suất–lương khoán, tài liệu SOP nội bộ và tầng điều phối Tool Calling cho Chatbot.
