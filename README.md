<p align="center">
  <a href="https://blockfrost.io" target="_blank" align="center">
    <img src="https://blockfrost.io/images/logo.svg" width="280">
  </a>
  <br />
</p>

# Official Blockfrost SDK Client

[![Build status](https://github.com/fabioDMFerreira/blockfrost-go/actions/workflows/test.yml/badge.svg?branch=staging)](https://github.com/fabioDMFerreira/blockfrost-go/actions/workflows/test.yml)
[![Go report card](https://goreportcard.com/badge/github.com/fabioDMFerreira/blockfrost-go)](https://goreportcard.com/report/github.com/fabioDMFerreira/blockfrost-go)
[![GoDoc](https://godoc.org/github.com/fabioDMFerreira/blockfrost-go?status.svg)](https://godoc.org/github.com/fabioDMFerreira/blockfrost-go)

## Getting started

To use this SDK, you first need to log in to [blockfrost.io](https://blockfrost.io), create your project and retrieve the API token.

<img src="https://i.imgur.com/smY12ro.png">

<br/>

## Installation

`blockfrost-go` can be installed through go get

```console
$ go get https://github.com/fabioDMFerreira/blockfrost-go
```

## Usage

### Cardano API

```go
package main

import (
	"context"
	"fmt"
	"log"

	"github.com/fabioDMFerreira/blockfrost-go"
)

func main() {
	api, err := blockfrost.NewAPIClient(
		blockfrost.APIClientOptions{
            ProjectID: "YOUR_PROJECT_ID_HERE", // Exclude to load from env:BLOCKFROST_PROJECT_ID
        },
	)
	if err != nil {
		log.Fatal(err)
	}

	info, err := api.Info(context.TODO())
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("API Info:\n\tUrl: %s\n\tVersion: %s", info.Url, info.Version)
}
```

### IPFS

```go
package main

import (
	"context"
	"flag"
	"fmt"
	"log"

	"github.com/fabioDMFerreira/blockfrost-go"
)

var (
	fp = flag.String("file", "", "Path to file")
)

func main() {
	flag.Parse()
	// Load project_id from env:BLOCKFROST_IPFS_PROJECT_ID
	ipfs := blockfrost.NewIPFSClient(blockfrost.IPFSClientOptions{})

	ipo, err := ipfs.Add(context.TODO(), *fp)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("IPFS Object: %+v\n", ipo)

	// Pin item to avoid being garbage collected.
	pin, err := ipfs.Pin(context.TODO(), ipo.IPFSHash)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Pin: %+v", pin)
}
```

More examples of usage can be found in [`example`](https://github.com/fabioDMFerreira/blockfrost-go/tree/master/example) folder.

## License

Licensed under the [Apache License 2.0](https://opensource.org/licenses/Apache-2.0), see [`LICENSE`](https://github.com/blockfrost-go/blob/master/LICENSE)
