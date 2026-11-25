# Yêu cầu chức năng (FR)

Nguồn chuẩn: `00-ke-hoach/THONG-TIN-NGHIEP-VU.md` & Đề tài `QL32-W` (`99-tham-chieu/trich-de-QL32-W.md`).

## Nhóm A — Xác thực, phân quyền & cổng cá nhân

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-01 | Đăng nhập, đăng xuất và duy trì phiên làm việc an toàn | Tất cả |
| FR-02 | Quản lý người dùng và phân quyền theo vai trò (`Admin`, `Kế hoạch`, `Tổ trưởng`, `Giám đốc`, `Công nhân`); hỗ trợ 1 tài khoản giữ nhiều vai | Admin |
| FR-03 | Quản lý hồ sơ công nhân gắn với tài khoản và chuyền sản xuất để phục vụ phân bổ sản lượng ca & tính lương khoán | Admin, Kế hoạch |
| FR-04 | Xem/cập nhật thông tin hồ sơ cá nhân cơ bản (đổi mật khẩu); riêng **Công nhân** có trang **Cá nhân chỉ đọc** để xem SL đạt chuẩn theo ca và phiếu lương khoán kỳ của chính mình | Tất cả / Công nhân |

## Nhóm B — Danh mục Mã hàng & Đơn hàng sản xuất (MVP)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-05 | CRUD **Danh mục Mã hàng (SKU/Style) dùng chung**: mã SKU, tên sản phẩm, đơn vị tính, năng suất định mức chuẩn (SP/giờ công), đơn giá khoán chuẩn, trạng thái sử dụng | Admin, Kế hoạch |
| FR-06 | CRUD **Đơn hàng** (mã đơn, khách hàng, ngày nhận, hạn giao, trạng thái, ghi chú) và chọn các Mã hàng từ danh mục vào chi tiết đơn (`order_items`) kèm số lượng đặt | Kế hoạch, Admin |
| FR-07 | **Kiểm soát biến động đơn hàng:** Đơn `Nháp` / `Đã lập kế hoạch` được sửa/xóa tự do; khi `Đang sản xuất` thì cấm xóa mã hàng đã phát sinh SL, nhưng cho phép cập nhật `Hạn giao`, điều chỉnh `SL đặt mới ≥ SL đạt chuẩn đã may` hoặc chuyển `Hủy` (vẫn bảo lưu SL đã chốt để tính lương công nhân) kèm bắt buộc nhập lý do biến động | Kế hoạch, Admin |
| FR-08 | Tra cứu, tìm kiếm và lọc danh sách đơn hàng theo trạng thái, khoảng hạn giao, mã hàng, khách hàng | Kế hoạch, Giám đốc, Tổ trưởng (đơn liên quan) |

## Nhóm C — Danh mục Chuyền, Ca & Kế hoạch sản xuất (MVP)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-09 | Quản lý danh mục **Chuyền may** (mã/tên chuyền, tổ trưởng phụ trách, số công nhân định biên, trạng thái) và cấu hình khung **Ca làm việc** chuẩn trong ngày (giờ BĐ–KT ca) | Admin, Kế hoạch |
| FR-10 | Lập kế hoạch sản xuất: gán Mã hàng của Đơn hàng vào Chuyền theo **ngày + ca** kèm sản lượng kế hoạch; hỗ trợ chia 1 mã hàng chạy song song trên nhiều chuyền | Kế hoạch |
| FR-11 | Xem lịch / bảng kế hoạch sản xuất lọc theo chuyền, ngày, ca, đơn hàng | Kế hoạch, Tổ trưởng, Giám đốc |

## Nhóm D — Sản lượng theo giờ, Phân bổ cuối ca & Chốt khóa (MVP)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-12 | **Nhập sản lượng theo giờ (cấp Chuyền):** Tổ trưởng chọn khung giờ BĐ–KT trong ca, chọn bậc thang **Mã hàng → Đơn hàng**, nhập `SL đạt chuẩn`, `SL lỗi tổng` (1 con số tổng quát, không chia mã lỗi QC) và `Số công nhân đứng chuyền thực tế` | Tổ trưởng |
| FR-13 | **Phân bổ sản lượng công nhân cuối ca:** Tổ trưởng nhập/phân bổ `SL đạt chuẩn` cho từng công nhân làm việc trong ca trên chuyền đó; hệ thống kiểm tra ràng buộc `Σ(SL công nhân) = Tổng SL đạt chuẩn đã chốt của ca` | Tổ trưởng |
| FR-14 | **Chốt khóa & Mở khóa có kiểm soát:** Khi Tổ trưởng bấm **Chốt**, bản ghi chuyển sang trạng thái khóa (Tổ trưởng không thể sửa/xóa). Trường hợp nhập nhầm, chỉ **Admin** được thực hiện **Mở khóa kèm lý do** (ghi audit log) với điều kiện bản ghi chưa thuộc kỳ lương `Đã chốt` | Tổ trưởng, Admin |
| FR-15 | Tách biệt rõ `SL đạt chuẩn` và `SL lỗi tổng`: chỉ dùng `SL đạt chuẩn` đã chốt để tính tiến độ hoàn thành đơn, báo cáo năng suất và lương khoán | Hệ thống |

## Nhóm E — Tiến độ & Cảnh báo sớm (MVP)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-16 | Tự động tính % tiến độ hoàn thành của từng dòng mã hàng, từng đơn hàng và từng kế hoạch chuyền trong ngày từ các bản ghi `SL đạt chuẩn` đã chốt | Hệ thống |
| FR-17 | Tự động phát hiện và sinh 3 loại cảnh báo: **(1) Đơn quá hạn giao** mà chưa hoàn thành, **(2) Chuyền/đơn hụt kế hoạch ngày** (`SL đạt chuẩn ngày < KH ngày`), và **(3) Chậm nhập sản lượng giờ** khi chuyền đang trong ca chạy nhưng quá khung giờ quy định chưa ghi nhận SL | Hệ thống |
| FR-18 | Xem danh sách cảnh báo in-app theo mức độ/loại; cập nhật trạng thái xử lý (`Mở` → `Đang xử lý` → `Đã xử lý` / `Bỏ qua`) kèm ghi chú phản hồi | Kế hoạch, Giám đốc, Tổ trưởng (chuyền phụ trách) |
| FR-19 | Đồng bộ dữ liệu cảnh báo đang mở lên **Dashboard TV xưởng** và cung cấp qua API cho **Chatbot AI** | Hệ thống |

## Nhóm F — Báo cáo năng suất (MVP)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-20 | Báo cáo năng suất theo trục chính **Chuyền × Ngày** và tổng hợp theo **Tuần** (bảng số liệu + biểu đồ trực quan) | Giám đốc, Kế hoạch, Tổ trưởng (xem chuyền mình) |
| FR-21 | Phân tích năng suất và sản lượng đạt chuẩn theo chiều **Đơn hàng / Mã hàng / Khách hàng** | Giám đốc, Kế hoạch |
| FR-22 | Tính tự động **Tổng giờ công thực tế** `= Σ((Giờ KT − Giờ BĐ) × Số công nhân thực tế)`, **NS chuyền**, **NS đơn** và **Hiệu suất so định mức (%)** theo công thức Cổng 0 (nguồn nền tảng: [VTI Solutions](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/)) | Hệ thống |

## Nhóm G — Chatbot AI hỏi đáp (MVP)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-23 | Hỏi đáp ngôn ngữ tự nhiên về tiến độ đơn hàng, tình trạng chuyền và giải thích quy chế tính toán dựa trên **dữ liệu hệ thống và tài liệu SOP nghiệp vụ nội bộ** | Tất cả (đã đăng nhập, theo phạm vi vai trò) |
| FR-24 | Kiểm soát phạm vi trả lời (từ chối câu hỏi ngoài nghiệp vụ xưởng hoặc vượt quyền xem dữ liệu); lưu đầy đủ **chat log** (người hỏi, vai trò, câu hỏi, tool gọi, phản hồi, thời gian) | Hệ thống |
| FR-25 | Kết nối dịch vụ LLM qua **CommandCode Provider API** (`POST /chat/completions`) với xử lý lỗi/timeout thân thiện (không tự huấn luyện mô hình ML) | Hệ thống |

## Nhóm H — Dashboard trên TV tại xưởng (Mức Tốt)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-26 | Giao diện **TV Kiosk toàn màn hình (16:9)** chỉ đọc: hiển thị tiến độ các chuyền trong ngày, danh sách đơn/chuyền đang có cảnh báo (trễ hạn, hụt KH, chậm nhập SL giờ) và sản lượng khung giờ gần nhất | Xưởng sản xuất (Kiosk), Giám đốc |
| FR-27 | Tự động làm mới dữ liệu định kỳ (`30–60s`), không có nút nhập/sửa nghiệp vụ; truy cập qua **Kiosk Token chỉ đọc** do Admin quản lý (không cần đăng nhập tài khoản Giám đốc trên TV chung) | Hệ thống, Admin |

## Nhóm I — Tính lương khoán (Mức Tốt)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-28 | Quản lý bảng / hiệu lực **Đơn giá khoán** theo từng Mã hàng trong Danh mục Mã hàng | Admin, Kế hoạch |
| FR-29 | Tạo kỳ lương (theo khoảng ngày) và tự động tính `Lương khoán = Σ(Đơn giá khoán × SL đạt chuẩn theo ca của từng công nhân)` (không tính tăng ca/BHXH/thuế) | Hệ thống, Admin, Giám đốc |
| FR-30 | Rà soát bảng lương chi tiết, **Chốt kỳ lương** (khóa vĩnh viễn các bản ghi SL thuộc kỳ đó) và hiển thị phiếu lương chỉ đọc trên trang Cá nhân của từng Công nhân | Giám đốc, Admin, Công nhân (chỉ xem của mình) |

## Nhóm J — Chatbot thao tác nghiệp vụ qua API / Tool Calling (Mức Tốt)

| Mã | Mô tả yêu cầu | Actor |
|---|---|---|
| FR-31 | Chatbot gọi **Read-tools** qua API nội bộ theo phân quyền: tra tiến độ đơn, trạng thái chuyền, cảnh báo mở, tóm tắt năng suất (KH/GD/Admin), tóm tắt kỳ lương (GD/Admin), hoặc tra SL & lương cá nhân (Công nhân) | Tất cả (theo ma trận quyền từng tool) |
| FR-32 | Chatbot thực hiện **1 Write-tool nghiệp vụ an toàn có xác nhận** (`acknowledge_alert`): hỗ trợ cập nhật trạng thái xử lý cảnh báo (`Đang xử lý` / `Đã xử lý` kèm ghi chú) **chỉ sau khi người dùng xác nhận rõ ràng** | Giám đốc, Kế hoạch, Admin |
| FR-33 | Chặn cứng ở tầng Backend: không cung cấp và không thực thi bất kỳ tool ghi/sửa/xóa nào đối với Đơn hàng, Kế hoạch, Sản lượng hay Lương khoán | Hệ thống |
