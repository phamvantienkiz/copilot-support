# 01 - System Context & Boundary

> **Status:** Active (Synchronized with Release 1 PRD)  
> **Layer:** Architecture Layer  
> **Scope:** Biên giới hệ thống Copilot Support, các tác nhân (Actors), sơ đồ Use Case ranh giới phạm vi nghiệp vụ  

---

## 1. Actors & External Entities
- **Khách hàng (Customer / Shopper Simulator):** Gửi tin nhắn thắc mắc/khiếu nại qua kênh chat của shop "Nhà Gọn".
- **Người trực CSKH (Support Operator):** Nhận tin nhắn, xem tiến trình suy luận và gợi ý của Copilot, bấm "Sử dụng", tinh chỉnh nội dung và bấm gửi.
- **LLM Provider (DeepSeek / OpenAI API):** Dịch vụ LLM đám mây xử lý các vòng suy luận ReAct, sinh câu trả lời theo khuôn mẫu JSON và tạo vector embeddings.

---

## 2. System Context Diagram (C4 Level 1)

```mermaid
flowchart TD
    Customer["👤 Khách hàng\n(Kênh chat / Simulator)"] -->|1. Gửi tin nhắn| ChatSystem["Support Copilot System\n(FastAPI + React Workspace)"]
    ChatSystem -->|2. Thông báo tin nhắn mới| Operator["👨‍💼 Người trực CSKH\n(Operator)"]
    ChatSystem -->|3. Tra cứu tự động & Soạn gợi ý| LLM["🌐 LLM Service\n(DeepSeek / OpenAI API)"]
    ChatSystem -->|4. Hiển thị Live Trace, Hồ sơ & 3 gợi ý| Operator
    Operator -->|5. Bấm 'Sử dụng', chỉnh sửa & Bấm gửi| ChatSystem
    ChatSystem -->|6. Phản hồi tin nhắn chính xác| Customer
```

---

## 3. Sơ đồ Use Case Ranh giới Hệ thống (Use Case Boundary Diagram)

Được đồng bộ trực tiếp từ bản đặc tả nghiệp vụ [docs/implementation/release-1/prd.md](../implementation/release-1/prd.md):

```mermaid
flowchart LR
    subgraph Actors ["Tác nhân (Actors)"]
        Customer["👤 Khách hàng"]
        Operator["👨‍💼 Người trực CSKH"]
        LLMService["🌐 LLM Provider"]
    end

    subgraph SystemBoundary ["Ranh giới Hệ thống: Support Copilot"]
        UC1["(Gửi tin nhắn khiếu nại)"]
        UC2["(Tự động tra cứu đơn & sản phẩm)"]
        UC3["(Tính toán chỉ số CLV, AOV & trễ hẹn)"]
        UC4["(Tìm kiếm Playbook theo Hybrid Search)"]
        UC5["(Soạn 3 mẫu phản hồi & Kiểm tra ID thật)"]
        UC6["(Hiển thị Live Trace & Gợi ý)"]
        UC7["(Chọn mẫu 'Sử dụng' & Chỉnh sửa)"]
        UC8["(Gửi tin nhắn phản hồi chính thức)"]
        
        UC1 -.->|<<trigger sau 1.2s>>| UC2
        UC2 -.->|<<include>>| UC3
        UC2 -.->|<<include>>| UC4
        UC2 -->|Chuyển dữ liệu| UC5
        UC5 <-->|Trao đổi ReAct| LLMService
        UC5 -->|Đẩy sự kiện SSE| UC6
        UC6 --> UC7
        UC7 --> UC8
    end

    Customer --> UC1
    UC6 --> Operator
    Operator --> UC7
    Operator --> UC8
    UC8 --> Customer
```

---

## 4. Boundary Invariants (Quy tắc Ranh giới Bất biến)
1. **Khách hàng không bao giờ được kết nối trực tiếp với LLM:** Toàn bộ phản hồi gửi đến khách hàng bắt buộc phải qua sự xác nhận của Người trực (Human in the loop).
2. **Cổng giao tiếp LLM kiểm soát Token:** Mọi tương tác với LLM Provider đều phải đi qua module `Meter` để kiểm soát độ dài context, giới hạn tối đa 6 vòng lặp và đo lường chi phí.
3. **Data Invariant:** Hệ thống chỉ được đọc dữ liệu từ 4 bảng JSON và SQLite Embeddings; Agent không có quyền sửa/xóa dữ liệu gốc.
