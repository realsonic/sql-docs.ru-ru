---
description: Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server
title: Запросы к системному каталогу SQL Server вопросы и ответы | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], examples
- metadata [SQL Server], frequently asked questions
- metadata [SQL Server], example queries
- system catalogs [SQL Server], example queries
- catalog views [SQL Server], frequently asked questions
ms.assetid: ca202580-c37e-4ccd-9275-77ce79481f64
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a67ede3c053d3b9458e1b880a268e923226f68d1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093290"
---
# <a name="querying-the-sql-server-system-catalog-faq"></a>Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Данный раздел содержит список часто задаваемых вопросов. Ответы на эти вопросы — запросы, основанные на представлениях каталога.  
  
##  <a name="frequently-asked-questions"></a><a name="_TOP"></a> Часто задаваемые вопросы  
 Следующие разделы содержат часто задаваемые вопросы по категориям.  
  
### <a name="data-types"></a>Типы данных  
  
-   [Как определить типы данных столбцов указанной таблицы?](#_FAQ7)  
  
-   [Как определить типы данных LOB указанной таблицы?](#_FAQ14)  
  
-   [Как определить столбцы, зависящие от указанного типа данных?](#_FAQ22)  
  
-   [Как определить вычисляемые столбцы, зависящие от указанного, определяемого пользователем типа данных CLR или псевдонима типа данных?](#_FAQ23)  
  
-   [Как определить параметры, зависящие от указанного, определяемого пользователем типа данных CLR или псевдонима типа данных?](#_FAQ24)  
  
-   [Как определить ограничения CHECK, зависящие от указанного, определяемого пользователем типа данных CLR?](#_FAQ25)  
  
-   [Как определить представления, функции Transact-SQL и хранимые процедуры Transact-SQL, зависящие от указанного, определяемого пользователем типа данных CLR или псевдонима типа данных?](#_FAQ26)  
  
### <a name="tables-indexes-views-and-constraints"></a>Таблицы, индексы, представления и ограничения  
  
-   [Как найти все определяемые пользователем таблицы в базе данных?](#_FAQ31)  
  
-   [Как найти все таблицы, по которым нет кластеризованного индекса в указанной базе данных?](#_FAQ1)  
  
-   [Как найти все таблицы, у которых нет никакого индекса?](#_FAQ4)  
  
-   [Как найти все таблицы, у которых нет первичного ключа?](#_FAQ3)  
  
-   [Как найти все таблицы, у которых нет никакого столбца идентификаторов?](#_FAQ5)  
  
-   [Как найти все таблицы и индексы, которые секционированы?](#_FAQ32)  
  
-   [Как найти все представления в базе данных?](#_FAQ13)  
  
-   [Инструкции по просмотру определения представления.](#_FAQ35)  
  
-   [Как найти все сущности, измененные за последние N дней?](#_FAQ6)  
  
-   [Как найти столбцы первичного ключа указанной таблицы?](#_FAQ16)  
  
-   [Как найти столбцы внешнего ключа указанной таблицы?](#_FAQ17)  
  
-   [Как определить, что столбец используется в выражении вычисляемого столбца?](#_FAQ20)  
  
-   [Как определить все столбцы, используемые в выражении вычисляемого столбца?](#_FAQ21)  
  
-   [Как найти все ограничения определенной таблицы?](#_FAQ27)  
  
-   [Как найти все индексы определенной таблицы?](#_FAQ28)  
  
-   [Как найти все таблицы с определенным названием столбца?](#_FAQ30)  
  
-   [Как найти все статистические данные определенного объекта?](#_FAQ33)  
  
-   [Как найти все статистические данные и статистические столбцы определенного объекта?](#_FAQ34)  
  
### <a name="modules-stored-procedures-user-defined-functions-and-triggers"></a>Модули (хранимые процедуры, определяемые пользователем функции и триггеры)  
  
-   [Как найти все хранимые процедуры в базе данных?](#_FAQ9)  
  
-   [Как найти все определяемые пользователем функции в базе данных?](#_FAQ12)  
  
-   [Как определить параметры указанной хранимой процедуры или функции?](#_FAQ10)  
  
-   [Как найти зависимости указанной функции?](#_FAQ8)  
  
-   [Как просмотреть определение модуля?](#_FAQ15)  
  
-   [Как просмотреть определение триггера уровня сервера?](#_FAQ19)  
  
### <a name="schemas-users-roles-and-permissions"></a>Схемы, пользователи, роли и разрешения  
  
-   [Как найти всех владельцев сущностей, принадлежащих определенной схеме?](#_FAQ2)  
  
-   [Как определить разрешения, предоставленные или запрещенные для указанного участника?](#_FAQ18)  
  
## <a name="answers"></a>Ответы  
  
###  <a name="how-do-i-find-all-the-tables-that-do-not-have-a-clustered-index-in-a-specified-database"></a><a name="_FAQ1"></a> Разделы справки найти все таблицы, в которых отсутствует кластеризованный индекс в указанной базе данных?  
 Перед запуском следующих запросов замените `<database_name>` действительным именем базы данных.  
  
```  
SELECT SCHEMA_NAME(t.schema_id) AS schema_name, t.name AS table_name  
FROM sys.tables AS t  
WHERE NOT EXISTS   
   (  
     SELECT * FROM sys.indexes AS i  
     WHERE i.object_id = t.object_id  
     AND i.type = 1  -- or type_desc = 'CLUSTERED'  
   )  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 Кроме того, можно использовать функцию `OBJECTPROPERTY`, как показано в следующем примере.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name, name AS table_name  
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasClustIndex') = 0  
ORDER BY schema_id, name;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-owners-of-entities-contained-in-a-specified-schema"></a><a name="_FAQ2"></a> Разделы справки найти всех владельцев сущностей, содержащихся в указанной схеме?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT 'OBJECT' AS entity_type  
    ,USER_NAME(OBJECTPROPERTY(object_id, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.objects WHERE SCHEMA_NAME(schema_id) = '<schema_name>'  
UNION   
SELECT 'TYPE' AS entity_type  
    ,USER_NAME(TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.types WHERE SCHEMA_NAME(schema_id) = '<schema_name>'   
UNION  
SELECT 'XML SCHEMA COLLECTION' AS entity_type   
    ,COALESCE(USER_NAME(xsc.principal_id),USER_NAME(s.principal_id)) AS owner_name  
    ,xsc.name   
FROM sys.xml_schema_collections AS xsc JOIN sys.schemas AS s  
    ON s.schema_id = xsc.schema_id  
WHERE s.name = '<schema_name>';  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-that-do-not-have-a-primary-key"></a><a name="_FAQ3"></a> Разделы справки найти все таблицы, у которых нет первичного ключа?  
 Перед запуском следующих запросов замените `<database_name>` действительным именем базы данных.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(t.schema_id) AS schema_name  
    ,t.name AS table_name  
FROM sys.tables t   
WHERE object_id NOT IN   
   (  
    SELECT parent_object_id   
    FROM sys.key_constraints   
    WHERE type_desc = 'PRIMARY_KEY_CONSTRAINT' -- or type = 'PK'  
    );  
GO  
  
```  
  
 Кроме того, можно выполнить следующий запрос.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-that-do-not-have-an-index"></a><a name="_FAQ4"></a> Разделы справки найти все таблицы, у которых нет индекса?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем базы данных.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name  
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'IsIndexed') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-that-have-an-identity-column"></a><a name="_FAQ5"></a> Разделы справки найти все таблицы, имеющие столбец идентификаторов?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем базы данных.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    , t.name AS table_name  
    , c.name AS column_name  
FROM sys.tables AS t  
JOIN sys.identity_columns c ON t.object_id = c.object_id  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 Кроме того, можно выполнить следующий запрос.  
  
> [!NOTE]  
>  Этот запрос не возвращает имена столбцов.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasIdentity') = 1  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-data-types-of-the-columns-of-a-specified-table"></a><a name="_FAQ7"></a> Разделы справки найти типы данных столбцов указанной таблицы?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.table_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT c.name AS column_name  
    ,c.column_id  
    ,SCHEMA_NAME(t.schema_id) AS type_schema  
    ,t.name AS type_name  
    ,t.is_user_defined  
    ,t.is_assembly_type  
    ,c.max_length  
    ,c.precision  
    ,c.scale  
FROM sys.columns AS c   
JOIN sys.types AS t ON c.user_type_id=t.user_type_id  
WHERE c.object_id = OBJECT_ID('<schema_name.table_name>')  
ORDER BY c.column_id;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-dependencies-on-a-specified-function"></a><a name="_FAQ8"></a> Разделы справки найти зависимости от указанной функции?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.function_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS referencing_object_name  
    ,COALESCE(COL_NAME(object_id, column_id), '(n/a)') AS referencing_column_name  
    ,*  
FROM sys.sql_dependencies  
WHERE referenced_major_id = OBJECT_ID('<schema_name.function_name>')  
ORDER BY OBJECT_NAME(object_id), COL_NAME(object_id, column_id);  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-stored-procedures-in-a-database"></a><a name="_FAQ9"></a> Разделы справки найти все хранимые процедуры в базе данных?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем.  
  
```  
  
USE <database_name>;  
GO  
SELECT name AS procedure_name   
    ,SCHEMA_NAME(schema_id) AS schema_name  
    ,type_desc  
    ,create_date  
    ,modify_date  
FROM sys.procedures;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-parameters-for-a-specified-stored-procedure-or-function"></a><a name="_FAQ10"></a> Разделы справки найти параметры для указанной хранимой процедуры или функции?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.object_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,o.name AS object_name  
    ,o.type_desc  
    ,p.parameter_id  
    ,p.name AS parameter_name  
    ,TYPE_NAME(p.user_type_id) AS parameter_type  
    ,p.max_length  
    ,p.precision  
    ,p.scale  
    ,p.is_output  
FROM sys.objects AS o  
INNER JOIN sys.parameters AS p ON o.object_id = p.object_id  
WHERE o.object_id = OBJECT_ID('<schema_name.object_name>')  
ORDER BY schema_name, object_name, p.parameter_id;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-user-defined-functions-in-a-database"></a><a name="_FAQ12"></a> Разделы справки найти все определяемые пользователем функции в базе данных?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем базы данных.  
  
```  
USE <database_name>;  
GO  
SELECT name AS function_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE type_desc LIKE '%FUNCTION%';  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-views-in-a-database"></a><a name="_FAQ13"></a> Разделы справки найти все представления в базе данных?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем базы данных.  
  
```  
USE <database_name>;  
GO  
SELECT name AS view_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,OBJECTPROPERTYEX(object_id,'IsIndexed') AS IsIndexed  
  ,OBJECTPROPERTYEX(object_id,'IsIndexable') AS IsIndexable  
  ,create_date  
  ,modify_date  
FROM sys.views;  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-entities-that-have-been-modified-in-the-last-n-days"></a><a name="_FAQ6"></a> Разделы справки найти все объекты, которые были изменены за последние N дней?  
 Перед запуском следующего запроса замените `<database_name>` и `<n_days>` действительными значениями.  
  
```  
USE <database_name>;  
GO  
SELECT name AS object_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE modify_date > GETDATE() - <n_days>  
ORDER BY modify_date;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-lob-data-types-of-a-specified-table"></a><a name="_FAQ14"></a> Разделы справки найти типы данных LOB указанной таблицы?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.table_name>` действительными именами.  
  
```  
  
USE <database_name>;  
GO  
SELECT name AS column_name   
    ,column_id   
    ,TYPE_NAME(user_type_id) AS type_name  
    ,max_length  
    ,CASE   
       WHEN max_length = -1 AND TYPE_NAME(user_type_id) <> 'xml'  
            THEN 1  
            ELSE 0  
     END AS [(max)]  
FROM sys.columns  
WHERE object_id=OBJECT_ID('<schema_name.table_name>')   
    AND ( TYPE_NAME(user_type_id) IN ('xml','text', 'ntext','image')  
         OR (TYPE_NAME(user_type_id) IN ('varchar','nvarchar','varbinary')  
         AND max_length = -1)  
        );  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-view-the-definition-of-a-module"></a><a name="_FAQ15"></a> Разделы справки Просмотреть определение модуля?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.object_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT definition  
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 Кроме того, можно использовать функцию `OBJECT_DEFINITION`, как показано в следующем примере.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID('<schema_name.object_name>')) AS ObjectDefinition;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-view-the-definition-of-a-server-level-trigger"></a><a name="_FAQ19"></a> Разделы справки Просмотреть определение триггера уровня сервера?  
  
```  
SELECT definition  
FROM sys.server_sql_modules;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-columns-of-a-primary-key-for-a-specified-table"></a><a name="_FAQ16"></a> Разделы справки найти столбцы первичного ключа для указанной таблицы?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.table_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,ic.index_column_id  
    ,key_ordinal  
    ,c.name AS column_name  
    ,TYPE_NAME(c.user_type_id)AS column_type   
    ,is_identity  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic   
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
INNER JOIN sys.columns AS c   
    ON ic.object_id = c.object_id AND c.column_id = ic.column_id  
WHERE i.is_primary_key = 1   
    AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 Кроме того, можно использовать функцию `COL_NAME`, как показано в следующем примере.  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,COL_NAME(ic.object_id,ic.column_id) AS column_name  
    ,ic.index_column_id  
    ,key_ordinal  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic   
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.is_primary_key = 1   
    AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-columns-of-a-foreign-key-for-a-specified-table"></a><a name="_FAQ17"></a> Разделы справки найти столбцы внешнего ключа для указанной таблицы?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.table_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT   
    f.name AS foreign_key_name  
   ,OBJECT_NAME(f.parent_object_id) AS table_name  
   ,COL_NAME(fc.parent_object_id, fc.parent_column_id) AS constraint_column_name  
   ,OBJECT_NAME (f.referenced_object_id) AS referenced_object  
   ,COL_NAME(fc.referenced_object_id, fc.referenced_column_id) AS referenced_column_name  
   ,is_disabled  
   ,delete_referential_action_desc  
   ,update_referential_action_desc  
FROM sys.foreign_keys AS f  
INNER JOIN sys.foreign_key_columns AS fc   
   ON f.object_id = fc.constraint_object_id   
WHERE f.parent_object_id = OBJECT_ID('<schema_name.table_name>');  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-permissions-granted-or-denied-to-a-specified-principal"></a><a name="_FAQ18"></a> Разделы справки найти разрешения, предоставленные или запрещенные для указанного участника?  
 В следующем примере создается функция, возвращающая имя сущности, для которой проверяются разрешения. Эта функция вызывается в следующих ниже запросах. Эту функцию нужно создать в каждой базе данных, в которой проверяются разрешения.  
  
```  
-- Create a function to return the name of the entity on which the permissions are checked.  
IF OBJECT_ID (N'dbo.entity_instance_name', N'FN') IS NOT NULL  
    DROP FUNCTION dbo.entity_instance_name;  
GO  
CREATE FUNCTION dbo.entity_instance_name(@class_desc nvarchar(60), @major_id int)   
RETURNS sysname AS  
BEGIN  
    DECLARE @the_entity_name sysname  
    SELECT @the_entity_name = CASE  
        WHEN @class_desc = 'DATABASE' THEN DB_NAME()  
        WHEN @class_desc = 'SCHEMA' THEN SCHEMA_NAME(@major_id)  
        WHEN @class_desc = 'OBJECT_OR_COLUMN' THEN OBJECT_NAME(@major_id)  
        WHEN @class_desc = 'DATABASE_PRINCIPAL' THEN USER_NAME(@major_id)  
        WHEN @class_desc = 'ASSEMBLY' THEN   
            (SELECT name FROM sys.assemblies WHERE assembly_id=@major_id)  
        WHEN @class_desc = 'TYPE' THEN TYPE_NAME(@major_id)  
        WHEN @class_desc = 'XML_SCHEMA_COLLECTION' THEN   
            (SELECT name FROM sys.xml_schema_collections  
              WHERE xml_collection_id=@major_id)  
        WHEN @class_desc = 'MESSAGE_TYPE' THEN   
            (SELECT name FROM sys.service_message_types WHERE message_type_id=@major_id)  
        WHEN @class_desc = 'SERVICE_CONTRACT' THEN   
           (SELECT name FROM sys.service_contracts  
              WHERE service_contract_id=@major_id)  
        WHEN @class_desc = 'SERVICE' THEN  
          (SELECT name FROM sys.services WHERE service_id=@major_id)  
        WHEN @class_desc = 'REMOTE_SERVICE_BINDING' THEN  
          (SELECT name FROM sys.remote_service_bindings  
             WHERE remote_service_binding_id=@major_id)  
        WHEN @class_desc = 'ROUTE' THEN  
          (SELECT name FROM sys.routes WHERE route_id=@major_id)  
        WHEN @class_desc = 'FULLTEXT_CATALOG' THEN  
          (SELECT name FROM sys.fulltext_catalogs WHERE fulltext_catalog_id=@major_id)  
        WHEN @class_desc = 'SYMMETRIC_KEY' THEN  
          (SELECT name FROM sys.symmetric_keys WHERE symmetric_key_id=@major_id)  
        WHEN @class_desc = 'CERTIFICATE' THEN  
          (SELECT name FROM sys.certificates WHERE certificate_id=@major_id)  
        WHEN @class_desc = 'ASYMMETRIC_KEY' THEN  
          (SELECT name FROM sys.asymmetric_keys WHERE asymmetric_key_id=@major_id)  
        WHEN @class_desc = 'SERVER' THEN   
             (SELECT name FROM sys.servers WHERE server_id=@major_id)  
        WHEN @class_desc = 'SERVER_PRINCIPAL' THEN SUSER_NAME(@major_id)  
        WHEN @class_desc = 'ENDPOINT' THEN   
             (SELECT name FROM sys.endpoints WHERE endpoint_id=@major_id)        
        ELSE '?'  
    END  
    RETURN @the_entity_name  
END;  
GO  
-- Return server-level permissions for the user.  
SELECT class  
    ,class_desc  
    ,dbo.entity_instance_name(class_desc, major_id) AS entity_name   
    ,minor_id  
    ,SUSER_NAME(grantee_principal_id) AS grantee  
    ,SUSER_NAME(grantor_principal_id) AS grantor  
    ,type  
    ,permission_name  
    ,state_desc   
FROM sys.server_permissions   
WHERE grantee_principal_id = SUSER_ID('public');  
GO  
-- Return database-level permissions for the user.  
SELECT class  
    ,class_desc  
    ,dbo.entity_instance_name(class_desc , major_id) AS entity_name   
    ,minor_id  
    ,USER_NAME(grantee_principal_id) AS grantee  
    ,USER_NAME(grantor_principal_id) AS grantor  
    ,type  
    ,permission_name  
    ,state_desc     
FROM  sys.database_permissions   
WHERE grantee_principal_id = DATABASE_PRINCIPAL_ID('public');  
GO  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-determine-if-a-column-is-used-in-a-computed-column-expression"></a><a name="_FAQ20"></a> Разделы справки определить, используется ли столбец в выражении вычисляемого столбца?  
 Перед выполнением следующего запроса замените `<database_name>` , `<schema_name.table_name>` и `<column_name`> допустимыми именами.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,COL_NAME(object_id, column_id) AS computed_column   
    ,class_desc  
    ,is_selected  
    ,is_updated  
    ,is_select_all  
FROM sys.sql_dependencies  
WHERE referenced_major_id = OBJECT_ID('<schema_name.table_name>')  
    AND referenced_minor_id = COLUMNPROPERTY(referenced_major_id, '<column_name>', 'ColumnId')  
    AND class = 1;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-columns-that-are-used-in-a-computed-column-expression"></a><a name="_FAQ21"></a> Разделы справки найти все столбцы, используемые в выражении вычисляемого столбца?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(d.referenced_major_id) AS object_name  
    ,COL_NAME(d.referenced_major_id, d.referenced_minor_id) AS column_name  
    ,OBJECT_NAME(referenced_major_id) AS dependent_object_name   
    ,COL_NAME(d.object_id, d.column_id) AS dependent_computed_column  
    ,cc.definition AS computed_column_definition  
FROM sys.sql_dependencies AS d  
JOIN sys.computed_columns AS cc   
    ON cc.object_id = d.object_id AND cc.column_id = d.column_id AND d.object_id=d.referenced_major_id       
WHERE d.class = 1  
ORDER BY object_name, column_name;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-columns-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ22"></a> Разделы справки найти столбцы, зависящие от указанного определяемого пользователем типа или псевдонима среды CLR?  
 Перед выполнением следующего запроса замените на `<database_name>` допустимое имя, а — на `<schema_name.data_type_name>` допустимый, определяемый пользователем тип данных CLR с указанием схемы или на имя типа псевдонима с указанием схемы. Следующий запрос требует членства в роли **db_owner** или разрешений для просмотра всех зависимых столбцов и метаданных вычисляемых столбцов в базе данных.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name   
    ,c.name AS column_name   
    ,SCHEMA_NAME(t.schema_id) AS schema_name  
    ,TYPE_NAME(c.user_type_id) AS user_type_name  
    ,c.max_length  
    ,c.precision  
    ,c.scale  
    ,c.is_nullable  
    ,c.is_computed  
FROM sys.columns AS c  
INNER JOIN sys.types AS t ON c.user_type_id = t.user_type_id  
WHERE c.user_type_id = TYPE_ID('<schema_name.data_type_name>');   
GO  
  
```  
  
 Следующий запрос возвращает ограниченное и узкие представления столбцов, зависящих от определяемого пользователем типа или псевдонима среды CLR, но результирующий набор является видимым для роли **Public** . Этот запрос следует использовать в том случае, если предоставлены разрешения REFERENCE определяемого пользователем типа данных для других пользователей и отсутствует разрешение на просмотр метаданных объектов, созданных другими пользователями при использовании этого типа.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name   
    ,COL_NAME(object_id, column_id) AS column_name  
    ,TYPE_NAME(user_type_id) AS user_type  
FROM sys.column_type_usages  
WHERE user_type_id = TYPE_ID('<schema_name.data_type_name>');  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-computed-columns-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ23"></a> Разделы справки найти вычисленные столбцы, зависящие от указанного определяемого пользователем типа или псевдонима среды CLR?  
 Перед выполнением следующего запроса замените `<database_name>` на допустимое имя, а `<schema_name.data_type_name>` — на допустимый, определяемый пользователем тип данных CLR с указанием схемы, имя псевдонима типа.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,COL_NAME(object_id, column_id) AS column_name  
FROM sys.sql_dependencies  
WHERE referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
    AND class = 2 -- schema-bound references to type  
    AND OBJECTPROPERTY(object_id, 'IsTable') = 1;   -- exclude non-table dependencies  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-parameters-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ24"></a> Разделы справки найти параметры, зависящие от указанного определяемого пользователем типа или псевдонима среды CLR?  
 Перед выполнением следующего запроса замените `<database_name>` на допустимое имя, а `<schema_name.data_type_name>` — на допустимый, определяемый пользователем тип данных CLR с указанием схемы, имя псевдонима типа. Следующий запрос требует членства в роли **db_owner** или разрешений для просмотра всех зависимых столбцов и метаданных вычисляемых столбцов в базе данных.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,NULL AS procedure_number  
    ,name AS param_name  
    ,parameter_id AS param_num  
    ,TYPE_NAME(p.user_TYPE_ID) AS type_name  
FROM sys.parameters AS p  
WHERE p.user_TYPE_ID = TYPE_ID('<schema_name.data_type_name>')  
UNION   
SELECT OBJECT_NAME(object_id) AS object_name  
    ,procedure_number  
    ,name AS param_name  
    ,parameter_id AS param_num  
    ,TYPE_NAME(p.user_TYPE_ID) AS type_name  
FROM sys.numbered_procedure_parameters AS p  
WHERE p.user_TYPE_ID = TYPE_ID('<schema_name.data_type_name>')  
ORDER BY object_name, procedure_number, param_num;  
GO  
  
```  
  
 Следующий запрос возвращает ограниченное и узкие представления параметров, которые зависят от определяемого пользователем типа или псевдонима среды CLR, но результирующий набор видим для роли **Public** . Этот запрос следует использовать в том случае, если предоставлены разрешения REFERENCE определяемого пользователем типа данных для других пользователей и отсутствует разрешение на просмотр метаданных объектов, созданных другими пользователями при использовании этого типа.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,parameter_id  
    ,TYPE_NAME(user_type_id) AS type_name  
FROM sys.parameter_type_usages   
WHERE user_type_id = TYPE_ID('<schema_name.data_type_name>');  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-check-constraints-that-depend-on-a-specified-clr-user-defined-type"></a><a name="_FAQ25"></a> Разделы справки найти ПРОВЕРОЧные ограничения, зависящие от указанного определяемого пользователем типа данных CLR?  
 Перед выполнением следующего запроса замените на `<database_name>` допустимое имя и на `<schema_name.data_type_name>` допустимое имя определяемого пользователем типа данных CLR с указанием схемы.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name  
    ,OBJECT_NAME(o.parent_object_id) AS table_name  
    ,OBJECT_NAME(o.object_id) AS constraint_name  
FROM sys.sql_dependencies AS d  
JOIN sys.objects AS o ON o.object_id = d.object_id  
WHERE referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
    AND class = 2 -- schema-bound references to type  
    AND OBJECTPROPERTY(o.object_id, 'IsCheckCnst') = 1; -- exclude non-CHECK dependencies  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-views-transact-sql-functions-and-transact-sql-stored-procedures-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ26"></a> Разделы справки найти представления, функции Transact-SQL и хранимые процедуры Transact-SQL, зависящие от указанного определяемого пользователем типа или псевдонима среды CLR?  
 Перед выполнением следующего запроса замените `<database_name>` на допустимое имя, а `<schema_name.data_type_name>` — на допустимый, определяемый пользователем тип данных CLR с указанием схемы, имя псевдонима типа.  
  
 Определяемые в функции или процедуре параметры неявно привязаны к схемам. Поэтому параметры, зависящие от определяемого пользователем типа или псевдонима среды CLR, можно просмотреть с помощью представления каталога [sys.sql_dependencies](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md) . Процедуры и триггеры не привязаны к схеме. Это означает, что зависимости между любыми выражениями, определенными в теле процедуры или триггера или определяемом пользователем типе данных CLR или псевдониме типа, не обслуживаются. Привязанные к схеме представления и определяемые пользователем функции, имеющие выражения, зависящие от определяемого пользователем типа или псевдонима среды CLR, поддерживаются в представлении каталога **sys.sql_dependencies** . Зависимости между типами и функциями среды CLR и процедурами среды CLR не обслуживаются.  
  
 Следующий запрос возвращает все зависящие от схемы зависимости в представлениях, функциях [!INCLUDE[tsql](../../includes/tsql-md.md)] и хранимых процедурах [!INCLUDE[tsql](../../includes/tsql-md.md)] для указанного, определяемого пользователем типа данных CLR и псевдонима типа.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS dependent_object_schema  
  ,OBJECT_NAME(o.object_id) AS dependent_object_name  
  ,o.type_desc AS dependent_object_type  
  ,d.class_desc AS kind_of_dependency  
  ,TYPE_NAME (d.referenced_major_id) AS type_name  
FROM sys.sql_dependencies AS d   
JOIN sys.objects AS o  
  ON d.object_id = o.object_id  
  AND o.type IN ('FN','IF','TF', 'V', 'P')  
WHERE d.class = 2 -- dependencies on types  
  AND d.referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
ORDER BY dependent_object_schema, dependent_object_name;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-constraints-for-a-specified-table"></a><a name="_FAQ27"></a> Разделы справки найти все ограничения для указанной таблицы?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.table_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) as constraint_name  
    ,SCHEMA_NAME(schema_id) AS schema_name  
    ,OBJECT_NAME(parent_object_id) AS table_name  
    ,type_desc  
    ,create_date  
    ,modify_date  
    ,is_ms_shipped  
    ,is_published  
    ,is_schema_published  
FROM sys.objects  
WHERE type_desc LIKE '%CONSTRAINT'   
    AND parent_object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-indexes-for-a-specified-table"></a><a name="_FAQ28"></a> Разделы справки найти все индексы для указанной таблицы?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.table_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,i.type_desc  
    ,is_unique  
    ,ds.type_desc AS filegroup_or_partition_scheme  
    ,ds.name AS filegroup_or_partition_scheme_name  
    ,ignore_dup_key  
    ,is_primary_key  
    ,is_unique_constraint  
    ,fill_factor  
    ,is_padded  
    ,is_disabled  
    ,allow_row_locks  
    ,allow_page_locks  
FROM sys.indexes AS i  
INNER JOIN sys.data_spaces AS ds ON i.data_space_id = ds.data_space_id  
WHERE is_hypothetical = 0 AND i.index_id <> 0   
AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-objects-that-have-a-specified-column-name"></a><a name="_FAQ30"></a> Разделы справки найти все объекты, у которых есть указанное имя столбца?  
 Перед запуском следующего запроса замените `<database_name>` и `<column_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id)  
FROM sys.columns  
WHERE name = '<column_name>';  
GO  
  
```  
  
 либо  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name   
    ,o.name AS object_name  
    ,type_desc  
FROM sys.objects AS o  
INNER JOIN sys.columns AS c ON o.object_id = c.object_id  
WHERE c.name = '<column_name>';  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-user-defined-tables-in-a-specified-database"></a><a name="_FAQ31"></a> Разделы справки найти все определяемые пользователем таблицы в указанной базе данных?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем.  
  
```  
USE <database_name>;  
GO  
SELECT *   
FROM sys.tables;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-and-indexes-that-are-partitioned"></a><a name="_FAQ32"></a> Разделы справки найти все таблицы и индексы, которые секционированы?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name  
    ,OBJECT_NAME(p.object_id) AS table_name  
    ,i.name AS index_name  
    ,p.partition_number  
    ,rows   
FROM sys.partitions AS p  
INNER JOIN sys.indexes AS i ON p.object_id = i.object_id AND p.index_id = i.index_id  
INNER JOIN sys.partition_schemes ps ON i.data_space_id=ps.data_space_id  
INNER JOIN sys.objects AS o ON o.object_id = i.object_id  
ORDER BY index_name, partition_number;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-statistics-on-a-specified-object"></a><a name="_FAQ33"></a> Разделы справки найти всю статистику по указанному объекту?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем и `<schema_name.object_name>` действительным именем таблицы, индексированного представления или возвращающей табличное значение функции.  
  
```  
USE <database_name>;  
GO  
SELECT name AS statistics_name  
    ,stats_id  
    ,auto_created  
    ,user_created  
    ,no_recompute  
FROM sys.stats  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-all-the-statistics-and-statistics-columns-on-a-specified-object"></a><a name="_FAQ34"></a> Разделы справки найти все столбцы статистики и статистики для указанного объекта?  
 Перед запуском следующего запроса замените `<database_name>` действительным именем и `<schema_name.object_name>` действительным именем таблицы, индексированного представления или возвращающей табличное значение функции.  
  
```  
USE <database_name>;  
GO  
SELECT s.name AS statistics_name  
    ,c.name AS column_name  
    ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
###  <a name="how-do-i-find-the-definition-of-a-view"></a><a name="_FAQ35"></a> Разделы справки найти определение представления?  
 Перед запуском следующего запроса замените `<database_name>` и `<schema_name.object_name>` действительными именами.  
  
```  
USE <database_name>;  
GO  
SELECT definition  
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 Кроме того, можно использовать функцию `OBJECT_DEFINITION`, как показано в следующем примере.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID('<schema_name.object_name>')) AS ObjectDefinition;  
GO  
  
```  
  
 [Вверх](#_TOP)  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
