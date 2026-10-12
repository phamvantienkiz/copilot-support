# BÁO CÁO PHÂN TÍCH & BẢN THIẾT KẾ CLONE DỰ ÁN "SUPPORT COPILOT"
> **Dự án gốc**: Support Copilot (200Lab Mini Solutions - Tác giả: Việt Trần)  
> **Mục tiêu**: Phân tích toàn diện kiến trúc, nghiệp vụ, thuật toán và lộ trình để clone lại 85 - 95% dự án sang hệ sinh thái **Python (FastAPI) + React/TypeScript**.  
> **Ngày lập**: 12/10/2026  
> **Người thực hiện**: Antigravity Pair-Programming Agent

---

## MỤC LỤC
1. [TỔNG QUAN DỰ ÁN & BỐI CẢNH KINH DOANH](#1-tổng-quan-dự-án--bối-cảnh-kinh-doanh)
2. [TRIẾT LÝ THIẾT KẾ: VÌ SAO LÀ COPILOT MÀ KHÔNG PHẢI AUTONOMOUS AGENT?](#2-triết-lý-thiết-kế-vì-sao-là-copilot-mà-không-phải-autonomous-agent)
3. [LỘ TRÌNH PHÁT TRIỂN 2 GIAI ĐOẠN (EVOLUTION PHASES)](#3-lộ-trình-phát-triển-2-giai-đoạn-evolution-phases)
4. [QUY TRÌNH HOẠT ĐỘNG 7 BƯỚC (7-STEP END-TO-END FLOW)](#4-quy-trình-hoạt-động-7-bước-7-step-end-to-end-flow)
5. [CÁC TRỤ CỘT KỸ THUẬT CỐT LÕI (CORE TECHNICAL INNOVATIONS)](#5-các-trụ-cột-kỹ-thuật-cốt-lõi-core-technical-innovations)
6. [BÓC TÁCH CHI TIẾT CẤU TRÚC FOLDER GỐC (GO BACKEND & REACT FRONTEND)](#6-bóc-tách-chi-tiết-cấu-trúc-folder-gốc-go-backend--react-frontend)
7. [BẢN THIẾT KẾ CHUYỂN ĐỔI SANG PYTHON (PYTHON CLONE BLUEPRINT)](#7-bản-thiết-kế-chuyển-đổi-sang-python-python-clone-blueprint)
8. [GIẢI MÃ BỘ DỮ LIỆU & 6 READ-ONLY TOOLS](#8-giải-mã-bộ-dữ-liệu--6-read-only-tools)
9. [BẢNG ÁNH XẠ SYLLABUS: 24 TÌNH HUỐNG THỰC CHIẾN CẦN VƯỢT QUA](#9-bảng-ánh-xạ-syllabus-24-tình-huống-thực-chiến-cần-vượt-qua)
10. [LỘ TRÌNH THỰC THI CLONE (STEP-BY-STEP IMPLEMENTATION ROADMAP)](#10-lộ-trình-thực-thi-clone-step-by-step-implementation-roadmap)

---

## 1. TỔNG QUAN DỰ ÁN & BỐI CẢNH KINH DOANH

### 1.1. Xuất thân dự án
Dự án **Support Copilot** xuất phát từ một bài toán thực tế: PoC/MVP được thiết kế cho một doanh nghiệp bán lẻ thiết bị lớn tại Việt Nam bởi anh **Việt Trần** (Founder 200Lab, cựu Solution Architect tại Sendo, tác giả nhiều dự án AI mã nguồn mở nổi bật). 

Toàn bộ dữ liệu của dự án được đóng gói lại thành một case study mẫu giả lập tên là shop **"Nhà Gọn"** (chuyên kinh doanh thiết bị gia dụng: máy xay sinh tố, máy sấy tóc, robot hút bụi,...).

### 1.2. Mục tiêu kỹ thuật & Định vị sản phẩm
- **Tech Stack gốc**: Go (Backend) + React / Vite / TypeScript (Frontend) + SQLite (Vector & Document Storage) + DeepSeek API (LLM & Embeddings).
- **Mục tiêu của chúng ta**: Clone lại hệ thống đạt **85 - 90%** độ trung thực so với bản gốc, chuyển dịch toàn bộ Backend sang **Python (FastAPI, Pydantic v2, Python native ReAct & BM25)**, duy trì nguyên vẹn triết lý thiết kế tối giản, minh bạch và không phụ thuộc vào các framework AI cồng kềnh (No-LangChain, No-LlamaIndex).

---

## 2. TRIẾT LÝ THIẾT KẾ: VÌ SAO LÀ COPILOT MÀ KHÔNG PHẢI AUTONOMOUS AGENT?

> *"Chạy đúng nhiều lần chưa đủ. Chỉ cần sai một lần là thành thảm họa."*

```
              ┌──────────────────────────────────────────────┐
              │  Mô hình Sai lầm: Autonomous Agent CSKH       │
              │  Khách hàng <======> [ AI Agent ] (RỦI RO CAO)│
              │  (Sai giá, sai cam kết, mất khách vĩnh viễn) │
              └──────────────────────────────────────────────┘

              ┌──────────────────────────────────────────────────────────────┐
              │  Mô hình Đúng đắn: Support Copilot (Human-in-the-Loop)       │
              │                                                              │
              │  Khách hàng <=======> [ Nhân viên CSKH ]                     │
              │                              ▲                               │
              │                              │ (Gợi ý câu trả lời + hồ sơ)   │
              │                              │ (Duyệt/Sửa trước khi bấm Gửi) │
              │                        [ Copilot AI ]                        │
              │                   (Lục đơn, lục case, tóm tắt)               │
              └──────────────────────────────────────────────────────────────┘
```

### 2.1. Phân tích rủi ro trong CSKH Bán lẻ
1. **Ảo giác thông tin (Hallucination)**: Agent tự hứa hoàn tiền 100%, tự bịa mã giảm giá 500k, hoặc nhận bừa lỗi khi khách chưa cung cấp bằng chứng.
2. **Sai lệch chính sách đổi trả**: Mỗi sản phẩm có thời hạn đổi trả và điều kiện khác nhau (hàng vỡ cối thủy tinh cần chụp ảnh tem vận chuyển; đổi máy mới trong 7 ngày).
3. **Mất khách hàng giá trị**: Khi khách phàn nàn vì lỗi kép (vừa giao trễ vừa bể hàng), nếu bot trả lời như máy hoặc từ chối trách nhiệm, khách rời bỏ ngay lập tức.

### 2.2. Lời giải: Copilot ngồi cạnh người trực
- Agent **không bao giờ nhắn thẳng cho khách**.
- Agent hoạt động như một trợ lý ảo siêu tốc độ: khi khách nhắn tới, trong vòng vài giây, Agent đọc lịch sử hội thoại, lục đơn hàng, lục thông tin bảo hành và các case khiếu nại tương tự.
- Agent trình bày cho nhân viên:
  - **Hồ sơ nhanh & Nhận định**: Khách là ai (hạng Silver, 3 đơn), lỗi là gì (giao trễ 4 ngày + nứt cối).
  - **3 mẫu phản hồi soạn sẵn**: *Empathetic* (Thấu cảm), *Concise* (Ngắn gọn), *Formal* (Trang trọng).
- Nhân viên chỉ cần: Liếc qua, chọn mẫu thích hợp, bấm **"Sử dụng"** (nội dung tự động điền vào ô chat), chỉnh sửa câu chữ nếu cần, rồi bấm **"Gửi"**.
- **Nếu Agent bịa hoặc sai**: Nhân viên nhìn thấy trước và sửa ngay; khách hàng hoàn toàn không thấy lỗi của AI.
- **Vòng lặp thu thập dữ liệu (Flywheel)**: Mọi thao tác của nhân viên (chọn mẫu nào, sửa từ nào, bỏ mẫu nào) được ghi log lại để đánh giá và tinh chỉnh prompt/model trong tương lai.

---

## 3. LỘ TRÌNH PHÁT TRIỂN 2 GIAI ĐOẠN (EVOLUTION PHASES)

Hệ thống được thiết kế theo tư duy đi từ cơ bản đến nâng cao:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ GIAI ĐOẠN 1: SINGLE-CALL COPILOT (Đọc dữ liệu & Soạn gợi ý 1 lần)            │
│ Tin nhắn khách ──> BM25 (4 nguồn) ──> Server tính số ──> Gọi LLM 1 lần       │
│                                              ──> Kiểm ID ──> Thẻ gợi ý       │
└──────────────────────────────────────┬───────────────────────────────────────┘
                                       │ Nâng cấp thêm Embedding & Agent Loop
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ GIAI ĐOẠN 2: MULTI-TURN AGENTIC & HYBRID SEARCH (Nâng cao)                   │
│ Khách nhắn ──> Agent ReAct (Tối đa 6 vòng, 6 Tools)                          │
│                     │                                                        │
│                     ├──> Hybrid Search (30% BM25 + 70% Semantic Embedding)   │
│                     ├──> Server tính số liệu deterministic                   │
│                     └──> Event Streaming (Hiện tiến độ từng bước trên UI)    │
│                           └──> Kiểm tra ID & Loại bỏ ảo giác                 │
│                                 └──> Thẻ gợi ý 3 mẫu (Empathetic/Concise/..) │
└──────────────────────────────────────────────────────────────────────────────┘
```

### 3.1. Giai đoạn 1 — Single-call Copilot (Bài 3 đến 13)
- **Cơ chế**: Không cần Agent phức tạp. Khi khách nhắn tin, lấy 3 tin nhắn gần nhất ghép lại làm câu truy vấn.
- **Tìm kiếm**: Dùng thuật toán **BM25** tự viết tìm trên 4 bảng dữ liệu (Khách hàng, Đơn hàng, Sản phẩm, Case khiếu nại).
- **Tính toán số liệu**: Server tính bằng code thuần (CLV, số lần trễ, AOV).
- **Gọi Model**: Gọi LLM **1 lần duy nhất** (Single call) với prompt chứa đầy đủ context đã gom.
- **Ràng buộc đầu ra**: Ép khuôn JSON (JSON mode), cơ chế tự sửa lỗi (auto-repair tối đa 1 lần nếu JSON bị malformed).
- **Kiểm định thực tế**: Server chạy hàm `grounding` để lọc bỏ mọi Order ID, Product ID, Case ID do model bịa ra.

### 3.2. Giai đoạn 2 — Multi-turn Agentic & Hybrid Search (Bài 14 đến 22)
- **Vấn đề xuất hiện ở GĐ 1**: Tìm theo từ khóa (BM25) bị gãy khi khách dùng từ đồng nghĩa hoặc từ lóng. Ví dụ: Khách than *"đồ xay trái cây tới tay đã bể"*, trong playbook chỉ có từ *"máy xay sinh tố bị nứt vỡ"* -> BM25 tìm không ra!
- **Cải tiến 1: Hybrid Search**:
  $$\text{Score} = 0.3 \times \text{BM25 Score} + 0.7 \times \text{Cosine Similarity}$$
  Lưu vector trực tiếp trong SQLite, tính Cosine thuần túy, không cần Vector DB ngoại lai.
- **Cải tiến 2: ReAct Agent Loop**:
  Thay vì gom tĩnh 1 lần, cho LLM tự quyết định cần tra cứu gì qua ReAct pattern.
  - Vòng 1: Tra đơn máy xay của khách -> Tìm thấy đơn #10032.
  - Vòng 2: Tra chi tiết sản phẩm máy xay Zenka B600 -> Biết cấu tạo có cối thủy tinh.
  - Vòng 3: Tra case tương tự về giao trễ nhiều lần.
  - Vòng 4: Tra case về cối thủy tinh nứt vỡ khi nhận hàng.
  - Vòng 5: Nhận thấy đủ thông tin -> Dừng gọi tool.
  - Vòng 6: Soạn kết quả trả về.
  - *Giới hạn cứng*: Tối đa 6 vòng lặp (`max_turns = 6`) để tránh lặp vô tận và kiểm soát chi phí.
- **Cải tiến 3: Streaming Trace trực tiếp lên UI**:
  Nhân viên không phải nhìn màn hình loading trống rỗng. Mọi bước Agent suy luận (thought), gọi tool gì, mất bao nhiêu giây, tiêu tốn bao nhiêu token đều được stream theo thời gian thực.

---

## 4. QUY TRÌNH HOẠT ĐỘNG 7 BƯỚC (7-STEP END-TO-END FLOW)

Từ ảnh chụp màn hình workflow (*Screenshot 2026-10-11 232630.png*), quy trình xử lý hoàn chỉnh gồm 7 bước:

```
[1. Tin nhắn của khách] ──(sau 1.2s debounce)──> [2. Agent tự tra cứu]
                                                         │ (Tối đa 6 vòng)
                                                         ▼
[4. Soạn kết quả] <──(Lọc kết quả)── [3. Dữ liệu của shop]
   │ (Khuôn JSON,                       │ (Hybrid: 30% BM25 + 70% Embed)
   │  sửa tối đa 1 lần)                 │ (Khách, Đơn, Sản phẩm, Case cũ)
   ▼
[5. Server kiểm lại (Grounding)] ───────> [6. Cột Copilot hiển thị]
   │ (Bỏ đơn/case không có thật)        │ (Hiện từng bước, hồ sơ nhanh,
   │                                     │  3 mẫu gợi ý câu trả lời)
   ▼                                     ▼
[7. Người trực duyệt & gửi] <────────────┘
   (Bấm "Sử dụng" -> Sửa text -> Bấm "Gửi" -> Khách nhận)
```

1. **Bước 1: Tin nhắn của khách**: Hệ thống chờ khoảng 1.2 giây sau tin nhắn cuối của khách (debounce) để tránh kích hoạt liên tục khi khách gõ nhiều dòng ngắn.
2. **Bước 2: Agent kích hoạt**: Khởi động vòng lặp ReAct, trang bị 6 tool chỉ đọc (read-only).
3. **Bước 3: Tra cứu dữ liệu shop**: Thực hiện hybrid search và query cơ sở dữ liệu giả lập.
4. **Bước 4: Soạn kết quả**: LLM định dạng câu trả lời theo JSON Schema nghiêm ngặt. Nếu schema lỗi, server phản hồi lỗi để model tự sửa lại trong 1 lượt retry.
5. **Bước 5: Server kiểm lại (Sanitization / Grounding)**: Server bóc tách các ID (mã đơn, mã sản phẩm, mã case khiếu nại), đối chiếu với DB. Nếu mã không tồn tại trong dữ liệu thực -> Gạch bỏ ngay lập tức, ngăn ngừa hallucination.
6. **Bước 6: Hiển thị trên Cột Copilot**:
   - Thẻ hồ sơ nhanh (CLV, AOV, tần suất mua, tỷ lệ giao đúng hạn).
   - Nhận định tình huống của AI.
   - 3 thẻ gợi ý câu trả lời theo 3 phong cách.
7. **Bước 7: Người trực quyết định (Human in the Loop)**:
   - Nhân viên bấm nút "Sử dụng" ở thẻ mong muốn.
   - Nội dung được điền vào input box.
   - Nhân viên đọc lướt, chỉnh sửa nếu cần, rồi tự bấm "Gửi".

---

## 5. CÁC TRỤ CỘT KỸ THUẬT CỐT LÕI (CORE TECHNICAL INNOVATIONS)

### 5.1. Triệt tiêu Framework (No-Framework Philosophy)
- Không dùng LangChain, LlamaIndex hay CrewAI.
- **Lý do**: Các framework lớn thường đóng gói quá nhiều lớp trừu tượng (abstractions), khó debug, khó kiểm soát prompt thực tế gửi đi, cồng kềnh và dễ thay đổi API liên tục.
- **Cách tác giả làm**: Tự viết toàn bộ:
  - BM25 tokenizer và scoring.
  - Vòng lặp ReAct Agent (chỉ khoảng 80-120 dòng code).
  - Cosine similarity tính toán trực tiếp bằng mã nguồn.
  - Quản lý token và chi phí bằng một struct/class `Meter` đơn giản.

### 5.2. Tách biệt hoàn toàn "Suy luận ngôn ngữ" và "Tính toán số liệu"
- **Quy tắc vàng**: **Tuyệt đối không để LLM làm tính toán tài chính và thống kê!**
- Thay vì yêu cầu LLM tính *"Khách này đã mua bao nhiêu tiền và tỷ lệ giao trễ là bao nhiêu%"*, một module riêng biệt (`customerstats`) bằng code thuần tính toán chính xác:
  - $\text{CLV}$ = Tổng tiền các đơn hoàn tất.
  - $\text{AOV}$ = Giá trị trung bình mỗi đơn.
  - $\text{On-time Rate}$ = Tỷ lệ đơn giao đúng hạn / Tổng đơn.
  - Tần suất mua hàng = Số đơn / Tổng số tháng từ đơn đầu tiên.
- Các số liệu chính xác 100% này được render trực tiếp lên UI và chỉ được đưa vào prompt dưới dạng các biến đã tính sẵn.

### 5.3. Công thức Hybrid Search thực chiến: 0.3 BM25 + 0.7 Semantic
- **BM25 (Lexical Search)**: Cực mạnh khi tìm từ khóa chính xác, mã đơn hàng (`#10032`), tên model thiết bị (`Zenka B600`).
- **Embedding Cosine Similarity (Semantic Search)**: Cực mạnh khi tìm ý nghĩa, cảm xúc, từ đồng nghĩa hoặc ngôn ngữ đời thường (`bể cối`, `vỡ tan`, `hàng giao chậm`).
- **Trọng số thực nghiệm**: $0.3 \times \text{BM25} + 0.7 \times \text{Semantic}$ tạo ra kết quả cân bằng tối ưu giữa tính chính xác của từ khóa và độ bao phủ ngữ nghĩa.

### 5.4. Hệ thống 3 Tone câu trả lời (Empathetic, Concise, Formal)
- **Empathetic (Thấu cảm - Ưu tiên cao cho khách bực tức)**: Thừa nhận lỗi lầm của shop ngay câu đầu tiên, không đổ lỗi bên vận chuyển, cam kết đổi mới hỏa tốc và tặng voucher đền bù.
- **Concise (Ngắn gọn)**: Đưa thẳng mã đơn, tình trạng và giải pháp trong 2-3 câu ngắn. Thích hợp cho khách vội hoặc giao tiếp nhanh.
- **Formal (Trang trọng)**: Xưng hô chuẩn mực doanh nghiệp, nêu quy trình xác minh và bồi thường theo đúng điều khoản.

### 5.5. Replay Engine phục vụ kiểm thử và tiết kiệm chi phí
- Trong quá trình phát triển và demo, việc gọi LLM liên tục gây tốn chi phí và mất thời gian chờ đợi.
- Hệ thống hỗ trợ chế độ **Replay Mode**: Đọc dữ liệu từ các file JSON bản ghi trước đó (`public/replay/`), mô phỏng lại 100% từng bước nhảy của agent, tiến trình gọi tool và câu trả lời mà không tốn một đồng API nào.

---

## 6. BÓC TÁCH CHI TIẾT CẤU TRÚC FOLDER GỐC (GO BACKEND & REACT FRONTEND)

Dựa trên tệp [folder-structure.md](file:///E:/MyProject/copilot-support/docs/clone/folder-structure.md), dưới đây là bản giải phẫu toàn diện:

```
├── backend/
│   ├── cmd/
│   │   ├── embed/
│   │   │   └── main.go              # CLI script chạy 1 lần: sinh embedding cho catalog & cases, lưu vào SQLite
│   │   └── server/
│   │       ├── main.go              # Entrypoint khởi động HTTP API server
│   │       └── retriever.go         # Bootstrap & khởi tạo BM25 + Vector Retriever
│   │
│   ├── internal/
│   │   ├── agent/                   # [GIAI ĐOẠN 2] Hệ thống ReAct Agent
│   │   │   ├── agent.go             # ReAct loop điều phối (tối đa 6 turns)
│   │   │   ├── events.go            # Cấu trúc streaming events (turn_start, tool_call, tool_done, finish)
│   │   │   ├── prompt.go            # System prompt hướng dẫn agent tra cứu và chọn tool
│   │   │   ├── records.go           # Bộ nhớ lưu lại các kết quả tra cứu được qua các vòng
│   │   │   ├── run_tool.go          # Dispatcher thực thi tool khi model yêu cầu
│   │   │   └── tools.go             # Khai báo schema 6 read-only tools
│   │   │
│   │   ├── assist/                  # [GIAI ĐOẠN 1 & 2] Dịch vụ tổng hợp hỗ trợ người trực
│   │   │   ├── service.go           # Logic điều phối chính của module assist
│   │   │   ├── gather.go            # Thu thập 3 tin nhắn gần nhất & context liên quan
│   │   │   ├── prompt.go            # Prompt soạn câu trả lời và nhận định
│   │   │   ├── render.go / format.go# Định dạng dữ liệu đưa vào prompt
│   │   │   ├── request.go / types.go# Kiểu dữ liệu đầu vào / đầu ra
│   │   │   ├── grounding.go         # Bộ lọc ID ảo giác: loại bỏ Order/Case/Product ID sai thực tế
│   │   │   └── finalize.go          # Đóng gói 3 câu trả lời gợi ý + hồ sơ
│   │   │
│   │   ├── catalog/                 # Quản lý tài liệu sản phẩm và case khiếu nại
│   │   │   ├── catalog.go           # Quản lý danh mục & in-memory cache
│   │   │   └── search_documents.go  # Chuẩn hóa tài liệu phục vụ indexing
│   │   │
│   │   ├── customerstats/           # Tính toán chỉ số thống kê khách hàng (100% Deterministic)
│   │   │   ├── calculator.go        # Hàm tính CLV, AOV, tỷ lệ đúng hạn, tần suất mua
│   │   │   └── customerstats.go     # Data structures & pipeline xử lý
│   │   │
│   │   ├── embeddings/              # Quản lý vector embeddings
│   │   │   ├── store.go             # Đọc/ghi vector vào database
│   │   │   └── sync.go              # Đồng bộ embedding từ raw JSON
│   │   │
│   │   ├── retrieval/               # Thuật toán tìm kiếm không phụ thuộc bên thứ ba
│   │   │   ├── tokenizer.go         # Tách từ, chuẩn hóa tiếng Việt, loại bỏ dấu thanh
│   │   │   ├── bm25.go              # Thuật toán Okapi BM25 tự code
│   │   │   ├── hybrid.go            # Công thức Hybrid: 0.3 * BM25 + 0.7 * Vector
│   │   │   └── retriever.go         # Interface tìm kiếm dùng chung
│   │   │
│   │   ├── storage/sqlite/          # Database SQLite
│   │   │   ├── sqlite.go            # Kết nối SQLite & migrate bảng
│   │   │   └── vector.go            # Đọc blob embedding, tính cosine distance
│   │   │
│   │   ├── llm/                     # Tương tác với LLM API (DeepSeek/OpenAI)
│   │   │   ├── client.go            # HTTP client gọi API
│   │   │   ├── embeddings.go        # Gọi embedding model
│   │   │   ├── json_mode.go         # Ép JSON + cơ chế auto-repair khi lỗi
│   │   │   ├── meter.go             # Đo lường token và tính tiền (VND/USD)
│   │   │   └── tools.go             # Chuyển đổi định dạng tool schema cho LLM
│   │   │
│   │   └── httpserver/              # API Server & Router
│   │       ├── server.go            # Thiết lập routes & middleware
│   │       ├── agent_handler.go     # Endpoint chạy Agent & stream progress (SSE)
│   │       ├── api_handlers.go      # Endpoint Single-call Assist
│   │       ├── customer_handlers.go # Endpoint lấy danh sách/thông tin khách hàng
│   │       └── spa_handler.go       # Serve Frontend SPA tĩnh
│   │
├── frontend/                        # Ứng dụng React + TypeScript + Vite + Tailwind
│   ├── src/
│   │   ├── components/
│   │   │   ├── shell/               # Layout 3 cột (Khách hàng | Người trực | Copilot)
│   │   │   ├── chat/                # Khung chat bên khách và bên shop
│   │   │   ├── operator/            # Giao diện người trực (ô nhập text, nút Sử dụng, Gửi)
│   │   │   ├── copilot/             # Cột Copilot (Timeline Agent, Nhận định, 3 mẫu gợi ý)
│   │   │   ├── customer/            # Thẻ hồ sơ nhanh (CLV, AOV, lịch sử đơn)
│   │   │   └── demo/                # Bộ kịch bản mẫu để test nhanh
│   │   └── public/replay/           # Bản ghi mẫu chạy replay offline
│   │
├── data/                            # Tập dữ liệu mẫu của Shop "Nhà Gọn"
│   ├── customers.json               # Dữ liệu khách hàng
│   ├── orders.json                  # Dữ liệu đơn hàng
│   ├── products.json                # Dữ liệu danh mục sản phẩm
│   ├── complaint-cases.json         # Dữ liệu kịch bản khiếu nại thực tế (Playbook)
│   └── embeddings.sqlite            # SQLite database lưu vectors
```

---

## 7. BẢN THIẾT KẾ CHUYỂN ĐỔI SANG PYTHON (PYTHON CLONE BLUEPRINT)

Để chuyển đổi từ Go sang Python một cách gọn gàng, đúng chuẩn **Clean Code** và tuân thủ nguyên tắc cách ly môi trường (`uv` + `.venv`), dưới đây là thiết kế chi tiết:

### 7.1. Bảng ánh xạ công nghệ (Go ➔ Python)

| Thành phần | Phiên bản Go (Gốc) | Phiên bản Python (Clone) | Lý do lựa chọn |
| :--- | :--- | :--- | :--- |
| **Web Framework** | Go HTTP Server (Standard/Gin) | **FastAPI** | Asynchronous, hiệu năng cao, native Pydantic validation, hỗ trợ SSE cực tốt. |
| **Môi trường & Package** | Go Modules (`go.mod`) | **uv + .venv** | Cực nhanh, cô lập workspace 100%, tuân thủ rule dự án. |
| **Data Validation** | Go Structs | **Pydantic v2** | Tự động parse, serialize và sinh JSON Schema cho LLM. |
| **Tìm kiếm Từ khóa** | `retrieval/bm25.go` (Tự code) | `retrieval/bm25.py` (Tự code) | Tự cài đặt thuật toán Okapi BM25 chuẩn xác, không phụ thuộc thư viện ngoài. |
| **Tìm kiếm Ngữ nghĩa** | `storage/sqlite/vector.go` | `storage/sqlite/vector.py` | Dùng `sqlite3` + `numpy` (hoặc math thuần) tính cosine similarity. |
| **LLM Client** | Custom Go HTTP Client | `openai` SDK hoặc `httpx` | Tương thích 100% với DeepSeek / OpenAI API, hỗ trợ streaming mượt mà. |
| **Agent ReAct Loop** | `agent/agent.go` (Tự code) | `agent/loop.py` (Tự code) | Tự viết vòng lặp `while turn < 6` với Async Generator để stream Server-Sent Events. |
| **Event Streaming** | Server-Sent Events (SSE) | **FastAPI `StreamingResponse`** | Truyền tải từng bước tra cứu (trace) xuống Frontend theo chuẩn SSE. |

### 7.2. Cấu trúc thư mục đề xuất cho Python Backend (`backend/`)

```
backend/
├── pyproject.toml               # Quản lý dependencies bằng uv
├── .env.example                 # DEEPSEEK_API_KEY, DEEPSEEK_BASE_URL, DB_PATH,...
├── cmd/                         # Scripts thực thi độc lập
│   ├── embed.py                 # Script tạo embeddings và ghi vào SQLite
│   └── main.py                  # Entrypoint khởi động Uvicorn FastAPI
│
├── app/
│   ├── core/
│   │   ├── config.py            # Cấu hình hệ thống qua pydantic-settings
│   │   └── logger.py            # Ghi log tập trung
│   │
│   ├── domain/                  # Pydantic Schemas & Data Models
│   │   ├── customer.py          # CustomerProfile, CustomerStats
│   │   ├── order.py             # Order, OrderItem
│   │   ├── product.py           # Product catalog
│   │   ├── complaint.py         # ComplaintCase (Playbook)
│   │   ├── agent_events.py      # TraceEvent, ToolCallEvent, TurnEvent
│   │   └── assist.py            # AssistRequest, AssistResponse (3 tones)
│   │
│   ├── services/
│   │   ├── retrieval/
│   │   │   ├── tokenizer.py     # Chuẩn hóa tiếng Việt, bỏ dấu, tách từ
│   │   │   ├── bm25.py          # Okapi BM25 engine
│   │   │   ├── vector.py        # Vector cosine similarity
│   │   │   └── hybrid.py        # Hybrid Search: 0.3 BM25 + 0.7 Vector
│   │   │
│   │   ├── customerstats/
│   │   │   └── calculator.py    # Tính CLV, AOV, on-time rate, order frequency
│   │   │
│   │   ├── llm/
│   │   │   ├── client.py        # Async client gọi DeepSeek / OpenAI
│   │   │   ├── meter.py         # Đo lường token và chi phí thực tế
│   │   │   └── json_mode.py     # Schema enforcement & auto-repair 1-retry
│   │   │
│   │   ├── assist/
│   │   │   ├── service.py       # Single-call assist coordinator
│   │   │   ├── gather.py        # Thu thập tin nhắn gần nhất
│   │   │   ├── grounding.py     # Lọc bỏ Order ID / Case ID ảo giác
│   │   │   └── prompts.py       # Prompts cho Single call
│   │   │
│   │   └── agent/
│   │       ├── loop.py          # ReAct Loop (Async generator stream ra SSE)
│   │       ├── tools.py         # Định nghĩa 6 read-only tools
│   │       ├── run_tool.py      # Tool execution dispatcher
│   │       └── prompts.py       # ReAct Agent system prompt
│   │
│   ├── storage/
│   │   ├── json_loader.py       # Đọc dữ liệu ban đầu từ 4 file JSON
│   │   └── sqlite_store.py      # Kết nối SQLite & quản lý bảng vectors
│   │
│   └── api/
│       ├── router.py            # Root API router
│       ├── v1/
│       │   ├── assist.py        # POST /api/v1/assist (Single call)
│       │   ├── agent.py         # POST /api/v1/agent/run (SSE Streaming)
│       │   ├── customers.py     # GET /api/v1/customers, GET /api/v1/customers/{id}
│       │   └── stats.py         # GET /api/v1/stats
│       └── sse.py               # Tiện ích phát SSE (Server-Sent Events)
```

---

## 8. GIẢI MÃ BỘ DỮ LIỆU & 6 READ-ONLY TOOLS

### 8.1. Bốn bộ dữ liệu cốt lõi của Shop "Nhà Gọn" (`data/*.json`)
1. **`customers.json`**:
   - Chứa thông tin khách hàng: `id`, `name`, `username`, `tier` (`Silver`, `Gold`,...), `registered_at`, `phone`, `address`.
2. **`orders.json`**:
   - Lịch sử đơn hàng: `order_id` (ví dụ `#10032`), `customer_id`, `items` (danh sách sản phẩm, số lượng, giá tiền), `total_amount`, `order_date`, `delivery_date`, `status` (`delivered`, `in_transit`), `delivery_delay_days` (số ngày trễ, ví dụ `4`).
3. **`products.json`**:
   - Danh mục sản phẩm: `product_id`, `name` (ví dụ *Máy xay sinh tố Zenka B600*), `category`, `price`, `specifications` (vật liệu cối thủy tinh, công suất), `warranty_months`.
4. **`complaint-cases.json`**:
   - Playbook các case xử lý khiếu nại mẫu trong quá khứ: `case_id`, `issue_type` (`giao_tre`, `nut_vo_san_pham`, `loi_nguon`), `summary`, `resolution` (chính sách đổi mới 7 ngày, quy trình thu hồi hàng lỗi miễn phí tại nhà, tặng voucher 100k).

### 8.2. Chi tiết 6 Read-Only Tools của Agent

| STT | Tên Tool | Input Schema | Mục đích nghiệp vụ | Nguồn tra cứu |
| :---: | :--- | :--- | :--- | :--- |
| **1** | `search_customers` | `query: str` | Tìm khách hàng theo tên, username hoặc số điện thoại. | `customers.json` |
| **2** | `get_customer_profile` | `customer_id: str` | Lấy chi tiết hồ sơ khách hàng và các chỉ số thống kê đã tính toán. | `customers.json` + `customerstats` |
| **3** | `search_orders` | `customer_id: str, query: Optional[str]` | Tìm các đơn hàng gần đây của khách, đặc biệt lọc theo tên sản phẩm đang nói tới. | `orders.json` |
| **4** | `get_order_detail` | `order_id: str` | Xem chi tiết 1 đơn hàng cụ thể (sản phẩm trong đơn, trạng thái giao hàng, số ngày trễ). | `orders.json` |
| **5** | `search_products` | `query: str` | Tra cứu thông tin sản phẩm và thông số kỹ thuật (ví dụ: máy dùng cối gì). | `products.json` (Hybrid search) |
| **6** | `search_cases` | `query: str` | Tra cứu playbook các case tương tự đã xử lý (chính sách đổi trả, bồi thường). | `complaint-cases.json` (Hybrid search) |

---

## 9. BẢNG ÁNH XẠ SYLLABUS: 24 TÌNH HUỐNG THỰC CHIẾN CẦN VƯỢT QUA

Dựa vào Syllabus trong ảnh chụp màn hình (*Screenshot 2026-10-11 233353.png*), đây là 24 bài toán thực tế mà bộ code clone cần giải quyết trọn vẹn:

```
┌────────────────────────────────────────────────────────────────────────────────────┐
│ GIAI ĐOẠN 0: BẮT ĐẦU (Bài 1 - 2)                                                   │
│ Bài 1: Intent - Một câu trả lời sai gửi thẳng tới khách đang bực là mất khách     │
│ Bài 2: Setup - Chuẩn bị môi trường & công cụ                                       │
├────────────────────────────────────────────────────────────────────────────────────┤
│ GIAI ĐOẠN 1: COPILOT ĐỌC DỮ LIỆU VÀ SOẠN GỢI Ý (Bài 3 - 13)                        │
│ Bài 3: UX Design - Ba khách đang chờ, nhân viên chỉ kịp liếc cột copilot 1 lần    │
│ Bài 4: Design - Khách bị trừ tiền 2 lần, số tiền gợi ý không được lệch 1 đồng     │
│ Bài 5: Plan - Khách hỏi mã hoàn tiền mà nhân viên phải mở 3 màn hình mới trả lời  │
│ Bài 6: IR - Vì sao văn bản lặp từ khóa nhiều nhất chưa chắc là kết quả cần tìm    │
│ Bài 7: Code - Khách nhắn không dấu mà copilot vẫn phải tìm ra đúng case giao trễ   │
│ Bài 8: Code - Khách bảo lần trước cũng giao trễ, gợi ý không được nói sai số lần  │
│ Bài 9: Code - Khách vừa than phiền, người trực đang gõ dở, copilot tìm theo lời ai│
│ Bài 10: Code - Khách dọa đánh giá 1 sao mà thẻ gợi ý trống vì model trả hỏng JSON │
│ Bài 11: Code - Khách than cối máy xay nứt mà model nhắc tới đơn robot hút bụi     │
│ Bài 12: Code - Chú Năm nhận nhầm nồi cơm, người trực cần thấy gợi ý ngay lập tức   │
│ Bài 13: Test - Khách đang chờ mà tới giây thứ 30 copilot vẫn chưa soạn xong       │
├────────────────────────────────────────────────────────────────────────────────────┤
│ GIAI ĐOẠN 2: CHO COPILOT TỰ TRA CỨU - NÂNG CAO (Bài 14 - 22)                       │
│ Bài 14: NLP - Khách viết "bể" còn playbook viết "nứt", nhận ra hai câu cùng nghĩa │
│ Bài 15: Code - Khách nói hàng bể mà tìm chữ bể trong playbook không ra case nào   │
│ Bài 16: Code - Khách nhắn đồ xay trái cây bể mà copilot đưa case tai nghe lên đầu │
│ Bài 17: Protocols - Khi model cần tra dữ liệu thật, app và model gửi cho nhau gì  │
│ Bài 18: UX - Người dùng nhìn biểu tượng chờ mà không biết server đang làm bước nào│
│ Bài 19: Code - Khách hỏi hoàn tiền mà người trực phải mở 4 màn hình               │
│ Bài 20: Code - Copilot tra cứu xong rồi mà người trực vẫn không nhận được gợi ý   │
│ Bài 21: Code - Người trực chỉ thấy biểu tượng chờ, không biết copilot tra tới đâu │
│ Bài 22: Test - Khách bị giao trễ 2 lần mà copilot lại ghi là lần thứ 3            │
├────────────────────────────────────────────────────────────────────────────────────┤
│ GIAI ĐOẠN 3: TRƯỚC KHI LÊN PRODUCTION (Bài 23 - 24)                                │
│ Bài 23: Deploy - Chủ shop hỏi chạy copilot cả tháng thì tốn bao nhiêu tiền (Meter)│
│ Bài 24: Deploy - Copilot chạy tốt trên máy bạn nhưng chưa dùng được với khách thật│
└────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 10. LỘ TRÌNH THỰC THI CLONE (STEP-BY-STEP IMPLEMENTATION ROADMAP)

Để xây dựng dự án clone bằng Python một cách bài bản, chúng ta chia lộ trình làm 6 cột mốc rõ ràng:

### Cột mốc 1: Chuẩn bị Môi trường & Bộ Dữ liệu Mẫu (`data/`)
- Khởi tạo thư mục dự án và môi trường ảo Python bằng `uv` (`uv venv`, `uv pip install fastapi uvicorn pydantic openai numpy aiosqlite pytest`).
- Tạo 4 tệp dữ liệu giả lập chuẩn của shop "Nhà Gọn": `customers.json`, `orders.json`, `products.json`, `complaint-cases.json`.

### Cột mốc 2: Core Retrieval Engine (BM25 + Tokenizer + Vector SQLite)
- Viết `tokenizer.py`: Xử lý tiếng Việt (loại bỏ dấu, lowercase, tách từ).
- Viết `bm25.py`: Thuật toán Okapi BM25 thuần túy.
- Viết `storage/sqlite_store.py`: Tạo bảng `embeddings` trong SQLite.
- Viết `cmd/embed.py`: Đọc các tệp JSON, gọi DeepSeek Embedding API để vector hóa sản phẩm và case, lưu blob vector vào SQLite.
- Viết `hybrid.py`: Kết hợp điểm số $0.3 \times \text{BM25} + 0.7 \times \text{Cosine}$.

### Cột mốc 3: Customer Stats Calculator & Anti-Hallucination Grounding
- Viết `calculator.py`: Tính toán chính xác CLV, AOV, số đơn trễ, tỷ lệ on-time.
- Viết `grounding.py`: Nhận diện mọi ID xuất hiện trong câu trả lời gợi ý của LLM và đối chiếu với database, loại bỏ mọi ID rác/ảo giác.

### Cột mốc 4: Giai đoạn 1 — Single-Call Assist Service
- Viết `services/assist/service.py`: Lấy 3 tin nhắn gần nhất, chạy BM25, tính stats, gọi LLM 1 lần duy nhất với JSON mode (kèm auto-repair 1 lần).
- Đóng gói 3 mẫu câu trả lời (`Empathetic`, `Concise`, `Formal`).

### Cột mốc 5: Giai đoạn 2 — ReAct Agent Loop & Streaming Events
- Khai báo 6 read-only tools theo chuẩn OpenAI Tool Calling.
- Viết `services/agent/loop.py`: Vòng lặp ReAct (tối đa 6 turns), cơ chế dừng khi đủ thông tin.
- Tạo Event Emitter sinh ra các sự kiện: `agent_start`, `turn_start`, `tool_call`, `tool_result`, `agent_finish` phục vụ streaming.
- Tích hợp `meter.py` để đo token và ước tính chi phí.

### Cột mốc 6: API Server FastAPI & Frontend Integration
- Viết các route FastAPI: `/api/v1/assist` (Single call), `/api/v1/agent/run` (SSE Streaming), `/api/v1/customers`.
- Tích hợp giao diện Frontend React: 3 cột (Khách hàng giả lập, Người trực, Cột Copilot với Live Trace).
- Tích hợp Replay Mode để có thể demo/kiểm thử offline mà không cần API key.

---

## TỔNG KẾT & KHUYẾN NGHỊ

Dự án này là một **kiến trúc mẫu mực (Gold Standard Architecture)** cho ứng dụng AI Agent trong doanh nghiệp:
1. **Thực dụng & An toàn**: Dùng mô hình Copilot (Human in the loop) thay vì liều lĩnh để AI chat thẳng với khách.
2. **Kỹ thuật tinh gọn**: Không phụ thuộc framework cồng kềnh, mọi con số và thuật toán (BM25, Hybrid search, Stats) đều minh bạch và kiểm soát được 100%.
3. **Trải nghiệm người dùng vượt trội**: Hiển thị rõ ràng suy nghĩ và tiến trình của Agent giúp giảm tâm lý chờ đợi của nhân viên trực.

Bản phân tích này sẽ là tài liệu nền tảng để chúng ta bước vào giai đoạn hiện thực hóa (coding) bản clone Python cho dự án Copilot Support.
