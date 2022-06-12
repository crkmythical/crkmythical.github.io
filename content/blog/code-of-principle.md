---
title: code_of_principle
encrypt: false
enc_pwd: askding
categories:
  - Coding
tags:
  - null
date: 2021-03-07 15:23:04
top:
layout:
---
编写进度
- [x] 


[谷歌代码规范](https://zh-google-styleguide.readthedocs.io/en/latest/contents/)




编程属于设计行为，负责制造的是编译器或者构建系统
程序员的能力是用来为用户创造价值的

# How to code
文学编程(代码即设计文档) 用于表述文档的语言和编程语言结合在了一起
高质量代码三大特性：可读性 > 简洁性 > 灵活性

  * [ ] 可读性 第一要务
  * [ ] 简洁性 保持代码简洁，不写多余的代码，不写重复的代码
  * [ ] 灵活性 添加新代码时，与已有代码不冲突、不排斥
高质量代码是拥有多种扩展方法、不存在多余要素、可读性高、易于理解的代码。 


## 软件

"插件式"软件架构


* 非功能需求
非功能对开发、运维、计算机资源的高校运用有较大影响，在发布后的运维阶段，比较大的问题多是由性能、系统宕机等非功能需求引起的。


## 再版-重构
> 在不改变代码外部行为的前提下，对代码内部结构进行优化，亦称代码体质优化

重构的征兆:
  * [ ] 代码重复、命名不一致
  * [ ] 函数体太长
  * [ ] 模块规模太大、太多




## 图书-包

包：一种方便组织代码的机制(实现封装和模块化开发)，类似于文件夹，还可以解决命令冲突问题。
等模块积攒到一定数量之后再自上而下的将模块按照某种有意义的单位整理并分割





## 章节-模块

针对借口编程，而不是针对实现编程

模块化：让修改模块所带来的影响停留在改模块内部
模块间互相频繁调用表明折俩函数应放在一个模块

抽象： 舍弃多余的东西，抽取对象共同的性质组成通用的概念

Parnas原则：
  * 对于模块的使用者，仅提供使用该模块锁必需的所有信息，其余信息概不提供
  * 对模块的开发者，仅提供实现该模块所必需的所有信息，其余信息概不提供


关注点分离： 将各个与关注点有关的代码集中起来做成独立的模块，与其他代码分离。如MVC模型，AOP(面向切面编程)

OCP开闭原则: 对扩展开发、对修改关闭 面向对象的多态性是实现OCP的代表技术


- 内聚度: 单个模块内的功能的纯粹程度
- 耦合度: 衡量模块间关系紧密程度 模块间仅通过参数传递标量类型的数据作为模块间的接口

模块应以实现高内聚、低耦合的模块为目标


## 序言-注释

注释的目的是帮值阅读代码的人了解作者写代码时的思想

 代码只能表达“What”、“How” 、注释表达“Why”
 注释内容要简洁明了，防止注释二义性
 
 ### 模块注释
 
 ```zsh
 
 # File Name: xxx
 # Author: Mr.Frame
 # Version: 0.1
 # Date: 
 # Description: 
 #
 #
 # Function List:
 ```

### 函数注释
```zsh
# Function: 
# Description: 
#
# Input: 
# Output:
# Return: 
# Call: 
# Called by: 
# Others:
```

### 代码注释

注释应放在代码上方/或/右方





## 目录-函数

将代码分割成级别统一的函数，函数一览起到目录的作用

复合函数： 将函数结构化，各函数的处理将以调用比自己低一个级别的函数为中心
不要在复合函数中调用不同抽象级别的函数，
一个函数中不能既有连接数据库(低级处理)、又有执行业务逻辑(高级处理)


将关系紧密的代码集中在一起函数化，同时保证关联性较弱的代码互不依赖

通用过滤器：接收某种数据流后，经过加工输出文本格式的数据流

函数行数不过百，仅输出重要信息

防御性编程:
  * [ ] 确认外部代码传来的数据输入值(有效范围、字符串长度)
  * [ ] 确认参数的值(检测无效输入)



## 段落-代码块

正交性: 代码之间应具有独立性和分离性(例如：访问数据库代码和用户接口代码)

代码块内列对齐

不同的代码块之间有空行

> 数据常量化
> 对代码的逻辑进行函数化、模块化


比较操作时，把改变的值放在左边，把稳定的值放在右边

将逻辑和该逻辑处理的数据放在相近的位置

处理流程尽量走直线,不使用条件分支


每个控制条件都存在与之成对的反条件，
保证控制条件具有统一性：
即便某个if语句一定成立，也要考虑else语句的情况
即便某个case语句一定成立，也要考虑default语句的情况
即使某个变量不可能为空，也要检查该变量是否为NULL



## 正文-代码

命名：名字是面向代码阅读者的用户界面。名字说明的是效果和目的，不是手段， 
- 使用专业具体含义的词
- 全局变量名较长，局部变量名较短
- 恰当使用大小写和下划线

- 减少变量作用域
 

  * [ ] 表示范围: max/min 、first/last 、begin/end
  * [ ] 表示bool: is/has


## 注释/索引-Editor自带
图书中有注释和索引，但IDE和编辑器的跳转和搜索功能可以满足这部分需求。


- 「编程的原则：改善代码质量的101个方法」



# How to read

代码阅读是一项机会主义、以目标为导向的思维活动。
在大多数情况下，我们无法承担阅读和理解整个软件系统代码的代价，
任何试图对代码进行精确分析的想法，都会导致陷入大量的类、文件和模块中。
因此，**我们必须积极低减少必须理解的代码**

  * [ ] 学习编写伟大代码的方式: 阅读大量优质代码
阅读优秀源码基础:
* [ ] 设计模式
* [ ] 算法、数据结构
	

在熟悉基本的编程和计算机科学概念后，通过阅读一套设计良好的软件系统的内部细节
	可以学到新颖的架构模式、数据结构、编码方法、算法、风格和文档规范、API、甚至一门新的计算机语言。

  * [ ] 良好习惯: 有目标地阅读辨认编写的高质量代码(遇到库元素就阅读库文档)



## Steps
  * [ ] 阅读README，安装依赖，编译运行,编译不成功不读
  * [ ] 主框架,阅读main函数，梳理主逻辑
	* [ ] a. 函数调用顺序、及其参数
	* [ ] b. 