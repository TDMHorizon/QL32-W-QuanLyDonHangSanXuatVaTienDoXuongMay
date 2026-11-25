# Kiến trúc hệ thống (phác thảo)

## 1. Tổng quan

```text
[Trình duyệt FE] --REST/JSON--> [BE API + Swagger]
                                      |
                    +-----------------+------------------+
                    |                 |                  |
                 [CSDL]     [CommandCode Provider API]  [Job/rule cảnh báo]
```

- FE: UI theo role (Desktop + Mobile Web cho TT/CN), TV kiosk `1920×1080`, chat widget  
- BE: auth/RBAC + TV Kiosk Token, nghiệp vụ, aggregate báo cáo/lương, chat orchestration + SOP + tool execute (read-tools + `acknowledge_alert` có xác nhận)  
- LLM: HTTP tới CommandCode — **không** chạy CLI `cmd` trên Render cho user chat  

## 2. Triển khai

- Hosting: **Render** (URL công khai)  
- Secret: `COMMAND_CODE_API_KEY`, `TV_KIOSK_TOKEN` (và chuỗi kết nối DB) qua env  
- Tách 1 service fullstack hay FE+BE riêng: **còn bàn (Q10)**  

## 3. Phân ranh giới FE / BE / FULL

| Lớp | Chịu trách nhiệm |
|---|---|
| BE | Schema, API, rule tiến độ/trễ/lương, seed, Swagger, tool handlers |
| FE | Màn hình, form, biểu đồ, TV layout, chat UI |
| FULL | Contract FE↔BE, LLM + prompt + tools, Render, demo E2E, smoke test |

Chi tiết epic: `00-ke-hoach/PHAN-CONG-BE-FE-FULL.md`.
