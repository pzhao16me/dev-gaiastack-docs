# 列表接口

## app列表

`GET /v2/applications`

**请求参数**

* size: 分页返回，页的大小

* page: 分页返回，第几页（从0开始）

* sort: 分页返回，排序方式，比如：created desc，按创建时间降序返回

* type: app类型，目前支持Deployment和PodStack两种值

* project_name: 根据业务进行过滤，多个业务用,分隔, 默认所有有权限业务

* cluster_name: 集群名称，可选，多个集群用,分割, 默认所有有业务配额的集群

* keyword: 根据关键字进行模糊查询

* app_name: 根据app名称查询

* app_status: 根据app状态查询

* user_name: 根据用户名查询

**返回**

```json
{
  "content": [
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
      "clusterName": "abnormal"
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


## 编排列表

`GET /v2/composes`

**请求参数**

* project_name: 根据业务进行过滤，多个业务用,分隔, 默认所有有权限业务

* cluster_name: 集群名称，可选，多个集群用,分割, 默认所有有业务配额的集群

* size: 分页参数，默认20

* page: 分页参数，从0开始

* sort: 排序，如last_update desc

* keyword: 搜索关键词

**返回**

```json
{
  "content": [
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
      "stackApps": null, // 编排的所有应用，在列表显示中为空
      "relations": null, // container之间的关系，在列表显示中为kong
      "clusterName": "abnormal"
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


## 云盘列表

`GET /v2/cinder/volumes`

**请求参数**

* project_name: 根据业务进行过滤，多个业务用,分隔, 默认所有有权限业务

* cluster_name: 集群名称，可选，多个集群用,分割, 默认所有有业务配额的集群

* size: 分页参数，默认20

* page: 分页参数，从0开始

* status: volume状态 in_use or available

* keyword: volume名称

**返回**

```json
{
  "content": [
    {
      "id": "49924659-f3d9-4ac8-b8ee-ec0949909ff2",
      "status": "available",
      "name": "ext-v",
      "created_at": "2016-10-03T07:16:27.000000",
      "size": 4,
      "metadata": {
        "used_in_kb": "0",
        "fs_type": "ext4"
      },
      "project_id": "a9c09996e87d4c6e812e27b122fc4961",
      "project_name": "admin",
      "user_id": "d4099f4b6fa14c1292e4cd25d3e3c300",
      "user_name": "admin",
      "volume_explorer_up": false,
      "clusterName": "abnormal"
    },
    {
      "id": "edefdef1-564b-486f-9bf4-c530575b3a8e",
      "status": "in-use",
      "name": "xfs-v",
      "created_at": "2016-10-03T07:15:59.000000",
      "size": 17,
      "metadata": {
        "used_in_kb": "0",
        "fs_type": "xfs"
      },
      "project_id": "a9c09996e87d4c6e812e27b122fc4961",
      "project_name": "admin",
      "attachments": [
        {
          "attachment_id": "40f9c0b3-e916-4ecf-8150-b3f5a65c3266",
          "device": "/volume",
          "volume_id": "edefdef1-564b-486f-9bf4-c530575b3a8e",
          "app_name": "v4exp-edefdef1-564b-486f-9bf4-c530575b3a8e",
          "app_Id": "ec653bb214e9d9119a529719657ae4229ed2d24ef17398793eba47a280e5cf5a",
          "app_type": "explorer",
          "project_name": "admin"
        }
      ],
      "user_id": "d4099f4b6fa14c1292e4cd25d3e3c300",
      "user_name": "admin",
      "volume_explorer_up": true,
      "clusterName": "abnormal"
    }
  ],
  "last": true,
  "totalElements": 2,
  "totalPages": 1,
  "first": true,
  "numberOfElements": 2,
  "size": 20,
  "number": 0
}
```


## 快照列表

`GET /v2/cinder/snapshots`

**请求参数**

* project_name: 根据业务进行过滤，多个业务用,分隔, 默认所有有权限业务

* cluster_name: 集群名称，可选，多个集群用,分割, 默认所有有业务配额的集群

* size: 分页参数，默认20

* page: 分页参数，从0开始

* status: snapshot状态 in_use or available

* keyword: snapshot名称

**返回**

```json
{
  "content": [
    {
      "id": "ce9e71a7-fe28-48aa-9638-b4f9b9665e52",
      "status": "available",
      "name": "test",
      "created_at": "2016-10-04T07:55:55.000000",
      "size": 1,
      "metadata": {
        "used_in_kb": "0",
        "fs_type": "xfs"
      },
      "project_id": "a9c09996e87d4c6e812e27b122fc4961",
      "project_name": "admin",
      "volume_id": "edefdef1-564b-486f-9bf4-c530575b3a8e",
      "volume_name": "xfs-v",
      "clusterName": "abnormal"
    }
  ],
  "last": true,
  "totalElements": 1,
  "totalPages": 1,
  "first": true,
  "numberOfElements": 1,
  "size": 20,
  "number": 0
}
```

## ConfigMap列表
  
`GET /v2/configmaps`

**请求参数**

* size: 分页返回，页的大小

* page：分页返回，第几页（从0开始）

* sort：分页返回，排序方式，支持project，name，created_at, username, confignum按照desc或asc排序，如name asc

* project_name: 根据业务进行过滤，多个业务用`,`分隔, 默认所有有权限业务

* cluster_name: 集群名称，可选，多个集群用`,`分割, 默认所有有业务配额的集群

* keyword：根据configMap和userName模糊查询
    
**返回**

* 返回配置项个数,不会返回具体配置内容

```json
{
  "content":[
    {
      "name":"special-config",
      "projectName": "demo",
      "clusterName": "abnormal",
      "created_at": "2016-12-08T15:34:45.276506116+08:00",
      "userName": "admin",
      "configNum": 2
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
