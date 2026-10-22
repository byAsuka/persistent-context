# Persistent Context

**Keep useful context across AI sessions, without turning guesses into facts.**

A file-backed workflow for AI assistants, extracted from my Raymond Layer project. It carries project state, decisions, source references, and working methods across sessions through a store the user controls.

The implementation here is an instruction protocol and a synthetic worked example. It is not a model-memory SDK or an automatic synchronization service.

## The problem

A long conversation can contain a completed task, an abandoned plan, and an uncertain explanation of what happened. Summarizing them into one paragraph can make an old task appear unfinished or promote a plausible interpretation into a fact. A later assistant then acts on the wrong state.

Persistent Context separates three questions:

| Question | Record |
| --- | --- |
| What do we currently know? | A scoped fact, user confirmation, hypothesis, or unknown, linked to its evidence |
| What should change? | An exact proposal against a known revision |
| Did that change actually persist? | Readback and a transaction receipt; an attempted write is not completion |

## Walk through the example

1. Read [Home.md](Home.md), then [Now.md](Now.md) and [Store_Head.json](Store_Head.json).
2. A synthetic source says a camera calibration task was completed. The store still says `in_progress`.
3. Inspect [update-proposal.json](update-proposal.json). It names one target, the expected revision, the evidence, and the exact change. A proposal alone must not alter the store.
4. Compare [approved-result.json](approved-result.json). This illustrates the expected post-approval state and readback receipt. The files are examples, not a record of a real transaction.
5. Use [SKILL.md](SKILL.md) as a small adaptation starting point for an assistant that can read your chosen store. Configure the real storage location privately.

## Design choices

- **One source of durable state.** Conversation history and local notes may locate evidence; they do not silently override the store.
- **Evidence stays distinct from interpretation.** A third-party claim remains attributed; an inference remains an inference.
- **Completion has stages.** Proposed, authorized, attempted, read back, and verified describe different outcomes.
- **Updates are bounded.** The assistant checks the target revision and writes only the authorized change.
- **Failures remain visible.** A missing readback produces an unverified result and a recovery item, rather than a second blind write.

## My contribution and current stage

I defined the continuity problem, designed the store and update rules, and iterated the workflow through personal use. The private deployment uses a canonical Google Drive store with assistant-side instructions. This public excerpt reorganizes the core ideas into a small, storage-neutral example.

It does not include personal records, store identifiers, credentials, or the full private deployment. Public examples are fictional.

## Limits

The assistant must actually load the relevant files. Instructions alone do not guarantee retrieval, transactional storage, compliance, or persistence across platforms. Multi-file cloud writes are not assumed atomic. This work does not establish autonomous learning, a learned belief state, or improved model performance; those require separate implementations and evaluation.

**Stage:** public protocol excerpt, October 2026.
