# 认证接口

> 系统的所有API接口都需要通过权限校验，校验方式为在请求的header中加入`x-auth-token: {token}`

> token 通过以下接口获取。目前token的有效时间是10小时，过期需要重新获取。

## 创建token

`POST /v2/auth/tokens`

**请求body**

```json
{
  "identity": {
    "methods": [
      "password"
    ],
    "password": {
      "user": {
        "name": "admin",
        "domain": {
          "id": "default"
        },
        "password": "admin"
      }
    }
  }
}
```

**返回**

```json
{
  "id": "0a72092f28434e7a8cb508b64dda439f",
  "methods": [
    "password"
  ],
  "domain": null,
  "project": null,
  "user": {
    "id": "0b8b2c4543a04fc29f6d8c9780c558b7",
    "name": "admin"
  },
  "roles": null,
  "catalog": null,
  "expires_at": 1473854977281,
  "issued_at": 1473818977313,
  "ProjectsSummary": [
    {
      "id": "d739b5ef381e42acab9e708e01d93256",
      "name": "default",
      "roles": [
        {
          "id": "a3f12c97c52d4855bd03f69fb1461530",
          "name": "user"
        }
      ]
    }
  ]
}
```
