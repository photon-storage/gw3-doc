---
layout: default
title: API
nav_order: 3
has_children: true
permalink: /api
---

# Gateway 3 API

Gateway 3 offers a read-only API implementing a subset of the IPFS's [path gateway specification](https://specs.ipfs.tech/http-gateways/path-gateway/).
The writable API extension, while not defined by the specification, allows you to upload data, create DAG and pin CIDs.
If you have used [Kubo](https://github.com/ipfs/kubo)'s HTTP gateway API, you will find them familiar.
To accomplish common tasks such as uploading and pinning data, [SDK](/sdk) is the preferred way to interact with Gateway 3.
If your choice of programming language is available yet, the Gateway 3 API can be used.

Gateway 3 API is a set of HTTP endpoints collectively offered by Gateway 3 and participating IPFS nodes.
Gateway 3 handles authorization and load balancing through HTTP redirection (i.e. 307 Temporary Redirect).
Most HTTP client should be able to handle the redirection transparently for GET request.
Other request types need a 2-step process.

## Authentication

Authentication allows server to verify the integrity of the request and identify the sender.
Based on the identity, access control, usage tracking and rate limiting can be enforced.
Gateway 3 uses [HMAC](https://en.wikipedia.org/wiki/HMAC) (Hash Message Authenciation Code) for authentication.
In order to access Gateway 3 programmatically, a pair of access key and a access key is required.

Signature is calculated using the following formula:
```
signature = BASE64( HMAC_SHA256( string_to_sign, access_key_secret) )
```

The `string_to_sign` has 3 components, concatenated together with delimiter newline "\n".
```
string_to_sign = http_method + "\n" +
                 request_path + "\n" +
                 request_params
```

The `http_method` is the request method in upper case, such as `GET`, `POST`, and `PUT`.
The `request_path` is the endpoint path.
For example, `/ipfs/QmNtEUdyHzVCbYqtnjKrK27xLg4Vm5NsS3ZHPMJmUjrsMy` is the path for retrieving a CID content.
The `request_params` is an optional string formed by concatenating parameters.
Parameters should be sorted based on key.
The resulting string is url encoded.
For example, parameter `bar` should appear before `foo`:
```
request_params = "bar=value0&foo=value1"
```

Gateway 3 API expects the signature in `X-Access-Signature` header along with access key in `X-Access-Key` header.

## API endpoints

This section goes through each endpoint and demonstrates the usage using `bash` command.
You can get hands dirty by playing with these bash commands without extra coding.
A few bash tools are required to successfully run the command:

* curl: making HTTP request
* openssl: calculating request signature
* date: current unix timestap
* base64: encoding/decoding Base64 string
* xxd: encoding/decoding hex stirng
* jq: json parser

They are preinstalled in common Unix-like systems.
The demostration assumes the following enviroment variables are defined when necessary:
```bash
# Content CID
CID="QmNtEUdyHzVCbYqtnjKrK27xLg4Vm5NsS3ZHPMJmUjrsMy"

# Unix timestamp in seconds
UNIX_TIMESTAMP=$(date +%s)

# Access key and secret can be acquired through gw3.io portal
GW3_ACCESS_KEY="YOUR ACCESS KEY"
GW3_SECRET_KEY="YOUR ACCESS SECRET KEY"
```


