---
title: 结合使用 Spark 连接器和 Microsoft Azure SQL 和 SQL Server
description: 了解如何通过 Azure SQL 数据库、Azure SQL 托管实例和 SQL Server 使用 Spark 连接器。
services: sql-database
ms.service: sql-db-mi
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: denzilribeiro
ms.author: denzilr
ms.reviewer: carlrab
ms.date: 09/25/2018
ms.openlocfilehash: cb7fb7f6c44f9e1c4a9b073c666543a2e892582a
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985493"
---
# <a name="accelerate-real-time-big-data-analytics-using-the-spark-connector"></a>使用 Spark 连接器加速实时大数据分析 
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Spark 连接器允许 Azure SQL 数据库中的数据库、Azure SQL 托管实例和 SQL Server 充当 Spark 作业的输入数据源或输出数据接收器。 它允许你在大数据分析中利用实时事务数据，并保存即席查询或报告的结果。 与内置 JDBC 连接器相比，此连接器提供向数据库大容量插入数据的功能。 它可以优于以10倍的方式插入行，以使性能更快20倍。 Spark 连接器支持 Azure Active Directory （Azure AD）身份验证连接到 Azure SQL 数据库和 Azure SQL 托管实例，使你可以使用 Azure AD 帐户从 Azure Databricks 连接数据库。 它提供与内置 JDBC 连接器类似的接口。 可以轻松迁移现有的 Spark 作业以使用此新连接器。

## <a name="download-and-build-a-spark-connector"></a>下载并构建 Spark 连接器

若要开始，请从 GitHub 上的[sqldb 存储库](https://github.com/Azure/azure-sqldb-spark)下载 Spark 连接器。

### <a name="official-supported-versions"></a>官方支持的版本

| 组件                             | 版本                  |
| :-----------------------------------  | :----------------------- |
| Apache Spark                          | 2.0.2 或更高版本           |
| Scala                                 | 2.10 或更高版本            |
| Microsoft JDBC Driver for SQL Server  | 6.2 或更高版本             |
| Microsoft SQL Server                  | SQL Server 2008 或更高版本 |
| Azure SQL 数据库                    | 支持                |
| Azure SQL 托管实例            | 支持                |

Spark 连接器利用 Microsoft JDBC Driver for SQL Server 在 Spark 辅助角色节点和数据库之间移动数据：

数据流如下所示：

1. Spark 主节点连接到 SQL 数据库中的数据库或 SQL Server，并从特定的表或使用特定的 SQL 查询加载数据。
2. Spark 主节点将数据分发到辅助角色节点以进行转换。
3. 辅助角色节点连接到连接到 SQL 数据库的数据库，并 SQL Server 并将数据写入数据库。 用户可选择使用逐行插入或批量插入。

下图演示了此数据流。

   ![体系结构](./media/spark-connector/architecture.png)

### <a name="build-the-spark-connector"></a>生成 Spark 连接器

目前，连接器项目使用 maven。 若要构建不带依赖项的连接器，可以运行：

- mvn 清理包
- 从 releases 文件夹中下载最新版本的 JAR
- 包括 SQL 数据库 Spark JAR

## <a name="connect-and-read-data-using-the-spark-connector"></a>使用 Spark 连接器连接和读取数据

可以连接到 SQL 数据库中的数据库，并从 Spark 作业 SQL Server 来读取或写入数据。 您还可以在 SQL 数据库中的数据库中运行 DML 或 DDL 查询，并 SQL Server。

### <a name="read-data-from-azure-sql-and-sql-server"></a>从 Azure SQL 和 SQL Server 读取数据

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "dbTable"        -> "dbo.Clients",
  "user"           -> "username",
  "password"       -> "*********",
  "connectTimeout" -> "5", //seconds
  "queryTimeout"   -> "5"  //seconds
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="read-data-from-azure-sql-and-sql-server-with-specified-sql-query"></a>通过指定的 SQL 查询从 Azure SQL 和 SQL Server 读取数据

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "queryCustom"  -> "SELECT TOP 100 * FROM dbo.Clients WHERE PostalCode = 98074" //Sql query
  "user"         -> "username",
  "password"     -> "*********",
))

//Read all data in table dbo.Clients
val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="write-data-to-azure-sql-and-sql-server"></a>将数据写入 Azure SQL 和 SQL Server

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

// Aquire a DataFrame collection (val collection)

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "dbTable"      -> "dbo.Clients",
  "user"         -> "username",
  "password"     -> "*********"
))

import org.apache.spark.sql.SaveMode
collection.write.mode(SaveMode.Append).sqlDB(config)
```

### <a name="run-dml-or-ddl-query-in-azure-sql-and-sql-server"></a>在 Azure SQL 中运行 DML 或 DDL 查询并 SQL Server

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.query._
val query = """
              |UPDATE Customers
              |SET ContactName = 'Alfred Schmidt', City = 'Frankfurt'
              |WHERE CustomerID = 1;
            """.stripMargin

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "user"         -> "username",
  "password"     -> "*********",
  "queryCustom"  -> query
))

sqlContext.sqlDBQuery(config)
```

## <a name="connect-from-spark-using-azure-ad-authentication"></a>使用 Azure AD 身份验证从 Spark 连接

可以使用 Azure AD 身份验证连接到 Azure SQL 数据库和 SQL 托管实例。 Azure AD 身份验证可用于集中管理数据库用户的标识，并替代 SQL Server 身份验证。

### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>使用 ActiveDirectoryPassword 身份验证模式进行连接

#### <a name="setup-requirement"></a>安装要求

如果使用 ActiveDirectoryPassword 身份验证模式，则需要下载 [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) 及其依赖项，并将他它们包含在 Java 生成路径中。

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "user"           -> "username",
  "password"       -> "*********",
  "authentication" -> "ActiveDirectoryPassword",
  "encrypt"        -> "true"
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="connecting-using-an-access-token"></a>使用访问令牌进行连接

#### <a name="setup-requirement"></a>安装要求

如果使用基于访问令牌的身份验证模式，则需要下载 [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) 及其依赖项，并将他它们包含在 Java 生成路径中。

若要了解如何在 Azure SQL 数据库或 Azure SQL 托管实例中获取数据库的访问令牌，请参阅[使用 Azure Active Directory 身份验证进行身份验证](authentication-aad-overview.md)。

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"                   -> "mysqlserver.database.windows.net",
  "databaseName"          -> "MyDatabase",
  "accessToken"           -> "access_token",
  "hostNameInCertificate" -> "*.database.windows.net",
  "encrypt"               -> "true"
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

## <a name="write-data-using-bulk-insert"></a>使用 bulk insert 编写数据

传统的 jdbc 连接器使用逐行插入操作将数据写入数据库。 可以使用 Spark 连接器将数据写入 Azure SQL，并使用 bulk insert SQL Server。 在加载大型数据集或将数据加载到使用列存储索引的表中时，该方式显着提高了写入性能。

```scala
import com.microsoft.azure.sqldb.spark.bulkcopy.BulkCopyMetadata
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

/**
  Add column Metadata.
  If not specified, metadata is automatically added
  from the destination table, which may suffer performance.
*/
var bulkCopyMetadata = new BulkCopyMetadata
bulkCopyMetadata.addColumnMetadata(1, "Title", java.sql.Types.NVARCHAR, 128, 0)
bulkCopyMetadata.addColumnMetadata(2, "FirstName", java.sql.Types.NVARCHAR, 50, 0)
bulkCopyMetadata.addColumnMetadata(3, "LastName", java.sql.Types.NVARCHAR, 50, 0)

val bulkCopyConfig = Config(Map(
  "url"               -> "mysqlserver.database.windows.net",
  "databaseName"      -> "MyDatabase",
  "user"              -> "username",
  "password"          -> "*********",
  "dbTable"           -> "dbo.Clients",
  "bulkCopyBatchSize" -> "2500",
  "bulkCopyTableLock" -> "true",
  "bulkCopyTimeout"   -> "600"
))

df.bulkCopyToSqlDB(bulkCopyConfig, bulkCopyMetadata)
//df.bulkCopyToSqlDB(bulkCopyConfig) if no metadata is specified.
```

## <a name="next-steps"></a>后续步骤

如果尚未这样做，请从[sqldb-Spark GitHub 存储库](https://github.com/Azure/azure-sqldb-spark)下载 Spark 连接器，并浏览存储库中的其他资源：

- [示例 Azure Databricks 笔记本](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [示例脚本 (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

此外，还可以查看 [Apache Spark SQL、数据框架和数据集指南](https://spark.apache.org/docs/latest/sql-programming-guide.html)以及 [Azure Databricks 文档](https://docs.microsoft.com/azure/azure-databricks/)。
