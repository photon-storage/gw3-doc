---
layout: default
title: Common
nav_order: 1
parent: Account
grand_parent: API
has_children: false
permalink: /api/account/common
---

# Account Stats

```javascript
GET /api/v0/stats
```
Get the usage status for the the account.

Response body

```json
{
    "code": 200,
    "msg": "ok",
    "data": {
        "name": "John Doe",
        "provider": "gw3",
        "email": "example@example.com",
        "email_verified": true,
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

# Plans

```javascript
GET /api/v0/plans
```
List all the available plans.

Response body

```json
{
    "code": 200,
    "msg": "",
    "data": [
        {
            "id": 1,
            "name": "Free",
            "price": "0.00",
            "pinned_count_limit": 100,
            "pinned_bytes_limit": 1073741824,
            "ingress_limit": 5368709120,
            "egress_limit": 5368709120,
            "ipns_limit": 3
        },
        {
            "id": 3,
            "name": "Plan0",
            "price": "$ 9.99",
            "pinned_count_limit": 2000,
            "pinned_bytes_limit": 107374182400,
            "ingress_limit": 21474836480,
            "egress_limit": 21474836480,
            "ipns_limit": 1
        },
    ]
}

```

# Create Access Key

```javascript
POST /api/v0/key
```
Create an access key with given permissions for user. The available permissions are admin, read, write, pin, and unpin. If you give admin permission, that's equivalent to giving all the permissions.

Request body

```json
{
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

# List Access Key

```javascript
GET /api/v0/keys
```
List all your access keys.

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

```javascript
DELETE /api/v0/key
```
Delete the access key by name.

Request body

```json
{
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