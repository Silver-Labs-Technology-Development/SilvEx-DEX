# SilvEx DEX

Wallet-authorized decentralized exchange for the Silver Labs ecosystem.

**Status: planned — scaffold only.** No swap UI, order builder, settlement
contract, or wallet integration exists in this repository yet. SilvEx DEX is
non-custodial: it never holds keys or funds.

## Purpose

Let a user trade assets from their own wallet. The application requests quotes,
builds an order, asks the user's wallet to sign, and submits the signed
transaction. Settlement happens on-chain; the user keeps custody throughout.

## Scope

Planned work, in dependency order:

1. **Wallet connection** — EIP-1193 connection, chain identity check, and
   explicit account-change handling.
2. **Quote client** — request routes and prices from `SilvEx-DEX-Node`, with
   freshness and slippage limits displayed before signing.
3. **Order builder** — deterministic order and swap payloads with token
   approvals, deadlines, and recipient checks.
4. **Signing and submission** — wallet-signed transactions only; no key
   material in the app.
5. **Settlement adapter** — call the reviewed on-chain settlement path and
   report pending, confirmed, reverted, and unknown states honestly.
6. **Swap UI** — token selection, amount entry, price impact, approval state,
   review screen, and transaction history from the connected wallet.
7. **Safety controls** — chain allowlist, token list, simulation or
   preflight checks, and clear warnings for unaudited markets.

## Out of scope

- Custody, account balances, or an operator-held order book.
- Private-key generation, import, or storage.
- Centralized-exchange accounts and fiat on-ramps (that is `SilvEx-CEX`).
- Indexing and routing infrastructure (that is `SilvEx-DEX-Node`).
- Real-value trading before contract audit and legal review.

## Planned components

| Component | Responsibility |
| --- | --- |
| `web/` | Swap interface and review flow |
| `wallet/` | EIP-1193 adapter and chain checks |
| `quotes/` | Quote client with freshness rules |
| `orders/` | Order and approval payload builders |
| `settlement/` | On-chain settlement adapter |
| `config/` | Chain, token, and market allowlists |

The directory names are proposed. They are not created yet.

## Integration points

- [SilvEx-DEX-Node](https://github.com/Silver-Labs-Technology-Development/SilvEx-DEX-Node)
  — quotes, routes, and indexed market data.
- [SilverPay](https://github.com/Silver-Labs-Technology-Development/SilverPay)
  — optional wallet entry point and shared wallet conventions.
- [btcs-l2](https://github.com/Silver-Labs-Technology-Development/btcs-l2)
  — EVM execution and the settlement contracts, once reviewed.
- [SilverForge-Studio](https://github.com/Silver-Labs-Technology-Development/SilverForge-Studio)
  — token metadata and contract links.
- [silver-labs-platform](https://github.com/Silver-Labs-Technology-Development/silver-labs-platform)
  — shared client and schema conventions.

## Local development

There is no runnable code yet. Future development must use a disposable local
BTCS L2 node, test tokens, and a test wallet. No mainnet keys or real-value
assets may be used.

## Safety and release status

This is a development scaffold, not a live exchange. Decentralized trading
still carries contract, approval, slippage, and front-running risk. Before any
real-value use:

- Independent audit of the settlement contracts and order format.
- A reviewed token list and market-creation policy.
- Explicit slippage, deadline, and price-impact limits.
- A tested failure path for rejected signatures, reverts, and stale quotes.

See [`SECURITY.md`](SECURITY.md).

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

## License

No open-source license is granted. See [`LICENSE`](LICENSE). The repository is
public for review and development, not for reuse or redistribution.

## Related repositories

- [SilvEx DEX Node](https://github.com/Silver-Labs-Technology-Development/SilvEx-DEX-Node)
- [SilvEx CEX](https://github.com/Silver-Labs-Technology-Development/SilvEx-CEX)
- [SilverPay](https://github.com/Silver-Labs-Technology-Development/SilverPay)
- [Silver Labs workspace](https://github.com/orgs/Silver-Labs-Technology-Development/repositories)
