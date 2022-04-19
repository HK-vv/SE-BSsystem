# 接口文档

**版本**: `v1.0.1`
**进度**: 基础版正式发布

## Content

| 序号 | 类型 | 接口名称                                        | 说明                                     | 完成情况(0: 接口确定, 1: 接口实现) |
| ---- | ---- | ----------------------------------------------- | -------- | ---------------------------------------- |
| 1 | 内部 | /api/admin/auth/login                           | 管理员登录                               | 1                                  |
| 2 | 内部 | /api/admin/auth/logout                          | 管理员登出                               | 1                                  |
| 3 | 内部 | /api/admin/admin_account                        | 创建、删除管理员账号，查看、修改个人信息 | 1                                  |
| 4 | 内部 | /api/admin/admin_account/get_username           | 查看账号用户名 | 0 |
| 5 | 内部 | /api/admin/admin_account/reset_password         | 重置管理员密码                           | 1                                  |
| 6 | 内部 | /api/admin/admin_account/list                   | 列出管理员账号                           | 1                                  |
| 7 | 内部 | /api/admin/admin_account/integrity_verification | 检测管理员信息是否完整                   | 1                                  |
| 8 | 内部 | /api/admin/admin_account/issuper                | 检测是否为超级管理员                     | 1                                  |
| 9 | 内部 | /api/admin/user_account/list                    | 列出用户账号                             | 1                                  |
| 10 | 内部 | /api/admin/user_account/contest/history         | 列出用户比赛历史                         | 0                                  |
| 11 | 内部 | /api/admin/user_account/contest/result          | 列出用户答题情况                         | 0                                   |
| 12 | 内部 | /api/admin/tag                                  | 增删改标签                               | 1                                  |
| 13 | 内部 | /api/admin/problem                              | 增删改查题目信息                         | 1                                  |
| 14 | 内部 | /api/admin/problem/batch/add                    | 批量添加题目                             | 1                                  |
| 15 | 内部 | /api/admin/problem/batch/public                 | 批量公开题目                             | 1                                  |
| 16 | 内部 | /api/admin/problem/batch/delete | 批量删除题目 | 1 |
|  | 内部 | /api/admin/problem/detail | 查看题目详细信息 |  |
| 17 | 内部 | /api/admin/contest                              | 增改查比赛                             | 1                                  |
| 18 | 内部 | /api/admin/contest/batch/delete | 批量删除比赛 | 1 |
| 19 | 内部 | /api/admin/contest/calculate                    | 比赛开始算分                             | 0                                   |
| 20 | 内部 | /api/admin/contest/leaderboard                  | 查看比赛排行榜                           | 0                                   |
| 21 | 内部 | /api/admin/contest/statistics                   | 查看比赛统计                             | 0                                   |
| 22 | 内部 | /api/user/auth/login                            | 用户登录                                 | 1                                  |
| 23 | 内部 | /api/user/auth/logout                           | 用户登出                                 | 1                                  |
| 24 | 内部 | /api/user/profile                               | 用户查看、修改个人信息                   | 1                                 |
| 25 | 内部 | /api/user/exercise/collect                      | 自主组卷                                 | 1                                 |
| 26 | 内部 | /api/user/exercise/problem                      | 获取题面                                 | 1                                 |
| 27 | 内部 | /api/user/exercise/problem/check                | 验证答案                                 | 1                                 |
| 28 | 内部 | /api/user/contest/register                      | 注册比赛                                 | 0                                  |
| 29 | 内部 | /api/user/contest/start                         | 开始比赛             | 0                                  |
| 30 | 内部 | /api/user/contest/problem                       | 获取比赛题目题面                         | 0                                  |
| 31 | 内部 | /api/user/contest/problem/submit                | 提交答案                                 | 0                                  |
| 32 | 内部 | /api/user/contest/record                        | 查询所有参加比赛                         | 0 |
| 33 | 内部 | /api/user/contest/result                        | 查询比赛记录                             | 0                                  |
| 34 | 内部 | /api/user/contest/leaderboard                   | 查询比赛排行榜                           | 0                                  |
| 35 | 内部 | /api/general/tag/list                           | 获取所有标签                             | 1                                 |
| 36 | 内部 | /api/general/contest/list                       | 查找所有比赛                             | 0                                  |
## 返回值(ret)规定

不同的返回值`ret`对应不同的含义，具体可参考下表：

| ret值 | 含义     | 后续操作                 | 备注                                   |
| :---: | -------- | ------------------------ | -------------------------------------- |
|   0   | 正常返回 | 继续执行                 |                                        |
|   1   | 一般错误 | 返回到执行前重新执行     | 一般错误包括权限不足、查询信息不存在等 |
|   2   | 登录过期 | 返回登录界面重新登录     | 默认存在                               |
|   3   | 其他错误 | 查看后台报错信息尝试修复 | 默认存在                               |

**notice**: 由于每个接口都可能返回2,3这样固定类型的错误, 我们在接口详情里省略这两种返回. 我们强烈建议前端在收到response时统一处理这两个返回值, 不要在每个接口调用处复制判断逻辑.

## 管理端

### 账号登录登出

#### 登录

管理员和超级管理员使用该接口进行登录。
前端发送的登录请求中包含**账号**、**密码**。
后端接收后，对账号密码的正确性进行校验。
后端使用session会话机制。
如果校验通过，服务端在响应消息头Set-Cookie 中存入sessionid ，该用户的后续请求头Cookie中必须携带sessionid。

##### 请求

**请求头**

```http
POST /api/admin/auth/login
Content-Type: application/json
```

**消息体**

```json
{
  "username" : "super",
  "password" : "123456"
}
```

**参数信息**

| 参数名   | 示例   | 必要性 | 含义           | 类型   |
| -------- | ------ | ------ | -------------- | ------ |
| username | super  | 必有   | 登录用的用户名 | string |
| password | 123456 | 必有   | 登录用的密码   | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=<sessionid数值>
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "用户名或密码错误"
}
```

**参数信息**

| 参数名 | 示例             | 必要性       | 含义         | 类型   |
| ------ | ---------------- | ------------ | ------------ | ------ |
| ret    | 0                | 必有         | 是否正常返回 | int    |
| msg    | 用户名或密码错误 | ret不为0时有 | 错误信息     | string |

#### 登出

##### 请求

**请求头**

```http
POST /api/admin/auth/logout
Cookie: sessionid=<sessionid数值>
```

##### 响应

后端应该根据sessionid清除掉session，然后返回响应消息。

**响应头**

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=""
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

**参数信息**

| 参数名 | 示例   | 必要性       | 含义         | 类型   |
| ------ | ------ | ------------ | ------------ | ------ |
| ret    | 0      | 必有         | 是否正常返回 | int    |
| msg    | 未登录 | ret不为0时有 | 错误信息     | string |

### 管理员账号

#### 创建管理员账号

只有**超级管理员**才能使用此api，后端应该做权限检查。

超级管理员可以通过设置**用户名**创建一个初始管理员账号，如不输入密码，默认密码为`123456`。

后端应做用户名重复检查。

被创建的账号可以修改自己的用户名和密码。

##### 请求

**请求头**

```http
PUT /api/admin/admin_account
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "username" : "admin"
}
```

**参数信息**

| 参数名   | 示例  | 必要性 | 含义               | 类型   |
| -------- | ----- | ------ | ------------------ | ------ |
| username | admin | 必有   | 创建账户时的用户名 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg":  "用户名已存在"
}
```

**参数信息**

| 参数名 | 示例         | 必要性       | 含义         | 类型   |
| ------ | ------------ | ------------ | ------------ | ------ |
| ret    | 0            | 必有         | 是否正常返回 | int    |
| msg    | 用户名已存在 | ret不为0时有 | 错误信息     | string |

#### 查看账号信息

管理员和超级管理员都可以使用此接口查看个人账号信息。

##### 请求

**请求头**

```http
GET /api/admin/admin_account
Cookie: sessionid=<sessionid数值>
```

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "info": { 
    "username": "eddie",
    "email":"19182605@buaa.edu.cn",
    "phone":"18800000001"
  }
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型       |
| ------ | ---------- | ------------ | ------------ | ---------- |
| ret    | 0          | 必有         | 是否正常返回 | int        |
| info   | { }        | 必有         | 个人信息     | dictionary |
| msg    | 标签不存在 | ret不为0时有 | 错误信息     | string     |

其中`info`中的参数信息如下所示：

| 参数名   | 示例                 | 必要性 | 含义     | 类型   |
| -------- | -------------------- | ------ | -------- | ------ |
| username | eddie                | 必有   | 用户名   | string |
| email    | 19182605@buaa.edu.cn | 必有   | 邮箱     | string |
| phone    | 18800000001          | 必有   | 联系电话 | string |

#### 修改账号信息

管理员和超级管理员都可以使用此接口修改个人账号信息。

修改用户名时，**后端**需要检测用户名是否存在。

修改密码时，**前端**需要验证两次输入密码是否相同。

**前端**还需要检验**邮箱**和**电话**是否符合格式。

##### 请求

**请求头**

```http
POST /api/admin/admin_account
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "newdata": { 
    "username": "eddie",
    "password": "111111",
    "email":"19182605@buaa.edu.cn",
    "phone":"18800000001"
  }
}
```

**参数信息**

| 参数名  | 示例 | 必要性 | 含义           | 类型       |
| ------- | ---- | ------ | -------------- | ---------- |
| newdata | { }  | 必有   | 需要修改的信息 | dictionary |

其中`newdata`中的参数信息如下所示：

| 参数名   | 示例                 | 必要性 | 含义     | 类型   |
| -------- | -------------------- | ------ | -------- | ------ |
| username | eddie                | 可选   | 用户名   | string |
| password | 111111               | 可选   | 密码     | string |
| email    | 19182605@buaa.edu.cn | 可选   | 邮箱     | string |
| phone    | 18800000001          | 可选   | 联系电话 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "用户名已存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| msg    | 用户名重复 | ret不为0时有 | 错误信息     | string |

#### 删除管理员账号

只有**超级管理员**才能使用此api，后端应该做权限检查。

超级管理员可以通过用户名删除回收管理员账号。

##### 请求

**请求头**

```http
DELETE /api/admin/admin_account
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "username" : "HKvv"
}
```

**参数信息**

| 参数名   | 示例 | 必要性 | 含义                   | 类型   |
| -------- | ---- | ------ | ---------------------- | ------ |
| username | HKvv | 必有   | 需要被删除账户的用户名 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "用户名不存在"
}
```

**参数信息**

| 参数名 | 示例         | 必要性       | 含义         | 类型   |
| ------ | ------------ | ------------ | ------------ | ------ |
| ret    | 0            | 必有         | 是否正常返回 | int    |
| msg    | 用户名不存在 | ret不为0时有 | 错误信息     | string |

#### 获取账号用户名

管理员和超级管理员都可以使用此接口查看自己的用户名。

##### 请求

**请求头**

```http
GET /api/admin/admin_account/get_username
Cookie: sessionid=<sessionid数值>
```

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "username": "eddie"
}
```

**参数信息**

| 参数名   | 示例  | 必要性       | 含义         | 类型   |
| -------- | ----- | ------------ | ------------ | ------ |
| ret      | 0     | 必有         | 是否正常返回 | int    |
| username | eddie | 必有         | 用户名       | string |
| msg      |       | ret不为0时有 | 错误信息     | string |

### 查看其他账号

#### 列出管理员账号

管理员和超级管路员可以根据要求，按照页数列出管理员账号，并查看账号信息, 查找为模糊查找。

##### 请求

**请求头**

```http
GET /api/admin/admin_account/list?pagesize=4&pagenum=2&keyword=HKvv
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名   | 示例 | 必要性 | 含义                 | 类型   |
| -------- | ---- | ------ | -------------------- | ------ |
| pagesize | 4    | 必有   | 每页列出的账号数量   | int    |
| pagenum  | 2    | 必有   | 获取第几页的信息     | int    |
| keyword  | HKvv | 可选   | 想要搜索账号的关键词 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "username": "eddie",
      "email":"19182605@buaa.edu.cn",
      "phone":"18800000001",
      "usertype": "admin"
    },
    {
      "username": "HKvv",
      "email":"19182637@buaa.edu.cn",
      "phone":"18800000002",
      "usertype": "admin"
    },
    {
      "username": "super",
      "email":"Brainstorm@163.com",
      "phone":"18800000003",
      "usertype": "super"
    }
  ],
  "total": 7
}
```

**参数信息**

| 参数名 | 示例           | 必要性       | 含义                               | 类型   |
| ------ | -------------- | ------------ | ---------------------------------- | ------ |
| ret    | 0              | 必有         | 是否正常返回                       | int    |
| items  | [ ]            | 必有         | 当前页的全部账号信息               | list   |
| total  | 7              | 必有         | 当前要求下，系统总共拥有的账号数量 | int    |
| msg    | 您未拥有此权限 | ret不为0时有 | 错误信息                           | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名   | 示例                 | 必要性 | 含义       | 类型   |
| -------- | -------------------- | ------ | ---------- | ------ |
| username | eddie                | 必有   | 用户名     | string |
| email    | 19182605@buaa.edu.cn | 必有   | 邮箱       | string |
| phone    | 18800000001          | 必有   | 联系电话   | string |
| usertype | admin                | 必有   | 管理员类型 | string |

#### 列出用户账号

管理员和超级管路员可以根据要求，按照页数列出用户账号，并查看账号信息, 查找为模糊查找。

可选择是否按照rating排序。若选择，则`descending`表示降序排序；`ascending`表示升序排序。

##### 请求

**请求头**

```http
GET /api/admin/user_account/list?pagesize=2&pagenum=2&sort_by_rating=descending
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名         | 示例       | 必要性 | 含义                 | 类型   |
| -------------- | ---------- | ------ | -------------------- | ------ |
| pagesize       | 2          | 必有   | 每页列出的账号数量   | int    |
| pagenum        | 2          | 必有   | 获取第几页的信息     | int    |
| keyword        | Hkvv       | 可选   | 想要搜索账号的关键词 | string |
| sort_by_rating | descending | 可选   | 按照rating排序       | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "username": "HKvv",
      "rating": 1983,
      "matches": 29
    },
    {
      "username": "eddie",
      "rating": 1304,
      "matches": 8
    }
  ],
  "total": 7
}
```

**参数信息**

| 参数名 | 示例           | 必要性       | 含义                               | 类型   |
| ------ | -------------- | ------------ | ---------------------------------- | ------ |
| ret    | 0              | 必有         | 是否正常返回                       | int    |
| items  | [ ]            | 必有         | 当前页的全部账号信息               | list   |
| total  | 7              | 必有         | 当前要求下，系统总共拥有的账号数量 | int    |
| msg    | 您未拥有此权限 | ret不为0时有 | 错误信息                           | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名   | 示例 | 必要性 | 含义             | 类型   |
| -------- | ---- | ------ | ---------------- | ------ |
| username | HKvv | 必有   | 用户名           | string |
| rating   | 1983 | 必有   | 用户分数         | int    |
| matches  | 29   | 必有   | 用户参加比赛次数 | int    |

#### 列出用户比赛历史

##### 请求

**请求头**

```http
GET /api/admin/user_account/contest/history?pagesize=2&pagenum=2&username=HKvv
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名   | 示例  | 必要性 | 含义               | 类型   |
| -------- | ----- | ------ | ------------------ | ------ |
| pagesize | 2     | 必有   | 每页列出的账号数量 | int    |
| pagenum  | 1     | 必有   | 获取第几页的信息   | int    |
| username | HKvv  | 必有   | 用户名             | string |
| keyword  | April | 可选   | 比赛名关键词       | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "contestid": 1,
      "name": "April Fools Day Contest 2022",
      "rated": true,
      "score": 76,
      "before_rating": 1500,
      "after_rating": 1532,
      "timecost": 42,
      "rank": 10
    }
  ],
  "total": 1
}
```

**参数信息**

| 参数名 | 示例         | 必要性       | 含义                           | 类型   |
| ------ | ------------ | ------------ | ------------------------------ | ------ |
| ret    | 0            | 必有         | 是否正常返回                   | int    |
| items  | [ ]          | 必有         | 比赛信息                       | list   |
| total  | 1            | 必有         | 当前要求下，总共拥有的比赛数量 | int    |
| msg    | 用户名不存在 | ret不为0时有 | 错误信息                       | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名        | 示例                         | 必要性 | 含义       | 类型    |
| ------------- | ---------------------------- | ------ | ---------- | ------- |
| contestid     | 1                            | 必有   | 比赛id     | int     |
| name          | April Fools Day Contest 2022 | 必有   | 比赛名     | string  |
| rated         | true                         | 必有   | 开始时间   | boolean |
| score         | 76                           | 必有   | 得分       | int     |
| before_rating | 1500                         | 必有   | 比前rating | int     |
| after_rating  | 1532                         | 必有   | 比后rating | int     |
| timecost      | 42                           | 必有   | 总耗时(s)  | int     |
| rank          | 10                           | 必有   | 比赛排名   | int     |


#### 查看用户答题情况

##### 请求

**请求头**

```http
GET /api/admin/user_account/contest/result?contestid=1&username=HKvv
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义   | 类型   |
| --------- | ---- | ------ | ------ | ------ |
| contestid | 1    | 必有   | 比赛id | int    |
| username  | HKvv | 必有   | 用户名 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "problemno": 1,
      "problemid": 5,
      "correct": true,
      "submitted": "A",
      "answer": "A"
    },
    {
      "problemno": 2,
      "problemid": 3,
      "correct": true,
      "submitted": "BCD",
      "answer": "BCD"
    },
    {
      "problemno": 3,
      "problemid": 8,
      "correct": false,
      "submitted": "HKwv",
      "answer": "HKvv"
    }
  ],
  "total": 3
}
```

**参数信息**

| 参数名 | 示例 | 必要性       | 含义         | 类型   |
| ------ | ---- | ------------ | ------------ | ------ |
| ret    | 0    | 必有         | 是否正常返回 | int    |
| items  | [ ]  | 必有         | 比赛答题记录 | list   |
| total  | 3    | 必有         | 总题目量     | int    |
| msg    |      | ret不为0时有 | 错误信息     | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名    | 示例 | 必要性 | 含义         | 类型    |
| --------- | ---- | ------ | ------------ | ------- |
| problemno | 1    | 必有   | 题目序号     | int     |
| problemid | 5    | 必有   | 题目id       | int     |
| correct   | true | 必有   | 是否正确     | boolean |
| submitted | A    | 必有   | 当时提交答案 | string  |
| answer    | A    | 必有   | 正确答案     | string  |

### 重置密码

#### 管理员重置密码

只有**超级管理员**才能使用此api，后端应该做权限检查。

当管理员忘记密码时，超级管理员可以根据其提供的用户名重置其密码，默认密码为`123456`。

##### 请求

**请求头**

```http
POST /api/admin/admin_account/reset_password
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "username": "HKvv"
}
```

**参数信息**

| 参数名   | 示例 | 必要性 | 含义                 | 类型   |
| -------- | ---- | ------ | -------------------- | ------ |
| username | HKvv | 必有   | 需要重置密码的用户名 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "用户名不存在"
}
```

**参数信息**

| 参数名 | 示例         | 必要性       | 含义         | 类型   |
| ------ | ------------ | ------------ | ------------ | ------ |
| ret    | 0            | 必有         | 是否正常返回 | int    |
| msg    | 用户名不存在 | ret不为0时有 | 错误信息     | string |

### 信息完整性

#### 检测信息是否完整

可以通过这个接口检测管理员的个人信息是否完整，即用户名、邮箱、联系方式都已填写。

##### 请求

**请求头**

```http
GET /api/admin/admin_account/integrity_verification
Cookie: sessionid=<sessionid数值>
```

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "integrity": true
}
```

**参数信息**

| 参数名    | 示例             | 必要性       | 含义         | 类型   |
| --------- | ---------------- | ------------ | ------------ | ------ |
| ret       | 0                | 必有         | 是否正常返回 | int    |
| integrity | true             | ret为0时有   | 信息是否完整 | boolen |
| msg       | 系统出现致命错误 | ret不为0时有 | 错误信息     | string |

### 管理员类型

#### 检测是否为超级管理员

可以通过该接口检测该管理员是否为超级管理员。

##### 请求

**请求头**

```http
GET /api/admin/admin_account/issuper
Cookie: sessionid=<sessionid数值>
```

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "issuper": false
}
```

**参数信息**

| 参数名  | 示例     | 必要性       | 含义             | 类型   |
| ------- | -------- | ------------ | ---------------- | ------ |
| ret     | 0        | 必有         | 是否正常返回     | int    |
| issuper | false    | ret为0时有   | 是否为超级管理员 | boolen |
| msg     | 登录过期 | ret不为0时有 | 错误信息         | string |

### 标签

#### 添加标签

管理员可以通过此接口添加标签。

##### 请求

**请求头**

```http
PUT /api/admin/tag
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "tag": "语文"
}
```

**参数信息**

| 参数名 | 示例 | 必要性 | 含义   | 类型   |
| ------ | ---- | ------ | ------ | ------ |
| tag    | 语文 | 必有   | 标签名 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

**参数信息**

| 参数名 | 示例     | 必要性       | 含义         | 类型   |
| ------ | -------- | ------------ | ------------ | ------ |
| ret    | 0        | 必有         | 是否正常返回 | int    |
| msg    | 登录过期 | ret不为0时有 | 错误信息     | string |

#### 修改标签

管理员可以使用此接口修改标签。

##### 请求

**请求头**

```http
POST /api/admin/tag
Cookie: sessionid=<sessionid数值>
```

**消息体**

```json
{
  "oldname": "高等数学",
  "newname": "数学分析"
}
```

**参数信息**

| 参数名  | 示例     | 必要性 | 含义     | 类型   |
| ------- | -------- | ------ | -------- | ------ |
| oldname | 高等数学 | 必有   | 标签原名 | string |
| newname | 数学分析 | 必有   | 标签新名 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

**参数信息**

| 参数名 | 示例     | 必要性       | 含义         | 类型   |
| ------ | -------- | ------------ | ------------ | ------ |
| ret    | 0        | 必有         | 是否正常返回 | int    |
| msg    | 登录过期 | ret不为0时有 | 错误信息     | string |

#### 删除标签

##### 请求

**请求头**

```http
DELETE /api/admin/tag
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "tag": "语文"
}
```

**参数信息**

| 参数名 | 示例 | 必要性 | 含义                 | 类型   |
| ------ | ---- | ------ | -------------------- | ------ |
| tag    | 语文 | 必有   | 需要被删除的标签名称 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "标签不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| msg    | 标签不存在 | ret不为0时有 | 错误信息     | string |

#### 查看标签

具体方法请见`通用 - 查看所有标签`，[点击查看](#查看所有标签)。

### 题目

#### 添加单个题目

管理员可以通过此接口添加单个题目。

对于单选、多选，`A` ` B` `C` `D`选项不能为空；对于判断，`A` ` B`选项应默认为**正确**和**错误**。

##### 请求

**请求头**

```http
PUT /api/admin/problem
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "type": "multiple",
  "tags": [
    "算法"
  ],
  "description": "请选出所有OJ",
  "options":[
    "Codeforces",
    "DBforces",
    "accoding", 
    "wacoding"
  ],
  "answer": "ABC",
  "public": true
}
```

**参数信息**

| 参数名      | 示例                                               | 必要性 | 含义     | 类型    |
| ----------- | -------------------------------------------------- | ------ | -------- | ------- |
| type        | "single" / "multiple" / "binary" / "completion"    | 必有   | 题目类型 | string  |
| tags        | [ ]                                                | 必有   | 题目标签 | list    |
| description | "请选出所有OJ"                                     | 必有   | 题面     | string  |
| options     | ["Codeforces", "DBforces", "accoding", "wacoding"] | 必有   | 选项     | list    |
| answer      | "ABC"                                              | 必有   | 答案     | string  |
| public      | true                                               | 必有   | 公开性   | boolean |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

**参数信息**

| 参数名 | 示例     | 必要性       | 含义         | 类型   |
| ------ | -------- | ------------ | ------------ | ------ |
| ret    | 0        | 必有         | 是否正常返回 | int    |
| msg    | 登录过期 | ret不为0时有 | 错误信息     | string |

#### 批量添加题目

管理员可以通过此接口使用文件批量添加题目，文件格式限定为`.xls`或`.xlsx`，即软件`excel`保存后的格式。

##### 请求

**请求头**

```http
POST /api/admin/problem/batch/add
Cookie: sessionid=<sessionid数值>
Content-Type: false
```

**消息体**

```json
{
  "file": (binary),
  "tags": [
    "算法"
  ]
}
```

**参数信息**

| 参数名 | 示例 | 必要性 | 含义     | 类型 |
| ------ | ---- | ------ | -------- | ---- |
| file   |      | 必有   | 文件     | file |
| tags   | [ ]  | 必有   | 题目标签 | list |

`file`表示文件，文件内表格格式如下：

| 题型   | 题目                                                         | 选项1    | 选项2    | 选项3    | 选项4            | 正确答案                     | 是否公开 |
| ------ | ------------------------------------------------------------ | -------- | -------- | -------- | ---------------- | ---------------------------- | -------- |
| 单选题 | 我国的火警报警电话是（ ）？                                  | 110      | 120      | 119      | 911              | C                            | 公开     |
| 多选题 | 四大文明古国中的，四大文明是下列选项中的哪四个（）？         | 埃及文明 | 印度文明 | 中国文明 | 美索不达米亚文明 | ABCD                         | 不公开   |
| 判断题 | 《出师表》中，“先帝創業未半，而中道崩殂“中的”先帝“是指刘备。 | 正确     | 错误     |          |                  | A                            | 公开     |
| 填空题 | 《史记》的作者是____                                         |          |          |          |                  | 司马迁                       | 不公开   |
| 填空题 | kmp本质是一种____算法, 其时间复杂度是____的.                 |          |          |          |                  | 动态规划/dp,线性/O(n)/O(n+m) | 公开     |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "文件格式有误, line 3",
}
```

**参数信息**

| 参数名 | 示例         | 必要性       | 含义         | 类型   |
| ------ | ------------ | ------------ | ------------ | ------ |
| ret    | 0            | 必有         | 是否正常返回 | int    |
| msg    | 文件格式有误 | ret不为0时有 | 错误信息     | string |

#### 查找题目

管理员可以使用此接口筛选并查看题目信息。

##### 请求

**请求头**

```http
GET /api/admin/problem?pagesize=2&pagenum=1&type=binary+single&tags=语文+数学&author=HKvv&public=1&keyword=出师表
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名   | 示例          | 必要性 | 含义                           | 类型   |
| -------- | ------------- | ------ | ------------------------------ | ------ |
| pagesize | 2             | 必有   | 每页列出的账号数量             | int    |
| pagenum  | 1             | 必有   | 获取第几页的信息               | int    |
| type     | binary+single | 可选   | 题目类型                       | string |
| tags     | 语文+数学     | 可选   | 题目标签                       | string |
| author   | HKvv          | 可选   | 作者用户名                     | string |
| public   | 0             | 可选   | 公开性，0表示未公开，1表示公开 | int    |
| keyword  | 出师表        | 可选   | 题目描述关键词                 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "problemid": 1,
      "type": "binary",
      "tags": [
        "语文"
      ],
      "description": "《出师表》中，“先帝創業未半，而中道崩殂“中的”先帝“是指刘备。",
      "options":[
        "正确",
        "错误"
      ],
      "answer": "A",
      "public": true,
      "author": "HKvv"
    }
  ],
  "total": 1
}
```

**参数信息**

| 参数名 | 示例     | 必要性       | 含义                               | 类型   |
| ------ | -------- | ------------ | ---------------------------------- | ------ |
| ret    | 0        | 必有         | 是否正常返回                       | int    |
| items  | [ ]      | 必有         | 当前页的全部题目信息               | list   |
| total  | 1        | 必有         | 当前要求下，系统总共拥有的题目数量 | int    |
| msg    | 登录过期 | ret不为0时有 | 错误信息                           | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名      | 示例                                                         | 必要性 | 含义       | 类型    |
| ----------- | ------------------------------------------------------------ | ------ | ---------- | ------- |
| problemid   | 1                                                            | 必有   | 题目id     | int     |
| type        | binary                                                       | 必有   | 题目类型   | string  |
| tags        | [ ]                                                          | 必有   | 题目标签   | list    |
| description | 《出师表》中，“先帝創業未半，而中道崩殂“中的”先帝“是指刘备。 | 必有   | 题目信息   | string  |
| options     | [ ]                                                          | 必有   | 选项       | list    |
| answer      | A                                                            | 必有   | 答案       | string  |
| public      | true                                                         | 必有   | 公开性     | boolean |
| author      | HKvv                                                         | 必有   | 作者用户名 | string  |

#### 查看题面

##### 请求

**请求头**

```http
GET /api/admin/problem/detail?problemid=1
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义   | 类型 |
| --------- | ---- | ------ | ------ | ---- |
| problemid | 1    | 必有   | 题目id | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "info": {
    "problemid": 1,
    "type": "binary",
    "tags": [
      "语文"
    ],
    "description": "《出师表》中，“先帝創業未半，而中道崩殂“中的”先帝“是指刘备。",
    "options":[
      "正确",
      "错误"
    ],
    "answer": "A",
    "public": true,
    "author": "HKvv"
  }
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "题目不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型       |
| ------ | ---------- | ------------ | ------------ | ---------- |
| ret    | 0          | 必有         | 是否正常返回 | int        |
| info   | { }        | 必有         | 题目信息     | dictionary |
| msg    | 题目不存在 | ret不为0时有 | 错误信息     | string     |

其中`info`的参数信息如下所示：

| 参数名      | 示例                                                         | 必要性 | 含义       | 类型    |
| ----------- | ------------------------------------------------------------ | ------ | ---------- | ------- |
| problemid   | 1                                                            | 必有   | 题目id     | int     |
| type        | binary                                                       | 必有   | 题目类型   | string  |
| tags        | [ ]                                                          | 必有   | 题目标签   | list    |
| description | 《出师表》中，“先帝創業未半，而中道崩殂“中的”先帝“是指刘备。 | 必有   | 题目信息   | string  |
| options     | [ ]                                                          | 必有   | 选项       | list    |
| answer      | A                                                            | 必有   | 答案       | string  |
| public      | true                                                         | 必有   | 公开性     | boolean |
| author      | HKvv                                                         | 必有   | 作者用户名 | string  |

#### 删除题目

管理员仅能删除**自己创建的题目**，超级管理员可以删除**所有题目**，**后端**应做权限检查。

##### 请求

**请求头**

```http
PUT /api/admin/problem/batch/delete
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "problems": [
    1,
    2
  ]
}
```

**参数信息**

| 参数名   | 示例 | 必要性 | 含义               | 类型 |
| -------- | ---- | ------ | ------------------ | ---- |
| problems | [ ]  | 必有   | 需要被删除的题目id | list |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "题目不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| msg    | 题目不存在 | ret不为0时有 | 错误信息     | string |

#### 修改题目信息

管理员仅能修改**自己创建的题目**的信息，超级管理员可以修改**所有题目**的信息，**后端**应做权限检查。

题目仅能修改为公开。

对于单选、多选，`A` ` B` `C` `D`选项不能为空；对于判断，`A` ` B`选项应默认为**正确**和**错误**，修改后逻辑仍应成立。

##### 请求

**请求头**

```http
POST /api/admin/problem
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "problemid": 1,
  "newdata": { 
    "type": "single",
    "tags": [
      "数学",
      "语文"
    ],
    "description": "猜猜我想的答案是哪个",
    "options":[
      "A",
      "B",
      "C",
      "D"
    ],
    "answer": "C",
    "public": true
  }
}
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义           | 类型       |
| --------- | ---- | ------ | -------------- | ---------- |
| problemid | 1    | 必有   | 题目id         | int        |
| newdata   | { }  | 必有   | 需要修改的信息 | dictionary |

其中`newdata`中的参数信息如下所示：

| 参数名      | 示例                                                         | 必要性 | 含义     | 类型    |
| ----------- | ------------------------------------------------------------ | ------ | -------- | ------- |
| type        | binary                                                       | 必选   | 题目类型 | string  |
| tags        | [ ]                                                          | 必选   | 题目标签 | list    |
| description | 《出师表》中，“先帝創業未半，而中道崩殂“中的”先帝“是指刘备。 | 必选   | 题目信息 | string  |
| options     | [ ]                                                          | 可选   | 选项     | list    |
| answer      | A                                                            | 必选   | 答案     | string  |
| public      | true                                                         | 必选   | 公开性   | boolean |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "选项格式错误"
}
```

**参数信息**

| 参数名 | 示例         | 必要性       | 含义         | 类型   |
| ------ | ------------ | ------------ | ------------ | ------ |
| ret    | 0            | 必有         | 是否正常返回 | int    |
| msg    | 选项格式错误 | ret不为0时有 | 错误信息     | string |

#### 批量公开题目

##### 请求

**请求头**

```http
POST /api/admin/problem/batch/public
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "problems": [
    1,
    2,
    4,
    3
  ]
}
```

**参数信息**

| 参数名   | 示例 | 必要性 | 含义             | 类型 |
| -------- | ---- | ------ | ---------------- | ---- |
| problems | [ ]  | 必有   | 需要公开的题目id | list |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

**参数信息**

| 参数名 | 示例     | 必要性       | 含义         | 类型   |
| ------ | -------- | ------------ | ------------ | ------ |
| ret    | 0        | 必有         | 是否正常返回 | int    |
| msg    | 登录过期 | ret不为0时有 | 错误信息     | string |

### 比赛

#### 创建比赛

管理员可以创建比赛。

时间格式请遵守格式`%Y-%m-%d %H:%M:%S`。

##### 请求

**请求头**

```http
PUT /api/admin/contest
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "name": "April Fools Day Contest 2022",
  "start": "2022-04-01 22:35:00",
  "latest": "2022-04-01 22:45:00",
  "password": "brainstorm",
  "rated": true,
  "time_limited": {
    "single": 30,
    "multiple": 40,
    "binary": 30,
    "completion": 60
  },
  "problems": [
    5,
    3,
    7,
    1
  ],
  "ordered": true
}
```

**参数信息**

| 参数名       | 示例                         | 必要性                    | 含义            | 类型       |
| ------------ | ---------------------------- | ------------------------- | --------------- | ---------- |
| name         | April Fools Day Contest 2022 | 必有                      | 比赛名          | string     |
| start        | 2022-04-01 22:35:00          | 必有                      | 开始时间        | datetime   |
| latest       | 2022-04-01 22:45:00          | 必有                      | 最晚开始时间    | datetime   |
| password     | brainstorm                   | 可选 (为空时代表比赛公开) | 比赛密码        | string     |
| rated        | true                         | 必有                      | 是否计分        | boolean    |
| time_limited | { }                          | 必有                      | 题目限时        | dictionary |
| problems     | [ ]                          | 必有                      | 题目编号        | list       |
| ordered      | true                         | 必有                      | 有序 / 随机顺序 | boolean    |

其中`time_limited`中的参数信息如下所示：

| 参数名     | 示例 | 必要性 | 含义          | 类型 |
| ---------- | ---- | ------ | ------------- | ---- |
| single     | 30   | 必有   | 单选题时限(s) | int  |
| multiple   | 40   | 必有   | 多选题时限(s) | int  |
| binary     | 30   | 必有   | 判断题时限(s) | int  |
| completion | 60   | 必有   | 填空题时限(s) | int  |

##### 响应

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

**参数信息**

| 参数名 | 示例     | 必要性       | 含义         | 类型   |
| ------ | -------- | ------------ | ------------ | ------ |
| ret    | 0        | 必有         | 是否正常返回 | int    |
| msg    | 登录过期 | ret不为0时有 | 错误信息     | string |

#### 查找比赛

具体方法请见`通用 - 查找比赛`，[点击查看](#查找所有比赛)。

#### 查看比赛信息

如果当前题目中无某类型题目，则该类型题目题目限时会显示为0。

##### 请求

**请求头**

```http
GET /api/admin/contest?contestid=1
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义   | 类型 |
| --------- | ---- | ------ | ------ | ---- |
| contestid | 1    | 必有   | 比赛id | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "info": {
    "contestid": 1,
    "name": "April Fools Day Contest 2022",
    "start": "2022-04-01 22:35:00",
    "latest": "2022-04-01 22:45:00",
    "password": "brainstorm",
    "rated": true,
    "time_limited": {
      "single": 30,
      "multiple": 40,
      "binary": 30,
      "completion": 60
    },
    "problems": [
      5,
      3,
      7,
      1
    ],
    "author": "HKvv"
  }
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型       |
| ------ | ---------- | ------------ | ------------ | ---------- |
| ret    | 0          | 必有         | 是否正常返回 | int        |
| info   | { }        | 必有         | 个人信息     | dictionary |
| msg    | 标签不存在 | ret不为0时有 | 错误信息     | string     |

其中`info`中的参数信息如下所示：

| 参数名       | 示例                         | 必要性                    | 含义         | 类型       |
| ------------ | ---------------------------- | ------------------------- | ------------ | ---------- |
| contestid    | 1                            | 必有                      | 比赛id       | int        |
| name         | April Fools Day Contest 2022 | 必有                      | 比赛名       | string     |
| start        | 2022-04-01 22:35:00          | 必有                      | 开始时间     | datetime   |
| latest       | 2022-04-01 22:45:00          | 必有                      | 最晚开始时间 | datetime   |
| password     | brainstorm                   | 必有 (为空时代表比赛公开) | 比赛密码     | string     |
| rated        | true                         | 必有                      | 是否计分     | boolean    |
| time_limited | { }                          | 必有                      | 题目限时     | dictionary |
| problems     | [ ]                          | 必有                      | 题目编号     | list       |
| author       | HKvv                         | 必有                      | 作者用户名   | string     |

其中`time_limited`中的参数信息如下所示：

| 参数名     | 示例 | 必要性 | 含义          | 类型 |
| ---------- | ---- | ------ | ------------- | ---- |
| single     | 30   | 必有   | 单选题时限(s) | int  |
| multiple   | 40   | 必有   | 多选题时限(s) | int  |
| binary     | 30   | 必有   | 判断题时限(s) | int  |
| completion | 60   | 必有   | 填空题时限(s) | int  |

#### 修改比赛信息

管理员仅能修改**自己创建的比赛**的信息，超级管理员可以修改**所有比赛**的信息，**后端**应做权限检查并检测当前时间是否早于`start`。

##### 请求

**请求头**

```http
POST /api/admin/contest
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "contestid": 1,
  "newdata": {
    "name": "April Fools Day Contest 2022",
    "start": "2022-04-01 22:30:00",
    "latest": "2022-04-01 22:40:00",
    "password": "brainstorm",
    "rated": true,
    "time_limited": {
      "single": 30,
      "multiple": 40,
      "binary": 30,
      "completion": 60
    },
    "problems": [
      5,
      3,
      7,
      1
    ],
    "ordered": true
  }
}
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义     | 类型       |
| --------- | ---- | ------ | -------- | ---------- |
| contestid | 1    | 必有   | 比赛id   | int        |
| newdata   | { }  | 必有   | 修改信息 | dictionary |

其中`newdate`中的参数信息如下所示：

| 参数名       | 示例                         | 必要性 | 含义            | 类型       |
| ------------ | ---------------------------- | ------ | --------------- | ---------- |
| name         | April Fools Day Contest 2022 | 可选   | 比赛名          | string     |
| start        | 2022-04-01 22:35:00          | 可选   | 开始时间        | datetime   |
| latest       | 2022-04-01 22:45:00          | 可选   | 最晚开始时间    | datetime   |
| password     | brainstorm                   | 可选   | 比赛密码        | string     |
| rated        | true                         | 可选   | 是否计分        | boolean    |
| time_limited | { }                          | 可选   | 题目限时        | dictionary |
| problems     | [ ]                          | 可选   | 题目编号        | list       |
| ordered      | true                         | 必有   | 有序 / 随机顺序 | boolean    |

其中`time_limited`中的参数信息如下所示：

| 参数名     | 示例 | 必要性 | 含义          | 类型 |
| ---------- | ---- | ------ | ------------- | ---- |
| single     | 30   | 可选   | 单选题时限(s) | int  |
| multiple   | 40   | 可选   | 多选题时限(s) | int  |
| binary     | 30   | 可选   | 判断题时限(s) | int  |
| completion | 60   | 可选   | 填空题时限(s) | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "选项格式错误"
}
```

**参数信息**

| 参数名 | 示例         | 必要性       | 含义         | 类型   |
| ------ | ------------ | ------------ | ------------ | ------ |
| ret    | 0            | 必有         | 是否正常返回 | int    |
| msg    | 选项格式错误 | ret不为0时有 | 错误信息     | string |

#### 删除比赛

管理员仅能删除**自己创建的比赛**，超级管理员可以删除**所有比赛**，**后端**应做权限检查并检测当前时间是否早于`start`。

##### 请求

**请求头**

```http
PUT /api/admin/contest/batch/delete
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "contests": [
    1,
    2
  ]
}
```

**参数信息**

| 参数名   | 示例 | 必要性 | 含义               | 类型 |
| -------- | ---- | ------ | ------------------ | ---- |
| contests | [ ]  | 必有   | 需要被删除的比赛id | list |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| msg    | 比赛不存在 | ret不为0时有 | 错误信息     | string |

### 比赛结果

#### 开始比赛算分

##### 请求

**请求头**

```http
POST /api/admin/contest/calculate
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "contestid": 1,
  "rated": true
}
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义     | 类型    |
| --------- | ---- | ------ | -------- | ------- |
| contestid | 1    | 必有   | 比赛id   | int     |
| rated     | true | 必有   | 是否计分 | boolean |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| msg    | 比赛不存在 | ret不为0时有 | 错误信息     | string |

#### 查看比赛排行榜

管理员可以查看排行榜所有用户的名次。

##### 请求

**请求头**

```http
GET /api/admin/contest/leaderboard?pagesize=3&pagenum=1&contestid=1
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义               | 类型   |
| --------- | ---- | ------ | ------------------ | ------ |
| pagesize  | 1    | 必有   | 每页列出的账号数量 | int    |
| pagenum   | 1    | 必有   | 获取第几页的信息   | int    |
| contestid | 1    | 必有   | 比赛id             | int    |
| keyword   | HKvv | 可选   | 用户名关键词       | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "rank": 1,
      "username": "CasanovaLLL",
      "score": 30,
      "timecost": 31,
      "correct":3,
      "before_rating": 130,
      "changed_rating": 30
    },
    {
      "rank": 2,
      "username": "HKwv",
      "score": 30,
      "timecost": 52,
      "correct":3,
      "before_rating": 120,
      "changed_rating": 20
    },
    {
      "rank": 3,
      "username": "eddie",
      "score": 30,
      "timecost": 61,
      "correct":3,
      "before_rating": 115,
      "changed_rating": 15
    }
  ],
  "total": 10
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义             | 类型   |
| ------ | ---------- | ------------ | ---------------- | ------ |
| ret    | 0          | 必有         | 是否正常返回     | int    |
| items  | [ ]        | 必有         | 排行榜信息       | list   |
| total  | 10         | 必有         | 当前条件总共人数 | int    |
| msg    | 比赛不存在 | ret不为0时有 | 错误信息         | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名        | 示例 | 必要性 | 含义             | 类型   |
| ------------- | ---- | ------ | ---------------- | ------ |
| rank          | 2    | 必有   | 排名             | int    |
| username      | HKwv | 必有   | 用户名           | string |
| score         | 30   | 必有   | 得分             | int    |
| timecost      | 52   | 必有   | 总耗时(s)        | int    |
| correct       | 3    | 必有   | 当前答对题目数量 | int    |
| before_rating | 120  | 必有   | 比赛前的分数     | int    |
| change_rating | 20   | 必有   | 分数变化         | int    |

#### 查看比赛统计

管理员可以通过此接口查看比赛统计信息。

其中，`sections`除第一个区间的左界为0外，其它都为上一个区间的右界。统计的人数的区间除第一个区间为**闭区间**外，其他都为**左开右闭**。

##### 请求

**请求头**

```http
GET /api/admin/contest/statistics?contestid=1&section=10
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义         | 类型 |
| --------- | ---- | ------ | ------------ | ---- |
| contestid | 1    | 必有   | 比赛id       | int  |
| section   | 3    | 必有   | 成绩分段数量 | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "problems": [
    {
      "problemno": 1,
      "problemid": 5,
      "correct": 9,
      "all": 10,
      "correct_rate": 0.9,
      "aver_timecost": 12.3
    },
    {
      "problemno": 2,
      "problemid": 3,
      "correct": 9,
      "all": 10,
      "correct_rate": 0.9,
      "aver_timecost": 21.3
    },
    {
      "problemno": 3,
      "problemid": 8,
      "correct": 6,
      "all": 9,
      "correct_rate": 0.666667,
      "aver_timecost": 8.5
    }
  ],
  "total": 3,
  "sections": [
    {
      "right_border": 10,
      "number": 1
    },
    {
      "right_border": 20,
      "number": 3
    },
    {
      "right_border": 30,
      "number": 6
    }
  ],
  "score": {
    "highest": 30,
    "average": 24,
    "lowest": 10
  },
  "registrants":15,
  "participants": 10
}
```

**参数信息**

| 参数名       | 示例 | 必要性 | 含义             | 类型       |
| ------------ | ---- | ------ | ---------------- | ---------- |
| ret          | 0    | 必有   | 是否正常返回     | int        |
| problems     | [ ]  | 必有   | 比赛题目统计信息 | list       |
| total        | 3    | 必有   | 总题目数量       | int        |
| sections     | [ ]  | 必有   | 各个分数区间人数 | list       |
| score        | { }  | 必有   | 分数特征         | dictionary |
| registrants  | 15   | 必有   | 注册人数         | int        |
| participants | 10   | 必有   | 实际参赛人数     | int        |

其中`problems`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名        | 示例 | 必要性 | 含义       | 类型   |
| ------------- | ---- | ------ | ---------- | ------ |
| problemno     | 1    | 必有   | 题目序号   | int    |
| problemid     | 5    | 必有   | 题目id     | int    |
| correct       | 9    | 必有   | 正确人数   | int    |
| all           | 10   | 必有   | 总提交人数 | int    |
| correct_rate  | 0.9  | 必有   | 正确率     | double |
| aver_timecost | 12.3 | 必有   | 平均用时   | double |

`sections`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名       | 示例 | 必要性 | 含义         | 类型   |
| ------------ | ---- | ------ | ------------ | ------ |
| right_border | 20   | 必有   | 分数右边界   | double |
| number       | 3    | 必有   | 分数段内人数 | int    |

其中`score`中的参数信息如下所示：

| 参数名  | 示例 | 必要性 | 含义   | 类型   |
| ------- | ---- | ------ | ------ | ------ |
| highest | 30   | 必有   | 最高分 | int    |
| average | 24   | 必有   | 平均分 | double |
| lowest  | 10   | 必有   | 最低分 | int    |

## 用户端

### 账号登录登出

#### 登录

小程序使用该接口进行登录。
前端通过获取用户登录凭证`code`并发送至后端进行登录。
后端接收后，调用 `auth.code2Session` 接口，换取用户唯一标识`OpenID` 、 用户在微信开放平台帐号下的唯一标识`UnionID`（若当前小程序已绑定到微信开放平台帐号）和会话密钥 `session_key`。

##### 请求

**请求头**

```http
POST /api/user/auth/login
Content-Type: application/json
```

**消息体**

```json
{
  "code" : "0514QOFa1cnRSC0KdfIa1Qsw7F34QOFs"
}
```

**参数信息**

| 参数名 | 示例                             | 必要性 | 含义         | 类型   |
| ------ | -------------------------------- | ------ | ------------ | ------ |
| code   | 0514QOFa1cnRSC0KdfIa1Qsw7F34QOFs | 必有   | 用户登录凭证 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=<sessionid数值>
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "username": "HKvv"
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "用户登录凭证有误"
}
```

**参数信息**

| 参数名   | 示例             | 必要性       | 含义         | 类型   |
| -------- | ---------------- | ------------ | ------------ | ------ |
| ret      | 0                | 必有         | 是否正常返回 | int    |
| username | HKvv             | ret为0是必有 | 用户名       | string |
| msg      | 用户登录凭证有误 | ret不为0时有 | 错误信息     | string |

#### 登出

##### 请求

**请求头**

```http
POST /api/user/auth/logout
Cookie: sessionid=<sessionid数值>
```

##### 响应

后端应该清除掉session_id，然后返回响应消息。

**响应头**

```http
200 OK
Content-Type: application/json
Set-Cookie: sessionid=""
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

**参数信息**

| 参数名 | 示例   | 必要性       | 含义         | 类型   |
| ------ | ------ | ------------ | ------------ | ------ |
| ret    | 0      | 必有         | 是否正常返回 | int    |
| msg    | 未登录 | ret不为0时有 | 错误信息     | string |

### 用户账号

#### 查看账号信息

用户可以使用此接口查看个人账号信息。

##### 请求

**请求头**

```http
GET /api/user/profile
Cookie: sessionid=<sessionid数值>
```

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "info": { 
    "username": "HKvv",
    "rating": 1983
  }
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型       |
| ------ | ---------- | ------------ | ------------ | ---------- |
| ret    | 0          | 必有         | 是否正常返回 | int        |
| info   | { }        | ret为0时有   | 个人信息     | dictionary |
| msg    | 标签不存在 | ret不为0时有 | 错误信息     | string     |

其中`info`中的参数信息如下所示：

| 参数名   | 示例 | 必要性 | 含义     | 类型   |
| -------- | ---- | ------ | -------- | ------ |
| username | HKvv | 必有   | 用户名   | string |
| rating   | 1983 | 必有   | 用户分数 | int    |

#### 修改账号信息

管理员和超级管理员都可以使用此接口修改个人账号信息。

修改用户名时，后端需要检测用户名是否存在。

修改密码时，前端需要验证两次输入密码是否相同。

##### 请求

**请求头**

```http
POST /api/user/profile
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "newdata": { 
    "username": "Hkvv"
  }
}
```

**参数信息**

| 参数名  | 示例 | 必要性 | 含义           | 类型       |
| ------- | ---- | ------ | -------------- | ---------- |
| newdata | { }  | 必有   | 需要修改的信息 | dictionary |

其中`newdata`中的参数信息如下所示：

| 参数名   | 示例 | 必要性 | 含义   | 类型   |
| -------- | ---- | ------ | ------ | ------ |
| username | Hkvv | 可选   | 用户名 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "用户名已存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| msg    | 用户名重复 | ret不为0时有 | 错误信息     | string |

### 练习

#### 自主组卷

用户可以使用此接口获取自主组卷。注意当`tag`有**多个**时，应使用加号`+`将多个标签分隔。题目数量不应超过50题。

##### 请求

**请求头**

```http
GET /api/user/exercise/collect?tag=数学+英语&amount=20
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名 | 示例      | 必要性 | 含义     | 类型   |
| ------ | --------- | ------ | -------- | ------ |
| tag    | 数学+英语 | 必有   | 标签     | string |
| amount | 5         | 必有   | 题目数量 | int    |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "problems": [
    5,
    7,
    3,
    1,
    20
  ],
  "total": 5
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "标签不存在"
}
```

**参数信息**

| 参数名   | 示例       | 必要性       | 含义                 | 类型   |
| -------- | ---------- | ------------ | -------------------- | ------ |
| ret      | 0          | 必有         | 是否正常返回         | int    |
| problems | [ ]        | ret为0时有   | 得到的每个题目的编号 | list   |
| total    | 5          | ret为0时有   | 总题数               | int    |
| msg      | 标签不存在 | ret不为0时有 | 错误信息             | string |

#### 获取题面

用户可以使用此接口获取题面.

##### 请求

**请求头**

```http
GET /api/user/exercise/problem?id=153
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名 | 示例 | 必要性 | 含义   | 类型 |
| ------ | ---- | ------ | ------ | ---- |
| id     | 153  | 必有   | 题目id | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "type": "multiple",
  "description": "请选出所有OJ",
  "options":[
    "Codeforces",
    "DBforces",
    "accoding",
    "wacoding"
  ]
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "题目不存在" / "题目未公开"
}
```

**参数信息**

| 参数名      | 示例                                               | 必要性 | 含义     | 类型   |
| ----------- | -------------------------------------------------- | ------ | -------- | ------ |
| type        | "single" / "multiple" / "binary" / "completion"    | 必有   | 题目类型 | string |
| description | "请选出所有OJ"                                     | 必有   | 题面     | string |
| options     | ["Codeforces", "DBforces", "accoding", "wacoding"] | 必有   | 选项     | list   |

#### 作答验证

用户可以使用此接口验证答案并获取正确答案.

##### 请求

**请求头**

```http
GET /api/user/exercise/problem/check?problem_id=24&user_answer=C
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名      | 示例 | 必要性 | 含义     | 类型   |
| ----------- | ---- | ------ | -------- | ------ |
| problem_id  | 24   | 必有   | 题目id   | int    |
| user_answer | "C"  | 必有   | 用户答案 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "correct": 1,
  "answer": "动态规划/dp/DP, 最小生成树/MST"
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "题目不存在" / "题目未公开"
}
```

**参数信息**

| 参数名  | 示例  | 必要性 | 含义          | 类型          |
| ------- | ----- | ------ | ------------- | ------------- |
| correct | 0     | 必有   | 作答正确则为1 | int(boolean?) |
| answer  | "ACD" | 必有   | 正确答案      | string        |

### 比赛

#### 注册比赛

##### 请求

**请求头**

```http
POST /api/user/contest/register
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**消息体**

```json
{
  "contestid": 1,
  "password": "123456"
}
```

**参数信息**

| 参数名    | 示例   | 必要性           | 含义           | 类型   |
| --------- | ------ | ---------------- | -------------- | ------ |
| contestid | 1      | 必有             | 用户注册比赛id | int    |
| password  | 123456 | 比赛不公开时必有 | 比赛密码       | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛不存在"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| msg    | 比赛不存在 | ret不为0时有 | 错误信息     | string |

#### 查询所有参加比赛

用户可以使用此接口查询自己所有注册的比赛。

##### 请求

**请求头**

```http
GET /api/user/contest/records?pagesize=1&pagenum=1&keyword=April
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**参数信息**

| 参数名   | 示例  | 必要性 | 含义               | 类型   |
| -------- | ----- | ------ | ------------------ | ------ |
| pagesize | 1     | 必有   | 每页列出的账号数量 | int    |
| pagenum  | 1     | 必有   | 获取第几页的信息   | int    |
| keyword  | April | 可选   | 比赛名关键词       | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "contestid": 1,
      "name": "April Fools Day Contest 2022",
      "start": "2022-04-01 22:35:00",
      "latest": "2022-04-01 22:45:00",
      "public": true,
      "rated": true,
      "status": "未开始",
      "time_limited": {
        "single": 30,
        "multiple": 40,
        "binary": 30,
        "completion": 60
      },
      "author": "Agnimandur"
    }
  ],
  "total": 3
}
```

**参数信息**

| 参数名 | 示例 | 必要性       | 含义                           | 类型   |
| ------ | ---- | ------------ | ------------------------------ | ------ |
| ret    | 0    | 必有         | 是否正常返回                   | int    |
| items  | [ ]  | 必有         | 比赛信息                       | list   |
| total  | 3    | 必有         | 当前要求下，总共拥有的比赛数量 | int    |
| msg    |      | ret不为0时有 | 错误信息                       | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名       | 示例                                  | 必要性 | 含义         | 类型       |
| ------------ | ------------------------------------- | ------ | ------------ | ---------- |
| contestid    | 1                                     | 必有   | 比赛id       | int        |
| name         | April Fools Day Contest 2022          | 必有   | 比赛名       | string     |
| start        | 2022-04-01 22:35:00                   | 必有   | 开始时间     | datetime   |
| latest       | 2022-04-01 22:45:00                   | 必有   | 最晚开始时间 | datetime   |
| public       | true                                  | 必有   | 比赛是否公开 | boolen     |
| rated        | true                                  | 必有   | 是否计分     | boolean    |
| status       | 比赛中 / 未开始 / 已结束 / 待公布成绩 | 必有   | 比赛状态     | string     |
| time_limited | { }                                   | 必有   | 题目限时     | dictionary |
| author       | HKvv                                  | 必有   | 作者用户名   | string     |

其中`time_limited`中的参数信息如下所示：

| 参数名     | 示例 | 必要性 | 含义          | 类型 |
| ---------- | ---- | ------ | ------------- | ---- |
| single     | 30   | 必有   | 单选题时限(s) | int  |
| multiple   | 40   | 必有   | 多选题时限(s) | int  |
| binary     | 30   | 必有   | 判断题时限(s) | int  |
| completion | 60   | 必有   | 填空题时限(s) | int  |

### 比赛答题

#### 开始比赛

##### 请求

**请求头**

```http
GET /api/user/contest/start?contestid=1
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义   | 类型 |
| --------- | ---- | ------ | ------ | ---- |
| contestid | 1    | 必有   | 比赛id | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "total": 10
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛未开始"
}
```

**参数信息**

| 参数名 | 示例       | 必要性       | 含义         | 类型   |
| ------ | ---------- | ------------ | ------------ | ------ |
| ret    | 0          | 必有         | 是否正常返回 | int    |
| total  | 10         | 必有         | 题目总数     | int    |
| msg    | 比赛未开始 | ret不为0时有 | 错误信息     | string |

#### 获取题面

用户可以使用此接口获取题面。

##### 请求

**请求头**

```http
GET /api/user/contest/problem?contestid=1
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义   | 类型 |
| --------- | ---- | ------ | ------ | ---- |
| contestid | 1    | 必有   | 比赛id | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "type": "multiple",
  "description": "请选出所有OJ",
  "options":[
    "Codeforces",
    "DBforces",
    "accoding",
    "wacoding"
  ],
  "problemnum": 5,
  "time": 30
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛完成"
}
```

**参数信息**

| 参数名      | 示例                                               | 必要性 | 含义         | 类型   |
| ----------- | -------------------------------------------------- | ------ | ------------ | ------ |
| ret         | 0                                                  | 必有   | 是否正常返回 | int    |
| type        | "single" / "multiple" / "binary" / "completion"    | 必有   | 题目类型     | string |
| description | "请选出所有OJ"                                     | 必有   | 题面         | string |
| options     | ["Codeforces", "DBforces", "accoding", "wacoding"] | 必有   | 选项         | list   |
| problemnum  | 5                                                  | 必有   | 题目序号     | int    |
| time        | 30                                                 | 必有   | 题目时限(s)  | int    |

#### 提交答案

用户可以使用此接口提交答案并获取下一题。

##### 请求

**请求头**

```http
GET /api/user/contest/problem/submit?contestid=1&problemnum=6&user_answer=C
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名      | 示例 | 必要性 | 含义     | 类型   |
| ----------- | ---- | ------ | -------- | ------ |
| contestid   | 1    | 必有   | 比赛id   | int    |
| problemnum  | 6    | 必有   | 题目序号 | int    |
| user_answer | C    | 必有   | 用户答案 | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
}
```

**参数信息**

| 参数名 | 示例     | 必要性       | 含义         | 类型   |
| ------ | -------- | ------------ | ------------ | ------ |
| ret    | 0        | 必有         | 是否正常返回 | int    |
| msg    | 比赛完成 | ret不为0时有 | 错误信息     | string |

### 比赛结果

#### 查询答题记录

##### 请求

**请求头**

```http
GET /api/user/contest/record?contestid=1
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义   | 类型 |
| --------- | ---- | ------ | ------ | ---- |
| contestid | 1    | 必有   | 比赛id | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "problemno": 1,
      "problemid": 5,
      "correct": true,
      "submitted": "A",
      "answer": "A"
    },
    {
      "problemno": 2,
      "problemid": 3,
      "correct": true,
      "submitted": "BCD",
      "answer": "BCD"
    },
    {
      "problemno": 3,
      "problemid": 8,
      "correct": false,
      "submitted": "HKwv",
      "answer": "HKvv"
    }
  ],
  "total": 3,
  "score": 20,
  "timecost": 15,
  "rank": 10,
  "before_rating": 100,
  "changed_rating": -15
}
```

**参数信息**

| 参数名        | 示例 | 必要性       | 含义         | 类型   |
| ------------- | ---- | ------------ | ------------ | ------ |
| ret           | 0    | 必有         | 是否正常返回 | int    |
| items         | [ ]  | 必有         | 比赛答题记录 | list   |
| total         | 3    | 必有         | 总题目量     | int    |
| score         | 20   | 必有         | 得分         | int    |
| timecost      | 15   | 必有         | 总耗时(s)    | int    |
| rank          | 10   | 必有         | 比赛排名     | int    |
| before_rating | 100  | 必有         | 比赛前的分数 | int    |
| change_rating | -15  | 必有         | 分数变化     | int    |
| msg           |      | ret不为0时有 | 错误信息     | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名    | 示例 | 必要性 | 含义         | 类型    |
| --------- | ---- | ------ | ------------ | ------- |
| problemno | 1    | 必有   | 题目序号     | int     |
| problemid | 5    | 必有   | 题目id       | int     |
| correct   | true | 必有   | 是否正确     | boolean |
| submitted | A    | 必有   | 当时提交答案 | string  |
| answer    | A    | 必有   | 正确答案     | string  |

#### 查看比赛排行榜

用户仅能查看排行榜**前三名**和**自己**的结果。

排行榜按照分数`score`排名，如果分数相同，则按总耗时`timecost`排名。

##### 请求

**请求头**

```http
GET /api/user/contest/leaderboard?contestid=1
Cookie: sessionid=<sessionid数值>
Content-Type: application/json
```

**参数信息**

| 参数名    | 示例 | 必要性 | 含义   | 类型 |
| --------- | ---- | ------ | ------ | ---- |
| contestid | 1    | 必有   | 比赛id | int  |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "top3": [
    {
      "rank": 1,
      "username": "CasanovaLLL",
      "score": 30,
      "timecost": 31,
      "correct":3,
      "before_rating": 130,
      "changed_rating": 30
    },
    {
      "rank": 2,
      "username": "HKwv",
      "score": 30,
      "timecost": 52,
      "correct":3,
      "before_rating": 120,
      "changed_rating": 20
    },
    {
      "rank": 3,
      "username": "eddie",
      "score": 30,
      "timecost": 61,
      "correct":3,
      "before_rating": 115,
      "changed_rating": 15
    }
  ],
  "user_rank": {
    "rank": 10,
    "username": "HKvv",
    "score": 20,
    "correct":2,
    "timecost": 15,
    "before_rating": 85,
    "changed_rating": -15
  }
}
```

异常返回(ret ≠ 0):

```json
{
  "ret": 1,
  "msg": "比赛不存在"
}
```

**参数信息**

| 参数名    | 示例       | 必要性       | 含义           | 类型       |
| --------- | ---------- | ------------ | -------------- | ---------- |
| ret       | 0          | 必有         | 是否正常返回   | int        |
| top3      | [ ]        | 必有         | 排行榜信息     | list       |
| user_rank | { }        | 必有         | 用户排名信息   | dictionary |
| total     | 3          | 必有         | 排行榜显示人数 | int        |
| msg       | 比赛不存在 | ret不为0时有 | 错误信息       | string     |

其中`top3`是包含多个查找结果的列表，每个结果的参数信息以及`user_rank`的参数信息如下所示：

| 参数名        | 示例 | 必要性 | 含义             | 类型   |
| ------------- | ---- | ------ | ---------------- | ------ |
| rank          | 2    | 必有   | 排名             | int    |
| username      | HKwv | 必有   | 用户名           | string |
| score         | 30   | 必有   | 得分             | int    |
| timecost      | 52   | 必有   | 总耗时(s)        | int    |
| correct       | 3    | 必有   | 当前答对题目数量 | int    |
| before_rating | 120  | 必有   | 比赛前的分数     | int    |
| change_rating | 20   | 必有   | 分数变化         | int    |

## 通用

### 查看标签

#### 查看所有标签

可以使用此接口获取所有标签.

##### 请求

**请求头**

```http
GET /api/general/tag/list
```

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "tags":[
    "哲学",
    "理学",
    "文学"
  ],
  "total": 3
}
```

**参数信息**

| 参数名 | 示例                     | 必要性 | 含义     | 类型 |
| ------ | ------------------------ | ------ | -------- | ---- |
| tags   | ["历史", "理学", "文学"] | 必有   | 所有标签 | list |
| total  | 3                        | 必有   | 标签数量 | int  |

### 查看比赛

#### 查找所有比赛

比赛按照**距离当前时间长短排序**。

##### 请求

**请求头**

```http
GET /api/general/contest/list?pagesize=1&pagenum=1&type=upcoming&keyword=April
Cookie: sessionid=<sessionid数值>
```

**参数信息**

| 参数名   | 示例                                               | 必要性 | 含义                                    | 类型   |
| -------- | -------------------------------------------------- | ------ | --------------------------------------- | ------ |
| pagesize | 1                                                  | 必有   | 每页列出的账号数量                      | int    |
| pagenum  | 1                                                  | 必有   | 获取第几页的信息                        | int    |
| type     | upcoming / history / in_progress / to_be_announced | 可选   | 获取未来/历史/正在进行/即将公布成绩比赛 | string |
| keyword  | April                                              | 可选   | 比赛名关键词                            | string |

##### 响应

**响应头**

```http
200 OK
Content-Type: application/json
```

**消息体**

正常返回(ret = 0):

```json
{
  "ret": 0,
  "items": [
    {
      "contestid": 1,
      "name": "April Fools Day Contest 2022",
      "start": "2022-04-01 22:35:00",
      "latest": "2022-04-01 22:45:00",
      "public": true,
      "rated": true,
      "status": "比赛中",
      "time_limited": {
        "single": 30,
        "multiple": 40,
        "binary": 30,
        "completion": 60
      },
      "author": "Agnimandur",
      "register_num": 163
    }
  ],
  "total": 3
}
```

**参数信息**

| 参数名 | 示例 | 必要性       | 含义                           | 类型   |
| ------ | ---- | ------------ | ------------------------------ | ------ |
| ret    | 0    | 必有         | 是否正常返回                   | int    |
| items  | [ ]  | 必有         | 比赛信息                       | list   |
| total  | 3    | 必有         | 当前要求下，总共拥有的比赛数量 | int    |
| msg    |      | ret不为0时有 | 错误信息                       | string |

其中`items`是包含多个查找结果的列表，每个结果的参数信息如下所示：

| 参数名       | 示例                                  | 必要性 | 含义         | 类型       |
| ------------ | ------------------------------------- | ------ | ------------ | ---------- |
| contestid    | 1                                     | 必有   | 比赛id       | int        |
| name         | April Fools Day Contest 2022          | 必有   | 比赛名       | string     |
| start        | 2022-04-01 22:35:00                   | 必有   | 开始时间     | datetime   |
| latest       | 2022-04-01 22:45:00                   | 必有   | 最晚开始时间 | datetime   |
| public       | true                                  | 必有   | 比赛是否公开 | boolen     |
| rated        | true                                  | 必有   | 是否计分     | boolean    |
| status       | 比赛中 / 未开始 / 已结束 / 待公布成绩 | 必有   | 比赛状态     | string     |
| time_limited | { }                                   | 必有   | 题目限时     | dictionary |
| author       | HKvv                                  | 必有   | 作者用户名   | string     |
| register_num | 163                                   | 必有   | 注册人数     | int        |

其中`time_limited`中的参数信息如下所示：

| 参数名     | 示例 | 必要性 | 含义          | 类型 |
| ---------- | ---- | ------ | ------------- | ---- |
| single     | 30   | 必有   | 单选题时限(s) | int  |
| multiple   | 40   | 必有   | 多选题时限(s) | int  |
| binary     | 30   | 必有   | 判断题时限(s) | int  |
| completion | 60   | 必有   | 填空题时限(s) | int  |

