# 错误码

API Switch返回的http status code为2XX时，表示程序正常返回，此时将不会返回error code，否则API Switch将会返回如下结构的信息：

```
{
  "Code": XXX,
  "Message": "XXX"
}
```

`Code`的值的含义如下所示：

```
//参数不合法
ILLEGAL_PARAMETER = 10001

//未找到相应主机
SERVER_NOT_FOUND = 10002

//未找到相应条目
ITEM_NOT_FOUND = 10003

//内部错误
INTERNAL_ERROR = 10004

//非法请求
BADREQUEST = 10005

//禁止访问
FORBIDDEN = 10006

//TOKEN不合法
TOKEN_INVALID = 10101

//应用名称已被占用
APP_NAME_DUPLICATED = 10202

//应用名称不合法
APP_NAME_ERROR = 10203

//应用名称为空
APP_NAME_EMPTY = 10204

//未找到应用
APP_NOT_FOUND = 10205

//提交应用失败
APP_SUBMIT_ERROR = 10206

//当前应用状态不允许该操作
APP_STATE_ERROR = 10207

//应用端口映射未找到
APP_PORT_NOT_FOUND = 10208

//终止应用失败
APP_KILL_ERROR = 10209

//删除应用失败
APP_DELETE_ERROR = 10210

//扩/缩容参数不合法
APP_RAMP_PARAM_ERROR = 10211

//扩容失败
APP_RAMPUP_ERROR = 10212

//data路径参数错误
APP_DATA_ERROR = 10213

//log路径参数错误
APP_LOG_ERROR = 10214

//未设置启动命令
APP_CMD_EMPTY = 10215

// 挂载云盘时实例数目必须为1
INVALID_INSTANCE_NUMBER = 10301

// 参数不合法
INVALID_VOL_PARAMETER = 10302

// 挂载参数中缺少挂载路径
MOUNT_POINT_NOT_FOUND = 10303

// 挂载路径格式错误
INVALID_MOUNT_POINT_FORMAT = 10304

// 应用的多个云盘的挂载路径重复
MOUNT_POINT_DUPLICATED = 10305

// 云盘不可用
VOL_UNAVAILABLE = 10306

// 修改云盘状态失败
VOL_SET_STATUS_ERROR = 10307

// 挂载云盘异常
VOL_ATTACH_FAILED = 10308

// 云硬盘名称不能为空
VOL_NAME_IS_NULL = 10309

// 云硬盘名称已存在
VOL_NAME_CONFLICT = 10310

// 删除云硬盘异常
VOL_DELETE_FAILED = 10311

// 创建云硬盘异常
VOL_CREATE_FAILED = 10312

// 获取云硬盘列表异常
VOL_LIST_FAILED = 10313

// 获取云硬盘信息异常
VOL_GET_FAILED = 10314

// 卸载云硬盘异常
VOL_DETACH_FAILED = 10315

// 云硬盘扩容后容量必须大于当前容量
VOL_EXT_BAD_SIZE = 10316

// 超出云硬盘容量配额，暂时无法进行扩容
VOL_EXT_EXCEED_QUOTA = 10317

// 云硬盘当前状态不支持扩容操作
VOL_EXT_BAD_VOL_STATUS = 10318

// 云硬盘扩容操作异常
VOL_EXT_FAILED = 10319

// 正在为您扩容云硬盘
VOL_EXT_CONTINUE = 10320

// 云硬盘正被资源管理器占用中，暂不支持扩容
VOL_EXT_USED_BY_EXPLORER = 10321

// 云硬盘正在扩容中，当前扩容结束后，才能进行再次扩容
VOL_EXT_ALREADY_EXTENDING = 10322

// 应用处于非运行状态或应用状态异常，暂无法扩容云盘
VOL_EXT_BAD_APP_STATUS = 10323

// 云硬盘离线扩容器启动异常
VOL_EXT_FAILED_EXTENDER = 10324

// 云硬盘资源管理器启动异常
VOL_EXP_FAILED_EXPLORER = 10325

// 云硬盘资源管理器正在启动
VOL_EXP_WAIT = 10326

// 云硬盘正被其他应用使用中，暂无法启动资源管理器
VOL_EXP_VOL_BUSY = 10327

// 云硬盘资源管理器启动成功
VOL_EXP_READY = 10328

// 云硬盘更新异常
VOL_UPDATE_FAILED = 10329

// 云硬盘Metadata获取失败
VOL_METADATA_GET_FAILED = 10330

// 云硬盘快照创建异常
VOL_SS_CREATE_FAILED = 10331

// 编辑云硬盘快照异常
VOL_SS_UPDATE_FAILED = 10332

// 删除云硬盘快照异常
VOL_SS_DELETE_FAILED = 10333

// 获取云硬盘快照异常
VOL_SS_GET_FAILED = 10334

// 获取云硬盘快照列表异常
VOL_SS_LIST_FAILED = 10335

// 获取云主机云硬盘挂载信息异常
VOL_SERVER_ATTACHMENT_GET_FAILED = 10336

// 获取云主机云硬盘挂载信息列表异常
VOL_SERVER_ATTACHMENT_LIST_FAILED = 10337

// app所有内置云盘都过期
ALL_DEDICATED_VOLUME_EXPIRED = 10338

// instance的内置云盘过期
DEDICATED_VOL_EXPIRED = 10339

// 应用处于非运行状态或应用状态异常，暂无法扩容云盘
VOL_EXT_BAD_APP_INSTANCE_STATUS = 10340

// 云硬盘已经挂载到了其他应用
VOL_ALREADY_ATTACHED = 10341

// 云硬盘回滚到指定快照失败
VOL_ROLLBACK_FAILED = 10342

// 检查云盘是否存在失败
VOL_CHECK_EXISTS_FAILED = 10343

// 云盘不存在
VOL_NOT_EXISTS = 10344

// 获取云盘使用状态失败
VOL_GET_USAGE_FAILED = 10345
// 云硬盘快照名称不能为空
VOL_SS_NAME_IS_NULL = 10346

// 云硬盘快照名称已存在
VOL_SS_NAME_CONFLICT = 10347

// 云硬盘文件系统类型不能为空
VOL_FS_IS_NULL = 10348

// 云硬盘不支持该类型文件系统
VOL_FS_UNSUPPORTED = 10349

// 获取云硬盘列表异常
VOL_SUM_LIST_FAILED = 10350

// 存在以该快照为源的云硬盘，该快照暂不能删除
VOL_SS_DELETE_VOL_EXIST = 10351

// 资源管理器关闭失败
VOL_EXPLORER_SHUTDOWN_FAILED = 10352

// 资源管理器正在关闭中
VOL_EXPLORER_STOPPING = 10353

// 禁止删除该云硬盘，存在依赖的快照
VOL_DELETE_SS_EXIST_FAILED = 10354

// 云硬盘名字不合法
VOL_BAD_NAME = 10355

// 云硬盘快照名字不合法
VOL_SS_BAD_NAME = 10356

// 该项目的QUOTA已经存在
QUOTA_PROJECT_EXIST = 10501

// 业务不存在
PROJECT_NOT_EXIST = 10509

// YAML文件语法错误
YAML_SYNTAX_ERROR = 10601

//  部分语法不支持
SYNTAX_NOT_SUPPORT = 10602

// 未找到操作的应用编排
STACK_NOT_FOUND = 10603

// 编排名重复
STACK_NAME_DUPLICATED = 10604

// YAML文件名称重复
YAML_NAME_DUPLICATED = 10605

// 编排名称错误
STACK_NAME_ERROR = 10606

// 未找到操作的YAML文件
YAML_NOT_FOUND = 10607

VOLUME_NOT_FOUND = 10608

// 超出配额
QUOTA_EXCEED = 10609

// 网络模式不支持
NET_NOT_SUPPORT = 10610

// 服务镜像信息未指定
IMAGE_NOT_SET = 10611

// 端口格式存在错误
PORT_FORMAT_ERROR = 10612

// 存在错误的depends_on服务
DEPENDS_ERROR = 10613

// depends_on有环存在，无法启动
DEPENDS_HAS_CYCLE = 10614

// VOLUME格式错误
VOLUME_FORMAT_ERROR = 10615

// YAML文件名错误
YAML_NAME_ERROR = 10616

// 重复使用同一个云盘
VOLUME_REUSE_ERROR = 10617

// memory或cpu设置不正确
RESOURCE_ERROR = 10618

// 重复挂载到同一个目录
MOUNT_ERROR = 10619

// 挂载路径应该是绝对路径
MOUNT_ABS_ERROR = 10620

// 提交中暂时不能删除
SUBMITTING_IN_PROGRESS = 10220

//应用配置名称已存在
CONFIGMAP_NAME_DUPLICATED = 10911

//应用配置名称不合法
CONFIGMAP_NAME_ERROR = 10912

//应用配置项名称不合法
CONFIGMAP_KEY_NAME_ERROR = 10913

```
