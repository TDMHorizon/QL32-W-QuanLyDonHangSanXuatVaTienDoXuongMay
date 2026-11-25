# Actor và ma trận phân quyền

Nguồn: `00-ke-hoach/THONG-TIN-NGHIEP-VU.md`.

## 1. Danh sách Actor

| Actor | Mô tả vai trò trong hệ thống |
|---|---|
| **Admin** | Quản trị tài khoản/vai trò, danh mục gốc, cấp Kiosk Token cho TV xưởng; mở khóa bản ghi sản lượng kèm lý do (khi chưa chốt kỳ lương); xem và chốt kỳ lương khoán |
| **Kế hoạch** | Quản lý Danh mục Mã hàng dùng chung, Đơn hàng (kể cả xử lý biến động đơn đang SX), lập Kế hoạch chuyền (ngày + ca), xử lý cảnh báo, xem báo cáo; dùng Chatbot + tools |
| **Tổ trưởng** | Nhập & chốt sản lượng giờ cấp chuyền; phân bổ & chốt sản lượng công nhân cuối ca trên chuyền phụ trách; xem kế hoạch & xử lý cảnh báo liên quan; dùng Chatbot trong phạm vi chuyền |
| **Giám đốc** | Giám sát tiến độ toàn xưởng, xử lý cảnh báo, xem báo cáo năng suất, mở Dashboard TV, xem & chốt kỳ lương khoán; dùng Chatbot + đầy đủ tools |
| **Công nhân** | Tài khoản cá nhân gắn với hồ sơ công nhân trên chuyền; chỉ truy cập trang **Cá nhân chỉ đọc** (xem SL đạt chuẩn theo ca + phiếu lương khoán kỳ của bản thân) và hỏi Chatbot về số liệu cá nhân |
| **Hệ thống / Chatbot** | Tự động tính tiến độ, phát sinh 3 loại cảnh báo, tổng hợp năng suất & lương; điều phối LLM + thực thi read-tools và 1 write-tool cập nhật cảnh báo có xác nhận |

- Một người dùng **có thể** được gán nhiều vai trò (hợp quyền theo các vai được gán).

## 2. Ma trận phân quyền chi tiết

| Nhóm / Chức năng | Admin | Kế hoạch | Tổ trưởng | Giám đốc | Công nhân |
|---|:---:|:---:|:---:|:---:|:---:|
| Quản lý tài khoản, vai trò & Kiosk Token TV | ✓ | | | | |
| Quản lý hồ sơ công nhân gắn chuyền | ✓ | ✓ | xem* | xem | |
| Danh mục Mã hàng dùng chung (SKU, định mức, đơn giá) | ✓ | ✓ | xem | xem | |
| Danh mục Chuyền & khung Ca làm việc | ✓ | ✓ | xem | xem | |
| CRUD Đơn hàng & điều chỉnh biến động đơn đang SX | ✓ | ✓ | xem* | xem | |
| Lập kế hoạch sản xuất (ngày + ca) | ✓ | ✓ | xem* | xem | |
| Nhập & Chốt sản lượng giờ (cấp Chuyền) | | | ✓* | | |
| Phân bổ & Chốt sản lượng công nhân cuối ca | | | ✓* | | |
| Mở khóa bản ghi sản lượng kèm lý do (trước chốt lương) | ✓ | | | | |
| Xem tiến độ & danh sách cảnh báo | ✓ | ✓ | ✓* | ✓ | |
| Cập nhật trạng thái xử lý cảnh báo (Web UI) | ✓ | ✓ | ✓* | ✓ | |
| Báo cáo năng suất (chuyền×ngày/tuần, đơn/mã hàng) | ✓ | ✓ | xem* | ✓ | |
| Dashboard TV xưởng (hoặc qua Kiosk Token chỉ đọc) | ✓ | xem | xem | ✓ | |
| Tính & Chốt kỳ lương khoán toàn xưởng | ✓ | | | ✓ | |
| Trang Cá nhân: xem SL ca & phiếu lương của bản thân | | | | | ✓** |
| Chatbot Q&A (tiến độ / quy chế SOP) | ✓ | ✓ | ✓* | ✓ | ✓** |
| Chatbot Read-tools (theo phạm vi dữ liệu vai trò) | ✓ | ✓ | ✓* | ✓ | ✓** |
| Chatbot Write-tool `acknowledge_alert` (có xác nhận) | ✓ | ✓ | | ✓ | |

*\* Giới hạn trong phạm vi chuyền / đơn hàng được phân công phụ trách.*  
*\*\* Giới hạn nghiêm ngặt theo `user_id` của chính công nhân đang đăng nhập (không xem được người khác hay số liệu tài chính toàn xưởng).*

## 3. Quy tắc kiểm soát đặc biệt

1. **Khóa hai lớp đối với sản lượng:** Sau khi Tổ trưởng bấm `Chốt`, Tổ trưởng không thể tự sửa/xóa; Admin chỉ mở khóa được khi nhập lý do rõ ràng vào audit log và kỳ lương chứa bản ghi đó chưa chuyển sang `Đã chốt`. Khi kỳ lương `Đã chốt`, toàn bộ bản ghi liên quan bị khóa bất biến.
2. **Cách ly màn hình TV Kiosk:** URL mở bằng `Kiosk Token` chỉ có quyền gọi duy nhất endpoint đọc bảng tin TV (`GET /tv/board`), không có phiên đăng nhập người dùng và không có quyền truy cập API lương hay danh mục nội bộ.
3. **Kiểm soát ghi của Chatbot:** Chỉ vai `Admin`, `Kế hoạch`, `Giám đốc` mới được gọi `acknowledge_alert` kèm xác nhận trên giao diện; không vai nào (kể cả Admin) có thể ra lệnh cho Chatbot tạo/sửa/xóa đơn hàng, sản lượng hay bảng lương.
