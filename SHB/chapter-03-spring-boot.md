# Chương 3 - Spring Boot & Spring Framework

> Câu 22 – 31 | IoC, Bean, Lifecycle, Auto Configuration và các "bẫy" phổ biến

---

## Câu 22: Spring Boot và Spring Framework khác nhau như thế nào?

### ✅ Câu trả lời

**Spring Framework** là framework gốc, cung cấp: IoC, DI, AOP, Transaction Management, MVC, Security...

**Spring Boot** được xây dựng trên Spring Framework với mục tiêu giảm cấu hình:
- Auto Configuration
- Starter Dependencies
- Embedded Tomcat
- Actuator
- Convention over Configuration

> Spring Boot không thay thế mà được xây dựng **trên** Spring Framework.

---

## Câu 23: IoC là gì? Tại sao Spring lại dùng IoC?

### ✅ Câu trả lời

**IoC (Inversion of Control)** là nguyên lý đảo ngược quyền điều khiển việc tạo và quản lý object.

```java
// Không có IoC
UserService service = new UserService();

// Có IoC (Spring)
@Autowired
private UserService service;
```

**Lợi ích:**
- Giảm phụ thuộc giữa các class
- Dễ mở rộng
- Dễ unit test
- Dễ thay thế implementation

### 🔄 Nếu không có IoC thì có vấn đề gì?

```
OrderService → new PaymentService() → new MailService() → new SmsService()
```

Các class phụ thuộc chặt vào nhau. Khi cần thay đổi hoặc mock để test sẽ rất khó.

---

## Câu 24: Bean là gì?

### ✅ Câu trả lời

Bean là object được Spring quản lý trong IoC Container.

Spring chịu trách nhiệm: tạo Bean → inject dependency → quản lý vòng đời → hủy Bean.

Được tạo từ:
- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@Configuration`

> ⚠️ Không phải object nào cũng là Bean. `new User()` là object Java bình thường, không phải Bean.

---

## Câu 25: Spring Bean có những Scope nào?

### ✅ Câu trả lời

| Scope | Mô tả |
|-------|--------|
| **Singleton** | Chỉ một instance (mặc định) |
| **Prototype** | Mỗi lần lấy Bean tạo object mới |
| **Request** | Một Bean cho mỗi HTTP Request |
| **Session** | Một Bean cho mỗi Session |
| **Application** | Một Bean cho toàn bộ ServletContext |

### 🔄 Trong backend ngân hàng thì thường dùng scope nào?

Gần như toàn bộ Service đều dùng **Singleton** vì ít tốn bộ nhớ, dễ quản lý, hiệu năng tốt.

Nếu Bean có state thì phải thiết kế thread-safe hoặc tránh dùng Singleton.

---

## Câu 26: Bean Lifecycle diễn ra như thế nào?

### ✅ Câu trả lời

```
Instantiate → Inject Dependency → @PostConstruct → Ready → Destroy → @PreDestroy
```

- `@PostConstruct`: khởi tạo tài nguyên sau khi inject xong
- `@PreDestroy`: giải phóng tài nguyên trước khi Bean bị hủy

---

## Câu 27: @Autowired hoạt động như thế nào?

### ✅ Câu trả lời

Khi Spring khởi động:
1. Quét package (Component Scan)
2. Tạo Bean
3. Tìm dependency phù hợp
4. Inject vào Bean

Spring ưu tiên inject theo **type**. Nếu có nhiều Bean cùng type → dùng `@Qualifier` hoặc `@Primary`.

### 🔄 Nếu có 2 Bean cùng interface thì sao?

```java
PaymentService
├── VisaPayment
└── MasterPayment
```

Spring báo lỗi: `NoUniqueBeanDefinitionException`

Giải quyết:
```java
@Qualifier("visaPayment")
// hoặc đánh dấu @Primary
```

---

## Câu 28: Constructor Injection tốt hơn Field Injection ở điểm nào?

### ✅ Câu trả lời

| Constructor Injection | Field Injection |
|----------------------|-----------------|
| Dependency bắt buộc khi khởi tạo | Có thể null |
| Field có thể `final` | Không |
| Dễ Unit Test (không cần Spring Context) | Khó test |
| Phụ thuộc thể hiện rõ ràng | Che giấu dependency |
| Tránh NullPointerException | Có thể NPE |

> Với project mới hoặc khi làm Tech Lead, nên thống nhất dùng Constructor Injection.

---

## Câu 29: Spring Boot Auto Configuration hoạt động như thế nào?

### ✅ Câu trả lời

1. `@SpringBootApplication` kích hoạt Auto Configuration
2. Spring đọc cấu hình trong `spring.factories`
3. Kiểm tra thư viện trong classpath
4. Nếu điều kiện phù hợp (`@ConditionalOnClass`, `@ConditionalOnMissingBean`...) → tự tạo Bean

**Ví dụ:** Nếu project có `spring-boot-starter-data-jpa`, Spring tự tạo: DataSource, EntityManager, TransactionManager.

### 🔄 Nếu không muốn Auto Configuration thì sao?

```java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
```

Hoặc tự định nghĩa Bean để ghi đè Bean mặc định.

---

## Câu 30: @Component, @Service, @Repository và @Controller khác nhau thế nào?

### ✅ Câu trả lời

| Annotation | Vai trò |
|------------|---------|
| `@Component` | Annotation chung cho mọi Bean |
| `@Service` | Tầng nghiệp vụ (Business Logic) |
| `@Repository` | Tầng truy cập dữ liệu + chuyển đổi exception |
| `@Controller` | Xử lý HTTP Request |

Sử dụng đúng annotation giúp code rõ ràng và tận dụng các tính năng riêng của Spring.

---

## Câu 31: Một Bean Singleton có biến instance để lưu trạng thái. Có vấn đề gì không?

> 🏆 **Mức Tech Lead**

### ✅ Câu trả lời

**Có, đây là rủi ro lớn.**

Bean Singleton chỉ có một instance dùng chung cho tất cả request. Nếu Bean chứa state có thể thay đổi → Race Condition, dữ liệu sai.

```java
@Service
public class PaymentService {
    private String currentUser; // ❌ Không an toàn!

    public void process(String user) {
        currentUser = user;
    }
}
```

**Cách thiết kế đúng:**
- Service nên **stateless** (không lưu trạng thái theo request)
- Dữ liệu cần xử lý được truyền qua **tham số của method**
- Nếu bắt buộc có state theo request → dùng request scope

---

## 💡 Góc nhìn của Interviewer

> **Singleton không có nghĩa là thread-safe.**
> Thread-safe phụ thuộc vào việc Bean có state hay không.
> Quy tắc quan trọng: **Service nên stateless.**
