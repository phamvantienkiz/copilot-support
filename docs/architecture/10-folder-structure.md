# 10 - Folder Structure Standards & Module Boundaries

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Quy chuẩn cấu trúc thư mục toàn dự án cho Backend (Python), Frontend (React) và Data  

---

## 1. Master Repository Layout

```
copilot-support/
├── backend/                         # Backend Python (FastAPI)
│   ├── app/
│   │   ├── api/                     # REST & SSE Endpoints
│   │   ├── core/                    # Settings & App config
│   │   ├── domain/                  # Pydantic schemas & Data models
│   │   ├── services/
│   │   │   ├── agent/               # ReAct loop, 6 tools, prompt templates
│   │   │   ├── assist/              # Single-call assist & grounding guard
│   │   │   ├── customerstats/       # Deterministic calculator
│   │   │   ├── llm/                 # Client, JSON mode, meter
│   │   │   └── retrieval/           # BM25, Tokenizer, Hybrid search
│   │   └── storage/                 # SQLite connection & JSON data loaders
│   ├── cmd/                         # CLI entrypoints (embed.py, main.py)
│   ├── pyproject.toml               # Quản lý dependencies qua uv
│   └── .venv/                       # Môi trường ảo Python cô lập
│
├── frontend/                        # Frontend React + TypeScript + Vite
│   ├── src/
│   │   ├── components/
│   │   │   ├── chat/                # Khung chat khách & shop
│   │   │   ├── copilot/             # Cột Copilot, timeline trace, gợi ý
│   │   │   ├── customer/            # Hồ sơ nhanh & metrics
│   │   │   ├── operator/            # Bàn làm việc người trực
│   │   │   ├── shell/               # Layout 3 cột (Song song)
│   │   │   └── ui/                  # Reusable UI components
│   │   └── public/replay/           # Bản ghi trace để demo offline
│   └── package.json
│
├── data/                            # Datasets shop "Nhà Gọn"
│   ├── customers.json
│   ├── orders.json
│   ├── products.json
│   ├── complaint-cases.json
│   └── embeddings.sqlite
│
└── docs/                            # Hệ thống Tài liệu 3 Tầng
    ├── business/
    ├── architecture/
    ├── implementation/
    └── views/
```
