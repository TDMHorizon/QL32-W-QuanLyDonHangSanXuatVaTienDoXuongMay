# API dự kiến (REST)

Base: `/api/v1` — có tài liệu Swagger/OpenAPI. Chi tiết path có thể tinh chỉnh khi chốt stack (Q10).

## Auth & Cá nhân
- `POST /auth/login`
- `POST /auth/logout`
- `GET /auth/me`
- `GET /me/outputs` & `GET /me/payslips` (trang Cá nhân chỉ đọc của Công nhân)

## Users & Danh mục gốc
- `GET/POST/PATCH /users` (gồm quản lý tài khoản & hồ sơ công nhân gắn chuyền)
- `GET/POST/PATCH /products` (Danh mục Mã hàng dùng chung: SKU, tên, ĐVT, NS định mức, đơn giá khoán chuẩn)
- `GET/POST/PATCH /lines` (chuyền may) & `GET/POST/PATCH /shifts` (khung ca)
- `GET/POST/PATCH /piece-rates` (nếu tách lịch sử hiệu lực đơn giá theo `product_id`)

## Đơn hàng & Biến động
- `GET/POST/PATCH /orders` (kiểm tra rule biến động khi đơn `Đang sản xuất`, lưu lý do điều chỉnh/hủy)
- `GET/POST/PATCH/DELETE /orders/{id}/items` (cấm xóa item đã phát sinh SL)

## Kế hoạch sản xuất
- `GET/POST/PATCH /plans` (lọc `line`, `date`, `shift`, `order`)
- `GET /plans/by-line/{lineId}`

## Sản lượng giờ (Chuyền) & Phân bổ cuối ca (Công nhân)
- `GET/POST/PATCH /hourly-outputs` (chỉ sửa được khi còn `Nháp`; gồm `goodQty`, `defectQty`, `workerCount`)
- `POST /hourly-outputs/{id}/confirm` (chốt → khóa Tổ trưởng; sửa sau chốt trả `409 Conflict`)
- `POST /hourly-outputs/{id}/unlock` (chỉ Admin, bắt buộc `reason`, chặn nếu kỳ lương đã chốt)
- `GET/POST /shift-worker-outputs` & `POST /shift-worker-outputs/confirm` (phân bổ & chốt SL công nhân cuối ca, kiểm tra tổng khớp SL đạt chuẩn ca)

## Tiến độ & Cảnh báo
- `GET /progress/orders/{id}`
- `GET /progress/lines/{id}`
- `GET/PATCH /alerts` (quá hạn giao, hụt KH ngày, chậm nhập SL giờ; cập nhật trạng thái xử lý)

## Báo cáo năng suất
- `GET /reports/productivity` (query: `line`, `from`, `to`, `groupBy=day|week`)
- `GET /reports/by-order` (lọc theo đơn / mã hàng / khách)

## Lương khoán
- `GET/POST /payroll/periods`
- `GET /payroll/periods/{id}`
- `POST /payroll/periods/{id}/finalize` (chốt kỳ lương → khóa bất biến SL thuộc kỳ)

## Chatbot AI
- `POST /chat` (message + xác nhận tool nếu có; BE nạp SOP + gọi CommandCode; trả câu trả lời + tool traces / yêu cầu xác nhận `acknowledge_alert`)
- `GET /chat/logs` (Admin / QA kiểm tra log)

## Dashboard TV Kiosk
- `GET /tv/board` (payload gọn cho màn hình `1920×1080`; xác thực qua header/query `TV_KIOSK_TOKEN` chỉ đọc, tách biệt tài khoản Giám đốc)

## Ghi chú Tool Calling
BE ánh xạ các hàm nội bộ đúng theo whitelist tại `01-dac-ta/07-pham-vi-ai-5-lop.md`: bộ read-tools theo vai trò và duy nhất 1 write-tool `acknowledge_alert` yêu cầu cờ xác nhận hợp lệ từ người dùng.
