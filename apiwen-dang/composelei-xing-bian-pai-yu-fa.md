# Compose 类型编排语法

> compose 类型编排支持类似compose语法，支持使用一个compose编排模板一次提交多个相互关联的应用。

## Compose基本语法

### image

编排容器的镜像，如 `public/helloworld:latest`, 目前不支持`docker.oa.com:8080`前缀

### command

容器启动命令，如 `/run.sh arg1`。如果不填会使用image默认的启动命令启动

### replicas

应用实例数

### links

依赖的其他编排app名字，数组类型

### environment

环境变量，map类型.

Compose编排会设置一些默认的环境变量，会把yaml文件中设置的container名字变成大写后设置成环境，值为利用这个container模版创建的app名称。用户可以利用这个环境变量进行名字服务，例如container名字为app1, 如果需要使用这个container对应的实际app名字，可以使用$(APP1)进行引用，见下面例子。

### volumes

需要挂载的云盘，数组类型，云盘需要提前创建好并处于可用状态，格式为 云盘名称:容器挂载路径

### mem_limit

内存的需求量，如256Mi, 1Gi

### cpu_shares

CPU需求量，如10

### ports

端口映射，数组类型，格式为 主机端口:容器端口/协议，协议支持tcp/udp

### log_dir

日志目录，需填写绝对路径

### data_dir

数据目录，需填写绝对路径

## Compose编排模板样例

```yaml
helloworld:
    image: public/phpmyadmin:latest
    replicas: 1
    mem_limit: 256Mi
    command: /run.sh phpmyadmin
    cpu_shares: 0.2
    environment:
        CONTAINER_NAME: helloworld
        APP_NAME: $(HELLOWORLD) # 得知自己的名字
        PMA_HOST: $(DB) # 引用db container的APP名字，用作mysql的host进行访问
        MYSQL_ROOT_PASSWORD: 123456
    links:
        - db
db:
    image: public/mysql
    replicas: 1
    mem_limit: 256Mi
    cpu_shares: 0.1
    command: /entrypoint.sh mysqld
    environment:
        CONTAINER_NAME: db
        APP_NAME: $(DB)
        MYSQL_ROOT_PASSWORD: 123456
    volumes:
        - mysql:/var/lib/mysql

 因为有helloworld和db两个container定义在yaml中，所以这个模版创建的两个app，都会默认有 `HELLOWORLD=${stackname}-helloworld DB=${stackname}-db` 两个环境变量
```
