## Database > Microsoft SQL Instance > User Guide 

## Create Microsoft SQL Instance

To use Microsoft SQL, it is required to create an instance first. 

![mssqlinstance_01_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_01_201812_en.png)

Click **Shortcuts** next to **Create Microsoft SQL Instances** to go to **Compute > Instance > Create Instances**.
Select Microsoft SQL image, complete additional settings, and create an instance.
For more details on creating instances, see [Instance Overview](https://docs.toast.com/ko/Compute/Instance/ko/overview/). 

After instance is created, access the instance by using Remote Desktop Protocol (RDP). 
To that end, an instance must be associated with a floating IP and TCP port 3389 (RDP) must be allowed for security group. 

![mssqlinstance_02_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_02_201812_en.png)

Click **+ Check Password** to check password by using key pair configured along with instance creation. 
Click **Associate** and download .rdp file, to access the instance by using the acquired password. 

## Initial Settings after Microsoft SQL Image is Created  

### 1. Set SQL Certification Mode  

The default certification mode of the server is set with"**Windows Certification Mode**". 
To use Microsoft SQL database account, the mode must be changed to SQL Certification Mode. 

Execute Microsoft SQL Server Management Studio and associate to an object under the instance name. 

![mssqlinstance_03_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_03_201812_en.png)

1. Select an object, right-click it, and choose **Attributes**. 
2. On **Server Attributes** , click **Security**. 
3. Change the **Server Certification** type into **SQL Server and Windows Certification Mode**.

※ To apply the changed SQL certification mode, restart Microsoft SQL. 

### 2. Change Microsoft SQL Service Port 

The default port 1433 for Microsoft SQL is widely known and might serve as a security vulnerability.   
A change is recommended to another port. 
※ For Express, no default port is specified. 

Execute SQL Server configuration manager as below. 

![mssqlinstance_04_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_04_201812_en.png)

1. Click **Protocol for MSSQLSERVER** below **SQL Server Network Configuration** from menu on the left. 
2. Click **TCP/IP** for **Protocol Name** and right click it. When the menu shows, select **Attributes**.  
3. Select the **IP Address** tab. 
4. Select **IP ALL** on the list and change the port number to another.  

※ To apply the changed service port, restart Microsoft SQL. 

### 3. Allow External Access to Microsoft SQL Database 

To allow external access to Microsoft SQL Database, go to the **Security Group** tab of **Network > VPC** and add Microsoft SQL service port for security rules. 
Also, register Microsoft SQL service port (default port: 1433) to allow access, as well as remote IP.  

## Data Volume Assignment  

Microsoft SQL data/log files (MDF/LDF) and backup files are recommended to be applied with separate block storages.  

![mssqlinstance_05_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_05_201812_en.png)

Go to **Compute > Instance > Block Storage** and create a block storage.  
**Universal SSD** is a recommended volume type for improved performance.

After a block storage is created, select the storage and click **Association Management** and associate it to an instance.  

![mssqlinstance_06_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_06_201812_en.png)

<br/>

Access instance with RDP and execute **Computer Management**, and go to **Storage>Disk Management**. 

![mssqlinstance_07_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_07_201812_en.png)

You can find the associated block storage is detected. To use it, initialize disk first. 
1. Right-click the **Disk 1** block and click **Initialize Disk**. 
2. Select a partition type and click **OK**. 

<br/>

After initialization is completed, create disk volume. 

![mssqlinstance_08_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_08_201812_en.png)

Click unassigned disk and right-click it. Select **New Simple Volume** and proceed with wizard for new simple volume. 

<br/>

In the **Database Setting** of **Server Attributes** of Microsoft SQL Server Management Studio,  change **Default Database Location** into the directory where the volume has been created. 

![mssqlinstance_09_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_09_201812_en.png)

※ To apply the changed default database location, restart Microsoft SQL. 

## Restart Microsoft SQL 
Change of Microsoft SQL settings sometimes requires a restart of the service.  
To apply changed settings, restart Microsoft SQL.   

From SQL Server Configuration Manager, go to **SQL Server Configuration Manager (local) > SQL Server Service > SQL Server (MSSQLSERVER)** and right click it. When the menu shows, click **Restart** to restart the service. 

![mssqlinstance_10_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_10_201812_en.png)

## Check/Set Automatic Microsoft SQL  Service Execution
Check if Microsoft SQL is set for automatic start with OS running.  

Go to **SQL Server Configuration Manager (local) > SQL Server** in the SQL Server Configuration Manager to find **Start Mode**.  

![mssqlinstance_11_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_11_201812_en.png)

When the service start mode for **SQL SERVER (MSSSQLSERVER) and SQL Server Agent (MSSQLSERVER)** are not **automatic**, do the followings: 

1. Click the service and right-click it. Select **Attributes** on the menu. 
2. Change **Service** on **General > Start Mode** to **Automatic**.

> [Note]
> For the release status of Microsoft SQL Instance, see [Instance Release Note](/Compute/Compute/ko/release-notes/).

