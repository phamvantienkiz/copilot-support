# ADR-002: Tích hợp Open-Weights LLM (Colab Pro vLLM) & OpenRouter Free thông qua OpenAI-Compatible Gateway

> **Status:** Accepted  
> **Date:** 2026-10-13  
> **Deciders:** Technical Lead, Solution Architect, Antigravity Agent  
> **Consulted:** System Design Specialist  

---

## 1. Context & Problem Statement (Bối cảnh & Vấn đề)

Trong quá trình nhân bản (clone) và học hỏi dự án mẫu Support Copilot, hệ thống gốc sử dụng API DeepSeek / OpenAI thương mại trả phí. Tuy nhiên, người học mong muốn:
1. **Tiết kiệm tối đa chi phí** (mục tiêu 0 đồng chi phí token API).
2. **Không làm thay đổi kiến trúc chuẩn của dự án mẫu**: Vẫn giữ nguyên 100% các chuẩn thiết kế API, cơ chế ReAct loop, Parallel Tool Calling, Structured JSON Output và Grounding Guard.
3. **Thỏa mãn nhu cầu kiểm thử liên tục**: Có thể chạy hàng nghìn lượt test mà không lo cạn hạn mức (quota) hoặc phát sinh chi phí phát sinh.

Hai giải pháp hạ tầng được đưa ra:
- **Phương án 1 (OpenRouter Free)**: Sử dụng các model mở miễn phí trên OpenRouter (`:free`).
- **Phương án 2 (Google Colab Pro Self-Hosting)**: Host các mô hình mã nguồn mở chất lượng cao (Qwen 2.5 7B/14B Instruct hoặc Gemma 2 9B) trên GPU Colab Pro (T4/L4/A100) qua engine `vLLM` và mở cổng public bằng Cloudflare Tunnel.

---

## 2. Decision (Quyết định Kiến trúc)

1. **Chuẩn hóa Giao thức OpenAI-Compatible làm Single Source of Truth**:
   - Tất cả các LLM Provider (DeepSeek, OpenRouter, Colab vLLM) đều phải giao tiếp qua endpoint chuẩn `/v1/chat/completions` với header `Authorization: Bearer <TOKEN>`.
   - Toàn bộ core logic của ReAct Agent, Tool Execution và Single-Call Assist tuyệt đối không phụ thuộc vào SDK riêng lẻ của từng hãng mà gọi qua một client adapter duy nhất.

2. **Triển khai Mô hình "Multi-Provider LLM Gateway" qua Biến Môi trường**:
   - Cho phép chuyển đổi Provider qua biến cấu hình `LLM_PROVIDER`:
     - `LLM_PROVIDER=colab`: Trỏ tới URL Cloudflare Tunnel của Colab Pro đang host Qwen 2.5 với `vLLM`.
     - `LLM_PROVIDER=openrouter`: Gọi các model miễn phí trên OpenRouter (dùng khi không bật Colab).
     - `LLM_PROVIDER=replay`: Chạy offline 100% dựa trên bản ghi JSON có sẵn trong `public/replay/`.

3. **Lựa chọn Mô hình Open-Weights Trọng tâm: Qwen 2.5 (7B / 14B Instruct)**:
   - Qwen 2.5 được chọn làm mô hình ưu tiên số 1 khi host trên Colab vì khả năng xử lý tiếng Việt vượt trội, hỗ trợ Structured JSON mode và Tool Calling (function calling) có độ chính xác tương đương GPT-4o-mini.
   - Khi chạy `vLLM`, bắt buộc cấu hình tham số `--enable-auto-tool-choice --tool-call-parser hermes` để đảm bảo định dạng `tool_calls` tương thích hoàn toàn với schema của OpenAI.

4. **Tách biệt Độc lập Vector Embedding Engine**:
   - Để tránh phụ thuộc vào API embedding trả phí, hệ thống hỗ trợ tạo embedding offline bằng model CPU siêu nhẹ (`sentence-transformers/all-MiniLM-L6-v2` hoặc `keepitreal/vietnamese-sbert`) hoặc nhúng trực tiếp qua vLLM server trên Colab.

5. **Cơ chế Fallback & Resilience**:
   - Backend thực hiện kiểm tra kết nối nhẹ (`health check`) tới Colab URL trước khi thực hiện phiên ReAct. Nếu Colab bị ngắt kết nối (idle disconnect), hệ thống tự động ghi log cảnh báo và fallback an toàn sang OpenRouter Free hoặc Replay Mode thay vì sập ứng dụng.

---

## 3. Trade-Off Analysis (Đánh giá Đánh đổi)

```
┌────────────────────────┬─────────────────────────────┬─────────────────────────────┐
│ Provider               │ Điểm mạnh                   │ Điểm yếu & Rủi ro           │
├────────────────────────┼─────────────────────────────┼─────────────────────────────┤
│ Google Colab Pro       │ - Không giới hạn số request │ - Runtime bị ngắt sau vài h │
│ (vLLM + Qwen 2.5)      │ - Tốc độ cao (50-100 tok/s) │ - URL Cloudflare Tunnel đổi │
│                        │ - Kiểm soát 100% tham số    │   mỗi lần khởi động lại     │
├────────────────────────┼─────────────────────────────┼─────────────────────────────┤
│ OpenRouter Free        │ - Luôn sẵn sàng, không cần  │ - Bị rate limit khắt khe    │
│                        │   bật máy hay GPU           │ - Queue delay lúc cao điểm  │
│                        │ - Đa dạng model thử nghiệm  │ - Context window bị giới hạn│
├────────────────────────┼─────────────────────────────┼─────────────────────────────┤
│ Replay Engine          │ - Ngoại tuyến 100%, 0 đồng  │ - Chỉ chạy các ca mẫu có sẵn│
│                        │ - 0ms độ trễ mạng           │ - Không sinh câu mới ngoài data│
└────────────────────────┴─────────────────────────────┴─────────────────────────────┘
```

---

## 4. Consequences & Impact (Hệ quả & Ảnh hưởng)

- **Tích cực:**
  - Tiết kiệm 100% chi phí API mà vẫn giữ nguyên trải nghiệm và kiến trúc của dự án mẫu.
  - Người học nắm vững kỹ năng triển khai mô hình mã nguồn mở trên hạ tầng GPU thực tế (`vLLM`, tunneling, OpenAI API server).
  - Codebase đạt chuẩn Enterprise: Hỗ trợ Multi-Model / Multi-Provider mà không cần sửa code ứng dụng.
- **Tiêu cực / Rủi ro & Giải pháp giảm thiểu:**
  - *URL Tunnel thay đổi*: Mỗi lần bật Colab, người dùng chỉ cần cập nhật biến `COLAB_BASE_URL` trong file `.env`.
  - *Bộ nhớ GPU Colab*: Qwen 2.5 7B chỉ chiếm ~14GB VRAM (vừa khít GPU T4 16GB) hoặc dùng bản GPTQ Int4 để chạy mượt mà trên mọi loại GPU của Colab Pro.
