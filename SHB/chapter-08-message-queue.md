# Chương 8 - Message Queue (RabbitMQ & Kafka)

> Câu 71 – 80 | ACK, DLQ, Idempotent Consumer, Ordering và xử lý sự cố

---

## Câu 71: Tại sao cần Message Queue? Tại sao không gọi REST trực tiếp?

### ✅ Câu trả lời

MQ giúp **tách biệt (decouple)** các service và xử lý bất đồng bộ.

```
Transfer → Commit DB → Publish Event → RabbitMQ
                                          │
                            ┌──────────────┼──────────────┐
                            ▼              ▼              ▼
                      Notification      Audit          Email/SMS
```

**Lợi ích:**
- API chỉ xử lý nghiệp vụ chính
- Các tác vụ phụ chạy nền
- Người dùng nhận phản hồi nhanh hơn
- Notification chết → không ảnh hưởng Transfer

---

## Câu 72: RabbitMQ khác Kafka như thế nào?

### ✅ Câu trả lời

| Tiêu chí | RabbitMQ | Kafka |
|-----------|----------|-------|
| Mô hình | Queue | Log |
| Message sau ACK | Thường bị xóa | Giữ theo Retention |
| Routing | Linh hoạt | Throughput cao |
| Phù hợp | Transaction, workflow | Event Streaming, Data Lake |
| Latency | Thấp | Tốt cho dữ liệu lớn |

### 🔄 Internet Banking thì chọn cái nào?

- Email, SMS, OTP, workflow → **RabbitMQ**
- Event Streaming, Audit Log, Analytics → **Kafka**
- Có thể **cùng tồn tại** trong hệ thống lớn

---

## Câu 73: ACK là gì?

### ✅ Câu trả lời

ACK = tín hiệu Consumer gửi về Broker: "Tôi đã xử lý thành công."

```
RabbitMQ → Message → Consumer → ACK → Delete Message
```

Nếu Consumer chết trước khi ACK → RabbitMQ gửi lại → Consumer phải **idempotent**.

---

## Câu 74: At Most Once, At Least Once và Exactly Once

### ✅ Câu trả lời

| Loại | Mô tả |
|------|--------|
| **At Most Once** | Message có thể mất, không xử lý hai lần |
| **At Least Once** | Đảm bảo xử lý ít nhất một lần (có thể trùng) — **phổ biến nhất** |
| **Exactly Once** | Đúng một lần — khó triển khai |

RabbitMQ thường là **At Least Once** → Consumer phải idempotent.

---

## Câu 75: Idempotent Consumer là gì?

### ✅ Câu trả lời

Cùng một Message gửi nhiều lần → kết quả chỉ xử lý **một lần**.

```
Message: TransferSuccess, ID=999
Lần 1: 999 → Process → Save ✅
Lần 2: 999 → Đã xử lý → Bỏ qua ✅
```

**Cách thực hiện:**
- Bảng `processed_message`
- Unique Constraint
- TransactionId / EventId

---

## Câu 76: Dead Letter Queue (DLQ) là gì?

### ✅ Câu trả lời

DLQ chứa các Message **không thể xử lý** sau nhiều lần Retry.

```
Consumer → Retry → Retry → Retry → DLQ
```

Dev/Ops kiểm tra nguyên nhân và xử lý lại.

### 🔄 Tại sao không Retry vô hạn?

- Queue bị đầy
- Consumer bị chiếm tài nguyên
- Các Message khác bị chậm

---

## Câu 77: Message Ordering là gì?

### ✅ Câu trả lời

Một số nghiệp vụ yêu cầu xử lý **đúng thứ tự**:

```
+100 → -50 → +30  (phải theo thứ tự này)
```

RabbitMQ giữ thứ tự trong một queue với một consumer, nhưng nhiều consumer song song → thứ tự không đảm bảo.

**Giải pháp:**
- Phân vùng theo khóa nghiệp vụ
- Chỉ một consumer cho một nhóm dữ liệu

---

## Câu 78: Consumer Scale như thế nào?

### ✅ Câu trả lời

```
Queue → Consumer 1, Consumer 2, Consumer 3
```

RabbitMQ phân phối Message cho các consumer.

### 🔄 Có phải thêm Consumer là nhanh hơn?

**Không.** Nếu Database là bottleneck → thêm Consumer không có tác dụng, thậm chí gây Lock, Deadlock, Connection Pool đầy.

> Phải xác định đúng **điểm nghẽn** trước khi scale.

---

## Câu 79: Làm sao biết Queue đang gặp vấn đề?

### ✅ Câu trả lời

Theo dõi:
- Queue Length
- Consumer Rate
- Publish Rate
- Retry Count
- DLQ Count
- Processing Time

> Queue Length tăng + Consumer Rate thấp = hệ thống **đang bị nghẽn**.

---

## Câu 80: Một Message xử lý mất 30 phút. Consumer chết ở phút thứ 29. Thì sao?

### ✅ Câu trả lời

Nếu chưa ACK → RabbitMQ gửi lại.

Consumer phải:
- **Idempotent**
- Có **Checkpoint** nếu xử lý dài
- **Chia nhỏ** công việc nếu có thể

> Không nên để một Message xử lý quá lâu.

---

## ⭐ Câu hỏi nâng cao (Mức Architect)

> "50.000 giao dịch/phút. RabbitMQ down 5 phút. Sau khi khởi động lại, hàng trăm nghìn Message tràn vào. Làm sao xử lý?"

### ✅ Câu trả lời

1. **Consumer có idempotent không?** — Nếu chưa, scale lên sẽ sinh lỗi
2. **Database chịu được bao nhiêu TPS?** — Scale Consumer vượt DB → DB chết nhanh hơn
3. **Rate Limit** — Consumer đọc Queue theo tốc độ DB chịu được
4. **Monitoring** — Queue backlog, Consumer lag, DB CPU, Connection Pool
5. **Scale từ từ** — Không tăng từ 5 → 100 Consumer. Tăng dần, theo dõi hệ thống.

---

## 🎯 Góc nhìn của Interviewer

> "Nếu giao hệ thống Production cho anh, anh có biết cách vận hành khi có sự cố không?"
> Đó là sự khác biệt giữa Senior Developer và Tech Lead.
