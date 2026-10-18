# Chương 12 - Production Incident

> Câu 111 – 120 | Troubleshooting, RCA, Monitoring — kỹ năng thực chiến của Tech Lead

---

## Câu 111: 10 giờ sáng, khách hàng báo Internet Banking rất chậm. Làm gì?

> ⚠️ Đừng bao giờ trả lời "Em restart server" → Fail ngay.

### ✅ Câu trả lời

| Bước | Hành động |
|------|-----------|
| 1 | **Xác nhận phạm vi** — Chậm tất cả API? Chỉ Transfer? Chỉ Mobile? |
| 2 | **Kiểm tra Dashboard** — CPU, RAM, TPS, Latency, Error Rate |
| 3 | **Xem API nào chậm** — Login 100ms, Transfer 5s, History 90ms → Transfer có vấn đề |
| 4 | **Trace Request** — Gateway 20ms → Transfer 50ms → Account 30ms → DB 4900ms |
| 5 | **Xem Slow Query** — Execution Plan, Lock, Deadlock |
| 6 | **Nếu cần** — Rollback Release |

> Interviewer muốn nghe: **có phương pháp điều tra**, không phải "restart".

---

## Câu 112: CPU lên 100%. Làm gì?

### ✅ Câu trả lời

**Nguyên nhân có thể:**
- Traffic tăng đột biến
- Infinite Loop
- Full GC
- SQL quá chậm
- Deadlock
- Thread Pool đầy

**Công cụ kiểm tra:**
- `top`
- `jstack`
- JFR (Java Flight Recorder)
- GC Log
- Application Metrics

> Không nên restart ngay nếu chưa hiểu nguyên nhân — có thể mất thông tin điều tra.

---

## Câu 113: Database CPU chỉ 20% nhưng API mất 8 giây. Tại sao?

> 🏆 **Câu rất hay**

### ✅ Câu trả lời

Nhiều khả năng:
- **Connection Pool Full** — API đang chờ Connection, không phải chờ SQL
- **Network** — Service timeout
- **Lock** — Thread waiting
- **MQ** — Blocking
- **External API** — Payment Gateway timeout

> ⚠️ CPU thấp **không có nghĩa** Database không có vấn đề.

---

## Câu 114: Làm sao biết Memory Leak?

### ✅ Câu trả lời

**Triệu chứng:**
- RAM tăng liên tục, không giảm
- GC chạy nhiều
- OutOfMemoryError

**Công cụ:**
- Heap Dump
- MAT (Memory Analyzer Tool)
- VisualVM
- JFR

**Ví dụ phổ biến:**
```java
static List<User> cache = new ArrayList<>();
// Mỗi Request add, không remove → RAM đầy
```

---

## Câu 115: Deadlock là gì?

### ✅ Câu trả lời

```
Thread A: Lock Account A → Chờ Account B
Thread B: Lock Account B → Chờ Account A
→ Cả hai đều chờ nhau, không chạy tiếp
```

**Cách tránh:**
- Luôn khóa theo cùng thứ tự (AccountId tăng dần)
- Transaction ngắn
- Hạn chế giữ lock lâu

---

## Câu 116: Release Production xong, CPU tăng. Làm gì?

### ✅ Câu trả lời

1. So sánh trước/sau release
2. Kiểm tra log lỗi + metric
3. Xác định service nào tăng CPU
4. Nếu ảnh hưởng lớn → **Rollback**
5. Sau khi ổn định → Root Cause Analysis

---

## Câu 117: Log quá nhiều, Disk đầy. Làm sao?

### ✅ Câu trả lời

- Phân cấp log: INFO, WARN, ERROR, DEBUG
- **Log Rotation** + **Log Retention**
- **Central Logging**: ELK, Grafana Loki, Splunk
- Không `log.info` mọi nơi

---

## Câu 118: Monitoring những gì?

### ✅ Câu trả lời

| Lớp | Metric |
|-----|--------|
| **Application** | TPS, Latency, Error |
| **JVM** | Heap, GC, Thread |
| **Database** | Connection, Deadlock, Slow Query |
| **MQ** | Consumer Lag, Retry, DLQ |
| **Redis** | Hit Rate, Memory, Eviction |
| **OS** | CPU, RAM, Disk, Network |

---

## Câu 119: API timeout. Điều tra như thế nào?

### ✅ Câu trả lời

```
Client → Gateway → Service A → Service B → Database → External API
```

Sử dụng **Distributed Tracing** để xác định chính xác thành phần gây chậm.

> **Không đoán. Đo.**

---

## Câu 120: Có một bug lâu lâu mới xuất hiện. Làm sao Debug?

> 🏆 **Câu khó nhất**

### ✅ Câu trả lời

1. **Không** debug bằng suy đoán
2. Tăng Log
3. Thêm Metric
4. Thêm TraceID
5. Record Request nếu cần
6. Reproduce ở Staging
7. Nếu vẫn không → Heap Dump, Thread Dump, JFR

---

## ⭐ Câu Tech Lead thật sự

> "Khách hàng báo: Tôi vừa chuyển 100 triệu. Tiền đã bị trừ. Nhưng người nhận chưa nhận được. Anh xử lý thế nào?"

### ✅ Câu trả lời

| Giai đoạn | Hành động |
|-----------|-----------|
| 1. **Xác minh** | TransactionID, Log, TraceID, Audit |
| 2. **Kiểm tra** | Database, Message Queue, Consumer |
| 3. **Xử lý** | Tiếp tục xử lý / bù trừ / hoàn tiền (tùy trạng thái và thiết kế) |
| 4. **Phòng ngừa** | RCA để tránh lặp lại |

---

## 🎯 Góc nhìn của Interviewer

> Không ai quan tâm bạn thuộc lòng bao nhiêu annotation của Spring.
> Họ chỉ muốn biết:
>
> **"Nếu giao Production cho anh, lúc 2 giờ sáng có sự cố, anh có biết điều tra không?"**
>
> Đó là kỹ năng của Tech Lead.
