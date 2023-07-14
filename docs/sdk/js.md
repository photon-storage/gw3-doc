---
layout: default
title: Javascript SDK
nav_order: 2
parent: SDK
has_children: false
permalink: /sdk/js.html
---

# Javascript SDK

Import the IPFS Gateway SDK in your JavaScript project:

```ts
import { Client } from 'gw3-sdk'
```

Use `new Client` to create a new Gateway3 client with the provided access key and access secret.

Here's a simple usage example:

```ts
import { Client } from 'gw3-sdk';

let client = new Client('YOUR-ACCESS-KEY', 'YOUR-ACCESS-SECRET');

// Replace 'YOUR-FILE' with the file you want to upload
let file = 'YOUR-FILE';
let hooks = {
  onProgress: (event) => console.log(`Upload progress: ${event.percent}%`),
  onSuccess: (body) => {
    console.log('Upload success', body);
    // After file is uploaded, pin the file using its CID.
    client.addPin(body.cid)
      .then(() => console.log(`File with CID ${body.cid} has been pinned successfully.`))
      .catch((error) => console.log(`Error in pinning file with CID ${body.cid}:`, error));
  },
  onError: (error) => console.log('Upload error', error),
};

client.uploadFile(file, hooks);
```

#### uploadFile

Uploads the given data to Gateway3 and returns the corresponding CID.

```ts
async function uploadFile(file: File, hooks?: Hooks)

interface Hooks {
  onProgress?: (event: ProgressEvent) => void
  onSuccess?: (body: any) => void
  onError?: (error: Error) => void
}
interface ProgressEvent {
  percent?: number
}
```

#### getIpfs

Retrieves data from the Gateway3 for the given CID.

```ts
async function getIpfs(cid: string)
```

#### addPin

Requests Gateway3 to pin the given CID.

```ts
async function addPin(cid: string)
```

#### removePin

Requests Gateway3 to unpin the given CID.

```ts
async function removePin(cid: string)
```
