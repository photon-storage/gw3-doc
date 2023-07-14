---
layout: default
title: Fetch
nav_order: 1
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/fetch.html
---

# Fetch CID

```javascript
GET /ipfs/{cid}[/{path}][?{params}]
```

Downloads data at the specified immutable content path.

- **cid**
  - Required: yes
  - Description: a valid Content Identifier (CID)
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **path**
  - Required: no
  - Description: a path parameter pointing at a file or a directory under the root CID
  - Example: `/folder/file.txt`

- **ts**
  - Required: yes
  - Description: a query parameter representing the current unix timestamp
  - Example: `1688644825`

- **filename**
  - Required: no
  - Description: a query parameter that sets the name returned in `Content-Disposition` HTTP header
  - Example: `filename=file.txt`

- **format**
  - Required: no
  - Description: a query parameter that controls returned data format.
  - Example: `format=car`

## Example

```bash
curl -sSL -X GET "https://gw3.io/ipfs/QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ?ts=1688698793" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"

# Output:
# "EThe Times 03/Jan/2009 Chancellor on brink of second bailout for banks"
```

# Probe CID

```javascript
HEAD /ipfs/{cid}[/{path}][?{params}]
```
Same as GET, but does not return any payload.

# Fetch IPNS

```javascript
GET /ipns/{name}[/{path}][?{params}]
```

Downloads data at the specified mutable content path.
The `name` is resolved to a CID before serving response from the corresponding `/ipfs/{resolved-cid}[/{path}][?{params}]` content path.

- **name**
  - Required: yes
  - Description: a cryptographic IPNS key hash or a human-readable DNS name with DNSLink set-up.
  - Example: `12D3KooWHWW3BLh5kFo1eDNdJhfznDDJJdtooSZJ42iRX756kYbP`

- **ts**
  - Required: yes
  - Description: a query parameter representing the current unix timestamp
  - Example: `1688644825`

- **path**
  - Required: no
  - Description: a path parameter pointing at a file or a directory under the root CID
  - Example: `/folder/file.txt`

## Example

```bash
curl -sSL -X GET "https://gw3.io/ipns/12D3KooWHWW3BLh5kFo1eDNdJhfznDDJJdtooSZJ42iRX756kYbP?ts=1688698793" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"

# The output is an image.
```
