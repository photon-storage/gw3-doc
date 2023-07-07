---
layout: default
title: Pinning
nav_order: 4
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/pinning
---

# Pin a CID

```javascript
POST /api/v0/pin/add?arg={cid}
```

Pinning prevents a CID and its descendents from being garbage collected.
This allows data to persist on the IPFS network.

- **cid**
  - Required: Yes
  - Description: A valid Content Identifier (CID)
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **ts**
  - Required: Yes
  - Description: Query parameters that represent the timestamp now
  - Example: `1688644825`

## Example

```bash
curl -sSL -X POST "https://gw3.io/api/v0/pin/add?arg=QmVR8ML33bKpJdEcvMR66gkm1Nraf2iWVgQsefPrd3U8og&ts=1688644825" \
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'

# {
#   "code": 200,
#   "msg": "ok"
# }
```

# Unpin a CID

```javascript
POST /api/v0/pin/rm?arg={cid}
```

Unpinning does not remove the CID from the IPFS network immediately.
It simply allows the CID to be garbage collected in the next cycle.

- **cid**
  - Required: Yes
  - Description: A valid Content Identifier (CID)
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **ts**
  - Required: Yes
  - Description: Query parameters that represent the timestamp now
  - Example: `1688644825`

## Example

```bash
curl -sSL -X POST "https://gw3.io/api/v0/pin/rm?arg=QmVR8ML33bKpJdEcvMR66gkm1Nraf2iWVgQsefPrd3U8og&ts=1688644825" \
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'

# {
#   "code": 200,
#   "msg": "ok"
# }
```
