---
layout: default
title: Partner
nav_order: 2
parent: Account
grand_parent: API
has_children: false
permalink: /api/account/partner.html
---

The Partner API is designed to support developers who want to create sub-accounts programmatically.
As a partner, you are enabled to create user accounts, manage their API keys, and select the their appropriate plans.
You must become a qualified Gateway3 partner to utilize the APIs listed below.

# Create Account

```bash
curl -X POST "https://account.gw3.io/api/v0/partner/user/create?ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Create a new user account.
The UUID is an unique key for each user.
It is used by other APIs.

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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
curl -X GET "https://account.gw3.io/api/v0/partner/user/stats?uuid=jack_001&ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Get usage stats for a given user.

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

- **uuid**
  - Required: yes
  - Description: The UUID used when creating the user.
  - Example: `jack_001`

Response body

```json
{
    "code": 200,
    "msg": "ok",
    "data": {
        "name": "John Doe",
        "provider": "gw3",
        "email": "example@example.com",
        "email_verified": false,
        "plan": "Advanced",
        "next_plan": "Starter",
        "next_bill_at": 1689902000,
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
curl -X POST "https://account.gw3.io/api/v0/partner/user/update-plan?ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Change a user's plan.
The incurred fee is charged directly to the partner's account.

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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
curl -X POST "https://account.gw3.io/api/v0/partner/user/key?ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Create an access key with the given permissions for a user.
The available permissions are admin, read, write, pin, and unpin.
Teh admin permission is equivalent to granting all the permissions.

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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
curl -X GET "https://account.gw3.io/api/v0/partner/user/keys?uuid=jack_001&?ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
List all the access keys for a given user.

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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
curl -X DELETE "https://account.gw3.io/api/v0/partner/user/key?ts=1688644825" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Delete the access key by name for a given user.

- **ts**
  - Required: yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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
