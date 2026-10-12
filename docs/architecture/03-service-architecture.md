# 03 - Service Architecture & Component Boundaries

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Phân chia ranh giới trách nhiệm giữa các module dịch vụ trong Backend  

---

## 1. Service Breakdown

| Service Module | Trách nhiệm chính | Dependencies |
| :--- | :--- | :--- |
| **`app.services.retrieval`** | Tokenizer tiếng Việt, Okapi BM25 engine, Vector cosine engine, Hybrid search combiner | SQLite, JSON datasets |
| **`app.services.customerstats`** | Tính toán deterministically: CLV, AOV, tỷ lệ giao đúng hạn, tần suất mua | Order dataset |
| **`app.services.llm`** | Quản lý kết nối LLM, đếm token, đo lường chi phí (meter), xử lý JSON mode repair | OpenAI SDK / httpx |
| **`app.services.assist`** | Điều phối flow Single-call Assist, gom tin nhắn, gọi LLM, kiểm tra Grounding | Retrieval, Stats, LLM |
| **`app.services.agent`** | Vòng lặp ReAct loop (tối đa 6 turns), dispatch 6 read-only tools, phát SSE events | Retrieval, Stats, LLM |

## 2. Dependency Invariants
- `customerstats` không được phụ thuộc vào `llm` hay `retrieval`.
- `retrieval` không được phụ thuộc vào `agent` hay `assist`.
- Toàn bộ Tool của Agent là **Read-Only**, không được phép có bất kỳ tool nào thực hiện thao tác ghi/xóa dữ liệu.
