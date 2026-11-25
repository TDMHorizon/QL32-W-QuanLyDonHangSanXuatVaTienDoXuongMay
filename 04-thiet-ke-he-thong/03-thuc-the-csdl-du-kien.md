# Thực thể CSDL dự kiến (≥ 8 bảng)

> Chỉ liệt kê thực thể khái niệm & quan hệ phục vụ phạm vi — **không** làm chuẩn hóa 3NF / phụ thuộc hàm trong đợt tài liệu này.

## Danh sách bảng dự kiến (~14 bảng)

| Bảng | Mục đích chính |
|---|---|
| `users` | Tài khoản đăng nhập (Admin, Kế hoạch, Tổ trưởng, Giám đốc, Công nhân) |
| `roles` / `user_roles` | Danh mục vai trò & bảng gán nhiều vai cho 1 người dùng |
| `workers` | Hồ sơ công nhân gắn `user_id` và chuyền hiện tại (mã ẩn danh khi UAT) |
| `production_lines` | Danh mục chuyền may; tổ trưởng phụ trách |
| `shifts` | Danh mục khung ca làm việc trong ngày (giờ BĐ–KT chuẩn) |
| `products` | **Danh mục Mã hàng (SKU/Style) dùng chung**: mã SKU, tên SP, ĐVT, NS định mức (SP/giờ công), đơn giá khoán hiện hành |
| `orders` | Đơn hàng sản xuất (khách hàng ẩn danh khi UAT, ngày nhận, hạn giao, trạng thái, lý do biến động/hủy) |
| `order_items` | Dòng mã hàng trong đơn (`order_id`, `product_id`, SL đặt) |
| `production_plans` | Kế hoạch gán `order_item` → `production_line` theo `ngày + ca`; SL kế hoạch ca/ngày |
| `hourly_outputs` | **SL theo khoảng giờ cấp Chuyền**: `good_qty` (SL đạt chuẩn), `defect_qty` (SL lỗi tổng), `worker_count` (số CN đứng chuyền), trạng thái nháp/chốt, thông tin unlock audit |
| `shift_worker_outputs` | **SL đạt chuẩn phân bổ theo từng Công nhân cuối ca** (đối soát khớp tổng `good_qty` của ca trên chuyền) |
| `alerts` | Cảnh báo sản xuất (trễ hạn giao, hụt KH ngày, chậm nhập SL giờ) & trạng thái xử lý |
| `piece_rates` | Lịch sử / bảng đơn giá khoán theo `product_id` (hoặc tích hợp cùng `products` tùy thiết kế vật lý, vẫn giữ ≥ 8 bảng) |
| `payroll_periods` | Kỳ lương khoán (từ ngày – đến ngày, trạng thái nháp / đã chốt) |
| `payroll_lines` | Chi tiết lương từng công nhân theo mã hàng trong kỳ |
| `chat_logs` | Nhật ký hội thoại Chatbot, tool đã gọi và xác nhận thao tác |

## Quan hệ khái niệm chính

- `products` 1—N `order_items`; `orders` 1—N `order_items`
- `order_items` + `production_lines` + `shifts` + ngày → `production_plans`
- `production_plans` 1—N `hourly_outputs` (cập nhật tiến độ theo giờ & tính tổng giờ công thực tế)
- `production_plans` / ca + `workers` → `shift_worker_outputs` (tổng `SL đạt chuẩn` công nhân khớp tổng ca của chuyền)
- Rule cảnh báo đối chiếu `orders` + `production_plans` + `hourly_outputs` + `shifts` → `alerts`
- `products` / `piece_rates` + `shift_worker_outputs` (đã chốt) → `payroll_lines` thuộc `payroll_periods`

## Các trường bắt buộc cho công thức Năng suất

Nguồn công thức: [VTI Solutions — Công thức tính năng suất lao động](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/) (chi tiết áp dụng: `00-ke-hoach/THONG-TIN-NGHIEP-VU.md` §7.4).

- `good_qty` (Sản lượng đạt chuẩn) và `defect_qty` (SL lỗi tổng — không dùng tính NS/lương) trên `hourly_outputs`  
- Khoảng giờ `(end_time - start_time)` × `worker_count` (Số công nhân đứng chuyền thực tế) → **Tổng giờ công thực tế**  
- `standard_productivity` (Năng suất định mức SP/giờ công) trên bảng `products`  
