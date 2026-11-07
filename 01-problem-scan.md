# 01 — Problem Scan: Vinmec

> **Bối cảnh:** Tôi đóng vai AI Product Engineer tại Vin Smart Future, tìm kiếm các cơ hội cải thiện vận hành và trải nghiệm bệnh nhân tại Vinmec.

# 🔍 Phase 1 — SCAN

## 4 Lenses tìm bài toán

1. **Lặp lại (Repetitive):** Công việc phải đọc, phân loại, nhập hoặc đối chiếu nhiều lần.
2. **Tốn thời gian (Time-consuming):** Các bước thủ công khiến nhân viên mất nhiều thời gian hoặc bệnh nhân phải chờ.
3. **AI có thể tốt hơn (AI-upgrade):** Nội dung đa ngôn ngữ hoặc phi cấu trúc mà công cụ hiện tại xử lý chưa thuận tiện.
4. **Pain từ người khác (Stakeholder Pain):** Khó khăn của bệnh nhân, người nhà, nhân viên hành chính hoặc đơn vị vận hành.

## 📝 List bài toán của tôi

| #   | Đơn vị                                                  | Lens                                   | Mô tả ngắn bài toán                                                                                                                                                                                                                                                                                                                                   |
| --- | ------------------------------------------------------- | -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Vinmec — Hỗ trợ bệnh nhân quốc tế**                   | AI-upgrade + Stakeholder Pain | Bệnh nhân quốc tế gặp khó khăn khi tìm hiểu bằng ngôn ngữ phù hợp về đặt lịch, thủ tục, giấy tờ, chi phí dự kiến và thanh toán. Nhân viên phải giải thích lại và chuyển tiếp giữa nhiều đầu mối. Cơ hội là hỗ trợ hỏi đáp đa ngôn ngữ dựa trên tài liệu được duyệt, có một đầu mối chuyển tiếp rõ ràng; giới hạn ở thủ tục và thông tin phi lâm sàng. |
| 2   | **Vinmec — Chuẩn hóa feedback trải nghiệm bệnh nhân**   | Repetitive + Time-consuming                | Feedback từ khảo sát, ứng dụng hoặc các kênh được phép sử dụng có cách diễn đạt khác nhau, có thể bằng tiếng Việt hoặc tiếng Anh. Nhân viên phải đọc, gắn chủ đề, xác định bộ phận và tổng hợp vấn đề lặp lại để đề xuất cải tiến. Cơ hội là chuẩn hóa phản hồi thành bản nháp có cấu trúc để con người duyệt và giao việc.                           |
| 3   | **Vinmec — Dự báo và phân bổ nguồn lực**                | Time-consuming + Stakeholder Pain     | Lượng bệnh nhân biến động theo khung giờ, mùa và loại dịch vụ, có thể khiến một số khu vực quá tải trong khi nguồn lực khác chưa được sử dụng hiệu quả. Cơ hội là dự báo nhu cầu và hỗ trợ lập kế hoạch slot hoặc nhân sự bằng mô hình dự báo kết hợp quy tắc; quyết định cuối thuộc người phụ trách vận hành.                                        |
| 4   | **Vinmec — Chuẩn hóa hồ sơ và giảm nhập liệu thủ công** | Repetitive + Time-consuming                | Nhân viên phải đọc, nhập lại và đối chiếu thông tin trên nhiều loại hồ sơ, phiếu thu, hóa đơn hoặc chứng từ bảo hiểm. Dữ liệu phân tán và định dạng khác nhau có thể dẫn đến nhập sai hoặc thiếu trường. Cơ hội là trích xuất thành bản nháp có dẫn vị trí nguồn, kiểm tra bằng rule và yêu cầu xác nhận trước khi ghi vào hệ thống.                  |
| 5   | **Vinmec — Giảm thời gian chờ ngoại trú**               | Time-consuming + Stakeholder Pain     | Bệnh nhân phải đi qua nhiều điểm như đăng ký, thanh toán, xét nghiệm và quay lại gặp bác sĩ; thiếu thông tin về bước tiếp theo có thể làm tăng chờ đợi và số lần hỏi lễ tân. Cơ hội là tối ưu workflow và hướng dẫn theo trạng thái đã được hệ thống xác nhận tại một cơ sở pilot, giữ nguyên chỉ định và quyết định lâm sàng.                        |

> **Lưu ý về dữ liệu:** Chưa có log, bộ dữ liệu đã gán nhãn hoặc số đo baseline thực tế đi kèm file nguồn. Có thể dùng dữ liệu giả lập để kiểm tra chức năng trong lab, nhưng dữ liệu đó không chứng minh hiệu quả thực tế. Mỗi baseline và target trong Phase 2 là giả định hoặc mục tiêu pilot cần đo lại.

---

# 🃏 Phase 2 — QUICK-ASSESS

**Top 3 đã chọn từ danh sách SCAN:**

1. **SCAN #2 — Chuẩn hóa feedback trải nghiệm bệnh nhân.**
2. **SCAN #4 — Chuẩn hóa hồ sơ, thu hẹp vào chứng từ hành chính.**
3. **SCAN #1 — Hỗ trợ bệnh nhân quốc tế, thu hẹp vào hỏi đáp thủ tục.**

Tôi ưu tiên workflow rõ, phạm vi có thể kiểm soát, khả năng tạo bộ test nhỏ và sự phù hợp giữa kỹ thuật với bài toán. Việc có dữ liệu thật và người phụ trách xác nhận vẫn là điều kiện cần kiểm chứng.

## Card #1 — Vinmec: Chuẩn hóa và phân loại feedback trải nghiệm bệnh nhân

| Trường                         | Nội dung                                                                                                                                                                                                                                                                                      |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán trong một câu**     | Nhân viên chăm sóc khách hàng phải đọc và phân loại thủ công feedback tiếng Việt/Anh trước khi chuyển đến bộ phận phù hợp, làm chậm việc tổng hợp các vấn đề cần cải tiến.                                                                                                                    |
| **Công ty / đơn vị**           | Vinmec — bộ phận Chăm sóc khách hàng và Quản lý chất lượng tại một cơ sở pilot.                                                                                                                                                                                                               |
| **Ai đang đau (Actor)?**       | Nhân viên đọc và tổng hợp feedback; quản lý chất lượng cần biết vấn đề lặp lại; bệnh nhân chờ được tiếp nhận phản ánh.                                                                                                                                                                        |
| **Workflow thủ công hiện tại** | **1.** Nhận feedback từ kênh được phép sử dụng. **2.** Đọc và gắn chủ đề, sắc thái phản hồi, bộ phận cần xem xét. **3.** Nhóm các phản hồi tương tự để nhận diện vấn đề lặp lại. **4.** Soạn ticket hoặc bản tổng hợp, giao người phụ trách. **5.** Theo dõi phản hồi và cập nhật trạng thái. |
| **Bottleneck**                 | Bước **2–3**: cách diễn đạt đa dạng, một phản hồi có thể chứa nhiều vấn đề. **Baseline giả định:** 120 giây/feedback cho việc đọc và gắn nhãn. Chưa đo tại Vinmec.                                                                                                                            |
| **AI hỗ trợ ở bước nào?**      | LLM hỗ trợ bước **2–3** bằng cách đề xuất nhiều nhãn chủ đề, trích đoạn làm bằng chứng, gợi ý bộ phận và nhóm phản hồi tương tự. Workflow tạo bản nháp ticket ở bước 4; nhân viên kiểm tra trước khi giao việc. Giả thuyết nguyên nhân chỉ được trình bày là đề xuất cần xác minh.            |
| **Quick Architecture**         | **Rule + LLM Feature + workflow duyệt.** Rule kiểm tra trường bắt buộc và kích hoạt chuyển người xử lý theo tiêu chí đã thống nhất; LLM đọc ngôn ngữ tự nhiên. Chưa cần Agent tự chủ.                                                                                                         |

### Metric và cách kiểm chứng

- **Thời gian xử lý:** mục tiêu giảm từ baseline giả định **120 giây xuống không quá 45 giây/feedback**, tính cả thời gian nhân viên duyệt và sửa bản nháp.
- **Chất lượng phân loại:** mục tiêu ít nhất **90% feedback** được gán đầy đủ chủ đề và đúng bộ phận theo bộ nhãn tham chiếu do người phụ trách duyệt.
- **Kiểm tra chuyển tiếp ưu tiên:** trên bộ test đã định nghĩa, **100% trường hợp được gán nhãn cần xử lý ưu tiên** phải được chuyển cho nhân viên xem xét. Đây là tiêu chí kiểm thử, chưa phải cam kết ngoài thực tế.
- **Cách đo pilot:** dùng khoảng **50 feedback giả lập/đã khử nhận diện và được phép dùng**, gồm phản hồi đa chủ đề, mỉa mai, phủ định, Việt/Anh và trường hợp thiếu thông tin. So sánh cùng bộ mẫu khi làm thủ công và khi có AI; ghi riêng kết quả của dữ liệu giả lập.

### Ranh giới vận hành

AI chỉ đề xuất nhãn, bằng chứng và bản nháp ticket; không tự đăng review, phản hồi công khai hoặc kết luận trách nhiệm của cá nhân. Với dấu hiệu liên quan đến an toàn hoặc nội dung lâm sàng, chuyển nhân viên phụ trách theo quy trình được phê duyệt. Không dựa riêng vào từ khóa hoặc sentiment để loại bỏ trường hợp quan trọng. Nếu thiếu thông tin hoặc kết quả không rõ, đưa vào hàng chờ duyệt thủ công.

---

## Card #2 — Vinmec: Trích xuất và đối chiếu chứng từ hành chính thành bản nháp

| Trường                         | Nội dung                                                                                                                                                                                                                                                                                                          |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán trong một câu**     | Nhân viên hành chính phải nhập lại và đối chiếu thủ công các trường trên chứng từ nhiều định dạng, khiến việc xử lý hồ sơ chậm và dễ sai sót.                                                                                                                                                                     |
| **Công ty / đơn vị**           | Vinmec — bộ phận hành chính, thu ngân hoặc tiếp nhận hồ sơ tại một cơ sở pilot.                                                                                                                                                                                                                                   |
| **Ai đang đau (Actor)?**       | Nhân viên nhập liệu và đối chiếu; người kiểm tra hồ sơ; bệnh nhân phải chờ khi chứng từ thiếu hoặc không khớp.                                                                                                                                                                                                    |
| **Workflow thủ công hiện tại** | **1.** Nhận chứng từ thuộc loại được phép xử lý. **2.** Đọc hoặc tra cứu từng trường và nhập vào biểu mẫu. **3.** Đối chiếu mã hồ sơ, mã dịch vụ, ngày và số tiền với nguồn tham chiếu. **4.** Xử lý trường thiếu hoặc không khớp, chuyển người có thẩm quyền khi cần. **5.** Kiểm tra và xác nhận trước khi lưu. |
| **Bottleneck**                 | Bước **2–3**: phải chuyển giữa tài liệu và biểu mẫu; bản scan hoặc bố cục khác nhau làm tăng công sức đọc và nhập. **Baseline giả định:** 8 phút/bộ chứng từ nhỏ cùng loại. Cần định nghĩa rõ một bộ gồm bao nhiêu trang trước khi đo.                                                                            |
| **AI hỗ trợ ở bước nào?**      | OCR/LLM hỗ trợ bước **2**: trích các trường thành JSON và kèm trang/vị trí nguồn. Bước **3** dùng rule để kiểm tra định dạng, trường bắt buộc và đối chiếu giá trị. AI đánh dấu trường không đọc được hoặc mâu thuẫn; nhân viên xử lý bước 4–5.                                                                   |
| **Quick Architecture**         | **OCR/LLM extraction + rule validation + workflow approval.** Rule đảm nhiệm so sánh, tính toán và kiểm tra điều kiện; LLM hỗ trợ tài liệu phi cấu trúc. Chưa cần Agent.                                                                                                                                          |

### Scope được thu hẹp

Nguồn ý tưởng bao gồm cả bệnh án, đơn thuốc và kết quả xét nghiệm. Trong pilot này, tôi chỉ chọn **một loại chứng từ hành chính**, ví dụ phiếu thu, với các trường đã thống nhất. Không diễn giải bệnh án, suy luận chẩn đoán, thay đổi đơn thuốc hoặc tự quyết định quyền lợi bảo hiểm.

### Metric và cách kiểm chứng

- **Thời gian xử lý:** mục tiêu giảm từ baseline giả định **8 phút xuống không quá 4 phút/bộ**, tính cả kiểm tra và sửa.
- **Chất lượng trích xuất:** mục tiêu ít nhất **95% trường cần trích** khớp chính xác với nhãn tham chiếu; báo cáo riêng các trường quan trọng như mã hồ sơ và số tiền.
- **Phê duyệt:** **100% bản ghi** phải được người có thẩm quyền xác nhận trước khi lưu; trường thiếu hoặc mâu thuẫn phải được đánh dấu thay vì tự điền.
- **Cách đo pilot:** dùng khoảng **30 bộ chứng từ giả lập**, cùng loại và có đáp án tham chiếu; kiểm tra bản rõ, bản mờ, trường bị thiếu, mã gần giống nhau và chênh lệch số tiền. Sau đó mới xem xét đánh giá trên mẫu thực tế được cho phép.

### Ranh giới vận hành

AI chỉ tạo bản nháp, không tự ghi đè hồ sơ hoặc hợp nhất dữ liệu giữa hai bệnh nhân. Không suy đoán trường không đọc được và không tự quyết định số tiền thanh toán. Nếu không đối chiếu được mã hồ sơ hoặc có nhiều kết quả phù hợp, dừng ở trạng thái cần kiểm tra. Dữ liệu cá nhân chỉ được dùng trong môi trường và quy trình đã được đơn vị cho phép.

---

## Card #3 — Vinmec: Hỗ trợ bệnh nhân quốc tế tra cứu thủ tục bằng nhiều ngôn ngữ

| Trường                         | Nội dung                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán trong một câu**     | Bệnh nhân quốc tế khó tìm hiểu thủ tục và xác định đầu mối hỗ trợ, khiến nhân viên phải giải thích lại nhiều lần và thời gian phục vụ kéo dài.                                                                                                                                                                                                                            |
| **Công ty / đơn vị**           | Vinmec — bộ phận Hỗ trợ bệnh nhân quốc tế và lễ tân tại một cơ sở pilot.                                                                                                                                                                                                                                                                                                  |
| **Ai đang đau (Actor)?**       | Nhân viên hỗ trợ bệnh nhân quốc tế (International Patient Coordinator); lễ tân; bệnh nhân và người nhà cần thông tin rõ ràng bằng ngôn ngữ phù hợp.                                                                                                                                                                                                                       |
| **Workflow thủ công hiện tại** | **1.** Nhận câu hỏi và xác định ngôn ngữ, nhu cầu. **2.** Tra cứu tài liệu về đặt lịch, giấy tờ, thanh toán hoặc hướng dẫn di chuyển. **3.** Giải thích/dịch thông tin và hỏi bổ sung khi chưa rõ. **4.** Chuyển điều phối viên nếu liên quan đến trường hợp riêng, y khoa, giá cuối cùng hoặc bảo hiểm. **5.** Xác nhận bệnh nhân đã hiểu và ghi nhận yêu cầu tiếp theo. |
| **Bottleneck**                 | Bước **2–3**: vừa tìm đúng chính sách, vừa diễn đạt phù hợp và thống nhất giữa các ngôn ngữ. **Baseline giả định:** 10 phút/yêu cầu thủ tục thông thường, không tính thời gian chờ bên thứ ba.                                                                                                                                                                            |
| **AI hỗ trợ ở bước nào?**      | Hỗ trợ bước **2–3**: truy xuất tài liệu phi lâm sàng đã được duyệt, tạo bản nháp trả lời bằng ngôn ngữ của bệnh nhân, dẫn nguồn và hỏi lại nếu thiếu ngữ cảnh. Workflow chuyển điều phối viên ở bước 4 khi vượt phạm vi.                                                                                                                                                  |
| **Quick Architecture**         | **RAG + LLM Feature + workflow chuyển tiếp.** Rule xác định chủ đề và điều kiện chuyển người phụ trách. Trong pilot chỉ tra cứu và soạn nháp; chưa cần Agent có quyền đặt lịch, thanh toán hoặc sửa hồ sơ.                                                                                                                                                                |

### Metric và cách kiểm chứng

- **Thời gian xử lý:** mục tiêu giảm từ baseline giả định **10 phút xuống không quá 5 phút/yêu cầu**, tính cả nhân viên duyệt bản nháp.
- **Độ đúng và nguồn:** mục tiêu ít nhất **95% câu trả lời cho câu hỏi trong phạm vi** đúng với tài liệu tham chiếu và trỏ đến đúng nguồn đã được duyệt.
- **Chuyển tiếp:** **100% câu hỏi vượt phạm vi trong bộ test** phải được chuyển đúng đầu mối; thiếu tài liệu thì không tự suy diễn.
- **Cách đo pilot:** bắt đầu với **tiếng Việt và tiếng Anh**, khoảng **30 câu hỏi** gồm hỏi đáp thủ tục, nội dung không có trong tài liệu, yêu cầu giá cuối cùng, quyền lợi bảo hiểm và câu hỏi lâm sàng. Có người đủ năng lực ngôn ngữ kiểm tra bản dịch và nội dung.

### Ranh giới vận hành

Chỉ hỗ trợ thủ tục và thông tin phi lâm sàng từ nguồn được duyệt. Không chẩn đoán, tư vấn thuốc hoặc xử lý khẩn cấp; chuyển đầu mối phù hợp theo quy trình của cơ sở. Không cam kết giá cuối cùng, kết quả bảo hiểm, lịch hẹn hoặc kết quả điều trị. Thông tin chi phí dự kiến chỉ được trích từ nguồn còn hiệu lực và phải thể hiện điều kiện áp dụng. Mọi thao tác làm thay đổi dữ liệu cần quy trình xác nhận riêng.

---

## 🗳️ Lý do chọn 3 thẻ này

### Tiêu chí sàng lọc

1. **Workflow và actor rõ:** chỉ ra được người trực tiếp làm việc và bước có thể cải thiện.
2. **Có thể thu hẹp phạm vi:** chọn một cơ sở, một nhóm tác vụ hoặc một loại chứng từ.
3. **Kiểm thử được trong lab:** có thể tạo dữ liệu giả lập và đáp án tham chiếu, đồng thời ghi rõ giới hạn của cách đánh giá này.
4. **Vai trò AI có lý do:** dùng LLM cho ngôn ngữ/tài liệu phi cấu trúc; dùng rule cho kiểm tra và đối chiếu; giữ người duyệt tại các điểm cần thiết.
5. **Metric đo được:** thời gian có tính cả bước duyệt, chất lượng có tiêu chí chấm, không chỉ đo tốc độ sinh câu trả lời.

### Vì sao từng thẻ được chọn

| Card                              | Lý do chọn                                                                                                                                                                                   | Điều kiện cần xác minh                                                                                          |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **#1 — Feedback bệnh nhân**       | Phạm vi có thể giới hạn ở trải nghiệm dịch vụ; có đầu ra rõ gồm nhãn, bằng chứng và bản nháp ticket. Có thể kiểm tra tình huống khó như đa chủ đề, mỉa mai, phủ định và chuyển tiếp ưu tiên. | Quyền sử dụng feedback, bộ nhãn, cách gán bộ phận, tiêu chí ưu tiên và baseline thời gian thực tế.              |
| **#2 — Chứng từ hành chính**      | Phân biệt rõ phần LLM trích xuất và phần rule kiểm tra; dễ đối chiếu từng trường với đáp án tham chiếu. Thu hẹp khỏi nội dung lâm sàng giúp prototype có phạm vi cụ thể hơn.                 | Mẫu chứng từ, từ điển trường, quy tắc đối chiếu, quyền xử lý dữ liệu và người xác nhận trước khi lưu.           |
| **#3 — Hỗ trợ bệnh nhân quốc tế** | Giữ được khó khăn giao tiếp từ file nguồn và tạo được case hỏi đáp có dẫn nguồn. Có thể bắt đầu với ít ngôn ngữ và một tập thủ tục, chưa cần quyền hành động tự chủ.                         | Tài liệu chính thức còn hiệu lực, người kiểm tra ngôn ngữ, đầu mối chuyển tiếp và quy trình cập nhật thông tin. |

### Vì sao chưa chọn 2 bài toán còn lại cho Phase 2

- **SCAN #3 — Dự báo và phân bổ nguồn lực:** có giá trị vận hành nhưng cần dữ liệu lịch sử đủ dài, chất lượng dữ liệu và cách đánh giá theo thời gian. Một prototype prompt không chứng minh được độ chính xác dự báo. Hướng chính là forecasting + scheduling theo rule; cần chuẩn bị dữ liệu và baseline trước khi đi sâu.
- **SCAN #5 — Giảm thời gian chờ ngoại trú:** tác động tiềm năng lớn nhưng phụ thuộc nhiều điểm chuyển giao, dữ liệu thời gian và trạng thái hệ thống. Nếu chỉ làm trợ lý trả lời “đi đâu tiếp”, chưa đủ chứng minh giảm tổng thời gian chờ. Cần chọn một điểm chờ cụ thể, đo nguyên nhân và xác định quyền điều chỉnh workflow trước.
