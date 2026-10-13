# Hướng Dẫn Host Qwen 2.5 Trên Google Colab Pro Với vLLM & Cloudflare Tunnel

> **Mục tiêu:** Cung cấp hạ tầng GPU mạnh mẽ (L4 / A100 / T4) để host mô hình open-weights chất lượng cao (`Qwen/Qwen2.5-7B-Instruct`), mở cổng API chuẩn OpenAI tương thích 100% với dự án Support Copilot với **0 đồng chi phí token**.

---

## 1. Kiến trúc Kết nối (Topology)

```
┌─────────────────────────────────┐                 ┌──────────────────────────────────┐
│      GOOGLE COLAB PRO (GPU)     │                 │     LOCAL MACHINE (Máy Của Bạn)  │
│                                 │                 │                                  │
│  [ Qwen 2.5 7B / 14B Model ]   │                 │   [ Copilot FastAPI Backend ]    │
│               │                 │                 │                │                 │
│               ▼                 │                 │                ▼                 │
│    [ vLLM OpenAI Server ]       │                 │      [ LLM Gateway Adapter ]     │
│        (Port 8000)              │                 │                ▲                 │
│               │                 │                 │                │                 │
│               ▼                 │                 │                │                 │
│   [ Cloudflared Tunnel ]        │ ── Public URL ──┼────────────────┘                 │
│  (https://xxx.trycloudflare.com)│  (Internet)     │   (Đọc biến COLAB_BASE_URL)      │
└─────────────────────────────────┘                 └──────────────────────────────────┘
```

---

## 2. Mã Nguồn Notebook Google Colab (1-Click Run)

Tạo một Google Colab Notebook mới, chọn **Runtime $\rightarrow$ Change runtime type $\rightarrow$ T4 GPU** (hoặc L4 / A100 nếu dùng Colab Pro), và chạy lần lượt các ô lệnh sau:

### Ô 1: Cài đặt vLLM và Cloudflared Tunnel
```python
# Cài đặt vLLM tốc độ cao
!pip install -q vllm

# Cài đặt Cloudflared để mở public URL mà không cần đăng ký tài khoản
!wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
!dpkg -i cloudflared-linux-amd64.deb
```

### Ô 2: Khởi động vLLM Server với Tool Calling Parser
> ⚠️ **Lưu ý đặc biệt:** Bắt buộc phải có tham số `--enable-auto-tool-choice` và `--tool-call-parser hermes` để vLLM hiểu format `tools` của OpenAI và hỗ trợ ReAct Agent hoàn hảo!

```python
import subprocess
import time

# Model khuyến nghị: Qwen/Qwen2.5-7B-Instruct (Rất mạnh tiếng Việt và Tool calling)
MODEL_NAME = "Qwen/Qwen2.5-7B-Instruct"

vllm_cmd = f"""
python3 -m vllm.entrypoints.openai.api_server \
    --model {MODEL_NAME} \
    --gpu-memory-utilization 0.90 \
    --max-model-len 8192 \
    --enable-auto-tool-choice \
    --tool-call-parser hermes \
    --port 8000
"""

print("Đang khởi động vLLM server... Quá trình tải model mất khoảng 2-3 phút.")
server_process = subprocess.Popen(vllm_cmd, shell=True)
time.sleep(15)
```

### Ô 3: Mở Cloudflare Tunnel và Lấy Public URL
```python
import re

# Khởi chạy tunnel trỏ vào port 8000 của vLLM
tunnel_process = subprocess.Popen(
    ["cloudflared", "tunnel", "--url", "http://localhost:8000"],
    stdout=subprocess.PIPE,
    stderr=subprocess.STDOUT,
    text=True
)

public_url = None
print("Đang tạo đường hầm kết nối...")

for line in iter(tunnel_process.stdout.readline, ""):
    match = re.search(r"https://[a-zA-Z0-9-]+\.trycloudflare\.com", line)
    if match:
        public_url = match.group(0)
        break

print("\n" + "="*70)
print("🚀 VLLM OPENAI ENDPOINT CỦA BẠN ĐÃ SẴN SÀNG!")
print(f"URL: {public_url}/v1")
print("="*70)
print(f"\n👉 Hãy copy dòng sau dán vào file .env của dự án local:")
print(f"LLM_PROVIDER=colab")
print(f"COLAB_BASE_URL={public_url}/v1")
print(f"COLAB_MODEL={MODEL_NAME}")
print(f"COLAB_API_KEY=colab-local-secret")
```

---

## 3. Cấu Hình Dự Án Tại Máy Local

Trong file `.env` ở thư mục gốc của dự án `copilot-support`:
```ini
# Kích hoạt chế độ Colab
LLM_PROVIDER=colab

# Dán URL lấy được từ Ô 3 ở trên
COLAB_BASE_URL=https://your-random-subdomain.trycloudflare.com/v1
COLAB_API_KEY=colab-local-secret
COLAB_MODEL=Qwen/Qwen2.5-7B-Instruct
```

Khởi động backend FastAPI:
```bash
uv run uvicorn app.main:app --reload
```

---

## 4. Xử Lý Các Sự Cố Thường Gặp (Troubleshooting)

1. **Colab bị ngắt kết nối khi không dùng (Idle Disconnect):**
   - *Nguyên nhân:* Colab tự ngắt sau 90 phút nếu không có thao tác chuột.
   - *Cách xử lý:* Giữ tab Colab mở; hoặc khi Colab tắt, chỉ cần bấm chạy lại Ô 2 & 3 và cập nhật lại biến `COLAB_BASE_URL` trong `.env`.
2. **GPU Out of Memory (OOM):**
   - *Nguyên nhân:* Nếu dùng GPU T4 (16GB VRAM) với model 14B sẽ bị tràn bộ nhớ.
   - *Khắc phục:* Với GPU T4, dùng model `Qwen/Qwen2.5-7B-Instruct` hoặc bản lượng tử hóa `Qwen/Qwen2.5-14B-Instruct-GPTQ-Int4`. Với Colab Pro chọn GPU A100 hoặc L4 thì chạy bản 14B gốc hoàn toàn mượt mà.
3. **Kiểm tra nhanh kết nối bằng curl từ máy local:**
   ```bash
   curl https://<your-subdomain>.trycloudflare.com/v1/models
   ```
   Nếu trả về danh sách model JSON là kết nối thành công 100%!
