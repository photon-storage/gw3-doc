---
layout: default
title: Gateway
nav_order: 1
parent: API
has_children: true
permalink: /api/gateway
---

# Gateway API

Gateway3 implements a subset of the IPFS's [path gateway specification](https://specs.ipfs.tech/http-gateways/path-gateway/).
The writable API extension, while not defined by the specification, allows you to upload data, create DAG and pin CIDs.
If you have used [Kubo](https://github.com/ipfs/kubo)'s HTTP gateway API, you will find it familiar.
To accomplish common tasks such as uploading and pinning data, [SDK](/sdk) is the preferred way to interact with Gateway3.
If your choice of programming language is not available yet, the Gateway3 API can be used.

Gateway3 API is a set of HTTP endpoints collectively offered by Gateway3 and participating IPFS nodes.
Gateway3 handles authorization and load balancing through HTTP redirection (i.e. 307 Temporary Redirect).
Most HTTP client should be able to handle the redirection transparently for GET requests.
Other request types need a 2-step process.

## API endpoints

Endpoint: `https://gw3.io`

This section goes through each endpoint and demonstrates the usage using `bash` command.
You can get hands dirty by playing with these bash commands without extra coding.
A few bash tools are required to successfully run the command:

* curl: making HTTP request
* date: current unix timestap
* jq: json parser

These are often preinstalled in common Unix-like systems, although usage may vary slightly in different systems.
The demostration assumes the following variables are defined when necessary:
```bash
# Content CID
CID="QmNtEUdyHzVCbYqtnjKrK27xLg4Vm5NsS3ZHPMJmUjrsMy"

# Unix timestamp in seconds
UNIX_TIMESTAMP=$(date +%s)

# Access key and secret can be acquired through gw3.io portal
GW3_ACCESS_KEY="YOUR ACCESS KEY"
GW3_SECRET_KEY="YOUR ACCESS SECRET KEY"
```
