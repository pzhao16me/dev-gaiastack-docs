#代码构建SVN指引

###1.步骤
当代码构建项目的代码源为SVN时，代码构建的过程是：

1.	平台以GaiaStackX用户以只读权限从SVN服务器拉取代码；
2.	在代码一级目录查找gaiastack.yml文件（单元测试配置文件），如果不存在则直接进行第3步，否则，根据gaiastack.yml配置，启动容器（带测试环境的image由用户在gaiastack.yrml文件中指定），在容器中进行单元测试。如果单元测试通过则继续进行第3步，否则失败退出。
3.	根据用户设定的Dockerfile路径中的Dockerfile构建image。构建不成功则失败并报错；成功则进行第4部；
4.	将生成的image push到镜像仓库。

###2.示例


假设项目对应的svn路径为：
http://tc-svn.tencent.com/doss/doss_tdw_rep/tdw_proj/branches/tdwgaia/gaia-stack/gaia_stack_test。

用户如果要将SVN项目加入代码构建，需要进行如下操作：


2.1	在code平台给GaiaStackX用户赋权；
打开http://code.oa.com/v2/form/permission/apply 进行赋权操作。如下图：
 ![](/assets/218.png)
如果地址不正确或者未赋权限，则有如下报错:
![](/assets/219.png)
 2.2  填充基本信息
   * 镜像类型
   
 个人类型，则说明构建的image是个人私有的，只有自己有权限访问。业务类型，则该image整个业务的人可见；
   * 项目名称
   
 构建出的image的名字
   *  Dockerfile路径
   
      Dockerfile路径默认在代码的一级目录（/）下面，用户也可以指定。
   * 持续集成功能：
   
   如果用户需要开通持续集成功能，则需要在code平台相应配置：
   http://code.oa.com/v2/tool/webhooks/init?comefrom=code
