# gqlclient [![GoDoc](https://godoc.org/github.com/mathieupost/gqlclient?status.png)](http://godoc.org/github.com/mathieupost/gqlclient) [![Build Status](https://travis-ci.org/mathieupost/gqlclient.svg?branch=master)](https://travis-ci.org/mathieupost/gqlclient) [![Go Report Card](https://goreportcard.com/badge/github.com/mathieupost/gqlclient)](https://goreportcard.com/report/github.com/mathieupost/gqlclient)

Low-level GraphQL client for Go.

* Simple, familiar API
* Respects `context.Context` timeouts and cancellation
* Build and execute any kind of GraphQL request
* Use strong Go types for response data
* Use variables and upload files
* Simple error handling

## Installation
Make sure you have a working Go environment. To install gqlclient, simply run:

```
$ go get github.com/mathieupost/gqlclient
```

## Usage

```go
import "context"

// create a client (safe to share across requests)
client := gqlclient.NewClient("https://machinebox.io/graphql")

// make a request
req := gqlclient.NewRequest(`
    query ($key: String!) {
        items (id:$key) {
            field1
            field2
            field3
        }
    }
`)

// set any variables
req.Var("key", "value")

// set header fields
req.Header.Set("Cache-Control", "no-cache")

// define a Context for the request
ctx := context.Background()

// run it and capture the response
var respData ResponseStruct
if err := client.Run(ctx, req, &respData); err != nil {
    log.Fatal(err)
}
```

### File support via multipart form data

By default, the package will send a JSON body. To enable the sending of files, you can opt to
use multipart form data instead using the `UseMultipartForm` option when you create your `Client`:

```
client := gqlclient.NewClient("https://machinebox.io/graphql", gqlclient.UseMultipartForm())
```

For more information, [read the godoc package documentation](http://godoc.org/github.com/mathieupost/gqlclient) or the [blog post](https://blog.machinebox.io/a-graphql-client-library-for-go-5bffd0455878).

## Thanks

Thanks to [Chris Broadfoot](https://github.com/broady) for design help.
