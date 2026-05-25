# AIKMOS Provenance Public Log

**Cryptographic Merkle-Root anchors for Customer-Output Audit-Trails.**

This repository is the **public verification layer** of the AIKMOS Triple-Anchor Provenance System operated by [ForgeSteer](https://forgesteer.com).

## What this repository contains

Every 5 minutes, the AIKMOS Engine appends a **signed commit** containing the current Merkle-Root of all Customer-Output provenance manifests since the previous anchor.

```
/YYYY/MM/DD/merkle-anchor-HH-MM.json
```

Each anchor JSON contains:
- `merkle_root` — BLAKE3 hash representing all manifests in this window
- `range_from_position` / `range_to_position` — Customer-Call positions covered
- `anchored_at` — UTC ISO 8601 timestamp
- `previous_anchor_sha` — Git commit hash of previous anchor (chain link)
- `bitcoin_ots_proof` — base64-encoded OpenTimestamps proof (Bitcoin-anchored ~1h later)
- `bitcoin_block` — Bitcoin block height (filled after ~6 confirmations)

## Triple-Anchor Architecture

1. **AIKMOS Hash-Chain** (internal, sub-second) — Postgres Merkle-tree at Customer-Call time
2. **GitHub Public Log** (this repo, every 5 min) — Signed commits, third-party-verifiable
3. **Bitcoin Blockchain via OpenTimestamps** (every 1h, ~6h confirmation) — Mathematically tamper-evident

## Verification by Third Parties

Anyone can independently verify a Customer's claim that AIKMOS produced a specific output at a specific time:

```bash
# 1. Customer provides their manifest-id from their ORIGO-Manifest
MANIFEST_ID="<customer-provided>"

# 2. Verify Merkle-proof in this repo
curl https://app.aikmos.com/v1/origo/verify/$MANIFEST_ID

# 3. Verify Bitcoin-anchor via OpenTimestamps
ots verify <manifest>.ots
```

## Trust Model

- **No trust in ForgeSteer required**: All anchors are public + cryptographically chained
- **GPG-Signed commits**: All commits signed by AIKMOS Provenance Service key (fingerprint: `545FA2EBD987ED6BEE7C90BA0E619CA011A46063`)
- **Append-only**: Force-pushes are blocked by branch protection; history is immutable

## Legal Status

These provenance anchors are **indicative evidence** (§ 286 ZPO freie Beweiswürdigung in DE). For qualified electronic timestamps under eIDAS Article 41, contract a Trust Service Provider (D-Trust, Swisscom, etc.). The OpenTimestamps Bitcoin-anchor provides strong cryptographic guarantees but is not equivalent to a qualified eIDAS timestamp.

## EU AI Act Article 50 Compliance

This system supports EU AI Act Article 50 transparency requirements through C2PA metadata embedding in Customer-Outputs and timestamped provenance documentation.

## License

MIT — see [LICENSE](./LICENSE)

## Contact

- Documentation: https://app.aikmos.com/origo
- Issues: contact@forgesteer.com
- Operator: Z&Z CONSULTING sp. z o.o. (DBA ForgeSteer)
