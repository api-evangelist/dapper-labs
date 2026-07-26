---
name: Submit a Cadence transaction and check its result
description: Submit a signed Cadence transaction to Flow and poll for its sealed result.
api: openapi/dapper-labs-flow-access-openapi-original.yml
operations:
  - "GET /blocks"
  - "POST /transactions"
  - "GET /transactions/{id}"
  - "GET /transaction_results/{transaction_id}"
---

# Submit a transaction

Transactions mutate on-chain state and MUST be signed. Build and sign with an SDK (`@onflow/fcl`, `flow-go-sdk`) — the Access API only relays the signed payload.

## Steps
1. `GET /blocks?height=sealed` — take the latest sealed block ID as the transaction's `reference_block_id`.
2. Build the transaction (script, arguments, `proposal_key`, `payer`, `authorizers`) and sign it (payload + envelope signatures) with your SDK.
3. `POST /transactions` — submit the signed transaction. Returns the transaction `id`.
4. `GET /transaction_results/{transaction_id}` — poll until `status` is `Sealed`; inspect `status_code`, `error_message`, and emitted `events`.

## Rules
- Transactions are naturally idempotent by content hash — resubmitting an identical signed transaction references the same on-chain tx; there is NO Idempotency-Key header (see `conventions/dapper-labs-conventions.yml`).
- A stale `reference_block_id` (expired) causes the transaction to be rejected — always use a recent sealed block.
- Non-`Sealed` statuses (Pending, Finalized, Executed) mean keep polling; check `error_message` on failure.
