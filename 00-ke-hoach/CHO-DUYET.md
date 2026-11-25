# Checklist duyệt trước khi code

Tick khi cả nhóm thống nhất. Nguồn sự thật: `THONG-TIN-NGHIEP-VU.md`.

## Phạm vi & mục tiêu
- [ ] Mục tiêu đo được A+B+C (định tính; đo baseline/target khi khảo sát & UAT) — OK
- [ ] In = MVP + mức Tốt như `01-dac-ta/05-pham-vi.md`
- [ ] Out & ranh giới cắt rõ ràng (kho/NPL, QC Pareto, IoT, mobile native, email thật, AI sửa đơn/SL/lương…)
- [ ] Lộ trình dữ liệu: seed mô phỏng Sprint 1–2 → nạp dữ liệu mẫu UAT ẩn danh PII tuần 6–7

## Nghiệp vụ
- [ ] Vai: Admin / Kế hoạch / Tổ trưởng / Giám đốc + trang **Cá nhân chỉ đọc** cho Công nhân
- [ ] Tách **Danh mục Mã hàng dùng chung** (lưu NS định mức & đơn giá khoán) khỏi chi tiết đơn (`order_items`)
- [ ] Quy tắc biến động đơn `Đang sản xuất` (sửa hạn giao, SL mới ≥ SL đã may, hủy bảo lưu SL tính lương)
- [ ] Kế hoạch theo ngày + ca; 1 mã hàng chia nhiều chuyền
- [ ] Sản lượng 2 tầng: **SL giờ cấp chuyền** (`SL đạt chuẩn + SL lỗi tổng + số CN`) + **SL công nhân cuối ca** (khớp tổng ca)
- [ ] Chốt → khóa Tổ trưởng; **Admin mở khóa kèm lý do** khi kỳ lương chưa chốt
- [ ] 3 loại cảnh báo (quá hạn giao, hụt KH ngày, chậm nhập SL giờ); hiển thị in-app + TV + Chatbot
- [ ] Công thức NS (giờ công = số giờ × số CN) + lương khoán đã thống nhất

## AI, NFR & triển khai
- [ ] CommandCode Provider API (HTTP, không dùng CLI trên server chat)
- [ ] Chatbot bám dữ liệu CSDL + **tài liệu SOP nội bộ**; bộ Read-tools theo vai + **1 Write-tool `acknowledge_alert` có xác nhận**
- [ ] NFR định lượng: API < 2s, Chatbot < 15s, Mobile Web ≥ 375px cho TT/CN, TV `1920×1080` refresh `30–60s` dùng **Kiosk Token chỉ đọc**
- [ ] Host Render; secret qua env

## Trích dẫn nguồn
- [ ] Công thức NS đã ghi nguồn [VTI Solutions](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/)
- [ ] Mọi tham khảo ngoài đều có link trong file tương ứng / `99-tham-chieu/nguon-tham-khao.md`

## Phân công & GitFlow
- [ ] FE / BE / FULL hiểu epic E1–E10 và artifact bàn giao
- [ ] Cả nhóm nắm quy ước nhánh `QUY-UOC-GITFLOW.md` (`main`, `develop`, `feature/*`, `release/*`, `hotfix/*` + gắn mã Jira)
- [ ] Q10 stack: Node.js / TypeScript đã định hướng — chốt chi tiết framework con trước Sprint 1

## Ký duyệt (ghi tên)
| Vai | Tên | Ngày | Đồng ý |
|---|---|---|---|
| FE | | | |
| BE | | | |
| FULL (Tech Lead) | | | |

Sau khi đủ tick → mở Sprint code.
