---
name: Read the latest value of an industrial signal
description: Navigate a Nortech workspace's asset hierarchy and fetch the most recent live data point for one or more signals.
api: openapi/nortech-openapi-original.json
operations:
  - v1.metadata.workspace.listWorkspaces
  - v1.metadata.asset.listWorkspaceAssets
  - v1.metadata.signal.listWorkspaceSignals
  - v1.signalData.liveData.getLatestData
---

# Read the latest value of an industrial signal

Use this to answer "what is the current reading of signal X?" against the Nortech Cloud API.

## Auth
All requests use `Authorization: Bearer <JWT>` against `https://api.apps.nor.tech`. Request a token from support@nortech.ai.

## Steps
1. `v1.metadata.workspace.listWorkspaces` — `GET /api/v1/workspaces`. Pick the target workspace `id`.
2. `v1.metadata.asset.listWorkspaceAssets` — `GET /api/v1/workspaces/{workspace}/assets`. Identify the asset.
3. `v1.metadata.signal.listWorkspaceSignals` — `GET /api/v1/workspaces/{workspace}/signals`. Find the signal `id` you want. Use cursor pagination (`size`, `nextToken`) to page through results.
4. `v1.signalData.liveData.getLatestData` — `POST /api/v1/live-data/points` with the signal id(s) in the body to get the most recent DataPoint(s).

## Conventions
- Pagination is cursor-based: pass `size` (max 100) and the returned `nextToken` for the next page (see `conventions/nortech-conventions.yml`).
- Errors return `{ "status": "<message>" }`; 401 = bad/missing token, 404 = unknown id (see `errors/nortech-problem-types.yml`).
- No idempotency key is supported; these are read operations so retries are safe.
