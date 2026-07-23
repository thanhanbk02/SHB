# Chương 5 - Database & SQL Optimization

> Câu 42 – 51 | Index, Execution Plan, Partition, N+1 — phần ngân hàng hỏi rất sâu

---

## Câu 42: API từ 100ms tăng lên 8 giây sau khi dữ liệu đạt 100 triệu bản ghi. Xử lý như thế nào?

### 🎯 Nhà tuyển dụng muốn biết

Họ muốn xem **quy trình phân tích** của một Tech Lead, không phải câu "Em sẽ thêm Index."

### ✅ Câu trả lời

#### Bước 1: Kiểm tra Execution Plan
- Full Table Scan? Index Scan? Index Seek? Sort? Hash Join? Nested Loop?
- **Nếu chưa biết nguyên nhân mà đã sửa → dễ tối ưu sai**

#### Bước 2: Kiểm tra Index
- Các cột trong WHERE, JOIN, ORDER BY, GROUP BY đã có index chưa?

#### Bước 3: Kiểm tra Query
```sql
-- ❌ Không nên
WHERE YEAR(CreateTime) = 2026

-- ✅ Nên
WHERE CreateTime >= '2026-01-01' AND CreateTime < '2027-01-01'
```

#### Bước 4: Kiểm tra dữ liệu
- Tránh `SELECT *` khi chỉ cần vài cột

#### Bước 5: Cache
- Redis cho dữ liệu thay đổi ít

#### Bước 6: Partition
- Partition theo tháng/năm/region

#### Bước 7: Monitoring
- Slow Query Log, CPU, IO, Lock

---

## Câu 43: Index hoạt động như thế nào?

### ✅ Câu trả lời

Index giống **mục lục** của một cuốn sách.

- Không có Index → Database đọc từng dòng: **O(n)**
- Có Index (B-Tree) → Tìm kiếm: **O(log n)**

### 🔄 Tại sao không tạo Index cho tất cả các cột?

Index **không miễn phí**:
- Tốn RAM, Disk
- INSERT/UPDATE/DELETE chậm hơn
- Mỗi lần dữ liệu thay đổi, Index cũng phải cập nhật

→ Chỉ nên tạo Index trên cột thường xuyên WHERE, JOIN, ORDER BY, GROUP BY.

---

## Câu 44: Clustered Index và Non-Clustered Index khác nhau thế nào?

### ✅ Câu trả lời

| | Clustered Index | Non-Clustered Index |
|--|-----------------|---------------------|
| Lưu trữ | Quyết định thứ tự dữ liệu trên đĩa | Cây B-Tree riêng + pointer đến data |
| Số lượng | Mỗi bảng chỉ **1** | Nhiều |

### 🔄 Tại sao chỉ có một Clustered Index?

Vì dữ liệu vật lý chỉ có thể được sắp xếp theo **một thứ tự duy nhất**.

---

## Câu 45: Composite Index là gì?

### ✅ Câu trả lời

Composite Index là Index trên **nhiều cột**:

```
Index: (Name, Age, Gender)
```

Có thể tối ưu:
- ✅ `WHERE Name = ?`
- ✅ `WHERE Name = ? AND Age = ?`
- ✅ `WHERE Name = ? AND Age = ? AND Gender = ?`
- ❌ `WHERE Age = ?` (không tận dụng được)

Đây gọi là **Left-most Prefix Rule** — B-Tree được sắp xếp theo cột đầu tiên, bỏ qua cột đầu thì DB không xác định được vị trí bắt đầu.

---

## Câu 46: Covering Index là gì?

### ✅ Câu trả lời

Covering Index = tất cả các cột cần lấy đều đã có **trong Index**.

```sql
SELECT Name FROM User WHERE Age = 18
-- Nếu Index là (Age, Name) → chỉ đọc Index, không đọc bảng
```

→ Giảm IO rất nhiều.

---

## Câu 47: Tại sao SELECT * lại không tốt?

### ✅ Câu trả lời

- Đọc nhiều dữ liệu không cần thiết
- Tăng IO, Network
- Không tận dụng Covering Index

---

## Câu 48: EXISTS và IN khác nhau thế nào?

### ✅ Câu trả lời

- **IN** — Đơn giản, dễ đọc. Phù hợp khi Subquery trả về ít dữ liệu
- **EXISTS** — Hiệu quả khi Subquery trả về nhiều dữ liệu (có thể dừng ngay khi tìm thấy)

> Không chọn theo thói quen. Xem Execution Plan và Cost rồi quyết định.

---

## Câu 49: Phân trang khi bảng có 500 triệu bản ghi?

### ✅ Câu trả lời

```sql
-- ❌ Không nên
LIMIT 1000000, 20  -- DB phải bỏ qua 1 triệu dòng

-- ✅ Keyset Pagination
WHERE Id > LastId LIMIT 20  -- Tốc độ gần như không đổi
```

### 🔄 Nếu người dùng muốn nhảy tới trang 3000 thì sao?

- Giao diện quản trị/báo cáo: có thể dùng OFFSET kết hợp giới hạn phạm vi
- API/giao dịch: ưu tiên Keyset Pagination

---

## Câu 50: N+1 Query là gì?

### ✅ Câu trả lời

```
1 Query lấy 100 khách hàng
+ 100 Query lấy lịch sử giao dịch mỗi khách hàng
= 101 Query → N+1!
```

**Cách xử lý:**
- JOIN
- Batch Query
- Fetch Join (JPA)
- Entity Graph
- Cache nếu phù hợp

---

## Câu 51: Bảng Transaction có 2 tỷ bản ghi. Làm sao vẫn truy vấn nhanh?

> 🏆 **Câu rất hay ở ngân hàng**

### ✅ Câu trả lời

| Giải pháp | Mô tả |
|-----------|--------|
| **Index** | Theo account_id, transaction_date, status |
| **Partition** | Chia theo tháng/năm → query chỉ quét partition liên quan |
| **Archive** | Dữ liệu cũ (>5 năm) chuyển sang hệ thống lưu trữ riêng |
| **Read Replica** | Báo cáo chạy trên replica, giảm tải DB chính |
| **Redis Cache** | Cache dữ liệu truy cập nhiều, ít thay đổi |
| **Tối ưu SQL** | Chỉ lấy cột cần thiết, kiểm tra Execution Plan |
| **Monitoring** | Slow Query, lock, CPU, IO, connection pool |

---

## 🎯 Góc nhìn của Interviewer

> Câu trả lời tốt không phải "Em sẽ tạo Index."
> Mà là: "Em sẽ phân tích bằng Execution Plan, xác định nguyên nhân, sau đó lựa chọn giải pháp phù hợp."
> Đó là tư duy của một Tech Lead.
