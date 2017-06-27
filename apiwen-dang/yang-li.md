# 样例

> 环境: gate地址10.0.0.1. 有两个集群c1(10.0.0.2), c2(10.0.0.3). 有业务p1, p2. 用户user1/password1

> 目标：创建带云盘的app

1. 用户使用auth接口登录获取token

	**request**

	`POST 10.0.0.1/v2/auth/tokens`

	**header**

	`Content-Type:application/json`

	**body**

	```
    {
      "identity": {
        "methods": [
          "password"
        ],
        "password": {
          "user": {
            "name": "user1",
            "domain": {
              "id": "default"
            },
            "password": "password1"
          }
        }
      }
    }
	```

	**response**

	```json
	{
	  "id": "adc1b0c4fa61451f96f092a433957435", // token 在之后的api请求中将其放入x-auth-token的header中用于权限验证
	  "expires_at": "2017-02-07T18:08:34.870152Z",
	  "issued_at": "2017-02-07T08:08:34.870176Z",
	  "methods": [
	   "password"
	  ],
	  "domain": {},
	  "project": {
	   "id": "4cbc075eb1c74f349930fb8d267030c6",
	   "domain": {
	    "id": "default",
	    "name": "Default"
	   },
	   "name": "p1"
	  },
	  "user": {
	   "domain": {
	    "id": "default",
	    "name": "Default"
	   },
	   "id": "08f0fa95108e44c4a8ffd31ebeb452c7",
	   "name": "user1"
	  },
	  "roles": [
	   {
	    "id": "bbb9a38c5a15443898ec9cc7687097a3",
	    "name": "user"
	   }
	  ],
	  "ProjectsSummary": [
	   {
	    "id": "4cbc075eb1c74f349930fb8d267030c6",
	    "name": "p1",
	    "roles": [
	     {
	      "id": "bbb9a38c5a15443898ec9cc7687097a3",
	      "name": "user"
	     }
	    ]
	   },
	   {
	    "id": "752be9c2bca74d39b6ea6735c02dbc8f",
	    "name": "p2",
	    "roles": [
	     {
	      "id": "a614273229b548f8b2e22f020b68bc71",
	      "name": "user"
	     }
	    ]
	   }
	  ]
	 }
	```

	*获取token，并可以得到用户所属的业务组*

2. 获取集群列表

	**request**

	`GET 10.0.0.1/v2/clusters`

	**header**

	`x-auth-token:adc1b0c4fa61451f96f092a433957435`

	**response**

	```json
	[
	  {
	   "name": "c1",
	   "address": "http://10.0.0.2",
	   "internalAddress": "http://10.2.0.2:9090",
	   "created_at": "2017-01-19T09:54:50+08:00",
	   "updated_at": "2017-02-07T16:07:30+08:00"
	  },
	  {
	   "name": "c2",
	   "address": "http://10.0.0.3",
	   "internalAddress": "http://10.2.0.3:9090",
	   "created_at": "2017-01-19T09:54:50+08:00",
	   "updated_at": "2017-02-07T16:07:30+08:00"
	  }
	 ]
	```

	*得到集群c1, c2以及他们的访问地址*

3. 获取p1/c1下的云盘列表，查询可用的云盘用于创建带云盘的app

	**request**

	`GET 10.0.0.1/v2/cinder/volumes?project_name=p1&cluster_name=c1&size=10&page=0`

	**header**

	`x-auth-token:adc1b0c4fa61451f96f092a433957435`

	**response**

	```json
	{
	  "content": [
	    {
	      "id": "4e59525a-77ec-4678-99b4-fcdcf2718ab3",
	      "status": "available",
	      "name": "volume1",
	      "created_at": "2017-01-20T02:16:45.000000",
	      "size": 2,
	      "metadata": {
	        "total_gb": "2",
	        "used_in_kb": "3072",
	        "fs_type": "ext4"
	      },
	      "project_id": "4cbc075eb1c74f349930fb8d267030c6",
	      "project_name": "p1",
	      "user_id": "79b7b30b7f55497e8df4d70af932d1d6",
	      "user_name": "user1",
	      "volume_explorer_up": false,
	      "clusterName": "c1"
	    },
	    {
	      "id": "99afe982-1ebe-42df-83d3-80a50b3f2e95",
	      "status": "available",
	      "description": "sdfasdfasdf",
	      "name": "windyliu1",
	      "created_at": "2017-01-19T02:54:23.000000",
	      "size": 1,
	      "metadata": {
	        "total_gb": "1",
	        "used_in_kb": "33088",
	        "fs_type": "xfs"
	      },
	      "project_id": "4cbc075eb1c74f349930fb8d267030c6",
	      "project_name": "demo",
	      "user_id": "79b7b30b7f55497e8df4d70af932d1d6",
	      "user_name": "admin",
	      "volume_explorer_up": false,
	      "clusterName": "global_common"
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

	*得到云盘volume1*

4. 向集群c1的p1业务提交作业 **注意这里请求地址是c1的地址**

	**request**

	`POST 10.0.0.2/v2/applications/p1`

	**header**

	`x-auth-token:adc1b0c4fa61451f96f092a433957435`

	`Content-Type:application/json`

	**body**

	```json
    {
      "name": "testapp",
      "imageRef": "public/2048:latest",
      "resources": {
        "cpu": "0.1",
        "mem": "128Mi"
      },
      "instanceNum": 1,
      "dockerEnv": [],
      "networkType": 1,
      "volumes": "4e59525a-77ec-4678-99b4-fcdcf2718ab3:/var/log/nginx"
    }
    ```

	**response**

	```json
	{
	  "AppID": "4bcb71dbef467c6cd6fde3b1ece7dd1ce0088b09bdbcce42016363a8e174ab69",
	  "Cmd": "",
	  "Created": "2017-02-07T16:36:19.837418926+08:00",
	  "UpdatedAt": "2017-02-07T16:36:19.83753872+08:00",
	  "DockerDataDir": "",
	  "DockerLogDir": "",
	  "Environments": null,
	  "Image": "public/2048:latest",
	  "InstanceNum": 1,
	  "InstanceSummary": null,
	  "Name": "testapp",
	  "PortsInfo": null,
	  "ProjectID": "4cbc075eb1c74f349930fb8d267030c6",
	  "ProjectName": "demo",
	  "Status": "CREATED",
	  "Username": "user1",
	  "ResInfo": {
	   "cpu": "",
	   "mem": ""
	  },
	  "Volumes": "4e59525a-77ec-4678-99b4-fcdcf2718ab3:/var/log/nginx",
	  "NetworkType": 1,
	  "StackInstanceID": "",
	  "VolumesDetail": null,
	  "Type": "Deployment",
	  "PodStackContent": ""
	 }
	```

	*向c1地址提交创建app请求*

5. 向gate查询app列表

	**request**

	`GET 10.0.0.1/v2/applications?project_name=p1&cluster_name=c1&size=10&page=0`

	**header**

	`x-auth-token:adc1b0c4fa61451f96f092a433957435`

	**response**

	```json
	{
	  "content": [
	   {
	    "AppID": "4bcb71dbef467c6cd6fde3b1ece7dd1ce0088b09bdbcce42016363a8e174ab69",
	    "Cmd": "",
	    "Created": "2017-02-07T16:36:19+08:00",
	    "UpdatedAt": "2017-02-07T16:36:20+08:00",
	    "DockerDataDir": "",
	    "DockerLogDir": "",
	    "Environments": null,
	    "Image": "public/2048:latest",
	    "InstanceNum": 1,
	    "InstanceSummary": {
	     "Pending": 1
	    },
	    "Name": "testapp",
	    "PortsInfo": null,
	    "ProjectID": "4cbc075eb1c74f349930fb8d267030c6",
	    "ProjectName": "demo",
	    "Status": "PENDING",
	    "Username": "user1",
	    "ResInfo": {
	     "cpu": "0.1",
	     "mem": "128Mi"
	    },
	    "Volumes": "4e59525a-77ec-4678-99b4-fcdcf2718ab3:/var/log/nginx",
	    "NetworkType": 1,
	    "StackInstanceID": "",
	    "VolumesDetail": [
	     {
	      "id": "4e59525a-77ec-4678-99b4-fcdcf2718ab3",
	      "name": "volume1",
	      "status": "in-use",
	      "size": 2,
	      "used_in_kb": 3072,
	      "mountpoint": "/var/log/nginx",
	      "readonly": false
	     }
	    ],
	    "Type": "Deployment",
	    "PodStackContent": "",
	    "clusterName": "c1"
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

	*从gate查询创建的app*
