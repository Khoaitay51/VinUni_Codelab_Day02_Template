# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## Bài toán: Battery Inventory & Distribution Optimization for Battery Swap Stations - Tối ưu tồn kho và phân phối pin tại trạm đổi pin

## 3.1. Current-State Workflow

Quy trình kiểm tra tồn kho và điều phối pin giữa các trạm đổi pin VinFast hiện tại:

```text
┌──────────────────┐
│ Bước 1           │
│ Nhận thông tin   │
│ tồn kho từ các   │
│ trạm đổi pin     │
│                  │
│ Ai: Nhân viên    │
│ điều phối        │
│ ⏱ 3 phút         │
│ In: Báo cáo tồn  │
│ Out: Dữ liệu tồn │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Bước 2           │
│ Tổng hợp và kiểm │
│ tra số lượng pin │
│ từng trạm        │
│                  │
│ Ai: Nhân viên    │
│ điều phối        │
│ ⏱ 5 phút 🔴      │
│ In: Dữ liệu trạm │
│ Out: Bảng tồn kho│
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Bước 3           │
│ Xác định trạm    │
│ thiếu pin và     │
│ trạm dư pin      │
│                  │
│ Ai: Nhân viên    │
│ điều phối        │
│ ⏱ 10 phút 🔴     │
│ In: Bảng tồn kho │
│ Out: Danh sách   │
│ trạm cần hỗ trợ  │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Bước 4           │
│ Tính toán số pin │
│ cần điều chuyển  │
│ và lựa chọn      │
│ nguồn cung       │
│                  │
│ Ai: Nhân viên    │
│ điều phối        │
│ ⏱ 7 phút 🔴      │
│ In: Nhu cầu pin  │
│ Out: Kế hoạch    │
│ điều chuyển      │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Bước 5           │
│ Liên hệ nhân sự  │
│ vận chuyển và    │
│ xác nhận kế hoạch│
│                  │
│ Ai: Điều phối +  │
│ nhân viên trạm   │
│ ⏱ 10 phút        │
│ In: Kế hoạch     │
│ Out: Lệnh điều   │
│ chuyển pin       │
└──────────────────┘
```

**Chú thích:**
- 🔴 Bottleneck: Bước 2, 3 và 4 do phải tổng hợp, so sánh và tính toán thủ công.
- 🔄 Handoff: Bước 1 (trạm → điều phối), bước 5 (điều phối → nhân viên vận chuyển/trạm).
- ⏱ **Tổng thời gian xử lý thủ công ước tính: 35 phút/lượt.**


---

## 3.2. Problem Statement (6-field) — VinFast Battery Operations

| **Field** | **Nội dung** |
|---|---|
| **1. Actor / Operator** | Nhân viên điều phối vận hành mạng lưới trạm đổi pin xe máy điện VinFast, phối hợp với nhân viên quản lý trạm và đội ngũ vận chuyển pin. |
| **2. Current Workflow** | Nhân viên điều phối thu thập dữ liệu tồn kho từ các trạm, tổng hợp số lượng pin, xác định trạm thiếu hoặc dư pin, tính toán số lượng cần điều chuyển và liên hệ nhân viên vận chuyển để thực hiện. Quy trình hiện phụ thuộc nhiều vào báo cáo, bảng tính và thao tác kiểm tra thủ công. |
| **3. Bottleneck** | Bước 2, 3 và 4: Tổng hợp dữ liệu tồn kho từ nhiều trạm, xác định khu vực thiếu/dư pin và tính toán phương án điều chuyển phù hợp. Nhân viên phải đối chiếu nhiều nguồn dữ liệu, trong khi nhu cầu đổi pin có thể thay đổi theo thời gian và khu vực. |
| **4. Business Impact** | Việc điều phối chậm có thể khiến một số trạm thiếu pin trong giờ cao điểm, làm giảm khả năng phục vụ khách hàng và tăng thời gian chờ. Đồng thời, pin dư tại một số trạm có thể không được tận dụng hiệu quả, phát sinh chi phí vận chuyển và lưu kho không cần thiết. |
| **5. Success Metric** | 1. Giảm thời gian phân tích và lập kế hoạch điều phối từ 35 phút xuống dưới 5 phút/lượt. 2. Đạt ít nhất 95% độ chính xác trong việc xác định trạm có nguy cơ thiếu pin. 3. Giảm ít nhất 30% số lượt điều chuyển pin khẩn cấp so với baseline. |
| **6. Operational Boundary** | AI được phép đọc dữ liệu tồn kho, phân tích nhu cầu, dự báo nguy cơ thiếu pin và đề xuất phương án điều chuyển. AI có thể tự động tạo báo cáo và lệnh điều chuyển ở trạng thái nháp. **CẤM:** AI tự quyết định xuất/nhập pin, tự điều động phương tiện vận chuyển hoặc thay đổi dữ liệu tồn kho chính thức khi chưa được phê duyệt. Nhân viên điều phối phải duyệt kế hoạch trước khi thực hiện. |

---

## 3.3. Future-State Flow & AI Fit

### 1. AI-Fit Matrix

**Lựa chọn chính: Rule / State-Machine, kết hợp LLM Feature.**

**Lý do lựa chọn:**

Bài toán có quy trình tương đối cố định, đầu vào chủ yếu là dữ liệu có cấu trúc như số lượng pin, vị trí trạm, sức chứa và nhu cầu đổi pin.

- **Rule / State-Machine:** Phù hợp để kiểm tra ngưỡng tồn kho an toàn, xác định trạm thiếu/dư pin và kiểm soát trạng thái điều chuyển.
- **LLM Feature:** Hỗ trợ giải thích nguyên nhân cảnh báo, tổng hợp báo cáo và tạo bản nháp kế hoạch điều phối bằng ngôn ngữ tự nhiên.
- **Agentic Loop:** Chưa cần thiết ở giai đoạn đầu vì việc tự động ra quyết định và thực hiện điều chuyển có thể gây rủi ro vận hành.

**Giải pháp đề xuất:** Rule / State-Machine làm lõi xử lý, kết hợp LLM làm lớp hỗ trợ phân tích và giao tiếp.

### 2. Future-State Workflow

```text
┌──────────────────────┐
│ Bước 1               │
│ Thu thập dữ liệu     │
│ tồn kho tự động từ   │
│ các trạm đổi pin     │
│                      │
│ Hệ thống             │
│ 🔄 API / Database    │
│ In: Dữ liệu tồn kho  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Bước 2               │
│ 🔵 AI phân tích      │
│ tồn kho và dự báo    │
│ nhu cầu pin          │
│                      │
│ Input: Tồn kho, lịch │
│ sử, nhu cầu theo khu │
│ vực và thời gian     │
│                      │
│ Output: Trạm thiếu/  │
│ dư pin, cảnh báo     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Bước 3               │
│ 🔵 Engine tính toán  │
│ phương án điều phối  │
│                      │
│ Kiểm tra số pin cần  │
│ bổ sung, nguồn cung  │
│ và chi phí vận chuyển│
│                      │
│ Output: Kế hoạch     │
│ điều chuyển tối ưu   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Bước 4               │
│ 🔵 LLM tạo báo cáo   │
│ và giải thích đề xuất│
│                      │
│ Output: Bản nháp     │
│ kế hoạch điều phối   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Bước 5               │
│ 🟢 Human-in-the-loop │
│                      │
│ Nhân viên điều phối  │
│ kiểm tra và phê duyệt│
│ kế hoạch             │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Bước 6               │
│ Thực hiện điều       │
│ chuyển pin           │
│                      │
│ Nhân viên vận chuyển │
│ và nhân viên trạm    │
│ xác nhận hoàn thành  │
└──────────────────────┘

↩️ FALLBACK:
- Nếu dữ liệu tồn kho thiếu hoặc không đồng bộ:
  Chuyển sang kiểm tra thủ công với nhân viên trạm.
- Nếu AI dự báo không đủ độ tin cậy:
  Sử dụng ngưỡng tồn kho an toàn và quy tắc
  điều phối cố định.
- Nếu phương án điều chuyển không hợp lệ:
  Không tạo lệnh điều chuyển, chuyển cho
  nhân viên điều phối xử lý thủ công.
```

# 🏁 Phase 5 — EVALUATE

## AI Readiness Checklist

### 1. Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?

- ⚠️ **CÓ ĐIỀU KIỆN:** Bài toán có thể sử dụng các nguồn dữ liệu vận hành như:
  - Lịch sử giao dịch đổi pin tại từng trạm.
  - Số lượng pin tồn kho và trạng thái pin tại mỗi trạm.
  - Vị trí địa lý và sức chứa của các trạm.
  - Lịch sử điều chuyển pin giữa các trạm.
  - Nhu cầu đổi pin theo khu vực và khung giờ.

- **Kế hoạch chuẩn bị dữ liệu:** Thu thập dữ liệu lịch sử từ khoảng 10–20 trạm trong ít nhất 4 tuần để xây dựng bộ dữ liệu thử nghiệm. Nếu chưa được cấp quyền truy cập dữ liệu nội bộ, có thể sử dụng dữ liệu mô phỏng để kiểm tra logic prototype trước.

- ⚠️ **Lưu ý:** Cần kiểm tra tính đầy đủ, đồng bộ của dữ liệu tồn kho và loại bỏ thông tin nhạy cảm không cần thiết trước khi đưa vào hệ thống AI.

### 2. Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?

- ✅ **CÓ:** AI chỉ đóng vai trò phân tích và đề xuất phương án điều chuyển pin. Mọi kế hoạch đều phải được nhân viên điều phối kiểm tra và phê duyệt trước khi thực hiện (HITL bắt buộc).

- **Fallback:**
  - Nếu dữ liệu tồn kho thiếu hoặc không đồng bộ: Hệ thống yêu cầu nhân viên trạm xác nhận lại.
  - Nếu AI dự báo không đủ độ tin cậy: Sử dụng ngưỡng tồn kho an toàn và quy tắc điều phối cố định.
  - Nếu phương án điều chuyển không hợp lệ: Không tạo lệnh điều chuyển và chuyển sang xử lý thủ công.

- ⚠️ **Rủi ro nhất:** AI dự báo sai nhu cầu, khiến một trạm bị thiếu pin trong giờ cao điểm hoặc đề xuất điều chuyển không hợp lý, gây phát sinh chi phí vận chuyển.

- **Giải pháp:** Thiết lập mức tồn kho an toàn cho từng trạm, kiểm tra tính hợp lệ của phương án bằng Rule Engine và bắt buộc nhân viên điều phối phê duyệt trước khi thực hiện.

### 3. Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

- ⚠️ **CẦN XÁC NHẬN:** Các bên liên quan trực tiếp gồm:
  - Nhân viên điều phối vận hành mạng lưới trạm đổi pin.
  - Nhân viên quản lý trạm đổi pin.
  - Đội ngũ vận chuyển và điều phối pin.

- **Lợi ích đối với stakeholders:**
  - Nhân viên điều phối giảm thời gian tổng hợp dữ liệu và lập kế hoạch.
  - Nhân viên trạm giảm nguy cơ thiếu pin và mất cân đối tồn kho.
  - Đội ngũ vận chuyển nhận được kế hoạch điều chuyển rõ ràng hơn.

- ⚠️ **Lưu ý:** Cần tổ chức một buổi hướng dẫn ngắn để nhân viên hiểu cách đọc cảnh báo, kiểm tra đề xuất AI và sử dụng chức năng phê duyệt hoặc từ chối kế hoạch.

---

## Quyết Định Của Ban Giám Đốc Vin Smart Future

### 🟢 GO (Bắt đầu xây dựng Prototype)

**Lựa chọn:** `[x] GO` — Bắt đầu phát triển với phạm vi thử nghiệm giới hạn.

**Phạm vi triển khai ban đầu:**

- Thu thập và phân tích dữ liệu tồn kho từ một nhóm trạm đổi pin.
- Dự báo nhu cầu đổi pin theo khu vực và khung giờ.
- Phát hiện các trạm có nguy cơ thiếu hoặc dư pin.
- Đề xuất phương án điều chuyển dựa trên tồn kho, nhu cầu và chi phí vận chuyển.
- Nhân viên điều phối kiểm tra và phê duyệt trước khi thực hiện.

Chưa triển khai chức năng tự động điều chuyển pin trên hệ thống vận hành thực tế.

---

## Justification (Lý Giải Quyết Định)

### ✅ Lý Do GO

**1. Bài toán rõ ràng, có thể chuẩn bị dữ liệu để thử nghiệm:**

- Các đầu vào cần thiết đều là những loại dữ liệu vận hành có thể xác định rõ: tồn kho, lịch sử đổi pin, vị trí trạm và lịch sử điều chuyển.
- Có thể xây dựng bộ dữ liệu thử nghiệm từ dữ liệu lịch sử hoặc dữ liệu mô phỏng.
- Không cần triển khai hệ thống AI tự trị ngay từ đầu. Có thể bắt đầu bằng mô hình dự báo nhu cầu kết hợp Rule Engine để đề xuất phương án điều chuyển.

**2. Business Impact có thể đo lường:**

- Quy trình hiện tại mất khoảng **35 phút/lượt** theo baseline giả định ở Phase 3.
- Mục tiêu giảm thời gian phân tích và lập kế hoạch xuống dưới **5 phút/lượt**.
- Giảm tình trạng mất cân đối tồn kho giữa các trạm.
- Hạn chế các lượt điều chuyển khẩn cấp và chi phí vận chuyển không cần thiết.
- Cải thiện khả năng đáp ứng nhu cầu đổi pin của khách hàng trong giờ cao điểm.

**3. Rủi ro có thể kiểm soát bằng HITL + Fallback:**

- AI chỉ đưa ra dự báo và đề xuất, không trực tiếp thực hiện điều chuyển pin.
- Mọi kế hoạch đều phải được nhân viên điều phối phê duyệt.
- Rule Engine kiểm tra các điều kiện như tồn kho tối thiểu, sức chứa và tính khả thi của phương án.
- Khi dữ liệu không đầy đủ hoặc dự báo không đáng tin cậy, hệ thống chuyển về quy trình xử lý thủ công.

**4. Có khả năng mở rộng trong vận hành:**

- Có thể thử nghiệm trước trên một nhóm trạm đổi pin.
- Khi kết quả ổn định, mở rộng sang các khu vực có nhiều trạm hơn.
- Có thể tích hợp thêm dữ liệu giao thông, chi phí vận chuyển và tình trạng pin để cải thiện chất lượng đề xuất.
- Không yêu cầu thay đổi hoàn toàn quy trình vận hành hiện tại vì nhân viên vẫn giữ quyền quyết định cuối cùng.

---

## ⚠️ Điều Kiện GO

### 1. Điều kiện kỹ thuật

- Có bộ dữ liệu lịch sử đủ chất lượng để kiểm tra và đánh giá mô hình dự báo nhu cầu.
- Mô hình phải đạt mục tiêu **≥90% Recall** trong việc phát hiện các trường hợp trạm có nguy cơ thiếu pin trên tập kiểm thử có gán nhãn.
- Thời gian phân tích và tạo phương án điều chuyển phải **≤5 phút/lượt**.
- Rule Engine phải kiểm tra được các ràng buộc về số lượng pin, tồn kho an toàn và sức chứa của trạm.
- Fallback phải hoạt động đúng khi dữ liệu thiếu, lỗi hoặc kết quả dự báo không đạt ngưỡng tin cậy.

### 2. Điều kiện tổ chức

- Được cấp quyền truy cập dữ liệu tồn kho và lịch sử đổi pin phục vụ thử nghiệm.
- Có sự phối hợp của nhân viên điều phối và nhân viên quản lý trạm.
- Có ít nhất một nhân viên điều phối tham gia kiểm tra và đánh giá các phương án AI đề xuất.
- Nhân viên vận hành được hướng dẫn cách sử dụng hệ thống và xử lý các trường hợp AI dự báo sai.

### 3. Milestone Phase 4 (Prototype)

- **Ngày 1–2:** Chuẩn bị dữ liệu lịch sử hoặc dữ liệu mô phỏng, làm sạch và chuẩn hóa dữ liệu tồn kho, giao dịch đổi pin.
- **Ngày 3–4:** Xây dựng mô hình dự báo nhu cầu pin và Rule Engine để phát hiện trạm thiếu/dư pin.
- **Ngày 5–6:** Xây dựng chức năng đề xuất phương án điều chuyển, kiểm tra ràng buộc và tạo báo cáo giải thích.
- **Ngày 7:** Kiểm thử trên dữ liệu chưa sử dụng trong quá trình phát triển, đánh giá độ chính xác và thời gian xử lý.

**Tiêu chí quyết định sau Prototype:**

Nếu mô hình đạt các ngưỡng đánh giá, phương án điều chuyển hợp lệ và nhân viên điều phối xác nhận kết quả hữu ích, nhóm sẽ đề xuất chuyển sang thử nghiệm thực tế có giám sát (Pilot).

---

## ❌ Tại sao KHÔNG chọn "NOT YET" hoặc "NO-GO"?

- **NOT YET:** Chưa cần trì hoãn toàn bộ dự án vì có thể bắt đầu bằng dữ liệu mô phỏng và xây dựng prototype để kiểm chứng tính khả thi. Tuy nhiên, việc triển khai thực tế vẫn phải chờ xác nhận quyền truy cập dữ liệu và sự tham gia của stakeholders.

- **NO-GO:** Bài toán có quy trình rõ ràng, đầu vào và đầu ra có thể xác định, đồng thời có thể giải quyết bằng sự kết hợp giữa mô hình dự báo, Rule Engine và LLM. Rủi ro vận hành cũng được kiểm soát thông qua HITL và Fallback nên chưa có lý do để loại bỏ giải pháp ngay từ đầu.

**Kết luận:** Nhóm lựa chọn **GO** để xây dựng Prototype với phạm vi giới hạn. Mục tiêu trước mắt là chứng minh AI có thể dự báo nhu cầu, phát hiện nguy cơ thiếu pin và đề xuất phương án điều chuyển hiệu quả hơn quy trình thủ công, trước khi xem xét triển khai trên mạng lưới trạm đổi pin thực tế.