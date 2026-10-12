# 11 - Frontend System Design

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Thiết kế hệ thống Frontend, kiến trúc điều hướng và layout 3 cột  

---

## 1. Kiến trúc Bố cục 3 Cột (Workspace Shell)

Giao diện hỗ trợ 3 chế độ xem (View Modes):
- **Khách hàng (`customer`):** Chỉ hiển thị khung chat mô phỏng người mua hàng.
- **Người trực (`operator`):** Chỉ hiển thị bàn làm việc và cột Copilot.
- **Song song (`dual` / `split`):** Hiển thị đồng thời cả Khách hàng (bên trái), Người trực (ở giữa) và Cột Copilot (bên phải) để kiểm thử và theo dõi phản hồi real-time.

```
┌───────────────────────┬───────────────────────────┬───────────────────────────┐
│ CỘT 1: KHÁCH HÀNG     │ CỘT 2: NGƯỜI TRỰC         │ CỘT 3: COPILOT PANEL      │
│ - Chọn khách hàng mẫu │ - Lịch sử chat shop       │ - Tabs: Agent / Single    │
│ - Khung chat với shop │ - Ô soạn thảo phản hồi    │ - Live trace (vòng, tool) │
│ - Kịch bản khiếu nại  │ - Nút "Gửi"               │ - 3 mẫu gợi ý câu trả lời │
│                       │                           │ - Hồ sơ nhanh khách hàng  │
└───────────────────────┴───────────────────────────┴───────────────────────────┘
```
