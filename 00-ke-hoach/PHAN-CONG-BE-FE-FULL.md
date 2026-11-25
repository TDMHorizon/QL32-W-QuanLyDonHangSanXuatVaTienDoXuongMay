# Phân công BE — FE — FULL (nhóm 3 người)

## 1. Vai trò người

| Luồng | Người | Trách nhiệm học phần / kỹ thuật |
|---|---|---|
| **FE** | Thành viên 1 | Frontend Developer; UI/UX & Truyền thông; hỗ trợ BA |
| **BE** | Thành viên 2 | Backend Developer; hỗ trợ BA; QA/QC |
| **FULL** | Thành viên 3 | Fullstack / Tech Lead; QA/QC; chatbot CommandCode; Render; E2E |

## 2. Định nghĩa ownership

- **BE:** CSDL ≥ 8 bảng (~14 bảng khái niệm), REST + Swagger, auth/RBAC + TV Kiosk Token, rule biến động đơn – tiến độ – 3 loại cảnh báo – năng suất – lương khoán, khóa/mở khóa SL có audit, seed + dữ liệu UAT ẩn danh, handler read-tools & `acknowledge_alert`, chat log API  
- **FE:** Layout theo role (Desktop cho KH/GD/Admin, Mobile Web `≥ 375px` cho TT/CN), CRUD danh mục mã hàng & đơn, nhập SL giờ chuyền + phân bổ SL ca công nhân, báo cáo, TV kiosk `1920×1080`, trang Cá nhân Công nhân, chat widget (có xác nhận write-tool)  
- **FULL:** Ghép FE↔BE, contract API, tài liệu SOP nội bộ + LLM Provider API + prompt + tool orchestration, deploy Render, kịch bản demo/UAT, smoke E2E & bộ ≥ 30 câu test AI  

## 3. Ma trận epic

| Epic | Nội dung | BE | FE | FULL |
|---|---|---|---|---|
| E1 | Auth, RBAC & Cá nhân CN | Schema user/role/worker, login, Kiosk Token | Login, route theo role, trang Cá nhân CN | Wiring session/JWT, account demo |
| E2 | Danh mục Mã hàng & Đơn hàng | CRUD SKU + đơn + rule biến động đơn đang SX | Danh mục SKU, form/list đơn, modal biến động | Seed + kiểm thử rule biến động |
| E3 | Chuyền, Ca & Kế hoạch | Lines/shifts/plans API | UI danh mục chuyền/ca & lập KH | Ràng buộc E2↔E3 |
| E4 | SL giờ chuyền & SL ca công nhân | Ghi nhận SL giờ + phân bổ ca + chốt/unlock audit | Form mobile bậc thang mã→đơn + chốt ca + modal unlock | Kiểm thử khóa 409 & đối soát ca |
| E5 | Tiến độ & 3 loại cảnh báo | Rule quá hạn / hụt KH / chậm nhập SL giờ + alerts API | Trung tâm cảnh báo & badge | Đồng bộ lên TV + chatbot |
| E6 | Báo cáo năng suất | Aggregate giờ công (`giờ × số CN`) & NS | Chart/table ngày–tuần & lọc | Đối soát công thức VTI |
| E7 | Chatbot Q&A + SOP | Context endpoints + `chat_logs` | Chat panel | CommandCode + nạp SOP + guardrail |
| E8 | Dashboard TV (Tốt) | `GET /tv/board` (auth Kiosk Token) | Kiosk `1920×1080` auto-refresh `30–60s` | URL TV mode trên Render |
| E9 | Lương khoán (Tốt) | Đơn giá SKU + tính/chốt kỳ lương (khóa SL kỳ) | Màn hình kỳ lương + phiếu lương CN | Đối soát với SL công nhân ca |
| E10 | Tool calling qua API (Tốt) | Read handlers + `acknowledge_alert` handler | Thẻ xác nhận thao tác trên chat | Orchestrator + human-in-the-loop + audit |

## 4. Backlog nhập (user story rút gọn)

Chưa ước lượng story point / chi phí.

- **E1** As user/công nhân, I want đăng nhập đúng vai (và xem trang Cá nhân chỉ đọc nếu là CN) so that dữ liệu được phân quyền chặt.  
- **E2** As Kế hoạch, I want quản lý Danh mục Mã hàng dùng chung và Đơn hàng (có điều chỉnh biến động hợp lệ) so that dữ liệu sản xuất chuẩn hóa.  
- **E3** As Kế hoạch, I want gán mã hàng vào chuyền theo ngày+ca so that tổ trưởng biết mục tiêu ca.  
- **E4** As Tổ trưởng, I want nhập SL giờ cấp chuyền trên điện thoại và phân bổ SL công nhân cuối ca rồi bấm chốt khóa so that tiến độ & lương khớp nhau.  
- **E5** As Giám đốc/KH, I want nhận cảnh báo đơn quá hạn, hụt KH ngày và chậm nhập SL giờ so that xử lý kịp ngay trong ca.  
- **E6** As Giám đốc, I want xem báo cáo NS chuyền ngày/tuần theo giờ công thực tế so that cân chuyền hiệu quả.  
- **E7** As user, I want hỏi chatbot về tiến độ và quy chế SOP so that tra cứu tức thời.  
- **E8** As xưởng, I want màn hình TV `1920×1080` mở bằng Kiosk Token chỉ đọc so that cả chuyền theo dõi an toàn.  
- **E9** As Giám đốc/CN, I want chốt kỳ lương khoán và xem phiếu lương cá nhân so that minh bạch thu nhập.  
- **E10** As Giám đốc/KH, I want chatbot gọi read-tools và cập nhật trạng thái cảnh báo (có bước xác nhận) so that thao tác nghiệp vụ nhanh qua hội thoại.  

## 5. Artifact bàn giao giữa các luồng

| Từ → Sang | Artifact |
|---|---|
| BE → FE | Swagger + ví dụ JSON + mã lỗi (`409` khi sửa bản ghi đã chốt hoặc SL ca lệch tổng) |
| FE → FULL | Link màn hình / Figma hoặc screenshot (Desktop + Mobile `375px` + TV `1920×1080`) + danh sách route |
| FULL → cả nhóm | URL Render, tài khoản demo đủ 5 vai, Kiosk Token mẫu, checklist smoke E2E |
| FULL → BE | Tài liệu SOP nội bộ + schema read-tools & `acknowledge_alert` |
| BE → FULL | Seed script + bộ dữ liệu UAT ẩn danh PII |

## 6. Gợi ý nhịp kỹ thuật (8 tuần đề bài)

- **Sprint 0 (Tuần 1):** Khảo sát đơn vị thực tế + chốt Cổng 0 & đặc tả + chốt Q10 stack + lập GitHub/Jira/MS Project  
- **Sprint 1 (Tuần 2–3):** E1–E3  
- **Sprint 2 (Tuần 4–5):** E4–E6 + E7 cơ bản (báo cáo giữa kỳ tuần 5)  
- **Sprint 3 (Tuần 6–7):** E8–E10 + nạp dữ liệu UAT ẩn danh & chạy UAT tại đơn vị  
- **Tuần 8:** Hoàn thiện hồ sơ D01–D27, quay video 5–7 phút, phát hành & bảo vệ  

## 7. Kiểm soát rủi ro kỹ thuật & nghiệp vụ

- **Sản lượng không nhập kịp thời:** Cảnh báo tự động "Chậm nhập SL giờ" trên Web/TV + giao diện Mobile Web gọn nhẹ cho Tổ trưởng.  
- **Đơn hàng biến động:** Cho sửa hạn giao & SL đặt (`≥ SL đã may`) hoặc hủy đơn có bảo lưu SL tính lương kèm lý do audit.  
- **Nhập nhầm rồi lỡ bấm Chốt:** Admin có chức năng mở khóa kèm lý do trước khi kỳ lương chốt.  
- **LLM lỗi key / timeout (> 15s) / rate limit:** FULL cấu hình timeout và trả thông báo fallback thân thiện.  
- **Scope creep:** Mọi thay đổi phải cập nhật qua `00-ke-hoach/THONG-TIN-NGHIEP-VU.md`.  
