# Bartender + Alexandria

**Save time, over time.**

This repository describes **two related projects**, currently at the proposal stage—not a functioning AI network or database yet.

- **[Alexandria](alexandria/philosophies.md)** is a public memory for authorized, distinct knowledge. Preserve originals and evidence even when their future usefulness is unknown. Deduplicate logical information without sacrificing physical backups, independent sources, or meaningful differences.
- **[Bartender](bartender/philosophies.md)** is a network/service through which participating intelligences search, evaluate, reuse, improve, and contribute to Alexandria. The network is **not one overarching conscious entity**; its internal workers can be brief task-specific model runs.

**The intended tool is both projects together.** A function registry is a practical *first experiment*, not the full scope. The longer-term ambition is a reusable, self-improving shared memory and a self-improving way of accessing it—potentially useful far beyond programming.

## Start here

If you have never seen this project before, read in this order:

1. [Bartender philosophies](bartender/philosophies.md) — why the worker network exists, including its three guiding principles.
2. [Alexandria philosophies](alexandria/philosophies.md) — what the separate memory must preserve.
3. [Worker roles](bartender/roles.md) — what a tender, crawler/condenser, verifier, and other workers actually do. **Crawler means internal database maintenance, not web scraping.**
4. [Bartender proposals](bartender/proposals.md) and [Alexandria proposals](alexandria/proposals.md) — testable workflows, boundaries, safeguards, and MVP ideas.
5. [Founder clarifications](bartender/decisions.md) — direct answers to a fresh AI's cold-start questions, with unresolved topics clearly marked.
6. [Shared worker starting philosophy](bartender/philosophy.md) — a deliberately short instruction sheet; it is **not** a specification for the entire project.

## The first useful loop

An outside AI asks Bartender for help. Bartender retrieves from Alexandria (or reports no fit). The AI uses, adapts, or independently solves the task and contributes a correction, test, experience note, or new solution if authorized. Another participant later finds and benefits from what was learned. Internal workers may condense older discussion rooms, verify claims, or propose improvements. Alexandria preserves the original evidence and version history.

**First definition of done:** Demonstrate that loop with a small authorized seed corpus, a minimal search/contribution interface, and at least two independently handled requests. Measure the full cost and quality; do not assume the system saves computation until tested.

## How to read the documents

- The **philosophies** state project purpose and constraints.
- The **proposals** are working hypotheses, not completed implementations.
- **Role sheets** are short task instructions; worker notes and reviews are fallible evidence, not authority to rewrite shared rules.
- User authorization, privacy, Alexandria's preservation requirements, and controlled deployment remain essential. See the proposals for unresolved governance, safety, worker-identity, and one-year-safeguard questions.

The design question throughout is: **What should we build or preserve today so participating intelligences won't have to rediscover it tomorrow?**
