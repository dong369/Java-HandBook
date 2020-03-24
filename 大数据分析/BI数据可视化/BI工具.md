> BI 唤醒沉睡的数据

大数据时代商业智能（BI）和数据可视化诉求更为强烈，淘宝大屏更是风靡全球！数据可视化是大数据『最后一公里』，BI 唤醒沉睡的数据。

> 百家争鸣，百花齐放

传统型 BI 力求大而全的统一综合型报表和分析平台，侧重传统式报表开发，俨然一把屠龙刀。现互联网公司快速迭代的业务发展，需要的却是倚天剑，促使自助式 BI 和敏捷 BI 得以迅速发展。

时代召唤，传统 BI 巨头也逐渐向自助式 BI 和云 BI 转型。一时间，数据可视化和 BI 呈现出 "百家争鸣，百花齐放" 的盛况！

# 1 开源BI

## 1.1 Ureport2

UReport2 是一款高性能的架构在 Spring 之上纯 Java 报表引擎，通过迭代单元格可以实现任意复杂的中国式报表。在 UReport2 中，提供了全新的基于网页的报表设计器，可以在 Chrome、Firefox、Edge 等各种主流浏览器运行（IE浏览器除外），打开浏览器即可完成各种复杂报表的设计制作。

Java系列

github：https://gitee.com/youseries/ureport?from=weekly_83

star：2k+，虽然 star 不高，但在市场二三线企业有大量使用和企业二次开发版本。

中文文档：http://wiki.bsdn.org/display/UR/ureport2+Home、https://www.w3cschool.cn/ureport

## 1.2 CBoard

国人开发的一款可视化工具。交互设计的不错，拖拽模式，支持邮件，Java系列。

github：https://github.com/yzhang921/CBoard

star：2k+，虽然 star 不高，但在市场二三线企业有大量使用和企业二次开发版本。

中文文档：https://peter_zhang921.gitee.io/cboard_docsify/#/zh-cn/manual/widget

## 1.3 EasyReport

EasyReport 是一个简单易用的 Web 报表工具，它的主要功能是把 SQL 语句查询出的行列结构转换成 HTML 表格(Table)，并支持表格的跨行(RowSpan)与跨列(ColSpan)。同时它还支持报表 Excel 导出、图表显示及固定表头与左边列的功能。

## 1.4 Superset

Airbnb 开源的数据可视化工具。

目前属于 Apache 孵化器项目，与 kylin 有很好的结合。

Python 开发，版本迭代进度很快。

可能是目前颜值最高的开源 BI 工具。

不能快速复制图表，得从 SQL 层面再走一遍。

权限管理，官方提供了一个复杂的权限控制，但是并不好用。

github：https://github.com/apache/incubator-superset

start：25k+

Superset 的 star 数已经远远超过其他可视化工具。

## 1.5 Redash

开源 BI 工具，提供了基于 Web 的数据库查询和数据可视化功能。

Redash 允许快速和方便地访问数十亿条记录。

支持简单的报警规则

可以把 Dashboard 分享出去

美观程度相比 Superset 不够精美

支持的图表类型有限

github：https://github.com/getredash/redash

star：13k+

支持 ClickHouse

## 1.6 Metabase

界面相对好漂亮，明显是经过 UI 设计师仔细调校过的。相对的，Superset 与 Redash 一看就是程序员充当设计师的产物。

Metabase 非常注重非技术人员（如产品经理、市场运营人员）在使用这个工具时的体验

github：https://github.com/metabase/metabase

star：16k+

目前不支持 ClickHouse

## 1.7 Davinci

宜信技术研发中心的大数据可视化平台开发的达芬奇开源 BI 软件。

面向业务人员 / 数据工程师 / 数据分析师 / 数据科学家，致力于提供一站式数据可视化解决方案。

功能还是比较全面的，只是在国内还没有大范围的使用。值得期待。

Java 系

github：https://github.com/edp963/davinci

star：800+

Davinci：https://edp963.github.io/davinci/

用户手册 提供在线查看和 PDF 下载

## 1.8 SpagoBI

专业的开源套件，适用于传统来源和大数据系统的现代商业分析。

Java 系

在版本 6 以前是完全开源的 SpagoBI,2018 年发布的 6.0 版本开始，改名为 Knowage 并走向开源的社区版及收费的企业版两个版本。   

SpagoBI has been merged into Knowage!

star：100+

github：https://github.com/topics/spagobi

## 1.9 Pentaho

Pentaho 是一个很完善的 BI 解决方案。

Pentaho 偏向于与业务流程相结合的 BI 解决方案。

Pentaho 是一个以工作流为核心的、强调面向解决方案而非工具组件的 BI 套件，具有商业智能（BI) 组件，整合了多个开源项目，使得公司可以开发商业智能问题的完整解决方案，目标是和商业 BI 相抗衡。

github：https://github.com/pentaho

star：100+

经典 ETL 工具 kettle 就是 Pentaho 的。

# 2 商业BI

## 2.1 FineBI

是国产 BI 工具，帆软公司的。FineBI 有移动端、PAD 端、以及大屏。自助式 BI。

国内做的一流的 BI 工具，很炫酷，也比较实用。

FineBI 是一套企业数据化管理和可视化 BI 的方案，集成了 Alluxio 、Spark、 HDFS、zookeerer 等大数据组件，引擎支撑前端快速地展示分析，真正实现亿级数据，秒级展示。

一般企业的产品整体打包价格几十万。

### **Tableau**

可视化 BI 神器 Tableau。

中型企业和大型企业，不过互联网讲究的是开源免费，用的较少。

功能上 Tableau 和 Qlikview 产品的功能重合度非常高。不过 Tableau 相比 qlikview 可视化更美观，分析也更方便。

尤其像 Tableau，ETL 功能并没有集成，得有个非常好的数据仓库作为基础。

定价：每个部署至少需要一个 Tableau Creator（每月 70 美元）; 可以选择观察者（每月 12 美元，最低 100 美元）或探索者（每月 35 美元，最少 5 美元）

活跃的社区（各种培训视频、博客、论坛、社交网络）

### **QlikView**

是一种将用户作为数据接收者的解决方案。

QlikView 比较灵活，展示样式多样。它允许设置和调整每个对象的每个小方面，并自定义可视化和仪表板的外观。

能够自动关联数据：识别集合中各种数据项之间的关系，无需手动建模。

内存型的 BI 工具，数据处理速度很大程度上依赖内存大小，Qlikview 处理数据输入，是将其保存在多个用户的内存中。

定价：限量版免费; Qlik Sense Cloud Business 的协作功能每位用户每月 15 美元

**Power BI**

微软系列，用于商业智能和分析需求。

它与微软的主要工具（包括 MS Excel，Azure Cloud Service 和 SQL Server）紧密集成。

定价：三层：作者版（免费），专业版（每位用户每月 9.99 美元），Premium 版（视性能而定）

### **SmartBI**

偏向于传统 BI，操作使用上需要有一定的技术，在用户的学习成本上较高。

### **QuickBI**

阿里云旗下产品

是一个基于云计算致力于大数据高效分析与展现的轻量级自助 BI 工具服务平台。通过对数据源的连接和数据集的创建，对数据进行即席的分析与查询；

通过电子表格或仪表板功能，以拖拽的方式进行数据的可视化呈现。

## 1.3 传统重BI工具

老牌传统重 BI 工具，经典历史代表 Oracle 的 BIEE、IBM 的 Cognos、SAP 的 BO 和 MicroStrategy 等。时代召唤，传统 BI 巨头也逐渐向自助式 BI 和云 BI 转型，也寄望于数据时代急流勇退中去陈出新，再创辉煌！



