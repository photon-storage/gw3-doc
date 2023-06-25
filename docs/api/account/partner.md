---
layout: default
title: Partner
nav_order: 2
parent: Account
has_children: false
permalink: /api/account/partner
---

The Partner API is designed to support the reseller business model. As a partner, you are enabled to create accounts for your users, manage their API keys, and select appropriate plan for them. Please note that this is not a universal API. You must become a partner of Gateway3 to utilize the APIs listed below.

# Create Account

```javascript
POST /api/v0/partner/create-account
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

```javascript
GET /api/v0/partner/stats?uuid={ user_uuid }
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
        "plan": "v1",
        "pinned_count": 23,
        "pinned_count_limit": 100,
        "pinned_bytes": 374182,
        "pinned_bytes_limit": 1073741824,
        "ingress": 3687091,
        "ingress_limit": 5368709120,
        "egress": 536870,
        "egress_limit": 5368709120
    }
}

```

# Update User Plan

```javascript
POST /api/v0/partner/update-plan
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

```javascript
POST /api/v0/partner/create-key
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

```javascript
GET /api/v0/partner/keys?uuid={ user_uuid }
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

```javascript
POST /api/v0/partner/delete-key
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
