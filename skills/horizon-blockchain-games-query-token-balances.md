---
name: Query wallet token balances and history (Sequence Indexer)
description: Read an account's native and token (ERC-20/721/1155) balances and transaction history across an EVM chain using the Sequence Indexer.
api: openapi/horizon-blockchain-games-indexer-openapi-original.json
operations: [GetTokenBalances, GetTokenBalancesSummary, GetTransactionHistory, GetNativeTokenBalance]
---

# Query wallet token balances and history

Use the Sequence **Indexer** to read on-chain balances and activity for an account. Every call is
an HTTP `POST` to a per-chain host (`https://<chain>-indexer.sequence.app`, e.g.
`https://mainnet-indexer.sequence.app`) at `/rpc/Indexer/<Method>` with a JSON body.

## Auth
- Send header `X-Access-Key: <project access key>` (create one in Sequence Builder at https://sequence.build).
- Server-side callers may use an HTTP Bearer JWT instead.

## Steps
1. **Pick the chain host** — choose the indexer host for the target chain (see the OpenAPI `servers[]`).
2. **Summary balances** — `POST /rpc/Indexer/GetTokenBalancesSummary` with the account address to get a compact per-contract balance summary.
3. **Detailed balances** — `POST /rpc/Indexer/GetTokenBalances` for full token balances; page with the `page` object (`page`, `pageSize`, follow `page.more` / `after`).
4. **Native balance** — `POST /rpc/Indexer/GetNativeTokenBalance` for the chain's native (gas) token balance.
5. **History** — `POST /rpc/Indexer/GetTransactionHistory` with the account (and optional contract filter) to list past transfers; paginate the same way.

## Rules
- Requests are always `POST` even for reads (webrpc convention).
- On non-2xx, parse the webrpc error envelope `{error, code, msg, cause, status}` — `code 2005` = rate limited, `code 1000` = unauthorized (bad/missing access key). See `errors/horizon-blockchain-games-error-codes.yml`.
- `GetEtherBalance` is deprecated; use `GetNativeTokenBalance`.
