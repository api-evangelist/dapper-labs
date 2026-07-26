---
name: Execute a read-only Cadence script
description: Run a Cadence script against Flow to read on-chain state without submitting a transaction.
api: openapi/dapper-labs-flow-access-openapi-original.yml
operations:
  - "POST /scripts"
  - "GET /blocks"
---

# Execute a Cadence script (read-only)

Scripts are read-only Cadence executed against a block's state — free, no signing, no gas.

## Steps
1. (Optional) `GET /blocks?height=sealed` — get the latest sealed block to pin the execution height.
2. `POST /scripts` — body carries the base64-encoded Cadence `script` and base64-encoded `arguments[]`. Add `?block_height=sealed` (or a specific height / `?block_id=`) to choose the state. Returns the base64-encoded result value.
3. Decode the returned value using `@onflow/types` (JS) or your SDK's Cadence decoder.

## Rules
- Scripts cannot mutate state — use `POST /transactions` (see the submit-transaction skill) for writes.
- Encode both the script and each argument as base64 per the spec.
- A malformed script or argument returns `400 {code, message}`.
