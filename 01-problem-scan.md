# 01 — Problem Scan: Vinhomes

2A2026-Trần Anh Duy-26ai.duyta@vinuni.edu.vn

> **Bối cảnh:** Tôi tìm một công việc cụ thể trong vận hành Vinhomes mà AI có thể hỗ trợ, rồi so với cách làm bằng quy tắc hoặc phần mềm thông thường trước khi chọn hướng triển khai.

# 🔍 Phase 1 — SCAN

## 4 lenses tìm bài toán

1. **Lặp lại:** cùng một loại việc phải làm nhiều lần.
2. **Tốn thời gian:** nhân viên phải đọc, tra cứu, đối chiếu hoặc theo dõi thủ công.
3. **AI có thể tốt hơn:** đầu vào là ngôn ngữ hoặc hình ảnh khó xử lý bằng biểu mẫu và quy tắc cố định.
4. **Pain từ người khác:** cư dân hoặc nhân viên thực địa phải chờ, hỏi lại hoặc làm lại việc.

## 📝 List bài toán của tôi

| # | Đơn vị / nghiệp vụ | Lens | Bài toán cần khảo sát |
|---|---|---|---|
| **1** | **Vinhomes — CSKH cư dân** | Tốn thời gian + AI có thể tốt hơn | Cư dân diễn đạt phản ánh theo nhiều cách. Nhân viên CSKH phải đọc, xác định mức khẩn cấp và chọn đúng bộ phận nhận việc; ticket thiếu thông tin có thể bị trả lại. |
| **2** | **Vinhomes — Đối soát thanh toán** | Lặp lại + Tốn thời gian | Kế toán phải đối chiếu hóa đơn với tiền đã nhận. Giao dịch thiếu mã, sai số tiền hoặc gộp nhiều khoản cần người kiểm tra. Phần lớn phép đối chiếu có thể làm bằng quy tắc. |
| **3** | **Vinhomes — Bảo trì thiết bị** | Tốn thời gian + Pain từ người khác | Khi có sự cố, kỹ thuật viên tìm lịch sử bảo trì và tài liệu hướng dẫn của đúng thiết bị trước khi biết nên kiểm tra từ đâu. |
| **4** | **Vinhomes — Sự cố thang máy** | Tốn thời gian + Pain từ người khác | Nhân viên trực phải xác nhận tình hình, tìm kỹ thuật viên hoặc nhà thầu, báo cho cư dân và theo dõi phản hồi. Nếu người được gọi không nhận việc, họ phải đổi phương án. |
| **5** | **Vinhomes — Nghiệm thu căn hộ** | Lặp lại + Tốn thời gian | Trước khi bàn giao, nhân viên QA/QC kiểm tra từng hạng mục, chụp ảnh lỗi và lập biên bản cho nhà thầu. Checklist điện tử có thể giảm nhập liệu; AI hình ảnh chỉ phù hợp với lỗi nhìn thấy được. |

> **Giới hạn dữ liệu:** Đây là các giả thuyết để sàng lọc bài lab. Tôi chưa có log, số đo thời gian hoặc xác nhận quy trình từ Vinhomes. Mọi baseline và ngưỡng trong các card là ước tính hoặc mục tiêu thử nghiệm, không phải kết quả thực tế.

---

# 🃏 Phase 2 — QUICK-ASSESS

**Ba bài chọn từ SCAN:** #1 → Card #1 (phản ánh cư dân), #3 → Card #2 (tài liệu bảo trì), #4 → Card #3 (sự cố thang máy). Chúng có ba kiểu đầu ra khác nhau: nhãn ticket, câu trả lời kèm nguồn và đề xuất bước điều phối tiếp theo.

## Card #1 — Phân loại và chuyển phản ánh cư dân

| Trường | Nội dung |
|---|---|
| **Bài toán trong một câu** | Nhân viên CSKH mất thời gian đọc từng phản ánh để hiểu vấn đề, nhận ra trường hợp khẩn cấp và chuyển cho đúng bộ phận. |
| **Công ty / đơn vị** | Vinhomes — CSKH và ban quản lý tại một tòa nhà hoặc khu đô thị được chọn để khảo sát. |
| **Ai đang đau (Actor)?** | CSKH trực tiếp phân loại; bộ phận xử lý phải trả lại ticket chuyển sai; cư dân chờ được tiếp nhận. |
| **Workflow thủ công hiện tại** | **1.** Nhận phản ánh qua app hoặc hotline. **2.** Đọc nội dung. **3.** Xác định vấn đề, mức khẩn cấp và thông tin thiếu. **4.** Tạo ticket, chuyển bộ phận. **5.** Kiểm tra đã có người nhận. |
| **Bottleneck** | Bước **2–3** mất ước tính **3–5 phút/ticket**; toàn bộ khâu tiếp nhận và chuyển việc khoảng **5–8 phút**. Chưa đo tại Vinhomes. |
| **AI hỗ trợ ở bước nào?** | LLM đọc nội dung ở bước 2–3, đề xuất nhóm việc và mức ưu tiên. Quy tắc cố định kiểm tra dấu hiệu khẩn cấp và bảng phân công; CSKH duyệt trước khi chuyển ticket. |
| **Quick Architecture** | **[x] LLM Feature**, kết hợp rule và người duyệt. **[ ] Agent** vì luồng phân loại này chưa cần tự chọn nhiều bước hay gọi công cụ để thực hiện hành động. |

### Metric và cách kiểm chứng

- **Thời gian:** mục tiêu ít nhất **80% ticket đủ điều kiện** được phân loại và nhân viên xác nhận trong **dưới 1 phút**, tính cả thời gian đọc/sửa bản nháp. So với thời gian thủ công của cùng loại ticket.
- **Chất lượng:** mục tiêu ít nhất **95% nhãn AI đề xuất** đúng nhóm và **95% đề xuất bộ phận** đúng theo nhãn tham chiếu do người phụ trách kiểm tra; đo trước khi nhân viên sửa.
- **Khẩn cấp:** mục tiêu nhận ra ít nhất **99% tình huống khẩn cấp** trên bộ đánh giá đã gán nhãn. Báo cáo riêng cảnh báo nhầm; bộ test nhỏ không chứng minh được độ an toàn khi vận hành thật.
- **Cách thử:** tạo ticket giả lập để kiểm tra chức năng, sau đó xin ticket lịch sử đã ẩn thông tin cá nhân và được phép sử dụng. Chấm cùng tập mẫu bằng thủ công, rule-only và rule + LLM; ghi cả lỗi, thời gian duyệt và trường hợp phải làm lại.

**Ticket đủ điều kiện** là phản ánh dạng văn bản có nội dung để phân loại, thuộc bộ nhãn đã thống nhất và không có dấu hiệu phải chuyển ngay theo quy trình khẩn cấp. Các ticket bị loại khỏi phạm vi vẫn phải được đếm và báo cáo riêng.

### Ranh giới vận hành

AI chỉ tạo đề xuất dạng nháp; không tự gửi tin cho cư dân, không tự chuyển/đóng ticket. Không lặp lại số điện thoại, email hoặc địa chỉ căn hộ trong phần giải thích. Dấu hiệu cháy, khói, người mắc kẹt hoặc rò điện phải chuyển người trực theo quy trình khẩn cấp, không chờ LLM. Khi nội dung mơ hồ, CSKH phân loại thủ công.

---

## Card #2 — Tra cứu tài liệu bảo trì

| Trường | Nội dung |
|---|---|
| **Bài toán trong một câu** | Kỹ thuật viên phải tìm lịch sử sửa chữa và tài liệu ở nhiều chỗ trước khi chọn bước kiểm tra thiết bị. |
| **Công ty / đơn vị** | Vinhomes — đội kỹ thuật vận hành tại một tòa nhà/khu được khảo sát. |
| **Ai đang đau (Actor)?** | Kỹ thuật viên xử lý sự cố và trưởng bộ phận phải hỗ trợ tra cứu; cư dân có thể chờ lâu hơn. |
| **Workflow thủ công hiện tại** | **1.** Nhận sự cố. **2.** Tìm đúng mã thiết bị và lịch sử của nó. **3.** Đọc tài liệu, biên bản sửa chữa cũ. **4.** Đối chiếu để chọn chỗ cần kiểm tra. **5.** Đề xuất cách xử lý. |
| **Bottleneck** | Bước **2–3** mất ước tính **6–10 phút/lượt**; cả lượt tra cứu khoảng **10–15 phút**, chưa tính sửa chữa. Chưa đo thực tế. |
| **AI hỗ trợ ở bước nào?** | Tìm tài liệu theo mã thiết bị hoặc mô tả sự cố, tóm tắt phần liên quan và gợi ý checklist có trích nguồn. Kỹ thuật viên kiểm tra tài liệu rồi quyết định. |
| **Quick Architecture** | **[x] LLM Feature** với tìm kiếm/RAG; tra cứu trực tiếp theo mã khi dữ liệu có cấu trúc. **[ ] Agent** ở giai đoạn đầu. |

### Metric và cách kiểm chứng

- **Thời gian:** mục tiêu rút một lượt tra cứu từ mức ước tính **10–15 phút xuống dưới 3 phút**, tính cả thời gian kỹ thuật viên kiểm tra nguồn; không tính thời gian sửa chữa.
- **Tìm đúng nguồn:** với câu hỏi có đáp án do kỹ sư chuẩn bị, mục tiêu ít nhất **90%** tìm được tài liệu liên quan trong **5 kết quả đầu**.
- **Nội dung trả lời:** mục tiêu ít nhất **95% câu trả lời** đúng với tài liệu tham chiếu và có nguồn hợp lệ. Có trích nguồn chưa đủ để kết luận câu trả lời đúng.
- **Cách thử:** bắt đầu với một nhóm thiết bị, các tài liệu đã duyệt và câu hỏi về lỗi thường gặp, lỗi hiếm, tài liệu cũ, sai mã thiết bị. So với tìm kiếm theo mã/từ khóa trước khi dùng LLM.

### Ranh giới vận hành

AI không tự kết luận nguyên nhân hỏng hoặc chỉ đạo sửa chữa; tài liệu không rõ phiên bản hay mã thiết bị thì yêu cầu kỹ thuật viên kiểm tra. Không dùng hướng dẫn cho thiết bị khác loại. Nếu không có nguồn phù hợp, trả lời rằng chưa tìm được tài liệu và quay về tra cứu thủ công.

---

## Card #3 — Điều phối xử lý sự cố thang máy

| Trường | Nội dung |
|---|---|
| **Bài toán trong một câu** | Khi thang máy ngừng hoạt động, nhân viên trực phải phối hợp nhiều bên và đổi phương án nếu người được gọi không thể đến hoặc tình hình thay đổi. |
| **Công ty / đơn vị** | Vinhomes — ban quản lý tòa nhà và đội kỹ thuật/nhà thầu phụ trách thang máy. |
| **Ai đang đau (Actor)?** | Nhân viên trực theo sát từng cuộc gọi, kỹ thuật viên nhận việc gấp và cư dân chờ thông tin hoặc lối đi thay thế. |
| **Workflow thủ công hiện tại** | **1.** Nhận cảnh báo/cuộc gọi. **2.** Xác nhận thang và kiểm tra có người mắc kẹt. **3.** Xem lịch trực, liên hệ kỹ thuật viên hoặc nhà thầu. **4.** Báo tình trạng và hướng dẫn cư dân. **5.** Theo dõi, đổi người xử lý nếu cần và cập nhật khi thang hoạt động lại. |
| **Bottleneck** | Bước **3–5**, nhất là khi người được gọi không nhận việc. Thời gian từ xác nhận sự cố đến khi có người nhận xử lý tạm ước tính **10–20 phút**; cần đo thực tế. |
| **AI hỗ trợ ở bước nào?** | Agent được phép đọc lịch trực, thông tin thang và trạng thái ticket; đề xuất người xử lý, soạn thông báo, kiểm tra phản hồi rồi gợi ý bước tiếp theo. Mọi phân công/thông báo phải được nhân viên duyệt. |
| **Quick Architecture** | **[x] Agentic Loop** để thử quy trình nhiều bước và phản hồi thay đổi; rule xử lý cảnh báo an toàn. Chỉ nên dùng Agent nếu cách làm bằng workflow cố định không đáp ứng được. |

### Metric và cách kiểm chứng

- **Thời gian:** mục tiêu giảm từ mức ước tính **10–20 phút xuống dưới 5 phút** để có người nhận xử lý; không nhầm với thời gian sửa xong thang.
- **Phân công:** mục tiêu ít nhất **95% đề xuất** đúng người, đúng ca trực và đúng loại sự cố trên bộ tình huống được người vận hành xác nhận.
- **Khẩn cấp:** **100% tình huống có người mắc kẹt trong bộ test** phải đưa ngay đến người trực và quy trình cứu hộ; kết quả test không thay thế diễn tập an toàn thực tế.
- **Cách thử:** mô phỏng cả trường hợp kỹ thuật viên bận, không phản hồi, nhà thầu từ chối hoặc thông tin thang thay đổi. So với workflow có nhánh cố định, không chỉ so với làm tay.

### Ranh giới vận hành

Khi có người mắc kẹt, báo người trực và gọi lực lượng phụ trách theo quy trình đã duyệt; không chờ Agent. Agent không tự gửi thông báo, không tự phân công, không điều khiển thang máy và không tuyên bố sự cố đã kết thúc. Chưa có quyền truy cập dữ liệu lịch trực, trạng thái thang hay ticket thì chỉ mô phỏng được bài toán, không thể khẳng định Agent vận hành được.

---

## 🗳️ Vì sao chọn ba card này?

| Card | Lý do đưa vào Quick-Assess | Điều cần xác minh |
|---|---|---|
| **#1 — Phản ánh cư dân** | Đầu vào và đầu ra rõ, có thể so LLM với rule trên cùng tập ticket. | Quy trình CSKH, bộ nhãn, thời gian và quyền dùng dữ liệu. |
| **#2 — Tài liệu bảo trì** | Cho thấy AI hỗ trợ tìm và tóm tắt tri thức, khác với phân loại ticket. | Tài liệu đúng phiên bản, mã thiết bị và người đánh giá câu trả lời. |
| **#3 — Sự cố thang máy** | Có nhiều bước thay đổi theo phản hồi, phù hợp để kiểm tra khi nào Agent đáng dùng. | Dữ liệu thời gian thực, quyền công cụ, quy trình cứu hộ và người phê duyệt. |

**Hai bài chưa chọn làm card:** SCAN #2 nên thử SQL/rule đối soát trước vì một phép ghép sai có thể gây hậu quả tài chính; SCAN #5 nên chuẩn hóa checklist và dữ liệu ảnh trước khi đầu tư Computer Vision. Đây là ưu tiên để học và kiểm chứng trong lab, chưa phải quyết định đầu tư của Vinhomes.
