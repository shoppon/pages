# 记一次惨淡的蚂蚁金服面试经历

不知不觉在华为已经工作5年多了，这5年多来成长了许多，也得到了许多。尤其是近两年担任华为云备份服务TL(Technology Leader)岗位以来，技术实力感觉得到了长足的发展。与此同时，也感觉离天花板越来越近。再往前发展，要么走技术管理的路线，要么走资深架构师的路线。管人不是我的强项，也不是我的兴趣。另一方面，备份服务架构基本稳定，业务盘子就这么大，在架构方面也不太可能有大的发展。这两条路对我来说都很难走得很远，因此渐渐地有了许多危机感。家庭方面，看着儿子一天天长大，希望能多点时间陪着他。而在华为上班比较远（我住南边，公司在北边），每天总路程大概需要1个半小时，浪费这些时间着实可惜。从这些方面考虑，确实萌生了一些退意。

### 一面：业务部门面试

项目中最有技术挑战，最有成就感的体验是什么。

备份服务合一的割接。全量—》流量拿的—》增量—》任务续做。流量倒换的方式，通过修改Haproxy的后端节点或者DNS解析地址，都是常规手段。

### 二面：编程题

拿到这个题目，

使用字典缓存整个IP白名单，将IP分成4段，每段作为字典的一层

```python
{
  '192': {
    '168': {
      '1': {
        ['1']
      }
    }
  }
}
```

MBP上操作Pycharm写代码不是很熟练，尤其是调试，快捷键一点不熟悉，touch bar也没有找到节奏。

写完才二三十行代码，花了一个多小时，自己非常不满意。

星期天下午打电话过来沟通面试题，大概问了这么几个问题，

用字典保存IP白名单，搜索效率为什么是常数级。我回答字典是红黑树，有Hash。面试管解释红黑树(平衡二叉树)的查找效率是Log级别。这块我不太确定，后来查询资料才知道字典底层用的是Hash(散列表)。

多线程环境下怎么处理？我回答Python有GIL问题，执行代码时需要获取全局解释锁，不会真正迸发，没有线程安全和线程不安全的说法。如果不考虑Python这个语言特性，应该怎么做，我回答加锁，但是加锁可能会影响性能。然后又问不加锁怎么做，有没有什么无锁方案？一时没想起来，回答说不知道。

### 三面：交叉部门面试

问了下项目的架构及如何实现。

系统遇到什么问题，怎么解决的。回答提高sql效率，网络调用优化。感觉面试管对这个回答不太满意。

拦截器的实现原理。当前项目的Web框架是基于Paste的，可以配置各种filter。说实话，这块在本地调试过多次，但具体怎么实现确实很难讲清楚。我回答的一层层调用，面试管问是不是函数的层层调用。最后我说用的Paste框架，通过配置文件配置各种filter。感觉面试管对这个回答不太满意。

除了基于openstack开发的项目外，有没有研究得很深的技术领域？其实这是个很有挑战的问题，说实话，还真不敢说自己在哪块有特别深的研究。

面试官打听了下华为云的技术栈，回答基于openstack，服务主要用python，底层主要用C。是否有开源项目经历，是否在github的开源项目上做过贡献，是否担任过开源项目的什么角色。

回想起来，三面问的问题确实深一点，没有做好准备，回答得不是很好，给面试管很简单的感觉。把简单的事情做优雅就不是件简单的事情。

确实反应了一个事实，个人的技术深度确实不够，竟然连Paste的实现原理都没法讲清楚。

祛除浮躁，沉下去，苦心钻研某项技术。然后多参与社区开源项目，尝试提交MR。

