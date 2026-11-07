# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.


### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | VinWonders Phú Quốc | Pain từ người khác | Gia đình có trẻ nhỏ có thể đi bộ và xếp hàng rồi mới biết trẻ chưa đủ chiều cao để tham gia trò chơi. |
| 2 | VinWonders Phú Quốc | AI có thể tốt hơn | Khi trò ngoài trời tạm dừng vì thời tiết xấu, khách phải tìm hoạt động thay thế còn mở, ở gần và phù hợp với cả nhóm. |
| 3 | VinWonders Phú Quốc | Tốn thời gian | Khách phải đối chiếu lịch biểu diễn với bản đồ và tự ước lượng thời gian đi bộ, nên có thể đến muộn hoặc lỡ show. |
| 4 | VinWonders Phú Quốc | Lặp lại | Nhân viên tư vấn phải xác định đúng gói vé và tra lại quyền lợi mỗi khi khách hỏi combo có kèm suất ăn hoặc dịch vụ khác hay không. |
| 5 | VinWonders Phú Quốc | Tốn thời gian | Khách chưa mở sẵn mã QR có thể phải tìm lại email hoặc thông tin đặt vé ngay tại cổng, làm chậm lượt soát vé. |

*Các điểm nghẽn là giả thuyết cần khảo sát. Thời gian trong các thẻ là ước tính ban đầu, còn KPI là mục tiêu thử nghiệm.*

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Gia đình có thể xếp hàng rồi mới biết      │
│ trẻ chưa đủ chiều cao để chơi.                              │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes   │
│                     [ ] Vinmec   [x] Khác: VinWonders       │
│                                                             │
│ Ai đang đau (Actor)? Phụ huynh đi cùng trẻ nhỏ.              │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Chọn trò ──> 2. Đi tới khu trò chơi                    │
│   ──> 3. Xếp hàng và kiểm tra chiều cao tại lối vào          │
│   ──> 4. Vào chơi hoặc chọn trò khác                        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 nếu phát hiện trẻ    │
│ chưa đủ chiều cao sau khi xếp hàng (⏱ thêm 10 phút/lượt).    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1: dùng Rule      │
│ lọc trò theo chiều cao và hiện các điều kiện đi kèm.         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ ≥ 90% phụ huynh trong bài thử tìm được một trò phù hợp       │
│ với chiều cao của trẻ trong vòng 2 phút.                    │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Khách có thể lỡ show vì ước lượng sai      │
│ thời gian đi bộ giữa các phân khu.                          │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes   │
│                     [ ] Vinmec   [x] Khác: VinWonders       │
│                                                             │
│ Ai đang đau (Actor)? Khách tham quan lần đầu muốn xem show. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xem lịch diễn ──> 2. Tìm điểm diễn trên bản đồ          │
│   ──> 3. Ước lượng giờ cần xuất phát                        │
│   ──> 4. Đi bộ đến điểm biểu diễn                           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3, tự đối chiếu giờ    │
│ diễn với vị trí và quãng đường (⏱ 5 phút/lượt).             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3: dùng Rule     │
│ tính giờ xuất phát từ lịch diễn và thời gian đi dự kiến.    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ ≥ 90% người tham gia bài thử chọn được giờ xuất phát        │
│ phù hợp trong vòng 2 phút.                                  │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│ Bài toán (1 câu): Nhân viên tư vấn mất thêm thời gian        │
│ xác định quyền lợi khi khách hỏi mà chưa nêu rõ gói vé.      │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes   │
│                     [ ] Vinmec   [x] Khác: VinWonders       │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên tư vấn vé và khách hỏi combo.│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đọc câu hỏi ──> 2. Xác định gói vé, hỏi lại nếu thiếu  │
│   ──> 3. Tra bảng quyền lợi ──> 4. Soạn và gửi câu trả lời  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 nếu khách chưa nêu   │
│ rõ tên gói (⏱ thêm 2 phút thao tác/lượt phải hỏi lại).       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 4: nhận      │
│ diện gói hoặc soạn câu hỏi làm rõ, rồi soạn nháp từ quyền    │
│ lợi do Rule tra được. Nhân viên duyệt trước khi gửi.         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Thời gian thao tác trung vị ≤ 60 giây/câu và ≥ 95% bản      │
│ nháp khớp với bảng quyền lợi trên bộ câu hỏi mẫu.            │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> *"Hãy phản biện Quick Problem Card #4 về tư vấn quyền lợi vé combo tại VinWonders Phú Quốc. Chỉ ra 3 điểm yếu về quy trình, cách đo KPI và dữ liệu. Bước nào chỉ cần Rule? LLM có ích ở đâu khi khách mô tả gói vé không rõ?"*

---