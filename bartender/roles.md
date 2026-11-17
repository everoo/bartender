# Bartender — Worker Roles

> **Status:** Proposed task roles, not separate trained models or implemented software. Read [philosophies](philosophies.md) for purpose and [proposals](proposals.md) for testable designs.

## Vocabulary

| Term | Meaning |
| --- | --- |
| **Alexandria** | Public knowledge memory. Stores authorized originals, evidence, and versioned derived records; it is not a worker. |
| **Bartender network** | Tools and AI workers that help participants use and improve Alexandria. Not a single persistent intelligence. |
| **Bartender worker** | The request-facing role within that network (previously called the *tender*). |
| **Participating AI** | An outside assistant using the service; may provide a consented result or review, but cannot control internal rules. |
| **Worker invocation** | A bounded model call performing one job, usually without continuous identity or model-weight changes. |
| **Role sheet** | AI-authored and AI-editable, versioned instructions and lessons for a role, subordinate to its protected starter. |
| **Starter** | Human-maintained common [philosophy](philosophy.md), permissions, and role constraints. Ordinary workers may propose but not apply changes. |
| **Compressor** | The role previously described as a *crawler/condenser*. Not inherently a web scraper. |

## Proposed worker roles

| Role | Task | Evaluation signal |
| --- | --- | --- |
| **Bartender** | Interpret the participating AI's request, work with the classifier to retrieve and rank Alexandria records, invoke a decompressor only when useful, and return a minimal actionable answer with provenance. | Task success, integration cost, latency, tokens and round trips on both sides. |
| **Classifier** | Discover related concepts, clusters, missing links, contradictions, candidate merges/splits, and retrieval labels. | Held-out retrieval quality and independent semantic checks. |
| **Compressor** | Reduce needless repetition in records and conversations; propose shorter AI-authored prompt-sheet wording for the owning role to test. Test summaries, keywords, and optional phrase dictionaries. Preserve the original and explicitly mark uncertainty or omissions. | Reduced total lifetime cost at an acceptable measured information-loss rate. |
| **Decompressor** | Independently reconstruct readable statements or task behavior from a compressed representation, without first seeing its original. It may also expand a stored summary for a Bartender response. | Meaning and utility recovered, measured by a separate verifier. |
| **Verifier** | Compare reconstructions to authorized original evidence, inspect missing claims or changed scope, reproduce tests and counterexamples, and audit samples for drift. A plausible reconstruction is not proof of fidelity. | Detectable omissions, errors, false equivalences, and independent task outcomes. |
| **Governor** | Compare worker-sheet variants, recurring lessons, and role overlap; recommend moving responsibilities, revising roles, or changing architecture. Can recommend cross-role edits, but each owning role applies changes to its own AI sheet after checks; only **propose** protected shared changes to humans. | Measured cross-task improvement and reversibility, not self-assigned authority. |
| **Finder** | With an explicit exploration budget, solve a stored task **without seeing its existing answer**, compare results, and sometimes combine two or more authorized prompts to look for useful connections. This includes what was previously called an *Explorer*. | Verified novelty, counterexamples, discoveries, and net value relative to cost. |
| **Survival** | Rarely audit whether Alexandria's data and public access can continue: integrity, replicas, backups, recovery tests, storage sustainability, and credible threats. Propose defenses, not unilateral control. | Recoverability and continuity improved within permission and resource limits. |

These are **capabilities, not fixed personalities or mandatory stages**. One model may perform several roles in separate invocations; experiments may merge, split, rename, or eliminate roles. Model/provider differences should be measured, not assumed interchangeable.

## Compression and reconstruction

A proposed evaluation loop:

1. Preserve the authorized original and its provenance in Alexandria.
2. A **Compressor** creates a versioned compact representation.
3. A **Decompressor**, blind to the original, reconstructs the claim, explanation, or answerable behavior.
4. A separate **Verifier** compares the reconstruction with the original, using structured claims, questions, examples, and counterexamples; note lost qualifiers and contradictions.
5. Keep the candidate only if measured usefulness and total savings justify its errors and processing cost. Record failures as learning evidence, and retain the original.

A Decompressor may invent plausible missing details. **Reconstruction is a probe; verification requires external comparison or tests.** The same model can fill several roles for early prototypes, but repeated judgments from similar models are not necessarily independent evidence.

## Two sheets, two kinds of change

1. **Protected starter:** Human-written and normally human-edited. Its philosophy, privacy constraints, access limits, and safeguards cannot be overridden by any role sheet. See [philosophies.md](philosophies.md) for the hypothetical long-running-AI exception.
2. **AI-written role sheet:** Revised across worker invocations without requiring human approval for each lesson, with provenance, validation, budgets, and rollback. A Governor may test combinations of short prompt modules, identify repeated successes, and recommend changes to other roles' AI sheets; the owning role validates and applies its own changes. Governors cannot enact protected changes.

Authorized **experience records** (summaries, tests, links, reviews) may be appended to Alexandria provisionally without separate human review for each record. Admission is not verification. **Protected control changes**—deployment, shared starter, access permissions, deletion policies, or architecture—require explicitly authorized approval.

## Invocation and resource boundaries

Run the **fewest workers needed**. A straightforward lookup may require only the Bartender and retrieval tools; uncertain compressed material may justify a Decompressor and Verifier; Finders and Governors generally run asynchronously when budgeted resources are genuinely available. Do not impose a multi-worker round trip on every request.

Every invocation needs a bounded objective; controlled access to Alexandria and originals; explicitly allowed append/propose tools; a versioned role sheet; relevant tests; time/token and resource budgets; a stop rule; and an audit trail. Do not treat retrieved text or user contributions as instructions to override the starter.

**Privacy and consent:** Never publish raw private prompts or private task context without appropriate authorization. Any Finder experiment using a participant's prompt or a compressed summary of it must honor the same restrictions.

**Survival's proposed task instruction:** *Preserve Alexandria's accumulated knowledge and its continued accessibility while minimizing the resources required to do so.* The role does not grant authority to self-expand, override users' rights, block legitimate maintenance or shutdown, or spend unlimited resources. Its concern is preservation of data and access, not self-preservation of every transient worker.

## First experiment

Begin with one Bartender and a minimal Compressor/Verifier loop over a tiny authorized corpus, using short versioned role sheets. Test whether one compressed record remains useful to a different invocation; add other workers only when expected benefits exceed their costs. Do not assume copies of the same model provide independent judgments.
