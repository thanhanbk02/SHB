# Chương 1 - Java Core (Phần 1)

> Câu 1 – 11 | Bao phủ khoảng 30% kiến thức Java Core thường gặp trong vòng phỏng vấn đầu tiên

---

## Câu 1: JVM là gì?

### 🎯 Nhà tuyển dụng muốn biết
- Có hiểu Java chạy như thế nào không?
- Có hiểu Memory không?

### ✅ Câu trả lời

JVM (Java Virtual Machine) là môi trường thực thi chương trình Java.

**Quá trình hoạt động:**

```
Java Source → javac → Bytecode (.class) → JVM → Machine Code → CPU
```

JVM giúp Java có thể chạy trên nhiều hệ điều hành mà không cần biên dịch lại, đúng với nguyên tắc **"Write Once, Run Anywhere"**.

Ngoài việc thực thi bytecode, JVM còn chịu trách nhiệm:
- Quản lý bộ nhớ (Memory Management)
- Garbage Collection
- Quản lý Thread
- Tối ưu thực thi bằng JIT Compiler
- Kiểm tra bảo mật khi chạy mã

Trong các hệ thống backend lớn như ngân hàng, việc hiểu JVM giúp tối ưu hiệu năng và xử lý các vấn đề về OutOfMemoryError, Full GC hoặc Thread Deadlock.

### 🔄 Interviewer hỏi tiếp: Tại sao Java lại chậm hơn C++?

Vì Java chạy qua JVM nên phải:
- Load Class
- Verify Bytecode
- Quản lý Garbage Collection
- JIT Compilation

Trong khi C++ biên dịch trực tiếp thành mã máy.

Tuy nhiên, nhờ JIT Compiler, hiệu năng Java hiện nay đã rất gần với C++ trong nhiều bài toán backend.

---

## Câu 2: JVM gồm những thành phần nào?

### ✅ Câu trả lời

JVM gồm các thành phần chính:

```
Class Loader → Runtime Data Area → Execution Engine → Garbage Collector → Native Interface
```

Trong đó:

| Thành phần | Mô tả |
|------------|--------|
| **Class Loader** | Nạp các file `.class` vào bộ nhớ |
| **Runtime Data Area** | Heap, Stack, Method Area, PC Register, Native Method Stack |
| **Execution Engine** | Interpreter + JIT Compiler |
| **Garbage Collector** | Thu hồi vùng nhớ không còn sử dụng |
| **JNI** | Cho phép Java gọi code C/C++ |

---

## Câu 3: Heap và Stack khác nhau thế nào?

> 💡 Đây là câu gần như chắc chắn sẽ gặp.

### ✅ Câu trả lời

| Tiêu chí | Stack | Heap |
|-----------|-------|------|
| Phạm vi | Thread riêng | Dùng chung |
| Tốc độ | Nhanh | Chậm hơn |
| Quản lý | Tự động giải phóng khi method kết thúc | GC quản lý |
| Lưu trữ | Local Variable | Object |

**Ví dụ:**

```java
User u = new User();
```

```
Stack                  Heap
┌─────────┐           ┌─────────────┐
│ u ──────────────────►│ User Object │
└─────────┘           └─────────────┘
```

- Biến `u` nằm trong Stack
- Đối tượng `User` nằm trong Heap

### 🔄 Interviewer hỏi tiếp: Tại sao Heap lại chậm hơn?

Heap phải:
- Cấp phát động
- Quản lý bộ nhớ
- Garbage Collection

Trong khi Stack chỉ tăng giảm con trỏ nên rất nhanh.

---

## Câu 4: Java truyền tham trị hay tham chiếu?

> 💡 Đây là câu đánh lừa rất nhiều ứng viên.

### ✅ Câu trả lời

**Java luôn truyền tham trị (Pass by Value).**

Tuy nhiên: Nếu truyền Object thì giá trị được truyền chính là **địa chỉ tham chiếu** của Object.

```java
User u = new User();
// Method nhận được bản sao của reference
```

Do đó:
- ✅ **Có thể** sửa object (`user.setName(...)`)
- ❌ **Không thể** đổi object (`user = new User()` — object bên ngoài không thay đổi)

> Đây là điểm rất nhiều ứng viên trả lời sai.

---

## Câu 5: String có immutable nghĩa là gì?

### ✅ Câu trả lời

**Immutable** nghĩa là không thể thay đổi sau khi được tạo.

```java
String s = "ABC";
s += "DEF";
// Không sửa object cũ — Java tạo object mới "ABCDEF"
// Object "ABC" vẫn tồn tại cho tới khi GC thu hồi
```

### 🔄 Interviewer hỏi tiếp: Tại sao Java lại thiết kế String immutable?

1. **Thread-safe** — Không cần lock
2. **Security** — Database Password, URL, File Path không bị sửa giữa chừng
3. **HashMap** — String thường làm Key, Immutable giúp hashCode không thay đổi. Nếu String thay đổi thì HashMap sẽ bị lỗi.

---

## Câu 6: `equals()` và `==` khác nhau thế nào?

### ✅ Câu trả lời

| Toán tử | So sánh |
|---------|---------|
| `==` | Địa chỉ (reference) |
| `equals()` | Nội dung (content) |

```java
String a = new String("abc");
String b = new String("abc");

a == b        // false
a.equals(b)   // true
```

### 🔄 Interviewer hỏi tiếp: Tại sao String lại override equals()?

Vì `Object` mặc định so sánh địa chỉ. String cần so sánh nội dung, do đó String override `equals()`.

---

## Câu 7: `hashCode()` dùng để làm gì?

### ✅ Câu trả lời

`hashCode()` tạo ra một số nguyên đại diện cho Object.

- HashMap dùng hashCode để xác định bucket
- Nếu hai object có `equals() = true` → `hashCode()` **phải** giống nhau
- Ngược lại: hashCode giống **chưa chắc** equals

---

## Câu 8: Tại sao phải override cả `equals()` và `hashCode()`?

### ✅ Câu trả lời

Nếu chỉ override `equals()` mà không override `hashCode()` thì **HashMap sẽ hoạt động sai**.

Ví dụ:
- Hai object giống nhau (`equals() = true`)
- Nhưng hashCode khác
- HashMap sẽ lưu thành **hai phần tử khác nhau**

> Đó là lỗi rất phổ biến.

---

## Câu 9: Integer Cache là gì?

> 💡 Đây là câu Senior Java rất thích hỏi.

### ✅ Câu trả lời

Java cache các Integer từ **-128** đến **127**.

```java
Integer a = 100;
Integer b = 100;
a == b  // true (cùng object từ cache)

Integer a = 200;
Integer b = 200;
a == b  // false (tạo object mới)
```

---

## Câu 10: Autoboxing là gì?

### ✅ Câu trả lời

Autoboxing là việc Java tự động chuyển `int` → `Integer`:

```java
Integer x = 10;
// Compiler chuyển thành: Integer.valueOf(10);

int y = x;
// Java tự Unboxing
```

---

## Câu 11: Nếu hệ thống đang chạy thì gặp OutOfMemoryError, anh sẽ xử lý như thế nào?

> 🏆 **Mức Tech Lead**

### ✅ Câu trả lời

Em sẽ xử lý theo thứ tự ưu tiên:

#### Bước 1: Khôi phục dịch vụ
- Nếu có nhiều instance → loại instance lỗi khỏi load balancer và khởi động lại
- Nếu chỉ có một instance → restart để giảm thời gian gián đoạn

#### Bước 2: Thu thập thông tin
- Kiểm tra log để xác định loại OOM (`Java heap space`, `Metaspace`, `GC overhead limit exceeded`...)
- Phân tích CPU, Memory, GC log
- Lấy Heap Dump nếu có thể

#### Bước 3: Phân tích nguyên nhân
- Memory Leak (object không được giải phóng)
- Cache tăng không giới hạn
- Đọc file lớn hoặc query trả về quá nhiều dữ liệu
- Thread bị giữ tham chiếu đến object
- Cấu hình JVM (`-Xms`, `-Xmx`) không phù hợp

#### Bước 4: Đưa ra giải pháp
- Sửa memory leak
- Phân trang hoặc stream dữ liệu thay vì load toàn bộ
- Giới hạn kích thước cache (ví dụ dùng Caffeine/Redis)
- Tối ưu cấu hình JVM và GC
- Bổ sung monitoring để cảnh báo trước khi bộ nhớ đầy

> 💡 **Điểm cộng khi phỏng vấn:** Không chỉ nói "restart server". Tech Lead cần thể hiện tư duy vừa xử lý sự cố nhanh, vừa tìm nguyên nhân gốc (Root Cause Analysis) và có biện pháp phòng ngừa.

---

## 📌 Nhận xét

Đây là 11 câu đầu tiên nhưng thực tế đã bao phủ khoảng **30% kiến thức Java Core** mà các Tech Lead thường được hỏi trong vòng phỏng vấn đầu tiên.
