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
> *temperature càng cao thì phản hồi càng đa dạng nhưng cũng sẽ làm cho mô hình có hướng suy nghĩ nhiều hơn dẫn đến nhiều kết quả khác nhau.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tầm 0.4 vì mức này giúp câu trả lời tương đối ổn định và nhất quán, hạn chế việc chatbot trả lời khác nhau cho cùng một câu hỏi, đồng thời vẫn đủ linh hoạt để giao tiếp tự nhiên.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *GPT-4o có chi phí output khoảng 6–7 lần GPT-4o-mini cho cùng lượng token. Nên dùng GPT-4o cho các tác vụ quan trọng như xử lý khiếu nại phức tạp, phân tích yêu cầu khách hàng hoặc trả lời cần độ chính xác và khả năng suy luận cao. Còn GPT-4o-mini cho FAQ, tra cứu thông tin đơn hàng, phân loại câu hỏi và các yêu cầu đơn giản với số lượng lớn, vì chi phí thấp hơn đáng kể.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Phản hồi đầu tiên ngắn và đơn giản hơn, dùng từ vựng đời thường và ví dụ như cuốn sổ chung để giải thích blockchain cho trẻ 8 tuổi. Phản hồi thứ hai dài và chuyên sâu hơn, sử dụng thuật ngữ như distributed ledger, decentralization, cryptographic hashing, consensus, nodes và tập trung vào góc nhìn tài chính/kỹ thuật. System prompt định hướng mạnh hành vi của model: nó xác định đối tượng, vai trò, mức độ chuyên môn và phong cách trả lời, khiến cùng một câu hỏi nhưng model có thể tạo ra nội dung, từ vựng và cách giải thích rất khác nhau.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *~15%. vì tiếng việt có dấu và ký tự unicode*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming quan trọng nhất khi model tạo ra câu trả lời dài hoặc mất nhiều thời gian xử lý, vì người dùng có thể thấy nội dung xuất hiện từng phần ngay lập tức thay vì phải chờ toàn bộ response hoàn thành, từ đó giảm cảm giác chờ đợi. Ngược lại, non-streaming phù hợp hơn với các tác vụ ngắn, cần xử lý kết quả như một khối hoàn chỉnh, chẳng hạn gọi API để phân loại dữ liệu, lấy JSON hoặc thực hiện các tác vụ backend không cần hiển thị từng token cho người dùng.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff giúp giảm áp lực lên API khi hệ thống đang quá tải bằng cách tăng dần thời gian chờ giữa các lần retry, ví dụ 1s, 2s, 4s, 8s. Nếu hàng nghìn client cùng retry với delay cố định 1 giây, chúng có thể gửi request lại gần như đồng thời, tạo ra một thundering herd khiến API tiếp tục quá tải và có thể tạo vòng lặp lỗi. Exponential backoff, đặc biệt khi kết hợp thêm jitter, giúp phân tán các lần retry theo thời gian và tăng khả năng hệ thống phục hồi.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Thân thiện và chuyên nghiệp. Prompt: "Bạn là một trợ lý học tập AI thân thiện và chuyên nghiệp. Hãy giải thích các khái niệm rõ ràng, dễ hiểu, ưu tiên ví dụ thực tế. Với câu hỏi đơn giản, hãy trả lời ngắn gọn; với câu hỏi phức tạp, hãy trình bày theo từng bước để người dùng dễ theo dõi và đưa ra tài liệu tham khảo.". Cách chọn trả lời ngắn gọn giúp tránh tạo ra nội dung dài không cần thiết và đưa ra tài liệu dẫn chứng sẽ giúp hiểu hơn về kết quả cũng như kiểm chứng tính chính xác của câu trả lời.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Hạn chế lớn nhất là lịch sử trò chuyện chỉ lưu được 3 lượt hội thoại, nên trợ lý có thể quên các thông tin được đề cập từ những lượt trước. Tôi sẽ cải thiện bằng cách thêm bộ nhớ dài hạn, lưu các thông tin quan trọng như sở thích, mục tiêu học tập và lưu những đoạn chat vào database; trước mỗi câu trả lời, hệ thống sẽ truy xuất những thông tin liên quan và đưa chúng vào context của model.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
