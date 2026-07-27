---
name: Fetch token and contract metadata (Sequence Metadata)
description: Retrieve ERC-721/ERC-1155 token metadata and collection/contract info, and search tokens, using the Sequence Metadata API.
api: openapi/horizon-blockchain-games-metadata-openapi-original.json
operations: [GetTokenMetadata, GetTokenMetadataBatch, GetContractInfo, GetContractInfoBatch, SearchTokenMetadata]
---

# Fetch token and contract metadata

Use the Sequence **Metadata** API to resolve NFT/token metadata and collection information. All calls
are `POST` to `https://metadata.sequence.app/rpc/Metadata/<Method>` with a JSON body.

## Auth
- Send header `X-Access-Key: <project access key>`.

## Steps
1. **Single token** — `POST /rpc/Metadata/GetTokenMetadata` with `chainID`, `contractAddress`, and `tokenIDs[]` to get names/images/attributes.
2. **Batch tokens** — `POST /rpc/Metadata/GetTokenMetadataBatch` to resolve many contracts/tokens in one request.
3. **Collection info** — `POST /rpc/Metadata/GetContractInfo` (or `GetContractInfoBatch`) for collection name, symbol, type, and logo.
4. **Search** — `POST /rpc/Metadata/SearchTokenMetadata` to find tokens by name/attributes within a contract.

## Rules
- Always `POST`; body is JSON; auth via `X-Access-Key`.
- `RefreshTokenMetadata`, `EnqueueTokensForRefresh`, and `SearchMetadata` are deprecated — prefer the current methods above.
- Handle the webrpc error envelope `{error, code, msg, cause, status}`; `code 3000` = resource not found. See `errors/horizon-blockchain-games-error-codes.yml`.
