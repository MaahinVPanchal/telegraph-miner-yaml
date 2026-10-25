# telegraph-miner-yaml

Public home for the Telegraph miner YAML configs used in Telegraph Hackathon Season II (1 Nov – 1 Dec 2026).

Telegraph's nodes fetch a miner's YAML from a public URL at registration and verify it against an on-chain sha256 commitment — so the YAML must be publicly reachable. This repo is that home. The files are public by design: every registered miner's YAML is readable by anyone on the network.

## The miners

| File | Intent served | Upstream (free, keyless) | Why |
|---|---|---|---|
| `onchain-metric-publicnode.yaml` | `ONCHAIN_METRIC_VERIFY` | PublicNode Ethereum RPC | Priority Intent (4 missions); current rank-1 incumbent scores 0.364 |
| `weather-verify-openmeteo.yaml` | `WEATHER_FORECAST` + `WEATHER_FORECAST_VERIFY` | Open-Meteo (ICON model) | Priority VERIFY Intent (5 missions); `WEATHER_FORECAST` rank-1 scores 0.001 |
| `fx-now-frankfurter.yaml` | `FX_NOW` | Frankfurter (ECB) | Priority Intent (4 missions); learning miner — an `fx-frankfurter` already exists, differentiate before competing |

Registered IDs: 9601 (fx), 9602 (onchain), 9603 (weather) — verified unused before registration.

## Registration flow

See the full runbook in the private project repo: `build/miner-kit/README.md` (validate → pin → `registerMiner` on the Diamond `0x5a2324aA18613FAD4e44bDF0d6c73Ec1f6D87ff8`, Base Sepolia). The on-chain hash must match the **exact bytes** at the URL — GitHub raw serves the committed blob verbatim, so the URL above is stable per commit.

Note: `updateMiner` issues a new registrationId/intentId — anything targeting the old intentId must be updated.

## Integrity

- `onchain-metric-publicnode.yaml` sha256: `0x6114d18c02ee5fa24606dd496acc4b91eb54b6af4dec745d53323f8d75cca39c` (registered commitment)
- Verify any time: `curl -s <raw-url> | sha256sum`
