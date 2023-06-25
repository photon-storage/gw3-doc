---
layout: default
title: Authentication
nav_order: 1
parent: API
has_children: false
permalink: /api/authentication
---

# Authentication

Authentication allows the network to verify the integrity of the request and identity of the requester.
The identity is used for access control, usage tracking, rate limiting and etc.
Gateway3 uses [HMAC](https://en.wikipedia.org/wiki/HMAC) (Hash Message Authenciation Code) for authentication.
In order to access Gateway3 programmatically, a pair of access key and a access key is required.

In addition to request specific headers, authentication requires a `X-Access-Signature` header along with access key in `X-Access-Key` header.
If signature is missing or mismatched, request is rejected.

Teh signature is calculated using the following formula:
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
A parameter `ts`, containing unix timestamp, is required for each request.
