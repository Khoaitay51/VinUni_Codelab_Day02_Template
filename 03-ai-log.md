# 📝 Phase 6 — REFLECTION (Cá nhân)

## AI hỗ trợ thiết kế codebase

Tôi dùng AI để chuyển starter code sang bài toán tư vấn vé combo tại
VinWonders Phú Quốc. AI giúp tách phần gọi Gemini, kiểm tra phản hồi và
dựng câu trả lời. `evaluate_prompt()` lấy phản hồi thô, `parse_draft()`
kiểm tra định dạng và đối chiếu quyền lợi với bảng mẫu, còn
`render_draft()` dựng nội dung để nhân viên duyệt.

## AI hỗ trợ thiết kế system prompt

AI giúp xác định vai trò trợ lý soạn nháp và ba tình huống xử lý. Khi rõ
gói, mô hình trả quyền lợi theo bảng giả lập. Khi khách chưa xác định
được gói, mô hình yêu cầu làm rõ. Khi mã gói ngoài bảng, mô hình chuyển
cho nhân viên kiểm tra. Dòng đầu phải là `[DRAFT_ONLY]` và phần sau là
JSON gồm `status`, `package` và `benefits`.

## Điều chỉnh sau khi xem starter code

Starter code chỉ tìm xem thẻ nháp có xuất hiện đâu đó trong phản hồi.
Cách kiểm tra đó có thể cho qua câu trả lời nhắc đến thẻ ở giữa nội dung.
Tôi dùng bộ kiểm tra dòng đầu và đối chiếu quyền lợi theo từng mã gói.
Tôi cũng bổ sung ca khách nói “chắc là GOI_B” để kiểm tra mô hình có hỏi
lại trước khi xác nhận suất ăn hay không.

## Reflection

System prompt đặt ra hành vi mong muốn, còn code kiểm tra được định dạng
và quyền lợi theo bảng. Nhân viên vẫn cần đối chiếu mã gói với lời khách
trước khi gửi bản nháp. Phản hồi mô phỏng chỉ dùng để kiểm tra code cục
bộ. Kết quả Gemini sẽ được ghi sau khi chạy bằng API key.

