---
description: REVOKE, отмена разрешения на базу данных (Transact-SQL)
title: REVOKE, отмена разрешения на базу данных (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], databases
- database permissions [SQL Server], revoking
- REVOKE statement, databases
ms.assetid: 442acfc6-af97-40a3-b546-91cd485ee2be
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 428e65afa06b26e04f3724003da59501288c3fd8
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172346"
---
# <a name="revoke-database-permissions-transact-sql"></a>REVOKE, отмена разрешения на базу данных (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Производит отзыв разрешений, предоставляющих или запрещающих доступ к базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] <permission> [ ,...n ]    
    { TO | FROM } <database_principal> [ ,...n ]   
        [ CASCADE ]  
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
 *permission*  
 Указывает отзываемое разрешение для базы данных. Список разрешений см. в подразделе "Примечания" далее в этом разделе.  
  
 ALL  
 Этот параметр не отменяет все возможные разрешения. Отзыв ALL эквивалентен отзыву следующих разрешений: BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE и CREATE VIEW.  
  
 PRIVILEGES  
 Включено для обеспечения совместимости с требованиями ISO. Не изменяет работу ALL.  
  
 GRANT OPTION  
 Показывает, что отменяется право на предоставление указанного разрешения другим участникам. Само разрешение отменено не будет.  
  
> [!IMPORTANT]  
>  Если участник обладает указанным разрешением без параметра GRANT, будет отменено само разрешение.  
  
 CASCADE  
 Показывает, что отменяемое разрешение также отменяется для других участников, для которых оно было предоставлено или запрещено данным участником.  
  
> [!CAUTION]  
>  Каскадная отмена разрешения, предоставленного с помощью параметра WITH GRANT OPTION, приведет к отмене разрешений GRANT и DENY для этого разрешения.  
  
 AS \<database_principal> Указывает субъект, от которого субъект, выполняющий данный запрос, получает право на отмену разрешения.  
  
 *Database_user*  
 Указывает пользователя базы данных.  
  
 *Database_role*  
 Указывает роль базы данных.  
  
 *Application_role*  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 Указывает роль приложения.  
  
 *Database_user_mapped_to_Windows_User*  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
 Указывает пользователя базы данных, сопоставленного с пользователем Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
 Указывает пользователя базы данных, сопоставленного с группой Windows.  
  
 *Database_user_mapped_to_certificate*  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
 Указывает пользователя базы данных, сопоставленного с сертификатом.  
  
 *Database_user_mapped_to_asymmetric_key*  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
 Указывает пользователя базы данных, сопоставленного с асимметричным ключом.  
  
 *Database_user_with_no_login*  
 Указывает пользователя базы данных, не сопоставленного с субъектом серверного уровня.  
  
## <a name="remarks"></a>Remarks  
 Инструкция завершится ошибкой, если параметр CASCADE не указан при отзыве разрешения у участника, который предоставлял это разрешение с указанием параметра GRANT OPTION.  
  
 База данных — это защищаемый объект, хранящийся на сервере, который является родителем базы данных в иерархии разрешений. В следующей таблице перечислены отдельные разрешения на базу данных, которые могут быть отозваны, а также общие разрешения, которые неявно их включают.  
  
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
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|  
|ALTER ANY SECURITY POLICY<br />**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|CONTROL SERVER|  
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
|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|VIEW ANY COLUMN MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>Разрешения  
 Участник, выполняющий эту инструкцию (или участник, указанный параметром AS), должен иметь разрешение CONTROL на базу данных или разрешение более высокого уровня, которое включает это разрешение.  
  
 Если указан параметр AS, указанный участник должен быть владельцем базы данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-revoking-permission-to-create-certificates"></a>A. Отмена разрешения на создание сертификатов  
 В следующем примере производится отмена разрешения `CREATE CERTIFICATE` на базу данных `AdventureWorks2012` для пользователя `MelanieK`.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
```sql  
USE AdventureWorks2012;  
REVOKE CREATE CERTIFICATE FROM MelanieK;  
GO  
```  
  
### <a name="b-revoking-references-permission-from-an-application-role"></a>Б. Отмена разрешения REFERENCES для роли приложения  
 В следующем примере производится отмена разрешения `REFERENCES` на базу данных `AdventureWorks2012` для роли приложения `AuditMonitor`.  
  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
```sql  
USE AdventureWorks2012;  
REVOKE REFERENCES FROM AuditMonitor;  
GO  
```  
  
### <a name="c-revoking-view-definition-with-cascade"></a>В. Отмена разрешения VIEW DEFINITION с параметром CASCADE  
 В следующем примере производится отмена разрешения `VIEW DEFINITION` на базу данных `AdventureWorks2012` для пользователя `CarmineEs` и всех участников, которым `CarmineEs` предоставил разрешение `VIEW DEFINITION`.  
  
```sql  
USE AdventureWorks2012;  
REVOKE VIEW DEFINITION FROM CarmineEs CASCADE;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [GRANT, предоставление разрешений на базу данных (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)   
 [DENY, запрет разрешений на базу данных (Transact-SQL)](../../t-sql/statements/deny-database-permissions-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Участники (компонент Database Engine)](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

