---
name: Query a Flow account and its balance
description: Read an account's balance, keys, and contracts from the Flow blockchain via the public Access API.
api: openapi/dapper-labs-flow-access-openapi-original.yml
operations:
  - "GET /accounts/{address}"
  - "GET /accounts/{address}/balance"
  - "GET /accounts/{address}/keys"
---

# Query a Flow account

The Flow Access API is public and unauthenticated — no API key or token is required. Use the mainnet base `https://rest-mainnet.onflow.org/v1` (or testnet `https://rest-testnet.onflow.org/v1`).

## Steps
1. `GET /accounts/{address}` — fetch the account by its Flow address (hex, e.g. `0x1654653399040a61`). Returns `address`, `balance`, `keys`, and deployed `contracts`. Optionally `?expand=keys,contracts`.
2. `GET /accounts/{address}/balance` — fetch just the FLOW balance, optionally at a specific `?block_height=`.
3. `GET /accounts/{address}/keys` — list the account's public keys (index, weight, signing/hashing algorithms, revoked flag).

## Rules
- Addresses differ between mainnet and testnet — do not reuse.
- Balances are returned in the smallest unit; divide as documented for display.
- On a bad address you receive a `400` `{code, message}`; on an unknown address a `404`. See `errors/dapper-labs-problem-types.yml`.
