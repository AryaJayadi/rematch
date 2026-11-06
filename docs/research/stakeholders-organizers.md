# Stakeholder research: organizers (hosts, clubs and communities, tournament and league organisers)

**Purpose:** work out what Indonesian/SEA organisers want, what hurts, and which ReMatch features and growth loops follow from that.
**Accessed:** 2026-11-05, unless a date is given.
**Labels:**
- A plain URL is a primary or first-hand source: an app-store review, an organiser's own listing, an official product page, an organiser quoted in the press, or a survey's own results page.
- **[secondary]** marks a news or vendor summary, or a figure I could not open myself.
- **[via search excerpt]** means I saw the quote in the search index but could not open the full document.
- Every line marked **Rec** (and every cell in a "ReMatch response" column, plus all of §4) is **my recommendation**, not a sourced fact.

**Related files:** `monetization.md` (comparables and pricing anchors) and `payments-provider.md` (Midtrans/Xendit fees, the QRIS 0% MDR rule). Sibling stakeholder files cover venues and coaches (`stakeholders-venues-coaches.md`) and players and brands (`stakeholders-players-brands.md`).

**Source caveats:**
- **Reddit and Kaskus could not be fetched** from this environment, and web search returned no matching threads. Few first-hand Indonesian forum voices are included as a result.
- The richest first-hand data comes from **Reclub Labs surveys** (May 2026). They have 554–1,086 respondents each, about 40% of them Indonesian. The samples are self-selected Reclub users, and Reclub discloses a commercial DUPR integration. Treat the percentages as directional.
- **Organiser listings on Reclub** (public meet, club and competition pages) are the best evidence of how hosts actually run things today. They show prices, payment accounts and refund rules in the organisers' own words.

---

## TL;DR

**How organising works today:**
- Indonesian organisers already list meets on apps (Reclub, AYO, KUYY!), but **the money still moves by hand**. The steps are:
  1. Transfer to the host's personal BCA or BNI account, or scan a QRIS.
  2. Send the proof ("bukti transfer") to the admin.
  3. The admin confirms the slot by hand.
- Refund policies push the risk onto players: "no refund, find your own replacement."
- Sources for this pattern: https://reclub.co/m/IHGQWS , https://reclub.co/m/3MLKPR , https://reclub.co/clubs/@tepokbulubandung

**Top 3 pains per stakeholder:**

| Stakeholder | Top 3 pains |
|---|---|
| **Hosts** | 1. No-shows and late dropouts leave the host out of pocket.<br>2. Collecting and reconciling transfers by hand.<br>3. Level mismatches, because labels are self-rated and gamed. |
| **Clubs** | 1. Dues and kas tracked by hand (cash, transfers, screenshots).<br>2. Admin load falls on a few volunteers, so the club depends on them (bus-factor).<br>3. Trust and retention: players leave over value-for-money, and there is no reputation signal. |
| **Tournament organisers** | 1. Registration means a Google Form plus a transfer plus manual proof-matching.<br>2. Eligibility and sandbagging ("second accounts") damage fairness.<br>3. Manual draws, scoring and standings cause errors and slow events. |

**Top 3 opportunities per stakeholder (my recommendations):**

| Stakeholder | Top 3 opportunities |
|---|---|
| **Hosts** | 1. **Pay-to-confirm** with QRIS, plus automatic waitlist promotion.<br>2. **Slot transfer / sell-my-spot**, which turns "find your own replacement" into a feature.<br>3. **Level-gated Open Games** using a match-based Level and host verification. |
| **Clubs** | 1. **Free club core plus a dues ledger**, with in-app or cash "mark paid."<br>2. **Co-admin roles and an audit trail**, which address the bus-factor.<br>3. **Public club page** with ratings and a "would join again" signal, which also works as SEO. |
| **Tournament organisers** | 1. **Registration plus entry-fee checkout** that replaces the Google Form, with an automatic team roster.<br>2. **Eligibility checks from verified match history**, to fight sandbagging.<br>3. **Shareable live leaderboard and bracket links** (no app needed), with **guest entrants who later claim their history**. |

**Surprises:**
1. **Communities become venues.** Rich Padel, which calls itself the largest padel community in Indonesia, is opening courts at five sites (https://timesindonesia.co.id/indonesia-positif/540054/rich-padel-komunitas-padel-terbesar-di-indonesia-pimpin-perkembangan-olahraga-padel-nasional). El Grande ran a 375-team tournament as its venue's grand opening (https://www.jawapos.com/sports/2605220310/el-grande-gelar-turnamen-padel-terbesar-di-jatim). In Indonesia, organisers and venues blur together.
2. **Indonesian players expect fast hosts.** 65% want a reply within one hour, versus 23% in Singapore. 60% flag late starts as a red flag (https://coolmeets.reclub.co/research/host-experience-2026-results). Responsiveness is a host KPI that ReMatch could measure.
3. **Ticket sizes vary by sport by roughly 8×.** A badminton mabar is **Rp28k per person** (https://reclub.co/clubs/@tepokbulubandung), while padel is **Rp110k–230k** (https://reclub.co/m/3MLKPR , https://health.detik.com/kebugaran/d-8006316/fomo-olahraga-padel-segini-harga-patungan-sewa-lapangan-per-orang-di-jaksel). A flat Rp3k–5k payment floor (from `monetization.md`, option D) would take **11–18% of a badminton slot**, so the fee design has to scale with the ticket size.
4. **Scoring sits in a separate app.** One respondent said: "People rarely use Reclub to keep score. They use Americano or Padlup" (https://coolmeets.reclub.co/research/padel-ratings-indonesia-2026-results). The organiser workflow is split across 2–3 apps plus WhatsApp, which is the opening ReMatch can take.
5. **Players want to rate hosts, and hosts accept it.** 90% support public host ratings (https://coolmeets.reclub.co/research/host-experience-2026-results), and AYO already shows host ratings and a "Superhost" filter (https://ayo.co.id/main-bareng/newbie-friendly-coaching-class-with-coach-weno-certitifed-padel-coach). Host reputation is becoming a basic expectation.

---

## 0. How organising works today (evidence)

- **Discovery on the app, money by hand.** Real Indonesian listings, in the organisers' own words:
  - **Semarang, padel, CLUB 79:** "IDR 120,000 / player", "Bank Transfer: BCA … a/n [personal name]", "No Cancellation: If you cannot attend, please find your own replacement player", "Host Cancel: 100% refund." https://reclub.co/m/IHGQWS
  - **Palu, padel, PLAY. Collective:** "Request-Waitlisted-Transfer-You are in the game", "First Pay, First Served", "Transfer to BNI an. [personal name]", "No reschedule or refund. But you can make others use your slot." https://reclub.co/m/3MLKPR
  - **Bandung, badminton, Tepok Bulu Bandung (494 members, 83 activities):** "Request → Waitlist → Payment → Confirmed … Setelah masuk waitlist, lakukan payment (No payment sebelum waitlist)." In English: *"Once you're on the waitlist, make the payment (no payment before the waitlist)."* The price is Rp28k per person, paid by QRIS, and players then send proof to the admin. https://reclub.co/clubs/@tepokbulubandung
  - **Jakarta, JakartaXPadel (488 members):** "No refund if you decide cancel, refund if only admin decide cancel the match … your payment becomes deposit for next match (reschedule)." https://reclub.co/clubs/@jakartaxpadel
- **Reclub's payment model is deliberately off-platform.** Hosts list bank or wallet details, players upload a receipt screenshot, and the host marks them paid (details in `monetization.md` §1). https://help.reclub.co/hc/reclub-help/articles/1765845748-how-do-payments-work-in-recub
- **WhatsApp is the backbone.**
  - It is the most-used social app in Indonesia (Q2 2025) **[secondary]**: https://databoks.katadata.co.id/en/technology-telecommunications/statistics/69959bb37aa8b/10-most-used-social-media-platforms-in-indonesia-in-q2-2025
  - An ethnography of a Jakarta squash community found that ties form across digital and physical space: "percakapan informal setelah bermain, aktivitas nongkrong bersama, komunikasi melalui grup WhatsApp" (*informal chats after play, hanging out together, communication through WhatsApp groups*). https://journal.ubm.ac.id/index.php/semiotika/article/download/10050/pdf **[via search excerpt]**
- **The social side is the product.**
  - Rich Padel's co-founder: "Banyak yang bilang, main padel itu kayak nongkrong sambil keringetan" (*Many say playing padel is like hanging out while sweating*), and "value-nya bukan cuma di harga, tapi di experience" (*the value isn't only the price but the experience*). https://health.detik.com/kebugaran/d-8006316/fomo-olahraga-padel-segini-harga-patungan-sewa-lapangan-per-orang-di-jaksel
  - Reclub players most want a host who is a "Greeter" (51–55%) or a "Matchmaker" (35–44%). 71–82% stay engaged after the session. https://coolmeets.reclub.co/research/social-sports-2026-results

---

## 1. Hosts of casual games (mabar, open play, sparring)

### What they're trying to achieve
- **Fill the court and keep it full.** Hosts "chase down RSVPs, rebuild the pool when someone bails," which one writer calls "a real job" done "for free, out of love for the game." https://pickleball.com/learn/confessions-of-a-pickleball-group-chat-hostage
- **Not be out of pocket.** In Reclub's survey, an Indonesian respondent wrote: "Add Indonesia local payment gateways like Xendit or Midtrans. We had no-shows and lost money." https://coolmeets.reclub.co/research/host-experience-2026-results
- **Balanced games.** Fair skill grouping is a top host trait (25%, and 55% in the Philippines), and 12% of players have stopped joining a host over unfair matchmaking. https://coolmeets.reclub.co/research/host-experience-2026-results
- **A reputation, and sometimes an income.** Many Indonesian hosts are semi-professional: their fees bundle court, balls, photos and a scorer.
  - JakartaXPadel describes itself as "komunitas sekedar tukang cariin lapangan dan mengatur permainan untuk temen-temen main bareng" (*a community that just finds courts and arranges games for friends to play together*). https://reclub.co/clubs/@jakartaxpadel
  - 40% of players would pay 10–25% more for a host they trust. https://coolmeets.reclub.co/research/host-experience-2026-results
- **Play more, with less admin.** The pickleball.com writer: "I do not love the 17 group texts it takes to play it." https://pickleball.com/learn/confessions-of-a-pickleball-group-chat-hostage

### Goals, pains and ReMatch response

| Goal | Pain (evidence) | ReMatch response (**Rec**) |
|---|---|---|
| Not out of pocket | **Fronting the court and absorbing no-shows.** "We had no-shows and lost money" (https://coolmeets.reclub.co/research/host-experience-2026-results). About 8% of open-text comments concern scams or no-shows (same source). Hosts reply with blunt rules such as "No reschedule or refund" (https://reclub.co/m/3MLKPR). | **Pay-to-confirm** (a slot is held only once paid) with a configurable hold window. For host-pays-all, show **live "who still owes" tracking** with one-tap nudges, and allow a cash or transfer "mark paid." Make **QRIS the default**: it is 0% MDR up to Rp100k (see `payments-provider.md`). |
| Minimal admin | **Manual reconciliation.** "Request → Waitlist → Payment → Confirmed" plus proof screenshots to the admin (https://reclub.co/clubs/@tepokbulubandung). Personal bank accounts appear in public listings (https://reclub.co/m/IHGQWS). | **Automatic confirmation when payment succeeds.** No screenshots and no personal account number in public. Keep a per-game ledger that exports to WhatsApp as a ready-made recap ("6/8 paid, 2 pending"). |
| Fill the court | **Last-minute dropouts.** Listings make players "find your own replacement" (https://reclub.co/m/IHGQWS) or say "you can make others use your slot" (https://reclub.co/m/3MLKPR). | **Slot transfer / sell-my-spot.** The leaving player offers the slot to the waitlist or a friend, and the money moves automatically. **Automatic waitlist promotion** with a short accept timer. Optional **"needs 1" push** to nearby players at the right Level. |
| Fill the court | **Finding a 4th.** Some groups exist only for when "someone needs a 4th, or an 8th, at the last minute" (https://pickleball.com/learn/confessions-of-a-pickleball-group-chat-hostage). For newcomers: "cari teman untuk main susah" (*finding people to play with is hard*) (https://momsmoney.kontan.co.id/news/padel-jadi-tren-komunitas-jadi-wadah-antar-pemain-1). | Public **Open Games** feed filtered by sport, area, time and Level. Followers of a host get a notification when the host posts. |
| Balanced games | **Level mismatch.** 47% say levels at meets are "hit-or-miss or rarely" accurate. 79% self-rate without an anchor. Trust in others' ratings averages 4.9/10. "Sometimes the 2.0 doesn't play as good as the 1.5" (Yogyakarta). https://coolmeets.reclub.co/research/padel-ratings-indonesia-2026-results | **Level range on every Open Game**, with a soft or hard gate. A **match-based Level** that starts as a provisional self-rating, plus **host verification**. Respondents trust hosts: "Hosts who know their community can tell within 5 minutes" (same source). **Team balancer** for the session. |
| Reputation | **No feedback loop.** "There should be a 'how's your experience with this session' segment … not only blindly enjoy the player's money … player should've been able to give ratings to the club" (https://apps.apple.com/id/app/reclub/id1323924315). 90% support public host ratings. Concerns: unfairness from one bad day (32%) and discouraging new hosts (19%). https://coolmeets.reclub.co/research/host-experience-2026-results | **Post-game "would join again" plus a star rating.** Hide it until there are at least N ratings, and show a "New host" badge so new hosts don't start with a penalty. Show **host stats**: games run, fill rate, on-time start rate, median reply time. |
| Trust between strangers | **Scams and ghost hosts.** "Blacklist clubs that tend to disappear when you have already transferred" (https://coolmeets.reclub.co/research/host-experience-2026-results). KUYY! is praised for being "minim scam" (*low on scams*) (https://apps.apple.com/id/app/kuyy/id1568995491). | **Funds held until the game happens.** The host is paid out after the game, or automatically refunded if the host cancels. Add **verified-host KYC** for paid games. "Host cancels → 100% refund" is already the norm in listings, so enforce it in the system. |
| Fair fees | **Fee backlash.** AYO review: "Saya mau bayar, harus ada convenience fee tambahan yang saya rasa tidak masuk akal apalagi dibebankan ke konsumen" (*I want to pay but there's an extra convenience fee that I find unreasonable, especially charged to the customer*) (https://apps.apple.com/id/app/ayo/id1539581877). "Too expensive for what I got" is the top reason players stop joining a host (37%) (https://coolmeets.reclub.co/research/host-experience-2026-results). | **Show the all-in price before a player joins.** Let the **host choose who absorbs** the fee. Make fees **scale with the ticket**: no flat floor on Rp28k badminton slots. |
| Reliability | **App outages push hosts back to WhatsApp.** "Better booking langsung via admin court nya sih daripada kesel gara2 gangguan terus" (*better to book directly via the court admin than get annoyed by the constant outages*) (https://apps.apple.com/id/app/ayo/id1539581877). Reclub's recent iOS reviews are dominated by force-closes after an update (https://apps.apple.com/id/app/reclub/id1323924315). | Treat **reliability at peak times (evenings, weekends) as a feature**. Make every game page **WhatsApp-shareable and openable on the web without the app**, so a host is never blocked by an app problem. |

Other host evidence:
- **Futsal and football sparring** has its own cost rule: the losing team pays the court, or a larger share such as 60/40 (Banjarmasin study, https://idr.uin-antasari.ac.id/31319/8/BAB%20V.pdf **[via search excerpt]**). **Rec:** support a "loser pays X%" split option for team-vs-team sparring.

### How this helps ReMatch
- **Acquisition:**
  - Every Open Game link a host drops into WhatsApp is an invite.
  - Players who join by link and pay by QRIS create an account at the moment of payment. This is my recommendation, and the loop is that "the host is the distributor."
  - Hosts are the supply side. Win about 50 active hosts in one city and the players follow.
- **Branding:** "the app where you don't chase transfers" and "no more 'sudah transfer?'" make a clear, local pitch.
  - Note: I found no primary source using the exact phrase "sudah transfer?". The surrounding behaviour (proof-of-transfer screenshots to the admin) is well documented above.
- **Awareness:** public Open Game pages get indexed.
  - Reclub meet pages (reclub.co/m/…) came up again and again in my Indonesian web searches for mabar and padel terms. That is my own observation, and it shows organic-search value.
- **Revenue:**
  - The **payment fee on online shares** is the main host-side revenue.
  - Hosts who run paid sessions as a small business are prospects for a **Host Pro** tier: analytics, recurring games, verified badge, promoted listing.

---

## 2. Clubs and communities (recurring groups and their admins)

### What they're trying to achieve
- **Growth from a real need.** Brakepad was founded because "cari teman untuk main susah" (*it was hard to find people to play with*). It has 450+ members on Reclub and plays Wednesdays and Fridays, each member chipping in Rp160k–200k. https://momsmoney.kontan.co.id/news/padel-jadi-tren-komunitas-jadi-wadah-antar-pemain-1
- **Identity and belonging.** WINpadel wants "ruang bagi anggota untuk bermain, belajar, dan berkembang bersama" (*a space for members to play, learn and grow together*) (same source). A thesis on a basketball community says success "tidak lagi dapat diukur hanya dari kualitas kegiatan olahraga semata, tetapi juga dari kemampuan membangun ikatan sosial" (*can no longer be measured only by the quality of the sport, but by the ability to build social ties*). https://kc.umn.ac.id/id/eprint/48657/2/BAB_I.pdf **[via search excerpt]**
- **Scale and influence, sometimes into a business.** Rich Padel's founder: "Kami ingin menjadi komunitas padel terbesar dan paling berpengaruh di Indonesia" (*we want to be the largest and most influential padel community in Indonesia*). They are opening courts at five sites in Jakarta, Tangerang, South Tangerang, Bekasi and Makassar. https://timesindonesia.co.id/indonesia-positif/540054/rich-padel-komunitas-padel-terbesar-di-indonesia-pimpin-perkembangan-olahraga-padel-nasional
- **A sustainable treasury.** Club fees bundle court, balls, a scorer, photos and loan rackets (JakartaXPadel, https://reclub.co/clubs/@jakartaxpadel). Football communities split the kas into "kas iuran pemain" (*players' per-activity contributions*) and "kas tim" (*team fund*) for general costs (template club statute, https://docs.google.com/document/d/1a-7hrJ2PwHcqrnd5IdDMAJOOmO2lJh_J/export?format=pdf **[via search excerpt]**).
- **Sponsorship and brand ties.** Brands court communities. Muscle First held a 7 km fun run "bersama 7 komunitas lari" (*with 7 running communities*), including Hoka Run Club (https://m.jpnn.com/news/700-runners-dari-7-komunitas-meriahkan-anniversary-muscle-first **[secondary]**). AYO sells "AYO Kolaborasi" brand reach (see `monetization.md`).

### Goals, pains and ReMatch response

| Goal | Pain (evidence) | ReMatch response (**Rec**) |
|---|---|---|
| Sustainable treasury | **Dues and kas tracked by hand.** An interviewed badminton club admin: "memiliki kesulitan dalam mengelola data anggota klub, jadwal latihan, iuran, dan turnamen" (*has difficulty managing member data, training schedules, dues and tournaments*), with dues paid in cash (https://repositori.telkomuniversity.ac.id/pustaka/files/209726/bab1/portal-klub-bulutangkis-cahaya-melati-dalam-bentuk-buku-karya-ilmiah.pdf **[via search excerpt]**). Reclub receipts require *both* host and player to buy Supporter (`monetization.md`). | **Club ledger** covering dues, per-meet fees and expenses, with **cash and transfer "mark paid" free forever** and in-app payment optional. Show a **"kas" balance** members can see if the club turns it on. Add **recurring dues** and membership passes. KUYY! already sells "membership pass, bundles and voucher codes" (https://apps.apple.com/id/app/kuyy/id1568995491). |
| Attendance and waitlists | **RSVP-to-payment gap.** Clubs run a manual "Request → Waitlist → Payment → Confirmed" flow (https://reclub.co/clubs/@tepokbulubandung). Reclub's own pitch: "manage your players with team assignments, RSVP's + waitlists" (https://reclub.co). | **RSVP, waitlist and payment in one state machine.** Track **attendance per member automatically** (attended, no-show, late cancel). Give admins an optional **no-show strike** policy. |
| Admin load and bus-factor | Admin work falls on a few volunteers. Organisers do it "for free, out of love for the game" (https://pickleball.com/learn/confessions-of-a-pickleball-group-chat-hostage). In Western club sport, admin is cited as the main driver of volunteer burnout (https://tidyhq.com/blog/sport-administration-is-broken **[secondary, vendor]**). Reclub host responsiveness expectations are high (65% of Indonesians want a reply within an hour) (https://coolmeets.reclub.co/research/host-experience-2026-results). | **Roles** (owner, admin, host, treasurer) with **co-hosts on every meet**. **Recurring meet templates.** An **ownership-transfer flow** and an **audit log** of money actions, so the club survives if its founder leaves. **Saved answers to FAQs**: KUYY! shipped "Hosts can now answer common questions once" (same KUYY! URL). |
| Retention | Players leave over value and fairness: "too expensive for what I got" (37%), unfair matchmaking (12%) (https://coolmeets.reclub.co/research/host-experience-2026-results). 51–52% prefer mixed levels, while 32% want tight competition at their level (https://coolmeets.reclub.co/research/social-sports-2026-results). | **Club health dashboard**: active members, retention cohort, no-show rate, fill rate. Offer **two meet types** (social mix and level-tight). A **"we miss you" nudge** to lapsed members (opt-in). **First-session discount for new members**: KUYY! offers "40–100% discount on a new participant's first booking" (https://apps.apple.com/id/app/kuyy/id1568995491). |
| Moving off WhatsApp (without fighting it) | WhatsApp is where community life happens (https://journal.ubm.ac.id/index.php/semiotika/article/download/10050/pdf **[via search excerpt]**). Group chats bury information and ping people "at random hours, forever" (https://pickleball.com/learn/confessions-of-a-pickleball-group-chat-hostage). KUYY! lets hosts send invoices through WhatsApp **[secondary, from a search summary of KUYY! release notes I could not verify on the current page]**. | **Don't replace WhatsApp, feed it.** One-tap "share to WA group" for each meet, with a rich preview of slots left, Level range and price. A **weekly digest message** the admin can paste. Deep links open in the app or on the web. Keep chat in the app optional. |
| Identity and branding | Clubs build identity through jerseys, names and logos. A Reclub-listed tournament fee includes a jersey per player (https://reclub.co/c/019d13e3-d84a-71c1-8a1c-b362286d221a). Reclub iOS review: a user "can't change profile picture either personal or for club" (https://apps.apple.com/id/app/reclub/id1323924315). | **Public club page** with logo, cover, colours, member count, upcoming meets, results and photo highlights. A **custom handle** (rematch.id/c/brakepad). **Posts and feed** for club news. |
| Finding venues | Communities act as "tukang cariin lapangan" (*court-finders*) (https://reclub.co/clubs/@jakartaxpadel). Venue cancellation risk is passed on: "No refund if cancellation decide by court management but your payment becomes deposit for next match" (same URL). | **Recurring block bookings** at partner venues, with venue-side cancellation handled as **automatic credit to members**. Keep "self-declared" bookings at public venues. |
| Sponsorship | Brands court communities (running, padel). Sponsor prizes rate 6.1/10 in importance to players, so prizes come second to the experience (https://coolmeets.reclub.co/research/host-experience-2026-results). | **Sponsor slots** on the club page, meet pages and leaderboards. A **verified club stats sheet** (members, weekly actives, city, sport mix) clubs can send to brands. Later, a **ReMatch-brokered brand marketplace**. |

### How this helps ReMatch
- **Acquisition:** clubs bring members in bulk.
  - Brakepad, JakartaXPadel and Tepok Bulu Bandung each have about 450–500 members on Reclub. Each moved its whole community onto one app, and joining was simply "Cukup bergabung di aplikasi Reclub" (*just join on the Reclub app*) (https://momsmoney.kontan.co.id/news/padel-jadi-tren-komunitas-jadi-wadah-antar-pemain-1).
  - **Rec:** offer a **free import tool** (paste a WhatsApp member list, or import a Reclub CSV) and a white-glove migration for the top 20 clubs per city.
- **Branding and awareness:** public club pages work as **local SEO** for searches like "komunitas padel Bekasi" or "mabar badminton Bandung". Reclub pages already rank for these searches (my observation).
- **Revenue:**
  - A **Club Pro** plan (see §4).
  - The **fee on online dues and meet fees**.
  - **Brand deals** brokered across many clubs. That is the AYO Kolaborasi model (`monetization.md`).
- **Strategic note:** clubs that grow into venues (Rich Padel) are **future venue-SaaS customers**. Win them as clubs first.

---

## 3. Tournament and league organisers

### What they're trying to achieve
- **Big, smooth events.** El Grande (Surabaya) ran **375 teams (750 players) across 11 categories**, with **50 more teams on a waiting list**, as the grand opening of a 12-court venue. https://www.jawapos.com/sports/2605220310/el-grande-gelar-turnamen-padel-terbesar-di-jatim
- **Bringing in new players.** El Grande's committee chair said of the rookie category: "Banyak pemain yang selama ini belum berani ikut di turnamen" (*many players who until now didn't dare join a tournament*). The rookie category drew 62 teams (same source).
- **Fair categories.** Organisers write detailed eligibility rules.
  - Upper Beginner category: "Pemain yang pernah berpartisipasi dalam turnamen level Silver, Gold, atau level di atasnya" are barred (*players who have played Silver, Gold or higher tournaments*), plus caps on past wins. "Panitia berhak mendiskualifikasi peserta yang melanggar persyaratan kualifikasi" (*the committee may disqualify participants who break the qualification rules*). https://reclub.co/c/019d13e3-d84a-71c1-8a1c-b362286d221a
- **Sponsors, prizes and networking.**
  - Minala Cup: Rp103.5M in prizes plus an Umrah trip (https://www.rctiplus.com/news/detail/inews/5466857/minala-cup-padel-tournament-tebar-hadiah-rp103-5-juta-dan-umrah **[secondary]**).
  - Wali Kota Cup Padel: run by PBPI with title sponsor Esteh Indonesia **[secondary, search summary]** (https://rri.co.id/bogor/olimpik/1960479/ratusan-peserta-padel-ramaikan-wali-kota-bogor-cup).
  - Corporate tournaments are "bukan sekadar kompetisi, tetapi juga untuk berjejaring" (*not just competition but networking*) (Katadata, https://katadata.co.id/foto/6a93ca0d0871c/foto-kompetisi-sembari-bangun-relasi-di-katadata-padel-tournament-2026 **[secondary]**).
- **Legitimate ratings.** DUPR-registered players in Indonesia grew **405%** over 12 months (https://english.vov.vn/en/sports/vietnam-among-worlds-top-10-fastest-growing-pickleball-nations-post1340523.vov **[secondary]**). Pickleball organisers increasingly need ratings that others recognise.

### Goals, pains and ReMatch response

| Goal | Pain (evidence) | ReMatch response (**Rec**) |
|---|---|---|
| Smooth registration and payment | **Google Form plus personal account plus proof-matching.** "📝 Register Now Here! (forms.gle/…)", "Transfer to BCA … ([personal name])", Rp750k per team (https://reclub.co/c/019d13e3-d84a-71c1-8a1c-b362286d221a). "Tolong sertakan nama tim pada saat melakukan transfer, sehingga memudahkan kami dalam mensorting bukti transfer yang masuk" (*please include your team name in the transfer so we can sort the incoming proofs of transfer*) (https://reclub.co/c/019d1861-6f56-7cad-8902-4eae136458a2). | **Team registration with built-in checkout.** Partner invite, a custom form (jersey size and so on) and QRIS or VA payment, so the **roster is auto-built and paid status is known**. A refund policy engine: "refund only before the tournament starts" is a common rule (same URL). |
| Fair categories | **Sandbagging and eligibility.** "Many padel players create second accounts just to enter competitions below their actual levels" (https://coolmeets.reclub.co/research/padel-ratings-indonesia-2026-results). DUPR: "Too easy to fake. Others just open a new DUPR account for a competition" and "players artificially lower their DUPR to enter rookie tournaments" (https://coolmeets.reclub.co/research/dupr-pulse-2026-results). | **Eligibility checks against verified history**: past category wins, playoff appearances and Level. Raise **duplicate-account flags** (same phone, same payment instrument). Give the committee a **review queue** ("curated by the event committee" is already standard). Add **verified Level** from sanctioned or logged results. |
| Accurate draws, scoring and standings | **Manual work is error-prone.** A 2026 Indonesian study notes manual padel tournament management "is still prone to score recording errors, ranking calculation mistakes, match scheduling conflicts, and errors in managing knockout brackets" (Wasesha, ELKOM 19(1), https://journal.stekom.ac.id/index.php/elkom/article/download/3779/2539/14941). Manual bracket-making "membuat panitia … menghabiskan waktu yang cukup lama" (*makes the committee spend a long time*) and can hurt trust in the draw's transparency (https://proceedings.ums.ac.id/korma/article/download/6719/6038/12760 **[via search excerpt]**). | **Format engine**: Americano, Mexicano, round-robin pools to knockout, the "Wins → Head-to-Head → Point Difference" tiebreakers organisers already publish (https://reclub.co/c/019d1861-6f56-7cad-8902-4eae136458a2), seeded draws with **published seeding logic**, and a court scheduler. |
| Live scoring | **Scoring happens in a separate app.** "People rarely use Reclub to keep score. They use Americano or Padlup" (https://coolmeets.reclub.co/research/padel-ratings-indonesia-2026-results). PDL Up reviews praise in-play changes ("keren bisa ganti dan off orang di saat main", *great that you can swap and bench people mid-play*) and complain when score export fails ("why always cannot download the score?") (https://apps.apple.com/id/app/id6752893039). | **Court-side scoring by players or referees** on a phone, with offline tolerance. **Mid-event edits** (swap, withdraw, add a late entrant). Exportable results (PDF or image for Instagram). |
| No-shows | Futsal rulebooks: a team absent 10 minutes after being called loses by walkover, and some use cash deposits (https://porprovkalsel.hulusungaiselatankab.go.id/handbook/handbook-futsal.pdf **[via search excerpt]**). Waitlists exist at scale (50 teams at El Grande). | **Check-in** by QR at the venue. **Automatic walkover** after a timeout. **Waitlist auto-promotion** for withdrawals before the event. A deposit or forfeit setting. |
| Guest entrants | Many entrants don't have, or won't install, the app. PDL Up's free tier sells exactly this: "Input names quickly, no login required" and "Send match link to your group – no app needed" (https://pdlup.com). | **Guest entrants** by name and phone. After the event, guests get a WhatsApp or SMS link: **"claim your results and Level"**. |
| Results sharing and reputation | Results live in screenshots and PDFs. PDL Up Pro charges for "Share & download results as PDF" and a "Private leaderboard, reveal results when you're ready" (https://pdlup.com). | **Public live leaderboard and bracket link** (web, no app), a "reveal at the end" option, **auto-generated story images** with the organiser's and sponsor's logos, and an **organiser profile** with past events and attendance. |
| Sponsors and prizes | Sponsors are central: an IKPI Depok committee said "Dukungan para sponsor menjadi bagian penting dalam menyukseskan kegiatan ini" (*sponsor support is an important part of making this event a success*) (https://ikpi.or.id/?p=32041 **[secondary]**). | **Sponsor placements** on the event page, live leaderboard and result images. A **post-event report** for sponsors (entrants, views, shares). |
| Sanctioning and legitimacy | No public Indonesian amateur padel ranking was found. Elsewhere, only federation-sanctioned events count toward national rankings (for example Padel New Zealand, https://www.padelnewzealand.org.nz/rankings **[secondary]**). Trust in DUPR averages 5.7/10, and the top reason for low trust is "inflated ratings from playing weaker opponents" (56%) (https://coolmeets.reclub.co/research/dupr-pulse-2026-results). | **Event tiers**: social, logged and verified (referee or both-captains confirmation). Only verified results move the Level fully. Seek **federation or venue partnerships** (PBPI, PB IPF, PBSI clubs) to recognise ReMatch-run Seasons. Import or link DUPR for pickleball rather than competing with it. |
| Multi-week leagues | Not directly evidenced in Indonesian sources. Organisers' Reclub listings are mostly one-day events (my observation). | **Seasons** with fixtures, standings, reschedule requests and promotion and relegation between divisions. These give clubs a weekly reason to come back. |

### How this helps ReMatch
- **Acquisition (viral loop):**
  - Each tournament brings 32–750 entrants, many as guests.
  - **Guest entrant → results link → "claim your history" → account.** This is the strongest viral loop available (my recommendation; PDL Up's no-login design shows the entry half works).
- **Awareness:**
  - **Live leaderboard links** spread in WhatsApp groups during the event.
  - **Result images** get posted to Instagram stories with the ReMatch watermark and the organiser's and sponsor's logos.
- **Branding:**
  - Become the "fair draw, verified level" brand.
  - Indonesian players already want "Playtomic rating based on real win rate" (https://coolmeets.reclub.co/research/padel-ratings-indonesia-2026-results).
- **Revenue:**
  - The **entry-fee payment fee**. Tickets are large (Rp600k–800k per team), so the payment fee fits better here than on Rp28k badminton slots.
  - A **per-event or monthly Organizer Pro** plan.
  - **Sponsor inventory**.

---

## 4. Implications for ReMatch growth and revenue (all **Rec**)

### Growth loops, ranked by how strong I expect them to be
1. **Guest entrant → claim history** (tournaments). This turns a large share of entrants into accounts after the event, with no app needed beforehand.
2. **Host link → pay-to-join by QRIS** (casual games). Paying is the moment of conversion, and the host does the distribution.
3. **Club migration** (clubs). Each migrated club brings 100–500 members. Offer an import tool and white-glove migration for anchor clubs in each city.
4. **Live leaderboard and result images** (awareness). These spread organically in WhatsApp and Instagram.
5. **Public club, host and game pages** (SEO). These serve local intent searches ("mabar padel Semarang").

### Positioning against comparables
- **vs Reclub:**
  - Reclub has the community graph, but payments are off-platform and receipts are paywalled (`monetization.md`).
  - Its own survey shows Indonesian hosts asking for Midtrans or Xendit (https://coolmeets.reclub.co/research/host-experience-2026-results), and Reclub's FAQ says in-app payments are "on the roadmap."
  - **ReMatch's window is short.** Win on payments plus slot transfer plus scoring in one app before Reclub ships payments.
- **vs PDL Up:** PDL Up owns padel scoring. Match its free tier (3 courts, no-login guests, share links) so ReMatch is never worse for a one-off Americano, then win on registration plus payment plus Level.
- **vs AYO:** AYO has host ratings, Superhost and a cash/online payment filter, but faces fee and outage backlash (https://apps.apple.com/id/app/ayo/id1539581877). Avoid both: publish the fee rules and stay reliable at peak.

### Revenue design (builds on `monetization.md` option D)

| Stream | Who pays | Proposal | Rationale |
|---|---|---|---|
| Online payment fee | Payer or organiser (organiser chooses) | % of ticket, **scaled**: about 0% extra on ≤Rp50k QRIS tickets (badminton), ramping to the ~4% Playtomic-ID level with a cap on large entry fees | A flat Rp3k–5k floor is 11–18% of a Rp28k badminton slot. QRIS ≤Rp100k is 0% MDR from 1 Oct 2026 (`payments-provider.md`), so small tickets cost little to process. |
| Club Pro | Club | **About Rp79k/mo or Rp699k/yr**. Free: unlimited members, meets, RSVP, waitlist, manual mark-paid, 2 admins. Pro: unlimited admins and roles, treasurer ledger and exports, recurring dues, health dashboard, custom handle and branding, sponsor slots, Seasons | Anchors: PDL Up Pro Rp79k/mo, Reclub Supporter Rp29k/mo (`monetization.md`). Treasurer tools and multiple admins answer the bus-factor pain directly. |
| Organizer event pass | One-off tournament organiser | **About Rp99k–249k per event**, by court count. Free up to 3 courts (parity with PDL Up) | Tournament organisers are episodic, so a monthly plan suits them badly. Big events (Rp750k × 25 teams = Rp18.75M entry gross) can easily bear it. |
| Host Pro (later) | Semi-pro hosts | About Rp29k–49k/mo: promoted listings, recurring templates, verified badge, analytics | Indonesian hosts run paid, bundled sessions as micro-businesses (JakartaXPadel, CLUB 79). |
| Sponsorship | Brands | Placements on leaderboards, result images and club pages; multi-club brand campaigns | Brands already court communities. Sponsor prizes matter less to players (6.1/10), so sell **reach**, not prizes. |

### Risks to watch
- **Leakage to cash and transfer:** keep "mark paid" free. Charging for it would push clubs back to WhatsApp.
- **Fee backlash:** the AYO convenience-fee complaint and Meetup's organiser-fee complaints ("they charge organizers a lot", https://ca.trustpilot.com/review/meetup.com) are warnings. Launch with the fee structure we intend to keep.
- **Rating gaming:** a Level that gates tournament entry will be gamed (second accounts). Duplicate-account detection is needed from day one of competitions.
- **Host-cancel liability:** holding funds until the game is played needs a legal opinion on holding third-party money (`payments-provider.md` §7).

---

## 5. Gaps and next steps
- **No first-hand Indonesian forum threads** were reachable (Reddit and Kaskus were blocked, and no X/Twitter results came up). **Next step:** run 8–10 organiser interviews (3 hosts, 4 club admins, 2–3 tournament committees) in Jakarta, Bandung and Surabaya, using the pains above as the interview guide.
- **No data on club dues amounts or kas practices** beyond academic excerpts. Ask in the interviews: do clubs charge dues, or only per-meet fees? Who holds the kas? Is it audited?
- **Sanctioning:** I found no public PBPI amateur ranking rules. Contact PBPI and PB IPF about recognition of logged or verified events.
- **The Reclub Labs survey pages contain internal inconsistencies** (for example, the share of respondents who want the skill range disclosed is given as both 26% and 44%). Quote ranges, not point figures.
