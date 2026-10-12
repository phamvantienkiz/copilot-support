# 04 - Deployment & Runtime Architecture

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Môi trường vận hành, cấu hình hạ tầng và cách ly workspace  

---

## 1. Runtime Environment Invariants
- **Backend Runtime:** Python 3.11+, quản lý qua virtual environment cô lập hoàn toàn (`.venv`) sử dụng công cụ `uv`.
- **Frontend Runtime:** Node.js 18+, Vite, package manager pnpm/npm.
- **Database:** Tệp nhúng SQLite cục bộ (`data/embeddings.sqlite`), không cần cài đặt database daemon rời.

## 2. Environment Variables (.env)
```bash
# LLM Settings
DEEPSEEK_API_KEY=your_api_key_here
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
CHAT_MODEL_NAME=deepseek-chat
EMBEDDING_MODEL_NAME=deepseek-embedding

# Server Settings
BACKEND_PORT=8000
BACKEND_HOST=127.0.0.1
CORS_ORIGINS=["http://localhost:5173"]

# Data Paths
DATA_DIR=./data
SQLITE_DB_PATH=./data/embeddings.sqlite
```
