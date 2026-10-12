# Software Requirements Specification (SRS) — Release 1

> **Status:** Draft  
> **Release:** Release 1  

---

## 1. Yêu cầu Chức năng (Functional Requirements)
- **FR-01 (Chat Simulator):** Cho phép người dùng chuyển đổi giữa các khách hàng mẫu và gửi tin nhắn thoại/văn bản khiếu nại.
- **FR-02 (Assist Trigger):** Hệ thống tự động kích hoạt Copilot sau 1.2 giây tính từ tin nhắn cuối của khách (debounce).
- **FR-03 (ReAct Agent Execution):** Agent tự chạy vòng lặp ReAct, gọi các tools phù hợp trong tối đa 6 vòng, không được lặp lại tool với cùng tham số.
- **FR-04 (Suggestion Generation):** Trả về đủ 3 mẫu câu trả lời theo 3 tone: Empathetic, Concise, Formal.
- **FR-05 (Action "Sử dụng"):** Khi người trực bấm "Sử dụng", nội dung gợi ý được điền vào khung soạn thảo để chỉnh sửa.
- **FR-06 (Grounding Filter):** Loại bỏ câu trả lời nếu chứa Order ID không có trong cơ sở dữ liệu.

## 2. Yêu cầu Phi chức năng (Non-Functional Requirements)
- **NFR-01 (Latency):** Thời gian phản hồi của Single-call assist < 3 giây. Vòng lặp ReAct hoàn thành trong < 25 giây.
- **NFR-02 (Reliability):** 100% các ID xuất hiện trong gợi ý phải được kiểm chứng tồn tại trong database.
- **NFR-03 (Portability):** Chạy được hoàn toàn trên máy tính cá nhân qua SQLite và môi trường `uv`.
