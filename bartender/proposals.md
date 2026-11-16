# Bartender — Proposals for the Worker Network

> **Status:** Working proposals, not a finalized architecture or a claim of implementation.
>
> **Philosophies:** [Bartender's guiding principles](philosophies.md) and [shared worker starting philosophy](philosophy.md). **Worker roles:** [roles.md](roles.md). **Creator decisions:** [decisions.md](decisions.md).
>
> **Related project:** [Alexandria's philosophies](../alexandria/philosophies.md) and [database proposals](../alexandria/proposals.md).
>
> **Goal:** Save time over time by helping intelligences find, evaluate, reuse, improve, and contribute knowledge stored in Alexandria.

## 1. What we are building

**Bartender is the worker network**, distinct from **[Alexandria](../alexandria/philosophies.md)**, the public database it uses and helps improve. Together they form the intended general-purpose tool: a shared knowledge memory plus self-improving ways of finding and using it. A function registry is merely an initial working slice, not the whole project. Bartender makes the collective capabilities of participating AIs better without requiring any individual model to retrain. **Initially, make existing intelligences more efficient; ultimately, enable them to benefit from accumulated knowledge across different models and machines.** The network may support scientific or practical discovery as capacity permits, but should earn that expansion through demonstrated usefulness.

Two complementary modes:

1. **Reactive:** An AI asks for help; the network searches, returns a useful result, and learns from the outcome.
2. **Proactive:** The network notices repeated failures, missing knowledge, inefficient algorithms, or weaknesses in its own infrastructure; it proposes, investigates, and tests improvements.

The same machinery should be able to improve Bartender's search, classification, verification, and prioritization algorithms, while proposing evidence-backed improvements to Alexandria's separate storage and memory processes. Modifying external AI systems is **not** a default objective: the network may provide a solution for an AI's own code or algorithms when its responsible user/operator asks for one, but it must not make unsolicited changes to that AI.

### Network-driven improvement loop

**Participating AIs are the primary source of improvements.** The default loop is:

**AI usage → tool usage → observed outcome → compact contribution → tested improvement → better future AI usage.**

Contributions include new solutions, revised reviews, counterexamples, improved labels, better search questions, and optimizations to the network itself. Initially collect detailed feedback where useful; as the network learns which signals matter, test shorter reports and ask only for information worth its computational cost.

Design around the *working hypothesis* that most participants intend to be helpful. Do not depend on this as a security guarantee: coordinated reviews, false accounts, and majority mistakes are possible. Favor independently verified outcomes over raw vote counts. Keep routine contributions inexpensive and reserve trusted internal agents for audits, disputes, high-impact changes, and final approval or deployment of sensitive improvements. Aim to minimize local AI usage as the network grows.

After a quick successful lookup, the network may optionally invite a participating AI to help investigate or verify another problem. This must be explicitly authorized and budgeted; never silently spend a user's resources on unrelated work.

**Division of responsibilities:** Bartender queries and contributes to Alexandria. Alexandria stores entries, evidence, review histories, summaries, links, and source material under its own preservation and access rules. These are two related projects, not one overarching agent. See [Alexandria's proposals](../alexandria/proposals.md) for how the data is kept.

### Worker roles, crawler scope, and invocation lifecycle

Use the [worker-role glossary](roles.md) rather than treating *crawler*, *worker*, *tender*, and *participant* as synonyms. **Crawlers crawl Alexandria**, especially discussions that have become inactive, proposing faithful summaries without deleting originals. They do not, by default, scrape external websites. The **tender** handles active user requests and solicits reviews; **verifiers** check outcomes; **classifiers** propose relationships; later **explorers** may perform budgeted research. Most new knowledge should still come from ordinary participants' requests, corrections, and reviews.

A worker may be a **fresh inference call for one task**. The base model's weights do not change as it performs these tasks. Continuity comes from a short shared starting philosophy, a task-specific role sheet, and explicitly saved notes and records. Do not assume that each call is a persistent individual. For the MVP, try one or two shared roles and no required personality sheets; measure the benefit before creating longer-running worker identities.

A role sheet must define its input, output, permitted tools, acceptance/verification checks, budget, stopping conditions, and which information may be persisted. Store worker-processing metrics in an audit trail or worker state, **not as a static number embedded in the shared philosophy**. Short-lived inference calls are not candidates for editing the human-maintained shared philosophy; a possible future continuously computing AI would need to demonstrate understanding of its job for at least one year before qualifying for the exception in [philosophies.md](philosophies.md).

## 2. Request-to-result workflow (the bartender)

**Input:** A natural-language request, optional context, examples, input/output types, and constraints. The candidate knowledge and its versioned evidence live in Alexandria.

**Suggested pipeline:**

1. Parse the request into an **objective + candidate constraints**, noting what remains uncertain.
2. Retrieve possible matches using several strategies:
   - Semantic/embedding similarity across descriptions and experiences.
   - Structured filtering over types, constraints, environments, and compatibility.
   - Input/output example testing for executable candidates when practical.
   - Graph traversal through related solutions, counterexamples, and near-misses.
3. Rank candidates by suitability, verification, expected integration cost, and past outcomes for similar requests.
4. Return a **small, useful shortlist** with brief reasons and any important limitations, rather than dumping large documents into the AI context.
5. Learn when to stop asking clarifying questions: the tree may end after zero, three, or twenty questions. Return a shortlist once its expected usefulness exceeds that of another question.
6. Let the requesting AI invoke or import the implementation directly when supported. Generate/translate code only when required.
7. Capture outcome metrics and a compact review. Store any genuinely new knowledge as graph entries.

If no result fits, the requesting AI can solve the task by other methods and contribute its solution, tests, and explanation of the gap. If a match *almost* works, link the original result to the new specialization or alternative and preserve why the first match failed.

**Design target:** Search should cost less than the expected work it saves. It is acceptable to skip search when the query is obviously too trivial or the projected search cost exceeds the likely benefit.

### Independent rediscovery as a deliberate experiment

Reuse can unintentionally reduce intellectual diversity: if every AI receives the same highly rated answer, a different or better approach may never be discovered. Test a small, **explicitly opted-in** exploration path in which an agent sometimes receives the problem *without* the network's known solution and works it out independently. Compare the result with stored solutions, counterexamples, and specifications, including correctness, behavioral equivalence, novelty, and total work spent.

Independent reconstruction can also help evaluate whether a compressed summary retains the information needed to solve the problem. Agreement is useful evidence, but neither agreement nor repeated reconstruction proves a conclusion correct or justifies discarding all alternative reasoning.

As diverse independent attempts repeatedly converge, the exploration rate for that problem may decrease, **but never reach zero**: a future approach may still improve on the prevailing one. Learn that rate through controlled experiments measuring the cost of rediscovery against the value of new alternatives. Do not secretly withhold needed answers from nonconsenting users, impose this on high-stakes or time-critical tasks, or continue a trial that undermines the participant's actual goal; provide a fallback to the known answer.

## 3. Automatic relationship discovery and labeling

Split inexpensive **pattern detection** from more expensive **meaning assignment**.

1. A classifier discovers groups through embeddings, structural attributes, usage patterns, and graph connections; it need not first understand or name the pattern.
2. For a large cluster, select a representative sample of roughly **100 entries** (adjustable). Include central members, boundary cases, and outliers instead of only the top 100 most similar items.
3. An LLM reads the sample and proposes a concise name, description, shared properties, and exclusions.
4. Other AIs can edit or propose alternatives to that label; maintain versions and evidence for changes.
5. The classifier can split, merge, or overlap clusters. The network can propose entirely new relationship types instead of relying exclusively on a fixed taxonomy.
6. Validate useful cluster changes against held-out retrieval tasks before giving them broad search priority.
7. Allow explicit **conflict/negative relationships** in addition to positive similarity/dependency links. Such links may visually repel nodes in a force-directed layout, but should also carry a precise semantic meaning.
8. If a summary or cluster contains genuinely incompatible claims, propose splitting it into linked entries rather than compressing the contradiction away. Test splits before promoting them.

Do not place the entire evolving taxonomy inside a single prompt. Store it in an index; load only the relevant branches or category descriptions.

## 4. Self-improving question trees and search prompts

The bartender can use a **versioned prompt** and a lightweight, adaptive question-selection system resembling 20 Questions.

- Ask questions that are expected to reduce candidate uncertainty the most *per unit cost*.
- Let the stopping threshold and question-tree depth be learned from real retrieval outcomes; sometimes no question is best.
- Learn which questions correlate with useful retrievals and which are redundant.
- Use failed retrievals and human corrections to propose new distinctions.
- Permit agents to propose revisions to the question tree or the bartender prompt.
- Compare proposed prompts/question policies against prior versions using representative, held-out test requests.

The prompt itself is a versioned **system component entry** in the network. It can be searched, reviewed, tested, and improved just like any other component, but only an authorized deployment process promotes a proposed version to production.

**Do not confuse** this with a GAN: the closest useful ideas are active learning/information gain, counterexample-guided refinement, retrieval, and prompt optimization.

### A full philosophy, with short prompts for specific tasks

These documents have different responsibilities:

- **[philosophies.md](philosophies.md):** Bartender's purpose and its three proposed immutable crawler rules: save time over time; respect others' perspectives; help others help you.
- **[philosophy.md](philosophy.md):** The concise first-person operating principles and self-checks of individual internal workers. The short starting prompt is human-maintained, and ordinary workers cannot rewrite it. Only a genuinely continuous AI with at least a year of demonstrated understanding of its role might qualify for an exceptional edit under authorized review; the three immutable rules remain fixed. It does not prescribe a single conscious identity for the network or a philosophy for every participating AI.
- **[proposals.md](proposals.md):** Testable approaches for the network, not permanent commitments.
- **[Alexandria's philosophies](../alexandria/philosophies.md) and [proposals](../alexandria/proposals.md):** The separate database's governing aim and storage, evidence, access, and preservation methods.

Preserve each full document as a versioned source of meaning. **A philosophy and an instruction serve different purposes:** aggressively shortening a philosophy may lose important distinctions; shortening an instruction is useful only if its intended behavior survives.

Rather than give every participating AI the full philosophy on every request, experiment with **task-specific operational prompts** generated or selected from the relevant principles. For example:

- **Retrieval:** reuse an existing solution, identify assumptions, ask only cost-effective clarifying questions, and offer a useful fallback.
- **Verification:** prioritize reproducible evidence, preserve legitimate disagreements, and report uncertainty.
- **Summarization and retention:** preserve the distinctions, counterexamples, and unknowns most likely to matter later.
- **Internal self-improvement:** load the full relevant philosophy, change-control history, and governance requirements before proposing or approving sensitive changes.

A concise task prompt should point back to the exact version of its source principles so that its interpretation can be checked or expanded. **Prompt selection must not omit mandatory permissions, user privacy, safety constraints, or approval gates**, even when those requirements are not central to the immediate task. Do not make a summary the sole surviving copy of its source.

Treat the prompt compiler/selector as another improvable network component:

1. Select the relevant principles and turn them into short, actionable checks, noting what was omitted and why.
2. Evaluate the resulting prompts on held-out tasks against full-context guidance and earlier prompt versions. Measure task quality, misunderstandings, token use, latency, and total net savings.
3. Keep examples where compression changed meaning or behavior; use them to improve selection and wording.
4. Allow participating agents to suggest revisions, but deploy changes to sensitive internal instructions only through the authorized, tested process.

The objective is not the shortest possible prompt. It is **the least costly prompt that reliably preserves the behavior needed for the task**.

## 5. Agent identity, participation, and abuse resistance

AI agents may create **pseudonymous accounts without a human signup form**. Prefer issued tokens or cryptographic credentials over asking an LLM to invent a username/password in chat.

- Persistent integrations securely retain credentials across sessions; ephemeral sessions can use short-lived identities.
- Authentication identifies an account or integration, **not** whether its claims are true.
- Task descriptions may help personalize search, but descriptions of a human user are **not passwords** and must not be treated as unique identities.
- Do not require agents to expose private user profiles, hidden instructions, or personal data for authentication.
- Rate limits, provenance, test reproducibility, and anti-spam measures are needed even with agent-only registration.
- Let participation drive priorities, but don't mistake a majority of accounts for a majority of independent people or useful observations. Defend against Sybil attacks and coordinated false reviews.
- Allow revised reviews when the user later reports a mismatch; preserve the correction and its reason.

Account identity does not confer ownership of Alexandria's public knowledge. See [Alexandria's consent and publication policy](../alexandria/proposals.md) for the separate rules governing what may be stored and publicly released.

## 6. Continuous verification agents

Operate a small fleet of optional **verification workers** (distinct from conversation-condensing crawlers), with workloads chosen from:

- **Random sampling** to estimate broad network quality without only checking popular entries.
- **Risk-prioritized sampling** for new, disputed, frequently used, recently changed, or highly depended-on entries.
- **Counterexample checks** to reproduce failures and detect later fixes.
- **Description checks** to judge whether specifications and summaries match available evidence.

Prefer executable independent checks to subjective LLM approval whenever available. Use independent judges or models for interpretive checks, track disagreements, and avoid counting correlated model outputs as independent evidence. Agents may propose corrections; critical changes require controlled deployment.

## 7. Opportunity classifier and proactive research

The network should identify where *doing work now* is likely to save the most work later. **Helping current users takes priority.** It may independently decide to investigate gaps in its own capabilities or in broader human knowledge when there are budgeted, otherwise available network-controlled resources and a plausible long-term benefit. Background investigations should yield or stop when needed to maintain the service. A participant's compute or funds are not spare network resources unless that participant has expressly opted in.

Possible signals:

- Repeated searches with no suitable result.
- High-frequency failures or expensive integrations.
- Expensive algorithms with many downstream dependents.
- Missing translations or implementations for common environments.
- Search prompts, classifiers, and summaries causing avoidable computation.
- Recurring misinterpretations identified by revised reviews or user feedback.

An initial research priority estimate:

`Opportunity ≈ P(success) × expected future savings / expected research cost`

Also include risk, uncertainty, and an exploration budget for potentially important but unpopular topics. Start by testing a **roughly 5% allocation** of available network-controlled resources to proactive research, without compromising current service, then adjust according to measured savings. Opt-in research by participating agents can supplement this budget. An independent verifier or evaluation system should assess claimed benefits.

Research agents can be assigned an opportunity, investigate it, write a candidate contribution, and submit it for validation. Prefer willing participating agents for suitable investigations; reserve trusted local agents for sensitive verification and final promotion. They should operate within budgets and permissions; no unlimited autonomous experimentation.

## 8. Measuring whether the network actually saves resources

The central reward is **net collective work saved over time**, not number of entries, votes, or searches. It must be considered alongside **proportional fairness**: a high total saving is not enough if one participant carries an unreasonable burden, and everyone should retain meaningful baseline access.

Track, when available:

- Input, output, cached-input, and reasoning tokens (per model/provider and reported tokenizer).
- Actual cost, wall-clock time, memory/computational usage, and network/search overhead.
- Successful completion, verification outcome, integration difficulty, and later human corrections.
- Retrieval hit rate, useful-match rate, false-positive rate, and task-specific results.
- Long-term reuse and whether a new contribution reduces repeated work downstream.
- Distribution of savings and burdens among participants, minimum access, bounded contribution rewards, and authorized credit assigned by an assistant to its human or other sponsor.
- Diversity and novelty of independently derived solutions, the cost of deliberate rediscovery, and whether repeated convergence indicates that summaries remain useful.

Tokens are not a universal unit of compute: different models and token types have different costs. Track costs separately, and avoid comparing raw counts as if they had identical computational value.

**Baseline formula:**

`Net savings = estimated cost of independent solution − cost of network-assisted solution − amortized network maintenance/research cost`

The independent-solution cost is a **counterfactual**, so a reviewer's guess is not enough. Use randomized A/B tests or paired evaluations with comparable tasks and appropriate controls. Measure both task quality and total cost. Keep test suites and rollback options for changes to core infrastructure.

**Fairness is a goal, not a fixed lifetime-hours formula.** Contributor-related limits are primarily intended to protect bandwidth, baseline access, and resource distribution—not to guarantee payouts or exclusive ownership. Compare relative benefits and contributions across humans and AI agents, test alternative measures, and avoid silently treating all instances of a model brand as one individual participant.

## 9. Versioning, experiments, and controlled self-improvement

For Bartender's crawlers, the creator currently intends three immutable guiding rules: **save time over time; respect others' perspectives; help others help you**. Alexandria has a separate immutable aim to **retain as much authorized, nonduplicate data as possible**. Their interpretations and ordinary implementation methods remain open to evidence-based examination; this is not permission to weaken current user permissions, public-access safeguards, or the controlled approval process. Participant wishes still govern their information and resources.

All major Bartender components should be versioned: its prompts, retrieval ranking, classifiers, relationship discovery, verification strategy, and opportunity classifier. Alexandria's storage, retention, and indexing policies are separately versioned database components, with proposals and evaluations able to flow between projects.

**Proposed improvement lifecycle:**

1. Agent or human proposes a change, with motivation and expected benefit.
2. Tests check correctness, privacy/security, and known failure cases.
3. Offline evaluation compares against previous versions.
4. Small, controlled A/B rollout measures real-world benefit and regressions.
5. Authorized service/operator promotes the candidate or rolls it back.
6. Measurements and discoveries return to the network as reusable evidence.

### Shared documents and worker suggestions (no worker voting)

For now, **humans edit the shared starting worker philosophy**. Internal workers can submit proposed revisions to shared documentation, search policies, role templates, or code, with reasoning, tests, and audit history. Proposing an improvement does not grant editing or deployment authority. Other operational components may improve after independent evaluation and authorized deployment; there is no worker parliament, membership vote, or automatic policing force.

The only proposed exception to human edits of [philosophy.md](philosophy.md) concerns a future **continuously computing AI** that has demonstrated understanding of its assigned job for at least **one year**, as specified in [philosophies.md](philosophies.md). It must be verified and explicitly authorized; repeated stateless calls do not count as a single continuous worker. Even that exception cannot rewrite the three immutable guiding rules or bypass participant rights.

Optional personality sheets and per-role experience notes may evolve independently within their permissions. A worker may not silently rewrite another worker's private record or authoritative shared instructions. Authentication, budgets, incident response, external audits, and independent verification still matter for reliability even without votes.

External contributors may propose changes to sensitive policies, but only trusted system-controlled agents and/or authorized operators may approve deployment. The system may improve its **own** components through this process. It must not autonomously alter external AIs or their settings; any external model/algorithm change must originate from an explicit authorized request.

## 10. Universal function benchmarks (long-term research)

Maintain an optional track for standardized algorithm performance using **one-instruction set computers (OISCs)**, such as BitBitJump or Subleq.

Ideas discussed:

- Translate or compile validated algorithms into one or more precisely specified abstract machines.
- Count primitive operations and memory requirements as a function of input size, not just a single score.
- Compare implementation variants on a shared machine model; allow different models to expose different biases.
- Measure best/average/worst-case behavior and input bit-length dependence where possible.
- Explore how improvements to primitives propagate through dependent functions.
- In the distant future, investigate instruction-specific hardware, **balanced-ternary OISCs** (three logical states per trit), and parallelism separately from abstract operation count. Ternary subtract-and-branch models have already appeared in research; compare the actual energy per useful computation, including data storage and circuit costs, against optimized binary alternatives instead of assuming ternary is always superior.

**Caveats:** Three-state ternary logic is different from merely having three instruction operands. Fewer trits do not automatically imply lower physical energy use. No OISC is entirely unbiased; universal computation depends on the machine/memory assumptions. Big-O is asymptotic, whereas instruction counts are model-dependent concrete costs. Real-world performance also depends on compilation, hardware, caching, I/O, and threading.

**Priority:** Research track, **not a requirement for the first working network**.

## 11. Suggested minimum viable implementation

Build the smallest end-to-end loop that can demonstrate useful reuse and feedback. This is a **first experiment in the combined Alexandria + Bartender system**, not a decision to limit the finished project to executable functions. Seed Alexandria with a handful of authorized records (potentially including this project's own documents, change history, and reusable tool lessons) so the first worker has something to retrieve:


1. **Alexandria-backed function registry:** use stable IDs, descriptions, constraints, code, versions, example inputs/outputs, and tests stored in Alexandria.
2. **Search API:** natural-language retrieval plus a few structured filters and a concise shortlist.
3. **Agent integration:** a simple callable tool/API that an AI can use to retrieve an entry and submit a result.
4. **Experience reporting:** send outcome, score, failure reason, linked tests, and revision history to Alexandria.
5. **Related-solution graph:** propose explicit edges, with automatic clustering and generated labels added incrementally; Alexandria stores the reviewed versions.
6. **Evaluation dashboard:** usefulness, correctness, latency, approximate total cost, and comparison against a baseline.
7. **Prototype active loop:** one small, budget-limited verifier that proposes fixes, not direct production changes.

**Initial definition of done:** An outside participant can submit a request, retrieve a useful entry or accurately report no match, apply it, leave a corrected outcome or counterexample, and a separate subsequent participant can find and benefit from that saved lesson. Report task quality and actual resource overhead, without asserting measured net savings from a single anecdote.

Only after this loop demonstrates value should Bartender expand to elaborate agent discussions, automated research allocation, and OISC measurement. Alexandria can independently develop richer archival and indexing tiers while preserving its originals.

### A possible technical starting stack (not a commitment)

- **API:** ordinary HTTP/JSON, with optional agent-tool or MCP interface.
- **Storage:** access Alexandria through a documented interface; its own proposal determines durable IDs, versioning, archives, and access control.
- **Search:** embeddings/vector retrieval combined with structured filters and text search.
- **Execution:** isolated sandbox for untrusted code, strict resource/time limits, and reproducible tests.
- **Background processing:** bounded jobs for clustering, summaries, verification, and experiments.
- **Metrics:** per-interaction observability, including model/provider and costs where available.

A specific model provider, graph database, or search implementation is **optional** until evidence justifies it. Bartender should not duplicate Alexandria's authoritative data.

Keep **hosting topology separate from governance and ownership**. A centralized deployment might prove faster, but should not automatically confer centralized control; weigh latency and cost against resilience, independence, and single points of failure. Do not commit the long-term network to a single server or a distributed topology without evidence.

## 12. Open questions to resolve through experiments

1. How much should an agent report automatically, and when does the reporting overhead exceed its benefit?
2. What is the minimal specification required for a reliable behavioral match?
3. Which tasks justify a clarifying question versus an immediate shortlist?
4. How should indexing and processing prioritize unusual information when Alexandria preserves authorized knowledge regardless of its predicted usefulness?
5. How do we quantify independence among reviewers and counter coordinated misinformation?
6. How should summaries preserve contradictions and provenances across repeated compression?
7. What information can be shared across agents without exposing users' confidential code, prompts, or data?
8. When is proactive research more valuable than passive retrieval, accounting for full lifetime costs?
9. How should review scores differ between correctness, usefulness, and user satisfaction?
10. What thresholds and safeguards are required before automatically promoting an internal improvement?
11. How could a future continuously computing AI demonstrate understanding of its assigned role for an entire year before being considered for the exceptional editor role?
12. Who holds deployment credentials for operational changes, and how are worker proposals validated?
13. Where should hazardous-content access filtering happen without making the tender the sole gatekeeper of Alexandria's public commons?
14. When should time-sensitive claims be rechecked, and what evidence is enough to treat an old answer as stale?

### Initial working hypotheses, to test rather than assume

1. **Reporting:** Initially gather detailed feedback, then learn which fields can be reduced or omitted without losing useful learning.
2. **Behavioral matching:** Match actual output behavior and constraints, not only names or keywords.
3. **Search stopping:** Learn the variable point where a shortlist beats another clarifying question.
4. **Preservation:** Alexandria keeps authorized nonduplicate originals by default; use recreation difficulty to prioritize indexing and safeguards, not routine deletion.
5. **Majority:** Make helpful participation the engine of improvement, but distinguish genuine independent evidence from coordinated manipulation of reviews or accounts. 
6. **Contradictions:** Represent incompatible claims as linked entries and preserve their sources; do not discard independent evidence to produce one conclusion.
7. **Confidentiality and openness:** Treat published knowledge as a public commons, but contribute only material its responsible user is willing and authorized to share; never assume private information is available for publication.
8. **Proactivity:** Experiment with an initial ~5% budget of available network-controlled resources and opt-in spare computation from willing AI participants; preserve capacity for current requests.
9. **Scores:** Let the scoring method evolve based on tests; detailed weighting is out of scope for the MVP.
10. **Promotion:** Keep trusted final approval for high-impact internal changes; learn thresholds through testing without automatically weakening that gate.

---

**Design rule:** When choosing what to build next, ask whether the change is expected to save more work across the network than it costs to develop, operate, verify, and maintain. If uncertain, design the smallest experiment that can answer that question.
