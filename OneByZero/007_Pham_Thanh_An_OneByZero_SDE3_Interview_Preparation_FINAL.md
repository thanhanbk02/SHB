# ONEBYZERO SDE III — FINAL INTERVIEW PREPARATION
## Pham Thanh An | Based on the official OneByZero Candidate Preparation Guide

> **Purpose:** This file is the new practice guide for the next OneByZero interview rounds.
>
> The most important change from my previous preparation is this:
>
> **The official guide now tells us exactly what to prioritize.**
>
> I should not try to memorize hundreds of answers. I should practice:
>
> **Understand → Explain in simple English → Build/Code → Defend my decisions.**

---

# 0. WHAT ONEBYZERO OFFICIALLY TOLD ME

The official Candidate Preparation Guide says the interview process includes:

- **Round 1 — Hiring Manager Fit:** 30–45 minutes
- **Round 2 — Technical Screen:** 60 minutes, coding + AWS
- **Round 3 — AI Live Coding:** 60 minutes, build a working AI-integrated application
- **Round 4 — Culture, Leadership & Behavioural:** 60 minutes

> The guide itself says "Five rounds" but then lists R1–R4. The recruiter email also lists four rounds. Therefore, I should prepare for the **four explicitly described rounds**, without guessing about an additional round.

The guide also says:

- I can use the programming language/framework I am most comfortable with.
- I should know my resume and projects very well.
- I should prepare STAR stories.
- I should explain my thought process while solving problems.
- I should state Big-O complexity.
- AI coding assistants are expected and encouraged.
- For the AI live coding round, I should have:
  - IDE
  - AI coding assistant such as Claude, Codex, or Cursor
  - API key for AWS Bedrock, OpenAI, or Anthropic
- For Round 2, the guide specifically mentions an IDE without autocompletion or an online interpreter with screen sharing enabled.

Sources:
- Official Candidate Preparation Guide
- Recruiter interview email

---

# 1. MY INTERVIEW STRATEGY

## 1.1. The four things I must demonstrate

At SDE III level, I want the interviewer to feel:

> **"An can own a complex system."**

Then:

1. I can make technical decisions.
2. I can explain trade-offs.
3. I can build and debug production software.
4. I can lead people and communicate with clients.

My core message:

> **I am a production engineer with strong backend and distributed-system experience. I have real AWS experience, system ownership, technical leadership, and practical AI experience. I can bring these strengths into enterprise AI systems.**

---

# 2. ENGLISH RULES FOR ME

My English does not need to be perfect.

My goal is:

> **Clear technical English. Short sentences. Simple words.**

Do not try to speak like a native speaker.

## 2.1. Ten emergency sentences

Memorize these:

1. **"That's a good question. Let me think for a moment."**
2. **"Let me structure my answer."**
3. **"There are two main points."**
4. **"Let me clarify the requirements first."**
5. **"The main trade-off is..."**
6. **"The reason I chose this approach is..."**
7. **"The main risk is..."**
8. **"I would verify this with metrics and tests."**
9. **"I have not used that in production, but I know the basic idea."**
10. **"Could you repeat the question, please?"**

## 2.2. When I do not understand

> "Could you repeat the question, please?"

> "Could you say that in another way?"

> "Just to make sure I understand, are you asking about...?"

This is better than guessing.

## 2.3. When I get stuck

Do not become silent.

Say:

> "Let me step back and reconsider the approach."

Or:

> "I see one issue with my current approach. Let me think about a better solution."

---

# 3. MY CORE SELF-INTRODUCTION

## Question

**"Tell me about yourself."**

## Easy English answer

> My name is Pham Thanh An.
>
> I am a Senior Software Engineer and Solution Architect with more than 15 years of experience.
>
> My strongest areas are backend engineering, distributed systems, AWS, and performance optimization.
>
> I mainly work with Python, Java, and C#.
>
> In my current role, I own the architecture and delivery of backend platforms serving more than 2 million users.
>
> I also lead technical work, review designs, solve production issues, and mentor engineers.
>
> One example is a real-time platform where I improved API response time by up to three times.
>
> I also have practical AI experience. I built a GPU-based backtesting system processing about 100 million trades, and I built a TensorFlow model for gold-price prediction.
>
> I am interested in OneByZero because it combines backend engineering, AWS, enterprise systems, and AI.
>
> I believe I can contribute from day one, especially in architecture, reliability, and production engineering.

### Keywords

```text
15+ years
→ Backend
→ Distributed systems
→ AWS
→ Python / Java / C#
→ 2M users
→ Architecture
→ Technical leadership
→ 3x latency
→ GPU / TensorFlow
→ Enterprise AI
```

---

# 4. ROUND 1 — HIRING MANAGER INTERVIEW

## Official focus

The official guide says I should prepare for:

- setting technical direction
- influencing clients/senior stakeholders
- mentoring
- pushing back on stakeholders
- GenAI / industry point of view

---

# 5. ROUND 1 — QUESTION 1
## Tell me about a time you changed the technical direction of a project.

### Easy answer

> In one project, we had performance problems in production.
>
> The initial idea was to add more servers.
>
> I did not think this was the best first step.
>
> I profiled the request path and found two bottlenecks.
>
> Some work was blocking the request, although it could run asynchronously.
>
> Some data was also loaded repeatedly.
>
> I changed the design.
>
> I moved non-blocking work to asynchronous processing and added local caching.
>
> After the change, the API response time improved by up to three times.
>
> The main lesson was: measure the real bottleneck before changing the infrastructure.

### Follow-up questions

**Why did you not add more servers?**

> Because the main problem was inefficient request processing. More servers could reduce pressure, but they would not remove the bottleneck.

**How did you know your solution worked?**

> We compared response time before and after the change under similar traffic. Today I would also define p50, p95, and p99 before the change and use load testing.

**What was the risk of local caching?**

> Stale data. I would use TTL and careful invalidation. I would not cache data that requires strong consistency.

---

# 6. ROUND 1 — QUESTION 2
## Tell me about a time you influenced a client or senior stakeholder.

Use my telecom experience.

### Easy structure

```text
Situation
→ Client had a business requirement
→ I understood the real business goal
→ I explained technical options
→ I explained cost / risk / delivery trade-offs
→ We agreed on a solution
→ I helped deliver it
```

### Easy English

> I worked with enterprise telecom clients before, including Viettel, Vinaphone, and Mobifone.
>
> My role was not only development.
>
> I also discussed requirements, proposed technical solutions, and handled technical issues with the client.
>
> When there were different opinions, I first tried to understand the business goal.
>
> Then I explained the technical trade-offs in simple terms.
>
> If necessary, I used data or a small prototype to support the decision.
>
> My goal was not to win an argument. My goal was to find the best solution for the business and the system.

---

# 7. ROUND 1 — QUESTION 3
## Tell me about someone you mentored.

Important: the official guide wants a **specific result**, not just "I mentored people."

### Structure

```text
Person / situation
→ What was difficult
→ What I did
→ What changed
→ Result
```

### Easy answer template

> One engineer on my team had difficulty with a complex backend task.
>
> Instead of giving the solution directly, I helped him break the problem into smaller parts.
>
> I asked questions about scalability, failure cases, security, and maintainability.
>
> I also reviewed the design and code with him.
>
> After that, he was able to complete the task and handle similar problems more independently.
>
> My goal in mentoring is not to make people depend on me. It is to help them make good technical decisions by themselves.

---

# 8. ROUND 1 — QUESTION 4
## Tell me about a time you disagreed with a client or stakeholder.

### Easy answer

> First, I try to understand why they want that solution.
>
> Sometimes a technical solution looks wrong, but it solves an important business constraint.
>
> Then I explain the risks and trade-offs clearly.
>
> I usually give two or three options.
>
> If possible, I use data or a small prototype.
>
> If the client still chooses the original option after understanding the risks, I support the decision and document the important risks and safeguards.

### Key sentence

> **"The goal is not to win the argument. The goal is to make the best decision for the project."**

---

# 9. ROUND 1 — QUESTION 5
## Why OneByZero?

> OneByZero combines several areas that match my experience.
>
> I have strong experience in backend engineering, AWS, distributed systems, and production operations.
>
> At the same time, I want to go deeper into enterprise AI and agentic AI.
>
> I also like the Forward Deployed model because I have worked directly with enterprise telecom clients before.
>
> I think I can contribute from day one in software architecture and production engineering, while growing my AI experience.

---

# 10. ROUND 1 — QUESTION 6
## Why are you leaving your current company?

> I have been with my current company for many years, and I have had the opportunity to build and operate large systems there.
>
> Now I want a new challenge with a larger enterprise context.
>
> I also want to work more deeply at the intersection of software engineering, cloud, and AI.
>
> So I see this as a step forward in my career.

---

# 11. ROUND 1 — QUESTION 7
## Where do you see yourself in the next few years?

> I want to become a stronger technical leader.
>
> I still want to be hands-on with engineering, but I also want to own larger systems and technical workstreams.
>
> I want to become stronger in enterprise AI architecture and help teams build reliable production AI systems.
>
> I also want to grow other engineers and become a trusted technical partner for clients.

---

# 12. ROUND 1 — QUESTION 8
## What do you think about the future of GenAI?

### Easy answer

> I think GenAI will become part of many enterprise workflows.
>
> But the difficult part is not only the model.
>
> Enterprise systems also need security, authorization, data protection, reliability, observability, cost control, and integration with existing systems.
>
> I think the strongest engineers will combine AI knowledge with strong software engineering.
>
> That is one reason I am interested in OneByZero.

---

# 13. ROUND 2 — TECHNICAL SCREEN

## Official focus

The official guide specifically calls out:

### DSA

- graph traversal
- shortest-path reasoning
- interval / scheduling problems
- connecting algorithms to infrastructure
- scaling algorithms by several orders of magnitude
- Big-O

### AWS

- multi-region
- multi-tenant
- active-active vs active-passive
- AI/model failover
- circuit breaker
- fallback / degraded mode
- cost governance
- distributed tracing
- RTO / RPO
- compliance logging
- encryption / key management
- auth + isolation + throttling
- serverless
- Lambda performance
- API Gateway
- CloudWatch
- load balancing
- EC2
- IAM

---

# 14. ROUND 2 — DSA STRATEGY

The most important rule:

> **Do not start coding immediately.**

Use this sequence:

```text
1. Clarify
2. Give simple approach
3. Improve the approach
4. State complexity
5. Code
6. Test
7. Discuss edge cases
8. Discuss scale
```

### English script

> "Let me clarify the input and output first."

> "The simple approach would be..."

> "The problem with this approach is O(n²)."

> "We can improve it using..."

> "The time complexity is O(n), and the space complexity is O(n)."

> "Let me test it with a small example."

> "Now let me think about the edge cases."

---

# 15. DSA PATTERN 1 — HASH MAP

Typical problems:

- Two Sum
- Group Anagrams
- Frequency counting

### Two Sum

Idea:

```text
for each number:
    target = target_sum - number
    check whether target is already in hash map
```

Complexity:

```text
Time: O(n)
Space: O(n)
```

### Easy explanation

> I use a hash map to remember numbers I have already seen.
>
> For each number, I calculate the number I need.
>
> If that number is already in the map, I found the answer.
>
> This changes the lookup from O(n) to O(1) on average.

---

# 16. DSA PATTERN 2 — SLIDING WINDOW

Important problem:

**Longest Substring Without Repeating Characters**

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

### Say this

> I keep a window from left to right.
>
> If I see a duplicate character inside the current window, I move the left pointer.
>
> Each character is processed once.
>
> The time complexity is O(n).

---

# 17. DSA PATTERN 3 — BFS

The official guide explicitly mentions graph traversal and shortest path.

## Example

> Given an unweighted graph, find the shortest number of edges from node A to node B.

### Key idea

BFS explores nodes level by level.

```text
Level 0: A
Level 1: neighbors of A
Level 2: neighbors of level 1
Level 3: ...
```

Therefore, the first time we reach B, we have the shortest number of edges.

### Easy English

> Because the graph is unweighted, every edge has the same cost.
>
> BFS explores the graph level by level.
>
> Therefore, the first time we reach the target, we have found the shortest path in terms of number of edges.

### Complexity

```text
Time: O(V + E)
Space: O(V)
```

---

# 18. DSA PATTERN 4 — TOPOLOGICAL SORT

The official guide gives this example:

> "Given a set of tasks with dependencies between them, determine a valid order to execute them in."

This is a classic **topological sorting** problem.

Example:

```text
A → B
A → C
B → D
C → D
```

A valid order:

```text
A → B → C → D
```

or:

```text
A → C → B → D
```

### Kahn's algorithm

Use:

- indegree
- queue

### Easy English

> Each task is a node.
>
> A dependency is a directed edge.
>
> I calculate the indegree of every node.
>
> I put all nodes with indegree zero into a queue.
>
> I process them and reduce the indegree of their dependent nodes.
>
> If a dependent node reaches zero, I add it to the queue.
>
> If I cannot process all nodes, there is a cycle.

### Complexity

```text
Time: O(V + E)
Space: O(V)
```

---

# 19. DSA PATTERN 5 — INTERVALS

Practice:

- Merge Intervals
- Meeting Rooms
- Meeting Rooms II
- Scheduling

Example:

```text
[1,3]
[2,6]
[8,10]
[9,12]
```

After sorting:

```text
[1,6]
[8,12]
```

### Key idea

Sort by start time, then merge overlapping intervals.

### Easy English

> First, I sort the intervals by start time.
>
> Then I scan from left to right.
>
> If the current interval overlaps the previous one, I merge them.
>
> Otherwise, I start a new interval.

---

# 20. DSA + INFRASTRUCTURE CONNECTION

The official guide specifically says to connect algorithmic thinking to infrastructure.

Example:

### Scheduling problem → connection pool

Suppose:

```text
1000 requests
10 database connections
```

The system cannot process all requests simultaneously.

We need:

```text
Queue
↓
Connection pool
↓
Workers
```

### Interview answer

> The scheduling problem is similar to connection-pool management.
>
> We have many tasks but limited resources.
>
> A queue can hold waiting tasks, and the worker count is limited by the available connections.
>
> This is also related to backpressure and concurrency control.

---

# 21. WHAT IF INPUT BECOMES 1000x LARGER?

This is explicitly mentioned in the OneByZero guide.

Ask myself:

```text
Does it still fit in memory?
Can I process it in streaming mode?
Can I partition the data?
Can I parallelize it?
Can I use a distributed system?
Can I reduce unnecessary work?
Can I change the data structure?
```

### English

> If the input becomes much larger, I first check whether the current algorithm is still practical.
>
> Then I look at memory usage, CPU cost, and I/O.
>
> I may use streaming, partitioning, batching, parallel processing, or a distributed approach.
>
> I would also measure the real bottleneck before optimizing.

---

# 22. ROUND 2 — AWS MULTI-REGION

This is one of the most important new topics from the official guide.

## Question

**"How would you design a system that stays available if an entire AWS region goes down?"**

### Basic architecture

```text
                 Route 53
                    |
          +---------+---------+
          |                   |
       Region A            Region B
          |                   |
     Load Balancer       Load Balancer
          |                   |
       Services            Services
          |                   |
       Database            Database
```

Depending on the workload:

```text
Active-Passive
or
Active-Active
```

---

# 23. ACTIVE-ACTIVE VS ACTIVE-PASSIVE

## Active-Active

Both regions serve traffic.

### Advantages

- Better utilization
- Faster failover
- Both regions are serving production

### Disadvantages

- More complex
- Data replication is harder
- Conflict handling may be needed
- More expensive

## Active-Passive

One region serves traffic.

The second region is ready for failover.

### Advantages

- Simpler
- Easier data consistency model

### Disadvantages

- More idle resources
- Failover can take longer

### Interview answer

> I would choose active-active or active-passive based on the SLA, RTO, RPO, data consistency requirements, and cost.
>
> For very strict availability, active-active can reduce failover time.
>
> But it introduces more complexity, especially for data replication and conflict handling.
>
> For a less demanding system, active-passive may be simpler and cheaper.

---

# 24. RTO AND RPO

## RTO

**Recovery Time Objective**

How quickly must the system recover?

Example:

```text
RTO = 5 minutes
```

The system should recover within five minutes.

## RPO

**Recovery Point Objective**

How much data loss is acceptable?

Example:

```text
RPO = 1 minute
```

We can lose at most about one minute of data.

### Easy English

> RTO is about recovery time.
>
> RPO is about acceptable data loss.
>
> They directly influence architecture decisions.

---

# 25. AI MODEL FAILOVER ACROSS REGIONS

Question:

**"What happens if your AI model service fails in one AWS region?"**

### Design

```text
User
 ↓
API Gateway
 ↓
AI Service
 ↓
Model Router
 ├── Region A / Model A
 └── Region B / Model B
```

Use:

- timeout
- circuit breaker
- health check
- fallback
- retry with limits
- degraded mode

### Easy English

> I would not retry blindly.
>
> First, I would use a timeout.
>
> If the failure is persistent, the circuit breaker opens.
>
> Then the request can fail over to another region or another model.
>
> If the full AI capability is unavailable, I would provide a degraded mode if the business allows it.

---

# 26. CIRCUIT BREAKER

Basic states:

```text
CLOSED
   ↓ too many failures
OPEN
   ↓ after cooldown
HALF-OPEN
   ↓ successful test
CLOSED
```

### Easy English

> A circuit breaker prevents repeated calls to a failing dependency.
>
> When failures exceed a threshold, the circuit opens.
>
> We stop sending requests for a period.
>
> Then we test the dependency again.
>
> This protects our system from cascading failures.

---

# 27. MULTI-TENANT ARCHITECTURE

OneByZero works with enterprise customers, so this is important.

Example:

```text
Tenant A
Tenant B
Tenant C
```

All may use the same platform.

## Main concerns

- authentication
- authorization
- data isolation
- throttling
- encryption
- tenant-aware logging
- cost tracking

### Easy answer

> I would treat tenant identity as part of the request context.
>
> Every request must be authenticated and authorized.
>
> Data access must always be scoped to the tenant.
>
> I would also use tenant-aware throttling and monitoring.
>
> For highly sensitive customers, I may use stronger isolation, such as separate accounts, databases, or infrastructure.

---

# 28. AUTH + ISOLATION + THROTTLING

The official guide says to practice these together.

Think:

```text
Authentication
      ↓
Who are you?
      ↓
Authorization
      ↓
What can you access?
      ↓
Tenant isolation
      ↓
Which tenant's data can you access?
      ↓
Throttling
      ↓
How much can you use?
```

### Interview answer

> Authentication tells me who the caller is.
>
> Authorization tells me what the caller can do.
>
> Tenant isolation makes sure the caller can only access the correct tenant's data.
>
> Throttling prevents one tenant from consuming all shared resources.

---

# 29. AWS SERVERLESS

## Lambda

Important factors from the guide:

- memory
- package size
- performance
- cost

### Question

**"How does Lambda memory affect performance?"**

> Increasing Lambda memory also gives the function more CPU resources.
>
> This can reduce execution time.
>
> But it can increase the cost per invocation.
>
> So I would benchmark different memory settings and compare both latency and cost.

---

# 30. API GATEWAY

Simple architecture:

```text
Client
  ↓
API Gateway
  ↓
Lambda
  ↓
Database
```

API Gateway can provide a front door for APIs.

Think about:

- authentication
- throttling
- routing
- monitoring
- request validation

---

# 31. CLOUDWATCH

Question:

**"How would you investigate a Lambda performance problem?"**

### Answer

> First, I check CloudWatch metrics.
>
> I look at duration, error count, invocation count, and throttling.
>
> Then I check logs for slow operations and errors.
>
> If the system has multiple services, I use distributed tracing to follow the request across the pipeline.
>
> I then identify whether the bottleneck is CPU, network, database, downstream service, or cold start.

---

# 32. DISTRIBUTED TRACING

Think:

```text
Request
 ↓
API Gateway
 ↓
Service A
 ↓
Service B
 ↓
Database
 ↓
AI model
```

Each step should have trace information.

### Why?

> Without distributed tracing, I may only know that the request is slow.
>
> With tracing, I can see which stage is slow.

Important metrics:

- latency
- p50
- p95
- p99
- error rate
- downstream latency

---

# 33. COST GOVERNANCE

Question:

**"How would you control AI/cloud cost at scale?"**

### Think about:

- model selection
- smaller models for simple tasks
- caching
- batching
- token limits
- rate limits
- budgets
- per-tenant usage
- monitoring
- autoscaling

### Easy answer

> I would treat cost as a production metric.
>
> For AI, I would not use the largest model for every request.
>
> Simple tasks can use smaller models.
>
> I would also control token usage, cache repeated results where appropriate, and monitor cost by tenant and workflow.
>
> I would set budgets and alerts for abnormal usage.

---

# 34. COMPLIANCE LOGGING

For enterprise systems, logs may need to answer:

```text
Who?
Did what?
When?
To which resource?
With what result?
```

For example:

```text
user_id
tenant_id
action
resource
timestamp
result
request_id
```

But do not put secrets or sensitive personal data into logs unnecessarily.

### Easy English

> Audit logs should be reliable and protected from unauthorized modification.
>
> They should capture who performed an action, what happened, when it happened, and the result.
>
> I would avoid logging secrets or unnecessary sensitive data.

---

# 35. ENCRYPTION AND KEY MANAGEMENT

Know the difference:

```text
Encryption in transit
→ TLS

Encryption at rest
→ encrypted database / S3 / storage

Key management
→ KMS
```

### Easy answer

> I would encrypt data in transit with TLS and encrypt sensitive data at rest.
>
> I would use a managed key-management service such as AWS KMS.
>
> Access to keys should follow least privilege.
>
> Keys should not be hard-coded in the application.

---

# 36. ROUND 2 — AWS RAPID-FIRE QUESTIONS

## Q: Why IAM roles instead of long-lived access keys?

> Roles reduce the need for long-lived credentials and support temporary access.

## Q: Why use a load balancer?

> To distribute traffic across healthy instances and improve availability and scalability.

## Q: What if Redis is down?

> If Redis is only a cache, I can fall back to the source of truth, but I must protect the database from a sudden traffic spike.

## Q: What if SQS delivers the same message twice?

> Assume at-least-once delivery and make consumers idempotent.

## Q: What is a DLQ?

> A dead-letter queue stores messages that cannot be processed successfully after retries.

## Q: Why exponential backoff?

> To avoid immediately sending many retry requests to an already failing service.

---

# 37. ROUND 3 — AI LIVE CODING

## THIS IS THE BIGGEST NEW PRIORITY

The official guide says:

> **Create a working AI-integrated application.**

This is different from a normal DSA interview.

I should prepare to actually **build something live**.

The guide specifically mentions:

- multi-model pipeline
- specialist models
- evaluation
- cost/latency trade-offs
- guardrails
- PII scrubbing
- output validation
- content-policy checks
- fallback
- enterprise deployment
- authentication
- multi-tenancy
- SLA
- governance
- observability

---

# 38. ROUND 3 — WHAT I SHOULD HAVE READY

Before the interview:

```text
IDE
+
AI coding assistant
+
API key
+
Working Python environment
+
Working API client
+
Internet connection
```

Possible AI assistants mentioned by OneByZero:

- Claude
- Codex
- Cursor

Possible APIs:

- AWS Bedrock
- OpenAI
- Anthropic

I should use the tool that I am most comfortable with.

---

# 39. ROUND 3 — GOLDEN PRACTICE PROJECT

## Build: Enterprise AI Router

Requirement:

> A user sends a question.
>
> The system decides which specialist model should answer.
>
> Then it validates the result before returning it.

Architecture:

```text
User
  |
  v
API
  |
  v
Auth
  |
  v
Router
  |
  +------------------+
  |        |         |
  v        v         v
General  Coding   Finance
Model     Model     Model
  |        |         |
  +--------+---------+
           |
           v
       Evaluator
           |
      +----+----+
      |         |
    PASS       FAIL
      |         |
      v         v
  Response   Fallback /
             retry / human review
```

---

# 40. ROUND 3 — MINIMUM WORKING VERSION

Do not over-engineer the first version.

Start with:

```text
POST /ask
```

Input:

```json
{
  "tenant_id": "tenant-a",
  "user_id": "user-123",
  "query": "..."
}
```

Flow:

```text
1. Validate request
2. Authenticate user
3. Select model
4. Call model
5. Validate output
6. Return response
```

Then add:

```text
7. PII protection
8. Logging
9. Metrics
10. Retry
11. Fallback
12. Tenant isolation
```

---

# 41. ROUND 3 — MULTI-MODEL ROUTING

Question:

**"Why not always use the largest model?"**

### Answer

> The largest model may be more accurate, but it can also be slower and more expensive.
>
> I would use a smaller model for simple tasks and a larger model for difficult tasks.
>
> The router can use the query type, confidence, or business rules to choose the model.
>
> I would measure quality, latency, and cost before deciding.

---

# 42. ROUND 3 — EVALUATION

The official guide explicitly mentions an evaluation step.

Possible flow:

```text
User query
   ↓
Specialist model
   ↓
Evaluator
   ↓
Pass? ---- Yes → Return
   |
   No
   ↓
Fallback / retry / human review
```

### Easy answer

> I would not return every model output directly.
>
> I would evaluate the output against business rules or a separate evaluator.
>
> For important workflows, I would validate correctness, safety, and policy requirements before returning or executing the result.

---

# 43. ROUND 3 — GUARDRAILS

## PII scrubbing

Example:

```text
Name
Email
Phone
National ID
Credit card
```

Before sending data to an external model:

```text
Input
 ↓
PII detection
 ↓
Mask / remove sensitive data
 ↓
LLM
```

### Easy English

> I would detect sensitive information before sending data to the model.
>
> Depending on the use case, I would mask or remove PII.
>
> The policy should be explicit because some enterprise workflows cannot send certain data to external models.

---

# 44. OUTPUT VALIDATION

Do not blindly trust model output.

Possible validation:

```text
Schema validation
+
Business rules
+
Safety checks
+
Content policy
```

Example:

> If the application expects JSON, validate that the model returned valid JSON before using it.

### English

> The model output is untrusted input.
>
> I validate the schema and business rules before using the result.

---

# 45. PROMPT INJECTION

### Easy explanation

> Prompt injection happens when untrusted content tries to manipulate the model's behavior.

Defense:

- treat retrieved documents as untrusted data
- least-privilege tools
- output validation
- authorization outside the model
- human approval for sensitive actions

### Very important sentence

> **"The LLM is not the security boundary."**

---

# 46. TOOL AUTHORIZATION

Suppose an agent can call:

```text
CRM
HR
Finance
Payment
```

Do not give the model unrestricted access.

Architecture:

```text
LLM
 ↓
Tool Gateway
 ↓
Authorization
 ↓
Specific tool
```

### Answer

> Every tool should enforce authorization itself.
>
> I would not rely on the model to decide whether the user is allowed to perform an action.

---

# 47. HUMAN APPROVAL

For sensitive actions:

```text
Delete account
Transfer money
Approve payment
Send external email
Change customer data
```

Use:

```text
Agent
 ↓
Risk check
 ↓
Human approval
 ↓
Action
```

### Easy English

> For high-risk or irreversible actions, I would require human approval before execution.

---

# 48. IDE / AI CODING ASSISTANT WORKFLOW

OneByZero explicitly says AI coding assistants are expected.

My workflow:

```text
1. Understand the requirement
2. Design the simplest solution
3. Ask AI assistant for a small implementation
4. Review the generated code
5. Run tests
6. Debug
7. Improve
8. Explain the final design
```

### Important

Never say:

> "The AI wrote it."

Say:

> **"I used the AI assistant to accelerate implementation, but I reviewed and validated the code."**

---

# 49. AI LIVE CODING — THINK ALOUD SCRIPT

Before coding:

> "Let me first clarify the requirements."

> "I will start with a minimal working version."

> "Then I will add the enterprise concerns."

While coding:

> "I will keep this component simple."

> "This function is responsible for model selection."

> "I want to keep authorization outside the model."

> "Now I will add validation."

After coding:

> "Let me test the happy path first."

> "Now I will test an error case."

> "The next improvement would be..."

---

# 50. AI LIVE CODING — FOLLOW-UP ATTACK

Expect questions such as:

### What if the model is down?

> Timeout + retry for transient errors + circuit breaker + fallback model.

### What if the model is slow?

> Timeout, model routing, smaller model, caching where safe.

### What if the model is expensive?

> Route simple tasks to smaller models and monitor cost per request.

### What if the user belongs to another tenant?

> Authorization and tenant isolation must happen before accessing tenant data.

### What if the model returns unsafe output?

> Validate output and apply policy checks before returning or executing it.

### What if the model returns invalid JSON?

> Validate schema and retry or use a fallback path.

### What if a tool call is repeated?

> Use an idempotency key for actions that must not execute twice.

---

# 51. ROUND 3 — ENTERPRISE AI CHECKLIST

Whenever I design an AI system, ask:

```text
[ ] Authentication
[ ] Authorization
[ ] Tenant isolation
[ ] PII protection
[ ] Prompt injection defense
[ ] Output validation
[ ] Model fallback
[ ] Timeout
[ ] Retry
[ ] Circuit breaker
[ ] Observability
[ ] Cost control
[ ] SLA
[ ] Audit logging
[ ] Human approval for risky actions
[ ] Idempotency for actions
```

---

# 52. ROUND 4 — CULTURE / LEADERSHIP / BEHAVIORAL

Official focus:

- disagreements
- feedback
- collaboration
- ambiguity
- technical pushback
- learning
- long-term goals
- unique value
- leadership
- consistency with earlier rounds

---

# 53. STAR FORMAT

Use:

```text
S — Situation
T — Task
A — Action
R — Result
```

But do not make the answer too long.

Target:

```text
Situation: 15 sec
Task: 10 sec
Action: 30 sec
Result: 15 sec
```

---

# 54. STORY 1 — GREATEST OWNERSHIP

Use the real-time gaming platform.

### S

> We had a real-time platform serving more than 2 million users.

### T

> I owned the backend architecture and delivery.

### A

> We had API latency problems.
>
> I profiled the request path.
>
> I found blocking work and repeated data loading.
>
> I moved non-blocking work to asynchronous processing and added local caching.

### R

> API response time improved by up to three times.

### Lesson

> Measure first. Do not guess.

---

# 55. STORY 2 — SECURITY

Use TheWinZone.

Key points:

```text
750K users
→ pentest
→ vulnerabilities
→ input sanitization
→ distributed locking
→ Redis / RedLock
→ rate limiting
→ security hardening
```

### Easy answer

> I worked on security hardening for a platform serving around 750,000 users.
>
> We had findings from penetration testing.
>
> I helped fix the vulnerabilities and added stronger input sanitization.
>
> I also implemented distributed locking and rate limiting using Redis.
>
> The main lesson was to make secure behavior part of the architecture, not something developers have to remember manually.

---

# 56. STORY 3 — PRODUCTION INCIDENT

> In one telecom project, a service was unstable.
>
> The failures were intermittent.
>
> Instead of restarting the service repeatedly, I collected and correlated logs.
>
> I built a small analysis tool to find the failure pattern.
>
> We identified the root cause and fixed it.
>
> After that, I improved monitoring so we could detect similar problems earlier.
>
> My incident process is:
>
> **stabilize → collect evidence → find root cause → fix → prevent recurrence.**

---

# 57. QUESTION
## Tell me about a failure.

Do not say:

> "I never failed."

Better:

> In one project, I underestimated the impact of a technical decision.
>
> The system worked, but later we found that it created additional operational complexity.
>
> I helped correct the design and improve the process.
>
> The lesson was to review the operational impact earlier, not only the implementation effort.

Use a **real example** if possible.

---

# 58. QUESTION
## How do you handle ambiguity?

> First, I identify what we know and what we do not know.
>
> Then I clarify the business goal.
>
> I make reasonable assumptions and state them clearly.
>
> I choose a simple approach that can be changed later.
>
> If the risk is high, I validate the assumption with data or a small prototype.

---

# 59. QUESTION
## How do you receive feedback?

> I try to understand the specific issue first.
>
> If the feedback is valid, I change my approach.
>
> If I disagree, I explain my reasoning and use data when possible.
>
> I do not take technical feedback personally.

---

# 60. QUESTION
## What unique value would you bring to OneByZero?

### Strong answer

> I bring a combination of hands-on engineering and system ownership.
>
> I have experience with large production systems, AWS, distributed systems, performance, security, and observability.
>
> I have also worked directly with enterprise clients.
>
> My AI experience is practical, including TensorFlow and GPU-based systems.
>
> I think this combination allows me to contribute to enterprise AI systems from the engineering and production side, while continuing to grow deeper in agentic AI.

---

# 61. QUESTION
## What is your leadership style?

> I lead by setting technical direction and helping people make good decisions.
>
> I do not want engineers to depend on me for every decision.
>
> I explain the problem, constraints, and trade-offs.
>
> Then I give engineers ownership and support them when they need help.
>
> My goal is to build a team that can operate independently.

---

# 62. QUESTIONS I SHOULD ASK ONEBYZERO

Choose 2–3.

### Question 1

> **"What would you expect an SDE III to accomplish in the first six months?"**

### Question 2

> **"What type of AI systems would I work on during the first project?"**

### Question 3

> **"How much direct client interaction does an SDE III typically have?"**

### Question 4

> **"What makes someone successful at the SDE III level at OneByZero?"**

### Question 5

> **"How does the team evaluate the quality and reliability of production AI systems?"**

### Question 6

> **"How are technical decisions shared between the engineering team and the client?"**

---

# 63. MOST IMPORTANT TECHNICAL DEFINITIONS

## Idempotency

> Doing the same operation multiple times has the same effect as doing it once.

Example:

```text
POST payment
idempotency_key = ABC123
```

If the client retries, the server returns the previous result instead of creating another payment.

---

## Retry

> Retry only transient failures.

Use:

```text
limited attempts
+
exponential backoff
+
jitter
+
timeout
```

---

## Circuit breaker

> Stop calling a failing dependency to protect the system.

---

## Eventual consistency

> Data may be temporarily different between replicas, but it converges.

Good for:

- feeds
- analytics
- caches

Not good for:

- payment invariants
- critical authorization decisions

---

## Backpressure

> When producers are faster than consumers, the system needs to slow down or buffer work.

Examples:

```text
Queue
Rate limit
Bounded worker pool
Connection pool
```

---

## DLQ

> A dead-letter queue stores messages that repeatedly fail processing.

---

## Transactional outbox

> Store the business update and the event in the same database transaction, then publish the event asynchronously.

---

# 64. AWS CHEAT SHEET

```text
EC2
→ virtual servers

Load Balancer
→ distribute traffic

DynamoDB
→ scalable NoSQL database

S3
→ object storage

Redis / ElastiCache
→ cache / fast shared state

SQS
→ asynchronous queue

Lambda
→ serverless compute

API Gateway
→ API front door

CloudWatch
→ metrics + logs

IAM
→ authentication / authorization control

KMS
→ encryption key management

Route 53
→ DNS / routing
```

---

# 65. SYSTEM DESIGN MASTER FRAMEWORK

For any design question:

```text
1. Requirements
2. Scale
3. API
4. High-level architecture
5. Data model
6. Cache
7. Async processing
8. Failure handling
9. Security
10. Observability
11. Cost
12. Trade-offs
```

### Opening sentence

> **"Let me clarify the requirements first. Then I will estimate the scale and propose a simple architecture."**

### Closing sentence

> **"The main trade-offs are complexity, cost, consistency, and availability."**

---

# 66. AI SYSTEM DESIGN MASTER FRAMEWORK

For AI systems:

```text
User
 ↓
Authentication
 ↓
Authorization / Tenant isolation
 ↓
Input validation / PII
 ↓
Router / Orchestrator
 ↓
Model / Tool
 ↓
Output validation
 ↓
Evaluation
 ↓
Response / Action
```

Then add:

```text
Observability
Cost
Latency
Retry
Fallback
Audit
Human approval
```

---

# 67. FULL MOCK TEST — ROUND 2

## Part A — Coding

### Problem

> You are given a list of tasks and dependencies between tasks. Return a valid order in which the tasks can be executed.

Example:

```text
tasks = ["A", "B", "C", "D"]

dependencies:
A -> B
A -> C
B -> D
C -> D
```

Possible answer:

```text
A, B, C, D
```

or:

```text
A, C, B, D
```

### What I must explain

```text
Graph
Indegree
Queue
Topological sort
Cycle detection
O(V + E)
```

---

# 68. FULL MOCK TEST — ROUND 2 AWS

## Question

> Design an AWS system that remains available if an entire region fails.

I must cover:

```text
Route 53
Multi-region
Active-active vs active-passive
Data replication
Failover
RTO
RPO
Health checks
IAM
Encryption
Observability
Cost
```

### Follow-up attack

> Why active-active?

> What if the database cannot replicate synchronously?

> What is your RPO?

> How do you test regional failover?

> How much does this cost?

---

# 69. FULL MOCK TEST — ROUND 3 AI

## Question

> Build an application that routes user queries to specialized AI models and validates the result before returning it.

### Minimum implementation

```text
POST /ask
 ↓
Router
 ↓
Specialist model
 ↓
Validator
 ↓
Response
```

### Then improve

```text
Auth
Tenant isolation
PII
Retry
Fallback
Evaluation
Logging
Metrics
Cost
```

### Follow-up attack

> What if the model is unavailable?

> What if the model is too expensive?

> What if the output is invalid?

> What if the user tries prompt injection?

> What if two users belong to different tenants?

> What if a tool is called twice?

---

# 70. FULL MOCK TEST — ROUND 4

Answer these without reading the model answers:

1. Tell me about a technical disagreement.
2. Tell me about a difficult stakeholder.
3. Tell me about a time you changed direction.
4. Tell me about a failure.
5. Tell me about mentoring.
6. How do you handle ambiguity?
7. How do you make decisions with incomplete information?
8. How do you give feedback?
9. How do you receive feedback?
10. What value will you bring to OneByZero?
11. Where do you want to be in three years?
12. Why should we hire you for SDE III?

---

# 71. "WHY SHOULD WE HIRE YOU?"

### Easy answer

> I think my background matches the role well.
>
> I have strong hands-on experience in backend systems, AWS, distributed systems, performance, security, and production operations.
>
> I have owned systems serving millions of users and led technical work.
>
> I also have enterprise client experience and practical AI experience.
>
> I believe I can contribute immediately on engineering and architecture, while growing deeper into enterprise AI.

---

# 72. MY TOP 10 ANSWERS TO MEMORIZE

Do not memorize every word.

Memorize the structure.

```text
1. Tell me about yourself
2. Why OneByZero?
3. Why are you leaving?
4. Greatest ownership
5. Technical direction change
6. Client disagreement
7. Mentoring
8. Production incident
9. AI experience
10. Unique value
```

---

# 73. MY TOP 10 TECHNICAL TOPICS

```text
1. BFS / Graph
2. Topological Sort
3. Intervals / Scheduling
4. Big-O
5. Multi-region AWS
6. Multi-tenant architecture
7. RTO / RPO
8. Circuit breaker / fallback
9. Serverless / Lambda / API Gateway
10. Enterprise AI workflow
```

---

# 74. MY TOP 10 AI TOPICS

```text
1. Multi-model routing
2. Specialist models
3. Evaluation
4. PII scrubbing
5. Output validation
6. Prompt injection
7. Tool authorization
8. Fallback
9. Cost / latency
10. Enterprise governance
```

---

# 75. WHAT I SHOULD NOT DO

## Do not memorize long English paragraphs.

Instead:

```text
Keywords
→ Speak
→ Follow-up
→ Speak again
```

## Do not invent experience.

If I have not done something:

> "I have not used that in production, but I know the basic idea. I would approach it this way..."

## Do not say "I don't know" and stop.

Continue reasoning.

## Do not start coding immediately.

Clarify first.

## Do not design a huge architecture immediately.

Start simple.

## Do not forget trade-offs.

Always ask:

> "What is the trade-off?"

---

# 76. 30-SECOND SYSTEM DESIGN ANSWER

When I am nervous:

> First, I clarify the functional and non-functional requirements.
>
> Then I estimate the scale.
>
> I design a simple stateless service behind a load balancer.
>
> I choose the database based on access patterns.
>
> I add cache and asynchronous processing where useful.
>
> Then I handle failures with timeouts, retries, idempotency, and circuit breakers.
>
> Finally, I cover security, observability, and cost.

---

# 77. 30-SECOND AI SYSTEM ANSWER

> I would separate the AI workflow into clear components.
>
> First, authenticate the user and identify the tenant.
>
> Then validate and protect the input.
>
> A router selects the appropriate model or tool.
>
> The output goes through validation and evaluation before returning to the user.
>
> I would also add timeout, retry, fallback, observability, cost control, and human approval for high-risk actions.

---

# 78. 30-SECOND CODING ANSWER

> Let me clarify the input and output first.
>
> I will start with a simple solution.
>
> Then I will look for a better data structure or algorithm.
>
> I will state the time and space complexity before coding.
>
> After coding, I will test normal cases and edge cases.

---

# 79. 7-DAY STUDY PLAN

## Day 1 — Round 1

Practice:

- self introduction
- greatest ownership
- technical direction
- client influence
- mentoring
- disagreement

Record yourself at least 5 times.

---

## Day 2 — DSA

Practice:

- Hash Map
- Sliding Window
- BFS
- Topological Sort
- Intervals

For every problem:

```text
Explain
→ Code
→ Test
→ Big-O
```

---

## Day 3 — AWS

Practice:

- multi-region
- active-active
- active-passive
- RTO / RPO
- DynamoDB
- SQS
- Lambda
- API Gateway
- CloudWatch
- IAM
- KMS

---

## Day 4 — Distributed Systems

Practice:

- retry
- timeout
- circuit breaker
- idempotency
- duplicate messages
- DLQ
- backpressure
- cache
- distributed tracing

---

## Day 5 — AI Live Coding

Build:

```text
AI Router
+
specialist models
+
validation
+
fallback
```

Use my real IDE.

Use the AI coding assistant.

Run the application.

Test the API.

---

## Day 6 — AI Enterprise

Practice:

- PII
- prompt injection
- authorization
- tenant isolation
- evaluation
- cost
- latency
- audit
- human approval

---

## Day 7 — FULL MOCK

Do not read this file during the mock.

```text
R1 — 30 min
        ↓
R2 — 60 min
        ↓
R3 — 60 min
        ↓
R4 — 60 min
```

If I cannot do the full schedule, do at least:

```text
15 min Hiring Manager
30 min DSA + AWS
30 min AI coding
20 min Behavioral
```

---

# 80. 24 HOURS BEFORE INTERVIEW

## Technical

```text
[ ] IDE works
[ ] Python environment works
[ ] AI coding assistant works
[ ] API key works
[ ] Test API call works
[ ] Screen sharing works
[ ] Camera works
[ ] Microphone works
[ ] Internet works
[ ] Backup hotspot ready
```

## Knowledge

```text
[ ] BFS
[ ] Topological sort
[ ] Intervals
[ ] Big-O
[ ] Multi-region
[ ] RTO / RPO
[ ] Lambda
[ ] API Gateway
[ ] CloudWatch
[ ] IAM
[ ] Multi-tenant
[ ] AI routing
[ ] Guardrails
[ ] Evaluation
[ ] Fallback
```

## English

```text
[ ] Self introduction
[ ] Ownership story
[ ] Client story
[ ] Mentoring story
[ ] "Why OneByZero?"
[ ] Emergency sentences
```

---

# 81. MY ANSWER SCORECARD

After every practice answer, score:

| Score | Meaning |
|---|---|
| 0 | I could not answer |
| 1 | Very general |
| 2 | Technically correct but weak |
| 3 | Good technical answer + example |
| 4 | Strong answer + trade-offs + metrics + follow-ups |

Target:

> **3+ for important questions.**

For coding/design:

```text
0 = cannot start
1 = needs much help
2 = can solve with hints
3 = can solve independently
4 = can defend trade-offs and handle follow-ups
```

---

# 82. THE MOST IMPORTANT FOLLOW-UP QUESTIONS

For almost every technical answer, expect:

> **Why?**

> **Why not another solution?**

> **What can fail?**

> **How do you measure it?**

> **What happens at 10x scale?**

> **What is the security risk?**

> **What is the cost?**

> **What did YOU personally implement?**

Train myself to answer these automatically.

---

# 83. FINAL MINDSET

OneByZero's guide explicitly says that interviewers are assessing **how I think**, not only the final answer.

Therefore:

> **Do not hide the thinking process.**

If I need time:

> "Let me think through the trade-offs."

If I make a mistake:

> "I see an issue with my current approach. Let me reconsider."

If the interviewer challenges me:

> "That's a good point. That changes the trade-off."

If I do not know a technology:

> "I have not used that in production, but I know the basic idea. I would approach it this way."

---

# 84. MY FINAL MESSAGE TO THE INTERVIEWER

I want the interviewer to remember these five things:

```text
1. Strong production experience
2. Can own architecture
3. Can reason about scale and failure
4. Can lead and communicate
5. Can bring software-engineering discipline into AI systems
```

My final positioning:

> **"My strongest background is production engineering. I have hands-on AI experience, and I can bring distributed systems, reliability, security, and architecture thinking into enterprise AI systems."**

---

# 85. ONE SENTENCE TO REMEMBER

> **"Technical thinking first. Simple English second."**

And for every technical question:

```text
Requirement
    ↓
Scale
    ↓
Options
    ↓
Trade-offs
    ↓
Decision
    ↓
Failure
    ↓
Security
    ↓
Observability
    ↓
Verification
```

---

# 86. IMPORTANT SOURCE NOTE

This preparation file is based primarily on:

1. **OneByZero — Software Engineering Candidate Preparation Guide**
2. The interview invitation email provided to me
3. My previous OneByZero preparation material and my actual CV/project experience

The official guide explicitly says it does **not** provide the exact interview questions. Therefore:

> The practice questions in this file are preparation exercises, not claims that OneByZero will ask exactly these questions.

The highest-confidence preparation areas are the ones explicitly listed by OneByZero:

- Hiring Manager fit and leadership
- Coding / DSA
- AWS architecture
- AI live coding
- Enterprise AI concerns
- Culture / behavioral / leadership
- Explaining thought process
- Big-O
- AI coding assistant usage

---

# 87. FINAL CHECKLIST

Before I enter the interview:

```text
I can introduce myself in 60–90 seconds.
I can explain my biggest project.
I can explain the 3x performance improvement.
I can explain my AWS experience.
I can solve BFS.
I can solve Topological Sort.
I can solve interval problems.
I can explain Big-O.
I can design multi-region AWS.
I can explain RTO/RPO.
I can explain multi-tenant isolation.
I can explain Lambda/API Gateway/CloudWatch/IAM.
I can build a small AI application live.
I can use my AI coding assistant.
I can explain model routing.
I can explain evaluation.
I can explain PII protection.
I can explain prompt injection.
I can explain fallback and circuit breaker.
I can explain cost and latency trade-offs.
I can tell a mentoring story.
I can tell a disagreement story.
I can tell an ownership story.
I can answer "Why OneByZero?"
I can ask senior-level questions.
```

# FINAL RULE

> **Do not try to prove that I know everything.**
>
> Prove that:
>
> **"I can understand a difficult problem, design a practical solution, build it, operate it in production, handle failure, explain trade-offs, and lead others."**
