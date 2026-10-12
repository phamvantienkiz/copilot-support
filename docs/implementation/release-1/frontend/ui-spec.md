# Frontend Screen Specification — Release 1: Copilot Support

> **Status:** Draft  
> **Release:** Release 1  

---

## 1. Màn hình Workspace (3 Cột)

```
┌─────────────────────────┬──────────────────────────┬─────────────────────────┐
│ Cột Khách hàng (Trái)   │ Cột Người trực (Giữa)    │ Cột Copilot (Phải)      │
│ - Selector: Nguyễn Văn A│ - Header: Info khách hàng│ - Mode Tabs: Agent/Sing.│
│ - Chat stream Nhà Gọn   │ - Khung chat với khách   │ - Live Trace timeline   │
│ - Nút Kịch bản mẫu      │ - Ô nhập liệu trả lời    │ - Card Nhận định tình thế│
│ - Input gửi tin nhắn    │ - Nút "Gửi"              │ - 3 Card Gợi ý phản hồi │
└─────────────────────────┴──────────────────────────┴─────────────────────────┘
```

## 2. Tương tác Người dùng (User Interactions)
1. **Bấm "Kịch bản mẫu":** Nạp sẵn chuỗi tin nhắn khiếu nại (VD: máy xay bể cối + giao trễ).
2. **Kích hoạt tự động:** Sau 1.2s khách dừng gõ, Cột Copilot chuyển sang trạng thái "Đang tra cứu" và hiện tiến độ từng vòng.
3. **Bấm "Sử dụng" tại Suggestion Card:** Copy text của mẫu được chọn vào ô nhập liệu ở Cột Người trực, cho phép sửa trước khi gửi.
