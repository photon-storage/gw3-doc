---
layout: default
title: Fetch
nav_order: 1
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/fetch
---

# Fetch CID

```javascript
GET /ipfs/{cid}[/{path}][?{params}]
```

Downloads data at specified immutable content path.

| Parameter | Required | Description | Example |
| --- | --- | --- | --- |
| cid | Yes | A valid Content Identifier (CID) | QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ |
| path | No | Path parameter pointing at a file or a directory under the cid content root | /folder/file.txt |
| filename | No | Query parameters that sets the name returned in Content-Disposition HTTP header | filename=file.txt |
| format | No | Query parameters that URL-friendly alternative to sending Accept header | format=car |

## Example
```bash
curl -sSL -X GET 'https://gw3.io/ipfs/QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ?ts=1688644825' \
    -H 'X-Access-Key: cfd406b6-edd7-40bc-9aae-13c527195f38' \
    -H 'X-Access-Signature: N/NUIELqc6DPnOlOiLE2iMEjducRMgIXHbwgVPSRipY='

# The output might look something like this:
# "EThe Times 03/Jan/2009 Chancellor on brink of second bailout for banks"
```

## Run Your Case
```bash
UNIX_TIMESTAMP=$(date +%s)
CID="QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ"
GW3_ACCESS_KEY="your access key here"
GW3_SECRET_KEY="your secret here"

# Calculate request signature
SIG=$(echo -e -n "GET\n/ipfs/${CID}\nts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
# Send request to Gateway3 and follow through redirection
curl -sSL -X GET "https://gw3.io/ipfs/${CID}?ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}"
```

A format parameter can be used to retrieve content in trustless form (.car file).
```bash
SIG=$(echo -e -n "GET\n/ipfs/${CID}\nformat=car&ts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
curl -sSL -X GET "https://gw3.io/ipfs/${CID}?ts=${UNIX_TIMESTAMP}&format=car" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}" \
    --output output.car
```

# Probe CID

```javascript
HEAD /ipfs/{cid}[/{path}][?{params}]
```
Same as GET, but does not return any payload.

Implementations are free to limit the scope of IPFS data transfer triggered by HEAD requests to a minimal DAG subset required for producing response headers such as X-Ipfs-Roots, Content-Length and Content-Type.

HTTP client can send HEAD request with `Cache-Control: only-if-cached` to disable IPFS data transfer and inexpensively probe if the gateway has the data cached.
This allows light clients to probe and prioritize gateways which already have the data.

# Fetch IPNS

```javascript
GET /ipns/{name}[/{path}][?{params}]
```

Downloads data at specified mutable content path.

The `name` is resolved to a CID, then serve response behind a `/ipfs/{resolved-cid}[/{path}][?{params}]` content path.

The `name` may refer to a cryptographic IPNS key hash or a human-readable DNS name with DNSLink set-up.

```bash
FILEPATH="en.wikipedia-on-ipfs.org/wiki"

SIG=$(echo -e -n "GET\n/ipns/${FILEPATH}\nts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
curl -sSL -X GET "https://gw3.io/ipns/${FILEPATH}?ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}"
```
