# Booking

Venues, their Courts, and the Reservations that hold Court time. Booking owns availability, prices, venue memberships and venue-level tax.

## Venues and courts

**Venue**:
A physical place with one or more Courts. Never a community group.
_Avoid_: Club, facility, location, GOR

**Managed Venue**:
A Venue whose operator runs its Courts, opening hours and prices in ReMatch.
_Avoid_: Partner venue, verified venue

**Unmanaged Venue**:
A public or non-partner Venue that anyone can add, with no availability or prices in ReMatch.
_Avoid_: Public court, free venue

**Court**:
A single bookable playing area at a Venue, for one or more Sports. A football pitch is a Court too.
_Avoid_: Pitch, field, space, lapangan

**Slot**:
A span of one Court's time that can be reserved.
_Avoid_: Session, time block

## Reservations

**Reservation**:
A hold on one Court for one Slot. At a Managed Venue it is checked against availability; at an Unmanaged Venue it is a Self-declared hold.
_Avoid_: Booking (as a noun), session

**Self-declared hold**:
A Reservation at an Unmanaged Venue, with no availability check and no payment to the Venue.
_Avoid_: Placeholder, fake booking

**Reservation block**:
Several Reservations held together, for an Event or a run of Lessons.
_Avoid_: Bulk booking, block booking

**Customer**:
The Player who holds a Reservation and answers to the Venue for it.
_Avoid_: Member, booker, user

**Cancellation policy**:
A Venue's rules for cancelling a Reservation and what is refunded.
_Avoid_: Refund policy

## Pricing

**Price rule**:
A rule that sets the price of a Court for a Slot, such as peak and off-peak rates or a Pass discount.
_Avoid_: Tariff, rate card

**Pass**:
A Venue's membership that a Customer holds, unlocking Price rules such as discounts.
_Avoid_: Membership, member, subscription

**Tax rule**:
The regional tax a Venue applies to its prices, such as Jakarta's 10% PBJT on paid sports.
_Avoid_: Service charge
