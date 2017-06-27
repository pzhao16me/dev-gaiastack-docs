# container接口

**path 参数说明**

> projectName: 业务名

> appName: app名

> instanceName: 实例名

> containerName: 容器名

## 查询容器

`GET /v2/containers/{projectName}/{instanceName}/{containerName}`

**返回**

```json
{
  "name": "gaia",
  "image": "nginx:1.7.9",
  "command": null,
  "args": null,
  "ports": null,
  "env": null,
  "resources": {
    "requests": {
      "cpu": "100m",
      "memory": "256Mi"
    }
  },
  "volumeMounts": [
    {
      "name": "default-token-an6zt",
      "readOnly": true,
      "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount"
    }
  ],
  "state": "Running",
  "ready": true,
  "containerId": "docker://47ff8fc43a0e085f357266e732ebd47aca18734a693e5b4d23ec36d0f5874550",
  "restartCount": 0
}
```

## 容器列表

`GET /v2/containers/{projectName}/{instanceName}/lists`

**请求参数**

* size: 单页大小 // optional, default:10

* page: 页数 // optional, default:0

**返回**

```json
{
  "content": [
    {
      "name": "gaia",
      "image": "nginx:1.7.9",
      "command": null,
      "args": null,
      "ports": null,
      "env": null,
      "resources": {
        "requests": {
          "cpu": "100m",
          "memory": "256Mi"
        }
      },
      "volumeMounts": [
        {
          "name": "default-token-an6zt",
          "readOnly": true,
          "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount"
        }
      ],
      "state": "Running",
      "ready": true,
      "containerId": "docker://47ff8fc43a0e085f357266e732ebd47aca18734a693e5b4d23ec36d0f5874550",
      "restartCount": 0
    }
  ],
  "last": true,
  "totalElements": 1,
  "totalPages": 1,
  "first": true,
  "numberOfElements": 1,
  "size": 10,
  "number": 0
}
```
