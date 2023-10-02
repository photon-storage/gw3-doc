---
layout: default
title: Common
nav_order: 1
parent: Account
grand_parent: API
has_children: false
permalink: /api/account/common.html
---

# Account Stats

```bash
curl -X GET "https://account.gw3.io/api/v0/stats?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Get the usage stats for a given account.

- **ts**
  - Required: Yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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

# List Pin

```bash
curl -X GET "https://account.gw3.io/api/v0/pin?status=pinned&limit=10&start=0&ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```

List all the pins under the account. There are four pin statuses: `pinning`, `pinned`, `failure`, and `unpinning`.

- **ts**
  - Required: Yes
  - Description: A query parameter that represents the current UNIX timestamp.
  - Example: `1688644825`

- **status**
  - Required: No
  - Description: Filter the specific type of pin to list.
  - Example: `pinned`

- **limit**
  - Required: No
  - Description: The maximum number of records returned for pagination. The default is `10`.
  - Example: `5`

- **start**
  - Required: No
  - Description: Skips the first n results for pagination. Default is `0`.
  - Example: `10`

## Response Body

```json
{
    "code": 200,
    "data": [
        {
            "name": "",
            "cid": "bafybeig6ka5mlwkl4subqhaiatalkcleo4jgnr3hqwvpmsqfca27cijp3i",
            "status": "unpinning",
            "size": 623,
            "created_at": 1690821977
        },
        {
            "name": "Unknow CID",
            "cid": "QmNYERzV2LfD2kkfahtfv44ocHzEFK1sLBaE7zdcYT2GAZ",
            "status": "failure",
            "size": 16,
            "created_at": 1690821971
        },
        {
            "name": "996.png",
            "cid": "bafybeiafyvqlazbbbtjnn6how5d6h6l6rxbqc4qgpbmteaiskjrffmyy4a",
            "status": "pinned",
            "size": 711,
            "created_at": 1690821965
        },
        {
            "name": "123.txt",
            "cid": "QmS2zdgve11D6Qg3yCne4pozZPs99TEXDNCNrct4mwSeKf",
            "status": "pinning",
            "size": 4297,
            "created_at": 1690821900
        }
    ],
    "total": 14
}
```

# Get Pin

```bash
curl -X GET "https://account.gw3.io/api/v0/pin/{cid}?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```

Retrieve the pin status of a specific CID.

- **cid**
  - Required: Yes
  - Description: A CID that has been previously pinned by GW3.
  - Example: `bafybeidsanbjcw4nzqh275lfz4zqssztgbz2zpzz76glkz34dbwfhyfnxy`

- **ts**
  - Required: Yes
  - Description: A query parameter representing the current UNIX timestamp.
  - Example: `1688644825`

## Response Body

```json
{
    "code": 200,
    "msg": "ok",
    "data": {
        "name": "my_codes",
        "cid": "bafybeidsanbjcw4nzqh275lfz4zqssztgbz2zpzz76glkz34dbwfhyfnxy",
        "status": "pinned",
        "size": 53695,
        "created_at": 1690978958
    }
}
```


# Update Pin Name

```bash
curl -X POST "https://account.gw3.io/api/v0/pin/rename?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```

To update the name of your pinned CID

- **ts**
  - Required: Yes
  - Description: A query parameter representing the current UNIX timestamp.
  - Example: `1688644825`

## Request body

```json
{
    "cid": "k51qzi5uqu5dlfmf1hd70bi1nqnzdsebu5n1ajtfhmx1blbexpiblpp5knvepe",
    "name": "hello.html"
}
```

## Response Body

```json
{
    "code": 200,
    "msg": "ok",
}
```

# List IPNS

```bash
curl -X GET "https://account.gw3.io/api/v0/ipns?limit=10&start=0&ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```

List all the ipns under the account.

- **ts**
  - Required: Yes
  - Description: A query parameter that represents the current UNIX timestamp.
  - Example: `1688644825`

- **limit**
  - Required: No
  - Description: The maximum number of records returned for pagination. The default is `10`.
  - Example: `5`

- **start**
  - Required: No
  - Description: Skips the first n results for pagination. Default is `0`.
  - Example: `10`

## Response Body

```json
{
    "code":200,
    "data":[
        {
            "name":"k51qzi5uqu5dgvei2o3pp7kkz4a6tyj5hp2fdvl2l32wpk5quy657lzh1yaqe0",
            "value":"QmNYERzV2LfD2kkfahtfv44ocHzEFK1sLBaE7zdcYT2GAZ",
            "alias":"My first IPNS",
            "created_at": 1690217292, // Created Unix timestamp.
            "publish_at": 1692526344  // Last timestamp, the user updated the record's CID
        }
    ],
    "total":1
}
```

# Get IPNS

```bash
curl -X GET "https://account.gw3.io/api/v0/ipns/{name}?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```

Retrieve the IPNS record by name.

- **name**
  - Required: Yes
  - Description: The IPNS record name you've created or imported.
  - Example: `k51qzi5uqu5dj14wlw7kc5xlzqzy4u42og35hhhihi2v8551odsw7hlgwhskz2`

- **ts**
  - Required: Yes
  - Description: A query parameter representing the current UNIX timestamp.
  - Example: `1688644825`

## Response Body

```json
{
    "code": 200,
    "msg": "ok",
    "data": {
        "name": "k51qzi5uqu5dj14wlw7kc5xlzqzy4u42og35hhhihi2v8551odsw7hlgwhskz2",
        "value": "QmVJ4ZUSC9HS7H1P8TrqP5UDGByGraJwwMpKpUKQnYLqV5",
        "alias": "Test1",
        "created_at": 1690217292,
        "publish_at": 1691747064
    }
}
```

# Search IPNS

```bash
curl -X GET "https://account.gw3.io/api/v0/ipns/search?alias={alias}&ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```

Search the IPNS record by alias.

- **alias**
  - Required: Yes
  - Description: The IPNS alias.
  - Example: `first_record`

- **ts**
  - Required: Yes
  - Description: A query parameter representing the current UNIX timestamp.
  - Example: `1688644825`

## Response Body

```json
{
    "code": 200,
    "msg": "ok",
    "data": {
        "name": "k51qzi5uqu5dj14wlw7kc5xlzqzy4u42og35hhhihi2v8551odsw7hlgwhskz2",
        "value": "QmVJ4ZUSC9HS7H1P8TrqP5UDGByGraJwwMpKpUKQnYLqV5",
        "alias": "first_record",
        "created_at": 1690217292,
        "publish_at": 1691747064
    }
}
```

# Update IPNS Alias

```bash
curl -X POST "https://account.gw3.io/api/v0/ipns/alias?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```

To update the alias of your IPNS record

- **ts**
  - Required: Yes
  - Description: A query parameter representing the current UNIX timestamp.
  - Example: `1688644825`

## Request body

```json
{
    "name": "k51qzi5uqu5dlfmf1hd70bi1nqnzdsebu5n1ajtfhmx1blbexpiblpp5knvepe",
    "alias": "icu"
}
```

## Response Body

```json
{
    "code": 200,
    "msg": "ok",
}
```

# Plans

```bash
curl -X GET "https://account.gw3.io/api/v0/plans"
```
List all the available plans.

Response body

```json
{
    "code": 200,
    "msg": "ok",
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

```bash
curl -X POST "https://account.gw3.io/api/v0/key?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Create an access key with the given permissions for a user.
The available permissions are admin, read, write, pin, and unpin.
The admin permission is equivalent to granting all the permissions.

- **ts**
  - Required: Yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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

```bash
curl -X GET "https://account.gw3.io/api/v0/keys?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
List all access keys.

- **ts**
  - Required: Yes
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
curl -X DELETE "https://account.gw3.io/api/v0/key?ts=$(date +%s)" \
   -H "X-Access-Key: YOUR_ACCESS_KEY" \
   -H "X-Access-Secret: YOUR_ACCESS_SECRET"
```
Delete the access key by name.

- **ts**
  - Required: Yes
  - Description: a query parameter that represents the current unix timestamp
  - Example: `1688644825`

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
