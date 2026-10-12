# Database Migration & Seed Plan — Release 1

> **Status:** Draft  
> **Release:** Release 1  

---

## 1. Schema Init Script (`migrations/001_init.sql`)
```sql
-- Bảng lưu trữ embeddings phục vụ tìm kiếm ngữ nghĩa
CREATE TABLE IF NOT EXISTS embeddings (
    id TEXT PRIMARY KEY,
    doc_type TEXT NOT NULL,       -- 'product' | 'case'
    content TEXT NOT NULL,
    vector BLOB NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX IF NOT EXISTS idx_doc_type ON embeddings(doc_type);
```

## 2. Data Seed Procedure
1. Sao chép 4 tệp dữ liệu mẫu vào `data/`: `customers.json`, `orders.json`, `products.json`, `complaint-cases.json`.
2. Chạy lệnh CLI sinh vector: `uv run python -m cmd.embed` để tạo `data/embeddings.sqlite`.
