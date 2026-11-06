# ReMatch owns the payments ledger; the Provider is swappable

Payments is a context of its own, holding ReMatch's ledger of Charges, Shares, Debts, Cash marks, Platform fees, Refunds and Payouts. The Provider (Midtrans, Xendit or DOKU) sits behind a thin adapter and only moves money. The provider is not chosen yet: get quotes first (see `docs/research/payments-provider.md`).

## Considered Options

- **Xendit xenPlatform:** splits one payment to several payees and takes ReMatch's fee automatically, but from 2026 adds Rp4.000 per attempt and, at list price, USD 10,000 setup plus USD 7,500/month from Feb 2027.
- **Midtrans:** far cheaper per payment, but has no splitting, so ReMatch collects everything and pays out itself. Needs a legal opinion on Bank Indonesia licensing.
- **Rely on the Provider's ledger:** rejected because it would lock ReMatch to one Provider and couldn't record Cash marks or Debts between Players.

## Consequences

- Bank transfer (virtual account) and convenience-store payments can't be refunded on either Provider, so prefer QRIS and e-wallets for anything that may be cancelled.
- Holding money until a Game is played is ReMatch's own balance logic, not a Provider feature.
