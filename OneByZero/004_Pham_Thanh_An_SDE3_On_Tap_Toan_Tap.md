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
> My strongest skills are: Python, Java, and C#.
> I have deep experience in distributed systems, AWS, and performance optimization.
> In my current role, I own the architecture of backend platforms.
> These platforms serve more than 2 million users.
> I also lead technical work and mentor engineers.
> One example: I reduced API response time by three times.
> I also built a GPU backtesting system. It processes 100 million trades.
> And I built a TensorFlow model to predict gold prices.
> I am interested in OneByZero because it combines engineering, AWS, and AI.
> I believe I can contribute from day one.

💡 *Từ khóa để dựng lại: name → role → 15 years → Python/Java/C# → distributed/AWS → 2M users → lead & mentor → 3x → GPU → TensorFlow → why OneByZero.*

## 1.2. 🔥 Script 2 — Greatest ownership (WWE Champion)

> The project with my greatest ownership was a real-time gaming platform.
> It runs on AWS and serves more than 2 million users.
> Twenty to twenty-five thousand users are active every day.
> My job was the backend architecture and the whole delivery.
> We had a problem: API latency was high under load.
> I did not add more servers first.
> I profiled the request path. I found the real bottlenecks.
> Some work was blocking the request, but it was not necessary.
> Some data was loaded again and again on every request.
> So I made two changes.
> First, I moved non-blocking work to async processing.
> Second, I added local caching on each node.
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
