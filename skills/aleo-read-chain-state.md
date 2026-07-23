---
name: Read Aleo chain state
description: Query the latest block height and hash, fetch blocks, and inspect transactions on the Aleo network via the public node REST API.
api: openapi/aleo-node-api-openapi.yml
operations: [getLatestHeight, getLatestHash, getBlock, getBlockTransactions, getTransaction, findBlockHashByTransaction]
---

# Read Aleo chain state

Use the public Aleo node REST API to read on-chain state. Base URL:
`https://api.explorer.provable.com/v1/{network}` where `network` is `mainnet` or `testnet`. No authentication is required for reads.

## Steps

1. **Get the chain tip.** Call `getLatestHeight` (`GET /{network}/latest/height`) for the current block height, and `getLatestHash` (`GET /{network}/latest/hash`) for its hash.
2. **Fetch a block.** Call `getBlock` (`GET /{network}/block/{height}`) with a height (integer) or block hash (`ab1...`).
3. **List a block's transactions.** Call `getBlockTransactions` (`GET /{network}/block/{height}/transactions`).
4. **Inspect a transaction.** Call `getTransaction` (`GET /{network}/transaction/{id}`) with a transaction ID (`at1...`).
5. **Locate a transaction's block.** Call `findBlockHashByTransaction` (`GET /{network}/find/blockHash/{transactionId}`) to get the containing block hash, then `getBlock` on it.

## Rules
- Responses are `application/json`. Errors are HTTP status codes (404 = not found), not RFC 9457 (see `errors/aleo-problem-types.yml`).
- GETs are idempotent and unauthenticated; there is no rate-limit header — back off on 429/522.
- Use `testnet` for experimentation.
