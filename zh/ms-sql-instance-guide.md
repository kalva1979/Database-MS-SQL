## Database > MS-SQL Instance > 使用指南

## 创建MS-SQL Intance

为使用MS-SQL，应先创建实例。
单击创建MS-SQL Instance**跳转**按钮，跳转至**Compute > Instance > 创建实例**。

![mssqlinstance_01_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_01_201812.png)

选择MS-SQL镜像并完成添加设置后，创建实例。
与创建实例相关的详细内容请参考【Instance概要】（https://docs.toast.com/ko/Compute/Instance/ko/overview/）。 

创建实例完成后，利用RDP（Remote Desktop Protocol）访问实例。
Floating IP应连接至实例，安全组中应允许使用TCP端口3389（RDP）。
单击**+确认密码**按钮创建实例时，使用设置的密钥对确认密码。

![mssqlinstance_02_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_02_201812.png)

单击**连接**按钮，下载.rdp文件后，利用获得的密码连接到实例。

## 创建MS-SQL镜像后进行初始设置

### 1.设置SQL验证模式

服务器的默认验证模式为“Windows验证模式”。 
为使用MS-SQL的数据库账户，需要更改为SQL验证模式。 

执行Microsoft SQL Server Management Studio，通过实例名连接到对象。

![mssqlinstance_03_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_03_201812.png)

1.右击对象。
2.在菜单中选择**属性**。
3.在服务器属性窗口中选择**安全**菜单。
4.将**服务器验证**方式更改为“SQL Server及Windows验证模式”。

※ 设置SQL验证模式后，为进行应用，应重启MS-SQL服务。 

### 2.更改MS-SQL服务端口

MS-SQL的默认服务端口1433是广为人知的端口，所以有可能成为安全漏洞。
建议更改为其他端口。
※ Express的情况无法指定默认端口。 

运行SQL Server配置管理器。

![mssqlinstance_04_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_04_201812.png)

1.在左侧面板中选择**SQL Server网络配置**的下级项目**MSSQLSERVER相关协议**。
2.右击协议名中的**TCP/IP**。
3.在菜单中选择**属性**。
4.单击**IP地址**标签。
5.在下拉菜单中选择**IP ALL**后，更改为其他端口号。

※ 更改MS-SQL服务端口后，为进行应用，应重启MS-SQL服务。 

### 3.设置允许从外部连接MS-SQL数据库

为从外部连接MS-SQL数据库，应在**Network > Security Group**中将MS-SQL服务端口添加到Security Group。 
添加Security Group时，注册允许连接的MS-SQL服务端口（默认端口：1433）及远程IP。 

## 分配数据卷

MS-SQL的数据/日志文件（MDF/LDF）、备份文件建议使用另外的Block Storage。
若欲创建Block Storage，在**Compute > Instance > Block Storage**标签中单击+创建Block Storage按钮。

![mssqlinstance_05_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_05_201812.png)

创建Block Storage时，为保证性能，Volume类型推荐“通用SSD”。

![mssqlinstance_06_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_06_201812.png)

完成创建Block Storage并选择Storage后，单击**连接管理**按钮，连接到实例。

<br/>

使用RDP连接到实例，执行**计算机管理**，跳转至**存储位置>磁盘管理**。

![mssqlinstance_07_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_07_201812.png)

可以确认是否检测到已连接的Block Storage。为进行使用，应先对磁盘执行初始化。
1.右击**磁盘1**块后，单击**初始化磁盘**。
2.选择分区格式后，单击**确定**按钮。

<br/>

初始化完成后，创建磁盘卷。

![mssqlinstance_08_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_08_201812.png)

右击未分配的磁盘后，单击**新建简单卷**，运行新建简单卷程序。

<br/>

在Microsoft SQL Server Management Studio服务器属性的数据库设置中，将数据库默认位置更改为创建的卷的目录。

![mssqlinstance_09_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_09_201812.png)

※ 更改MS-SQL数据库默认位置后，为进行应用，应重启MS-SQL服务。 

## 重启MS-SQL服务
更改MS-SQL设置时，有时需要重启MS-SQL服务。
为应用更改的设置，重启MS-SQL服务。 

选择SQL Server配置管理器的**SQL Server配置管理器（本地）>SQL Server服务>SQL Server（MSSQLSERVER）**后，右击，在显示的菜单中选择“重启”重启MS-SQL服务。

![mssqlinstance_10_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_10_201812.png)

## 确认/设置自动执行MS-SQL服务
确认MS-SQL的服务是否设置为OS驱动时自动启动。 

在SQL Server配置管理器的SQL Server配置管理器（本地）>SQL Server服务中可确认“启动模式”。 

![mssqlinstance_11_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_11_201812.png)

**SQL SERVER (MSSSQLSERVER)**及**SQL Server代理程序（MSSQLSERVER）**等的服务启动模式不是**自动**时：
1.右击相应服务后，选择**属性**。
2.在**服务**标签中将**General>启动模式**更改为**自动**。

> 【参考】
> MS-SQL Instance的发布现状请参考【实例发布说明】(/Compute/Compute/ko/release-notes/)。
