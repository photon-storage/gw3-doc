---
layout: default
title: API
nav_order: 3
has_children: true
permalink: /api.html
---

# Gateway3 APIs

Gateway3 offers two types of APIs: the Gateway API and the Account API.

## Gateway API

The Gateway API implements the IPFS gateway and includes additional services provided by Gateway3.
This API is designed to facilitate interactions related to data storage and retrieval, leveraging the power of the IPFS network while adding value through Gateway3's unique features.

Endpoint: `https://gw3.io`

## Account API

The Account API, on the other hand, supports non-storage related API interactions.
This API is designed to manage user account details, including access key creation, updates, and other account-related operations.
It provides the necessary tools to manage user accounts programmatically, offering flexibility and control over user data.

Endpoint: `https://account.gw3.io`

## Authentication

To access these APIs, you need to log into [Gateway3 dashboard](https://gw3.io) and generate your API access key and secret.
Gateway3 supports two authentication methods.
The authentication allows Gateway3 to verify your identity.
Please refer to the [Authentication](/api/authentication.html) section for details.
