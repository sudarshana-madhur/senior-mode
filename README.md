# senior-mode

**Agent skills distilled from real senior-software-engineer workflows, not
prompt experiments.**

Every skill in this repo came out of actual shipping work: sitting with an AI
agent, planning and building real software under real deadlines, and noticing
where the collaboration kept breaking down, then encoding the fix as a
repeatable skill.

## Why this exists

Working with AI agents as a senior engineer has a specific failure mode:
the agent is eager, fast, and *decides things you never agreed to*. Decisions
get made silently, plans drift out of sync with reality, and by the time you
notice, you're reviewing work built on assumptions you never made.

These skills make the agent behave like a senior peer instead: explore before
asking, surface every decision, never decide on your behalf, and never let
documentation drift from what was actually built.

## Skills

| Skill | What it does |
|---|---|
| **[plan-tickets](plan-tickets/SKILL.md)** | Turns a greenfield idea, a single feature, or a half-built project into fully-decided GitHub issues through deep, decision-by-decision discussion |
| **[ship-ticket](ship-ticket/SKILL.md)** | Picks the best next ticket, implements it honoring its recorded decisions, syncs every affected ticket when decisions change, and closes it only when acceptance criteria are verified |

### plan-tickets

A planning session that adapts to where you are:

- **Greenfield**, starts at the stack level: architecture, backend split,
  database, auth, before touching feature details.
- **Feature on an existing codebase**, explores the codebase first, then
  plans only what that feature needs.
- **Mid-way / rescue**, audits what's built vs. mocked vs. missing, and maps
  the completion path.

Then it grills you. Every decision that isn't determined by the codebase is
surfaced as an explicit question with a recommendation. Blocked decisions get
parked visibly and revisited when unblocked. GitHub issues are created *along
the way*, never in one lossy batch at the end, and updated as understanding
evolves. The output: a board of moderate, end-to-end-verifiable tickets with
zero unresolved decisions.

### ship-ticket

One ticket per session, by design. It re-reads the whole board fresh (that's
the drift protection), picks the ticket whose dependencies are green and
unblocks the most work, and confirms the pick with you before touching code.

Recorded decisions are binding. Missing or ambiguous decisions stop the work
for a question, with a recommendation. If a decision changes mid-build, it
updates the current ticket **and every other ticket affected** before
continuing. An issue closes only when its acceptance criteria are actually
verified, and if the agent can't verify something itself, it hands you exact
steps, assesses your report, and closes on that evidence. Never closes on
unverified claims, never leaves a red ticket green.

## Design principles

1. **Senior-to-senior.** The agent treats you as a peer: every trade-off on
   the table, recommendations given but never forced.
2. **Explore before asking.** Questions the codebase can answer never reach
   you.
3. **No silent decisions.** Anything not 100% determined gets asked. No
   exceptions for "obvious" calls.
4. **Durable context.** Decisions live in GitHub issues, written while fresh,
   edited as they evolve, not trapped in a chat scrollback.
5. **AC-gated shipping.** "Done" is defined by acceptance criteria verified
   with evidence, not by enthusiasm.
6. **One ticket, clean session.** Fresh context per ticket is the drift
   protection, not a limitation.

## Usage

#### Plan a whole project from scratch
/plan-tickets  "I want to build X. Here's the high-level idea..."

#### Plan one feature on an existing repo
/plan-tickets  "Add SSO login to this app"

#### Plan the completion of a half-built project
/plan-tickets  "Half of this is built, figure out what's missing and plan it"

#### Then ship, one ticket at a time
/ship-ticket

## More coming
This collection grows as real work surfaces new failure modes worth encoding.
