# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature tăng, câu trả lời thường đa dạng và sáng tạo hơn, nhưng cũng có thể dài dòng hoặc ít ổn định hơn. Ở temperature 0.0, phản hồi thường nhất quán và tập trung; các mức 1.0 và 1.5 dễ đưa ra cách diễn đạt hoặc chi tiết khác nhau qua mỗi lần gọi.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn khoảng 0.2–0.4 để câu trả lời ổn định, chính xác và ít bịa thông tin, nhưng vẫn tự nhiên. Với các tình huống cần sáng tạo như viết nội dung quảng bá, có thể tăng temperature sau khi đã kiểm soát chất lượng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Nếu chỉ xét token đầu ra theo bảng giá trong bài, GPT-4o đắt hơn GPT-4o-mini khoảng 16,7 lần (0.010/0.0006). GPT-4o xứng đáng khi cần suy luận phức tạp hoặc chất lượng cao cho khách hàng quan trọng; mini phù hợp với phân loại yêu cầu, trả lời FAQ và các tác vụ lặp lại quy mô lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với system prompt dành cho trẻ 8 tuổi, phản hồi thường ngắn hơn, dùng từ đơn giản và ví dụ gần gũi như một cuốn sổ ghi chép chung. Với prompt chuyên gia tài chính, phản hồi có thể dài hơn, dùng các thuật ngữ như sổ cái phân tán, đồng thuận và mật mã học. System prompt định hướng persona, mức độ chi tiết, giọng điệu và cách chọn ví dụ của model. Nó không thay thế câu hỏi của người dùng mà bổ sung nguyên tắc để model tạo câu trả lời phù hợp.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn văn tôi chọn, số token đo bằng tiktoken là khoảng 1,3–1,8 lần ước lượng `số từ / 0.75`, nên chênh lệch có thể khoảng 30–80% tùy nội dung. Đây là con số phụ thuộc vào đoạn văn và tokenizer, không phải hằng số cố định. Tiếng Việt có dấu, nhiều âm tiết cách nhau bằng khoảng trắng và cách phân tách của tokenizer không trùng hoàn toàn với “từ”, nên cùng số từ có thể tạo ra nhiều token hơn tiếng Anh.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng khi phản hồi dài hoặc người dùng cần cảm giác hệ thống đang xử lý ngay, chẳng hạn chatbot, viết nội dung và trợ lý lập trình. Người dùng có thể bắt đầu đọc trước khi model sinh xong toàn bộ câu trả lời, nên độ trễ cảm nhận giảm. Non-streaming phù hợp với tác vụ ngắn, xử lý nền hoặc khi ứng dụng cần nhận toàn bộ kết quả để kiểm tra, parse JSON và lưu trữ trước khi hiển thị.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff tăng dần thời gian chờ giữa các lần thử, giúp giảm tải khi server đang quá tải và cho hệ thống thời gian phục hồi. Nó cũng làm các client retry ở các thời điểm khác nhau hơn. Nếu hàng nghìn client cùng chờ đúng một giây rồi retry, chúng tạo ra các đợt request đồng thời, khiến tình trạng quá tải kéo dài và dễ tiếp tục nhận lỗi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: trợ giảng thân thiện cho khóa học AI. System prompt: “Bạn là trợ giảng AI thân thiện. Hãy giải thích khái niệm bằng tiếng Việt, ưu tiên ví dụ thực tế, trình bày ngắn gọn theo từng bước và nói rõ khi không chắc chắn.” Cụm “theo từng bước” giúp người học dễ theo dõi, còn “nói rõ khi không chắc chắn” hạn chế việc trình bày suy đoán như sự thật. Chỉ định tiếng Việt giúp câu trả lời phù hợp với người học.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ giữ ba lượt nên trợ lý có thể quên mục tiêu hoặc thông tin từ đầu cuộc trò chuyện. Tôi sẽ bổ sung bộ nhớ tóm tắt: trước khi cắt history, dùng một bước tóm tắt các yêu cầu và quyết định quan trọng rồi lưu summary cùng phiên. Mỗi request sẽ gửi persona, summary và ba lượt gần nhất; summary cần được cập nhật sau mỗi vài lượt và giới hạn độ dài bằng token.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
