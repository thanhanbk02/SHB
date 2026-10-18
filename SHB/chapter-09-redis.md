# Chương 9 - Redis

> Câu 81 – 92 | Cache Pattern, Distributed Lock, Cluster và khi nào KHÔNG dùng Redis

---

## Câu 81: Redis là gì?

### ✅ Câu trả lời

Redis là **in-memory data store**. Dữ liệu lưu trên RAM → tốc độ micro giây đến vài mili giây.

Redis không chỉ là cache:
- Cache
- Distributed Lock
- Session
- Rate Limiter
- Counter
- Pub/Sub
- Stream

### 🔄 Tại sao Redis nhanh?

- Dữ liệu trong RAM
- Cấu trúc dữ liệu tối ưu (Hash, Set, Sorted Set...)
- Event loop đơn luồng → tránh chi phí đồng bộ
- Giao tiếp mạng đơn giản

---

## Câu 82: Dữ liệu nào nên Cache?

### ✅ Câu trả lời

Dữ liệu **đọc nhiều, thay đổi ít**:
- Danh mục tỉnh/thành
- Danh sách ngân hàng
- Cấu hình hệ thống
- Tỷ giá (nếu chấp nhận độ trễ ngắn)

### 🔄 Có nên Cache số dư tài khoản không?

**Thông thường không nên** dùng cache làm nguồn dữ liệu chính cho số dư:
- Thay đổi liên tục
- Cache không kịp đồng bộ → khách nhìn thấy số dư sai
- Database mới là **source of truth**

---

## Câu 83: Cache Aside Pattern

### ✅ Câu trả lời

```
Application → Redis → Miss → Database → Ghi vào Redis → Client
                  ↓
                 Hit → Client
```

### 🔄 Nếu Update Database thì sao?

Sau khi cập nhật Database → **xóa Cache** (hoặc cập nhật lại Cache).

---

## Câu 84: Cache Penetration

### ✅ Câu trả lời

User query key **không tồn tại** (ví dụ `UserId = 999999999999`):
- Redis không có
- Database cũng không có
- Mỗi request đều xuống Database → **quá tải**

**Cách xử lý:**
- Cache giá trị rỗng (Null Cache) với TTL ngắn
- Bloom Filter loại bỏ key chắc chắn không tồn tại

---

## Câu 85: Cache Breakdown

### ✅ Câu trả lời

Một key rất nóng (hot key) → TTL hết → 100.000 request cùng xuống Database.

**Cách xử lý:**
- Mutex/Distributed Lock (chỉ 1 request tải lại)
- Refresh Ahead (làm mới trước khi hết hạn)
- TTL hợp lý

---

## Câu 86: Cache Avalanche

### ✅ Câu trả lời

Nhiều key hết TTL **cùng lúc** → Database quá tải.

**Cách xử lý:**
```
TTL: 3600 → Random: 3600~3900
```
Không để tất cả hết hạn cùng lúc.

---

## Câu 87: Distributed Lock là gì?

### ✅ Câu trả lời

Nhiều instance cùng xử lý → cần lock chung:

```
SET lock_key value NX PX 30000
```

Chỉ một server lấy được lock.

### 🔄 Tại sao không dùng synchronized?

`synchronized` chỉ có tác dụng trong **một JVM**. Nếu có Server A, B, C → mỗi server lock riêng, không đồng bộ được.

Redis Distributed Lock → dùng chung cho tất cả instance.

---

## Câu 88: RedLock là gì?

### ✅ Câu trả lời

Thuật toán khóa phân tán: lấy lock trên **nhiều Redis node** độc lập, chỉ thành công khi đạt **quorum** (đa số).

> ⚠️ Vẫn còn tranh luận trong cộng đồng. Nếu cần nhất quán cao → cân nhắc ZooKeeper hoặc etcd.

---

## Câu 89: Redis mất dữ liệu thì sao?

### ✅ Câu trả lời

Redis lưu đĩa bằng:
- **RDB** (snapshot định kỳ)
- **AOF** (Append Only File)

Nếu Redis chỉ là cache → mất dữ liệu không nghiêm trọng → đọc lại từ Database.

Nếu dùng cho Lock/Session → cần HA, replication, chiến lược khôi phục.

---

## Câu 90: Redis Cluster hoạt động như thế nào?

### ✅ Câu trả lời

Chia dữ liệu thành **16384 hash slot**:

```
Node A: 0 → 5000
Node B: 5001 → 10000
Node C: 10001 → 16383
```

Thêm node → Redis tự cân bằng → **horizontal scaling**.

---

## Câu 91: Khi nào KHÔNG nên dùng Redis?

### ✅ Câu trả lời

Không dùng Redis làm nguồn dữ liệu chính cho:
- Số dư tài khoản
- Trạng thái cuối cùng của giao dịch
- Dữ liệu kế toán

Redis = **hỗ trợ** (Cache, Counter, Session, Rate Limiter). Database quan hệ = **source of truth**.

---

## Câu 92: Redis bị Down. Hệ thống xảy ra chuyện gì?

### ✅ Câu trả lời

| Redis dùng cho | Ảnh hưởng |
|----------------|-----------|
| Cache | DB chịu tải lớn hơn, Response Time tăng |
| Session | Người dùng bị Logout |
| Distributed Lock | Một số chức năng tạm không hoạt động |

> Thiết kế: Redis là **thành phần tăng hiệu năng**, không phải nơi lưu trữ dữ liệu nghiệp vụ cốt lõi.

---

## ⭐ Câu SHB rất thích: "Tại sao không Cache toàn bộ Database?"

### ✅ Câu trả lời

1. **RAM không đủ** — DB: 20TB, Redis: 64GB
2. **Đồng bộ khó** — DB update, Redis chưa update → dữ liệu sai
3. **Chi phí lớn** — RAM đắt hơn Disk
4. **Không phải dữ liệu nào cũng được đọc** — 100 triệu dòng, có khi 1 năm không ai đọc

---

## 🎯 Góc nhìn của Interviewer

> Interviewer không đánh giá "Anh có biết Redis không?"
> Họ đánh giá: **"Anh có biết khi nào KHÔNG dùng Redis không?"**
> Đó là tư duy của Tech Lead.
