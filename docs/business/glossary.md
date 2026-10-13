# Domain Glossary — Copilot Support

> **Status:** Draft  
> **Project:** Copilot Support  
> **Last Updated:** 2026-10-12  

---

## 1. Domain Terminology

| Thuật ngữ | Tiếng Anh | Định nghĩa & Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| **Copilot** | AI Copilot | Trợ lý AI hỗ trợ người trực, không gửi tin nhắn trực tiếp tới khách hàng. |
| **Người trực** | Support Operator / Agent | Nhân viên chăm sóc khách hàng vận hành giao diện chat và duyệt nội dung. |
| **Human in the Loop** | HITL | Cơ chế người trực duyệt, sửa đổi hoặc loại bỏ câu trả lời của AI trước khi gửi. |
| **Khách hàng** | Customer / Buyer | Người mua hàng nhắn tin khiếu nại hoặc hỏi thông tin đơn hàng. |
| **Đơn hàng** | Order | Đơn mua hàng đã ghi nhận trong hệ thống gồm mã đơn, sản phẩm, trạng thái giao. |
| **Playbook / Case khiếu nại** | Complaint Case | Kịch bản hoặc tiền lệ xử lý khiếu nại trong quá khứ đã được kiểm chứng. |
| **Lỗi kép** | Compound Issue | Tình huống khách gặp từ 2 vấn đề trở lên (ví dụ: vừa giao trễ vừa vỡ hàng). |
| **CLV** | Customer Lifetime Value | Tổng giá trị chi tiêu tích lũy của khách hàng từ trước đến nay. |
| **AOV** | Average Order Value | Giá trị trung bình trên một đơn hàng của khách. |
| **Shop Benchmark** | Shop Benchmark | Mức chung toàn shop (Trung vị CLV, AOV trung bình, Tỷ lệ giao đúng hạn 92%) dùng để đối chiếu hồ sơ khách. |
| **Cần lưu ý (Context Warning)** | AI Context Warning | Thẻ cảnh báo người trực về tiền lệ khiếu nại cũ (lỗi lặp lại) kèm tag truy vết mã đơn quá khứ. |
| **Ba giọng phản hồi** | 3 Response Tones | 3 phong cách trả lời soạn sẵn: Đồng cảm (*Empathetic*), Ngắn gọn (*Concise*), Trang trọng (*Formal*). |
| **Nút "Sử dụng"** | Use Suggestion Button | Nút bấm 1 chạm đưa văn bản gợi ý vào ngay ô chat để người trực chỉnh sửa trước khi gửi. |
| **Live Trace** | Live Execution Trace | Dòng thời gian hiển thị tiến trình suy luận (thought), gọi công cụ và số liệu token/thời gian của Agent. |
| **Parallel Tool Calling** | Batch/Parallel Tools | Khả năng Agent kích hoạt đồng thời nhiều tool tra cứu (Đơn, Sản phẩm, Playbook) trong cùng một vòng ReAct. |
| **Hybrid Search** | Hybrid Search | Thuật toán tìm kiếm kết hợp $0.3 \times \text{BM25 (từ khóa)} + 0.7 \times \text{semantic (ý nghĩa)}$. |
| **Dual Score Bar** | Dual Score Bar | Thanh hiển thị phân rã độ khớp kết quả: phần bên trái là điểm BM25, phần bên phải là điểm Semantic. |
| **Pinned Entity** | Pinned Entity | Thực thể khách hàng đang tương tác được ghim cố định trên đầu kết quả tìm kiếm. |
| **Grounding** | Grounding Check | Bộ lọc hậu xử lý đối chiếu mã đơn, sản phẩm, chính sách của LLM với dữ liệu thật. |
| **Replay Mode** | Trace Replay | Chế độ chạy lại các lượt suy luận và tra cứu đã ghi hình sẵn mà không tốn API. |
| **DeepSeek v4.1 Flash** | DeepSeek Flash LLM | Mô hình ngôn ngữ chi phí siêu tiết kiệm (~$0,0016 / lượt chat) làm bộ não cho Copilot. |
