# app接口

**path 参数说明**

> projectName: 业务名
> appName: app名


## 创建app

`POST /v2/applications/{projectName}`

**请求body**

```json
{
  "name": "demoapp",
  "imageRef": "nginx:1.7.9",
  "resources": {
    "cpu": "0.1",
    "mem": "256Mi"
  },
  "instanceNum": 2,
  "cmd": "sleep 10000",
  "dockerEnv": [
    {
      "name": "PATH1",
      "value": "/bin"
    },
    {
      "name": "PATH2",
      "value": "/usr/bin"
    }
  ],
  "portMappings": [
    {
      "protocol": "tcp",
      "containerPort": 8088
    },
    {
      "protocol": "udp",
      "containerPort": 9999
    }
  ],
  "dockerLogDir": "/gaia/log/dir",
  "dockerDataDir": "/gaia/data/dir",
  "volumes": "rbd1:/data/rbd1,rbd2:/data/rbd2",
  "networkType": 0,
  "scale": {
    "minReplicas": 1,
    "maxReplicas": 5,
    "targetCPUUtilization": 50
  },
  "configMap":[
    {
      "type": "Volume",
      "name": "special-config",
      "key": ["game-config", "game-config2.conf"],
      "mountPath": "/etc/config"
    },
    {
      "type": "Env",
      "name": "special-config2",
      "key": ["HELLO", "WORLD"]
    }
  ]
}
```

**返回**

```json
{
  "AppID": "033613b240a2986011223370c94d1519147a0479d7a0eb6e88bb5ce20282be05",
  "Cmd": "/bin/bash",
  "Created": "2016-09-28T17:29:22+08:00",
  "DockerDataDir": "/gaia/log/dir",
  "DockerLogDir": "/gaia/log/dir",
  "Environments": [
    {
      "name": "PATH1",
      "value": "/bin"
    },
    {
      "name": "PATH2",
      "value": "/usr/bin"
    }
  ],
  "Image": "nginx:1.7.9",
  "InstanceNum": 2,
  "InstanceSummary": "",
  "Name": "demoapp",
  "PortsInfo": [
    {
      "protocol": "tcp",
      "containerPort": 8088
    },
    {
      "protocol": "udp",
      "containerPort": 9999
    }
  ],
  "ProjectID": "abcdefg",
  "ProjectName": "default",
  "Status": "RUNNING",
  "Username": "thomassong",
  "ResInfo": {
    "cpu": "0.1",
    "mem": "256Mi"
  },
  "NetworkType": 0,
  "Type": "Deployment",
  "scale": {
    "minReplicas": 1,
    "maxReplicas": 5,
    "targetCPUUtilization": 50
  },
  "configMap":[
    {
      "type": "Volume",
      "name": "special-config",
      "key": ["game-config", "game-config2.conf"],
      "mountPath": "/etc/config"
    },
    {
      "type": "Env",
      "name": "special-config2",
      "key": ["HELLO", "WORLD"]
    }
  ]
}
```

## 删除app

`DELETE /v2/applications/{projectName}/{appName}?app_id=appID`

**请求参数**

* app_id: 查询条件，选填，如果不设置默认删除正在运行的app

**返回**

```json
{
  "Code": 0,
  "Message": ""
}
```

## 获取app信息
`GET /v2/applications/{projectName}/{appName}`

**请求参数**

* app_id: appID,选填。如果没有指定,默认获取运行中app的信息。

**返回**

```json
{
  "AppID": "033613b240a2986011223370c94d1519147a0479d7a0eb6e88bb5ce20282be05",
  "Cmd": "",
  "Created": "2016-09-28T17:29:22+08:00",
  "DockerDataDir": "",
  "DockerLogDir": "",
  "Environments": null,
  "Image": "nginx:1.7.9",
  "InstanceNum": 2,
  "InstanceSummary": "",
  "Name": "demoapp",
  "PortsInfo": null,
  "ProjectID": "abcdefg",
  "ProjectName": "default",
  "Status": "RUNNING",
  "Username": "thomassong",
  "ResInfo": {
    "cpu": "0.1",
    "mem": "256Mi"
  },
  "VolumesDetail": [
    {
      "id": "cb015de9-14b1-40e3-98e8-0f7ba1ad4e6b",
      "name": "elsanli-test99",
      "status": "in-use",
      "size": 1,
      "used_in_kb": 33088,
      "mountpoint": "/volume",
      "readonly": false
    }
  ],
  "Type": "Deployment",
  "scale": {
    "minReplicas": 1,
    "maxReplicas": 5,
    "targetCPUUtilization": 50
  },
  "configMap":[
    {
      "type": "Volume",
      "name": "special-config",
      "key": ["game-config", "game-config2.conf"],
      "mountPath": "/etc/config"
    },
    {
      "type": "Env",
      "name": "special-config2",
      "key": ["HELLO", "WORLD"]
    }
  ]
}
```

## 灰度升级
`POST /v2/applications/{projectName}/{appName}/rollout`

**请求body**

```json
{
  "image": "NewImageName:NewTag"
}
```

**返回**

```json
{
  "Code": 0,
  "Message": ""
}
```

## 扩容缩容
`POST /v2/applications/{projectName}/{appName}/scale?type=xxx`
**请求参数**

* type: 扩容缩容的类型，目前支持两种：manual和auto，对于manual类型，需要传scale参数; 对于auto类型，需要传body
* scale: 期望的副本数


**请求body**

```json
{
  "minReplicas": 1,
  "maxReplicas": 10,
  "targetCPUUtilization": 50
}
```

**返回**

```json
{
  "AppID": "033613b240a2986011223370c94d1519147a0479d7a0eb6e88bb5ce20282be05",
  "Cmd": "",
  "Created": "2016-09-28T17:29:22+08:00",
  "DockerDataDir": "",
  "DockerLogDir": "",
  "Environments": null,
  "Image": "nginx:1.7.9",
  "InstanceNum": 3,
  "InstanceSummary": "",
  "Name": "demoapp",
  "PortsInfo": null,
  "ProjectID": "abcdefg",
  "ProjectName": "default",
  "Status": "RUNNING",
  "Username": "thomassong",
  "ResInfo": {
    "cpu": "0.1",
    "mem": "256Mi"
  },
  "Type": "Deployment",
  "scale": {
    "minReplicas": 1,
    "maxReplicas": 5,
    "targetCPUUtilization": 50
  },
  "configMap":[
    {
      "type": "Volume",
      "name": "special-config",
      "key": ["game-config", "game-config2.conf"],
      "mountPath": "/etc/config"
    },
    {
      "type": "Env",
      "name": "special-config2",
      "key": ["HELLO", "WORLD"]
    }
  ]
}
```

## 终止app
`POST /v2/applications/{projectName}/{appName}/kill?app_id=xxx`
**请求参数**

* app_id: appID，可选，如果不填则会杀死还在运行中的app

**返回**

```json
{
  "Code": 0,
  "Message": ""
}
```

## 启动app
`POST /v2/applications/{projectName}/{appName}/start?app_id=xxxxx`
**请求参数**

* app_id: appID, 必填

**返回**

```json
{
  "AppID": "033613b240a2986011223370c94d1519147a0479d7a0eb6e88bb5ce20282be05",
  "Cmd": "",
  "Created": "2016-09-28T17:29:22+08:00",
  "DockerDataDir": "",
  "DockerLogDir": "",
  "Environments": null,
  "Image": "nginx:1.7.9",
  "InstanceNum": 2,
  "InstanceSummary": "",
  "Name": "demoapp",
  "PortsInfo": null,
  "ProjectID": "abcdefg",
  "ProjectName": "default",
  "Status": "RUNNING",
  "Username": "thomassong",
  "ResInfo": {
    "cpu": "0.1",
    "mem": "256Mi"
  },
  "Type": "Deployment",
  "scale": {
    "minReplicas": 1,
    "maxReplicas": 5,
    "targetCPUUtilization": 50
  },
  "configMap":[
    {
      "type": "Volume",
      "name": "special-config",
      "key": ["game-config", "game-config2.conf"],
      "mountPath": "/etc/config"
    },
    {
      "type": "Env",
      "name": "special-config2",
      "key": ["HELLO", "WORLD"]
    }
  ]
}
```

