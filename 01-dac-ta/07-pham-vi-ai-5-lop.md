# Năm lớp quyết định phạm vi AI (Chatbot)

Phục vụ hồ sơ phạm vi Bài 2 & Kế hoạch chất lượng Bài 5 (MAN104). Dịch vụ LLM: **CommandCode Provider API**.

## Lớp 1 — Mục đích sử dụng (Purpose)

- **MVP:** Hỗ trợ người dùng tra cứu nhanh **tiến độ đơn hàng, tình trạng sản xuất của chuyền** và giải đáp thắc mắc về **quy chế tính toán nghiệp vụ** (quy tắc tính trễ, công thức năng suất, quy chế lương khoán, quy trình nhập/chốt ca) bằng ngôn ngữ tự nhiên tiếng Việt.
- **Mức Tốt:** Thực thi **tool calling qua API nghiệp vụ** để truy xuất số liệu thời gian thực chính xác theo phân quyền và hỗ trợ thực hiện **1 thao tác nghiệp vụ an toàn có xác nhận** (cập nhật trạng thái xử lý cảnh báo).

## Lớp 2 — Nguồn tri thức được phép (Knowledge Sources)

1. **Dữ liệu thời gian thực từ CSDL/API nội bộ:** Đơn hàng, danh mục mã hàng, kế hoạch ca, sản lượng đã chốt, cảnh báo mở, báo cáo năng suất, kỳ lương khoán (lọc chặt theo quyền của người đang chat).
2. **Tài liệu nghiệp vụ nội bộ (SOP xưởng may):** Bộ tài liệu chuẩn hóa nạp vào ngữ cảnh hệ thống gồm: định nghĩa trạng thái đơn, 3 điều kiện sinh cảnh báo, công thức năng suất theo nguồn [VTI Solutions](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/), công thức lương khoán và hướng dẫn chốt sản lượng ca.
- **Cấm:** Không tìm kiếm web bên ngoài; không tự suy diễn/bịa số liệu khi API trả về rỗng; không tự huấn luyện hay fine-tune mô hình học máy.

## Lớp 3 — Công cụ được cấp quyền (Tool Whitelist)

### 3.1 Nhóm Read-tools (chỉ đọc, lọc dữ liệu theo vai trò người gọi)

| Tên Tool (dự kiến) | Chức năng | Vai trò được phép |
|---|---|---|
| `get_order_progress` | Tra tiến độ chi tiết của đơn hàng / mã hàng | Admin, Kế hoạch, Giám đốc, Tổ trưởng (đơn liên quan) |
| `get_line_status` | Xem tình trạng kế hoạch & sản lượng chuyền trong ngày/ca | Admin, Kế hoạch, Giám đốc, Tổ trưởng (chuyền mình) |
| `list_production_alerts` | Liệt kê cảnh báo đang mở (trễ hạn, hụt KH, chậm nhập SL giờ) | Admin, Kế hoạch, Giám đốc, Tổ trưởng (chuyền mình) |
| `get_productivity_summary` | Tóm tắt năng suất & % hiệu suất định mức ngày/tuần | Admin, Kế hoạch, Giám đốc |
| `get_payroll_period` | Tóm tắt số liệu kỳ lương khoán toàn xưởng | Admin, Giám đốc |
| `get_my_output_and_payroll` | Xem SL đạt chuẩn theo ca & phiếu lương kỳ của chính mình | Công nhân |

### 3.2 Nhóm Write-tool có kiểm soát (Human-in-the-loop)

| Tên Tool (dự kiến) | Chức năng & Cơ chế an toàn | Vai trò được phép |
|---|---|---|
| `acknowledge_alert` | Chuyển trạng thái cảnh báo sang `Đang xử lý` / `Đã xử lý` kèm ghi chú phản hồi. **Bắt buộc 2 bước:** Bot tóm tắt cảnh báo & trạng thái dự kiến → Người dùng bấm nút/gõ xác nhận → Backend mới ghi DB và lưu audit log. | Admin, Kế hoạch, Giám đốc |

- **Tuyệt đối KHÔNG đưa vào whitelist:** Tạo/sửa/hủy đơn hàng, lập/sửa kế hoạch, nhập/chốt/mở khóa sản lượng, sửa đơn giá, chốt kỳ lương, quản lý tài khoản.

## Lớp 4 — Giới hạn & Quy tắc từ chối (Guardrails & Refusal Policy)

- **Ngoài chủ đề sản xuất của xưởng** (thời tiết, giải trí, viết code, tư vấn pháp luật lao động/BHXH/thuế, y tế, chính trị) → từ chối lịch sự và gợi ý các câu hỏi hợp lệ trong hệ thống.
- **Vượt quyền truy cập dữ liệu** (VD: Công nhân hỏi lương người khác hoặc hỏi tổng lương xưởng; Tổ trưởng chuyền 1 hỏi chi tiết lương) → từ chối ở cả tầng kiểm tra quyền của Backend tool handler lẫn phản hồi của Bot.
- **Yêu cầu ghi/sửa dữ liệu bị cấm** (VD: *"Chốt giúp tôi 150 áo cho chuyền 2"*, *"Đổi hạn giao đơn DH05"*) → từ chối thực hiện và hướng dẫn người dùng vào đúng màn hình chức năng tương ứng.

## Lớp 5 — Quan sát, Ghi nhận & Kiểm thử (Observability & Evaluation)

- **Ghi `chat_logs` 100% lượt hội thoại:** `user_id`, vai trò hiện tại, nội dung câu hỏi, danh sách tool đã gọi + tham số, trạng thái xác nhận (nếu là `acknowledge_alert`), câu trả lời của LLM, độ trễ phản hồi và mã lỗi (nếu có).
- **Xử lý sự cố dịch vụ ngoài:** Khi CommandCode API timeout (`> 15s`), lỗi khóa hoặc chạm giới hạn rate limit → trả thông báo tiếng Việt thân thiện, không làm lộ stack trace hay API key.
- **Bộ kiểm thử chất lượng AI:** Xây dựng **≥ 30 câu hỏi kiểm thử chuẩn** phủ đủ 5 nhóm: (1) hỏi tiến độ đơn/chuyền, (2) hỏi quy chế SOP, (3) gọi read-tools tổng hợp cảnh báo/năng suất/lương, (4) luồng xác nhận & từ chối `acknowledge_alert`, (5) câu hỏi ngoài phạm vi & kiểm thử vượt quyền (prompt injection / leo thang đặc quyền).

## Tóm tắt kết nối kỹ thuật

- Endpoint: `https://api.commandcode.ai/provider/v1/chat/completions`
- Header: `Authorization: Bearer <COMMAND_CODE_API_KEY>` (lấy từ biến môi trường trên Render)
- Tài liệu chi tiết: `99-tham-chieu/commandcode-provider-api.md`
