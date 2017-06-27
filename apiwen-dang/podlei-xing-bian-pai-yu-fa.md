# Pod 类型编排语法

> Pod编排支持用户提交kubernetes原生支持的Pod类型的编排应用，即一个编排中可以有多个container，已Pod的形式对这些container进行组织。在对原生Pod语法的基础上，还扩展了对container日志和数据路径的设置

> Pod编排提交后将已Deployment的形式运行，保障多副本可用性

## Pod基本语法

完全支持Pod的Spec语法，可以定义多个container，volume以及其他字段例如dnsPolicy，restartPolicy等

> 由于进行quota限制，如果用户没有对container的 `resources.requests.{memory/cpu}` 进行设置，默认设置为 `256Mi`, `100m`.

### replicas

设置pod的副本数

### annotation

给container定义额外的日志路径和数据路径，见示例


## Pod编排模板样例

```yaml
containers:
- image: docker.oa.com:8080/public/mysql:5.5.48
  name: mysql
  env:
  - name: POD_NAMESPACE
    valueFrom:
      fieldRef:
        fieldPath: metadata.namespace
  - name: MYSQL_ROOT_PASSWORD
    value: hhh
  ports:
  - containerPort: 8080
    name: admin-port
  - containerPort: 3306
    name: db-port
  resources:
    requests:
      cpu: 100m
      memory: 128Mi
  volumeMounts:
  - mountPath: /var/lib/mysql
    name: rethinkdb-storage
- image: docker.oa.com:8080/public/2048:latest
  name: 2048app
  env:
  - name: POD_NAME
    value: 2048app
  ports:
  - containerPort: 80
    name: web
volumes:
- name: rethinkdb-storage
  emptyDir: {}
replicas: 3
annotation:
- containerName: mysql
  logPath: /data1
  dataPath: /data2
- containerName: 2048app
  logPath: /var/log/nginx
```
