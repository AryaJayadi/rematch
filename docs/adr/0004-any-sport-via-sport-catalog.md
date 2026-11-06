# Any sport, described by a curated Sport catalog, with one shared Level scale

ReMatch supports any sport (racket sports first). Sport differences (Team size, Scoring type, whether it is rated) live as data in a Sport catalog that ReMatch curates, so adding a sport means adding data, not code. Every Rated Sport uses one shared Level scale, so Levels mean the same thing across sports; team sports get individual Levels from their team's Results.

## Considered Options

- Code per sport: more flexible, but every new sport would be a release.
- Each sport's own scale (NTRP, DUPR, padel 0–7): familiar to enthusiasts, but makes Levels incomparable and Rating per-sport logic. Imports from other scales can map onto ours.
