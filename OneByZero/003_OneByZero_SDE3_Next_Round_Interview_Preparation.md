# OneByZero SDE III (L6) — Next Round Interview Preparation

## Current status

Recruiter confirmed:

> "Your profile has been shortlisted :) I'm waiting for the slots from them. Will share as soon as I have them"

This means you passed the headhunter screening and your profile has been shortlisted for the next stage. The interview format (online/video call vs onsite) has not yet been confirmed.

Suggested message to recruiter:

> Hi Abhi,
>
> Thanks for the update, that's great news!
>
> Just one quick question — do you know whether the next interview will be conducted online via video call or onsite?
>
> Thanks!

---

# 1. Priority for the next round

Prepare in this order:

1. **System Design / HLD**
2. **Coding / Data Structures & Algorithms**
3. **Deep dive into your CV and production experience**
4. **Distributed systems**
5. **AWS / security / reliability**
6. **Leadership / ownership / client-facing**
7. **AI / enterprise AI / agentic AI**

For SDE III, do not only demonstrate that you can code. Demonstrate:

> **Ownership + Architecture + Production + Technical Leadership**

---

# 2. Your core interview positioning

> I am a senior backend engineer and solution architect with more than 15 years of experience. My strongest areas are distributed systems, backend architecture, production systems, performance optimization, AWS, and technical leadership.
>
> I've worked on systems serving more than 2 million users, improved API performance by up to three times, designed event-driven architectures using asynchronous processing, worked on security and observability, and led technical delivery.
>
> I also have hands-on experience with quantitative trading and AI-related projects, including GPU-accelerated backtesting and TensorFlow-based prediction.
>
> I see OneByZero as a strong match because it combines enterprise engineering, AWS, and AI.

---

# 3. Five stories you must be ready to tell

## Story A — 2M+ user production platform

Be ready to explain:

- Architecture
- Traffic/workload
- Backend services
- Database
- Cache
- AWS
- Scaling
- Monitoring
- Failure handling
- Your exact responsibilities
- What you personally implemented
- What decisions you owned

Opening:

> One of the largest systems I worked on was a real-time platform serving more than two million users.

## Story B — 3x API performance improvement

Structure:

1. Problem
2. Measurement
3. Root cause
4. Technical change
5. Validation
6. Result
7. Trade-offs

Sample:

> We had an API latency problem under concurrent workloads.
>
> I first investigated the request flow and identified blocking operations and repeated data access.
>
> We introduced asynchronous processing for suitable operations and caching at the appropriate level.
>
> These changes reduced API response time by up to three times.
>
> I validated the improvement through performance measurements and production monitoring.

Be ready for:

- What was the original latency?
- What was the final latency?
- p50 / p95 / p99?
- What exactly was blocking?
- Why async?
- Why cache?
- Where did you cache?
- What consistency problem did caching introduce?
- How did you measure improvement?
- What would you do differently today?

**Never invent metrics. Use your real numbers if asked.**

## Story C — Service Bus / Event-driven architecture

Core model:

```text
Producer
   |
   | message/event
   v
+----------------+
| Message Queue  |
| / Service Bus  |
+----------------+
        |
        v
   Consumers
   /   |   \
Worker Worker Worker
```

Explain:

> Instead of having the producer synchronously wait for another service to complete the work, we send a message to a queue. Consumers process the messages asynchronously.

Benefits:

- Decoupling
- Traffic buffering
- Horizontal scaling
- Better responsiveness
- Failure isolation
- Retry
- Background processing

Be ready for:

- Duplicate messages
- Idempotency
- Retry
- Dead-letter queue
- Ordering
- Poison messages
- Consumer crash
- Queue backlog
- Monitoring queue depth
- Scaling consumers

## Story D — Security / pentest / rate limiting

Be ready to explain:

- Vulnerability discovered
- Root cause
- Remediation
- Input validation
- Sanitization
- Authentication / authorization
- Rate limiting
- Distributed locking
- Secure API design
- Testing

Do not simply say "We fixed security issues." Explain:

> what the threat was → how it could be exploited → how you mitigated it → how you verified the fix.

## Story E — AI / Quant / GPU

Your strongest AI-related evidence:

- GPU-accelerated backtesting
- Python
- CuPy
- NumPy
- Pandas
- ~100M trades over 10 years in parallel
- TensorFlow
- Gold-price prediction
- Integration into trading bot

Sample:

> My strongest AI-related experience is currently in quantitative and applied AI systems rather than large-scale agentic AI production.
>
> I built GPU-accelerated backtesting pipelines using Python, CuPy, NumPy and Pandas, processing around 100 million trades over a ten-year period in parallel.
>
> I also developed a TensorFlow-based gold-price prediction model and integrated it into a live trading workflow.
>
> My strongest production background remains backend architecture and distributed systems, which I believe is highly relevant to enterprise AI systems.

---

# 4. System Design framework

When given a design problem:

### Step 1 — Clarify requirements

Ask:

- Who are the users?
- What are the main operations?
- Read-heavy or write-heavy?
- Peak traffic?
- Latency target?
- Availability requirement?
- Consistency requirement?
- Data retention?
- Security constraints?

Say:

> Before designing the architecture, I'd like to clarify the functional and non-functional requirements.

### Step 2 — Estimate scale

Think about:

- Users
- DAU/MAU
- Requests per second
- Peak traffic
- Data size
- Storage growth
- Bandwidth

Example:

> Two million registered users does not necessarily mean two million concurrent users, so I'd first estimate the actual concurrency and peak request rate.

### Step 3 — High-Level Architecture

```text
Client
   |
   v
Load Balancer / API Gateway
   |
   v
Stateless API Services
   |
   +------------+
   |            |
   v            v
Cache        Database
   |
   |
   +-------> Message Queue
                  |
                  v
              Workers
```

### Step 4 — Data

Discuss:

- SQL vs NoSQL
- Access patterns
- Indexes
- Partitioning
- Replication
- Consistency
- Transactions
- Data lifecycle

Say:

> I'd choose the data store based on access patterns, consistency requirements, transaction needs, and expected scale.

### Step 5 — Async processing

Use a queue when:

- Work is slow
- Work does not need immediate response
- Traffic is bursty
- Producer and consumer should be decoupled
- Consumers need independent scaling

### Step 6 — Cache

Discuss:

- What should be cached?
- Cache key
- TTL
- Invalidation
- Stale data
- Cache stampede
- Cache availability

Important:

> Caching improves performance but introduces consistency and invalidation problems.

### Step 7 — Failure handling

Always discuss:

- Timeout
- Retry
- Exponential backoff
- Jitter
- Circuit breaker
- Idempotency
- Dead-letter queue
- Graceful degradation

### Step 8 — Observability

Mention:

- Metrics
- Logs
- Traces
- Alerts
- Error rate
- Latency
- Throughput
- Queue depth
- Resource utilization

For APIs:

> I would monitor p50, p95 and p99 latency rather than relying only on average latency.

### Step 9 — Security

Discuss:

- Authentication
- Authorization
- IAM
- Least privilege
- Encryption
- Secrets management
- Input validation
- Rate limiting
- Audit logs
- Network boundaries

---

# 5. System Design Test — 2M+ users

> Design a backend platform supporting two million users and tens of thousands of daily active users.

Start with:

> I'd first clarify whether the two million users are registered users or concurrent users, because that has a major impact on capacity planning.

Then cover:

```text
Clients
   |
API Gateway / Load Balancer
   |
Stateless API Services
   |
+---------+----------+
|         |          |
Cache   Database   Queue
                    |
              +-----+-----+
              |     |     |
            Worker Worker Worker
```

Discuss:

- Horizontal scaling
- Database scaling
- Caching
- Async processing
- Failure handling
- Monitoring
- Security

---

# 6. System Design Test — Event-driven order processing

Requirements:

- User creates order
- Payment
- Inventory
- Notification
- Order status
- High traffic
- Reliability

Possible architecture:

```text
                +----------------+
User ---------->| Order API      |
                +-------+--------+
                        |
                        v
                  Order Database
                        |
                        v
                OrderCreated Event
                        |
                        v
                  Message Bus
                  /    |     \
                 /     |      \
                v      v       v
           Payment  Inventory Notification
             |         |          |
             v         v          v
          Payment DB Inventory   Email/SMS
```

If payment fails:

> Retry transient failures. If the operation cannot succeed after bounded retries, move the message to a dead-letter queue and trigger appropriate recovery or compensation.

If Payment receives the same event twice:

> The payment operation must be idempotent. I would use a stable idempotency key or event ID and ensure concurrent duplicate requests cannot create duplicate side effects.

---

# 7. System Design Test — Real-time multiplayer backend

Relevant to your experience.

Discuss:

- Authentication
- Matchmaking
- Game/session state
- Real-time communication
- Low latency
- State synchronization
- Redis/cache
- Database persistence
- Horizontal scaling
- Player/session affinity if required
- Monitoring

Possible high-level:

```text
Clients
  |
  v
Load Balancer
  |
  +----------+----------+
  |          |          |
Game Node  Game Node  Game Node
  |          |          |
  +----------+----------+
             |
           Redis
             |
          Database
```

Be ready for:

- How do you handle a game server failure?
- How do you prevent stale state?
- How do you scale when player activity suddenly increases?

---

# 8. System Design Test — Enterprise AI assistant

Prompt:

> Design an AI assistant for an enterprise with one million employees. The assistant can answer questions about internal documents and perform approved actions in enterprise systems.

Architecture:

```text
User
 |
v
API Gateway
 |
v
Authentication / Authorization
 |
v
Agent Orchestrator
 |
+-------------------+
|                   |
v                   v
Knowledge Retrieval  LLM
|                   |
v                   v
Document Store      Tool Selection
|                   |
ACL filtering       v
                 Tool Gateway
                     |
          +----------+----------+
          |          |          |
        CRM        HR         Finance
```

Must discuss:

### Security

The model must NOT have unrestricted access.

> Each tool should enforce authorization independently.

### Document access

Do not retrieve documents the user is not allowed to access.

### Prompt injection

Treat retrieved content and user-provided content as untrusted input.

### Tool execution

Use:

- Allowlisted tools
- Strong authorization
- Input validation
- Audit logs
- Rate limits
- Approval for sensitive operations

### Observability

Trace:

```text
User request
   |
Agent
   |
Retrieval
   |
Model call
   |
Tool call
   |
Tool result
   |
Final answer
```

---

# 9. Distributed Systems Q&A

## What is idempotency?

> An operation is idempotent if executing it multiple times produces the same intended effect as executing it once.
>
> This is important because distributed systems may retry requests after timeouts.
>
> For example, a payment API can accept an idempotency key and associate the resulting operation with that key. A retry with the same key can return the previous result rather than creating a duplicate payment.

## How do you handle duplicate messages?

> I assume messages may be delivered more than once unless the complete system provides a stronger guarantee.
>
> I use a stable event ID and make consumers idempotent. Depending on the storage model, I may record processed event IDs together with the business update.
>
> I also consider concurrency, ordering, retries and dead-letter handling.

## How do retries work?

> I retry transient failures with bounded retries, exponential backoff and jitter.
>
> I also consider request deadlines and idempotency.
>
> Retrying a non-idempotent operation without protection can create duplicate side effects.

## What is eventual consistency?

> Eventual consistency means that different replicas or components may temporarily observe different states, but they converge if updates stop and the system continues to make progress.
>
> It can be appropriate for asynchronous projections, analytics, and some caching scenarios.
>
> I would identify business invariants that require stronger consistency and enforce those at the appropriate boundary.

---

# 10. Caching Q&A

## When would you use caching?

> I use caching when repeated reads are expensive and the data can tolerate the consistency characteristics of the cache.
>
> I consider access frequency, data size, TTL, invalidation strategy, cache hit rate, and failure behavior.

## What problems can caching introduce?

Mention:

- Stale data
- Invalidation complexity
- Cache stampede
- Memory pressure
- Inconsistent replicas
- Cold cache
- Cache availability

## What happens when Redis goes down?

> It depends on whether the cache is an optimization or a required component.
>
> If it is only a cache, the application should ideally fall back to the source of truth, subject to protecting the database from a sudden load spike.
>
> I would also consider timeouts, circuit breakers and rate limiting to avoid cascading failure.

---

# 11. Low-latency / high-throughput

When asked how to improve API latency:

```text
Measure
  ↓
Find bottleneck
  ↓
Database?
Network?
CPU?
Lock?
External API?
Serialization?
Repeated work?
  ↓
Optimize
  ↓
Load test
  ↓
Measure again
```

Possible techniques:

- Caching
- Async processing
- Connection pooling
- Batch operations
- Reduce network calls
- Better database indexes
- Avoid unnecessary serialization
- Reduce lock contention
- Horizontal scaling
- Pre-computation

Important:

> Never say "add more servers" as your first answer. First identify the bottleneck.

---

# 12. Coding tests to practice

## Longest Substring Without Repeating Characters

```python
def longest_substring(s: str) -> int:
    last_seen = {}
    left = 0
    result = 0

    for right, char in enumerate(s):
        if char in last_seen and last_seen[char] >= left:
            left = last_seen[char] + 1

        last_seen[char] = right
        result = max(result, right - left + 1)

    return result
```

Complexity: O(n) time, O(k) space.

## Top K Frequent Elements

Know:

- Hash map
- Counter
- Heap
- Bucket sort alternative

Be ready to discuss complexity.

## Merge Intervals

Know:

- Sort by start
- Iterate
- Merge overlapping intervals

Complexity: O(n log n).

## LRU Cache

Understand:

```text
HashMap
   +
Doubly Linked List
```

Goal: O(1) get + O(1) put.

Be ready to explain eviction and update behavior.

## Rate Limiter

> Allow at most 100 requests per user in a rolling 60-second window.

Discuss:

- User ID
- Timestamp
- Atomicity
- Concurrent requests
- Distributed deployment
- Shared state

Senior-level point:

> A rate limiter that is correct in one process may not be correct when the service runs across multiple instances.

---

# 13. Python questions

## Explain the GIL

> The Global Interpreter Lock in CPython allows only one thread at a time to execute Python bytecode within a process.
>
> Threads can still be useful for I/O-bound workloads because they can make progress while waiting on I/O.
>
> For CPU-bound Python workloads, multiprocessing or native/vectorized/GPU-based computation may be more appropriate depending on the workload.

## Threading vs asyncio vs multiprocessing

**Threading:** I/O and blocking libraries.

**asyncio:** high-concurrency I/O with async-compatible libraries.

**multiprocessing:** CPU-bound workloads across processes.

Your GPU work gives you another angle:

> For numerical workloads, vectorized or GPU-accelerated computation can sometimes be more appropriate than Python-level threading.

---

# 14. AWS questions

## How would you secure an AWS application?

Mention:

- IAM
- Least privilege
- Network isolation
- Encryption
- Secrets
- Logging
- Monitoring
- Security scanning
- Secure CI/CD

## DynamoDB — what matters?

Be ready to explain:

- Partition key
- Sort key
- Access patterns
- Hot partitions
- Query vs scan
- Capacity
- Consistency
- Secondary indexes

Principle:

> Design DynamoDB tables around access patterns rather than relational normalization.

## SQS — what should you know?

Know:

- Producer
- Queue
- Consumer
- Visibility timeout
- Retry
- Dead-letter queue
- At-least-once delivery considerations
- Message retention
- Long polling
- Idempotent consumers

---

# 15. Security questions

## How do you secure an API?

```text
Authentication
      ↓
Authorization
      ↓
Input validation
      ↓
Rate limiting
      ↓
Secure transport
      ↓
Audit logging
      ↓
Monitoring
```

Mention:

- JWT/OAuth where appropriate
- Least privilege
- HTTPS/TLS
- Input validation
- Sanitization
- Rate limiting
- Secret management
- Logging without sensitive information

---

# 16. Production incident test

Interviewer:

> Your production API suddenly becomes five times slower. What do you do?

Strong structure:

### 1. Establish impact

- Error rate
- Latency
- Affected endpoints
- Affected users

### 2. Check recent changes

- Deployment
- Configuration
- Database changes
- Infrastructure

### 3. Check system health

- CPU
- Memory
- Network
- DB
- Cache
- Queue

### 4. Trace request path

```text
Client
 ↓
API
 ↓
Service
 ↓
Cache
 ↓
Database
 ↓
External Service
```

### 5. Mitigate

Possibilities:

- Rollback
- Disable problematic feature
- Scale
- Rate limit
- Fail over

### 6. Root cause

Do not stop after mitigation.

### 7. Prevention

- Test
- Monitoring
- Alert
- Architecture change
- Deployment improvement

---

# 17. Behavioral / leadership

## Tell me about a technical disagreement.

> I first make sure I understand the other person's reasoning and the actual requirements.
>
> Then I compare alternatives based on reliability, performance, security, complexity and delivery constraints.
>
> If necessary, I use a prototype or data to validate assumptions.
>
> Once the team makes a decision, I support the decision and focus on execution.

## How do you mentor engineers?

> I try to explain the reasoning behind a decision rather than simply giving the answer.
>
> During code and design reviews, I ask questions around scalability, reliability, security, performance and maintainability.
>
> My goal is to help engineers develop the ability to make good technical decisions independently.

---

# 18. Forward Deployed / client questions

## What does Forward Deployed mean to you?

> My understanding is that a forward-deployed engineer works closely with customers to solve real business problems and turn those requirements into production software.
>
> This requires strong engineering skills as well as communication, problem solving and the ability to explain technical trade-offs.
>
> My previous experience working on enterprise telecom systems and communicating technical requirements with clients is relevant to this model.

## How do you handle changing requirements?

> I first understand the business objective behind the change.
>
> Then I assess the impact on architecture, scope, timeline and operational risk.
>
> I explain the available options and trade-offs and align with the stakeholders on priorities.
>
> I then document the decision and adjust the implementation plan.

---

# 19. AI / Agentic AI

## What is your AI experience?

> My direct AI experience includes TensorFlow-based model development and GPU-accelerated quantitative backtesting.
>
> My strongest production experience is in backend systems, distributed architecture and scalable services.
>
> I am particularly interested in applying those engineering skills to production AI systems.

## What are the biggest challenges in enterprise AI?

Mention:

- Security
- Data access
- Authorization
- Hallucination
- Evaluation
- Observability
- Cost
- Latency
- Reliability
- Integration with legacy systems

Good sentence:

> Enterprise AI is not only a model problem. It is also a systems engineering, security, integration and reliability problem.

---

# 20. AI coding tools

Possible question:

> How do you use AI coding tools?

Answer:

> I use AI coding tools to accelerate implementation, explore approaches, generate test cases, refactor code, and investigate unfamiliar APIs or codebases.
>
> However, I treat generated code as a proposal rather than as an authoritative answer.
>
> I still review the design, correctness, security, performance and maintainability, and I validate important changes with tests.

---

# 21. Interviewer follow-up traps

If you say:

> "We used caching."

Expect:

- Why?
- What did you cache?
- What was the TTL?
- How did you invalidate it?
- What happens when the cache is unavailable?

If you say:

> "We used asynchronous processing."

Expect:

- Why?
- What queue?
- What happens when the worker crashes?
- Can messages be duplicated?
- How do you guarantee idempotency?
- How do you monitor backlog?

If you say:

> "We scaled horizontally."

Expect:

- What was the bottleneck?
- How did you decide when to scale?
- Is the service stateless?
- What about shared state?

If you say:

> "We improved performance 3x."

Expect:

- How did you measure it?
- What was the baseline?
- What was the bottleneck?
- How do you know your change caused the improvement?

---

# 22. 60-minute mock interview

## 0–5 min

> Tell me about yourself.

## 5–10 min

> Tell me about a project where you had the greatest ownership.

## 10–20 min

Deep dive:

> You mentioned a 3x API performance improvement. Walk me through it.

Follow-ups:

- Root cause?
- Architecture?
- Metrics?
- Trade-offs?

## 20–40 min

System Design:

> Design an event-driven order processing platform.

Must cover:

- API
- Database
- Queue
- Consumers
- Retry
- Idempotency
- DLQ
- Scaling
- Observability

## 40–52 min

Coding:

> Given a string, find the length of the longest substring without repeating characters.

Then explain:

- Algorithm
- Complexity
- Edge cases

## 52–57 min

AWS / Distributed Systems:

> How would you make this system reliable?

## 57–60 min

Your questions.

---

# 23. Questions you should ask them

1. > What kinds of systems or projects would an SDE III typically own at OneByZero?

2. > How is the responsibility split between hands-on engineering, architecture, technical leadership, and client interaction?

3. > What would you expect an SDE III to accomplish during the first six months?

4. > How much direct interaction would an SDE III typically have with clients, and how often would onsite travel be required?

5. > What types of agentic AI systems is the team currently building?

6. > How do you evaluate the success of an SDE III beyond delivering the technical implementation?

---

# 24. What to memorize vs understand

Do NOT memorize every sentence.

Memorize the structures.

## System design

```text
Requirement
   ↓
Scale
   ↓
Architecture
   ↓
Data
   ↓
Cache
   ↓
Async
   ↓
Failure
   ↓
Security
   ↓
Observability
```

## Behavioral

```text
Situation
   ↓
Problem
   ↓
Action
   ↓
Result
   ↓
Lesson
```

## Production

```text
Detect
   ↓
Measure
   ↓
Investigate
   ↓
Mitigate
   ↓
Root Cause
   ↓
Prevent
```

---

# 25. Your biggest advantage

Your strongest advantage is that your CV contains real production experience.

You do not need to pretend to be an AI researcher.

Your positioning should be:

> **Strong backend/distributed systems engineer who is expanding into production AI.**

The combination is:

```text
15+ years engineering
        +
Distributed Systems
        +
AWS
        +
2M+ production users
        +
Performance
        +
Security
        +
Event-driven architecture
        +
AI / GPU / TensorFlow
        +
Technical Leadership
```

---

# 26. Final 24-hour checklist

### Personal

- [ ] 60-second introduction
- [ ] Current role
- [ ] Greatest ownership project
- [ ] Why OneByZero
- [ ] Why now

### Architecture

- [ ] 2M-user architecture
- [ ] Event-driven architecture
- [ ] Service Bus / SQS
- [ ] Horizontal scaling
- [ ] Caching
- [ ] Low latency
- [ ] High throughput

### Distributed systems

- [ ] Idempotency
- [ ] Retry
- [ ] DLQ
- [ ] Eventual consistency
- [ ] Duplicate messages
- [ ] Circuit breaker

### AWS

- [ ] EC2
- [ ] DynamoDB
- [ ] S3
- [ ] SQS
- [ ] IAM
- [ ] Security

### Coding

- [ ] Sliding window
- [ ] Hash map
- [ ] Heap
- [ ] Sorting
- [ ] LRU
- [ ] Rate limiter

### Production

- [ ] Incident response
- [ ] Performance investigation
- [ ] Monitoring
- [ ] Rollback
- [ ] Root cause analysis

### Leadership

- [ ] Mentoring
- [ ] Technical disagreement
- [ ] Ownership
- [ ] Client communication

### AI

- [ ] TensorFlow project
- [ ] GPU backtesting
- [ ] Enterprise AI
- [ ] Prompt injection
- [ ] Tool authorization
- [ ] AI evaluation

---

# 27. Most important rule

For every technical answer, try to demonstrate at least one of these:

> **Why did you choose this design?**

> **What trade-off did you make?**

> **What happens when it fails?**

> **How do you measure whether it works?**

> **How does it scale?**

These questions distinguish a senior engineer from someone who simply knows individual technologies.

---

# 28. One sentence to remember

If you become stuck during system design, return to:

> **"Let me first clarify the requirements and constraints, then I'll choose the simplest architecture that satisfies them, and I'll explicitly consider scalability, failure handling, security, and observability."**
