# BÀI 3: PHÂN TÍCH TÀI CHÍNH – TÍNH TOÁN CHI PHÍ AI

## 1. Xác định tổng số token sử dụng

Hệ thống xử lý:

```text
10.000 hóa đơn/ngày
```

Mỗi hóa đơn sử dụng:

```text
Input:  1.500 token
Output:   500 token
```

### Tổng Input token mỗi ngày

[
10.000 \times 1.500 = 15.000.000\text{ token}
]

Tương đương:

[
15.000.000\div1.000.000=15\text{ triệu token}
]

### Tổng Output token mỗi ngày

[
10.000 \times 500=5.000.000\text{ token}
]

Tương đương:

[
5.000.000\div1.000.000=5\text{ triệu token}
]

### Tổng token mỗi tháng

Với 30 ngày:

[
15.000.000\times30=450.000.000\text{ Input token}
]

[
5.000.000\times30=150.000.000\text{ Output token}
]

---

# 2. Chi phí Mô hình A – DeepSeek Chat Direct API

Đơn giá:

```text
Input:  $0,14/1 triệu token
Output: $0,28/1 triệu token
```

## 2.1. Chi phí mỗi ngày

### Chi phí Input

[
15\times$0,14=$2,10
]

### Chi phí Output

[
5\times$0,28=$1,40
]

### Tổng chi phí mỗi ngày

[
$2,10+$1,40=\boxed{$3,50/ngày}
]

## 2.2. Chi phí mỗi tháng

### Chi phí Input

[
450\times$0,14=$63,00
]

### Chi phí Output

[
150\times$0,28=$42,00
]

### Tổng chi phí mỗi tháng

[
$63,00+$42,00=\boxed{$105,00/tháng}
]

---

# 3. Chi phí Mô hình B – OpenRouter gọi Gemini 2.5 Flash

Đơn giá:

```text
Input:  $0,075/1 triệu token
Output: $0,30/1 triệu token
```

## 3.1. Chi phí mỗi ngày

### Chi phí Input

[
15\times$0,075=$1,125
]

### Chi phí Output

[
5\times$0,30=$1,50
]

### Tổng chi phí mỗi ngày

[
$1,125+$1,50=\boxed{$2,625/ngày}
]

Làm tròn:

[
\boxed{$2,63/ngày}
]

## 3.2. Chi phí mỗi tháng

### Chi phí Input

[
450\times$0,075=$33,75
]

### Chi phí Output

[
150\times$0,30=$45,00
]

### Tổng chi phí mỗi tháng

[
$33,75+$45,00=\boxed{$78,75/tháng}
]

---

# 4. So sánh chi phí ban đầu

| Phương án               | Input/ngày | Output/ngày |  Tổng/ngày |  Tổng/tháng |
| ----------------------- | ---------: | ----------: | ---------: | ----------: |
| A – DeepSeek Direct API |      $2,10 |       $1,40 |  **$3,50** | **$105,00** |
| B – OpenRouter + Gemini |     $1,125 |       $1,50 | **$2,625** |  **$78,75** |

### Số tiền Mô hình B tiết kiệm mỗi ngày

[
$3,50-$2,625=$0,875
]

### Số tiền tiết kiệm mỗi tháng

[
$105,00-$78,75=$26,25
]

### Tỷ lệ tiết kiệm

[
\frac{26,25}{105}\times100=25%
]

Như vậy, nếu chưa tính retry, Mô hình B rẻ hơn Mô hình A:

[
\boxed{25%}
]

---

# 5. Chi phí Mô hình B sau khi tính retry

Theo đề bài, lỗi hoặc mất kết nối làm phát sinh thêm lượng Input token bằng khoảng 5% tổng Input token hằng ngày.

> Mặc dù tỷ lệ lỗi là 0,5%, bài toán quy định mức token phát sinh do retry là 5%, nên phép tính sử dụng trực tiếp mức tăng 5%.

## 5.1. Input token phát sinh

[
15.000.000\times5%=750.000\text{ token}
]

### Tổng Input token sau retry

[
15.000.000+750.000=15.750.000\text{ token}
]

Hoặc:

[
15.000.000\times1,05=15.750.000\text{ token}
]

## 5.2. Chi phí Input thực tế mỗi ngày

[
\frac{15.750.000}{1.000.000}\times$0,075
]

[
15,75\times$0,075=$1,18125
]

Chi phí Output không thay đổi theo giả thiết của đề:

[
5\times$0,30=$1,50
]

### Tổng chi phí thực tế mỗi ngày

[
$1,18125+$1,50=\boxed{$2,68125/ngày}
]

Làm tròn:

[
\boxed{$2,68/ngày}
]

## 5.3. Chi phí thực tế mỗi tháng

### Input token mỗi tháng sau retry

[
15.750.000\times30=472.500.000\text{ token}
]

### Chi phí Input mỗi tháng

[
472,5\times$0,075=$35,4375
]

### Chi phí Output mỗi tháng

[
150\times$0,30=$45,00
]

### Tổng chi phí mỗi tháng

[
$35,4375+$45,00=\boxed{$80,4375/tháng}
]

Làm tròn:

[
\boxed{$80,44/tháng}
]

## 5.4. Chi phí tăng thêm do retry

Mỗi ngày:

[
$2,68125-$2,625=$0,05625
]

Mỗi tháng:

[
$80,4375-$78,75=$1,6875
]

Như vậy, retry làm tăng chi phí khoảng:

[
\boxed{$1,69/tháng}
]

---

# 6. So sánh sau khi tính retry

| Phương án                  | Chi phí/ngày | Chi phí/tháng |
| -------------------------- | -----------: | ------------: |
| A – DeepSeek Direct API    |    **$3,50** |   **$105,00** |
| B – OpenRouter, chưa retry |   **$2,625** |    **$78,75** |
| B – OpenRouter, có retry   | **$2,68125** |  **$80,4375** |

### Mức tiết kiệm thực tế của Mô hình B so với A

Theo ngày:

[
$3,50-$2,68125=$0,81875
]

Theo tháng:

[
$105-$80,4375=\boxed{$24,5625}
]

Tỷ lệ tiết kiệm:

[
\frac{24,5625}{105}\times100\approx23,39%
]

Sau khi tính retry, Mô hình B vẫn rẻ hơn khoảng:

[
\boxed{23,39%}
]

---

# 7. Phân tích các yếu tố phi tài chính

## 7.1. Vendor lock-in

### DeepSeek Direct API

Ứng dụng kết nối trực tiếp với API của DeepSeek nên phụ thuộc vào:

* API schema của DeepSeek.
* Chính sách giá.
* Quota và rate limit.
* Cách xác thực.
* Định dạng phản hồi.
* Khả năng cung cấp dịch vụ tại từng khu vực.

Nếu muốn chuyển sang Gemini hoặc mô hình khác, đội phát triển có thể phải sửa cấu hình, adapter hoặc mã tích hợp.

### OpenRouter Aggregator

OpenRouter cung cấp API tương thích chuẩn OpenAI và truy cập được nhiều mô hình qua một giao diện thống nhất. Việc chuyển đổi model thường chỉ cần đổi:

```properties
spring.ai.openai.chat.options.model=...
```

Điều này giảm phụ thuộc vào một nhà cung cấp mô hình cụ thể.

Tuy nhiên, nó tạo ra một loại phụ thuộc mới: **lock-in với aggregator**. Hệ thống vừa phụ thuộc vào OpenRouter, vừa chịu ảnh hưởng gián tiếp từ Google Gemini.

Kết luận về lock-in:

* Direct API: phụ thuộc trực tiếp vào một model provider.
* Aggregator: dễ đổi model hơn nhưng phụ thuộc thêm vào nền tảng trung gian.
* Cách giảm lock-in tốt nhất là xây dựng một lớp `AI Provider Adapter` trong kiến trúc ứng dụng.

---

## 7.2. Latency

### Direct API

Luồng request ngắn hơn:

```text
R-Logistics → DeepSeek → R-Logistics
```

Ưu điểm:

* Ít tầng mạng trung gian.
* Thường có độ trễ ổn định hơn.
* Dễ xác định nguyên nhân khi request chậm.
* Dễ đo latency trực tiếp của nhà cung cấp.

### API Aggregator

Luồng request dài hơn:

```text
R-Logistics → OpenRouter → Google Gemini → OpenRouter → R-Logistics
```

Tầng trung gian có thể làm tăng:

* Network latency.
* Thời gian định tuyến.
* Thời gian xếp hàng.
* Khả năng timeout.
* Độ biến động của thời gian phản hồi.

Tuy nhiên, aggregator có thể hỗ trợ routing, fallback hoặc lựa chọn provider phù hợp. Nếu được cấu hình tốt, điều này có thể tăng khả năng chịu lỗi tổng thể.

---

## 7.3. SLA và độ ổn định

### Direct API

Ưu điểm:

* Ít điểm lỗi hơn.
* Làm việc trực tiếp với nhà cung cấp.
* Dễ theo dõi quota, billing và sự cố.
* Có thể nhận SLA trực tiếp nếu sử dụng gói doanh nghiệp.

Nhược điểm:

* Nếu DeepSeek ngừng hoạt động, toàn bộ chức năng AI có thể bị gián đoạn.
* Không tự động chuyển sang mô hình khác nếu chưa xây dựng fallback.

### API Aggregator

Ưu điểm:

* Có khả năng đổi model hoặc provider nhanh.
* Có thể triển khai fallback khi một model gặp sự cố.
* Quản lý nhiều model qua một API thống nhất.

Nhược điểm:

* Tạo thêm một điểm lỗi trung gian.
* Dịch vụ có thể lỗi dù Gemini vẫn hoạt động bình thường.
* SLA thực tế phụ thuộc cả aggregator và nhà cung cấp phía sau.
* Việc xác định lỗi thuộc OpenRouter hay Google có thể phức tạp hơn.

Về mặt xác suất, nếu một request cần cả OpenRouter và Gemini cùng hoạt động, độ sẵn sàng tổng thể có thể được hiểu đơn giản là:

[
Availability_{total}
====================

Availability_{OpenRouter}
\times
Availability_{Gemini}
]

Do đó, nếu không có fallback thực sự, thêm một tầng trung gian không tự động đồng nghĩa với độ ổn định cao hơn.

---

## 7.4. Bảo mật và quyền riêng tư dữ liệu

Hóa đơn có thể chứa:

* Tên khách hàng.
* Địa chỉ.
* Số điện thoại.
* Mã số thuế.
* Giá trị giao dịch.
* Thông tin doanh nghiệp.

Với Direct API, dữ liệu đi qua một nhà cung cấp AI. Với Aggregator, dữ liệu có thể đi qua cả:

```text
R-Logistics → OpenRouter → Google
```

Do đó cần kiểm tra:

* Dữ liệu có được lưu lại hay không.
* Dữ liệu có được dùng để huấn luyện không.
* Vị trí lưu trữ và xử lý dữ liệu.
* Thời gian lưu log.
* Cơ chế mã hóa.
* Thỏa thuận xử lý dữ liệu.
* Khả năng đáp ứng quy định bảo vệ dữ liệu.

Nên che hoặc mã hóa các trường nhạy cảm trước khi gửi tới API nếu chúng không cần thiết cho việc trích xuất.

---

## 7.5. Chất lượng trích xuất JSON

Chi phí thấp không có ý nghĩa nếu model thường xuyên:

* Trả về JSON sai cú pháp.
* Thiếu trường dữ liệu.
* Đọc sai số tiền.
* Nhầm ngày tháng.
* Tạo thông tin không tồn tại.
* Không tuân thủ JSON Schema.

Ví dụ, nếu model rẻ hơn nhưng tỷ lệ lỗi nghiệp vụ cao hơn, chi phí kiểm tra thủ công và xử lý lại hóa đơn có thể lớn hơn số tiền tiết kiệm từ token.

Do đó cần đánh giá trên bộ dữ liệu hóa đơn thực tế bằng các chỉ số:

* JSON validity rate.
* Field accuracy.
* Exact match.
* Tỷ lệ phải retry.
* Tỷ lệ cần kiểm tra thủ công.
* P95 latency.
* Chi phí trên một hóa đơn xử lý thành công.

Chỉ số phù hợp hơn là:

[
\text{Chi phí hiệu dụng}
========================

\frac{\text{Tổng chi phí API + retry + xử lý lỗi}}
{\text{Số hóa đơn xử lý chính xác}}
]

---

# 8. Đề xuất lựa chọn

Với số liệu của đề bài, **nên ưu tiên Mô hình B – OpenRouter gọi Gemini 2.5 Flash**, vì:

* Chi phí sau retry chỉ khoảng **$80,44/tháng**.
* Thấp hơn DeepSeek Direct API khoảng **$24,56/tháng**.
* Tiết kiệm khoảng **23,39%**.
* Dễ chuyển đổi sang mô hình khác.
* API tương thích OpenAI, thuận tiện tích hợp với Spring AI.

Tuy nhiên, chỉ nên chọn lâu dài nếu thử nghiệm thực tế cho thấy:

* Độ chính xác trích xuất hóa đơn đạt yêu cầu.
* JSON trả về ổn định.
* P95 latency nằm trong giới hạn.
* Điều khoản bảo mật dữ liệu phù hợp.
* OpenRouter có SLA đáp ứng yêu cầu vận hành.

Nếu hệ thống yêu cầu độ ổn định rất cao, dữ liệu đặc biệt nhạy cảm hoặc cần SLA trực tiếp với nhà cung cấp, **Mô hình A – Direct API** có lợi thế kiến trúc dù chi phí cao hơn.

Giải pháp lâu dài phù hợp nhất là thiết kế kiến trúc đa nhà cung cấp:

```text
Primary: OpenRouter + Gemini
Fallback: DeepSeek Direct API
```

Ứng dụng nên có:

* Provider abstraction.
* Timeout.
* Retry với exponential backoff.
* Circuit breaker.
* Giới hạn số lần retry.
* Theo dõi token và chi phí.
* Kiểm tra JSON Schema.
* Cơ chế loại bỏ dữ liệu nhạy cảm.
* Fallback sang Direct API khi aggregator gặp sự cố.

Như vậy, hệ thống vừa tận dụng được chi phí thấp của Mô hình B, vừa giảm rủi ro gián đoạn bằng Mô hình A.
