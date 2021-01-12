---
description: sys.fn_builtin_permissions (Transact-SQL)
title: sys.fn_builtin_permissions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_builtin_permissions
- sys.fn_builtin_permissions_TSQL
- fn_builtin_permissions_TSQL
- sys.fn_builtin_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- compact permissions types
- viewing permission hierarchy
- hierarchies [SQL Server], permissions
- built-in permissions hierarchy [SQL Server]
- fn_builtin_permissions function
- permissions [SQL Server], hierarchy
- displaying permission hierarchy
- sys.fn_builtin_permissions function
ms.assetid: 704b1ad3-3534-4cf3-aff4-9fb70064b6cc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ca23195b680ca5e2e16010b04c357fa2369d7f0a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093882"
---
# <a name="sysfn_builtin_permissions-transact-sql"></a>sys.fn_builtin_permissions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Функция возвращает описание иерархии встроенных разрешений сервера. `sys.fn_builtin_permissions` может вызываться только для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , и возвращает все разрешения независимо от того, поддерживаются ли они на текущей платформе. Большинство разрешений применяются ко всем платформам, но некоторые из них — только к определенным. Например, разрешения уровня сервера не могут быть предоставлены в базе данных SQL. Сведения о том, какие платформы поддерживают каждое разрешение, см. в разделе [разрешения &#40;ядро СУБД&#41;](../../relational-databases/security/permissions-database-engine.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_builtin_permissions ( [ DEFAULT | NULL ]  
    | empty_string | '<securable_class>' } )  
  
<securable_class> ::=   
      APPLICATION ROLE | ASSEMBLY | ASYMMETRIC KEY | AVAILABILITY GROUP  
    | CERTIFICATE | CONTRACT | DATABASE | DATABASE SCOPED CREDENTIAL    
    | ENDPOINT | FULLTEXT CATALOG | FULLTEXT STOPLIST | LOGIN      
    | MESSAGE TYPE | OBJECT | REMOTE SERVICE BINDING | ROLE | ROUTE    
    | SCHEMA | SEARCH PROPERTY LIST | SERVER | SERVER ROLE | SERVICE    
    | SYMMETRIC KEY | TYPE | USER | XML SCHEMA COLLECTION  
```  
  
## <a name="arguments"></a>Аргументы  
 DEFAULT  
 При вызове с параметром по УМОЛЧАНИю (без кавычек) функция возвращает полный список встроенных разрешений.  
  
 NULL  
 Эквивалентен DEFAULT.  
  
 *empty_string*  
 Эквивалентен DEFAULT.  
  
 **"**<securable_class>**"**  
 При вызове с именем одного защищаемого класса sys.fn_builtin_permissions возвратит все разрешения, применяемые к классу. <securable_class> — строковый литерал, для которого требуются кавычки. **nvarchar(60)**  
  
## <a name="tables-returned"></a>Возвращаемые таблицы  
  
|Имя столбца|Тип данных|Параметры сортировки|Описание|  
|-----------------|---------------|---------------|-----------------|  
|class_desc|**nvarchar(60)**|Параметры сортировки сервера|Описание защищаемого класса.|  
|permission_name|**nvarchar(60)**|Параметры сортировки сервера|Имя разрешения.|  
|тип|**varchar(4)**|Параметры сортировки сервера|Код компактных типов разрешений. См. таблицу далее.|  
|covering_permission_name|**nvarchar(60)**|Параметры сортировки сервера|Если значение не NULL, то это имя разрешения на данный класс, который содержит в себе другие разрешения данного класса.|  
|parent_class_desc|**nvarchar(60)**|Параметры сортировки сервера|Если значение не NULL, то это имя родительского класса, который содержит текущий класс.|  
|parent_covering_permission_name|**nvarchar(60)**|Параметры сортировки сервера|Если значение не NULL, то это имя разрешения на родительский класс, который содержит в себе другие разрешения класса.|  
  
### <a name="permission-types"></a>Типы разрешений  
  
|Тип разрешения|Имя разрешения|Применяется к защищаемому объекту или классу|  
|---------------------|---------------------|-----------------------------------|  
|AADS|ALTER ANY DATABASE EVENT SESSION<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|AAES|ALTER ANY EVENT SESSION|SERVER|  
|AAMK|ALTER ANY MASK<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|ADBO|ADMINISTER BULK OPERATIONS|SERVER|  
|AEDS|ALTER ANY EXTERNAL DATA SOURCE<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|AEFF|ALTER ANY EXTERNAL FILE FORMAT<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|AL|ALTER|APPLICATION ROLE|  
|AL|ALTER|ASSEMBLY|  
|AL|ALTER<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|AVAILABILITY GROUP|  
|AL|ALTER|ASYMMETRIC KEY|  
|AL|ALTER|CERTIFICATE|  
|AL|ALTER|CONTRACT|  
|AL|ALTER|DATABASE|  
|AL|ALTER<br /> **Применяет t o**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] . |DATABASE SCOPED CREDENTIAL|
|AL|ALTER|ENDPOINT|  
|AL|ALTER|FULLTEXT CATALOG|  
|AL|ALTER|FULLTEXT STOPLIST|  
|AL|ALTER|Имя_для_входа|  
|AL|ALTER|MESSAGE TYPE|  
|AL|ALTER|OBJECT|  
|AL|ALTER|REMOTE SERVICE BINDING|  
|AL|ALTER|ROLE|  
|AL|ALTER|ROUTE|  
|AL|ALTER|SCHEMA|  
|AL|ALTER|SEARCH PROPERTY LIST|  
|AL|ALTER<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER ROLE|  
|AL|ALTER|SERVICE|  
|AL|ALTER|SYMMETRIC KEY|  
|AL|ALTER|Пользователь|  
|AL|ALTER|XML SCHEMA COLLECTION|  
|ALAA|ALTER ANY SERVER AUDIT|SERVER|  
|ALAG|ALTER ANY AVAILABILITY GROUP<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER|  
|ALAK|ALTER ANY ASYMMETRIC KEY|DATABASE|  
|ALAR|ALTER ANY APPLICATION ROLE|DATABASE|  
|ALAS|ALTER ANY ASSEMBLY|DATABASE|  
|ALCD|ALTER ANY CREDENTIAL|SERVER|  
|ALCF|ALTER ANY CERTIFICATE|DATABASE|  
|ALCK|ALTER ANY COLUMN ENCRYPTION KEY<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|ALCM|ALTER ANY COLUMN MASTER KEY<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|ALCO|ALTER ANY CONNECTION|SERVER|  
|ALDA|ALTER ANY DATABASE AUDIT|DATABASE|  
|ALDB|ALTER ANY DATABASE|SERVER|  
|ALDC|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|ALDS|ALTER ANY DATASPACE|DATABASE|  
|ALED|ALTER ANY DATABASE EVENT NOTIFICATION|DATABASE|  
|ALES|ALTER ANY EVENT NOTIFICATION|SERVER|  
|ALFT|ALTER ANY FULLTEXT CATALOG|DATABASE|  
|ALHE|ALTER ANY ENDPOINT|SERVER|  
|ALLG|ALTER ANY LOGIN|SERVER|  
|ALLS|ALTER ANY LINKED SERVER|SERVER|  
|ALMT|ALTER ANY MESSAGE TYPE|DATABASE|  
|ALRL|ALTER ANY ROLE|DATABASE|  
|ALRS|ALTER RESOURCES|SERVER|  
|ALRT|ALTER ANY ROUTE|DATABASE|  
|ALSB|ALTER ANY REMOTE SERVICE BINDING|DATABASE|  
|ALSC|ALTER ANY CONTRACT|DATABASE|  
|ALSK|ALTER ANY SYMMETRIC KEY|DATABASE|  
|ALSM|ALTER ANY SCHEMA|DATABASE|  
|ALSP|ALTER ANY SECURITY POLICY<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|ALSR|ALTER ANY SERVER ROLE<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER|  
|ALSS|ALTER SERVER STATE|SERVER|  
|ALST|ALTER SETTINGS|SERVER|  
|ALSV|ALTER ANY SERVICE|DATABASE|  
|ALTG|ALTER ANY DATABASE DDL TRIGGER|DATABASE|  
|ALTR|ALTER TRACE|SERVER|  
|ALUS|ALTER ANY USER|DATABASE|  
|AUTH|AUTHENTICATE|DATABASE|  
|AUTH|AUTHENTICATE SERVER|SERVER|  
|BADB|BACKUP DATABASE|DATABASE|  
|BALO|BACKUP LOG|DATABASE|  
|CADB|CONNECT ANY DATABASE<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER|  
|CL|CONTROL|APPLICATION ROLE|  
|CL|CONTROL|ASSEMBLY|  
|CL|CONTROL|ASYMMETRIC KEY|  
|CL|CONTROL<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|AVAILABILITY GROUP|  
|CL|CONTROL|CERTIFICATE|  
|CL|CONTROL|CONTRACT|  
|CL|CONTROL|DATABASE|  
|CL|CONTROL<br /> **Применимо к**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. |DATABASE SCOPED CREDENTIAL|
|CL|CONTROL|ENDPOINT|  
|CL|CONTROL|FULLTEXT CATALOG|  
|CL|CONTROL|FULLTEXT STOPLIST|  
|CL|CONTROL|Имя_для_входа|  
|CL|CONTROL|MESSAGE TYPE|  
|CL|CONTROL|OBJECT|  
|CL|CONTROL|REMOTE SERVICE BINDING|  
|CL|CONTROL|ROLE|  
|CL|CONTROL|ROUTE|  
|CL|CONTROL|SCHEMA|  
|CL|CONTROL|SEARCH PROPERTY LIST|  
|CL|CONTROL SERVER|SERVER|  
|CL|CONTROL<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER ROLE|  
|CL|CONTROL|SERVICE|  
|CL|CONTROL|SYMMETRIC KEY|  
|CL|CONTROL|TYPE|  
|CL|CONTROL|Пользователь|  
|CL|CONTROL|XML SCHEMA COLLECTION|  
|CO|CONNECT|DATABASE|  
|CO|CONNECT|ENDPOINT|  
|CORP|CONNECT REPLICATION|DATABASE|  
|COSQ|CONNECT SQL|SERVER|  
|CP|CHECKPOINT|DATABASE|  
|CRAC|Создание группы доступности<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER|  
|CRAG|CREATE AGGREGATE|DATABASE|  
|CRAK|CREATE ASYMMETRIC KEY|DATABASE|  
|CRAS|CREATE ASSEMBLY|DATABASE|  
|CRCF|CREATE CERTIFICATE|DATABASE|  
|CRDB|CREATE ANY DATABASE|SERVER|  
|CRDB|CREATE DATABASE|DATABASE|  
|CRDE|CREATE DDL EVENT NOTIFICATION|SERVER|  
|CRDF|CREATE DEFAULT|DATABASE|  
|CRED|CREATE DATABASE DDL EVENT NOTIFICATION|DATABASE|  
|CRFN|CREATE FUNCTION|DATABASE|  
|CRFT|CREATE FULLTEXT CATALOG|DATABASE|  
|CRHE|CREATE ENDPOINT|SERVER|  
|CRMT|CREATE MESSAGE TYPE|DATABASE|  
|CRPR|CREATE PROCEDURE|DATABASE|  
|CRQU|CREATE QUEUE|DATABASE|  
|CRRL|CREATE ROLE|DATABASE|  
|CRRT|CREATE ROUTE|DATABASE|  
|CRRU|CREATE RULE|DATABASE|  
|CRSB|CREATE REMOTE SERVICE BINDING|DATABASE|  
|CRSC|CREATE CONTRACT|DATABASE|  
|CRSK|CREATE SYMMETRIC KEY|DATABASE|  
|CRSM|CREATE SCHEMA|DATABASE|  
|CRSN|CREATE SYNONYM|DATABASE|  
|CRSO|CREATE SEQUENCE|SCHEMA|  
|CRSR|CREATE SERVER ROLE<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER|  
|CRSV|CREATE SERVICE|DATABASE|  
|CRTB|CREATE TABLE|DATABASE|  
|CRTE|CREATE TRACE EVENT NOTIFICATION|SERVER|  
|CRTY|CREATE TYPE|DATABASE|  
|CRVW|CREATE VIEW|DATABASE|  
|CRXS|CREATE XML SCHEMA COLLECTION|DATABASE|  
|DABO|ADMINISTER DATABASE BULK OPERATIONS<br /> **Применимо к**: [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|DATABASE|  
|DL|DELETE|DATABASE|  
|DL|DELETE|OBJECT|  
|DL|DELETE|SCHEMA|  
|EAES|EXECUTE ANY EXTERNAL SCRIPT<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|EX|EXECUTE|DATABASE|  
|EX|EXECUTE|OBJECT|  
|EX|EXECUTE|SCHEMA|  
|EX|EXECUTE|TYPE|  
|EX|EXECUTE|XML SCHEMA COLLECTION|  
|IAL|IMPERSONATE ANY LOGIN<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER|  
|IM|IMPERSONATE|Имя_для_входа|  
|IM|IMPERSONATE|Пользователь|  
|IN|INSERT|DATABASE|  
|IN|INSERT|OBJECT|  
|IN|INSERT|SCHEMA|  
|KIDC|KILL DATABASE CONNECTION<br />**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|DATABASE|  
|RC|RECEIVE|OBJECT|  
|RF;|REFERENCES|ASSEMBLY|  
|RF;|REFERENCES|ASYMMETRIC KEY|  
|RF;|REFERENCES|CERTIFICATE|  
|RF;|REFERENCES|CONTRACT|  
|RF;|REFERENCES|DATABASE|  
|RF;|REFERENCES<br /> **Применимо к**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. |DATABASE SCOPED CREDENTIAL|
|RF;|REFERENCES|FULLTEXT CATALOG|  
|RF;|REFERENCES|FULLTEXT STOPLIST|  
|RF;|REFERENCES|SEARCH PROPERTY LIST|  
|RF;|REFERENCES|MESSAGE TYPE|  
|RF;|REFERENCES|OBJECT|  
|RF;|REFERENCES|SCHEMA|  
|RF;|REFERENCES|SYMMETRIC KEY|  
|RF;|REFERENCES|TYPE|  
|RF;|REFERENCES|XML SCHEMA COLLECTION|  
|SHDN|SHUTDOWN|SERVER|  
|SL|SELECT|DATABASE|  
|SL|SELECT|OBJECT|  
|SL|SELECT|SCHEMA|  
|SN|SEND|SERVICE|  
|SPLN|SHOWPLAN|DATABASE|  
|SUQN|SUBSCRIBE QUERY NOTIFICATIONS|DATABASE|  
|SUS|SELECT ALL USER SECURABLES<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER|  
|TO|TAKE OWNERSHIP|ASSEMBLY|  
|TO|TAKE OWNERSHIP|ASYMMETRIC KEY|  
|TO|TAKE OWNERSHIP<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|AVAILABILITY GROUP|  
|TO|TAKE OWNERSHIP|CERTIFICATE|  
|TO|TAKE OWNERSHIP|CONTRACT|  
|TO|TAKE OWNERSHIP|DATABASE|  
|TO|TAKE OWNERSHIP<br /> **Применимо к**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. |DATABASE SCOPED CREDENTIAL|
|TO|TAKE OWNERSHIP|ENDPOINT|  
|TO|TAKE OWNERSHIP|FULLTEXT CATALOG|  
|TO|TAKE OWNERSHIP|FULLTEXT STOPLIST|  
|TO|TAKE OWNERSHIP|SEARCH PROPERTY LIST|  
|TO|TAKE OWNERSHIP|MESSAGE TYPE|  
|TO|TAKE OWNERSHIP|OBJECT|  
|TO|TAKE OWNERSHIP|REMOTE SERVICE BINDING|  
|TO|TAKE OWNERSHIP|ROLE|  
|TO|TAKE OWNERSHIP|ROUTE|  
|TO|TAKE OWNERSHIP|SCHEMA|  
|TO|TAKE OWNERSHIP<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER ROLE|  
|TO|TAKE OWNERSHIP|SERVICE|  
|TO|TAKE OWNERSHIP|SYMMETRIC KEY|  
|TO|TAKE OWNERSHIP|TYPE|  
|TO|TAKE OWNERSHIP|XML SCHEMA COLLECTION|  
|UMSK|UNMASK<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|UP|UPDATE|DATABASE|  
|UP|UPDATE|OBJECT|  
|UP|UPDATE|SCHEMA|  
|VW|VIEW DEFINITION|APPLICATION ROLE|  
|VW|VIEW DEFINITION|ASSEMBLY|  
|VW|VIEW DEFINITION|ASYMMETRIC KEY|  
|VW|VIEW DEFINITION<br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|AVAILABILITY GROUP|  
|VW|VIEW DEFINITION|CERTIFICATE|  
|VW|VIEW DEFINITION|CONTRACT|  
|VW|VIEW DEFINITION|DATABASE|  
|VW|VIEW DEFINITION<br /> **Применимо к**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. |DATABASE SCOPED CREDENTIAL|
|VW|VIEW DEFINITION|ENDPOINT|  
|VW|VIEW DEFINITION|FULLTEXT CATALOG|  
|VW|VIEW DEFINITION|FULLTEXT STOPLIST|  
|VW|VIEW DEFINITION|Имя_для_входа|  
|VW|VIEW DEFINITION|MESSAGE TYPE|  
|VW|VIEW DEFINITION|OBJECT|  
|VW|VIEW DEFINITION|REMOTE SERVICE BINDING|  
|VW|VIEW DEFINITION|ROLE|  
|VW|VIEW DEFINITION|ROUTE|  
|VW|VIEW DEFINITION|SCHEMA|  
|VW|VIEW DEFINITION|SEARCH PROPERTY LIST|  
|VW|VIEW DEFINITION<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|SERVER ROLE|  
|VW|VIEW DEFINITION|SERVICE|  
|VW|VIEW DEFINITION|SYMMETRIC KEY|  
|VW|VIEW DEFINITION|TYPE|  
|VW|VIEW DEFINITION|Пользователь|  
|VW|VIEW DEFINITION|XML SCHEMA COLLECTION|  
|VWAD|VIEW ANY DEFINITION|SERVER|  
|VWCK|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|ввкм|VIEW ANY COLUMN MASTER KEY DEFINITION<br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)).|DATABASE|  
|VWCT|VIEW CHANGE TRACKING|OBJECT|  
|VWCT|VIEW CHANGE TRACKING|SCHEMA|  
|VWDB|VIEW ANY DATABASE|SERVER|  
|VWDS|VIEW DATABASE STATE|DATABASE|  
|VWSS|VIEW SERVER STATE|SERVER|  
|XA|EXTERNAL ACCESS ASSEMBLY|SERVER|  
|XU|UNSAFE ASSEMBLY|SERVER|  
  
## <a name="remarks"></a>Комментарии  
 Функция с табличным значением `sys.fn_builtin_permissions` выдает копию стандартной иерархии разрешений. Такая иерархия включает в себя покрытие разрешений. `DEFAULT`Результирующий набор описывает направленную ациклический диаграмму иерархии разрешений, для которой корневой каталог имеет значение (класс = сервер, разрешение = сервер управления).  
  
 Функция `sys.fn_builtin_permissions` не принимает связанные параметры.  
  
 Функция `sys.fn_builtin_permissions` возвращает пустой набор, если вызвана с недопустимым именем класса.  
 
[!INCLUDE[database-engine-permissions](../../includes/paragraph-content/database-engine-permissions.md)]
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли public.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-listing-all-built-in-permissions"></a>A. Листинг всех встроенных разрешений   
`DEFAULT`Чтобы получить все разрешения, используйте или пустую строку.   
```sql  
SELECT * FROM sys.fn_builtin_permissions(DEFAULT);
SELECT * FROM sys.fn_builtin_permissions('');  
```  
  
### <a name="b-listing-permissions-that-can-be-set-on-a-symmetric-key"></a>Б. Листинг разрешений для симметричного ключа   
Укажите класс, чтобы получить все возможные разрешения для этого класса.   
```sql  
SELECT * FROM sys.fn_builtin_permissions(N'SYMMETRIC KEY');  
```  
  
### <a name="c-listing-classes-on-which-there-is-a-select-permission"></a>В. Листинг классов с разрешением SELECT   
  
```sql  
SELECT * FROM sys.fn_builtin_permissions(DEFAULT)   
    WHERE permission_name = 'SELECT';  
```  
  
## <a name="see-also"></a>См. также:  
 [Иерархия разрешений (ядро СУБД)](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [CREATE SCHEMA (Transact-SQL)](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
