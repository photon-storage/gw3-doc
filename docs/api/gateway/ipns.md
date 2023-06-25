---
layout: default
title: IPNS
nav_order: 5
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/ipns
---

# Create IPNS Record

```javascript
POST /api/v0/name/create?arg={cid}
```

Create a new IPNS record and bind it to the given `cid`.
An IPNS record name is returned in the response JSON if the creation request succeeds.

```bash
CID="QmVR8ML33bKpJdEcvMR66gkm1Nraf2iWVgQsefPrd3U8og"

SIG=$(echo -e -n "POST\n/api/v0/name/create\narg=${CID}&ts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
curl -sSL -X POST "https://gw3.io/api/v0/name/create?arg=${CID}&ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}" | \
    jq -r ".data.name"
```

# Update IPNS Record

```javascript
POST /api/v0/name/publish?arg={cid}&key={ipns_name}
```

Update an existing IPNS record with `name`, pointing it to the given `cid`.

```bash
NAME="12D3KooWLps3iFBRDtXpgS4yjfek1hntk6zMMkraqYC8bPHjWvDk"
CID="QmUcCD6xUMkwQVsChPRYKJQVtduea9VFJJjzuEFqa92fYm"

SIG=$(echo -e -n "POST\n/api/v0/name/publish\narg=${CID}&key=${NAME}&ts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
curl -sSL -X POST "https://gw3.io/api/v0/name/publish?arg=${CID}&key=${NAME}&ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}"
```
