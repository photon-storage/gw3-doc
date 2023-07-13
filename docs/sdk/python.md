---
layout: default
title: Python SDK
nav_order: 3
parent: SDK
has_children: false
permalink: /sdk/python
---

# Python SDK

## Installation

To install the Gateway3 SDK, use the following command:

```sh
pip install gw3
```

## Index

- [class GW3Client](#class-gw3client)
  - [def \_\_init\_\_(self, access_key: str, access_secret: str)](#def-__init__)
  - [def get_ipfs(self, hash)](#def-get_ipfs)
  - [def get_ipns(self, name)](#def-get_ipns)
  - [def upload(self, data)](#def-upload)
  - [def dag_add(self, root, path, data)](#def-dag_add)
  - [def dag_rm(self, root, path)](#def-dag_rm)
  - [def pin(self, cid)](#def-pin)
  - [def unpin(self, cid)](#def-unpin)
  - [def create_ipns(self, cid)](#def-create_ipns)
  - [def update_ipns(self, name, cid)](#def-update_ipns)

## class [GW3Client](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L14-L250)

GW3Client represents a Gateway3 client for interacting with the Gateway3.

```python
class GW3Client:
    # contains filtered or unexported fields
```

### def [__init__](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L19-L23)

```python
def GW3Client.__init__(self, access_key: str, access_secret: str)
```

__init__ creates a new GW3Client instance with the provided access key and access secret.

### def [get_ipfs](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L57-L66)

```python
def GW3Client.get_ipfs(self, hash)
```

get_ipfs retrieves data from the IPFS network for the given cid.

### def [get_ipns](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L69-L78)

```python
def GW3Client.get_ipns(self, name)
```

get_ipns retrieves data from the IPFS network for the given IPNS.

### def [upload](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L93-L102)

```python
def GW3Client.upload(self, data)
```

upload uploads the given data to Gateway3 and returns the corresponding IPFS hash.

### def [dag_add](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L117-L128)

```python
def GW3Client.dag_add(self, root, path, data)
```

dag_add adds new data to an existing dag and returns the new dag root.

### def [dag_rm](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L143-L154)

```python
def GW3Client.dag_rm(self, root, path)
```

dag_rm removes a path from an existing dag and returns the new dag root.

### def [pin](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L157-L166)

```python
def GW3Client.pin(self, cid)
```

pin requests Gateway3 to pin the given CID.

### def [unpin](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L169-L178)

```python
def GW3Client.unpin(self, cid)
```

unpin requests Gateway3 to unpin the given CID.

### def [create_ipns](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L181-L192)

```python
def GW3Client.create_ipns(self, cid)
```

create_ipns creates a new IPNS record and binds it to the given CID.

### def [update_ipns](https://github.com/photon-storage/gw3-sdk-python/blob/main/gw3/client.py#L195-L206)

```python
def GW3Client.update_ipns(self, name, cid)
```

update_ipns updates the value for the IPNS record specified by the given name.