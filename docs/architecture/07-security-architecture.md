# 07 - Security Architecture & Data Guardrails

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** An toàn dữ liệu, kiểm soát bảo mật và ranh giới an toàn cho Agent  

---

## 1. Nguyên tắc An toàn Cốt tử
1. **Read-Only Agent Invariant:** Agent tuyệt đối **không có quyền ghi, sửa, xóa** bất kỳ bản ghi nào trong database. Mọi tool đều là hàm đọc dữ liệu (`read-only`).
2. **Human-in-the-Loop Barrier:** Agent không có quyền gửi tin nhắn ra kênh chat của khách hàng. Chỉ người trực sau khi duyệt/sửa mới có thể kích hoạt hành động gửi (`send_message`).
3. **Secret Management:** Toàn bộ API keys (DeepSeek API, OpenAI API) chỉ được nạp từ biến môi trường (`.env`), tuyệt đối không hardcode trong mã nguồn hoặc commit lên Git.
4. **Sanitization & PII Protection:** Số điện thoại, địa chỉ khách hàng được che (mask) khi đưa vào ngữ cảnh hoặc hiển thị trên các màn hình public demo.
