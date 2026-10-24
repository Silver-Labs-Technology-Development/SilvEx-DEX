# SilvEx DEX

> **Status: Planned — repository and scope only.**
> This repository contains no swap contracts, pools, router, quote engine, or
> trading interface. No executable swap exists.

## Purpose

SilvEx DEX is the wallet-authorized, non-custodial exchange product boundary
for Silver Labs. The intended model is that users keep control of their keys and
authorize swaps from a wallet; the product does not take custody of user funds.

SilvEx DEX is independent of SilvEx CEX. A centralized order book and an
on-chain swap protocol have different custody, settlement, and risk models.

## Proposed scope

- Selection of a supported chain and contract standard.
- Pool discovery and on-chain state reads.
- Route and quote construction with explicit slippage and failure semantics.
- Wallet authorization and transaction submission through SilverPay.
- Pre-trade checks, transaction status, and recovery guidance.
- Contract and integration tests on a disposable local chain or testnet.

## Explicitly out of scope until reviewed and approved

- Real-value swaps or liquidity incentives.
- Unreviewed contracts or unaudited upgrades.
- Cross-chain transfers or bridges.
- Any claim of guaranteed liquidity, price, or execution.
- Treating a quote as a promise or a simulated transaction as a settlement.

## Integration points

- **SilverPay** — wallet connection, account selection, and signing boundary.
- **SilvEx DEX Node** — indexing, routing, and quote services when justified by
  the selected architecture.
- **BTCS L2** — candidate execution environment for contracts and settlement.
- **BTCS Core** — L1 asset and deposit/withdrawal context where relevant.
- **Silver Scan** — transaction and pool visibility.
- **silver-labs-platform** — shared schemas and read-only client contracts.

## Decisions required before implementation

1. Supported chain(s) and the exact contract standard.
2. AMM, order-book, or hybrid design, with a written rationale.
3. Pool creation, fee, slippage, and MEV/front-running assumptions.
4. Upgradeability, admin-key, and emergency-stop policy.
5. Independent contract review and a reproducible testnet swap.

## First milestone

A reviewed design plus a reproducible swap on a disposable local chain or
public testnet, using test assets and a wallet-authorized signature. No
real-value trading.

## Repository facts

- Local workspace path: `~/Silver-Labs/SilvEx-DEX`
- Default branch: `main`
- Implementation: none
- Tests run: none (no code)
- License: proprietary; all rights reserved (see `LICENSE`); no reuse rights are granted
- Visibility: public planning repository

This repository is labeled planned so that its existence is not mistaken for a
working exchange.

## Repository layout

```text
.
├── README.md
├── LICENSE
├── SECURITY.md
├── .gitignore
└── docs/
    └── ARCHITECTURE.md
```

## Architecture note

[`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) records a candidate component
breakdown and threat model. It is subordinate to the scope and decisions above
and does not authorize implementation.

## Security

See [`SECURITY.md`](SECURITY.md) for the reporting policy and security
boundaries. No implementation, test suite, or independent audit exists yet.

## License

No open-source license is granted. See [`LICENSE`](LICENSE). The repository is
public for review and development, not for reuse or redistribution.
