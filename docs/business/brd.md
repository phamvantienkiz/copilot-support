# Business Requirement Document (BRD) — Support Copilot · Nhà Gọn

> **Tầng tài liệu:** Business Layer  
> **Dự án:** Support Copilot · Nhà Gọn (Hệ thống Trợ lý AI CSKH Bán lẻ Thiết bị)  
> **Chủ quản:** Product Owner / Senior Business Analyst  
> **Phiên bản:** 1.0 (Approved)  
> **Ngày cập nhật:** 2026-10-12  

---

## 1. Tóm tắt Điều hành & Tầm nhìn Dự án (Executive Summary & Vision)

### 1.1. Bài toán Nghiệp vụ & Nỗi đau Thực tế (Problem Statement)
Trong ngành bán lẻ thiết bị điện tử gia dụng trực tuyến (như shop "Nhà Gọn"), xử lý khiếu nại sau bán hàng là điểm chạm nhạy cảm nhất quyết định sự trung thành của khách hàng:
1. **Rủi ro chí mạng của Chatbot tự động (Autonomous Chatbot):**  
   Nhiều doanh nghiệp muốn đưa AI chat thẳng với khách hàng để cắt giảm nhân sự. Tuy nhiên, trong các sự cố phức tạp (hàng vỡ, giao trễ, đòi hoàn tiền), chỉ cần AI phát ngôn sai một lần (hứa bồi thường sai quy định, nói sai chính sách, hoặc đưa ra câu trả lời vô cảm khi khách đang giận dữ) là **mất khách vĩnh viễn và tạo khủng hoảng truyền thông**.
   > *"Chạy đúng nhiều lần chưa đủ. Chỉ cần sai một lần là thành thảm họa."*
2. **Quá tải và áp lực của Nhân viên trực CSKH (Support Operator):**  
   Khi khách hàng khiếu nại một "lỗi kép" (ví dụ: vừa giao trễ 4 ngày vừa nứt cối thủy tinh), nhân viên phải mở 3 - 4 màn hình khác nhau (tìm mã đơn, tra thông số máy xay, xem chính sách bảo hành, kiểm tra lịch sử mua). Quá trình này mất 3 - 5 phút khiến khách hàng chờ đợi và bực tức tăng cao.

### 1.2. Tầm nhìn Sản phẩm (Product Vision)
**Support Copilot** được định vị là **Trợ lý AI ngồi cạnh nhân viên CSKH (Human-in-the-Loop)**, đóng vai trò một bộ não phụ tá đắc lực:
- **Tự động trinh sát dữ liệu:** Đọc tin nhắn khách, tự lục hồ sơ khách hàng, tra đơn hàng liên quan, đối chiếu playbook xử lý sự cố trong quá khứ.
- **Tính toán số liệu xác định (Deterministic) kèm Shop Benchmark:** Tự động tính toán tổng chi tiêu (CLV so với trung vị shop 3,93tr), giá trị đơn trung bình (AOV so với TB shop), tần suất mua hàng và tỷ lệ giao đúng hạn (so với chuẩn 92% của shop) mà không để AI bịa số.
- **Phát hiện rủi ro lịch sử (Context Warning):** Tự động nhận diện các sự cố lặp lại từ quá khứ (ví dụ: từng khiếu nại trễ hạn nhận voucher `XINLOI30K`) để nhắc người trực xin lỗi triệt để ngay tin nhắn đầu.
- **Soạn sẵn 3 mẫu phản hồi:** Cung cấp 3 phương án trả lời theo 3 phong cách (*Empathetic*, *Concise*, *Formal*) với nút `↩ Sử dụng` đưa thẳng văn bản vào ô chat.
- **Minh bạch cơ chế tìm kiếm (Hybrid Breakdown):** Hiển thị rõ công thức $\text{score} = 0.3 \times \text{BM25} + 0.7 \times \text{semantic}$ và thanh phân rã điểm kép (Dual Score Bar) giúp người trực hiểu vì sao kết quả được gợi ý.
- **Tối ưu chi phí vượt trội:** Vận hành trên model DeepSeek v4.1 Flash với chi phí chỉ từ **$0,0016 một lượt chat** (~$1,6 cho 1.000 lượt chat), hoàn tất tra cứu và soạn thảo trong ~20 giây.
- **Nhân viên là người kiểm duyệt cuối cùng:** Nhân viên chỉ cần liếc mắt 5 giây, bấm "Sử dụng", tinh chỉnh nếu cần rồi tự tay bấm gửi. AI có sai thì nhân viên phát hiện ngay, khách hàng tuyệt đối không nhìn thấy lỗi của AI.

---

## 2. Mục tiêu Kinh doanh & Chỉ số Thành công (KPIs)

| Chỉ số Đo lường (KPI) | Thực trạng Chưa có Copilot | Mục tiêu khi có Copilot | Phương pháp Đo lường |
| :--- | :--- | :--- | :--- |
| **Thời gian giải quyết (ART - Avg Resolution Time)** | 4 - 6 phút / case | $\le$ 90 - 120 giây / case | Log hệ thống chat từ lúc khách gửi đến khi gửi phản hồi đầu tiên |
| **Tỷ lệ giải quyết lần đầu (FCR - First Contact Resolution)** | ~60% | $\ge$ 85% | Tỷ lệ case khiếu nại đóng ngay sau lượt phản hồi đầu tiên |
| **Tỷ lệ Chấp nhận Gợi ý (Suggestion Adoption Rate)** | 0% | $\ge$ 75% | Tỷ lệ nhân viên bấm nút "Sử dụng" một trong 3 mẫu gợi ý |
| **Rủi ro Ảo giác / Bịa đặt (Hallucination Rate)** | Không áp dụng | **0% sai lệch ID & chính sách** | Bộ lọc Grounding Guard đối chiếu 100% ID với database thực tế |
| **Chi phí vận hành AI (AI Cost per Session)** | Không áp dụng | **$\le$ $0,002 / lượt chat** (~$1,6 / 1k lượt) | Bảng Meter theo dõi DeepSeek API tokens |
| **Chỉ số Hài lòng Khách hàng (CSAT)** | 3.5 / 5 sao | $\ge$ 4.7 / 5 sao | Đánh giá sao của khách hàng sau phiên hỗ trợ |

---

## 3. Ranh giới Phạm vi Dự án (Project Scope)

### 3.1. Phạm vi Thực hiện (In-Scope)
- **Hệ thống dữ liệu giả lập chuẩn của shop "Nhà Gọn":** 4 bảng dữ liệu (Khách hàng, Đơn hàng, Sản phẩm, Playbook case khiếu nại).
- **Hai cơ chế vận hành:** Single-call Assist (nhanh, gọn) và Multi-turn ReAct Agent (tự chủ tra cứu qua 6 tools, stream tiến trình thời gian thực).
- **Công cụ tìm kiếm hỗn hợp (Hybrid Search):** Kết hợp 30% BM25 (từ khóa chính xác) + 70% Semantic Vector Embedding (từ đồng nghĩa, từ lóng như "bể" / "nứt") kèm trực quan hóa phân rã điểm BM25 vs Semantic.
- **Tính toán chỉ số độc lập (Deterministic Stats Engine):** CLV, AOV, tỷ lệ đúng hạn tính bằng code thuần, đối chiếu trực tiếp với Benchmark toàn shop.
- **Giao diện làm việc 3 cột với Cột Copilot gồm 4 chế độ xem:**
  1. *Hồ sơ nhanh:* Nhận định AI, chỉ số kèm benchmark shop, thẻ Cần lưu ý (lỗi lặp lại) và biểu đồ 3 đơn gần nhất.
  2. *Gợi ý trả lời:* 3 mẫu phản hồi (Đồng cảm, Ngắn gọn, Trang trọng) kèm nút bấm "Sử dụng" đưa vào ô chat.
  3. *Agent đang làm gì:* Live trace từng vòng suy luận (thought), công cụ gọi (parallel tools), thời gian và score bar.
  4. *Tìm kiếm:* Bảng kết quả hybrid search, phân loại Khách hàng, Đơn hàng, Sản phẩm kèm cờ ghim (`pinned`) và thanh điểm kép.
- **Chế độ Replay Mode:** Lưu trữ và phát lại các trace mẫu mà không cần kết nối LLM API thật.

### 3.2. Phạm vi Loại trừ (Out-of-Scope)
- **Tuyệt đối không gửi tin nhắn tự động:** Copilot không có quyền tự dispatch tin nhắn ra cổng chat của khách.
- **Không tự động trừ tiền/hoàn tiền cổng thanh toán:** Hệ thống chỉ đưa ra hướng dẫn chính sách, không tích hợp API gateway ngân hàng tự động refund.
- **Không thay thế CRM tổng của doanh nghiệp:** Dự án là module trợ lý (Copilot extension/workspace), không xây dựng lại hệ thống ERP kho bãi hoàn chỉnh.

---

## 4. Chân dung Người dùng & Giá trị Mang lại (User Personas)

### Persona 1: Nhân viên trực CSKH (Support Operator) — *Người dùng Chính*
- **Đặc điểm:** Trực tiếp đối mặt với khách hàng giận dữ, phải xử lý 3 - 5 hội thoại cùng lúc.
- **Nỗi đau:** Mất thời gian tra cứu nhiều màn hình, áp lực gõ phím nhanh dễ sai sót hoặc dùng từ ngữ thiếu thấu cảm.
- **Giá trị nhận được:** Có ngay hồ sơ tóm tắt, cảnh báo lỗi lặp lại, benchmark so sánh và 3 câu trả lời soạn sẵn chuẩn chính sách trong 5 giây; chỉ cần bấm 1 nút để đưa vào ô chat.

### Persona 2: Quản lý Shop / Chủ Doanh nghiệp (Business Owner)
- **Đặc điểm:** Chịu trách nhiệm về doanh thu, tỷ lệ giữ chân khách hàng (Retention) và chi phí vận hành.
- **Nỗi đau:** Sợ nhân viên nói sai làm mất khách; sợ AI tự động gây thảm họa bồi thường vượt ngân sách; sợ chi phí token LLM đội trần.
- **Giá trị nhận được:** Kiểm soát 100% rủi ro nhờ Human-in-the-loop; theo dõi được chi phí token qua bảng Meter (chỉ $1,6/1.000 lượt chat); cải thiện năng suất đội ngũ gấp 2 - 3 lần.

### Persona 3: Khách hàng Mua sắm (Online Shopper)
- **Đặc điểm:** Mua thiết bị gia dụng (máy xay, máy sấy), gặp sự cố vỡ cối hoặc giao trễ, đang trong tâm lý bức xúc.
- **Nỗi đau:** Chờ đợi lâu, gặp bot trả lời vòng vo vô cảm, bị đùn đẩy trách nhiệm cho đơn vị vận chuyển.
- **Giá trị nhận được:** Được nhận phản hồi siêu tốc, được lắng nghe và xin lỗi chân thành, nhận được giải pháp đổi mới tận nhà và voucher đền bù thỏa đáng.

---

## 5. Giả định & Ràng buộc (Assumptions & Constraints)

### 5.1. Giả định (Assumptions)
- Dữ liệu đơn hàng và danh mục sản phẩm của shop có thể được nạp và cập nhật định kỳ vào SQLite.
- Khách hàng giao tiếp chủ yếu bằng tiếng Việt (bao gồm cả tiếng Việt có dấu, không dấu, và từ ngữ địa phương/từ lóng).

### 5.2. Ràng buộc (Constraints)
- **Ràng buộc Chi phí & Token:** Mỗi phiên hỗ trợ của Agent không được vượt quá 6 vòng lặp (`max_turns = 6`), chi phí trung bình không vượt quá $0,002 / lượt.
- **Ràng buộc Môi trường:** Hệ thống phải chạy được độc lập trên môi trường cục bộ (local machine) với SQLite và Python virtual environment cô lập.
