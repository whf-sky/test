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

- **app.name**

_应用名称，默认是 gsigo。_

配置文件中设置

```ini
app.name = "gsigo"
````

代码中调用

```go
gsigo.Config.APP.Name
````

- **app.debug**

_应用debug，默认是 true。_

配置文件中设置

```ini
app.debug = true
````

代码中调用

```go
gsigo.Config.APP.Debug
````


- **app.host**

_应用HOST，默认是 0.0.0.0_

配置文件中设置

```ini
app.host = 0.0.0.0
````

代码中调用

```go
gsigo.Config.APP.Host
````

- **app.port**

_应用PORT，默认是 8080。_

配置文件中设置

```ini
app.port = "8080"
````

代码中调用

```go
gsigo.Config.APP.Port
````

- **app.mode**

_应用模式，默认是 default(默认：gin+socketio)。_

`default` `gin` `cmd`

配置文件中设置

```ini
app.mode = 'default'
````

代码中调用

```go
gsigo.Config.APP.Mode
````

**SOCKETIO 配置**

- **socket.ping_timeout**

_，ping 超时时间，默认是 60。_

配置文件中设置

```ini
socket.ping_timeout = 60
````

代码中调用

```go
gsigo.Config.Socket.PingTimeout
````


- **socket.ping_interval**

_，ping 时间间隔，默认是 20。_

配置文件中设置

```ini
socket.ping_interval = 20
````

代码中调用

```go
gsigo.Config.Socket.PingInterval
````

**日志配置**

- **log.hook**

_，日志钩子，默认是 `default`，可自定义钩子。_

`default` `syslog`

配置文件中设置

```ini
log.hook = "stdout"
````

代码中调用

```go
gsigo.Config.Log.Hook
````

- **log.formatter**

_，日志输出格式，默认是 `text`。_

`text` `json`

配置文件中设置

```ini
log.formatter = "text"
````

代码中调用

```go
gsigo.Config.Log.Formatter
````


- **log.params**

_，日志需要的参数，无默认值。_

`text` `json`

配置文件中设置,syslog例子

```ini
log.params.priority = "LOG_LOCAL0"
log.params.tag = ""
log.params.network = ""
log.params.addr = ""
````

代码中调用

```go
gsigo.Config.Log.params["priority"]
````


## 环境变量

_环境变量的使用示例_


```sh
$ go run main.go -env=develop
````
