---
layout: default
title: DAG Operation
nav_order: 3
parent: Gateway
grand_parent: API
has_children: false
permalink: /api/gateway/dag_operation
---

A close analogy for the DAG operation is the directory operation in operating system.
You can create and organize files using paths under the root CID.
A DAG operation always requires a CID, which identifies the DAG root.
`QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn` is the CID for an empty DAG root node.

# Add to DAG

```javascript
PUT /ipfs/{cid}[/{path}][?{params}]
```

Adds data to a DAG.
`cid` specifies the DAG root node, under which the `path` is created.
The `path` can be nested directories that lead to a file.
To create a DAG from scratch, use CID constant `QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn`.

Similar to upload request, a CID is returned in the response header `ipfs-hash` if the PUT request succeeds.
The CID represents the root node of the new DAG after the addition.
A follow-up operation can be performed on the new CID to further update the DAG.

```bash
ROOT="QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn"
FILEPATH="${ROOT}/example.txt"
DATA="EThe Times 03/Jan/2009 Chancellor on brink of second bailout for banks"

SIG=$(echo -e -n "PUT\n/ipfs/${FILEPATH}\nsize=${#DATA}&ts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
URL=$(curl -sS -X PUT "https://gw3.io/ipfs/${FILEPATH}?size=${#DATA}&ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}" | \
    jq -r ".data.url")
curl -sSD - -X PUT $URL -H "Content-Type: text/plain" --data "${DATA}" -o /dev/null | \
    grep "ipfs-hash"
```

# Remove from DAG

```javascript
DELETE /ipfs/{cid}[/{path}][?{params}]
```

Remove data from a DAG.
`cid` specifies the DAG root node, from which the `path` is deleted.
A CID is returned in the response header `ipfs-hash` if the PUT request succeeds.
The CID represents the root node of the new DAG after deletion.
A follow-up operation can be performed on the new CID to further update the DAG.

```bash
ROOT="QmUcCD6xUMkwQVsChPRYKJQVtduea9VFJJjzuEFqa92fYm"
FILEPATH="${ROOT}/example.txt"

SIG=$(echo -e -n "DELETE\n/ipfs/${FILEPATH}\nts=${UNIX_TIMESTAMP}" | \
    openssl sha256 -hex -mac HMAC \
    -macopt hexkey:$(echo ${GW3_SECRET_KEY} | base64 -d | xxd -p -c0) | \
    xxd -r -p | base64)
URL=$(curl -sS -X DELETE "https://gw3.io/ipfs/${FILEPATH}?ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: ${GW3_ACCESS_KEY}" \
    -H "X-Access-Signature: ${SIG}" | \
    jq -r ".data.url")
curl -sSD - -X DELETE $URL -o /dev/null | grep "ipfs-hash"
```
