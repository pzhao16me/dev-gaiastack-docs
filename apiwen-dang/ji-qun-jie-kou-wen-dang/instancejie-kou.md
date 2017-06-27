# instance接口

**path 参数说明**

> projectName: 业务名

> appName: app名

> instanceName: 实例名

> containerName: 容器名

> fileName: 文件名


## 实例信息

`GET /v2/instances/{projectName}/{instanceName}?instance_id={instance_id}`

**请求参数**

* instance_id: 实例ID


**返回**

```json
{
  "ID": "bd92470c6c3f6616ce103ff34890d77d7ce7730560f15372bf2cc929819b9777",
  "Name": "gaia-4058157996-9o8vv",
  "AppID": "e5da270d6cb25901c311aa35a50ce877dc2c49c3fe8c2831b13513db5104ad18",
  "AppName": "gaia",
  "ProjectID": "abcdefg",
  "ProjectName": "default",
  "Username": "thomassong",
  "Image": "nginx:1.7.9",
  "Cmd": null,
  "Environments": null,
  "Ports": null,
  "Resource": {
    "cpu": "0.1",
    "mem": "256Mi"
  },
  "NodeName": "127.0.0.1",
  "PodIP": "172.17.0.3",
  "HostIP": "127.0.0.1",
  "State": "Running",
  "ContainerNum": 1,
  "ContainerSummary": {
    "Running": 1
  },
  "CreatedAt": "2016-10-04T12:57:14Z",
  "UpdatedAt": "2016-10-04T12:57:17Z"
}
```

## 实例列表

`GET /v2/instances/{projectName}/{appName}/lists?app_id={app_id}`

**请求参数**

* app_id: appID

* size: 单页大小 // optional, default:10

* page: 页数 // optional, default:0

**返回**

```json
{
  "content": [
    {
      "ID": "75429ffed10d4a368571eee18cd3af61d2933c64e68db22e3db6e15bf0a77872",
      "Name": "gaia-471438139-4eytv",
      "AppID": "a19a3b893d3c6655949c27de65da7152a552041b05d51f7cc952ba0956d44a0d",
      "AppName": "gaia",
      "ProjectID": "abcdefg",
      "ProjectName": "default",
      "Username": "testUserName",
      "Image": "nginx:1.7.9",
      "Cmd": null,
      "Environments": null,
      "Ports": null,
      "Resource": {
        "cpu": "",
        "mem": ""
      },
      "NodeName": "127.0.0.1",
      "PodIP": "172.17.0.3",
      "HostIP": "127.0.0.1",
      "State": "Running",
      "ContainerNum": 1,
      "ContainerSummary": {
        "Running": 1
      },
      "CreatedAt": "2016-10-09T03:19:58Z",
      "UpdatedAt": "2016-10-09T03:20:02Z"
    },
    {
      "ID": "bb9c07f7ae15181546670f43397cdd101253b905c4d50a3f14abcda74062134d",
      "Name": "gaia-471438139-a4q22",
      "AppID": "a19a3b893d3c6655949c27de65da7152a552041b05d51f7cc952ba0956d44a0d",
      "AppName": "gaia",
      "ProjectID": "abcdefg",
      "ProjectName": "default",
      "Username": "testUserName",
      "Image": "nginx:1.7.9",
      "Cmd": null,
      "Environments": null,
      "Ports": null,
      "Resource": {
        "cpu": "",
        "mem": ""
      },
      "NodeName": "127.0.0.1",
      "PodIP": "172.17.0.2",
      "HostIP": "127.0.0.1",
      "State": "Running",
      "ContainerNum": 1,
      "ContainerSummary": {
        "Running": 1
      },
      "CreatedAt": "2016-10-09T03:19:58Z",
      "UpdatedAt": "2016-10-09T03:20:01Z"
    }
  ],
  "last": true,
  "totalElements": 2,
  "totalPages": 1,
  "first": true,
  "numberOfElements": 2,
  "size": 10,
  "number": 0
}
```

