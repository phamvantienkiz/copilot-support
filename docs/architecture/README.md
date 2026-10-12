# Architecture Map & System Invariants

> **Status:** Active  
> **Single Source of Truth:** Lớp tài liệu Kiến trúc này là nguồn sự thật duy nhất và bất biến (Living Architecture).  
> ⛔ **TUYỆT ĐỐI KHÔNG chia nhỏ thư mục này theo Release.**

---

## 1. Bản đồ Tài liệu Kiến trúc

| Thứ tự | Tài liệu | Mô tả phạm vi | Trạng thái |
| :---: | :--- | :--- | :---: |
| `01` | [01-system-context.md](./01-system-context.md) | Biên giới hệ thống, Actors, Use Case Boundary | Template |
| `02` | [02-high-level-architecture.md](./02-high-level-architecture.md) | Topo tổng thể hệ thống, mô hình 3 cột | Template |
| `03` | [03-service-architecture.md](./03-service-architecture.md) | Phân rã Service, Module boundaries | Template |
| `04` | [04-deployment-architecture.md](./04-deployment-architecture.md) | Môi trường runtime, Docker, SQLite | Template |
| `05` | [05-data-architecture.md](./05-data-architecture.md) | Database schema, 4 Dataset JSON, Vector store | Template |
| `06` | [06-ai-architecture.md](./06-ai-architecture.md) | ReAct loop, 6 Tools, Hybrid search, Grounding | Template |
| `07` | [07-security-architecture.md](./07-security-architecture.md) | Read-only tools, API Key security, Sanitization | Template |
| `08` | [08-integration-architecture.md](./08-integration-architecture.md) | REST API, Server-Sent Events (SSE) streaming | Template |
| `09` | [09-design-principles.md](./09-design-principles.md) | Clean Architecture, Deterministic vs LLM logic | Template |
| `10` | [10-folder-structure.md](./10-folder-structure.md) | Cấu trúc thư mục chuẩn cho backend & frontend | Template |
| `11` | [11-frontend-system-design.md](./11-frontend-system-design.md) | Bố cục 3 cột (Song song), State management | Template |
| `12` | [12-design-system.md](./12-design-system.md) | Component library (Chat, Copilot cards, Badge) | Template |
| `13` | [13-frontend-architecture.md](./13-frontend-architecture.md) | Luồng SSE, Optimistic update, Replay engine | Template |
| `14` | [14-design-language.md](./14-design-language.md) | Theme tokens, Dark mode, Typography | Template |

---

## 2. Danh mục Quyết định Kiến trúc (ADRs)
- [ADR-001: Chuyển đổi sang Python FastAPI & Tách biệt Deterministic Data](./adr/ADR-001.md)
