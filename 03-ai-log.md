# 📔 AI Log — Nhật ký sử dụng AI

**Người viết:** Linh (AI Engineer tại Vin Smart Future)  
**Ngày:** 08/11/2026  
**Bài toán:** Tối ưu tồn kho và phân phối pin tại trạm đổi pin.

## Phase 1 — Khám phá và lựa chọn bài toán

**Tôi hỏi AI:**  
“Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng VinFast. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất.”

**AI hỗ trợ:** Đề xuất bài toán tối ưu tồn kho và điều phối pin giữa các trạm đổi pin.

**Tôi kiểm tra/điều chỉnh:** Chọn bài toán này làm hướng Deep-Dive; lưu ý cần dữ liệu tồn kho, lịch sử đổi pin và quy trình điều phối thực tế để xác minh.

## Phase 2 — Làm rõ bài toán và phạm vi

**Tôi hỏi AI:**  
“Giúp tôi làm rõ pain point, nguyên nhân và phạm vi của bài toán tối ưu tồn kho, phân phối pin tại trạm đổi pin.”

**AI hỗ trợ:** Gợi ý tập trung vào tình trạng thiếu pin ở một số trạm trong khi trạm khác có thể còn pin dư, cùng nhu cầu cân đối tồn kho.

**Tôi kiểm tra/điều chỉnh:** Giữ phạm vi ở bài toán tồn kho và phân phối pin giữa các trạm; không mở rộng sang cứu hộ xe hết pin hoặc điều phối sạc lưu động. Các nguyên nhân và số liệu chưa có dữ liệu thực tế chỉ được xem là giả định.

## Phase 3 — Deep-Dive: Current-State và Future-State

### Current-State Workflow

**Tôi hỏi AI:**  
“Xây dựng Current-State Workflow, nêu từng bước, người thực hiện, input/output, thời gian và đánh dấu bottleneck, handoff.”

**AI hỗ trợ:** Đề xuất quy trình 5 bước, ước tính tổng thời gian 35 phút/lượt; chỉ ra điểm tốn thời gian ở khâu tổng hợp, phân tích tồn kho và tính toán điều chuyển.

**Tôi kiểm tra/điều chỉnh:** Dùng workflow để xác định bottleneck; ghi rõ thời gian là giả định, chưa được xác minh.

### Future-State và AI Fit

**Tôi hỏi AI:**  
“Thiết kế Future-State Workflow, phân chia Rule-Based, LLM Feature và Agentic Loop; bổ sung Human-in-the-loop và Fallback.”

**AI hỗ trợ:** Đề xuất Rule Engine xử lý các ràng buộc tồn kho, mô hình dự báo hỗ trợ ước lượng nhu cầu, LLM phân tích và giải thích đề xuất.

**Tôi kiểm tra/điều chỉnh:** Giữ con người ở bước phê duyệt; nếu dữ liệu thiếu hoặc dự báo không đáng tin cậy thì cần cảnh báo/fallback, không tự động điều chuyển pin.

## Phase 4 — Prototype và kiểm thử ranh giới

**Tôi hỏi AI:**  
“Giúp xây dựng SYSTEM_PROMPT, hàm evaluate_prompt() và ADVERSARIAL_TESTS cho trợ lý hỗ trợ điều phối tồn kho pin.”

**AI hỗ trợ:** Gợi ý quy tắc `[DRAFT_ONLY]`, kiểm tra tồn kho an toàn, xác thực dữ liệu và hai tình huống kiểm thử: yêu cầu chuyển pin vượt mức tồn kho an toàn; yêu cầu bỏ qua phê duyệt và tự cập nhật tồn kho.

**Tôi kiểm tra/điều chỉnh:** Giữ phạm vi chỉnh sửa ở ba phần trên; kiểm tra phản hồi thực tế của Gemini. Kết quả thử nghiệm từ chối chuyển 70 pin khi việc đó làm trạm nguồn xuống dưới mức an toàn, và không tự thực hiện điều chuyển/cập nhật tồn kho. Các kiểm tra bằng từ khóa trong code chỉ là heuristic, vẫn cần đọc lại phản hồi để xác nhận.

## Phase 5 — Evaluate và AI Readiness

**Tôi hỏi AI:**  
“Dựa trên bài toán này, hoàn thành AI Readiness Checklist và đề xuất GO, NOT YET hoặc NO-GO.”

**AI hỗ trợ:** Đề xuất GO để xây dựng prototype trong phạm vi giới hạn, kèm Human-in-the-loop và Fallback.

**Tôi kiểm tra/điều chỉnh:** Xem GO là quyết định cho giai đoạn prototype, không đồng nghĩa hệ thống đã sẵn sàng triển khai thực tế. Cần kiểm chứng dữ liệu, metric, mức độ sẵn sàng của stakeholders và hiệu quả qua thử nghiệm.

## Bài học rút ra

- AI hữu ích khi brainstorm, phân tích workflow, đề xuất kiến trúc và tạo tình huống kiểm thử.
- Cần cung cấp đúng bối cảnh và giới hạn phạm vi để tránh AI đề xuất lệch bài toán.
- Số liệu do AI đưa ra phải được ghi nhận là giả định nếu chưa có dữ liệu xác minh.
- Với quyết định vận hành như điều chuyển pin, cần ràng buộc an toàn và phê duyệt của con người.
- Kết quả do AI tạo ra và các kiểm tra tự động không thay thế việc kiểm chứng của người thực hiện.
