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
        Customer["👤 Khách hàng\n(Shopper Simulator)"]
        Operator["👨‍💼 Người trực CSKH\n(Support Operator)"]
        LLMService["🌐 LLM Provider\n(DeepSeek v4.1 Flash)"]
    end

    subgraph SystemBoundary ["Ranh giới Hệ thống: Support Copilot"]
        UC1["(1. Gửi tin nhắn khiếu nại)"]
        UC2["(2. Kích hoạt trinh sát sau 1.2s)"]
        
        subgraph CopilotCore ["Bộ Não Copilot & Tra Cứu"]
            UC3["(3. ReAct Loop & Parallel Tool Calling)"]
            UC4["(4. Hybrid Search: 0.3 BM25 + 0.7 Semantic)"]
            UC5["(5. Tính CLV, AOV & Benchmark Shop)"]
            UC6["(6. Grounding Guard & Soạn 3 Gợi ý)"]
        end
        
        subgraph WorkspaceUI ["Giao Diện 4 Thẻ Copilot Panel"]
            UC7["(Thẻ 1: Xem Hồ sơ nhanh, Nhận định AI, Cảnh báo & Biểu đồ 3 đơn)"]
            UC8["(Thẻ 2: Duyệt 3 mẫu gợi ý & Bấm 'Sử dụng' đưa vào ô chat)"]
            UC9["(Thẻ 3: Xem Live Trace tiến trình Agent theo thời gian thực)"]
            UC10["(Thẻ 4: Xem Phân rã điểm tìm kiếm BM25 vs Semantic)"]
        end
        
        UC11["(Chỉnh sửa nội dung & Bấm 'Gửi')"]
        
        UC1 -.->|<<trigger 1.2s>>| UC2
        UC2 --> UC3
        UC3 -.->|<<include>>| UC4
        UC3 -.->|<<include>>| UC5
        UC3 <-->|Trao đổi ReAct| LLMService
        UC3 --> UC6
        UC6 --> UC7
        UC6 --> UC8
        UC6 --> UC9
        UC6 --> UC10
        UC8 --> Operator
        Operator --> UC11
        UC11 --> Customer
    end

    Customer --> UC1
    UC7 --> Operator
    UC9 --> Operator
    UC10 --> Operator
```

---

## 4. Boundary Invariants (Quy tắc Ranh giới Bất biến)
1. **Khách hàng không bao giờ được kết nối trực tiếp với LLM:** Toàn bộ phản hồi gửi đến khách hàng bắt buộc phải qua sự xác nhận của Người trực (Human in the loop). Nút `↩ Sử dụng` chỉ dán text vào input để người trực duyệt/chỉnh sửa trước khi gửi.
2. **Cổng giao tiếp LLM kiểm soát Token & Chi phí:** Mọi tương tác với LLM Provider đều phải đi qua module `Meter` để kiểm soát độ dài context, giới hạn tối đa 6 vòng lặp và đo lường chi phí (mục tiêu ~ $0,0016 / lượt chat).
3. **Data Invariant:** Hệ thống chỉ được đọc dữ liệu từ 4 bảng JSON và SQLite Embeddings; Agent không có quyền sửa/xóa dữ liệu gốc.
4. **Deterministic Calculation Invariant:** Các chỉ số tài chính (CLV, AOV), tần suất mua hàng, tỷ lệ giao đúng hạn và Shop Benchmark bắt buộc do Server tính toán deterministic, tuyệt đối không để LLM tự suy diễn số liệu.
