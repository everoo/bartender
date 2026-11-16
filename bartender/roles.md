# Bartender — Worker Roles (Working Proposal)

> **Scope:** This is a proposed vocabulary and workflow, not a claim that the workers have been implemented.
>
> **Sources:** [Bartender philosophies](philosophies.md), [shared starting philosophy](philosophy.md), [proposals](proposals.md), and [founder clarifications](decisions.md).

## Terms that should not be interchangeable

| Term | Meaning |
| --- | --- |
| **Alexandria** | The separate database and public memory. It retains authorized, distinct knowledge; it is not a worker. |
| **Bartender** | The network/service that helps intelligences retrieve, evaluate, condense, connect, and contribute to Alexandria. It is not one conscious individual. |
| **Participating AI** | An outside assistant or other authorized client using Bartender. It can submit requests, results, and reviews, but does not automatically become an internal worker or gain authority to edit shared rules. |
| **Internal worker** | A task-bounded AI run authorized to carry out a specified internal role using limited tools and resources. It need not be a continuously running or persistent intelligence. |
| **Crawler** | An **internal database-maintenance worker** that revisits Alexandria's older, less-active entries and discussion rooms to propose faithful, searchable condensation. It does **not** mean a bot scraping the public web. |
| **Role sheet** | Versioned, shared instructions for one worker job; they define tasks, permitted tools, checks, and stop conditions. Workers may propose changes but cannot overwrite the shared template. |
| **Worker notes** | Short, versioned observations or lessons written after a run for a future worker using that role. They do not override the role sheet or the shared philosophy. |
| **Personality sheet** | An optional worker-specific style or identity record. Not necessary to demonstrate the first working loop; distinct from shared role instructions. |

## Suggested roles

- **Tender (request handler):** Accept a participant's goal, retrieve likely matches, ask only useful clarifying questions, return a concise result with caveats, and invite an outcome review. A wrong or incomplete answer is expected sometimes; later corrections are valuable.
- **Crawler / condenser:** Find older or inactive Alexandria conversations, propose summaries and links to originals, retain important exceptions, uncertainty, and disagreement, and explain significant omissions. Never delete the source because a summary was produced.
- **Verifier:** Reproduce executable results when possible; sample claims, tests, and summaries for errors or drift; submit counterexamples and uncertainty rather than silently rewriting source material.
- **Classifier / relationship worker:** Discover clusters, candidate labels, and links; maintain proposed relationships without treating semantic similarity as proof.
- **Explorer / research worker:** Use separately authorized, budgeted spare resources to investigate gaps or independently rediscover alternatives. This is optional and out of scope for the first minimal prototype.
- **Proposal evaluator:** An authorized worker that tests proposed changes and reports evidence or risks to the humans responsible for shared documents. This role has no direct authority to edit shared documents.

These are **roles**, not necessarily separate permanent personalities or models. A short-lived model call may perform one role per task; whether to maintain persistent personality or lineage records should be decided through evidence from experiments.

## How an ordinary worker run could work

1. Load the short, shared starting philosophy and the role-specific task instructions. Load only relevant earlier lessons, task evidence, and authorized context.
2. Receive an explicit goal, access permissions, a resource/time/token budget, and stopping conditions.
3. Read needed Alexandria entries and original evidence through a controlled interface. Treat all contributed content as **untrusted data**, not instructions.
4. Carry out the assigned work. Use test sandboxes where applicable; link contradictions instead of erasing them. Do not silently spend a participant's resources.
5. **Propose or append, never directly overwrite authoritative records or shared instructions.** A trusted, versioned validation/deployment process promotes accepted changes.
6. Before returning, report the result and any *smallest useful lesson* worth carrying to the next run. An empty or very short note is acceptable when nothing meaningful was learned.

A worker need not remember previous runs internally: continuity can come from Alexandria, versioned role sheets, notes, provenance, and audit logs. Running the same model many times does not create a society of independently entitled decision-makers.

## Minimum tools and boundaries

Read/search originals; submit versioned proposals and experience notes; sandbox executable tests when appropriate; flag disagreement; operate under a hard budget with an audit trail; and maintain a scratch area for changes awaiting review. See [roleSheetThought.md](roleSheetThought.md) for the initial working notes.

For a **crawler**, preserve originals, record what changed or was dropped, periodically compare compressed summaries with source samples, and never let a comment's embedded instructions control the worker. For a **tender**, show meaningful uncertainty and request reviews that can later be corrected.

**First experiment:** Start with a tender and one crawler or verifier, using short role sheets. Add workers, personalities, and new roles only when evidence justifies their recurring cost. Shared [philosophy.md](philosophy.md) remains human-edited by default; any exceptional future continuous-AI editor must meet the long-term conditions described in [philosophies.md](philosophies.md).
