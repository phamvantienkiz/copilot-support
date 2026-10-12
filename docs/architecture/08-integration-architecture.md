# 08 - Integration Architecture & Protocols

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Đặc tả giao thức tích hợp REST API, SSE streaming và định dạng dữ liệu truyền thông  

---

## 1. Các Giao thức Tích hợp Chính

### 1.1. REST API
- Chuẩn giao tiếp cho các tác vụ lấy dữ liệu tĩnh, cập nhật trạng thái tin nhắn và kích hoạt Single-Call Assist.
- Format: JSON chuẩn với HTTP status codes rõ ràng (200, 400, 404, 500).

### 1.2. Server-Sent Events (SSE) cho Live Agent Trace
- Endpoint: `POST /api/v1/agent/run`
- Header: `Content-Type: text/event-stream; charset=utf-8`
- Danh sách Event Types:
  - `event: turn_start` $\rightarrow$ Thông báo vòng lặp hiện tại (`turn: 1/6`)
  - `event: thought` $\rightarrow$ Nội dung suy luận của Agent
  - `event: tool_call` $\rightarrow$ Tên tool và tham số gọi
  - `event: tool_result` $\rightarrow$ Kết quả trả về từ tool, thời gian chạy (ms)
  - `event: suggestion` $\rightarrow$ Gợi ý 3 câu trả lời cuối cùng
  - `event: done` $\rightarrow$ Hoàn thành lượt xử lý, thống kê tổng token và chi phí
