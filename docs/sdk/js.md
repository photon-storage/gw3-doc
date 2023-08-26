---
layout: default
title: Javascript SDK
nav_order: 2
parent: SDK
has_children: false
permalink: /sdk/js.html
---

# Javascript SDK

Import the IPFS Gateway SDK into your JavaScript project:

```ts
import { Client } from 'gw3-sdk';
```

Use `new Client` to instantiate a new Gateway3 client with the provided access key and access secret.

Here's a simple usage example:

```ts
import { Client } from 'gw3-sdk';

let client = new Client('YOUR-ACCESS-KEY', 'YOUR-ACCESS-SECRET');

// Replace 'YOUR-FILE' with the file you want to upload
let file = 'YOUR-FILE';
let hooks = {
  onProgress: (event) => console.log(`Upload progress: ${event.percent}%`),
  onSuccess: (body) => {
    console.log('Upload successful', body);
    // After the file is uploaded, pin it using its CID.
    client.addPin(body.cid)
      .then(() => console.log(`File with CID ${body.cid} has been pinned successfully.`))
      .catch((error) => console.log(`Error pinning file with CID ${body.cid}:`, error));
  },
  onError: (error) => console.log('Upload error', error),
};

client.uploadFile(file, hooks);
```

#### uploadFile

Uploads the provided data to Gateway3 and returns the corresponding CID.

```ts
async function uploadFile(file: File, hooks?: Hooks);

interface Hooks {
  onProgress?: (event: ProgressEvent) => void;
  onSuccess?: (body: any) => void;
  onError?: (error: Error) => void;
}

interface ProgressEvent {
  percent?: number;
}
```

#### getIpfs

Retrieves data from Gateway3 using the specified CID.

```ts
async function getIpfs(cid: string)
```

#### addPin

Requests Gateway3 to pin the specified CID.

```ts
async function addPin(cid: string)
```

#### removePin

Asks Gateway3 to unpin the specified CID.

```ts
async function removePin(cid: string)
```

#### dagAdd

Adds data to a DAG under the specified root and path.

```ts
async function dagAdd(root: string, path: string, data: Uint8Array)
```

#### dagRm

Removes the data from a DAG at the given path.

```ts
async function dagRm(root: string, path: string)
```

#### dagImport

Imports the contents of CAR files into the DAG.

```ts
async function dagImport(file: File)
```

#### createIpns

Creates a new IPNS record and associates it with the given CID.

```ts
async function createIpns(cid: string)
```

#### updateIpns

Updates an existing IPNS record by its name, pointing it to the specified CID.

```ts
async function updateIpns(name: string, cid: string)
```