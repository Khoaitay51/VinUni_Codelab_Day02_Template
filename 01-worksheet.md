# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

2A2026-Trần Anh Duy-26ai.duyta@vinuni.edu.vn

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)


### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Vinhomes | Tốn thời gian / AI có thể tốt hơn | Mỗi phản ánh của cư dân lại được viết theo một cách khác nhau. Nhân viên CSKH phải đọc từng nội dung rồi xác định nên chuyển cho bộ phận nào. |
| 2 | Vinhomes | Lặp lại / Tốn thời gian | Kế toán đối chiếu hóa đơn với các khoản cư dân đã thanh toán. Những giao dịch thiếu hay không đúng nội dung, không đúng số tiền hoặc gộp nhiều khoản phải kiểm tra lại bằng tay. |
| 3 | Vinhomes | Tốn thời gian / Pain từ người khác | Khi có sự cố, kỹ thuật viên phải tìm lịch sử bảo trì và tài liệu hướng dẫn của thiết bị trước khi biết nên kiểm tra như thế nào. |
| 4 | Vinhomes | Tốn thời gian / Pain từ người khác | Khi thang máy gặp sự cố, nhân viên trực phải kiểm tra tình hình, tìm người xử lý, báo cho cư dân và theo dõi đến khi thang hoạt động lại. Nếu kỹ thuật viên bận hoặc sự cố thay đổi, họ phải điều phối lại từ đầu. |
| 5 | Vinhomes | Lặp lại / Tốn thời gian | Trước khi bàn giao căn hộ, nhân viên QA/QC kiểm tra từng hạng mục, chụp ảnh lỗi và ghi lại để báo cáo cho nhà thầu. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)


**QUICK PROBLEM CARD #1 — Phân loại và chuyển phản ánh cư dân**

- **Bài toán (1 câu):** Nhân viên CSKH mất thời gian đọc từng phản ánh của cư dân để xác định vấn đề và chuyển cho đúng bộ phận.
- **Công ty thành viên:** Vinhomes.
- **Ai đang đau (Actor)?** Chủ yếu là nhân viên CSKH; ban quản lý phải xử lý khi chuyển sai, còn cư dân phải chờ lâu hơn.
- **Workflow thủ công hiện tại (5 bước):** (1) Nhận phản ánh qua app hoặc hotline → (2) đọc nội dung → (3) xác định vấn đề có khẩn cấp không → (4) tạo ticket và chuyển cho bộ phận phụ trách → (5) theo dõi đến khi có người xử lý.
- **Bước tốn thời gian/lỗi nhất:** Bước 2–3. Tạm ước tính mất 3–5 phút/ticket; cả khâu tiếp nhận và chuyển việc mất khoảng 5–8 phút.
- **AI hỗ trợ ở bước nào?** Ở bước 2–3, AI đọc nội dung và gợi ý loại vấn đề, mức độ khẩn cấp. Việc chuyển ticket vẫn theo quy tắc có sẵn; nhân viên kiểm tra các trường hợp khó hoặc khẩn cấp.
- **Metric có số:** Mục tiêu đưa thời gian đọc và phân loại xuống dưới 1 phút/ticket. Trên bộ ticket được kiểm tra thủ công, ít nhất 95% ticket AI gán nhãn phải đúng nhóm, ít nhất 95% ticket được chuyển đúng bộ phận và ít nhất 99% phản ánh khẩn cấp phải được nhận ra.
- **Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent — Dùng LLM cho những phản ánh viết dài hoặc không rõ ý; các trường hợp quen thuộc xử lý bằng quy tắc. Nhân viên duyệt trước khi chuyển các ticket có rủi ro.

**QUICK PROBLEM CARD #2 — Tra cứu tài liệu bảo trì**

- **Bài toán (1 câu):** Khi có sự cố, kỹ thuật viên phải tìm lịch sử bảo trì và tài liệu hướng dẫn của thiết bị trước khi biết nên kiểm tra như thế nào.
- **Công ty thành viên:** Vinhomes.
- **Ai đang đau (Actor)?** Kỹ thuật viên trực tiếp xử lý sự cố; trưởng bộ phận kỹ thuật cũng mất thời gian khi phải hỗ trợ tra cứu.
- **Workflow thủ công hiện tại (5 bước):** (1) Nhận thông tin sự cố → (2) tìm lịch sử của thiết bị → (3) đọc tài liệu và các biên bản sửa chữa cũ → (4) đối chiếu để chọn chỗ cần kiểm tra → (5) đề xuất cách xử lý.
- **Bước tốn thời gian/lỗi nhất:** Bước 2–3. Tạm ước tính mất 6–10 phút; cả lượt tra cứu mất khoảng 10–15 phút.
- **AI hỗ trợ ở bước nào?** Bước 2–4: tìm đúng tài liệu theo mã thiết bị hoặc mô tả sự cố, tóm tắt những thông tin liên quan và gợi ý các bước kiểm tra. Kỹ thuật viên xem nguồn tài liệu rồi mới quyết định.
- **Metric có số:** Mục tiêu rút thời gian tra cứu từ 10–15 phút xuống dưới 3 phút/lượt. Với bộ câu hỏi do kỹ sư chuẩn bị, ít nhất 90% câu hỏi phải tìm được tài liệu đúng trong 5 kết quả đầu và ít nhất 95% câu trả lời phải chính xác, có nguồn để kiểm tra lại.
- **Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent — Tìm tài liệu trước, sau đó dùng LLM tóm tắt kèm nguồn. Dữ liệu có mã rõ ràng thì tra cứu trực tiếp; AI không tự quyết định cách sửa chữa.

**QUICK PROBLEM CARD #3 — Điều phối xử lý sự cố thang máy**

- **Bài toán (1 câu):** Khi thang máy ngừng hoạt động, nhân viên trực phải phối hợp nhiều bên và liên tục đổi phương án nếu người được gọi không thể đến hoặc tình hình tại tòa nhà thay đổi.
- **Công ty thành viên:** Vinhomes.
- **Ai đang đau (Actor)?** Nhân viên trực ban quản lý phải theo sát từng cuộc gọi; kỹ thuật viên nhận việc gấp, còn cư dân chờ thông tin và phương án đi lại.
- **Workflow thủ công hiện tại (5 bước):** (1) Nhận cảnh báo hoặc cuộc gọi báo hỏng → (2) xác nhận thang nào gặp sự cố, có người mắc kẹt hay không → (3) xem lịch trực và gọi kỹ thuật viên hoặc nhà thầu → (4) báo tình trạng, hướng dẫn cư dân dùng thang khác → (5) theo dõi phản hồi, đổi người xử lý nếu cần và cập nhật khi thang hoạt động lại.
- **Bước tốn thời gian/lỗi nhất:** Bước 3–5, nhất là khi người được gọi không nhận việc hoặc tình trạng thang thay đổi. Tạm ước tính mất 10–20 phút từ lúc xác nhận sự cố đến khi có người nhận xử lý.
- **AI hỗ trợ ở bước nào?** Ở bước 3–5, agent tra lịch trực, thông tin thang và tiến độ ticket qua các hệ thống được phép đọc; đề xuất người xử lý, soạn thông báo rồi theo dõi phản hồi. Nếu kỹ thuật viên không nhận việc, agent tìm phương án tiếp theo và trình nhân viên trực duyệt. Trường hợp có người mắc kẹt phải chuyển ngay cho người trực và quy trình cứu hộ, không chờ agent.
- **Metric có số:** Mục tiêu đưa thời gian từ lúc xác nhận sự cố đến khi có người nhận xử lý xuống dưới 5 phút, so với mức ước tính 10–20 phút. Ít nhất 95% đề xuất phân công phải đúng người/đúng ca trực; 100% tình huống có người mắc kẹt phải được báo ngay cho người trực. Đo trên các tình huống thử nghiệm, gồm cả trường hợp người được gọi từ chối hoặc không phản hồi.
- **Quick Architecture:** [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent — Cần một agent vì việc xử lý có nhiều bước, phải đọc nhiều hệ thống và chọn bước tiếp theo theo phản hồi mới. Quy tắc cố định xử lý cảnh báo an toàn; nhân viên trực duyệt mọi lệnh phân công và thông báo trước khi gửi. Cần xác nhận có dữ liệu lịch trực, tình trạng thang và phản hồi xử lý; sau đó thử so với workflow thông thường để xem agent có thật sự giúp ích.
