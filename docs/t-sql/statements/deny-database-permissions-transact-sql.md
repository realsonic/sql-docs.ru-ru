---
description: DENY, запрет разрешений на базу данных (Transact-SQL)
title: DENY, запрет разрешений на базу данных (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, databases
- permissions [SQL Server], databases
- database permissions [SQL Server], denying
- denying permissions [SQL Server], databases
ms.assetid: 36cc4e2c-5a24-4975-9920-9305f12c6e7c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a9e716cc4e3138c8d43cf63185aa294c50f53cc
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172546"
---
# <a name="deny-database-permissions-transact-sql"></a>DENY, запрет разрешений на базу данных (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Запрещает разрешения на базу данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
DENY <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ CASCADE ]
    [ AS <database_principal> ]

<permission> ::=
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

*permission* — указывает отзываемое разрешение для базы данных. Список разрешений см. в подразделе "Примечания" далее в этом разделе.

ALL — этот параметр запрещает не все возможные разрешения. Указание аргумента ALL эквивалентно запрету следующих разрешений: BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE и CREATE VIEW.

PRIVILEGES — включено для обеспечения совместимости с требованиями ISO. Не изменяет работу ALL.

CASCADE — показывает, что разрешение будет запрещено еще и для тех участников, которым его предоставил указанный участник.

AS \<database_principal> Задает участника, от которого участник, выполняющий данный запрос, получает право на запрет разрешения.

*Database_user* — указывает пользователя базы данных.

*Database_role* — указывает роль базы данных.

*Application_role* — 
**применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

Указывает роль приложения.

*Database_user_mapped_to_Windows_User* — указывает пользователя базы данных, сопоставленного с пользователем Windows.

*Database_user_mapped_to_Windows_Group* — указывает пользователя базы данных, сопоставленного с группой Windows.

*Database_user_mapped_to_certificate*  — указывает пользователя базы данных, сопоставленного с сертификатом.

*Database_user_mapped_to_asymmetric_key* — указывает пользователя базы данных, сопоставленного с асимметричным ключом.

*Database_user_with_no_login* — указывает пользователя базы данных, не сопоставленного с субъектом серверного уровня.

## <a name="remarks"></a>Remarks

База данных — это защищаемый объект, хранящийся на сервере, который является родителем базы данных в иерархии разрешений. Самые специфичные и ограниченные разрешения на работу с базой данных, которые можно запрещать, приведены в следующей таблице вместе с общими разрешениями, неявно их охватывающими.

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
|ALTER ANY DATABASE EVENT SESSION<br /> **Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|ALTER|ALTER ANY EVENT SESSION|
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
|ALTER ANY SECURITY POLICY<br /> **Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
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
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br /> **Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>Разрешения

Участник, выполняющий эту инструкцию (или участник, указанный параметром AS), должен иметь разрешение CONTROL на базу данных или разрешение более высокого уровня, которое включает это разрешение.

Если указан параметр AS, указанный участник должен быть владельцем базы данных.

## <a name="examples"></a>Примеры

### <a name="a-denying-permission-to-create-certificates"></a>A. Запрет разрешения на создание сертификатов

Следующий код отказывает в разрешении `CREATE CERTIFICATE` пользователю `MelanieK` базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].

```sql
USE AdventureWorks2012;
DENY CREATE CERTIFICATE TO MelanieK;
GO
```

### <a name="b-denying-references-permission-to-an-application-role"></a>Б. Запрет разрешения REFERENCES для роли приложения

Следующий код запрещает разрешение `REFERENCES`, связанное с базой данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] для роли приложения `AuditMonitor`.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

```sql
USE AdventureWorks2012;
DENY REFERENCES TO AuditMonitor;
GO
```

### <a name="c-denying-view-definition-with-cascade"></a>В. Запрет разрешения на просмотр определения базы данных с аргументом CASCADE

Следующий код запрещает разрешение `VIEW DEFINITION`, связанное с базой данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], для пользователя `CarmineEs` и всех участников, которым `CarmineEs` предоставил разрешение `VIEW DEFINITION`.

```sql
USE AdventureWorks2012;
DENY VIEW DEFINITION TO CarmineEs CASCADE;
GO
```

## <a name="see-also"></a>См. также:

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [Разрешения](../../relational-databases/security/permissions-database-engine.md)
- [Субъекты](../../relational-databases/security/authentication-access/principals-database-engine.md)
