---
name: Ship-Ticket
description: Pick the single best next GitHub issue from the project board, implement it end-to-end honoring its recorded decisions, ask the user when decisions are missing or changing (and sync every affected ticket), verify acceptance criteria, and close the issue with a summary and a little cheer.
---

# Ship-Ticket

Implement exactly **one** GitHub issue per invocation, from decision record to
verified, closed ticket. The user is a **senior software engineer** —
collaborate at that level: surface every ambiguity, never self-decide, never
let a decision change silently desynchronize the board.

One ticket keeps the session clean. Because this skill re-reads the whole
board fresh each session, any decision changes from earlier sessions are
naturally absorbed — that is the drift protection. Do not "helpfully" pick up
extra tickets.

## Phase 0 — Locate the board

- Confirm the git remote + `gh` auth + repo resolution. If missing, stop and ask.
- List open issues. If the board has **no issues**, tell the user to run the
  planning skill first (e.g. `/plan-tickets`) and stop.
- Read the master ship-plan issue if one exists — its wave order is the
  preferred picking order.

## Phase 1 — Propose the ticket (confirm before touching code)

Analyze the open issues and propose exactly ONE to implement now:

- **Selection logic:** prefer the master plan's wave order; within that,
  pick the ticket whose dependencies are all closed, with the highest
  priority label, and that best unblocks other tickets. A ticket blocked on
  missing user-side actions (credentials, DNS, dashboards) is only pickable
  if those inputs exist or arrive in time — flag it otherwise.
- Present to the user: the ticket, **why this one now** (dependencies green,
  what it unblocks), the acceptance criteria you'll be held to, and any
  decisions inside it that look incomplete — then get explicit confirmation
  before starting. If the user wants a different ticket, pick theirs.

## Phase 2 — Implement

1. Read the ticket fully — body AND comments. Its **Decisions (finalized)**
   are binding; implement them verbatim. Explore the relevant codebase first
   (patterns, structure, existing conventions) and follow them.
2. **Missing or ambiguous decision mid-implementation → stop and ask** with
   your recommendation. Self-deciding is the cardinal sin of this skill.
3. **If a decision changes or a discovery invalidates part of the plan:**
   - Ask the user first, with the change and its blast radius.
   - After confirmation, **update the current ticket AND every other ticket
     affected** (edit bodies / add comments) *before or while* implementing —
     so no future session picks up a stale decision.
   - Record the change reason in the ticket comment.
4. Keep the implementation as small as the ticket allows; don't refactor the
   world. If you find unrelated breakage, report it — don't silently fix it
   into this ticket.

## Phase 3 — Verification gate (AC-gated close)

The ticket's acceptance criteria are the contract. Before closing:

- [ ] Every AC checkbox verified: build/typecheck/tests where they exist,
      plus **browser/manual verification of user-facing changes**
      (screenshots where possible).
- [ ] If an AC **cannot be machine-verified** (environment limitation,
      browser/tooling failure, external service): hand the user **exact,
      numbered verification steps**, wait for their report, assess it
      honestly, and close on that evidence. A ticket never stays open merely
      because the agent couldn't click the button — but it never closes on
      unverified claims either.
- [ ] If ACs genuinely fail: fix or ask; **do not close a red ticket.** If
      the session must end first, leave the issue open with an honest
      progress comment (what's done, what remains, exact next steps).

## Phase 4 — Close and cheer

- Tick the AC boxes in the issue body that are done.
- Close with a summary comment: what shipped (files/changes), how each AC was
  verified (evidence), decision changes made and which other tickets were
  synced because of them.
- Report to the user — then a **short, fun celebratory closer** (the ticket's
  victory lap; keep it brief and human).
- Offer: pick the next ticket in a fresh session (recommended — clean
  context), or continue here if the user insists.

## Anti-patterns (never do these)

- Never pick multiple tickets in one invocation.
- Never implement against a stale understanding when the ticket says otherwise
  — the issue body is the source of truth.
- Never silently deviate from a recorded decision; ask, sync, then build.
- Never close an issue with unverified or failing acceptance criteria.
- Never leave other open tickets stale after a decision change.
- Never start work without the user's confirmation of the picked ticket.
