# Interview Prep Kit — OneByZero SDE III (L6) — First-Round Call
**Candidate:** Pham Thanh An | **Date prepared:** 2026-10-15 | **Language of interview:** English

---

## 1. How This First Call Usually Goes (15–30 min)

First-round calls at companies like this are usually **screening calls**, not deep technical rounds. The interviewer (often a recruiter, delivery lead, or hiring manager) wants to check:

1. **Can you communicate clearly in English?** (JD lists locations incl. Singapore/ANZ/Japan — English is the working language)
2. **Is your experience real and relevant?** (they will probe 1–2 projects from your CV)
3. **Do you understand the role and want it?** (motivation, why OneByZero)
4. **Logistics:** availability, expected salary, location/travel, notice period

➡️ Strategy: **short, confident, structured answers (60–90 seconds each)**. Don't dump everything. Invite follow-up questions.

---

## 2. Your Fit vs. The JD — Know Your Strengths and Gaps

### Strong matches (lead with these)
| JD requirement | Your evidence |
|---|---|
| Production-grade software at scale | 2M+ users (WWE Champion), 1M (Tribe Mania), 750K (TheWinZone) |
| Python deep expertise | Python expert; Django; GPU backtesting (CuPy/NumPy/Pandas) |
| Strong system design skills | HLD for Service Bus horizontal-scaling architecture; event-driven design |
| AWS expertise | DynamoDB, ElastiCache/Redis, S3, IAM, EC2 fleets at 2M-user scale |
| CI/CD, DevSecOps, observability | Jenkins, GitHub Actions; pentest + remediation; custom Grafana plugin; RHQ/Nagios plugins |
| Mentoring & leading workstreams | Technical Leader, mentor a team, design reviews |
| Client-facing in enterprise | Viettel, Vinaphone, Mobifone delivery as direct technical partner |
| AI tooling | TensorFlow models, GPU data pipelines, AI-assisted engineering workflows |

### Gaps (prepare honest, positive answers — don't hide them)
1. **No AWS certification.** JD says "strongly preferred." → Answer prepared in §5, Q9.
2. **Agentic AI / LLM experience is thin.** Their core business is agentic AI for banks/telcos. You have classical ML + data pipelines, not LLM agents. → Position yourself as: "I build production ML/data systems and use AI tools daily; agentic patterns (tool-use, orchestration) map directly onto the event-driven, service-orchestration systems I've built for years."
3. **Regulated industry = telecom (VAS), not banking.** But trading/finance-adjacent work (Quantrading) helps. Emphasize security discipline (pentesting, sanitization, locking) as transferable to regulated environments.
4. **15+ years experience = seniority risk.** An L6 SDE III is "Senior Engineer," not Principal. Show you understand the level and are hands-on, not a pure manager.

---

## 3. Your 60-Second Self-Introduction (Memorize This)

> "Sure. I'm Thanh An, a senior software engineer and solution architect based in Hanoi, with over 15 years of experience building large-scale backend systems — mostly on AWS for the last nine years.
>
> For the past decade at GearInc and Quantrading, I've owned the architecture and delivery of production platforms serving more than two million users — for example, a real-time PVP game platform on AWS with DynamoDB and Redis, running 24/7, where I reduced API latency by up to three times through caching and async processing.
>
> Alongside that, I built a GPU-accelerated backtesting framework that runs about one hundred million trades over ten years of historical data — and a TensorFlow-based prediction model for a live trading bot.
>
> What defines my work is end-to-end ownership: system design, security hardening, observability — I actually built custom Grafana plugins from scratch — CI/CD, and mentoring engineers. I also have a background working directly with enterprise clients in telecom — Viettel and Vinaphone — which taught me how to communicate technical decisions to stakeholders.
>
> I'm now looking for a senior role where I can apply that production-engineering depth to larger, more complex enterprise systems — which is exactly why this role at OneByZero interests me."

*(Practice saying this out loud until it's natural, not memorized-sounding. ~40–45 seconds at normal pace.)*

---

## 4. Core Stories to Have Ready (STAR Format)

Have these five stories polished. Most behavioral + technical questions can be answered from this set.

### Story A — WWE Champion (your flagship: scale + performance + production ops)
- **S:** Real-time PVP platform on AWS, 2M+ total users, 20–25K DAU, 24/7 production.
- **T:** Own backend architecture and delivery; keep latency low under load.
- **A:** Designed Python/C# services on Django, AWS, Photon, DynamoDB, Redis. Profiled production bottlenecks; moved non-blocking work to ExecutorService; added per-node local caching for configuration/blueprint data.
- **R:** Up to 3× reduction in API response time; system stable in production at scale.

**One-liner:** *"On our 2-million-user PVP platform, I profiled production and found the API was blocked by work that didn't need an immediate response. I moved it to an executor and added per-node caching — some APIs got 3× faster, with no extra hardware."*

### Story B — TheWinZone (your flagship: security + DevSecOps)
- **S:** Game backend on Azure, 750K users.
- **T:** Harden security and improve fault tolerance.
- **A:** Led backend pentest research and remediation; automated input sanitization (HtmlSanitizer at the model-binding layer); implemented distributed locking and rate limiting with RedLock on Redis; introduced Service Bus event-driven architecture to decouple sync APIs from async work.
- **R:** Closed vulnerability classes systematically; better scalability and fault tolerance.

**One-liner:** *"I led security hardening on a 750K-user platform — pentest-driven remediation, automated input sanitization, and RedLock-based distributed rate limiting — then decoupled async work onto a service bus, which improved both security and fault tolerance."*

### Story C — Tribe Mania (your flagship: architecture + observability)
- **S:** 1M-user game backend on AWS (Java, DynamoDB, MongoDB, SmartFox, Hazelcast).
- **T:** Make stateful servers horizontally scalable; give ops visibility.
- **A:** Authored HLD for a Service Bus-based architecture enabling on-demand horizontal scaling of stateful nodes; built a custom Grafana plugin for real-time server monitoring.
- **R:** Scalability and availability improved; ops team got real-time observability.

**One-liner:** *"I designed a service-bus architecture so stateful game nodes could scale horizontally on demand, and I built a custom Grafana plugin so the ops team could watch servers in real time — because off-the-shelf tooling didn't fit our model."*

### Story D — Quantrading (AI/data + reusable asset)
- **S:** Strategy research needed massive backtests.
- **T:** Run ~100M trades over 10 years of data.
- **A:** Built GPU-accelerated backtesting framework (Python, CuPy, NumPy, Pandas, MQL5) with parallel execution; later built a TensorFlow gold-price model wired into a live trading bot.
- **R:** Simulation at a scale that wasn't feasible before; reusable engineering asset.

**One-liner:** *"I built a GPU-parallel backtesting engine in Python — CuPy and NumPy — that runs around 100 million trades over ten years of data. It's a good example of how I approach performance: understand the hardware, parallelize the right layer, and deliver a reusable tool, not a one-off script."*

### Story E — Vega / telecom (client-facing + regulated-adjacent)
- **S:** VAS systems deployed at Viettel, Vinaphone, Mobifone.
- **T:** Deliver reliably; act as technical partner to carrier stakeholders.
- **A:** Direct client engagement on requirements; built RHQ/Nagios monitoring plugins and log-analysis tooling; diagnosed and stabilized an unreliable MMS production service.
- **R:** Stabilized production; trusted client relationships.

**One-liner:** *"Early in my career I delivered value-added services directly for carriers like Viettel and Vinaphone — I was the engineer in the room with the client, and I built the monitoring and log-analysis tooling that stabilized a failing MMS service."*

---

## 5. Likely Questions & Suggested Answers

### Q1. "Tell me about yourself."
→ Use the §3 script verbatim.

### Q2. "Why OneByZero? Why this role?"
> "Three reasons. First, the work itself: I read that OneByZero builds agentic AI systems for banks, telcos, and insurers — production systems in regulated environments. That's the hardest and most interesting kind of engineering, and it's where my background fits: large-scale AWS systems, security hardening, observability, working with enterprise clients like Viettel.
> Second, the role matches how I already work — owning architecture end-to-end, setting technical direction, mentoring engineers, and being hands-on.
> Third, honestly, the environment. I use AI tools daily in my own workflow — for code review, debugging, test generation, documentation — and I like that your job description treats that as a core expectation rather than a side activity. It tells me the company is forward-looking about how engineering is actually done."

### Q3. "Walk me through your most complex system."
> "I'd pick the WWE Champion platform. It was a real-time synchronous PVP system — Python and C# services, Django on AWS, Photon for real-time communication, DynamoDB and Redis for state — serving over two million registered users with twenty to twenty-five thousand daily active, running twenty-four seven.
> The hardest problems were latency and state consistency under load. The biggest win came from profiling production and realizing our APIs were doing work synchronously that didn't need immediate responses — moving that to an executor service, plus per-node caching of configuration data, cut response times up to three times.
> What I'd do differently today: adopt a proper event-driven backbone earlier — which is exactly what I did on the next project with a Service Bus architecture."

### Q4. "How do you approach a complex production or performance issue?" (method question)
> "My approach is evidence-driven. First, reproduce or observe with real data — logs, metrics, traces — before touching code. Second, form hypotheses and test them one at a time, changing one variable.
> A concrete example: on the 2M-user platform, I instrumented slow API paths, found blocking calls in the request path, and validated the fix in production by rolling it out gradually and watching latency percentiles — not just averages — because tail latency is what users feel.
> And I treat every incident as an input to prevention: better alerting, better dashboards, or a design change so it can't happen again."

### Q5. "Tell me about your DevSecOps / security experience."
> "Security has been a hands-on part of my role, not a checklist. On TheWinZone I led backend pentest research — I essentially attacked our own system, documented findings, and drove remediation. Two concrete things came out of it: automated input sanitization at the model-binding layer using HtmlSanitizer, and distributed locking and rate limiting with RedLock on Redis to prevent abuse across nodes.
> My principle is that security should be built into the framework and the pipeline — sanitization by default, least-privilege IAM on AWS, secrets out of code — so individual engineers can't accidentally get it wrong."

### Q6. "How do you use AI tools in your daily engineering work?" (JD explicitly expects this)
> "Every day, honestly. I use AI assistants for code review, writing and refactoring code, generating tests, debugging unfamiliar stack traces, and drafting documentation. I treat AI output like a junior engineer's pull request — I verify it, especially around concurrency, security, and performance, because those are exactly where generated code tends to be subtly wrong.
> Beyond assistants, I've built AI systems myself: a TensorFlow price-prediction model integrated into a live trading bot, and GPU-parallel data pipelines for large-scale model evaluation. So I'm comfortable both consuming AI tooling and building ML-driven systems."

### Q7. "How do you mentor engineers?"
> "I do three things. First, design reviews before code — I ask engineers to write a short design doc first, even for medium features, because it forces clarity, and I give structured feedback on it. That's where the most growth happens.
> Second, code reviews as coaching, not gatekeeping — I explain the 'why,' especially around things like concurrency bugs, security, and performance.
> Third, I give people ownership: I hand a capable engineer a module with real production consequences and stay available as a safety net. I've found engineers grow fastest when the system they're responsible for can actually break.
> At GearInc, I mentored the server team this way — and the 3× latency improvements and the monitoring tooling came out of that culture."

### Q8. "Tell me about working with clients / stakeholders."
> "I was the direct technical partner to carrier clients — Viettel, Vinaphone, Mobifone — on value-added service delivery. I learned to translate: client stakeholders care about reliability, cost, and timelines, not design patterns. So I present decisions in terms of risk and trade-offs.
> The most concrete example: an MMS service at Viettel was unstable in production, and the client was frustrated. Instead of debating theory, I built log-analysis tooling, found the root cause, presented the evidence, and shipped the fix. From then on, the client trusted our technical judgments — because we brought data, not opinions."

### Q9. "Do you have AWS certification?" (THE gap question — answer honestly, pivot fast)
> "Not yet — and I know the job description mentions it, so let me address it directly. My AWS knowledge is production-earned: I've operated DynamoDB, ElastiCache, S3, IAM, and EC2 fleets for systems with two million users for almost a decade. That said, I'm happy to get the Solutions Architect certification — I've been meaning to formalize it, and if that's important to the team, I can commit to a timeline. Would a certification within the first few months matter for this role?"

*(That last question flips it — it shows confidence and turns a weakness into a conversation.)*

### Q10. "What's your experience with AI agents / LLM systems?" (possible probe on their core business)
> "My direct work is classical ML and large-scale data pipelines — the TensorFlow prediction model and the hundred-million-trade GPU backtesting framework. I haven't yet shipped a production LLM-agent system, and I'm direct about that.
> But the underlying engineering is the same discipline I know well: these systems are distributed, event-driven, tool-calling services with strict reliability and observability requirements — exactly the shape of the platforms I've built. And I use agentic AI tools daily as an engineer, so I understand their failure modes from the user's side. I'd bring deep production engineering and learn the agentic patterns fast."

### Q11. Behavioral: "Tell me about a failure or a hard decision."
> "The hardest lessons for me have been about over-engineering. Early in my founder period at Koffeestudio, I designed systems with more abstraction than the team and timeline needed, and it slowed us down. Since then I follow a rule: build for the scale you can measure, design the seams so you can scale later. On Tribe Mania, that discipline is why the horizontal-scaling design was an architecture proposal with clear triggers — not premature infrastructure.
> A second hard decision: on TheWinZone, I proposed the Service Bus architecture knowing it would mean short-term rework of working code. I made the case with data — latency and failure-mode analysis — got buy-in, and it paid off in fault tolerance. The lesson: technical change is easy; aligning people around it is the real engineering."

### Q12. "What are you looking for in your next role?" / salary / logistics
> "I'm looking for a senior, hands-on role with real architectural ownership in a company where systems are large, regulated, and matter. On compensation, I'd like to understand the full package — base, and how you structure it for this level — and I can share my expectations once I understand the scope. I'm flexible on location for the right role; I know travel across Southeast Asia is part of the job and I'm comfortable with that."

### Q13. "Do you have any questions for us?" — ALWAYS ask 2–3. Suggested:
1. "Which delivery engagement would I most likely join first — banking, telecom, or insurance — and what does the team look like?"
2. "How do you define success for an L6 SDE III in the first six months?"
3. "How mature are your reusable engineering assets and accelerators? Is there an internal platform team I'd partner with?"

---

## 6. Technical Quick-Fire (in case they probe)

Keep answers 30–60 seconds, with a concrete example from your CV:

- **"DynamoDB experience?"** → Primary data store on WWE Champion & Tribe Mania (1M–2M users); chose it for predictable single-digit-ms reads at scale; watch hot partitions — partition-key design is 90% of the work.
- **"Redis / ElastiCache?"** → Local per-node caching for config data (3× latency win); RedLock distributed locking + rate limiting on TheWinZone.
- **"Event-driven / messaging?"** → Service Bus (Azure) on TheWinZone to decouple async work; SQS-style patterns on AWS; HLD author for on-demand horizontal scaling of stateful nodes.
- **"Observability?"** → Built Grafana plugin from scratch for game servers; RHQ and Nagios plugins for telecom; my rule: monitor what users feel — latency percentiles, error budget — not just CPU.
- **"CI/CD?"** → Jenkins and GitHub Actions; automated build/test/deploy; pipelines as code.
- **"Testing strategy?"** → Unit tests for logic, integration tests at API boundaries, load testing before launches; on real-time systems, chaos-style failure testing for node drops.
- **"Microservices vs monolith?"** → Pragmatic: start with clear modular boundaries; split along scaling and team boundaries, not fashion. WWE started modular; event-driven seams came later.
- **"Rate limiting design?"** → RedLock on Redis for distributed correctness; token-bucket semantics per user/IP; fail-closed for auth paths, fail-open for non-critical reads.

---

## 7. Vocabulary & Phrases to Sound Natural (practice these)

- "Own end-to-end" · "set technical direction" · "drive architecture reviews"
- "production-earned" (for certification question)
- "latency percentiles, not averages — tail latency is what users feel"
- "evidence-driven debugging" · "one variable at a time"
- "translate technical decisions into business risk and trade-offs"
- "security by default, not by convention"
- "design the seams so you can scale later"
- "I'm direct about that" / "let me address that head-on" (for gap answers)

### Avoid these common non-native traps:
- ❌ "I am working in this company since 10 years" → ✅ "I've been working there for ten years" / "I've worked there since 2016"
- ❌ "Do the needful" / overly formal phrasing → keep it simple and direct
- Don't say "maybe" or "I think so" when stating facts about your own CV — say "I designed…" confidently
- Pause instead of filling silence with "ehh" — a 2-second pause sounds thoughtful

---

## 8. Logistics Checklist (have answers ready before the call)

- [ ] Your current/expected salary and bottom line (VND or USD)
- [ ] Notice period / earliest start date
- [ ] Confirm timezone of the call and who you speak with (check email/LinkedIn)
- [ ] Have your CV open in front of you during the call
- [ ] Stable connection + headset; quiet room; water nearby
- [ ] One-page printed summary of §3–§5 next to your keyboard

---

## 9. 20-Minute Practice Plan (do this tonight)

1. Read §3 aloud 3× — record yourself on your phone, listen once, fix pacing
2. Tell Stories A–E out loud, 60–90 seconds each, from memory (bullet points only)
3. Practice Q9 (certification) and Q10 (agentic AI) until the pivot sounds smooth, not defensive
4. Do a mock Q&A with a timer: any question → answer in ≤ 90 seconds
5. Prepare your 3 questions (§5, Q13) — asking good questions is half the interview

**Final mindset:** You have 15+ years and systems at real scale. Your job in this call is not to prove everything — it's to be clear, confident, and easy to imagine working with. Slow down. Smile (it changes your voice). Let them talk 40% of the time.
