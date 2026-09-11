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
> khi mức nhiệt độ càng cao thì câu trả lời trở nên dần đa dạng hơn, nhiều facts hơn và có phần thoải mái hơn trong phong cách trả lời có thể ví như càng nhỏ thì như HDV đọc bài thuyết trình còn càng cao thì giống như thằng bạn thân đi du lịch về rồi kể

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> hỗ trợ khách hàng thì phải đưa ra câu trả lời chính xác, chuẩn thông tin nhưng mà văn phong phải có 1 chút gì đó thoải mái nên tôi sẽ đặt mức temperature dưới 0.5 tầm 0.2 hoặc 0.3

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Workload mỗi ngày là: 10.000 × 3 × 350 = 10.500.000 token đầu ra/ngày. Theo mức giá tham khảo, GPT-4o có thể đắt hơn GPT-4o-mini khoảng 10–15 lần cho phần output. GPT-4o xứng đáng khi cần suy luận phức tạp, độ chính xác cao hoặc xử lý yêu cầu quan trọng như phân tích pháp lý, kỹ thuật. GPT-4o-mini phù hợp với hỏi đáp thông thường, phân loại văn bản, tóm tắt và các tác vụ có lưu lượng lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với system prompt là giáo viên tiểu học, câu trả lời ngắn hơn, dùng từ đơn giản, câu văn dễ hiểu và thường có ví dụ gần gũi như các viên bi hoặc quyển sổ. Với system prompt là chuyên gia tài chính nó trả lời dài và chuyên sâu hơn, sử dụng các thuật ngữ như sổ cái phân tán, cơ chế đồng thuận và mật mã học. System prompt định hướng giọng điệu, mức độ chi tiết, từ vựng và cách model lựa chọn ví dụ. Nó hoạt động như bối cảnh và vai trò ưu tiên cho toàn bộ cuộc hội thoại.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với một đoạn tiếng Việt khoảng 100 từ, cách ước lượng số từ / 0.75 cho khoảng 133 token, trong khi tiktoken có thể đếm khoảng 160 token. Chênh lệch là khoảng 20%. Tiếng Việt thường tốn nhiều token hơn tiếng Anh vì cách tách từ, dấu thanh và các chuỗi ký tự có dấu khiến tokenizer chia nhỏ văn bản thành nhiều token hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng khi model tạo câu trả lời dài hoặc tác vụ cần phản hồi tức thì, chẳng hạn chatbot, trợ lý viết nội dung hoặc giao diện hỏi đáp trực tiếp. Người dùng có thể đọc phần đầu trong khi model vẫn đang sinh phần còn lại, nên cảm giác chờ đợi ngắn hơn. Non-streaming phù hợp với câu trả lời ngắn, các tác vụ backend cần nhận toàn bộ kết quả để xử lý tiếp, hoặc khi muốn triển khai đơn giản và dễ kiểm thử hơn.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff tăng dần thời gian chờ giữa các lần retry, giúp API có thời gian phục hồi khi đang quá tải và giảm số request lặp lại cùng lúc. Nếu hàng nghìn client đều retry cố định sau 1 giây, chúng sẽ tạo ra các đợt request đồng thời, làm tình trạng quá tải nghiêm trọng hơn. Có thêm jitter ngẫu nhiên sẽ giúp phân tán các lần retry theo thời gian.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> promt: "Bạn là trợ lý học tập bằng tiếng Việt cực kì kiên nhẫn, cố gắng dùng ví dụ đơn giản, chia vấn đề nhỏ ra và phân tích từ từ, trả lời đầy đủ và có giải thích, hỏi trước khi kết luận". Cụm “bằng tiếng Việt” bảo đảm câu trả lời phù hợp với người học, dùng ví dụ đơn giản để có thể dễ hiểu hơn

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Trợ lý hiện tại của tôi thì không thể lưu trữ đoạn chat quá dài với lại khi paste 1 đoạn văn hay 1 bài báo vào thì nó phân tích không đủ ý. Chỉ phân tích những ý chính nhất thôi và không gian lưu trữ của nó cũng có giới hạn vì sau khi chat khoảng 20 đoạn chat dài thì nó sẽ bắt đầu hỏi lại những thông tin đã được cung cấp trước đó

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
