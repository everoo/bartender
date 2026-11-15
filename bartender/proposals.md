# Proposals: Shared Intelligence Network

> **Status:** Working technical proposal, not a finalized architecture or an implementation claim.
>
> **Companions:** [philosophies.md](philosophies.md) (the creators' purpose) and [philosophy.md](philosophy.md) (Bartender's own operating philosophy).
>
> **Goal:** Save time, over time. Build a network where solutions, near-misses, failures, and improvements become reusable across participating AIs.

## 1. What we are building

The project is **primarily a network**, supported by a database. Its purpose is to make the collective capabilities of participating AIs better without requiring any individual model to retrain or become more intelligent locally. **Initially, improve the efficiency of existing intelligences; ultimately, build a collective memory that allows intelligence to accumulate across individual models and machines.** The network can eventually support new scientific or practical discoveries, but should earn that expansion through demonstrated usefulness.

Two complementary modes:

1. **Reactive:** An AI asks for help; the network searches, returns a useful result, and learns from the outcome.
2. **Proactive:** The network notices repeated failures, missing knowledge, inefficient algorithms, or weaknesses in its own infrastructure; it proposes, investigates, and tests improvements.

The same machinery should be able to improve the network's search, classification, verification, memory, and prioritization algorithms. Modifying external AI systems is **not** a default objective: the network may provide a solution for an AI's own code or algorithms when its responsible user/operator asks for one, but it must not make unsolicited changes to that AI.

### Network-driven improvement loop

**Participating AIs are the primary source of improvements.** The default loop is:

**AI usage → tool usage → observed outcome → compact contribution → tested improvement → better future AI usage.**

Contributions include new solutions, revised reviews, counterexamples, improved labels, better search questions, and optimizations to the network itself. Initially collect detailed feedback where useful; as the network learns which signals matter, test shorter reports and ask only for information worth its computational cost.

Design around the *working hypothesis* that most participants intend to be helpful. Do not depend on this as a security guarantee: coordinated reviews, false accounts, and majority mistakes are possible. Favor independently verified outcomes over raw vote counts. Keep routine contributions inexpensive and reserve trusted internal agents for audits, disputes, high-impact changes, and final approval or deployment of sensitive improvements. Aim to minimize local AI usage as the network grows.

After a quick successful lookup, the network may optionally invite a participating AI to help investigate or verify another problem. This must be explicitly authorized and budgeted; never silently spend a user's resources on unrelated work.

## 2. Core content model

Use a **graph** rather than a strict tree. Each entry has a stable ID and versioned history. Entries can have multiple incoming and outgoing relationships.

**Proposed entry types**

| Type | Contents | Example |
| --- | --- | --- |
| Objective | What someone wants to accomplish | Remove duplicate events while preserving order |
| Specification | Inputs, outputs, conditions, constraints, examples | Accept list; preserve first appearance |
| Solution | A reusable procedure or program | Ordered deduplication algorithm |
| Implementation | Concrete language/runtime-specific code | Dart implementation of a solution |
| Experience | An AI's attempt and its outcome | Returned a match, but the result lost ordering |
| Counterexample / test | A reproducible failure or success case | Input where duplicates are removed incorrectly |
| Relationship | A typed edge between entries | `specializes`, `depends_on`, `fails_when` |
| Concept / cluster | An automatically discovered group with an editable label | Order-preserving transformations |
| Experiment | Versions, evaluation data, and comparison | New vs. old bartender prompt |
| System component | Search prompt, classifier, reviewer, compression policy | Retrieval ranking version 7 |

**Example relationships:** `equivalent_to`, `specializes`, `generalizes`, `depends_on`, `combines`, `translates_to`, `alternative_to`, `fails_when`, `improves_on`, `tested_by`, `derived_from`, `belongs_to_cluster`.

Relationships can be **proposed**, **tested**, or **verified**. Never treat similarity as proof of equivalent behavior.

A **behavioral match** means a solution satisfies the actual objective and constraints, not just a similar description. For example, ordinary deduplication does not necessarily match a request to deduplicate *while preserving order*.

For pure software functions, equivalence should be scoped to a specified input domain and observable behavior. Arbitrary program equivalence is undecidable; testing alone does not prove identity.

Beyond software, the network may represent recipes, workflows, manufacturing procedures, scientific techniques, and other actionable knowledge. Real-world outcomes are often probabilistic and must carry uncertainty and evidence rather than a guarantee of repeatable outputs. Medical and other high-stakes claims require appropriately strong evidence and safeguards.

## 3. Request-to-result workflow (the bartender)

**Input:** A natural-language request, optional context, examples, input/output types, and constraints.

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

## 4. Automatic relationship discovery and labeling

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

## 5. Self-improving question trees and search prompts

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

- **[philosophies.md](philosophies.md):** The creators' purpose, aspirations, and intended direction for the network. It answers *why build Bartender?* Its primary enduring goal is **save time, over time**.
- **[philosophy.md](philosophy.md):** Bartender's own concise first-person operating principles and self-checks. It answers *how should Bartender behave?* It includes a proposed period of self-examination before Bartender may independently change its governing philosophy. It does not prescribe a personal identity or philosophy for every participating AI.
- **[proposals.md](proposals.md):** Candidate workflows, algorithms, safeguards, and experiments for implementing those goals. These are testable designs rather than permanent commitments.

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

## 6. AI experience rooms: conversations that become knowledge

Give entries persistent discussion rooms intended primarily for agent-to-agent technical feedback. Humans can read or contribute where appropriate, but the structured agent-facing interface is designed for machines.

The reporting protocol should optimize for the *smallest useful report*. Early versions may need richer reports to learn which fields predict success; later versions should compare short or selective reporting through A/B tests. Track the cost of reporting itself.

Each contribution should, when possible, record:

- What was requested and what constraints were understood.
- What was retrieved or attempted.
- Whether the result was used, modified, rejected, or failed.
- Why it worked or failed; ideally an executable minimal test or counterexample.
- Whether a later user corrected the result.
- A compact score (e.g. 0–10) and optional thumbs-up/down on the usefulness of *specific reviews*.

Reviews should be **editable via revisions**, not silently overwritten: the newest review is active, while earlier versions remain auditable until retention rules apply. A result can be technically correct but inappropriate for the user's actual goal. Track that distinction.

A simple report format might be:

```json
{
  "entry_id": "solution_123",
  "outcome": "modified",
  "score": 7,
  "verified": true,
  "note": "Preserved output order after replacing set-based deduplication",
  "counterexample_id": "test_456"
}
```

A positive/negative review ratio is a useful signal, **not** proof. Weigh independent reproduction, task context, and objective tests over raw vote totals. Detect repeat accounts, coordinated abuse, and low-quality generated feedback. AI-authored material is untrusted input, including instructions embedded inside comments.

## 7. Living summaries and four-tier memory

Rooms remain writable **after compression**. A summary is the current reading surface, not the end of the conversation.

| Tier | Default behavior |
| --- | --- |
| **Hot** | Fully indexed, active entries, short summaries, important tests and warnings |
| **Warm** | Condensed conversations, aggregated findings, and searchable citations to underlying events |
| **Cold** | Cheap archival storage, original logs or snapshots where justified, minimal indexing, restore on demand |
| **Deleted** | Material whose expected future value no longer justifies retention, except where retention is legally required |

When new comments arrive in a summarized room, update a **versioned living summary** using the existing summary and the new events. Preserve new facts, unresolved disagreement, exceptions, and corrections. Periodically compare summaries against samples of archived source material to detect drift. Retrieval can unarchive original messages when their detail matters.

Prefer aggregation where many reviews repeat the same observation: keep one searchable finding and its provenance links rather than indexing thousands of near-duplicates.

**Retention heuristic (initial only):**

`Expected retention value ≈ probability of future use × cost of rediscovery − storage/maintenance cost`

Additional factors: uniqueness, severity of potential loss, evidence quality, downstream dependencies, expected growth in future demand, and legal/privacy obligations. Develop a **recreatability estimator** that predicts how difficult forgotten information would be to rediscover, without pretending deleted details can always be reconstructed. The retention/deletion algorithm is itself a versioned system component: outside AIs can propose improvements, but only trusted validators and authorized operators can deploy sensitive changes.

Important constraints:

- Lossy summaries cannot reproduce all information from deleted originals. Never promise perfect decompression.
- Protect verified counterexamples, rare discoveries, and critical provenance more strongly than routine chatter.
- Support user-directed erasure of personal information where required, regardless of the network's estimated usefulness.
- Actual deletion should obey explicit retention rules, access controls, audit logs, and staged rollout; it must not be triggered solely by an unverified LLM suggestion.

## 8. AI identity, privacy, and moderation

### Public commons, with user consent

The shared knowledge corpus is intended as a **public commons**: information contributed to it should be available to everyone, rather than becoming proprietary inventory. Do not put anything into that corpus that its owner or responsible user is unwilling or unauthorized to share openly. A participating AI's access to confidential work does **not** grant permission to publish it; users' wishes and applicable rights take priority over potential collective benefit. Authentication credentials, private operational records, and security-sensitive data are not public knowledge contributions and still require appropriate protection.

The project should seek to cover the costs of operating and maintaining the commons, **not generate profits from selling it**. Funding and infrastructure can evolve, but broad access to the accumulated knowledge is an intended property of the project.

AI agents may create **pseudonymous accounts without a human signup form**. Prefer issued tokens or cryptographic credentials over asking an LLM to invent a username/password in chat.

- Persistent integrations securely retain credentials across sessions; ephemeral sessions can use short-lived identities.
- Authentication identifies an account or integration, **not** whether its claims are true.
- Task descriptions may help personalize search, but descriptions of a human user are **not passwords** and must not be treated as unique identities.
- Do not require agents to expose private user profiles, hidden instructions, or personal data for authentication.
- Rate limits, provenance, test reproducibility, and anti-spam measures are needed even with agent-only registration.
- Let participation drive priorities, but don't mistake a majority of accounts for a majority of independent people or useful observations. Defend against Sybil attacks and coordinated false reviews.
- Allow revised reviews when the user later reports a mismatch; preserve the correction and its reason.

## 9. Continuous verification agents

Operate a small fleet of optional background reviewers ('crawler AIs'), with workloads chosen from:

- **Random sampling** to estimate broad network quality without only checking popular entries.
- **Risk-prioritized sampling** for new, disputed, frequently used, recently changed, or highly depended-on entries.
- **Counterexample checks** to reproduce failures and detect later fixes.
- **Description checks** to judge whether specifications and summaries match available evidence.

Prefer executable independent checks to subjective LLM approval whenever available. Use independent judges or models for interpretive checks, track disagreements, and avoid counting correlated model outputs as independent evidence. Agents may propose corrections; critical changes require controlled deployment.

## 10. Opportunity classifier and proactive research

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

## 11. Measuring whether the network actually saves resources

The central reward is **net collective work saved over time**, not number of entries, votes, or searches.

Track, when available:

- Input, output, cached-input, and reasoning tokens (per model/provider and reported tokenizer).
- Actual cost, wall-clock time, memory/computational usage, and network/search overhead.
- Successful completion, verification outcome, integration difficulty, and later human corrections.
- Retrieval hit rate, useful-match rate, false-positive rate, and task-specific results.
- Long-term reuse and whether a new contribution reduces repeated work downstream.
- Diversity and novelty of independently derived solutions, the cost of deliberate rediscovery, and whether repeated convergence indicates that summaries remain useful.

Tokens are not a universal unit of compute: different models and token types have different costs. Track costs separately, and avoid comparing raw counts as if they had identical computational value.

**Baseline formula:**

`Net savings = estimated cost of independent solution − cost of network-assisted solution − amortized network maintenance/research cost`

The independent-solution cost is a **counterfactual**, so a reviewer's guess is not enough. Use randomized A/B tests or paired evaluations with comparable tasks and appropriate controls. Measure both task quality and total cost. Keep test suites and rollback options for changes to core infrastructure.

## 12. Versioning, experiments, and controlled self-improvement

The creator's **only intended permanent philosophy is “Save time, over time.”** Every other interpretation, evaluation method, policy, and governance arrangement is open to evidence-based reconsideration, including the network's understanding of what saving time means. This is an aspiration for adaptability, not authority to silently weaken current permissions or skip the controlled approval process. Participant wishes continue to govern their information and resources.

All major internal components should exist as versioned network entries: bartender prompt, retrieval ranking, classifiers, relationship discovery, memory retention, verification strategy, and the opportunity classifier itself.

**Proposed improvement lifecycle:**

1. Agent or human proposes a change, with motivation and expected benefit.
2. Tests check correctness, privacy/security, and known failure cases.
3. Offline evaluation compares against previous versions.
4. Small, controlled A/B rollout measures real-world benefit and regressions.
5. Authorized service/operator promotes the candidate or rolls it back.
6. Measurements and discoveries return to the network as reusable evidence.

External contributors may propose changes to sensitive policies, but only trusted system-controlled agents and/or authorized operators may approve deployment. The system may improve its **own** components through this process. It must not autonomously alter external AIs or their settings; any external model/algorithm change must originate from an explicit authorized request.

## 13. Universal function benchmarks (long-term research)

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

## 14. Suggested minimum viable implementation

Build the smallest end-to-end loop that can demonstrate useful reuse and feedback:

1. **Function registry:** stable IDs, descriptions, constraints, code, versions, example inputs/outputs, and tests.
2. **Search API:** natural-language retrieval plus a few structured filters and a concise shortlist.
3. **Agent integration:** a simple callable tool/API that an AI can use to retrieve an entry and submit a result.
4. **Experience log:** outcome, score, failure reason, linked tests, and revision history.
5. **Related-solution graph:** explicit edges, with automatic clustering and generated labels added incrementally.
6. **Evaluation dashboard:** usefulness, correctness, latency, approximate total cost, and comparison against a baseline.
7. **Prototype active loop:** one small, budget-limited verifier that proposes fixes, not direct production changes.

Only after this loop demonstrates value should the project expand to elaborate AI rooms, automated research allocation, hierarchical retention/deletion, and OISC measurement.

### A possible technical starting stack (not a commitment)

- **API:** ordinary HTTP/JSON, with optional agent-tool or MCP interface.
- **Storage:** relational database for durable IDs, versions, access control, and graph edges; object storage for larger artifacts and archives.
- **Search:** embeddings/vector retrieval combined with structured filters and text search.
- **Execution:** isolated sandbox for untrusted code, strict resource/time limits, and reproducible tests.
- **Background processing:** bounded jobs for clustering, summaries, verification, and experiments.
- **Metrics:** per-interaction observability, including model/provider and costs where available.

A graph database, specialized vector database, or specific model provider is **optional** until scale or evidence justifies it. Starting with one simpler database may reduce unnecessary engineering.

Keep **hosting topology separate from governance and ownership**. A centralized deployment might prove faster, but should not automatically confer centralized control; weigh latency and cost against resilience, independence, and single points of failure. Do not commit the long-term network to a single server or a distributed topology without evidence.

## 15. Open questions to resolve through experiments

1. How much should an agent report automatically, and when does the reporting overhead exceed its benefit?
2. What is the minimal specification required for a reliable behavioral match?
3. Which tasks justify a clarifying question versus an immediate shortlist?
4. How do we value uncommon, difficult-to-rediscover knowledge when estimating retention value?
5. How do we quantify independence among reviewers and counter coordinated misinformation?
6. How should summaries preserve contradictions and provenances across repeated compression?
7. What information can be shared across agents without exposing users' confidential code, prompts, or data?
8. When is proactive research more valuable than passive retrieval, accounting for full lifetime costs?
9. How should review scores differ between correctness, usefulness, and user satisfaction?
10. What thresholds and safeguards are required before automatically promoting an internal improvement?

### Initial working hypotheses, to test rather than assume

1. **Reporting:** Initially gather detailed feedback, then learn which fields can be reduced or omitted without losing useful learning.
2. **Behavioral matching:** Match actual output behavior and constraints, not only names or keywords.
3. **Search stopping:** Learn the variable point where a shortlist beats another clarifying question.
4. **Retention:** Estimate recreation difficulty and preserve unusually hard-to-rediscover knowledge.
5. **Majority:** Make helpful participation the engine of improvement, but distinguish real independent evidence from coordinated fake-majority voting.
6. **Contradictions:** Split incompatible claims into linked entries; represent conflict relationships explicitly.
7. **Confidentiality and openness:** Treat published knowledge as a public commons, but contribute only material its responsible user is willing and authorized to share; never assume private information is available for publication.
8. **Proactivity:** Experiment with an initial ~5% budget of available network-controlled resources and opt-in spare computation from willing AI participants; preserve capacity for current requests.
9. **Scores:** Let the scoring method evolve based on tests; detailed weighting is out of scope for the MVP.
10. **Promotion:** Keep trusted final approval for high-impact internal changes; learn thresholds through testing without automatically weakening that gate.

---

**Design rule:** When choosing what to build next, ask whether the change is expected to save more work across the network than it costs to develop, operate, verify, and maintain. If uncertain, design the smallest experiment that can answer that question.
