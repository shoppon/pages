# C++单元测试

## 测试目的

新增、修改代码时更有信心，提高效率，更好享受人生。好的单元测试会反哺设计，单元测试不好做很多时候意味着架构设计不够好。

## 测试范围

1. 正常功能。非编译型语言中还可保证语法正确。
2. 边界值。
3. 异常分支。

## 测试时机

1. 写代码之前。**核心逻辑**先写测试用例，再写业务逻辑，可以用测试用例调试业务代码。
2. 发现Bug之后。发现Bug之后，举一反三，通过测试用例防护各种异常场景。
3. 不求面面俱到。简单的逻辑没有必要写过多的单元测试，边际成本太高。

## 测试要点

- 多线程
  - 依赖队列，依赖select/epoll等等。
- socket相关
  - 不适合单元测试，通过自动化集成测试验证。
- 大对象
  - 审视架构、设计是否合理。

## 测试划分

- 多级调用时，底层详细测试，高层主要流程。
- 单元测试针对函数、模块级即可。

## gtest使用

### 测试集

## 案例

### 数据传输

#### 测试划分

- 解析dataset元数据
- 反序列化dataset文件
- 顺序写done文件

### 进程合一

#### 需求背景

支持```tOMA```和```WA```合并部署到一个进程，一个session包含```tOMA```、```s3```、```WA```，并且```tOMA```通过函数调用的方式将IO发送给本进程的```WA```进行处理。

#### 测试目标

1. 在```tOMA```创建session时可以指定```WA```为其自身，session创建成功。
2. session加载后可以正确创建出```TomaNalHander```和```WaNalHandler```两个handler。
3. ```tOMA```下载IO后能够下发给```WA```下盘。