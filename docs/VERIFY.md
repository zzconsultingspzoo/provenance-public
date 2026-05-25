# How to Verify an AIKMOS Provenance Anchor

This document explains how an independent third party can verify that a specific
AIKMOS Customer-Output existed at a claimed point in time.

## What you need

1. The Customer's **ORIGO-Manifest JSON** (the Customer obtained this from app.aikmos.com)
2. `opentimestamps-client` (Python): `pip install opentimestamps-client`
3. `git` and access to this repository

## Three Independent Verification Paths

### Path A — AIKMOS Hash-Chain (live API, instant)

```bash
curl https://app.aikmos.com/v1/origo/verify/<manifest-id>
```

Returns the Merkle-proof showing the manifest is at position N in the chain.

### Path B — GitHub Signed Anchor (this repository, 5-min latency)

Find the anchor commit that includes the manifest:

```bash
git clone https://github.com/zzconsultingspzoo/provenance-public.git
cd provenance-public
# Find the anchor JSON file containing the manifest's position
grep -r "<manifest-position>" .

# Verify the commit is GPG-signed
git log --show-signature <commit-sha>
```

### Path C — Bitcoin Blockchain (1h-6h latency, mathematically strongest)

```bash
ots verify <manifest-id>.ots
```

Returns Bitcoin block height where the anchor was committed.

## Trust Hierarchy

Each path is independent. If at least two paths confirm, the evidence is exceptionally strong.
