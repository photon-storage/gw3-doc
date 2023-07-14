---
layout: default
title: Getting Started
nav_order: 2
has_children: false
permalink: /getting-started.html
---

# Getting Started

This section provides a simple example to upload and retrieve data through Gateway3.
A pair of access key and access secret is needed and can be acquired from the Gateway3 portal.
For convenience, set environment variable `GW3_ACCESS_KEY` and `GW3_SECRET_KEY`

## Upload and retrieve data using Bash script

```bash
#!/bin/bash

UNIX_TIMESTAMP=$(date +%s)

## Upload a text string
DATA="EThe Times 03/Jan/2009 Chancellor on brink of second bailout for banks"

URL=$(curl -sS -X POST "https://gw3.io/ipfs/?size=${#DATA}&ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: YOUR_ACCESS_KEY" \
    -H "X-Access-Secret: YOUR_ACCESS_SECRET | \"
    jq -r ".data.url")
# curl header dump has \r at the end of each line.
CID=$(curl -sSi -X POST $URL -H "Content-Type: text/plain" --data "${DATA}" | \
    grep "ipfs-hash:" | \
    cut -d" " -f2 | \
    tr -d "\r")

echo -e "Data uploaded, CID = ${CID}"
echo -e "Retrieving data:"

## Retrieve the uploaded data using its CID.
curl -sSL -X GET "https://gw3.io/ipfs/${CID}?ts=${UNIX_TIMESTAMP}" \
    -H "X-Access-Key: YOUR_ACCESS_KEY" \
    -H "X-Access-Secret: YOUR_ACCESS_SECRET"

echo -e "Done"
```

## Upload and retrieve data using Go

```go
package main

import (
	"fmt"
	"os"

	"github.com/photon-storage/gw3-sdk-go"
)

func main() {
	client, err := gw3.NewClient(
		os.Getenv("GW3_ACCESS_KEY"),
		os.Getenv("GW3_SECRET_KEY"),
	)
	if err != nil {
		panic(err)
	}

	data := "EThe Times 03/Jan/2009 Chancellor on brink of second bailout for banks"

	// Post the data to the IPFS network, receiving a CID as a result
	cid, err := client.Post([]byte(data))
	if err != nil {
		panic(err)
	}
	fmt.Println("Data posted, CID = ", cid)

	// Retrieve the data from the IPFS network using the CID
	got, err := client.Get(cid)
	if err != nil {
		panic(err)
	}
	fmt.Println("Data retrieved: ", string(got))
}
```
