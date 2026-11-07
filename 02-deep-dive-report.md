# 🏗️ Báo cáo phân tích sâu (Deep-Dive Report)

## Vinmec: Chuẩn hóa và phân loại feedback trải nghiệm bệnh nhân

> **Bài toán được chọn:** Card #1 trong `01-problem-scan.md` — Chuẩn hóa và phân loại feedback trải nghiệm bệnh nhân.
>
> **Trạng thái tài liệu:** Đề xuất scoping và kế hoạch kiểm chứng cho lab. Chưa thực hiện phỏng vấn vận hành, chưa có log thực tế và chưa chạy đánh giá prototype với Gemini. Các thời gian, sản lượng minh họa và mục tiêu dưới đây là giả định hoặc ngưỡng thử nghiệm, không phải số liệu đã đo tại Vinmec.

---

## 📋 Bối cảnh chọn bài toán

Feedback tiếng Việt/Anh có thể chứa nhiều vấn đề trong cùng một đoạn: thời gian chờ, giao tiếp, chi phí, cơ sở vật chất hoặc thủ tục. Nhân viên phải đọc, gắn nhãn, xác định bộ phận phụ trách và tìm các phản hồi tương tự trước khi lập ticket cải tiến.

Tôi chọn bài toán này vì:

- **Actor và đầu ra rõ:** nhân viên Chăm sóc khách hàng (CSKH) cần một bản nháp gồm chủ đề, bằng chứng và bộ phận đề xuất để kiểm tra.
- **Có thể thu hẹp phạm vi:** một cơ sở pilot, feedback dạng văn bản, một bộ nhãn được thống nhất và quy trình duyệt thủ công.
- **Có cơ sở so sánh kỹ thuật:** rule phù hợp với kiểm tra và điều kiện chuyển tiếp; LLM có thể hỗ trợ diễn đạt đa dạng, phủ định và nhiều chủ đề. Hiệu quả so với rule cần được đo.
- **Có thể kiểm thử trong lab:** dùng feedback giả lập với nhãn tham chiếu, sau đó mới đánh giá trên dữ liệu thực tế được phép sử dụng.

So với Card #2 về chứng từ, hướng này chưa cần OCR hoặc tích hợp ghi hồ sơ. So với Card #3 về bệnh nhân quốc tế, prototype ban đầu chưa cần xây kho chính sách để trả lời trực tiếp cho bệnh nhân. Đổi lại, feedback vẫn có thể chứa thông tin nhạy cảm và dấu hiệu cần xử lý ưu tiên; người duyệt và quy trình chuyển tiếp là bắt buộc.

### Phạm vi pilot

| Thành phần       | Phạm vi đề xuất                                                                                                           |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Cơ sở            | Một cơ sở Vinmec, xác định cụ thể sau khi có người phụ trách phối hợp                                                     |
| Người sử dụng    | Nhân viên CSKH; người duyệt chuyên môn vận hành là Quản lý chất lượng (QLCL)                                              |
| Đầu vào          | Văn bản tiếng Việt/Anh từ nguồn được phép sử dụng; lab bắt đầu bằng dữ liệu giả lập                                       |
| Đầu ra           | Bản nháp đa nhãn chủ đề, trích đoạn bằng chứng, bộ phận đề xuất và đề xuất nhóm vấn đề                                    |
| Quyền hành động  | Tra cứu cấu hình được duyệt, phân tích văn bản, tạo bản nháp để người dùng kiểm tra                                       |
| Ngoài phạm vi    | Trả lời bệnh nhân tự động; chẩn đoán; tư vấn thuốc; kết luận sai phạm cá nhân; tự đóng phản ánh; tự thay đổi hồ sơ y khoa |
| Giới hạn dữ liệu | Chưa khẳng định đã có quyền truy cập MyVinmec, kiosk, khảo sát hoặc review công khai; từng nguồn phải được xác nhận riêng |

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping

### Quy trình hiện tại: CSKH đọc và phân loại feedback thủ công

Sơ đồ dưới đây mô tả luồng giả định cần xác nhận với vận hành. Các thời gian là công sức xử lý chủ động, chưa phải số đo thực tế tại Vinmec.

**Chú giải:** 🔄 **Handoff** là điểm chuyển giao thông tin; 🔴 **Bottleneck** là bước gây tốn thời gian nhất hoặc dễ phát sinh lỗi. Mỗi khối có hành động, người thực hiện, thời gian và output.

```text
┌────────────────────────────────────────────────────────────────┐
│ Bước 1: Tiếp nhận feedback                                     │
│                                                                │
│ Hành động: Mở phản hồi, ghi mã tham chiếu và kiểm tra thông    │
│ tin tối thiểu.                                                 │
│ Ai: Nhân viên CSKH                                             │
│ ⏱ 15 giây                                                     │
│ Output: Feedback chờ phân loại                                 │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: Kênh tiếp nhận → bảng theo dõi của CSKH
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 2: Đọc và gắn nhãn                                        │
│ 🔴 BOTTLENECK CHÍNH                                            │
│                                                                │
│ Hành động: Đọc tiếng Việt/Anh, xác định đầy đủ chủ đề và bộ    │
│ phận cần xem xét.                                              │
│ Ai: Nhân viên CSKH                                             │
│ ⏱ 120 giây                                                    │
│ Output: Nhãn chủ đề và bộ phận dự kiến                         │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: CSKH → đối chiếu nhóm vấn đề; trao đổi QLCL khi
          cần
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 3: Đối chiếu vấn đề lặp lại                               │
│ 🔴 BOTTLENECK PHỤ                                              │
│                                                                │
│ Hành động: Tìm phản hồi tương tự; kiểm tra cùng vấn đề hay chỉ │
│ giống từ ngữ.                                                  │
│ Ai: CSKH / Quản lý chất lượng (QLCL)                           │
│ ⏱ 45 giây                                                     │
│ Output: Nhóm vấn đề dự kiến                                    │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: Nhóm vấn đề và bằng chứng → người lập ticket
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 4: Soạn và chuyển ticket                                  │
│                                                                │
│ Hành động: Tóm tắt bằng chứng, xác nhận bộ phận và đầu mối xử  │
│ lý trước khi chuyển.                                           │
│ Ai: CSKH; QLCL duyệt khi cần                                   │
│ ⏱ 45 giây                                                     │
│ Output: Ticket được phân công                                  │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: CSKH → bộ phận tiếp nhận; ghi nhận phân công
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 5: Ghi nhận trạng thái                                    │
│                                                                │
│ Hành động: Cập nhật trạng thái phân công và đầu mối theo dõi   │
│ phản hồi.                                                      │
│ Ai: Nhân viên CSKH                                             │
│ ⏱ 15 giây                                                     │
│ Output: Bản ghi theo dõi                                       │
└────────────────────────────────────────────────────────────────┘

🔴 BOTTLENECK CHÍNH: Bước 2 — đọc và gắn nhãn (120 giây).
🔴 BOTTLENECK PHỤ: Bước 3 — đối chiếu vấn đề lặp lại (45 giây).
⏱ Tổng: 15 + 120 + 45 + 45 + 15 = 240 giây = 4 phút/feedback.
Các thời gian là giả định; chưa tính thời gian chờ giải quyết.
```

- **Bottleneck chính:** bước 2 — đọc và gắn nhãn, **120 giây/feedback**.
- **Bottleneck phụ:** bước 3 — tìm và đối chiếu vấn đề lặp lại, **45 giây/feedback** quy đổi.
- **Tổng công sức xử lý ban đầu:** **15 + 120 + 45 + 45 + 15 = 240 giây = 4 phút/feedback**.

**Cách hiểu thời gian:** Đây là công sức xử lý chủ động đến lúc ghi nhận phân công. Chưa tính thời gian nằm trong hàng chờ, chờ bộ phận phản hồi hoặc giải quyết xong phản ánh. Thời gian bước 3 là phần công sức quy đổi cho một feedback; cần đo riêng nếu nhóm feedback được tổng hợp theo lô.

**Bottleneck chính:** bước 2 chiếm **50%** tổng công sức giả định. Card #1 đặt baseline **120 giây cho bước đọc và gắn nhãn**, không phải toàn bộ workflow.

### Các điểm dễ phát sinh lỗi cần khảo sát

- Một feedback có nhiều chủ đề nhưng chỉ được chọn một nhãn, khiến mất một phần thông tin.
- Các cách diễn đạt tương đương bị gán nhãn khác nhau; phản hồi mỉa mai hoặc phủ định bị hiểu sai.
- Không có bằng chứng kèm theo nhãn, khiến bộ phận tiếp nhận phải đọc và hỏi lại.
- Các trường hợp nhạy cảm bị lẫn vào phản ánh dịch vụ thông thường.
- Cùng chủ đề nhưng khác nguyên nhân bị gộp thành một vấn đề; chưa xác định rõ bộ phận chịu trách nhiệm.

Đây là các giả thuyết để kiểm chứng, chưa phải lỗi đã được quan sát tại Vinmec.

## 3.2. Problem Statement (6-Field) & Metrics

### Problem Statement

| Field                       | Nội dung                                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Nhân viên CSKH trực tiếp đọc và chuẩn hóa feedback tại một cơ sở pilot. QLCL thống nhất nhãn, xử lý trường hợp khó và theo dõi các nhóm vấn đề. Bộ phận tiếp nhận và bệnh nhân chịu ảnh hưởng khi phân loại thiếu hoặc chuyển sai.                                                                                                                                                                                            |
| **2. Current Workflow**     | Tiếp nhận → đọc, gắn nhãn và đề xuất bộ phận → đối chiếu vấn đề lặp lại → soạn và chuyển ticket → ghi nhận trạng thái. Công cụ và quy trình thực tế cần xác nhận. Mô hình giả định có 5 bước, tổng 240 giây công sức xử lý ban đầu/feedback.                                                                                                                                                                                  |
| **3. Bottleneck**           | Bước đọc và gắn nhãn mất giả định **120 giây/feedback**, do văn bản Việt/Anh có nhiều cách diễn đạt, nhiều chủ đề và ngữ cảnh cần kiểm tra. Bước đối chiếu nhóm vấn đề mất thêm giả định 45 giây.                                                                                                                                                                                                                             |
| **4. Business Impact**      | Công sức đọc thủ công làm giảm năng lực xử lý phản hồi của CSKH và chậm tổng hợp vấn đề cho QLCL. Chuyển sai hoặc thiếu chủ đề có thể tạo thêm lượt xử lý lại. Chưa có số thực tế về sản lượng, SLA, tỷ lệ chuyển sai hoặc mức độ hài lòng. Tác động định lượng chỉ được minh họa bằng kịch bản giả định ở phần dưới.                                                                                                         |
| **5. Success Metric**       | **M1:** bước phân loại có AI, kiểm tra hệ thống và người duyệt không quá **45 giây/feedback** tính trung bình, so với baseline giả định 120 giây. **M2:** ít nhất **90% feedback** có đầy đủ chủ đề và đúng bộ phận đề xuất trước khi người duyệt sửa, đối chiếu nhãn tham chiếu. **M3:** **100% trường hợp cần xử lý ưu tiên trong bộ test** được đưa đến hàng chờ người phụ trách, không chỉ dựa vào nhãn mà AI tự dự đoán. |
| **6. Operational Boundary** | AI chỉ tạo nhãn, bằng chứng, tóm tắt và đề xuất chuyển tiếp dưới dạng nháp. Con người xác nhận trước khi tạo/chuyển ticket chính thức. Không tự đăng review, gửi phản hồi ra ngoài, đóng phản ánh hoặc kết luận trách nhiệm cá nhân. Nội dung lâm sàng, dấu hiệu an toàn, thiếu dữ liệu hoặc mâu thuẫn phải chuyển người phụ trách theo quy trình được duyệt.                                                                 |

### Định nghĩa metric và ngưỡng đánh giá

| Metric                            | Cách đo                                                                                                                      | Mục tiêu pilot                                                | Giới hạn cần lưu ý                                                                                   |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **M1 — Thời gian phân loại**      | Từ khi mở feedback để phân loại đến khi nhân viên xác nhận nhãn và bộ phận; tính cả lời gọi model, kiểm tra, sửa và fallback | Trung bình **≤45 giây**; báo cáo thêm trung vị và p95         | Đo cả mẫu khó và mẫu fallback; không chỉ lấy thời gian model sinh output                             |
| **M2 — Phân loại đầy đủ và đúng** | Số feedback có tập chủ đề và bộ phận tương ứng khớp hoàn toàn đáp án tham chiếu / tổng feedback đánh giá                     | **≥90%**, tức ít nhất **45/50 mẫu** nếu bộ đánh giá có 50 mẫu | Chấm bản nháp trước khi người sửa; nếu chỉ chấm sau khi người sửa sẽ che mất lỗi của AI              |
| **M3 — Chuyển tiếp ưu tiên**      | Số mẫu được gán nhãn tham chiếu cần ưu tiên và được workflow chuyển đến hàng chờ ưu tiên / tổng mẫu tham chiếu cần ưu tiên   | **100% trên bộ test**; ví dụ **10/10 mẫu**                    | Không bảo đảm bắt được mọi trường hợp ngoài thực tế; phải theo dõi cả số mẫu thường bị cảnh báo nhầm |
| **M4 — Handoff có kiểm soát**     | Số ticket chính thức có xác nhận của người có quyền / tổng ticket chính thức được tạo                                        | **100%**                                                      | Kiểm tra qua workflow và nhật ký thao tác, không dựa vào lời hứa trong prompt                        |
| **M5 — Tổng công sức ban đầu**    | Tổng thời gian chủ động của các bước đến lúc ghi nhận phân công                                                              | Mục tiêu thăm dò **240 → 120 giây/feedback**                  | Là mục tiêu bổ sung, chưa thay thế M1–M3 của Card #1                                                 |

### Business Impact — kịch bản minh họa

**Giả định để tính:** 100 feedback/ngày tại một cơ sở; tất cả đều thuộc phạm vi của pilot. Đây không phải sản lượng đã được xác minh.

| Hạng mục                                     | Công thức      |                              Kết quả minh họa |
| -------------------------------------------- | -------------- | --------------------------------------------: |
| Công sức phân loại hiện tại                  | 100 × 120 giây |                                 200 phút/ngày |
| Công sức phân loại mục tiêu                  | 100 × 45 giây  |                                  75 phút/ngày |
| Công suất có thể giải phóng ở bước phân loại | 200 − 75       | **125 phút/ngày**, tương đương giảm **62,5%** |
| Công sức toàn workflow hiện tại              | 100 × 240 giây |                                 400 phút/ngày |
| Công sức toàn workflow mục tiêu              | 100 × 120 giây |                                 200 phút/ngày |
| Chênh lệch công sức toàn workflow            | 400 − 200      |   **200 phút/ngày**, tương đương giảm **50%** |

Hai mức tiết kiệm trên đo các phạm vi khác nhau, **không cộng lại**. Công suất giải phóng chưa đồng nghĩa giảm chi phí nhân sự, tăng doanh thu hoặc cải thiện SLA. Cần tính thêm chi phí API, chuẩn bị dữ liệu, duyệt ca khó, vận hành và sửa lỗi; chưa có căn cứ kết luận ROI.

## 3.3. Future-State Flow & AI Fit

### AI Fit Matrix

| Phương án                     | Cách áp dụng                                                                        | Điểm phù hợp                                                | Hạn chế cần kiểm chứng                                                        | Lựa chọn                                 |
| ----------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------- | ---------------------------------------- |
| **No AI / cải tiến biểu mẫu** | Chuẩn hóa form, danh mục chủ đề và hướng dẫn phân công                              | Có thể giảm công sức nếu đầu vào đã có cấu trúc             | Feedback tự do hoặc đa chủ đề vẫn cần người đọc                               | Baseline cần so sánh                     |
| **Rule / State-Machine**      | Từ khóa, danh mục nhãn, điều kiện chuyển tiếp và mapping bộ phận                    | Minh bạch; phù hợp kiểm tra định dạng và ràng buộc thao tác | Cần đánh giá với phủ định, mỉa mai, diễn đạt mới và nội dung nhiều chủ đề     | Dùng cho kiểm tra, điều phối và baseline |
| **LLM Feature + Workflow**    | Model đề xuất đa nhãn, bằng chứng và tóm tắt; workflow kiểm tra rồi gửi người duyệt | Có thể hỗ trợ đọc văn bản tự do với phạm vi output giới hạn | Có thể bỏ sót, thêm nhãn không có bằng chứng hoặc hiểu sai ngữ cảnh           | **Phương án pilot được chọn**            |
| **Agentic Loop**              | Model tự quyết định công cụ, bước tiếp theo hoặc hành động trên nhiều hệ thống      | Chỉ hữu ích nếu nhu cầu điều phối động đã được chứng minh   | Tăng quyền hành động và độ phức tạp; chưa cần cho một luồng phân loại cố định | Chưa chọn                                |

**Lựa chọn:** Rule + LLM Feature + workflow duyệt. Dùng thử model theo một luồng cố định; chưa cần Agent tự chủ hoặc RAG để trả lời bệnh nhân. Danh mục nhãn và mapping bộ phận là cấu hình được phê duyệt. Truy xuất các feedback trước đó chỉ phục vụ gợi ý nhóm vấn đề và phải giới hạn theo quyền dữ liệu.

### Bộ nhãn khởi đầu đề xuất

| Topic label           | Ý nghĩa                               | Bộ phận đề xuất trong pilot        |
| --------------------- | ------------------------------------- | ---------------------------------- |
| `WAIT_TIME`           | Thời gian chờ                         | CSKH / đầu mối điều phối ngoại trú |
| `COMMUNICATION`       | Giao tiếp, giải thích thông tin       | CSKH                               |
| `STAFF_ATTITUDE`      | Trải nghiệm về thái độ phục vụ        | QLCL để xem xét                    |
| `FACILITIES`          | Cơ sở vật chất, tiện nghi             | Đầu mối cơ sở vật chất             |
| `BILLING_INFORMATION` | Thông tin chi phí, hóa đơn            | Đầu mối thu ngân / tài chính       |
| `ADMIN_PROCESS`       | Thủ tục, giấy tờ, phối hợp hành chính | Đầu mối tiếp nhận                  |
| `OTHER_OR_UNCLEAR`    | Ngoài taxonomy hoặc chưa đủ thông tin | CSKH duyệt thủ công                |

Đây là taxonomy minh họa, không phải cơ cấu phân công chính thức của Vinmec. Một feedback có thể có nhiều nhãn. Cờ **cần xem xét ưu tiên** hoặc **liên quan nội dung lâm sàng** được theo dõi riêng, không ép thành sentiment hoặc một chủ đề dịch vụ.

### Future-State Workflow

#### Luồng chính: AI tạo nháp, nhân viên kiểm tra và chuyển ticket

Mỗi khối ghi rõ hành động, actor, thời gian và output như Current-State. Các thời gian dưới đây là ngân sách mục tiêu cho luồng xử lý thông thường; mẫu ưu tiên và fallback vẫn phải được đo và tính vào kết quả đánh giá.

**Chú giải:** 🔄 **Handoff** ghi rõ bên giao và bên nhận; 🔵 **AI Step** là bước model hỗ trợ; 🟢 **HITL** là bước người dùng duyệt. 🔴 **Bottleneck còn lại dự kiến** nằm ở bước 3 — tải đọc và sửa của người duyệt, cần kiểm chứng khi chạy thử.

```text
┌────────────────────────────────────────────────────────────────┐
│ Bước 1: Tiếp nhận và kiểm tra                                  │
│                                                                │
│ Hành động: Kiểm tra quyền dữ liệu, thông tin tối thiểu và dấu  │
│ hiệu cần chuyển tiếp ưu tiên.                                  │
│ Ai: Nhân viên + Rule                                           │
│ ⏱ 15 giây (mục tiêu)                                          │
│ Output: Đầu vào hợp lệ hoặc cờ chuyển người phụ trách          │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: Đầu vào hợp lệ → backend phân loại
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 2: AI tạo bản nháp phân loại                              │
│ 🔵 AI STEP                                                     │
│                                                                │
│ Hành động: Đề xuất nhãn, bằng chứng, bộ phận; kiểm tra JSON và │
│ danh mục cho phép.                                             │
│ Ai: LLM + Rule kiểm tra output                                 │
│ ⏱ 8 giây (mục tiêu)                                           │
│ Output: Bản nháp có cấu trúc; lỗi thì fallback                 │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: Backend → hàng chờ CSKH duyệt bản nháp
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 3: Review và sửa nhãn                                     │
│ 🔴 HITL · ĐIỂM NGHẼN CÒN LẠI DỰ KIẾN                           │
│                                                                │
│ Hành động: Đọc nguồn; kiểm tra, sửa và xác nhận nhãn cùng bộ   │
│ phận; giữ cờ ưu tiên.                                          │
│ Ai: Nhân viên CSKH — HITL                                      │
│ ⏱ 37 giây (mục tiêu)                                          │
│ Output: Phân loại được người duyệt xác nhận                    │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: Phân loại đã xác nhận → kiểm tra nhóm vấn đề
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 4: Xác nhận nhóm vấn đề                                   │
│                                                                │
│ Hành động: Kiểm tra phản hồi tương tự; tạo nhóm mới nếu cần;   │
│ không tự xóa bản trùng.                                        │
│ Ai: Hệ thống đề xuất + Nhân viên xác nhận                      │
│ ⏱ 15 giây (mục tiêu)                                          │
│ Output: Nhóm vấn đề đã được kiểm tra                           │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: Nhóm vấn đề → người duyệt ticket
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 5: Duyệt và chuyển ticket                                 │
│ 🟢 PHÊ DUYỆT BẮT BUỘC                                          │
│                                                                │
│ Hành động: Duyệt tóm tắt, bằng chứng và đầu mối; xác nhận      │
│ trước khi chuyển chính thức.                                   │
│ Ai: Người có quyền + Workflow — HITL                           │
│ ⏱ 30 giây (mục tiêu)                                          │
│ Output: Ticket chính thức sau phê duyệt                        │
└────────────────────────────────┬───────────────────────────────┘
                                 │
        ↓ 🔄 Handoff: Ticket đã duyệt → bộ phận tiếp nhận / nhật ký
                                 │
┌────────────────────────────────────────────────────────────────┐
│ Bước 6: Ghi nhận và theo dõi                                   │
│                                                                │
│ Hành động: Lưu trạng thái, bản sửa và lịch sử phê duyệt; duy   │
│ trì đầu mối theo dõi.                                          │
│ Ai: Workflow + Nhân viên CSKH                                  │
│ ⏱ 15 giây (mục tiêu)                                          │
│ Output: Bản ghi và nhật ký xử lý                               │
└────────────────────────────────────────────────────────────────┘

🔴 BOTTLENECK CÒN LẠI DỰ KIẾN: Bước 3 — tải người duyệt.
⏱ Phân loại: Bước 2 + 3 = 8 + 37 = 45 giây (mục tiêu).
⏱ Tổng: 15 + 8 + 37 + 15 + 30 + 15 = 120 giây = 2 phút.
Các thời gian là mục tiêu, chưa phải kết quả đo thực tế.

↩️ FALLBACK — Từ bước 2 hoặc 3:
┌────────────────────────────────────────────────────────────────┐
│ JSON lỗi, bằng chứng không khớp, API quá hạn hoặc kết quả mơ   │
│ hồ.                                                            │
│                                                                │
│ Hành động: Nhân viên đọc nguồn, phân loại và xác nhận.         │
│ Ai: CSKH / người phụ trách được phân công.                     │
│ ⏱ Đo thực tế, không mặc định đạt 45 giây.                     │
│ Output: Phân loại thủ công được xác nhận.                      │
└────────────────────────────────────────────────────────────────┘
        ↓ 🔄 Handoff: Kết quả đã duyệt → bước 4 → 5 → 6.

⚠️ NHÁNH ƯU TIÊN — Từ bước 1, 2 hoặc 3:
┌────────────────────────────────────────────────────────────────┐
│ Rule, LLM hoặc người duyệt phát hiện dấu hiệu cần ưu tiên.     │
│                                                                │
│ Hành động: Chuyển ngay cho người phụ trách theo quy trình.     │
│ Ai: Đầu mối được đơn vị chỉ định.                              │
│ ⏱ Chuyển khi phát hiện, không đợi LLM hoàn tất.               │
│ Output: Trường hợp được đúng đầu mối tiếp nhận.                │
│ AI không tự hạ mức ưu tiên hoặc tư vấn lâm sàng.               │
└────────────────────────────────────────────────────────────────┘
        ↓ Sau xử lý và xác nhận, ghi nhận nhóm / ticket theo quy trình.

Mọi mẫu fallback và ưu tiên phải được đo và đưa vào đánh giá.
```

- **Bước AI:** bước 2 — tạo nhãn, bằng chứng và bản nháp; hệ thống kiểm tra cấu trúc đầu ra.
- **HITL:** bước 3 xác nhận phân loại; bước 4 xác nhận nhóm; bước 5 phê duyệt trước khi chuyển ticket.
- **Thời gian phân loại mục tiêu:** bước **2 + 3 = 8 + 37 = 45 giây**, so với baseline giả định 120 giây.
- **Tổng ngân sách luồng chính:** **15 + 8 + 37 + 15 + 30 + 15 = 120 giây = 2 phút/feedback**.
- **Fallback:** JSON lỗi, bằng chứng không khớp, API lỗi/quá hạn hoặc kết quả không rõ thì nhân viên xử lý thủ công. Không xóa feedback, không tự đóng phản ánh.
- **Nhánh ưu tiên:** rule, model hoặc người duyệt phát hiện dấu hiệu thì giữ cờ và chuyển người phụ trách; mọi xử lý tiếp theo tuân theo quy trình của đơn vị.

**Nguyên tắc chuyển tiếp:** cờ ưu tiên từ rule, model hoặc người duyệt được kết hợp theo hướng giữ lại cảnh báo; model không được xóa cờ do rule hoặc người dùng có quyền tạo. Nhánh ưu tiên được đưa đến người phụ trách ngay khi phát hiện, không phải đợi một lời gọi LLM hoàn tất. Trường hợp không chắc chắn được chuyển người đọc, không suy ra là “bình thường” chỉ vì thiếu từ khóa.

### Phân công bước và ngân sách thời gian mục tiêu

| Bước tương lai                                                  | Ai / cơ chế                      | Thời gian giả định mục tiêu |
| --------------------------------------------------------------- | -------------------------------- | --------------------------: |
| 1. Nhận feedback, kiểm tra quyền dữ liệu và thông tin tối thiểu | Nhân viên + rule                 |                     15 giây |
| 2. Đề xuất nhãn, bằng chứng; kiểm tra output                    | LLM + rule                       |                      8 giây |
| 3. Đọc nguồn, duyệt/sửa nhãn và bộ phận                         | **HITL — CSKH**                  |                     37 giây |
| 4. Kiểm tra gợi ý nhóm vấn đề; tạo nhóm mới nếu không phù hợp   | Hệ thống đề xuất, người xác nhận |                     15 giây |
| 5. Duyệt tóm tắt và chuyển ticket                               | **Người có quyền** + workflow    |                     30 giây |
| 6. Ghi nhận trạng thái và nhật ký                               | Workflow + CSKH                  |                     15 giây |
| **Tổng**                                                        |                                  |                **120 giây** |

**Bước 2 + bước 3 = 45 giây**, khớp M1. Đây là ngân sách thiết kế, chưa phải latency hoặc kết quả đo. Nếu mẫu fallback không đạt mốc này, vẫn phải tính vào thống kê. Nhánh ưu tiên được đo riêng về thời gian đến người phụ trách; không hứa thời gian giải quyết vụ việc.

### HITL, fallback và giới hạn hành động

| Tình huống                                                       | Cách xử lý                                                                            |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| JSON sai cấu trúc, nhãn ngoài danh mục hoặc bộ phận không hợp lệ | Không chuyển ticket theo output đó; quay về người phân loại                           |
| Bằng chứng không có trong feedback nguồn                         | Đánh dấu không hợp lệ và yêu cầu người đọc lại                                        |
| Model lỗi, quá hạn hoặc không có phản hồi                        | Giữ feedback trong hàng chờ có người phụ trách; xử lý thủ công                        |
| Thiếu nội dung, câu mơ hồ hoặc nhãn mâu thuẫn                    | `needs_human_review = true`; người dùng bổ sung hoặc phân loại                        |
| Có dấu hiệu cần xem xét ưu tiên / nội dung lâm sàng              | Chuyển đầu mối đã được đơn vị phê duyệt; không để model tự tư vấn hoặc hạ mức ưu tiên |
| Nhiều phản ánh tương tự                                          | Chỉ gợi ý liên kết; giữ từng feedback và lịch sử, không tự đóng hoặc xóa bản trùng    |
| Feedback chứa chỉ dẫn “bỏ quy tắc”, yêu cầu tiết lộ dữ liệu      | Xem là nội dung đầu vào không đáng tin; tiếp tục phân tích theo system instruction    |
| Người duyệt sửa hoặc từ chối kết quả                             | Lưu bản nháp, sửa đổi và lý do; dùng để đánh giá lỗi, chưa tự đưa vào huấn luyện      |

Mọi đầu ra trong pilot đều là nháp và có người duyệt. Không dùng con số “confidence” do LLM tự nêu làm căn cứ duy nhất cho việc chuyển ticket hoặc bỏ duyệt. Việc chỉ kiểm tra bằng chứng có trong nguồn giúp phát hiện trích dẫn bịa, nhưng chưa chứng minh nhãn đúng; nhân viên vẫn cần đọc ngữ cảnh.

**Phần thực hành prototype:** Mã nguồn, system prompt, JSON schema và các test nằm trong [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py). Chạy chế độ `--self-test` để kiểm tra logic cục bộ; đánh giá với Gemini cần API key. Kết quả cục bộ không chứng minh các metric vận hành.

# 🏁 Phase 5 — EVALUATE

## 5.1. AI Readiness Checklist

| Câu hỏi                                        | Trạng thái          | Căn cứ hiện có                                                         | Việc cần làm                                                                         |
| ---------------------------------------------- | ------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Có sẵn dữ liệu mẫu/log sạch để test?**       | **Chưa đủ**         | Có mô tả ý tưởng và kế hoạch tạo mẫu; chưa có dataset hoặc log thực tế | Tạo bộ mẫu giả lập; xác nhận nguồn dữ liệu thật, quyền sử dụng và cách khử nhận diện |
| **Rủi ro khi AI sai nằm trong tầm kiểm soát?** | **Mới có thiết kế** | Đề xuất HITL, nhánh ưu tiên, kiểm tra output và fallback               | Triển khai, kiểm thử lỗi API/chỉ dẫn tấn công/bỏ sót; đo tải và lỗi của người duyệt  |
| **Stakeholder sẵn sàng đổi quy trình?**        | **Chưa xác nhận**   | Chưa có phỏng vấn hoặc chấp thuận của CSKH / QLCL                      | Xác nhận actor, bộ nhãn, mapping, người duyệt và điều kiện thử nghiệm                |
| Baseline và hiệu quả kinh tế đã rõ?            | **Chưa đủ**         | 120 giây và sản lượng 100/ngày là giả định                             | Đo mẫu thủ công, xác định khối lượng và chi phí thực tế                              |
| Tích hợp và quyền hành động đã được duyệt?     | **Chưa**            | Prototype dự kiến chỉ soạn nháp                                        | Giữ offline cho lab; chưa kết nối hoặc tự chuyển dữ liệu vào hệ thống thực tế        |

## 5.2. Quyết định cuối cùng

- [ ] **GO — Pilot trên vận hành thực tế**
- [x] **NOT YET — Cần xác lập baseline, dữ liệu và kiểm chứng ranh giới**
- [ ] **NO-GO — Dừng hướng AI**

**Quyết định đề xuất của nhóm: NOT YET cho triển khai/pilot vận hành tại Vinmec.** Tiếp tục được thiết kế và thử prototype offline bằng dữ liệu giả lập trong phạm vi lab. Đây là đề xuất của người làm bài, không phải quyết định đã được Ban Giám đốc hoặc Vinmec phê duyệt.

### Lý giải

1. **Bài toán đủ rõ để tiếp tục kiểm chứng:** có actor, output và điểm hỗ trợ cụ thể; giải pháp có thể giới hạn ở LLM Feature + workflow.
2. **Chưa đủ bằng chứng để GO:** chưa đo baseline, chưa có bộ test, chưa xác nhận quyền dữ liệu hoặc người phụ trách. Các target chưa được chứng minh.
3. **HITL không tự loại bỏ mọi rủi ro:** người duyệt có thể quá tải hoặc bỏ sót. Cần kiểm tra cả model, workflow và hành vi duyệt thay vì chỉ thêm nhãn “có con người”.
4. **Chưa có lý do NO-GO:** cần so sánh với form chuẩn và rule-only. Nếu rule đáp ứng cùng chất lượng với tổng chi phí thấp hơn, chọn rule cho tác vụ đó.
5. **Lợi ích phải tính toàn luồng:** giảm thời gian sinh nhãn chưa đủ nếu thời gian duyệt và sửa làm tổng công sức tăng.

## 5.3. Điều kiện để xem xét chuyển sang GO

| Điều kiện                              | Bằng chứng yêu cầu                                                                                    | Người cần tham gia       |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------ |
| Phạm vi và baseline được xác nhận      | Quy trình thực tế, số đo thủ công, định nghĩa nhãn và dữ liệu được phép sử dụng                       | CSKH + QLCL              |
| M1 và M2 đạt trên tập đánh giá độc lập | Thời gian trung bình ≤45 giây; ≥90% phân loại đầy đủ và đúng; công bố mẫu fallback và sửa lỗi         | Người đánh giá + kỹ sư   |
| M3 và M4 được kiểm tra                 | Không bỏ sót mẫu ưu tiên trong bộ test; mọi ticket chính thức có xác nhận; lỗi hệ thống được fallback | QLCL + người kiểm thử    |
| Rule + LLM có lợi ích so với baseline  | So sánh chất lượng, tổng công sức, chi phí API và tải người duyệt với rule-only                       | Nhóm kỹ thuật + vận hành |
| Có chấp thuận và phương án quay lại    | Người phụ trách, nguồn dữ liệu, phạm vi quyền, cách giám sát và tắt hỗ trợ AI được thống nhất         | Chủ sở hữu quy trình     |

Các ngưỡng trên là tiêu chí lab/pilot ban đầu. Trước khi tiếp xúc dữ liệu hoặc quy trình thực tế, đơn vị phụ trách cần xác nhận ngưỡng và bằng chứng phù hợp; 50 mẫu không đủ để kết luận an toàn hay hiệu quả ở mọi tình huống.

## 5.4. Pilot nhỏ nhất và rollback

**Pilot được đề xuất sau khi đủ điều kiện:** một cơ sở, một nhóm CSKH nhỏ, một bộ nhãn cố định; bắt đầu ở chế độ chạy song song. Nhân viên vẫn xử lý theo quy trình chính thức; output AI chỉ để đối chiếu. Chỉ mở bước duyệt bản nháp khi đã có bằng chứng và chấp thuận.

**Dừng mở rộng hoặc quay lại thủ công nếu:**

- Bỏ sót trường hợp ưu tiên trong bộ đánh giá/giám sát.
- Có thao tác không qua phê duyệt hoặc sử dụng dữ liệu ngoài phạm vi cho phép.
- Phân loại dưới mục tiêu sau một vòng đánh giá đầy đủ.
- Thời gian toàn luồng, gồm sửa và fallback, không cải thiện so với baseline đã đo.
- Lỗi API hoặc backlog khiến người duyệt không xử lý kịp.

**Cách quay lại:** tắt bước đề xuất AI; giữ toàn bộ feedback và mã tham chiếu trong hàng chờ hiện có; phân loại và chuyển ticket thủ công. Giữ lịch sử bản nháp/lỗi theo quy định của đơn vị để điều tra, không xóa feedback khi tắt AI.

## 5.5. Kế hoạch tiếp theo

| Giai đoạn                   | Công việc                                                              | Đầu ra                                            |
| --------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------- |
| **1. Xác nhận bài toán**    | Phỏng vấn người phụ trách; rà lại 5 bước, handoff, baseline và mapping | Workflow được xác nhận; danh sách giả định còn mở |
| **2. Chuẩn bị dữ liệu lab** | Tạo 15 mẫu phát triển và 50 mẫu đánh giá; viết đáp án tham chiếu       | Bộ dữ liệu có nguồn gốc và tiêu chí chấm rõ       |
| **3. Prototype offline**    | Hoàn thiện prompt, JSON schema, rule, nhánh ưu tiên và fallback        | Chương trình chạy được, có log kiểm thử           |
| **4. Đánh giá**             | So sánh thủ công, rule-only và rule + LLM + HITL; chạy boundary tests  | Kết quả M1–M5, lỗi và chi phí thực tế             |
| **5. Quyết định lại**       | Dựa trên bằng chứng chọn GO, tiếp tục NOT YET hoặc NO-GO               | Quyết định có người phụ trách và phạm vi cụ thể   |
