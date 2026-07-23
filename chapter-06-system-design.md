# Chương 6 - System Design

> Câu 52 – 60 | Thiết kế hệ thống chuyển tiền — phần quyết định nhất trong vòng Tech Lead

---

## Câu 52: Thiết kế hệ thống chuyển tiền giữa hai tài khoản trong cùng ngân hàng

### ✅ Câu trả lời

```
                    Client
                      │
                      ▼
              API Gateway
                      │
      ┌───────────────┼───────────────┐
      ▼                               ▼
 Authentication                Rate Limiter
                      │
                      ▼
            Transfer Service
                      │
      ┌───────────────┼───────────────┐
      ▼                               ▼
Account Service              Transaction Service
      │                               │
      └───────────────┬───────────────┘
                      ▼
              Database Cluster
                      │
                      ▼
              Message Queue
                      │
      ┌───────────────┼───────────────┐
      ▼                               ▼
 Notification                   Audit Service
```

**Các thành phần:**

| Thành phần | Chức năng |
|------------|-----------|
| API Gateway | Authentication, Authorization, Rate Limit, Logging |
| Transfer Service | Check Balance → Lock → Debit → Credit → Commit → Publish Event |
| Database | Transaction đảm bảo Debit + Credit + History commit cùng lúc |
| MQ | Đẩy event `TransferSuccess` cho Notification, Audit |

### 🔄 Tại sao không gửi SMS luôn?

Nếu gửi trực tiếp → SMS Timeout → API chậm → Khách hàng phải chờ.

→ Notification nên xử lý **bất đồng bộ**.

---

## Câu 53: Khách hàng bấm nút chuyển tiền liên tục thì sao?

> 🏆 **Câu ngân hàng cực kỳ thích**

### ✅ Câu trả lời — Idempotency Key

```
Client gửi: TransactionId = 123456
Server lưu: 123456 → SUCCESS

Nếu request gửi lại → Server kiểm tra: 123456 đã tồn tại → Trả kết quả cũ
→ KHÔNG thực hiện chuyển tiền lần hai
```

### 🔄 Nếu hai request tới cùng lúc?

- Ràng buộc UNIQUE trên `transaction_id`/`idempotency_key`
- Thực hiện trong cùng Transaction
- Insert thất bại do trùng khóa → trả về kết quả giao dịch đã xử lý

---

## Câu 54: Sau khi trừ tiền xong, Server bị Crash thì sao?

### ✅ Câu trả lời

**Nếu Debit → Crash → chưa Commit:** Database rollback. Không mất tiền.

**Nếu Commit → Crash:** Dữ liệu đã an toàn.

### 🔄 Thế Message Queue chưa gửi thì sao?

**Transactional Outbox Pattern:**

1. Trong cùng một transaction:
   - Cập nhật số dư
   - Ghi lịch sử giao dịch
   - Ghi bản ghi vào bảng `outbox_events`
2. Sau khi commit, tiến trình riêng đọc Outbox → gửi message lên MQ
3. Gửi thành công → đánh dấu đã xử lý

**Đảm bảo:**
- Không gửi message khi transaction bị rollback
- Không mất message nếu server crash sau commit

---

## Câu 55: Database bị Down thì sao?

### ✅ Câu trả lời

- **Read** → Có thể dùng Read Replica
- **Write** → Không thể dùng Redis thay thế cho giao dịch tài chính

Xử lý:
- Trả lỗi phù hợp cho client
- Failover sang database dự phòng nếu hệ thống hỗ trợ
- Theo dõi và khôi phục

> ⚠️ **Không được chấp nhận giao dịch khi chưa ghi thành công vào hệ thống lưu trữ chính.**

---

## Câu 56: RabbitMQ bị chết thì sao?

### ✅ Câu trả lời

- Queue và message ở chế độ **durable/persistent**
- Publisher sử dụng **Publisher Confirm**
- Consumer xử lý theo cơ chế **ACK + retry**
- Thất bại nhiều lần → **Dead Letter Queue (DLQ)**
- Cluster hoặc HA cho RabbitMQ

Nhờ **Outbox Pattern**, các event vẫn nằm trong database và sẽ được gửi lại khi MQ hoạt động trở lại.

---

## Câu 57: Làm sao đảm bảo một Message không xử lý hai lần?

### ✅ Câu trả lời

Consumer phải được thiết kế **idempotent**:

```
Message: TransferSuccess, ID=99999
Consumer nhận 99999 → Process → Save
Consumer nhận lại 99999 → Đã xử lý → Bỏ qua
```

**Cách thực hiện:**
- Bảng `processed_message`
- Unique Constraint
- TransactionId/EventId

### 🔄 Tại sao Kafka lại có thể gửi trùng?

Vì phần lớn MQ đảm bảo **At Least Once Delivery**. Consumer xử lý xong nhưng chưa ACK → Server chết → Kafka gửi lại.

---

## Câu 58: 100.000 giao dịch cùng chuyển vào một tài khoản thì sao?

### ✅ Câu trả lời (Hotspot Problem)

- Row-level lock (không khóa cả bảng)
- Transaction ngắn nhất có thể
- Atomic update
- Hàng đợi tuần tự hóa cập nhật trên cùng tài khoản (nếu nghiệp vụ cho phép)

---

## Câu 59: Làm sao phát hiện hệ thống đang có vấn đề?

### ✅ Câu trả lời

| Lớp | Metric |
|-----|--------|
| Application | TPS, Latency, Error Rate |
| JVM | Heap, GC, Thread |
| Database | Slow Query, Connection Pool, Deadlock |
| MQ | Queue Length, Consumer Lag, Retry |
| Infrastructure | CPU, RAM, Disk, Network |

Cấu hình **alert** để phát hiện sớm trước khi người dùng bị ảnh hưởng.

---

## Câu 60: Nếu thiết kế Internet Banking từ đầu, ưu tiên điều gì nhất?

### ✅ Câu trả lời

| Ưu tiên | Giải thích |
|---------|------------|
| 1. **Correctness** | Một giao dịch sai còn nguy hiểm hơn giao dịch chậm |
| 2. **Availability** | Tránh Single Point of Failure, có failover |
| 3. **Security** | Xác thực, phân quyền, mã hóa, audit, chống replay |
| 4. **Scalability** | Microservice, horizontal scaling, cache, MQ |
| 5. **Observability** | Logging, Metrics, Tracing, Alerting |

---

## 🎯 Góc nhìn của Interviewer

> 80% tư duy System Design **không phụ thuộc** Java hay .NET.
> Nếu trả lời được: Idempotency, Outbox Pattern, Retry, Locking, Transaction, Monitoring, HA
> → Interviewer sẽ nhìn bạn như một **Backend Architect**.
