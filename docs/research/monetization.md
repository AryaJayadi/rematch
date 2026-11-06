# Monetization research: sports booking, community and tournament platforms

**Purpose:** decide ReMatch's revenue model.
**Accessed:** all pricing below was checked on **2026-11-05** unless noted. Prices change often and many vary by country.
**Source labels:** an unlabeled URL is a primary source (the vendor's own pricing page, help center, terms or press release). **[secondary]** means a news, review, aggregator or competitor site. **not public** means I could not find the figure, so none is given.
**Market focus:** ReMatch is probably **Indonesia/SEA-first**, with payments on **Midtrans or Xendit** rather than Stripe Connect. Processor fees are covered separately in `docs/research/payments-provider.md`. The Stripe figures below are only a reference point for the split-payment math, which needs redoing with the chosen provider's fixed and percentage fees for QRIS, virtual accounts and e-wallets.
**FX assumption:** **Rp18,000 = US$1**, rounded from Rp18,146 on 2026-10-30 **[secondary]** (https://listrikindonesia.com/detail/21613/rupiah-hari-ini-tertekan-dolar-as-menguat-ke-rp18146). The rupiah weakened through 2026, so re-check before using the conversions.

---

## TL;DR

- **Indonesia specifics (section 0):**
  - **Playtomic Indonesia** charges **no player fee on court bookings**. It charges 4% on open matches (min Rp5,500, no max) and 4% on classes, courses, leagues and tournaments (Rp5,500–37,900).
  - **AYO Indonesia**, the local leader (900k+ users, 2,000+ venues, 3,700+ padel courts), sells venue software (AVM) **by subscription with no per-booking commission**.
  - **Reclub** keeps money off-platform and sells Supporter at **Rp29k/mo or Rp349k/yr**.
  - **PDL Up**, Indonesian, sells Pro at **Rp79k/mo or Rp399k/yr**.
  - The Indonesian norm is free booking for players. Money comes from venue subscriptions, small fees on matches and events, and player or organizer subscriptions.

- Racket-booking marketplaces (Playtomic, MATCHi) make money mainly from **per-transaction fees on online payments**. Players pay a capped percentage service fee, and venues pay a B2B commission. A player subscription that removes those fees comes on top.
- Venue SaaS (CourtReserve, PlayByPoint, Skedda, ClubSpark) charges **flat monthly fees by court or space count**, roughly $99–$600/mo. Most add little or nothing to payments, and "0% commission" is how they sell against Playtomic.
- Community and team apps (Reclub, Spond, Heja, Meetup, TeamSnap) charge in one of three ways: an **organizer subscription** (Meetup, Heja, TeamSnap), a **cut of payments** with a free app (Spond), or **ads plus a small supporter subscription** with money handled off-platform (Reclub).
- Every fee applies only to money that flows **online**. Wallets, punch cards, memberships and cash are usually fee-free or outside the platform. That is the main leakage path, and it matters directly for ReMatch's "mark paid in cash" feature.
- **Split payments multiply the fixed per-charge cost.** One court split four ways costs four fixed fees. On Stripe that means about 3.5–5% instead of about 2–3.4%, and the same effect applies to any provider with a fixed fee per transaction. Playtomic and MATCHi both charge players a separate fee for split payments or open matches.

---

## 0. Indonesia and SEA comparables (priority market)

**Market context:**
- Padel court rental in Indonesia is about **Rp300,000–600,000 per hour** (US$17–33) **[secondary]**: https://semenmerahputih.com/en/news/construction/padel-court-business
- At those prices, four players sharing a 1.5-hour court each pay about Rp110k–225k (US$6–12).
- Campus studies from Sidoarjo, Jember and elsewhere describe badminton and futsal booking as still largely **manual, by walk-in or WhatsApp** **[secondary, academic]**: https://pels.umsida.ac.id/index.php/PELS/article/download/876/524 , https://sipora.polije.ac.id/14243/1/RINGKASAN.pdf
- That makes WhatsApp plus bank transfer the default competitor and the main leakage channel.

### Playtomic in Indonesia (and SG/TH)
- **Fee table:**

| Market | Player bookings | Open matches | Classes / courses / leagues / tournaments |
|---|---|---|---|
| Indonesia | **No fee** | 4%, min Rp5,500, no max | 4%, Rp5,500–37,900 |
| Singapore | **No fee** | 4%, min SGD 0.59, no max | 4%, SGD 0.49–4.49 |
| Thailand | **No fee** | 4%, min THB 11, no max | 4%, THB 11–75 |

- Source: country tables on https://playerhelp.playtomic.com/hc/en-gb/articles/19831779272337-Service-Fee
- Compare the UK (3% booking fee, 20% on open matches capped at £0.99). In SEA, Playtomic keeps booking free and charges a flat 4% on social and organized play.
- **Premium in Indonesia:** the App Store lists "Basic Monthly" at **Rp149k** and "Basic Annual" at **Rp849k** (about US$8.3 and US$47). That these are the Premium/Unlimited plan is my inference, because they are the only subscriptions listed. https://apps.apple.com/id/app/id1242321076
- Venue B2B commission and Manager prices: **not public** (see section 1).

### AYO Indonesia (local multi-sport leader)
- **Product:** a "super sport community app" covering sparring and drop-in sessions, coach booking, court booking at "2,300+ venues" for padel, tennis, badminton, football, futsal, basketball and pickleball, Americano/Mexicano with live scoring, levels, leaderboards and badges. It is **nearly the same feature set as ReMatch.**
  - "Activity fees are collected automatically from players," so hosts collect money in the app.
  - The App Store listing (developer PT Ayo Indonesia Maju) says venue partners use **AVM (Ayo Venue Management) on a subscription model with no per-booking commission**.
  - https://apps.apple.com/id/app/ayo/id1539581877
- **AVM:** the app is free to download, so the subscription is billed outside the app stores. One review says "Kami bayar utk berlangganan" ("we pay a subscription"). It includes finance tracking, payout (withdrawal) accounts, payment links and manual payments for internal reservations, and taxes and vouchers. https://apps.apple.com/id/app/id1607354989
  - **AVM price: not public.**
- **Player fees:** a convenience fee exists. A user review on the AYO listing complains about one, but the **amount is not public**.
- **Scale** (homepage): 900,000+ users, 100+ cities, 2,000+ facilities, 6,800+ courts (padel 3,700+, badminton 1,400+, tennis 400+, pickleball 100+), 42,000+ teams. https://ayo.co.id
- **Other revenue:**
  - Brand collaborations ("AYO Kolaborasi": reach "hundreds of thousands of sports fans"), same page.
  - A co-branded Teh Botol Sosro AYO Arena football pitch **[secondary]**: https://www.bola.com/indonesia/read/5077573/kolaborasi-ayo-indonesia-dan-tehbotol-sosro-menghasilkan-tehbotol-ayo-arena-lapangan-sepak-bola-berstandar-internasional
  - 9,600+ communities and an investment from badminton Olympian Greysia Polli, amount undisclosed (May 2023) **[secondary]**: https://momsmoney.kontan.co.id/news/usai-gantung-raket-greysia-polli-investasi-di-aplikasi-olahraga-ini
- **Lesson:** the incumbent local model is **venue SaaS without commission, plus in-app activity fee collection, plus brand deals.** ReMatch would compete with AYO directly, not just with Playtomic.

### Other Indonesian apps
- **Courtside** (padel, Indonesia):
  - Player app plus "Club Manager" with dashboard, scheduling, events and coaching, and "all local payment options." https://courtside.id/club-manager , https://apps.apple.com/id/app/id6737027746
  - Covers 23 cities **[secondary]**: https://erafone.com/artikel/post/5-pilihan-aplikasi-untuk-mabar-olahraga-pesan-lapangan-wajib-tahu
  - Pricing and commission: **not public**.
- **KUYY!** (PT. Kreasi Untuk Indonesia):
  - Classes, coaching, venues and community sessions, with a "for Business" side for organizers. Refunds go to an internal "Kuyy Balance." https://kuyy.id/activities/7eae80bb-b37a-4561-bf1c-60203b7a410a
  - 2,000+ communities in 30+ cities **[secondary, erafone]**.
  - Monetization: **not public**.
- **White-label club apps:** several Jakarta padel clubs (Tamora, Clubhouse Indonesia, Courtyard) run their own branded apps from Book & Go, which sells "0% commission." Book & Go also says custom-built apps cost "hundreds of millions of rupiah" **[secondary, vendor]**: https://bookandgo.app/es/booking/padel-booking-system-jakarta
- **Lesson:** larger venues will pay for **their own brand and zero commission.**

### Reclub in SEA
- **Supporter on the Indonesian App Store:** **Rp29k/month, Rp349k/year** (about US$1.6 and US$19). Developer Reclub PBC, 14k ratings. https://apps.apple.com/id/app/id1323924315
- **Strong in SEA pickleball:** for example, a Hanoi club has 1,199 members and 257 activities (Reclub's own page). https://reclub.co/id/clubs/@glean-hanoi
- **Revenue split:** a third-party tracker estimates about 395k Android installs and revenue of about 48% in-app purchases and 52% ads **[secondary, estimate]**: https://appgoblin.info/apps/co.reclub
- **Payments are off-platform** through bank transfer and a receipt screenshot (section 1). This matches how Indonesian groups already pay hosts, which explains why it works without a processor.

### PDL Up (Indonesian)
- Developer PT. Starva Digital Teknologi. The Indonesian App Store lists **Pro Weekly Rp39k, Monthly Rp79k, Yearly Rp399k** (about US$2.2, $4.4 and $22), with 348 ratings averaging 4.9. https://apps.apple.com/id/app/id6752893039
- The free tier is capped at 3 courts per tournament (section 1).

### Indonesian price anchors for subscriptions (App Store ID / official page)

| Product | Monthly | Annual |
|---|---|---|
| Reclub Supporter | Rp29k | Rp349k |
| PDL Up Pro | Rp79k | Rp399k |
| Strava (Indonesia, official pricing page) | Rp49k | Rp349k |
| Playtomic "Basic" (probably Premium) | Rp149k | Rp849k |

- Strava source: https://www.strava.com/pricing
- **Takeaway:** Indonesian player subscriptions cluster at **Rp29k–79k/month and Rp349k–399k/year (about US$2–4 and US$19–22)**. Playtomic is the premium outlier.

---

## 1. Platforms

### Playtomic (player app, Playtomic Manager, Premium)

**How it makes money:** a player service fee on online transactions, a B2B commission taken from the venue's online revenue, a Manager SaaS subscription for venues, and the Premium player subscription.

- **Player service fee:** "applicable to every transaction processed online, with the exception of wallet transactions." It covers reservations, open matches, tournaments, classes and courses, and the amount varies by country. It is not refunded when a reservation or tournament is cancelled, but it is refunded when a player leaves an open match or the match is cancelled. https://playerhelp.playtomic.com/hc/en-gb/articles/19831779272337-Service-Fee
  - **UK:** open matches 20% (min £0.24, max £0.99), player bookings 3% (£0.35–£0.99), classes 4% (£0.24–£1.99), courses and leagues 2% (£0.24–£2.99), tournaments 4% (£0.24–£1.99). Source: the UK fee table image on the same page.
  - **US:** open matches 20% ($0.29–$0.99). There is **no fee** on player bookings, classes, courses, leagues or tournaments. Same page.
  - **France:** open matches 20% (€0.29–€0.99), courses 4% (max €1.99), and no fee on the other categories. Same page.
  - The pattern: the fee on **open matches** (filling a game with strangers) is the one charged everywhere. Booking fees are switched off in the US and France, presumably to compete.
- **B2B commission (venue pays):** "Every time a player pays for a booking online through Playtomic, our service fee is automatically deducted from that payment before the money reaches you." It is invoiced monthly. https://helpmanager.playtomic.com/hc/en-gb/articles/50654517291537
  - **Rate: not public.** It is set "in your club's contract with Playtomic." https://helpmanager.playtomic.com/hc/en-gb/articles/44898846238609
  - A competitor claims it is 4% per booking **[secondary, competitor, sources conflict on whether club or player pays]**: https://bookandgo.app/en/compare/playtomic
- **Payments:** these run on Stripe Connect. Venue funds go to the venue's Stripe connected account net of the B2B fee, with weekly payouts. Split bookings are guaranteed through a **pre-authorization hold** on the booking owner, or by **charging the owner's card on file** for unpaid shares about 2 hours after the match. https://helpmanager.playtomic.com/hc/en-gb/articles/26436916409361
- **Manager SaaS:** there are four tiers (Standard, Professional, Champion, Master), billed monthly or annually through Stripe Checkout with a 2-week free trial. If payment fails for 5 days, the club is made inactive and hidden from the app. https://helpmanager.playtomic.com/hc/en-gb/articles/20535458071313-Pricing-and-subscription-plans
  - **Prices: not public.** One directory lists €59 and €119/mo under tier names that do not match **[secondary, unreliable]**: https://appvizer.com/recreational-activities/gym-mgt/syltek
- **Premium ("Unlimited") player subscription:** sold monthly or annually through the Apple and Google app stores, with a free trial on the annual plan. https://playerhelp.playtomic.com/hc/en-gb/articles/19831696399633
  - **What it unlocks:** zero booking, match and cancellation fees (the club's cancellation policy still applies), advanced stats, full level-progression graphs, unlimited priority match alerts, and "Golden Matches" (matches the player creates or joins get promoted so they fill faster).
  - Venues are not affected. The venue help center says "you don't need to do anything." https://helpmanager.playtomic.com/hc/en-gb/articles/20534670300561
  - **Price: not public** on the help pages. The Saudi App Store lists "Basic Monthly" at SAR 42.99 and "Basic Annual" at SAR 174.99, but I could not confirm these are Premium **[secondary]**: https://apps.apple.com/sa/app/playtomic/id1242321076
- **Club Wallet and promotions:** these are prepaid club credit sold in the app (for example, "pay €50 and receive €60"). The club manages them and they never expire. **Wallet payments are exempt from the player service fee.** https://playerhelp.playtomic.com/hc/en-gb/articles/19831817632273 , https://playerhelp.playtomic.com/hc/en-gb/articles/19831826591633
- **Scale:** "gross revenues exceeding $260 million (€240 million) in 2024" with 40% growth, 6,000 clubs, 1.5M monthly active players, clubs in 63 countries, and a €65M ($70M) round (March 2025). "Gross revenues" most likely means gross booking value, not net revenue. https://stories.prowly.com/388092-playtomic-raises-70-million-in-funding-accelerating-its-expansion-in-the-us-market
  - Aggregators estimate net revenue far lower, at $15M (2024) and $34M (2025) **[secondary, conflicting]**: https://getlatka.com/companies/playtomic , https://www.cbinsights.com/company/playtomic/financials

| Party | Pays | Gains |
|---|---|---|
| Venue | SaaS subscription + B2B commission on online payments | Demand from the marketplace, payments, guaranteed split payments |
| Player | Service fee (mostly on open matches), or Premium | Discovery, matchmaking, level |
| Host | Same as player. Owns the booking and its debt risk | Fills the court |

### Reclub (community clubs, meets, RSVP)

**How it makes money:** ads plus an optional "Supporter" subscription. Payments are **entirely off-platform**.

- The terms say: "The App is supported by advertising and premium 'Supporter' subscriptions." Billing goes through the app stores. "All financial transactions occur strictly off-platform (e.g., cash, Venmo)." Also: "Reclub is not a payment processor." Effective 2025-12-21, materially updated 2026-10-19. https://reclub.co/terms-of-service
- **Supporter:** about $2–3/mo or $20–30/yr, varying by region. It removes the top banner ad and adds stats, full "Street Cred," match history, **payment receipts**, and a badge. https://pickleball.reclub.co/faq
- **Fee collection:** hosts list bank or wallet details, players pay outside the app and upload a receipt screenshot, and the host marks them paid. Both the host and the player need Supporter for receipts. https://help.reclub.co/hc/reclub-help/articles/1765845748-how-do-payments-work-in-recub
- Reclub is a Public Benefit Corporation. It is still unclear whether it will build in-app payments: the FAQ says they are "on the roadmap."

| Party | Pays | Gains |
|---|---|---|
| Club or host | Nothing, or about $20–30/yr Supporter for receipts | Free roster, RSVP and waitlist |
| Player | Nothing (sees ads), or Supporter | Discovery, stats |

### PDL Up (PDLUP: Americano, Mexicano, tournaments)

**How it makes money:** a freemium Pro subscription sold in the app.

- Pro is $2.99/week, $5.99/month or $29.99/year. It covers Americano and Mexicano variants and King of the Hill, Knockout and Group Stage tournaments, and recent versions added guest formats. The developer is PT. Starva Digital Teknologi (Indonesia). https://apps.apple.com/app/id6752893039
- The free tier is "100% free, no sign-up," capped at **3 courts per tournament**, with live leaderboards and share links. Pro removes the court limit and adds advanced scheduling, larger tournaments, club management and attendance, private leaderboards, official scoring and PDF export. https://pdlup.com
- No entry-fee handling and no venue plans: **not public / none found**.

The useful lesson: an organizer-pays paywall keyed on event size (number of courts) is a clean upgrade trigger.

### CourtReserve (venue SaaS, US-centric)

**How it makes money:** a flat SaaS fee by number of courts. Payments are passed through.

- Launch (8 courts) is $199/mo, or $159/mo billed annually. Advance (16 courts) is $369/mo ($329 annual). Momentum (32 courts) is $549/mo ($499 annual). Larger setups get custom pricing. Players and members are unlimited. https://courtreserve.com/pricing/
- **Payments:** the preferred processor is Stripe at 2.9% + $0.30 per card or 0.8% ACH. A former **0.2% CourtReserve usage fee** on top of Stripe was dropped for new Stripe integrations from 2025-02-18. https://intercom.help/courtreserve/en/articles/5772805-overview-of-integrated-payment-partners
- **Convenience fee:** clubs can choose to pass card costs to members, capped at 4% (lower in some jurisdictions). It cannot be applied in person. https://help.courtreserve.com/en/articles/7172928-convenience-fees
- In community feedback threads, clubs say most clubs absorb card costs rather than pass them on **[secondary, user forum]**: https://feedback.courtreserve.com/communities/1/topics/143-convenience-fee-when-using-a-credit-card

### Padel Mates (Sweden: venue booking and player app)

- The player app is free, with a **Pro** in-app subscription at $5.99/mo or $49.99/yr on the US App Store. https://apps.apple.com/us/app/padel-mates/id1531797995 (price seen via the search-index listing **[secondary]**)
- Venue pricing and commission: **not public**. padelmates.com currently redirects to a domain-sale page.
- A Danish operator, Racket Club, moved its bookings from MATCHi to Padel Mates, which shows venues do switch booking platforms **[secondary]**: https://padelalto.com/2024/11/21/danish-padel-operator-switches-from-matchi-to-swedish-platform

### MATCHi (Nordic and European venue booking)

**How it makes money:** a player service fee plus a venue subscription.

- **Player service fee:** 4% of the transaction, with a minimum and maximum per market, applied to court and activity bookings. It is charged in BE, NL, TH, NO, RO, SE, DK and FI. Caps: Sweden 3–15 SEK, Netherlands/Belgium €0.30–1.80, Finland €0.30–1.50, Norway 5–30 NOK. It is **not refundable** on cancellation.
  - It is calculated on each participant's share in a split payment.
  - **There is no fee on memberships, subscriptions, value or punch cards, or application forms.**
  - https://matchiplayers.zendesk.com/hc/en-gb/articles/21632070063517-Service-fee
- **Other markets:** players pay a split-payment service fee only when they choose to split. Paying the whole booking is free. https://matchiplayers.zendesk.com/hc/en-gb/articles/14685446085661-Split-Payment-Q-A
- MATCHi's Q&A says the unified fee replaced separate fees for split payments, cancellations and public matches with "one coherent fee." https://matchi.zendesk.com/hc/en-gb/articles/21757027952029-Service-fee-Q-A
- Venue SaaS pricing: **not public** (demo only). https://playmore.matchi.com

### PlayByPoint (US venue SaaS)

- The official pricing page (403 to my fetch; figures from a search-index copy) lists Starter $99.99, Starter+ $199.99 and Advanced $399.99 per month, plus Premium+. A www2 page lists Essential $199 and Premium $599. **The two official pages conflict.** https://playbypoint.com/pricing , https://www2.playbypoint.com/pricing/
- A competitor lists Essential $199 ($159 annual), Advanced $399 ($329), Premium $599 ($499) and "no per-booking fees" **[secondary]**: https://www.bookandgo.app/en/compare/play-by-point
- PlayByPoint says it "charges the same transaction fee regardless of where the booking originates." The rate is **not public**. Its pitch is aimed directly at Playtomic: "PLAYTOMIC TAKES A CUT OF YOUR BOOKINGS." https://www.playbypoint.com/tools/comparison/vs-playtomic
- Marketing Kit Pro is $149/mo (SMS and campaigns) **[via search snippet of help center]**.

### DUPR (pickleball rating)

**How it makes money:** a player subscription (DUPR+), sponsorship and retail partnerships.

- DUPR+ is **$19.99/yr**. The core rating, reliability score, W-L record and similar stay **free**.
- **Paid features:** AI-enhanced rating, AI match recaps and video analysis, scouting report, insights, metro rankings, a verified checkmark, and eligibility for DUPR Verified Events.
- **Partner perks:** a $30 JOOLA gift card and $20 "Club Cash" at participating clubs (USA/Canada), for "up to $130 in annual value."
- https://www.dupr.com/duprplus
- Sponsorship: "JOOLA named title sponsor of DUPR" (DUPR powered by JOOLA). https://www.dupr.com/post/joola-named-title-sponsor-of-dupr-introducing-dupr-powered-by-joola
- **Club bundling:** a club added $6/mo to its membership to cover DUPR+ for every member (from DUPR's blog). https://www.dupr.com/post/performance-pickleball-building-community-and-competitive-balance-with-dupr-plus
- The cost for a club to become "DUPR Verified" is **not public**. https://www.dupr.com/post/why-the-biggest-pickleball-clubs-are-becoming-dupr-verified

The useful lesson: the **rating stays free** and the **insight on top of the rating is paid**, sweetened by partner-funded perks that cost DUPR little.

### Meetup (community groups)

**How it makes money:** an organizer subscription, a member subscription (Meetup+), and a service fee on paid RSVPs.

- **Organizer plans:**
  - Standard from $29.99/mo, $99.99 per 6 months or $174.99/yr, with up to 3 groups.
  - Pro from $55/group/mo, or $47 on 6-month billing. Pro adds sponsors, paid promotions, Mailchimp and mailing-list export.
  - **Prices were raised in June 2024.**
  - https://help.meetup.com/hc/en-us/articles/28677808413197-Organizer-Subscription-prices-overview
- "Every Meetup group must be led by a local organizer with an active subscription." Organizers can recoup the cost through member dues and event fees. https://help.meetup.com/hc/en-us/articles/39428334646541-Who-pays-for-a-Meetup-group
- **Ticketed-event service fee (member pays):** for example, $1.62 on a $10 ticket. It is not paid to the organizer. Organizers with a recurring subscription and Meetup+ members are exempt. https://help.meetup.com/hc/en-us/articles/39489419634189
- **Meetup+ (member):** exempt from service fees, **priority position on waitlists**, full attendee lists, unrestricted DMs, ad-free. **Price not public** on the help pages. https://help.meetup.com/hc/en-us/articles/34744060726669

### Eventbrite (ticketing)

- Free events cost nothing.
- Paid tickets carry a **3.7% + $1.79 service fee per ticket** plus **2.9% payment processing per order** (US). Buyers pay by default, and organizers can choose to absorb the fees.
- https://www.eventbrite.com/organizer/pricing/
- Pro email marketing starts at $15/mo. Ads exist in select markets, with spend not included. Same page.

### Strava (social fitness)

- **US:** $11.99/mo or $79.99/yr, Family $139.99/yr, Strava + Runna $149.99/yr. The page was updated 2025-07-01, and **prices are set per country**. https://www.strava.com/pricing (my fetch returned the Indonesian variant; US figures come from the search-index copy of the same page)
- **What the subscription unlocks:** routes, segment leaderboards, advanced training analysis, challenges, and Fitness Score. https://www.strava.com/subscribe
- The free tier keeps the social feed and activity logging.
- The late-2024 price rise (US annual from $59.99 to $79.99) caused backlash and a public apology **[secondary]**: https://gearjunkie.com/news/strava-issues-apology , https://www.cyclingnews.com/news/the-strava-price-rise-controversy-what-happened-and-how-much-will-it-be-now

### Spond (team and club organizing)

**How it makes money:** a free app that earns only from payments.

- "Spond is a free platform and we only make money when our club and group users process their payments through Spond." Processing runs on Stripe.
- **Fees per product:** £0.20 + 2.5% (GBP), €0.20 + 2.5% (EUR), $1.00 + 3.29% (USD), 2 kr + 2.5% (NOK/SEK/DKK).
- **The group covers the fee by default.** A group can choose to add it on top so members pay it.
- https://help.spond.com/app/en/articles/118091-payments-costs-in-the-spond-app
- Spond Club (the admin product) in Norway charges 2.99% + NOK 2, plus NOK 27 per member per year for registered memberships **[via search snippet of help center]**: https://help.spond.com/club/en/articles/58192
- Spond reports 40% annual payment growth across 19 countries on Stripe (Stripe case study). https://stripe.com/en-br/customers/spond

The useful lesson: this is the closest analogue to ReMatch's Club fee collection. A free app plus a roughly 1% margin over Stripe's cost is enough to run a business at Spond's scale.

### TeamSnap (youth and team management)

- Single teams start at $0. Clubs and leagues get custom pricing. https://www.teamsnap.com/pricing
- **Ultra team plan:** about $12.50/team/mo billed annually, or $21.99 monthly **[secondary, sources conflict]**: https://us.fitgap.com/products/teamsnap
- **Payment processing:** reported as 3.25% + $1.50 or as 2.95% + $0.30 **[secondary, conflicting; not public on the official page]**.
- Also earns from youth-sports brand marketing (a "brands" section on the site; terms **not public**).

### Others

- **Heja (team app):**
  - Free is "Ad supported."
  - Team Pro is from $8.33/mo, about $99.99/yr per team. It adds payment tracking, attendance stats, unlimited admins, and no ads for admins.
  - Team Pro Max removes ads for the whole team.
  - Club pricing is by quote.
  - https://heja.io/pricing
  - The ads-on-free, per-team-admin-pays pattern fits ReMatch Clubs.
- **ClubSpark (UK LTA venues and clubs):**
  - **Free** for LTA-registered venues and coaches.
  - Online money carries **0.8% (uncapped) per transaction, deducted from the member's payment**, on top of the payment provider's fees (GoCardless 1.0% + 20p + VAT, capped at £4).
  - https://sportlabs.zendesk.com/hc/en-us/articles/203030535-What-are-the-Costs-of-Clubspark
  - This is a **governing-body-subsidized** model: the LTA pays, and ClubSpark adds a thin take on payments.
- **Skedda (generic space booking):**
  - US/Canada: Plus from $249/mo (35 spaces), Premier $349/mo (45 spaces).
  - EMEA/APAC: Starter $99/mo (15), Plus $149 (20), Premier $199 (25).
  - Extra spaces $4.99 each. Admin and membership add-ons $99/mo. Payments go through Stripe Connect.
  - https://www.skedda.com/pricing
- **"0% commission" challengers:**
  - Book & Go: setup fee plus flat monthly, no player fees, and payouts to the club's own gateway. https://www.bookandgo.app/en/compare/playtomic
  - Reservadeportes: from €700/yr, no commission **[vendor pages]**. https://www.reservadeportes.com/en/precios/
  - Together these show that a fee-free position is how rivals attack take-rate incumbents.

### Payments baseline: Stripe (reference only, since ReMatch will likely use Midtrans or Xendit)

The processor comparison lives in `docs/research/payments-provider.md`. Stripe is kept here only because Playtomic, Spond and Skedda run on it, so it explains their fee designs.

- **Card pricing:**
  - **US:** 2.9% + 30¢. https://stripe.com/pricing
  - **UK:** 1.5% + 20p for standard UK cards. https://stripe.com/gb/pricing
  - **EEA:** 1.5% + €0.25 for standard EEA cards. https://stripe.com/ie/pricing
  - Premium and international cards cost 1–2 points more.
- **Connect: "Stripe handles pricing."** Connected accounts pay processing, and the platform pays no account or payout fees. https://stripe.com/connect/pricing
- **Connect: "You handle pricing."** **$2 per monthly active account** plus **0.25% + 25¢ per payout** (same page).
  - **The general lesson for any provider:** per-account and per-payout fixed costs make low-volume hosts unprofitable unless payouts are batched or held as balance. Check whether Midtrans/Xendit sub-accounts or disbursements carry similar fixed fees.

---

## 2. Pricing cheat-sheet

IDR is shown first where an Indonesian price exists. USD conversions use Rp18,000 = US$1.

| Model | Indonesia / SEA | Global reference (2026-11-05) |
|---|---|---|
| Venue SaaS | AYO AVM: subscription, no per-booking commission, price **not public**. Courtside Club Manager: **not public**. Book & Go white-label: setup plus monthly, **not public**. | CourtReserve $159–549/mo (Rp2.9–9.9M). PlayByPoint about $99–599/mo. Skedda EMEA/APAC $99–199/mo (Rp1.8–3.6M). ClubSpark free (LTA-subsidized). |
| Player fee on court bookings | Playtomic ID/SG/TH: **none**. AYO: a convenience fee exists, amount **not public**. | Playtomic UK 3% (£0.35–0.99). MATCHi 4% with caps. |
| Fee on open matches or joins | Playtomic ID 4%, min Rp5,500 (US$0.31), no max. | Playtomic UK/US/FR 20% capped at 0.99 (GBP/USD/EUR). |
| Fee on classes, leagues and tournaments | Playtomic ID 4%, Rp5,500–37,900 (US$0.31–2.11). | Playtomic UK 2–4% (£0.24–2.99). Eventbrite 3.7% + $1.79 + 2.9%. Meetup about $1.62 on $10. |
| Venue commission | AYO: **none** (subscription). | Playtomic B2B fee, contract-set (**not public**). |
| Payment-only take | AYO collects activity fees in the app; its cut is **not public**. Reclub: none (off-platform). | Spond 2.5% + £/€0.20 (US 3.29% + $1). ClubSpark 0.8% on top. |
| Organizer subscription | PDL Up Pro Rp79k/mo, Rp399k/yr (US$4.4/22). | Meetup $29.99/mo. Heja about $99.99/yr per team. |
| Player premium | Reclub Rp29k/mo, Rp349k/yr (US$1.6/19). Strava ID Rp49k/mo, Rp349k/yr. Playtomic "Basic" Rp149k/mo, Rp849k/yr (US$8.3/47). | DUPR+ $19.99/yr. Padel Mates Pro $49.99/yr. Strava US $79.99/yr. |
| Ads and sponsorship | Reclub (ads plus Supporter). AYO brand collaborations and a co-branded arena. | DUPR title sponsor (JOOLA). Heja ads. Meetup Pro sponsors. |

**Split-cost mechanics (illustrative, Stripe UK rates):**
- One £40 charge costs £0.80 (2.0%). Four £10 shares cost £1.40 (3.5%).
- The same multiplication applies to any provider with a fixed fee per transaction. On an Indonesian share of about Rp100k–150k, a fixed fee of a few thousand rupiah per payment is a meaningful percentage. Redo this with the chosen provider's QRIS, virtual-account and e-wallet rates.
- This is why Playtomic charges a minimum fee (Rp5,500 in Indonesia) on open-match shares instead of on whole bookings.

---

## 3. Pitfalls

1. **Disintermediation and leakage, worst in Indonesia.**
   - The baseline competitor is **WhatsApp plus bank transfer**. Venues and hosts already take bookings and payments that way, and Reclub built its payment flow around transfer receipts for exactly that reason.
   - Every take-rate platform exempts some channel:
     - **Playtomic** exempts wallet payments, and its B2B fee applies only to online payments.
     - **MATCHi** exempts memberships and punch cards.
     - **CourtReserve's** convenience fee cannot apply in person.
   - For ReMatch, **"mark paid in cash" and venue memberships are built-in bypasses.** Model take-rate revenue on the online share only, and expect regulars to move off it.
2. **AYO already offers "no commission" to venues in Indonesia.** A ReMatch venue commission would be undercut on day one by the local incumbent's subscription model, as well as by Book & Go's "0% commission" white-label apps for big Jakarta padel clubs.
3. **Low willingness to pay for subscriptions.** Indonesian sports-app subscriptions sit around **Rp29k–79k/month (US$2–4)**, so premium revenue per user is roughly a third to a half of the US/EU equivalent. Playtomic's Rp149k/month is the outlier.
4. **Fee backlash.**
   - **Meetup 2019:** a test of a $2 RSVP fee on attendees, with the organizer fee cut to $2/mo, caused a revolt and was walked back **[secondary]**. https://www.theregister.com/2019/10/15/meetup_fee_increase_backlash
   - Meetup raised organizer prices again in June 2024.
   - Strava's late-2024 increase forced an apology **[secondary]**.
   - On AYO's own App Store page, a user complains about a newly added convenience fee.
   - **Lessons:** launch with the fee structure you intend to keep; announce changes well in advance; grandfather existing users.
5. **Non-refundable fees create friction.** Playtomic and MATCHi keep their service fee when a player cancels, which hands rivals a "no player fees" pitch.
6. **Chicken-and-egg.**
   - Playtomic sells demand bundled with its software.
   - AYO built its community (sparring, 42k teams) **before** launching venue booking in March 2022 **[secondary]**: https://teknologi.bisnis.com/read/20220318/266/1512094/ayo-indonesia-bikin-fitur-booking-lapangan-di-jawa-dan-sumatra
   - Reclub and PDL Up are **organizer-first**.
   - ReMatch's self-declared holds at public venues fit the organizer-first path, but they produce **no booking GMV**.
7. **Fixed per-transaction and per-account processor costs** punish small split shares and casual hosts (see sections 1 and 2). Confirm Midtrans/Xendit pricing for QRIS, virtual accounts, e-wallets, sub-accounts and disbursements before setting fee floors.
8. **Split-payment debt risk.** When shares go unpaid, someone absorbs the gap. Playtomic uses a pre-authorization hold or charges the owner's card on file, and failing that "the club is responsible." Card pre-auth is uncommon with QRIS, virtual accounts and e-wallets, so ReMatch probably needs **pay-to-join (prepaid shares) or host-pays-all with in-app reimbursement**.
9. **App-store subscriptions.** Playtomic Premium, Reclub Supporter, PDL Up Pro and DUPR+ are all billed through Apple and Google, which own refunds. The store commission rate was not researched here.

---

## 4. Revenue approaches for ReMatch

Indicative numbers are my estimates, anchored to the comparables above, in IDR with USD at Rp18,000. They are not validated.

### A. Venue-SaaS-led ("AYO AVM / CourtReserve with a social layer")

- **Who pays:** venues, about **Rp500k–2.5M per month (US$28–140)** by court count. That is priced against AYO AVM (price unknown) and well below CourtReserve's US$159–549. Players, hosts and clubs use it free, with no commission.
- **Value:**
  - **Venue:** schedule, pricing and hours, memberships, tournaments and Seasons, and demand from ReMatch players.
  - **Host and club:** free.
  - **Player:** no fees.
- **Durability:** high. It is sticky and unaffected by cash leakage.
- **Cold start:** poor. It needs field sales city by city, and **AYO is already the default "no commission" choice.**
- **Risks:** a head-on fight with AYO, low SME software budgets, and public-venue Games generating nothing.

### B. Take-rate marketplace ("Playtomic/MATCHi")

- **Who pays:**
  - **Players:** a fee on joins and split shares of about **4% with a minimum of Rp5,000–6,000 (US$0.28–0.33)**, matching Playtomic Indonesia.
  - **Entry fees** for classes, leagues and tournaments: about **4% capped at about Rp35k–40k (US$2)**.
  - Court bookings **free** for players, because that is the norm in Indonesia for both Playtomic and AYO.
  - **Venues:** optionally a small commission (about 2–5%) on marketplace bookings only.
- **Value:**
  - **Venue:** free software plus demand.
  - **Host:** a guaranteed, pre-split court.
  - **Club:** fee collection.
  - **Player:** discovery and fill.
- **Durability:** medium. It scales with online GMV, which leaks to WhatsApp and transfers, cash, memberships and wallets.
- **Cold start:** medium. Open Games at public courts earn nothing.
- **Risks:** WhatsApp leakage, AYO's no-commission positioning, the processor's fixed fee eating small shares, and debt handling without card pre-authorization.

### C. Player-premium freemium ("Reclub/PDL Up/DUPR")

- **Who pays:**
  - **Players:** Premium at about **Rp29k–59k/month or Rp299k–449k/year (US$1.6–3.3 / US$17–25)**, priced between Reclub and PDL Up. It unlocks fee waivers on joins, full Level history and analytics, priority waitlist and alerts, promoted Games, and a badge.
  - **Organizers:** Organizer Pro at about **Rp79k/month or Rp399k/year (US$4.4/22)**, matching PDL Up, for large Americanos, Seasons, guest entrants and exports.
  - **Brand sponsorship** of leagues and Seasons (the AYO/Teh Botol and DUPR/JOOLA pattern), plus ads on the free tier.
- **Value:**
  - **Venue:** free.
  - **Host and club:** the core is free.
  - **Player:** free core plus status and insight on top.
- **Durability:** medium-low. Revenue per user is low in Indonesia, subscriptions churn, and the app store takes its commission.
- **Cold start:** best. It works at public courts with zero venues.
- **Risks:** it needs large scale. Keep the core Level free (the DUPR model). Ads can annoy the community.

### D. Hybrid, sequenced ("organizer-first, small payment margin, SaaS later") — recommended

- **Phase 1:**
  - **Free for everyone.** When money moves in the app (split-at-booking, club dues, entry fees), charge a **transparent, Spond-style fee of the processor's cost plus about 1%, with a floor of about Rp3k–5k per payment**. The host or club chooses whether the payer or the organizer absorbs it.
  - **Cash/transfer "mark paid" stays free**, which competes with WhatsApp instead of fighting it.
  - **Organizer/Club Pro at about Rp49k–99k/month (US$3–5.5)** for Seasons, large brackets, guest entrants and exports.
  - **Sponsorship** of leagues and Seasons.
- **Phase 2:** player Premium at about **Rp29k–49k/month** (join-fee waiver, Level analytics, priority waitlist).
- **Phase 3:** **venue SaaS at about Rp500k–1.5M/month** with no commission on the venue's own customers, sold to venues that already receive ReMatch Games. **Match AYO's no-commission pitch instead of fighting it.**
- **Value:**
  - **Venue:** demand first, then software.
  - **Host and club:** easier than chasing transfers.
  - **Player:** free play with optional Premium.
- **Durability:** medium-high once Phase 3 lands, because it draws on several revenue streams.
- **Cold start:** good, since no venues are needed.
- **Risks:** complexity, a thin payment margin until volume grows, and backlash if fees are added later. Publish the fee roadmap and grandfather early users.

### Comparison

| | A. Venue SaaS | B. Take-rate | C. Player premium | D. Hybrid |
|---|---|---|---|---|
| **Primary payer** | Venue | Player (+ venue) | Player (+ power organizers, sponsors) | Online payers → organizers → players → venues |
| **Indicative price** | Rp500k–2.5M/venue/mo (US$28–140) | 4%, min about Rp5.5k (US$0.31), on joins and entries; bookings free | Rp29k–59k/mo per player (US$1.6–3.3); Org Pro about Rp79k/mo (US$4.4) | Processor cost +1% (floor Rp3k–5k); Pro Rp49k–99k/mo; later SaaS Rp0.5–1.5M/mo |
| **Venue value** | High | Medium | Low | Medium → high |
| **Host/club value** | Medium | Medium | High | High |
| **Player value** | High | Medium (bookings free, joins charged) | High | High |
| **Revenue durability** | High | Medium (leaks) | Medium-low (low revenue per user) | Medium-high |
| **Cold-start fit** | Poor | Medium | Best | Good |
| **Works at public courts** | No | No | Yes | Yes |
| **Cash/WhatsApp leakage exposure** | None | High | None | Medium (designed in) |
| **Main SEA risk** | AYO incumbent | WhatsApp leakage, AYO no-commission | Low revenue per user | Complexity |
