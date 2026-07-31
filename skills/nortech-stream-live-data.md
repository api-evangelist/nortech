---
name: Stream live signal data over MQTT
description: Create a Nortech Data Connection and subscribe to the per-signal MQTT live-data stream.
api: openapi/nortech-openapi-original.json
operations:
  - v1.signalData.mqttLiveData.createConnection
  - v1.signalData.mqttLiveData.listConnections
  - v1.signalData.mqttLiveData.getConnection
---

# Stream live signal data over MQTT

Use this to receive real-time signal values as they are produced.

## Auth
HTTP API: `Authorization: Bearer <JWT>` against `https://api.apps.nor.tech`.
MQTT broker: `mqtts://live.data.apps.nor.tech` — username = your username, password = an API Bearer Token.

## Steps
1. `v1.signalData.mqttLiveData.createConnection` — `POST /api/v1/live-data/connections`, declaring which signals to stream and the data format (`json` or `protobuf`). Returns a `connectionId`.
2. `v1.signalData.mqttLiveData.listConnections` / `v1.signalData.mqttLiveData.getConnection` — verify the connection.
3. Subscribe over MQTTS to the topic `<dataFormat>/workspaces/<workspaceId>/assets/<assetId>/divisions/<divisionId>/units/<unitId>/signals/<signalId>`. Each message is a `DataPoint` (timestamp + bool/double/string value). See `asyncapi/nortech-live-data-asyncapi.yml`.

## Conventions
- A Data Connection must exist before subscribing.
- Single-level wildcards (`+`) are not supported in topic subscriptions.
- Errors on the HTTP side return `{ "status": "<message>" }`.
