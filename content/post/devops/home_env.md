---
title: "家庭环境"
categories: ["devops"]
tags: [""]
date: 2020-07-11T22:51:47+08:00
---

# 家庭环境

k8s已经是事实标准

## 规划

### 硬件

普通的家用电脑CPU核心数、硬盘一般比较小，无法满足部署大规模服务的要求。因此开始规划时就将目标瞄准在服务器这块，最开始看的机架式服务器，后来担心噪声太大改为规划使用塔式服务器。而且塔式服务器空间较大，后期万一想加显卡也很方便。决定使用塔式服务器之后，平台基本上就没啥纠结的，Dell T630是首选，T640只能支持V5 CPU，洋垃圾还没有淘汰，新的买不起，遂放弃；其他厂商的没有Dell的漂亮，所以也不考虑。

最终选定配置如下：

- **平台：**Dell T630
- **CPU：**Intel Xeon E2680 V4 * 2
- **内存：**16GB DDR4 2113HZ * 6
- **硬盘：**2TB SAS * 8

### 操作系统

操作系统主要在centos和ubuntu这两个之间纠结，直观感觉centos较稳定，ubuntu软件支持度更好。

### 存储

为了避免一块盘损坏而导致数据全部丢失，raid肯定是要使用raid的。纠结在于使用raid10还是raid5，raid10空间利用率较低，raid5如果坏盘重构时间较长。由于对空间的要求不是很高，最终决定使用raid10。

## 部署

## 维护