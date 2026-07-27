---
name: Browse marketplace listings and offers (Sequence Marketplace)
description: Read collectible details, lowest/highest listings and offers, and currencies from the Sequence Marketplace API.
api: openapi/horizon-blockchain-games-marketplace-openapi-original.json
operations: [GetCollectible, GetCollectionDetail, ListListingsForCollectible, ListOffersForCollectible, GetLowestPriceListingForCollectible, ListCurrencies]
---

# Browse marketplace listings and offers

Use the Sequence **Marketplace** API to read collection and collectible market data. Calls are `POST`
to a per-chain path `https://marketplace-api.sequence.app/<network>/rpc/Marketplace/<Method>`
(e.g. `.../base/rpc/Marketplace/GetCollectible`).

## Auth
- Send header `X-Access-Key: <project access key>`.

## Steps
1. **Currencies** — `POST /rpc/Marketplace/ListCurrencies` to get the currencies a marketplace accepts.
2. **Collection** — `POST /rpc/Marketplace/GetCollectionDetail` for collection-level marketplace config.
3. **Collectible** — `POST /rpc/Marketplace/GetCollectible` for a single token's marketplace detail.
4. **Best price** — `POST /rpc/Marketplace/GetLowestPriceListingForCollectible` and `GetHighestPriceOfferForCollectible` for the current best listing/offer.
5. **Full books** — `POST /rpc/Marketplace/ListListingsForCollectible` and `ListOffersForCollectible`; paginate with the `page` object (`page`, `pageSize`, `page.more`, `after`).

## Rules
- Select the chain via the URL path segment (`/base/`, `/polygon/`, `/arbitrum/`, testnets `/base-sepolia/`, …).
- Prefer the `*PriceListing/*PriceOffer` methods; the `GetCollectibleLowest*/Highest*` and `ListCollectible*` variants are deprecated.
- Handle the webrpc error envelope `{error, code, msg, cause, status}`. See `errors/horizon-blockchain-games-error-codes.yml`.
