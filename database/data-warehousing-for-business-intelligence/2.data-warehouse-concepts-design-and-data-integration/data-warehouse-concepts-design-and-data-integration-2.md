# Data Warehouse Concepts, Design, and Data Integration （2）

## 4. Data Integration （ETL） <a id="4-data-integration-etl"></a>

#### Refreshing Process <a id="refreshing-process"></a>

基本更新过程，首先合并数据源，然后进入数据整合。![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM33XjMCeqwat1AOqT2%2Fimage.png?alt=media&token=9d79b54e-edae-443e-b53b-90675af496ca)

#### Workflow  <a id="workflow"></a>

更新的流程![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM33jEyqIa5llt1TbOu%2Fimage.png?alt=media&token=f2703d20-6fb8-41ed-8148-505bfada6d5c)

#### Data Problems <a id="data-problems"></a>

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM33yWOXP69mGiHXW4O%2Fimage.png?alt=media&token=5aae1d51-bc61-4681-ace2-2fce4060c7c7)

* Major development activity
* More open ended than refresh with difficult to estimate time requirements
* Use profiling tools to discover data quality problems
* Perform for major data warehouse extensions

####  Refresh Processing Decision Making <a id="refresh-processing-decision-making"></a>

需要权衡约束，时间节点以及成本的问题。![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM34PSpYcJcxd7OvVpq%2FScreen%20Shot%202018-09-10%20at%2011.24.21%20AM.png?alt=media&token=5605db4b-c45a-48fe-a4a0-9dfd713e8cb4)

#### Refresh Constraints <a id="refresh-constraints"></a>

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM34c4TlgVrDo3TbePG%2FScreen%20Shot%202018-09-10%20at%2011.25.14%20AM.png?alt=media&token=df2fff55-b2ca-4b14-95a1-a7f1ff9ab228)

#### Data Change Concept <a id="data-change-concept"></a>

* 从internal 和 external 数据源读写和合并
* 更新数据库 data warehouse
  * Insert rows in fact and dimension tables
  * Update rows in dimension tables
* 主要挑战
  * 数据源难以改变，尤其是外部数据源
  * 缺少SQL路径和描述性数据特别是对于遗留数据 （Legacy Data）

具体分类![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM36CNvhnBVSS4LQPk_%2Fimage.png?alt=media&token=b8ba5c3d-d896-4fbf-8760-35fdaf34ae3f)

* Cooperative
  * Notification using triggers
  * Requires source system changes
* Logged
  * Readily available
  * Extraneous data in logs
* Queryable
  * Queries using timestamps
  * Requires timestamps in source data
* Snapshot
  * Periodic dumps of source data
  * Significant processing for difference operations

## 5. Data Cleaning <a id="5-data-cleaning"></a>

#### 数据解析 Parsing <a id="shu-ju-jie-xi-parsing"></a>

将数据从源数据读入，产生成table的形式

#### 数据值的处理 <a id="shu-ju-zhi-de-chu-li"></a>

* Missing values
  * 使用默认值
  * 用average, median, or mode填补
  * 用relationships 或者 other fields 填补
* Conflicting values
  * 使用最近的数据
  * 找更加可靠的数据源

#### 数据标准化 Standardization <a id="shu-ju-biao-zhun-hua-standardization"></a>

* Applies conversion routines to transform data into preferred formats
* Uses both standard and custom business rules
* Common standardizations:
  * Unit of measure transformations
  * Standard abbreviations \(state names, titles, street types\)

#### 正则表达式 <a id="zheng-ze-biao-da-shi"></a>

这里比较熟悉了就不细说了，主要掌握常用的。![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM38dh3DSel8qVBX0tq%2FScreen%20Shot%202018-09-10%20at%2011.42.44%20AM.png?alt=media&token=020c9914-6e54-4035-9a66-87eca74556fb)![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM38xcSa5iTBaVeSdFa%2FScreen%20Shot%202018-09-10%20at%2011.44.14%20AM.png?alt=media&token=90e5950a-a9a1-4574-a7e0-1a8bb1666a66)![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM398GJwkMW9iYHZk4s%2FScreen%20Shot%202018-09-10%20at%2011.45.06%20AM.png?alt=media&token=f7d479bd-5545-4979-b467-fa5de0c97d50)

## 6. Data Integration Tool <a id="6-data-integration-tool"></a>

ETL工具产生的原因如下:

* 支持最初的数据库填充和过程更新
* 解决缺乏工具和性能不佳的问题
* 提高现有DBMS软件的生产能力
  * 集成开发环境、图形化及可视化环境、减少自定义的代码
* 实现高性能

#### 两种主流的框架 <a id="liang-zhong-zhu-liu-de-kuang-jia"></a>

#### ETL ： <a id="etl"></a>

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM3Dg2YfDnlijXVzy_d%2Fimage.png?alt=media&token=b2d526e6-69d6-4624-a101-5bb1b967f284)

#### ELT <a id="elt"></a>

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM3BwD09dhtqUA_kacM%2Fimage.png?alt=media&token=042853ca-c0bd-4c74-8e4f-ddf7036c9973)

两个框架之间的主要评价:

* 优势
  * ETL 不依赖于 DBMS ET
  * 对 relational DBMS 有更高的优化科技
* 其他问题
  * ETL 在 transformations 过程更加复杂
  * ELT 需要较少的网络带宽

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM3ESLxvEEEFs9iLblq%2Fimage.png?alt=media&token=2b5233ba-8926-484f-b9ca-b34e28e0b832)

不管怎样，这种集成的IDE都有以下的特性:![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM3F-vscAA0XOsEwjn8%2FScreen%20Shot%202018-09-10%20at%2012.10.27%20PM.png?alt=media&token=4890cf88-7002-4403-a7a4-9ccefd661b4d)

#### IDE Overview <a id="ide-overview"></a>

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM3F5lH1yFaR66Yi2y6%2Fimage.png?alt=media&token=f8adeac8-f00f-42b1-9636-67bc71ddd034)![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LJESl7zYNKp_larRnQM%2F-LM37mGyVaaHl3-pAiYc%2F-LM3FBwSsEhwunU7zB4x%2FScreen%20Shot%202018-09-10%20at%2012.11.31%20PM.png?alt=media&token=86cd69a4-3ae3-4007-a119-de168604e50d)

​

