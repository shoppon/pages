---
title: "二进制分析"
categories: ["linux"]
tags: [""]
date: 2020-05-16T17:37:18+08:00
---

# 二进制分析

### readelf

elf全称为executable and linking format。

**查看so信息：**`readelf -d liboma.so`

### objdump

### ldd

**查看可执行文件/动态库的依赖：**`ldd xx.so`

### nm

#### 选项

- `-n` 按照地址/符号值来排序；
- `-u` 打印出那些**未定**义的符号；
- `-l` 使用对象文件中的调试信息打印出所在源文件及行号；
- `-A` 在每个符号信息的前面打印所在对象文件名称；

#### 符号类型

- `A` 该符号的值在今后的链接中将不再改变；
- `B` 该符号放在BSS段中，通常是那些未初始化的全局变量；
- `D` 该符号放在普通的数据段中，通常是那些已经初始化的全局变量；
- `T` 该符号放在代码段中，通常是那些全局非静态函数；
- `U` 该符号未定义过，需要自其他对象文件中链接进来；
- `W` 未明确指定的弱链接符号；同链接的其他对象文件中有它的定义就用上，否则就用一个系统特别指定的默认值。


