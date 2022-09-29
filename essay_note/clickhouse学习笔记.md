# ClickHouse学习笔记



ClickHouse是一个用于联机分析（OLAP）的列式数据库管理系统（DBMS）

**补充1：**

OLAP与OLTP的区别：

OLAP（On-Line Analytical Processing）联机分析处理，也称为面向交易的处理过程，其基本特征是前台接收的用户数据可以立即传送到计算中心进行处理，并在很短的时间内给出处理结果，是对用户操作快速响应的方式之一。应用在数据仓库，使用对象是决策者。OLAP系统强调的是数据分析，响应速度要求没那么高。

OLTP（On-Line Transaction Processing）联机事务处理，它使分析人员能够迅速、一致、交互地从各个方面观察信息，以达到深入理解数据的目的。它具有FASMI(Fast Analysis of Shared Multidimensional Information)，即共享多维信息的快速分析的特征。主要应用是传统关系型数据库。OLTP系统强调的是内存效率，实时性比较高

<img src="https://img2018.cnblogs.com/blog/1275932/201810/1275932-20181009110146392-29346933.png" alt="img" style="zoom:150%;" />

OLAP的数据有hive、impala、clickhouse等，OLTP的数据库有MySQL、Oracle等

### ClickHouse的特性： 

#### 1、真正的列式数据库管理系统

除了数据本身不存在其他额外数据（如数据的长度等），并且ch允许在运行时创建表和数据库、加载数据和运行查询，而无需重新配置或重启服务。

#### 2、数据压缩

压缩方式：LZ4（默认）和ZSTD两种压缩算法

LZ4在速度上会更快，但是压缩率较低，ZSTD正好相反。

LZ4的压缩比能达到将近4，而ZSTD的压缩比能达到5。我们集群使用的是默认配置。

但实际的压缩比要高很多，达到了9.（测试环境）

#### 3、数据的磁盘存储

#### 4、多核心并行处理

利用服务器的一切可用资源进行并行处理

#### 5、多服务器分布式处理

在ClickHouse中，数据可以保存在不同的shard上，每一个shard都由一组用于容错的replication组成，查询可以并行地在所有shard上进行处理。这些对用户来说是透明的。

#### 6、支持SQL

ClickHouse支持基于SQL的声明式查询语言，该语言大部分情况下是与SQL标准兼容的。 支持的查询包括 GROUP BY，ORDER BY，IN，JOIN以及非相关子查询。 

不支持窗口函数和相关子查询。

#### 7、向量引擎

为了高效的使用CPU，数据不仅仅按列存储，同时还按向量(列的一部分)进行处理，这样可以更加高效地使用CPU。

#### 8、实时的数据更新

ClickHouse支持在表中定义主键。为了使查询能够快速在主键中进行范围查找，数据总是以增量的方式有序的存储在MergeTree中。因此，数据可以持续不断地高效的写入到表 中，并且写入的过程中不会存在任何加锁的行为。 

#### 9、索引

按照主键对数据进行排序，这将帮助ClickHouse在几十毫秒以内完成对数据特定值或范围的查找。 

#### 10、适合在线查询 

在线查询意味着在没有对数据做任何预处理的情况下以极低的延迟处理查询并将结果加载到用户的页面中。 

#### 11、支持近似计算 

ClickHouse提供各种各样在允许牺牲数据精度的情况下对查询进行加速的方法： 

①用于近似计算的各类聚合函数，如：distinct values, medians, quantiles 

②基于数据的部分样本进行近似查询。这时，仅会从磁盘检索少部分比例的数据。 

③不使用全部的聚合条件，通过随机选择有限个数据聚合条件进行聚合。这在数据聚合条件满足某些分布条件下，在提供相当准确的聚合结果的同时降低了计算资源的使用。 

#### 12、支持数据复制和数据完整性 

ClickHouse使用异步的多主复制技术。当数据被写入任何一个可用副本后，系统会在后台将数据分发给其他副本，以保证系统在不同副本上保持相同的数据。在大多数情况下 ClickHouse能在故障后自动恢复，在一些少数的复杂情况下需要手动恢复。



### ClickHouse的限制： 

#### 1、没有完整的事务支持。 

#### 2、缺少高频率，低延迟的修改或删除已存在数据的能力。仅能用于批量删除或修改数据，但这符合 GDPR。 

#### 3、稀疏索引使得ClickHouse不适合通过其键检索单行的点查询。 



<u>备注：GDPR(通用数据保护条例)，主要是为了保护数据隐私而制定的保护条例。</u>

### ClickHouse的性能：

#### 1、单个大查询的吞吐量

如果数据放在page cache中，一个不太复杂的查询在单个服务器上能以2-10GB/s（未压缩）的速度进行处理（对于简单的查询，可以达到30GB/s）。

如果数据未放在page cache中，速度取决于磁盘系统和数据的压缩率。例如，如果一个磁盘允许以 400MB／s的速度读取数据，并且数据压缩率是3，则数据的处理速度为1.2GB/s。

对于分布式处理，处理速度几乎是线性扩展的，但这受限于聚合或排序的结果不是那么大的情况下。

#### 2、处理短查询的延迟时间

延迟时间 = 查找时间（10ms） \* 查询的列的数量 \* 查询的数据列的数量

#### 3、处理大量短查询的吞吐量

在相同的情况下，ClickHouse可以在单个服务器上每秒处理数百个查询（在最佳的情况下最多可以处理数千个）。但是由于这不适用于分析型场景。因此建议每秒最多查询 100次。 

#### 4、数据的写入性能

建议每次写入不少于1000行的批量写入，或每秒不超过一个写入请求。

为了提高写入性能，您可以使用多个INSERT进行并行写入，这将带来线性的性能提升。



### ClickHouse引擎介绍：

#### 1、延时引擎Lazy

在距最近一次访问间隔expiration_time_in_seconds时间段内，将表保存在内存中，仅适用于 *Log引擎表

```sql
CREATE DATABASE testlazy ENGINE = Lazy(expiration_time_in_seconds); 
```



#### 2、MySQL

MySQL引擎用于将远程的MySQL服务器中的表映射到ClickHouse中，并允许对表进行INSERT和SELECT查询，方便在ClickHouse与MySQL之间进行数据交换。 

MySQL数据库引擎会将对其的查询转换为MySQL语法并发送到MySQL服务器中，因此可以执行诸如SHOW TABLES或SHOW CREATE TABLE之类的操作。 

但以下几种操作无法执行：

```
RENAME

CREATE TABLE

ALTER
```

创建库：

```mysql
CREATE DATABASE [IF NOT EXISTS] db_name [ON CLUSTER cluster] ENGINE = MySQL('host:port', ['database' | database], 'user', 'password')
```

MySQL数据库引擎参数介绍：

```
host:port — 链接的MySQL地址。 

database — 链接的MySQL数据库。 

user — 链接的MySQL用户。 

password — 链接的MySQL用户密码。
```

支持的类型对应关系如下表：

![image-20210222135715839](C:\Users\jg\AppData\Roaming\Typora\typora-user-images\image-20210222135715839.png)



#### 3、VersionedCollapsingMergeTree

版本折叠MergeTree，这个引擎的允许快速写入不断变化的对象状态，删除后台中的旧对象状态，可以降低储存体积。

允许以多个线程的任何顺序插入数据。特别是， Version 列有助于正确折叠行，即使它们以错误的顺序插入。

相比之下, **CollapsingMergeTree** 只允许严格连续插入。

```sql
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster] 
( 
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1], 
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2], 
    ... 
) ENGINE = VersionedCollapsingMergeTree(sign, version) 
[PARTITION BY expr] 
[ORDER BY expr] 
[SAMPLE BY expr] 
[SETTINGS name=value, ...]
```



```
VersionedCollapsingMergeTree(sign,version) 

sign — 指定行类型的列名: 1 是一个 “state” 行, -1 是一个 “cancel” 划 

列数据类型应为 Int8. 

version — 指定对象状态版本的列名。 

列数据类型应为 UInt*
```

##### 折叠：

数据

对应数据库管理系统来说，更新操作很麻烦，因此可以用以下方法对目标对象进行修改。

使用 Sign 列写入行时。 如果 Sign = 1 这意味着该行是一个对象的状态（让我们把它称为 “state” 行）。 如果 Sign = -1 它指示具有相同属性的对象的状态的取消（让我们称之为 “cancel” 行）。 还可以使用 Version 列，它应该用单独的数字标识对象的每个状态。 

![image-20210222151018313](C:\Users\jg\AppData\Roaming\Typora\typora-user-images\image-20210222151018313.png)

算法

当ClickHouse合并数据部分时，它会删除具有相同主键和版本但 Sign值不同的一对行. 行的顺序并不重要。 

当ClickHouse插入数据时，它会按主键对行进行排序。 如果 Version 列不在主键中，ClickHouse将其隐式添加到主键作为最后一个字段并使用它进行排序。



#### 4、GraphiteMergeTree 

该引擎用来对 Graphite数据进行瘦身及汇总。对于想使用CH来存储Graphite数据的开发者来说可能有用。 

如果不需要对Graphite数据做汇总，那么可以使用任意的CH表引擎；但若需要，那就采用 GraphiteMergeTree 引擎。它能减少存储空间，同时能提高Graphite数据的查询效率。

```sql
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster] 
( 
    Path String, 
    Time DateTime, 
    Value <Numeric_type>, 
    Version <Numeric_type> ... 
) ENGINE = GraphiteMergeTree(config_section) 
[PARTITION BY expr] 
[ORDER BY expr] 
[SAMPLE BY expr] 
[SETTINGS name=value, ...]
```



#### 5、MergeTree

该引擎是ch中最大的表引擎，MergeTree 系列的引擎被设计用于插入极大量的数据到一张表当中。数据可以以数据片段的形式一个接着一个的快速写入，数据片段在后台按照一定的规则进行合并。相比在插入时不断修改（重写）已存储的数据，这种策略会高效很多。

特点：

- 存储的数据按主键排序。
- 支持数据分区，如果指定了分区键的话。
- 支持数据副本。 
- 支持数据采样。

```sql
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster] 
( 
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1] [TTL expr1], 
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2] [TTL expr2], 
    ... 
    INDEX index_name1 expr1 TYPE type1(...) GRANULARITY value1, 
    INDEX index_name2 expr2 TYPE type2(...) GRANULARITY value2 
) ENGINE = MergeTree() 
ORDER BY expr 
[PARTITION BY expr] 
[PRIMARY KEY expr] 
[SAMPLE BY expr] 
[TTL expr [DELETE|TO DISK 'xxx'|TO VOLUME 'xxx'], ...] 
[SETTINGS name=value, ...]
```

数据存储

1.当数据被插入到表中时，会创建多个数据片段并按主键的字典序排序。

2.不同分区的数据会被分成不同的片段，ClickHouse 在后台合并数据片段以便更高效存储。不同分区的数据片段不会进行合并。合并机制并不保证具有相同主键的行全都合并到同一个数据片段中。

3.数据片段可以以 Wide（分开存储） 或 Compact（整体存储） 格式存储。在 Wide 格式下，每一列都会在文件系统中存储为单独的文件，在 Compact 格式下所有列都存储在一个文件中。Compact 格式可以提高插入量少插入频率频繁时的性能。

4.数据存储格式由 min_bytes_for_wide_part 和 min_rows_for_wide_part表引擎参数控制。如果数据片段中的字节数或行数少于相应的设置值，数据片段会以 Compact格式存储，否则会以 Wide 格式存储。



ClickHouse 不要求主键惟一，所以我们可以插入多条具有相同主键的行。



#### 6、ReplacingMergeTree

该引擎和 MergeTree 的不同之处在于它会删除排序键值相同的重复项。目前我们线上的表格采用的是这种引擎。

引擎合并重复数据是在一个不确定的时间，因此我们不能完全依靠他。我们国际短信的去重操作在flink阶段就已经进行了初步的去重，因此能保证数据量的差异不会很大。

```sql
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster] 
( 
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1], 
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2], 
    ... 
) ENGINE = ReplacingMergeTree([ver]) 
[PARTITION BY expr] 
[ORDER BY expr] 
[SAMPLE BY expr] 
[SETTINGS name=value, ...]
```

重要参数：

ver — 版本列。类型为 UInt*, Date 或 DateTime。可选参数。 

在数据合并的时候，ReplacingMergeTree 从所有具有相同排序键的行中选择一行留下： 

- ​		如果 ver 列未指定，保留最后一条。 
- ​		如果 ver 列已指定，保留 ver 值最大的版本。 



#### 7、日志引擎TinyLog

最简单的表引擎，用于将数据存储在磁盘上。每列都存储在单独的压缩文件中。写入时，数据将附加到文件末尾。

这种表引擎的典型用法是 write-once：首先只写入一次数据，然后根据需要多次读取。查询在单个流中执行。换句话说，此引擎适用于相对较小的表（建议最多1,000,000 行）。如果您有许多小表，则使用此表引擎是适合的，因为它比Log引擎更简单（需要打开的文件更少）。



#### 8、JDBC

ch可以通过JDBC的方式连接外部的数据库。要实现JDBC连接，CH需要使用以后台进程运行的程序 clickhouse-jdbc-bridge。 

```sql
CREATE TABLE [IF NOT EXISTS] [db.]table_name 
( 
    columns list... 
)ENGINE = JDBC(dbms_uri, external_database, external_table)
```

```bash
dbms_uri — 外部DBMS的uri. 

​		格式: jdbc:<driver_name>://<host_name>:<port>/?user=<username>&password=<password>. 

​		MySQL示例: jdbc:mysql://localhost:3306/?user=root&password=root. 

external_database — 外部DBMS的数据库名. 

external_table — external_database中的外部表名.
```



#### 9、ODBC

ch可以通过ODBC的方式连接外部的数据库。要实现ODBC连接，CH需要使用以后台进程运行的程序 clickhouse-odbc-bridge。 

```sql
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster] 
( 
    name1 [type1], name2 [type2], ... 
)ENGINE = ODBC(connection_settings, external_database, external_table)
```



#### 10、HDFS

该引擎提供了集成 Apache Hadoop 生态系统通过允许管理数据 HDFS通过ClickHouse. 这个引擎是相似的到文件和URL引擎，但提供Hadoop特定的功能。

```sql
ENGINE = HDFS(URI, format)
```

该 URI 参数是HDFS中的整个文件URI。 

该 format 参数指定一种可用的文件格式。



#### 11、Kafka

```sql
Kafka SETTINGS 
    kafka_broker_list = 'localhost:9092', 
    kafka_topic_list = 'topic1,topic2', 
    kafka_group_name = 'group1', 
    kafka_format = 'JSONEachRow', 
    kafka_row_delimiter = '\n', 
    kafka_schema = '', 
    kafka_num_consumers = 2
```

```bash
必要参数：
    kafka_broker_list – 以逗号分隔的 brokers 列表 (localhost:9092)。 
    kafka_topic_list – topic 列表 (my_topic)。 
    kafka_group_name – Kafka 消费组名称 (group1)。如果不希望消息在集群中重复，请在每个分片中使用相同的组名。 
    kafka_format – 消息体格式。使用与 SQL 部分的 FORMAT 函数相同表示方法，例如 JSONEachRow。了解详细信息，请参考 Formats 部分。
可选参数：
    kafka_row_delimiter - 每个消息体（记录）之间的分隔符。 
    kafka_schema – 如果解析格式需要一个 schema 时，此参数必填。例如，普罗托船长 需要 schema 文件路径以及根对象 schema.capnp:Message 的名字。 	kafka_num_consumers – 单个表的消费者数量。默认值是：1，如果一个消费者的吞吐量不足，则指定更多的消费者。消费者的总数不应该超过 topic 中分区的数量，	因为每个分区只能分配一个消费者。
```

