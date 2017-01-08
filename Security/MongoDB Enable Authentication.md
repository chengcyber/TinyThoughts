# Mongodb enable authentication

MongoDB 默认直接连接，无须身份验证，如果当前机器可以公网访问，且不注意Mongodb 端口（默认 27017）的开放状态，那么Mongodb就会产生安全风险，被利用此配置漏洞，入侵数据库。

# 容易遭受入侵的环境

- 使用默认 mongod 命令启动 Mongodb
- 机器可以被公网访问
- 在公网上开放了 Mongodb 端口

# 安全风险

- 数据库隐私泄露
- 数据库被清空
- 数据库运行缓慢

# 解决方案

## 1. 禁止公网访问 Mongodb 端口

### 1.1 网络配置
由于网络配置因人而异，需要根据自己实际环境进行配置，不作冗述。大致可以从以下方面禁止。

- 在路由器中关闭端口转发
- 防火墙 iptables 禁止访问

### 1.2 验证端口能否访问方式

在外网机器命令行中运行
```Bash
telnet your.machine.open.ip 27017
```

## 2. 启用验证

### 2.1 创建用户管理员账户

当前数据库版本：Mongodb 3.4

使用 mongod 启动数据库
新建终端

```Bash
mongod --port 27017 --dbpath /data/db1
```
**参数默认可以不加，若有自定义参数，才要加上，下同。**
    
另起一个终端，运行下列命令
```Bash
mongo --port 27017

use admin

db.createUser(
  {
    user: "adminUser",
    pwd: "adminPass",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```
管理员创建成功，现在拥有了用户管理员
用户名：adminUser
密码：adminPass
然后，断开 mongodb 连接， 关闭数据库
两个终端下 <C - c>

### 2.2 Mongodb 用户验证登陆

启动带访问控制的 Mongodb
新建终端
```Bash
mongod --auth --port 27017 --dbpath /data/db1
```

现在有两种方式进行用户身份的验证
第一种 （类似 MySql）
客户端连接时，指定用户名，密码，db名称
```Bash
mongo --port 27017 -u "adminUser" -p "adminPass" --authenticationDatabase "admin"
```

第二种 
客户端连接后，再进行验证
```Bash
mongo --port 27017

use admin
db.auth("adminUser", "adminPass")

// 输出 1 表示验证成功
```

### 2.3 创建普通用户
过程类似创建管理员账户，只是 role 有所不同

```Bash
use foo

db.createUser(
  {
    user: "simpleUser",
    pwd: "simplePass",
    roles: [ { role: "readWrite", db: "foo" },
             { role: "read", db: "bar" } ]
  }
)
```
现在我们有了一个普通用户
用户名：simpleUser
密码：simplePass
权限：读写数据库 foo， 只读数据库 bar。

**注意**
**NOTE**
**WARN**
`use foo`表示用户在 foo 库中创建，就一定要 foo 库验证身份，即用户的信息跟随随数据库。比如上述 simpleUser 虽然有 bar 库的读取权限，但是一定要先在 foo 库进行身份验证，直接访问会提示验证失败。
```Bash
use foo
db.auth("simpleUser", "simplePass")

use bar
show collections
```
还有一点需要注意，如果 admin 库没有任何用户的话，即使在其他数据库中创建了用户，启用身份验证，默认的连接方式依然会有超级权限

### 2.4 内建角色
- Read：允许用户读取指定数据库
- readWrite：允许用户读写指定数据库
- dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
- userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
- clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
- readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
- readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
- userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
- dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
- root：只在admin数据库中可用。超级账号，超级权限

### 2.5 URI 形式的访问
生产中常用 URI 形式对数据库进行连接
```
mongodb://your.db.ip.address:27017/foo
```
添加用户名密码验证
```
mongodb://simpleUser:simplePass@your.db.ip.address:27017/foo
```

# 参考链接
- [Enable Authentication](https://docs.mongodb.com/manual/tutorial/enable-authentication/)
- [Build-in Roles](https://docs.mongodb.com/manual/core/security-built-in-roles/)
- [Mongodb 3.0 用户创建](http://www.cnblogs.com/zhoujinyi/p/4610050.html)
- [Mongodb Authentication](http://bubkoo.com/2014/02/07/mongodb-authentication/)

# 结语
在使用数据库的过程中，一定要注意安全风险，由于 Mongodb 的默认配置，使得数据库有入侵风险，应该予以防范。