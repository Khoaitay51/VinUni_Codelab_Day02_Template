# Lab 02 — Problem Scan: Vin Smart Future (GSM / Xanh SM)

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

---

### 📝 List bài toán của tôi:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Xanh SM (Vận hành tài xế)** | Stakeholder Pain | Tài xế tự quyết định di chuyển đến khu vực nào khi rảnh (idle) mà không có gợi ý dựa trên dự báo nhu cầu real-time, gây lãng phí pin và lệch cung - cầu cục bộ vào giờ cao điểm. |
| 2 | **Xanh SM (Trung tâm điều vận)** | Time-consuming | Điều phối viên xử lý thủ công các cuộc gọi khẩn cấp khi xe báo pin yếu (< 5%) hoặc sự cố sạc: phải tra cứu GPS, tìm trạm sạc VinFast còn trụ trống phù hợp và soạn tin hướng dẫn (mất 15-20 min/lượt). |
| 3 | **Xanh SM (CSKH & Đối soát)** | Repetitive | Nhân viên vận hành phải đọc và phân loại thủ công hàng nghìn khiếu nại, đánh giá 1-star (thái độ tài xế, mùi xe, tính cước, hủy chuyến) để gán tag phòng ban xử lý. |
| 4 | **Xanh SM (Tổng đài đặt xe)** | AI-upgrade | Khách hàng mô tả điểm đón bằng ngôn ngữ tự nhiên địa phương phức tạp (ngõ ngách, mốc dân gian), hệ thống chatbot rule-based hiện tại không hiểu, phải chuyển điện thoại viên xử lý thủ công. |
| 5 | **Xanh SM (QA & Vận hành)** | Time-consuming | Chuyên viên QA phải nghe lại thủ công mẫu ghi âm cuộc gọi hủy chuyến và đọc ghi chú tài xế để phân tích pattern nguyên nhân thực tế gây rò rỉ cuốc xe (khách chờ lâu, tài xế ép hủy...). |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

**Top 3 được chọn:**
* **#1 (Card #1):** Xanh SM — Xử lý sự cố sạc pin / hết pin thực địa khẩn cấp (Lens: Time-consuming)
* **#2 (Card #2):** Xanh SM — Phân loại & Tự động draft phản hồi khiếu nại, đánh giá 1-star (Lens: Repetitive)
* **#3 (Card #3):** Xanh SM — Chuẩn hóa điểm đón khách từ mô tả ngôn ngữ tự nhiên (Lens: AI-upgrade)

---

### 🃏 QUICK PROBLEM CARD #1

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Điều phối viên hỗ trợ xử lý sự cố tài xế cạn pin   │
│ (< 5%) hoặc lỗi trạm sạc giữa đường cần cứu hộ/trạm gần nhất │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên (Dispatcher) và Tài xế  │
│ Xanh SM đang kẹt trên đường.                                │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài báo sự cố hết pin/lỗi trạm          │
│   ──> 2. Điều phối viên tra cứu thủ công toạ độ GPS xe      │
│   ──> 3. Tra cứu dashboard trạm sạc VinFast tìm trụ trống   │
│   ──> 4. Soạn tin nhắn hướng dẫn/lộ trình gửi App tài xế    │
│   ──> 5. Liên hệ đội xe cứu hộ pin lưu động nếu pin < 5%    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 12-15 min/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Tự động pull vị trí, lọc trạm trống tương thích cổng sạc,  │
│ draft tin nhắn và tự trigger cứu hộ pin nếu dưới ngưỡng 5%)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian xử lý sự cố từ 15 min ──> dưới 3 min/lượt;│
│    độ chính xác trạm sạc gợi ý đạt ≥ 98%."                  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### 🃏 QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phân loại tự động và draft phản hồi khiếu nại,    │
│ đánh giá 1-star của khách hàng trên ứng dụng Xanh SM.       │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Chuyên viên CSKH Xanh SM (quá tải)     │
│ và Khách hàng (bực bội vì chờ đợi phản hồi lâu).            │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gửi đánh giá 1-sao / khiếu nại trên app          │
│   ──> 2. Chuyên viên đọc nội dung text và xem log cuốc xe    │
│   ──> 3. Phân loại thủ công nguyên nhân (thái độ/cước/xe)    │
│   ──> 4. Tra cứu quy định đền bù/voucher tương ứng           │
│   ──> 5. Viết tin nhắn xin lỗi và gửi cho khách duyệt        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 8-10 min/ticket)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3, 5          │
│ (Phân tích sentiment, trích xuất lý do chính, gợi ý tag và  │
│ sinh draft phản hồi xin lỗi lịch sự, đồng cảm)              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian phản hồi từ 4 giờ ──> dưới 15 phút;       │
│    90% ticket được phân loại đúng ngay lần đầu."             │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### 🃏 QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Chuẩn hóa và định vị điểm đón từ mô tả ngôn ngữ    │
│ tự nhiên địa phương của khách hàng khi đặt xe Xanh SM.      │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng (khó ghim điểm đón ngõ nhỏ), │
│ Tài xế (mất công gọi hỏi đường) và Điện thoại viên tổng đài.│
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gọi/nhập mô tả đón (VD: 'Cổng sau chợ, đối diện')│
│   ──> 2. Điện thoại viên lắng nghe và hình dung vị trí       │
│   ──> 3. Tra cứu thủ công bản đồ tìm điểm đón xe vào được    │
│   ──> 4. Nhập địa chỉ chuẩn hóa vào hệ thống điều xe        │
│   ──> 5. Tài xế nhận cuốc và phải gọi lại xác nhận vị trí   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 3-5 min/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Trích xuất thực thể địa điểm, đối chiếu GIS nội bộ, gợi ý  │
│ toạ độ đón xe tối ưu nhất cho cả khách và tài xế)           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian chốt điểm đón từ 3 min ──> dưới 30s;      │
│    giảm 50% số cuộc gọi tài xế phải hỏi lại đường đón."      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
