# Kết nối CommandCode cho chatbot QL32-W

Tài liệu chính thức:
- [Provider API](https://commandcode.ai/docs/provider)
- [Studio / API Keys](https://commandcode.ai/docs/studio)
- [Quickstart CLI](https://commandcode.ai/docs/quickstart)
- [Headless](https://commandcode.ai/docs/headless)

## 1. Hai kênh — cùng một User API Key

| Kênh | Cách dùng | Dùng cho đồ án? |
|---|---|---|
| CLI terminal | `cmd login` hoặc env `COMMAND_CODE_API_KEY` rồi chạy `cmd` | Dev máy local / agent — **không** phải chatbot user trên web |
| **Provider API (HTTP)** | `Authorization: Bearer <KEY>` gọi REST | **Có** — BE trên Render gọi LLM |

Studio → **Generate API key** (User API Keys). Key này dùng được cho cả CLI và Provider API.

## 2. Endpoint chatbot

- Base: `https://api.commandcode.ai/provider/v1`
- Chat OpenAI-compatible: `POST /chat/completions`
- Danh sách model: `GET /models`
- Responses / Anthropic messages: xem docs nếu chọn model tương ứng

### Ví dụ curl

```bash
curl https://api.commandcode.ai/provider/v1/chat/completions \
  -H "Authorization: Bearer <CMD_API_KEY>" \
  -H "Content-Type: application/json" \
  -d "{\"model\":\"deepseek/deepseek-v4-flash\",\"messages\":[{\"role\":\"user\",\"content\":\"Hello\"}]}"
```

### OpenAI SDK (hướng BE)

```js
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: process.env.COMMAND_CODE_API_KEY,
  baseURL: "https://api.commandcode.ai/provider/v1",
});
```

## 3. Tool calling

- Mảng `tools` / function tools **pass-through**; **BE của nhóm** tự execute và kiểm tra phân quyền.
- Kỳ này đăng ký bộ **read-tools** theo vai + duy nhất **1 write-tool an toàn có xác nhận** (`acknowledge_alert` cập nhật trạng thái xử lý cảnh báo — xem `01-dac-ta/07-pham-vi-ai-5-lop.md`).
- Không chạy MCP remote qua CommandCode credential; tool nội bộ khai báo `type: "function"`.

## 4. Render / bảo mật

- Biến môi trường: `COMMAND_CODE_API_KEY` (hoặc `CMD_API_KEY`)
- Không commit key vào GitHub
- Plan **Go** không có API access — cần GOAT / Pro / Max / Team / Provider (theo docs)

## 5. Trách nhiệm FULL

- Tạo key, cấu hình env Render
- Prompt + guardrail 5 lớp AI
- Orchestrator: LLM ↔ read-tools ↔ API nghiệp vụ
- Log chat; hỗ trợ bộ ≥ 30 câu test
