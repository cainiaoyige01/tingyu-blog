## 项目部署

项目使用 docker-compose 一键部署运行。

./config/config.docker.toml 是部署时读取的配置文件，其中有些变量会由 docker-compose 中的环境变量覆盖

具体可参考 deploy/start/docker-compose.yml

配置文件读取值的优先级: 环境变量 > config.docker.toml 中的值，因此使用 docker-compose 运行时，修改其中的 environment 即可。

## 开发规范

Model 层返回 error, Service 层返回 code, Controller 层通过 `GetMsg(code)` 获取具体信息返回给前端

- 错误码在 global/errmsg 中

JSON 传输数据一律使用 **小写 + 下划线**，Go 项目中使用 **驼峰**

## 数据库

MySQL 设置字段 true false 使用 tinyint, 注意在 Golang 定义结构体时该字段要使用指针
否则结构体的字段会被初始化其零值 (0), 设置为指针可以用 nil 来判断


## Gin

### gin 的参数校验 validator 中零值问题

gin 的参数校验是基于 validator 的，如果给了 `required` 标签，则不能传入零值
- 比如字符串的不能传入空串
- int 类型的不能传入 0
- bool 类型的不能传入 false。

有时候需要参数是必填，并且需要可以传入 0，例如 sex 为 0 表示女，为 1 表示男，且必填
通过定义 int 类型的**指针** 解决这个问题：指针的零值是 `nil`


### 关于使用 POST 还是 PUT？

POST:
- 用于提交请求，可以更新或者创建资源，是非幂等的
- 在用户注册时，每次提交都是创建一个用户账号，此时用 POST

PUT:
- 用于向指定的 url 传送更新资源，是幂等的
- 比如修改密码，虽然提交的还是账户名和密码，但是每次提交都只是更新该用户密码，每次请求都只是覆盖原型的值，此时用 PUT


### 日志记录

启动相关的日志直接输出到控制台，运行过程中的日志记录到文件

## 项目特色

使用 RESTful API
