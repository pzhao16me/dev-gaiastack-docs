# compose编排接口

**path 参数说明**

> projectName: 业务名

> composeName: compose名

## 创建compose编排

`POST /v2/composes/{projectName}`

**请求body**

```json
{
  "stackInstanceName": "compose",
  "content": "c1:\n    image: docker.oa.com:8080/public/helloworld\n    replicas: 2\n    mem_limit: 256m\n    cpu_shares: 1\n    environment:\n        CONTAINER_NAME: c1\n        app_NAME: app-c1\n    links:\n        - c2\n        - c3\nc2:\n    image: docker.oa.com:8080/public/helloworld\n    replicas: 1\n    mem_limit: 256m\n    cpu_shares: 1\n    environment:\n        CONTAINER_NAME: c2\n        app_NAME: app-c2\nc3:\n    image: docker.oa.com:8080/public/helloworld\n    replicas: 3\n    mem_limit: 128m\n    cpu_shares: 1\n    environment:\n        CONTAINER_NAME: c3\n        app_NAME: app-c3\n"
}
```

**返回**

```json
{
  "Code": 0,
  "Message": ""
}
```

## 获取compose编排

`GET /v2/composes/{projectName}/{composeName}`

**返回**

```json
{
  "id": "0f848c4aacecea4718089c1ca22af842d625760ed9c2491a94a35e1eab9c297d",
  "stackName": "compose",
  "stackContent": "c1:\n    image: docker.oa.com:8080/public/helloworld\n    replicas: 2\n    mem_limit: 256m\n    cpu_shares: 1\n    environment:\n        CONTAINER_NAME: c1\n        app_NAME: app-c1\n    links:\n        - c2\n        - c3\nc2:\n    image: docker.oa.com:8080/public/helloworld\n    replicas: 1\n    mem_limit: 256m\n    cpu_shares: 1\n    environment:\n        CONTAINER_NAME: c2\n        app_NAME: app-c2\nc3:\n    image: docker.oa.com:8080/public/helloworld\n    replicas: 3\n    mem_limit: 128m\n    cpu_shares: 1\n    environment:\n        CONTAINER_NAME: c3\n        app_NAME: app-c3\n",
  "appNum": 3,
  "instanceNum": 6,
  "projectName": "default",
  "createAt": "2016-10-17T14:07:14+08:00",
  "lastUpdate": "2016-10-17T14:07:19+08:00",
  "status": "RUNNING",
  "userName": "testUserName",
  "images": "docker.oa.com:8080/public/helloworld:latest, docker.oa.com:8080/public/helloworld:latest, docker.oa.com:8080/public/helloworld:latest",
  "stackApps": [
    {
      "AppID": "02263cb535e1f31dd090ca540834913ec3c2fc6d794641712d205e21544fdd35",
      "Cmd": "",
      "Created": "2016-10-17T14:07:16+08:00",
      "UpdatedAt": "2016-10-17T14:07:27+08:00",
      "DockerDataDir": "",
      "DockerLogDir": "",
      "Environments": null,
      "Image": "docker.oa.com:80/docker.oa.com:8080/public/helloworld:latest",
      "InstanceNum": 2,
      "InstanceSummary": "{\"PENDING\":2}",
      "Name": "compose-c1",
      "PortsInfo": null,
      "ProjectName": "default",
      "Status": "RUNNING",
      "Username": "testUserName",
      "ResInfo": {
        "cpu": "",
        "mem": ""
      },
      "Volumes": "",
      "NetworkType": 0,
      "StackInstanceID": "0f848c4aacecea4718089c1ca22af842d625760ed9c2491a94a35e1eab9c297d"
    },
    {
      "AppID": "73c6a2968c07117f374ca66234445a31d051d40b522f30502e39dcaaa4159622",
      "Cmd": "",
      "Created": "2016-10-17T14:07:17+08:00",
      "UpdatedAt": "2016-10-17T14:07:24+08:00",
      "DockerDataDir": "",
      "DockerLogDir": "",
      "Environments": null,
      "Image": "docker.oa.com:80/docker.oa.com:8080/public/helloworld:latest",
      "InstanceNum": 1,
      "InstanceSummary": "{\"PENDING\":1}",
      "Name": "compose-c2",
      "PortsInfo": null,
      "ProjectName": "default",
      "Status": "RUNNING",
      "Username": "testUserName",
      "ResInfo": {
        "cpu": "",
        "mem": ""
      },
      "Volumes": "",
      "NetworkType": 0,
      "StackInstanceID": "0f848c4aacecea4718089c1ca22af842d625760ed9c2491a94a35e1eab9c297d"
    },
    {
      "AppID": "cfbb95973ee33147b64753e9cdebc56f278f841c5f2f213787a97b56f695ea6a",
      "Cmd": "",
      "Created": "2016-10-17T14:07:18+08:00",
      "UpdatedAt": "2016-10-17T14:07:30+08:00",
      "DockerDataDir": "",
      "DockerLogDir": "",
      "Environments": null,
      "Image": "docker.oa.com:80/docker.oa.com:8080/public/helloworld:latest",
      "InstanceNum": 3,
      "InstanceSummary": "{\"PENDING\":2}",
      "Name": "compose-c3",
      "PortsInfo": null,
      "ProjectName": "default",
      "Status": "RUNNING",
      "Username": "testUserName",
      "ResInfo": {
        "cpu": "",
        "mem": ""
      },
      "Volumes": "",
      "NetworkType": 0,
      "StackInstanceID": "0f848c4aacecea4718089c1ca22af842d625760ed9c2491a94a35e1eab9c297d"
    }
  ],
  "relations": {
    "c1": {
      "Image": "docker.oa.com:8080/public/helloworld:latest",
      "image_pic": "/v2/registry/imagePic/public/helloworld",
      "Command": "",
      "replicas": 2,
      "Links": [
        "c2",
        "c3"
      ],
      "Environment": {
        "CONTAINER_NAME": "c1",
        "app_NAME": "app-c1"
      },
      "Volumes": null,
      "MemLimit": "256m",
      "CPUShares": "1"
    },
    "c2": {
      "Image": "docker.oa.com:8080/public/helloworld:latest",
      "image_pic": "/v2/registry/imagePic/public/helloworld",
      "Command": "",
      "replicas": 1,
      "Links": null,
      "Environment": {
        "CONTAINER_NAME": "c2",
        "app_NAME": "app-c2"
      },
      "Volumes": null,
      "MemLimit": "256m",
      "CPUShares": "1"
    },
    "c3": {
      "Image": "docker.oa.com:8080/public/helloworld:latest",
      "image_pic": "/v2/registry/imagePic/public/helloworld",
      "Command": "",
      "replicas": 3,
      "Links": null,
      "Environment": {
        "CONTAINER_NAME": "c3",
        "app_NAME": "app-c3"
      },
      "Volumes": null,
      "MemLimit": "128m",
      "CPUShares": "1"
    }
  }
}
```

## 删除compose编排

`DELETE /v2/composes/{projectName}/{composeName}`

**返回**

```json
{
  "Code": 0,
  "Message": ""
}
```
