# QA Test Plan & Acceptance Test Cases — Release 1

> **Status:** Draft  
> **Release:** Release 1  

---

## 1. Test Cases Cốt lõi (Dựa trên 24 Bài học Thực chiến)

| Test ID | Tên kịch bản kiểm thử | Dữ liệu đầu vào | Kết quả kỳ vọng |
| :---: | :--- | :--- | :--- |
| **TC-01** | Khách nhắn không dấu | `"don may xay zenka cua minh giao tre"` | BM25 và Agent tìm chính xác đơn #10032 và sản phẩm Zenka B600. |
| **TC-02** | Khách dùng từ lóng "bể" | `"cối máy xay bị bể rồi"` | Hybrid Search tìm được case "cối thủy tinh nứt vỡ khi nhận hàng". |
| **TC-03** | Khách than phiền giao trễ nhiều lần | Khách có 2 đơn trễ trên tổng 3 đơn | Chỉ số giao đúng hẹn hiển thị đúng 33%, gợi ý thừa nhận chính xác số lần trễ. |
| **TC-04** | Grounding Guard chống bịa đơn | LLM thử sinh ra mã đơn `#99999` không có thật | Server phát hiện và loại bỏ mã đơn sai khỏi gợi ý. |
| **TC-05** | Giới hạn vòng lặp ReAct | Tình huống phức tạp hoặc truy vấn không rõ ràng | Agent dừng đúng lúc sau tối đa 6 vòng lặp, không bị loop vô tận. |
| **TC-06** | Hoạt động của nút "Sử dụng" | Bấm nút "Sử dụng" tại mẫu Empathetic | Toàn bộ text được điền vào ô chat của người trực, không tự động gửi cho khách. |
