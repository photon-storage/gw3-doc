---
layout: default
title: Partner
nav_order: 2
parent: Account
grand_parent: API
has_children: false
permalink: /api/account/partner
---

The Partner API is designed to support developers who want to create sub-accounts programmatically. As a partner, you are enabled to create accounts for your users, manage their API keys, and select the appropriate plan for them. Please note that this is not a universal API. You must become a partner with Gateway3 to utilize the APIs listed below.

# Create Account

```bash
curl -X POST https://account.gw3.io/api/v0/partner/user/create\
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'
```
Create a new account for your user. The UUID is the unique key that partners use to identify their users. It will be used to interact with the other APIs like modify that user's status.

Request body

```json
{
    "email": "example@example.com",
    "name": "John Doe",
    "uuid": "123e4567-e89b-12d3-a456-426614174000"
}
```

Response body

```json
{
    "code": 200,
    "msg": "ok"
}
```

# User Stats

```bash
curl -X GET https://account.gw3.io/api/v0/partner/user/stats?uuid={ user_uuid }\
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'
```
Get the usage status for the user by UUID.

Response body

```json
{
    "code": 200,
    "msg": "ok",
    "data": {
        "name": "John Doe",
        "email": "example@example.com",
        "email_verified": false,
        "plan": "v1",
        "pinned_count": 23,
        "pinned_count_limit": 100,
        "pinned_bytes": 374182,
        "pinned_bytes_limit": 1073741824,
        "ingress": 3687091,
        "ingress_limit": 5368709120,
        "egress": 536870,
        "egress_limit": 5368709120,
        "ipns": 1,
        "ipns_limit": 1
    }
}

```

# Update User Plan

```bash
curl -X POST https://account.gw3.io/api/v0/partner/user/update-plan\
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'
```
Change the user's plan under the partner. The difference in fees will be charged directly to the partner's account.

Request body

```json
{
    "uuid": "123e4567-e89b-12d3-a456-426614174000",
    "plan_id": 6
}
```

Response body

```json
{
    "code": 200,
    "msg": "ok"
}
```

# Create User Access Key

```bash
curl -X POST https://account.gw3.io/api/v0/partner/user/key\
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'
```
Create an access key with given permissions for user. The available permissions are admin, read, write, pin, and unpin. If you give admin permission, that's equivalent to giving all the permissions.

Request body

```json
{
    "uuid": "123e4567-e89b-12d3-a456-426614174000",
    "name": "test_key",
    "permissions": ["read", "write", "pin", "unpin"]
}
```

Response body

```json
{
    "code": 200,
    "msg": "ok",
    "data": {
        "access_key": "d2e8c7c1-f2cd-4768-b8cd-2c10ea940f64",
        "access_secret": "LQTT4T5XzNtm9tXzUoLoknsYDif64Zr2W6zwrwFYRD35egaGq+WGkPbderrgr+9bPP9fhozhAwu7Zv6YByVaMXdQklxu4wMb6WUnss4+BIzDlfJx2m2a3EBGCs3PMSwsICp5XwJ/Qe3YdFA8JuA5NwiMQ03GXyjN8fFBYNa0UYA="
    }
}
```

# List User Access Key

```bash
curl -X GET https://account.gw3.io/api/v0/partner/user/keys?uuid={ user_uuid }\
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'
```
List all the access keys for the given user.

Response body

```json
{
    "code": 200,
    "msg": "ok",
    "data": [
        {
            "name": "k3",
            "access_key": "05e8d09b-cc5c-4429-8bb5-2fee6c26b244",
            "permissions": [
                "read"
            ]
        },
        {
            "name": "k4",
            "access_key": "99e12dbb-1824-4885-8917-f1f64d4df8b1",
            "permissions": [
                "admin"
            ]
        }
    ]
}
```

# Delete User Access Key

```bash
curl -X DELETE https://account.gw3.io/api/v0/partner/user/key\
   -H 'X-Access-Key: YOUR_ACCESS_KEY' \
   -H 'X-Access-Secret: YOUR_ACCESS_SECRET'
```
Delete the access key by name for the selected user.

Request body

```json
{
    "uuid": "123e4567-e89b-12d3-a456-426614174000",
    "name": "test_key"
}
```

Response body

```json
{
    "code": 200,
    "msg": "ok"
}
```
