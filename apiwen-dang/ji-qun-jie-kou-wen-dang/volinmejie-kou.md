# volume接口

**path 参数说明**

> projectName: 业务名

> volumeId: 云盘ID

> snapshotId: 快照ID

## 创建云盘

`POST /v2/cinder/{projectName}/volumes`

**请求body**

```json
{
  "size": 10,
  "name": "test",
  "description": "volume for test",
  "snapshot_id": "",
  "metadata": {
    "fs_type": "xfs"
  }
}
```

**返回**

```json
{
  "Code": 0,
  "Message": "xxx",
}
```

## 查询云盘

`GET /v2/cinder/{projectName}/volumes/{volumeId}`

**返回**

```json
{
  "attachments": [
    {
      "app_Id": "64ca50b0062d2a187419c4801f8cee6e741cd96a52a88a354f2ce156326cb159",
      "app_name": "ext-v",
      "app_type": "normal",
      "attachment_id": "b4a098f6-298e-4ab0-be97-384b4551ace2",
      "device": "/data/rbd",
      "project_name": "default",
      "volume_id": "49924659-f3d9-4ac8-b8ee-ec0949909ff2"
    }
  ],
  "created_at": "2016-10-03T07:16:27.000000",
  "id": "49924659-f3d9-4ac8-b8ee-ec0949909ff2",
  "metadata": {
    "fs_type": "ext4",
    "used_in_kb": "0"
  },
  "name": "ext-v",
  "project_id": "a9c09996e87d4c6e812e27b122fc4961",
  "project_name": "default",
  "size": 1,
  "status": "in-use",
  "user_id": "d4099f4b6fa14c1292e4cd25d3e3c300",
  "volume_explorer_up": false
}
```

## 删除云盘

`DELETE /v2/cinder/{projectName}/volumes/{volumeId}`

**返回**

```json
{
  "Code": 0
}
```

## 扩展云盘

`POST /v2/cinder/{projectName}/volumes/{volume_id}/extend`

**请求body**

```json
{
  "new_size": 10 // 单位是G
}
```

**返回**

```json
{
  "Code": 0,
  "Message": "xxx",
}
```

## 创建快照

`POST /v2/cinder/{projectName}/snapshots`

**请求body**

```json
{
  "force": false,
  "name": "test",
  "description": "volume for test",
  "volume_id": "xxx"
}
```

**返回**

```json
{
  "Code": 0,
  "Message": "xxx",
}
```

## 查询快照

`GET /v2/cinder/{projectName}/snapshots/{snapshotId}`

**返回**

```json
{
  "created_at": "2016-10-04T07:55:55.000000",
  "id": "ce9e71a7-fe28-48aa-9638-b4f9b9665e52",
  "metadata": {
    "fs_type": "xfs",
    "used_in_kb": "0"
  },
  "name": "test",
  "project_id": "a9c09996e87d4c6e812e27b122fc4961",
  "project_name": "admin",
  "size": 1,
  "status": "available",
  "volume_id": "edefdef1-564b-486f-9bf4-c530575b3a8e",
  "volume_name": "xfs-v"
}
```

## 删除快照

`DELETE /v2/cinder/{projectName}/snapshots/{snapshotId}`

**返回**

```json
{
  "Code": 0,
  "Message": "xxx",
}
```
