---
layout: post
title: EFK 搭建使用
category: 大数据
tags: efk elk
bg: efk.png
---

# 2020-01-19-efk

全文检索的应用越来越广泛，几乎成了互联网应用的标配.

商品搜索、日志分析、历史数据归档等等，各种场景都会涉及到大批量的数据，在全文检索方面，方案无外乎Lucene、Solr、Elasticsearch三种应用的较为广泛。es、solr的底层都依托于Lucene，但es比solr学习成本更低，由于其提供的RESTful API简单快捷，对互联网应用开发而言更是如虎添翼。 Elasticsearch集群配置，集成spring boot，开发记录.

## ElasticSearch

### 安装Java

```text
yum -y install java
```

### 手动安装

```text
# 下载
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.5.4.rpm
# 安装
rpm -ivh elasticsearch-6.5.4.rpm # yum -y install elasticsearch-6.5.4.rpm
# 启动服务
systemctl start elasticsearch.service # 启动
systemctl enable elasticsearch.service # 开机启动
```

### docker 安装

```text
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" --restart always elasticsearch:6.5.4
```

### root用户注意事项

非rpm安装启动elasticsearch是不允许在root用户下操作的，如果在root用户下启动

```text
cd elasticsearch-6.5.4
./bin/elasticsearch
```

会提示如下错误

```text
java.lang.RuntimeException: can not run elasticsearch as root
```

因此我们可以在root用户下，创建新用户组及用户，在elasticsearch-6.5.4同级目录下

```text
groupadd esgroup # 创建用户组
useradd esuser -g esgroup # 创建用户并指定分组
chown -R esuser:esgroup elasticsearch-6.5.4 # 更改es的权限到新建用户和分组
```

用户组及用户创建完毕，权限也提升之后，切换到esuser，启动elasticsearch即可（**此时ES的配置文件没有做任何更改**）

```text
su - esuser（不能使用su esuser命令，否则刚才配置的java环境变量在esuser用户下不会生效）
cd elasticsearch-6.5.4
./bin/elasticsearch（可以在后面加 -d，即./bin/elasticsearch -d，可以后台运行）
```

### 测试

```text
curl http://localhost:9200/?pretty
# 打印出es信息如下
{
  "name" : "v1",
  "cluster_name" : "imjcker-es-cluster",
  "cluster_uuid" : "vAgh7MQVTqeu1_RkzT8ZdQ",
  "version" : {
    "number" : "6.5.4",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "d2ef93d",
    "build_date" : "2018-12-17T21:17:40.758843Z",
    "build_snapshot" : false,
    "lucene_version" : "7.5.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

### 集群搭建

```text
vi /etc/elasticsearch/elasticsearch.yml
# 修改配置
# 1.集群名字，每个节点统一
cluster.name: imjcker-es-cluster
# 2.节点名字，每个节点不一样
node.name: node-1
# 3.节点IP, 改为 0.0.0.0
network.host: 0.0.0.0
# 4.集群节点配置，改为给个节点的IP地址
discovery.zen.ping.unicast.hosts: ["172.16.11.130","172.16.11.131","172.16.11.132"]
```

#### 开放端口

```text
# 如果是开发环境，不在乎安全，可直接关闭防火墙
systemctl stop firewalld
systemctl disable firewalld

# 不关闭防火墙，开放端口
# 开放elasticsearch默认的客户端端口和集群通信端口
firewall-cmd --zone=public --add-port=9200/tcp --permanent
firewall-cmd --zone=public --add-port=9300/tcp --permanent
firewall-cmd --reload
```

#### 重启服务

```text
# 重启每个节点
systemctl restart elasticsearch
```

#### 测试

```text
curl http://localhost:9200/_cluster/health?pretty
# 打印内容类似如下
{
  "cluster_name" : "imjcker-es-cluster",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 3,
  "number_of_data_nodes" : 3,
  "active_primary_shards" : 0,
  "active_shards" : 0,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
```

### 工具和其它

创建360的ElasticHD容器

1. 建议安装 [Elasticsearch Head](https://chrome.google.com/webstore/detail/elasticsearch-head/ffmkiejjmecolpfloofpjologoblkegm) 浏览器插件，便于开发查询数据。
2. es集群管理 [https://github.com/lmenezes/elasticsearch-kopf](https://github.com/lmenezes/elasticsearch-kopf)
3. es集群监控 [https://github.com/lukas-vlcek/bigdesk](https://github.com/lukas-vlcek/bigdesk) 
4. 中文分词器 ik-analyzer [https://github.com/medcl/elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik)
5. 高亮查询问题的解决（特定版本）
6. 解决满足业务的查询问题（主要是涉及到权限问题）
7. 查询解决方案

说明：只要前一步骤查询不为空就结束查询，否则进入下一步查询

1. matchPhraseQuery（不分词查询）slop设置匹配度
2. matchQuery（分词匹配查询）
3. termQuery（完全匹配查询）
4. findAllByContentContains（利用spring boot自带功能ElasticsearchRepository查询，类似于jpa）
5. 代码

```java
public class ElasticSearch{
    public Page search(String ajbm, String keyword, Pageable pageable) {
            //在辅助工具中查询
            if (StringUtils.isEmpty(ajbm)) {
                Page<Doc> page = matchPhraseQuery(keyword, pageable);
                if (page != null) {
                    return page;
                } else {
                    page = matchQuery(keyword, "", pageable);
                    if (page == null)
                        page = termQuery(keyword, pageable);
                    if (page == null) {
                        Page<Doc> docPage = docEsRepository.findAllByContentContains(keyword, pageable);
                        if (docPage != null) {
                            page = docPage.map(source -> source);
                        }
                    }
                }
                return page;
            }
    }
}
```

1. 配置说明
2. 配置1

配置文件2.0版

配置文件位于%ES\_HOME%/config/elasticsearch.yml文件中，用Editplus打开它，你便可以进行配置。 所有的配置都可以使用环境变量，例如： node.rack: ${RACK\_ENV\_VAR} 表示环境变量中有一个RACK\_ENV\_VAR变量。 下面列举一下elasticsearch的可配置项：

1. 集群名称，默认为elasticsearch：

   cluster.name: elasticsearch

2. 节点名称，es启动时会自动创建节点名称，但你也可进行配置：

   node.name: "Franz Kafka"

3. 是否作为主节点，每个节点都可以被配置成为主节点，默认值为true：

   node.master: true

4. 是否存储数据，即存储索引片段，默认值为true：

   node.data: true

   master和data同时配置会产生一些奇异的效果：

   1\) 当master为false，而data为true时，会对该节点产生严重负荷；

   2\) 当master为true，而data为false时，该节点作为一个协调者；

   3\) 当master为false，data也为false时，该节点就变成了一个负载均衡器。

   你可以通过连接[http://localhost:9200/\_cluster/health或者http://localhost:9200/\_cluster/nodes，或者使用插件http://github.com/lukas-vlcek/bigdesk或http://mobz.github.com/elasticsearch-head来查看集群状态。](http://localhost:9200/_cluster/health或者http://localhost:9200/_cluster/nodes，或者使用插件http://github.com/lukas-vlcek/bigdesk或http://mobz.github.com/elasticsearch-head来查看集群状态。)

5. 每个节点都可以定义一些与之关联的通用属性，用于后期集群进行碎片分配时的过滤：

   node.rack: rack314

6. 默认情况下，多个节点可以在同一个安装路径启动，如果你想让你的es只启动一个节点，可以进行如下设置：

   node.max\_local\_storage\_nodes: 1

7. 设置一个索引的碎片数量，默认值为5：

   index.number\_of\_shards: 5

8. 设置一个索引可被复制的数量，默认值为1：

   index.number\_of\_replicas: 1

   当你想要禁用公布式时，你可以进行如下设置：

   index.number\_of\_shards: 1

   index.number\_of\_replicas: 0

   这两个属性的设置直接影响集群中索引和搜索操作的执行。假设你有足够的机器来持有碎片和复制品，那么可以按如下规则设置这两个值：

   1\) 拥有更多的碎片可以提升索引执行能力，并允许通过机器分发一个大型的索引；

   2\) 拥有更多的复制器能够提升搜索执行能力以及集群能力。

   对于一个索引来说，number\_of\_shards只能设置一次，而number\_of\_replicas可以使用索引更新设置API在任何时候被增加或者减少。

   ElasticSearch关注加载均衡、迁移、从节点聚集结果等等。可以尝试多种设计来完成这些功能。

   可以连接[http://localhost:9200/A/\_status来检测索引的状态。](http://localhost:9200/A/_status来检测索引的状态。)

9. 配置文件所在的位置，即elasticsearch.yml和logging.yml所在的位置：

   path.conf: /path/to/conf

10. 分配给当前节点的索引数据所在的位置：

    path.data: /path/to/data

    可以可选择的包含一个以上的位置，使得数据在文件级别跨越位置，这样在创建时就有更多的自由路径，如：

    path.data: /path/to/data1,/path/to/data2

11. 临时文件位置：

    path.work: /path/to/work

12. 日志文件所在位置：

    path.logs: /path/to/logs

13. 插件安装位置：

    path.plugins: /path/to/plugins

14. 插件托管位置，若列表中的某一个插件未安装，则节点无法启动：

    plugin.mandatory: mapper-attachments,lang-groovy

15. JVM开始交换时，ElasticSearch表现并不好：你需要保障JVM不进行交换，可以将bootstrap.mlockall设置为true禁止交换：

    bootstrap.mlockall: true

    请确保ES\_MIN\_MEM和ES\_MAX\_MEM的值是一样的，并且能够为ElasticSearch分配足够的内在，并为系统操作保留足够的内存。

16. 默认情况下，ElasticSearch使用0.0.0.0地址，并为http传输开启9200-9300端口，为节点到节点的通信开启9300-9400端口，也可以自行设置IP地址：

    network.bind\_host: 192.168.0.1

17. publish\_host设置其他节点连接此节点的地址，如果不设置的话，则自动获取，publish\_host的地址必须为真实地址：

    network.publish\_host: 192.168.0.1

18. bind\_host和publish\_host可以一起设置：

    network.host: 192.168.0.1

19. 可以定制该节点与其他节点交互的端口：

    transport.tcp.port: 9300

20. 节点间交互时，可以设置是否压缩，转为为不压缩：

    transport.tcp.compress: true

21. 可以为Http传输监听定制端口：

    http.port: 9200

22. 设置内容的最大长度：

    http.max\_content\_length: 100mb

23. 禁止HTTP

    http.enabled: false

24. 网关允许在所有集群重启后持有集群状态，集群状态的变更都会被保存下来，当第一次启用集群时，可以从网关中读取到状态，默认网关类型（也是推荐的）是local：

    gateway.type: local

25. 允许在N个节点启动后恢复过程：

    gateway.recover\_after\_nodes: 1

26. 设置初始化恢复过程的超时时间：

    gateway.recover\_after\_time: 5m

27. 设置该集群中可存在的节点上限：

    gateway.expected\_nodes: 2

28. 设置一个节点的并发数量，有两种情况，一种是在初始复苏过程中：

    cluster.routing.allocation.node\_initial\_primaries\_recoveries: 4

    另一种是在添加、删除节点及调整时：

    cluster.routing.allocation.node\_concurrent\_recoveries: 2

29. 设置复苏时的吞吐量，默认情况下是无限的：

    indices.recovery.max\_size\_per\_sec: 0

30. 设置从对等节点恢复片段时打开的流的数量上限：

    indices.recovery.concurrent\_streams: 5

31. 设置一个集群中主节点的数量，当多于三个节点时，该值可在2-4之间：

    discovery.zen.minimum\_master\_nodes: 1

32. 设置ping其他节点时的超时时间，网络比较慢时可将该值设大：

    discovery.zen.ping.timeout: 3s

    [http://elasticsearch.org/guide/reference/modules/discovery/zen.html上有更多关于discovery的设置。](http://elasticsearch.org/guide/reference/modules/discovery/zen.html上有更多关于discovery的设置。)

33. 禁止当前节点发现多个集群节点，默认值为true：

    discovery.zen.ping.multicast.enabled: false

34. 设置新节点被启动时能够发现的主节点列表（主要用于不同网段机器连接）： discovery.zen.ping.unicast.hosts: \["host1", "host2:port", "host3\[portX-portY\]"\] 35.设置是否可以通过正则或者\_all删除或者关闭索引 action.destructive\_requires\_name 默认false 允许 可设置true不允许
35. 配置2

elasticsearch的config文件夹里面有两个配置文件：elasticsearch.yml和logging.yml，第一个是es的基本配置文件，第二个是日志配置文件，es也是使用log4j来记录日志的，所以logging.yml里的设置按普通log4j配置文件来设置就行了。下面主要讲解下elasticsearch.yml这个文件中可配置的东西。

cluster.name: elasticsearch 配置es的集群名称，默认是elasticsearch，es会自动发现在同一网段下的es，如果在同一网段下有多个集群，就可以用这个属性来区分不同的集群。

node.name: "Franz Kafka" 节点名，默认随机指定一个name列表中名字，该列表在es的jar包中config文件夹里name.txt文件中，其中有很多作者添加的有趣名字。

node.master: true 指定该节点是否有资格被选举成为node，默认是true，es是默认集群中的第一台机器为master，如果这台机挂了就会重新选举master。

node.data: true 指定该节点是否存储索引数据，默认为true。

index.number\_of\_shards: 5 设置默认索引分片个数，默认为5片。

index.number\_of\_replicas: 1 设置默认索引副本个数，默认为1个副本。

path.conf: /path/to/conf 设置配置文件的存储路径，默认是es根目录下的config文件夹。

path.data: /path/to/data 设置索引数据的存储路径，默认是es根目录下的data文件夹，可以设置多个存储路径，用逗号隔开，例： path.data: /path/to/data1,/path/to/data2

path.work: /path/to/work 设置临时文件的存储路径，默认是es根目录下的work文件夹。

path.logs: /path/to/logs 设置日志文件的存储路径，默认是es根目录下的logs文件夹

path.plugins: /path/to/plugins 设置插件的存放路径，默认是es根目录下的plugins文件夹

bootstrap.mlockall: true 设置为true来锁住内存。因为当jvm开始swapping时es的效率会降低，所以要保证它不swap，可以把ES\_MIN\_MEM和ES\_MAX\_MEM两个环境变量设置成同一个值，并且保证机器有足够的内存分配给es。同时也要允许elasticsearch的进程可以锁住内存，linux下可以通过`ulimit -l unlimited`命令。

network.bind\_host: 192.168.0.1 设置绑定的ip地址，可以是ipv4或ipv6的，默认为0.0.0.0。

network.publish\_host: 192.168.0.1 设置其它节点和该节点交互的ip地址，如果不设置它会自动判断，值必须是个真实的ip地址。

network.host: 192.168.0.1 这个参数是用来同时设置bind\_host和publish\_host上面两个参数。

transport.tcp.port: 9300 设置节点间交互的tcp端口，默认是9300。

transport.tcp.compress: true 设置是否压缩tcp传输时的数据，默认为false，不压缩。

http.port: 9200 设置对外服务的http端口，默认为9200。

http.max\_content\_length: 100mb 设置内容的最大容量，默认100mb

http.enabled: false 是否使用http协议对外提供服务，默认为true，开启。

gateway.type: local gateway的类型，默认为local即为本地文件系统，可以设置为本地文件系统，分布式文件系统，hadoop的HDFS，和amazon的s3服务器，其它文件系统的设置方法下次再详细说。

gateway.recover\_after\_nodes: 1 设置集群中N个节点启动时进行数据恢复，默认为1。

gateway.recover\_after\_time: 5m 设置初始化数据恢复进程的超时时间，默认是5分钟。

gateway.expected\_nodes: 2 设置这个集群中节点的数量，默认为2，一旦这N个节点启动，就会立即进行数据恢复。

cluster.routing.allocation.node\_initial\_primaries\_recoveries: 4 初始化数据恢复时，并发恢复线程的个数，默认为4。

cluster.routing.allocation.node\_concurrent\_recoveries: 2 添加删除节点或负载均衡时并发恢复线程的个数，默认为4。

indices.recovery.max\_size\_per\_sec: 0 设置数据恢复时限制的带宽，如入100mb，默认为0，即无限制。

indices.recovery.concurrent\_streams: 5 设置这个参数来限制从其它分片恢复数据时最大同时打开并发流的个数，默认为5。

discovery.zen.minimum\_master\_nodes: 1 设置这个参数来保证集群中的节点可以知道其它N个有master资格的节点。默认为1，对于大的集群来说，可以设置大一点的值（2-4）

discovery.zen.ping.timeout: 3s 设置集群中自动发现其它节点时ping连接超时时间，默认为3秒，对于比较差的网络环境可以高点的值来防止自动发现时出错。

discovery.zen.ping.multicast.enabled: false 设置是否打开多播发现节点，默认是true。

discovery.zen.ping.unicast.hosts: \["host1", "host2:port", "host3\[portX-portY\]"\] 设置集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点。

下面是一些查询时的慢日志参数设置 index.search.slowlog.level: TRACE index.search.slowlog.threshold.query.warn: 10s index.search.slowlog.threshold.query.info: 5s index.search.slowlog.threshold.query.debug: 2s index.search.slowlog.threshold.query.trace: 500ms

index.search.slowlog.threshold.fetch.warn: 1s index.search.slowlog.threshold.fetch.info: 800ms index.search.slowlog.threshold.fetch.debug:500ms index.search.slowlog.threshold.fetch.trace: 200ms

## Kibana

1. 下载

```text
wget https://artifacts.elastic.co/downloads/kibana/kibana-6.5.4-x86_64.rpm
```

1. 安装

```text
yum -y install kibana-6.5.4-x86_64.rpm
```

1. 配置

```text
vi /etc/kibana/kibana.yml

server.host: "0.0.0.0"
elasticsearch.url: "http://elasticsearch-host:9200"
```

1. 启动

```text
systemctl start kibana
```

1. 访问

[http://kibanahost:5601](http://kibanahost:5601)

## Filebeat

1. 下载

```text
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.5.4-x86_64.rpm
```

1. 安装

```text
yum -y install filebeat-6.5.4-x86_64.rpm
```

1. 配置

```text
vi /etc/filebeat/filebeat.yml
```

```yaml
enabled: true
paths: /log-directory/*.log
output.elasticsearch:
  hosts: ["http://xxxx.imjcker.com:9200"]
```

1. 启动

```text
systemctl start filebeat
```

1. 测试

到kibana的管理界面搜索关键字

