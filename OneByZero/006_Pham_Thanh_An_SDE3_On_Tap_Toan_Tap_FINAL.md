# PHỎNG VẤN ONEBYZERO SDE III — BỘ ÔN LUYỆN TOÀN TẬP
**Phạm Thanh An | Vòng: Technical (sau headhunter) | Phiên bản: dành cho người luyện nói tiếng Anh**

> **Cách dùng file này:** Đọc xong một phần → ghi âm mình nói to bằng tiếng Anh → nghe lại → nói lại.
> Mọi câu trả lờ mẫu đều viết bằng **câu ngắn, từ đơn giản** để dễ thuộc. Đừng cố nói hay hơn mẫu — nói đúng và rõ là đủ.

---

# PHẦN 0 — DÀNH CHO NGƯỜI YẾU TIẾNG ANH (đọc trước tiên)

## 0.1. Sự thật giải tỏa áp lực
- Interviewer ngước ngoài **không kỳ vọng tiếng Anh hoàn hảo**. Họ cần hiểu ý anh.
- Sai ngữ pháp = bình thường. **Im lặng lâu = mất điểm.**
- Nói chậm, câu ngắn, luôn thắng ngườ nói nhanh câu dài.
- Mục tiêu của anh: nói được ý chính bằng 100–200 từ đơn giản cho mỗi câu hỏi 🔥. Không cần hơn.

## 0.2. 10 câu "mua thờ gian" — THUỘC LÒNG TRƯỚC MỌI THỨ
Dùng khi não chưa kịp nghĩ. Nói tự nhiên, không ai biết anh đang dịch trong đầu.

| Tiếng Anh | Nghĩa |
|---|---|
| "That's a good question. Let me think for a moment." | Câu hỏi hay, cho tôi nghĩ một chút. |
| "Let me structure my answer." | Để tôi trình bày theo cấu trúc. |
| "There are three main points here." | Có 3 ý chính ở đây. |
| "First... Second... Finally..." | Thứ nhất... Thứ hai... Cuối cùng... |
| "Let me clarify the requirements first." | Để tôi làm rõ yêu cầu đã. |
| "There are trade-offs in both options." | Cả hai phương án đều có đánh đổi. |
| "In production, the failure mode would be..." | Trên production, kiểu lỗi sẽ là... |
| "I would measure it with p95 latency and error rate." | Tôi sẽ đo bằng p95 và tỉ lệ lỗi. |
| "That's a fair point. Let me reconsider." | Ý đó đúng. Để tôi xem lại. |
| "Could you repeat the question, please?" | Nhắc lại câu hỏi được không? (Không xấu hổ gì!) |

## 0.3. Câu cứu nguy khi bí trong system design
> "Let me first clarify the requirements. Then I will choose the simplest architecture. I will also think about scalability, failure handling, security, and observability."
> *(Để tôi làm rõ yêu cầu trước. Rồi tôi chọn kiến trúc đơn giản nhất. Tôi cũng sẽ nghĩ về khả năng mở rộng, xử lý lỗi, bảo mật và quan sát hệ thống.)*

Nói câu này, rồi mở file nhìm framework 9 bước (Phần 3.1) và đi từng bước.

## 0.4. Quy tắc luyện 30 phút mỗi ngày
1. Chọn 1 câu hỏi 🔥.
2. Nhìn từ khóa (KHÔNG nhìn câu mẫu).
3. Nói to, ghi âm điện thoại.
4. Nghe lại, so với mẫu, nói lại 1 lần.
5. Tối trước khi ngủ: nghe lại tất cả bản ghi âm trong tuần.

---

# PHẦN 1 — 5 SCRIPT BẮT BUỘC THUỘC LÒNG (viết sẵn câu ngắn)

> Đây là 5 bài nói anh phải ra **tự động không cần nghĩ**. Mỗi bài 60–90 giây. Thuộc từ khóa, không thuộc chữ từng chữ.

## 1.1. 🔥 Script 1 — Tell me about yourself (60–90s)

> My name is Pham Thanh An.
> I am a Senior Software Engineer and Solution Architect.
> I have more than 15 years of experience in backend systems.
> 
> My strongest skills are: Python, Java, and C#.
> I have deep experience in distributed systems, AWS, and performance optimization.
> 
> In my current role, I own the architecture of backend platforms.
> These platforms serve more than 2 million users.
> I also lead technical work and mentor engineers.
> One example: I reduced API response time by three times.
> I also built a GPU backtesting system. It concurrently processes 100 million trades.
> And I built a TensorFlow model to predict gold prices.
>
> I am interested in OneByZero because it combines engineering, AWS, and AI.
> I believe I can contribute from day one.

💡 *Từ khóa để dựng lại: name → role → 15 years → Python/Java/C# → distributed/AWS → 2M users → lead & mentor → 3x → GPU → TensorFlow → why OneByZero.*

## 1.2. 🔥 Script 2 — Greatest ownership (WWE Champion)

> The project with my greatest ownership was a real-time gaming platform.
> It runs on AWS and serves more than 2 million users.
> Twenty to twenty-five thousand users are active every day.
> 
> My job was the backend architecture and the whole delivery.
> 
> We had a problem: API latency was high under load.
> I did not add more servers first.
> I profiled the request path. I found the real bottlenecks.
> Some work was blocking the request, but it was not necessary.
> Some data was loaded again and again on every request.
> 
> So I made two changes.
> First, I moved non-blocking work to async processing.
> Second, I added local caching on each node.
> 
> After these changes, API response time improved up to three times.
> The lesson: always measure first. Do not guess.

**Follow-up chắc chắn bị hỏi:**
- *What did YOU implement yourself?* → "I designed the architecture and wrote the core server logic and the performance changes. My team had about five engineers."
- *How did you measure 3x?* → "We compared response time before and after, with similar traffic. Some endpoints became about three times faster." *(Dùng số thật của anh nếu nhớ.)*
- *Risk of local cache?* → "Stale data. So we need TTL and careful invalidation. Some data must not be cached."

## 1.3. 🔥 Script 3 — 3x latency (kể ngắn gọn, 60s)

> We had slow APIs in production.
> My first step was measurement, not guessing.
> I profiled the request path.
> I found two problems.
> Problem one: some work blocked the response, but it could run later.
> Problem two: some data was loaded repeatedly.
> My fix: async processing with ExecutorService, plus per-node caching.
> Result: response time improved up to three times.
> Today I would do it more carefully.
> I would define baseline metrics first: p50, p95, p99.
> Then I would test each change with load tests.

## 1.4. 🔥 Script 4 — Production incident (Mnews / game outage)

> In one telecom project, a news service was unstable.
> Failures happened sometimes, with no clear pattern.
> I did not restart services blindly.
> I wrote a log-analysis tool.
> It collected and correlated failure events.
> The tool showed us the root cause pattern.
> I proposed a fix. The service became stable.
> After that, I improved monitoring to catch it earlier next time.
> My incident process is: stabilize first, find root cause with evidence, then prevent recurrence.

- In most backend systems, we have monitoring and centralized logging systems. When an incident occurs, the monitoring system sends an alert. We use metrics, logs, and traces to investigate the root cause.
- If the incident is urgent, we first apply a workaround to restore the service as quickly as possible. Then, we investigate the root cause and implement a hotfix if necessary.
- After the issue is resolved, I write an incident report and share it with the relevant stakeholders. The report includes the impact, root cause, resolution, and preventive actions to avoid similar incidents in the future.

## 1.5. Script 5 — Why OneByZero / Why SDE III (30s)

> OneByZero combines things I know well: backend engineering, AWS, distributed systems.
> And things I want to grow in: enterprise AI.
> The SDE III role matches how I already work: I own systems end-to-end, I mentor, and I work with enterprise clients.
> I did client-facing work with telecom carriers before, so this model is familiar to me.

## 1.6. Script 6 — Câu hỏi về chứng chỉ AWS (nói thật, biến yếu thành mạnh)

> I do not have the AWS certification yet.
> But I have run production systems on AWS for almost ten years.
> DynamoDB, Redis, fleets behind load balancers — at millions of users.
> The certification is formal proof. Real production is real proof.
> I am planning to take the Solutions Architect Associate exam.

---

# PHẦN 2 — NGÂN HÀNG CÂU HỎI / TRẢ LỜI

> Chú thích: 🔥 bắt buộc | ⭐ khả năng cao | 💡 câu tạo khác biệt

## 2.1. 🔥 How do you approach system design?

> First, I clarify the requirements. Functional and non-functional.
> I ask about: users, traffic, read/write ratio, latency, availability, consistency, security.
> Second, I estimate the scale. Requests per second, data volume.
> I state my assumptions clearly.
> Third, I design the high-level architecture.
> APIs, data stores, cache, async processing.
> I choose the data store based on access patterns.
> Fourth, I design for failure. Timeouts, retries, idempotency.
> Fifth, security and observability.
> My goal: the simplest architecture that works and can grow.

1. Clarify requirements
   - Functional / non-functional
   - Users, traffic, read/write ratio
   - Latency, availability, consistency, security
2. Estimate scale
   - RPS
   - Data volume
   - Nêu rõ assumptions
3. High-level architecture
   - APIs
   - Database / data store
   - Cache
   - Async processing
   - Chọn DB dựa trên access pattern
4. Design for failure
   - Timeout
   - Retry
   - Idempotency
5. Security & observability
   - Security
   - Monitoring / logging / tracing
   - Mục tiêu: simplest architecture that works and can grow
Và câu kết:
My goal: the simplest architecture that works and can grow.

## 2.2. 🔥 What is your AWS experience?

> I design and run production systems on AWS.
> I use EC2 fleets behind load balancers.
> DynamoDB is my main NoSQL store. I design tables around access patterns.
> ElastiCache / Redis for caching.
> S3 for objects. IAM with least privilege.
> I also built the DevOps around it: Jenkins and GitHub Actions for CI/CD.
> And custom Grafana plugins for observability.
> I also worked on Azure — Cosmos DB and Service Bus.
> So I can compare trade-offs honestly.

## 2.3. ⭐ How do you handle production incidents?

> First: understand the impact. Error rate, latency, affected users.
> Second: check recent changes. Deployment, config, database.
> Third: check system health. CPU, memory, database, cache, queue.
> Fourth: trace the request path step by step.
> Then mitigate. Rollback if needed. Or rate limit. Or scale.
> After stabilization: find the real root cause with evidence.
> Finally: prevention. Better tests, better monitoring, better alerts.

## 2.4. ⭐ How do you mentor engineers?

> I teach reasoning, not answers.
> In code review, I ask questions.
> What happens under load? What fails first? Any security risk?
> For complex tasks, I help split the problem. But the engineer owns the solution.
> My goal: engineers who can make good decisions alone.

## 2.5. 🔥 What is your experience with AI / agentic AI? (nói thật, định vị mạnh)

> My AI experience is hands-on but practical.
> I built a TensorFlow model to predict gold prices. It runs in a live trading bot.
> I built a GPU backtesting framework. It processes about 100 million trades in parallel.
> My core strength is production engineering: distributed systems, AWS, reliability, security.
> I see an AI agent as a distributed system with an LLM inside.
> It fails in all the classical ways: retries, duplicates, timeouts, unauthorized access.
> Plus new risks: hallucination, prompt injection.
> So I can contribute immediately on architecture, reliability, and security of AI systems.
> And I will grow my agentic AI practice on the job.

**Follow-up:** *What is an AI agent?* → "A system where a model reasons about a task, chooses tools, executes actions, observes results, and iterates toward a goal."

## 2.6. ⭐ How do you use AI coding tools?

> I use AI tools to move faster.
> Code exploration, test cases, debugging ideas, documentation drafts.
> But I treat AI output as a draft.
> I always validate: tests, code review, security checks.
> I own the production outcome, not the tool.

## 2.7. 💡 Tell me about your Grafana plugin (điểm khác biệt của anh)

> For a game backend with about 1 million users, the ops team had no visibility.
> So I developed a custom Grafana plugin.
> It monitors production game servers in real time.
> The team can see server health on dashboards.
> Earlier, I also built monitoring plugins with RHQ and Nagios.
> And log-analysis tools that fixed an unstable carrier service.
> My principle: metrics, logs and traces must show symptoms AND root cause.

**Follow-up:** *What metrics?* → (chuẩn bị số liệu thật: CPU, memory, connection count, players per node, message rate...)
*Grafana plugin written in?* → "TypeScript and React for the panel. A backend collector, often in Go."

## 2.8. 💡 How would you design distributed rate limiting? (nối với RedLock của anh)

> Single instance first: token bucket or sliding window in memory.
> Distributed: store counters in Redis. Use a Lua script.
> The script makes check-and-increment atomic.
> Without atomicity, two requests at the same time can both pass.
> I implemented distributed locking and rate limiting with RedLock on Redis before.
> I know the failure modes: Redis latency, clock drift, Redis outage.
> Redis outage means a choice: fail open, or fail closed. Must decide per endpoint.
> I would also rate limit at the API gateway, plus per-service limits.

## 2.9. ⭐ Why are you leaving your current job?

> I have been at my current company for almost ten years.
> I built great systems there.
> Now I want a bigger enterprise context, and to go deeper into AI systems.
> This role is a step forward, not an escape.

## 2.10. ⭐ Compensation expectations?

> Based on the SDE III scope — system ownership, architecture, client work — and my experience, I am looking at around 110 million VND gross per month, depending on the full package. I am open to discussion.
> *(Đổi con số theo mức anh thật sự muốn.)*

---

# PHẦN 3 — SYSTEM DESIGN (framework + 5 bài tập)

## 3.1. 🔥 Framework 9 bước — in trong đầu, nói to được

```
1. Requirements      — Yêu cầu chức năng + phi chức năng
2. Scale estimates   — Ước lượng: users, RPS, data
3. High-level design — Vẽ các khối chính
4. Data model        — Chọn SQL/NoSQL, partition key
5. Cache             — Có cần không, TTL, invalidation
6. Async processing  — Queue cho việc không cần trả lờ ngay
7. Failure handling  — Timeout, retry, idempotency, DLQ
8. Security          — AuthN, AuthZ, rate limit, encryption
9. Observability     — Metrics, logs, traces, alerts
```

Câu mở đầu mọi bài design: *"Let me clarify the requirements first. Two million registered users does not mean two million concurrent users."*

## 3.2. 🔥 Bài 1: API platform cho 2 triệu users

```
Client → CDN → Load Balancer → Stateless API services (autoscaling)
                                  ├── Redis (cache, rate limit)
                                  ├── DynamoDB (primary store)
                                  ├── SQS/Event bus → async workers
                                  └── S3
```

**Nói được những ý này:**
- Stateless services → scale ngang dễ.
- DynamoDB: chọn vì access pattern rõ, latency ổn định, scale ngang. Đổi lại: không query tự do, phải thiết kế partition key theo access pattern. Cẩn thận hot partition.
- Cache-aside cho dữ liệu đọc nhiều. TTL + invalidation. Không cache dữ liệu cần consistent.
- SQS cho việc không cần response ngay (email, report...).
- Failure: timeout, retry + backoff, circuit breaker.
- Observability: p50/p95/p99, error rate, alarms.

**Hỏi sâu:** *What breaks first at 10x traffic?* → "Hot partitions in DynamoDB. And worker fan-out. Mitigation: better partition keys, backpressure, autoscaling."

## 3.3. 🔥 Bài 2: Design a Facebook page (bài ứng viên thật bị hỏi tại OneByZero!)

Anh có lợi thế: từng làm social network ở EGGroup với Elasticsearch.

```
Client → Load Balancer → API services
                            ├── Post service → SQL/NoSQL
                            ├── News Feed service ← Redis cache
                            ├── Search service → Elasticsearch
                            └── Media → Object storage (S3) + CDN
```

**Nói được:**
- News feed: fan-out model. Push cho user ít follower, pull cho user nhiều follower (hoặc hybrid).
- Cache feed ở Redis.
- Search: Elasticsearch, index theo document.
- Media: S3 + CDN, presigned URL.
- Feed eventual consistency được — user thấy bài mới chậm 1–2 giây không sao.
- *"I actually built a social network backend with Elasticsearch before. So I know this problem in practice."* ← CÂU VÀNG, nhớ nói.

## 3.4. ⭐ Bài 3: Event-driven order processing

```
Order API → Order DB (transactional outbox)
    → Message bus (SQS)
        ├── Payment consumer → Payment DB
        ├── Inventory consumer
        └── Notification consumer
    ← DLQ for poison messages
```

**Nói được:**
- *Exactly-once là khẩu hiệu. Thực tế: at-least-once delivery + idempotent consumer = hiệu ứng exactly-once.*
- Idempotency: event ID lưu cùng transaction với business update.
- Outbox pattern: ghi DB + event trong CÙNG một transaction, relay đẩy lên bus.
- Payment fails → retry có giới hạn → DLQ → compensation/refund flow.
- Monitor: queue depth, consumer lag.

## 3.5. ⭐ Bài 4: Enterprise AI assistant (rất quan trọng với OneByZero)

```
User → API Gateway + Auth → Agent Orchestrator
                              ├── Retrieval (ingestion, chunking, embedding,
                              │    ACL filtering — chỉ trả doc user được phép xem)
                              ├── LLM
                              └── Tool Gateway (CRM/HR/Finance APIs)
                                    mỗi tool tự check authorization
                              + Human approval cho action nhạy cảm
                              + Full trace mọi bước
```

**5 ý bắt buộc nói (khác biệt L6 vs junior):**
1. *"The model never has unrestricted access. Each tool checks authorization itself. The LLM is not the security boundary."* (LLM không phải ranh giới bảo mật.)
2. Retrieval phải lọc theo ACL **cờ ngườ dùng** trước khi đưa vào prompt — chống lộ dữ liệu.
3. Prompt injection: coi nội dung retrieve được là dữ liệu không tin cậy; tool least-privilege; không bao giờ thực thi instruction nằm trong document.
4. Human-in-the-loop cho hành động không thể hoàn tác; idempotency key để retry tool call không chạy 2 lần.
5. Evaluation: bộ test cố định, đo chất lượng câu trả lờ, retrieval accuracy, unauthorized access, latency, cost. Trace đầy đủ để debug.

## 3.6. 💡 Bài 5: Real-time multiplayer backend (thế mạnh riêng của anh)

```
Clients → Load Balancer → Game nodes (stateful, session affinity)
                              ├── Redis (shared state, locks)
                              └── Database (persistence)
```

- Session affinity: một phiên chơi gắn với một node.
- Node chết → migrate session, hoặc chấp nhận mất ván.
- *"I built systems like this in production — SmartFox nodes with a Service Bus design for horizontal scaling."*

---

# PHẦN 4 — CODING (3 bài chính + thinking aloud script)

**Quy trình nói khi code (ghi âm luyện mãi thành phản xạ):**
1. *"Let me think aloud."*
2. *"First, the brute force is... but it is O(n²). Too slow."*
3. *"Better idea: [ Sliding window / hash map / heap ]."*
4. *"Time complexity: O(n). Space: O(k)."*
5. Code xong: *"Let me test with an example. Edge cases: empty input, single element."*

## 4.1. Longest Substring Without Repeating Characters
```python
def longest_substring(s: str) -> int:
    last_seen = {}
    left = 0
    best = 0
    for right, ch in enumerate(s):
        if ch in last_seen and last_seen[ch] >= left:
            left = last_seen[ch] + 1
        last_seen[ch] = right
        best = max(best, right - left + 1)
    return best
```
*Thinking aloud: "I keep a window [left, right]. If a char repeats inside the window, I move left. Each char is visited once → O(n)."*

## 4.2. Top K Frequent Elements
```python
from collections import Counter
import heapq

def top_k(nums, k):
    counts = Counter(nums)
    return [v for v, _ in heapq.nlargest(k, counts.items(), key=lambda x: x[1])]
```
*Thinking aloud: "Count with a hash map — O(n). Then a heap of size k — O(m log k). Bucket sort is an O(n) alternative."*

## 4.3. LRU Cache
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.store = OrderedDict()

    def get(self, key):
        if key not in self.store:
            return -1
        self.store.move_to_end(key)
        return self.store[key]

    def put(self, key, value):
        if key in self.store:
            self.store.move_to_end(key)
        self.store[key] = value
        if len(self.store) > self.cap:
            self.store.popitem(last=False)
```
*Nói thêm: "OrderedDict is the practical way. The real structure is a hash map plus a doubly linked list — both O(1). I can implement it from scratch if you want."*

## 4.4. Rate Limiter (100 req / 60s / user) 💡 đúng thế mạnh anh
```python
# Sliding window log với Redis sorted set, atomic bằng Lua script:
# 1. ZREMRANGEBYSCORE key 0 (now - 60s)   → xóa entry hết hạn
# 2. Nếu ZCARD key < 100 → ZADD + trả 1 (cho qua)
# 3. Ngược lại trả 0 (từ chối)
```
*Thinking aloud: "Single instance: keep counters in memory. Distributed: Redis + Lua script for atomic check-and-increment. Otherwise two requests at the same moment can both pass. Sliding window log is exact but heavy; token bucket is cheaper. I built this with RedLock before, so I know the failure modes: Redis latency, and on Redis outage you must choose fail-open or fail-closed."*

---

# PHẦN 5 — KIẾN THỨC NỀN (tự hỏi tự trả lờ TO, mỗi câu 30–60s)

## 5.1. Distributed Systems
- **Idempotency?** → "An operation is idempotent if doing it many times has the same effect as doing it once. Important because retries can duplicate an operation that already succeeded. Pattern: client sends an idempotency key. Server stores the result. A retry returns the stored result, not a new operation."
- **Duplicate messages?** → "Assume at-least-once delivery. Use a stable event ID. Make the consumer idempotent. Record processed IDs in the same transaction as the business update."
- **Retries?** → "Only for transient failures. Bounded attempts. Exponential backoff with jitter. Respect the deadline. Never retry non-idempotent operations without protection."
- **Eventual consistency?** → "Replicas may differ temporarily, but converge if updates stop. Good for caches, feeds, analytics. Not good for payment invariants. Strong consistency must be enforced at the invariant boundary."
- **Distributed lock?** (RedLock — anh có kinh nghiệm thật!) → "A lock is a key with a TTL, set atomically: SET NX PX. Danger: a long GC pause can make the lock expire while you still hold it — two holders. Mitigation: fencing tokens, short critical sections, TTL above worst-case pause. This is exactly the RedLock debate."
- **CAP?** → "During a network partition, choose consistency or availability. In practice, design for partition tolerance and choose per operation. Payments choose C. Feeds choose A."

## 5.2. Caching
- **When to cache?** → "Repeated expensive reads, and data that tolerates staleness. Consider: frequency, size, TTL, invalidation, hit rate."
- **Cache problems?** → "Stale data, invalidation complexity, stampede, memory, cold cache, availability."
- **Redis down?** → "If cache is only an optimization, fall back to the source of truth, but protect the database from the load spike. Timeouts, circuit breaker, rate limit."

## 5.3. AWS
- **Secure an AWS app?** → "Least-privilege IAM, roles over users. Public/private subnets, security groups. Encryption in transit and at rest. Secrets in a manager, never in code or logs. Input validation, authN, authZ, rate limiting at the app. Security scanning in CI/CD. Central audit logs."
- **DynamoDB?** → "Design tables around access patterns, not normalization. Partition key, sort key, watch hot partitions, GSI for alternate access. Query vs scan. Predictable low latency at scale."
- **SQS?** → "Producer, queue, consumer. Visibility timeout. At-least-once delivery. Idempotent consumers. DLQ for poison messages. Long polling. Monitor queue depth."

## 5.4. Security (gắn với TheWinZone)
- **Secure an API?** → "Authentication, authorization, input validation, sanitization, rate limiting, TLS, secrets management, audit logs, no sensitive data in logs."
- **Kể câu chuyện pentest:** → "I led backend security hardening on a 750K-user platform. We found vulnerabilities in pentests. I fixed them, added automatic input sanitization at the model-binding layer, and added distributed locking and rate limiting with RedLock on Redis. My principle: make the secure path the default path, so developers cannot forget it."

## 5.5. Python
- **GIL?** → "In CPython, only one thread executes bytecode at a time in a process. Threads still help for I/O-bound work. For CPU-bound work, use multiprocessing — or vectorized/GPU code, like my CuPy backtesting."
- **Threading vs asyncio vs multiprocessing?** → "Asyncio: many concurrent I/O with async libraries. Threads: blocking I/O libraries. Multiprocessing: CPU-bound. GPU: parallel numerical work."

---

# PHẦN 6 — AI / AGENTIC AI (bổ sung cho JD OneByZero)

## 6.1. Biggest challenges in enterprise AI?
> "Enterprise AI is not only a model problem. It is also a systems engineering, security, integration and reliability problem."
> Risks: security, data access, authorization, hallucination, evaluation, observability, cost, latency, legacy integration.

## 6.2. What is prompt injection and how to defend?
> "Prompt injection is when untrusted content manipulates the model's instructions.
> Defense: treat retrieved documents as untrusted data. Least-privilege tools. Output validation. Never execute instructions found inside documents. Human approval for sensitive actions."

## 6.3. How to evaluate an AI system?
> "Golden test set with representative questions. Measure: answer quality, retrieval accuracy, unauthorized access attempts, latency, cost. Full execution traces to debug multi-step failures."

---

# PHẦN 7 — BEHAVIORAL & CLIENT

## 7.1. Technical disagreement?
> "First I make sure I understand their reasoning and the real requirements.
> Then I compare options on clear criteria: reliability, performance, security, complexity, delivery time.
> If needed, I validate with data or a small prototype.
> After the decision, I support it and focus on execution.
> The goal is the best decision for the project, not to win the argument."
> *(Chuẩn bị 1 ví dụ THẬT từ công việc của anh.)*

## 7.2. Client insists on a wrong solution?
> "First, understand the business objective. Often the 'wrong' solution solves a real constraint.
> Then explain trade-offs concretely: cost, scale, delivery time.
> Present alternatives with clear trade-offs.
> If they still choose it after understanding the risks, I document the decision and add safeguards."

## 7.3. Changing requirements?
> "Understand the business goal. Assess impact on architecture, scope, risk. Explain options in simple language. Align on priorities. Document. Adjust the plan."

## 7.4. What does Forward Deployed mean?
> "An engineer close to the customer: find the real business problem, turn it into software, ship it into the customer environment, iterate in production. It needs engineering depth plus communication. I did this with telecom enterprise clients before."

---

# PHẦN 8 — LUYỆN TẬP THỰC CHIẾN

## 8.1. Rubric tự chấm (sau mỗi lần nói)
- **0** — Không nói được gì.
- **1** — Chung chung, không ví dụ, không cấu trúc.
- **2** — Đúng kỹ thuật nhưng không có trade-offs hay kết quả.
- **3** — Có ví dụ thật, giải thích được quyết định + rủi ro + kết quả.
- **4** — Thêm số liệu, cách kiểm chứng, bài học, áp dụng được sang bối cảnh khác.
- **Mục tiêu: 3+ cho mọi câu 🔥 trước ngày phỏng vấn.**

## 8.2. Mock loop 60 phút (làm tối thiểu 3 lần)
| Phút | Bài | Dùng script nào |
|---|---|---|
| 0–5 | Self-intro | 1.1 |
| 5–15 | Ownership + 3x latency + follow-ups | 1.2 + 1.3 |
| 15–35 | System design trắng (giấy) | 3.2 hoặc 3.3 |
| 35–50 | Coding + thinking aloud | 4.1 / 4.3 / 4.4 |
| 50–57 | AI + 1 câu Phần 5 | 2.5 + 5.x |
| 57–60 | Hỏi lại họ | 8.3 |

## 8.3. Câu hỏi để hỏi lại họ (chọn 2–3, hỏi cuối buổi)
1. "What would you expect an SDE III to accomplish in the first six months?"
2. "How much direct client interaction does an SDE III have? How often is travel needed?"
3. 💡 "What distinguishes someone who succeeds at L6 here from someone technically strong but not yet at that level?" *(Câu vàng — hỏi được cảm ơn tôi sau.)*
4. "What agentic AI systems is the team building now?"

## 8.4. Lịch 7 ngày cho ngườ yếu tiếng Anh
| Ngày | Làm gì |
|---|---|
| 1 | Thuộc Script 1.1 + 1.2. Ghi âm từng bài tối thiểu 5 lần. Nghe lại trước ngủ. |
| 2 | Thuộc Script 1.3 + 1.4 + 1.5. Cùng quy trình ghi âm. |
| 3 | Nói thành thạo framework 9 bước + luyện bài 3.2 và 3.3. |
| 4 | Luyện bài 3.4 + 3.5 (AI assistant — bắt buộc nói đủ 5 ý vàng). |
| 5 | Coding: 4.1, 4.2, 4.3 với thinking aloud script. |
| 6 | Tự hỏi tự trả lờ TO toàn bộ Phần 5 + mock loop đầy đủ 60 phút. |
| 7 | Nhẹ: nghe lại mọi bản ghi âm. Đi ngủ sớm. Không học mới. |

---

# PHẦN 9 — CHECKLIST 24 GIỜ TRƯỚC PHỎNG VẤN

### Nội dung
- [ ] Script 1.1, 1.2, 1.3 nói trôi không nhìn giấy
- [ ] Framework 9 bước nói được
- [ ] Bài Facebook page + AI assistant nói được ý chính
- [ ] 3 bài coding + câu complexity
- [ ] 5 ý vàng của AI assistant
- [ ] Câu trả lờ chứng chỉ AWS (1.6)

### Kỹ thuật (nếu online)
- [ ] Test camera + mic trước 1 ngày
- [ ] Chỗ ngồi sáng, yên tĩnh, nền gọn
- [ ] Mạng dự phòng: hotspot điện thoại bật sẵn
- [ ] Giấy bút để vẽ khi system design
- [ ] Nước uống để cạnh bàn
- [ ] Điện thoại im lặng, đóng mọi thông báo máy tính

### Trong lúc phỏng vấn
- [ ] Nói chậm. Câu ngắn.
- [ ] Não lag → dùng câu mua thờ gian (Phần 0.2)
- [ ] Không biết → "I have not done that in production, but here is how I would approach it..."
- [ ] Mọi câu design đều nói được từ "trade-off"

---

# PHẦN 10 — 1 CÂU NHỚ CUỐI CÙNG

Đừng chứng minh "tôi biết mọi công nghệ". Chứng minh:

> **"I can own a complex system, make sound decisions under constraints, operate it in production, lead others, and explain trade-offs to a client."**
> *(Tôi có thể sở hữu một hệ thống phức tạp, ra quyết định đúng dưới ràng buộc, vận hành nó trên production, dẫn dắt ngườ khác, và giải thích trade-offs cho khách hàng.)*

Mọi câu trả lờ nên chứa: requirement → options → trade-offs → decision → verification → production outcome.


---

# PHẦN 11 — BẢN ĐIỀU CHỈNH: HỌC ĐỂ LÀM ĐƯỢC, KHÔNG HỌC THUỘC

> **Đây là phần quan trọng nhất đối với An nếu tiếng Anh là điểm yếu.**
>
> File này không nên được dùng như một "script để học thuộc". Mục tiêu là:
> **hiểu kiến thức → nói được bằng tiếng Anh đơn giản → làm được bài → chịu được follow-up.**

## 11.1. Nguyên tắc luyện mới

Mỗi chủ đề phải đi qua 4 tầng:

1. **Understand** — hiểu bằng tiếng Việt.
2. **Explain** — giải thích lại bằng tiếng Anh đơn giản trong 30–60 giây.
3. **Apply** — làm một bài coding/design/tình huống liên quan mà không nhìn đáp án.
4. **Defend** — trả lời 2–4 câu hỏi đào sâu "Why?", "What if?", "How do you measure it?", "What can fail?"

Nếu chưa qua tầng 4 thì **chưa coi là đã học xong**.

### Không học thuộc câu trả lời dài

Ví dụ không cần học thuộc:

> "First, I clarify the functional and non-functional requirements..."

Hãy nhớ khung:

> **Requirements → Scale → Design → Data → Failure → Security → Observability**

Sau đó tự dựng câu bằng vốn tiếng Anh của mình.

---

## 11.2. Mỗi câu hỏi phải có "Answer Card"

Với mỗi câu hỏi 🔥, tự chuẩn bị một card gồm đúng 6 dòng:

```text
QUESTION:
KEY POINTS:
MY REAL EXAMPLE:
TRADE-OFF:
RESULT / METRIC:
FOLLOW-UP QUESTIONS:
```

Ví dụ:

```text
QUESTION:
How did you improve API latency?

KEY POINTS:
Measure → profile → find bottleneck → async → cache

MY REAL EXAMPLE:
Real-time gaming platform

TRADE-OFF:
Cache can become stale

RESULT / METRIC:
Up to 3x faster

FOLLOW-UP QUESTIONS:
How did you measure it?
Why local cache?
What about stale data?
What happens at 10x traffic?
```

**Mục tiêu:** nhìn 6 dòng này và nói được 60–90 giây.

---

# PHẦN 12 — TIẾNG ANH PHỎNG VẤN: BỘ CÂU KHUNG TỐI THIỂU

## 12.1. Câu mở đầu khi chưa hiểu

Học theo thứ tự này:

- "Could you repeat the question, please?"
- "Could you say that in another way?"
- "Just to make sure I understand, are you asking about...?"
- "Let me clarify the requirement first."

Không cần cố đoán câu hỏi rồi trả lời sai.

## 12.2. Câu để chuyển ý

- "There are two main points."
- "The first issue is..."
- "The main trade-off is..."
- "The reason I chose this approach is..."
- "The risk here is..."
- "I would verify this with..."
- "In production, I would also consider..."

## 12.3. Khi không biết

Không nói:

> "I don't know."

Nói:

> "I have not used that in production, but I know the basic idea. I would approach it this way..."

Hoặc:

> "I am not sure about that detail. Let me reason through it."

Điểm quan trọng là **không bịa kinh nghiệm**.

## 12.4. Khi interviewer sửa hoặc phản biện

- "That's a good point."
- "I agree. That changes the trade-off."
- "Let me reconsider that."
- "In that case, I would change the design to..."

Mục tiêu là thể hiện khả năng suy luận, không phải cố bảo vệ câu trả lời ban đầu.

---

# PHẦN 13 — KNOWLEDGE MAP: PHẢI HỌC GÌ TRƯỚC?

> Không cần học mọi thứ trong file với độ sâu như nhau.

## Priority A — phải làm được

### A1. System Design

Phải nắm:

- requirements / assumptions
- scale estimation
- API
- stateless service
- load balancer
- database selection
- partition key / indexing
- cache
- queue
- retry / timeout / backoff
- idempotency
- failure handling
- security
- observability

### A2. Coding / DSA

Phải luyện:

- Hash Map
- Two Pointers
- Sliding Window
- Stack / Queue
- Binary Search
- Heap / Top K
- Linked List
- Tree / BFS / DFS
- basic Graph traversal
- intervals
- complexity analysis

Không cần chỉ đọc code mẫu. **Phải tự code được từ đầu và nói được trong lúc code.**

### A3. Distributed Systems

Phải giải thích được bằng lời của mình:

- idempotency
- at-least-once delivery
- duplicate message
- retry
- timeout
- exponential backoff + jitter
- circuit breaker
- eventual consistency
- distributed lock
- cache stampede
- backpressure
- dead letter queue
- transactional outbox

### A4. AWS

Ưu tiên:

- EC2 / Load Balancer
- DynamoDB
- S3
- Redis / ElastiCache
- SQS
- IAM
- networking cơ bản
- CI/CD
- monitoring / logging

Không cần cố học thuộc toàn bộ AWS.

### A5. AI / Enterprise AI

Ưu tiên:

- RAG
- embeddings
- chunking
- retrieval
- ACL / authorization
- tool calling
- agent orchestration
- prompt injection
- hallucination
- evaluation
- tracing
- cost / latency
- human approval
- idempotency cho tool actions

---

# PHẦN 14 — CODING: THÊM BỘ BÀI TẬP, KHÔNG CHỈ 3 BÀI

> Ba bài trong Phần 4 chỉ là bài mẫu. Không nên chỉ luyện đúng ba bài đó.

## 14.1. Pattern phải nhận diện được

| Pattern | Ví dụ bài nên luyện |
|---|---|
| Hash Map | Two Sum, Group Anagrams |
| Sliding Window | Longest Substring, Minimum Window |
| Two Pointers | 3Sum, Container With Most Water |
| Stack | Valid Parentheses, Daily Temperatures |
| Binary Search | Search Rotated Array |
| Heap | Top K Frequent, Kth Largest |
| Linked List | Reverse List, Detect Cycle |
| Tree | Level Order, Lowest Common Ancestor |
| Graph | Number of Islands, Course Schedule |
| Intervals | Merge Intervals |
| Design | LRU Cache, Rate Limiter |

### Quy tắc

Không cần giải hàng trăm bài.

Mục tiêu đầu tiên:

> **Mỗi pattern → 1–2 bài đại diện → tự nhận diện được pattern → tự code → giải thích complexity.**

---

## 14.2. Coding interview script

Trước khi code:

> "Let me clarify the input and output first."

> "I will start with the simple approach."

> "The brute-force solution is O(n²)."

> "We can improve this using a hash map."

Trong khi code:

> "I keep this variable to track..."

> "Here is the important edge case..."

Sau khi code:

> "Let me test it with a small example."

> "The time complexity is O(n), and the space complexity is O(n)."

Nếu bị stuck:

> "I see one issue with my current approach. Let me step back and reconsider."

---

# PHẦN 15 — SYSTEM DESIGN: LUYỆN BẰNG "WHITEBOARD LOOP"

Không đọc solution trước.

Mỗi bài phải làm theo vòng:

### Round 1 — 5 phút

Chỉ làm:

- clarify requirements
- assumptions
- scale

Nói thành tiếng bằng tiếng Anh.

### Round 2 — 10 phút

Vẽ:

```text
Client
  ↓
API Gateway / Load Balancer
  ↓
Services
  ↓
Cache / Database
  ↓
Queue / Workers
```

Sau đó giải thích từng block.

### Round 3 — 10 phút

Interviewer giả lập hỏi:

- What happens if Redis is down?
- What happens if traffic becomes 10x?
- What if a message is delivered twice?
- Where is the bottleneck?
- How do you secure this?
- How do you monitor it?

### Round 4 — 5 phút

Nói:

> "The main trade-offs are..."

và nêu 2–3 trade-offs.

**Đây mới là phần nên luyện nhiều, thay vì đọc sơ đồ có sẵn.**

---

# PHẦN 16 — BỘ "FOLLOW-UP ATTACK"

> Đây là phần file cũ còn thiếu tương đối nhiều. Với SDE III, không nên chỉ luyện câu trả lời đầu tiên.

Với **mọi** câu trả lời kỹ thuật, hãy tự hỏi:

1. **Why?**
2. **Why not another solution?**
3. **What can fail?**
4. **How do you measure it?**
5. **What happens at 10x scale?**
6. **What is the security risk?**
7. **What is the operational cost?**
8. **What did YOU personally implement?**

### Ví dụ

Bạn nói:

> "I use Redis for caching."

Phải tiếp tục trả lời được:

> Why Redis?

> What if Redis is down?

> How do you invalidate the cache?

> What if the cache stampedes?

> What data should not be cached?

> How do you measure cache effectiveness?

Đây là cách chuyển từ **"biết công nghệ"** sang **"SDE III có ownership"**.

---

# PHẦN 17 — PHẦN CẦN SỬA TRONG CÁC SCRIPT CŨ

## 17.1. Script không nên học thuộc từng chữ

Các Script 1.1–1.6 giữ lại, nhưng đổi cách luyện:

### Lần 1

Đọc script.

### Lần 2

Đọc từ khóa.

### Lần 3

Đóng file và tự nói.

### Lần 4

Mình giả lập interviewer hỏi follow-up.

### Lần 5

Trả lời lại mà không nhìn.

**Chỉ khi làm được lần 5 mới đánh dấu ✓.**

---

## 17.2. Những con số phải kiểm tra lại

Các số như:

- 2 million users
- 20–25K daily active users
- 3x faster
- 100 million trades
- 750K users
- gần 10 năm AWS

chỉ được sử dụng nếu đó là số liệu thật mà An có thể bảo vệ khi interviewer hỏi sâu.

Nếu không chắc, dùng:

> "more than..."

hoặc:

> "around..."

hoặc bỏ số.

**Một con số đẹp nhưng không giải thích được sẽ nguy hiểm hơn việc không có số.**

---

## 17.3. Không biến kinh nghiệm thành "claim" quá mạnh

Ví dụ AI:

Không nên cố chứng minh rằng mình là AI expert nếu kinh nghiệm thực tế chủ yếu nằm ở production engineering.

Định vị tốt hơn:

> "My strongest background is production engineering. I have hands-on AI experience, and I can bring distributed systems, reliability and security thinking into AI systems."

Điểm mạnh của An là **engineering depth + production ownership**, không phải cố dùng thật nhiều buzzword AI.

---

# PHẦN 18 — THỨ TỰ ÔN TỐI ƯU CHO NGƯỜI YẾU TIẾNG ANH

## Nếu còn 7 ngày

### Day 1 — English survival + self introduction

- Script 1.1
- Script 1.2
- 10 câu mua thời gian
- 10 câu chuyển ý
- ghi âm 5 lần

### Day 2 — Project deep dive

- 3x latency
- production incident
- Grafana
- security story
- mỗi story phải chịu được 8 follow-up

### Day 3 — System Design

- framework 9 bước
- API platform
- Facebook/social network
- whiteboard loop

### Day 4 — Distributed Systems + AWS

- idempotency
- retry
- queue
- cache
- DynamoDB
- SQS
- Redis
- failure scenarios

### Day 5 — Coding

- Sliding Window
- Hash Map
- Heap
- Linked List
- Tree / Graph
- LRU
- Rate limiter

**Mỗi bài: code + nói. Không chỉ code.**

### Day 6 — Enterprise AI + Client

- RAG
- ACL
- prompt injection
- tool authorization
- agent
- evaluation
- client disagreement
- changing requirements

### Day 7 — Full mock

Một vòng 60–90 phút:

```text
Self introduction
        ↓
Project deep dive
        ↓
System design
        ↓
Coding
        ↓
Distributed systems
        ↓
AI
        ↓
Behavioral / client
        ↓
Your questions
```

Không mở file trong lúc mock.

---

# PHẦN 19 — CÁCH ĐO "ĐÃ SẴN SÀNG CHƯA?"

Đừng đo bằng số trang đã đọc.

Đo bằng 5 tiêu chí:

| Tiêu chí | Chưa đạt | Đạt |
|---|---|---|
| English | đọc hiểu nhưng khó nói | nói câu ngắn tự nhiên |
| Knowledge | nhớ định nghĩa | giải thích + áp dụng |
| Coding | hiểu solution | tự code từ đầu |
| Design | nhớ architecture | tự thiết kế và defend |
| Follow-up | dễ bị bí | có thể reason khi bị hỏi sâu |

### Mục tiêu trước interview

**Không cần 100/100.**

Mục tiêu thực tế:

- Self intro: **90%**
- 3–5 project stories: **80%**
- System design: **70–80%**
- Coding patterns: **70%**
- Distributed/AWS: **70%**
- AI/client: **60–70%**
- English fluency: **đủ để truyền đạt technical reasoning**

---

# PHẦN 20 — QUY TẮC CUỐI CÙNG

> **Không học tiếng Anh riêng rồi mới học kỹ thuật.**

Với buổi interview này, hãy học **English through technical content**.

Ví dụ thay vì học:

> "I would like to improve my communication skills."

Hãy học:

> "The main trade-off is consistency versus availability."

Thay vì học vocabulary rời rạc:

> cache, queue, retry, timeout...

Hãy học thành cụm:

> "If Redis is down, I would fall back to the database, but I need to protect the database from a traffic spike."

Mỗi ngày bạn đang luyện đồng thời:

**English + Technical Knowledge + Interview Communication.**

Đó mới là cách phù hợp nhất với tình trạng hiện tại của An.

---

# PHẦN 21 — LƯU Ý VỀ ĐỘ CHẮC CHẮN CỦA TÀI LIỆU

File này là **bộ chuẩn bị**, không phải đề thi chính thức của OneByZero.

Các nội dung như:

- một bài Facebook page được cho là từng xuất hiện;
- tỷ trọng HLD / coding;
- câu hỏi cụ thể của interviewer;
- mức độ AI/agentic AI trong vòng tiếp theo;

nên được coi là **hypothesis để luyện**, không phải thông tin đã được OneByZero xác nhận.

Vì vậy, chiến lược đúng là chuẩn bị các **transferable skills**:

> explain → design → code → debug → defend decisions → communicate with client.

Nếu đề thay đổi, các kỹ năng này vẫn dùng được.

---

# PHẦN 22 — BẢNG TIẾN ĐỘ HÀNG NGÀY

Mỗi ngày copy bảng này và tự đánh dấu:

```text
DATE: __________

[ ] 10 phút — English warm-up
[ ] 20 phút — Technical knowledge
[ ] 20 phút — nói 1 project story
[ ] 30 phút — coding OR system design
[ ] 15 phút — follow-up questions
[ ] 10 phút — nghe lại recording

TODAY'S TOPIC:
____________________________

3 THINGS I CAN EXPLAIN:
1. _________________________
2. _________________________
3. _________________________

1 THING I STILL DON'T UNDERSTAND:
____________________________

1 ENGLISH SENTENCE I WANT TO SAY BETTER:
____________________________

SCORE:
English:  __/5
Knowledge: __/5
Reasoning: __/5
Confidence: __/5
```

---

# PHẦN 23 — CÁCH CHÚNG TA NÊN LUYỆN CÙNG NHAU

Nếu dùng ChatGPT để luyện, **không nên yêu cầu "cho tôi 100 câu hỏi phỏng vấn"**.

Hãy luyện theo một vòng:

```text
1. Interviewer asks ONE question
              ↓
2. An answers in English
              ↓
3. Correct English
              ↓
4. Check technical accuracy
              ↓
5. Improve the answer
              ↓
6. Ask follow-up
              ↓
7. An answers again
              ↓
8. Score
              ↓
9. Move to next question
```

### Quy tắc đặc biệt

Nếu An trả lời bằng tiếng Việt vì không biết nói tiếng Anh:

**không bỏ qua câu hỏi.**

Hãy:

1. Xác định ý kỹ thuật An muốn nói.
2. Chuyển thành 3–6 câu tiếng Anh đơn giản.
3. Cho An nói lại.
4. Hỏi follow-up bằng tiếng Anh.
5. Lặp lại cho đến khi An tự nói được.

Đây sẽ là **mock interview thực tế**, thay vì chỉ đọc tài liệu.

---

# PHẦN 24 — MỤC TIÊU THỰC SỰ

Mục tiêu không phải:

> "Tôi phải nói tiếng Anh như người bản xứ."

Mục tiêu là:

> **"The interviewer understands how I think."**

Nếu interviewer hiểu được:

- bạn hiểu requirement;
- bạn biết thiết kế;
- bạn biết coding;
- bạn hiểu failure modes;
- bạn biết trade-offs;
- bạn có kinh nghiệm production;
- bạn có thể làm việc với client;
- và khi không biết, bạn biết cách reason;

thì tiếng Anh không hoàn hảo **không còn là điểm yếu chí mạng**.

> **Technical thinking first. Simple English second.**

Đó là chiến lược xuyên suốt của bộ tài liệu này.
---

# PHẦN 25 — ONEBYZERO L6 INTERVIEW PLAYBOOK
> **Mục đích:** gom những phần quan trọng nhất từ tài liệu `OneByZero_SDE3_Interview_Preparation.md` vào đúng chỗ để An chỉ cần học **một file**.
>
> **Cách học:** không học thuộc câu chữ. Học theo **KEYWORDS → nói → follow-up → defend**.

## 25.1. L6 thực sự khác Senior ở đâu?

Khi trả lời technical, đừng dừng ở:

> "I would use Redis."

Hãy đi theo:

> **Why → Assumptions → Trade-offs → Failure modes → Production operation**

Ví dụ:

> "I would use Redis here because the access pattern is read-heavy and the data can tolerate eventual consistency. I would use cache-aside with a TTL. The main risks are stale data and cache stampede, so I would consider TTL jitter or request coalescing depending on traffic."

### Công thức nhớ nhanh

```text
Technology
    ↓
Why?
    ↓
Under what assumptions?
    ↓
Trade-off?
    ↓
What can fail?
    ↓
How do I operate / measure it?
```

> **Đây là một trong những phần quan trọng nhất của toàn bộ file.**

---

# PHẦN 26 — 42 CÂU HỎI PHẢI LUYỆN

> Đây là **question bank để luyện**, không phải danh sách câu hỏi chính thức của OneByZero.

## A. Background

1. Tell me about yourself.
2. Walk me through your current role.
3. What are you strongest at technically?
4. Why are you considering a new opportunity?
5. Why OneByZero?
6. Why SDE III?

## B. Ownership

7. Tell me about a project where you had the greatest ownership.
8. What was the most difficult technical decision you made?
9. Tell me about a production issue you solved.
10. Tell me about a system you designed from scratch.

## C. Architecture

11. Design a scalable API platform.
12. Design an event-driven backend.
13. How would you scale a system from 100K to millions of users?
14. How do you choose between SQL and NoSQL?
15. When should you use caching?
16. How do you design for failure?
17. How do you guarantee idempotency?

## D. AWS

18. How would you architect a production system on AWS?
19. DynamoDB vs relational database?
20. EC2 vs containers/serverless?
21. How would you secure an AWS architecture?
22. How would you monitor a production AWS system?

## E. Python

23. Explain the GIL.
24. Threading vs multiprocessing vs asyncio.
25. How do you optimize a slow Python service?
26. How would you investigate memory growth?

## F. Distributed Systems

27. Explain eventual consistency.
28. How do you handle duplicate messages?
29. How do retries create problems?
30. How do you implement distributed rate limiting?

## G. Leadership

31. How do you mentor engineers?
32. How do you handle technical disagreement?
33. How do you make architecture decisions?
34. How do you handle an engineer who repeatedly introduces production issues?

## H. Client

35. How do you handle a difficult client?
36. What if client requirements conflict with your technical recommendation?
37. How do you explain a complex technical issue to a non-technical stakeholder?

## I. AI

38. What is your experience with AI/ML?
39. What is an AI agent?
40. How would you design an enterprise agentic AI system?
41. What are the risks of using LLMs in regulated environments?
42. How do you use AI coding tools responsibly?

### Cách học 42 câu

Không học 42 câu cùng lúc.

```text
🔥 Tier 1 — phải nói được
1, 5, 7, 9, 11, 13, 16, 17,
18, 19, 21, 27, 28, 30, 32,
35, 36, 38, 39, 40, 41, 42

⭐ Tier 2 — phải hiểu
2, 3, 4, 6, 8, 10, 12, 14, 15,
20, 22, 23, 24, 25, 26, 29,
31, 33, 34, 37

💡 Tier 3 — luyện khi còn thời gian
Các follow-up và biến thể của Tier 1.
```

---

# PHẦN 27 — 15-STEP SYSTEM DESIGN WHITEBOARD

> Framework 9 bước ở Phần 3 vẫn là **framework nhớ nhanh**.
>
> Khi interview hỏi sâu, mở rộng thành 15 bước dưới đây.

```text
1. Requirements
2. Scale / assumptions
3. APIs
4. High-level architecture
5. Data model
6. Storage choice
7. Caching
8. Async processing
9. Scaling
10. Failure handling
11. Consistency / idempotency
12. Security
13. Observability
14. Deployment
15. Bottlenecks / trade-offs
```

### Nếu interviewer nói: "You have only a few minutes."

Nhớ:

> **Requirements → Architecture → Data → Scaling → Failure → Security → Observability**

### Câu mở đầu

> "Let me clarify the requirements first. Then I will make a few assumptions about scale and traffic before choosing the architecture."

### Câu kết

> "The main trade-offs are..."

Sau đó nêu **2–3 trade-offs**.

---

# PHẦN 28 — 3 CÂU CHUYỆN TRỤ CỘT

Nếu thời gian cực ít, ưu tiên 3 story này.

## Story 1 — 2M+ Users / AWS / 3x Performance

```text
2M+ users
    ↓
AWS production
    ↓
Real-time system
    ↓
Performance bottleneck
    ↓
Profile first
    ↓
Async + local cache
    ↓
Up to 3x improvement
```

### Interviewer muốn thấy

- Scale
- AWS
- Architecture
- Performance
- Production ownership

### Follow-up phải chịu được

- Why did you choose this architecture?
- What did you personally implement?
- How did you measure 3x?
- Why local cache?
- What about stale data?
- What happens at 10x traffic?
- What would you redesign today?
- What was the biggest risk?

---

## Story 2 — TheWinZone / Tribe Mania

```text
Security
    ↓
Event-driven architecture
    ↓
Horizontal scaling
    ↓
Reliability
    ↓
Grafana / observability
```

### Interviewer muốn thấy

- Security
- Scalability
- Reliability
- Architecture
- DevOps / observability
- Technical ownership

---

## Story 3 — Quant / GPU / AI

```text
Python
    ↓
CuPy / GPU
    ↓
~100M trades
    ↓
High-performance computing
    ↓
TensorFlow
    ↓
AI / ML
```

### Interviewer muốn thấy

- Python
- Performance
- GPU
- ML / AI
- Advanced problem solving

### Cách định vị AI

> "My strongest background is production backend and distributed systems. I also have hands-on experience with ML, TensorFlow, GPU computing and AI-assisted engineering."

**Không claim mình đã xây large-scale agentic AI production nếu đó không phải kinh nghiệm thật.**

---

# PHẦN 29 — L6 CHEAT SHEET: 10 CÂU NÊN THUỘC PHẢN XẠ

Không học cả đoạn. Chỉ cần nói được câu đầu tiên rồi tự phát triển.

### Ownership

> "I owned the architecture and end-to-end delivery."

### Performance

> "I first investigated the request path and identified the actual bottleneck."

### Architecture

> "The decision depends on the access pattern and non-functional requirements."

### Scaling

> "I would prefer horizontal scaling and stateless services where possible."

### Reliability

> "I would design for retries, idempotency, timeouts and graceful failure."

### Security

> "Security should be part of the architecture, not an afterthought."

### Observability

> "I want metrics, logs and traces that allow us to understand both symptoms and root cause."

### Leadership

> "My goal is not just to solve the problem myself, but to make the team stronger."

### Client

> "I would first understand the business objective behind the requirement."

### AI

> "My strongest background is production backend and distributed systems, complemented by hands-on ML, GPU computing and AI-assisted engineering."

---

# PHẦN 30 — ANSWER CARD: CÁCH HỌC TỪNG CÂU

Với mỗi câu 🔥, tạo một card:

```text
QUESTION:
________________________________

KEY POINTS:
________________________________

MY REAL EXAMPLE:
________________________________

WHY THIS APPROACH:
________________________________

TRADE-OFF:
________________________________

RESULT / METRIC:
________________________________

WHAT CAN FAIL:
________________________________

FOLLOW-UP:
1. ______________________________
2. ______________________________
3. ______________________________
```

### Tiêu chuẩn đạt

Nhìn card **30 giây** → nói được **60–90 giây**.

Không nhìn card → vẫn nói được ý chính.

Interviewer hỏi follow-up → không bị mất phương hướng.

---

# PHẦN 31 — FOLLOW-UP ATTACK: L6 MODE

Sau **mọi** technical answer, tự hỏi:

```text
1. Why?
2. Why not another solution?
3. What can fail?
4. How do you measure it?
5. What happens at 10x scale?
6. What is the security risk?
7. What is the operational cost?
8. What did YOU personally implement?
```

## Ví dụ: "I use Redis for caching."

Interviewer có thể tiếp:

> Why Redis?

> What if Redis is down?

> How do you invalidate the cache?

> What if the cache stampedes?

> What data should not be cached?

> How do you measure cache effectiveness?

### Mục tiêu

Không trả lời:

> "Because Redis is fast."

Mà phải trả lời:

> "The access pattern is read-heavy, the data can tolerate some staleness, and Redis gives us low-latency access. The trade-off is cache invalidation and another dependency to operate."

---

# PHẦN 32 — ENGLISH: HỌC THEO CỤM, KHÔNG HỌC TỪ RỜI

## Khi chưa hiểu câu hỏi

> "Could you repeat the question, please?"

> "Could you say that in another way?"

> "Just to make sure I understand, are you asking about...?"

## Khi cần thời gian

> "That's a good question. Let me think for a moment."

> "Let me structure my answer."

> "There are two main points."

## Khi bắt đầu design

> "Let me clarify the requirements first."

> "Let me make a few assumptions about the scale."

## Khi nói trade-off

> "The main trade-off is..."

> "The reason I chose this approach is..."

> "The risk here is..."

## Khi không biết

> "I have not used that in production, but I know the basic idea. I would approach it this way..."

> "I am not sure about that detail. Let me reason through it."

## Khi interviewer phản biện

> "That's a good point."

> "Let me reconsider that."

> "In that case, I would change the design to..."

> **Không bịa kinh nghiệm.**

---

# PHẦN 33 — CODING: KHÔNG CHỈ CODE, PHẢI NÓI

## Trước khi code

> "Let me clarify the input and output first."

> "I will start with the simple approach."

> "The brute-force solution is O(n²)."

> "We can improve this using a hash map."

## Trong khi code

> "I keep this variable to track..."

> "The important edge case here is..."

## Sau khi code

> "Let me test it with a small example."

> "The time complexity is O(n), and the space complexity is O(n)."

## Khi bị stuck

> "I see one issue with my current approach. Let me step back and reconsider."

---

# PHẦN 34 — SYSTEM DESIGN: WHITEBOARD LOOP

Mỗi bài **không được đọc solution trước**.

## Round 1 — 5 phút

Chỉ làm:

- Requirements
- Assumptions
- Scale

Nói bằng tiếng Anh.

## Round 2 — 10 phút

Vẽ:

```text
Client
  ↓
API Gateway / Load Balancer
  ↓
Services
  ↓
Cache / Database
  ↓
Queue / Workers
```

Giải thích từng block.

## Round 3 — 10 phút

Tự đóng vai interviewer:

- What happens if Redis is down?
- What happens if traffic becomes 10x?
- What if a message is delivered twice?
- Where is the bottleneck?
- How do you secure this?
- How do you monitor it?

## Round 4 — 5 phút

Nói:

> "The main trade-offs are..."

và đưa ra 2–3 trade-offs.

> **Phần này quan trọng hơn việc đọc một architecture diagram đẹp.**

---

# PHẦN 35 — 6 CÂU SYSTEM DESIGN NÊN LUYỆN TRƯỚC

Không cần làm hàng chục bài.

### 1. Scalable API Platform

Luyện:

- stateless service
- load balancer
- Redis
- DynamoDB / SQL
- SQS
- scaling
- failure

### 2. Social Network / Facebook-like Page

Luyện:

- feed
- fan-out
- cache
- search
- media
- eventual consistency

> **Lưu ý:** tài liệu gốc coi đây là một hypothesis luyện tập, không phải đề OneByZero đã được xác nhận.

### 3. Event-driven Order Processing

Luyện:

- transactional outbox
- queue
- at-least-once
- idempotent consumer
- DLQ
- compensation

### 4. Enterprise AI Assistant

Luyện:

- RAG
- ACL filtering
- LLM
- tool gateway
- authorization
- prompt injection
- human approval
- tracing
- evaluation

### 5. Real-time Multiplayer Backend

Luyện:

- stateful nodes
- session affinity
- Redis
- shared state
- node failure
- horizontal scaling

### 6. Distributed Rate Limiter

Luyện:

- token bucket / sliding window
- Redis
- atomic operation
- Lua
- rate limit policy
- fail-open vs fail-closed

---

# PHẦN 36 — ENTERPRISE AI: 5 Ý PHẢI NÓI

Nếu gặp câu:

> "How would you design an enterprise AI assistant?"

Nhớ 5 ý:

### 1. LLM is NOT the security boundary

> "The model should never have unrestricted access. Each tool must enforce authorization itself."

### 2. Retrieval phải respect ACL

```text
User
 ↓
Authentication
 ↓
Retrieval
 ↓
ACL filtering
 ↓
Only authorized documents
 ↓
Prompt / LLM
```

### 3. Prompt injection

> "Retrieved content should be treated as untrusted data."

### 4. Tool actions

Sensitive actions cần:

- authorization
- idempotency
- human approval nếu cần
- audit

### 5. Evaluation

Đo:

- answer quality
- retrieval accuracy
- unauthorized access
- latency
- cost
- execution traces

---

# PHẦN 37 — CLIENT / FORWARD DEPLOYED

## Forward Deployed mindset

```text
Business problem
      ↓
Requirement discovery
      ↓
Technical solution
      ↓
Architecture
      ↓
Implementation
      ↓
Production
      ↓
Iteration with client
```

### Nếu client muốn giải pháp bạn cho là sai

Đừng nói:

> "Your solution is wrong."

Nói:

> "First, I would make sure I understand the business objective behind the request. Then I would explain the technical trade-offs and risks. If there are multiple viable options, I would present them with clear trade-offs in cost, complexity, scalability and delivery time."

### Công thức

> **Understand business goal → Explain trade-offs → Present options → Align → Document**

---

# PHẦN 38 — PRODUCTION INCIDENT: CÁCH TRẢ LỜI L6

Khi được hỏi:

> "Tell me about a production issue you solved."

Đi theo:

```text
1. Impact
2. Evidence
3. Investigation
4. Mitigation
5. Root cause
6. Fix
7. Prevention
```

### English skeleton

> "First, I looked at the impact: error rate, latency and affected users."

> "Then I checked recent changes and system health."

> "I traced the request path and used evidence to identify the root cause."

> "After stabilizing the service, I fixed the underlying problem."

> "Finally, I improved monitoring and prevention so the issue would be detected earlier."

---

# PHẦN 39 — FINAL STUDY MAP

## 🔴 MUST KNOW

```text
Self Introduction
        ↓
3 Core Stories
        ↓
System Design
        ↓
Distributed Systems
        ↓
Coding Patterns
        ↓
AWS
        ↓
AI / Enterprise AI
        ↓
Client / Leadership
```

## 🟡 MUST PRACTICE

```text
Explain
  ↓
Code / Design
  ↓
Follow-up
  ↓
Defend
```

## 🟢 NICE TO HAVE

- extra AWS services
- extra DSA problems
- advanced AI terminology
- less likely Python details

> **Đừng dành 80% thời gian cho 🟢 trong khi 🔴 chưa nói được.**

---

# PHẦN 40 — BẢNG "ĐÃ SẴN SÀNG CHƯA?"

| Chủ đề | Mức cần đạt |
|---|---:|
| Self introduction | 90% |
| 3 core stories | 80–90% |
| System Design | 70–80% |
| Coding patterns | 70% |
| Distributed Systems | 70% |
| AWS | 70% |
| AI / Client | 60–70% |
| English technical reasoning | Đủ để interviewer hiểu cách nghĩ |
| Follow-up | Không bị đứng hình |

### Không đo bằng số trang đã đọc.

Đo bằng:

> **"Tôi có thể nói, làm và defend hay chưa?"**

---

# PHẦN 41 — 30 PHÚT MỖI NGÀY

```text
10 phút  — English warm-up
20 phút  — Technical knowledge
20 phút  — 1 project story
30 phút  — Coding OR System Design
15 phút  — Follow-up
10 phút  — nghe lại recording
```

Nếu chỉ có **30 phút**:

```text
5 phút   — English phrases
10 phút  — 1 technical question
10 phút  — nói + follow-up
5 phút   — nghe lại
```

---

# PHẦN 42 — MOCK INTERVIEW VỚI CHATGPT

Không yêu cầu:

> "Cho tôi 100 câu hỏi."

Làm vòng lặp:

```text
Interviewer asks ONE question
          ↓
An answers in English
          ↓
Correct English
          ↓
Check technical accuracy
          ↓
Improve answer
          ↓
Follow-up question
          ↓
An answers again
          ↓
Score
          ↓
Next question
```

Nếu An không nói được tiếng Anh:

```text
An trả lời tiếng Việt
        ↓
Xác định technical intent
        ↓
Chuyển thành 3–6 câu English đơn giản
        ↓
An nói lại
        ↓
Follow-up
        ↓
Nói lại lần nữa
```

> **Mục tiêu cuối cùng: interviewer hiểu cách An suy nghĩ.**

---

# PHẦN 43 — NGUYÊN TẮC CUỐI CÙNG

Không cần chứng minh:

> "I know every technology."

Cần chứng minh:

> **"I can own a complex system, make sound decisions under constraints, operate it in production, lead others, and explain trade-offs to a client."**

Và với **mọi câu technical**, cố gắng đi theo:

```text
Requirement
    ↓
Options
    ↓
Trade-offs
    ↓
Decision
    ↓
Verification
    ↓
Production outcome
```

> **Technical thinking first. Simple English second.**
