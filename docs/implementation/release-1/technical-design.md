# Technical Design — Release 1: MVP Support Copilot

> **Status:** Draft  
> **Release:** Release 1  

---

## 1. Sequence Diagram: Luồng ReAct Agent Streaming (SSE)

```mermaid
sequenceDiagram
    autonumber
    actor Customer as Khách hàng
    participant Frontend as Frontend (Chat UI)
    participant FastAPI as FastAPI Server
    participant Agent as ReAct Agent Engine
    participant Tools as 6 Read-Only Tools
    participant LLM as DeepSeek / OpenAI API
    actor Operator as Người trực (Operator)

    Customer->>Frontend: Gửi tin nhắn khiếu nại
    Frontend->>FastAPI: POST /api/v1/agent/run (SSE stream request)
    FastAPI->>Agent: Khởi tạo Agent Context & ReAct Loop
    
    loop Tối đa 6 vòng (Turns)
        Agent->>LLM: Gửi Prompt + Tool Schemas + History
        LLM-->>Agent: Trả về Thought + Tool Calls (VD: search_orders)
        Agent-->>FastAPI: Emit Event: turn_start, thought, tool_call
        FastAPI-->>Frontend: SSE Stream Event
        Frontend-->>Operator: Render timeline trực tiếp
        
        Agent->>Tools: Thực thi tool (VD: search_orders("#10032"))
        Tools-->>Agent: Dữ liệu đơn hàng thực tế
        Agent-->>FastAPI: Emit Event: tool_result (kèm latency ms)
        FastAPI-->>Frontend: SSE Stream Event
    end

    Agent->>FastAPI: Tổng hợp 3 mẫu gợi ý (Empathetic, Concise, Formal)
    FastAPI->>FastAPI: Chạy Grounding Guard kiểm tra ID thật
    FastAPI-->>Frontend: Emit Event: suggestion & done
    Frontend-->>Operator: Hiển thị 3 Card gợi ý
    Operator->>Frontend: Bấm "Sử dụng" -> Chỉnh sửa -> Bấm "Gửi"
    Frontend->>Customer: Phản hồi tin nhắn
```

## 2. Activity Swimlane: Xử lý Grounding Guard

```mermaid
flowchart TD
    Start["Nhận JSON Output từ LLM"] --> ParseJSON["Parse Pydantic Schema"]
    ParseJSON --> ExtractIDs["Trích xuất Order IDs, Product IDs, Case IDs"]
    ExtractIDs --> CheckDB{"Tất cả ID có trong DB?"}
    CheckDB -- Có --> RenderOK["Đóng gói 3 mẫu gợi ý và gửi cho UI"]
    CheckDB -- Không --> Sanitize["Loại bỏ câu chứa ID bịa / Cảnh báo"]
    Sanitize --> RenderOK
```
