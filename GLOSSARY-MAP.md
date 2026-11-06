# Glossary Map

ReMatch is a multi-sport app (racket sports first) for booking courts, finding Games, running Clubs and Competitions, coaching, and tracking Levels. Each context below has its own language; the same word can mean different things in different contexts, so read the glossary for the context you are working in.

## Contexts

- [Booking](./src/booking/GLOSSARY.md): Venues, Courts, Reservations, Passes, Price and Tax rules
- [Play](./src/play/GLOSSARY.md): casual Games players join, with Spots, Waitlists and Hosts
- [Clubs](./src/clubs/GLOSSARY.md): community groups, their Members, Roles, Dues and Kas
- [Competitions](./src/competitions/GLOSSARY.md): Events with a Format (Americano, groups, knockout, Seasons) that produce Fixtures and Standings
- [Rating](./src/rating/GLOSSARY.md): a Player's Level per rated Sport, from confirmed Results and Level Assessments
- [Social](./src/social/GLOSSARY.md): Posts, Follows and Feeds
- [Coaching](./src/coaching/GLOSSARY.md): Coaches, Lessons and Packs
- [Payments](./src/payments/GLOSSARY.md): ReMatch's own ledger of Charges, Shares, Debts, Cash marks, Platform fees, Refunds and Payouts
- [Shared kernel](./src/shared/GLOSSARY.md): Player, Guest and the Sport catalog, used by every context

## Relationships

- **Play → Booking**: every Game holds one Reservation; the Game's Host is the Reservation's Customer.
- **Competitions → Booking**: an Event (or each Matchday of a Season) holds a Reservation block.
- **Coaching → Booking**: Lessons hold Reservations, singly or as a Reservation block.
- **Clubs → Play / Competitions**: a Club can be the Host of a Game or the Organizer of an Event; Club-only visibility is decided by Club membership.
- **Play, Competitions → Rating**: confirmed Results go to Rating; Play and Competitions read Levels for Level ranges and Categories.
- **Coaching → Rating**: Coaches submit Level Assessments.
- **Booking, Play, Clubs, Competitions, Coaching → Payments**: each raises Charges or Debts for what is owed; Payments collects, records Cash marks, takes the Platform fee and pays out to Payees.
- **Social → Clubs, Play, Competitions**: Posts can be attached to a Club, Game or Event; Clubs decides who may post in a Club.
- **All contexts ↔ Shared kernel**: people are referenced by `PlayerId` (or a Guest), and sport rules come from the Sport catalog.
