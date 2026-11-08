# Lab 02 — AI Log & Reflection

2A2026-Trần Anh Duy-26ai.duyta@vinuni.edu.vn


## Tôi đã dùng AI vào việc gì?

Tôi dùng AI để mở rộng danh sách bài toán Vinhomes, đặt câu hỏi ngược về giá trị của từng ý tưởng và viết nháp ba card. Tôi cũng thay card cuối bằng điều phối sự cố thang máy để có một ví dụ thực sự cần cân nhắc Agent, thay vì gọi mọi bài toán phức tạp là Agent.

Khi chọn bài deep dive, AI giúp tôi so ba card theo cùng một tiêu chí: có mô tả được quy trình hiện tại không, lấy gì để đo, và phần nào có thể thử trong thời gian lab. Tôi chọn phân loại phản ánh cư dân vì có thể tạo tập ticket gán nhãn và so LLM với quy tắc đơn giản. Lựa chọn này vẫn cần được kiểm tra với người làm CSKH thực tế.

## Chỗ AI trả lời chưa ổn và tôi đã sửa

- AI từng đưa ra các con số như 3–5 phút đọc/phân loại, 95% phân loại đúng và 99% phát hiện khẩn cấp quá dễ dàng, dù chưa có log vận hành. Tôi giữ chúng như **giả định và ngưỡng mục tiêu**, không gọi là kết quả của Vinhomes. Trong báo cáo, tôi tách thời gian phân loại khỏi thời gian giải quyết toàn bộ phản ánh.
- Có lúc phương án năng lượng được gắn vào các ô Rule/LLM/Agent dù phần chính thực ra là học máy chuỗi thời gian. Điều đó cho thấy phải chọn kiến trúc theo công việc, không chọn theo tên công nghệ có sẵn trong mẫu.
- Ý tưởng Agent cho sự cố thang máy nghe hấp dẫn, nhưng nếu chưa có lịch trực, tình trạng thang và phản hồi từ nhà thầu thì chỉ vẽ được một quy trình giả định. Tôi không chọn nó để prototype trong bài này.

Từ các điểm trên, tôi yêu cầu AI viết rõ **bước nào được hỗ trợ, ai duyệt, lỗi thì quay về cách làm nào**, và luôn so với cách làm bằng quy tắc trước. Tôi cũng yêu cầu diễn đạt bằng lời bình thường thay cho những cụm quá khuôn mẫu.

Điều tôi rút ra là AI hữu ích nhất ở bước gợi ý và phản biện. Quyết định về metric, phạm vi và mức rủi ro vẫn phải dựa vào người vận hành và dữ liệu thật.
