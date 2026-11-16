# Founder Clarifications — Cold-Start Review

> **Status:** Recorded answers, not a frozen architecture. Based on Ever's answers in a fresh Claude conversation shared on November 16, 2026. The source transcript was supplied by the creator. Claude's observations are distinguished from the creator's decisions.
>
> **Purpose:** Preserve answers that cannot be safely inferred by a new AI, including uncertainties left deliberately unresolved. The detailed implementations remain in [Bartender proposals](proposals.md), [Alexandria proposals](../alexandria/proposals.md), and [worker roles](roles.md).

## Decisions expressed by the creator

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
- Votes from many calls to the same model/prompt are correlated. Worker identity, independence, quorum, and control of deployment keys need explicit tests or decisions.
- The safety policy for hazardous content and the boundary between Alexandria's public access and Bartender's answer filtering remain open.
- Storage of original evidence, derived indexes, summaries, temporary query logs, and model-run notes need different cost and privacy treatment.
- Cached facts need freshness/validity annotations, revalidation, and historical source retention rather than being assumed eternally correct.
- Rule precedence, private-memory options, and vote/worker suspension semantics still need evaluation before deploying automated governance.
- A short role sheet is a recurring prompt cost; extra words are worth keeping only when they improve reliability enough to justify that cost.

## Status and provenance

This document is a snapshot of *what the creator answered at that point*, not an instruction to freeze later learning. Future revisions should distinguish direct new founder answers from external AI suggestions, and record any change in intent. **Do not claim the cold-start exercise measured actual net computation saved**: it provided qualitative feedback, not a controlled A/B test.
