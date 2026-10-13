# Frontend Screen Specification — Release 1: Copilot Support MVP

> **Status:** Active (Synchronized with 4 Real-app UI Screenshots)  
> **Release:** Release 1 (MVP)  
> **Layer:** Implementation Layer  

---

## 1. Màn hình Workspace Shell (3 Cột & Top Banner)

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ TOP BANNER: Lần chạy này: DeepSeek v4.1 Flash · 21,0 giây · 3 lần gọi model · 4 lần    │
│ Chi phí khi gọi thẳng API DeepSeek: từ $0,0016 một lượt, 1.000 lượt chat từ $1,6.      │
├─────────────────────────┬──────────────────────────┬───────────────────────────────────┤
│ CỘT 1: KHÁCH HÀNG (Trái)│ CỘT 2: NGƯỜI TRỰC (Giữa) │ CỘT 3: COPILOT PANEL (Phải)       │
│ - Selector: cus_003     │ - Header: Nguyễn Văn A   │ [Menu 4 thẻ dọc bên trái drawer]  │
│ - Stream hội thoại Nhà  │ - Lịch sử chat thời gian │ 1. Hồ sơ nhanh (Active)           │
│   Gọn                   │   thực (15:22)           │ 2. Gợi ý trả lời                  │
│ - Nút Kịch bản mẫu      │ - Ô soạn thảo phản hồi   │ 3. Agent đang làm gì              │
│ - Input gửi tin nhắn    │ - Nút "Gửi"              │ 4. Tìm kiếm                       │
└─────────────────────────┴──────────────────────────┴───────────────────────────────────┘
```

---

## 2. Đặc tả Chi tiết 4 Màn hình / Chế độ Xem của Cột Copilot

Cột Copilot được trang bị menu điều hướng 4 thẻ dọc bên trái (nhãn: *"Ảnh chụp app thật - Bấm từng mục để xem"*):

### Màn hình 1: Hồ sơ nhanh (`view = profile`)
1. **Thẻ Thông tin Khách hàng:**
   - Avatar chữ viết tắt (`VA`), Tên `Nguyễn Văn A`.
   - Huy hiệu hạng: `Silver`, Khu vực: `Đà Nẵng`, Thời gian: `khách từ 08/2025`.
   - Nút thu gọn / đóng (`x`).
2. **Thẻ Nhận định AI (`NHẬN ĐỊNH AI`):**
   - Icon sparkles, Tiêu đề: *"Lỗi kép: giao trễ 4 ngày và cối thủy tinh nứt"*.
   - Nội dung nhận định: *"Khách đã có 2 đơn giao trễ trên 3 đơn, tỉ lệ đúng hẹn chỉ 33% so với mức 92% của shop, và đây là lần giao trễ thứ hai liên tiếp anh phải chịu. Cối thủy tinh nứt khi nhận là lỗi shop phải xử lý ngay, không để anh phải chờ kiểm tra."*
3. **Lưới 4 Chỉ số Tài chính (kèm Shop Benchmark):**
   - `CLV lịch sử`: **2,52 tr đ** — So sánh: `↘ Dưới trung vị 3,93tr`.
   - `AOV`: **840 k đ** — So sánh: `↘ Thấp hơn TB shop 46%`.
   - `Tần suất mua`: **3 đơn / 13 tháng** — Nhận định: `🔄 Đều, ~2 tháng một đơn`.
   - `Giao đúng hẹn`: **33 %** — Cảnh báo: `⚠ Shop đạt 92%`.
4. **Thẻ Cảnh báo Tiền lệ (`CẦN LƯU Ý` - Badge `AI · từ context`):**
   - Icon cảnh báo tam giác màu cam đỏ.
   - Tiêu đề: *"Đây là lần giao trễ thứ hai"*.
   - Nội dung: *"Anh đã từng khiếu nại vì ấm siêu tốc giao trễ 3 ngày và nhận voucher xin lỗi XINLOI30K. Lặp lại lỗi cũ dễ khiến anh mất niềm tin, nên cần xin lỗi rõ ràng cả hai lỗi ngay từ tin nhắn đầu."*
   - Tag chips liên kết: `#10006`, `#10032`, `hồ sơ khách` (cho phép bấm để tra cứu nhanh).
5. **Biểu đồ "3 ĐƠN GẦN NHẤT" (Timeline `05 → 09/2026`):**
   - Bar 1 (`#10006 · 09/05`): Ấm Mavio K17, `420k`, note `trễ 3 ngày` (màu vàng).
   - Bar 2 (`#10018 · 18/07`): Máy sấy Nordi, `660k`, note `đúng hẹn` (màu xanh cyan).
   - Bar 3 (`#10032 · 27/09`): Máy xay Zenka, `1,44tr`, note `trễ 4 ngày` (vàng sọc viền đỏ - Đơn đang khiếu nại).
   - Chú thích (Legend): Đúng hẹn (Cyan) | Trễ hẹn (Yellow) | Đơn đang khiếu nại (Red Outline).

---

### Màn hình 2: Gợi ý trả lời (`view = suggestions`)
1. **Header Panel:** Icon tin nhắn, Tiêu đề `Gợi ý trả lời`, Badge `3 mẫu`.
2. **Mẫu 1: Đồng cảm (`Empathetic`):**
   - Icon trái tim, Nút Copy, Nút bấm nổi bật `↩ Sử dụng` (màu xanh neon `#84cc16`).
   - Văn bản: Nhận lỗi của shop, không đổ lỗi bên vận chuyển, dặn không dùng cối nứt để tránh nguy hiểm, gửi ảnh cối + tem, shop gửi máy mới hỏa tốc trước và lấy hàng lỗi tận nhà không mất phí, tặng voucher 100.000đ.
3. **Mẫu 2: Ngắn gọn (`Concise`):**
   - Icon tia sét, Nút Copy, Nút `↩ Sử dụng`.
   - Văn bản: Nêu rõ mã đơn `#10032`, đổi mới trong 7 ngày, giao hỏa tốc miễn phí tận nơi, voucher 100k, tùy chọn hoàn tiền 24h nếu muốn trả hàng.
4. **Mẫu 3: Trang trọng (`Formal`):**
   - Icon cà vạt/tài liệu, Nút Copy, Nút `↩ Sử dụng`.
   - Văn bản chuẩn mực doanh nghiệp, nêu quy trình xác minh và bồi thường chính thức.

---

### Màn hình 3: Agent đang làm gì (`view = trace`)
1. **Header:** Icon branch, trạng thái *"Agent đang soạn gợi ý..."*, đồng hồ đo thời gian `14 giây`.
2. **Vòng 1/6 (1,4 giây · 1,8k token):**
   - Thought: *"Tôi sẽ tra đơn máy xay của khách, sản phẩm liên quan, và các case đã xử lý cho cả vấn đề giao trễ lẫn hàng nứt."*
   - Thẻ Tool 1: `Tìm đơn "máy xay sinh tố giao..."` (1,2s · bar 0,96 · `> +2`).
   - Thẻ Tool 2: `Tìm sản phẩm "máy xay Zenk..."` (1,0s · bar 1,00 · `> +4`).
   - Thẻ Tool 3: `Tìm case "giao trễ nhiều lần"` (1,0s · bar 0,95 · `> +3`).
   - Thẻ Tool 4: `Tìm case "cối thủy tinh nứt khi..."` (2,4s · bar 1,00 · `> +3`).
3. **Vòng 2/6 (483 ms · 7k token):**
   - Thought: *"Đủ dữ liệu, không gọi thêm tool."*
   - Thẻ Action: *"Soạn gợi ý từ 16 bản ghi"* (icon bút xanh, spinner `C`, 4,1 giây).

---

### Màn hình 4: Tìm kiếm (`view = retrieval`)
1. **Header:** Icon scan, `Retrieval hybrid`, Badge `12 hits · 1 pinned`, toggle thu gọn `^`.
2. **Công thức trực quan:** $\text{score} = 0,3 \times \text{BM25 (từ khóa)} + 0,7 \times \text{semantic (ý nghĩa)}$ kèm 2 mẫu màu chú thích.
3. **Thực thể được ghim:** Khách hàng `cus_003 - Nguyễn Văn A` có badge xanh lá `pinned`.
4. **Phân loại kết quả kèm Dual Score Bar:**
   - **Orders (3):**
     - `ord_10032`: `Máy xay sinh tố Zenka B600 cối t...` (BM25: 1,00, Semantic: 0,92 $\rightarrow$ Tổng: 0,94).
     - `ord_10018`: `Máy sấy tóc ion Nordi HD18 1800W` (BM25: 0,29, Semantic: 0,79 $\rightarrow$ Tổng: 0,64).
     - `ord_10006`: `Ấm siêu tốc inox Mavio K17 1.7L` (BM25: 0,23, Semantic: 0,56 $\rightarrow$ Tổng: 0,46).
   - **Products (5):**
     - `prd_001`: `Máy xay sinh tố Zenka B600 cối t...` (BM25: 1,00, Semantic: 1,00 $\rightarrow$ Tổng: 1,00).
     - `prd_010`: `Hộp cơm giữ nhiệt Coral Home 3 ...` (BM25: 0,16, Semantic: 0,64 $\rightarrow$ Tổng: 0,49).
     - `prd_016`: `Cây lau nhà xoay 360 độ kèm th...` (BM25: 0,07, Semantic: 0,65 $\rightarrow$ Tổng: 0,47).

---

## 3. Tương tác Người dùng Chuẩn mực (User Interaction Flows)

1. **Khách nhắn tin $\rightarrow$ Debounce 1.2s $\rightarrow$ Copilot chạy tự động:**
   - Thanh top banner bắt đầu đếm thời gian. Cột Copilot tự chuyển sang chế độ `Agent đang làm gì` hoặc hiện spinner.
2. **Sau khi Agent tổng hợp xong (~21s):**
   - Tab tự động chuyển sang `Gợi ý trả lời` hoặc `Hồ sơ nhanh`.
   - Người trực xem xét nhận định và cảnh báo "CẦN LƯU Ý".
3. **Bấm nút `↩ Sử dụng`:**
   - Nội dung câu gợi ý được điền tức thì vào ô soạn thảo ở Cột Người trực.
   - Con trỏ chuột tự động focus vào cuối đoạn văn bản để người trực thêm bớt lời chào hoặc điều chỉnh tên.
   - **Quy tắc an toàn:** Nút `↩ Sử dụng` không bao giờ tự ý kích hoạt hành động gửi tin nhắn. Người trực bắt buộc phải chủ động bấm nút "Gửi" hoặc nhấn phím `Enter`.
