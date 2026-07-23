# Chương 4 - Transaction (Cực kỳ quan trọng)

> Câu 32 – 41 | ACID, Propagation, Isolation Level, Locking — phần quyết định trong phỏng vấn ngân hàng

---

## Câu 32: Transaction là gì? Vì sao hệ thống ngân hàng bắt buộc phải dùng?

### ✅ Câu trả lời

Transaction là một tập hợp nhiều thao tác được thực hiện như một **đơn vị công việc duy nhất** (Unit of Work).

Chỉ có hai kết quả:
- ✅ Thành công toàn bộ (Commit)
- ❌ Thất bại toàn bộ (Rollback)

**Ví dụ chuyển tiền A → B:**
1. Trừ tiền A
2. Cộng tiền B
3. Ghi lịch sử giao dịch

Nếu sau khi trừ tiền A mà hệ thống lỗi trước khi cộng tiền B → **mất tiền**.

### 🔄 Nếu không dùng Transaction thì chuyện gì xảy ra?

```
UPDATE Balance = Balance - 1000 → Server Crash → Không cộng tiền B
→ Tiền bị mất. Đây là lỗi nghiêm trọng nhất trong ngân hàng.
```

---

## Câu 33: ACID là gì?

### ✅ Câu trả lời

| Chữ | Ý nghĩa | Mô tả |
|-----|----------|--------|
| **A** | Atomicity | Hoặc thành công tất cả, hoặc rollback tất cả |
| **C** | Consistency | Sau Transaction dữ liệu vẫn hợp lệ |
| **I** | Isolation | Các Transaction không ảnh hưởng lẫn nhau |
| **D** | Durability | Sau Commit, dữ liệu được lưu vĩnh viễn |

### 🔄 Trong ngân hàng chữ nào quan trọng nhất?

Cả bốn đều quan trọng. Nhưng xét riêng nghiệp vụ chuyển tiền:
- **Atomicity** — Không được trừ tiền mà không cộng tiền
- **Consistency** — Dữ liệu tài chính phải chính xác

---

## Câu 34: Spring @Transactional hoạt động như thế nào?

### ✅ Câu trả lời

Spring sử dụng **AOP Proxy**:

```
Controller → Spring Proxy → Open Transaction → Execute SQL → Commit → Close Transaction
```

Nếu xảy ra `RuntimeException` → Spring sẽ **rollback**.

### 🔄 Tại sao gọi method trong cùng class thì @Transactional không chạy?

```java
@Service
public class PaymentService {
    public void A() {
        B(); // Gọi trực tiếp, KHÔNG đi qua Proxy!
    }

    @Transactional
    public void B() { }
}
```

> ⚠️ Đây là lỗi rất nhiều developer Java gặp phải.

---

## Câu 35: @Transactional mặc định rollback khi nào?

### ✅ Câu trả lời

Mặc định Spring chỉ rollback khi gặp:
- `RuntimeException`
- `Error`

**Không rollback** đối với Checked Exception (ví dụ `IOException`).

Nếu muốn rollback:
```java
@Transactional(rollbackFor = Exception.class)
```

---

## Câu 36: Propagation là gì?

### ✅ Câu trả lời

Propagation quy định cách Transaction hiện tại tương tác với Transaction đang tồn tại.

| Mode | Hành vi |
|------|---------|
| **REQUIRED** (Mặc định) | Có Transaction → dùng luôn. Chưa có → tạo mới |
| **REQUIRES_NEW** | Luôn tạo Transaction mới, tạm dừng Transaction cũ |
| **SUPPORTS** | Có thì dùng, không có cũng chạy bình thường |
| **MANDATORY** | Bắt buộc phải có Transaction, không có → lỗi |
| **NEVER** | Đang có Transaction → lỗi ngay |
| **NOT_SUPPORTED** | Tạm dừng Transaction, chạy không Transaction |

---

## Câu 37: Khi nào dùng REQUIRES_NEW?

### ✅ Câu trả lời

**Ví dụ:** Chuyển tiền + Ghi Audit Log

Nếu Audit Log lỗi → **không nên** rollback giao dịch chuyển tiền.

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void saveAuditLog(...) { }
```

Audit có Transaction riêng. Transfer vẫn Commit bình thường.

### 🔄 Nếu Audit ghi thất bại thì sao?

- Ghi log lỗi
- Retry nếu phù hợp
- Đưa vào Message Queue hoặc Dead Letter Queue

Quan trọng là **không ảnh hưởng** đến giao dịch chính.

---

## Câu 38: Isolation Level là gì?

### ✅ Câu trả lời

| Isolation | Ngăn được |
|-----------|-----------|
| Read Uncommitted | Không ngăn được nhiều vấn đề |
| Read Committed | Dirty Read |
| Repeatable Read | Dirty Read + Non-repeatable Read |
| Serializable | Tất cả, kể cả Phantom Read |

> Isolation càng cao → dữ liệu càng an toàn nhưng hiệu năng giảm.

---

## Câu 39: Dirty Read là gì?

### ✅ Câu trả lời

```
Transaction A: UPDATE Balance 100 → 200 (Chưa Commit)
Transaction B: Đọc được 200
Transaction A: Rollback!
→ Thực tế Balance vẫn là 100
→ Transaction B đã đọc dữ liệu chưa commit = Dirty Read
```

---

## Câu 40: Lost Update là gì?

> 🏆 **Câu ngân hàng cực kỳ thích**

### ✅ Câu trả lời

```
Balance: 1000

Thread A: Đọc 1000 → -100 → Ghi 900
Thread B: Đọc 1000 → -200 → Ghi 800

Kết quả cuối: 800
Đúng ra phải là: 700
→ Một cập nhật đã bị ghi đè = Lost Update
```

### 🔄 Làm sao tránh Lost Update?

- **Optimistic Lock** (`@Version` trong JPA)
- **Pessimistic Lock** (`SELECT ... FOR UPDATE`)
- **Atomic Update** (`UPDATE account SET balance = balance - ? WHERE id = ?`)
- **Serializable Isolation** (ít dùng do ảnh hưởng hiệu năng)

---

## Câu 41: Optimistic Lock và Pessimistic Lock khác nhau thế nào?

### ✅ Câu trả lời

### Optimistic Lock
- Giả định xung đột **hiếm khi** xảy ra
- Dùng cột `version`

```sql
UPDATE account SET balance = ?, version = version + 1
WHERE id = ? AND version = ?;
```

| Ưu điểm | Nhược điểm |
|----------|------------|
| Hiệu năng cao | Có thể phải retry |
| Không giữ lock lâu | |

### Pessimistic Lock
- Giả định xung đột **thường xuyên**
- Khóa bản ghi ngay khi đọc

```sql
SELECT * FROM account WHERE id = ? FOR UPDATE;
```

| Ưu điểm | Nhược điểm |
|----------|------------|
| An toàn tuyệt đối | Giảm khả năng song song |
| | Nguy cơ deadlock |

### 🔄 Nếu là Internet Banking thì chọn loại nào?

- Dữ liệu cập nhật thấp (hồ sơ khách hàng) → **Optimistic Lock**
- Số dư tài khoản, chuyển tiền → **Pessimistic Lock** hoặc cơ chế khóa ở mức nghiệp vụ

> 💡 Tech Lead không trả lời "luôn dùng A" mà giải thích dựa trên **bối cảnh nghiệp vụ**.

---

## 🎯 Góc nhìn của Interviewer

> Người phỏng vấn đang tự hỏi:
> "Nếu giao cho người này phụ trách hệ thống chuyển tiền của ngân hàng, liệu họ có thiết kế được một luồng giao dịch vừa đúng vừa an toàn không?"
