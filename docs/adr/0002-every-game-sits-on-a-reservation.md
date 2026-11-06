# Every Game and Event sits on a Reservation

Every Game holds a Reservation, and every Event (or Matchday) holds a Reservation block, so Booking is the single source of truth for who is using which Court and when. Games at public or non-partner courts still work: anyone can add an Unmanaged Venue, where a Reservation is a Self-declared hold with no availability check and no payment to the Venue.

## Considered Options

- A Game's place is either a Reservation or a plain address. Rejected because "where and when" would then live in two models.
