# Telegraph Go

Golang [Telegraph API](https://telegra.ph/api) SDK.

## Guide

### Installation

```bash
go get github.com/frontware/telegraph-go
```

### Example

```go
package main

import (
	"fmt"
	"github.com/frontware/telegraph-go"
	log "github.com/sirupsen/logrus"
)

func main() {
	client, err := telegraph.NewClient("", nil)
	if err != nil {
		log.Panicln(err)
	}
	account, err := client.CreateAccount("telegraph-go", &telegraph.CreateAccountOption{
		AuthorName: "TelegraphGo",
		AuthorURL:  "https://github.com/kallydev",
	})
	if err != nil {
		log.Panicln(err)
	}
	client.AccessToken = account.AccessToken
	paths, err := client.Upload([]string{
		"public/banner.png",
	})
	if err != nil {
		log.Panicln(err)
	}
	page, err := client.CreatePage("Telegraph-Go Example", []telegraph.Node{
		telegraph.NodeElement{
			Tag: "p",
			Children: []telegraph.Node{
				"hello world",
				telegraph.NodeElement{
					Tag: "img",
					Attrs: map[string]string{
						"src":  paths[0],
						"alt": "Banner",
					},
				},
			},
		},
	}, &telegraph.CreatePageOption{
		ReturnContent: true,
	})
	if err != nil {
		log.Panicln(err)
	}
	fmt.Println(page)
}
```

## License

Copyright (c) KallyDev. All rights reserved.

Licensed under the [MIT](LICENSE) license.
