---
name: committing-to-git
description: Drafts messages, creates and verifies signed commits, and preflights authorized direct or GitHub PR publication. Use for message drafts, new commits, or their delivery. Excludes history amendment and existing merge, rebase, cherry-pick, or revert operations.
compatibility: Requires a Git working tree, Node.js 24+, Git 2.45+, and configured signing. Required SSH verification needs its configured trust source readable.
license: MPL-2.0
metadata:
  category: development
  version: "0.2.0-rc.5"
---

# Committing to Git

Parse complete JSON (Codex included).
Run `--help --format json` (`schemaVersion` 1, diagnostic contract version 3) to inspect exact options.
Keep `proposal`, `publicationProposal`, and `transaction` opaque; follow `disposition`, `status`, `phase`, `nextAction`, and `recovery`; show `displayText` verbatim.
Follow [diagnostics](references/diagnostics.md); never execute diagnostic prose.

Treat the user's hint as a hypothesis.
Use policy, task evidence, and Git facts to correct type and scope, sharpen the outcome, and add rationale or user-experience consequences.

## State storage and commands

Default memory storage writes nothing before the commit; `commit prepare` returns a `proposal`.
After interruption or `PROPOSAL_STALE` (nothing mutated), prepare again and reapprove.
Prefer supported memory routes (`message finalize --proposal` takes lean stdin JSON; `detailed` never needs disk).
Propose disk only when verified requirements lack a memory route; explain effects and await user consent for an absolute `--state-directory` (no OS temp).
See [state storage](references/state-storage.md).

Resources/verbs: `commit` (`prepare`, `promote`, `create`, `verify-signature`), `message` (`validate`, `finalize`), `review` (`next`), `check` (`run`, `output`), `publication` (`preflight`, `prepare`, `push`, `observe`), `transaction` (`resume`, `extend`, `recover`, `inspect`, `compact`, `purge`).

| Message structure | Memory completion route                                                                                                                                                           |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `subject-only`    | For transport-safe `subject + LF` (`canUseDirectSubjectTransport()`), route is `commit prepare` -> exact approval -> `commit create`; no artifact access between helper calls     |
| `narrative`       | Pipe body without `File Changes:` to `message validate --proposal`, show `displayText`, approve, pipe exact bytes (`--message-sha256`) to `commit create`                         |
| `detailed`        | Default (`File Changes:` <= 32 KiB projected UTF-8 bytes; no file-count cutoff): pipe lean JSON to `message finalize --proposal`, show `displayText`, pipe to `commit create`     |
| `grouped`         | >32 KiB projected inventory or explicit grouping: pipe lean JSON to `message finalize --proposal` (or disk `content.json`), show `displayText`, pipe same JSON to `commit create` |

Disk bodies use `checkedAuthoring.messagePath` (`message validate --transaction`); structured authoring follows [message format](references/message-format.md), `contentContract`, `contentPath`, and `contentTemplate` (else worksheet).
Preserve requested sections; finish required evidence first.
`--draft` authorizes neither staging nor committing; pushing needs separate authority.

## Publication intent before drafting

Distinguish source publication from target integration.
Follow [publication routing](references/publication-routing.md) with `publication preflight --remote <name> [--destination-ref <ref>]`; reuse `feasibility.discoveryReuse` (`--prior-discovery`) in unchanged tasks.
Resolve mandatory feasibility/reviews/checks; prefer direct signed publication, normal PR merge, then disclosed squash.
Rediscover after context/policy changes or rejection; local commits need no discovery.
Optional identity failures never block/prompt; stop guard-denied operations.
Derive scope from task lineage and Git, never from a semantic hint used as a glob, pathspec, prefix, or fuzzy selector.
Ask if two materially different scopes remain plausible; never autocorrect selectors.

## Prepare

Git 2.45+ lets the helper preflight `--no-lazy-fetch` and enforce `GIT_NO_LAZY_FETCH=1`.
Request narrow metadata capability before actual work in read-only `.git`; never delete a live `index.lock`.

| Intent                         | Flag      | Scope                        |
| ------------------------------ | --------- | ---------------------------- |
| Authorized commit (default)    | *(none)*  | `staged`, `full`, or `paths` |
| Propose without touching index | `--draft` | `staged`, `full`, or `paths` |

`staged` keeps partial hunks; `full` selects all changes; `paths` selects whole working-tree paths.
Memory `--draft` never commits; memory `paths` keeps unrelated staging.
Disk actual `paths` needs an empty index; disk `--draft` needs disjoint staging.
Include rename sides.
Inclusion is not review.

| Evidence      | Use when                                                                                                                |
| ------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `reuse`       | Default; specific authored, read, generated, or surviving task-lineage evidence covers full selection (no 32 KiB limit) |
| `inline-diff` | Content you have not seen fits one bounded inline diff (<32 KiB); a hint alone belongs here                             |
| `review`      | User asks, or unseen content or consequential Git facts exceed 32 KiB                                                   |

Do not reread diffs you wrote; use the change summary.
Check `reviewBudget` before paging; on `REVIEW_BUDGET_EXCEEDED`, report the estimate, prefer `reuse` for known files, and continue requested reviews.
Age is not uncertainty.
When a targeted exact-path diff explains a dependency, integrity hash, lock entry, or metadata scalar change, use `inline-diff` with `read-current-task`; do not choose `review` because it predates this turn.
Use exact non-overlapping selections covering the scope for mixed provenance; rationales may overlap, grouped domains cannot.
Scope verification proves selection, message evidence supports claims, and full review inspects content.
Inline evidence must be complete; else use packets.
`task-lineage` needs a specific `--evidence-plan` note; see [recovery](references/inspection-recovery.md#reusable-evidence-plans).
Cross-host evidence needs matching identity, subject, and applicability.

```text
node <skill>/scripts/commitWorkflow.mjs commit prepare [--draft] --scope <staged|full|paths> --evidence <reuse|inline-diff|review> --basis <authored-current-task|read-current-task|task-lineage|user-grounded|generated|unknown-preexisting> [--evidence-plan <file>] [--enforce-review] [--message-structure <detailed|grouped|narrative|subject-only>] [--signature-policy <required|advisory|skipped>] [--path <p> ...] [--scope-file <file>] [--allowed-type <t> ...] [--state-storage disk --state-directory <abs>] [--supersedes <handle>]
```

Loaded repository type policy wins.
Pick the most specific dominant outcome: `feat`, `fix`, `perf`, `refactor`, `docs`, `test`, `build`, `ci`, or `chore`.
Do not routinely scan history; disclose a tie that changes release or user meaning.
Concise eligibility tracks semantic uncertainty; file count never determines concise eligibility.
No path or domain label is an escalation deny-list.
Concise inline evidence never truncates; explicit `review`, >32 KiB diffs, or oversized envelopes route to `extended` (`requiredPacketCount: null`).

## Validate before approval

Validate or finalize before approval; show `displayText` verbatim.
Reapprove only changed bytes or failed-check acknowledgements.

Before presenting any subject for approval, while authoring the first proposal, apply supported skill message policy: description immediately after `: ` must begin with an uppercase Unicode cased letter; optional scope does not change this rule.
Examples: valid: `fix: Tolerate unreachable imports`; valid: `fix(owl2vowl): Tolerate unreachable imports`; invalid: `fix: tolerate unreachable imports`; invalid: `fix(owl2vowl): tolerate unreachable imports`.
On `SUBJECT_DESCRIPTION_NOT_CAPITALIZED`, correct it before showing the message to the user, avoiding a capitalization-only second approval.

Canonical bytes: strict UTF-8, one LF.
Direct `--subject` (no LF) is only for `subject-only` when `canUseDirectSubjectTransport()` holds (disallowed with extended `--enforce-review`).

| Revision           | Invalidation and route                               |
| ------------------ | ---------------------------------------------------- |
| Wording-only       | Revalidate/reapprove prose; reuse tree and evidence  |
| New semantic claim | Read missing evidence delta, then use same finalizer |
| Changed tree/scope | Start a fresh preparation and approval anchor        |

Classify revisions by judgment, not prose matching or embeddings.

## Disk transactions

Disk success consumes fixed input; recreate revisions.
Failure preserves valid state and rejected input.
Extended review is advisory by default (`reviewRequired: false`), including `transaction extend`; `--enforce-review` on `commit prepare` enforces packet review and final `review next` `--review-receipt`.
Trust returned `reviewRequired` and `nextAction`, not null queue/phase (`.transaction/review/` materializes on first `review next`).
Call `review next` cursorless (with `--scope-file`/`--evidence-plan` if externalized), then exact `reviewProgress.nextCursor`; zero packets means complete, cursorless replay is idempotent.
Never open queue paths, hash artifacts manually, or inspect helper source.
See [inspection recovery](references/inspection-recovery.md) for deletion, binary/gitlink limits, corruption, or `--supersedes`; read [disk authoring](references/state-storage.md#disk-authoring) before `nextAction`.
Optional checks must answer a material unresolved question.
Only an unchanged draft may become actual, only through promotion; `commit promote` rechecks attached, detached, or zero-parent unborn head anchor, tree/scope, and staging without committing.

## Commit

Report checks require disk `check run` before approval; inspect slices via `check output --receipt-id`.
Read [check evidence](references/check-evidence.md) for failure authorization, drift, or recovery.
Confirm exact-byte authorization and failed-check receipts.
The helper signs with `git commit`, compares raw commit-message bytes without trimming, and verifies full OID.
See [native execution](references/native-execution.md):

```text
node <skill>/scripts/commitWorkflow.mjs commit create (--proposal <proposal> | --transaction <transaction>) [--scope-file <file>] [--evidence-plan <file>] [--review-receipt <receipt>] [--subject <subject> | --message-sha256 <digest>] [--signature-policy <required|advisory|skipped>] [--acknowledge-failed-check <id> ...]
```

Use returned `commit.arguments` (carries `--evidence-plan` if externalized); `commit.stdinSchema: lean-semantic-json` reads finalized JSON, not `displayText`.

Preserve hook-altered commits; use [signature recovery](references/signature-recovery.md) and `commit verify-signature` for trust/policy limits.
Disk journals preserve unknown outcomes without replay.

## Interpret, recover, and publish

Use [exit outcomes](references/diagnostics.md#exit-outcomes): 0 success, 1 rejection, 2 invalid input, 3 commit with failure, 4 unknown outcome, 5 unmet prerequisite, 6 internal failure.
Preserve known commits; recover without replay (`UNSUPPORTED_ATTEMPT_VERSION` is terminal).
Resume a recoverably interrupted preparation only with `transaction resume --transaction <transaction>`.
Use [transaction recovery](references/transaction-recovery.md) for permissions, locks, `transaction compact`, or `transaction purge`.
Bounded diagnostics point to a complete hashed failure log.
Query count/byte-limited report paths through `transaction inspect`; replay the same cursor or cursorless completed page, using `--refresh` only for a new observation.
Use `--result-detail summary` for concise reports.

Confirm explicit push authority for OID, remote, and `--destination-ref`; reuse [discovery](references/publication-routing.md).
Memory push uses `publicationPreparation.arguments`, or `publication prepare --commit-oid` + `publication push --publication-proposal` (`publication observe` on unknown outcome); see [publication recovery](references/publication-recovery.md#memory-publication).
Disk uses `publication push --transaction`.
PR source push does not finish integration; witnessed success differs from a recovery-time matching remote observation.
Never retry unknown outcomes automatically; `--confirm-no-live-child` requires explicit user confirmation that the child ended or host restarted.
Separately authorized retry uses `--retry-after-attempt`; continue covered fallbacks without duplicate approval.
