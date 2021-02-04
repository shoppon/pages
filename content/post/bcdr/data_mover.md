---
title: "基于Ceph RADOS的灾备解决方案"
categories: ["云计算"]
tags: ["ceph", "灾备"]
date: 2020-11-04T17:13:23+08:00
typora-root-url: ../../
---

# 核心原理
当前基于Ceph的灾备解决方案主要有两种技术方向：`snapshot based`和`rbd mirror`。

# 产品现状

# 可选方案

## DR Scheduler
快照是对当前镜像的只读拷贝，一般有COW(Copy on Write/写时复制)和ROW(Redirect On Write/写时重定向)两种实现。

在DR中调用`librbd`的`rbd export-diff --from-snap`获取快照间的差异，然后通过`rbd import-diff`导入快照。

### 优点
>1. 实现简单，工作量小。

### 缺点

>1. 频繁创建/导出/删除快照对性能影响较大。
>2. 需要将快照保存成本地文件，再导入到备集群，多两次读写IO。
>3. 无法保证崩溃一致性。

## RBD Mirror

简单地说，通过配置两个集群为mirror模式，将主集群的Journal在备集群**回放**(replay)达到镜像的异步复制实现灾备。

### 缺点

> 1. 对生产站点性能影响极大

## Data Mover

基于`rados`实现`DataMover`模块，实现，避免多余的文件操作。

### 优点
>1. 减少本地文件操作，传输效率较高。
>2. 后期可扩展备份/容灾到公有云存储。

### 缺点
>1. 实现较复杂。

# 数据一致性解决方案
基于`qemu-guest-agent`。

# 跨云灾备解决方案

## 优势

- 计算节点平常可不用购买，容灾failover时再购买，降低容灾成本。

## 关键技术点

控制节点、计算节点等ceph外数据如何同步？

failover后产生的增量数据如何在failback之前同步？

线上数据中心可能无法直接访问线下？（ASR通过对象存储中转）

数据中心与公有云之间的网络质量可能比较差。

如何保证多虚拟机/整站点之间数据一致性？（快照一致性激活）

# 参考

- [Ceph RBD灾备方案对比](https://www.jianshu.com/p/a25992de5aff)
- [分布式存储Ceph RBD-Mirror 灾备实践](https://www.infoq.cn/article/9RdDKkpMu10*zS9AhrWh)