# BÀI 2: CƠ CHẾ TOKENIZATION VÀ CONTEXT WINDOW

## 1. Cơ chế Tokenization

### 1.1. Tokenization là gì?

Tokenization là quá trình chia văn bản đầu vào thành những đơn vị nhỏ gọi là **token** trước khi đưa vào mô hình ngôn ngữ lớn.

Token không nhất thiết tương ứng với một từ hoàn chỉnh. Một token có thể là:

* Một từ.
* Một phần của từ.
* Dấu câu.
* Chữ số.
* Khoảng trắng.
* Ký tự hoặc chuỗi ký tự thường gặp.

Ví dụ minh họa:

```text
Học viên đang lập trình ứng dụng.
```

Tokenizer có thể phân tách thành:

```text
"Học" | " viên" | " đang" | " lập" | " trình" | " ứng" | " dụng" | "."
```

Sau đó, mỗi token được ánh xạ thành một mã số để mô hình xử lý:

```text
[1524, 983, 428, 721, 1182, 934, 2051, 13]
```

Cách phân chia cụ thể phụ thuộc vào bộ tokenizer của từng mô hình. Vì vậy, cùng một văn bản có thể tạo ra số lượng token khác nhau khi sử dụng Llama, Qwen hoặc một mô hình khác.

### 1.2. Vì sao tiếng Việt thường tốn nhiều token hơn tiếng Anh?

Tiếng Việt thường sử dụng nhiều token hơn tiếng Anh trên một số tokenizer vì các nguyên nhân sau:

#### Thứ nhất: Một từ tiếng Việt có thể gồm nhiều âm tiết cách nhau bằng dấu cách

Ví dụ:

```text
lập trình viên
```

Về mặt ngữ nghĩa, đây là một cụm chỉ nghề nghiệp. Tuy nhiên, tokenizer có thể tách thành:

```text
"lập" | "trình" | "viên"
```

Trong tiếng Anh, từ tương ứng:

```text
programmer
```

có thể được lưu thành một token hoặc ít token hơn nếu nó xuất hiện phổ biến trong dữ liệu huấn luyện.

#### Thứ hai: Tiếng Việt sử dụng nhiều dấu thanh và ký tự Unicode

Tiếng Việt có các ký tự như:

```text
ă, â, đ, ê, ô, ơ, ư
```

kết hợp với các dấu:

```text
á, à, ả, ã, ạ...
```

Nếu tokenizer không có đủ các chuỗi tiếng Việt phổ biến trong từ điển, một từ có thể bị chia thành nhiều token con.

#### Thứ ba: Dữ liệu huấn luyện thường thiên về tiếng Anh

Nhiều tokenizer được xây dựng từ kho dữ liệu có tỷ lệ tiếng Anh lớn. Do đó:

* Những từ và cụm từ tiếng Anh phổ biến thường có token riêng.
* Từ tiếng Việt ít xuất hiện hơn có thể bị chia nhỏ.
* Một câu tiếng Việt vì vậy có thể cần nhiều token hơn câu tiếng Anh có cùng ý nghĩa.

Tuy nhiên, đây không phải quy luật tuyệt đối. Những mô hình đa ngôn ngữ như Qwen đã hỗ trợ tiếng Việt tốt hơn, nhưng tỷ lệ token thực tế vẫn phụ thuộc vào tokenizer, nội dung và cách chuẩn hóa Unicode của văn bản.

---

## 2. Phân tích hiện tượng tràn Context Window

### 2.1. Context Window là gì?

Context Window là số token tối đa mà mô hình có thể xem xét trong một lần suy luận.

Context Window không chỉ chứa tài liệu người dùng gửi lên mà còn bao gồm:

```text
System Prompt
+ lịch sử hội thoại
+ câu lệnh của người dùng
+ nội dung tài liệu
+ token đặc biệt của mẫu chat
+ phần token dành cho câu trả lời
```

Điều kiện tổng quát là:

[
T_{system}+T_{history}+T_{prompt}+T_{document}+T_{output}
\leq T_{context}
]

Trong bài toán:

```text
Tài liệu              ≈ 12.000 token
Context Window         = 8.192 token
```

Ngay cả khi chưa tính prompt, system message và câu trả lời:

[
12.000 > 8.192
]

Số token vượt tối thiểu là:

[
12.000-8.192=3.808\text{ token}
]

Trên thực tế, mức vượt còn lớn hơn vì phải dành không gian cho prompt và kết quả tóm tắt.

Ví dụ, nếu:

```text
System Prompt và yêu cầu: 200 token
Kết quả mong muốn:       1.000 token
```

Thì dung lượng đầu vào an toàn chỉ còn:

[
8.192-200-1.000=6.992\text{ token}
]

Trong khi tài liệu cần khoảng 12.000 token, tức vượt khoảng:

[
12.000-6.992=5.008\text{ token}
]

### 2.2. Hiện tượng có thể xảy ra

Khi gửi toàn bộ tài liệu trong một request, tùy phiên bản Ollama, API và client đang sử dụng, hệ thống có thể:

* Từ chối request vì đầu vào quá dài.
* Tự động cắt bớt nội dung đầu vào.
* Loại bỏ các message cũ.
* Không còn đủ không gian để sinh bản tóm tắt.
* Trả về nội dung ngắn, thiếu hoặc dừng giữa chừng.

Nếu Ollama tự động cắt tài liệu, mô hình chỉ nhìn thấy một phần nội dung. Đây là trường hợp nguy hiểm vì request vẫn có thể trả kết quả khiến người dùng tưởng rằng toàn bộ tài liệu đã được xử lý.

### 2.3. Hậu quả đối với chất lượng tóm tắt

Bản tóm tắt có thể:

* Bỏ sót các chương hoặc nội dung quan trọng.
* Không phản ánh đầy đủ toàn bộ tài liệu.
* Mất sự liên kết giữa phần đầu và phần cuối.
* Hiểu sai các khái niệm do thiếu ngữ cảnh.
* Không tổng hợp được kết luận chung.
* Lặp lại nội dung hoặc tạo ra thông tin không chính xác.
* Dừng đột ngột nếu hết không gian sinh output.

Vì vậy, tăng giới hạn độ dài output không giải quyết được vấn đề. Nếu input đã vượt Context Window thì mô hình vẫn không thể đọc đầy đủ tài liệu.

---

## 3. Các giải pháp kỹ thuật

## Giải pháp 1: Chunking kết hợp MapReduce Summarization

Đây là giải pháp phù hợp và an toàn nhất đối với tài liệu dài.

### Bước 1: Chia tài liệu thành các chunk

Tài liệu 12.000 token có thể chia thành 6–8 phần, mỗi phần khoảng:

```text
1.500–2.000 token
```

Nên chia theo cấu trúc tự nhiên như:

* Chương.
* Tiêu đề.
* Mục lớn.
* Đoạn văn.

Không nên cắt cứng giữa một câu hoặc một đoạn mã.

Có thể sử dụng phần nội dung chồng lấn khoảng 100–200 token giữa hai chunk để tránh mất thông tin tại ranh giới:

```text
Chunk 1: token 1–2.000
Chunk 2: token 1.801–3.800
Chunk 3: token 3.601–5.600
```

### Bước 2: Map – Tóm tắt từng chunk

Gửi từng chunk tới mô hình với prompt:

```text
Hãy tóm tắt phần tài liệu sau thành 5–7 ý chính.
Giữ lại thuật ngữ kỹ thuật, quy trình, cảnh báo và ví dụ quan trọng.
Không bổ sung thông tin không có trong tài liệu.
```

Giả sử 6 chunk, mỗi bản tóm tắt khoảng 250 token:

[
6\times250=1.500\text{ token}
]

### Bước 3: Reduce – Tổng hợp các bản tóm tắt

Ghép các bản tóm tắt trung gian và gửi một request cuối:

```text
Dựa trên các bản tóm tắt từng phần dưới đây, hãy tạo một bản
tóm tắt tổng thể có cấu trúc, loại bỏ ý trùng lặp nhưng không
bỏ sót nội dung kỹ thuật quan trọng.
```

Tổng đầu vào lúc này chỉ khoảng 1.500–2.000 token, thấp hơn nhiều so với Context Window 8.192 token.

### Ưu điểm

* Không cần nâng cấp phần cứng.
* Hoạt động với Context Window nhỏ.
* Hạn chế mất nội dung.
* Có thể xử lý tài liệu dài tùy ý.
* Dễ triển khai theo luồng MapReduce.
* Có thể chạy các chunk song song để tăng tốc độ.

### Hạn chế

* Cần thực hiện nhiều request.
* Thời gian xử lý tổng có thể tăng.
* Quan hệ giữa các phần có thể bị suy giảm.
* Chất lượng bản cuối phụ thuộc vào chất lượng các bản tóm tắt trung gian.

---

## Giải pháp 2: Tăng Context Window của Ollama

Vì tài liệu khoảng 12.000 token, có thể tăng Context Window từ 8.192 lên ít nhất 16.384 token.

Không nên đặt đúng 12.000 token vì còn phải dành chỗ cho:

* System Prompt.
* Câu lệnh tóm tắt.
* Chat template.
* Kết quả đầu ra.

Ví dụ tạo `Modelfile`:

```dockerfile
FROM qwen2.5-coder:7b

PARAMETER num_ctx 16384
PARAMETER num_predict 1500
PARAMETER temperature 0.2

SYSTEM """
Bạn là trợ lý chuyên tóm tắt tài liệu kỹ thuật bằng tiếng Việt.
Chỉ sử dụng thông tin có trong tài liệu và không tự bổ sung dữ kiện.
"""
```

Sau đó tạo model mới:

```bash
ollama create qwen2.5-coder-16k -f Modelfile
```

Chạy model:

```bash
ollama run qwen2.5-coder-16k
```

Trong Spring AI:

```properties
spring.ai.ollama.chat.options.model=qwen2.5-coder-16k
```

Ollama chính thức hỗ trợ thiết lập kích thước context bằng `PARAMETER num_ctx`; cũng có thể cấu hình khi chạy server hoặc truyền qua API. [Ollama Modelfile Reference](https://docs.ollama.com/modelfile), [Ollama Context Length](https://docs.ollama.com/context-length).

### Ưu điểm

* Mô hình có thể đọc tài liệu trong một request.
* Giữ được mối liên hệ giữa các chương.
* Luồng xử lý đơn giản hơn Chunking.
* Giảm nguy cơ mất thông tin tại ranh giới các chunk.

### Hạn chế

* Context càng lớn càng cần nhiều RAM hoặc VRAM cho KV cache.
* Thời gian xử lý prompt tăng.
* Tốc độ sinh phản hồi có thể giảm.
* Máy yếu có thể chuyển một phần xử lý sang CPU.
* Context dài không bảo đảm mô hình chú ý tốt như nhau đến mọi phần.
* Không được đặt `num_ctx` cao hơn khả năng mà model/runtime thực sự hỗ trợ.

Phiên bản Qwen2.5-Coder-7B-Instruct gốc có hỗ trợ ngữ cảnh dài, nhưng mức context thực tế còn phụ thuộc định dạng model, bản quantization và runtime. Chẳng hạn, model card GGUF công bố mức context 32.768 token cho cấu hình thông thường. [Qwen2.5-Coder-7B-Instruct-GGUF](https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct-GGUF).

---

## Giải pháp bổ sung: Tóm tắt phân cấp kết hợp RAG

Có thể lưu từng chunk vào Vector Database, sau đó:

1. Chia tài liệu thành các đoạn nhỏ.
2. Tạo embedding và lưu vào Vector Database.
3. Truy xuất các đoạn theo từng chủ đề.
4. Tóm tắt từng nhóm nội dung.
5. Tổng hợp thành bản tóm tắt cuối cùng.

Giải pháp này phù hợp khi chatbot không chỉ tóm tắt một lần mà còn phải trả lời câu hỏi nhiều lần dựa trên tài liệu.

Tuy nhiên, RAG thông thường chỉ lấy các đoạn có liên quan nhất nên có thể bỏ sót nội dung khi mục tiêu là tóm tắt toàn bộ. Vì vậy, đối với yêu cầu tóm tắt toàn văn, nên kết hợp RAG với **tóm tắt phân cấp**, không chỉ dùng truy xuất top-k.

---

## 4. Kết luận

Tài liệu khoảng 12.000 token không thể nằm trọn trong Context Window 8.192 token. Tổng số token thực tế còn phải bao gồm prompt, system message và phần trả lời, nên hệ thống có thể báo lỗi hoặc cắt mất một phần tài liệu. Kết quả tóm tắt vì vậy dễ thiếu ý và không chính xác.

Hai giải pháp chính là:

1. **Chunking kết hợp MapReduce:** chia tài liệu, tóm tắt từng phần rồi tổng hợp; đây là phương án được khuyến nghị khi phần cứng bị giới hạn.
2. **Tăng Context Window:** cấu hình `num_ctx` lên khoảng 16.384 token hoặc cao hơn nếu model và phần cứng hỗ trợ.

Trong hệ thống thực tế, phương án tốt nhất là kết hợp cả hai: tăng Context Window ở mức phù hợp với phần cứng, đồng thời vẫn áp dụng Chunking và tóm tắt phân cấp để hệ thống xử lý ổn định các tài liệu có độ dài khác nhau.
