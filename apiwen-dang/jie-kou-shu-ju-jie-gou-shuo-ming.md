# 接口数据结构说明

## app

> app信息数据结构

```json
{
  "AppID": "string: 系统生成的ID",
  "Cmd": "string:运行的命令行",
  "Created": "2017-02-09T08:57:21.405Z",
  "UpdatedAt": "2017-02-09T08:57:21.405Z",
  "DockerDataDir": "string:container数据目录",
  "DockerLogDir": "string:container日志目录",
  "Environments": [
    {
      "name": "string",
      "value": "string"
    }
  ],
  "Image": "运行的镜像",
  "InstanceNum": 0, // 运行副本数
  "InstanceSummary": {}, // map: ["status"->number]实例状态
  "Name": "string: app名称",
  "PortsInfo": [ //端口映射信息
    {
      "protocol": "string:TCP|UDP",
      "containerPort": 0,// 容器端口
      "hostPort": 0 //本地端口
    }
  ],
  "ProjectID": "string:业务ID",
  "ProjectName": "string:业务名",
  "Status": "string:app状态",
  "Username": "string:创建app的用户名",
  "ResInfo": { // 资源信息
    "cpu": "string: 使用的核数，如0.1",
    "mem": "string: 内存，如128Mi"
  },
  "Volumes": "string: 云硬盘参数，指定云硬盘与其对应的挂载目录，挂载多个云硬盘时用“,”分隔，如: 'rbd1:/data/rbd1,rbd2:/data/rbd2'",
  "NetworkType": 0, // 网络模式，值只有0或者1，0是默认的，1是带浮动IP
  "StackInstanceID": "string:如果该app是通过compose创建的，则这里记录对应compose stack的id",
  "VolumesDetail": [ // 挂载云盘的详细信息
    {
      "id": "string:云盘id",
      "name": "string:云盘名",
      "status": "string:云盘状态",
      "size": 0, // 云盘大小
      "used_in_kb": 0, // 使用情况
      "mountpoint": "string", // 挂载点
      "readonly": true
    }
  ],
  "Type": "string:app类型，如Deployment, PodStack. 其中由Pod接口创建的多容器app的类型为PodStack，其他app类型为Deployment",
  "PodStackContent": "string:如果类型是PodStack，则Pod的yaml内容在这里存储，则以上PortsInfo,ResInfo,Environments等字段可能为空",
  "Scale": { // 自动扩容缩容参数
    "minReplicas": 0, // 最小副本数
    "maxReplicas": 0, // 最大副本数
    "targetCPUUtilization": 50 // 目标CPU使用率
  },
  "configMap":[ //关联的ConfigMap列表
    {
      "type": "string: 关联类型，Volume or Env",
      "name": "string: ConfigMap名称",
      "key": "array: 关联的配置项的key",
      "mountPath": "string: Volume模式下的挂载目录"
    }
  ]
}
```

## app提交结构

> 提交app作业时使用的结构

```json
{
  "name": "string：指定name",
  "imageRef": "string：指定image",
  "resources": { //指定资源
    "cpu": "string: 如0.1, 1",
    "mem": "string: 如128Mi, 1G"
  },
  "instanceNum": 0, // 指定副本数
  "cmd": "string: 可选，container运行命令，默认为image默认的命令",
  "dockerEnv": [ // 环境变量设置, 可选
    {
      "name": "string",
      "value": "string"
    }
  ],
  "portMappings": [ // 端口映射
    {
      "protocol": "string: TCP/UDP",
      "containerPort": 0, // 容器端口
      "hostPort": 0 // 目前无法指定主机端口
    }
  ],
  "dockerLogDir": "string：指定log存储目录",
  "dockerDataDir": "string：指定data数据存储目录",
  "volumes": "string:云硬盘参数，指定云硬盘与其对应的挂载目录，挂载多个云硬盘时用“,”分隔，如: 'rbd1:/data/rbd1,rbd2:/data/rbd2'",
  "networkType": 0,
  "scale": { // 自动扩容缩容参数，如果这里进行设置，则instanceNum参数无效
    "minReplicas": 0,
    "maxReplicas": 0,
    "targetCPUUtilization": 0
  },
  "configMap":[ //关联的ConfigMap列表
    {
      "type": "string: 关联类型，Volume or Env",
      "name": "string: ConfigMap名称",
      "key": "array: 关联的配置项的key",
      "mountPath": "string: Volume模式下的挂载目录"
    }
  ]
}
```

## compose应用编排模版

> 编排模版结构，在创建时只需要填写name, content字段

```json
{
  "id": "string:模版ID",
  "name": "string:模版名称",
  "content": "string:yaml文件内容",
  "project_name": "string:所在业务名",
  "last_update": "string:最后一次更新事件，格式为 2016-10-17T12:55:46+08:00",
  "type": "Compose", // 固定这个值
  "relations": { // 根据yaml文件内容得出的各个app之间的关系
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
      ...
    },
    "c3": {
      ...
    }
  }
}
```

## compose编排数据结构

> 由compose模版创建的运行时态, 一个compose编排由多个app构成

```json
{
  "id": "string:编排ID",
  "stackName": "string:编排名",
  "stackContent": "string:编排文件内容",
  "appNum": 3, // 该编排下实际运行的app数量
  "instanceNum": 6, // 该编排下实际运行的container数
  "projectName": "string: 业务名",
  "createAt": "string: 创建事件，格式为：2016-10-17T14:07:14+08:00",
  "lastUpdate": "string: 更新事件，格式为：2016-10-17T14:07:19+08:00",
  "status": "string:编排状态，如RUNNING, LAUNCHING, DELETING, FAILED",
  "userName": "testUserName",
  "images": "docker.oa.com:8080/public/helloworld:latest, docker.oa.com:8080/public/helloworld:latest, docker.oa.com:8080/public/helloworld:latest",
  "stackApps": [ // 这个编排所启动的app列表
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
    }
  ],
  "relations": { // app之间的关系
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
    }
  }
}
```

## pod编排数据结构

> pod编排实际与app共用一个结构，对于pod编排应用，主要信息在`PodStackContent`中，其他很多字段为空

```json
{
  "AppID": "80c918dd71e1c265cb938430c80ee60950fbbb65575fd2447763ca052df26130",
  "Cmd": "",
  "Created": "2016-12-08T15:34:45.276506116+08:00",
  "UpdatedAt": "2016-12-08T15:34:45.279496485+08:00",
  "InstanceNum": 3,
  "Name": "pod666",
  "ProjectID": "testProjectId",
  "ProjectName": "default",
  "Status": "CREATED",
  "Username": "testUserName",
  "ResInfo": {},
  "NetworkType": 0,
  "Type": "PodStack", // pod编排app的类型为PodStack
  "PodStackContent": "string: 编排文件内容",
  "scale": {
    "minReplicas": 1,
    "maxReplicas": 5,
    "targetCPUUtilization": 50
  }
}
```

## image

> image信息数据结构

```json
{
  "namespace": "string:镜像的命名空间",
  "reponame": "string：镜像名字",
  "picAddress": "string：镜像图片访问地址",
  "readme": "string：镜像概览信息",
  "instanceNum": "int:镜像下载次数",
  "latestTag": "string：镜像最新版本名称",
  "updateTime": "2017-01-04T09:41:53.096286704Z",
  "tags": [ //镜像版本列表
    {
      "tagName": "string：版本名称",
      "imageId": "string：版本ID",
      "updateTime": "2017-01-04T09:41:53.096286704Z"
    }
  ]
}
```
## project & quota

> project数据结构，用于创建project，及返回信息

```json
{
  "id": string, // 项目id
  "doman_id": string, // 默认
  "name": string, // 项目名
  "enabled": bool, // 激活使用
  "description": string, // 项目描述
  "is_default_project": bool, // 是否是默认的项目
  "needVmService": bool, // 是否需要虚拟机服务，设置为false即可
}
```

> project quota in cluster数据结构，用户创建集群的project quota，及返回信息

```json
{
  "projectId": string,   // project id
  "projectName": string, // project名称
  "clusterName": string, // 集群名称
  "cpuVcoreQuota": float64, // project在集群上总cpu资源
  "memoryQuota": float64, // project在集群总内存资源
  "diskQuota": int, // project在集群总云硬盘资源
  "usedVcoreNum": float64, //已分配cpu资源
  "usedMemorySize": float64, //已分配内存资源
  "assignedDiskSize": int, // 已分配云硬盘资源
  "usedDiskSize": float64, // 实际使用云硬盘资源
}
```

> cluster quota数据结构，用于返回集群所有project的quota信息

```json
{
  "clusterName": string, // cluster name
  "cpuVcoreQuota": float64, // 集群总的cpu资源
  "memoryQuota": float64, // 集群总的内存资源
  "diskSpace": float32, // 集群总的云硬盘资源
  "assignedVcore": float64, // 集群已分配cpu资源
  "assignedMemorySize": float64 // 集群已分配内存资源
  "usedVcoreNum": float64, // 集群已使用cpu资源
  "usedMemorySize": float64, // 集群已使用内存资源
  "usedDiskSize": float64, // 集群已使用云硬盘资源
  "assignedDiskSize": int, // 集群已分配云硬盘资源
}
```

> project summary quota数据结构，用于返回project在所有集群上的quota信息

```json
{
  "cpuVcoreQuota": float64, // project在各个集群上的总cpu资源
  "memoryQuota": float64,  // projeect在各个集群上的总内存资源
  "diskQuota": int, // project在各个集群上的总云硬盘资源
  "usedVcoreNum": float64, //project在各个集群上的已分配cpu资源
  "usedMemorySize": float64, // project在各个集群上的已分配内存资源
  "usedDiskSize": float64, // project在各个集群上的已使用云硬盘资源
  "assignedDiskSize": int, // project在各个集群上的已分配云硬盘资源
}
```

## 操作记录结构

> 操作记录结构用于展示用户操作的结果

```json
{
  "id": "string:操作记录ID",
  "user_id": "string:操作人ID",
  "user_name": "string:操作人用户名",
  "action_type": "string:操作类型，中文！如：创建，更新，终止，启动，重启，删除，快照，扩容，缩容，切换",
  "resource_type": "string:操作对象的类型，中文！如：应用，实例，云盘，云盘快照，编排模版，Compose编排，Pod编排，应用编排应用",
  "timestamp": "string: 记录事件，格式为：2016-10-20T15:14:40.491063896+08:00",
  "target_name": "string:操作对象名称",
  "target_identity": "string:操作对象的唯一ID",
  "project_name": "string:业务名",
  "result": "string:操作结果，中文！如：成功，失败，正在(用户发起的操作还在进行，常用户异步处理中)，被打断(一个操作被一个新的操作打断)",
  "reason": "string: 操作结果若为失败，则这个字段不为空，用于记录操作失败的原因",
  "detail": "string: 操作记录的完整描述，如：testUserName 成功创建了应用编排compose",
  "clusterName": "string: 操作发生的集群"
}
```

## User

> 获取用户信息的User数据结构

```json
{
  "id": "string:用户的ID",
  "domain_id": "default", //默认
  "default_project_id": "string:用户默认的project的ID",
  "default_project_name": "string:用户默认的project的名称",
  "name": "string:用户名称",
  "enabled": "bool:是否激活"
}
```

## ConfigMap数据结构

> 获取ConfigMap的数据结构

``` json
{
  "name": "string:ConfigMap的名称",
  "projectName": "string:业务名",
  "clusterName": "string:集群名",
  "created_at": "创建时间，格式为：2016-10-17T14:07:14+08:00",
  "configNum": "int: 配置项的个数",
  "data": "map结构: key为配置项名称，value为配置内容",
  "appMapNum" "map结构: key为配置项的名称, value为关联的app个数",
}
```

## AppMap数据结构

> ConfigMapkey关联应用的数据结构

```json
{
  "type": "string: 关联类型，Volume or Env",
  "appName": "string: app名称",
  "appId": "string: app ID",
  "mountPath": "string: Volume关联类型的挂在目录"
}
```

## Token

> 创建token，用户的用户名和密码信息

```json
{
  "identity": {
    "methods": "password", //默认
    "password": {
      "user": {
        "name": "string:用户名称",
        "domain": {
          "name": "Default"   //默认
        },
        "password": "string:用户的登录密码"
      }
    }
  }
}
```

> 创建token响应，返回的数据结构

```json
{
  "id": "string:token的id",
  "methods": "password",  //默认
  "user": {
    "domain": { //默认
      "id": "default",
      "name": "Default"
    },
    "id": "string:token所属用户的id",
    "name": "string:token所属用户的名称"
  },
  "expires_at": "2017-02-13T15:01:53.11888Z" //token的超时时间
  "issued_at": "2017-02-13T05:01:53.118913Z" //token的创建时间
  "projectsSummary": [ //token所属用户的所在的所有project及role信息
    {
      "id": "string:project的id",
      "name": "string:project的name",
      "roles": "[]string:用户在当前project的role名称"
    } 
  ]
}
```
## 分页列表结构

> 大部分列表接口都是以分页的形式返回给用户的，例如app列表，云盘列表等。这些结构基本遵循一下结构

```json
{
  "content": [ // 列表数据存储在content里
    {...},
    {...}
  ],
  "last": true, // 返回的数据是不是最后一页
  "totalElements": 2, // 所有数据量
  "totalPages": 1, // 所有页数
  "first": true, // 返回的数据是否是第一个
  "numberOfElements": 2, // 当前页返回的数据量
  "size": 20, // 每页的条数
  "number": 0 // 页码，从0开始表示第一页
}
```

## 操作错误数据结构

> 接口的调用可能出现失败的情况，当出现失败时，http接口会返回非2xx的http code, 并在response body中给出以下结构说明

```json
{
  "Code": 10000, // 错误码，出错的错误码，可以从错误码文档中查到对应的错误信息，返回0表示操作成功
  "Message": "string:错误信息"
}
```
