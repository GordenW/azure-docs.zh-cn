---
title: 创建用户-超大规模 (Citus) -Azure Database for PostgreSQL
description: 本文介绍如何创建新的用户帐户，以与 Azure Database for PostgreSQL 超大规模 (Citus) 进行交互。
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 1/8/2019
ms.openlocfilehash: 8a1b38b9f673669adb0b5fcf67d9d560c24d5c2a
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87825950"
---
# <a name="create-users-in-azure-database-for-postgresql---hyperscale-citus"></a>在 Azure Database for PostgreSQL-超大规模 (Citus 中创建用户) 

> [!NOTE]
> 术语 "用户" 指的是超大规模 (Citus) 服务器组内的用户。 若要改为了解 Azure 订阅用户及其权限，请访问 azure [RBAC) 一文中的 azure 基于角色的访问控制 (](../role-based-access-control/built-in-roles.md)或查看[如何自定义角色](../role-based-access-control/custom-roles.md)。

## <a name="the-server-admin-account"></a>服务器管理员帐户

PostgreSQL 引擎使用[角色](https://www.postgresql.org/docs/current/sql-createrole.html)来控制对数据库对象的访问，新创建的超大规模 (Citus) 服务器组附带了几个预定义的角色：

* [默认 PostgreSQL 角色](https://www.postgresql.org/docs/current/default-roles.html)
* `azure_pg_admin`
* `postgres`
* `citus`

由于超大规模是托管的 PaaS 服务，因此只有 Microsoft 才能使用 `postgres` 超级用户角色登录。 对于有限的管理访问权限，超大规模提供了 `citus` 角色。

角色的权限 `citus` ：

* 读取所有配置变量，甚至是通常仅对超级
* 读取所有 pg \_ 状态 \_ \* 视图，并使用各种统计信息相关的扩展，甚至是通常仅对超级超级程序可见的视图或扩展。
* 执行可能需要很长时间才能对表进行共享锁定的监视函数。
*  ([创建 PostgreSQL 扩展](concepts-hyperscale-extensions.md)，因为该角色是) 的成员 `azure_pg_admin` 。

特别要注意的是，该 `citus` 角色有一些限制：

* 无法创建角色
* 无法创建数据库

## <a name="how-to-create-additional-user-roles"></a>如何创建其他用户角色

如前所述， `citus` 管理员帐户没有创建其他用户的权限。 若要添加用户，请使用 Azure 门户接口。

1. 中转到超大规模服务器组的 "**角色**" 页，然后单击 " **+ 添加**"：

   !["角色" 页](media/howto-hyperscale-create-users/1-role-page.png)

2. 输入 "角色名称" 和 "密码"。 单击“保存”。

   ![添加角色](media/howto-hyperscale-create-users/2-add-user-fields.png)

将在服务器组的协调器节点上创建用户，并将其传播到所有工作节点。 通过 Azure 门户创建的角色具有 `LOGIN` 属性，这意味着它们是可以登录数据库的真实用户。

## <a name="how-to-modify-privileges-for-user-role"></a>如何修改用户角色的权限

新用户角色通常用于提供具有受限权限的数据库访问权限。 若要修改用户权限，请使用 PgAdmin 或 psql 等工具的标准 PostgreSQL 命令。  (参阅超大规模 () Citus 中的[psql 连接](quickstart-create-hyperscale-portal.md#connect-to-the-database-using-psql)) 

例如，若要允许 `db_user` 读取 `mytable` ，请授予权限：

```sql
GRANT SELECT ON mytable TO db_user;
```

超大规模 (Citus) 通过整个群集传播单表 GRANT 语句，并将其应用于所有工作节点。 它还将传播系统范围内的 (，例如，对于架构中的所有表) ：

```sql
-- applies to the coordinator node and propagates to workers
GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;
```

## <a name="how-to-delete-a-user-role-or-change-their-password"></a>如何删除用户角色或更改其密码

若要更新用户，请访问超大规模服务器组的 "**角色**" 页，然后单击 "用户" 旁边**的省略号 "..."。** 省略号将打开一个菜单以删除用户或重置其密码。

   ![编辑角色](media/howto-hyperscale-create-users/edit-role.png)

该 `citus` 角色具有特权，不能删除。

## <a name="next-steps"></a>后续步骤

为新用户计算机的 IP 地址打开防火墙，使其能够连接：[使用 Azure 门户创建和管理超大规模 (Citus) 防火墙规则](howto-hyperscale-manage-firewall-using-portal.md)。

有关数据库用户帐户管理的详细信息，请参阅 PostgreSQL 产品文档：

* [数据库角色和特权](https://www.postgresql.org/docs/current/static/user-manag.html)
* [GRANT 语法](https://www.postgresql.org/docs/current/static/sql-grant.html)
* [权限](https://www.postgresql.org/docs/current/static/ddl-priv.html)
