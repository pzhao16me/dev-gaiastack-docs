#代码构建GitLab指引

###1.步骤

当代码构建项目的代码源为GitLab时，代码构建的过程为：
1.1	平台以GaiaStackX用户以只读权限从GitLab平台拉取对应分支的代码；
1.2	在代码一级目录查找gaiastack.yml文件（单元测试配置文件），如果不存在则直接进行第3步，否则，根据gaiastack.yml配置，启动容器（带测试环境的image由用户在gaiastack.yrml文件中指定），在容器中进行单元测试。如果单元测试通过则继续进行第3步，否则失败退出。
1.3	根据用户设定的Dockerfile路径中的Dockerfile构建image。构建不成功则失败并报错；成功则进行第4部；
1.4	将生成的image push到镜像仓库。

###2.示例
假设项目地址为：
http://git.code.oa.com/gaiastack/golang-helloworld-web.git

用户如果要将GitLab项目加入代码构建，需要进行如下操作：
2.1	给GaiaStackX用户赋权，方法如下
![](/assets/220.png)
 
 

2.2	如果需要加入持续集成，对应分支有代码提交时自动触发代码功能，还需要在git平台设置webhook。方法如下：
 
  * 点击setting
![](/assets/221.png)
 
* 设置webhook：
![](/assets/224.png)
 
2.3  填充基本信息
  * 镜像类型
  
 个人类型，则说明构建的image是个人私有的，只有自己有权限访问。业务类型，则该image整个业务的人可见；
  * 项目名称
  
 构建出的image的名字
  * Dockerfile路径
  
      Dockerfile路径默认在代码的一级目录（/）下面，用户也可以指定。
  * 持续集成
  
      如果开启了持续集成功能，并在gitlab平台设置了webhook，当项目对应分支有新提交时（PushEvent），将自动触发代码构建。
