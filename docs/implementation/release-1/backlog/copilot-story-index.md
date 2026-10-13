# Backlog Sổ cái: Support Copilot · Release 1 (MVP)

> **Tầng tài liệu:** Implementation Layer  
> **Release:** Release 1 (MVP)  
> **Nguồn tham chiếu:** PRD [docs/implementation/release-1/prd.md](../prd.md)  
> **Sơ đồ luồng:** Technical Design [docs/implementation/release-1/technical-design.md](../technical-design.md)  
> **Ngày phê duyệt:** 2026-10-12  
> **Tổng Story Points:** 55 SP (11 User Stories)  

---

## 1. Ma trận Phân bổ Story & Traceability

| Mã Story | Tiêu đề User Story | Persona | Ưu tiên | PRD FR Ref | Điểm SP | Architecture Guardrails | Trạng thái |
|:---:|:---|:---:|:---:|:---:|:---:|:---|:---:|
| [**US-001**](./stories/US-001.md) | Khởi tạo Mock Data Shop "Nhà Gọn" & Engine BM25 Tiếng Việt | Developer | P0 (Must) | FR-01 | 3 SP | `05-data`, `09-principles`, `10-folder` | Ready for Dev |
| [**US-002**](./stories/US-002.md) | Deterministic Customer Stats kèm Shop Benchmark & Grounding Guard | Support Operator | P0 (Must) | FR-03, FR-10 | 5 SP | `03-service`, `05-data`, `09-principles` | Ready for Dev |
| [**US-003**](./stories/US-003.md) | Single-Call Assist Service & Soạn 3 Mẫu Phản hồi theo Tone | Support Operator | P0 (Must) | FR-06 | 5 SP | `06-ai`, `07-security`, `08-integration` | Ready for Dev |
| [**US-004**](./stories/US-004.md) | SQLite Vector Store & Hybrid Search (0.3 BM25 + 0.7 Semantic) | Developer | P0 (Must) | FR-08 | 5 SP | `05-data`, `06-ai`, `09-principles` | Ready for Dev |
| [**US-005**](./stories/US-005.md) | ReAct Agent Loop với Parallel Tool Calling (4 Tools Đồng thời) | Support Operator | P0 (Must) | FR-07 | 8 SP | `06-ai`, `07-security`, `08-integration` | Ready for Dev |
| [**US-006**](./stories/US-006.md) | Endpoint Server-Sent Events (SSE) Streaming Live Trace Thời gian thực | Support Operator | P0 (Must) | FR-07 | 5 SP | `08-integration`, `11-frontend`, `13-arch` | Ready for Dev |
| [**US-007**](./stories/US-007.md) | Giao diện Workspace Shell 3 Cột & Cột Copilot 4 Chế độ Xem | Support Operator | P0 (Must) | FR-01, FR-06 | 8 SP | `11-frontend`, `12-design`, `14-language` | Ready for Dev |
| [**US-008**](./stories/US-008.md) | Trình Mô phỏng Replay Mode Chạy Trace Ngoại tuyến | Support Operator | P1 (Should) | NFR-04 | 3 SP | `09-principles`, `13-frontend-arch` | Ready for Dev |
| [**US-009**](./stories/US-009.md) | Thẻ Cảnh báo Tiền lệ ("CẦN LƯU Ý") & Biểu đồ Cột 3 Đơn Gần Nhất | Support Operator | P0 (Must) | FR-04, FR-05 | 5 SP | `11-frontend`, `12-design`, `05-data` | Ready for Dev |
| [**US-010**](./stories/US-010.md) | Minh bạch Tra cứu: Bảng Phân rã Điểm kép (Dual Score Bar) & Ghim Khách hàng | Support Operator | P0 (Must) | FR-08 | 5 SP | `06-ai`, `11-frontend`, `12-design` | Ready for Dev |
| [**US-011**](./stories/US-011.md) | Top Banner Giám sát Hiệu năng, Độ trễ & Chi phí Vận hành DeepSeek Flash | Business Owner | P1 (Should) | FR-09 | 3 SP | `11-frontend`, `12-design`, `08-integration` | Ready for Dev |

---

## 2. Quản lý Câu hỏi Mở (Open Questions Log)

| Mã OQ | Vấn đề nghiệp vụ cần làm rõ | Bên phụ trách | Trạng thái | Quyết định đã chốt |
|:---:|:---|:---:|:---:|:---|
| `OQ-01` | Hạn mức voucher đền bù tối đa mà Copilot được phép đề xuất là bao nhiêu? | CSKH Lead / PO | `[x] Resolved` | Tối đa 100.000đ cho sự cố lỗi kép (giao trễ + nứt vỡ); Grounding Guard tự động chặn các đề xuất vượt trần này. |
| `OQ-02` | Khi khách hàng gửi tin nhắn thoại hoặc ảnh chụp cối nứt, Copilot xử lý ra sao trong Release 1? | PO | `[~] Out of scope` | Release 1 chỉ tập trung xử lý tin nhắn văn bản và kịch bản text mô phỏng; tính năng Multimodal hoãn sang Release 2. |
| `OQ-03` | Nút "Sử dụng" có hỗ trợ phím tắt trên bàn phím không? | Lead Frontend | `[x] Resolved` | Hỗ trợ 100%: Phím `Tab` điều hướng tới thẻ gợi ý, nhấn `Enter` hoặc `Space` là text tự động điền vào ô chat và focus con trỏ. |
