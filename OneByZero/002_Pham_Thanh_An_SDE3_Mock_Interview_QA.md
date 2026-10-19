# MOCK INTERVIEW PRACTICE — OneByZero SDE III (L6)
**Candidate:** Pham Thanh An | **Prepared:** 2026-10-19 | **Round:** Technical / Hiring-manager round (post-headhunter)

## How to use this file
- Practice OUT LOUD in English. Reading silently creates false confidence.
- Legend: 🔥 = must master (answer without notes) | ⭐ = high probability | 💡 = differentiator question
- For every answer, self-score with the rubric at the end. Target: 3–4 on all 🔥 questions.
- **Honesty rule:** never invent metrics you cannot back. If asked "what was p95 before/after?", use real numbers only.

---

# PART 1 — CORE ENGLISH Q&A

## 🔥 Q1. Tell me about yourself.
**Why they ask:** map your level in 90 seconds; check communication.

**Model answer (90s):**
> My name is Pham Thanh An. I'm a Senior Software Engineer and Solution Architect with more than 15 years of experience in backend development, system architecture, and production systems.
>
> My main technical strengths are Python, Java, and C#, with extensive experience in distributed systems, microservices, event-driven architecture, AWS, CI/CD, observability, and performance optimization.
>
> In my current role, I've owned the architecture and delivery of backend platforms serving more than 2 million users. I've led technical workstreams, conducted design reviews, mentored engineers, and resolved complex production performance issues — for example, reducing API response time by up to 3x.
>
> In parallel, I've built a GPU-accelerated backtesting framework processing around 100 million trades, and a TensorFlow-based gold-price prediction model.
>
> What attracts me to OneByZero is the combination of production engineering, AWS, enterprise-scale systems, and AI — and I believe my background lets me contribute from day one.

**L6 upgrade tip:** end with *why here*, not just *what I did*.

## 🔥 Q2. Tell me about a project where you had the greatest ownership.
**Why they ask:** SDE III = owns systems, not tasks.

**Model answer (STAR, ~2.5 min):**
> The project with my greatest ownership was a real-time multiplayer platform on AWS serving more than 2 million registered users, with 20–25K daily active users.
>
> **Situation:** The platform needed to run 24/7 with low-latency synchronous PvP gameplay, and several APIs were degrading under concurrent load.
> **Task:** I was responsible for backend architecture, technical decisions, implementation, deployment, and production support — end to end.
> **Action:** First I profiled the request path instead of adding hardware. I found non-blocking work executing synchronously and repeated data retrieval on every request. I moved the non-blocking work to ExecutorService-based asynchronous processing, and introduced per-node caching for frequently accessed configuration data. Separately, I designed the DynamoDB access patterns around our actual query patterns.
> **Result:** API response time improved up to 3x on the affected endpoints, and the platform has been operating reliably in production since.
>
> The key lesson: ownership means being accountable for the full lifecycle — design, trade-offs, delivery, and measurable production outcomes.

**Follow-up chain (expect ALL of these):**
1. *What exactly did you implement yourself vs the team?* → Be precise. You wrote the core server logic and the performance changes; be honest about team size (~5).
2. *How did you measure the 3x?* → "We compared average response time before and after under similar load; some endpoints went from roughly X ms to X/3." (fill in your REAL numbers)
3. *What were the risks of per-node caching?* → stale data, cache invalidation, memory, inconsistency between nodes.
4. *What would you do differently today?* → "I'd add cache TTL + invalidation events, and consider a shared cache for consistency-critical data — per-node cache was the right trade-off at that scale."

## 🔥 Q3. How did you improve API performance by three times?
**Why they ask:** this is your flagship story — they will drill it.

**Model answer:**
> I started by establishing where time was actually spent — profiling the request flow and measuring latency per stage, rather than guessing.
>
> I found two main problems: work that did not need to block the response was executing synchronously, and some data was being repeatedly retrieved and processed on every request.
>
> I addressed the first with asynchronous processing via ExecutorService, and the second with per-node caching of frequently accessed data.
>
> The result was up to a 3x improvement in API response time.
>
> Today I would formalize this: define baseline metrics first, track p50/p95/p99, trace external calls, validate each change with load testing, and confirm through production monitoring that the improvement is real and doesn't break consistency.

**Follow-up chain:**
1. *Why not just scale out more instances?* → cheapest resource is the work you eliminate; scaling multiplies cost AND the bottleneck.
2. *Stale cache risk?* → TTL, invalidation on update, acceptable staleness analyzed per data type.
3. *How do you know it was causation, not correlation?* → controlled rollout, before/after metrics at similar traffic, no other deploys in window.

## 🔥 Q4. Tell me about a difficult production incident you resolved.
**Model answer (use the Mnews log-analysis story or a game outage):**
> In an earlier telecom project, an MMS news service at a major carrier was unstable — intermittent failures with no clear pattern.
>
> Instead of restarting services blindly, I wrote a log-analysis tool to aggregate and correlate failure events across time windows and components.
>
> The analysis revealed the root cause pattern [state yours: e.g., resource exhaustion at peak hours / a specific component failing under a condition].
>
> I proposed and implemented a fix, and the service stabilized. After that I improved monitoring so the symptom would be caught earlier next time.
>
> My approach to any incident: stabilize first with safe mitigation, then evidence-based root cause, then fix the detection gap — not just the bug.

**Follow-up:** *What if rollback isn't possible?* → feature flags, config kill-switches, graceful degradation, capacity mitigation; then root cause.

## 🔥 Q5. How do you approach system design?
**Why they ask:** tests your thinking process, not memorized architectures.

**Model answer:**
> I start by clarifying functional and non-functional requirements: users, use cases, traffic, read/write ratio, latency targets, availability, consistency, security and compliance constraints.
>
> Then I estimate scale — requests per second, concurrent users, data volume, storage growth — and state my assumptions explicitly.
>
> Only then do I design: high-level components, API surface, data model, storage choices driven by access patterns, caching where it earns its complexity, and asynchronous processing for non-blocking work.
>
> After that I work through failure handling — timeouts, retries with backoff, idempotency, circuit breakers — then security, observability, and deployment strategy.
>
> My goal is the simplest architecture that meets the requirements and can evolve — not the most sophisticated one.

## ⭐ Q6. What is your experience with AWS?
**Model answer (say ONLY what you can defend):**
> I've designed and operated production systems on AWS at multi-million-user scale: EC2-based fleets behind load balancers, DynamoDB as the primary NoSQL store, ElastiCache/Redis for caching, S3, IAM with least-privilege roles.
>
> I've also built the surrounding production discipline: CI/CD with Jenkins and GitHub Actions, observability through custom Grafana plugins and monitoring plugins, and security hardening including pentest-driven remediation.
>
> When I choose AWS services I start from access patterns and non-functional requirements, not from what's fashionable. I've also worked on Azure — Cosmos DB and Service Bus — which helps me compare trade-offs honestly.
>
> On certifications: I don't hold the AWS cert yet, but I've been running production workloads on AWS for nearly a decade. I'm planning to take the Solutions Architect Associate exam.

**(If pressed on the cert):** "Production ownership has taught me the same trade-offs the cert covers — but I agree the cert is a useful formal validation, which is why I'm pursuing it."

## 💡 Q7. Tell me about the observability tooling you built. (Grafana plugin)
**Why they ask:** JD explicitly lists observability — this is your rare, concrete proof.

**Model answer:**
> For a game backend serving around 1 million users, the operations team lacked visibility into the stateful game server nodes.
>
> I developed a custom Grafana plugin to monitor production game servers in real time — exposing key server metrics into Grafana dashboards so the team could see health and performance at a glance.
>
> Earlier in my career I also built monitoring and alerting plugins on RHQ and Nagios, and log-analysis tooling that diagnosed an unstable carrier service.
>
> The principle behind all of this: metrics, logs and traces must let you see both symptoms and root cause. Building the tooling myself taught me exactly what good observability looks like from the operator's side.

**Follow-up chain:**
1. *What metrics did you expose?* → (prepare real answers: CPU, memory, connection counts, per-node player load, message rates, GC pauses — whichever applies)
2. *What language is a Grafana plugin written in?* → TypeScript/React frontend panel + Go backend for data collection.
3. *How did you alert?* → threshold alerts on the metrics above; routed to on-call.

## ⭐ Q8. How do you mentor engineers?
**Model answer:**
> I focus on reasoning, not answers. In design discussions and code reviews I ask questions: what happens under load, what fails first, what's the security implication, what are the alternatives?
>
> For complex tasks I help decompose the problem but let the engineer own the implementation, then review the decision-making, not just the code.
>
> My goal is engineers who make sound technical decisions independently — not engineers who depend on me.

**Follow-up:** *An engineer repeatedly introduces production issues — what do you do?* → pair on root cause, tighten review gates for risky areas, add tests/monitoring, give direct feedback; if it continues, escalate honestly — production safety comes first.

## ⭐ Q9. Why OneByZero? Why SDE III?
**Model answer:**
> OneByZero sits at the intersection of things I know deeply — production backend engineering, AWS, distributed systems — and things I want to go deeper on: enterprise-scale agentic AI.
>
> What I've read about the role matches how I already work: owning large systems end-to-end, driving architecture and delivery, mentoring, and working directly with enterprise clients. I've done client-facing enterprise delivery with telecom carriers, so the forward-deployed model is familiar territory.
>
> SDE III is the right level because I already operate at system-ownership scope — what I'd gain here is depth in AI systems and a larger enterprise context.

## 🔥 Q10. What is your experience with AI / agentic AI?
**Why they ask:** OneByZero builds agentic AI — they need honest capability assessment, not buzzwords.

**Model answer (honest positioning):**
> My direct AI experience is hands-on but different in shape from a pure ML role: I've built a TensorFlow gold-price prediction model integrated into a live trading bot, and a GPU-accelerated backtesting framework using CuPy and NumPy that processes around 100 million trades in parallel.
>
> My core strength is production engineering — distributed systems, AWS, reliability, security. I see agentic AI as precisely the area where those fundamentals matter most: an enterprise AI agent is a distributed system with an LLM inside, and it fails in all the classical ways — retries, duplicates, timeouts, unauthorized access — plus new ones like hallucination and prompt injection.
>
> So I'd contribute immediately on architecture, reliability and security of AI systems, while deepening my agentic-AI practice on the job.

**Follow-up chain:**
1. *What is an AI agent?* → a system where a model reasons over a task, selects tools, executes actions, observes results, and iterates toward a goal.
2. *How would you design an enterprise AI assistant?* → see Part 2, Drill 2.
3. *Key risks in regulated industries?* → data leakage, over-permissive tools, hallucination, audit gaps, cost/latency; mitigations: ACL-filtered retrieval, scoped tool permissions, human-in-the-loop approvals, full execution traces, evaluation suites.

## ⭐ Q11. How do you use AI coding tools in your daily work?
**Why they ask:** JD explicitly expects AI-tool fluency.

**Model answer:**
> I use AI tools as a productivity multiplier, not a substitute for engineering judgment: code exploration in unfamiliar codebases, generating test cases, brainstorming debugging hypotheses, documentation drafts, and repetitive boilerplate.
>
> I always validate the output — tests, code review, security checks — because I own the production outcome, not the tool. For critical paths I treat AI output as a draft to be verified, never as code to be trusted.

## 💡 Q12. How would you design distributed rate limiting?
**Why they ask:** bridges your Redis/RedLock experience with system design — likely at L6.

**Model answer:**
> Single instance first: token bucket or sliding window log per user in memory — simple and exact enough.
>
> Distributed: the hard part is atomicity across nodes. I'd store counters in Redis with a Lua script to make "check + increment" atomic — avoiding race conditions where two concurrent requests both pass.
>
> For strictness, a sliding window log (Redis sorted set per user, drop expired entries) is accurate; token bucket is cheaper and usually sufficient.
>
> I implemented distributed locking and rate limiting with RedLock on Redis in a previous project, so I know the failure modes: Redis latency counts against your budget, clock drift matters, and any Redis outage means choosing fail-open (availability) or fail-closed (protection) — that choice must be explicit per endpoint.
>
> I'd also put rate limits at the gateway (API Gateway / load balancer) for cheap enforcement, and per-service limits for protection in depth.

---

# PART 2 — SYSTEM DESIGN DRILLS

## Drill 1 🔥 — Design an API platform for 2M users
**Opening move (say this verbatim before drawing anything):**
> Before choosing architecture, I'd clarify: are these 2M registered or concurrent users? What's the peak RPS, read/write ratio, latency target, availability target, and consistency requirements? Two million registered users may mean only tens of thousands of concurrent users.

**Capacity math example:** 2M registered, 25K DAU → ~2K concurrent peak → ~2–5K RPS peak. State assumptions explicitly.

**Architecture skeleton:**
```
Client → CDN → Load Balancer → Stateless API services (autoscaling group)
                                  ├── Redis (cache / rate limits / locks)
                                  ├── DynamoDB (primary store — access-pattern driven)
                                  ├── SQS / event bus → async workers (non-blocking work)
                                  └── S3 (static / exports)
Observability: metrics, logs, traces, alerts | CI/CD: GitHub Actions, blue/green
```

**Deep-dive prompts & short answers:**
- *Why DynamoDB?* → key-value access patterns at this scale, predictable single-digit-ms latency, horizontal scaling without shards management; costs + query flexibility trade-off accepted.
- *Where's the cache?* → cache-aside for read-heavy config/profile data; TTL + invalidation; never cache what must be strongly consistent.
- *What breaks first at 10x?* → hottest partitions in DynamoDB, fan-out on async workers; mitigation: partition key design, backpressure, autoscaling.
- *How do you deploy safely?* → canary/blue-green, automated rollback triggers on error-rate/latency SLO burn.

## Drill 2 🔥 — Design an enterprise AI assistant (agentic)
**Architecture skeleton:**
```
Employee App → API Gateway + Identity/Auth → Agent Orchestrator
   (context, model calls, workflow state, limits, guardrails)
      ├── Knowledge Retrieval (ingestion pipeline, chunking, embedding,
      │    ACL-filtered search — retrieval respects document permissions)
      ├── Controlled Tools (business APIs; each enforces its own authz,
      │    input validation; sensitive actions → human approval step)
      └── Observability & Evaluation (traces of every agent step, quality
           evals, latency, cost, security events, audit log)
```

**Must-say points (this is what separates L6 from junior):**
- The model never gets unrestricted access to enterprise systems; **each tool validates its own authorization** — the LLM is not the security boundary.
- Retrieval must filter by the **caller's** ACL before context reaches the prompt (prevents data leakage via prompt).
- **Human-in-the-loop** approval for irreversible/sensitive actions; **idempotency keys** so a retried tool call can't execute twice.
- **Prompt injection** defense: treat retrieved documents as untrusted data, least-privilege tools, output validation, never execute instructions from retrieved content.
- Evaluation: golden test set, measure answer quality/retrieval accuracy/unauthorized-access attempts; full execution traces for debugging multi-step failures.
- Failure design: model-provider timeouts, fallback models, safe recovery from partially completed workflows, cost and latency budgets per request.

**Deep-dive:** *What if the agent executes the same action twice?* → idempotency keys recorded transactionally with the business effect; dedup at the tool layer; retry-safe design.

## Drill 3 ⭐ — Event-driven payment/order processing
**Why practice this:** maps directly to your Service Bus experience.

**Key beats:** API accepts order → publishes event → idempotent consumer processes → transactional outbox if DB+queue must be atomic → DLQ for poison messages → reconciliation job for drift.

**Deep-dive answers:**
- *Exactly-once?* → "Exactly-once delivery is a marketing phrase; I design for at-least-once delivery plus idempotent consumers — the effect is exactly-once."
- *Outbox pattern* → write business state + event in the same DB transaction; relay publishes to the bus.

---

# PART 3 — CODING DRILLS

**Rules:** think aloud → state brute force → optimize → state complexity BEFORE coding → test edge cases after.

## Drill 1 — Longest Substring Without Repeating Characters
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
- Time O(n), Space O(k).
- Follow-up: *why better than checking all substrings?* → sliding window visits each char once; brute force is O(n²) or worse.

## Drill 2 — Top K Frequent Elements
```python
from collections import Counter
import heapq

def top_k_frequent(nums, k):
    counts = Counter(nums)                      # O(n)
    return [v for v, _ in heapq.nlargest(k, counts.items(),
                                          key=lambda x: x[1])]  # O(m log k)
```
- Time O(n + m log k); mention bucket-sort O(n) alternative as a bonus.

## Drill 3 — Merge Intervals
```python
def merge_intervals(intervals):
    if not intervals:
        return []
    intervals.sort(key=lambda i: i[0])          # O(n log n)
    merged = [list(intervals[0])]
    for start, end in intervals[1:]:
        if start <= merged[-1][1]:
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])
    return merged
```
- Edge cases: empty input, single interval, fully contained interval, unsorted input.

## Drill 4 — LRU Cache (O(1) get/put)
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.store = OrderedDict()

    def get(self, key: int) -> int:
        if key not in self.store:
            return -1
        self.store.move_to_end(key)
        return self.store[key]

    def put(self, key: int, value: int) -> None:
        if key in self.store:
            self.store.move_to_end(key)
        self.store[key] = value
        if len(self.store) > self.cap:
            self.store.popitem(last=False)
```
- **Say this:** "OrderedDict is the interview-practical version; the underlying structure is a hash map for O(1) lookup plus a doubly linked list for O(1) eviction — I can implement that from scratch if you'd like."
- Follow-ups: TTL eviction, concurrency (sharded locks), memory bound.

## Drill 5 — Rate Limiter (100 req / 60s per user) 💡
**Think aloud BEFORE coding:**
1. Key by user ID.
2. Sliding window log: Redis sorted set, score = timestamp; on each request remove entries older than 60s (ZREMRANGEBYSCORE), count (ZCARD); if count >= 100 reject, else ZADD + EXPIRE and allow.
3. **Concurrency:** wrap check-and-increment in a Redis **Lua script** (atomic) — two simultaneous requests can't both pass.
4. Multi-instance: all nodes share Redis → consistent global limit.
5. Trade-offs: sliding window log is accurate but memory-heavy at scale; token bucket is cheaper; state per user matters.

```python
# Sketch of the Lua-script approach (single atomic step in Redis):
#   KEYS[1] = rate key, ARGV = [now, window_ms, limit, member]
#   1. ZREMRANGEBYSCORE key 0 (now-window)
#   2. if ZCARD key < limit → ZADD key now member; return 1 else return 0
```

---

# PART 4 — DISTRIBUTED SYSTEMS HARD QUESTIONS (short Q&A)

**Q: What is idempotency and why does it matter?**
An operation has the same intended effect whether executed once or many times. It matters because retries after timeouts can duplicate an operation that actually succeeded. Pattern: client sends an idempotency key; server stores the result keyed by it; a retry returns the stored result instead of re-executing. Must handle concurrent retries with the same key and define key expiry.

**Q: How do you handle retries safely?**
Retry only transient failures; set explicit timeouts and attempt limits; exponential backoff with jitter; never retry non-idempotent operations without an idempotency mechanism; respect the overall request deadline; for persistent failures use circuit breaker, DLQ, or a clear error.

**Q: Explain eventual consistency. When is it OK?**
Replicas may diverge temporarily but converge if updates stop. Fine for caches, async projections, analytics, activity feeds. Not fine for invariants like "one payment, one debit" — enforce strong consistency at the invariant's boundary.

**Q: How do you prevent duplicate event processing?**
Assume at-least-once delivery. Stable event ID + idempotent consumer: record processed IDs in the same transaction as the business update. Handle ordering, poison messages (DLQ), and consumer crash recovery. Never claim exactly-once from a queue feature alone — the guarantee covers the whole workflow including side effects.

**Q: How does a distributed lock work, and when is it dangerous?**
(Your RedLock experience — own this.) A lock is acquired by writing a key with a TTL via an atomic operation (SET NX PX in Redis); the holder must renew or release before expiry. Danger: GC pause or network delay can let the lock expire while you still hold it → two holders. Mitigations: fencing tokens (monotonic IDs checked by the resource), short critical sections, TTL well above worst-case pause. RedLock debates this trade-off — knowing the controversy is a plus.

**Q: CAP theorem — practical version?**
Under a network partition you choose consistency (refuse/stall some requests) or availability (serve possibly stale data). In practice: design for partition tolerance and choose per-operation — payments choose C, feeds choose A.

---

# PART 5 — AWS / SECURITY / DEVSECOPS Q&A

**Q: How would you secure an AWS production system?**
Least-privilege IAM, roles over users, short-lived credentials; network isolation with public/private subnets and security groups as stateful firewalls; encryption in transit and at rest; secrets in a manager, never in code or logs; input validation and authn/authz at the application layer; rate limiting at the gateway; dependency/security scanning in CI/CD; centralized audit logging and alerting. Security is part of the architecture, not an afterthought.

**Q: DynamoDB vs relational DB?**
DynamoDB when access patterns are known, key-based, high scale, predictable latency; you give up ad-hoc queries and pay for RCU/WCU. SQL when you need flexible queries, joins, strong transactional relationships. I've used DynamoDB for game backends at millions of users — it worked because we designed partition keys around real access patterns. Hot partitions are the classic failure — watch for skewed key distribution.

**Q: How do you investigate a slow database query?**
Find the actual slow queries from metrics, not guesses → examine execution plan → check indexes, selectivity, join patterns, lock contention, connection pool saturation, and queries-per-request. Fix one thing, measure under representative load. Avoid blind indexing and blind caching — both carry costs.

**Q: What does a good CI/CD pipeline include?**
Repeatable builds; unit + integration tests; static analysis and dependency/security scanning; infrastructure-as-code validation; controlled deployment with canary or blue/green; automated smoke tests post-deploy; monitored rollout with automatic rollback triggers; clear ownership when a release goes wrong.

**Q: Tell me about your security work. (TheWinZone pentest story)**
> I led backend security hardening on a 750K-user platform: researched and remediated vulnerabilities found in pentesting, added automatic input sanitization at the model-binding layer (HtmlSanitizer), and implemented distributed locking and rate limiting with RedLock on Redis. My principle: sanitize and authorize at every boundary, and make the secure path the default path so developers can't forget it.

---

# PART 6 — LEADERSHIP & CLIENT-FACING Q&A

**Q: Tell me about a technical disagreement.**
I first make sure I understand the requirement and the reasoning behind the other proposal. Then I compare options on explicit criteria — reliability, performance, security, delivery time, operational complexity — and use data or a small prototype when assumptions differ. Once decided, I commit and help execute, and I revisit if the assumptions change. The goal is the best decision for the project, not being right. *(Prepare one REAL example from your work.)*

**Q: A client insists on a solution you believe is technically wrong.**
First understand the business objective behind the request — often the "wrong" solution solves a real constraint I didn't know. Then explain trade-offs and risks concretely (cost, scalability, delivery time), and present viable alternatives with clear trade-offs. If they still choose it after understanding the risks, I document the decision and assumptions, and build in safeguards where possible.

**Q: How do you handle changing client requirements?**
Clarify the business outcome driving the change; assess impact on architecture, scope, and risk; present options in language both technical and non-technical stakeholders understand; agree priorities, document, update the plan. For complex systems, keep seams in the architecture so future change doesn't force rework.

**Q: What does "Forward Deployed" mean to you?**
An engineer embedded close to the customer: discovering the real business problem, translating it into technical design, shipping it into the customer's environment, and iterating with them in production. It demands engineering depth plus communication and ambiguity tolerance — which matches my telecom enterprise delivery experience with Viettel and Vinaphone.

**Q: Why are you leaving your current role?**
Positive framing only: > "I've been at my current company for nearly a decade and built great systems there. I'm looking for a larger enterprise context and to go deeper into AI systems — this role is that step, not an escape from anything."

**Q: Compensation expectations?**
> "Based on the scope of SDE III — system ownership, architecture, client-facing responsibility — and my experience, I'm looking at around 110 million VND gross per month, depending on the overall package. I'm open to discussion." *(Adjust the number to your real target.)*

---

# PART 7 — SELF-SCORING RUBRIC & MOCK LOOP

## Rubric (apply after EVERY practice answer)
- **0** — Cannot answer.
- **1** — Generic, no example, no structure.
- **2** — Technically correct but no trade-offs or results.
- **3** — Real example, explains decision + risks + outcome.
- **4** — Adds metrics, verification method, lessons, and how it transfers to a new context.

Target: **3+ on all 🔥 questions before the interview.**

## 60-minute mock loop (do at least 3 times before the real interview)
| Time | Exercise |
|---|---|
| 5 min | Self-intro (Q1) — out loud, no notes |
| 10 min | Q2 + Q3 with full follow-up chain (have a friend or timer drill you) |
| 15 min | System design whiteboard: Drill 1 or 2, timed |
| 15 min | Coding: Drill 4 or 5, thinking aloud |
| 10 min | Q10 + Q12 (AI) + one Part 5 question |
| 5 min | Your questions for them (see below) |

## Your questions to ask them (pick 2–3)
- "What would you expect an SDE III to accomplish in the first six months?"
- "How much direct client interaction does an SDE III typically have, and how often is travel involved?"
- "What distinguishes someone who succeeds at L6 here from someone technically strong but not yet at that level?" *(best question — reveals their real bar)*

## The L6 mindset (read before every practice session)
Don't prove "I know every technology." Prove: **"I can own a complex system, make sound architectural decisions under constraints, operate it in production, lead others through the problem, and explain trade-offs to a client."** Every answer should show: requirement → options → trade-offs → decision → verification → production outcome.
