# OneByZero --- SDE III (L6) Interview Preparation

**Candidate:** Pham Thanh An\
**Target:** Software Development Engineer III (SDE III / L6) ---
Vietnam\
**Company:** OneByZero\
**Purpose:** Preparation for the next interview after passing the
headhunter/recruiter screening.

> **Important:** OneByZero has not been found to publish a fixed number
> of interview rounds for this exact SDE III Vietnam role. Public
> interview experiences show that technical evaluation can include
> HLD/system design and coding/DSA, while OneByZero has described
> interview loops involving hiring managers and people who would work
> closely with the candidate. Therefore, this document prepares for the
> most likely technical/engineering evaluation without assuming an exact
> round count.

------------------------------------------------------------------------

# 1. What OneByZero Is Looking For

The official SDE III job description defines this as **Internal Level L6
/ SDE III (Senior)**.

The role is not simply "senior backend developer". The candidate is
expected to:

-   Own large systems and technical workstreams.
-   Drive architecture, design, and delivery of complex production
    software.
-   Provide technical leadership.
-   Combine hands-on engineering with design ownership and mentorship.
-   Work directly with client stakeholders.
-   Build and operate production systems on AWS.
-   Ensure scalability, security, and reliability.
-   Establish CI/CD, testing, observability, and DevSecOps practices.
-   Resolve complex production/performance issues.
-   Mentor engineers.
-   Contribute reusable engineering assets/frameworks.
-   Use AI tools effectively in daily engineering work.

## The real question behind the interview

The interviewer is likely trying to determine:

> **Can this engineer independently own a complex production system,
> make sound architectural decisions, lead engineers, solve difficult
> production problems, and communicate effectively with
> enterprise/client stakeholders?**

This is the central theme for the SDE III interview.

------------------------------------------------------------------------

# 2. Candidate Positioning

## Your strongest positioning

Do NOT position yourself simply as:

> "A developer with 15+ years of experience."

Position yourself as:

> **A hands-on senior backend engineer / solution architect who has
> owned production systems end-to-end, worked at millions-of-users
> scale, solved performance and reliability problems, led technical
> workstreams, mentored engineers, and worked directly with enterprise
> clients.**

Your strongest evidence:

-   15+ years of software engineering experience.
-   Production systems on AWS serving 2M+ users.
-   3x API latency reduction.
-   Distributed systems / microservices / event-driven architecture.
-   Python, Java, C#.
-   AWS: EC2, DynamoDB, ElastiCache/Redis, S3, IAM.
-   CI/CD with Jenkins and GitHub Actions.
-   Production observability with Grafana plugins.
-   Security hardening, pentesting, input sanitization, rate limiting,
    distributed locking.
-   HLD/LLD and architecture reviews.
-   Mentoring and technical leadership.
-   Direct enterprise client experience with Viettel, Vinaphone and
    Mobifone.
-   GPU-accelerated quantitative backtesting at \~100M trades.
-   TensorFlow gold-price prediction model.
-   AI-assisted engineering workflows.

------------------------------------------------------------------------

# 3. 90-Second Self Introduction

## Recommended answer

> Sure. My name is Pham Thanh An. I'm a Senior Software Engineer and
> Solution Architect with more than 15 years of experience in backend
> development, system architecture, and production systems.
>
> My main technical background is in Python, Java, and C#, with strong
> experience in distributed systems, microservices, event-driven
> architecture, AWS, CI/CD, observability, security, and performance
> optimization.
>
> In my current role, I've been responsible for the end-to-end
> architecture and delivery of backend platforms serving more than 2
> million users. I've also led technical workstreams, conducted
> architecture and design reviews, mentored engineers, and worked on
> complex production and performance issues.
>
> One example is a real-time platform where I helped reduce API response
> time by up to three times through architectural and performance
> improvements.
>
> I'm also working on quantitative trading and AI-related projects,
> including a GPU-accelerated backtesting framework and a
> TensorFlow-based gold-price prediction model.
>
> What attracted me to OneByZero is the combination of strong software
> engineering, AWS, enterprise-scale systems, and AI. I think this role
> is a very good match for my background, while also giving me an
> opportunity to go deeper into production-scale AI systems.

## Short version

> I'm a senior backend engineer and solution architect with 15+ years of
> experience. My strongest areas are distributed systems, AWS, system
> architecture, performance, security, and production operations. I've
> owned systems serving more than 2 million users and have led technical
> workstreams and mentored engineers. I'm also working with AI/ML and
> GPU-based systems, so OneByZero is attractive to me because it
> combines production engineering, AWS, enterprise systems, and AI.

------------------------------------------------------------------------

# 4. The Five Stories You Must Master

Prepare these stories in STAR format.

## Story 1 --- 2M+ Users / AWS Production System

### Situation

A real-time PVP gaming platform needed to operate continuously at large
scale.

### Your role

You owned backend architecture and delivery.

### Key technologies

-   Python
-   C#
-   Django
-   AWS
-   DynamoDB
-   Redis
-   Photon

### Scale

-   2M+ users
-   20--25K DAU
-   24/7 production

### Key message

> I was not just implementing individual features. I was responsible for
> architecture, production reliability, performance and technical
> decisions.

### Likely questions

-   How did you scale it?
-   What were the main bottlenecks?
-   Why DynamoDB?
-   Why Redis?
-   How did you handle failures?
-   How did you monitor the system?
-   What would you redesign today?

------------------------------------------------------------------------

# 5. Story 2 --- 3x API Latency Improvement

This is one of your strongest interview stories.

## Problem

A production API had performance issues.

## Investigation

You analyzed the request flow and identified work that did not need to
block the main request path.

## Solution

-   Offloaded non-blocking work using ExecutorService.
-   Added per-node caching.
-   Reduced unnecessary repeated work.

## Result

> API response time improved by up to approximately 3x.

## Strong answer

> We had a production API with noticeable latency under load. Instead of
> immediately adding more infrastructure, I first profiled the request
> path and looked at where time was actually being spent.
>
> I found that some non-blocking work was still happening synchronously
> in the request path, and some data was being repeatedly calculated or
> retrieved.
>
> I changed the architecture so that the non-blocking work could be
> executed asynchronously using ExecutorService, and I introduced
> per-node caching for frequently accessed data.
>
> After the change, the API response time improved by up to around three
> times.
>
> The important lesson for me was that performance problems should first
> be understood at the request-flow and architecture level before simply
> scaling out infrastructure.

## Likely follow-ups

### Why asynchronous processing?

Because the work did not need to block the user's request.

### Why per-node cache?

Because the data was suitable for local caching and this reduced
repeated access/processing on each request.

### What are the risks?

-   stale data
-   cache invalidation
-   memory usage
-   inconsistent state between nodes

### How would you improve it?

Explain the trade-off rather than claiming that local cache is always
best.

------------------------------------------------------------------------

# 6. Story 3 --- Event-Driven Architecture / Horizontal Scaling

## TheWinZone

You:

-   Led security and reliability work.
-   Introduced Service Bus event-driven architecture.
-   Decoupled synchronous APIs from asynchronous work.
-   Improved scalability and fault tolerance.

## Tribe Mania

You:

-   Authored HLD.
-   Designed Service Bus-based architecture.
-   Enabled on-demand horizontal scaling of stateful server nodes.
-   Built custom Grafana observability tooling.

## Key interview message

> I understand not only how to build a service, but also how to change
> the architecture when scale, coupling, fault tolerance, and
> operational visibility become problems.

------------------------------------------------------------------------

# 7. Story 4 --- Security

Prepare to discuss:

-   Backend pentesting.
-   Vulnerability remediation.
-   Input sanitization.
-   HtmlSanitizer.
-   Redis-based distributed locking.
-   Rate limiting.
-   Secure API design.

## Strong framing

> Security should be part of architecture rather than something added
> after implementation.

## Possible questions

### How do you secure an API?

Discuss:

1.  Authentication
2.  Authorization
3.  Input validation
4.  Rate limiting
5.  Secure transport
6.  Secrets management
7.  Logging/auditing
8.  Dependency/security scanning
9.  Least privilege
10. Monitoring and incident response

------------------------------------------------------------------------

# 8. Story 5 --- AI / Quantitative Systems

## GPU backtesting

You built a GPU-accelerated framework using:

-   Python
-   CuPy
-   NumPy
-   Pandas
-   MQL5

Scale:

> \~100M trades over 10 years of data in parallel.

## TensorFlow

You developed a gold-price prediction model and integrated it into a
live trading bot.

## Important positioning

Do NOT claim:

> "I have already built large-scale agentic AI production systems."

Instead say:

> My strongest production background is backend engineering and
> distributed systems. I also have hands-on experience with ML,
> TensorFlow, GPU computing and AI-assisted engineering. I see agentic
> AI as an area where strong production engineering fundamentals are
> extremely important, and that's an area where I can contribute
> immediately while deepening my AI expertise.

This is honest and strong.

------------------------------------------------------------------------

# 9. System Design Framework

When given a system-design question, use this sequence.

## Step 1 --- Clarify requirements

Ask:

-   Who are the users?
-   What are the main use cases?
-   Read/write ratio?
-   Expected traffic?
-   Peak traffic?
-   Latency requirement?
-   Availability requirement?
-   Consistency requirement?
-   Data retention?
-   Security/compliance?
-   Geographic distribution?

Do not jump directly to AWS services.

------------------------------------------------------------------------

## Step 2 --- Estimate scale

For example:

-   2M users
-   20K DAU
-   peak concurrent users
-   requests/second
-   average payload
-   storage growth

Show assumptions.

------------------------------------------------------------------------

## Step 3 --- High-level architecture

Typical structure:

``` text
Client
  |
API Gateway / Load Balancer
  |
Stateless Application Services
  |
  +---- Cache (Redis)
  |
  +---- Database
  |
  +---- Message Queue / Event Bus
             |
             +---- Async Workers
             |
             +---- Notification / Processing
```

Then explain each component.

------------------------------------------------------------------------

## Step 4 --- Data design

Discuss:

-   SQL vs NoSQL
-   partition key
-   indexes
-   consistency
-   transactions
-   data lifecycle
-   hot partitions
-   read/write patterns

------------------------------------------------------------------------

## Step 5 --- Scaling

Discuss:

-   horizontal scaling
-   stateless services
-   load balancing
-   caching
-   asynchronous processing
-   database scaling
-   partitioning
-   backpressure

------------------------------------------------------------------------

## Step 6 --- Reliability

Discuss:

-   timeout
-   retry
-   exponential backoff
-   circuit breaker
-   idempotency
-   dead-letter queue
-   graceful degradation
-   health checks
-   failover

------------------------------------------------------------------------

## Step 7 --- Security

Discuss:

-   IAM
-   authentication
-   authorization
-   encryption
-   secrets
-   network isolation
-   rate limiting
-   audit logs

------------------------------------------------------------------------

## Step 8 --- Observability

Discuss:

-   logs
-   metrics
-   traces
-   latency
-   error rate
-   saturation
-   dashboards
-   alerts
-   correlation/request IDs

------------------------------------------------------------------------

## Step 9 --- Deployment

Discuss:

-   CI/CD
-   automated testing
-   infrastructure as code
-   blue/green or canary deployment
-   rollback
-   monitoring after deployment

------------------------------------------------------------------------

# 10. AWS Topics to Review

## EC2

Know:

-   Auto Scaling
-   Load Balancer
-   health checks
-   instance types
-   security groups
-   IAM roles
-   deployment strategies

## DynamoDB

Know:

-   partition key
-   sort key
-   access-pattern-driven design
-   hot partitions
-   consistency
-   GSI
-   capacity/scaling
-   TTL

## Redis / ElastiCache

Know:

-   caching
-   TTL
-   cache-aside
-   invalidation
-   distributed lock
-   rate limiting
-   failure behavior

## S3

Know:

-   object storage
-   lifecycle
-   versioning
-   encryption
-   presigned URLs
-   event integration

## IAM

Know:

-   least privilege
-   role vs user
-   temporary credentials
-   service roles
-   resource policies

## Networking

Review:

-   VPC
-   subnet
-   public/private subnet
-   security group
-   NAT Gateway
-   load balancer
-   DNS

------------------------------------------------------------------------

# 11. Python Topics

Be prepared for technical questions on:

-   GIL
-   threading
-   multiprocessing
-   asyncio
-   concurrent.futures
-   I/O-bound vs CPU-bound workloads
-   memory management
-   generators
-   decorators
-   context managers
-   exception handling
-   profiling
-   performance optimization

## Key answer

If asked:

> When would you use async vs threads vs multiprocessing?

Answer conceptually:

-   Asyncio → many concurrent I/O operations with async-compatible
    libraries.
-   Threads → I/O-bound work or libraries that release the GIL.
-   Multiprocessing → CPU-bound work where parallel CPU execution is
    required.
-   GPU → highly parallel numerical workloads suitable for GPU
    computation.

------------------------------------------------------------------------

# 12. Distributed Systems Topics

Review:

-   CAP theorem
-   consistency models
-   eventual consistency
-   idempotency
-   distributed locks
-   leader election
-   retries
-   duplicate messages
-   ordering
-   queues
-   backpressure
-   rate limiting
-   partitioning
-   replication
-   caching
-   failure detection

## Important question

> How do you make an asynchronous operation safe when the same message
> can be delivered twice?

Expected concepts:

-   idempotency key
-   unique constraint
-   deduplication
-   transactional state transition
-   retry-safe processing

------------------------------------------------------------------------

# 13. Coding / DSA Preparation

Public OneByZero interview experiences indicate that coding/DSA can
appear in technical interviews.

Review at minimum:

## Arrays / Strings

-   Two Sum
-   Longest Substring Without Repeating Characters
-   Sliding Window
-   Two Pointers
-   Prefix Sum

## Hash Map

-   frequency counting
-   grouping
-   duplicate detection

## Stack / Queue

-   valid parentheses
-   monotonic stack
-   BFS

## Trees / Graphs

-   DFS
-   BFS
-   shortest path
-   topological sort

## Binary Search

-   classic binary search
-   search on answer

## Heap

-   Top K
-   K-way merge
-   scheduling

## Complexity

Always state:

> Time complexity: O(n)\
> Space complexity: O(n)

Do not code silently. Explain your thinking.

------------------------------------------------------------------------

# 14. Behavioral / Leadership Questions

Prepare concise STAR answers for:

### Ownership

> Tell me about a project where you had the greatest ownership.

Best story: WWE Champion / 2M+ AWS platform.

### Technical disagreement

Prepare an example where engineers disagreed about architecture.

Structure:

1.  Understand the disagreement.
2.  Define requirements.
3.  Compare trade-offs.
4.  Use data/prototype where useful.
5.  Make a decision.
6.  Document it.
7.  Revisit if assumptions change.

### Mentoring

Key message:

> I don't want engineers to depend on me for every answer. I want them
> to become better at making technical decisions themselves.

### Production incident

Use the 3x latency story.

### Client conflict

Use enterprise telecom experience.

### Failure

Choose a genuine technical decision that had a downside and explain what
you learned.

------------------------------------------------------------------------

# 15. Client-Facing Questions

## Question

> How comfortable are you working directly with clients?

Answer:

> I've had direct client-facing experience from my earlier enterprise
> telecom projects, where I worked with clients such as Viettel,
> Vinaphone and Mobifone. I was involved in understanding requirements,
> discussing technical solutions, troubleshooting production issues and
> coordinating delivery.
>
> I'm comfortable explaining technical trade-offs to both technical and
> non-technical stakeholders. I actually find direct client interaction
> useful because it helps me understand the business problem rather than
> only the technical requirement.

------------------------------------------------------------------------

# 16. Forward Deployed Role

Understand the concept.

A Forward Deployed engineer is usually closer to:

``` text
Client business problem
        ↓
Requirement discovery
        ↓
Technical solution
        ↓
Architecture
        ↓
Implementation
        ↓
Production deployment
        ↓
Iteration with client
```

Your advantage:

You already have experience combining:

-   technical implementation
-   architecture
-   production troubleshooting
-   client communication

## Likely question

> How would you handle a client asking for a solution you believe is
> technically wrong?

Answer:

> First, I would make sure I understand the business objective behind
> the request. Then I would explain the technical trade-offs and the
> risks of the proposed approach, rather than simply rejecting it. If
> there are multiple viable options, I would present them with clear
> trade-offs in cost, complexity, scalability and delivery time.
> Ultimately, I would align with the client and internal stakeholders on
> the decision and document the important assumptions.

------------------------------------------------------------------------

# 17. AI / Agentic AI Questions

## What is an AI agent?

A useful practical definition:

> An AI agent is a system where an AI model can reason over a task,
> decide what actions or tools to use, execute those actions, observe
> results, and continue iterating toward a goal.

## Enterprise agentic AI architecture

Think about:

``` text
User / Enterprise Application
          |
       API Layer
          |
    Agent Orchestrator
          |
    +-----+-----+
    |           |
 LLM/Model    Memory
    |
 Tool / Function Calling
    |
 +--+--------+----------+
 |           |          |
Database    APIs      Enterprise Systems
```

Then add:

-   authentication
-   authorization
-   audit
-   guardrails
-   rate limits
-   observability
-   evaluation
-   human approval
-   data privacy
-   prompt/version management

## Critical enterprise concerns

Do not talk only about the LLM.

Discuss:

-   security
-   data leakage
-   access control
-   hallucination
-   tool permissions
-   auditability
-   observability
-   cost
-   latency
-   reliability
-   evaluation
-   human-in-the-loop

This is particularly relevant because OneByZero works with banks,
telecoms, insurers and retailers in enterprise environments.

------------------------------------------------------------------------

# 18. AI Tools in Daily Engineering

The JD explicitly expects engineers to use AI tools for:

-   productivity
-   testing
-   debugging
-   documentation
-   delivery

A good answer:

> I use AI tools as an engineering productivity multiplier rather than
> as a replacement for engineering judgment. I use them for code
> exploration, generating test cases, debugging hypotheses,
> documentation, refactoring ideas and repetitive development tasks. I
> still validate the output through tests, code review, security checks
> and production considerations.

------------------------------------------------------------------------

# 19. Questions You Should Ask the Interviewer

Choose 2--4 depending on the time.

## About the role

> What does a typical SDE III own within a delivery engagement?

> How is the role split between hands-on engineering, architecture,
> technical leadership and client interaction?

## About projects

> What types of enterprise AI or agentic AI projects is the Vietnam team
> currently working on?

## About success

> What would you expect an SDE III to accomplish in the first six
> months?

## About Forward Deployed

> How much direct interaction does an SDE III typically have with the
> client?

> How frequently do engineers normally travel or work onsite?

## About engineering

> How are technical decisions and architecture reviews handled within
> the engineering organization?

## Excellent final question

> What distinguishes an engineer who is successful at SDE III level at
> OneByZero from someone who is technically strong but not yet operating
> at that level?

This question can reveal exactly what the interviewer cares about.

------------------------------------------------------------------------

# 20. Salary

Do not volunteer your current salary unless necessary.

Preferred answer:

> I'm flexible on compensation and more interested in finding the right
> fit. Based on the scope of the SDE III role and my experience in
> system architecture, AWS, distributed systems and technical
> leadership, I would be looking for around 110 million VND gross per
> month, depending on the overall compensation package.

If asked for current salary:

> My current compensation is around X million VND gross per month.
> However, I don't think it would be the best benchmark for this
> position because the scope and responsibilities are different. For the
> SDE III role, considering the technical ownership, architecture and
> client-facing responsibilities, I would be looking for around 110
> million VND gross per month, depending on the overall package.

Replace **X** with your actual figure.

------------------------------------------------------------------------

# 21. High-Probability Interview Questions

Practice answering these without reading notes.

## Background

1.  Tell me about yourself.
2.  Walk me through your current role.
3.  What are you strongest at technically?
4.  Why are you considering a new opportunity?
5.  Why OneByZero?
6.  Why SDE III?

## Ownership

7.  Tell me about a project where you had the greatest ownership.
8.  What was the most difficult technical decision you made?
9.  Tell me about a production issue you solved.
10. Tell me about a system you designed from scratch.

## Architecture

11. Design a scalable API platform.
12. Design an event-driven backend.
13. How would you scale a system from 100K to millions of users?
14. How do you choose between SQL and NoSQL?
15. When should you use caching?
16. How do you design for failure?
17. How do you guarantee idempotency?

## AWS

18. How would you architect a production system on AWS?
19. DynamoDB vs relational database?
20. EC2 vs containers/serverless?
21. How would you secure an AWS architecture?
22. How would you monitor a production AWS system?

## Python

23. Explain the GIL.
24. Threading vs multiprocessing vs asyncio.
25. How do you optimize a slow Python service?
26. How would you investigate memory growth?

## Distributed systems

27. Explain eventual consistency.
28. How do you handle duplicate messages?
29. How do retries create problems?
30. How do you implement distributed rate limiting?

## Leadership

31. How do you mentor engineers?
32. How do you handle technical disagreement?
33. How do you make architecture decisions?
34. How do you handle an engineer who repeatedly introduces production
    issues?

## Client

35. How do you handle a difficult client?
36. What if client requirements conflict with your technical
    recommendation?
37. How do you explain a complex technical issue to a non-technical
    stakeholder?

## AI

38. What is your experience with AI/ML?
39. What is an AI agent?
40. How would you design an enterprise agentic AI system?
41. What are the risks of using LLMs in regulated environments?
42. How do you use AI coding tools responsibly?

------------------------------------------------------------------------

# 22. How to Answer at L6 Level

Avoid answers like:

> "I would use Redis."

Instead:

> "I would use Redis here because the access pattern is read-heavy and
> the data can tolerate eventual consistency. I'd use a cache-aside
> strategy with a TTL. The main risks are stale data and cache stampede,
> so I would consider TTL jitter and request coalescing depending on
> traffic."

The difference is:

**Junior/Senior answer:** \> What technology?

**L6 answer:** \> Why this technology, under what assumptions, what
trade-offs, what failure modes, and how do we operate it in production?

------------------------------------------------------------------------

# 23. System Design Whiteboard Template

Use this exact order in the interview:

``` text
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

If time is limited, prioritize:

**Requirements → Architecture → Data → Scaling → Failure → Security →
Observability**

------------------------------------------------------------------------

# 24. 7-Day Preparation Plan

## Day 1 --- Your stories

Practice:

-   Self introduction
-   Current role
-   2M users
-   3x latency
-   Event-driven architecture
-   Security

Goal: answer each in 2--3 minutes.

## Day 2 --- System Design

Practice:

-   scalable API
-   event-driven system
-   real-time system

## Day 3 --- AWS

Review:

-   EC2
-   DynamoDB
-   Redis
-   S3
-   IAM
-   VPC
-   Load Balancer
-   monitoring

## Day 4 --- Coding

Practice 6--8 medium problems:

-   sliding window
-   hash map
-   BFS/DFS
-   heap
-   binary search
-   intervals

Focus on explaining your reasoning.

## Day 5 --- Distributed systems

Review:

-   consistency
-   idempotency
-   retry
-   queue
-   cache
-   rate limiting
-   distributed lock
-   partitioning

## Day 6 --- AI / Agentic AI + client

Practice:

-   agent architecture
-   enterprise AI security
-   hallucination
-   tool calling
-   evaluation
-   client disagreement
-   stakeholder communication

## Day 7 --- Full mock interview

Simulate:

``` text
5 min   Introduction
20 min  Technical / coding
30 min  System design
15 min  Leadership / client
10 min  AI
5 min   Your questions
```

------------------------------------------------------------------------

# 25. Final Cheat Sheet

Before the interview, remember these phrases:

### Ownership

> "I owned the architecture and end-to-end delivery."

### Performance

> "I first investigated the request path and identified the actual
> bottleneck."

### Architecture

> "The decision depends on the access pattern and non-functional
> requirements."

### Scaling

> "I would prefer horizontal scaling and stateless services where
> possible."

### Reliability

> "I would design for retries, idempotency, timeouts and graceful
> failure."

### Security

> "Security should be part of the architecture, not an afterthought."

### Observability

> "I want metrics, logs and traces that allow us to understand both
> symptoms and root cause."

### Leadership

> "My goal is not just to solve the problem myself, but to make the team
> stronger."

### Client

> "I would first understand the business objective behind the
> requirement."

### AI

> "My strongest background is production backend and distributed
> systems, complemented by hands-on ML, GPU computing and AI-assisted
> engineering."

------------------------------------------------------------------------

# 26. The Three Stories to Memorize First

If you have limited preparation time, memorize these three:

## #1 --- WWE Champion

**2M+ users → AWS → real-time → production → 3x latency improvement**

This demonstrates:

-   scale
-   AWS
-   architecture
-   performance
-   production ownership

## #2 --- TheWinZone / Tribe Mania

**Security → event-driven architecture → horizontal scaling →
observability**

This demonstrates:

-   security
-   scalability
-   reliability
-   architecture
-   DevOps/observability

## #3 --- Quant Trading

**GPU → 100M trades → Python/CuPy → TensorFlow → AI**

This demonstrates:

-   advanced technical problem solving
-   Python
-   high-performance computing
-   ML
-   AI

Together these three stories cover a very large portion of the SDE III
requirements.

------------------------------------------------------------------------

# 27. Interview Mindset

Do not try to prove:

> "I know every technology."

Prove:

> **"I know how to take ownership of a complex system, make engineering
> decisions, understand trade-offs, operate it in production, and lead
> others through the problem."**

That is the SDE III/L6 mindset.
