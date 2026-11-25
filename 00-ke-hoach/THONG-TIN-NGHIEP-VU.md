# Thông tin nghiệp vụ đã chốt (Cổng 0) — QL32-W

> Nguồn sự thật cho toàn bộ đặc tả / thiết kế / phân công. Không được mâu thuẫn với file này khi viết tài liệu khác.

## 1. Hành chính đề tài

| Hạng mục | Nội dung |
|---|---|
| STT / Mã | 94 / **QL32-W** |
| Tên đề tài | Website quản lý đơn hàng sản xuất và tiến độ xưởng may tích hợp chatbot AI |
| Lĩnh vực | 32. Sản xuất |
| Học phần | Quản lý dự án công nghệ thông tin (MAN104) |
| Nền tảng | Web app (front-end + back-end API; có Web Responsive di động cho Tổ trưởng & Công nhân) |
| Mục tiêu sản phẩm | **MVP + mức Tốt** |
| Dữ liệu & áp dụng thực tế | Sprint phát triển dùng **seed mô phỏng chuẩn hóa**; giai đoạn UAT (tuần 6–7) nạp **dữ liệu mẫu khảo sát từ xưởng thật đã ẩn danh thông tin cá nhân (PII)** để dùng thử trên Render |
| Hosting demo | **Render** (URL công khai) |
| LLM chatbot | **CommandCode Provider API** (User API Key) |
| Stack chi tiết FE/BE/DB | Còn bàn (Q10) — không chặn phạm vi nghiệp vụ |

## 2. Mục tiêu đo được tại đơn vị (định tính; điền baseline/target sau khảo sát)

1. Giảm thời gian tra cứu tiến độ một đơn hàng (qua web / chatbot thay cho hỏi miệng hoặc dò sổ).
2. Phát hiện và cảnh báo sớm trong ngày đối với đơn trễ hạn, chuyền hụt kế hoạch ngày hoặc chậm nhập sản lượng giờ (trên in-app, Dashboard TV và chatbot).
3. Có báo cáo năng suất chuyền theo ngày / tuần chuẩn hóa để cân chuyền và đối soát lương khoán minh bạch.

> Đây là giá trị phần mềm mang lại cho xưởng (đo trước/sau khi khảo sát & UAT) — **không** phải KPI quản lý nội bộ nhóm (Jira/burndown).

## 3. Actor & quyền (tóm tắt)

| Vai trò | Trách nhiệm chính |
|---|---|
| Admin | Quản trị tài khoản/vai trò, danh mục hệ thống, mở khóa bản ghi sản lượng kèm lý do (khi kỳ lương chưa chốt), xem/chốt kỳ lương, cấp Kiosk Token TV |
| Kế hoạch | Quản lý danh mục mã hàng, đơn hàng (kể cả điều chỉnh biến động), lập kế hoạch theo chuyền (ngày + ca), xử lý cảnh báo, xem báo cáo |
| Tổ trưởng | Nhập / chốt sản lượng giờ cấp chuyền; phân bổ / chốt sản lượng công nhân cuối ca; xem kế hoạch & xử lý cảnh báo chuyền phụ trách |
| Giám đốc | Theo dõi tiến độ, cảnh báo, báo cáo năng suất, Dashboard TV, xem / chốt kỳ lương khoán |
| Công nhân | Có tài khoản riêng; chỉ truy cập 1 trang **Cá nhân** chỉ đọc (xem sản lượng đạt chuẩn của bản thân theo ca + phiếu lương khoán kỳ + hỏi Chatbot về SL/lương cá nhân) |
| Chatbot / Hệ thống | Hỏi đáp tiến độ & quy chế SOP; gọi read-tools theo vai + 1 write-tool an toàn có xác nhận (`acknowledge_alert`); tự động sinh cảnh báo |

- Một người dùng **có thể** giữ nhiều vai.
- Chi tiết ma trận: xem `01-dac-ta/06-actor-vai-tro.md`.

## 4. Thuật ngữ

| Thuật ngữ | Định nghĩa chốt |
|---|---|
| Danh mục Mã hàng (`products`) | Danh mục SKU/Style dùng chung toàn xưởng (mã SKU, tên SP, ĐVT, năng suất định mức SP/giờ công, đơn giá khoán chuẩn) |
| Đơn hàng (`orders`) | Lệnh sản xuất từ khách; có hạn giao, trạng thái, gồm nhiều dòng chi tiết (`order_items`) tham chiếu tới Danh mục Mã hàng |
| Chuyền (`production_lines`) | Dây chuyền may; có danh mục, tổ trưởng phụ trách và danh sách công nhân gắn chuyền |
| Ca (`shifts`) | Khung ca làm việc trong ngày (VD: Sáng, Chiều, Tối — có giờ bắt đầu/kết thúc chuẩn để kiểm tra chậm nhập SL) |
| Kế hoạch SX | Gán đơn + mã hàng → chuyền theo **ngày + ca** kèm SL kế hoạch ca/ngày; một mã hàng được chia nhiều chuyền |
| Sản lượng giờ (cấp Chuyền) | Bản ghi theo khoảng giờ BĐ–KT trên chuyền (UI bậc thang mã hàng → đơn): gồm **SL đạt chuẩn**, **SL lỗi tổng** (1 con số, không phân loại mã lỗi QC) và **Số công nhân đứng chuyền thực tế** |
| Sản lượng công nhân cuối ca | Bảng phân bổ/chốt SL đạt chuẩn cho từng công nhân trong ca của chuyền; tổng SL công nhân phải khớp tổng SL đạt chuẩn đã chốt của ca để tính lương khoán |
| Chốt sản lượng | Xác nhận bản ghi sản lượng → **khóa** đối với Tổ trưởng; riêng Admin được **Mở khóa kèm lý do audit** nếu kỳ lương liên quan chưa chốt |
| Tiến độ | Tỷ lệ sản lượng đạt chuẩn thực tế đã chốt so với kế hoạch ca/ngày và tổng SL đặt của đơn |
| Cảnh báo sản xuất | Sinh khi: (1) Quá hạn giao mà đơn chưa hoàn thành, (2) SL đạt chuẩn ngày &lt; KH ngày, hoặc (3) Chuyền đang có ca chạy nhưng quá khung giờ quy định chưa nhập SL |
| Đơn giá khoán | Đơn giá may trên 1 sản phẩm đạt chuẩn của Mã hàng (áp dụng tính lương khoán) |
| Kỳ lương | Khoảng thời gian chốt `Σ(đơn giá × SL đạt chuẩn công nhân)`; sau khi chốt kỳ lương thì khóa vĩnh viễn mọi bản ghi SL thuộc kỳ đó |
| Dashboard TV | Màn hình kiosk 16:9 (`1920×1080`) chỉ đọc, auto-refresh `30–60s`, mở bằng **Kiosk Token chỉ đọc** riêng biệt (không lộ dữ liệu lương) |
| Tool calling | Chatbot gọi API nghiệp vụ: bộ **read-tools** theo quyền + **1 write-tool an toàn có xác nhận** (`acknowledge_alert` cập nhật trạng thái xử lý cảnh báo) |

## 5. Thực thể tối thiểu (≥ 8 bảng — mức khái niệm)

`users`, `roles` / `user_roles`, `workers` (gắn `user_id` + chuyền), `production_lines`, `products` (danh mục mã hàng dùng chung), `orders`, `order_items`, `production_plans`, `hourly_outputs` (SL giờ cấp chuyền), `shift_worker_outputs` (SL công nhân theo ca), `alerts`, `piece_rates` (hoặc lịch sử đơn giá theo `product_id`), `payroll_periods`, `payroll_lines`, `chat_logs` (+ log mở khóa/biến động).

Chi tiết: `04-thiet-ke-he-thong/03-thuc-the-csdl-du-kien.md`.

## 6. Vòng đời & trạng thái

- **Đơn hàng:** `Nháp` → `Đã lập kế hoạch` → `Đang sản xuất` → `Hoàn thành` | `Trễ` | `Hủy`  
- **Kế hoạch chuyền (ngày + ca):** `Chưa chạy` → `Đang chạy` → `Đóng`  
- **Bản ghi sản lượng (giờ chuyền & ca công nhân):** `Nháp` → `Đã chốt (khóa Tổ trưởng)` ⇄ *(Admin mở khóa kèm lý do khi chưa chốt lương)* → `Khóa theo kỳ lương`  
- **Cảnh báo:** `Mở` → `Đang xử lý` → `Đã xử lý` | `Bỏ qua`  
- **Kỳ lương:** `Nháp (tạm tính)` → `Đã chốt`

## 7. Quy tắc nghiệp vụ

### 7.1 Sản lượng & Chốt khóa
- **Theo giờ (cấp Chuyền):** Tổ trưởng chọn giờ BĐ–KT, chọn bậc thang **Mã hàng → Đơn**, nhập **SL đạt chuẩn**, **SL lỗi tổng** (không chia loại lỗi QC) và **Số công nhân đứng chuyền thực tế**.
- **Cuối ca (cấp Công nhân):** Tổ trưởng nhập/phân bổ SL đạt chuẩn cho từng công nhân làm việc trong ca trên chuyền đó; hệ thống kiểm tra `Σ(SL đạt chuẩn công nhân) = Tổng SL đạt chuẩn của chuyền trong ca` trước khi cho chốt ca.
- **Khóa & Mở khóa có kiểm soát:** Sau khi bấm **Chốt**, Tổ trưởng không được sửa/xóa. Nếu nhập nhầm, chỉ **Admin** được mở khóa kèm bắt buộc nhập **lý do mở khóa** (lưu audit log), và chỉ thực hiện được khi bản ghi đó **chưa nằm trong kỳ lương Đã chốt**.

### 7.2 Biến động đơn hàng (xử lý rủi ro đề bài)
- Khi đơn ở `Nháp` hoặc `Đã lập kế hoạch` (chưa phát sinh sản lượng chốt): Kế hoạch được sửa/xóa tự do.
- Khi đơn đã ở `Đang sản xuất`:
  - **Cấm** xóa đơn hoặc xóa dòng mã hàng đã có sản lượng.
  - **Cho phép** Kế hoạch cập nhật `Hạn giao`, điều chỉnh `SL đặt mới` với điều kiện **`SL đặt mới ≥ Tổng SL đạt chuẩn đã sản xuất`**, hoặc chuyển trạng thái `Hủy` kèm **lý do biến động**.
  - Khi đơn `Hủy` giữa chừng, toàn bộ sản lượng đạt chuẩn công nhân đã làm trước thời điểm hủy **vẫn được bảo lưu** để tính lương khoán.

### 7.3 Cảnh báo trễ & chậm nhập sản lượng
- Hệ thống gắn cờ / sinh cảnh báo khi xảy ra một trong 3 điều kiện:
  1. **Trễ hạn giao:** `hôm_nay > hạn_giao` và đơn chưa `Hoàn thành` / `Hủy`.
  2. **Hụt kế hoạch ngày:** `Tổng SL đạt chuẩn ngày < SL kế hoạch ngày` (theo chuyền/đơn).
  3. **Chậm nhập sản lượng giờ** *(xử lý rủi ro đề bài)*: Chuyền có kế hoạch `Đang chạy` trong ca nhưng quá khung giờ quy định chưa có bản ghi sản lượng giờ.
- Hiển thị đồng bộ trên: **in-app + Dashboard TV + Chatbot**.

### 7.4 Năng suất (báo cáo)
**Nguồn tham khảo:** [VTI Solutions — Công thức tính năng suất lao động](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/)  
*(Công thức nền tảng: Kết quả đầu ra đạt chuẩn / Thời gian hoặc giờ công nhân lực thực tế).*

Áp dụng vào đề QL32-W:
- **Tổng giờ công thực tế** = `Σ ( (Giờ_KT − Giờ_BĐ) × Số_công_nhân_đứng_chuyền )`
- **NS chuyền (ngày/tuần)** = `Tổng SL đạt chuẩn của chuyền / Tổng giờ công thực tế của chuyền` (SP / giờ công)
- **NS đơn** = `Tổng SL đạt chuẩn của đơn / Tổng giờ công thực tế chi cho đơn` (SP / giờ công)
- **Hiệu suất so định mức (%)** = `(NS thực tế / NS định mức của Mã hàng) × 100%`  
  *(Bổ sung của nhóm trên nền công thức VTI để so sánh công bằng các mã hàng có độ khó may khác nhau).*

### 7.5 Lương khoán (mức Tốt)
- `LuongKhoanKy[congNhan] = Σ ( DonGiaKhoan[maHang] × SanLuongDatChuanCa[congNhan, maHang, ky] )`
- Không tính hệ số tăng ca, phụ cấp, thuế hay BHXH trong kỳ này.
- Xem / chốt kỳ lương: **Giám đốc + Admin**; **Công nhân** chỉ xem phiếu lương của chính mình.

### 7.6 Báo cáo & Dashboard TV
- **Báo cáo:** Trục chính **chuyền × ngày** → tổng hợp **tuần**; chiều lọc phụ theo **đơn / mã hàng / khách**.
- **Dashboard TV:** Chuẩn màn hình ngang `1920×1080`, chỉ đọc, auto-refresh `30–60s`, mở qua **Kiosk Token chỉ đọc**; hiển thị tiến độ các chuyền hôm nay, danh sách cảnh báo (trễ hạn / hụt KH / chậm nhập SL giờ) và SL giờ gần nhất từng chuyền.

## 8. Chatbot AI (CommandCode)

- **Tri thức (Lớp 2):** Dữ liệu CSDL qua API nghiệp vụ + **Tài liệu SOP/quy chế nội bộ xưởng** (quy tắc tính trễ, công thức năng suất, quy chế lương khoán, hướng dẫn chốt ca).
- **MVP:** Hỏi đáp tiến độ đơn / chuyền và giải đáp quy chế tính toán theo quyền đăng nhập.
- **Mức Tốt (Tool calling qua API):**
  - **Read-tools:** Tra tiến độ đơn, trạng thái chuyền, cảnh báo mở, tóm tắt năng suất, tóm tắt kỳ lương (GD/Admin), tra SL & lương cá nhân (Công nhân).
  - **1 Write-tool an toàn có xác nhận:** `acknowledge_alert` — cho phép **Giám đốc / Kế hoạch / Admin** cập nhật trạng thái xử lý cảnh báo (`Đang xử lý` / `Đã xử lý` kèm ghi chú) sau khi người dùng xác nhận rõ ràng trên khung chat.
  - **Cấm tuyệt đối:** Không có tool tạo/sửa/xóa đơn hàng, sản lượng hay lương khoán.
- Kết nối HTTP Provider API — xem `99-tham-chieu/commandcode-provider-api.md`.

## 9. MVP bắt buộc (In)

1. Auth + phân quyền RBAC (Admin, Kế hoạch, Tổ trưởng, Giám đốc + trang Cá nhân Công nhân)
2. Danh mục Mã hàng dùng chung & Quản lý Đơn hàng (kèm quy tắc biến động đơn đang SX)
3. Danh mục Chuyền, Ca & Kế hoạch sản xuất theo chuyền (ngày + ca)
4. Cập nhật sản lượng giờ cấp chuyền (`SL đạt chuẩn + SL lỗi tổng + số CN`) & chốt phân bổ SL công nhân cuối ca (khóa + Admin mở khóa có lý do)
5. Tiến độ & 3 loại cảnh báo (trễ hạn giao, hụt KH ngày, chậm nhập SL giờ)
6. Báo cáo năng suất theo công thức mục 7.4
7. Chatbot Q&A tiến độ + quy chế SOP + ghi log

## 10. Mức Tốt (In thêm)

8. Dashboard TV xưởng (kiosk `1920×1080`, auto-refresh, Kiosk Token chỉ đọc)
9. Tính lương khoán kỳ theo công nhân (mục 7.5) + phiếu lương cá nhân cho Công nhân
10. Chatbot tool calling qua API (bộ read-tools theo vai + 1 write-tool `acknowledge_alert` có xác nhận)

## 11. Ngoài phạm vi (Out) & Ranh giới cắt

- Kho / định mức nguyên phụ liệu (BOM) / mua hàng  
- Quản lý QC chi tiết theo mã lỗi, phiếu kiểm lô, biểu đồ Pareto *(thuộc đề QL32-D — kỳ này chỉ ghi 1 số SL lỗi tổng để tách SL đạt chuẩn)*  
- Kết nối phần cứng IoT / đếm sản phẩm tự động từ máy may / MES  
- ERP kế toán tổng hợp, chấm công vân tay, BHXH, thuế TNCN, tăng ca phức tạp  
- Mobile app native iOS/Android *(kỳ này hỗ trợ Web Responsive trên trình duyệt di động cho Tổ trưởng & Công nhân)*  
- Gửi Email / SMS / Zalo / Web Push ra ngoài hệ thống  
- Huấn luyện / fine-tune mô hình ML riêng  
- Tool chatbot ghi/sửa đơn hàng, sản lượng hoặc lương khoán  
- Tích hợp đồng bộ trực tiếp vào phần mềm nội bộ sẵn có của xưởng *(kỳ này UAT trên Render với dữ liệu mẫu khảo sát từ xưởng thật đã ẩn danh PII)*

## 12. Dữ liệu Seed & UAT

- **Seed phát triển (Sprint 1–2):** 2–3 chuyền; 6–10 mã hàng dùng chung; 5–10 đơn (đủ trạng thái đúng hạn, trễ hạn, biến động SL, hủy giữa chừng); user đủ 5 vai (kèm 6–10 công nhân gắn chuyền); dữ liệu sản lượng nhiều ngày và 1 kỳ lương mẫu.
- **Dữ liệu UAT (Tuần 6–7):** Chuyển đổi từ số liệu khảo sát đơn vị thực tế, **ẩn danh toàn bộ PII** (họ tên công nhân, SĐT, tên thương mại nhạy cảm của khách đặt may).

## 13. Kịch bản demo bảo vệ (tóm tắt)

1. Admin kiểm tra tài khoản các vai, danh mục chuyền/mã hàng/đơn giá, Kiosk Token TV.
2. Kế hoạch tạo đơn từ Danh mục Mã hàng → chia kế hoạch trên 2 chuyền (ngày + ca) → thử điều chỉnh biến động SL đơn đang SX.
3. Tổ trưởng (giao diện mobile/web) nhập SL giờ cấp chuyền (`SL đạt chuẩn + SL lỗi tổng + số CN`) → bấm Chốt → thử sửa lại (bị chặn); cuối ca chốt phân bổ SL công nhân.
4. Hệ thống phát sinh cảnh báo (trễ hạn / hụt KH / chậm nhập SL giờ) → đối chiếu trên in-app và **Dashboard TV Kiosk**.
5. Giám đốc xem báo cáo năng suất chuyền ngày/tuần; xem & chốt kỳ lương khoán; Công nhân đăng nhập xem phiếu lương cá nhân.
6. Chatbot trả lời tiến độ + giải thích công thức từ SOP + gọi read-tool + thực hiện write-tool `acknowledge_alert` có bước xác nhận; từ chối câu hỏi ngoài phạm vi.

Chi tiết: `01-dac-ta/08-man-hinh-va-kich-ban-demo.md`.
