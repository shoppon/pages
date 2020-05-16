# 安全设计

## 安全架构设计与方法

架构是以组件与组件之间的关系、组件与环境之间的关系为内容的某一系统的基本组织结构，以及指导上述内容设计与演化的原理。

系统架构=组件+交互+重要决策集

安全架构=系统业务架构+安全控制集

### 4+1视图

**逻辑视图：**架构师、PM（职责划分、职责间协作）

**数据视图：**数据库、开发工程师（表结构）

**运行视图：**SE、开发、测试工程师（控制流、加锁与同步、进程、线程、系统启动与停机）

**物理视图：**运维、部署工程师（物理节点、拓扑结构）

**开发视图：**开发工程师（程序单元、框架、配置文件、project划分）

### 8维度安全架构技术地图

- 系统可信保护Trust
  - 文件完整性
  - 安全启动
- 认证及权限控制Authentication
  - 身份认证
  - 权限控制
  - 用户管理
- 安全管理Management
  - 日志审计
  - 安全升级
  - 安全事件管理
- 安全检测响应Detection
  - 攻击检测
  - 入侵保护
  - 防DoS/DDoS
  - 内存防护
- 数据保护Data
  - 密钥管理
  - 数据传输安全
  - 数据存储安全
- 安全隔离Seperation
  - 网络隔离
  - 应用隔离
  - 容器隔离
  - 水箱隔离
- 隐私保护Privacy
  - 隐私控制
  - 隐私合规
  - 数据脱敏
- 安全部署Deployment
  - 安全加固
  - 组网安全

## 安全架构和设计原则

### 安全三要素CIA

**可用性（Availability）：**系统必须及时的服务，不能拒绝向授权用户提供服务。

**完整性（Integrity）：**信息或者程序不能受到故意或无意的非授权操纵或破坏。

**机密性（Confidentiality）：**私密的数据不能透露给非授权的个体。

### 安全设计八+N原则

#### 开放设计原则

设计不应该是秘密，开放设计更安全。

加密算法使用公开的算法，不要使用私有加密算法，对密钥进行保护。

#### 失败-默认安全原则

访问决策应当基于允许，而不是拒绝。默认情况下是不允许访问的，保护机制仅用来识别允许访问的情况。

失败安全：默认可以登陆，多次失败后锁定账号。

default分支 ：保证是默认安全的。

通信领域：三面隔离（管理面、用户面、控制面）

#### 权限分离原则

“两把钥匙”比“一把钥匙”安全。

linux使用超级管理员和普通管理员。

三权分立：立法权（国会）、行政权（总统）、司法权（法院）。

用户管理设计：系统管理员、安全管理员、安全审计员。

#### 最小权限原则

系统的每一个用户、每一个程序，都应该使用最小且必须的权限来完成工作。

文件属性640、750等限制。

web程序、数据库不能使用root用户运行。

移动应用的权限问题。

#### 经济适用原则

保持系统的设计和代码尽可能简单、紧凑。

#### 最小公共化原则

尽量避免提供多个对象共享同一资源的场景。对资源访问的共享数量和使用应尽可少。

共享内存最小化。

三面隔离，端口绑定最化小，不要监控0.0.0.0。

通过负载均衡防DDoS攻击。

#### 完全仲裁原则

对于每个对象的每次访问都必须经过安全检查审核。

每次web访问都要校验用户权限。

注意缓存的使用和检查。

DNS缓存欺骗，DNS劫持。

#### 心理可承受原则

安全机制可能为用户增加额外的负担，但这种负担必须是最小的而且是合理的。

多次登陆失败后才需要输入验证码。

反例：12306的验证码。

#### 纵深防御原则

高屋建瓴地从系统架构层面来关注整个系统级的安全防御机制。

保护最薄弱的环节。

## 安全威胁分类STRIDEP

### 流程

绘制数据流图--》威胁分析--》风险评估--》制定消减措施--》产品响应

元素：外部交互方/处理过程/数据存储/数据流/信任边界

### Spoofing仿冒

**消减措施：**认证（密码认证、SSL/TLS）

### Tampering篡改

**消减措施：**完整性（Hash、MAC）

### Repudiation抵赖

**消减措施：**认证、审计日志

### Information Discloure信息泄露

**消减措施：**加密、ACL、权限控制

### Denial of Service拒绝服务

**消减措施：**可用性、负载平衡、过滤、缓存

### Elevation of Privilege权限提升

**削减措施：**授权（权限最小化、沙箱）

### Privacy隐私

**削减措施：**匿名化、数据最小化、脱敏

## 威胁分析知识库

### 攻击手段

#### 中间人攻击

#### CSRF

#### 缓冲区溢出