# Software Requirements Specification (SRS) — Release 1: Copilot Support MVP

> **Status:** Active (Synchronized with Release 1 PRD)  
> **Release:** Release 1 (MVP)  
> **Layer:** Implementation Layer  

---

## 1. Yêu cầu Chức năng Chi tiết (Detailed Functional Requirements)

### 1.1. FR-01: Chat Simulator & Nạp Kịch bản Mẫu
- Hệ thống cung cấp selector chọn khách hàng mẫu (mặc định: `cus_003 - Nguyễn Văn A`).
- Hỗ trợ nút nạp sẵn kịch bản khiếu nại "lỗi kép" (máy xay Zenka B600 nứt cối thủy tinh + giao trễ 4 ngày).

### 1.2. FR-02: Cơ chế Kích hoạt Trì hoãn (Debounce Trigger)
- Bộ đếm thời gian kích hoạt đúng **1.2 giây** sau tin nhắn cuối cùng của khách hàng.
- Nếu khách tiếp tục gõ tin nhắn trước khi hết 1.2s, bộ đếm tự động reset để gom hội thoại đầy đủ.

### 1.3. FR-03: Hồ sơ Nhanh & Nhận định AI kèm Shop Benchmark
- Server tính toán deterministic 4 chỉ số khách hàng: CLV (2,52tr), AOV (840k), Tần suất mua (3 đơn/13 tháng), Tỷ lệ đúng hẹn (33%).
- Đối chiếu trực tiếp với Benchmark của shop: Trung vị CLV shop (3,93tr), AOV TB shop (-46%), Tỷ lệ giao đúng hạn toàn shop (92%).
- Thẻ Nhận định AI tóm tắt tình thế: tiêu đề "Lỗi kép: giao trễ 4 ngày và cối thủy tinh nứt" kèm phân tích mức độ ưu tiên xử lý.

### 1.4. FR-04: Thẻ Cảnh báo Tiền lệ (Context Warning)
- Agent quét lịch sử khiếu nại cũ, phát hiện tiền lệ giao trễ ấm siêu tốc đơn `#10006` từng được đền voucher `XINLOI30K`.
- Cảnh báo người trực cần nhận lỗi triệt để ở tin nhắn đầu tiên; hiển thị các tag clickable `#10006`, `#10032`, `hồ sơ khách`.

### 1.5. FR-05: Biểu đồ Cột 3 Đơn Gần Nhất
- Hiển thị trực quan 3 đơn hàng theo timeline (05 → 09/2026):
  - `#10006`: 420k, trễ 3 ngày (cột vàng).
  - `#10018`: 660k, đúng hẹn (cột xanh cyan).
  - `#10032`: 1,44tr, trễ 4 ngày (cột sọc vàng viền đỏ - đơn đang khiếu nại).

### 1.6. FR-06: 3 Mẫu Phản hồi Soạn sẵn & Nút "↩ Sử dụng"
- Sinh đầy đủ 3 tone phản hồi: Đồng cảm (*Empathetic*), Ngắn gọn (*Concise*), Trang trọng (*Formal*).
- Nút bấm `↩ Sử dụng` đưa toàn bộ văn bản vào ô soạn thảo của người trực, tự động focus con trỏ để chỉnh sửa, không tự động gửi.

### 1.7. FR-07: Live Trace Streaming "Agent đang làm gì"
- Thiết lập kênh Server-Sent Events (SSE) đẩy sự kiện theo thời gian thực:
  - Vòng 1: Thought + gọi song song 4 tools (tìm đơn, tìm sản phẩm, 2 tool tìm case) kèm thanh điểm độ khớp và badge kết quả phụ.
  - Vòng 2: Thought "Đủ dữ liệu" + Action soạn gợi ý từ 16 bản ghi.

### 1.8. FR-08: Minh bạch Tra cứu "Tìm kiếm" (Hybrid Retrieval Breakdown)
- Hiển thị công thức: $\text{score} = 0.3 \times \text{BM25} + 0.7 \times \text{semantic}$.
- Ghim khách hàng `cus_003` ở đầu (`pinned`).
- Hiển thị thanh điểm kép (Dual Score Bar) cho Orders, Products, Cases giải thích mức độ trùng từ khóa vs tương đồng ngữ nghĩa.

### 1.9. FR-09: Thanh Giám sát Hiệu năng & Chi phí Vận hành
- Hiển thị trên top banner: Tên model `DeepSeek v4.1 Flash`, tổng thời gian (~21s), số lượt gọi model (3), số lượt tra cứu (4).
- Đo lường chi phí API thực tế ($0,0016 / lượt chat, $1,6 / 1.000 lượt).

### 1.10. FR-10: Grounding Guard & Anti-Hallucination
- Kiểm tra tính hợp lệ của toàn bộ Order ID, Product ID và Voucher code. Tự động loại bỏ hoặc gắn cảnh báo nếu phát hiện thông tin không có trong DB.

---

## 2. Yêu cầu Phi chức năng (Non-Functional Requirements)

- **NFR-01 (Thời gian Phản hồi & Hiệu năng):**
  - Thời gian xử lý debounce: 1.2s.
  - Tổng thời gian hoàn tất ReAct loop: $\le$ 25 giây (thực tế ~21s).
  - Chi phí API trung bình: $\le$ $0,002 / phiên hỗ trợ.
- **NFR-02 (Độ tin cậy & An toàn Nghiệp vụ):**
  - Tỷ lệ ảo giác mã đơn hàng / chính sách bồi thường: **0%**.
  - 100% phản hồi gửi đến khách hàng phải do con người phê duyệt (Human-in-the-loop).
- **NFR-03 (Khả năng Truy cập & Phím tắt):**
  - Đạt chuẩn **WCAG 2.2 AA** trên giao diện Dark theme.
  - Hỗ trợ thao tác phím tắt 100%: `Tab` duyệt card $\rightarrow$ `Enter` chọn "Sử dụng" $\rightarrow$ Sửa text $\rightarrow$ `Enter` gửi tin.
- **NFR-04 (Môi trường & Cô lập Hệ thống):**
  - Chạy độc lập hoàn toàn trên môi trường cục bộ (Python FastAPI trong `.venv` + SQLite + Vite React), không phụ thuộc SaaS ngoại lai ngoại trừ API LLM DeepSeek.
