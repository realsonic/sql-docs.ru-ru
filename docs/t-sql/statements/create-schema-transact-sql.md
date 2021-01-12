---
description: CREATE SCHEMA (Transact-SQL)
title: CREATE SCHEMA (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SCHEMA_TSQL
- SCHEMA
- CREATE SCHEMA
- SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], schemas
- table creation [SQL Server], CREATE SCHEMA statement
- permissions [SQL Server], schemas
- schemas [SQL Server], creating
- CREATE SCHEMA statement
ms.assetid: df74fc36-20da-4efa-b412-c4e191786695
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 83699c45a36f353d20b69ba6b9a4101608bd2be8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092019"
---
# <a name="create-schema-transact-sql"></a>CREATE SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Создает схему в текущей базе данных. При помощи транзакции CREATE SCHEMA также можно создавать таблицы и представления в новой схеме и предоставлять разрешения GRANT, DENY или REVOKE на такие объекты.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE SCHEMA schema_name_clause [ <schema_element> [ ...n ] ]  
  
<schema_name_clause> ::=  
    {  
    schema_name  
    | AUTHORIZATION owner_name  
    | schema_name AUTHORIZATION owner_name  
    }  
  
<schema_element> ::=   
    {   
        table_definition | view_definition | grant_statement |   
        revoke_statement | deny_statement   
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE SCHEMA schema_name [ AUTHORIZATION owner_name ] [;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *schema_name*  
 Имя, по которому схема идентифицируется в данной базе данных.  
  
 AUTHORIZATION *owner_name*  
 Указывает имя участника уровня базы данных, который является владельцем схемы. Этому участнику могут принадлежать и другие схемы, при этом текущая схема может не использоваться по умолчанию.  
  
 *table_definition*  
 Указывает инструкцию CREATE TABLE, которая создает таблицу внутри схемы. У участника, выполняющего эту инструкцию, должно быть разрешение CREATE TABLE в текущей базе данных.  
  
 *view_definition*  
 Указывает инструкцию CREATE VIEW, которая создает представление внутри схемы. У участника, выполняющего эту инструкцию, должно быть разрешение CREATE VIEW в текущей базе данных.  
  
 *grant_statement*  
 Указывает инструкцию GRANT, которая предоставляет разрешения на любой защищаемый объект, за исключением новой схемы.  
  
 *revoke_statement*  
 Указывает инструкцию REVOKE, которая отменяет разрешения на любой защищаемый объект, за исключением новой схемы.  
  
 *deny_statement*  
 Указывает инструкцию DENY, которая запрещает разрешения на любой защищаемый объект, за исключением новой схемы.  
  
## <a name="remarks"></a>Remarks  
  
> [!NOTE]  
>  Выражения, которые содержат инструкцию CREATE SCHEMA AUTHORIZATION, но не определяют имя, разрешены только для обратной совместимости. Выражение не вызывает ошибку, но не создает схему.  
  
 Инструкция CREATE SCHEMA создает схему, содержащиеся в ней таблицы и представления, а также разрешения GRANT, REVOKE или DENY на любой защищаемый объект в одной инструкции. Эта инструкция должна выполняться как отдельный пакет. При помощи инструкции CREATE SCHEMA объекты создаются внутри создаваемой схемы.  
  
 Транзакции CREATE SCHEMA являются атомарными. Если в процессе выполнения инструкции CREATE SCHEMA возникают ошибки, ни один из указанных защищаемых объектов не создается и ни одно разрешение не предоставляется.  
  
 Защищаемые объекты, которые необходимо создать при помощи инструкции CREATE SCHEMA, могут быть перечислены в любом порядке, за исключением представлений, ссылающихся на другие представления. В этом случае ссылающееся представление должно быть создано после того представления, на которое оно ссылается.  
  
 Таким образом, при помощи инструкции GRANT можно предоставлять разрешения на объект еще до того, как он будет создан, а инструкция CREATE VIEW может появляться раньше инструкций CREATE TABLE, создающих таблицы, на которые ссылается представление. Кроме того, инструкции CREATE TABLE могут декларировать внешние ключи к таблицам, определенным позже в инструкции CREATE SCHEMA.  
  
> [!NOTE]  
>  В инструкциях CREATE SCHEMA поддерживаются DENY и REVOKE. Предложения DENY и REVOKE будут исполняться в той последовательности, в которой они появляются в инструкции CREATE SCHEMA.  
  
 Участник, выполняющий инструкцию CREATE SCHEMA, может указать другого участника базы данных в качестве владельца создаваемой схемы. Для этого необходимы дополнительные разрешения, описанные в подразделе "Разрешения" далее в этом разделе.  
  
 Новая схема принадлежит одному из следующих участников уровня базы данных: пользователю базы данных, роли базы данных или роли приложения. Объекты, создаваемые в схеме, принадлежат владельцу схемы и имеют значение NULL для **principal_id** в **sys.objects**. Владение объектами, содержащимися в схеме, можно передать любому участнику уровня базы данных, однако у владельца схемы всегда остается разрешение CONTROL на объекты в схеме.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
 **Неявное создание схемы и пользователя**  
  
 В некоторых случаях пользователь может использовать базу данных без учетной записи пользователя базы данных (субъект базы данных в базе данных). Это может случаться в следующих ситуациях:  
  
-   Имя входа имеет привилегии **CONTROL SERVER**.  
  
-   Пользователь Windows не имеет индивидуальной учетной записи пользователя базы данных (субъект базы данных в базе данных), однако получает доступ в базу данных как член группы Windows, которая имеет учетную запись пользователя базы данных (субъект базы данных для группы Windows).  
  
 Когда пользователь без учетной записи пользователя базы данных создает объект без определения существующей схемы, субъект базы данных и схема по умолчанию будут автоматически созданы в базе данных для этого пользователя. Созданные субъект и схема базы данных будут иметь то же имя, которым пользовался пользователь при подключении к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (имя входа в систему проверки подлинности или имя пользователя Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
 Данное поведение необходимо, чтобы позволить пользователям, базирующимся в группах Windows, создавать и обладать объектами. Однако это может привести к неумышленному созданию схем и пользователей. Во избежание неявного создания пользователей и схем каждый раз, при возможности, явно создавайте субъекты базы данных и назначайте схему по умолчанию. Или явно выражайте существующую схему при создании объектов в базе данных, используя двух- или трехчастные имена объектов.  

> [!NOTE]
>  Неявное создание пользователя Azure Active Directory невозможно в [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Так как при создании пользователя Azure AD из внешнего поставщика необходимо проверить состояние пользователей в AAD, создание пользователя завершится ошибкой 2760: **Указанное имя схемы «\<user_name@domain>» не существует, или у вас нет разрешения для его использования.** Затем выводится ошибка 2759: **Выполнение CREATE SCHEMA завершилось неудачно из-за предыдущих ошибок.** Чтобы устранить эти ошибки, сначала создайте пользователя Azure AD из внешнего поставщика, а затем повторно запустите инструкцию для создания объекта.
 
  
## <a name="deprecation-notice"></a>Уведомление об устаревании  
 Инструкции CREATE SCHEMA, не указывающие имя схемы, поддерживаются в данный момент только для обратной совместимости. Такие инструкции на самом деле не создают схему внутри базы данных, а создают таблицы и представления, а также предоставляют разрешения. Участникам не нужны разрешения CREATE SCHEMA для выполнения этой более ранней формы инструкции CREATE SCHEMA, поскольку схема не создается. Эта функция не будет включена в последующие версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения CREATE SCHEMA в базе данных.  
  
 Чтобы создать объект, указанный в инструкции CREATE SCHEMA, у пользователя должно быть соответствующее разрешение CREATE.  
  
 Чтобы назначить другого пользователя владельцем создаваемой схемы, у участника должно быть разрешение IMPERSONATE на этого пользователя. Если роль базы данных указана в качестве владельца, то вызывающий объект должен входить в роль или иметь на нее разрешение ALTER.  
  
> [!NOTE]  
>  Для обеспечения обратной совместимости синтаксиса разрешения на CREATE SCHEMA не проверяются, поскольку схема не создается.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-schema-and-granting-permissions"></a>A. Создание схемы и предоставление разрешений  
 В следующем примере создается схема `Sprockets`, принадлежащая `Annik`, которая содержит таблицу `NineProngs`. Инструкция предоставляет разрешение `SELECT` для `Mandar` и запрещает `SELECT` для `Prasanna`. Обратите внимание на то, что `Sprockets` и `NineProngs` создаются в одной инструкции.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SCHEMA Sprockets AUTHORIZATION Annik  
    CREATE TABLE NineProngs (source int, cost int, partnumber int)  
    GRANT SELECT ON SCHEMA::Sprockets TO Mandar  
    DENY SELECT ON SCHEMA::Sprockets TO Prasanna;  
GO   
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-creating-a-schema-and-a-table-in-the-schema"></a>Б. Создание схемы и таблицы в схеме  
 В следующем примере создается схема `Sales`, а затем таблица `Sales.Region`.  
  
```sql  
CREATE SCHEMA Sales;  
GO
  
CREATE TABLE Sales.Region   
(Region_id INT NOT NULL,  
Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
```  
  
### <a name="c-setting-the-owner-of-a-schema"></a>В. Задание владельца схемы  
 В следующем примере создается схема `Production`, принадлежащая `Mary`.  
  
```sql  
CREATE SCHEMA Production AUTHORIZATION [Contoso\Mary];  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER SCHEMA (Transact-SQL)](../../t-sql/statements/alter-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.schemas (Transact-SQL)](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [Создание схемы базы данных](../../relational-databases/security/authentication-access/create-a-database-schema.md)  
  
  


