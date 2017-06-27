# 名字服务接口

**path 参数说明**

> projectName: 业务名

> appName: app名

## 获取app名字服务

`GET /v2/registry/{projectName}/{appName}`


**返回**

```json
{
  "deploy-test": {
    "1": {
      "80": "10.0.2.15:31790"
    }
  }
}
```

## 实例端口映射信息

`GET /v2/instances/{projectName}/{instanceName}/portmapping?instance_id={instance_id}`

**请求参数**

* instance_id: 实例ID

* size: 单页大小 // optional, default:10

* page: 页数 // optional, default:0

**返回**

```json
{
  "content": [
    {
      "name": "",
      "hostPort": 0,
      "containerPort": 80,
      "hostIP": "",
      "protocol": "TCP"
    }
  ],
  "last": true,
  "totalElements": 0,
  "totalPages": 0,
  "first": true,
  "numberOfElements": 0,
  "size": 10,
  "number": 0
}
```
