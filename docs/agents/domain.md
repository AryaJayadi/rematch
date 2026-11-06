# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

This repo is **multi-context**. `GLOSSARY-MAP.md` at the root lists the contexts and how they relate.

## Before exploring, read these

- **`GLOSSARY-MAP.md`** at the repo root. Then read the `GLOSSARY.md` of each context relevant to the topic:
  - `src/booking/`: Venues, Courts, Reservations, Passes, Price and Tax rules
  - `src/play/`: Games, Spots, Hosts, Waitlists
  - `src/clubs/`: Clubs, Members, Roles, Dues, Kas
  - `src/competitions/`: Events, Formats, Entrants, Fixtures, Standings, Seasons
  - `src/rating/`: Levels, Results, Level Assessments
  - `src/social/`: Posts, Follows, Feeds
  - `src/coaching/`: Coaches, Lessons, Packs
  - `src/payments/`: Charges, Shares, Debts, Cash marks, Platform fees, Payouts
  - `src/shared/`: Player, Guest, Sport catalog (used by every context)
- **`docs/adr/`**: system-wide decisions. Also check `src/<context>/docs/adr/` for decisions scoped to one context.
- **`docs/research/`**: background research (monetization, payments provider, stakeholder goals and pains). Read it when a decision touches revenue, payments or user needs.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill creates them lazily when terms or decisions actually get resolved.

## File structure

```
/
├── GLOSSARY-MAP.md
├── docs/adr/                          ← system-wide decisions
├── docs/research/                     ← background research
└── src/
    ├── shared/GLOSSARY.md             ← shared kernel
    ├── booking/
    │   ├── GLOSSARY.md
    │   └── docs/adr/                  ← context-specific decisions
    ├── play/ …
    └── payments/ …
```

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in the relevant context's `GLOSSARY.md`. Don't drift to synonyms the glossary explicitly avoids.

The same word can mean different things in different contexts. Check which context you are in before using it. For example, a Club is never a Venue; a Game (Play) is not a Fixture (Competitions); a Participant (Play) is not an Entrant (Competitions) or a Student (Coaching).

If the concept you need isn't in the glossary yet, that's a signal: either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0002 (every Game sits on a Reservation), but worth reopening because…_
