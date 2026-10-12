# 09 - Design Principles & Code Standards

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Các nguyên tắc thiết kế mã nguồn, tiêu chuẩn Clean Code và quy tắc kỹ thuật bất biến  

---

## 1. Nguyên tắc Cốt lõi

### 1.1. No-Framework Minimalism
- Tự viết ReAct loop và thuật toán tìm kiếm BM25.
- Không đưa vào các thư viện trừu tượng hóa quá mức (LangChain, LlamaIndex, AutoGen) để duy trì khả năng kiểm soát 100% từng dòng prompt và payload.

### 1.2. Tách biệt Deterministic Data khỏi Suy luận LLM
- Mọi con số tài chính và thống kê hành vi (CLV, AOV, số lần giao trễ, tỉ lệ đúng hạn) **BẮT BUỘC** phải do code thuần Python (`calculator.py`) tính toán.
- LLM chỉ được dùng để diễn giải ngữ nghĩa, thấu cảm và soạn văn phong giao tiếp.

### 1.3. Thử trước - Kiểm tra sau (Grounding by Verification)
- Không bao giờ tin tưởng mù quáng vào output của LLM.
- Mọi thực thể (ID đơn, mã sản phẩm, điều khoản chính sách) trong câu trả lời đều phải qua bộ kiểm tra đối chiếu (Grounding Guard) trước khi xuất ra UI.

### 1.4. Clean Architecture & Type Safety
- Toàn bộ dữ liệu vào/ra các tầng đều sử dụng Pydantic v2 schemas.
- Đảm bảo type hints đầy đủ (PEP 484).
- Phân tầng nghiêm ngặt: `api` $\rightarrow$ `services` $\rightarrow$ `storage`/`domain`.
