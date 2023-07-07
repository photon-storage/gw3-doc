---
layout: default
title: Upload
nav_order: 2
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/upload
---

# Upload Data

```javascript
POST /ipfs/
```

Uploads data to the IPFS network and returns its CID for retrieval.
This is a 2-step process.
It first acquires authorization from Gateway3 by providing data size.
A successful response contains a url, which is used to upload data.
Data is included in the HTTP body of the second POST request.
A CID is returned in the response header `ipfs-hash` if the POST request succeeds.

- **size**
  - Required: Yes
  - Description: Query parameters that represent the size of upload content.
  - Example: `87718`

- **ts**
  - Required: Yes
  - Description: Query parameters that represent the timestamp now
  - Example: `1688644825`

## Example

```bash
# Upload a picture named A88.jpg with a size of 88718
curl -sS -X POST 'https://gw3.io/ipfs/?size=88718&ts=1688709607' \
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET' ｜ jq

# You should receive a response similar to this:
# {
#    "code":200,
#    "msg":"ok",
#    "data":{
#       "url":"https://sample.upload.com/ipfs/?sargs=auth-data&ssig=sign-data"
#    }
# }


curl -X POST -D - 'https://sample.upload.com/ipfs/?sargs=auth-data&ssig=sign-data' \
     -F file=@A88.jpg
# The response header will have something like:
# ipfs-hash: QmPQqenerWGemsMnSAWqT33PLpdVGaivW48JumpUjFuZwt
```
