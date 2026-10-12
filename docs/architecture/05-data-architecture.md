# 05 - Data Architecture & Schemas

> **Status:** Draft  
> **Layer:** Architecture Layer  
> **Scope:** Cấu trúc dữ liệu của 4 tệp JSON và cơ sở dữ liệu SQLite Vector  

---

## 1. Dữ liệu tĩnh / Bán tĩnh (JSON Files)

### 1.1. `customers.json`
```json
[
  {
    "id": "cust_001",
    "name": "Nguyễn Văn A",
    "username": "nguyenvana",
    "tier": "Silver",
    "registered_at": "2025-09-15",
    "phone": "0901234567"
  }
]
```

### 1.2. `orders.json`
```json
[
  {
    "order_id": "#10032",
    "customer_id": "cust_001",
    "order_date": "2026-09-23",
    "delivery_date": "2026-09-27",
    "status": "delivered",
    "delivery_delay_days": 4,
    "total_amount": 1440000,
    "items": [
      {
        "product_id": "prod_zenka_b600",
        "name": "Máy xay sinh tố Zenka B600",
        "quantity": 1,
        "price": 1440000
      }
    ]
  }
]
```

### 1.3. `products.json`
```json
[
  {
    "product_id": "prod_zenka_b600",
    "name": "Máy xay sinh tố Zenka B600",
    "category": "Thiết bị nhà bếp",
    "price": 1440000,
    "features": ["Cối thủy tinh chịu lực", "Công suất 600W", "Dao 6 lưỡi"],
    "warranty_policy": "Bảo hành 12 tháng, đổi mới 7 ngày nếu lỗi nhà sản xuất"
  }
]
```

### 1.4. `complaint-cases.json`
```json
[
  {
    "case_id": "case_shattered_glass_delivery",
    "title": "Cối thủy tinh máy xay bị nứt vỡ khi nhận hàng",
    "issue_keywords": ["nứt", "vỡ", "bể", "cối thủy tinh", "giao trễ"],
    "policy_action": "Đổi mới miễn phí hai chiều trong 7 ngày, hỗ trợ giao hỏa tốc trước, lấy hàng lỗi sau, tặng voucher 100k nếu kèm giao trễ"
  }
]
```

## 2. Cấu trúc SQLite Vector (`embeddings.sqlite`)
```sql
CREATE TABLE IF NOT EXISTS embeddings (
    id TEXT PRIMARY KEY,
    doc_type TEXT NOT NULL,       -- 'product' | 'case' | 'policy'
    content TEXT NOT NULL,
    vector BLOB NOT NULL,         -- Float32 array binary serialized
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
