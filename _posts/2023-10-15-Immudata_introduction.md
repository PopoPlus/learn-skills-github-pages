---
title: "什么是Immutable Data不可变数据结构？"
date: 2023-10-15
---


### Immutable Data的概念
Immutable Data，不可变数据结构（下文为方便简称为ImmuData）。从名字可以看出其最重要的特性，即产生后便无法修改原始内容的一种数据结构（待资料收集）。它本身更多是一种设计思想，而不是具体的一套逻辑。
Immudata在数据库（1.待资料收集 2. 有没有可能搜索到论文？）得到了比较广泛的应用——一些文章直接将数据库按照基于可变数据结构或者不可变数据结构划分为了两类。
对Python比较熟悉的同学可能会联想到Python中的`tuple`、`list`类型变量，事实上前者确实是一种Immutable对象，而后者则是Mutable对象。当`tuple`被创建后，即无法再被修改；而对`list`的写操作则会直接覆盖掉原始的数据。


### 如何对Immutable Data进行操作
一份无法进行修改的数据？那要如何体现代码逻辑对数据的增、删、改呢？听起来似乎有点反直觉。
我们再回想Python中，是不是有一种变量既是不可变的，同时又是经常被修改的？
string变量在python中具有不可变属性，这也是为什么如果我们尝试修改string中某位字符会报错的原因：

~~~
test_string = "Hellow World!"
test_string[0] = 'h'
print(test_string)

Traceback (most recent call last):  
File "my_py_console.py", line 4, in <module>
    test_string[0] = 'h'
TypeError: 'str' object does not support item assignment
~~~
所以对python字符串的写操作都是建立在一份新的copy上（如`replace`方法）。这也是对Immutable数据结构进行写操作的基本思路，即拷贝一份再做修改，这样就保证了旧数据的不变性。相应的，后续的修改都会基于最新数据的copy来进行。随着持续的操作，我们会得到**一份数据一系列的“快照”**——记录了从最初到最后的所有状态。

但每次写操作之前都深拷贝一份数据未免太过消耗内存。字符串的消耗也许还能接受，但如果是复杂且频繁修改的数据结构，这开销可就真的让人难以接受了。所以我们需要一种更轻巧的拷贝方式。  
以对Immutable Tree的操作为例，下面图1中左侧蓝色节点为Immutable Tree原本的结构，现在我们尝试为3号节点增加一个新的叶节点6。

<p align="center" width="100%">
<img src="../drafts/Pics_normal/ImmuTree添加节点第一步.drawio.png" alt="为Immutable Tree新增叶节点" title = "为Immutable Tree新增叶节点" width="70%">
<p align="center">图1 Step1 新增所需的叶节点。</p>
</p>

<p align="center" width="100%">
<img src="../drafts/Pics_normal/ImmuTree添加节点第二步.drawio.png" alt="为Immutable Tree重连节点" title = "为Immutable Tree重连节点" width="70%">
<p align="center">图2 Step2 重连叶节点。</p>
</p>

第一步，生成新节点：  
从根节点到目标节点沿途的所有节点，如图1中所示，即为1，3，6三个节点。6是从无到有新创建的节点，而1'，3'实际是直接拷贝得到的“新节点”。  
  
第二步，重连节点：  
如图2所示，将新的节点1'与原本的2号节点相连（图2中红线），并将1'指定为这棵树新的根节点。  
至此，我们就得到了添加了6号节点的新的树。  
  
那如果我们想要继续操作，如删除掉原本的5号节点呢？
<p align="center" width="100%">
<img src="../drafts/Pics_normal/ImmuTree删除节点.drawio.png" alt="为Immutable Tree重连节点" title = "为Immutable Tree重连节点" width="70%">
<p align="center">图3 删除5号节点。</p>
</p>


### 采用Immutable Data设计带来的好处
如果一个设计并不会带来实际的效益，那我们还为何要采用这种设计呢？
1. 缓存历史副本
2. 天然的数据驱动设计
3. 线程安全


*参考资料：
https://terminusdb.com/blog/9-reasons-why-you-need-an-immutable-database/
https://www.moderntreasury.com/learn/data-immutability

