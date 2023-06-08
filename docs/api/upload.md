---
layout: default
title: Upload
nav_order: 2
parent: API
has_children: false
permalink: /api/upload
---

# Upload Data

```javascript
POST /ipfs/
```

Uploads data to the IPFS network and returns its CID for retrieval.
This is a 2-step process.
It first acquires authorization from Gateway3 by providing data size and signature.
A successful response contains a url, which is used to upload data.
Data is included in the HTTP body of the second POST request.
A CID is returned in the response header `ipfs-hash` if the POST request succeeds.

```bash
DATA="EThe Times 03/Jan/2009 Chancellor on brink of second bailout for banks"

SIG=$(echo -e -n "POST\n/ipfs/\nsize=${#DATA}&ts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
URL=$(curl -sS -X POST "https://gw3.io/ipfs/?size=${#DATA}&ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}" | \
    jq -r ".data.url")
curl -sSD - -X POST $URL -H "Content-Type: text/plain" --data "${DATA}" -o /dev/null | \
    grep "ipfs-hash"
```
