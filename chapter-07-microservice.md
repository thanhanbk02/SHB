# Chương 7 - Microservice

> Câu 61 – 70 | Kiến trúc, giao tiếp, Circuit Breaker, Saga Pattern

---

## Câu 61: Microservice là gì?

### ✅ Câu trả lời

Microservice là kiến trúc chia hệ thống thành nhiều dịch vụ nhỏ, mỗi dịch vụ:
- Chịu trách nhiệm cho một nghiệp vụ riêng
- Phát triển, triển khai và mở rộng độc lập

**Ví dụ Internet Banking:**
```
Authentication Service
Transfer Service
Account Service
Notification Service
Audit Service
Report Service
```

**Ba lợi ích chính:**
1. **Scale độc lập** — Transfer: 2000 TPS, Notification: 50 TPS → chỉ scale Transfer
2. **Deploy độc lập** — Sửa Notification, không cần deploy Transfer
3. **Team độc lập** — Không ảnh hưởng nhau

---

## Câu 62: Tại sao nhiều công ty vẫn dùng Monolith?

> 💡 Câu rất nhiều người trả lời sai

### ✅ Câu trả lời

Microservice **không phải lúc nào cũng tốt hơn**.

Monolith phù hợp khi hệ thống nhỏ, ít module, ít người phát triển:
- Dễ debug, deploy
- Transaction đơn giản
- Không cần Service Discovery, API Gateway
- Không có Distributed Transaction

### 🔄 Khi nào nên chuyển sang Microservice?

- Nhiều module độc lập
- Các nhóm thường xuyên xung đột khi cùng sửa codebase
- Một số chức năng cần scale mạnh hơn
- Build/deploy Monolith quá lâu
- Lỗi nhỏ buộc redeploy toàn bộ

---

## Câu 63: Nhược điểm lớn nhất của Microservice?

### ✅ Câu trả lời

**Độ phức tạp tăng lên:**
- Distributed Transaction
- Network Timeout
- Retry & Circuit Breaker
- Service Discovery
- Logging/Tracing phân tán
- Monitoring
- Version API

---

## Câu 64: Mỗi Microservice có nên dùng chung Database không?

### ✅ Câu trả lời

**Không.** Mỗi service nên sở hữu database riêng:

```
Transfer Service → TransferDB
User Service → UserDB
Notification → NotificationDB
```

Nếu cần dữ liệu từ service khác → thông qua **API** hoặc **Event**, không truy cập trực tiếp DB.

---

## Câu 65: Hai Microservice giao tiếp bằng cách nào?

### ✅ Câu trả lời

| Loại | Công nghệ | Khi nào |
|------|-----------|---------|
| **Đồng bộ** | REST, gRPC | Cần kết quả ngay (Login, Validate OTP) |
| **Bất đồng bộ** | RabbitMQ, Kafka | Không cần phản hồi ngay (Email, SMS, Audit) |

---

## Câu 66: Service B bị Down thì sao?

### ✅ Câu trả lời — Defense in Depth

| Lớp | Mô tả |
|-----|--------|
| **Timeout** | Không để request chờ quá lâu |
| **Retry** | Số lần giới hạn + khoảng nghỉ |
| **Circuit Breaker** | Ngừng gọi nếu liên tục lỗi |
| **Fallback** | Trả cache hoặc thông báo tạm không khả dụng |
| **Monitoring** | Tỷ lệ lỗi, độ trễ, Circuit Breaker status |

---

## Câu 67: Circuit Breaker hoạt động như thế nào?

### ✅ Câu trả lời

```
┌─────────┐         ┌──────┐         ┌───────────┐
│  Closed │ ──lỗi──►│ Open │ ──thời──►│ Half Open │
│(bình    │         │(ngừng│  gian   │(thử lại)  │
│thường)  │◄──OK────│ gọi) │◄──lỗi──│           │
└─────────┘         └──────┘         └───────────┘
```

- **Closed**: Hoạt động bình thường
- **Open**: Lỗi vượt ngưỡng → ngừng gọi, trả lỗi ngay
- **Half Open**: Cho một số request thử → thành công → Closed; lỗi → Open

### 🔄 Tại sao cần?

Nếu không có Circuit Breaker → 1000 request đều gọi service chết → Thread bị giữ → Connection Pool đầy → **Toàn bộ hệ thống bị ảnh hưởng**.

---

## Câu 68: Một request phải gọi 10 service thì sao?

### ✅ Câu trả lời

Đây là dấu hiệu cần **xem lại thiết kế**:
- Tăng độ trễ
- Tăng xác suất lỗi
- Khó debug

**Cân nhắc:**
- API Gateway hoặc Backend For Frontend (BFF)
- Chuyển một số xử lý sang bất đồng bộ
- Thiết kế lại ranh giới giữa các service

---

## Câu 69: Trace một request đi qua 20 Microservice?

### ✅ Câu trả lời — Distributed Tracing

Mỗi request được gắn **Trace ID** + **Span ID**. Khi cần điều tra → tìm theo Trace ID.

**Công cụ:** OpenTelemetry, Jaeger, Zipkin

---

## Câu 70: Giao dịch chuyển tiền giữa hai Microservice thì dùng Transaction như thế nào?

> 🏆 **Câu SHB rất thích**

### ✅ Câu trả lời — Saga Pattern

Không dùng 2PC vì: hiệu năng thấp, khóa tài nguyên lâu, khó mở rộng.

```
Transfer Service → Trừ tiền A → Event → Account Service → Cộng tiền B
                                                                │
                                                          Nếu lỗi ↓
                                                    Compensate → Hoàn tiền A
```

Kết hợp:
- Idempotency
- Outbox Pattern
- Retry có kiểm soát
- Consumer idempotent

→ Đảm bảo **Eventual Consistency** trong khi vẫn giữ khả năng mở rộng.

---

## 🎯 Góc nhìn của Interviewer

> Không nói "Microservice luôn tốt hơn Monolith"
> Không nói "REST luôn tốt hơn MQ"
> Không nói "Saga luôn tốt hơn 2PC"
>
> Một Tech Lead mạnh luôn nói: **Bối cảnh → Trade-off → Lý do chọn giải pháp**
