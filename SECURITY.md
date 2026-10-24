# Security policy

## Scope

This repository is a **planned development scaffold** for a non-custodial
exchange. There is no runnable application, settlement contract, or audit.

## Reporting

Do not open a public issue containing exploit details, wallet data, or a
working fund-drain path. Report privately to the Silver Labs maintainers through
the organization account
<https://github.com/Silver-Labs-Technology-Development> and allow time for a
fix before disclosure.

## Security boundaries for future work

- The application never requests, stores, or logs a private key or seed phrase.
- Every signature is preceded by a chain check, quote freshness check, and
  explicit review screen.
- Token approvals are explicit and limited; unlimited approvals require a
  separate warning.
- Settlement contracts require an independent audit before real-value use.
- Unknown transaction state is reported as unknown, never as success.
- Never commit `.env` files, wallet data, or production RPC credentials.

## Known limitations

- No implementation, test suite, contract audit, or legal review exists.
- No live liquidity, routing, or settlement path has been tested.
