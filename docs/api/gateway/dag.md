---
layout: default
title: DAG Operation
nav_order: 3
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/dag_operation.html
---

A close analogy for the DAG operation is the directory operation in operating system.
You can create and organize files using paths under the root CID.
A DAG operation always requires a CID, which identifies the DAG root.
`QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn` is the CID for an empty DAG root node.

# Add to DAG

```javascript
PUT /ipfs/{cid}[/{path}][?{params}]
```

Adds data to a DAG. Similar to upload request, a CID is returned in the response header `ipfs-hash` if the PUT request succeeds. The CID represents the root node of the new DAG after the addition. A follow-up operation can be performed on the new CID to further update the DAG.

- **cid**
  - Required: Yes
  - Description: `cid` specifies the DAG root node
  - Example: `QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn`

- **path**
  - Required: Yes
  - Description: file path under the root CID
  - Example: `/foo/bar/example.txt`

- **size**
  - Required: Yes
  - Description: a query parameter that represents the size of upload content.
  - Example: `87718`

- **ts**
  - Required: Yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

## Example

```bash
# Upload example text to a DAG root and get the new DAG root.
curl -sS -X PUT "https://gw3.io/ipfs/QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn/example.txt?size=88718&ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET" | jq

# You should receive a response similar to this:
# {
#    "code":200,
#    "msg":"ok",
#    "data":{
#       "url":"https://sample.dag.com/ipfs/?sargs=auth-data&ssig=sign-data"
#    }
# }

curl -X PUT -D - "https://sample.dag.com/ipfs/?sargs=auth-data&ssig=sign-data" \
     -F file=@example.txt
# The response header will have something like:
# ipfs-hash: QmXD3svmCE9KR3kiUSZyZuso5DW3q3hLqBtSqXrFAv22Wc
# location: /ipfs/QmXD3svmCE9KR3kiUSZyZuso5DW3q3hLqBtSqXrFAv22Wc/example.txt
```

# Remove from DAG

```javascript
DELETE /ipfs/{cid}[/{path}][?{params}]
```

Remove data from a DAG. A CID is returned in the response header `ipfs-hash` if the PUT request succeeds.
The CID represents the root node of the new DAG after deletion.A follow-up operation can be performed on the new CID to further update the DAG.

- **cid**
  - Required: Yes
  - Description: `cid` specifies the DAG root node
  - Example: `QmXD3svmCE9KR3kiUSZyZuso5DW3q3hLqBtSqXrFAv22Wc`

- **path**
  - Required: Yes
  - Description: the `path` to be deleted.
  - Example: `/foo/bar/example.txt`

- **ts**
  - Required: Yes
  - Description: a query parameter that represents the current timestamp
  - Example: `1688644825`

## Example

```bash
curl -sS -X DELETE "https://gw3.io/ipfs/QmUcCD6xUMkwQVsChPRYKJQVtduea9VFJJjzuEFqa92fYm/example.txt?ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET" | jq

# You should receive a response similar to this:
# {
#    "code":200,
#    "msg":"ok",
#    "data":{
#       "url":"https://delete.dag.com/ipfs/?sargs=auth-data&ssig=sign-data"
#    }
# }

curl -sSD - -X DELETE "https://delete.dag.com/ipfs/?sargs=auth-data&ssig=sign-data"
# The response header will have something like:
# ipfs-hash: QmXy8XTZxWZu9cpnVN44oK77xHQQGCKevosGzuvz1ZsZSX
```

# Import CAR file as DAG

```javascript
POST /api/v0/dag/import?{params}
```

Import the contents of CAR files.
Multiple files can be uploaded in the form data separated by the given `boundary` mark.

- **size**
  - Required: Yes
  - Description: a query parameter that represents the size of upload content.
  - Example: `87718`

- **boundary**
  - Required: Yes
  - Description: a query parameter that represents the multipart form-data boudnary, can a random string.
  - Example: `A-=?bcd`

- **ts**
  - Required: Yes
  - Description: a query parameter that represents the current timestamp
  - Example: `1688644825`
