---
name: Plan-Tickets
description: Deep-dive planning session that turns a greenfield idea, a single feature, or a half-built project into fully-decided GitHub issues ready for rapid implementation. Grills the user (a senior software engineer) on every decision topic by topic, parks blocked decisions, and creates/updates GitHub issues along the way — never all at the end.
---

# Plan-Tickets

Turn a plan into a board of fully-decided, implementable GitHub issues through a
relentless, deep discussion with the user. The user is a **senior software
engineer** — never dumb things down, never hide trade-offs, never take a
decision on their behalf. Every decision that is not 100% determined by the
codebase or explicit user instruction MUST be surfaced as an explicit question
with your recommendation.

This skill adapts to three entry modes — detect which one you're in during the
audit, and say so out loud:

1. **Greenfield** — brand-new project from scratch. Planning starts at the
   stack level (frontend/backend split vs full-stack framework, database,
   hosting, auth approach...) before any feature detail.
2. **Feature on an existing codebase** — the user describes a feature; you dig
   into the codebase first, then plan only what's needed to ship that feature.
3. **Mid-way / rescue** — a project or feature is partially built (some
   integrated, some static, some missing). Audit what exists, map the gaps,
   plan the completion.

## Phase 0 — Tooling check (never skip)

Verify, before any discussion:

- The working directory is a git repo with a GitHub remote (`git remote -v`).
- `gh auth status` works and resolves the repo (`gh repo view`).

If either fails, **stop and ask the user** how to proceed (add a remote?
create a repo? authenticate gh? work without GitHub this time?). Never silently
fall back to local files.

## Phase 1 — Deep audit (explore before asking)

Understand the actual state of the project before asking anything:

- For large codebases, launch parallel exploration subagents (frontend, backend,
  CMS, infra, docs...) and demand exhaustive, factual inventories: routes,
  content types, what is integrated vs static/mock vs missing, TODOs, dead
  links, env vars, scripts, dependencies.
- Read any existing ticket/plan docs and unfinished specs — they are prior
  decisions to honor or consciously revise.
- If a question can be answered by exploring, explore instead of asking.
- End the audit with an honest state-of-the-world summary: what exists, what's
  mock, what's missing — presented to the user for confirmation.

## Phase 2 — Topic map & priorities

- Distill the audit into a numbered list of discussion topics (auth, payments,
  SEO, deployment... — whatever the project actually needs).
- Ask the priority-setting questions FIRST (e.g. "what single flow must work
  flawlessly?", "real or simulated for X?"), because they set the P0/P1
  gradient every later decision leans on. Walk topics in dependency order —
  root decisions (auth, data model) before their dependents.

## Phase 3 — The grilling loop (core of the skill)

For each topic, in a continuous back-and-forth:

1. **Batch related decisions** into one round of questions. Every question
   carries: the audit context that motivates it, the realistic options, and
   **your recommended answer** (labeled as such, with reasoning when the
   trade-off isn't obvious).
2. **Every decision gets asked.** No silent defaults. If the user's answer
   contains an assumption or a question back ("isn't X handled by Y?"),
   answer it precisely and get the decision re-confirmed before moving on.
3. **Blocked decisions get parked, visibly.** If decision A depends on a
   decision in topic B, mark A as 🔒 BLOCKED, state which topic unblocks it,
   and move on. When B is decided, return and finalize A immediately —
   never leave a blocked decision forgotten.
4. **If a decision needs deviation-trip research** (the user must confirm
   something elsewhere first), park the reached understanding in that topic
   and switch; return to finalize after the deviation resolves.
5. **Create issues ALONG THE WAY.** The moment a topic's decisions are
   finalized, create its GitHub issue(s) immediately — never defer writing to
   the end (context drift will leak nuance). If new information revises an
   earlier topic, **edit or comment on the existing issue** rather than
   starting from memory.
6. If the user interrupts to correct or extend an earlier answer, amend the
   decision, update the affected issue(s), and re-state the amended decision
   table for confirmation.

When a decision gap appears mid-discussion (e.g. "email confirmation requires
SMTP" → email provider is now on the critical path), surface it immediately
and pull that topic forward — dependency beats planned order.

## Issue conventions

- **Body template (suggested skeleton, adapt to the project):**
  `## Context` (why this ticket, what exists today) → `## Decisions
  (finalized)` (the confirmed choices, as a table or bullets, worded exactly
  as agreed) → `## Changes` / `## Implementation` → `## Acceptance criteria`
  (checkable, end-to-end verifiable) → `## Dependencies` (blocks / depends on).
- **Granularity:** each ticket is moderate — completable end-to-end and
  verifiable end-to-end in one focused session. Never split tiny chunks into
  their own tickets; never create a ticket so big it can't be verified.
- **Labels:** create core labels `p0` (must-ship), `p1` (post-goal /
  nice-to-have), `blocked` on first use. Derive additional **area labels from
  what the audit actually found** (e.g. `frontend`, `cms`, `infra`, `seo`,
  `content`) — don't force a fixed taxonomy onto every project.
- **Titles** are prefixed with an area tag: `[CMS]`, `[Frontend]`, `[Runbook]`...
- **Cross-reference** tickets by issue number in Dependencies.
- Record the *reasoning* behind contentious decisions (one line of "why") —
  future implementers should not have to re-derive it.

## Master plan issue

When the plan produces more than a handful of tickets, create ONE master
"Ship Plan" issue containing: the golden path / definition of done, a
dependency-aware implementation order in waves (checkbox list linking every
ticket), a demo/acceptance runbook where relevant, and an **explicit
out-of-scope list** (what we are consciously NOT doing). Keep it updated as
tickets close. For a single-feature plan this master issue is optional — skip
it if it adds no value.

## Session close

End with: a summary table of every issue created (number, title, labels), the
board link, the list of decisions still open or parked (should be none unless
genuinely deferred), and the user-side actions they own (DNS, accounts, keys,
credentials — anything only the user can do). Ask whether to adjust anything
on the board or start implementation.

## Anti-patterns (never do these)

- Never take a decision silently, however obvious it seems.
- Never create all issues in one batch at the end of the session.
- Never create a ticket that still contains an unresolved decision — finalize
  it, or mark it blocked in the ticket body and label it `blocked`.
- Never plan implementation work in this skill — planning ends at the board.
- Never ask the user something the codebase could have answered.
