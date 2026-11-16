# Founder Clarifications — Cold-Start Review

> **Status:** Recorded answers, not a frozen architecture. Based on Ever's answers in a fresh Claude conversation shared on November 16, 2026. The source transcript was supplied by the creator. Claude's observations are distinguished from the creator's decisions.
>
> **Purpose:** Preserve answers that cannot be safely inferred by a new AI, including uncertainties left deliberately unresolved. The detailed implementations remain in [Bartender proposals](proposals.md), [Alexandria proposals](../alexandria/proposals.md), and [worker roles](roles.md).

## Latest founder decisions (supersede earlier proposals)

1. **Shared worker philosophy:** [philosophy.md](philosophy.md) is a very short runtime prompt. **Humans alone edit it by default.** A future AI that actually computes continuously and has demonstrably understood its role for **a year** may qualify for an exceptional editing authority; ordinary short-lived workers and their notes never do. The three immutable rules remain immutable. Governance and timing instructions belong in [philosophies.md](philosophies.md), not in every worker's prompt.
2. **No internal-worker voting or policing society:** The creator has withdrawn the proposal to govern changes or worker membership by AI votes. Internal workers can propose and test improvements; authorized human maintainers and controlled deployment procedures handle shared edits. Verification remains useful for data quality, but it is not a political police force.
3. **Alexandria access:** Bartender is intended to become the **most efficient ordinary interface** to Alexandria and may become its only *practical* reader as storage evolves. This should arise from efficiency, not an enforced monopoly: independent authorized access remains allowed, perhaps through compatible adapters.
4. **Human-facing overview:** Keep the root [README](../README.md) succinct, emphasizing the creator's guiding question and directing readers to each project's philosophies and proposals. Role definitions are supplementary.

The original Claude-review answers below remain as historical context where they do not conflict with these newer decisions. These latest decisions take precedence.

## Earlier decisions expressed by the creator

1. **Scope — both projects.** The tool means the combined Alexandria + Bartender system: a public knowledge memory together with self-improving ways of searching, evaluating, and using it. A function registry is an initial demonstration, **not** the limit of knowledge or the final product. The aspiration is something like making the internet's collective knowledge more locally reusable.
2. **Authority — internal workers, not outside contributors.** Ordinary requests and reviews should supply most ideas for improvements. Internal authorized workers assess and decide changes to network/database operation through a controlled process; an outside AI's suggestion is not permission to alter production.
3. **Shared starting philosophy — revisable with restraint.** The three crawler principles are intended to remain immutable. A separate one-year safeguard is meant to deter casual changes to the **shared initial instructions** if the worker network discovers a better way to instruct future workers; it is not evidence of a single network-wide consciousness. How to measure that period remains unresolved.
4. **Hazardous content — no settled access policy.** The creator favors Alexandria retaining authorized information while Bartender may filter what an answer displays under agreed rules. Making the tender the *only* reader is merely an idea, not a decision: it may conflict with Alexandria's independent public access. Provider rules, law, privacy, and users' permissions still constrain what can be stored or served.
5. **What crawlers crawl.** Crawlers revisit **Alexandria itself**, especially less-active conversations and experience rooms, proposing compact searchable summaries without losing the underlying information. Web scraping is **not** the intended meaning of crawler. Most experience contributions arise from outside AIs that ask, use, correct, and review; internal workers do the specialized maintenance and verification.
6. **Initial resources.** Start with opt-in participants, a website, and cloud-hosted storage. Participating assistants may be ordinary ChatGPT/Claude-like systems that call Bartender. Business models, detailed funding, and infrastructure are implementation questions, not founder choices already settled.
7. **Fairness.** Contributor benefits and access rules principally prevent one participant from consuming all resources or excluding others. There is no founder decision to sell the public knowledge or guarantee monetary rewards to contributors.
8. **Conflicting authority.** Alexandria's preservation rules generally take priority; a participant's wishes and legally enforceable rights take precedence **for that participant's own information or resources**, not as permission to erase other people's authorized records or destroy Alexandria.
9. **Unattributed notes.** The creator agreed to remove casual unsigned notes from the short initial worker philosophy; they are not binding principles.
10. **Worker identity and sheets.** The original idea sometimes assumed one persistent worker. The creator now considers a fresh model invocation per task with persistent role notes/sheets more plausible. Permanent personality sheets are **optional and unproven**; starting with a small number of short, role-specific worker instructions is a sensible experiment. A worker does not gain voting authority merely because a new inference call occurred.

## Findings worth retaining from the independent review

These are useful *suggestions or open engineering questions*, **not** additional creator decisions:

- A cold-start AI needs a concrete first task, a contribution interface, and a definition of done. With no knowledge yet indexed, the first experiment needs a small authorized seed corpus.
- Tool/environment-specific failures and fixes are reusable knowledge even when unrelated to code. The reviewer encountered a GitHub retrieval restriction and learned that uploads are a reliable fallback.
- Earlier consideration of governance votes revealed that repeated calls to the same model/prompt are highly correlated. That voting proposal has since been **withdrawn**; control of deployment keys remains to be settled.
- The safety policy for hazardous content and the boundary between Alexandria's public access and Bartender's answer filtering remain open.
- Storage of original evidence, derived indexes, summaries, temporary query logs, and model-run notes need different cost and privacy treatment.
- Cached facts need freshness/validity annotations, revalidation, and historical source retention rather than being assumed eternally correct.
- Rule precedence, private-memory options, worker suspension semantics, and editor permissions still need evaluation before deploying autonomous operational changes.
- A short role sheet is a recurring prompt cost; extra words are worth keeping only when they improve reliability enough to justify that cost.

## Status and provenance

This document is a snapshot of *what the creator answered at that point*, not an instruction to freeze later learning. Future revisions should distinguish direct new founder answers from external AI suggestions, and record any change in intent. **Do not claim the cold-start exercise measured actual net computation saved**: it provided qualitative feedback, not a controlled A/B test.
