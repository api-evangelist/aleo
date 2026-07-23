---
name: Broadcast a signed Aleo transaction
description: Submit a snarkVM/Provable-SDK-signed transaction to the Aleo network and confirm its on-chain inclusion.
api: openapi/aleo-node-api-openapi.yml
operations: [broadcastTransaction, findBlockHashByTransaction, getTransaction]
---

# Broadcast a signed Aleo transaction

Aleo transactions are built and signed **client-side** with the Provable SDK
(`@provablehq/sdk`) or the Leo CLI — the private key never leaves the client and
authorization is enforced by zk-proof + signature, not by an API credential. The
node REST API only accepts the already-signed transaction. Base URL:
`https://api.explorer.provable.com/v1/{network}`.

## Steps

1. **Build and sign off-band.** Use the Provable SDK / snarkVM or `leo execute`
   to produce a signed transaction JSON (an execution or deployment). This runs
   the zero-knowledge proof generation locally.
2. **Broadcast.** Call `broadcastTransaction` (`POST /{network}/transaction/broadcast`)
   with the signed transaction JSON as the body. On success it returns the
   transaction ID (`at1...`); a `400` means the transaction failed validation.
3. **Confirm inclusion.** Poll `findBlockHashByTransaction`
   (`GET /{network}/find/blockHash/{transactionId}`) until it returns a block hash,
   then `getTransaction` (`GET /{network}/transaction/{id}`) to read the confirmed
   transaction.

## Rules
- The transaction ID is deterministic; re-broadcasting the same transaction is safe (ledger-level idempotency), though there is no HTTP `Idempotency-Key` header.
- Test on `testnet` first; fund a testnet account via the community faucet (see `sandbox/aleo-sandbox.yml`).
- Never transmit or hardcode a private key to the API — signing is local only.
