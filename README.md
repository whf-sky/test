# Gsigo Web socketio and cmd Framework

Gsigo是一个用Go (Golang)编写的web、socketio、command框架。

gsigo 主要基于下面的包进行了封装, 保留了原有包的用法

https://github.com/gin-gonic/gin

https://github.com/googollee/go-socket.io

https://github.com/sirupsen/logrus

https://github.com/jinzhu/gorm

https://github.com/gomodule/redigo/redis

# 目录

- [安装](#安装)
- [快速开始](#快速开始)
- [配置文件](#配置文件)
    - [应用配置文件](#应用配置文件)
    - [Redis配置文件](#Redis配置文件)
    - [数据库配置文件](#数据库配置文件)
- [环境变量](#环境变量)  
- [路由规则](#路由规则)  
- [web应用](#web应用)  
- [socketio应用](#socketio应用)
- [CMD应用](#CMD应用)  
- [数据库](#数据库)
- [Redis](#Redis)
- [Log](#Log)

## 安装

1. 首先需要安装 [Go](https://golang.org/) (**version 1.10+**), 可以使用下面的命令进行安装 Gsigo.

```sh
$ go get github.com/whf-sky/gsigo
```

2. 导入你的代码

```go
import "github.com/whf-sky/gsigo"
```

**如使用go mod包依赖管理工具**

Windows 下开启 GO111MODULE 的命令为：
```sh
$ set GO111MODULE=on
```

MacOS 或者 Linux 下开启 GO111MODULE 的命令为：
```sh
$ export GO111MODULE=on
```

Windows 下设置 GOPROXY 的命令为：
```sh
$ go env -w GOPROXY=https://goproxy.cn,direct
```

MacOS 或 Linux 下设置 GOPROXY 的命令为：
```sh
$ export GOPROXY=https://goproxy.cn
```



## 快速开始

```sh
# 假设文件 main.go 中有如下代码：
$ cat main.go
```

```go
package main

import (
	"github.com/whf-sky/gsigo"
	"net/http"
)

type IndexController struct {
	gsigo.Controller
}

func (this *IndexController) Get() {
	this.Ctx.String(http.StatusOK, "test")
}

func main()  {
	gsigo.Run()
}

func init() {
	gsigo.GET("/", &IndexController{})
}
```
## 配置文件

gsigo 默认是不加载配置文件的，配置文件格式.ini文件

### 应用配置文件

**配置文件的使用**

```go
gsigo.Run("./config/.app.ini")
```

**不加载配置文件的默认参数：**

```ini
app.name = "gsigo"
app.debug = true
app.host = 0.0.0.0
app.port = "8080"
app.mode = 'default'

socket.ping_timeout = 60
socket.ping_interval = 20

log.hook = "default"
log.formatter = "text"
```


**不同级别的配置：**

当使用环境变量时，当前环境变量会替换调公共环境变量信息，环境变量需自定义

```go
app.name = "gsigo"
app.debug = true
app.host = 0.0.0.0
app.port = "8080"
app.mode = 'default'

socket.ping_timeout = 60
socket.ping_interval = 20

log.hook = "stdout"
log.formatter = "text"
log.params.priority = "LOG_LOCAL0"

[production]
app.name = "test"

[develop]


[testing]
```

**App 配置**

- app.name

应用名称，默认是 beego。通过 bee new 创建的是创建的项目名。




## 环境变量

环境变量的使用示例



```sh
$ go run main.go -env=develop
````
