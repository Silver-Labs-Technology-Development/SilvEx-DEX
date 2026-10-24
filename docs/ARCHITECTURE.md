# SilvEx DEX architecture

> **Status: candidate design note.** The scope and decisions in
> [`README.md`](../README.md) remain authoritative. This document does not
> authorize implementation.


This document describes the intended non-custodial exchange boundary. It is a
design note, not a description of running software.

## Design principles

1. **The wallet is the signer.** The application never receives a private key
   or seed phrase.
2. **Quotes expire.** Every route carries a timestamp, block reference, and
   slippage bound; stale quotes are rejected, not silently refreshed.
3. **Settlement is on-chain.** The app builds and submits a transaction; the
   reviewed contract enforces the trade.
4. **Approvals are explicit.** Token allowances are shown before signing and
   limited where the standard permits.
5. **Unknown is not success.** A missing receipt or an unreachable indexer is
   reported as unknown, never as a completed swap.
6. **Fail closed.** Wrong chain, changed account, stale quote, or failed
   preflight blocks submission.

## Proposed data flow

```text
user ──► swap UI ──► quote client ──► SilvEx-DEX-Node
             │                              │
             ▼                              ▼
        order builder ◄──────────── route, price, expiry
             │
             ▼
        wallet adapter ──► user signature ──► settlement contract
             │                                   │
             ▼                                   ▼
        status tracker ◄──────────────── chain receipt / logs
```

## Components

### Wallet adapter

Wraps EIP-1193. Checks chain ID before every signature, handles account and
chain changes, and disconnects cleanly when the wallet reports no account.
Never stores keys, signatures, or seed material.

### Quote client

Requests a route from `SilvEx-DEX-Node`, validates the response schema, and
enforces a maximum age and a minimum output after slippage. The UI displays
price impact, route, and fees before review.

### Order builder

Produces a deterministic payload: chain ID, tokens, amounts, minimum output,
recipient, deadline, and nonce. Token approval is a separate transaction with
its own review. The builder never signs.

### Settlement adapter

Submits the signed transaction and tracks it to a receipt. Distinguishes
pending, confirmed, reverted, dropped, and unknown. A reverted swap is reported
as reverted with the fee consequence.

### Configuration

Chain allowlist, token allowlist, market registry, and contract addresses are
versioned configuration. An unknown chain or token is blocked by default.

## Open decisions

- Pool-based automated market maker, signed order book, or both?
- Which settlement contract lives in `btcs-l2` versus this repository?
- Permit-style approvals or standard `approve` plus `transferFrom`?
- How to source token logos and metadata without trusting arbitrary URLs?
- What preflight simulation is available on the local BTCS L2 node?

## Threat model (initial)

| Risk | Impact | Initial control |
| --- | --- | --- |
| Malicious token contract | Loss of approved funds | Allowlist and approval limits |
| Stale quote | Worse execution | Expiry, block reference, slippage bound |
| Wrong chain | Funds sent to wrong network | Chain check before signing |
| Phishing UI | Signature theft | Clear review screen and typed data |
| Front-running | Worse price | Slippage limits and private submission where available |
| Indexer outage | Misleading status | Explicit unknown state and direct receipt check |
| Unaudited settlement | Loss of funds | No real-value use before audit |

## Non-goals

SilvEx DEX does not custody funds, run a centralized ledger, provide fiat
on-ramps, or index the chain. Those boundaries belong to `SilverPay`,
`SilvEx-CEX`, and `SilvEx-DEX-Node`.
