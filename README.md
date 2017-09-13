# client-go [![GoDoc](https://godoc.org/gopkg.in/bblfsh/client-go.v0?status.svg)](https://godoc.org/gopkg.in/bblfsh/client-go.v0) [![Build Status](https://travis-ci.org/bblfsh/client-go.svg?branch=master)](https://travis-ci.org/bblfsh/client-go) [![codecov](https://codecov.io/gh/bblfsh/client-go/branch/master/graph/badge.svg)](https://codecov.io/gh/bblfsh/client-go)

[Babelfish](https://doc.bblf.sh) Go client library provides functionality to both
connect to the Babelfish server to parse code
(obtaining an [UAST](https://doc.bblf.sh/uast/specification.html) as a result)
and to analyse UASTs with the functionality provided by [libuast](https://github.com/bblfsh/libuast).

## Installation

The recommended way to install *client-go* is:

```
go get -u gopkg.in/bblfsh/client-go.v0/...
cd $GOPATH/src/gopkg.in/bblfsh/client-go.v0
make dependencies
```

## Example

This small example illustrates how to retrieve the [UAST](https://doc.bblf.sh/uast/specification.html) from a small Python script.

If you don't have a bblfsh server running you can execute it using the following command:

```sh
docker run --privileged --rm -it -p 9432:9432 --name bblfsh bblfsh/server
```

Please read the [getting started](https://doc.bblf.sh/user/getting-started.html) guide, to learn more about how to use and deploy a bblfsh server.


```go
client, err := bblfsh.NewBblfshClient("0.0.0.0:9432")
if err != nil {
    panic(err)
}

python := "import foo"

res, err := client.NewParseRequest().Content(python).Do()
if err != nil {
    panic(err)
}

fmt.Println(res.UAST)
```

```
Module {
.  Roles: File
.  Children: {
.  .  0: Import {
.  .  .  Roles: ImportDeclaration,Statement
.  .  .  StartPosition: {
.  .  .  .  Offset: 0
.  .  .  .  Line: 1
.  .  .  .  Col: 1
.  .  .  }
.  .  .  Properties: {
.  .  .  .  internalRole: body
.  .  .  }
.  .  .  Children: {
.  .  .  .  0: alias {
.  .  .  .  .  Roles: ImportPath,SimpleIdentifier
.  .  .  .  .  TOKEN "foo"
.  .  .  .  .  Properties: {
.  .  .  .  .  .  asname: <nil>
.  .  .  .  .  .  internalRole: names
.  .  .  .  .  }
.  .  .  .  }
.  .  .  }
.  .  }
.  }
}
```


## License

Apache License 2.0, see [LICENSE](LICENSE)
