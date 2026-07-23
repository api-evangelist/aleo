---
name: Read Aleo program and mapping state
description: Fetch a deployed program's Aleo instructions and read its public on-chain mapping values (e.g. account balances in credits.aleo).
api: openapi/aleo-node-api-openapi.yml
operations: [getProgram, getProgramMappings, getMappingValue]
---

# Read Aleo program and mapping state

Deployed Aleo programs (smart contracts) and their public state are readable via the node REST API. Base URL: `https://api.explorer.provable.com/v1/{network}`.

## Steps

1. **Get the program source.** Call `getProgram` (`GET /{network}/program/{programId}`) with a program ID such as `credits.aleo`. Returns the program's Aleo instructions.
2. **List its mappings.** Call `getProgramMappings` (`GET /{network}/program/{programId}/mappings`) to discover the program's public key/value stores (e.g. `account`).
3. **Read a mapping value.** Call `getMappingValue` (`GET /{network}/program/{programId}/mapping/{mappingName}/{key}`). Example: the public balance of an address is `credits.aleo` / `account` / `aleo1...`. Returns the value, or `null` if the key is unset.

## Rules
- Program IDs end in `.aleo`; mapping keys are typed Aleo literals (addresses `aleo1...`, `u64`, `field`, etc.).
- Only PUBLIC program state lives in mappings; private record state is never exposed by this API (that is the zero-knowledge point).
- Unauthenticated GETs; JSON responses; 404 when the program or key does not exist.
