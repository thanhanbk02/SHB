# 📘 SÁCH PHỎNG VẤN — BACKEND ARCHITECT / JAVA SPRING BOOT

> **Vị trí:** Backend Developer / Software Architect (Java, Spring Boot, Microservices, AWS)  
> **Lĩnh vực:** EV / Automotive / Connected Vehicle / IoT Platforms  
> **Ngôn ngữ:** Tiếng Việt  

---

## MỤC LỤC

1. [Java & Spring Boot](#1-java--spring-boot)
2. [REST API Design](#2-rest-api-design)
3. [Microservices & Domain-Driven Design (DDD)](#3-microservices--domain-driven-design-ddd)
4. [Event-Driven Architecture & Messaging](#4-event-driven-architecture--messaging)
5. [Bảo mật — OAuth2, OIDC & IAM](#5-bảo-mật--oauth2-oidc--iam)
6. [Cơ sở dữ liệu & SQL nâng cao](#6-cơ-sở-dữ-liệu--sql-nâng-cao)
7. [AWS & Cloud Infrastructure](#7-aws--cloud-infrastructure)
8. [Thiết kế kiến trúc & Diagrams](#8-thiết-kế-kiến-trúc--diagrams)
9. [Tích hợp hệ thống doanh nghiệp](#9-tích-hợp-hệ-thống-doanh-nghiệp)
10. [DevOps, CI/CD & Containerization](#10-devops-cicd--containerization)
11. [IoT, Connected Vehicle & EV Platform](#11-iot-connected-vehicle--ev-platform)
12. [Kỹ năng mềm & Tư duy giải quyết vấn đề](#12-kỹ-năng-mềm--tư-duy-giải-quyết-vấn-đề)
13. [Câu hỏi hành vi & Leadership](#13-câu-hỏi-hành-vi--leadership)

---

## 1. Java & Spring Boot

### Câu hỏi 1.1: Sự khác biệt giữa `@Component`, `@Service`, `@Repository` và `@Controller` trong Spring là gì? Khi nào nên dùng cái nào?

**Trả lờI:**
- **`@Component`**: Annotation cơ bản nhất, đánh dấu một class là Spring Bean. Dùng cho các class utility, helper không thuộc tầng nào cụ thể.
- **`@Service`**: Chuyên dùng cho tầng Business Logic. Khác biệt về mặt semantic — giúp code dễ đọc và dễ bảo trì. Spring không xử lý đặc biệt gì thêm.
- **`@Repository`**: Dùng cho tầng Data Access. Spring tự động translate các exception JDBC thành `DataAccessException` đồng nhất, giúp decouple tầng service khỏi database cụ thể.
- **`@Controller` / `@RestController`**: Dùng cho tầng Presentation/Web. `@RestController` = `@Controller` + `@ResponseBody`, tự động serialize response thành JSON/XML.

**Khi nào dùng:** Tuân thủ layered architecture. `@Service` cho business logic, `@Repository` cho DAO/Repository pattern, `@Controller` cho REST endpoints.

---

### Câu hỏi 1.2: Giải thích Spring Bean Lifecycle và cách bạn quản lý vòng đờI của Bean trong microservices.

**Trả lờI:**
Spring Bean Lifecycle gồm các giai đoạn:
1. **Instantiation**: Spring tạo instance bằng constructor.
2. **Population of properties**: Inject dependencies.
3. **BeanNameAware.setBeanName()**, **BeanFactoryAware.setBeanFactory()**, **ApplicationContextAware.setApplicationContext()**: Set context nếu implement.
4. **@PostConstruct / InitializingBean.afterPropertiesSet()**: Khởi tạo resource (DB connection, cache warm-up).
5. **Bean ready for use**.
6. **@PreDestroy / DisposableBean.destroy()**: Dọn dẹp resource trước khi destroy.

**Trong microservices:**
- Dùng `@Scope("prototype")` cho stateful beans (ít dùng).
- `@Scope("singleton")` mặc định — phù hợp với stateless service.
- Trong cloud-native app, cần graceful shutdown: implement `DisposableBean` để đóng Kafka producer/consumer, connection pool trước khi pod terminate.
- Dùng Spring Boot Actuator `/actuator/health` và graceful shutdown config (`server.shutdown=graceful`) để đảm bảo request đang xử lý không bị drop.

---

### Câu hỏi 1.3: Spring Security filter chain hoạt động như thế nào? Làm sao để custom filter trong microservices?

**Trả lờI:**
Spring Security hoạt động qua **Filter Chain** (delegating filter proxy):
1. `SecurityContextPersistenceFilter`: Load SecurityContext từ session/security context repository.
2. `UsernamePasswordAuthenticationFilter`: Xử lý form login.
3. `BasicAuthenticationFilter`: Xử lý HTTP Basic.
4. `BearerTokenAuthenticationFilter` (OAuth2): Validate JWT token.
5. `ExceptionTranslationFilter`: Bắt `AccessDeniedException`, `AuthenticationException`.
6. `FilterSecurityInterceptor`: Authorization cuối cùng.

**Custom filter trong microservices:**
```java
http.addFilterBefore(new CustomJwtFilter(), BearerTokenAuthenticationFilter.class);
```
- Custom filter để extract claims từ JWT, validate token với Authorization Server, hoặc thêm custom header (tenant-id, correlation-id).
- Trong microservices, thường dùng **Resource Server** pattern: validate JWT tại API Gateway và propagate thông tin qua headers.

---

### Câu hỏi 1.4: Spring Cloud cung cấp những gì cho microservices? Kể tên các module quan trọng.

**Trả lờI:**
Spring Cloud cung cấp giải pháp cho các vấn đề phân tán:
- **Spring Cloud Gateway**: API Gateway — routing, rate limiting, circuit breaker.
- **Spring Cloud Netflix Eureka**: Service Discovery (đã deprecated, thay bằng Kubernetes DNS hoặc Consul).
- **Spring Cloud Config**: Centralized configuration management.
- **Spring Cloud Circuit Breaker**: Abstract Resilience4j, Hystrix (deprecated) — fault tolerance.
- **Spring Cloud Sleuth + Zipkin**: Distributed tracing.
- **Spring Cloud OpenFeign**: Declarative HTTP client giữa các service.
- **Spring Cloud Stream**: Abstraction cho messaging (Kafka, RabbitMQ).
- **Spring Cloud LoadBalancer**: Client-side load balancing (thay thế Ribbon).

**Thực tế hiện nay:** Nhiều team chuyển sang Kubernetes-native (Ingress, Service Mesh Istio/Linkerd) thay vì Spring Cloud Netflix stack.

---

### Câu hỏi 1.5: Làm sao xử lý transaction trong microservices khi có nhiều service và database riêng biệt?

**Trả lờI:**
Trong microservices, **distributed transaction** là anti-pattern vì vi phạm bounded context. Các giải pháp:

1. **Saga Pattern** (thay thế 2PC):
   - **Choreography**: Các service publish/subscribe event. Service A hoàn thành → publish event → Service B nhận và xử lý.
   - **Orchestration**: Một orchestrator service điều phối toàn bộ flow, gọi từng service theo sequence.

2. **Outbox Pattern**: Thay vì publish trực tiếp, ghi event vào bảng `outbox` trong cùng DB transaction. Một background worker đọc và publish lên Kafka.

3. **Event Sourcing**: Lưu trạng thái dưới dạng chuỗi event. Replay event để reconstruct state.

4. **Compensating Transaction**: Khi một step fail, trigger rollback bằng compensating action (ví dụ: refund khi thanh toán thành công nhưng đặt hàng fail).

---

## 2. REST API Design

### Câu hỏi 2.1: Thiết kế REST API cho hệ thống quản lý phương tiện EV. Làm sao để versioning?

**Trả lờI:**
**Resource Design:**
```
GET    /api/v1/vehicles              → List vehicles (pagination, filter)
GET    /api/v1/vehicles/{id}         → Get vehicle detail
POST   /api/v1/vehicles              → Create vehicle
PUT    /api/v1/vehicles/{id}         → Full update
PATCH  /api/v1/vehicles/{id}         → Partial update (e.g., update battery status)
DELETE /api/v1/vehicles/{id}         → Delete vehicle
GET    /api/v1/vehicles/{id}/telemetry → Get latest telemetry data
POST   /api/v1/vehicles/{id}/commands/lock → Send remote lock command
```

**Versioning Strategies:**
1. **URL Path** (`/api/v1/`, `/api/v2/`): Dễ implement, dễ cache, explicit. Khuyến nghị cho public API.
2. **Header** (`Accept: application/vnd.company.v1+json`): Clean URL, nhưng khó test và cache.
3. **Query Parameter** (`?version=1`): Đơn giản nhưng không RESTful lắm.

**Best practice:** Dùng URL path versioning. Khi có breaking change (response structure thay đổi, field bị remove), tạo version mới. Giữ old version deprecated trong ít nhất 6-12 tháng.

---

### Câu hỏi 2.2: Làm sao để thiết kế API idempotent? Cho ví dụ trong ngữ cảnh payment gateway.

**Trả lờI:**
**Idempotency** nghĩa là gọi API nhiều lần với cùng input cho kết quả giống nhau, không gây side effect thêm.

**Cách implement:**
1. **Idempotency Key**: Client gửi header `Idempotency-Key: <uuid>`. Server lưu key và kết quả vào cache/DB (với TTL, ví dụ 24h). Nếu nhận lại cùng key, trả về kết quả đã lưu.
2. **Natural Idempotency**:
   - `PUT /orders/123` với toàn bộ resource → idempotent.
   - `DELETE /orders/123` → idempotent (xóa lần 2 vẫn trả 404).
3. **State Machine**: Payment có các trạng thái `PENDING` → `PROCESSING` → `COMPLETED`/`FAILED`. Nếu nhận request thanh toán khi đã `COMPLETED`, trả về kết quả cũ.

**Ví dụ Payment:**
```
POST /api/v1/payments
Idempotency-Key: abc-123-xyz

{
  "order_id": "ORD-001",
  "amount": 1000000,
  "method": "credit_card"
}
```
Server lưu `abc-123-xyz` → `payment_id: PAY-001, status: SUCCESS`. Retry sẽ trả về `PAY-001` thay vì tạo payment mới.

---

### Câu hỏi 2.3: Làm sao document API hiệu quả? So sánh OpenAPI (Swagger) với AsyncAPI.

**Trả lờI:**
**OpenAPI (Swagger):**
- Dùng cho REST API (HTTP request/response).
- Mô tả endpoints, methods, parameters, request/response schema, authentication.
- Tools: Swagger UI, Swagger Codegen, SpringDoc OpenAPI.

**AsyncAPI:**
- Dùng cho event-driven/messaging API (Kafka, MQTT, WebSocket).
- Mô tả channels, publishers, subscribers, message schema, bindings.
- Tương đương OpenAPI nhưng cho async protocols.

**Best practice:**
- Dùng **OpenAPI 3.0+** cho REST API, tích hợp SpringDoc để auto-generate từ code.
- Dùng **AsyncAPI** cho Kafka topics, MQTT channels trong IoT platform.
- Lưu spec vào Git, review như code. Dùng contract testing (Pact, Spring Cloud Contract) để đảm bảo producer/consumer tuân thủ spec.

---

## 3. Microservices & Domain-Driven Design (DDD)

### Câu hỏi 3.1: Giải thích các khái niệm core trong DDD: Entity, Value Object, Aggregate, Repository, Domain Service.

**Trả lờI:**
- **Entity**: Đối tượng có identity duy nhất (ví dụ: `Vehicle` với `vehicleId`). Identity không đổi dù thuộc tính thay đổi.
- **Value Object (VO)**: Không có identity, immutable, dùng để mô tả đặc điểm (ví dụ: `Money`, `Coordinate`, `VIN`). So sánh bằng giá trị.
- **Aggregate**: Cluster của Entity và VO, có Aggregate Root là entry point duy nhất. Ví dụ: `Order` là root, chứa `OrderItem` (không truy cập `OrderItem` trực tiếp từ bên ngoài).
- **Repository**: Abstract tầng persistence. Domain layer định nghĩa interface, infrastructure layer implement (JPA, MongoDB).
- **Domain Service**: Chứa business logic không thuộc về một Entity/VO cụ thể. Ví dụ: `PricingService` tính giá dựa trên nhiều rule.
- **Application Service**: Điều phối use case, không chứa business logic.
- **Bounded Context**: Ranh giới domain, mỗi context có ubiquitous language riêng. Ví dụ: `VehicleManagement` và `TelemetryProcessing` là 2 bounded context khác nhau.

---

### Câu hỏi 3.2: Làm sao xác định bounded context cho hệ thống xe điện (EV platform)?

**Trả lờI:**
Dựa trên business capability:
1. **Vehicle Management Context**: Đăng ký xe, thông tin cấu hình, firmware version.
2. **Telemetry Context**: Nhận dữ liệu sensor (battery, GPS, speed), xử lý real-time.
3. **Charging Context**: Tìm trạm sạc, đặt lịch sạc, thanh toán điện.
4. **User/Customer Context**: Profile, preference, subscription.
5. **Trip/Navigation Context**: Lập lịch trình, tính toán range.
6. **Service/Maintenance Context**: Bảo dưỡng định kỳ, diagnostic, OTA update.
7. **Payment/Billing Context**: Thanh toán, hóa đơn, subscription.

**Cách xác định:**
- Dùng **Event Storming** workshop với business stakeholders.
- Tìm **aggregate boundaries** — nơi business invariant cần bảo toàn trong transaction.
- Mỗi bounded context có database riêng, team riêng (Conway's Law).
- Context mapping: xác định relationship (Partnership, Shared Kernel, Customer-Supplier, Anti-Corruption Layer).

---

### Câu hỏi 3.3: Khi nào nên refactor, merge hoặc retire một microservice?

**Trả lờI:**
**Refactor khi:**
- Code complexity cao (cyclomatic complexity > 15), technical debt nhiều.
- Performance bottleneck nhưng domain boundary vẫn hợp lý.
- Cần nâng cấp tech stack mà không thay đổi contract.

**Merge khi:**
- Tight coupling cao — 2 service gọi nhau synchronous liên tục, chia sẻ database.
- Chung bounded context — 2 service thực chất cùng quản lý một domain.
- Deployment overhead không đáng — operational cost cao hơn giá trị tách biệt.
- Ví dụ: `NotificationService` và `EmailService` có thể merge nếu cùng xử lý outbound communication.

**Retire khi:**
- Business capability không còn cần thiết.
- Chức năng được thay thế bởi SaaS/third-party (ví dụ: chuyển từ custom auth sang Auth0).
- Strangler Fig pattern: dần dần redirect traffic sang service mới, sau đó tắt service cũ.

---

## 4. Event-Driven Architecture & Messaging

### Câu hỏi 4.1: So sánh Kafka và MQTT. Khi nào dùng cái nào trong hệ thống xe điện?

**Trả lờI:**
| Tiêu chí | Apache Kafka | MQTT |
|----------|-------------|------|
| **Protocol** | TCP binary protocol | Lightweight pub/sub (over TCP) |
| **Use case** | High-throughput event streaming, log aggregation | IoT device communication, low bandwidth |
| **QoS** | At-least-once, exactly-once | QoS 0/1/2 |
| **Persistence** | Durable, replicated, configurable retention | Broker-dependent (e.g., HiveMQ, EMQX) |
| **Scalability** | Horizontal scale (partitions, consumers) | Vertical + clustering |
| **Overhead** | Higher | Very low (2 byte header min) |

**Trong EV platform:**
- **MQTT**: Dùng cho communication giữa vehicle và cloud (telematics). Xe gửi battery status, GPS qua MQTT (qua AWS IoT Core). Low latency, handle millions of connections.
- **Kafka**: Dùng cho backend event processing. MQTT message từ IoT Core được forward sang Kafka để: stream processing (Flink/Spark), analytics, trigger business events.
- **Architecture:** `Vehicle → MQTT (IoT Core) → Kafka → Microservices/Analytics`

---

### Câu hỏi 4.2: Làm sao đảm bảo message ordering trong Kafka?

**Trả lờI:**
Kafka đảm bảo ordering **trong cùng một partition**.

**Cách implement:**
1. **Partition Key**: Dùng entity ID làm key. Ví dụ: `vehicle_id` làm key → tất cả event của cùng xe vào cùng partition, đảm bảo order.
2. **Single Partition**: Nếu cần global order (ít hiệu năng), dùng 1 partition — chỉ phù hợp low throughput.
3. **Consumer Group**: Một consumer group có nhiều instance, mỗi instance đọc partition khác nhau. Đảm bảo mỗi partition chỉ được đọc bởi 1 consumer tại 1 thờI điểm.

**Lưu ý:**
- Nếu cần process song song nhưng giữ order, dùng **parallel consumer** pattern: đọc theo partition key, buffer và process theo sequence number.
- Tránh rebalance bằng cách config `session.timeout.ms`, `max.poll.interval.ms` hợp lý.

---

### Câu hỏi 4.3: Giải thích Dead Letter Queue (DLQ) pattern và cách implement trong Kafka.

**Trả lờI:**
**DLQ** dùng để xử lý message lỗi mà không block main queue.

**Flow:**
1. Consumer đọc message từ topic chính.
2. Nếu xử lý thất bại (deserialization error, business validation fail, DB unavailable):
   - Retry N lần với exponential backoff.
   - Nếu vẫn fail, publish message sang **DLQ topic** (ví dụ: `orders.dlq`).
3. DLQ consumer hoặc admin tool phân tích message lỗi, quyết định: replay, discard, hoặc manual fix.

**Implement trong Spring Kafka:**
```java
@KafkaListener(topics = "orders", errorHandler = "kafkaErrorHandler")
public void handleOrder(OrderEvent order) { ... }

// Config DeadLetterPublishingRecoverer
DefaultErrorHandler errorHandler = new DefaultErrorHandler(
    new DeadLetterPublishingRecoverer(kafkaTemplate),
    new FixedBackOff(3000L, 3L) // 3 retries, 3s interval
);
```

**Best practice:**
- DLQ nên có retention dài hơn topic chính.
- Ghi log chi tiết: error type, stack trace, timestamp, retry count.
- Alert khi DLQ có message.

---

## 5. Bảo mật — OAuth2, OIDC & IAM

### Câu hỏi 5.1: Giải thích luồng Authorization Code Flow trong OAuth2. Tại sao nên dùng nó thay vì Implicit Flow?

**Trả lờI:**
**Authorization Code Flow:**
1. User click login → redirect đến Authorization Server (AS) với `response_type=code`, `client_id`, `redirect_uri`, `scope`, `state`.
2. User authenticate với AS.
3. AS redirect về app với **authorization code** (temporary, short-lived).
4. App gọi AS token endpoint (`POST /token`) với code + `client_secret` để đổi lấy **access token** + **refresh token**.
5. App dùng access token để gọi Resource Server.

**Tại sao không dùng Implicit Flow:**
- Implicit trả access token trực tiếp qua URL fragment (`#access_token=...`), dễ bị leak qua browser history, referrer logs.
- Không có refresh token → user phải login lại thường xuyên.
- Không xác thực client (không dùng client_secret) → dễ bị token injection.
- **PKCE (Proof Key for Code Exchange)** mở rộng Authorization Code Flow cho mobile/SPA — thay thế hoàn toàn Implicit.

---

### Câu hỏi 5.2: Phân biệt Authentication và Authorization. Làm sao implement RBAC trong Spring Security?

**Trả lờI:**
- **Authentication**: Xác minh "bạn là ai" (verify identity — username/password, JWT, MFA).
- **Authorization**: Xác định "bạn được phép làm gì" (permissions, roles, policies).

**RBAC trong Spring Security:**
```java
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests(auth -> auth
            .requestMatchers("/api/admin/**").hasRole("ADMIN")
            .requestMatchers("/api/vehicles/**").hasAnyRole("ADMIN", "OPERATOR")
            .requestMatchers("/api/public/**").permitAll()
            .anyRequest().authenticated()
        )
        .oauth2ResourceServer(oauth2 -> oauth2.jwt(jwt -> 
            jwt.jwtAuthenticationConverter(jwtAuthConverter())
        ));
        return http.build();
    }
}
```
- JWT chứa `roles` hoặc `scope` claim.
- `JwtAuthenticationConverter` map claim thành `GrantedAuthority`.
- Có thể dùng **ABAC** (Attribute-Based) nâng cao: kiểm tra ownership (user chỉ xem xe của mình) bằng custom `PermissionEvaluator`.

---

### Câu hỏi 5.3: Làm sao bảo mật API giữa các internal microservices?

**Trả lờI:**
1. **mTLS (Mutual TLS)**: Service này xác thực service kia bằng certificate. Implement qua Service Mesh (Istio) hoặc Spring Boot config.
2. **Service Account Token**: Trong Kubernetes, mỗi pod có ServiceAccount token. Service B xác thực token này với Kubernetes API hoặc Identity Provider.
3. **Internal JWT**: API Gateway issue short-lived JWT cho internal communication. Mỗi service validate JWT với shared secret hoặc JWKS endpoint.
4. **Network Policies**: Kubernetes NetworkPolicy chỉ cho phép traffic từ namespace/service cụ thể.
5. **Zero Trust**: Không tin tưởng bất kỳ request nào, kể cả internal. Mọi call đều qua authentication + authorization.

---

## 6. Cơ sở dữ liệu & SQL nâng cao

### Câu hỏi 6.1: Giải thích các normal form (1NF, 2NF, 3NF, BCNF). Khi nào nên denormalize?

**Trả lờI:**
- **1NF**: Mỗi cell chứa atomic value (không có repeating groups). Ví dụ: không lưu `phones: "0912,0934"` mà tách thành bảng riêng.
- **2NF**: Thỏa 1NF + không có partial dependency (non-key attribute phụ thuộc vào một phần của composite key).
- **3NF**: Thỏa 2NF + không có transitive dependency (non-key attribute phụ thuộc vào non-key attribute khác).
- **BCNF**: Thỏa 3NF + mọi determinant đều là superkey (stricter than 3NF).

**Khi nào denormalize:**
- Read-heavy workloads, cần query performance (OLAP, reporting).
- Microservices — mỗi service có data store riêng, có thể duplicate data để tránh cross-service query.
- Caching layer (Redis) lưu denormalized view.
- Trade-off: tăng write complexity, risk inconsistency → cần event-driven sync hoặc CDC (Change Data Capture).

---

### Câu hỏi 6.2: Làm sao optimize query chậm trong PostgreSQL?

**Trả lờI:**
1. **EXPLAIN ANALYZE**: Xem execution plan — sequential scan, index scan, nested loop, hash join.
2. **Indexing**:
   - B-tree index cho equality và range queries.
   - GiST/GIN index cho JSONB, full-text search, geospatial.
   - Partial index cho subset dữ liệu (`WHERE status = 'ACTIVE'`).
   - Covering index (INCLUDE) để index-only scan.
3. **Query rewrite**:
   - Tránh `SELECT *`, chỉ select cần thiết.
   - Tránh function trên indexed column (`WHERE UPPER(name) = 'ABC'` → dùng functional index hoặc case-insensitive collation).
   - Dùng JOIN thay vì correlated subquery.
   - Pagination: `KEYSET` pagination thay vì `OFFSET` lớn.
4. **Schema optimization**:
   - Partitioning cho bảng lớn (range partition by date).
   - Archiving dữ liệu cũ.
5. **Connection pooling**: PgBouncer giảm overhead tạo connection.
6. **VACUUM & ANALYZE**: Update statistics, reclaim dead tuples.

---

### Câu hỏi 6.3: Phân biệt SQL (PostgreSQL/MySQL) và NoSQL (DynamoDB, MongoDB). Khi nào dùng cái nào trong microservices?

**Trả lờI:**
| | SQL (PostgreSQL) | NoSQL (DynamoDB) |
|---|---|---|
| **Schema** | Rigid, predefined | Flexible, schema-less |
| **Transaction** | ACID, complex joins | Eventually consistent, limited ACID |
| **Query** | Complex ad-hoc queries | Simple key-value, document queries |
| **Scale** | Vertical + read replicas | Horizontal auto-scale |
| **Use case** | Relational data, reporting, complex business logic | High throughput, simple access patterns, IoT telemetry |

**Trong microservices:**
- **PostgreSQL**: User profile, orders, payments — cần consistency, complex queries.
- **DynamoDB**: Vehicle telemetry (timeseries-like), session store, real-time leaderboard — high write throughput, predictable latency.
- **MongoDB**: Catalog, configuration data có schema thay đổi thường xuyên.
- **Polyglot persistence**: Mỗi service chọn DB phù hợp với access pattern của mình.

---

## 7. AWS & Cloud Infrastructure

### Câu hỏi 7.1: Thiết kế hệ thống microservices trên AWS cho nền tảng xe điện. Vẽ kiến trúc tổng quan.

**Trả lờI:**
**Kiến trúc tổng quan:**

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENTS                              │
│  Mobile App  │  Web Dashboard  │  Vehicle (IoT)            │
└──────────────┴─────────────────┴───────────────────────────┘
         │              │                    │
         ▼              ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│                    AWS CLOUDFRONT / CDN                     │
└─────────────────────────────────────────────────────────────┘
         │              │                    │
         ▼              ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│              AWS API GATEWAY (REST + WebSocket)             │
│         Auth: Cognito / Custom Lambda Authorizer            │
└─────────────────────────────────────────────────────────────┘
         │              │                    │
         ▼              ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│                  APPLICATION LAYER (EKS/ECS)                │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐       │
│  │ Vehicle  │ │ Telemetry│ │ Payment  │ │ User     │       │
│  │ Service  │ │ Service  │ │ Service  │ │ Service  │       │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘       │
│       │            │            │            │              │
│       └────────────┴────────────┴────────────┘              │
│                    Internal ALB / Service Mesh              │
└─────────────────────────────────────────────────────────────┘
         │              │                    │
         ▼              ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│                   DATA LAYER                                │
│  RDS (PostgreSQL) │ DynamoDB │ ElastiCache │ S3            │
└─────────────────────────────────────────────────────────────┘
         │              │                    │
         ▼              ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│              EVENT & STREAMING LAYER                        │
│       AWS IoT Core ←→ Kafka (MSK) ←→ Kinesis              │
│              │                    │                         │
│              ▼                    ▼                         │
│       Lambda (real-time)    S3 (data lake)                  │
│       Flink/Spark (analytics)                               │
└─────────────────────────────────────────────────────────────┘
```

**Giải thích các thành phần:**
- **IoT Core**: Nhận MQTT từ xe, rule engine forward message sang Kafka/Kinesis/Lambda.
- **EKS (Elastic Kubernetes Service)**: Orchestrate microservices. Dùng Fargate hoặc EC2 node group.
- **API Gateway**: Rate limiting, caching, request validation, throttling.
- **RDS Aurora PostgreSQL**: Primary database cho transactional data.
- **DynamoDB**: Telemetry hot data, session store.
- **S3**: Data lake cho historical telemetry, log archive.
- **MSK (Managed Kafka)**: Event streaming giữa các service.

---

### Câu hỏi 7.2: So sánh EKS, ECS và Lambda. Khi nào dùng cái nào?

**Trả lờI:**
| | AWS Lambda | Amazon ECS | Amazon EKS |
|---|---|---|---|
| **Model** | Serverless (FaaS) | Container orchestration (Docker) | Kubernetes managed |
| **Control** | Low — AWS quản lý hoàn toàn | Medium — task definition, service | High — full K8s API |
| **Cold start** | Có (vài trăm ms đến vài giây) | Không (container chạy liên tục) | Không |
| **Scale** | Auto, per-request | Auto Scaling (CPU/memory) | HPA, VPA, Cluster Autoscaler |
| **Use case** | Event processing, webhook, cron job, IoT rule | Simple microservices, stateless app | Complex microservices, service mesh, multi-cloud |

**Trong EV platform:**
- **Lambda**: Xử lý IoT Core rule (ví dụ: alert khi battery < 10%), image resize, cron job báo cáo.
- **ECS**: Dùng nếu team chưa cần K8s complexity, hoặc dùng Fargate để serverless container.
- **EKS**: Dùng cho core microservices — cần service mesh (Istio), advanced deployment strategy (canary, blue-green), multi-region.

---

### Câu hỏi 7.3: Làm sao thiết kế VPC cho microservices trên AWS?

**Trả lờI:**
**VPC Architecture (3-tier):**
```
┌─────────────────────────────────────────────────────────────┐
│                         VPC (10.0.0.0/16)                   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  PUBLIC SUBNETS (ALB, NAT Gateway, Bastion)         │   │
│  │  10.0.1.0/24 (AZ-1a) │ 10.0.2.0/24 (AZ-1b)        │   │
│  └─────────────────────────────────────────────────────┘   │
│                         │                                   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  PRIVATE SUBNETS (Application — EKS/ECS/Lambda)     │   │
│  │  10.0.3.0/24 (AZ-1a) │ 10.0.4.0/24 (AZ-1b)        │   │
│  └─────────────────────────────────────────────────────┘   │
│                         │                                   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  DATA SUBNETS (RDS, ElastiCache, MSK)               │   │
│  │  10.0.5.0/24 (AZ-1a) │ 10.0.6.0/24 (AZ-1b)        │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Best practice:**
- Multi-AZ cho high availability.
- Public subnet chỉ chứa ALB, NAT Gateway — không đặt app trực tiếp.
- Private subnet cho application — outbound qua NAT Gateway, inbound qua ALB.
- Data subnet — không có route đến internet, chỉ cho phép access từ private subnet qua security group.
- Security Group: least privilege — chỉ mở port cần thiết, reference SG thay vì IP.
- VPC Peering hoặc Transit Gateway nếu có nhiều VPC (prod, staging, shared services).

---

## 8. Thiết kế kiến trúc & Diagrams

### Câu hỏi 8.1: Làm sao document AS-IS architecture hiệu quả? Những loại diagram nào cần có?

**Trả lờI:**
**Các loại diagram cần thiết:**
1. **C4 Model — Level 1 (System Context Diagram)**: Hệ thống nằm trong bối cảnh tổ chức, tương tác với user và system bên ngoài.
2. **C4 Model — Level 2 (Container Diagram)**: Các container (app, database, message broker) và interaction giữa chúng.
3. **C4 Model — Level 3 (Component Diagram)**: Các component trong một container (controllers, services, repositories).
4. **Sequence Diagram**: Flow xử lý một use case cụ thể theo thờI gian.
5. **Data Flow Diagram (DFD)**: Luồng dữ liệu từ input đến output, qua các process và data store.
6. **Deployment Diagram**: Cấu hình infrastructure — server, network, cloud services.

**Tools:** PlantUML, Structurizr, Draw.io, Mermaid.
**Best practice:**
- Mỗi diagram có 1 mục đích rõ ràng, không gộp chung.
- Giữ consistency — cùng một component phải có tên giống nhau ở mọi diagram.
- Version control diagram như code (PlantUML/Mermaid trong Git).

---

### Câu hỏi 8.2: Vẽ Sequence Diagram cho use case: "User gửi lệnh khóa xe từ mobile app"

**Trả lờI:**

```
┌─────────┐     ┌─────────────┐     ┌─────────────┐     ┌──────────┐     ┌───────┐
│  User   │     │  Mobile App │     │ API Gateway │     │ Command  │     │ Vehicle│
│         │     │             │     │             │     │ Service  │     │ (IoT)  │
└────┬────┘     └──────┬──────┘     └──────┬──────┘     └────┬─────┘     └───┬───┘
     │                 │                   │                 │               │
     │ 1. Tap "Lock"   │                   │                 │               │
     │────────────────>│                   │                 │               │
     │                 │ 2. POST /vehicles/123/commands/lock │               │
     │                 │ (Bearer Token)    │                 │               │
     │                 │──────────────────>│                 │               │
     │                 │                   │ 3. Validate JWT │               │
     │                 │                   │ (Cognito/Auth)  │               │
     │                 │                   │───────┐         │               │
     │                 │                   │<──────┘         │               │
     │                 │                   │ 4. Route to     │               │
     │                 │                   │    Command Svc  │               │
     │                 │                   │────────────────>│               │
     │                 │                   │                 │ 5. Validate   │
     │                 │                   │                 │    ownership  │
     │                 │                   │                 │───────┐       │
     │                 │                   │                 │<──────┘       │
     │                 │                   │                 │ 6. Publish    │
     │                 │                   │                 │    "LockCmd"  │
     │                 │                   │                 │    to Kafka/  │
     │                 │                   │                 │    IoT Core   │
     │                 │                   │                 │──────────────>│
     │                 │                   │                 │               │
     │                 │                   │                 │ 7. MQTT msg   │
     │                 │                   │                 │    to Vehicle │
     │                 │                   │                 │<──────────────│
     │                 │                   │                 │ 8. Ack/Status │
     │                 │                   │                 │    update     │
     │                 │                   │                 │───────┐       │
     │                 │                   │                 │<──────┘       │
     │                 │                   │ 9. Response     │               │
     │                 │                   │<────────────────│               │
     │                 │ 10. Show "Locked" │                 │               │
     │                 │<──────────────────│                 │               │
     │                 │                   │                 │               │
```

**Giải thích:**
- Mobile app gọi API Gateway với JWT.
- API Gateway validate token, route đến Command Service.
- Command Service kiểm tra quyền sở hữu xe (RBAC + ownership).
- Publish command event lên Kafka hoặc trực tiếp qua IoT Core MQTT.
- Vehicle nhận lệnh, thực hiện, gửi status update.
- Command Service cập nhật trạng thái, trả response về mobile app.

---

### Câu hỏi 8.3: Làm sao đánh giá pain points và bottlenecks trong AS-IS architecture?

**Trả lờI:**
**Phương pháp đánh giá:**
1. **Performance Analysis**:
   - APM tools (Datadog, New Relic, AWS X-Ray) để tìm slow endpoints, high latency.
   - Database query analysis: slow query log, missing index.
   - Resource utilization: CPU, memory, disk I/O, network bandwidth.

2. **Dependency Analysis**:
   - Vẽ dependency graph giữa các service.
   - Tìm cyclic dependencies, tight coupling.
   - Đếm số lượng synchronous call giữa các service (càng nhiều càng dễ fail cascade).

3. **Code Quality Metrics**:
   - Cyclomatic complexity, code coverage, technical debt ratio (SonarQube).
   - Code churn — file nào thay đổi thường xuyên nhất (hotspot).

4. **Operational Metrics**:
   - MTTR (Mean Time To Recovery), MTBF (Mean Time Between Failures).
   - Error rate, circuit breaker trigger frequency.
   - Deployment frequency, lead time for changes (DORA metrics).

5. **Architecture Review**:
   - ATAM (Architecture Tradeoff Analysis Method): đánh giá quality attributes (scalability, availability, security).
   - SWOT analysis cho từng component.

---

## 9. Tích hợp hệ thống doanh nghiệp

### Câu hỏi 9.1: Làm sao tích hợp với SAP/ERP và CRM? Những challenge thường gặp?

**Trả lờI:**
**Integration Patterns:**
1. **API-based**: SAP expose OData/SOAP API, microservices gọi qua REST/SOAP client. Dùng khi real-time cần thiết.
2. **Event-driven**: SAP publish event (ví dụ: order created) lên message bus (Kafka), microservices subscribe. Dùng SAP Event Mesh hoặc custom middleware.
3. **File-based**: SAP export CSV/IDoc/XML định kỳ, microservices đọc từ SFTP/S3. Dùng cho batch processing.
4. **Database Replication**: Đọc trực tiếp từ SAP DB (không khuyến khích — tight coupling).
5. **Integration Platform**: Dùng middleware như MuleSoft, Boomi, Apache Camel để transform và route message.

**Challenges:**
- **Protocol mismatch**: SAP dùng SOAP/OData, microservices dùng REST/JSON → cần transformation layer.
- **Data model difference**: SAP có schema phức tạp, cần anti-corruption layer (DDD pattern) để map sang domain model của mình.
- **Transaction boundary**: SAP transaction khác microservices transaction → dùng Saga pattern.
- **Availability**: SAP thường có maintenance window, cần circuit breaker và fallback.
- **Security**: SAP có auth mechanism riêng (SAP SSO, certificates) → cần federated identity.

---

### Câu hỏi 9.2: Thiết kế integration với Payment Gateway. Làm sao đảm bảo an toàn và idempotency?

**Trả lờI:**
**Architecture:**
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌──────────────┐
│   Client    │────>│   Payment   │────>│  Payment    │────>│   Gateway    │
│             │     │   Service   │     │  Adapter    │     │ (Stripe/VNPay│
└─────────────┘     └─────────────┘     └─────────────┘     └──────────────┘
                            │                   │
                            ▼                   ▼
                     ┌─────────────┐     ┌─────────────┐
                     │   Kafka     │     │  Webhook    │
                     │  (events)   │     │  Handler    │
                     └─────────────┘     └─────────────┘
```

**Bảo mật:**
- PCI DSS compliance — không lưu CVV, tokenize card number.
- HTTPS/TLS 1.3 cho mọi communication.
- HMAC signature để verify webhook từ gateway.
- IP whitelist gateway endpoints.

**Idempotency:**
- Dùng `Idempotency-Key` header cho mọi request.
- Lưu trạng thái payment trong DB với state machine: `INITIATED` → `PROCESSING` → `COMPLETED`/`FAILED`.
- Nếu gateway timeout, query status trước khi retry (idempotent query API).
- Webhook handler phải idempotent: kiểm tra `event_id` đã xử lý chưa.

---

## 10. DevOps, CI/CD & Containerization

### Câu hỏi 10.1: Mô tả CI/CD pipeline lý tưởng cho microservices trên AWS.

**Trả lờI:**
**Pipeline:**
```
Developer push code → GitHub/GitLab
         │
         ▼
┌─────────────────┐
│  1. Build Stage │  → Compile, unit test, SonarQube scan
│     (Maven/Gradle)│
└─────────────────┘
         │
         ▼
┌─────────────────┐
│  2. Security    │  → Snyk/Trivy scan dependencies, SAST
│     Scan        │
└─────────────────┘
         │
         ▼
┌─────────────────┐
│  3. Build Image │  → Docker build, tag with commit SHA
│     (Docker)    │
└─────────────────┘
         │
         ▼
┌─────────────────┐
│  4. Push to ECR │  → Amazon Elastic Container Registry
└─────────────────┘
         │
         ▼
┌─────────────────┐
│  5. Deploy to   │  → Helm/Kustomize deploy to EKS staging
│     Staging     │    Integration test, contract test
└─────────────────┘
         │
         ▼
┌─────────────────┐
│  6. Approval    │  → Manual/Auto approval gate
└─────────────────┘
         │
         ▼
┌─────────────────┐
│  7. Deploy to   │  → Canary/Blue-green to EKS production
│     Production  │    Smoke test, rollback nếu error rate > threshold
└─────────────────┘
```

**Tools:** GitHub Actions/GitLab CI, ArgoCD (GitOps), Helm, Prometheus/Grafana.

---

### Câu hỏi 10.2: Làm sao implement Blue-Green hoặc Canary deployment?

**Trả lờI:**
**Blue-Green:**
- Chạy 2 môi trường identical: Blue (current), Green (new).
- Deploy new version lên Green, chạy smoke test.
- Switch traffic từ Blue sang Green bằng load balancer (instant switch).
- Nếu lỗi, switch lại Blue (instant rollback).
- **Nhược điểm**: Tốn resource gấp đôi.

**Canary:**
- Deploy new version cho 5% traffic.
- Monitor metrics: error rate, latency, CPU.
- Nếu ổn, tăng dần: 25% → 50% → 100%.
- Nếu lỗi, automatic rollback.
- **Implement**: Istio/Flagger (Kubernetes), AWS App Mesh, hoặc ALB weighted target group.

---

## 11. IoT, Connected Vehicle & EV Platform

### Câu hỏi 11.1: Kiến trúc tổng quan của Connected Vehicle Platform như thế nào?

**Trả lờI:**
**Layers:**
1. **Vehicle Layer**: ECU, TCU (Telematics Control Unit), sensors, OBD-II, CAN bus.
2. **Connectivity Layer**: Cellular (4G/5G), WiFi, Bluetooth. MQTT/HTTP client trên vehicle.
3. **Ingestion Layer**: AWS IoT Core / Azure IoT Hub — nhận message, authenticate device bằng X.509 certificate.
4. **Stream Processing Layer**: Kafka/Kinesis → Flink/Spark Streaming → real-time analytics, anomaly detection.
5. **Application Layer**: Microservices — vehicle management, telemetry, remote command, OTA update.
6. **Data Layer**: Hot storage (DynamoDB/Redis), Warm storage (RDS), Cold storage (S3 Data Lake).
7. **Presentation Layer**: Mobile app, web dashboard cho user và fleet operator.

**Key capabilities:**
- **Remote Command**: Lock/unlock, climate control, horn/flash.
- **Telemetry**: Battery SOC, GPS, speed, tire pressure, diagnostic codes.
- **OTA (Over-The-Air)**: Firmware update phân tán, rollback nếu fail.
- **Predictive Maintenance**: ML model phân tích telemetry để dự đoán hỏng hóc.

---

### Câu hỏi 11.2: Làm sao xử lý OTA (Over-The-Air) update cho phương tiện an toàn?

**Trả lờI:**
**OTA Architecture:**
```
Cloud OTA Service → CDN (S3/CloudFront) → Vehicle downloads firmware
```

**Safety measures:**
1. **Digital Signature**: Firmware được sign bằng private key. Vehicle verify bằng public key trước khi flash.
2. **Rollback**: Giữ 2 firmware slots (A/B partition). Nếu new firmware boot fail, tự động rollback sang old firmware.
3. **Phased Rollout**: Update 1% fleet → 10% → 100%. Monitor error rate ở mỗi phase.
4. **Condition Check**: Chỉ update khi vehicle đỗ, battery > 50%, đang kết nối WiFi (hoặc stable 4G).
5. **Campaign Management**: Schedule update, allow user defer, emergency update cho critical security patch.
6. **Audit Log**: Ghi lại mọi update attempt, success/failure, version trước/sau.

---

### Câu hỏi 11.3: Làm sao xử lý millions of IoT connections với AWS IoT Core?

**Trả lờI:**
1. **Device Provisioning**: Dùng JITP (Just-In-Time Provisioning) hoặc fleet provisioning để tự động register device khi first connect.
2. **Certificate Management**: X.509 certificate cho mỗi device, rotate định kỳ. Dùng AWS IoT Device Management.
3. **Topic Design**: Hierarchical topic structure:
   - `vehicles/{vehicle_id}/telemetry`
   - `vehicles/{vehicle_id}/commands/lock`
   - Dùng topic policy để device chỉ publish/subscribe topic của mình.
4. **Rule Engine**: IoT Core rule forward message sang Kinesis/Kafka/Lambda mà không cần custom broker.
5. **Device Shadow**: Lưu trạng thái desired/reported cho offline device. Khi device online, sync tự động.
6. **Scaling**: IoT Core tự động scale. Dùng multiple AWS accounts nếu vượt quota.
7. **Cost**: Batch telemetry data (gửi mỗi 30s thay vì real-time liên tục) để giảm message count.

---

## 12. Kỹ năng mềm & Tư duy giải quyết vấn đề

### Câu hỏi 12.1: Làm sao giải thích kiến trúc microservices cho stakeholder không kỹ thuật?

**Trả lờI:**
**Analogies:**
- "Monolith như một cửa hàng tạp hóa lớn — mọi thứ ở chung một chỗ. Nếu khu rau củ cháy, cả cửa hàng đóng cửa."
- "Microservices như một trung tâm thương mại — mỗi gian hàng độc lập. Gian hàng A sửa chữa không ảnh hưởng gian hàng B."

**Communication tips:**
- Dùng visual diagram (C4 Level 1-2), không dùng code.
- Tập trung vào **business value**: "Tách riêng dịch vụ thanh toán giúp chúng ta update tính năng mới mà không ảnh hưởng đặt xe."
- Dùng metrics dễ hiểu: "ThờI gian deploy giảm từ 2 tuần xuống 2 giờ."
- Tránh jargon: không nói "eventual consistency", mà nói "dữ liệu sẽ đồng bộ trong vài giây."

---

### Câu hỏi 12.2: Kể về lần bạn identify và fix một bottleneck nghiêm trọng.

**Trả lờI (STAR Method):**
- **Situation**: Hệ thống xử lý đơn hàng chậm dần trong giờ cao điểm, latency tăng từ 200ms lên 5s.
- **Task**: Tôi được giao tìm nguyên nhân và fix trong 48 giờ.
- **Action**:
  1. Dùng APM trace thấy bottleneck ở DB query — `SELECT * FROM orders WHERE status = 'PENDING'` full table scan.
  2. Kiểm tra execution plan: sequential scan trên 10M rows.
  3. Thêm composite index `(status, created_at)`, rewrite query chỉ select cần thiết.
  4. Implement connection pooling (HikariCP) và cache cho status count.
  5. Add read replica cho reporting query.
- **Result**: Latency giảm về 150ms, throughput tăng 3x. Viết post-mortem và add monitoring alert.

---

## 13. Câu hỏi hành vi & Leadership

### Câu hỏi 13.1: Bạn làm sao khi có conflict về technical approach với team member?

**Trả lờI:**
1. **Listen first**: Hiểu perspective của họ — có thể họ thấy risk mình bỏ sót.
2. **Data-driven**: Dùng benchmark, POC, hoặc reference architecture để so sánh approach.
3. **Trade-off analysis**: Lập bảng so sánh — scalability, complexity, time to market, maintenance cost.
4. **Seek third opinion**: Nếu vẫn bế tắc, nhờ senior engineer hoặc architect khác review.
5. **Decision**: Nếu mình là owner, quyết định và chịu trách nhiệm. Nếu không, respect decision của tech lead.
6. **Document**: Ghi lại lý do chọn approach để team hiểu và học hỏi.

---

### Câu hỏi 13.2: Làm sao bạn mentor junior developer?

**Trả lờI:**
1. **Pair programming**: Code cùng để họ thấy thought process, không chỉ kết quả.
2. **Code review as teaching**: Giải thích "tại sao" thay vì chỉ yêu cầu sửa.
3. **Gradual responsibility**: Giao task từ dễ đến khó, với clear expectation và deadline.
4. **Reading list**: Recommend sách (Clean Code, Designing Data-Intensive Applications), blog, conference talks.
5. **Regular 1:1**: Hỏi về career goal, blockers, feedback hai chiều.
6. **Safe environment**: Không blame khi lỗi — focus vào fix và learn.

---

### Câu hỏi 13.3: Bạn prioritize task như thế nào khi có nhiều deadline chồng chéo?

**Trả lờI:**
1. **Impact vs Effort matrix**: Ưu tiên task high impact, low effort (quick wins).
2. **Business value**: Task ảnh hưởng đến revenue, customer experience, compliance được ưu tiên.
3. **Dependency**: Task block người khác được làm trước.
4. **Communicate early**: Nếu không kịp, báo sớm với stakeholder để negotiate scope hoặc deadline.
5. **Time blocking**: Dành block thờI gian focus cho deep work, tránh context switching.
6. **Delegate**: Nếu có team, delegate task phù hợp với capability của từng người.

---

## PHỤ LỤC: CHECKLIST TRƯỚC KHI PHỎNG VẤN

- [ ] Ôn lại Java core: Collections, Concurrency, JVM, GC.
- [ ] Ôn Spring Boot: Auto-configuration, AOP, Transaction, Security.
- [ ] Chuẩn bị 2-3 dự án cụ thể để kể (dùng STAR method).
- [ ] Chuẩn bị câu hỏi cho interviewer về team, tech stack, growth opportunity.
- [ ] Luyện vẽ architecture diagram trên giấy/whiteboard.
- [ ] Chuẩn bị explain một system design từ A-Z (ví dụ: EV platform).
- [ ] Review AWS services liên quan đến JD.
- [ ] Chuẩn bị câu hỏi về lương, benefit (nếu đến vòng cuối).

---

> **Chúc bạn phỏng vấn thành công! 🚀**
