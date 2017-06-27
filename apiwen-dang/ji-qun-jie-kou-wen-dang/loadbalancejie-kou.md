# loadbalance接口

## 绑定负载均衡 

`POST /v2/qcloud_bm/bindLoadBalancer` 

**请求参数**

* projectName: string, App所属的项目名
* appName: string, App名称
* lbID: string, 黑石负载均衡的lbid
* ports: array, 端口绑定规则数组，每个元素都是一个port对象

**port对象参数**

* publicPort: int, 负载均衡的公网端口
* dockerPort: int, 容器内进程监听的端口
* protocol: string, 协议类型，仅支持“TCP”与“UDP”两种（大写）

**请求body**

```json
{
  "projectName": "default",
  "appName": "my-nginx2",
  "lbID": "lb-0nxveqgz",
  "ports": [
    {
      "publicPort": 9080,
      "dockerPort": 80,
      "protocol": "TCP"
    },
    {
      "publicPort": 9090,
      "dockerPort": 90,
      "protocol": "TCP"
    }
  ]
}
```

**返回**

```json
{
  "returnCode": 0,//返回0时表示成功，非0时表示失败
  "returnMessage":""
}
```

## 解绑负载均衡 

`POST /v2/qcloud_bm/releaseLoadBalancer`

**请求参数**
* 与绑定接口相同

**返回**
* 与绑定接口相同

## 查询绑定状态 

`POST /v2/qcloud_bm/checkLoadBalancer`

**请求参数**

* projectName: string, App所属的项目名
* appName: string, App名称
* appID: string, App的AppID 

**返回**

```json
{
  "appStatus": 0,
  "ports": [
    {
      "publicPort": 9080,
      "dockerPort": 80,
      "protocol": "TCP",
      "status": 0,
      "lbID": "lb-0nxveqgz",
      "vip": "115.159.246.149",
      "pods": [
        {
          "podName": "loadbalancePod-1",
          "status": 0,
          "bindErrorMessage": "",
          "unBindErrorMessage": "",
          "tunnelErrorMessage": ""
        }
      ]
    }
  ]
}
```
