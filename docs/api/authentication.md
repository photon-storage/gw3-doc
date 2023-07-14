---
layout: default
title: Authentication
nav_order: 3
parent: API
has_children: false
permalink: /api/authentication.html
---

# Authentication

Authentication is used to identify the requester.
The identity is the basis for access control, usage tracking, rate limiting and etc.

Gateway3 supports two authentication methods:
- Access headers: this is the most convenient method and fits for most scenarios.
- Request signature: this is considered more secure than the access headers' approach. With signed request, the URL can be shared without leaking your access secret.

## Auth Headers
This is the preferred way to access Gateway3 if you are not sharing your acess URLs.
Simply sending a request with headers `X-Access-Key` and `X-Access-Secret`.

## Request Signature
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

## Example Implementations

- For Golang, you can refer to this [Go SDK](https://github.com/photon-storage/gw3-sdk-go/blob/77dd520560d6d1e7f869214b54fb502ee92d3243/common.go#L16)
- For JavaScript, you can refer to this [JS SDK](https://github.com/photon-storage/gw3-sdk-js/blob/d725f9e3741af24e4a682cfc135cd300117358e3/lib/utils.ts#L5)
