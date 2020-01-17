# Gsigo Web socketio and cmd Framework

###### Gsigo是一个用Go (Golang)编写的web、socketio、command框架。

###### gsigo 主要基于下面的包进行了封装, 保留了原有包的用法

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
    - [REDIS配置文件](#Redis配置文件)
    - [数据库配置文件](#数据库配置文件)
- [路由规则](#路由规则)
    - [WEB路由规则](#WEB路由规则)
    - [SOCKETIO路由规则](#SOCKETIO路由规则)  
    - [CMD路由规则](#CMD路由规则)  
- [WEB应用](#WEB应用)  
- [SOCKETIO应用](#SOCKETIO应用)
- [CMD应用](#CMD应用)  
- [数据库](#数据库)
- [REDIS](#REDIS)
- [日志](#日志)
- [环境变量](#环境变量)  

## 安装

###### 1. 首先需要安装 [Go](https://golang.org/) (**version 1.10+**), 可以使用下面的命令进行安装 Gsigo.

```sh
$ go get github.com/whf-sky/gsigo
```

###### 2. 导入你的代码

```go
import "github.com/whf-sky/gsigo"
```

如使用go mod包依赖管理工具,请参考下面命令

###### Windows 下开启 GO111MODULE 的命令为：
```sh
$ set GO111MODULE=on
```

###### MacOS 或者 Linux 下开启 GO111MODULE 的命令为：
```sh
$ export GO111MODULE=on
```

###### Windows 下设置 GOPROXY 的命令为：
```sh
$ go env -w GOPROXY=https://goproxy.cn,direct
```

###### MacOS 或 Linux 下设置 GOPROXY 的命令为：
```sh
$ export GOPROXY=https://goproxy.cn
```



## 快速开始

###### 假设文件 main.go 中有如下代码：

```sh
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

###### gsigo 默认是不加载配置文件的，配置文件格式.ini文件

### 应用配置文件

配置文件的使用

```go
package main

func main()  {
    gsigo.Run("./config/.app.ini")
}
```

不加载配置文件的默认参数：

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


不同级别的配置：

###### 当使用环境变量时，当前环境变量会替换调公共环境变量信息，环境变量需自定义

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

App 配置

- app.name

###### 应用名称，默认值`gsigo`。

###### 配置文件中设置

```ini
app.name = "gsigo"
````

###### 代码中调用

```go
gsigo.Config.APP.Name
````

- app.debug

###### 应用debug，默认值`true`。

###### 配置文件中设置

```ini
app.debug = true
````

###### 代码中调用

```go
gsigo.Config.APP.Debug
````


- app.host

###### 应用HOST，默认值`0.0.0.0`

###### 配置文件中设置

```ini
app.host = 0.0.0.0
````

###### 代码中调用

```go
gsigo.Config.APP.Host
````

- app.port

###### 应用PORT，默认值 `8080`。

###### 配置文件中设置

```ini
app.port = "8080"
````

###### 代码中调用

```go
gsigo.Config.APP.Port
````

- app.mode

`default` `gin` `cmd`

###### 应用模式，默认值 `default`(默认：gin+socketio)。


###### 配置文件中设置

```ini
app.mode = 'default'
````

###### 代码中调用

```go
gsigo.Config.APP.Mode
````

SOCKETIO配置

- socket.ping_timeout

###### ping 超时时间，默认值 `60`。

###### 配置文件中设置

```ini
socket.ping_timeout = 60
````

###### 代码中调用

```go
gsigo.Config.Socket.PingTimeout
````


- **socket.ping_interval**

###### ping 时间间隔，默认值 `20`。

###### 配置文件中设置

```ini
socket.ping_interval = 20
````

###### 代码中调用

```go
gsigo.Config.Socket.PingInterval
````

日志配置

- log.hook


`default` `syslog`

###### 日志钩子，默认值 `default`，可自定义钩子。

###### 配置文件中设置

```ini
log.hook = "stdout"
````

###### 代码中调用

```go
gsigo.Config.Log.Hook
````

- log.formatter

`text` `json`

###### 日志输出格式，默认值 `text`。

###### 配置文件中设置

```ini
log.formatter = "text"
````

###### 代码中调用

```go
gsigo.Config.Log.Formatter
````


- log.params

`text` `json`

###### 日志需要的参数，无默认值.

###### 配置文件中设置,syslog例子

```ini
log.params.priority = "LOG_LOCAL0"
log.params.tag = ""
log.params.network = ""
log.params.addr = ""
````

###### 代码中调用

```go
gsigo.Config.Log.params["priority"]
````

### REDIS配置文件

###### 存放路径 项目目录/config/`gsigo.ENV`/redis.ini

```ini
;分组
[redis]

;链接地址
address = 127.0.0.1:6379

;redis密码
password =

;redis库
select = 0

;保持链接时间，单位小时
keep_alive = 10

;连接池，开启链接数量
max_idle = 10

;主
master.address = 127.0.0.1:6379
master.max_idle = 10

;从
slave.max_idle = 10
slave.address[] = 127.0.0.1:6379
slave.address[] = 127.0.0.1:6379
slave.address[] = 127.0.0.1:6379
```

### 数据库配置文件

###### 存放路径 项目目录/config/`gsigo.ENV`/database.ini

```ini
;分组
[english]
;数据库驱动
driver = mysql

;数据库dsn
dsn = root:password@tcp(host:port)/database?charset=utf8&parseTime=True&loc=Local

;打开到数据库的最大连接数。
max_open =  20

;空闲连接池中的最大连接数
max_idle = 10

;可重用连接的最大时间
max_lifetime = 1

;主库
master.dsn = root:password@tcp(host:port)/database?charset=utf8&parseTime=True&loc=Local
master.max_open =  20
master.max_idle = 10

;从库
slave.max_open =  20
slave.max_idle = 10
slave.dsn[] = root:password@tcp(host:port)/database?charset=utf8&parseTime=True&loc=Local
slave.dsn[] = root:password@tcp(host:port)/database?charset=utf8&parseTime=True&loc=Local
slave.dsn[] = root:password@tcp(host:port)/database?charset=utf8&parseTime=True&loc=Local

```

## 路由规则

### WEB路由规则


##### 分组

```go
gsigo.Group(relativePath string, controller ...ControllerInterface) *router
```

###### 示例

```go

package routers

import (
	"github.com/whf-sky/gsigo"
	"test/controllers/web/index"
)

func init()  {
	rootGin := gsigo.Group("/root/")
	{
		rootGin.GET("/", &index.IndexController{})
	}
}
```

##### 使用中间件

```go
gsigo.Use(controller ControllerInterface)
```

##### 静态文件路由规则

```go
gsigo.Static(relativePath string, filePath string)
```

##### POST

```go
gsigo.POST(relativePath string, controller ControllerInterface)
```
##### GET

```go
gsigo.GET(relativePath string, controller ControllerInterface)
```

##### DELETE

```go
gsigo.DELETE(relativePath string, controller ControllerInterface)
```

##### PATCH

```go
gsigo.PATCH(relativePath string, controller ControllerInterface)
```
##### PUT

```go
gsigo.PUT(relativePath string, controller ControllerInterface)
```

##### OPTIONS

```go
gsigo.OPTIONS(relativePath string, controller ControllerInterface)
```

##### HEAD

```go
gsigo.HEAD(relativePath string, controller ControllerInterface)
```

##### Any

`GET, POST, PUT, PATCH, HEAD, OPTIONS, DELETE, CONNECT, TRACE`

```go
gsigo.Any(relativePath string, controller ControllerInterface)
```

### SOCKETIO路由规则

###### 示例

```go
package routers

import (
	"github.com/whf-sky/gsigo"
	"test/controllers/sio/chat"
	"test/controllers/sio/root"
)

func init()  {
	rootRouter := gsigo.Nsp("/")
	{
		rootRouter.OnConnect(&root.ConnectEvent{})
		rootRouter.OnDisconnect(&root.DisconnectEvent{})
		rootRouter.OnError(&root.ErrorEvent{})
		rootRouter.OnEvent("notice", &root.NoticeEvent{})
		rootRouter.OnEvent("bye", &root.ByeEvent{})
	}

	chatRouter := gsigo.Nsp("/chat")
	{
		//如需要ack需要按照如下设置，否则不设置
		chatRouter.OnEvent("msg", &chat.MsgEvent{gsigo.Event{Ack: true},})
	}

}
```

##### Nsp 命名空间相当于WEB组

```go
gsigo.Nsp(nsp string, event ...EventInterface) *router
```

##### OnConnect

```go
gsigo.OnConnect(event EventInterface)
```

##### OnEvent

```go
gsigo.OnEvent(eventName string, event EventInterface)
```


##### OnError

```go
gsigo.OnError(event EventInterface)
```

##### OnDisconnect

```go
gsigo.OnDisconnect(event EventInterface)
```

### CMD路由规则

###### 示例

```go

package routers

import (
	"github.com/whf-sky/gsigo"
	"test/controllers/cmd"
)

func init()  {
	gsigo.CmdRouter(&cmd.TestCmd{})
}

```

```go
gsigo.CmdRouter(cmd CmdInterface) *router
```

## WEB应用

###### 遵循RESTFUL设计风格和开发方式

##### 示例

```go
package index

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
```

##### 结构体定义必须内嵌`gsigo.Controller`结构体 

##### 可定义的Action，参考gin

- `Get()`

- `Post()`

- `Delete()`

- `Put()`

- `Head()`

- `Patch()`

- `Options()` 

- `Any()`  包含请求 GET, POST, PUT, PATCH, HEAD, OPTIONS, DELETE, CONNECT, TRACE.

- `Group()` 组

- `Use()` 中间件

- `Prepare() ` 上述方法执行前执行 

- `Finish()` 在执行上述方法之后执行 


##### 可用属性

###### Ctx 用法参考 gin.Context

```go
type Controller struct {
	Ctx  *gin.Context
}
```

##### 可调用方法

###### 获取组名

```go
func (c *Controller) GetGroup() string
```

###### 获取控制器名称

```go
func (c *Controller) GetController() string
```

###### 获取操作名称

```go
func (c *Controller) GetAction() string
```

## SOCKETIO应用

##### 示例

```go
package chat

import (
	"fmt"
	"github.com/whf-sky/gsigo"
)

type MsgEvent struct {
	gsigo.Event
}

func (this *MsgEvent) Execute() {
	this.Conn.Emit("reply", "have "+this.GetMessage())
}
```
##### 结构体定义必须内嵌`gsigo.Event`结构体

##### 可定义的方法

- `Execute` 执行方法

- `Prepare()` 在执行 `Execute` 前执行

- `Finish()` 在执行 `Execute` 后执行

##### 可用属性

###### Ctx 用法参考 gin.Context

```go
type Event struct {
	//是否发送ack
	Ack bool
	//socket 链接用法参考 socketio.Conn
	Conn socketio.Conn
	//事件类型 connect/event/errordisconnect
	EventType string
}
```



##### 可调用方法

###### 绑定业务用户

```go
func (e *Event) SetUser(uid string)
```

###### 获取业务用户

```go
func (e *Event) GetUser() string
```

###### 根据用户获取所有的链接编号,map[Conn.ID()]无意义占位符

```go
func (e *Event) GetCidsByUser(uid string) map[string]int
```

###### 是否ACK消息

```go
func (e *Event) IsAck() bool
```

###### 获取消息

```go
func (e *Event) GetMessage() string
```

###### 设置ACK消息

```go
func (e *Event) SetAckMsg(msg string)
```


###### 获取ACK消息

```go
func (e *Event) GetAckMsg() string
```

###### 获取命名空间

```go
func (e *Event) GetNamespace() string
```

###### 设置错误消息

```go
func (e *Event) SetError(text string)
```

###### 获取错误消息

```go
func (e *Event) GetError() error
```

## CMD应用

## 数据库

## REDIS

## 日志

## 环境变量

###### 环境变量的使用示例

```sh
$ go run main.go -env=develop
````
