# Gin Web Framework

<img align="right" width="159px" src="https://raw.githubusercontent.com/gin-gonic/logo/master/color.png">

Gin is a web framework written in Go (Golang). It features a martini-like API with much better performance, up to 40 times faster thanks to [httprouter](https://github.com/julienschmidt/httprouter). If you need performance and good productivity, you will love Gin.


## Contents

- [安装](#installation)
- [Prerequisite](#prerequisite)

## 安装

To install Gin package, you need to install Go and set your Go workspace first.

1. The first need [Go](https://golang.org/) installed (**version 1.10+ is required**), then you can use the below Go command to install Gin.

```sh
$ go get -u github.com/gin-gonic/gin
```

2. Import it in your code:

```go
import "github.com/gin-gonic/gin"
```

3. (Optional) Import `net/http`. This is required for example if using constants such as `http.StatusOK`.

```go
import "net/http"
```

