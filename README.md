# telegraph-miner-yaml

Public home for the Telegraph miner YAML configs used in Telegraph Hackathon Season II (1 Nov – 1 Dec 2026).

Telegraph's nodes fetch a miner's YAML from a public URL at registration and verify it against an on-chain sha256 commitment — so the YAML must be publicly reachable. This repo is that home. The files are public by design: every registered miner's YAML is readable by anyone on the network.

## Onchain miner status

The root `onchain-metric-publicnode.yaml` is the restored active-registration YAML snapshot. Its raw
GitHub SHA-256 is `6dc1d1e7ed9853bb1a5e6b405d82c4fc28afbb7b48005411a4c61de1a0373342`, and its
`base_url` is `https://onchain-hybrid.panchalmaahin.workers.dev`.

The separate `onchain-metric-publicnode-multichain-candidate.yaml` combines the multichain worker
proposal with that Cloudflare URL. It requires explicit `chain` and `metric` inputs and describes
Ethereum, Polygon PoS, and Arbitrum One, plus structured receipt, logs, gas, and total-supply
operations. It does not parse natural-language questions. Its raw SHA-256 is
`59eb4683a790a28d922bee02ad5198bf29d26c088a57966c57fee9f0dc38a0ab`.

Miner 9602's multichain candidate YAML has been updated and published, but registration #4975 has
**not** been changed or verified by this commit. The Anjoman edit page says an update creates a new
registration ID. Engine forwarding of `chain` and operation inputs, and the deployed Cloudflare
Worker revision, still need confirmation before the candidate can be claimed as a working end-to-end
miner. The `upload/` snapshot remains unchanged.

## The miners

| File | Intent served | Upstream (free, keyless) | Why |
|---|---|---|---|
| `onchain-metric-publicnode.yaml` | `ONCHAIN_METRIC_VERIFY_GENERAL` | Restored active-registration YAML snapshot; Cloudflare `onchain-hybrid` URL | Older Hybrid YAML; hash above. It does not declare the explicit chain input required by the current multichain worker |
| `onchain-metric-publicnode-multichain-candidate.yaml` | `ONCHAIN_METRIC_VERIFY_GENERAL` | Cloudflare `onchain-hybrid` URL | Published candidate for Ethereum, Polygon PoS, and Arbitrum One; not yet registered or verified through Engine |
| `weather-verify-openmeteo.yaml` | `WEATHER_FORECAST` + `WEATHER_FORECAST_VERIFY` | Open-Meteo (ICON model) | Priority VERIFY Intent (5 missions); `WEATHER_FORECAST` rank-1 scores 0.001 |
| `fx-now-frankfurter.yaml` | `FX_NOW` | Frankfurter (ECB) | Priority Intent (4 missions); learning miner — an `fx-frankfurter` already exists, differentiate before competing |

Registered IDs: 9601 (fx), 9602 (onchain), 9603 (weather) — verified unused before registration.

## Registration flow

See the full runbook in the private project repo: `build/miner-kit/README.md` (validate → pin → `registerMiner` on the Diamond `0x5a2324aA18613FAD4e44bDF0d6c73Ec1f6D87ff8`, Base Sepolia). The on-chain hash must match the **exact bytes** at the URL — GitHub raw serves the committed blob verbatim, so the URL above is stable per commit.

Note: `updateMiner` issues a new registrationId/intentId — anything targeting the old intentId must be updated.

## Integrity

- The root onchain YAML retains the previously verified active-registration bytes and hash shown above.
- The multichain candidate raw URL is
  `https://raw.githubusercontent.com/MaahinVPanchal/telegraph-miner-yaml/main/onchain-metric-publicnode-multichain-candidate.yaml`;
  its published hash is shown above. This hash has not been submitted to Anjoman in this work.
- `upload/onchain-metric-publicnode.yaml` is the previously prepared snapshot and remains unchanged.
- weather-verify-openmeteo.yaml sha256: `0x56386e7f23e67ff2d5f92205b82c8b874a9d97063f8ab56ca143ef52aa17f40b` (ACTIVE, registration 4847)
- fx-now-frankfurter.yaml sha256: `0xcf2df08ea7098c32733c0cbeed9bc98eeab2696e0eeccdc4dfad38ee810c13e6` (not yet registered)

Verify any time: `curl -s <raw-url> | sha256sum`. NOTE: GitHub raw caches ~5 min — after pushing YAML changes, confirm the URL serves the new hash before registering/updating.
