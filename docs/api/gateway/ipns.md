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

# Import IPNS Record

```javascript
POST /api/v0/name/import

{
      "name":      "k51qzi5uqu5dj14wlw7kc5xlzqzy4u42og35hhhihi2v8551odsw7hlgwhskz2",
      "value":     "QmYHqzKbdrXf3Nunj46BU6MLHSXigzcZsCsVdnv9MFMhzj",
      "secret_key": "-----BEGIN PRIVATE KEY-----\nAC3CAEAwBQYDJ2VwECIDIA2eisrccrvn/3ctIaow2wb3PN6/SM+gPJALY68WYKqg\n-----END PRIVATE KEY-----\n",
      "format":    "pem-pkcs8-cleartext",
      "seq":       100
}
```

Import an IPNS record using a user-side generated private key.

- **ts**
  - Required: Yes
  - Description: A query parameter that represents the current Unix timestamp.
  - Example: `1688644825`

- **name**
  - Required: Yes
  - Description: The imported IPNS record name.
  - Example: `k51qzi5uqu5dj14wlw7kc5xlzqzy4u42og35hhhihi2v8551odsw7hlgwhskz2`

- **value**
  - Required: Yes
  - Description: The setup of the IPNS record's value (CID).
  - Example: `QmRsz7zXvecvwJPaPjwR6WMHFJPbMc63SEJtuXJC4U16VZ`

- **secret_key**
  - Required: Yes
  - Description: The text string of the private key information.
  - Example: `-----BEGIN PRIVATE KEY-----\nAC3CAEAwBQYDJ2VwECIDIA2eisrccrvn/3ctIaow2wb3PN6/SM+gPJALY68WYKqg\n-----END PRIVATE KEY-----\n`

- **format**
  - Required: Yes
  - Description: The format of the secret key.
  - Example: `pem-pkcs8-cleartext` or `libp2p-protobuf-cleartext`

- **seq**
  - Required: No
  - Description: Starting value of the record sequence ID.
  - Example: `10000`

## Example

```bash
curl -sSL -X POST "http://gw3.io/api/v0/name/import?ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET" \
   -d '{
   "name": "YOUR_IPNS_NAME",
   "value": "QmYHqzKbdrXf3Nunj46BU6MLHSXigzcZsCsVdnv9MFMhzj",
   "secret_key": "YOUR_SECRET_TEXT",
   "format": "pem-pkcs8-cleartext",
   "seq": 1
}'
# {
#   "code": 200,
#   "msg": "ok",
# }
```