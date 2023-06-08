---
layout: default
title: Fetch
nav_order: 1
parent: API
has_children: false
permalink: /api/fetch
---

# Fetch CID

```javascript
GET /ipfs/{cid}[/{path}][?{params}]
```

Downloads data at specified immutable content path.

`cid`: a valid content identifier (CID).

`path`: optional path parameter pointing at a file or a directory under the cid content root.

`params`: optional query parameters that adjust response behavior.
Supported params are: format, filename.

```bash
# Calculate request signature
SIG=$(echo -e -n "GET\n/ipfs/${CID}\nts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
# Send request to Gateway 3 and follow through redirection
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
