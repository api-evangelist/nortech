---
name: Query historical signal data
description: Run a synchronous or asynchronous historical-data query over Nortech signals and retrieve the result.
api: openapi/nortech-openapi-original.json
operations:
  - v1.signalData.historicalData.syncRequest
  - v1.signalData.historicalData.asyncRequest
  - v1.signalData.historicalData.getRequest
---

# Query historical signal data

Use this to pull a time window of values for one or more signals.

## Auth
`Authorization: Bearer <JWT>` against `https://api.apps.nor.tech`.

## Steps
1. For a small window, `v1.signalData.historicalData.syncRequest` — `POST /api/v1/historical-data/sync` with the signals and time range; the response returns the data inline.
2. For a large window, `v1.signalData.historicalData.asyncRequest` — `POST /api/v1/historical-data/async`. This returns a `requestId`.
3. Poll `v1.signalData.historicalData.getRequest` — `GET /api/v1/historical-data/{requestId}` until the request is ready, then read the result.

## Conventions
- Prefer async for large/long-range pulls; sync for interactive small queries.
- Errors return `{ "status": "<message>" }` (see `errors/nortech-problem-types.yml`).
- The official Python SDK (`pip install nortech`) wraps these into Pandas/Polars DataFrame fetches — see `packages/nortech-packages.yml`.
