---
layout: default
title: Pinning
nav_order: 4
parent: API
has_children: false
permalink: /api/pinning
---

# POST /api/v0/pin/add?arg={cid}

Pin the given `cid`.
Pinning prevents a CID and its descendents from being garbage collected.
This allows data to persist on the IPFS network.

```bash
CID="QmVR8ML33bKpJdEcvMR66gkm1Nraf2iWVgQsefPrd3U8og"
SIG=$(echo -e -n "POST\n/api/v0/pin/add\narg=${CID}&ts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
curl -sSL -X POST "https://gw3.io/api/v0/pin/add?arg=${CID}&ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}"
```

# POST /api/v0/pin/rm?arg={cid}

Unpin the given `cid`.
Unpinning does not remove the CID from the IPFS network immediately.
It simply allows the CID to be garbage collected in the next cycle.

```bash
CID="QmVR8ML33bKpJdEcvMR66gkm1Nraf2iWVgQsefPrd3U8og"
SIG=$(echo -e -n "POST\n/api/v0/pin/rm\narg=${CID}&ts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
curl -sSL -X POST "https://gw3.io/api/v0/pin/rm?arg=${CID}&ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}"
```
