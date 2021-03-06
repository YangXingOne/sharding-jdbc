+++
date = "2016-01-08T16:14:21+08:00"
title = "读写分离"
weight = 6
+++
# 读写分离

## 概念
为了缓解数据库压力，将写入和读取操作分离为不同数据源，写库称为主库，读库称为从库，一主库可配置多从库。

## 支持项
1. 提供了一主多从的读写分离配置，可配合分库分表使用。
1. 同一线程如果有写入操作，以后的读操作均从主库读取，用于保证同一线程中的数据一致性。

## 待支持项
1. Spring命名空间
1. 基于Hint的强制主库读取配置

## 不支持范围
1. 主库和从库的数据同步。
1. 主库和从库的数据同步延迟导致的数据不一致。
1. 主库双写或多写。

## 开发示例

```java
// 构建读写分离数据源
MasterSlaveDataSource masterSlaveDs0 = new MasterSlaveDataSourceFactory.createDataSource("ms_0", masterDataSource0, slaveDataSource00, slaveDataSource01);
MasterSlaveDataSource masterSlaveDs1 = new MasterSlaveDataSourceFactory.createDataSource("ms_1", masterDataSource1, slaveDataSource11, slaveDataSource11);

// 构建分库分表数据源
Map<String, DataSource> dataSourceMap = new HashMap<>(2);
dataSourceMap.put("ms_0", masterSlaveDs0);
dataSourceMap.put("ms_1", masterSlaveDs1);

// 通过ShardingDataSourceFactory继续创建ShardingDataSource
```
