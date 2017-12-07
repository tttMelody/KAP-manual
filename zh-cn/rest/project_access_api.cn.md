## Project Access REST API

> **提示**
>
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
>


* [获得项目下的用户权限](#获得项目下的用户权限)
* [赋予用户项目权限](#赋予用户项目权限)
* [修改用户项目权限](#修改用户项目权限)
* [收回用户项目权限](#收回用户项目权限)

http://localhost:8080/kylin/api/access/ProjectInstance/2fbca32a-a33e-4b69-83dd-0bb8b1f8c91b

### 获得项目下的用户权限
`请求方式 GET`

`访问路径 http://host:port/kylin/api/access/{type}/{uuid}`

#### 路径变量
* type - `必选` `string` 目前来说, type 只能是ProjectInstance
* uuid - `必选` `string` project 的 ID

#### 请求示例
`请求路径:http://host:port/kylin/api/access/ProjectInstance/2fbca32a-a33e-4b69-83dd-0bb8b1f8c91b`

#### 响应示例
```json
{
  "code": "000",
  "data": [
    {
      "permission": {
        "mask": 16,
        "pattern": "...........................A...."
      },
      "id": 0,
      "sid": {
        "principal": "ADMIN"
      },
      "granting": true
    }
  ],
  "msg": ""
}
```

### 赋予用户项目权限
`请求方式 POST`

`访问路径 http://host:port/kylin/api/access/{type}/{uuid}`

#### 路径变量
* type - `必选` `string` 目前来说, type 只能是ProjectInstance
* uuid - `必选` `string` project 的 ID

#### 请求主体
* permission - `必选` `string` 在 project 上的权限
* principal - `必选` `string` principal 为 true
* sid - `必选` `string` 用户名

#### 请求示例
`请求路径:http://host:port/kylin/api/access/ProjectInstance/2fbca32a-a33e-4b69-83dd-0bb8b1f8c91b`

```
request body:
{permission: "READ", principal: true, sid: "MODELER"}
```

#### 响应示例
```json
{
  "code": "000",
  "data": [
    {
      "permission": {
        "mask": 16,
        "pattern": "...........................A...."
      },
      "id": 0,
      "sid": {
        "principal": "ADMIN"
      },
      "granting": true
    },
    {
      "permission": {
        "mask": 1,
        "pattern": "...............................R"
      },
      "id": 1,
      "sid": {
        "principal": "MODELER"
      },
      "granting": true
    }
  ],
  "msg": ""
}
```

### 修改用户项目权限
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/access/{type}/{uuid}`

#### 路径变量
* type - `必选` `string` 目前来说, type 只能是ProjectInstance
* uuid - `必选` `string` project 的 ID

#### 请求主体
* permission - `必选` `string` 在 project 上的权限
* principal - `必选` `string` principal 为 true
* sid - `必选` `string` 用户名

#### 请求示例
`请求路径:http://host:port/kylin/api/access/ProjectInstance/2fbca32a-a33e-4b69-83dd-0bb8b1f8c91b`

`request body:{permission: "READ", principal: true, sid: "MODELER"}`

#### 响应示例
```json
{
  "code": "000",
  "data": [
    {
      "permission": {
        "mask": 16,
        "pattern": "...........................A...."
      },
      "id": 0,
      "sid": {
        "principal": "ADMIN"
      },
      "granting": true
    },
    {
      "permission": {
        "mask": 1,
        "pattern": "...............................R"
      },
      "id": 1,
      "sid": {
        "principal": "MODELER"
      },
      "granting": true
    }
  ],
  "msg": ""
}
```

### 收回用户项目权限
`请求方式 DELETE`

`访问路径 http://host:port/kylin/api/access/{type}/{uuid}`

#### 路径变量
* type - `必选` `string` 目前来说, type 只能是ProjectInstance
* uuid - `必选` `string` project 的 ID

#### 请求主体
* accessEntryId - `必选` `string` ACL 的序号
* sid - `必选` `string` 用户名

#### 请求示例
`请求路径:http://host:port/kylin/api/access/ProjectInstance/2fbca32a-a33e-4b69-83dd-0bb8b1f8c91b?accessEntryId=1&sid=MODELER`

`request body:{permission: "READ", principal: true, sid: "MODELER"}`

#### 响应示例
```json
{
  "code": "000",
  "data": [
    {
      "permission": {
        "mask": 16,
        "pattern": "...........................A...."
      },
      "id": 0,
      "sid": {
        "principal": "ADMIN"
      },
      "granting": true
    },
    {
      "permission": {
        "mask": 1,
        "pattern": "...............................R"
      },
      "id": 1,
      "sid": {
        "principal": "MODELER"
      },
      "granting": true
    }
  ],
  "msg": ""
}
```