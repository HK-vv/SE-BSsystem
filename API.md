# 接口文档

## 管理端

### 账号登录登出

#### 登录

管理员和超级管理员使用该接口进行登录。
前端发送的登录请求中包含**账号**、**密码**。
后端接收后，对账号密码的正确性进行校验。
后端使用session会话机制。
如果校验通过，服务端在响应消息头Set-Cookie 中存入sessionid ，该用户的后续请求头Cookie中必须携带sessionid。

##### 请求

###### 请求头

```http
POST /api/admin/login
Content-Type: application/json
```

###### 消息体

```json
{
	"username" : "super",
	"password" : "123456"
}
```

###### 参数信息

| 参数名   | 示例   | 必要性 | 含义           | 类型   |
| -------- | ------ | ------ | -------------- | ------ |
| username | super  | 必有   | 登录用的用户名 | string |
| password | 123456 | 必有   | 登录用的密码   | string |

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=<sessionid数值>
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0,
  "first_time": true
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,    
	"msg": "用户名或密码错误"
}
```

###### 参数信息

| 参数名     | 示例             | 必要性     | 含义           | 类型    |
| ---------- | ---------------- | ---------- | -------------- | ------- |
| ret        | 0                | 必有       | 是否正常返回   | int     |
| first_time | true             | ret为0时有 | 是否为首次登陆 | boolean |
| msg        | 用户名或密码错误 | ret为1时有 | 错误信息       | string  |

#### 登出

##### 请求

###### 请求头

```http
POST /api/admin/logout
Cookie: sessionid=<sessionid数值>
```

##### 响应

后端应该根据sessionid清除掉session，然后返回响应消息。

###### 响应头

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=""
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,    
	"msg": "未登录"
}
```

###### 参数信息

| 参数名 | 示例   | 必要性     | 含义         | 类型   |
| ------ | ------ | ---------- | ------------ | ------ |
| ret    | 0      | 必有       | 是否正常返回 | int    |
| msg    | 未登录 | ret为1时有 | 错误信息     | string |

### 管理员账号

#### 创建管理员账号

只有**超级管理员**才能使用此api，后端应该做权限检查。

超级管理员可以通过设置**用户名**创建一个初始管理员账号，如不输入密码，默认密码为`123456`。

后端应做用户名重复检查。

被创建的账号可以修改自己的用户名和密码。

##### 请求

###### 请求头

```http
PUT /api/admin/account
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

###### 消息体

```json
{
	"username" : "admin",
  "password" : "114514"
}
```

###### 参数信息

| 参数名   | 示例   | 必要性 | 含义               | 类型   |
| -------- | ------ | ------ | ------------------ | ------ |
| username | admin  | 必有   | 创建账户时的用户名 | string |
| password | 114514 | 可选   | 创建账户时的密码   | string |

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,    
	"msg":  "用户名已存在"
}
```

###### 参数信息

| 参数名 | 示例         | 必要性     | 含义         | 类型   |
| ------ | ------------ | ---------- | ------------ | ------ |
| ret    | 0            | 必有       | 是否正常返回 | int    |
| msg    | 用户名已存在 | ret为1时有 | 错误信息     | string |

#### 列出账号

管理员可以根据要求，按照页数列出所有用户账号，并查看账号信息。

而超级管理员除拥有管理员的权限外，**还可以列出管理员账号**，并查看账号信息。后端应做权限检查。

##### 请求

###### 请求头

```http
GET /api/admin/account?pagesize=4&pagenum=2&usertype=admin&keyword=HKvv
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

###### 参数信息

| 参数名   | 示例  | 必要性 | 含义                                                         | 类型   |
| -------- | ----- | ------ | ------------------------------------------------------------ | ------ |
| pagesize | 4     | 必有   | 每页列出的账号数量                                           | int    |
| pagenum  | 2     | 必有   | 获取第几页的信息                                             | int    |
| usertpye | admin | 可选   | 想要获取的用户类型，如管理员`admin`、用户`user`、管理员及用户`all`，不填时为列出所有用户 | string |
| keyword  | HKvv  | 可选   | 想要搜索账号的关键词                                         | string |

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0,
  "items": [
    {
      "username": "eddie",
      "name": "zrx",
      "gender":"男",
      "email":"19182605@buaa.edu.cn",
      "phone":"18800000001"
    },
    {
      "username": "HKvv",
      "name": "zzh",
      "gender":"男",
      "email":"19182637@buaa.edu.cn",
      "phone":"18800000002"
    },
    {
      "username": "littlehuo",
      "name": "hcl",
      "gender":"男",
      "email":"19182603@buaa.edu.cn",
      "phone":"18800000003"
    }
  ],
  "total": 7
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,    
	"msg": "您未拥有此权限"
}
```

###### 参数信息

| 参数名 | 示例           | 必要性     | 含义                               | 类型   |
| ------ | -------------- | ---------- | ---------------------------------- | ------ |
| ret    | 0              | 必有       | 是否正常返回                       | int    |
| items  | []             | 必有       | 当前页的全部账号信息               | list   |
| total  | 8              | 必有       | 当前要求下，系统总共拥有的账号数量 | int    |
| msg    | 您未拥有此权限 | ret为1时有 | 错误信息                           | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名   | 示例                 | 必要性 | 含义     | 类型   |
| -------- | -------------------- | ------ | -------- | ------ |
| username | eddie                | 必有   | 用户名   | string |
| name     | zrx                  | 必有   | 姓名     | string |
| gender   | 男                   | 必有   | 性别     | string |
| email    | 19182605@buaa.edu.cn | 必有   | 邮箱     | string |
| phone    | 18800000001          | 必有   | 联系电话 | string |

#### 删除管理员账号

只有**超级管理员**才能使用此api，后端应该做权限检查。

超级管理员可以通过用户名删除回收管理员账号。

##### 请求

###### 请求头

```http
DELETE /api/admin/account
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

###### 消息体

```json
{
	"username" : "HKvv"
}
```

###### 参数信息

| 参数名   | 示例 | 必要性 | 含义                   | 类型   |
| -------- | ---- | ------ | ---------------------- | ------ |
| username | HKvv | 必有   | 需要被删除账户的用户名 | string |

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,    
	"msg":  "用户名不存在"
}
```

###### 参数信息

| 参数名 | 示例         | 必要性     | 含义         | 类型   |
| ------ | ------------ | ---------- | ------------ | ------ |
| ret    | 0            | 必有       | 是否正常返回 | int    |
| msg    | 用户名不存在 | ret为1时有 | 错误信息     | string |

### 管理员信息

#### 查看账号信息

管理员和超级管理员都可以使用此接口查看个人账号信息。

##### 请求

###### 请求头

```http
GET /api/admin/information
Cookie: sessionid=<sessionid数值>
```

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
```

###### 消息体

正常返回(ret = 0):

```json
{
  "ret": 0,
  "infor": { 
    "username": "eddie",
    "name": "zrx",
    "gender":"男",
    "email":"19182605@buaa.edu.cn",
    "phone":"18800000001"
  }
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,
	"msg": "账号未登录"
}
```

###### 参数信息

| 参数名 | 示例 | 必要性 | 含义           | 类型       |
| ------ | ---- | ------ | -------------- | ---------- |
| infor  | {}   | 必有   | 需要修改的信息 | dictionary |

其中`infor`中的参数信息如下所示：

| 参数名   | 示例                 | 必要性 | 含义     | 类型   |
| -------- | -------------------- | ------ | -------- | ------ |
| username | eddie                | 必有   | 用户名   | string |
| name     | zrx                  | 必有   | 姓名     | string |
| gender   | 男                   | 必有   | 性别     | string |
| email    | 19182605@buaa.edu.cn | 必有   | 邮箱     | string |
| phone    | 18800000001          | 必有   | 联系电话 | string |

#### 修改账号信息

管理员和超级管理员都可以使用此接口修改个人账号信息。

修改用户名时，后端需要检测用户名是否存在。

修改密码时，前端需要验证两次输入密码是否相同。

##### 请求

###### 请求头

```http
POST /api/account
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

###### 消息体

```json
{
  "action": "modify",
  "newdata": { 
    "username": "eddie",
    "name": "zrx",
    "gender":"男",
    "password": "111111",
    "email":"19182605@buaa.edu.cn",
    "phone":"18800000001"
  }
}
```

###### 参数信息

| 参数名  | 示例   | 必要性 | 含义                 | 类型       |
| ------- | ------ | ------ | -------------------- | ---------- |
| action  | modify | 必有   | modify为修改个人信息 |            |
| newdata | {}     | 必有   | 需要修改的信息       | dictionary |

其中`newdata`中的参数信息如下所示：

| 参数名   | 示例                 | 必要性 | 含义     | 类型   |
| -------- | -------------------- | ------ | -------- | ------ |
| username | eddie                | 可选   | 用户名   | string |
| name     | zrx                  | 可选   | 姓名     | string |
| gender   | 男                   | 可选   | 性别     | string |
| password | 111111               | 可选   | 密码     | string |
| email    | 19182605@buaa.edu.cn | 可选   | 邮箱     | string |
| phone    | 18800000001          | 可选   | 联系电话 | string |

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0,
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,
	"msg": "用户名已存在"
}
```

###### 参数信息

| 参数名 | 示例       | 必要性     | 含义         | 类型   |
| ------ | ---------- | ---------- | ------------ | ------ |
| ret    | 0          | 必有       | 是否正常返回 | int    |
| msg    | 用户名重复 | ret为1时有 | 错误信息     | string |

#### 重置密码

只有**超级管理员**才能使用此api，后端应该做权限检查。

当管理员忘记密码时，超级管理员可以根据其提供的用户名重置其密码，默认密码为`123456`。

##### 请求

###### 请求头

```http
POST /api/account
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

###### 消息体

```json
{
  "action": "reset_password",
  "username": "HKvv"
}
```

###### 参数信息

| 参数名   | 示例           | 必要性 | 含义                     | 类型   |
| -------- | -------------- | ------ | ------------------------ | ------ |
| action   | reset_password | 必有   | reset_password为重置密码 | string |
| username | HKvv           | 必有   | 需要重置密码的用户名     | string |

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0,
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,
	"msg": "用户名不存在"
}
```

###### 参数信息

| 参数名 | 示例         | 必要性     | 含义         | 类型   |
| ------ | ------------ | ---------- | ------------ | ------ |
| ret    | 0            | 必有       | 是否正常返回 | int    |
| msg    | 用户名不存在 | ret为1时有 | 错误信息     | string |

## 客户端

### 账号登录登出

#### 登录

小程序使用该接口进行登录。
前端通过获取用户登录凭证`code`并发送至后端进行登录。
后端接收后，调用 `auth.code2Session` 接口，换取用户唯一标识`OpenID` 、 用户在微信开放平台帐号下的唯一标识`UnionID`（若当前小程序已绑定到微信开放平台帐号） 和会话密钥 `session_key`。

##### 请求

###### 请求头

```http
POST /api/client/login
Content-Type: application/json
```

###### 消息体

```json
{
	"code" : "0514QOFa1cnRSC0KdfIa1Qsw7F34QOFs"
}
```

###### 参数信息

| 参数名 | 示例                             | 必要性 | 含义         | 类型   |
| ------ | -------------------------------- | ------ | ------------ | ------ |
| code   | 0514QOFa1cnRSC0KdfIa1Qsw7F34QOFs | 必有   | 用户登录凭证 | string |

##### 响应

###### 响应头

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=<sessionid数值>
```

###### 消息体

正常返回(ret = 0):

```json
{
  // 俺不知道你要啥QwQ
	"ret": 0
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,    
	"msg": "用户登录凭证有误"
}
```

###### 参数信息

| 参数名 | 示例             | 必要性     | 含义         | 类型   |
| ------ | ---------------- | ---------- | ------------ | ------ |
| ret    | 0                | 必有       | 是否正常返回 | int    |
| msg    | 用户登录凭证有误 | ret为1时有 | 错误信息     | string |

#### 登出

##### 请求

###### 请求头

```http
POST /api/client/logout
Cookie: sessionid=<sessionid数值>
```

##### 响应

后端应该清除掉session_key，然后返回响应消息

###### 响应头

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=""
```

###### 消息体

正常返回(ret = 0):

```json
{
	"ret": 0
}
```

异常返回(ret = 1):

```json
{
	"ret": 1,    
	"msg": "未登录"
}
```

###### 参数信息

| 参数名 | 示例   | 必要性     | 含义         | 类型   |
| ------ | ------ | ---------- | ------------ | ------ |
| ret    | 0      | 必有       | 是否正常返回 | int    |
| msg    | 未登录 | ret为1时有 | 错误信息     | string |
