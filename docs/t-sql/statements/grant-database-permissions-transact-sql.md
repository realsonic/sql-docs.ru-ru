---
description: GRANT, предоставление разрешений на базу данных (Transact-SQL)
title: GRANT, предоставление разрешений на базу данных (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server]
- database permissions [SQL Server]
- granting permissions [SQL Server], databases
- permissions [SQL Server], databases
- database permissions [SQL Server], granting
- GRANT statement, databases
ms.assetid: 499e5ed6-945c-4791-ab45-68dec0b9c289
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 18a18c170ffddafca6656f7f3092a22e1f3b6524
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171716"
---
# <a name="grant-database-permissions-transact-sql"></a>GRANT, предоставление разрешений на базу данных (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Предоставляет разрешения на базу данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql

GRANT <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ WITH GRANT OPTION ]
    [ AS <database_principal> ]

<permission>::=
permission | ALL [ PRIVILEGES ]

<database_principal> ::=
    Database_user
  | Database_role
  | Application_role
  | Database_user_mapped_to_Windows_User
  | Database_user_mapped_to_Windows_Group
  | Database_user_mapped_to_certificate
  | Database_user_mapped_to_asymmetric_key
  | Database_user_with_no_login
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*permission* — указывает предоставляемое разрешение для базы данных. Список разрешений см. в подразделе "Примечания" далее в этом разделе.

ALL — этот параметр предоставляет не все возможные разрешения. Предоставление разрешения ALL эквивалентно предоставлению следующих разрешений: BACKUP DATABASE, BACKUP LOG, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE и CREATE VIEW.

PRIVILEGES — включено для обеспечения совместимости с требованиями ANSI-92. Не изменяет работу ALL.

WITH GRANT OPTION — указывает, что субъекту будет дана возможность предоставлять заданное разрешение другим субъектам.

AS \<database_principal>. Указывает субъект, от которого субъект, выполняющий данный запрос, наследует право на предоставление разрешения.

*Database_user* — указывает пользователя базы данных.

*Database_role* — указывает роль базы данных.

*Application_role* — 
**применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

Указывает роль приложения.

*Database_user_mapped_to_Windows_User* — 
**применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше

Указывает пользователя базы данных, сопоставленного с пользователем Windows.

*Database_user_mapped_to_Windows_Group* — 
**применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше

Указывает пользователя базы данных, сопоставленного с группой Windows.

*Database_user_mapped_to_certificate* — 
**применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше

Указывает пользователя базы данных, сопоставленного с сертификатом.

*Database_user_mapped_to_asymmetric_key* — 
**применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше

Указывает пользователя базы данных, сопоставленного с асимметричным ключом.

*Database_user_with_no_login* — указывает пользователя базы данных, не сопоставленного с субъектом серверного уровня.

## <a name="remarks"></a>Remarks

> [!IMPORTANT]
> Сочетание разрешений ALTER и REFERENCE в некоторых случаях может позволить просматривать данные или выполнять несанкционированные функции. Пример: Пользователь с разрешением ALTER на таблицу и разрешением REFERENCE на функцию может создавать вычисляемый столбец на основе функции и в результате — выполнять ее. В этом случае пользователю также требуется разрешение SELECT на вычисляемый столбец.

База данных — это защищаемый объект, хранящийся на сервере, который является родителем базы данных в иерархии разрешений. Наиболее специфичные и ограниченные разрешения, которые можно предоставлять в базе данных, перечислены в следующей таблице вместе с общими разрешениями, неявно содержащими их.

|Разрешение в базе данных|Содержится в разрешении базы данных|Подразумевается в разрешении сервера|
|-------------------------|------------------------------------|----------------------------------|
|ADMINISTER DATABASE BULK OPERATIONS<br/>**Область применения**: [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER|CONTROL|ALTER ANY DATABASE|
|ALTER ANY APPLICATION ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ASSEMBLY|ALTER|CONTROL SERVER|
|ALTER ANY ASYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY CERTIFICATE|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN ENCRYPTION KEY|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN MASTER KEY DEFINITION|ALTER|CONTROL SERVER|
|ALTER ANY CONTRACT|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE AUDIT|ALTER|ALTER ANY SERVER AUDIT|
|ALTER ANY DATABASE DDL TRIGGER|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE EVENT NOTIFICATION|ALTER|ALTER ANY EVENT NOTIFICATION|
|ALTER ANY DATABASE EVENT SESSION<br />**Применимо к**: [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|ALTER|ALTER ANY EVENT SESSION|
|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY DATASPACE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL DATA SOURCE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL FILE FORMAT|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL LIBRARY <br /> **Применимо к**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].|CONTROL|CONTROL SERVER |
|ALTER ANY FULLTEXT CATALOG|ALTER|CONTROL SERVER|
|ALTER ANY MASK|CONTROL|CONTROL SERVER|
|ALTER ANY MESSAGE TYPE|ALTER|CONTROL SERVER|
|ALTER ANY REMOTE SERVICE BINDING|ALTER|CONTROL SERVER|
|ALTER ANY ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ROUTE|ALTER|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
|ALTER ANY SECURITY POLICY<br /> **Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY SERVICE|ALTER|CONTROL SERVER|
|ALTER ANY SYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY USER|ALTER|CONTROL SERVER|
|AUTHENTICATE|CONTROL|AUTHENTICATE SERVER|
|BACKUP DATABASE|CONTROL|CONTROL SERVER|
|BACKUP LOG|CONTROL|CONTROL SERVER|
|CHECKPOINT|CONTROL|CONTROL SERVER|
|CONNECT|CONNECT REPLICATION|CONTROL SERVER|
|CONNECT REPLICATION|CONTROL|CONTROL SERVER|
|CONTROL|CONTROL|CONTROL SERVER|
|CREATE AGGREGATE|ALTER|CONTROL SERVER|
|CREATE ANY EXTERNAL LIBRARY <br /> **Применимо к**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].|CONTROL|CONTROL SERVER |
|CREATE ASSEMBLY|ALTER ANY ASSEMBLY|CONTROL SERVER|
|CREATE ASYMMETRIC KEY|ALTER ANY ASYMMETRIC KEY|CONTROL SERVER|
|CREATE CERTIFICATE|ALTER ANY CERTIFICATE|CONTROL SERVER|
|CREATE CONTRACT|ALTER ANY CONTRACT|CONTROL SERVER|
|CREATE DATABASE|CONTROL|CREATE ANY DATABASE|
|CREATE DATABASE DDL EVENT NOTIFICATION|ALTER ANY DATABASE EVENT NOTIFICATION|CREATE DDL EVENT NOTIFICATION|
|CREATE DEFAULT|ALTER|CONTROL SERVER|
|CREATE FULLTEXT CATALOG|ALTER ANY FULLTEXT CATALOG|CONTROL SERVER|
|CREATE FUNCTION|ALTER|CONTROL SERVER|
|CREATE MESSAGE TYPE|ALTER ANY MESSAGE TYPE|CONTROL SERVER|
|CREATE PROCEDURE|ALTER|CONTROL SERVER|
|CREATE QUEUE|ALTER|CONTROL SERVER|
|CREATE REMOTE SERVICE BINDING|ALTER ANY REMOTE SERVICE BINDING|CONTROL SERVER|
|CREATE ROLE|ALTER ANY ROLE|CONTROL SERVER|
|CREATE ROUTE|ALTER ANY ROUTE|CONTROL SERVER|
|CREATE RULE|ALTER|CONTROL SERVER|
|CREATE SCHEMA|ALTER ANY SCHEMA|CONTROL SERVER|
|CREATE SERVICE|ALTER ANY SERVICE|CONTROL SERVER|
|CREATE SYMMETRIC KEY|ALTER ANY SYMMETRIC KEY|CONTROL SERVER|
|CREATE SYNONYM|ALTER|CONTROL SERVER|
|CREATE TABLE|ALTER|CONTROL SERVER|
|CREATE TYPE|ALTER|CONTROL SERVER|
|CREATE VIEW|ALTER|CONTROL SERVER|
|CREATE XML SCHEMA COLLECTION|ALTER|CONTROL SERVER|
|DELETE|CONTROL|CONTROL SERVER|
|EXECUTE|CONTROL|CONTROL SERVER|
|EXECUTE ANY EXTERNAL SCRIPT <br /> **Применимо к**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)].|CONTROL|CONTROL SERVER|
|EXECUTE EXTERNAL SCRIPT <br /> **Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)].|EXECUTE ANY EXTERNAL SCRIPT|CONTROL SERVER|
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br />**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY COLUMN MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>Разрешения

Объект, предоставляющий разрешение (или участник, указанный параметром AS), должен иметь либо само разрешение, выданное с помощью параметра GRANT OPTION, либо разрешение более высокого уровня, которое неявно включает предоставляемое.

При использовании параметра AS налагаются следующие дополнительные требования.

|AS *granting_principal*|Необходимо дополнительное разрешение|
|------------------------------|------------------------------------|
|пользователь базы данных;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|
|пользователь базы данных, сопоставленный с именем входа Windows;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|
|Пользователь базы данных, сопоставленный группе Windows|Членство в группе Windows, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|
|пользователь базы данных, сопоставленный с сертификатом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|
|пользователь базы данных, сопоставленный с асимметричным ключом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|
|Пользователь базы данных, не сопоставленный ни с одним участником на уровне сервера|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|
|роль базы данных;|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|
|Роль приложения|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|

Владельцы объектов могут предоставлять разрешения на объекты, которыми они владеют. Участники, имеющие разрешение CONTROL на защищаемый объект, могут предоставлять разрешение на этот защищаемый объект.

Участники, которым предоставлено разрешение CONTROL SERVER, такие как члены предопределенной роли сервера &lt;legacyBold&gt;sysadmin&lt;/legacyBold&gt;, могут предоставлять любое разрешение на любой защищаемый объект сервера.

## <a name="examples"></a>Примеры

### <a name="a-granting-permission-to-create-tables"></a>A. Предоставление разрешения на создание таблиц

В следующем примере пользователю `MelanieK` предоставляется разрешение `CREATE TABLE` для базы данных `AdventureWorks`.

```sql
USE AdventureWorks;
GRANT CREATE TABLE TO MelanieK;
GO
```

### <a name="b-granting-showplan-permission-to-an-application-role"></a>Б. Предоставление разрешения SHOWPLAN роли приложения

 В следующем примере роли приложения `SHOWPLAN` предоставляется разрешение `AdventureWorks2012` в базе данных `AuditMonitor`.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

```sql
USE AdventureWorks2012;
GRANT SHOWPLAN TO AuditMonitor;
GO
```

### <a name="c-granting-create-view-with-grant-option"></a>В. Предоставление разрешения CREATE VIEW с параметром GRANT OPTION

 В следующем примере пользователю `CREATE VIEW` предоставляется разрешение `AdventureWorks2012` в базе данных `CarmineEs` с правом предоставлять разрешение `CREATE VIEW` другим участникам.

```sql
USE AdventureWorks2012;
GRANT CREATE VIEW TO CarmineEs WITH GRANT OPTION;
GO
```

### <a name="d-granting-control-permission-to-a-database-user"></a>Г. Предоставление разрешения CONTROL пользователю базы данных

 В следующем примере пользователю `Sarah` предоставляется разрешение `CONTROL` для базы данных `AdventureWorks2012`. Пользователь должен существовать в базе данных, которая должна быть настроена в качестве контекста.

```sql
USE AdventureWorks2012;
GRANT CONTROL ON DATABASE::AdventureWorks2012 TO Sarah;
GO
```

## <a name="see-also"></a>См. также:

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [Разрешения](../../relational-databases/security/permissions-database-engine.md)
- [Субъекты](../../relational-databases/security/authentication-access/principals-database-engine.md)
