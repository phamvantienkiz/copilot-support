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
    "id": "cus_003",
    "name": "Nguyễn Văn A",
    "tier": "Silver",
    "location": "Đà Nẵng",
    "registered_at": "2025-08-10",
    "phone": "0901234567",
    "complaint_history": [
      {
        "order_id": "#10006",
        "issue": "Ấm siêu tốc giao trễ 3 ngày",
        "compensation_voucher": "XINLOI30K",
        "status": "resolved"
      }
    ]
  }
]
```

### 1.2. `orders.json`
```json
[
  {
    "order_id": "#10006",
    "customer_id": "cus_003",
    "order_date": "2026-05-06",
    "delivery_date": "2026-05-09",
    "status": "delivered",
    "is_on_time": false,
    "delivery_delay_days": 3,
    "total_amount": 420000,
    "items": [
      {
        "product_id": "prd_mavio_k17",
        "name": "Ấm siêu tốc inox Mavio K17 1.7L",
        "quantity": 1,
        "price": 420000
      }
    ]
  },
  {
    "order_id": "#10018",
    "customer_id": "cus_003",
    "order_date": "2026-07-15",
    "delivery_date": "2026-07-18",
    "status": "delivered",
    "is_on_time": true,
    "delivery_delay_days": 0,
    "total_amount": 660000,
    "items": [
      {
        "product_id": "prd_nordi_hd18",
        "name": "Máy sấy tóc ion Nordi HD18 1800W",
        "quantity": 1,
        "price": 660000
      }
    ]
  },
  {
    "order_id": "#10032",
    "customer_id": "cus_003",
    "order_date": "2026-09-23",
    "delivery_date": "2026-09-27",
    "status": "complaint_pending",
    "is_on_time": false,
    "delivery_delay_days": 4,
    "total_amount": 1440000,
    "items": [
      {
        "product_id": "prd_001",
        "name": "Máy xay sinh tố Zenka B600 cối thủy tinh",
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
    "product_id": "prd_001",
    "name": "Máy xay sinh tố Zenka B600 cối thủy tinh",
    "category": "Thiết bị nhà bếp",
    "price": 1440000,
    "features": ["Cối thủy tinh chịu lực", "Công suất 600W", "Dao 6 lưỡi titan"],
    "warranty_policy": "Bảo hành 12 tháng, đổi mới 7 ngày hỏa tốc nếu lỗi nứt vỡ khi nhận"
  },
  {
    "product_id": "prd_010",
    "name": "Hộp cơm giữ nhiệt Coral Home 3 tầng",
    "category": "Gia dụng nhà bếp",
    "price": 450000,
    "features": ["Inox 304", "Giữ nhiệt 8 tiếng"],
    "warranty_policy": "Bảo hành 6 tháng"
  },
  {
    "product_id": "prd_016",
    "name": "Cây lau nhà xoay 360 độ kèm thùng vắt",
    "category": "Vệ sinh nhà cửa",
    "price": 380000,
    "features": ["Bông lau microfiber", "Mâm xoay ly tâm"],
    "warranty_policy": "Bảo hành 3 tháng"
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
    "policy_action": "Đổi mới hỏa tốc 2 chiều trong 7 ngày, miễn phí hoàn toàn, không bắt khách chờ thẩm định; tặng voucher 100.000đ nếu kèm giao trễ"
  },
  {
    "case_id": "case_repeated_delay",
    "title": "Hàng vừa giao trễ vừa phát sinh lỗi sản phẩm (giao trễ nhiều lần)",
    "issue_keywords": ["giao trễ nhiều lần", "lần trước cũng trễ", "mất niềm tin"],
    "policy_action": "Nhận lỗi của shop ngay từ câu đầu, không đổ lỗi bên vận chuyển; ưu tiên giao hàng đổi mới trước, lấy hàng lỗi sau; gửi voucher bồi thường"
  }
]
```

---

## 2. Dữ liệu Tham chiếu Ngưỡng chung Toàn Shop (`shop_benchmarks.json`)
```json
{
  "shop_id": "nha_gon_flagship",
  "clv_median": 3930000,
  "aov_mean": 1555000,
  "on_time_rate": 0.92,
  "standard_delay_threshold_days": 1
}
```

---

## 3. Cấu trúc SQLite Vector & Retrieval Store (`embeddings.sqlite`)
```sql
CREATE TABLE IF NOT EXISTS embeddings (
    id TEXT PRIMARY KEY,
    doc_type TEXT NOT NULL,       -- 'customer' | 'order' | 'product' | 'case'
    entity_code TEXT NOT NULL,    -- 'cus_003' | 'ord_10032' | 'prd_001' | ...
    content TEXT NOT NULL,
    vector BLOB NOT NULL,         -- Float32 array binary serialized (Cosine similarity)
    is_pinned BOOLEAN DEFAULT 0,  -- 1 nếu là khách hàng đang active
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
