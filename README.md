# QL32-W — Quản lý đơn hàng sản xuất & tiến độ xưởng may + chatbot AI

Đồ án MAN104 — Quản lý dự án công nghệ thông tin. Đề **94 / QL32-W**.

## Thông tin nhanh

| Hạng mục | Nội dung |
|---|---|
| Mục tiêu sản phẩm | MVP + mức Tốt |
| Nền tảng | Web app FE + BE API |
| Hosting demo | Render (URL công khai) |
| LLM | CommandCode Provider API |
| Dữ liệu & thực tế | Seed mô phỏng (Sprint đầu) → Dữ liệu UAT xưởng thật đã ẩn danh PII (tuần 6–7) |
| Nhóm kỹ thuật | 3 người: FE / BE / FULL |

## Cấu trúc tài liệu

- `00-ke-hoach/` — Cổng 0 nghiệp vụ, phân công BE–FE–FULL, checklist duyệt
- `01-dac-ta/` — Ý tưởng, FR/NFR, phạm vi, actor, 5 lớp AI, màn hình & demo
- `04-thiet-ke-he-thong/` — Kiến trúc, API dự kiến, thực thể CSDL (không chuẩn hóa)
- `99-tham-chieu/` — Trích đề QL32-W, CommandCode, **danh mục nguồn + quy tắc trích dẫn**

## Nguồn sự thật

Mọi quyết định nghiệp vụ lấy từ [`00-ke-hoach/THONG-TIN-NGHIEP-VU.md`](00-ke-hoach/THONG-TIN-NGHIEP-VU.md).

Trước khi code: tick [`00-ke-hoach/CHO-DUYET.md`](00-ke-hoach/CHO-DUYET.md).

## Quy tắc trích dẫn (bắt buộc)

Mọi tham khảo bên ngoài **phải có link nguồn** — xem [`99-tham-chieu/nguon-tham-khao.md`](99-tham-chieu/nguon-tham-khao.md).

Công thức năng suất: [VTI Solutions](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/).

## Phân công

Xem [`00-ke-hoach/PHAN-CONG-BE-FE-FULL.md`](00-ke-hoach/PHAN-CONG-BE-FE-FULL.md).

## Lưu ý

- Stack FE/BE/DB chi tiết còn bàn (Q10) — không chặn phạm vi.
- Không commit API key CommandCode; dùng env trên Render.
- Đợt tài liệu này **không** gồm tính chi phí / chuẩn hóa CSDL.
