# SHB Backend Tech Lead Interview

> **Tài liệu ôn tập & phỏng vấn vị trí Tech Lead Backend tại ngân hàng**  
> Tổng hợp 120+ câu hỏi phỏng vấn thực tế từ Java Core, Spring Boot, Transaction, Database, System Design, Microservice, Message Queue, Redis, Security đến Leadership và xử lý sự cố Production.

---

## Mục lục

- [Chương 1 - Java Core (Phần 1)](#chương-1---java-core-phần-1)
- [Chương 2 - Java Core (Collection & Concurrency)](#chương-2---java-core-collection--concurrency)
- [Chương 3 - Spring Boot & Spring Framework](#chương-3---spring-boot--spring-framework)
- [Chương 4 - Transaction (Cực kỳ quan trọng)](#chương-4---transaction-cực-kỳ-quan-trọng)
- [Chương 5 - Database & SQL Optimization](#chương-5---database--sql-optimization)
- [Chương 6 - System Design](#chương-6---system-design)
- [Chương 7 - Microservice](#chương-7---microservice)
- [Chương 8 - Message Queue (RabbitMQ & Kafka)](#chương-8---message-queue-rabbitmq--kafka)
- [Chương 9 - Redis](#chương-9---redis)
- [Chương 10 - Security](#chương-10---security)
- [Chương 11 - Leadership & Team Management](#chương-11---leadership--team-management)
- [Chương 12 - Production Incident](#chương-12---production-incident)

---

## Chương 1 - Java Core (Phần 1)

### Câu 1: JVM là gì?

**JVM (Java Virtual Machine)** là môi trường thực thi chương trình Java.

**Quá trình hoạt động:**

```
Java Source → javac → Bytecode (.class) → JVM → Machine Code → CPU
```

JVM giúp Java có thể chạy trên nhiều hệ điều hành mà không cần biên dịch lại, đúng với nguyên tắc **"Write Once, Run Anywhere"**.

**Ngoài việc thực thi bytecode, JVM còn chịu trách nhiệm:**
- Quản lý bộ nhớ (Memory Management)
- Garbage Collection
- Quản lý Thread
- Tối ưu thực thi bằng JIT Compiler
- Kiểm tra bảo mật khi chạy mã

> 💡 **Trong các hệ thống backend lớn như ngân hàng**, việc hiểu JVM giúp tối ưu hiệu năng và xử lý các vấn đề về `OutOfMemoryError`, Full GC hoặc Thread Deadlock.

**Hỏi tiếp: Tại sao Java lại chậm hơn C++?**

Vì Java chạy qua JVM nên phải:
- Load Class
- Verify Bytecode
- Quản lý Garbage Collection
- JIT Compilation

Trong khi C++ biên dịch trực tiếp thành mã máy. Tuy nhiên, nhờ JIT Compiler, hiệu năng Java hiện nay đã rất gần với C++ trong nhiều bài toán backend.

---

### Câu 2: JVM gồm những thành phần nào?

```
Class Loader → Runtime Data Area → Execution Engine → Garbage Collector → Native Interface
```

| Thành phần | Chức năng |
|------------|-----------|
| **Class Loader** | Nạp các file `.class` vào bộ nhớ |
| **Runtime Data Area** | Bao gồm: Heap, Stack, Method Area, PC Register, Native Method Stack |
| **Execution Engine** | Chạy bytecode (Interpreter, JIT Compiler) |
| **Garbage Collector** | Thu hồi vùng nhớ không còn sử dụng |
| **JNI** | Cho phép Java gọi code C/C++ |

---

### Câu 3: Heap và Stack khác nhau thế nào?

| Tiêu chí | Stack | Heap |
|----------|-------|------|
| Phạm vi | Thread riêng | Dùng chung |
| Tốc độ | Nhanh | Chậm hơn |
| Giải phóng | Tự động khi method kết thúc | GC quản lý |
| Lưu trữ | Local Variable, tham chiếu | Object |

**Ví dụ:**
```java
User u = new User();
```
- Biến `u` nằm trong **Stack**
- Đối tượng `User` nằm trong **Heap**

**Hỏi tiếp: Tại sao Heap lại chậm hơn?**

Heap phải cấp phát động, quản lý bộ nhớ, Garbage Collection. Trong khi Stack chỉ tăng giảm con trỏ nên rất nhanh.

---

### Câu 4: Java truyền tham trị hay tham chiếu?

**Java luôn truyền tham trị (Pass by Value).**

Tuy nhiên:
- Nếu truyền Object thì giá trị được truyền chính là **địa chỉ tham chiếu** của Object.
- Method nhận được **bản sao của reference**.

**Kết quả:**
- Có thể sửa object (qua method)
- Không thể đổi object (gán reference mới không ảnh hưởng bên ngoài)

```java
void change(User user) {
    user.setName("A"); // Object bên ngoài BỊ thay đổi
    user = new User(); // Object bên ngoài KHÔNG thay đổi
}
```

---

### Câu 5: String có immutable nghĩa là gì?

**Immutable** nghĩa là không thể thay đổi sau khi được tạo.

```java
String s = "ABC";
s += "DEF";
// Không sửa object cũ. Java tạo object mới "ABCDEF"
// Object cũ "ABC" vẫn tồn tại cho tới khi GC thu hồi
```

**Tại sao Java lại thiết kế String immutable?**
1. **Thread-safe**: Không cần lock
2. **Security**: Database Password, URL, File Path không bị sửa giữa chừng
3. **HashMap**: String thường làm Key. Immutable giúp `hashCode` không thay đổi.

---

### Câu 6: `equals()` và `==` khác nhau thế nào?

| Toán tử | Ý nghĩa |
|---------|---------|
| `==` | So sánh địa chỉ (reference) |
| `equals()` | So sánh nội dung |

```java
String a = new String("abc");
String b = new String("abc");
a == b;     // false
a.equals(b); // true
```

**Tại sao String lại override `equals()`?**

Vì Object mặc định so sánh địa chỉ. String cần so sánh nội dung nên override `equals()`.

---

### Câu 7: `hashCode()` dùng để làm gì?

`hashCode` tạo ra một số nguyên đại diện cho Object. HashMap dùng `hashCode` để xác định bucket.

**Quy tắc:**
- Nếu hai object có `equals()` = `true` thì bắt buộc `hashCode()` phải giống nhau.
- Ngược lại: `hashCode` giống chưa chắc `equals`.

---

### Câu 8: Tại sao phải override cả `equals()` và `hashCode()`?

Nếu chỉ override `equals` mà không override `hashCode` thì HashMap sẽ hoạt động sai.

**Ví dụ lỗi phổ biến:**
- Hai object giống nhau → `equals()` = `true`
- Nhưng `hashCode` khác → HashMap lưu thành hai phần tử khác nhau

---

### Câu 9: Integer Cache là gì?

Java cache các Integer từ **-128 đến 127**.

```java
Integer a = 100;
Integer b = 100;
a == b; // true (cùng object trong cache)

Integer a = 200;
Integer b = 200;
a == b; // false (tạo object mới)
```

---

### Câu 10: Autoboxing là gì?

Autoboxing là việc Java tự động chuyển `int` → `Integer`.

```java
Integer x = 10; // Compiler tự chuyển thành Integer.valueOf(10)
int y = x;      // Unboxing
```

---

### Câu 11 (Mức Tech Lead): Nếu hệ thống đang chạy thì gặp OutOfMemoryError, anh sẽ xử lý như thế nào?

**Bước 1: Khôi phục dịch vụ**
- Nếu có nhiều instance: loại instance lỗi khỏi load balancer và khởi động lại
- Nếu chỉ một instance: restart để giảm thời gian gián đoạn

**Bước 2: Thu thập thông tin**
- Kiểm tra log xác định loại OOM (Java heap space, Metaspace, GC overhead...)
- Phân tích CPU, Memory, GC log
- Lấy Heap Dump nếu có thể

**Bước 3: Phân tích nguyên nhân phổ biến**
- Memory Leak (object không được giải phóng)
- Cache tăng không giới hạn
- Đọc file lớn hoặc query trả về quá nhiều dữ liệu
- Thread bị giữ tham chiếu đến object
- Cấu hình JVM (`-Xms`, `-Xmx`) không phù hợp

**Bước 4: Đưa ra giải pháp**
- Sửa memory leak
- Phân trang hoặc stream dữ liệu thay vì load toàn bộ
- Giới hạn kích thước cache (Caffeine/Redis)
- Tối ưu cấu hình JVM và GC
- Bổ sung monitoring để cảnh báo trước khi bộ nhớ đầy

> 🎯 **Điểm cộng:** Không chỉ nói "restart server". Tech Lead cần thể hiện tư duy vừa xử lý sự cố nhanh, vừa tìm nguyên nhân gốc (Root Cause Analysis) và có biện pháp phòng ngừa.

---

## Chương 2 - Java Core (Collection & Concurrency)

### Câu 12: So sánh ArrayList và LinkedList

| Tiêu chí | ArrayList | LinkedList |
|----------|-----------|------------|
| Cấu trúc | Dynamic Array | Doubly Linked List |
| Truy cập index | O(1) | O(n) |
| Thêm/xóa cuối | O(1) trung bình | O(1) |
| Thêm/xóa đầu hoặc giữa | O(n) | O(1) nếu đã có vị trí |
| Bộ nhớ | Ít | Nhiều hơn (lưu prev/next) |
| Cache CPU | Tốt | Kém |

**Trong backend thực tế:** ArrayList được dùng phổ biến hơn vì đa số thao tác là đọc dữ liệu và duyệt danh sách. LinkedList chỉ phù hợp khi thường xuyên thêm/xóa ở đầu hoặc giữa.

**Hỏi tiếp: Nếu phải lưu 10 triệu bản ghi thì chọn cái nào?**

Chọn **ArrayList** vì ít tốn bộ nhớ hơn, locality của CPU cache tốt hơn, tốc độ duyệt nhanh hơn.

---

### Câu 13: ArrayList tăng kích thước như thế nào khi đầy?

Khi mảng bên trong đầy, ArrayList sẽ tạo mảng mới có kích thước khoảng **1.5 lần** mảng cũ, sau đó copy toàn bộ dữ liệu sang.

```
10 → 15 → 22 → 33
```

Việc copy có chi phí O(n). Nếu biết trước số lượng phần tử, nên khởi tạo trước capacity:

```java
List<User> users = new ArrayList<>(100000);
```

**Ảnh hưởng nếu resize nhiều lần:**
- Tăng thời gian copy dữ liệu
- Tăng số lần cấp phát bộ nhớ
- Tăng áp lực cho Garbage Collector

---

### Câu 14: HashSet hoạt động như thế nào?

HashSet thực chất được xây dựng dựa trên **HashMap**.

Khi thêm một phần tử:
```java
set.add(user);
// Key = user, Value = PRESENT (một object giả)
```

Do HashMap không cho phép trùng key nên HashSet cũng không cho phép phần tử trùng.

**Hỏi tiếp: HashSet có đảm bảo thứ tự không?**

Không. Nếu cần giữ thứ tự thêm vào → dùng `LinkedHashSet`. Nếu cần tự động sắp xếp → dùng `TreeSet`.

---

### Câu 15: TreeMap khác HashMap như thế nào?

| Đặc điểm | HashMap | TreeMap |
|----------|---------|---------|
| Sắp xếp | Không | Luôn sắp xếp theo key |
| Cấu trúc | Hash Table | Red-Black Tree |
| Tìm kiếm | O(1) trung bình | O(log n) |

Nếu hệ thống cần lấy dữ liệu theo thứ tự hoặc range query (`firstKey`, `lastKey`, `subMap`), TreeMap phù hợp hơn.

**Hỏi tiếp: Nếu lưu 5 triệu bản ghi thì dùng cái nào?**

Nếu chỉ tra cứu theo key → **HashMap** (O(1)). Chỉ chọn TreeMap khi thực sự cần dữ liệu có thứ tự.

---

### Câu 16: Iterator và fail-fast là gì?

Các Collection như ArrayList sử dụng Iterator để duyệt. Nếu trong khi duyệt mà Collection bị thay đổi trực tiếp:

```java
for(User u : list) {
    list.remove(u); // Ném ConcurrentModificationException
}
```

Đây gọi là cơ chế **Fail-Fast**.

Nếu muốn xóa trong lúc duyệt thì dùng:
```java
iterator.remove();
```

**Hỏi tiếp: ConcurrentHashMap có bị Fail-Fast không?**

Không. ConcurrentHashMap sử dụng **Weakly Consistent Iterator** — iterator vẫn hoạt động dù Collection đang bị thay đổi bởi thread khác.

---

### Câu 17: Thread là gì?

Thread là đơn vị thực thi nhỏ nhất trong một process. Một ứng dụng Java có thể có nhiều thread chạy đồng thời.

**Ví dụ trong hệ thống ngân hàng:**
- Thread xử lý HTTP Request
- Thread ghi log
- Thread gửi email
- Thread xử lý Message Queue

---

### Câu 18: Thread và Process khác nhau như thế nào?

- **Process**: Một chương trình đang chạy, có vùng nhớ riêng
- **Thread**: Đơn vị thực thi bên trong Process

Các thread **dùng chung Heap**, nhưng mỗi thread có **Stack riêng**.

---

### Câu 19: Race Condition là gì?

Race Condition xảy ra khi nhiều thread cùng truy cập và thay đổi một tài nguyên mà không có cơ chế đồng bộ.

```java
count++; // Hai thread cùng đọc 5, cùng tăng lên 6 → Kết quả 6 thay vì 7
```

**Cách tránh:** `synchronized`, `ReentrantLock`, `AtomicInteger`, `ConcurrentHashMap`, Immutable Object.

---

### Câu 20: Deadlock là gì?

Deadlock xảy ra khi hai hoặc nhiều thread chờ nhau vô thời hạn.

```
Thread A: Lock Account A → Đợi Lock Account B
Thread B: Lock Account B → Đợi Lock Account A
```

**Hỏi tiếp: Trong hệ thống chuyển tiền thì Deadlock có thể xảy ra không?**

Có. Ví dụ: Giao dịch 1 chuyển A→B, Giao dịch 2 chuyển B→A. Nếu cùng khóa ngược thứ tự → deadlock.

**Giải pháp:**
- Luôn khóa tài khoản theo cùng một thứ tự (ví dụ theo Account ID tăng dần)
- Giảm thời gian giữ lock
- Thiết kế transaction ngắn gọn
- Sử dụng cơ chế retry nếu DB phát hiện deadlock

---

### Câu 21 (Câu ngân hàng cực thích): Hệ thống Internet Banking có khoảng 20.000 request/giây. Anh sẽ xử lý như thế nào?

Em sẽ xem xét theo nhiều lớp:

1. **Load Balancer**: Phân phối request đến nhiều instance
2. **Horizontal Scaling**: Chạy nhiều instance thay vì chỉ tăng cấu hình một máy
3. **Thread Pool**: Giới hạn số thread xử lý, tránh tạo thread không kiểm soát
4. **Database**: Tối ưu index, Read/Write Separation, Connection Pool (HikariCP)
5. **Redis Cache**: Cache dữ liệu ít thay đổi (cấu hình, danh mục)
6. **Message Queue**: Đưa tác vụ không cần phản hồi ngay (email, SMS, audit) sang xử lý bất đồng bộ
7. **Monitoring**: Theo dõi CPU, Memory, GC, TPS, độ trễ, lỗi để phát hiện sớm

> 🎯 **Góc nhìn của Interviewer:** Đến câu này, người phỏng vấn không còn đánh giá bạn ở mức "biết Java" nữa, mà đang xem bạn có tư duy vận hành hệ thống lớn hay không.

---

## Chương 3 - Spring Boot & Spring Framework

### Câu 22: Spring Boot và Spring Framework khác nhau như thế nào?

**Spring Framework** là framework gốc, cung cấp: IoC, DI, AOP, Transaction Management, MVC, Security...

**Spring Boot** được xây dựng trên Spring Framework với mục tiêu giảm cấu hình và tăng tốc độ phát triển:
- Auto Configuration
- Starter Dependencies
- Embedded Tomcat
- Actuator
- Convention over Configuration

**Hỏi tiếp: Spring Boot có thay thế Spring Framework không?**

Không. Spring Boot không thay thế mà được xây dựng trên Spring Framework. Có thể hiểu Spring Boot là một cách sử dụng Spring Framework nhanh và tiện hơn.

---

### Câu 23: IoC là gì? Tại sao Spring lại dùng IoC?

**IoC (Inversion of Control)** là nguyên lý đảo ngược quyền điều khiển việc tạo và quản lý object.

- **Java thông thường**: `UserService service = new UserService();` — Developer tự tạo
- **Spring**: `@Autowired private UserService service;` — Spring Container tạo và quản lý

**Lợi ích:**
- Giảm phụ thuộc giữa các class
- Dễ mở rộng
- Dễ unit test
- Dễ thay thế implementation

**Hỏi tiếp: Nếu không có IoC thì có vấn đề gì?**

Các class phụ thuộc chặt vào nhau. Khi cần thay đổi hoặc mock để test sẽ rất khó.

---

### Câu 24: Bean là gì?

Bean là object được Spring quản lý trong IoC Container. Spring chịu trách nhiệm:
- Tạo Bean
- Inject dependency
- Quản lý vòng đời
- Hủy Bean khi ứng dụng dừng

**Các annotation tạo Bean:** `@Component`, `@Service`, `@Repository`, `@Controller`, `@Configuration`

**Hỏi tiếp: Có phải object nào cũng là Bean không?**

Không. Chỉ những object được Spring quản lý mới gọi là Bean. `new User()` chỉ là object Java bình thường.

---

### Câu 25: Spring Bean có những Scope nào?

| Scope | Mô tả |
|-------|-------|
| **Singleton** | Chỉ tạo một instance (mặc định) |
| **Prototype** | Mỗi lần lấy Bean tạo object mới |
| **Request** | Một Bean cho mỗi HTTP Request |
| **Session** | Một Bean cho mỗi Session |
| **Application** | Một Bean cho toàn bộ ServletContext |

**Trong backend ngân hàng:** Gần như toàn bộ Service đều dùng **Singleton** vì ít tốn bộ nhớ, dễ quản lý, hiệu năng tốt. Nếu Bean có state thì phải thiết kế thread-safe.

---

### Câu 26: Bean Lifecycle diễn ra như thế nào?

```
Instantiate → Inject Dependency → @PostConstruct → Ready → Destroy → @PreDestroy
```

- `@PostConstruct`: Khởi tạo tài nguyên sau khi inject xong
- `@PreDestroy`: Giải phóng tài nguyên trước khi Bean bị hủy

---

### Câu 27: `@Autowired` hoạt động như thế nào?

Khi Spring khởi động:
1. Quét package (Component Scan)
2. Tạo Bean
3. Tìm dependency phù hợp
4. Inject vào Bean

Spring ưu tiên inject theo **type**. Nếu có nhiều Bean cùng type → dùng `@Qualifier` hoặc `@Primary`.

**Hỏi tiếp: Nếu có 2 Bean cùng interface thì sao?**

Spring báo lỗi `NoUniqueBeanDefinitionException`. Cần dùng `@Qualifier("visaPayment")` hoặc đánh dấu một Bean bằng `@Primary`.

---

### Câu 28: Constructor Injection tốt hơn Field Injection ở điểm nào?

**Ưu tiên Constructor Injection vì:**
- Dependency bắt buộc phải được truyền vào khi khởi tạo
- Có thể khai báo field là `final`
- Dễ viết Unit Test (không cần Spring Context)
- Tránh NullPointerException
- Phụ thuộc được thể hiện rõ ràng

**Hỏi tiếp: Vậy tại sao nhiều project vẫn dùng Field Injection?**

Do viết nhanh và các project cũ sử dụng từ lâu. Tuy nhiên, với project mới hoặc khi làm Tech Lead, nên thống nhất dùng Constructor Injection.

---

### Câu 29: Spring Boot Auto Configuration hoạt động như thế nào?

1. `@SpringBootApplication` kích hoạt Auto Configuration
2. Spring đọc các cấu hình trong `spring.factories`
3. Kiểm tra các thư viện có trong classpath
4. Nếu điều kiện phù hợp (`@ConditionalOnClass`, `@ConditionalOnMissingBean`...), Spring tự tạo Bean

**Ví dụ:** Nếu có `spring-boot-starter-data-jpa` → Spring tự tạo `DataSource`, `EntityManager`, `TransactionManager`.

**Hỏi tiếp: Nếu không muốn Auto Configuration thì sao?**

```java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
```
Hoặc tự định nghĩa Bean để ghi đè Bean mặc định.

---

### Câu 30: `@Component`, `@Service`, `@Repository`, `@Controller` khác nhau thế nào?

Về bản chất, cả bốn đều đánh dấu class để Spring quản lý như một Bean.

| Annotation | Ý nghĩa |
|------------|---------|
| `@Component` | Chung cho mọi Bean |
| `@Service` | Tầng nghiệp vụ (Business Logic) |
| `@Repository` | Tầng truy cập dữ liệu + chuyển đổi exception persistence sang Spring exception |
| `@Controller` | Xử lý HTTP Request trong Spring MVC |

---

### Câu 31 (Mức Tech Lead): Bean Singleton có biến instance để lưu trạng thái. Có vấn đề gì không?

**Có, đây là rủi ro lớn.**

Bean Singleton chỉ có một instance dùng chung cho tất cả request. Nếu chứa state có thể thay đổi → nhiều request chạy đồng thời sẽ ghi đè dữ liệu của nhau → Race Condition, dữ liệu sai.

```java
@Service
public class PaymentService {
    private String currentUser; // KHÔNG AN TOÀN
}
```

**Thiết kế đúng:**
- Service nên **stateless** (không lưu trạng thái theo request)
- Mọi dữ liệu cần xử lý được truyền qua tham số method
- Nếu bắt buộc phải có state theo request → dùng request scope hoặc lưu trong đối tượng request

> 🎯 **Góc nhìn của Interviewer:** Singleton không có nghĩa là thread-safe. Thread-safe phụ thuộc vào việc Bean có state hay không. Quy tắc quan trọng: **Service nên stateless**.

---

## Chương 4 - Transaction (Cực kỳ quan trọng)

### Câu 32: Transaction là gì? Vì sao hệ thống ngân hàng bắt buộc phải dùng?

Transaction là một tập hợp nhiều thao tác được thực hiện như một đơn vị công việc duy nhất (Unit of Work).

**Kết quả chỉ có hai:**
- **Commit**: Thành công toàn bộ
- **Rollback**: Thất bại toàn bộ

**Ví dụ chuyển tiền:**
1. Trừ tiền A
2. Cộng tiền B
3. Ghi lịch sử giao dịch

Nếu sau khi trừ tiền A mà hệ thống bị lỗi trước khi cộng tiền B → **mất tiền**. Cả ba bước phải nằm trong cùng một Transaction.

---

### Câu 33: ACID là gì?

| Chữ | Ý nghĩa | Ví dụ ngân hàng |
|-----|---------|-----------------|
| **A**tomicity | Hoặc thành công tất cả, hoặc rollback tất cả | Không thể đã trừ tiền mà chưa cộng tiền |
| **C**onsistency | Sau Transaction dữ liệu vẫn hợp lệ | Tổng tiền trong hệ thống không tự nhiên tăng/giảm |
| **I**solation | Các Transaction không ảnh hưởng lẫn nhau | |
| **D**urability | Sau khi Commit, dữ liệu lưu vĩnh viễn | Kể cả server bị tắt |

**Trong ngân hàng:** Atomicity và Consistency là quan trọng nhất.

---

### Câu 34: Spring `@Transactional` hoạt động như thế nào?

Spring sử dụng **AOP Proxy**:

```
Controller → Spring Proxy → Open Transaction → Execute SQL → Commit → Close Transaction
```

Nếu xảy ra RuntimeException → Spring sẽ **rollback**.

**Hỏi tiếp: Tại sao gọi method trong cùng class thì `@Transactional` không chạy?**

Vì method gọi trực tiếp trong cùng object không đi qua Proxy → Transaction không được tạo. Đây là lỗi rất nhiều developer gặp phải.

---

### Câu 35: `@Transactional` mặc định rollback khi nào?

Mặc định Spring chỉ rollback khi gặp:
- `RuntimeException`
- `Error`

**Không rollback** đối với Checked Exception (ví dụ: `IOException`).

Nếu muốn rollback mọi exception:
```java
@Transactional(rollbackFor = Exception.class)
```

**Tại sao Spring thiết kế như vậy?**

RuntimeException thường đại diện cho lỗi không thể phục hồi. Checked Exception thường là tình huống có thể dự đoán và xử lý, nên Spring để lập trình viên chủ động quyết định.

---

### Câu 36: Propagation là gì?

Propagation quy định cách Transaction hiện tại tương tác với Transaction đang tồn tại.

| Chế độ | Mô tả |
|--------|-------|
| **REQUIRED** (mặc định) | Có Transaction thì dùng, chưa có thì tạo mới |
| **REQUIRES_NEW** | Luôn tạo Transaction mới, Transaction cũ bị tạm dừng |
| **SUPPORTS** | Có Transaction thì dùng, không có cũng chạy bình thường |
| **MANDATORY** | Bắt buộc phải có Transaction, không có sẽ báo lỗi |
| **NEVER** | Nếu đang có Transaction → báo lỗi |
| **NOT_SUPPORTED** | Nếu có Transaction → tạm dừng, chạy không Transaction |

---

### Câu 37: Khi nào dùng REQUIRES_NEW?

**Ví dụ:** Chuyển tiền gọi Audit Log. Nếu Audit Log lỗi, không nên rollback giao dịch chuyển tiền.

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void auditLog() { ... }
```

Audit có Transaction riêng. Transfer vẫn Commit bình thường.

**Nếu Audit ghi thất bại thì sao?**
- Ghi log lỗi
- Retry nếu phù hợp
- Đưa vào Message Queue hoặc Dead Letter Queue

> Quan trọng là không ảnh hưởng đến giao dịch chính.

---

### Câu 38: Isolation Level là gì?

Isolation quy định mức độ cô lập giữa các Transaction.

| Isolation | Ngăn được |
|-----------|-----------|
| Read Uncommitted | Không ngăn được nhiều vấn đề |
| Read Committed | Dirty Read |
| Repeatable Read | Dirty Read + Non-repeatable Read |
| Serializable | Tất cả, kể cả Phantom Read |

Isolation càng cao → dữ liệu càng an toàn nhưng hiệu năng giảm do tăng mức khóa.

---

### Câu 39: Dirty Read là gì?

Transaction A UPDATE Balance từ 100→200, **chưa Commit**. Transaction B đọc được 200. Sau đó Transaction A rollback. Thực tế Balance vẫn là 100 nhưng B đã đọc dữ liệu chưa được commit.

---

### Câu 40: Lost Update là gì?

Balance = 1000
- Thread A: Đọc 1000 → -100 → Ghi 900
- Thread B: Đọc 1000 → -200 → Ghi 800

**Kết quả cuối: 800** (Đúng ra phải là 700). Một cập nhật đã bị ghi đè.

**Cách tránh:**
- Optimistic Lock (`@Version` trong JPA)
- Pessimistic Lock (`SELECT ... FOR UPDATE`)
- Atomic Update ở mức SQL
- Serializable Isolation (ít dùng)

---

### Câu 41: Optimistic Lock và Pessimistic Lock

**Optimistic Lock**
- Giả định xung đột hiếm khi xảy ra
- Mỗi bản ghi có cột `version`
- Khi cập nhật: `UPDATE ... SET version = version + 1 WHERE ... AND version = ?`
- Ưu điểm: Hiệu năng cao, không giữ lock lâu
- Nhược điểm: Có thể phải retry

**Pessimistic Lock**
- Giả định xung đột thường xuyên
- Ngay khi đọc sẽ khóa bản ghi: `SELECT ... FOR UPDATE`
- Ưu điểm: An toàn, tránh xung đột ngay từ đầu
- Nhược điểm: Giảm khả năng xử lý song song, nguy cơ deadlock

**Hỏi tiếp: Nếu là hệ thống Internet Banking thì chọn loại nào?**

- Dữ liệu cập nhật thấp (hồ sơ khách hàng) → **Optimistic Lock**
- Nghiệp vụ số dư, chuyển tiền → **Pessimistic Lock** hoặc khóa nghiệp vụ để đảm bảo chính xác

> 🎯 **Tech Lead không trả lời "luôn dùng A" mà giải thích dựa trên bối cảnh nghiệp vụ.**

---
## Chương 5 - Database & SQL Optimization

### Câu 42: API lấy danh sách giao dịch tăng từ 100ms lên 8 giây sau khi dữ liệu đạt 100 triệu bản ghi. Xử lý thế nào?

**Tech Lead không nói ngay "thêm Index" mà phân tích theo bước:**

1. **Kiểm tra Execution Plan**: Full Table Scan? Index Scan? Sort? Hash Join?
2. **Kiểm tra Index**: Các cột trong WHERE, JOIN, ORDER BY, GROUP BY đã có index chưa?
3. **Kiểm tra Query**: 
   - Không nên: `WHERE YEAR(CreateTime)=2026`
   - Nên: `WHERE CreateTime >= '2026-01-01' AND CreateTime < '2027-01-01'`
4. **Kiểm tra dữ liệu**: Tránh `SELECT *` khi frontend chỉ cần vài cột
5. **Cache**: Redis nếu dữ liệu thay đổi ít
6. **Partition**: Chia bảng theo tháng/năm/region
7. **Monitoring**: Slow Query Log, CPU, IO, Lock

---

### Câu 43: Index hoạt động như thế nào?

Index giống như mục lục của cuốn sách. Đa số Database sử dụng **B-Tree** → tìm kiếm chỉ mất **O(log n)** thay vì **O(n)**.

**Tại sao không tạo Index cho tất cả các cột?**
- Tốn RAM, tốn Disk
- INSERT/UPDATE/DELETE chậm hơn (index cũng phải cập nhật)
- Chỉ nên tạo Index trên các cột thường xuyên: WHERE, JOIN, ORDER BY, GROUP BY

---

### Câu 44: Clustered Index và Non-Clustered Index

| Loại | Đặc điểm |
|------|----------|
| **Clustered Index** | Quyết định thứ tự lưu dữ liệu thật trên đĩa. Mỗi bảng chỉ có một. |
| **Non-Clustered Index** | Cây B-Tree riêng, lưu Key → Pointer → Data. Một bảng có thể có nhiều. |

**Tại sao chỉ có một Clustered Index?**

Vì dữ liệu vật lý chỉ có thể được sắp xếp theo một thứ tự duy nhất.

---

### Câu 45: Composite Index là gì?

Index trên nhiều cột, ví dụ: `(Name, Age, Gender)`

**Left-most Prefix Rule:**
- Tận dụng được: `WHERE Name`, `WHERE Name AND Age`, `WHERE Name AND Age AND Gender`
- Không tận dụng được: `WHERE Age` (bỏ qua cột đầu tiên)

Vì B-Tree được sắp xếp theo cột đầu tiên.

---

### Câu 46: Covering Index là gì?

Trường hợp tất cả các cột cần lấy đều đã có trong Index.

```sql
SELECT Name FROM User WHERE Age=18
-- Index (Age, Name) → Database chỉ cần đọc Index, không cần đọc bảng
```

Giúp giảm IO rất nhiều.

---

### Câu 47: Tại sao `SELECT *` lại không tốt?

- Đọc nhiều dữ liệu không cần thiết
- Tăng IO, tăng Network
- Không tận dụng Covering Index

---

### Câu 48: EXISTS và IN khác nhau thế nào?

- **IN**: Thường đơn giản, dễ đọc. Phù hợp khi subquery trả về ít dữ liệu.
- **EXISTS**: Hiệu quả hơn khi subquery trả về nhiều dữ liệu vì Database có thể dừng ngay khi tìm thấy bản ghi phù hợp.

> Em không chọn theo thói quen mà xem Execution Plan, Cost, rồi quyết định.

---

### Câu 49: Phân trang như thế nào khi bảng có 500 triệu bản ghi?

**Không nên:**
```sql
LIMIT 1000000, 20 -- Database phải bỏ qua 1 triệu dòng, rất chậm
```

**Nên dùng Keyset Pagination:**
```sql
WHERE Id > LastId LIMIT 20 -- Tốc độ gần như không đổi
```

**Nếu người dùng muốn nhảy tới trang 3000?**
- Giao diện quản trị/báo cáo: có thể dùng OFFSET kết hợp giới hạn phạm vi
- Giao diện giao dịch/API: ưu tiên Keyset Pagination

---

### Câu 50: N+1 Query là gì?

Lấy 100 khách hàng, sau đó mỗi khách hàng lại query thêm lịch sử giao dịch → 1 + 100 = 101 query.

**Cách xử lý:** JOIN, Batch Query, Fetch Join (JPA), Entity Graph, Cache.

---

### Câu 51: Bảng Transaction có 2 tỷ bản ghi. Làm sao truy vấn nhanh?

1. **Index phù hợp**: Theo `account_id`, `transaction_date`, `status`
2. **Partition**: Chia bảng theo tháng/năm
3. **Archive**: Dữ liệu cũ (trên 5 năm) chuyển sang hệ thống lưu trữ riêng
4. **Read Replica**: Truy vấn báo cáo chuyển sang replica
5. **Redis Cache**: Cache dữ liệu ít thay đổi
6. **Tối ưu SQL**: Chỉ lấy cột cần thiết, tránh `SELECT *`, kiểm tra Execution Plan định kỳ
7. **Monitoring**: Slow Query, lock, CPU, IO, connection pool

> 🎯 **Một Tech Lead luôn phân tích bằng Execution Plan trước rồi mới tối ưu.**

---

## Chương 6 - System Design

### Câu 52: Thiết kế hệ thống chuyển tiền giữa hai tài khoản trong cùng ngân hàng

```
Client → API Gateway → Authentication/Rate Limiter → Transfer Service
                                      ↓
                        ┌─────────────┴─────────────┐
                        ↓                           ↓
                Account Service         Transaction Service
                        └─────────────┬─────────────┘
                                      ↓
                              Database Cluster
                                      ↓
                               Message Queue
                                      ↓
                        ┌─────────────┴─────────────┐
                        ↓                           ↓
                Notification Service         Audit Service
```

**Giải thích:**
- **API Gateway**: Authentication, Authorization, Rate Limit, Logging
- **Transfer Service**: Check Balance → Lock Account → Debit → Credit → Commit → Publish Event
- **Database**: Dùng Transaction đảm bảo Debit + Credit + History phải Commit cùng lúc
- **MQ**: Sau khi Commit, đẩy event `TransferSuccess` đến MQ để xử lý SMS/Email/Audit bất đồng bộ

**Hỏi tiếp: Tại sao không gửi SMS luôn?**

Nếu gửi SMS trực tiếp → SMS Timeout → API chậm → Khách hàng phải chờ. Notification nên xử lý bất đồng bộ.

---

### Câu 53: Khách hàng bấm nút chuyển tiền liên tục thì sao?

**Idempotency Key:**

```
Client gửi TransactionId = 123456
Server lưu: 123456 → SUCCESS
Nếu request gửi lại → kiểm tra đã tồn tại → trả về kết quả cũ
```

**Hỏi tiếp: Nếu hai request tới cùng lúc?**

Chỉ kiểm tra bằng code là chưa đủ (race condition). Cần:
- Ràng buộc UNIQUE trên `transaction_id`
- Thực hiện trong cùng Transaction
- Nếu insert thất bại do trùng khóa → trả về kết quả giao dịch đã xử lý trước đó

---

### Câu 54: Sau khi trừ tiền xong, Server bị Crash thì sao?

- **Nếu chưa Commit** → Database rollback → Không mất tiền
- **Nếu đã Commit** → Dữ liệu đã an toàn, sau khi server lên vẫn còn

**Hỏi tiếp: Message Queue chưa gửi thì sao?**

Sử dụng **Transactional Outbox Pattern**:
- Trong cùng một DB transaction: Cập nhật số dư + Ghi lịch sử + Ghi bản ghi vào `outbox_events`
- Sau khi commit thành công, tiến trình riêng đọc bảng Outbox và gửi message lên MQ
- Đảm bảo: Không gửi message khi rollback + Không làm mất message nếu crash sau commit

---

### Câu 55: Nếu Database bị Down?

- **Read**: Dùng Read Replica
- **Write**: Không thể ghi vào Redis thay thế cho giao dịch tài chính. Phải:
  - Trả lỗi phù hợp cho client
  - Failover sang DB dự phòng nếu có
  - Không được chấp nhận giao dịch khi chưa ghi thành công vào hệ thống lưu trữ chính

---

### Câu 56: Nếu RabbitMQ bị chết?

- Cấu hình queue và message ở chế độ **durable/persistent**
- Publisher sử dụng **Publisher Confirm**
- Consumer xử lý theo cơ chế **ACK và retry**
- Thất bại nhiều lần → chuyển sang **Dead Letter Queue (DLQ)**
- Thiết lập cluster/HA cho RabbitMQ
- Nhờ Outbox Pattern, event vẫn nằm trong database và sẽ được gửi lại khi MQ hoạt động

---

### Câu 57: Làm sao đảm bảo một Message không xử lý hai lần?

Không nên giả định MQ chỉ giao message một lần. Consumer phải **idempotent**:

```
Message TransferSuccess ID=99999
Consumer kiểm tra: 99999 đã xử lý chưa?
  → Rồi → Bỏ qua
  → Chưa → Xử lý
```

**Cách kiểm tra:** Bảng `processed_message`, Unique Key, TransactionId.

**Tại sao Kafka/RabbitMQ có thể gửi trùng?**

Vì phần lớn MQ đảm bảo **At Least Once Delivery**. Nếu consumer xử lý xong nhưng chưa ACK, server chết → gửi lại.

---

### Câu 58: 100.000 giao dịch cùng chuyển vào một tài khoản?

Đây là **hotspot**:
- Khóa ở mức bản ghi (row-level lock) thay vì khóa cả bảng
- Giữ transaction ngắn nhất có thể
- Thiết kế cập nhật theo hướng atomic
- Nếu nghiệp vụ cho phép, dùng hàng đợi để tuần tự hóa các cập nhật trên cùng một tài khoản

---

### Câu 59: Làm sao phát hiện hệ thống đang có vấn đề?

**Monitoring theo nhiều lớp:**

| Lớp | Chỉ số |
|-----|--------|
| Application | TPS, Latency, Error Rate |
| JVM | Heap, GC, Thread |
| Database | Slow Query, Connection Pool, Deadlock |
| MQ | Queue Length, Consumer Lag, Retry |
| Infrastructure | CPU, RAM, Disk, Network |

Cấu hình cảnh báo (alert) để đội vận hành phát hiện sớm.

---

### Câu 60: Nếu thiết kế Internet Banking từ đầu, ưu tiên điều gì nhất?

1. **Tính đúng đắn của dữ liệu (Correctness)** — Giao dịch sai còn nguy hiểm hơn giao dịch chậm
2. **Tính sẵn sàng (Availability)** — Tránh điểm lỗi đơn, có khả năng failover
3. **Bảo mật** — Xác thực, phân quyền, mã hóa, Audit Log, chống replay/brute force
4. **Khả năng mở rộng** — Microservice, horizontal scaling, cache, MQ
5. **Khả năng quan sát (Observability)** — Logging, Metrics, Tracing, Alerting

---

## Chương 7 - Microservice

### Câu 61: Microservice là gì?

Kiến trúc chia hệ thống thành nhiều dịch vụ nhỏ, mỗi dịch vụ chịu trách nhiệm một nghiệp vụ riêng, có thể phát triển, triển khai và mở rộng độc lập.

**Ví dụ Internet Banking:**
- Authentication Service
- Transfer Service
- Account Service
- Notification Service
- Audit Service
- Report Service

---

### Câu 62: Tại sao nhiều công ty vẫn dùng Monolith?

Microservice không phải lúc nào cũng tốt hơn. Nếu hệ thống nhỏ, ít module, ít người phát triển → **Monolith đơn giản hơn**.

**Ưu điểm Monolith:**
- Dễ debug
- Dễ deploy
- Transaction đơn giản
- Không cần Service Discovery, API Gateway
- Không có Distributed Transaction

**Khi nào chuyển sang Microservice?**
- Nhiều module độc lập
- Các nhóm thường xuyên xung đột khi cùng sửa codebase
- Một số chức năng cần scale mạnh hơn
- Build/deploy Monolith quá dài
- Một lỗi nhỏ buộc phải redeploy toàn bộ

---

### Câu 63: Nhược điểm lớn nhất của Microservice?

**Độ phức tạp tăng lên.**

Monolith: Service → Database

Microservice: Service A → Service B → Service C → MQ → Redis → DB

Phát sinh thêm:
- Distributed Transaction
- Network Timeout
- Retry, Circuit Breaker
- Service Discovery
- Logging phân tán, Tracing, Monitoring
- Version API

---

### Câu 64: Mỗi Microservice có nên dùng chung Database không?

**Không.** Theo nguyên tắc Microservice, mỗi service nên sở hữu database riêng.

- Các service không truy cập trực tiếp DB của nhau
- Có thể thay đổi schema mà ít ảnh hưởng
- Dễ mở rộng và triển khai độc lập

**Nếu Transfer cần thông tin User?**
- Gọi User Service qua REST/gRPC nếu cần real-time
- Hoặc đồng bộ một phần dữ liệu qua event

---

### Câu 65: Hai Microservice giao tiếp bằng cách nào?

| Nhóm | Phương thức | Khi nào dùng |
|------|-------------|--------------|
| **Đồng bộ** | REST API, gRPC | Cần kết quả ngay (Login, Validate OTP) |
| **Bất đồng bộ** | RabbitMQ, Kafka | Không cần phản hồi tức thì (Email, SMS, Audit) |

---

### Câu 66: Nếu Service B bị Down?

Thiết kế theo nhiều lớp bảo vệ:
- **Timeout**: Không để request chờ quá lâu
- **Retry**: Có kiểm soát, giới hạn số lần
- **Circuit Breaker**: Nếu Service B liên tục lỗi → ngừng gọi trong một khoảng thời gian
- **Fallback**: Trả dữ liệu cache hoặc thông báo dịch vụ tạm thời không khả dụng
- **Monitoring**: Theo dõi tỷ lệ lỗi, độ trễ, số lần Circuit Breaker mở

---

### Câu 67: Circuit Breaker hoạt động như thế nào?

Ba trạng thái:

1. **Closed**: Hoạt động bình thường
2. **Open**: Lỗi vượt ngưỡng → Ngừng gọi Service B, trả lỗi ngay
3. **Half Open**: Sau một thời gian, cho phép một số request thử
   - Thành công → Closed
   - Tiếp tục lỗi → Open

**Tại sao cần Circuit Breaker?**

Nếu không có: 1000 request đều gọi Service chết → Thread bị giữ → Connection Pool đầy → Toàn bộ hệ thống bị ảnh hưởng.

---

### Câu 68: Nếu một request phải gọi 10 service?

Đây là dấu hiệu cần xem lại thiết kế. Một request gọi quá nhiều service sẽ:
- Tăng độ trễ
- Tăng xác suất lỗi
- Khó debug

**Cân nhắc:**
- Gom dữ liệu qua API Gateway hoặc BFF (Backend For Frontend)
- Chuyển một số xử lý sang bất đồng bộ
- Thiết kế lại ranh giới giữa các service

---

### Câu 69: Làm sao trace một request đi qua 20 Microservice?

**Distributed Tracing:**
- Mỗi request được gắn **Trace ID** và **Span ID**
- Các service ghi cùng Trace ID vào log
- Có thể theo dõi toàn bộ đường đi của request

**Công cụ:** OpenTelemetry, Jaeger, Zipkin

---

### Câu 70: Giao dịch chuyển tiền giữa hai Microservice thì dùng Transaction như thế nào?

**Không dùng Two-Phase Commit (2PC)** vì hiệu năng thấp, khóa tài nguyên lâu, khó mở rộng.

**Ưu tiên Saga Pattern:**
- Mỗi service thực hiện transaction cục bộ
- Nếu một bước thất bại → Compensating Transaction hoàn tác các bước trước

```
Transfer Service: Trừ tiền A → Event → Account Service: Cộng tiền B
  → Nếu lỗi → Compensate: Hoàn tiền A
```

Kết hợp: Idempotency, Outbox Pattern, Retry có kiểm soát, Consumer idempotent → đảm bảo **Eventual Consistency**.

> 🎯 **Tech Lead không tuyệt đối hóa một giải pháp. Luôn nói: Bối cảnh → Trade-off → Lý do chọn.**

---
## Chương 8 - Message Queue (RabbitMQ & Kafka)

### Câu 71: Tại sao cần Message Queue? Tại sao không gọi REST trực tiếp?

MQ giúp **tách biệt (decouple)** các service và xử lý bất đồng bộ.

**Ví dụ:** Nếu API phải trừ tiền + gửi SMS + Email + Push + Audit + Đồng bộ BI → Thời gian phản hồi rất lâu.

**Thiết kế:**
```
Transfer → Commit DB → Publish Event → RabbitMQ → Notification/Audit/Email/SMS
```

API chỉ xử lý nghiệp vụ chính. Các tác vụ phụ chạy nền. Người dùng nhận phản hồi nhanh hơn.

**Nếu Notification chết?** Không ảnh hưởng. Transfer đã Commit. Notification có thể Retry.

---

### Câu 72: RabbitMQ khác Kafka như thế nào?

| Đặc điểm | RabbitMQ | Kafka |
|----------|----------|-------|
| Mô hình | Queue | Log |
| Message | Thường bị xóa sau ACK | Giữ theo Retention |
| Routing | Linh hoạt | Đơn giản |
| Phù hợp | Transaction, workflow | Event Streaming, Big Data |
| Latency | Thấp | Throughput rất cao |

**Lựa chọn:**
- Email, SMS, OTP, workflow nghiệp vụ → **RabbitMQ**
- Event Streaming, Audit Log quy mô lớn, Data Lake → **Kafka**
- Trong nhiều hệ thống lớn, cả hai có thể cùng tồn tại.

---

### Câu 73: ACK là gì?

ACK là tín hiệu Consumer gửi về Broker: *"Tôi đã xử lý Message thành công."*

```
RabbitMQ → Message → Consumer → ACK → Delete Message
```

**Nếu Consumer chết trước khi ACK?** RabbitMQ gửi lại Message cho Consumer khác. Do đó Consumer phải **idempotent**.

---

### Câu 74: At Most Once, At Least Once, Exactly Once

| Semantic | Đảm bảo | Ví dụ |
|----------|---------|-------|
| **At Most Once** | Có thể mất, không xử lý 2 lần | Message bị mất nếu consumer chết trước xử lý |
| **At Least Once** | Đảm bảo xử lý ≥1 lần, có thể trùng | **Phổ biến nhất** |
| **Exactly Once** | Đúng 1 lần | Khó triển khai, cần phối hợp broker + ứng dụng |

**RabbitMQ** thường triển khai theo **At Least Once**.

---

### Câu 75: Idempotent Consumer là gì?

Nếu cùng một Message được gửi nhiều lần, kết quả vẫn chỉ được xử lý một lần.

```
Message TransferSuccess ID=999
Consumer nhận: 999 → Process → Save
Nếu nhận lại: 999 → Đã xử lý → Bỏ qua
```

**Làm sao biết đã xử lý?**
- Bảng `processed_message`
- Unique Constraint
- TransactionId / EventId

---

### Câu 76: Dead Letter Queue là gì?

DLQ là Queue chứa các Message không thể xử lý thành công sau nhiều lần Retry.

```
Consumer → Retry → Retry → Retry → DLQ
```

Sau đó Dev hoặc Ops kiểm tra nguyên nhân và xử lý lại.

**Tại sao không Retry vô hạn?**
- Queue sẽ bị đầy
- Consumer bị chiếm tài nguyên
- Các Message khác bị chậm

---

### Câu 77: Message Ordering là gì?

Một số nghiệp vụ yêu cầu Message phải được xử lý theo đúng thứ tự.

```
+100 → -50 → +30
```

Nếu xử lý sai thứ tự → Số dư sẽ sai.

**RabbitMQ có đảm bảo thứ tự không?**

Có thể giữ thứ tự trong một queue với một consumer, nhưng khi có nhiều consumer xử lý song song thì thứ tự tổng thể không còn được đảm bảo.

**Giải pháp:**
- Phân vùng theo khóa nghiệp vụ
- Chỉ một consumer xử lý một nhóm dữ liệu

---

### Câu 78: Consumer Scale như thế nào?

```
Queue → Consumer 1 (không đủ)
Queue → Consumer 1 + Consumer 2 + Consumer 3
```

RabbitMQ sẽ phân phối Message.

**Có phải cứ thêm Consumer là nhanh hơn?**

Không. Nếu Database là bottleneck, thêm Consumer không có tác dụng, thậm chí gây Lock, Deadlock, Connection Pool đầy.

---

### Câu 79: Làm sao biết Queue đang gặp vấn đề?

Theo dõi:
- Queue Length
- Consumer Rate
- Publish Rate
- Retry Count
- DLQ Count
- Processing Time

Nếu Queue Length tăng liên tục trong khi Consumer Rate thấp → Hệ thống đang bị nghẽn.

---

### Câu 80: Một Message xử lý mất 30 phút. Consumer chết ở phút thứ 29. Thì sao?

Nếu chưa ACK, RabbitMQ sẽ gửi lại Message. Do đó Consumer phải:
- **Idempotent**
- Có **Checkpoint** nếu xử lý dài
- Chia nhỏ công việc nếu có thể

Không nên để một Message xử lý quá lâu.

---

### 🎯 Câu hỏi nâng cao (Mức Architect)

> **"Hệ thống chuyển tiền phát sinh khoảng 50.000 giao dịch/phút. RabbitMQ bị down trong 5 phút. Sau khi khởi động lại, hàng trăm nghìn Message tràn vào Consumer. Làm sao xử lý?"**

**Trả lời mẫu:**

Em sẽ không chỉ tăng Consumer ngay. Đầu tiên đánh giá:

1. **Consumer có idempotent không?** Nếu chưa, scale lên sẽ sinh lỗi.
2. **Database chịu được bao nhiêu TPS?** Nếu DB chỉ chịu 3000 TPS, scale Consumer lên 10000 TPS chỉ làm DB chết nhanh hơn.
3. **Có Rate Limit không?** Consumer nên đọc Queue theo tốc độ DB chịu được.
4. **Monitoring**: Queue backlog, Consumer lag, DB CPU, Connection Pool.
5. **Scale từ từ**: Không tăng từ 5 → 100 Consumer, mà tăng dần và theo dõi hệ thống.

> 🎯 **Interviewer không còn đánh giá "Anh biết RabbitMQ không?" mà đánh giá "Nếu giao hệ thống Production cho anh, anh có biết cách vận hành khi có sự cố không?"**

---

## Chương 9 - Redis

### Câu 81: Redis là gì?

Redis là một **in-memory data store**. Dữ liệu chủ yếu được lưu trên RAM nên tốc độ đọc/ghi rất nhanh (micro giây đến vài mili giây).

Redis không chỉ là cache mà còn dùng cho:
- Cache
- Distributed Lock
- Session
- Rate Limiter
- Counter
- Pub/Sub
- Stream

**Tại sao Redis lại nhanh?**
- Dữ liệu nằm trong RAM
- Cấu trúc dữ liệu tối ưu (Hash, Set, Sorted Set...)
- Thiết kế event loop đơn luồng
- Giao tiếp mạng đơn giản

---

### Câu 82: Dữ liệu nào nên Cache?

Những dữ liệu:
- Đọc nhiều
- Thay đổi ít
- Không yêu cầu chính xác tuyệt đối theo từng mili giây

**Ví dụ:** Danh mục tỉnh/thành, danh sách ngân hàng, cấu hình hệ thống, tỷ giá (nếu chấp nhận độ trễ ngắn).

**Hỏi tiếp: Có nên Cache số dư tài khoản không?**

Thông thường **không nên** dùng cache làm nguồn dữ liệu chính cho số dư. Số dư thay đổi liên tục và ảnh hưởng trực tiếp đến giao dịch tài chính. Nếu cần cache để giảm tải đọc, phải có chiến lược đồng bộ rất chặt chẽ và luôn xác định database mới là nguồn dữ liệu chính (source of truth).

---

### Câu 83: Cache Aside Pattern là gì?

Chiến lược cache phổ biến nhất:

```
Application → Redis → Miss → Database → Redis → Client
```

- Lần đầu: Không có Cache → Đọc Database → Ghi vào Redis
- Các lần sau: Chỉ đọc Redis

**Nếu Update Database thì sao?**

Sau khi cập nhật Database, xóa Cache hoặc cập nhật lại Cache để tránh dữ liệu cũ. Thông thường **Delete Cache** được dùng nhiều hơn.

---

### Câu 84: Cache Penetration là gì?

User liên tục query `UserId = 999999999999` (không tồn tại). Redis không có, Database cũng không có. Mỗi request đều xuống Database → Có thể làm Database quá tải.

**Cách xử lý:**
- Cache giá trị rỗng (Null Cache) với TTL ngắn
- Dùng Bloom Filter để loại bỏ các key chắc chắn không tồn tại

---

### Câu 85: Cache Breakdown là gì?

Một key rất nóng (ví dụ: Exchange Rate) TTL hết. 100.000 request cùng xuống Database → Database bị quá tải.

**Cách xử lý:**
- Mutex/Distributed Lock để chỉ một request tải lại dữ liệu
- Làm mới cache trước khi hết hạn (Refresh Ahead)
- Thiết kế TTL hợp lý

---

### Câu 86: Cache Avalanche là gì?

Nhiều key hết TTL cùng lúc. 100.000 key expire → Database quá tải.

**Cách xử lý:**
- Thêm ngẫu nhiên TTL (ví dụ: 3600~3900 thay vì cố định 3600)
- Không để tất cả hết hạn cùng lúc

---

### Câu 87: Distributed Lock là gì?

Trong hệ thống có nhiều instance (Server A, B, C) cùng xử lý Update Interest → Có thể xảy ra xung đột.

Redis có thể dùng để tạo khóa phân tán:
```
SET lock_key value NX PX 30000
```
Chỉ một server lấy được lock. Các server khác phải chờ.

**Tại sao không dùng `synchronized`?**

`synchronized` chỉ có tác dụng trong phạm vi một JVM. Nếu có nhiều server, mỗi server vẫn có lock riêng → Không đồng bộ toàn hệ thống.

---

### Câu 88: RedLock là gì?

RedLock là thuật toán khóa phân tán do Redis đề xuất. Ý tưởng là lấy lock trên nhiều Redis node độc lập và chỉ coi là thành công khi đạt được đa số (quorum).

Tuy nhiên, RedLock vẫn còn nhiều tranh luận. Nếu hệ thống yêu cầu mức nhất quán rất cao, cân nhắc kỹ giữa Redis Lock và ZooKeeper/etcd.

---

### Câu 89: Redis mất dữ liệu thì sao?

Redis có thể cấu hình lưu dữ liệu xuống đĩa bằng:
- **RDB** (snapshot định kỳ)
- **AOF** (Append Only File)

Nếu Redis chỉ dùng làm cache → Mất dữ liệu không quá nghiêm trọng. Hệ thống sẽ đọc lại từ Database.

Nếu Redis dùng cho Distributed Lock hoặc Session → Cần thiết kế HA, replication và chiến lược khôi phục.

---

### Câu 90: Redis Cluster hoạt động như thế nào?

Redis Cluster chia dữ liệu thành **16384 hash slot**:
- Node A: 0 → 5000
- Node B: 5001 → 10000
- Node C: 10001 → 16383

Khi thêm node, Redis tự cân bằng dữ liệu → Mở rộng theo chiều ngang (horizontal scaling).

---

### Câu 91: Khi nào KHÔNG nên dùng Redis?

Không dùng Redis làm nguồn dữ liệu chính cho các nghiệp vụ yêu cầu tính chính xác tuyệt đối:
- Số dư tài khoản
- Trạng thái cuối cùng của giao dịch chuyển tiền
- Dữ liệu kế toán

Redis nên đóng vai trò hỗ trợ: Cache, Counter, Session, Rate Limiter.

---

### Câu 92: Giả sử Redis bị Down. Hệ thống sẽ xảy ra chuyện gì?

- **Nếu Redis là Cache**: Hệ thống vẫn hoạt động. Database chịu tải lớn hơn, Response Time tăng.
- **Nếu Redis là Session**: Có thể người dùng bị Logout.
- **Nếu Redis là Distributed Lock**: Một số chức năng phụ thuộc lock có thể tạm thời không hoạt động.

> **Redis là thành phần tăng hiệu năng, không phải nơi lưu trữ dữ liệu nghiệp vụ cốt lõi.**

---

### ⭐ Câu SHB rất thích: "Tại sao không Cache toàn bộ Database?"

1. **RAM không đủ**: Database 20 TB, Redis 64 GB → Không thể chứa hết.
2. **Đồng bộ dữ liệu rất khó**: Database đã update, Redis chưa update → Dữ liệu sai.
3. **Chi phí rất lớn**: RAM đắt hơn Disk.
4. **Không phải dữ liệu nào cũng được đọc**: Có bảng 100 triệu dòng nhưng 1 năm không ai đọc → Cache là lãng phí.

> 🎯 **Interviewer không đánh giá "Anh có biết Redis không?" mà đánh giá "Anh có biết khi nào KHÔNG dùng Redis không?"**

---

## Chương 10 - Security

### Câu 93: Anh sẽ bảo vệ API chuyển tiền như thế nào?

Bảo vệ theo nhiều lớp (Defense in Depth):

1. **HTTPS/TLS**: Mã hóa toàn bộ dữ liệu trên đường truyền. Không cho phép HTTP.
2. **Authentication**: JWT, OAuth2, Session
3. **Authorization**: Kiểm tra quyền chuyển tiền, xem tài khoản, xem lịch sử
4. **Rate Limit**: Ví dụ 5 request/giây → Tránh Brute Force
5. **Idempotency**: Khách hàng bấm nhiều lần không được chuyển tiền nhiều lần
6. **Audit Log**: Ghi User, IP, Device, Time, TransactionId
7. **Monitoring**: Theo dõi Login Fail, TPS, Fraud

**Chỉ HTTPS có đủ không?**

Không. HTTPS chỉ bảo vệ dữ liệu khi truyền trên mạng. Không bảo vệ: SQL Injection, XSS, CSRF, Broken Access Control, Logic Bug, Replay Attack.

---

### Câu 94: JWT là gì?

JWT (JSON Web Token) là chuẩn token dùng để xác thực. Gồm ba phần:
```
Header.Payload.Signature
```

Payload có thể chứa: UserId, Role, Expire Time. Signature dùng để chống sửa token.

**Có nên lưu Password trong JWT không?**

Không. JWT không được thiết kế để lưu dữ liệu nhạy cảm. Payload có thể đọc được nếu giải mã Base64. Chỉ nên lưu: UserId, Role, Permission, Token Expiration.

---

### Câu 95: JWT có nhược điểm gì?

JWT là **Stateless** → Server không lưu Session. Nếu token có thời hạn 24 giờ thì trong khoảng thời gian đó, Server rất khó thu hồi ngay.

**Ví dụ:** User đổi Password → Token cũ vẫn còn hiệu lực.

**Làm sao Logout?**
- Access Token ngắn hạn (10-15 phút) + Refresh Token
- Lưu Refresh Token trong database/Redis để thu hồi
- Blacklist token trong trường hợp đặc biệt

---

### Câu 96: OAuth2 là gì?

OAuth2 là **chuẩn ủy quyền (Authorization)**, không phải chuẩn xác thực.

**Ví dụ:** Đăng nhập bằng Google → Website A không biết Password Google, chỉ nhận Access Token.

**OAuth khác JWT thế nào?**

Đây là hai khái niệm khác nhau:
- OAuth2 là giao thức ủy quyền
- JWT là định dạng token

OAuth2 có thể sử dụng JWT làm Access Token, nhưng cũng có thể dùng các loại token khác.

---

### Câu 97: SQL Injection là gì?

```java
String sql = "SELECT * FROM User WHERE username='" + username + "'";
// Nếu User nhập: ' OR 1=1 --
// SQL trở thành: SELECT * FROM User WHERE username='' OR 1=1
```

**Cách chống:**
- Không nối chuỗi để tạo SQL
- Dùng Prepared Statement, Parameterized Query
- Dùng ORM (JPA, MyBatis) đúng cách
- Validate Input
- Phân quyền tài khoản DB theo Least Privilege

---

### Câu 98: XSS là gì?

User nhập `<script>alert(1)</script>`. Nếu Website render luôn → Javascript chạy.

**Cách chống:**
- Escape Output
- Encode HTML
- Content Security Policy (CSP)
- Không render HTML từ dữ liệu người dùng nếu không cần

---

### Câu 99: CSRF là gì?

CSRF xảy ra khi trình duyệt tự động gửi Cookie xác thực đến server mà người dùng không chủ động thực hiện.

**Ví dụ:** Người dùng đã đăng nhập Internet Banking, truy cập website độc hại → Website gửi `POST /transfer` → Browser tự gửi Cookie → Server tin là User thật.

**JWT có bị CSRF không?**

Nếu JWT được lưu trong `Authorization Header` → thông thường không bị CSRF vì trình duyệt không tự động gửi header này.

Nếu JWT lưu trong Cookie → vẫn cần CSRF Token hoặc cấu hình SameSite.

---

### Câu 100: Replay Attack là gì?

Tin tặc nghe lén được Request chuyển 100 triệu → Gửi lại nhiều lần. Nếu Server không kiểm tra → Tiền chuyển nhiều lần.

**Cách chống:**
- Idempotency Key
- Timestamp
- Nonce
- Chữ ký số (Digital Signature)
- Thời gian hết hạn ngắn cho request

Server từ chối: Request quá cũ, Nonce đã sử dụng, Chữ ký không hợp lệ.

---

### Câu 101: Dữ liệu nào trong Database cần mã hóa?

| Loại dữ liệu | Cách xử lý |
|--------------|------------|
| CCCD, Số thẻ, Thông tin cá nhân nhạy cảm | Mã hóa hoặc bảo vệ đặc biệt |
| CVV | Theo PCI DSS, thường không được lưu sau khi xác thực |
| Password | **Hash** bằng Argon2, bcrypt hoặc PBKDF2 + salt |

**Tại sao Password không Encrypt?**

Vì khi đăng nhập, Server không cần biết Password gốc. Chỉ cần so sánh Hash. Nếu mã hóa đối xứng, ai có khóa giải mã đều có thể xem được Password gốc.

---

### Câu 102: Nếu Database bị Hack thì sao?

Thiết kế theo nguyên tắc **"giả định có thể bị xâm nhập"**:
- Password được hash
- Dữ liệu nhạy cảm được mã hóa
- Phân quyền tài khoản DB theo Least Privilege
- Audit Log
- Giám sát truy cập bất thường
- Backup được mã hóa
- Quản lý khóa mã hóa bằng KMS/HSM

---

### ⭐ Câu SHB rất thích: "Nếu anh thiết kế Internet Banking, anh sợ điều gì nhất?"

Không phải hệ thống chậm, mà là:
- **Sai giao dịch hoặc giao dịch gian lận**
- Chuyển tiền hai lần
- Sai số dư
- Người A xem được tài khoản người B
- Replay Attack
- Broken Authorization

**Ưu tiên:**
- Thiết kế đúng nghiệp vụ
- Kiểm soát phân quyền
- Ghi nhận Audit
- Theo dõi bất thường
- Kiểm thử bảo mật trước khi phát hành

> 🎯 **Interviewer sẽ nghĩ: "Nếu giao người này phụ trách Internet Banking, liệu họ có thiết kế hệ thống đủ an toàn để bảo vệ tiền của khách hàng không?"**

---

## Chương 11 - Leadership & Team Management

### Câu 103: Tech Lead khác Senior Developer ở điểm nào?

**Senior Developer** chịu trách nhiệm chính về chất lượng phần code mình phát triển.

**Tech Lead** chịu trách nhiệm về thành công của cả team và sản phẩm. Ngoài giải quyết bài toán kỹ thuật, còn cần:
- Định hướng kiến trúc
- Phân chia công việc
- Review thiết kế và code
- Hỗ trợ các thành viên
- Quản lý rủi ro kỹ thuật
- Phối hợp với QA, BA, DevOps và Product

**Tech Lead có nên code không?**

Có, nhưng không nên là người code nhiều nhất. Thường dành thời gian cho: module khó, review kiến trúc, mentoring, xử lý production issue.

---

### Câu 104: Một Junior code rất chậm. Anh làm gì?

Không đánh giá ngay là yếu. Đầu tiên hiểu nguyên nhân:
- Chưa quen domain?
- Thiếu kiến thức Java?
- Chưa hiểu yêu cầu?
- Thiếu kinh nghiệm debug?

**Sau đó:**
- Pair Programming
- Review code thường xuyên
- Chia task nhỏ hơn
- Đặt mục tiêu theo từng tuần

Nếu sau một thời gian vẫn không cải thiện → Trao đổi thẳng thắn, đưa ra Improvement Plan với mục tiêu và thời hạn rõ ràng.

---

### Câu 105: Hai Senior cãi nhau về kỹ thuật. Anh xử lý thế nào?

Không quyết định theo người có chức vụ cao hơn hoặc nói to hơn.

Yêu cầu cả hai trình bày:
- Giải pháp
- Ưu điểm
- Nhược điểm
- Ảnh hưởng tới hệ thống

**Đánh giá dựa trên:**
- Yêu cầu nghiệp vụ
- Hiệu năng
- Khả năng bảo trì
- Chi phí triển khai
- Rủi ro

Nếu cần, làm Proof of Concept (PoC) hoặc benchmark để có dữ liệu thực tế trước khi quyết định.

---

### Câu 106: Một Developer không chịu Review Code của người khác. Anh làm sao?

Code Review là trách nhiệm của cả team. Trao đổi riêng để hiểu nguyên nhân:
- Quá bận?
- Không tự tin?
- Không thấy giá trị?

**Sau đó:**
- Đưa Code Review vào Definition of Done
- Quy định rõ thời gian review
- Hướng dẫn cách review hiệu quả
- Theo dõi qua Pull Request

Mục tiêu: Xây dựng văn hóa chia sẻ kiến thức.

---

### Câu 107: Production bị Down lúc 2 giờ sáng. Anh làm gì?

**Ưu tiên theo thứ tự:**

1. **Khôi phục dịch vụ (Restore Service)**
   - Rollback, chuyển sang node dự phòng, restart nếu phù hợp
   - Mục tiêu: Giảm thời gian gián đoạn

2. **Thông báo**
   - Team, người quản lý, các bên liên quan

3. **Điều tra nguyên nhân**
   - Log, Monitoring, Metrics, Tracing
   - Không suy đoán khi chưa có dữ liệu

4. **Root Cause Analysis**
   - Postmortem sau khi hệ thống ổn định

5. **Preventive Action**
   - Alert, Test, Monitoring, Quy trình Release

**Ai chịu trách nhiệm?**

Là Tech Lead. Việc đầu tiên là khôi phục dịch vụ và phối hợp xử lý. Sau đó mới phân tích nguyên nhân để cải thiện quy trình, không tập trung vào việc đổ lỗi cá nhân.

---

### Câu 108: Deadline còn 2 ngày. Team nói không kịp. Anh làm gì?

Đánh giá lại:
- Còn bao nhiêu việc?
- Task nào là bắt buộc?
- Task nào có thể lùi?

Trao đổi với Product Owner/Business để thống nhất phạm vi. Nếu cần, chia thành Release 1 và Release 2 thay vì cố làm tất cả rồi giảm chất lượng.

**Có bắt OT không?**

OT có thể cần trong tình huống khẩn cấp, nhưng không nên là giải pháp mặc định. Nếu dự án thường xuyên phải OT → Dấu hiệu estimation chưa tốt, scope thay đổi nhiều, planning có vấn đề.

---

### Câu 109: Anh Review Code theo tiêu chí gì?

| Tiêu chí | Nội dung |
|----------|----------|
| **Correctness** | Code đúng nghiệp vụ chưa? |
| **Readability** | Tên biến, tên method có rõ ràng không? |
| **Maintainability** | Có dễ sửa? Có tách module hợp lý? |
| **Performance** | Có N+1 Query? Có query tối ưu không? |
| **Security** | Có SQL Injection? Có lộ dữ liệu? Có kiểm tra quyền? |
| **Testing** | Có Unit Test? Có xử lý các trường hợp lỗi? |

---

### Câu 110: Một Tech Lead giỏi cần gì?

1. **Technical**: Đủ khả năng giải quyết bài toán khó và định hướng kỹ thuật
2. **Communication**: Trao đổi hiệu quả với Developer, QA, BA, Product, DevOps, Quản lý
3. **Leadership**: Tạo điều kiện để các thành viên phát triển, không ôm hết việc, biết trao quyền
4. **Decision Making**: Dám đưa ra quyết định khi thông tin chưa hoàn hảo, chịu trách nhiệm và sẵn sàng điều chỉnh

---

### ⭐ Câu SHB có thể hỏi: "Một Developer vừa gây ra lỗi làm hệ thống Internet Banking bị Down. Anh xử lý thế nào?"

Việc đầu tiên **không phải tìm người trách** mà là:
1. **Khôi phục hệ thống**
2. **Phân tích:** Vì sao lỗi vào Production? Unit Test? Code Review? CI/CD? QA? Monitoring?

Thông thường một sự cố nghiêm trọng là kết quả của nhiều lỗ hổng trong quy trình, không chỉ từ một cá nhân.

**Sau khi phân tích:**
- Chia sẻ bài học với cả team
- Cập nhật checklist review
- Bổ sung test nếu thiếu
- Cải thiện quy trình release

Nếu developer mắc lỗi do thiếu kinh nghiệm → mentoring. Nếu vi phạm quy trình nhiều lần sau khi đã được hỗ trợ → xử lý theo quy định.

---

## Chương 12 - Production Incident

### Câu 111: 10 giờ sáng. Khách hàng gọi: "Internet Banking rất chậm." Anh làm gì?

**Không bao giờ trả lời: "Em restart server."**

**Bước 1: Xác nhận phạm vi**
- Chậm tất cả API hay chỉ Transfer?
- Chỉ Mobile hay cả Web?

**Bước 2: Kiểm tra Dashboard**
- CPU, RAM, TPS, Latency, Error Rate

**Bước 3: Xem API nào chậm**
- Login: 100ms, Transfer: 5s, History: 90ms → Transfer có vấn đề

**Bước 4: Trace Request**
- Gateway: 20ms → Transfer: 50ms → Account: 30ms → Database: 4900ms → Database đang chậm

**Bước 5: Xem Slow Query**, Execution Plan, Lock, Deadlock

**Bước 6: Nếu cần** → Rollback Release

> 🎯 **Interviewer muốn nghe: Có phương pháp điều tra, không phải Restart.**

---

### Câu 112: CPU của Server lên 100%. Anh làm gì?

Xác định nguyên nhân:
- Traffic tăng đột biến?
- Infinite Loop?
- Full GC?
- SQL quá chậm?
- Deadlock?
- Thread Pool đầy?

Kiểm tra: `top`, `jstack`, JFR, GC Log, Application Metrics.

Không nên restart ngay nếu chưa hiểu nguyên nhân → Có thể làm mất thông tin phục vụ điều tra.

---

### Câu 113: Database CPU chỉ 20%. Nhưng API mất 8 giây. Tại sao?

Có nhiều khả năng:
- **Connection Pool Full**: API đang chờ Connection, không phải chờ SQL
- **Network**: Service timeout
- **Lock**: Thread đang waiting
- **MQ**: Blocking
- **External API**: Payment Gateway timeout

> Không phải CPU thấp là Database không có vấn đề.

---

### Câu 114: Làm sao biết Memory Leak?

**Triệu chứng:**
- RAM tăng liên tục, không giảm
- GC chạy nhiều
- OutOfMemory

**Công cụ:** Heap Dump, MAT, VisualVM, JFR.

**Ví dụ:** Developer lưu `static List`, mỗi request `add()` nhưng không `remove()` → RAM sẽ đầy.

---

### Câu 115: Deadlock là gì?

```
Thread A: Lock Account A → Chờ Account B
Thread B: Lock Account B → Chờ Account A
```

Hai thread đều chờ nhau, không chạy tiếp.

**Tránh:**
- Luôn khóa theo cùng một thứ tự (ví dụ AccountId tăng dần)
- Giữ transaction ngắn
- Hạn chế giữ lock quá lâu

---

### Câu 116: Release Production xong, CPU tăng. Anh làm gì?

1. So sánh trước và sau release
2. Kiểm tra log lỗi, metric
3. Xác định service nào tăng CPU
4. Nếu xác định nguyên nhân từ bản phát hành mới và ảnh hưởng lớn → **Rollback**
5. Sau khi ổn định → Root Cause Analysis

---

### Câu 117: Log quá nhiều, Disk đầy. Làm sao?

Không nên `log.info` mọi nơi.

**Giải pháp:**
- Phân cấp log: INFO, WARN, ERROR, DEBUG
- Log Rotation
- Log Retention
- Central Logging (ELK, Grafana Loki, Splunk)

---

### Câu 118: Monitoring những gì?

| Lớp | Chỉ số |
|-----|--------|
| Application | TPS, Latency, Error |
| JVM | Heap, GC, Thread |
| Database | Connection, Deadlock, Slow Query |
| MQ | Consumer Lag, Retry, DLQ |
| Redis | Hit Rate, Memory, Eviction |
| OS | CPU, RAM, Disk, Network |

---

### Câu 119: API timeout. Anh điều tra như thế nào?

Kiểm tra lần lượt:
```
Client → Gateway → Service A → Service B → Database → External API
```

Sử dụng **Distributed Tracing** để xác định chính xác thành phần gây chậm. Không đoán. Đo.

---

### Câu 120: Có một bug lâu lâu mới xuất hiện. Làm sao Debug?

1. Không Debug bằng suy đoán
2. Tăng Log
3. Thêm Metric
4. Thêm TraceID
5. Record Request nếu cần
6. Reproduce ở môi trường Staging
7. Nếu vẫn không được → Heap Dump, Thread Dump, JFR

---

### ⭐ Câu SHB rất thích: "Khách hàng báo: 'Tôi vừa chuyển 100 triệu. Tiền đã bị trừ. Nhưng người nhận chưa nhận được.' Anh xử lý thế nào?"

**Giai đoạn 1: Xác minh**
- TransactionID, Log, TraceID, Audit

**Giai đoạn 2: Kiểm tra**
- Database, Message Queue, Consumer

**Giai đoạn 3: Xử lý**
- Nếu giao dịch chưa hoàn tất → xử lý theo đúng quy trình nghiệp vụ (tiếp tục xử lý, bù trừ hoặc hoàn tiền tùy trạng thái)

**Sau đó: Làm RCA để tránh lặp lại.**

> 🎯 **Đến chương này, không ai còn quan tâm bạn thuộc lòng bao nhiêu annotation của Spring. Họ chỉ muốn biết: "Nếu giao Production cho anh, lúc 2 giờ sáng có sự cố, anh có biết điều tra không?"**

---

## Kết luận

> **"Một Tech Lead mạnh không phải là người trả lời đúng nhiều câu hỏi nhất, mà là người thể hiện được tư duy: đúng ngữ cảnh, có trade-off, chịu trách nhiệm, và luôn đặt tính đúng đắn của dữ liệu lên hàng đầu."**

---

*Biên soạn từ tài liệu phỏng vấn thực tế vị trí Tech Lead Backend tại SHB và các ngân hàng lớn.*
