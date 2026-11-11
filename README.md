# telegraph-miner-yaml

Public home for the Telegraph miner YAML configs used in Telegraph Hackathon Season II (1 Nov – 1 Dec 2026).

Telegraph's nodes fetch a miner's YAML from a public URL at registration and verify it against an on-chain sha256 commitment — so the YAML must be publicly reachable. This repo is that home. The files are public by design: every registered miner's YAML is readable by anyone on the network.

## Local onchain proposal status

The root `onchain-metric-publicnode.yaml` is a local-only multichain proposal for the worker patch in
`telegraph-hackathon/build/onchain-hybrid`. It is not published or registered. The `upload/` snapshot is
intentionally unchanged. Do not push, upload, or register this draft until Engine input validation,
scored outputs, chain-default policy, and live RPC/archive availability are separately confirmed.

## The miners

| File | Intent served | Upstream (free, keyless) | Why |
|---|---|---|---|
| `onchain-metric-publicnode.yaml` | `ONCHAIN_METRIC_VERIFY_GENERAL` | Local-only worker proposal (Ethereum, Polygon, Arbitrum) | Live intent currently declares address + block and scores balance_gwei; extra params/outputs are not validated or claimed as scored |
| `weather-verify-openmeteo.yaml` | `WEATHER_FORECAST` + `WEATHER_FORECAST_VERIFY` | Open-Meteo (ICON model) | Priority VERIFY Intent (5 missions); `WEATHER_FORECAST` rank-1 scores 0.001 |
| `fx-now-frankfurter.yaml` | `FX_NOW` | Frankfurter (ECB) | Priority Intent (4 missions); learning miner — an `fx-frankfurter` already exists, differentiate before competing |

Registered IDs: 9601 (fx), 9602 (onchain), 9603 (weather) — verified unused before registration.

## Registration flow

See the full runbook in the private project repo: `build/miner-kit/README.md` (validate → pin → `registerMiner` on the Diamond `0x5a2324aA18613FAD4e44bDF0d6c73Ec1f6D87ff8`, Base Sepolia). The on-chain hash must match the **exact bytes** at the URL — GitHub raw serves the committed blob verbatim, so the URL above is stable per commit.

Note: `updateMiner` issues a new registrationId/intentId — anything targeting the old intentId must be updated.

## Integrity

- The root onchain YAML is a local draft; no new commitment/hash is published or registered.
- `upload/onchain-metric-publicnode.yaml` is the previously prepared snapshot and was not changed by this patch.
- weather-verify-openmeteo.yaml sha256: `0x56386e7f23e67ff2d5f92205b82c8b874a9d97063f8ab56ca143ef52aa17f40b` (ACTIVE, registration 4847)
- fx-now-frankfurter.yaml sha256: `0xcf2df08ea7098c32733c0cbeed9bc98eeab2696e0eeccdc4dfad38ee810c13e6` (not yet registered)

Verify any time: `curl -s <raw-url> | sha256sum`. NOTE: GitHub raw caches ~5 min — after pushing YAML changes, confirm the URL serves the new hash before registering/updating.
