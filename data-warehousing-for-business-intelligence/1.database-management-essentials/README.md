# 1.Database Management Essentials

## Overview

从商业决策的角度讲，要先区分data和information，因为最终的目的是为了管理决策而服务，所以数据清洗的目的在于商业附加值。就data和information而言，只有能够为决策提供信息的才是information。

* Data: 关于事情和时间的原始数据
* Information: 将data进行转换后能直接服务于管理决策

数据库主要具有以下的特性，具体来讲数据库要足够稳定，支持内部交互以及共享。

* Persistent
* Inter-related
* Shared

数据的存储都是以table的形式来进行，这里就讲到了entities和relationship，relationship将entities相互连接，进而形成一个diagram。

**Entities & Relationship :**

* Entities : 特定的主体，包括学生
* Relationship : 主体间存在的关系，比如师生之类

![Entities and Relationship](../../.gitbook/assets/image.png)

![Entities Relationship Diagram](../../.gitbook/assets/image%20%281%29.png)

**DBMS的具体职位和职能分工:**

这里主要将同DBMS相关的分成了两个部分，一个是功能部门，主要是借助数据发挥作用，比如分析师。另一个就是IT部门，主要是管理和支持。具体的划分其实相对比较模糊

![Organization Roles](../../.gitbook/assets/image%20%282%29.png)

* Indirect user : 从数据库中直接fetch数据和报告
* Parametric user : 通过改变数据库中的参数，来改变已有的报告
* Power user: 能够用自己的能力直接做一个report
* Database Administrator: 
  * focused on individual databases and DBMSs
  * Need strong skills in specific DBMSs
* Data administrator
  * Planning: databases and technology
  * Standards setting
  * Computerized and non-computerized databases

**DBMS的主要区分 ：**

* DBMS \(Database Management System\): collection of components 
* Enterprise DBMS: 
  * 支持非常重要的数据决策
  * 数据库非常大，用户多，性能要求高
* Desktop DBMS: 主要是终端部门和单机用户
* Embedded DBMS: 嵌入在大型系统中例如 Personal Digital Assistant 和 smart card. 
  * limited transaction processing features 
  * **low memory, processing, and storage requirements.**

 **Non-procedural Database Access**

这个是DBMS的主要特征，非过程和过程化的区别就在于有没有Loop，非过程化是没有Loop的，它主要是用query去回答问题，它关注的是去哪里拿，拿什么的问题，而不关注如何去拿这样的过程化细节。

* Improve productivity and improve accessibility
* SQL SELECT statement and graphical tools

**过程化和非过程化的结合**

主要是时间工作中需要使用其他的辅助脚本语言比如Python来和SQL结合解决问题，因此出现了例如 PL/SQL \(Oracle\), Transact-SQL \(SQL Server\)。

* Batch processing: 更多的商业行为需要批量化进行，在线处理越来越流行
* Customization: 定制化输入表格的要求和行为
* Automation: 要求自动化处理和检查数据和结构
* Performance: 更强调控制

**交易过程**

交易过程主要是在每天的基本操作中，虽然很平凡但是很重要，尤其是当互联网增长快速的时候，举一个ATM的例子，先从数据库中fetch数据，然后进行逐步交易。

![](../../.gitbook/assets/image%20%284%29.png)

这种operational transaction，比较重要的是两个以下特征：

* 有效 ：控制并发的用户数，
* 可靠 ：错误修复

具体到DBMS ：

* Concurrency control manager
* Recovery manager
* Transparent services for application developers

从管理决策上面来说，不同的层级对应不同的决策要求，具体如下：

![](../../.gitbook/assets/screen-shot-2018-09-05-at-1.38.19-pm.png)

但这种决策要求反映到数据库层面的时候，就变成了下图：

![](../../.gitbook/assets/screen-shot-2018-09-05-at-1.38.30-pm.png)

  
由此，就要求data warehouse具有以下特性:

* 从operational databases 和 external data sources演变
* Integrated and transformed data
* Optimized for reporting

数据库技术的发展和演变：

![](../../.gitbook/assets/screen-shot-2018-09-05-at-1.43.49-pm.png)

* 第一代：支持序列化和随机查找，如果是排序和文件系统，它需要程序辅助。
* 第二代：真正实现了entity types 和 relationships，但依赖程序员写程序来指引文件存储的相关位置，所以也就是natigational级别的。
* 第三代：relational DBMSs 主要是从数学上设计和实现了全面功能，并且支持使用non-procedural languages，从而使得效率有了明显提高。
* 第四代：主要向分布式存储、新型格式（XML）等新型领域进行了发展。

