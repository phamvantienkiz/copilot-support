# 13 - Frontend Architecture & State Management

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Quản lý state, luồng Server-Sent Events và chế độ Replay  

---

## 1. State Management Architecture
- **Chat State:** Danh sách tin nhắn qua lại giữa khách hàng và người trực.
- **Agent Trace State:** Danh sách các sự kiện streaming nhận được từ SSE:
  - `turns`: mảng các vòng suy luận (thought, tool_calls, kết quả).
  - `status`: `'idle' | 'running' | 'completed' | 'error'`.
  - `elapsed_time`: số giây đã trôi qua.
- **Suggestion State:** 3 câu trả lời gợi ý kèm tone tags.

## 2. Replay Engine Architecture
- Khi bật chế độ **Replay Mode**:
  - Frontend nạp tệp JSON bản ghi từ `public/replay/{case-name}.json`.
  - Thay vì gọi API thật, một timer giả lập sẽ emit từng event theo đúng timestamp trong bản ghi để tái hiện lại chuyển động của Agent và câu trả lời gợi ý.
