# Payments provider research: Midtrans vs Xendit (for ReMatch)

**Accessed:** 2026-11-05. Prices and policies change, and both providers changed theirs in 2026. Check the live pricing pages before you sign anything.
**Scope:** Indonesia first, with SEA expansion later. This is a marketplace money flow: player → Venue / Club organiser / Event organiser / Coach / Host, minus the ReMatch fee.
**Source labels:** links with no label are primary (official docs, help centres, regulators). **[secondary]** means a third party. **not public** means I could not find the figure.

> **Access caveat:** Xendit's marketing pricing pages (`xendit.co/*/pricing`, the pricing calculator, `/id/biaya`) return HTTP 403 to automated fetches. Xendit fees below come from three places: Xendit's own help centre and docs, the last archived official page (Wayback, 2025-11-03), and **[secondary]** reports of the post-Oct-2026 numbers. Someone should open https://www.xendit.co/en-id/pricing/ in a browser and confirm them.

---

## TL;DR

- **Xendit (xenPlatform)** has the marketplace features ReMatch needs: sub-accounts for each payee, split rules that route one charge to up to 5 destinations and take a platform fee automatically, KYC through API, payouts, and SEA coverage. Two 2026 pricing changes hurt badly at ReMatch's ticket sizes. First, an **IDR 4,000 processing fee on every attempt** (payments, payouts, refunds, and failed attempts too) from 1 Oct 2026. Second, **new xenPlatform list pricing from 1 Feb 2027: USD 10,000 setup + USD 7,500/month + USD 15 per KYC + USD 5 per active sub-account per month**.
- **Midtrans** is cheaper per transaction and has no platform fee. However, it has **no documented split-payment or sub-merchant product**. The fallback is for ReMatch to be the merchant of record and pay venues and coaches out through Midtrans Payouts (Iris). ReMatch would then hold third-party funds, run its own ledger and KYC of payees, and need a legal opinion on licensing.
- **Recommendation (detail in §9):** keep the architecture provider-agnostic (internal ledger plus a payments adapter) and get commercial quotes from **Xendit, DOKU and Midtrans** before committing. Xendit is the best functional fit only if the xenPlatform platform fee can be negotiated down. Without that, Midtrans as merchant of record plus Payouts is the cheapest MVP route, but only once a lawyer signs off.

---

## 1. Marketplace / split payments

### Xendit: xenPlatform
- **Model:** a master account plus sub-accounts. Each sub-account has its own balance and transaction history. You transact on a sub-account's behalf with the `for-user-id` header. https://docs.xendit.co/docs/sub-accounts.md
- **Split rules:** `POST /split_rules` defines `routes`, each with a `flat_amount` or `percent_amount` (up to 2 dp) and a `destination_account_id`. You apply a rule to a charge with the `with-split-rule` header. One rule can have multiple routes, so a single charge can pay several payees and the platform. The split is computed on the amount after fees and runs **when funds settle**. A 100% split fails because the fee can't be deducted. https://docs.xendit.co/docs/split-payments.md, https://docs.xendit.co/apidocs/create-split-rule.md
  - Limit: **up to 5 routes** (destinations) per split rule. https://help.xendit.co/hc/en-us/articles/10390264065561
  - Split rules can't be deleted, only stopped by no longer passing them. Split is API-only, auto-enabled for ID and PH, and needs activation for TH, MY and VN. (same article)
  - **Refunds don't reverse splits.** "Upon issuing refunds, your split fees will *not* be returned to the source account automatically." ReMatch must claw back the split with a Transfer. https://docs.xendit.co/docs/split-payments.md
  - If a split fails (e.g. too large after fees), the money stays in the receiving account and you fix it with a Transfer. (same)
- **Supported flows:** sub→master (commission), sub→sub, master→sub (marketplace). https://docs.xendit.co/docs/split-payments.md
- **Transfers API:** `POST /transfers` moves balance between any accounts, executes immediately, and only works with the master key. This is the tool for an "escrow then release" pattern (see §4). Cross-border transfers and splits are **not supported for Indonesian-incorporated accounts**, and splits must be same-currency. https://docs.xendit.co/docs/transfer-balances.md
- **Managed vs Owned sub-accounts** https://help.xendit.co/hc/en-us/articles/6787784288665:
  - *Owned:* invisible to the partner and fully controlled by the platform. The platform's name is shown to payers. The platform sets the payout schedule. **Disabled by default for Indonesia-based accounts** (you have to ask support to enable it).
  - *Managed:* the partner gets a full Xendit account, sees their own name on statements, and pays Xendit fees. You can't convert between the two types.
  - The newer docs describe a single sub-account flow with mandatory KYC. Either the platform verifies on the sub-account's behalf, or it invites the Authorized Representative. https://docs.xendit.co/docs/sub-accounts.md
- **For ReMatch:** a charge on a Reservation can go to the Venue's sub-account, with ReMatch's fee split to the master. Club, Event and Coach payments work the same way. For host-pays-all, charge the host to the Venue sub-account. "Others owe host" is a separate set of charges that route to the Host's sub-account.

### Midtrans
- **No dedicated split-payment, sub-merchant or marketplace product appears in the docs index** (searched https://docs.midtrans.com/llms.txt for split/marketplace/sub-merchant). The pricing page doesn't mention split payment or sub-merchants either. https://midtrans.com/pricing
- **Closest features:**
  - **Partner account / multi-outlet:** one Partner ID holds several Merchant IDs, managed from one dashboard. The admin can "+ Add merchant" for merchants not yet registered. Each Merchant ID is a separate merchant with its own settlement. https://docs.midtrans.com/docs/difference-between-merchant-account-and-partner-account.md, https://docs.midtrans.com/docs/merchant-administration-portal-partner-multi-outlet.md. There's **no automatic platform-fee split** between Merchant IDs, so ReMatch would have to bill its fee separately.
  - **Payouts (formerly Iris):** disburses to bank accounts and e-wallets in Indonesia. Funds come either from a deposit balance (Aggregator scheme) or from your own bank account (Facilitator scheme), with maker/approver roles. https://docs.midtrans.com/docs/disbursement-overview.md
- A competitor-written comparison also says Midtrans has no published split/sub-merchant product **[secondary, competitor]**: https://www.xendit.co/id/blog/5-payment-gateway-terbaik-di-indonesia-2026/
- **Two possible models with Midtrans:**
  - **(a) ReMatch as merchant of record:** ReMatch collects everything into one Midtrans merchant, keeps a ledger, and pays payees through Payouts.
  - **(b) Each Venue or Coach as its own Midtrans merchant under a ReMatch Partner account:** funds settle directly to the payee, and ReMatch invoices its fee separately. API-driven onboarding for (b) is **not documented publicly**, so ask sales.

**Verdict for Q1:** only Xendit can route one charge to several payees and take a platform fee automatically. Midtrans can't do this natively.

---

## 2. Onboarding sub-merchants (KYC)

### Xendit
- **Individuals are allowed, but only as xenPlatform sub-accounts:** "Individual applications are only allowed if they are XP sub-accounts." https://docs.xendit.co/docs/indonesia-business-documents.md. Account-holder entity types include `INDIVIDUAL`, `SOLE_PROPRIETORSHIP` and `CORPORATION`. https://docs.xendit.co/apidocs/create-account-holder.md. Independent coaches, hosts and club organisers without a PT/CV can therefore be sub-accounts.
- **Who does KYC:** the platform either submits KYC on the sub-account's behalf (Dashboard, or the Submit Account Verification API for OWNED / dashboard-created sub-accounts) or invites the Authorized Representative. **The AR has to complete a face-verification (liveness) scan.** https://docs.xendit.co/docs/xenplatform-global-accounts-setup.md, https://docs.xendit.co/apidocs/submit-account-verification.md, https://docs.xendit.co/docs/authorized-representative-requirements.md
- **AR documents:** government ID (KTP/passport), proof of address if it isn't on the ID, and proof of authority for companies. (same AR page)
- **Business documents in Indonesia:** PT/CV/PMA need director KTP and NPWP, company NPWP, NIB, and the deed plus the SK Kemenkumham. A sole proprietorship (Perseroan Perorangan) needs KTP, NPWP, NIB and its registration certificate. https://help.xendit.co/hc/en-us/articles/10891368765593
- **Time:** managed sub-accounts take "3-5 business days" for document verification. For Indonesia, the partner's KTP must be different from the one used for the master account. https://help.xendit.co/hc/en-us/articles/4408655256589
- **Early-account settlement holds:** individual businesses (not PT/CV/Yayasan) can get VA settlement at **T+3 working days**, or **T+7** if they sell through micro e-commerce or social media. The hold lifts after 2 months or 60 transactions. https://help.xendit.co/hc/en-us/articles/29558611188633
- **Activating xenPlatform** asks about your use case, any payment licences, the countries your sub-merchants are in, and **who is liable for chargebacks**. https://docs.xendit.co/docs/activate-xenplatform.md
- **Cost:** USD 15 per KYC/KYB attempt from 1 Feb 2027 (see §3).

### Midtrans
- **Merchant registration:** an individual needs KTP + NPWP. PT/CV/PMA need the deed, SK Kemenkumham, director KTP and NPWP, company NPWP, NIB and licences. https://docs.midtrans.com/docs/what-are-the-legal-documents-required-for-midtrans-account-registration.md
- **Individuals get fewer payment methods than business entities**, and some methods need extra activation forms. https://docs.midtrans.com/docs/differences-in-available-payment-methods-between-individuals-and-business-entities.md
- **Time:** if documents are incomplete, Midtrans contacts you within 3 working days. https://docs.midtrans.com/docs/how-can-i-have-my-money-in-my-account-payout.md. Typical end-to-end time is **not public**.
- **No sub-merchant KYC API is documented.**
  - In model (a), ReMatch would KYC its own payees (in practice: KTP, bank-account name match, NPWP for tax) before paying them through Payouts.
  - In model (b), each payee registers as a full Midtrans merchant themselves.

---

## 3. Payment methods and fees (Indonesia)

All fees are per successful transaction unless noted. **PPN:** Xendit charges a 12% rate on a base of 11/12 of the fee, which works out to **11% effective**. https://docs.xendit.co/docs/value-added-tax-vat.md. Midtrans fees exclude VAT except QRIS, GoPay and ShopeePay. https://midtrans.com/pricing

### Midtrans: https://midtrans.com/pricing (English page; the Indonesian article on the same page differs in places, noted below)
| Method | Fee |
|---|---|
| QRIS | 0.7% (VAT-incl.) |
| GoPay | 2% (VAT-incl.) |
| ShopeePay | 2% (VAT-incl.). The Indonesian article says 1.5% |
| DANA / OVO | 1.5% |
| LinkAja | not listed |
| VA / bank transfer (BCA, BNI, Mandiri, Permata, BRI, CIMB, Danamon, BSI, SeaBank, Saqu) | IDR 4,000 |
| Cards (Visa/MC/JCB/Amex/UnionPay) | 2.9% + IDR 2,000 |
| Indomaret / Alfamart / Alfamidi / DAN+DAN | IDR 5,000 |
| Akulaku PayLater / Kredivo | 1.7% / 2% |
| Direct debit | Indonesian article only: IDR 5,000 (Octo Clicks, e-Pay BRI, Danamon); IDR 2,200 + bank fee (BCA KlikPay) |
| Payouts (to bank) | IDR 5,000 |
| Payouts (to GoPay) | IDR 2,500 (English page) vs IDR 1,000 (Indonesian article). **These conflict** |
| Withdrawing your own merchant balance | free ("no additional fee"). https://docs.midtrans.com/docs/how-can-i-have-my-money-in-my-account-payout.md |
| Setup / monthly fees | none listed |
| Refund fee (GoPay) | none: "Midtrans does not charge additional fees for the refund process", but the original fee is kept. https://docs.midtrans.com/docs/will-i-be-charged-for-chargebacks-or-refunds.md |

### Xendit: three layers of pricing
**(A) Policy changes, primary.** https://help.xendit.co/hc/en-us/articles/59516240127129 (updated 2026-10-29)
- Payment acceptance and payout fees were **revised across all methods from 1 Aug 2026**. The new per-method figures are on the blocked pricing page.
- **Xendit Processing Fee** from 1 Oct 2026, charged on **every attempt** (payment, payout, refund, including failed attempts). On a refund, the original method fee and processing fee are kept and a new processing fee is charged.
- Chargeback dispute fee: USD 25 for cards and USD 15 for local methods (IDR 450,000 / 270,000).
- **Monthly minimum fee USD 50 (IDR 900,000)** if the monthly invoice is under USD 50 or the account is dormant.
- Legacy-API maintenance fee: USD 250/month. Avoid it by building on the new Payments/Payouts API from day one.

**(B) xenPlatform list pricing from 1 Feb 2027, primary.** https://help.xendit.co/hc/en-us/articles/62671958518297 (updated 2026-10-14)
| Item | Fee |
|---|---|
| One-time platform setup | **USD 10,000** per master account. Waived only for *existing* merchants already on the latest xenPlatform version before 1 Feb 2027 |
| Monthly platform fee | **USD 7,500** per master account |
| KYC/KYB | USD 15 per attempt per new sub-account |
| Active sub-account | USD 5/month (at least 1 transaction attempt that month) |
| Cross-jurisdiction | 1.5% of volume for the first 12 months while the master sets up a local entity |

The current (pre-Feb-2027) xenPlatform fees in the archived official page were IDR 25,000 per active sub-account per month, and 0.5% (capped at IDR 10,000) per in-house transfer or platform fee. https://web.archive.org/web/20251103043044/https://www.xendit.co/id/biaya/

**(C) Per-method fees.** "Archived" is the last official snapshot (2025-11-03, before the Aug-2026 revision): https://web.archive.org/web/20251103043044/https://www.xendit.co/id/biaya/. "Reported current" is **[secondary]**: https://teguhcoding.com/blog/xendit-pricing-policy-update-2026 (updated 2026-10-19), plus a user report on Threads.
| Method | Archived official (Nov 2025) | Reported current (Oct 2026) **[secondary]** |
|---|---|---|
| Processing fee (every attempt) | none | **IDR 4,000** |
| QRIS | 0.7% (VAT-incl.) | 0.7% + 4,000 |
| VA (aggregator) | IDR 4,000. Switcher: 2,000 + bank fee | **IDR 9,000** + 4,000 = 13,000 |
| OVO | 1.5% (2.73–3.18% for digital content) | 3% + 4,000 |
| DANA | 1.5% with PIN / 3% without PIN, incl. recurring | 3% + 4,000 |
| ShopeePay | 2% retail (VAT-incl.) | 3% + 4,000 |
| GoPay | not listed on the archived page (supported as a channel) | 3% + 4,000 |
| LinkAja | 1.5% retail | not reported |
| AstraPay / JeniusPay | 1.5% / 2% | not reported |
| Cards | 2.9% + 2,000 (Amex 3.9% + 2,000). Confirmed primary: https://help.xendit.co/hc/en-us/articles/4418966204441 | 2.9% + 6,000 total |
| Alfamart / Indomaret | IDR 5,000 / 5,500 | IDR 9,000 + 4,000 |
| PayLater: Akulaku / Kredivo / Indodana / Atome / UangMe | 1.7% / 2.3% / 2.3% / 5% / 1.8% | not reported |
| Direct debit: BRI / Mandiri / BNI | 1.9% / IDR 4,500 / max(2%, 4,500) | not reported |
| Payouts (bank, VA, e-wallet) | IDR 2,500 | not reported (plus 4,000 processing fee per policy A) |
| Payment Links / Invoices | free | the processing fee applies to Payment Link attempts. https://docs.xendit.co/docs/transaction-fees.md |
| Subscriptions | IDR 2,500 per active plan per month | not reported |

### Worked example: one IDR 50,000 court share
| | Midtrans | Xendit (reported current, **[secondary]**) |
|---|---|---|
| QRIS | IDR 350 (0.7%) | ~IDR 4,350 (+VAT on processing, not public) |
| VA | IDR 4,440 (4,000 + 11%) | ~IDR 13,000+ |
| GoPay | IDR 1,000 | ~IDR 5,500 |

On top of that, xenPlatform from Feb 2027 costs USD 7,500/month (≈ IDR 135M at the help centre's implied rate of IDR 18,000/USD). ReMatch's tickets are small and frequent, so per-attempt fixed fees dominate.

**QRIS MDR rule change:** Bank Indonesia set **0% QRIS MDR for transactions ≤ IDR 100,000 for all merchants** (0.7% above that). Micro merchants (UMi) pay 0% up to IDR 500,000. Effective 1 Oct 2026, per BI press release 28/159/DKom dated 17 Aug 2026. https://www.bi.go.id/id/publikasi/ruang-media/cerita-bi/pages/mdr-qris.aspx (the snippet showing the figures was truncated; figures confirmed by **[secondary]** https://infobanknews.com/bi-bebaskan-tarif-mdr-qris-transaksi-rp100-ribu-mulai-1-oktober-2026). Whether Midtrans or Xendit pass this through to aggregator merchants is **not public**, so ask both. It would make QRIS nearly free for court shares on Midtrans, but not on Xendit, where the IDR 4,000 processing fee still applies.

### SEA coverage
- **Xendit:** pay-in channels for **ID, PH, MY, TH, VN**. https://docs.xendit.co/docs/available-payment-methods.md. Split-rule currencies: IDR, PHP, VND, MYR, THB, SGD, USD. https://docs.xendit.co/apidocs/create-split-rule.md. Payout coverage includes **SG** and others. https://docs.xendit.co/docs/overview-payouts-coverage.md. Each country needs its own local entity or master; there are no cross-border splits from ID. https://docs.xendit.co/docs/transfer-balances.md
- **Midtrans:** Indonesia only. Its docs describe Midtrans as "the leading payment gateway in Indonesia", and Payouts only go to Indonesian banks and e-wallets. https://docs.midtrans.com/llms.txt, https://docs.midtrans.com/docs/disbursement-overview.md. A second provider would be needed for PH/MY/TH/VN/SG.

---

## 4. Refunds, cancellation, holding funds, settlement

### Xendit: per-channel capabilities for Indonesia
Source: https://docs.xendit.co/docs/available-payment-methods.md (updated 2026-09-30)
| Channel | Settlement | Refund | Partial | Refund window (days) | Tokenise / MIT |
|---|---|---|---|---|---|
| QRIS | T+2 BD | ✓ | ✗ | 30 | ✗ |
| VA (BCA) | T+1 BD | ✗ | ✗ | — | ✗ |
| VA (BNI, BRI, Mandiri, BSI, CIMB, Permata…) | instant | ✗ | ✗ | — | ✗ |
| GoPay | T+1 BD | ✓ | ✓ | 45 | ✓ / ✓ |
| OVO | T+2 BD | ✓ | ✓ | 14 | ✓ / ✓ |
| DANA | T+2 BD | ✓ | ✓ | 30 | ✓ / ✓ |
| ShopeePay | T+2 BD | ✓ | ✓ | 365 | ✓ / ✓ |
| LinkAja | T+2 BD | ✓ | ✗ | 30 | ✗ |
| AstraPay | T+2 BD | ✗ | ✗ | — | ✗ |
| Cards | T+5 BD | ✓ | ✓ | 365 | ✓ / ✓ (auth & capture ✓) |
| BRI direct debit | instant | ✓ | ✗ | 120 | ✓ / ✓ |
| Alfamart / Indomaret | T+5 BD | ✗ | ✗ | — | ✗ |
| Akulaku / Atome / Indodana / Kredivo | T+2 / T+2 / T+2 / T+4 BD | ✓ | ✓ | ∞ / ∞ / 30 / 14 | ✗ |

- **QRIS refunds are inconsistent across Xendit's own sources.** The help centre says full or partial refunds are allowed, **the window is 7 days**, a full refund within 24h becomes a VOID and returns the fee, and refunds only work for certain issuers (QRIS paid via Mandiri, for example, can't be refunded). Use a payout for the rest. https://help.xendit.co/hc/en-us/articles/17209624920345. The channel table says 30 days and no partial refunds. Design for the stricter rule.
- **VA can't be refunded.** Use a disbursement back to the payer instead. https://help.xendit.co/hc/en-us/articles/4417382730137
- **E-wallets:** a same-day VOID before 23:50 returns the fee. Later refunds return the fee only on a full refund, and never for DANA. OVO one-time payments can't be refunded. https://help.xendit.co/hc/en-us/articles/360049598671. (The 2026 policy says the processing fee is kept and charged again on refunds.)
- Refunds always go to the original method. https://docs.xendit.co/docs/refund-payment-request.md

### Midtrans
Source: https://docs.midtrans.com/docs/what-payment-method-that-have-refund-feature.md (updated 2026-04-01)
| Channel | Refund | Window |
|---|---|---|
| Cards | full and partial | 6 months. Customer sees it in 7–14 days |
| VA (all banks) | **no** | — |
| GoPay / DANA | full and partial | 45 days after settlement |
| ShopeePay | full and partial | 365 days |
| OVO | **full only** | 7 days (regular merchants) |
| QRIS (GoPay acquirer) | full and partial | on-us 45 days / off-us 7 days |
| QRIS (ShopeePay acquirer) | full and partial | 365 days, but not refundable off-us via BCA/BNI/BRI |
| Alfamart / Indomaret | **no** | — |
| Akulaku / Kredivo | full and partial | 6 months / 14 days |

- Cancel works for cards in CAPTURE status and other methods while PENDING. After SETTLEMENT, refunds are only possible for card, GoPay, QRIS, ShopeePay and Akulaku. https://docs.midtrans.com/docs/can-i-cancel-a-transaction-that-has-been-settled.md
- Card refunds on the aggregator scheme go through the dashboard, are processed the next working day, and are deducted from your next payout. https://docs.midtrans.com/docs/introduction-to-refund.md
- **Settlement:** you can withdraw at least 3 business days after the transaction. Requests before 11:00 arrive the same day. The minimum is IDR 10,000 (IDR 50,000 for auto-withdrawal). https://docs.midtrans.com/docs/how-can-i-have-my-money-in-my-account-payout.md. A per-method T+n table is **not public**.

### Holding funds until the game is played (escrow-like behaviour)
- **Card pre-authorisation:** Xendit supports auth and capture on cards, including partial capture. https://docs.xendit.co/docs/available-payment-methods.md. Midtrans pre-auth holds the funds and **releases them after 7 days if not captured**. https://docs.midtrans.com/reference/card-feature-pre-authorization.md. Neither offers auth/capture on QRIS, VA or e-wallets, which are Indonesia's dominant methods.
- **Xendit pattern:** collect into the master account (or charge on the sub-account with no split), then release with `POST /transfers` (immediate) after the game. On cancellation, refund or pay out from the master. The money sits in Xendit-held balances, not ReMatch's bank account. https://docs.xendit.co/docs/transfer-balances.md
  - For *Managed* sub-accounts, the sub-merchant can withdraw their own balance, so only money still in the master is effectively "held". https://docs.xendit.co/docs/payouts-for-sub-accounts.md
  - A pending split only settles at the channel's settlement time. https://docs.xendit.co/docs/split-payments.md
- **Midtrans pattern:** all money lands in ReMatch's merchant balance, and ReMatch decides when to pay each payee through Payouts. This gives full control, but **ReMatch is plainly holding third-party funds** (see §7).
- **Refunds of split payments (Xendit):** splits are not reversed automatically, so ReMatch must Transfer back from the payee's sub-account. Build this into the cancellation flow.
- **No-refund methods (VA, OTC):** under both providers, cancellations on these must be refunded by payout to a bank account the player provides. Xendit also has a "send refund" UI where the recipient enters their own account (IDR 2,500 archived). https://web.archive.org/web/20251103043044/https://www.xendit.co/id/biaya/

---

## 5. Recurring payments (Venue Passes, club dues, premium)

- **Xendit:** has a Subscriptions API. You create a plan from a Payment Session (`type: SUBSCRIPTION`) or an existing payment token. Xendit schedules the charges, retries failures, and can send a payment link on failure. https://docs.xendit.co/docs/how-subscriptions-work.md. Tokenised "pay with token" works too. https://docs.xendit.co/docs/pay-with-tokens.md. Merchant-initiated-capable channels in Indonesia are **Cards, GoPay, OVO, DANA, ShopeePay and BRI direct debit**. https://docs.xendit.co/docs/available-payment-methods.md. Archived fee: IDR 2,500 per active plan per month, and DANA without PIN (incl. recurring) costs 3% (archived pricing page).
- **Midtrans:** has a Subscription API, but `payment_type` supports **only `credit_card` and `gopay`**. https://docs.midtrans.com/reference/create-subscription.md. Card one-click and recurring need **extra bank approval**, and the first transaction must be 3DS. https://docs.midtrans.com/docs/one-click-two-clicks-and-recurring-transaction.md. GoPay tokenisation is documented. https://docs.midtrans.com/reference/direct-debit-api-gopay-tokenization.md. No subscription fee ("Tidak ada biaya langganan"). https://midtrans.com/pricing
- **Practical note:** most Indonesian players won't save a card. A plan of "recurring via e-wallet token, or a renewal payment link each cycle" covers more users. Xendit supports that with OVO, DANA and ShopeePay tokens; Midtrans only with GoPay.

---

## 6. Developer experience

| | Xendit | Midtrans |
|---|---|---|
| API style | Unified `/payment_requests`, Payment Sessions, `/refunds`, Payouts v3, xenPlatform headers. The legacy API costs USD 250/month from Oct 2026, so build on the new API | Snap (hosted) + Core API (REST). Separate Payouts API |
| Node/TS SDK | Official `xendit-node` in TypeScript (last push 2025-10-01): https://github.com/xendit/xendit-node | Official `midtrans-nodejs-client` in JavaScript (last push 2025-06-03): https://github.com/Midtrans/midtrans-nodejs-client. Listed on https://docs.midtrans.com/docs/midtrans-api-libraries-plugins.md |
| Mobile | Hosted checkout or components. Native SDK status not checked | Native Android/iOS SDKs. No official Flutter/React Native SDK, so use a Snap webview or Core API. https://docs.midtrans.com/docs/does-midtrans-support-flutter-react-native-or-other-hybrid-non-native-mobile-framework.md |
| Sandbox | Test mode with a payment simulator. xenPlatform test mode allows up to 500 sub-accounts. https://help.xendit.co/hc/en-us/articles/4408655256589 | Sandbox environment. https://docs.midtrans.com/docs/testing-payment-on-sandbox.md |
| Webhooks | `x-callback-token` header to verify. Up to 6 retries with exponential backoff. Can arrive duplicated or out of order, so dedupe on `payment_id`. https://docs.xendit.co/docs/handling-webhooks.md. Separate split-payment, refund and account-verification webhooks | HTTP notification with a SHA-512 `signature_key`. Configured in the dashboard. https://docs.midtrans.com/docs/https-notification-webhooks.md |
| Idempotency | `idempotency-key` header on Payouts (same key with a different body returns `DUPLICATE_ERROR`). https://docs.xendit.co/apidocs/create-payout-v3.md. Payment idempotency relies on `reference_id` | `Idempotency-Key` on POSTs. **Keys only live 5 minutes.** Not supported for Permata VA, CIMB Clicks, KlikBCA or Indomaret. https://docs.midtrans.com/reference/api-headers.md |
| Payment links (guests) | Payment Links / Payment Sessions, hosted, can be created for sub-accounts. https://docs.xendit.co/docs/creating-payment-links.md, https://docs.xendit.co/docs/accepting-payments-for-sub-accounts.md | Payment Link via API. https://docs.midtrans.com/docs/payment-link-via-api.md |

"Mark paid in cash" is provider-independent: it's a ledger entry in ReMatch. With Xendit, a cash payment to a Venue just means no Xendit charge. If ReMatch's fee on cash bookings has to be collected, it's a separate invoice or a Transfer from the sub-account.

---

## 7. Compliance and regulation (not legal advice; get a legal opinion)

- **Framework:** PBI 22/23/PBI/2020 on Payment Systems defines PJP (payment service provider) activities. These include account issuance / fund-source administration, payment initiation and/or acquiring, and remittance, licensed by category. https://www.bi.go.id/id/fungsi-utama/sistem-pembayaran/perizinan/default.aspx, https://www.bi.go.id/id/publikasi/peraturan/Pages/PBI_222320.aspx
- **Provider licences:** Xendit was "licensed as Payment Gateway in Indonesia by Bank Indonesia on February 6th, 2020". https://help.xendit.co/hc/en-us/articles/360045244972. Midtrans says it is BI-licensed and a registered PSE (electronic system operator) on its own site. https://midtrans.com. I didn't pull BI's public licensee register.
- **Does ReMatch need its own licence?** **I couldn't resolve this from primary sources.** None of BI's pages I found say whether a marketplace that collects and splits funds needs its own PJP licence or can rely on its licensed PSP's. Risk in practice:
  - *Xendit xenPlatform:* lower risk. Funds sit in Xendit-held, per-payee balances, and Xendit does KYC of the payees and the splitting. Xendit still asks at activation whether you hold payment licences and who is liable for chargebacks.
  - *Midtrans merchant of record plus Payouts:* higher risk. Funds land in ReMatch's balance and ReMatch pays them on to third parties, which can look like holding or remitting funds. Also, Midtrans only allows one account across multiple sites if they're "in the same domain & scope of business". https://docs.midtrans.com/docs/can-i-use-one-midtrans-account-to-manage-multiple-websites-or-businesses.md
- **Tax:**
  - *PPN on provider fees:* 11% effective at Xendit, with a Faktur Pajak issued because Xendit is PKP. https://help.xendit.co/hc/en-us/articles/360025721371, https://docs.xendit.co/docs/value-added-tax-vat.md
  - *PPh 23:* the merchant withholds **2%** from Xendit's service fees and uploads the bukti potong. https://docs.xendit.co/docs/indonesia-withholding-tax.md. Midtrans equivalent: **not public** in the docs I reviewed.
  - *ReMatch's own fee:* PPN on the platform fee, and whether ReMatch has gross revenue (merchant-of-record model) or only commission (xenPlatform split model), matters a lot for tax. Confirm with a tax adviser. This is outside what the provider sources cover.
  - *PMK 37/2025:* PPh 22 at 0.5% collected through marketplaces. It applies to marketplaces **designated by DJP** (Blibli, Shopee, Tokopedia and Lazada so far), with collection from 1 Oct 2026. https://pajak.go.id/en/node/120661, https://www.pajak.go.id/en/node/120146. ReMatch isn't designated. Keep an eye on whether designations widen to service platforms.
- **OJK:** escrow by banks and financial institutions falls under OJK, but I found no primary source covering marketplace escrow. **[secondary]** https://valeed.id/escrow-account-cara-kerja-dan-contohnya-di-indonesia/

---

## 8. Alternatives (one line each)

- **DOKU:** has a **Sub Account** product (Wallet-as-a-Service) with split rules (percent or flat), payouts, and routing through Checkout or Direct API using `profileId` and `split_rule_id`. The split is computed after fees, and activation goes through sales. https://developers.doku.com/wallet-as-a-service/sub-account/sub-account-v2.md, https://docs.doku.com/wallet-as-a-service/sub-account/collect-and-route. **This is the main alternative to xenPlatform, so get a quote.** Pricing: https://www.doku.com/pricing (not reviewed in detail).
- **Stripe:** Indonesian accounts exist and support Connect (Standard, Express, Custom), but the only listed method is **ID Bank Transfer (VA)**. There's no cross-border, no top-ups, no `on_behalf_of` destination charges, and no plugins. https://support.stripe.com/questions/stripe-indonesia-support-for-marketplaces. Not viable without QRIS and e-wallets.
- **Flip for Business:** has Accept Payment (VA, e-wallet, QRIS, cards, retail) and disbursement APIs. Split or marketplace support and pricing were **not found**. https://keuangan.kontan.co.id/news/flip-for-business-solusi-otomasi-transaksi-keuangan-bagi-pelaku-usaha?page=2 **[secondary]**. Strong for cheap payouts, so it could pair with Midtrans.
- **Durianpay:** an aggregator/orchestrator with subaccounts and fee configuration for platform sellers **[secondary]**. https://supergood.ai/api-report-card/durianpay. Not verified in primary docs.
- **iPaymu:** offers "Split Payment" for agents and resellers at IDR 150 per split (min IDR 500). All payees need iPaymu accounts, and the merchant builds the commission logic. https://ipaymu.com/en/split-payment/. Aimed at small merchants. Not evaluated further.

---

## 9. Comparison and recommendation

| Criterion | Xendit (xenPlatform) | Midtrans |
|---|---|---|
| Native split / platform fee | ✅ split rules, ≤5 routes, auto-deducted | ❌ none. ReMatch ledger + Payouts, or a Partner multi-merchant setup with the fee billed separately |
| Individuals as payees (no PT/CV) | ✅ as sub-accounts (KYC + face scan) | ⚠️ register as individual merchants (KTP + NPWP, fewer methods), or be paid out by ReMatch |
| Sub-merchant KYC via API | ✅ | ❌ not documented |
| Onboarding time | 3–5 BD (managed). Possible T+3/T+7 holds for new individuals | not public. Docs follow-up within 3 WD |
| QRIS fee on IDR 50k | ~IDR 4,350 (0.7% + 4,000 processing) **[secondary]** | IDR 350 (possibly 0 under BI's ≤100k rule, if passed through) |
| VA fee | ~IDR 13,000 **[secondary]** (was 4,000) | IDR 4,000 + VAT |
| Fixed platform cost | **USD 7,500/mo + USD 10k setup from Feb 2027** (list) | none |
| Payout fee | IDR 2,500 (archived) + 4,000 processing | IDR 5,000 bank / 1,000–2,500 GoPay |
| Refund fee | processing fee charged again; original fees kept | no extra fee; original fee kept |
| VA / OTC refunds | ❌ (use a payout) | ❌ (manual) |
| QRIS refunds | ✓ full (partial per help centre), 7–30 days, some issuers excluded | ✓ full and partial, 7–45 days on-us, some off-us excluded |
| Hold until game played | Transfers from master (immediate). Card auth/capture | pre-auth cards only (7 days). Otherwise ReMatch holds the balance |
| Recurring | Cards, GoPay, OVO, DANA, ShopeePay, BRI DD. Subscriptions API | Cards (bank approval needed) + GoPay only |
| SEA | ID, PH, MY, TH, VN (+SG payouts), one vendor | Indonesia only |
| Node SDK | TypeScript, official | JavaScript, official |
| Regulatory exposure for ReMatch | lower (funds in per-payee PSP balances) | higher (ReMatch holds third-party funds) |

### Recommendation
1. **Short term: don't sign on list price.** Both deciding facts are 2026 changes: Xendit's per-attempt processing fee, and xenPlatform's Feb-2027 platform fee. Run a 2–3 week RFQ with **Xendit** (ask for the xenPlatform monthly fee to be waived or tiered, and confirm whether the setup waiver applies if activated before 1 Feb 2027), **DOKU** (Sub Account pricing) and **Midtrans** (Partner/multi-merchant + Payouts pricing, and whether the BI 0% QRIS MDR ≤ IDR 100k is passed through).
2. **If Xendit's platform fee is negotiated down to something early-stage-viable, choose Xendit xenPlatform.** It is the only option that covers ReMatch's model natively: individual coaches and hosts as KYC'd sub-accounts, automatic ReMatch fee, Transfers for "release after play", Payment Links for guests, e-wallet recurring for Passes, and SEA later with the same vendor.
3. **If not, launch Indonesia on Midtrans as merchant of record plus Midtrans Payouts** (or Flip for payouts). This is the lowest per-transaction cost and works well for small QRIS and e-wallet tickets. It needs (a) a legal opinion on holding and paying out third-party funds, (b) ReMatch-run payee KYC and bank-account verification, and (c) a ledger that ReMatch owns. Plan to move to a split-capable provider (Xendit or DOKU) before multi-country expansion.
4. **Either way:** build an internal ledger (Participant shares, host-owes, cash marks, ReMatch fee, refunds) and a thin provider adapter, so the provider is replaceable. Prefer QRIS and e-wallets in checkout (refundable, cheap). Avoid VA and OTC for anything that might be cancelled (not refundable).

### Open questions for sales calls
- Xendit: the live Indonesia per-method fees after 1 Aug 2026; whether VAT applies to the processing fee; the xenPlatform 2027 fee waiver; Owned-sub-account enablement for Indonesia; whether the platform can block Managed sub-account withdrawals until release.
- Midtrans: an API for onboarding merchants under a Partner account; any split or marketplace roadmap; settlement T+n per method; the Payouts price (the pricing page has conflicting GoPay figures).
- Both: whether the QRIS ≤ IDR 100k 0% MDR is passed through, and contract terms on chargeback liability for sub-merchants.
