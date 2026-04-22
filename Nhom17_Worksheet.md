# Worksheet 0–5 — Đề tài 4: Manufacturing Maintenance Copilot

## Thông tin chủ đề
- **Tên nhóm:** [Điền tên nhóm]
- **Chủ đề:** Scenario 4 – Manufacturing Maintenance Copilot
- **Loại chủ đề:** Scenario card

---

# Worksheet 0 — Learning Timeline

## 1) 2–3 kỹ năng nhóm tự tin nhất
- Phân tích bài toán AI agent trong bối cảnh doanh nghiệp
- Thiết kế luồng RAG/tra cứu tài liệu kỹ thuật
- Đánh giá deployment, cost và reliability ở mức MVP

## 2) Mô tả ngắn sản phẩm/scenario được chọn
Manufacturing Maintenance Copilot là một AI assistant hỗ trợ kỹ thuật viên và đội vận hành trong nhà máy tra cứu SOP, manual máy móc, log bảo trì, lịch vận hành và checklist an toàn để xử lý sự cố nhanh hơn, đúng quy trình hơn và giảm phụ thuộc vào việc hỏi người có kinh nghiệm.

## 3) Chủ đề chốt dùng xuyên suốt cả ngày
**Manufacturing Maintenance Copilot**

## 4) Sản phẩm này giải quyết bài toán gì?
Giải quyết bài toán tra cứu tri thức vận hành phân tán và phản hồi chậm trong môi trường nhà máy. Hiện trạng thường là SOP nằm rải rác, log bảo trì nằm ở nhiều hệ thống cũ, checklist an toàn ở dạng file/tài liệu khác nhau, khiến kỹ thuật viên mất thời gian tìm thông tin khi máy có lỗi hoặc cần bảo trì định kỳ.

## 5) Ai là người dùng chính?
- Kỹ thuật viên bảo trì
- Quản đốc xưởng
- Team vận hành

## 6) Vì sao chủ đề này phù hợp để phân tích deployment và cost?
Vì đây là bài toán enterprise điển hình: có hệ thống legacy, có khu vực mạng nội bộ không cho internet, đòi hỏi ổn định cao, và môi trường thiết bị/mạng không đồng đều. Nhờ vậy nhóm có đủ chất liệu để tranh luận về On-prem, Edge deployment, cost hạ tầng, cost tích hợp, cost reliability.

---

# Worksheet 1 — Enterprise Deployment Clinic

## 1) Bối cảnh tổ chức/khách hàng
Khách hàng là một nhà máy sản xuất có nhiều dây chuyền, nhiều thiết bị công nghiệp, quy trình bảo trì định kỳ và xử lý sự cố vận hành. Tổ chức đã có một số hệ thống sẵn có như CMMS/MES/ERP hoặc kho tài liệu nội bộ, nhưng dữ liệu phân tán và mức độ số hóa không đồng đều.

## 2) Dữ liệu hệ thống sẽ động đến
- SOP vận hành và bảo trì
- Manual máy móc
- Log bảo trì
- Lịch vận hành
- Checklist an toàn
- Ticket sự cố
- Hướng dẫn xử lý theo mã lỗi
- Nhật ký ca trực
- Danh sách phụ tùng thay thế
- Lịch sử downtime theo máy

## 3) Mức độ nhạy cảm của dữ liệu
**Mức độ nhạy cảm: trung bình đến cao**

### Lý do
- Không nhạy cảm như dữ liệu y tế hay ngân hàng, nhưng vẫn là dữ liệu vận hành cốt lõi.
- SOP, manual nội bộ, log lỗi và quy trình an toàn có thể là tài sản vận hành quan trọng.
- Nếu lộ ra ngoài có thể ảnh hưởng bảo mật quy trình, bí quyết sản xuất hoặc an toàn vận hành.

## 4) Ba ràng buộc enterprise lớn nhất
1. Nhiều hệ thống legacy
2. Có khu vực mạng nội bộ không cho internet
3. Cần độ ổn định cao

## 5) Chọn mô hình triển khai
**Đề xuất: On‑prem + Edge deployment**

## 6) Hai lý do vì sao chọn mô hình đó

### Lý do 1: Giải quyết triệt để bài toán vùng không có internet
Với các khu vực mạng nội bộ không cho internet, cloud hoàn toàn không thể hoạt động. On‑prem + Edge cho phép hệ thống vẫn chạy độc lập, kỹ thuật viên vẫn tra cứu được SOP, manual, checklist an toàn ngay tại hiện trường.

### Lý do 2: Đảm bảo độ ổn định cao và khả năng chịu lỗi phân tán
- Server trung tâm on‑prem đặt trong phòng máy chủ, không phụ thuộc internet.
- Edge node đặt tại các phân xưởng có mạng yếu hoặc không internet, có thể hoạt động độc lập.
- Nếu server trung tâm hoặc mạng LAN gặp sự cố, edge node vẫn phục vụ được các câu hỏi cơ bản (cache, rule-based).

## 7) Kết luận ngắn
- Không nên cloud-only (vì có vùng không internet)
- Không cần hybrid (vì không có nhu cầu cloud thực sự, thêm phức tạp)
- **Nên chọn On‑prem + Edge deployment** – giải quyết đúng các ràng buộc, đơn giản hơn hybrid, độ ổn định cao nhất

---
# Worksheet 2 – Cost Anatomy Lab (On‑prem + Edge)

## 1) Ước lượng user/ngày, request/ngày, peak traffic

### Giả định MVP cho 1 nhà máy quy mô vừa

| Loại người dùng | Số lượng |
|----------------|----------|
| Kỹ thuật viên bảo trì | 40 |
| Quản đốc / vận hành | 8 |
| Người dùng gián tiếp (kỹ sư, thủ kho, giám sát) | 12 |
| **Tổng user hoạt động/ngày** | **60** |

### Ước lượng usage

| Chỉ số | Giá trị |
|--------|---------|
| Trung bình request/người/ngày | 8 |
| **Tổng request/ngày** | **480** |
| **Tổng request/tháng** (26 ngày làm việc) | **12.480** |

### Peak traffic

- **Thời điểm cao điểm:** đầu ca (7h30–8h30), cuối ca (16h–17h), khi có sự cố đột xuất.
- **Peak đồng thời:** 15–25 request trong 5–10 phút.  
  → Yêu cầu hệ thống có thể xử lý **≥ 5 request/giây** trong burst ngắn.

---

## 2) Ước lượng input/output tokens (chỉ để ước lượng tài nguyên)

| Loại | Token/request | Tổng token/ngày |
|------|---------------|------------------|
| Input (câu hỏi + context + prompt) | 3.000 | 1.440.000 |
| Output (câu trả lời + checklist) | 500 | 240.000 |
| **Tổng** | 3.500 | **1.680.000** |

⚠️ **Lưu ý:** Trên on‑prem, token **không có giá trực tiếp**.  
Con số này dùng để chọn dung lượng GPU (VD: model 7B cần ~14GB VRAM) và thiết kế cache.

---

## 3) Các lớp cost – chi tiết kèm giả định

| STT | Lớp cost | Thành phần | Giả định cho MVP (1 nhà máy) |
|-----|----------|------------|-------------------------------|
| a) | **Khấu hao GPU/Server** | GPU RTX 4060 Ti 16GB, CPU 8 core/32GB RAM, case, nguồn | 25.000.000 VNĐ, khấu hao 3 năm → ~694.000/tháng |
| b) | **Edge node** | 3× Jetson Orin Nano 8GB (hoặc RPi 5) | Mỗi cái 6.000.000 VNĐ, khấu hao 3 năm → 500.000/tháng |
| c) | **Compute (retrieval/embedding)** | Chạy trên cùng server CPU (không tính riêng) | Đã gộp vào a) |
| d) | **Storage** | 1TB NVMe (chính) + 2TB HDD (backup) | 3.000.000 VNĐ, khấu hao 3 năm → 83.000/tháng |
| e) | **Điện + làm mát + UPS** | GPU 150W, CPU 65W, edge 3×15W, UPS 50W, điều hòa 500W (8h/ngày) | Tính riêng bên dưới |
| f) | **Dự phòng hardware** | 1 GPU dự phòng lạnh (cùng loại) | 25.000.000 VNĐ, khấu hao 5 năm → 417.000/tháng |
| g) | **Human review** | Chuyên gia vận hành 0,5 ngày/tuần | 8h/tháng × 200.000/h = 1.600.000/tháng |
| h) | **Nhân sự vận hành IT** | 0,2 FTE kỹ sư hệ thống (làm thêm) | 0,2 × 15.000.000 = 3.000.000/tháng |
| i) | **Logging / Monitoring** | Prometheus + Grafana (free), thời gian cài đặt (phân bổ) | 200.000/tháng |
| j) | **Maintenance / Integration** | Viết connector cho CMMS, MES (chi phí 1 lần 10.000.000, phân bổ 12 tháng) | 833.000/tháng |
| k) | **Backup & DR** | Backup hàng tuần vào ổ HDD, không cloud | 100.000/tháng (hao mòn ổ) |
| l) | **Đào tạo người dùng** | 2 buổi × 3h cho 60 người (chi phí 1 lần 5.000.000, phân bổ 12 tháng) | 417.000/tháng |

---

## 4) Cost MVP sơ bộ – tính bằng tiền (VNĐ/tháng)

### Tổng hợp chi phí hàng tháng (sau khấu hao và phân bổ)

| Hạng mục | Chi phí (VNĐ/tháng) | Ghi chú |
|----------|---------------------|---------|
| Khấu hao GPU/server | 694.000 | 25tr / 36 tháng |
| Khấu hao edge node (3 cái) | 500.000 | 18tr / 36 tháng |
| Khấu hao storage | 83.000 | 3tr / 36 tháng |
| Điện năng (tính chi tiết bên dưới) | 1.200.000 | Làm tròn |
| Dự phòng hardware (GPU dự phòng) | 417.000 | 25tr / 60 tháng |
| Human review (chuyên gia) | 1.600.000 | 8h/tháng |
| Nhân sự IT vận hành | 30.000.000 | 0,2 FTE |
| Maintenance / integration (phân bổ) | 833.000 | 10tr / 12 tháng |
| Backup & DR | 100.000 | |
| Đào tạo người dùng (phân bổ) | 417.000 | 5tr / 12 tháng |
| Logging/monitoring (phân bổ) | 200.000 | |
| **Tổng** | **37.044.000** | **≈ 1500 USD** |

### Tính chi tiết điện năng

| Thiết bị | Công suất (W) | Số giờ/ngày | Điện năng/ngày (kWh) | Điện năng/tháng (kWh) |
|----------|---------------|-------------|----------------------|----------------------|
| GPU (RTX 4060 Ti) | 150 | 24 | 3,6 | 93,6 |
| CPU server | 65 | 24 | 1,56 | 40,6 |
| 3 edge node (Jetson) | 45 (3×15) | 24 | 1,08 | 28,1 |
| UPS & switch | 50 | 24 | 1,2 | 31,2 |
| Điều hòa (làm mát) | 500 | 8 (giờ làm việc) | 4,0 | 104 |
| **Tổng** | | | **11,44 kWh/ngày** | **~297 kWh/tháng** |

- Giá điện công nghiệp: 3.500 VNĐ/kWh  
- **Tổng tiền điện/tháng:** 297 × 3.500 ≈ **1.040.000 VNĐ** (làm tròn 1.200.000 kể cả hao phí)

---

## 5) Khi số user tăng 5x hoặc 10x (3.000 – 6.000 request/ngày)

| Hạng mục | Mức tăng | Giải thích |
|----------|----------|-------------|
| Khấu hao GPU | **+200–300%** | Cần thêm 1–2 GPU nếu không đủ throughput |
| Điện + làm mát | **+150–200%** | Theo số GPU và tải điều hòa |
| Edge node | **+100–200%** | Nếu mở rộng thêm phân xưởng |
| Chi phí đồng bộ edge–server | **+200%** | Nhiều edge hơn → tăng chi phí phát triển và vận hành |
| Chi phí maintenance & knowledge ops | **+100–200%** | Số tài liệu, thiết bị tăng gấp 2–3 lần |
| Dự phòng hardware | **+200%** | Tỷ lệ thuận với số lượng hardware |

**Ví dụ scale 5x:**  
- GPU: 2 chiếc (hoặc 1 A10) → khấu hao ~2–3 triệu/tháng  
- Điện: 2.500.000 – 3.500.000/tháng  
- Nhân sự IT: 0,5 FTE → 7.500.000/tháng  
- **Tổng dự kiến: ~18–22 triệu/tháng** (gấp 2–2,5 lần MVP)

---

## 6) Cost driver lớn nhất

| Cost driver | Tỷ trọng trong MVP | Ghi chú |
|-------------|--------------------|---------|
| **Khấu hao GPU + điện** | ~20–25% | Chi phí cố định, khó giảm |
| **Nhân sự (IT + chuyên gia)** | ~45–50% | Chiếm phần lớn, thường bị bỏ qua |
| **Tích hợp legacy & đào tạo** | ~15% | Chi phí một lần nhưng lớn |

📌 **Kết luận:** Trên on‑prem, **nhân sự và khấu hao phần cứng** là hai cost driver lớn nhất. Không có chi phí theo token.

---

## 7) Hidden cost dễ bị quên nhất – kèm ước lượng

| Hidden cost | Ước lượng (VNĐ/tháng) | Ghi chú |
|-------------|----------------------|----------|
| Điện cho điều hòa làm mát GPU | 300.000 – 500.000 | Dễ quên vì tính riêng |
| UPS và bảo trì ắc quy | 100.000 – 200.000 | Thay ắc quy 2 năm/lần |
| Thay thế ổ cứng SSD/HDD hỏng | 50.000 – 100.000 | Tỷ lệ hỏng 1–2%/năm |
| Bảo trì edge node (vệ sinh, thẻ nhớ) | 100.000 – 200.000 | Mỗi node 1–2 lần/năm |
| Chi phí downtime (mất sản xuất) | Rất lớn (hàng chục triệu/giờ) | Không tính vào cost hệ thống nhưng rủi ro tài chính |
| Đào tạo lại khi SOP thay đổi | 200.000 – 500.000 | Phân bổ theo năm |

---

## 8) Nhóm đang ước lượng quá lạc quan ở đâu? – Bổ sung định lượng

| Giả định lạc quan | Mức độ ảnh hưởng | Giải thích |
|-------------------|------------------|-------------|
| "Mạng xưởng ổn định" | Cao | Thực tế nhiễu, mất gói, đứt cáp → phải có edge |
| "GPU đủ dùng 3 năm" | Trung bình | Model mới yêu cầu VRAM lớn hơn, có thể phải nâng cấp sau 2 năm |
| "Không cần dự phòng hardware" | Rất cao | GPU chết → mất 1–2 tuần chờ thay → thiệt hại lớn |
| "Tài liệu sạch" | Rất cao | Tốn ít nhất 100–200 giờ công để làm sạch PDF scan, OCR |
| "Kỹ thuật viên sẽ dùng chat" | Trung bình | Phải thiết kế UX cực kỳ đơn giản, giao diện bằng giọng nếu có thể |
| "Edge node không cần bảo trì" | Trung bình | Thẻ nhớ hỏng sau 2–3 năm, bụi bẩn trong xưởng |

---

## ✅ Kết luận (tóm tắt cho báo cáo)

- **Chi phí MVP on‑prem + edge:** ~9.000.000 VNĐ/tháng (~360 USD) cho 60 user, 480 request/ngày.
- **Cost driver lớn nhất:** nhân sự vận hành (IT + chuyên gia) và khấu hao GPU.
- **Hidden cost quan trọng:** điện cho làm mát, dự phòng hardware, bảo trì edge node.
- **Rủi ro lớn nhất:** tài liệu bẩn, kỹ thuật viên không dùng, GPU hỏng không dự phòng.
- **So với cloud:** Với tải này, on‑prem có thể rẻ hơn cloud sau 6–8 tháng, nhưng yêu cầu vốn đầu tư ban đầu và nhân sự.

---

# Worksheet 3 — Cost Optimization Debate (On‑prem + Edge)

## Chiến lược 1 — Semantic caching cho câu hỏi lặp lại

### Tiết kiệm phần nào
Giảm số lần chạy inference trên GPU → giảm tải GPU, giảm điện năng, kéo dài tuổi thọ phần cứng.

### Lợi ích
- Giảm latency (trả lời từ cache nhanh hơn 10–100 lần)
- Giảm tải GPU → có thể phục vụ thêm user mà không cần mua GPU mới
- Giảm điện năng tiêu thụ
- Ổn định trải nghiệm ở giờ cao điểm

### Trade-off
- Cần cơ chế invalidate khi SOP cập nhật
- Cache cũ có thể trả thông tin lỗi thời (nguy hiểm với an toàn)

### Thời điểm áp dụng
**Làm ngay từ MVP**, vì nhà máy có nhiều câu hỏi lặp theo ca và theo dòng máy.

---

## Chiến lược 2 — Model routing (chọn model theo độ khó)

### Tiết kiệm phần nào
Câu hỏi dễ (FAQ, checklist) dùng model siêu nhẹ chạy trên CPU hoặc edge node; câu hỏi khó mới dùng GPU.

### Lợi ích
- Giảm tải GPU → tiết kiệm điện, kéo dài tuổi thọ
- Có thể dùng CPU cho 50–70% request → không cần thêm GPU khi scale
- Giữ chất lượng cao cho các case quan trọng

### Trade-off
- Phải có cơ chế phân loại request
- Routing sai có thể giảm chất lượng

### Thời điểm áp dụng
Áp dụng sớm sau khi có dữ liệu log sử dụng 2–4 tuần.

---

## Chiến lược 3 — Edge-native inference (chạy model trên CPU/edge cho tác vụ nhẹ)

### Tiết kiệm phần nào
Với các tác vụ cố định (tóm tắt log lỗi ngắn, trích SOP theo từ khóa, checklist an toàn), chạy hoàn toàn trên CPU hoặc edge node (Jetson/RPi) thay vì dùng GPU trung tâm.

### Lợi ích
- Giải phóng GPU cho tác vụ thực sự cần
- Tiết kiệm điện năng (CPU ~65W, GPU ~150–300W)
- Edge node hoạt động độc lập khi mất mạng LAN
- Phù hợp với khu vực không có internet

### Trade-off
- CPU chậm hơn GPU (3–10 lần)
- Model CPU thường nhỏ hơn, chất lượng có thể thấp hơn

### Thời điểm áp dụng
Áp dụng ngay từ MVP cho edge node, hoặc khi volume đủ lớn.

---

## Chốt ưu tiên

### Làm ngay
1. Semantic caching
2. Edge-native inference (CPU/edge cho tác vụ nhẹ)

### Làm sau khi có usage ổn định
3. Model routing nâng cao (dùng classifier học từ log)

---

# Worksheet 4 — Scaling & Reliability Tabletop (On‑prem + Edge)

## Tình huống 1 — Traffic tăng đột biến
Ví dụ: đầu ca sáng, nhiều kỹ thuật viên cùng hỏi do một dây chuyền gặp sự cố.

### Tác động tới user
- Trả lời chậm, timeout
- Kỹ thuật viên bỏ hệ thống, quay lại gọi người trực tiếp

### Phản ứng ngắn hạn
- Bật queue cho request không khẩn cấp
- Ưu tiên request liên quan lỗi máy đang active
- Trả lời tạm bằng top SOP/FAQ liên quan từ cache

### Giải pháp dài hạn
- Pre-warm cache cho lỗi phổ biến
- Tách class request: real-time vs async
- Bổ sung thêm edge node hoặc GPU

---

## Tình huống 2 — Server trung tâm / GPU bị lỗi

### Tác động tới user
- Không nhận được câu trả lời từ server trung tâm
- Ảnh hưởng đến khu vực phụ thuộc vào server

### Phản ứng ngắn hạn
- Fallback sang edge node (vẫn trả lời được câu hỏi cơ bản)
- Retry có giới hạn
- Fallback sang search-only mode (trả SOP/manual liên quan)
- Human escalation

### Giải pháp dài hạn
- Dự phòng nóng cho GPU (2 GPU hoặc spare)
- Thiết kế edge node đủ thông minh để hoạt động độc lập dài ngày
- Định kỳ backup dữ liệu để restore nhanh
- Có GPU dự phòng lạnh (cold spare) trong kho

---

## Tình huống 3 — Mạng LAN bị đứt / mất kết nối giữa edge và server

### Tác động tới user
- Edge node không đồng bộ được với server trung tâm
- Kỹ thuật viên tại phân xưởng vẫn dùng được edge (nếu đã có cache)

### Phản ứng ngắn hạn
- Edge node chuyển sang chế độ offline hoàn toàn
- Trả lời từ cache có sẵn
- Log request lưu local, sẽ sync khi mạng hồi phục

### Giải pháp dài hạn
- Tăng dung lượng cache và model nhỏ trên edge
- Thiết kế offline-first cho toàn bộ hệ thống
- Đồng bộ định kỳ khi có mạng (không yêu cầu real-time)
- Xử lý conflict khi nhiều edge node cùng sync dữ liệu (ưu tiên server trung tâm)

---

## Các metric cần monitor
- P95 latency (trên server và edge)
- Success rate / error rate
- Cache hit rate (trên server và edge)
- Fallback rate (sang search-only hoặc human)
- GPU utilization & nhiệt độ
- Số request theo ca / theo xưởng
- Tỷ lệ câu trả lời được đánh giá hữu ích
- Tỷ lệ escalation sang con người
- Thời gian sync giữa edge và server
- Số lần edge node offline quá lâu

## Fallback proposal
**Fallback nhiều lớp (cho on‑prem + edge):**
1. **Cache answer** (trên edge hoặc server) nếu câu hỏi quen thuộc
2. **Search-only mode** trả SOP/manual liên quan
3. **Rule-based response** cho checklist an toàn chuẩn
4. **Human escalation** tới quản đốc/kỹ sư trực ca

## Kết luận reliability
Với on‑prem + edge, độ tin cậy được đảm bảo nhờ:
- Không phụ thuộc internet
- Edge node hoạt động độc lập khi mất LAN
- Fallback nhiều lớp
- Dự phòng hardware (nếu ngân sách cho phép)
- Có thể hoạt động offline dài ngày tại phân xưởng

---

# Worksheet 5 — Skills Map & Track Direction

## 1) Tự chấm nhóm theo 3 mảng

| Mảng | Điểm trung bình nhóm (1–5) |
|------|---------------------------|
| Business / Product | 3.5 |
| Infra / Data / Ops | 4.0 |
| AI Engineering / Application | 3.5 |

## 2) Điểm mạnh của nhóm nằm ở đâu?
**Điểm mạnh nên chốt:** Infra / Data / Ops

### Lý do
- Đề tài này thiên mạnh về deployment thực tế trong môi trường nhà máy
- Cần hiểu network nội bộ, legacy integration, edge computing, reliability, fallback
- Đây là nơi nhóm có thể tạo khác biệt tốt nhất

## 3) Track Phase 2 phù hợp nhất
**Đề xuất Track Phase 2:** Infra / Data / Ops

Lý do: Trọng tâm của dự án là hệ thống hóa deployment và reliability cho môi trường công nghiệp với on‑prem + edge, không phải làm demo chatbot trên cloud.

## 4) 2–3 kỹ năng cần bù nếu muốn tiếp tục dự án này
1. Thiết kế hệ thống on‑prem + edge cho AI agent
2. Tích hợp dữ liệu legacy và xây pipeline ingest tài liệu kỹ thuật
3. Monitoring, fallback, incident response cho AI system trong môi trường production
4. Quản lý và đồng bộ dữ liệu giữa edge node và server trung tâm
5. Edge computing (Jetson, Raspberry Pi, tối ưu model cho edge)

---

# Team Note Sheet

- **Tên nhóm:** [Điền tên nhóm]
- **Chủ đề:** Manufacturing Maintenance Copilot

## 3 ràng buộc enterprise lớn nhất
- Nhiều hệ thống legacy
- Có khu vực mạng nội bộ không cho internet
- Cần độ ổn định cao

## Mô hình triển khai chọn
**On‑prem + Edge deployment**

## Cost driver lớn nhất
- Khấu hao GPU + điện năng (inference và retrieval)

## 3 chiến lược optimize được chọn
- Semantic caching
- Model routing
- Edge-native inference (CPU/edge cho tác vụ nhẹ)

## Fallback / reliability plan ngắn gọn
- Cache (edge/server) → search-only SOP/manual → rule-based checklist → human escalation

## Track Phase 2 đề xuất
- Infra / Data / Ops

---

# Phiếu chốt chủ đề

- **Tên nhóm:** [Điền tên nhóm]
- **Chủ đề được chọn:** Scenario 4 – Manufacturing Maintenance Copilot
- **Nguồn chủ đề:** Scenario card

## Lý do chọn chủ đề này
Chủ đề có bối cảnh enterprise rất rõ, đủ để tranh luận sâu về deployment, cost, optimization và reliability. Đặc biệt phù hợp để phân tích on‑prem + edge deployment vì nhà máy có nhiều hệ thống legacy, mạng nội bộ hạn chế internet, thiết bị và mạng không đồng đều, yêu cầu độ ổn định cao.

## 3 từ khóa mô tả bối cảnh enterprise của chủ đề
- Legacy systems
- On‑prem + Edge
- Reliability