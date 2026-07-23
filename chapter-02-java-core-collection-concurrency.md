# Chương 2 - Java Core (Collection & Concurrency)

> Câu 12 – 21 | Collection Framework, Thread, Deadlock và câu hỏi System Design mức cao

---

## Câu 12: So sánh ArrayList và LinkedList

### 🎯 Nhà tuyển dụng muốn biết
- Có hiểu cấu trúc dữ liệu không?
- Có biết chọn đúng cấu trúc cho bài toán thực tế không?

### ✅ Câu trả lời

| Tiêu chí | ArrayList | LinkedList |
|-----------|-----------|------------|
| Truy cập theo index | Rất nhanh O(1) | Chậm O(n) |
| Thêm/xóa cuối | O(1) (trung bình) | O(1) |
| Thêm/xóa đầu hoặc giữa | O(n) | O(1) nếu đã có vị trí |
| Bộ nhớ | Ít | Nhiều hơn (con trỏ prev/next) |
| Cache CPU | Tốt | Kém |

Trong thực tế Backend, **ArrayList** được sử dụng phổ biến hơn rất nhiều vì đa số thao tác là đọc dữ liệu và duyệt danh sách.

### 🔄 Interviewer hỏi tiếp: Nếu phải lưu 10 triệu bản ghi thì chọn cái nào?

Chọn **ArrayList** vì:
- Ít tốn bộ nhớ hơn
- Locality của CPU cache tốt hơn
- Tốc độ duyệt nhanh hơn

LinkedList phải tạo thêm object Node cho từng phần tử nên vừa tốn RAM vừa chậm khi duyệt.

---

## Câu 13: ArrayList tăng kích thước như thế nào khi đầy?

### ✅ Câu trả lời

Khi mảng bên trong đầy, ArrayList sẽ tạo một mảng mới có kích thước khoảng **1.5 lần** mảng cũ, sau đó copy toàn bộ dữ liệu sang mảng mới.

```
10 → 15 → 22 → 33 → ...
```

Việc copy này có chi phí O(n), vì vậy nếu biết trước số lượng phần tử, nên khởi tạo trước capacity:

```java
List<User> users = new ArrayList<>(100000);
```

### 🔄 Interviewer hỏi tiếp: Nếu resize nhiều lần thì ảnh hưởng gì?

- Tăng thời gian copy dữ liệu
- Tăng số lần cấp phát bộ nhớ
- Tăng áp lực cho Garbage Collector

---

## Câu 14: HashSet hoạt động như thế nào?

### ✅ Câu trả lời

HashSet thực chất được xây dựng dựa trên **HashMap**.

```java
set.add(user);
// Lưu: Key = user, Value = PRESENT (một object giả)
```

Do HashMap không cho phép trùng key nên HashSet cũng không cho phép phần tử trùng.

### 🔄 Interviewer hỏi tiếp: HashSet có đảm bảo thứ tự không?

- **HashSet** — Không đảm bảo thứ tự
- **LinkedHashSet** — Giữ thứ tự thêm vào
- **TreeSet** — Tự động sắp xếp

---

## Câu 15: TreeMap khác HashMap như thế nào?

### ✅ Câu trả lời

| Tiêu chí | HashMap | TreeMap |
|-----------|---------|---------|
| Sắp xếp | Không | Luôn sắp xếp theo key |
| Cấu trúc | Hash Table | Red-Black Tree |
| Tìm kiếm | O(1) trung bình | O(log n) |

TreeMap phù hợp khi cần dữ liệu theo thứ tự hoặc range query.

### 🔄 Interviewer hỏi tiếp: Nếu lưu 5 triệu bản ghi thì dùng cái nào?

- Chỉ tra cứu theo key → **HashMap** (O(1))
- Cần thứ tự hoặc `firstKey()`, `lastKey()`, `subMap()` → **TreeMap**

---

## Câu 16: Iterator và fail-fast là gì?

### ✅ Câu trả lời

Nếu trong khi duyệt mà Collection bị thay đổi trực tiếp:

```java
for (User u : list) {
    list.remove(u); // ConcurrentModificationException!
}
```

Java sẽ ném `ConcurrentModificationException`. Đây gọi là cơ chế **Fail-Fast**.

Nếu muốn xóa trong lúc duyệt:

```java
iterator.remove(); // An toàn
```

### 🔄 Interviewer hỏi tiếp: ConcurrentHashMap có bị Fail-Fast không?

Không. ConcurrentHashMap sử dụng **Weakly Consistent Iterator** — Iterator vẫn hoạt động dù Collection đang bị thay đổi bởi thread khác.

---

## Câu 17: Thread là gì?

### ✅ Câu trả lời

Thread là đơn vị thực thi nhỏ nhất trong một process. Một ứng dụng Java có thể có nhiều thread chạy đồng thời.

Ví dụ:
- Thread xử lý HTTP Request
- Thread ghi log
- Thread gửi email
- Thread xử lý Message Queue

Trong hệ thống ngân hàng, hàng nghìn request có thể được xử lý đồng thời nhờ nhiều thread.

---

## Câu 18: Thread và Process khác nhau như thế nào?

### ✅ Câu trả lời

- **Process** — Một chương trình đang chạy, có vùng nhớ riêng
- **Thread** — Đơn vị thực thi bên trong Process

Các thread:
- Dùng chung **Heap**
- Có **Stack** riêng

```
Process
├── Thread A
├── Thread B
└── Thread C
```

---

## Câu 19: Race Condition là gì?

### ✅ Câu trả lời

Race Condition xảy ra khi nhiều thread cùng truy cập và thay đổi một tài nguyên mà không có cơ chế đồng bộ.

```java
count++; // Không an toàn với multi-thread
```

Hai thread cùng đọc `count = 5`, cả hai cùng tăng lên 6. Kết quả cuối cùng là 6 thay vì 7.

### 🔄 Interviewer hỏi tiếp: Làm sao tránh Race Condition?

- `synchronized`
- `ReentrantLock`
- `AtomicInteger`
- `ConcurrentHashMap`
- Immutable Object

Tùy bài toán mà chọn giải pháp phù hợp để cân bằng giữa an toàn và hiệu năng.

---

## Câu 20: Deadlock là gì?

### ✅ Câu trả lời

Deadlock xảy ra khi hai hoặc nhiều thread chờ nhau vô thời hạn.

```
Thread A: Lock Account A → Đợi Lock Account B
Thread B: Lock Account B → Đợi Lock Account A
→ Cả hai đều không thể tiếp tục
```

### 🔄 Interviewer hỏi tiếp: Trong hệ thống chuyển tiền thì Deadlock có thể xảy ra không?

**Có.** Ví dụ:
- Giao dịch 1: chuyển từ A → B (khóa A, chờ B)
- Giao dịch 2: chuyển từ B → A (khóa B, chờ A)

**Cách tránh:**
- Luôn khóa tài khoản theo cùng một thứ tự (ví dụ theo Account ID tăng dần)
- Giảm thời gian giữ lock
- Thiết kế transaction ngắn gọn
- Sử dụng cơ chế retry nếu DB phát hiện deadlock

---

## Câu 21: Hệ thống Internet Banking có 20.000 request/giây. Xử lý như thế nào?

> 🏆 **Câu ngân hàng cực thích**

### ✅ Câu trả lời

Em sẽ xem xét theo nhiều lớp:

| Lớp | Giải pháp |
|-----|-----------|
| **Load Balancer** | Phân phối request đến nhiều instance |
| **Horizontal Scaling** | Nhiều instance thay vì tăng cấu hình một máy |
| **Thread Pool** | Giới hạn số thread, tránh tạo thread không kiểm soát |
| **Database** | Tối ưu index, Connection Pool (HikariCP), Read/Write Separation |
| **Redis Cache** | Cache dữ liệu ít thay đổi |
| **Message Queue** | Xử lý bất đồng bộ (email, SMS, audit...) |
| **Monitoring** | CPU, Memory, GC, TPS, độ trễ, lỗi |

---

## 💡 Góc nhìn của Interviewer

> Một Tech Lead giỏi thường không trả lời theo một công nghệ duy nhất. Họ suy nghĩ theo nhiều tầng:
> - Ứng dụng (Application)
> - Thread & JVM
> - Database
> - Cache
> - Message Queue
> - Hạ tầng (Infrastructure)
> - Monitoring & Observability
>
> Đó chính là sự khác biệt giữa một Senior Developer và một Tech Lead.
