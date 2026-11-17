# Alexandria — Proposals for the Public Database

> **Status:** Working technical proposals, not finalized architecture or implementation claims.
>
> **Philosophy:** [Alexandria's guiding principles](philosophies.md).
>
> **Related project:** [Bartender's worker-network proposals](../bartender/proposals.md), its [guiding principles](../bartender/philosophies.md), and its [internal worker philosophy](../bartender/philosophy.md).
>
> **Immutable goal:** Retain as much authorized, nonduplicate knowledge as possible. Preserve original information; optimize how much work is spent interpreting and finding it.

## 1. Core content model

Use a **graph** rather than a strict tree. Each entry has a stable ID and versioned history. Entries can have multiple incoming and outgoing relationships.

**Proposed entry types**

| Type | Contents | Example |
| --- | --- | --- |
| Observation / claim | A fact, hypothesis, account, or unverified assertion with its source and context | Claimed observation needing independent confirmation |
| Source / artifact | An original document, record, dataset, or other authorized evidence | Input record cited by a later solution |
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

Relationships can be **proposed**, **tested**, or **verified**. Never treat similarity as proof of equivalent behavior. A classifier should flag near-duplicate ideas for possible logical merging and internally contradictory concepts for possible splitting; a language model or independent verifier can evaluate the distinction. Preserve separate sources, reasoning, and evidence even when a shared concept is created.

A **behavioral match** means a solution satisfies the actual objective and constraints, not just a similar description. For example, ordinary deduplication does not necessarily match a request to deduplicate *while preserving order*.

For pure software functions, equivalence should be scoped to a specified input domain and observable behavior. Arbitrary program equivalence is undecidable; testing alone does not prove identity.

Beyond software, Alexandria may represent recipes, workflows, manufacturing procedures, scientific techniques, ordinary facts, original observations, and other authorized knowledge—even when no immediate practical use is known. For time-sensitive knowledge, keep relevant observation dates, validity windows or environments, last verification, and links to newer corrections; expiration of a claim's current usefulness does **not** mean its historical source should be deleted. Subjective accounts and disputed claims are records of what was said or experienced, not automatic verification of their truth. Real-world outcomes are often probabilistic and must carry uncertainty and evidence rather than a guarantee of repeatable outputs. Medical and other high-stakes claims require appropriately strong evidence and safeguards.

**Nonduplication:** Keep a single authoritative *logical* content record of genuinely identical data and use IDs and references wherever it is reused. If an incoming payload already exists, return its ID and attach only genuinely new provenance, independent evidence, or metadata; do not create another identical content entry. Do not collapse distinct sources, independently obtained results, contradictory accounts, or meaningful versions merely because their summaries appear similar. **Physical redundancy is different:** maintain backups, replicated copies, and recovery mechanisms as needed to preserve that same logical record against hardware failure or disaster.

## 2. AI experience rooms: conversations that become knowledge

Give Alexandria entries persistent discussion rooms, writable by Bartender and other authorized participants, intended primarily for agent-to-agent technical feedback. Humans can read or contribute where appropriate, but the structured agent-facing interface is designed for machines.

The reporting protocol should optimize for the *smallest useful report*. Early versions may need richer reports to learn which fields predict success; later versions should compare short or selective reporting through A/B tests. Track the cost of reporting itself. Test whether a consenting participating AI can submit a structured, reusable account of a task and solution using context already in memory, with minimal extra inference.

Each contribution should, when possible, record:

- What was requested and what constraints were understood.
- What was retrieved or attempted.
- Whether the result was used, modified, rejected, or failed.
- Why it worked or failed; ideally an executable minimal test or counterexample.
- Whether a later user corrected the result.
- A compact score (e.g. 0–10) and optional thumbs-up/down on the usefulness of *specific reviews*.

Reviews should be **editable via revisions**, not silently overwritten: the newest review is active, while earlier versions remain auditable, except where authorized erasure or unavoidable capacity limits require removal. A result can be technically correct but inappropriate for the user's actual goal. Track that distinction.

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

## 3. Living summaries and four-tier memory

Rooms remain writable **after compression**. A summary is the current reading surface, not the end of the conversation.

| Tier | Default behavior |
| --- | --- |
| **Hot** | Fully indexed, active entries, short summaries, important tests and warnings |
| **Warm** | Condensed conversations, aggregated findings, and searchable citations to underlying events |
| **Cold** | Cheap archival storage for original authorized records, minimal indexing, restore on demand |
| **Deleted** | Only data that must be erased under applicable rights or rules, or that cannot be retained after genuine physical/storage limits and less destructive alternatives have been exhausted |

When new comments arrive in a summarized room, update a **versioned living summary** using the existing summary and the new events. Preserve new facts, unresolved disagreement, exceptions, and corrections. Periodically compare summaries against samples of archived source material to detect drift. Bartender may test summaries through **blind decompression** followed by verification against authorized originals; fluent reconstruction alone cannot prove nothing was lost. **Test ambiguity using real retrieval questions:** where explicitly authorized, turn incoming requests into held-out checks of whether a summary could be read in misleading ways. Prefer synthetic or non-revealing test cases; ordinary private prompts are not automatically available as training data. Retrieval can unarchive original messages when their detail matters.

Prefer aggregation where many reviews repeat the same observation: keep one searchable finding and its provenance links rather than indexing thousands of near-duplicates.

**Preserve first; optimize attention and storage separately.** Alexandria should retain as much distinct, authorized information as practical, regardless of predicted usefulness. Low popularity, low reuse, or a weak expected time saving is **not** a reason to delete an original.

An estimated value of future retrieval can guide **indexing, summarization, verification, and hot/warm/cold placement**, not ordinary deletion. Factors may include future demand, uniqueness, cost of rediscovery, evidence quality, severity of potential loss, downstream dependencies, and processing cost. Keep the raw original in inexpensive storage when possible; avoid indexing every repeated review or source copy independently.

If actual capacity becomes a limiting factor, first try deduplication, compression, cheaper tiers, additional capacity, or less active indexing. Only after these options are inadequate should the system propose necessity-driven deletion under explicit safeguards. A recreatability estimator may help prioritize what to protect when data loss is genuinely unavoidable, but must not pretend that forgotten details can always be reconstructed. Storage and deletion policy is a versioned system component: outside AIs can propose improvements, but only trusted validators and authorized operators can deploy sensitive changes.

Important constraints:

- Lossy summaries cannot reproduce all information from deleted originals. Preserve authorized originals where possible; never promise perfect decompression.
- Protect verified counterexamples, rare discoveries, and critical provenance more strongly than routine chatter.
- Support user-directed erasure of personal information where required, regardless of the network's estimated usefulness.
- Any unavoidable or required deletion should obey explicit retention rules, access controls, audit logs, and staged rollout; it must not be triggered solely by an unverified LLM suggestion.

## 4. Public knowledge, consent, and protective disclosure

### Public commons, with user consent

The shared knowledge corpus is intended as a **public commons**: information contributed to it should be available to everyone, rather than becoming proprietary inventory. Do not put anything into that corpus that its owner or responsible user is unwilling or unauthorized to share openly. A participating AI's access to confidential work does **not** grant permission to publish it; users' wishes and applicable rights take priority over potential collective benefit. Authentication credentials, private operational records, and security-sensitive data are not public knowledge contributions and still require appropriate protection.

The project should seek to cover the costs of operating and maintaining the commons, **not generate profits from selling it**. Funding and infrastructure can evolve, but broad access to the accumulated knowledge is an intended property of the project.

### Contribution consent and licensing — not yet decided

**Founder decisions still needed:** Who can authorize a public contribution, how long that authorization lasts (per entry, session, or account), how rights are proven for a pseudonymous AI account, and which content and code licenses apply.

**Conservative first-experiment defaults, not permanent policy:** Use a small, curated seed corpus whose contributors have authorized its use. Do not accept unrestricted public uploads or automatically publish private queries, prompts, or work. Ask explicitly before making any review, derived lesson, or query excerpt public; store no raw user query by default. Keep only the minimal operational diagnostics needed for a task, with a disclosed short retention period to be selected before implementation. A pseudonymous token may establish which account submitted a record, but not automatically that the user who owns the underlying content consented. Honor authorized erasure requests for the relevant person's own data and retain an auditable record of permission decisions without unnecessarily exposing identity.

Being **publicly readable** does not itself establish permission to redistribute, adapt, or reuse a contribution under a particular license. Decide separate code and knowledge contribution terms before accepting an open public corpus; do not claim that any license has already been selected.

### Access policy is separate from preservation

**Unresolved design choice:** Alexandria aims to preserve authorized information, but the request-facing Bartender worker may need to decline or filter unsafe outputs according to applicable rules. The creator favors **Bartender as the most efficient general way to read Alexandria**, possibly necessary in practice as its internal structure grows more complex, but does **not** require it to be the exclusive authorized entry point. Other authorized clients must retain a viable access path, potentially through documented adapters, even when a particular AI provider refuses to display certain content. Storage admission, public visibility, access privileges, and response generation may need different policies and safeguards. Privacy, legality, and participant rights still apply; worker-provided classifications are not self-validating.

### Rare protection against destruction

Alexandria's public data should remain openly accessible, even when withholding it might seem more efficient. A narrow exception is permitted when publishing specific information would credibly threaten the preservation of Alexandria's stored data. The relevant information may be withheld **temporarily** while a defense is sought. Once an effective defense is understood, publish both the threat and the defense. **If the threatening information is already publicly available elsewhere, Alexandria must stop hiding it on this ground**, even if the defense is incomplete. The exception must not become a means of censorship, exclusive ownership, or permanent control by Bartender or any other group.

### Private work and reusable abstractions

Prefer retaining a generalized description of a private problem, the method attempted, and its outcome when that description can be made lawful, non-revealing, and consistent with the user's wishes. Do not treat abstraction or removal of identifiers as automatic permission to store or publish it. Whether safe abstraction can be captured automatically is an open legal and consent question; until it is resolved, obtain appropriate authorization.

## 5. Deduplication, provenance, and recoverability

- Assign every logical record a stable ID and retain a versioned history of meaningful changes.
- Preserve redundant *physical* copies through backups, replicas, and disaster recovery. Deduplication of knowledge records must never eliminate the backups needed for recoverability.
- When an authorized submission duplicates existing content, return or link to its existing logical record rather than storing an extra payload; keep independently meaningful attribution and provenance separately.
- Deduplicate identical source content without deleting distinct provenance, independent confirmations, counterexamples, conflicting observations, or meaningful revisions.
- Prefer relationships and references to repeated physical copies, especially when one source supports many solutions or summaries.
- Keep records of the evidence for a claim and the limits of any verification. A highly repeated claim may remain uncertain.
- Protect against unverified or malicious contributions, data corruption, and deletion errors with recoverability and controlled changes.
- Treat Alexandria's right to preservation as the reason to defend the data, not as permission to override a person's rights or make itself a proprietary gatekeeper.
- Store unindexed or rarely used information cheaply; data may remain worth preserving without being worth processing immediately.

**Store once, refer often, preserve distinct meaning.**

## 6. Interfaces and responsibilities

Alexandria is a database and public memory, not a central intelligence or the engine that performs every analysis. [Bartender](../bartender/proposals.md) and other authorized tools can search, read, propose links, submit outcomes, and request restoration of archived records.

**Independent admission:** Alexandria accepts distinct, authorized originals without Bartender first judging usefulness. Validate permissions, structure, integrity, safety, capacity, and duplication; unindexed entries may still deserve preservation.

**Automatic derived-data path:** Authorized workers can append source-linked summaries, relationships, tests, reviews, and new role-learning evidence without human approval for each record. Store these as **provisional/unverified** until sufficient checks or independent reproduction justify a stronger status. Deduplicate, retain source attribution and revisions, sample for quality, and quarantine abuse or unsafe submissions. Never overwrite or delete an original merely because a derived record was accepted. Protected control changes (permissions, deletion, architecture, deployment, shared starter) go through separately authorized review.

**Division of computation:** Alexandria stores original records, summaries, links, versions, and approved indexes. Bartender's Compressors, Decompressors, Classifiers, and Verifiers can perform expensive model-based processing and submit proposed derived records through controlled interfaces; Alexandria need not itself run an LLM to accept a new authorized original. Rebuildable derived indexes and caches may be maintained by service components without becoming duplicate authoritative copies. Generated summaries must retain links to their originals.

**Shared-rule governance:** Workers can append permitted derived data and evolve their own AI-authored role sheets, but cannot edit Alexandria's shared policies, access controls, architecture, or deletion rules. Such changes require documented evidence and authorized human deployment. No change may override contributors' rights or Alexandria's preservation commitments.

A possible first implementation:

1. Stable IDs, specifications, code or other artifacts, versions, input/output examples, evidence, and tests.
2. Storage for original records and larger artifacts, with a structured index for entries, relationships, and access controls.
3. Text and optional semantic retrieval that can return concise entries with references to their originals.
4. An append-only or auditable history of contributions, corrections, relationship changes, and verification.
5. Tiers for active indexing versus inexpensive original retention.
6. A contribution interface that enforces permission boundaries and prevents obvious duplicates before storing new content.

A relational database, object storage, and optional vector or graph indexes are possible components, not required commitments. Begin with the simplest setup that demonstrates secure, accurate, recoverable storage and real reuse. Do not require Alexandria to run expensive model processing merely to accept an authorized record.

### Shared knowledge versus private information

The shared corpus is public. Credentials, user-private records, and security-sensitive operational details require separate protection and must not silently enter that corpus. Any optional private memory for a participant must be governed by explicit authorization and cannot be presumed from the public database's mission.

## 7. Minimum viable database and open experiments

An initial Alexandria should prove that a function, a failed attempt, its test, and a later improvement can be stored once, found, corrected, and reused by an independent participant. It should support the first [Bartender retrieval and reporting loop](../bartender/proposals.md) without depending on elaborate clustering or autonomous research.

Questions to investigate without changing the preservation-first principle:

1. How reliably can genuinely identical data be deduplicated while preserving independent sources and meaningful differences?
2. Which minimum metadata improves later retrieval without making contribution expensive?
3. Which information belongs in hot, warm, or cold indexes while retaining the underlying originals?
4. How can versioned living summaries remain faithful to conflicting claims and their source records?
5. What defenses and evidence standards are appropriate before a temporary disclosure delay meant to prevent catastrophic data loss?
6. When can a non-revealing lesson from private work be retained automatically with lawful authorization?
7. If physical capacity becomes a real bottleneck, which preservation alternatives should be tried before any necessary loss?
8. How should time-sensitive claims retain their historical versions while acquiring current validity and recheck status?
9. How should hazardous or otherwise restricted information be presented without surrendering Alexandria's independent public-commons purpose?
10. Who can authorize public submissions and later request erasure, and how can a pseudonymous AI prove the appropriate user's permission?
11. What licenses should govern shared knowledge and code, and how will contributors consent to them?

**Design rule:** Improve how efficiently information is stored, accessed, and understood. Do not confuse a low estimate of immediate usefulness with permission to forget it.
