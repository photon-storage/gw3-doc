---
layout: default
title: IPNS
nav_order: 5
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/ipns.html
---

# Create IPNS Record

```javascript
POST /api/v0/name/create?arg={cid}
```

Create a new IPNS record and bind it to the given `cid`.
An IPNS record name is returned in the response JSON if the creation request succeeds.

- **arg**
  - Required: yes
  - Description: a valid Content Identifier (CID)
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

## Example

```bash
curl -sSL -X POST "https://gw3.io/api/v0/name/create?arg=QmVR8ML33bKpJdEcvMR66gkm1Nraf2iWVgQsefPrd3U8og&ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"

# {
#   "code": 200,
#   "msg": "ok",
#   "data": {
#     "name": "12D3KooWL4iYQFgUJErG1HHHPMGVrh6rdopcosH6wNyyLcAjnNFn",
#     "value": "QmVR8ML33bKpJdEcvMR66gkm1Nraf2iWVgQsefPrd3U8og"
#   }
# }
```

# Update IPNS Record

```javascript
POST /api/v0/name/publish?arg={cid}&key={ipns_name}
```

Update an existing IPNS record with `name`, pointing it to the given `cid`.

- **key**
  - Required: yes
  - Description: an IPNS record
  - Example: `12D3KooWL4iYQFgUJErG1HHHPMGVrh6rdopcosH6wNyyLcAjnNFn`

- **arg**
  - Required: yes
  - Description: a valid Content Identifier (CID)
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

## Example

```bash
curl -sSL -X POST "https://gw3.io/api/v0/name/publish?arg=Qmc7pB5AgED3fKa2MVxY6PBoVswQACfDDfBtFs1c7XCwpU&key=12D3KooWL4iYQFgUJErG1HHHPMGVrh6rdopcosH6wNyyLcAjnNFn&ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"

# {
#   "code": 200,
#   "msg": "ok",
#   "data": {
#     "name": "12D3KooWL4iYQFgUJErG1HHHPMGVrh6rdopcosH6wNyyLcAjnNFn",
#     "value": "Qmc7pB5AgED3fKa2MVxY6PBoVswQACfDDfBtFs1c7XCwpU"
#   }
# }
```
