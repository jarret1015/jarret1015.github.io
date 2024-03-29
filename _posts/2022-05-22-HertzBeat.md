<h1 align = "center">HertzBeat</h1>

1、可监控内容有哪些，以及开源方未来的监控规划（可以简单对标普罗米修斯生态进行归纳）
2、已有的各监控项相关的配置说明
3、告警内容即接入方式，针对钉钉邮件等免费方式深入实践



最新版本：[v1.0-beta.5](https://github.com/dromara/hertzbeat/releases/tag/v1.0-beta.5)

[官网地址](https://hertzbeat.com/)：https://hertzbeat.com

![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/web-monitor.svg) ![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/ping-connect.svg) ![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/port-available.svg) ![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/database-monitor.svg) ![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/os-monitor.svg) ![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/custom-monitor.svg) ![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/threshold.svg) ![tan-cloud](https://cdn.jsdelivr.net/gh/dromara/hertzbeat@gh-pages/img/badge/alert.svg)

# 简介

> [HertzBeat赫兹跳动](https://github.com/dromara/hertzbeat) 是由[Dromara](https://dromara.org/)孵化，[TanCloud](https://tancloud.cn/)开源的一个支持网站，API，PING，端口，数据库，全站等监控类型，支持阈值告警，告警通知(邮箱，webhook，钉钉，企业微信，飞书机器人)，拥有易用友好的可视化操作界面的开源监控告警项目。目前还在开发初期，后面会支持更多的监控类型。比如操作系统，云原生，中间件，应用服务等等。

# 模块

- [manager](https://github.com/dromara/hertzbeat/tree/master/manager) 提供监控管理,系统管理基础服务

  > 提供对监控的管理，监控应用配置的管理，系统用户租户后台管理等。

- [collector](https://github.com/dromara/hertzbeat/tree/master/collector) 提供监控数据采集服务

  > 使用通用协议远程采集获取对端指标数据。

- [warehouse](https://github.com/dromara/hertzbeat/tree/master/warehouse) 提供监控数据仓储服务

  > 采集指标结果数据管理，数据落盘，查询，计算统计。

- [alerter](https://github.com/dromara/hertzbeat/tree/master/alerter) 提供告警服务

  > 告警计算触发，监控状态联动，告警配置，告警通知。

- [web-app](https://github.com/dromara/hertzbeat/tree/master/web-app) 提供可视化控制台页面

  > 监控告警系统可视化控制台前端(angular+ts+zorro)

![hertzBeat](https://tancloud.gd2.qingstor.com/img/docs/hertzbeat-stru.svg)

# 配置说明

## 监控服务

### 应用服务监控

#### 网站监测

>  对网站是否可用，响应时间等指标进行监测

##### 配置参数

| 参数名称  | 参数帮助描述                                                 |
| --------- | ------------------------------------------------------------ |
| 监控Host  | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称  | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口      | 网站对外提供的端口，http一般默认为80，https一般默认为443。   |
| 相对路径  | 网站地址除IP端口外的后缀路径，例如 `www.tancloud.cn/console` 网站的相对路径为 `/console`。 |
| 启用HTTPS | 是否通过HTTPS访问网站，注意开启HTTPS一般默认对应端口需要改为443 |
| 采集间隔  | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测  | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注  | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：summary

| 指标名称     | 指标单位 | 指标帮助描述 |
| ------------ | -------- | ------------ |
| responseTime | ms毫秒   | 网站响应时间 |

#### HTTP API 

> 调用HTTP API接口，查看接口是否可用，对其响应时间等指标进行监测

##### 配置参数

| 参数名称     | 参数帮助描述                                                 |
| ------------ | ------------------------------------------------------------ |
| 监控Host     | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称     | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口         | 网站对外提供的端口，http一般默认为80，https一般默认为443。   |
| 相对路径     | 网站地址除IP端口外的后缀路径，例如 `www.tancloud.cn/console` 网站的相对路径为 `/console`。 |
| 请求方式     | 设置接口调用的请求方式：GET,POST,PUT,DELETE。                |
| 启用HTTPS    | 是否通过HTTPS访问网站，注意开启HTTPS一般默认对应端口需要改为443 |
| 用户名       | 接口Basic认证或Digest认证时使用的用户名                      |
| 密码         | 接口Basic认证或Digest认证时使用的密码                        |
| Content-Type | 设置携带BODY请求体数据请求时的资源类型                       |
| 请求BODY     | 设置携带BODY请求体数据，PUT POST请求方式时有效               |
| 采集间隔     | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测     | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注     | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：summary

| 指标名称     | 指标单位 | 指标帮助描述 |
| ------------ | -------- | ------------ |
| responseTime | ms毫秒   | 网站响应时间 |

##### 使用问题

指标集合：summary

#### PING连通性

> 对对端HOST地址进行PING操作，判断其连通性

##### 配置参数

| 参数名称     | 参数帮助描述                                                 |
| ------------ | ------------------------------------------------------------ |
| 监控Host     | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称     | 标识此监控的名称，名称需要保证唯一性。                       |
| Ping超时时间 | 设置PING未响应数据时的超时时间，单位ms毫秒，默认3000毫秒。   |
| 采集间隔     | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测     | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注     | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：summary

| 指标名称     | 指标单位 | 指标帮助描述 |
| ------------ | -------- | ------------ |
| responseTime | ms毫秒   | 网站响应时间 |

#### 端口可用性

> 判断对端服务暴露端口是否可用，进而判断对端服务是否可用，采集响应时间等指标进行监测

##### 配置参数

| 参数名称     | 参数帮助描述                                                 |
| ------------ | ------------------------------------------------------------ |
| 监控Host     | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称     | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口         | 网站对外提供的端口，http一般默认为80，https一般默认为443。   |
| 连接超时时间 | 端口连接的等待超时时间，单位毫秒，默认3000毫秒。             |
| 采集间隔     | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测     | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注     | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：summary

| 指标名称     | 指标单位 | 指标帮助描述 |
| ------------ | -------- | ------------ |
| responseTime | ms毫秒   | 网站响应时间 |

#### 全站监控

> 对网站的全部页面监测是否可用
> 往往一个网站有多个不同服务提供的页面，我们通过采集网站暴露出来的网站地图SiteMap来监控全站。
> 注意，此监控需您网站支持SiteMap。我们支持XML和TXT格式的SiteMap。

##### 配置参数

| 参数名称  | 参数帮助描述                                                 |
| --------- | ------------------------------------------------------------ |
| 监控Host  | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称  | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口      | 网站对外提供的端口，http一般默认为80，https一般默认为443。   |
| 网站地图  | 网站SiteMap地图地址的相对路径，例如：/sitemap.xml。          |
| 启用HTTPS | 是否通过HTTPS访问网站，注意开启HTTPS一般默认对应端口需要改为443 |
| 采集间隔  | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测  | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注  | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：summary

| 指标名称     | 指标单位 | 指标帮助描述               |
| ------------ | -------- | -------------------------- |
| url          | 无       | 网页的URL路径              |
| statusCode   | 无       | 请求此网页的响应HTTP状态码 |
| responseTime | ms毫秒   | 网站响应时间               |
| errorMsg     | 无       | 请求此网站反馈的错误信息   |

### 数据库监控

#### MYSQL数据库监控

> 对MYSQL数据库的通用性能指标进行采集监控。支持MYSQL5+。

##### 配置参数

| 参数名称   | 参数帮助描述                                                 |
| ---------- | ------------------------------------------------------------ |
| 监控Host   | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称   | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口       | 数据库对外提供的端口，默认为3306。                           |
| 数据库名称 | 数据库实例名称，可选。                                       |
| 用户名     | 数据库连接用户名，可选                                       |
| 密码       | 数据库连接密码，可选                                         |
| URL        | 数据库连接URL，可选，若配置，则URL里面的数据库名称，用户名密码等参数会覆盖上面配置的参数 |
| 采集间隔   | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测   | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注   | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：basic

| 指标名称        | 指标单位 | 指标帮助描述         |
| --------------- | -------- | -------------------- |
| version         | 无       | 数据库版本           |
| port            | 无       | 数据库暴露服务端口   |
| datadir         | 无       | 数据库存储数据盘地址 |
| max_connections | 无       | 数据库最大连接数     |

指标集合：status

| 指标名称          | 指标单位 | 指标帮助描述            |
| ----------------- | -------- | ----------------------- |
| threads_created   | 无       | MySql已经创建的总连接数 |
| threads_connected | 无       | MySql已经连接的连接数   |
| threads_cached    | 无       | MySql当前缓存的连接数   |
| threads_running   | 无       | MySql当前活跃的连接数   |

指标集合：innodb

| 指标名称            | 指标单位 | 指标帮助描述                           |
| ------------------- | -------- | -------------------------------------- |
| innodb_data_reads   | 无       | innodb平均每秒从文件中读取的次数       |
| innodb_data_writes  | 无       | innodb平均每秒从文件中写入的次数       |
| innodb_data_read    | KB       | innodb平均每秒钟读取的数据量，单位为KB |
| innodb_data_written | KB       | innodb平均每秒钟写入的数据量，单位为KB |

#### MariaDB数据库监控

> 对MariaDB数据库的通用性能指标进行采集监控。支持MariaDB5+。

##### 配置参数

| 参数名称   | 参数帮助描述                                                 |
| ---------- | ------------------------------------------------------------ |
| 监控Host   | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称   | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口       | 数据库对外提供的端口，默认为3306。                           |
| 数据库名称 | 数据库实例名称，可选。                                       |
| 用户名     | 数据库连接用户名，可选                                       |
| 密码       | 数据库连接密码，可选                                         |
| URL        | 数据库连接URL，可选，若配置，则URL里面的数据库名称，用户名密码等参数会覆盖上面配置的参数 |
| 采集间隔   | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测   | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注   | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：basic

| 指标名称        | 指标单位 | 指标帮助描述         |
| --------------- | -------- | -------------------- |
| version         | 无       | 数据库版本           |
| port            | 无       | 数据库暴露服务端口   |
| datadir         | 无       | 数据库存储数据盘地址 |
| max_connections | 无       | 数据库最大连接数     |

指标集合：status

| 指标名称          | 指标单位 | 指标帮助描述              |
| ----------------- | -------- | ------------------------- |
| threads_created   | 无       | MariaDB已经创建的总连接数 |
| threads_connected | 无       | MariaDB已经连接的连接数   |
| threads_cached    | 无       | MariaDB当前缓存的连接数   |
| threads_running   | 无       | MariaDB当前活跃的连接数   |

指标集合：innodb

| 指标名称            | 指标单位 | 指标帮助描述                           |
| ------------------- | -------- | -------------------------------------- |
| innodb_data_reads   | 无       | innodb平均每秒从文件中读取的次数       |
| innodb_data_writes  | 无       | innodb平均每秒从文件中写入的次数       |
| innodb_data_read    | KB       | innodb平均每秒钟读取的数据量，单位为KB |
| innodb_data_written | KB       | innodb平均每秒钟写入的数据量，单位为KB |

#### PostgreSQL数据库监控

> 对PostgreSQL数据库的通用性能指标进行采集监控。支持PostgreSQL 10+。

##### 配置参数

| 参数名称   | 参数帮助描述                                                 |
| ---------- | ------------------------------------------------------------ |
| 监控Host   | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称   | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口       | 数据库对外提供的端口，默认为5432。                           |
| 数据库名称 | 数据库实例名称，可选。                                       |
| 用户名     | 数据库连接用户名，可选                                       |
| 密码       | 数据库连接密码，可选                                         |
| URL        | 数据库连接URL，可选，若配置，则URL里面的数据库名称，用户名密码等参数会覆盖上面配置的参数 |
| 采集间隔   | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测   | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注   | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：basic

| 指标名称        | 指标单位 | 指标帮助描述               |
| --------------- | -------- | -------------------------- |
| server_version  | 无       | 数据库服务器的版本号       |
| port            | 无       | 数据库服务器端暴露服务端口 |
| server_encoding | 无       | 数据库服务器端的字符集编码 |
| data_directory  | 无       | 数据库存储数据盘地址       |
| max_connections | 连接数   | 数据库最大连接数           |

指标集合：state

| 指标名称       | 指标单位 | 指标帮助描述                                                 |
| -------------- | -------- | ------------------------------------------------------------ |
| name           | 无       | 数据库名称，或share-object为共享对象。                       |
| conflicts      | 次数     | 由于与恢复冲突而在这个数据库中被取消的查询的数目             |
| deadlocks      | 个数     | 在这个数据库中被检测到的死锁数                               |
| blks_read      | 次数     | 在这个数据库中被读取的磁盘块的数量                           |
| blks_hit       | 次数     | 磁盘块被发现已经在缓冲区中的次数，这样不需要一次读取（这只包括 PostgreSQL 缓冲区中的命中，而不包括在操作系统文件系统缓冲区中的命中） |
| blk_read_time  | ms       | 在这个数据库中后端花费在读取数据文件块的时间                 |
| blk_write_time | ms       | 在这个数据库中后端花费在写数据文件块的时间                   |
| stats_reset    | 无       | 这些统计信息上次被重置的时间                                 |

指标集合：activity

| 指标名称 | 指标单位 | 指标帮助描述     |
| -------- | -------- | ---------------- |
| running  | 连接数   | 当前客户端连接数 |

### 操作系统监控

#### Linux操作系统监控

> 对Linux操作系统的通用性能指标进行采集监控。

##### 配置参数

| 参数名称 | 参数帮助描述                                                 |
| -------- | ------------------------------------------------------------ |
| 监控Host | 被监控的对端IPV4，IPV6或域名。注意不带协议头(eg: https://, http://)。 |
| 监控名称 | 标识此监控的名称，名称需要保证唯一性。                       |
| 端口     | Linux SSH对外提供的端口，默认为22。                          |
| 用户名   | SSH连接用户名，可选                                          |
| 密码     | SSH连接密码，可选                                            |
| 采集间隔 | 监控周期性采集数据间隔时间，单位秒，可设置的最小间隔为10秒   |
| 是否探测 | 新增监控前是否先探测检查监控可用性，探测成功才会继续新增修改操作 |
| 描述备注 | 更多标识和描述此监控的备注信息，用户可以在这里备注信息       |

##### 采集指标

指标集合：basic

| 指标名称 | 指标单位 | 指标帮助描述 |
| -------- | -------- | ------------ |
| hostname | 无       | 主机名称     |
| version  | 无       | 操作系统版本 |
| uptime   | 无       | 系统运行时间 |

指标集合：cpu

| 指标名称       | 指标单位 | 指标帮助描述                |
| -------------- | -------- | --------------------------- |
| info           | 无       | CPU型号                     |
| cores          | 核数     | CPU内核数量                 |
| interrupt      | 个数     | CPU中断数量                 |
| load           | 无       | CPU最近1/5/15分钟的平均负载 |
| context_switch | 个数     | 当前上下文切换数量          |

指标集合：memory

| 指标名称   | 指标单位 | 指标帮助描述   |
| ---------- | -------- | -------------- |
| total      | Mb       | 总内存容量     |
| used       | Mb       | 用户程序内存量 |
| free       | Mb       | 空闲内存容量   |
| buff_cache | Mb       | 缓存占用内存   |
| available  | Mb       | 剩余可用内存容 |

指标集合：disk

| 指标名称      | 指标单位 | 指标帮助描述       |
| ------------- | -------- | ------------------ |
| disk_num      | 块数     | 磁盘总数           |
| partition_num | 分区数   | 分区总数           |
| block_write   | 块数     | 写入磁盘的总块数   |
| block_read    | 块数     | 从磁盘读出的块数   |
| write_rate    | iops     | 每秒写磁盘块的速率 |

指标集合：interface

| 指标名称       | 指标单位 | 指标帮助描述        |
| -------------- | -------- | ------------------- |
| interface_name | 无       | 网卡名称            |
| receive_bytes  | byte     | 入站数据流量(bytes) |
| transmit_bytes | byte     | 出站数据流量(bytes) |

## 告警服务

### 告警中心

> 已触发的告警信息中心，提供告警删除，告警处理，标记未处理，告警级别状态等查询过滤。

### 告警配置

> 指标阈值配置，提供表达式形式的指标阈值配置，可设置告警级别，触发次数，告警通知模版和是否启用，关联监控等功能。

#### 阈值告警

> 对监控指标配置告警阈值(警告告警，严重告警，紧急告警)，系统根据阈值配置和采集指标数据计算触发告警。

##### 操作步骤

1. **【告警配置】->【新增阈值】-> 【配置后确定】**

   ![threshold](https://hertzbeat.com/assets/images/alert-threshold-1-d42690576e2d740cd262b0c83841ae91.png)

   **指标对象**：选择我们需要配置阈值的监控指标对象 例如：网站监控类型下的 -> summary指标集合下的 -> responseTime响应时间指标
   **阈值触发表达式**：根据此表达式来计算判断是否触发阈值，表达式环境变量和操作符见页面提示，例如：设置响应时间大于50触发告警，表达式为 `responseTime > 50`。阈值表达式详细帮助见 [阈值表达式帮助](https://hertzbeat.com/docs/help/alert_threshold_expr)
   **告警级别**：触发阈值的告警级别,从低到高依次为:警告-warning，严重-critical，紧急-emergency
   **触发次数**：设置触发阈值多少次之后才会真正的触发告警
   **通知模版**：告警触发后发送的通知信息模版,模版环境变量见页面提示，例如：`${app}.${metrics}.${metric}指标的值为${responseTime}，大于50触发告警`
   **全局默认**： 设置此阈值是否对全局的此类指标都应用有效，默认否。新增阈值后还需将阈值与监控对象关联，这样阈值才会对此监控生效。
   **启用告警**：此告警阈值配置开启生效或关闭

2. **阈值关联监控 【告警配置】-> 【将刚设置的阈值】-> 【配置关联监控】-> 【配置后确定】**

   > **注意新增阈值后还需将阈值与监控对象关联(即设置此阈值对哪些监控有效)，这样阈值才会对此监控生效** 。

   ![threshold](https://hertzbeat.com/assets/images/alert-threshold-2-c642d543791b5a6a326ec75f3900582a.png)

   ![threshold](https://hertzbeat.com/assets/images/alert-threshold-3-d92f01483904b4c76a459e6e095a2292.png)

   **阈值告警配置完毕，已经被成功触发的告警信息可以在【告警中心】看到。**
   **若需要将告警信息邮件，微信，钉钉飞书通知给相关人员，可以在【告警通知】配置。**

#### 阈值表达式

> 在我们配置阈值告警时，需要配置阈值触发表达式，系统根据表达式和监控指标值计算触发是否告警，这里详细介绍下表达式使用。

##### 表达式支持的操作符

```undefined
equals(str1,str2) 
==
<
<=
>
>=
!=
( )
+
-
&&
||
```

Copy

丰富的操作符让我们可以很自由的定义表达式。
注意字符串的相等请用 `equals(str1,str2)` 数字类型的相等判断请用== 或 !=

##### 支持的环境变量

> 环境变量即指标值等支持的变量，用于在表达式中，阈值计算判断时会将变量替换成实际值进行计算

非固定环境变量：这些变量会根据我们选择的监控指标对象而动态变化，例如我们选择了**网站监控的响应时间指标**，则环境变量就有 `responseTime - 此为响应时间变量`
如果我们想设置**网站监控的响应时间大于400时**触发告警，则表达式为 `responseTime>400`

固定环境变量(不常用)：`instance : 所属行实例值`
此变量主要用于计算多实例时，比如采集到c盘d盘的`usage`(`usage为非固定环境变量`),我们只想设置**c盘的usage大于80**时告警，则表达式为 `equals(instance,"c")&&usage>80`

##### 表达式设置案例

1. 网站监控->响应时间大于等于400ms时触发告警
   `responseTime>=400`
2. API监控->响应时间大于3000ms时触发告警
   `responseTime>3000`
3. 全站监控->URL(instance)路径为 `https://baidu.com/book/3` 的响应时间大于200ms时触发告警
   `equals(instance,"https://baidu.com/book/3")&&responseTime>200`
4. MYSQL监控->status指标组->threads_running(运行线程数)指标大于7时触发告警
   `threads_running>7`

### 告警通知

#### 配置邮箱通知

> 阈值触发后发送告警信息，通过邮件通知到接收人。

##### 操作步骤

1. **【告警通知】->【新增接收人】 ->【选择邮件通知方式】**

   ![](https://hertzbeat.com/assets/images/alert-notice-1-97b12cf267f0d5737ce04a5e67d04a66.png)

2. **【获取验证码】-> 【输入邮箱验证码】-> 【确定】**

   ![](https://hertzbeat.com/assets/images/alert-notice-2-06cca23b9fdf814816dcd34e96b5c67b.png)

   ![email](https://hertzbeat.com/assets/images/alert-notice-3-c18a90e98e1af7ed78bba845ca5b535f.png)

3. **配置关联的告警通知策略【新增通知策略】-> 【将刚设置的接收人关联】-> 【确定】**

   > **注意新增了接收人并不代表已经生效可以接收告警信息，还需配置关联的告警通知策略，即指定哪些消息发给哪些接收人** 。

   ![email](https://hertzbeat.com/assets/images/alert-notice-4-7b968f3d348ff468ad66fd59466be545.png)

   ##### 邮件通知常见问题

   1. 自己内网部署的HertzBeat无法接收到邮件通知

      > HertzBeat需要自己配置邮件服务器，TanCloud无需，请确认是否在application.yml配置了自己的邮件服务器

   2. 云环境TanCloud无法接收到邮件通知

      > 请排查在告警中心是否已有触发的告警信息
      > 请排查是否配置正确邮箱，是否已配置告警策略关联
      > 请查询邮箱的垃圾箱里是否把告警邮件拦截

#### 配置WebHook通知

> 阈值触发后发送告警信息，通过post请求方式调用WebHook接口通知到接收人。

##### 操作步骤

1. **【告警通知】->【新增接收人】 ->【选择WebHook通知方式】-> 【设置WebHook回调地址】 -> 【确定】**![email](https://hertzbeat.com/assets/images/alert-notice-5-2a87516f9ad552183d6f7d715e55f4af.png)

2. **配置关联的告警通知策略【新增通知策略】-> 【将刚设置的接收人关联】-> 【确定】**

   *注意新增了接收人并不代表已经生效可以接收告警信息，还需配置关联的告警通知策略，即指定哪些消息发给哪些接收人** 。![email](https://hertzbeat.com/assets/images/alert-notice-4-7b968f3d348ff468ad66fd59466be545.png)

##### WebHook回调POST请求体BODY内容

内容格式：JSON

```json
{
    "id":76456,
    "target":"available",
    "monitorId":5739609486000128,
    "monitorName":"API_poetry.apiopen.top",
    "priority":0,
    "content":"监控紧急可用性告警: UN_CONNECTABLE",
    "status":0,
    "times":1,
    "tenantId":10000,
    "gmtCreate":"2022-02-25T13:32:13",
    "gmtUpdate":"2022-02-25T13:32:13"
}
```

##### webhook通知常见问题

WebHook回调未生效

> 请查看告警中心是否已经产生此条告警信息
> 请排查配置的WebHook回调地址是否正确

#### 配置企业微信机器人通知

> 阈值触发后发送告警信息，通过企业微信机器人通知到接收人。

##### 操作步骤

1. **【企业微信端】-> 【群设置】-> 【群机器人】-> 【添加新建机器人】-> 【设置机器人名称头像】-> 【添加成功后复制其WebHook地址】**

   <img src="https://hertzbeat.com/assets/images/alert-notice-6-d706cd903604bb21c0a7b0a313d63368.jpg" alt="email" style="zoom: 33%;" />

2. **【保存机器人的WebHook地址的KEY值】**

   > 例如： webHook地址：`https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=3adafc96-23d0-4cd5-8feb-17f6e0b5fcs4`
   > 其机器人KEY值为 `3adafc96-23d0-4cd5-8feb-17f6e0b5fcs4`

3. **【告警通知】->【新增接收人】 ->【选择企业微信机器人通知方式】->【设置企业微信机器人KEY】-> 【确定】**

   ![email](https://hertzbeat.com/assets/images/alert-notice-7-567edc108fd5e9943fdf60d00229d223.png)

4. **配置关联的告警通知策略【新增通知策略】-> 【将刚设置的接收人关联】-> 【确定】**

   > **注意新增了接收人并不代表已经生效可以接收告警信息，还需配置关联的告警通知策略，即指定哪些消息发给哪些接收人** 。

   ![email](https://hertzbeat.com/assets/images/alert-notice-4-7b968f3d348ff468ad66fd59466be545.png)

##### 企业微信机器人通知常见问题

企业微信群未收到机器人告警通知

> 请排查在告警中心是否已有触发的告警信息
> 请排查是否配置正确机器人KEY，是否已配置告警策略关联

#### 配置钉钉机器人通知

> 阈值触发后发送告警信息，通过钉钉机器人通知到接收人。

##### 操作步骤

1. **【钉钉桌面客户端】-> 【群设置】-> 【智能群助手】-> 【添加新建机器人-选自定义】-> 【设置机器人名称头像】-> 【注意设置自定义关键字: TanCloud】 ->【添加成功后复制其WebHook地址】**

   > 注意新增机器人时需在安全设置块需设置其自定义关键字: TanCloud ，其它安全设置加签或IP段不填写

   ![email](https://tancloud.cn/assets/images/alert-notice-8-197567d52f856c256a3ea2fe098f9c71.png)

2. **【保存机器人的WebHook地址access_token值】**

   > 例如： webHook地址：`https://oapi.dingtalk.com/robot/send?access_token=43aac28a236e001285ed84e473f8eabee70f63c7a70287acb0e0f8b65fade64f`
   > 其机器人access_token值为 `43aac28a236e001285ed84e473f8eabee70f63c7a70287acb0e0f8b65fade64f`

3. **【告警通知】->【新增接收人】 ->【选择钉钉机器人通知方式】->【设置钉钉机器人ACCESS_TOKEN】-> 【确定】**

   ![email](https://tancloud.cn/assets/images/alert-notice-9-fceafe37c9566f50bf6dd2b151b0dcb8.png)

4. **配置关联的告警通知策略【新增通知策略】-> 【将刚设置的接收人关联】-> 【确定】**

   > **注意新增了接收人并不代表已经生效可以接收告警信息，还需配置关联的告警通知策略，即指定哪些消息发给哪些接收人** 。

   ![email](https://tancloud.cn/assets/images/alert-notice-4-7b968f3d348ff468ad66fd59466be545.png)

##### 钉钉机器人通知常见问题

钉钉群未收到机器人告警通知

> 请排查在告警中心是否已有触发的告警信息
> 请排查钉钉机器人是否配置了安全自定义关键字：TanCloud
> 请排查是否配置正确机器人ACCESS_TOKEN，是否已配置告警策略关联

#### 配置飞书机器人通知

> 阈值触发后发送告警信息，通过飞书机器人通知到接收人。

##### 操作步骤

1. **【飞书客户端】-> 【群设置】-> 【群机器人】-> 【添加新建机器人】-> 【设置机器人名称头像】-> 【添加成功后复制其WebHook地址】**

2. **【保存机器人的WebHook地址的KEY值】**

   > 例如： webHook地址：`https://open.feishu.cn/open-apis/bot/v2/hook/3adafc96-23d0-4cd5-8feb-17f6e0b5fcs4`
   > 其机器人KEY值为 `3adafc96-23d0-4cd5-8feb-17f6e0b5fcs4`

3. **【告警通知】->【新增接收人】 ->【选择飞书机器人通知方式】->【设置飞书机器人KEY】-> 【确定】**

4. **配置关联的告警通知策略【新增通知策略】-> 【将刚设置的接收人关联】-> 【确定】**

   > **注意新增了接收人并不代表已经生效可以接收告警信息，还需配置关联的告警通知策略，即指定哪些消息发给哪些接收人** 。

   ![email](https://tancloud.cn/assets/images/alert-notice-4-7b968f3d348ff468ad66fd59466be545.png)

##### 飞书机器人通知常见问题

飞书群未收到机器人告警通知

> 请排查在告警中心是否已有触发的告警信息
> 请排查是否配置正确机器人KEY，是否已配置告警策略关联