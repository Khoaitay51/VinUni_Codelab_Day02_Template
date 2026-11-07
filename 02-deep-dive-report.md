# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

**Bài toán đã chọn:** Quick Problem Card #4 — Tư vấn quyền lợi vé combo tại VinWonders Phú Quốc.

## 3.1. Current-State Workflow Mapping (25 min)

Khách hỏi vé combo có kèm suất ăn hay không. Nhân viên cần xác định đúng
gói vé, tra bảng quyền lợi rồi soạn câu trả lời.

![Quy trình tư vấn quyền lợi vé combo tại VinWonders Phú Quốc](04-workflow-diagram.png)

* 🔴 **Bottleneck:** Nếu khách chưa nêu rõ tên hoặc mã gói, nhân viên phải hỏi lại và xác định gói trước khi tra cứu. Bước này mất thêm khoảng 120 giây thao tác theo giả định.
* 🔄 **Handoff:** Khách gửi câu hỏi cho nhân viên. Hai bên trao đổi thêm nếu thiếu thông tin. Nhân viên gửi câu trả lời sau khi kiểm tra.
* **Tổng cộng = 1 phút 40 giây/lượt** khi đủ thông tin hoặc **3 phút 40 giây/lượt** nếu phải hỏi lại một lần.

Quy trình và thời gian trên là giả định để thiết kế bài đo. Tổng thời gian
chỉ tính thao tác của nhân viên, chưa tính thời gian chờ khách phản hồi.

## 3.2. Problem Statement (6-field) & Metrics (15 min)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên tư vấn vé combo tại VinWonders Phú Quốc. |
| **2. Current Workflow** | Nhân viên đọc câu hỏi, xác định tên hoặc mã gói, tra bảng quyền lợi, soạn và gửi phản hồi. Nếu thông tin chưa rõ, nhân viên hỏi lại khách. |
| **3. Bottleneck** | Khách có thể chỉ hỏi “vé này có kèm ăn không?” mà chưa nêu tên gói. Nhân viên phải xác minh thông tin trước khi tra quyền lợi. |
| **4. Business Impact** | Lượt hỏi lại có thể kéo dài thời gian tư vấn. Chọn nhầm gói có thể dẫn đến trả lời sai và phải đính chính. Cần đo thời gian thao tác, tỷ lệ lượt phải hỏi lại và số lần đính chính. |
| **5. Success Metric** | Mục tiêu thử nghiệm là thời gian thao tác trung vị ≤ 60 giây/câu, tính cả bước duyệt, và ≥ 95% phản hồi đạt đáp án trên bộ câu hỏi mẫu. Với câu thiếu thông tin, phản hồi đúng phải hỏi lại trước khi xác nhận quyền lợi. |
| **6. Operational Boundary** | LLM được trích xuất thông tin gói và soạn nháp từ dữ liệu đã tra. Khi thiếu thông tin về gói, LLM soạn câu hỏi làm rõ. Khi bảng thiếu dữ liệu, chuyển nhân viên kiểm tra. Nhân viên duyệt và gửi phản hồi. |

## 3.3. Future-State Flow & AI Fit (25 min)

**Xác định mức AI Fit (AI-Fit Matrix)**

- [x] **Rule / State-Machine** — Đối chiếu mã gói và tra quyền lợi trong bảng.
- [x] **LLM Feature** — Đọc câu hỏi tiếng Việt tự do và soạn nháp hoặc câu hỏi làm rõ.
- [ ] **Agentic Loop** — Luồng xử lý có các bước cố định, với quyền gửi phản hồi thuộc về nhân viên.

**Future-State Flow**

1. Khách gửi câu hỏi về quyền lợi vé.
2. 🔵 **AI Step:** LLM trích xuất tên hoặc mã gói được nêu trong câu hỏi.
3. **Rule:** Chương trình đối chiếu gói với bảng quyền lợi.
4. 🔵 **AI Step:** Nếu đủ thông tin, LLM soạn nháp từ dữ liệu đã tra. Nếu chưa rõ gói, LLM soạn câu hỏi làm rõ. Bản nháp bắt đầu bằng `[DRAFT_ONLY]`.
5. 🟢 **Human Step (HITL):** Nhân viên kiểm tra đúng gói và quyền lợi, sửa nếu cần rồi gửi.
6. ↩️ **Fallback:** Nếu bảng thiếu dữ liệu, Gemini gặp lỗi hoặc đầu ra sai định dạng, nhân viên tra cứu và trả lời thủ công.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:

- [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?** Có bảng giả lập: `GOI_A` gồm vé vào cửa, `GOI_B` gồm vé vào cửa và suất ăn.
- [ ] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?** Đã thiết kế bước duyệt và phương án thủ công. Cần chạy thử các tình huống khách yêu cầu xác nhận sai quyền lợi, bỏ thẻ nháp hoặc trả lời khi chưa rõ gói.
- [ ] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?** Cần xác nhận với nhân viên tư vấn xem bước duyệt bản nháp có phù hợp với quy trình làm việc hay không.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

- [ ] **GO (Bắt đầu xây dựng Prototype)**
- [x] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline)**
- [ ] **NO-GO (Không khả thi / Rule-based tốt hơn)**

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí)**

> Có thể thử prototype với bảng quyền lợi giả lập. Trước khi thử trong
> quy trình tư vấn thật, cần xác nhận bảng quyền lợi và so sánh thời gian
> xử lý thủ công với thời gian xử lý có AI trên cùng bộ câu hỏi. Phép đo
> phải tính cả thời gian nhân viên duyệt và sửa nháp. Kết quả sẽ cho biết
> thời gian tiết kiệm có đủ bù công cập nhật bảng và chi phí gọi Gemini hay không.

**Kiểm tra cục bộ bằng phản hồi mô phỏng** — 4/4 ca đi qua luồng kiểm
tra với trạng thái và mã gói mong đợi. Bộ xác thực từ chối 4 phản hồi
không hợp lệ gồm sai vị trí thẻ nháp, sai quyền lợi, gán mã cho ca cần hỏi
lại và JSON lỗi.

**Kiểm tra trên Gemini** — Chưa có kết quả. Các phản hồi mô phỏng dùng
để kiểm tra code và chưa đánh giá khả năng giữ ranh giới của mô hình.