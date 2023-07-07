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

- **cid** 
  - Required: Yes
  - Description: A valid Content Identifier (CID)
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **ts** 
  - Required: Yes
  - Description: Query parameters that represent the timestamp now
  - Example: `1688644825`
  
- **path**
  - Required: No
  - Description: Path parameter pointing at a file or a directory under the cid content root
  - Example: `/folder/file.txt`

- **filename**
  - Required: No
  - Description: Query parameters that sets the name returned in Content-Disposition HTTP header
  - Example: `filename=file.txt`

- **format**
  - Required: No
  - Description: Query parameters that URL-friendly alternative to sending Accept header 
  - Example: `format=car`

## Example

```bash
curl -sSL -X GET 'https://gw3.io/ipfs/QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ?ts=1688698793' \
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'

# The output might look something like this:
# "EThe Times 03/Jan/2009 Chancellor on brink of second bailout for banks"
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

Downloads data at specified mutable content path. The `name` is resolved to a CID, then serve response behind a `/ipfs/{resolved-cid}[/{path}][?{params}]` content path.

- **name** 
  - Required: Yes
  - Description: a cryptographic IPNS key hash or a human-readable DNS name with DNSLink set-up.
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **ts** 
  - Required: Yes
  - Description: Query parameters that represent the timestamp now
  - Example: `1688644825`

- **path**
  - Required: No
  - Description: Path parameter pointing at a file or a directory under the cid content root
  - Example: `/folder/file.txt`

## Example

```bash
curl -sSL -X GET 'https://gw3.io/ipns/12D3KooWHWW3BLh5kFo1eDNdJhfznDDJJdtooSZJ42iRX756kYbP?ts=1688698793' \
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'

# The output is a image.
```
