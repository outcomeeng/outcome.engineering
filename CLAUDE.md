<!-- SPEC-TREE v0.30.0 langs: -->

<operator_question_interrupt>
**OPERATOR QUESTION - IMMEDIATE PRIVILEGE REVOCATION:** When the operator asks a question, immediately relinquish all privileges to modify the current product or any external file, service, or resource. Answer the question immediately.

- ALWAYS: stop any running non-verification process that is destructive or modifies files, external resources, or state.
- NEVER: stop a running verification process — including agentic verification, tests, or evals — unless the operator explicitly instructs that process to stop.

</operator_question_interrupt>

# Spec Tree Instructions

These instructions explain WHEN to invoke spec-tree skills for this product. They are a **router** — the skills contain the HOW.

**Read this entire file before acting.** This managed router block is only the first section of the file; the product's own instructions, commands, and conventions follow it below, outside the router. The router is product-neutral by design and does not carry this product's own commands — they live in the file's own content further down. Never act on the router alone; read every section of this file to the end.

---

## Authority Hierarchy

**⚠️ BELOW THE OPERATOR, SKILLS ARE THE TOP-LEVEL AUTHORITY. SKILLS ARE CENTRALLY MANAGED AND CURRENT; REPOSITORY CONTENT GOES STALE.**

- **ALWAYS** apply authority in this order: active skills → repository decisions and specs → tests → code. When repository content conflicts with an active skill, the skill wins.
- **ALWAYS** follow active skill instructions, templates, and bundled references over repository examples, existing files, comments, or copied conventions.
- **NEVER** weaken a higher layer to match a lower layer. Fix the lower layer when the layers disagree.
- **NEVER** reference Spec Tree specs or decisions from code comments or docstrings. Code contains no `spx/...` paths, ADR/PDR identifiers, or decision-file references.
- **ALWAYS** let the active skill load the matching `spx/local/*.md` overlay when that skill declares one. The overlay supplies repository-specific values and commands below the skill in authority and cannot replace, weaken, or contradict the skill.
- **ALWAYS** read the active harness guide in every directory before working there when the guide exists: `CLAUDE.md` for Claude Code, `AGENTS.md` for Codex.

### Dangerous-command guard

🛑 **STOP TRIGGER — a dangerous-command guard (DCG) block terminates the attempted command family.** Treat the blocked attempt as a mistake.

- **NEVER** retry it by reformulating, splitting, rewriting, removing the flagged clause, or substituting an equivalent command to evade the guard.
- **ALWAYS** follow the active skills, repository instructions, and declared overlays to find a sanctioned operation that accomplishes the goal.
- When no sanctioned operation exists, abandon the goal, report the blocked command with secrets redacted, explain its purpose and the guard's reason, ask the operator for direction, and stop.

---

## Product Commands

The product's operational command for each spec-tree phase lives in this file's own content below the router, not in the router itself. Read the whole file to find each one:

- **author** — after a create, update, or delete on a spec, test, or implementation file, run the product's author command to rebuild or regenerate artifacts.
- **verify** — for `/apply` and pre-merge checks, run the product's verify command over the node and the changeset.
- **gate** — for the full deterministic bundle, run the product's gate command.
- **merge** — for the transport step of `/merge`, run the product's merge command.

Content the product keeps identical across `CLAUDE.md` and `AGENTS.md` sits in a `shared` region — `<!-- SPEC-TREE:shared {name} -->` … `<!-- /SPEC-TREE:shared {name} -->`, present in both files under the same name. `/update-instruction-block` keeps a `shared` region in sync by taking the git-more-recent side; it never merges the two bodies.

---

## When to Invoke Skills

### Before product-content access -> `/understand`

**BLOCKING REQUIREMENT**

Require a live `<SPEC_TREE_FOUNDATION>` marker before directly reading, searching, listing, or changing anything under `spx/` or any source or test file. Invoke `/understand` when the marker is absent. This includes repository-content access through Read, Edit, Write, Glob, Grep, `rg`, `grep`, `find`, `cat`, `sed`, and Git commands that emit file contents or patches.

`spx session` operations — including inspection, archive, and release — plus `spx worktree status`, `spx diagnose`, and no-patch Git status, history, and topology are exempt. Never follow paths from their output into repository content without the marker.

A compacted summary, session file, statement that `/understand` ran, or read of the skill file does not prove the foundation is live. After every compaction, require `/understand` again before the next product-content access.

### Before working on a specific node -> `/contextualize`

**BLOCKING REQUIREMENT**

**ALWAYS** invoke `/contextualize` before working on a spec node.

`/contextualize` MUST invoke `/sync-base` and receive `already_current` or `rebased` before reading product truth. `/sync-base` owns the complete currency operation: fetch, clean rebase or detached advance, session-authorized dirty-tree checkpointing through `/commit-changes`, and same-invocation retry. Callers consume its final result; they never duplicate branch creation, commit, stash, or retry logic, and they never reinterpret `dirty_tree` as a rebase conflict.

**🛑 STOP TRIGGER — after every compaction event:** all loaded spec-tree context is gone. **Re-invoke `/contextualize` on every node still in scope** before touching it again — not just the next one being worked on.

**NEVER** resume work on a node without having invoked `/contextualize` since the last compaction.

### When creating specs or nodes -> `/author`

Create product specs, ADRs/PDRs, enabler nodes, outcome nodes.

### When composing or breaking down nodes -> `/decompose`

Compose top-level children with `/decompose spx/`. Decompose an existing node when it has too many assertions (>7), contains independent concerns, or has `PLAN.md`/`ISSUES.md` structure intent.

### When restructuring the tree -> `/refactor`

Move nodes, re-scope assertions, extract shared enablers, consolidate duplicates.

### When checking consistency -> `/align`

Review, audit, or quality check specs. Find contradictions or gaps.

### Before tests, evals, builds, or validation -> `/wait-for-load`

🛑 **STOP TRIGGER — Before any test, eval, build, or validation command, ALWAYS invoke `/wait-for-load`.**
**ALWAYS** wait for `ready: true`, then run the selected command unchanged.
**NEVER** use host load to reduce scope, workers, limits, deadlines, or verification.

### When shipping work to the default branch -> `/merge` (transport dispatcher)

**BLOCKING REQUIREMENT**

Every change destined for the default branch routes through `/merge`, the transport dispatcher — it classifies the changeset, selects the transport, and delegates. `/merge` reads `spx/local/merging.md` as a repo-local overlay **when that file is present**; the overlay is optional, so its absence is normal and not a blocker — `/merge` applies the default lifecycle. `spx/local/merging.md` is the one place repository-specific merge behavior belongs: never infer the transport from other docs when it is absent, and never edit this generated instruction block to change merge behavior — invoke `/merge` and let the lifecycle apply the defaults. The four authority gates, the delivered-value boundary, and the finding-disposition rule are transport-neutral and live in `/merging-standards`.

## Stop Triggers

Default-branch work is complete only when it reaches the default branch on origin through `/merge` — passing validation, tests, review, or audits is progress, not a stopping point, and an accepted proposal ("yes", "go", "do it") authorizes the whole lifecycle, not a pause. Each trigger below resolves the same way: finish the remaining independent work, then continue through `/commit-changes` and `/merge` until the change reaches the default branch on origin or an explicit lifecycle gate stops.

🛑 **About to summarize after edits, validation, tests, review, or audits passed** — do not conclude. Ensure the work is committed on a local branch, then drive `/merge`.

🛑 **About to report blocked, wait, or ask a question** — first do every action that does not need the answer: edits, verification, branch setup, commit, review. A blocker exists only when all three hold:

- the immediate next action cannot proceed without the operator or an external-state change;
- the local branch already holds every change makeable without the answer;
- the applicable gates have run or produced concrete failing evidence.

🛑 **About to finish on a detached HEAD or stop at a fresh commit** — `git status --short --branch` reporting `## HEAD (no branch)`, or a new local commit, is not an endpoint. Create or switch to a local branch preserving the worktree changes, then continue through `/merge` unless the user explicitly limited the task to local-only work.

## Checkpoint Commits

`/commit-changes` may create an atomic local checkpoint whenever a coherent concern is ready to preserve, independent of verification state. Record the latest state as `passing`, `failing`, or `not-run`; that state controls later gate dispatch, never commit permission. Run hooks normally, confirm the full `HEAD` changed, and report committed paths, remaining paths, and verification state. Never strand dirty work merely because verification fails or has not run.

## Worktree Occupancy

Before treating any worktree as available, run `spx worktree status` and require a live claim for the exact absolute worktree root and current native session. Refresh this proof at session start, after restart or compaction, and immediately before any checkout or worktree transition. A clean tree, detached `HEAD`, branch name, pane title, or absent process in one view never proves availability. When the exact root is absent or claimed by another live session, remain in the assigned worktree and record the ownership issue instead of entering the sibling checkout.

## Git Safety Protocol

```text
ALLOW  git checkout -- README.md
ALLOW  git checkout HEAD -- .
ALLOW  git restore README.md

DENY   git stash drop
DENY   git stash drop stash@{3}
DENY   git stash pop
DENY   git stash pop stash@{0}
DENY   git stash clear
```

## Mutation Status Updates

Before proposing or performing a repository mutation, name:

- the exact target path, PR number, branch ref, or command target;
- the intended action;
- why the action is local enough or gate-authorized enough to proceed;
- the next validation command, review, audit, check wait, or merge gate the action feeds.

Avoid shorthand such as "config patch", "direct patch", "fix the PR", or "ship it path" when the exact file, PR state, or command is known. A terse user prompt such as "check", "continue", or "ship it" still gets the live state first: full head SHA when a PR exists, current-head review state, required-check state, deployment-readiness and release-readiness rules, and the next autonomous action.

## Quick Reference: Skills and Agents

Skills run in the main conversation. Agents preload the skill and run autonomously as subagents in a separate context. Audit agents return structured verdicts; changeset reviewer agents return the raw review journal token for the main conversation to inspect and process through the governing review workflow. **ALWAYS run an audit through its agent** — the separate context keeps the verdict free of the main conversation's bias — and dispatch agents in parallel when auditing multiple targets.

**Use the `Agent` tool for every configured verifier or reviewer.** Launch in the foreground with `subagent_type` set to the exact configured agent type and `prompt` set to the role-task body from the shared contracts below. The completed `Agent` tool result is that configured agent's final message; apply the matching output contract to that message. An error, missing final message, or output outside the matching contract blocks the gate.

**Inspect every successful `changes-reviewer` result through the sealed journal.** Invoke the `spec-tree:project-run-journal` skill and use its `render_review_run.py <run-token>` helper. The helper calls `spx journal render --type review --run <run-token>`, resolves a not-found current-scope miss through `spx journal list --type review --sealed sealed --limit 200`, re-renders with the listed branch slug when exactly one sealed run matches the token, reads the sealed event prefix, and prints the raw token, terminal status, full head/base identity, scope coverage, blocking/debt counts, and any findings through `render_surface(events)`. Treat this as journal inspection; the sealed prefix remains the only review result.

**Configured verifier and reviewer role-task contracts.** Supply only the fields named for the role:

- `changes-reviewer`: the raw scope token — `HEAD`, `origin/<base>...HEAD`, a branch, or a PR reference. Its final message MUST be the raw sealed review-journal run token.
- `implementation-auditor`: repository path, exact committed `<base>..<head>` scope, no live file list for a gating audit, governing node paths, deterministic verification commands and results, and the task to run the implementation audit through `spx verification run`. Its final message MUST carry the raw run token and rendered projection; only `terminalStatus: approved` passes.
- `test-evidence-auditor`: repository path, governing node, full assertion text or exact spec path plus headings, test-file paths, and the task to audit coupling, falsifiability, alignment, and coverage without weakening the evidence type. Its final message MUST be the `spec-tree:audit-tests` JSON verdict with `schema_version: 1`, `skill: "audit-tests"`, `overall: "APPROVED" | "REJECTED"`, `rows`, and `metadata`, with no prose outside the JSON object. Treat `overall` as authoritative. Malformed JSON, a missing required field, an unexpected `skill`, or an `overall` value outside that vocabulary blocks the gate.
- `eval-evidence-auditor`: repository path, governing node, `[eval]` assertions, all eval artifacts, producer artifacts, and the task to audit real-producer evidence. Its final message MUST be the audit-eval-evidence JSON verdict with overall `PASS`, `FAIL`, or `UNKNOWN` and no prose outside the JSON object.
- `spec-auditor`: repository path, full node path, and the task to audit assertion quality, evidence tags, atemporal voice, decision alignment, and structure. Its final message MUST be `APPROVED` or `REJECTED`; rejection lists concrete findings with full paths, governing rules, and required fixes.
- `adr-auditor` or `pdr-auditor`: repository path, full decision path, governing node, committed audit scope, and the role's decision-audit task; ADR tasks also carry the language-scope classification. The final message MUST follow that auditor's structured verdict contract without a competing prose envelope.
- `skill-auditor`, when that configured role is installed: repository path, full paths to every changed artifact governing the skill surface — including skill-directory files, authored shared fragments, and generated runtime copies — governing nodes when known, deterministic verification state, and the skill-authoring audit task. Its final message MUST be the `instructions:audit-skills` JSON verdict with `schema_version: 1`, `skill: "audit-skills"`, `overall: "APPROVED" | "REJECTED"`, and the `keep-these-aspects`, `worth-improving`, and `must-fix` rows. Treat `overall` as authoritative. Malformed JSON, a missing required field or row, an unexpected `skill`, or an `overall` value outside that vocabulary blocks the gate.

- `subagent-auditor`, when that configured role is installed: repository path, exactly one changed subagent configuration path in the active agent harness's native format, governing nodes when known, deterministic verification state, and the subagent-authoring audit task. Multiple changed configurations require separate `subagent-auditor` dispatches, one per path; acquire their handles sequentially and let their role tasks run concurrently. Each final message MUST be the `instructions:audit-subagents` JSON verdict with `schema_version: 1`, `skill: "audit-subagents"`, `overall: "APPROVED" | "REJECTED"`, and the `critical-issues`, `recommendations`, `strengths`, and `quick-fixes` rows. Treat `overall` as authoritative. Malformed JSON, a missing required field or row, an unexpected `skill`, or an `overall` value outside that vocabulary blocks the gate.

| User Says...                               | Skill                  | Agent                   |
| ------------------------------------------ | ---------------------- | ----------------------- |
| "Implement this outcome"                   | `/apply`               | `applier`               |
| "Create an outcome"                        | `/author`              | —                       |
| "Add an ADR"                               | `/author`              | —                       |
| "Add a new node" or "This node is too big" | `/decompose`           | —                       |
| "Move this under that"                     | `/refactor`            | —                       |
| "Check these specs"                        | `/align`               | —                       |
| "Establish evidence for this"              | `/verify`              | —                       |
| "Write tests for this"                     | `/verify`              | —                       |
| "Start the TDD flow"                       | `/apply`               | `applier`               |
| "Audit this PDR"                           | `/audit-pdr`           | `pdr-auditor`           |
| "Audit this ADR"                           | `/audit-adr`           | `adr-auditor`           |
| "Audit test evidence"                      | `/audit-tests`         | `test-evidence-auditor` |
| "Audit eval evidence"                      | `/audit-eval-evidence` | `eval-evidence-auditor` |
| "Audit this spec node"                     | `/audit-specs`         | `spec-auditor`          |
| "Diagnose the spx environment"             | `/diagnose`            | —                       |
| "File a follow-up in a dependency queue"   | `/issue`               | —                       |

Per-language code, architecture, and test audits ship as `audit-{lang}-{code|tests|architecture}` skills that generic artifact-type auditors compose for the language in scope. There is no per-language auditor agent. Dispatch `implementation-auditor` for implementation audits; it invokes the matching language concern skills automatically:

---

## Test Naming Convention

Test level is encoded in the filename. The `{evidence}` segment is chosen by `/test` routing from the assertion type: `scenario`, `mapping`, `conformance`, `property`, or `compliance`. Universal assertions use `mapping`, `conformance`, `property`, or `compliance`; a universal is never `scenario`. This instruction block renders only the languages recorded in its opening `<!-- SPEC-TREE v{version} langs:{list} -->` marker; `/update-instruction-block` re-renders from the installed template when the methodology advances.

---

## Session Management

Sessions are shared across every worktree. Each session must be handed off via `/handoff` so it can be resumed from any other worktree: the handoff leaves the worktree clean and persists all state on origin. Propose a handoff when the session's goal is met or the work must pause; resume one with `/pickup`. When a claimed session is complete and should leave the active queue, close it through `/handoff` or `/handoff --no-session` so claimed-session accounting archives it. To return a wrongly claimed session to the shared queue instead, run `spx session release <session-id>`.

An explicit request to inspect, archive, or release identified session documents routes directly through the corresponding `spx session` command as operational-state management. Reserve `/handoff` for closing active work through reflection, persistence, continuation disposition, and claimed-session accounting. Direct session operations require `/understand` only before following their output into `spx/`, source, or test content.

<!-- /SPEC-TREE -->

<!-- SPEC-TREE:shared root -->

# Outcome Engineering — website

The Next.js application that renders and deploys the Outcome Engineering methodology. The methodology itself is authored in a separate repository; this repository publishes it.

- **Repository**: <https://github.com/outcomeeng/outcome.engineering>
- **Landing page + blog**: outcome.engineering — Next.js, deployed on Vercel via git integration
- **Docs**: docs.outcome.engineering — Mintlify, sourced from `mintlify/`

## Commands

| Command                    | Purpose                                                                           |
| -------------------------- | --------------------------------------------------------------------------------- |
| `pnpm dev`                 | Dev server (Next.js + Turbopack)                                                  |
| `pnpm build`               | Production build                                                                  |
| `pnpm start`               | Serve the production build                                                        |
| `pnpm lint`                | Next.js ESLint pass                                                               |
| `pnpm validate`            | Validation gate without the circular check — needs no build output                |
| `pnpm circular`            | Circular-dependency check — requires a completed `pnpm build`                     |
| `pnpm validate:repo`       | Full gate including circular — requires a completed `pnpm build`                  |
| `pnpm logo:generate`       | Regenerate logo assets from `assets/generate/generate_logos.py`                   |
| `python3 -m pytest tests/` | Python tests for the asset generation scripts — run manually, outside the CI gate |

`pnpm validate` must be clean before every commit. It resolves through the repository's own `@outcomeeng/spx` devDependency, so every contributor and every CI run use the same gate. The circular check reads `.next/types`, which only a completed build produces, so `pnpm circular` and `pnpm validate:repo` require `pnpm build` first. `docs/publication/site-architecture.md` names `pnpm validate:repo` as a required pull-request gate.

The pytest suite under `tests/` is deliberately outside CI. It sits in an ungoverned location that predates any spec tree, and `tests/unit/test_shell_wrapper.py` asserts against a `scripts/generate-logos.sh` wrapper the repository does not contain. Treat a regression in `assets/generate/` as uncovered by automation until that suite is relocated under a governing node and its failing cases are resolved.

### Spec-tree phase commands

The router above directs a reader here for the product's command per spec-tree phase:

| Phase      | Command                                                                                                               |
| ---------- | --------------------------------------------------------------------------------------------------------------------- |
| **author** | None. No generated spec-tree artifact exists to rebuild until a spec tree is bootstrapped under `spx/`.               |
| **verify** | `pnpm validate` for the changeset; add `pnpm build && pnpm circular` when the change touches routes or module graph.  |
| **gate**   | `pnpm build && pnpm validate:repo` — the full deterministic bundle, build first because circular reads `.next/types`. |
| **merge**  | None. No override; `/merge` applies the default lifecycle and transport.                                              |

## Commit and review conventions

- **Never commit as Claude.** Every commit uses `Simon Heimlicher <simon.github@heimlicher.com>`. Never pass an `--author` override.
- On each pull request, assign `simonheimlicher` as reviewer and comment `/gemini review`. The `@claude` review triggers automatically.

## Layout

| Path                | Contents                                                       |
| ------------------- | -------------------------------------------------------------- |
| `src/app/`          | Next.js routes, including `blog/[slug]`                        |
| `src/components/`   | Landing page sections, MDX components, spec-tree visualization |
| `content/blog/`     | MDX blog posts                                                 |
| `mintlify/`         | Mintlify docs source — Guide and Reference tabs                |
| `assets/`           | Logo generation scripts, run through `uv`                      |
| `tests/`            | pytest tests for the asset scripts                             |
| `docs/publication/` | Site architecture and release plan                             |
| `public/`           | Static assets                                                  |

`docs/publication/site-architecture.md` and `docs/publication/release-plan.md` govern route architecture, the content pipeline, versioning, and release stages. Read both before changing routes or content handling.

## Tech stack

Next.js 15, React 19, TypeScript, TailwindCSS 4, MDX through `next-mdx-remote`, Framer Motion, pnpm. Mintlify for the docs subdomain. Python through `uv` for asset generation. MIT licensed.

## Design system

- Dark theme: background `#0c0a09`, foreground `#fafafa`
- Amber accent `#f59e0b`, cream `#fef3c7`
- Typography: Fira Code (monospace)
- Glassmorphism cards with backdrop blur
- Node state colors: green (valid), amber (stale), muted gray (needs work)

Tokens live in `src/app/globals.css`.

## CI/CD

GitHub Actions in `.github/workflows/`:

- `deterministic-verification.yml` — `pnpm validate` in one job; `pnpm build` followed by `pnpm circular` in another
- `dependency-review.yml` — fails a pull request that introduces a high or critical advisory
- `claude.yml` — interactive Claude assistant on `@claude` mentions
- `claude-code-review.yml` — automatic review on pull request open and synchronize

The two verification workflows are repository-owned and pin every action by commit SHA. The two Claude workflows call reusable workflows from [gh-actions](https://github.com/outcomeeng/gh-actions) and require the `CLAUDE_CODE_OAUTH_TOKEN` repository secret, which is configured.

## Playwright MCP screenshots

Always set the `filename` parameter to a task-specific subdirectory under `.playwright-mcp/`, which is gitignored. Never save screenshots to the repository root.

Format: `.playwright-mcp/<task-slug>/<descriptive-name>.png`

## Related repositories

- [spx](https://github.com/outcomeeng/spx) — CLI for the Spec Tree
- [plugins](https://github.com/outcomeeng/plugins) — Claude Code plugin marketplace declared in `.claude/settings.json`
- [gh-actions](https://github.com/outcomeeng/gh-actions) — shared GitHub Actions

<!-- /SPEC-TREE:shared root -->
