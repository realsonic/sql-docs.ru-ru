---
description: Функция динамического управления sys.dm_sql_referenced_entities (Transact-SQL)
title: sys.dm_sql_referenced_entities (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_sql_referenced_entities_TSQL
- dm_sql_referenced_entities
- sys.dm_sql_referenced_entities
- sys.dm_sql_referenced_entities_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_sql_referenced_entities dynamic management function
ms.assetid: 077111cb-b860-4d61-916f-bac5d532912f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a5b90048297d0573749f4751a3730beb87d56ddb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094014"
---
# <a name="sysdm_sql_referenced_entities-transact-sql"></a>Функция динамического управления sys.dm_sql_referenced_entities (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает по одной строке для каждой определяемой пользователем сущности, на которую ссылается имя в определении указанной ссылающейся сущности в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Зависимость между двумя сущностями создается, когда одна определяемая пользователем сущность, называемая *упоминаемой сущностью*, отображается по имени в сохраненном выражении SQL другой определяемой пользователем сущности, называемой *ссылающейся сущностью*. Например, если в качестве ссылающейся сущности выступает хранимая процедура, то данная функция выводит список всех определяемых пользователем сущностей, упоминаемых в данной хранимой процедуре. К упоминаемым определяемым пользователем сущностям относятся: таблицы, представления, определяемые пользователем типы (UDT), а также другие хранимые процедуры.  
  
 Данная функция динамического управления может быть использована для отображения списка сущностей, упоминаемых заданной ссылающейся сущностью и имеющих следующие типы:  
  
-   сущности, привязанные к схеме;  
  
-   несвязанные сущности;  
  
-   межбазовые и межсерверные сущности;  
  
-   зависимости уровня столбца, как связанные, так и не связанные со схемами;  
  
-   определяемые пользователем типы (псевдонимы и типы CLR);  
  
-   коллекции XML-схем  
  
-   функции секционирования  

## <a name="syntax"></a>Синтаксис  
  
```  
sys.dm_sql_referenced_entities (  
    ' [ schema_name. ] referencing_entity_name ' ,
    ' <referencing_class> ' )  
  
<referencing_class> ::=  
{  
    OBJECT  
  | DATABASE_DDL_TRIGGER  
  | SERVER_DDL_TRIGGER  
}  
```  
  
## <a name="arguments"></a>Аргументы  
 [ *schema_name*. ] *referencing_entity_name*  
 Имя ссылающейся сущности. *schema_name* является обязательным, если ссылающийся класс является объектом.  
  
 *schema_name. referencing_entity_name* имеет тип **nvarchar (517)**.  
  
 *<referencing_class>* :: = {Object | DATABASE_DDL_TRIGGER | SERVER_DDL_TRIGGER}  
 Класс заданной ссылающейся сущности. В одной инструкции может быть указан только один класс.  
  
 *<referencing_class>* имеет тип **nvarchar (60)**.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|referencing_minor_id|**int**|Идентификатор столбца, если ссылающаяся сущность является столбцом; в противном случае — 0. Не допускает значение NULL.|  
|referenced_server_name|**sysname**|Имя сервера упоминаемой сущности.<br /><br /> Этот столбец заполняется для межсерверных зависимостей, которые создаются путем задания допустимого четырехкомпонентного имени. Сведения о составных именах см. в разделе [соглашения о синтаксисе Transact-sql &#40;&#41;Transact-SQL ](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).<br /><br /> Имеет значение NULL для не привязанных к схеме зависимостей, в которых сущность упоминается без указания четырехкомпонентного имени.<br /><br /> Значение NULL для сущностей, привязанных к схеме, так как они должны находиться в одной базе данных и поэтому могут быть определены только с помощью имени из двух частей (*Schema. Object*).|  
|referenced_database_name|**sysname**|Имя базы данных упоминаемой сущности.<br /><br /> Этот столбец заполняется для межбазовых и межсерверных ссылок, которые задаются путем указания допустимого трехкомпонентного или четырехкомпонентного имени.<br /><br /> Имеет значение NULL для не привязанных к схеме ссылок, задаваемых с помощью однокомпонентного или двухкомпонентного имени.<br /><br /> Значение NULL для сущностей, привязанных к схеме, так как они должны находиться в одной базе данных и поэтому могут быть определены только с помощью имени из двух частей (*Schema. Object*).|  
|referenced_schema_name|**sysname**|Схема, которой принадлежит упоминаемая сущность.<br /><br /> Имеет значение NULL для не привязанных к схеме ссылок, в которых сущность упоминается без указания имени схемы.<br /><br /> Никогда не принимает значение NULL для привязанных к схеме ссылок.|  
|referenced_entity_name|**sysname**|Имя упоминаемой сущности. Не допускает значение NULL.|  
|referenced_minor_name|**sysname**|Имя столбца, если упоминаемая сущность является столбцом. В противном случае — значение NULL. Например, параметр referenced_minor_name будет иметь значение NULL в строке, которая содержит саму упоминаемую сущность.<br /><br /> Упоминаемая сущность представляет собой столбец, если в ссылающейся сущности столбец определяется по имени или если в инструкции SELECT * используется родительская сущность.|  
|referenced_id|**int**|Идентификатор упоминаемой сущности. Если значение параметра referenced_minor_id отлично от 0, то величина referenced_id представляет сущность, определяющую столбец.<br /><br /> Для межсерверных ссылок всегда принимает значение NULL.<br /><br /> Принимает значение NULL для межбазовых ссылок в тех случаях, когда не удается определить идентификатор базы данных вне сети или неограниченности сущности.<br /><br /> Имеет значение NULL для ссылок в пределах базы данных, когда не удается определить идентификатор. Для ссылок, не привязанных к схеме, идентификатор не может быть разрешен, если упоминаемая сущность не существует в базе данных или если разрешение имен зависит от вызывающего объекта.  В последнем случае is_caller_dependent имеет значение 1.<br /><br /> Никогда не принимает значение NULL для привязанных к схеме ссылок.|  
|referenced_minor_id|**int**|Идентификатор столбца, если упоминаемая сущность является столбцом. В противном случае — 0. Например, параметр referenced_minor_id будет иметь значение 0 в строке, которая содержит саму упоминаемую сущность.<br /><br /> При наличии не привязанных к схеме ссылок список зависимостей уровня столбцов может быть выведен только в случае ограниченности всех упоминаемых сущностей. Если какая-либо из упоминаемых сущностей не может быть привязана, в списке не отображается ни одной зависимости уровня столбца, а параметру referenced_minor_id присваивается значение 0. См. пример Г.|  
|referenced_class|**tinyint**|Класс упоминаемой сущности.<br /><br /> 1 = Объект или столбец<br /><br /> 6 = Тип<br /><br /> 10 = коллекция схем XML<br /><br /> 21 = функция секционирования|  
|referenced_class_desc|**nvarchar(60)**|Описание класса упоминаемой сущности.<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION|  
|is_caller_dependent|**bit**|Отображает привязку к схеме для упоминаемой сущности, полученную во время выполнения (так как идентификатор сущности зависит от схемы вызывающего объекта). Данная ситуация возможна только для хранимой процедуры, расширенной хранимой процедуры или определяемой пользователем функции, выполняемых в инструкции EXECUTE.<br /><br /> 1 = упоминаемая сущность зависит от вызывающего объекта и определяется во время выполнения. В этом случае параметр referenced_id принимает значение NULL.<br /><br /> 0 = идентификатор упоминаемой сущности не зависит от вызывающего объекта. Всегда имеет значение 0 для привязанных к схеме ссылок, а также для межбазовых и межсерверных ссылок, которые явно указывают имя схемы. Например, ссылка на сущность в формате `EXEC MyDatabase.MySchema.MyProc` не зависит от вызывающего объекта. При этом ссылка в формате `EXEC MyDatabase..MyProc` зависит от вызывающего объекта.|  
|is_ambiguous|**bit**|Указывает, что ссылка является неоднозначным и может быть разрешена во время выполнения в определяемую пользователем функцию, определяемый пользователем тип (UDT) или ссылку XQuery на столбец типа **XML**. Например, предположим, что инструкция `SELECT Sales.GetOrder() FROM Sales.MySales` определяется в хранимой процедуре. До выполнения хранимой процедуры неизвестно, является ли `Sales.GetOrder()` определяемой пользователем функцией в схеме `Sales` или столбцом `Sales` определяемого пользователем типа с методом `GetOrder()`.<br /><br /> 1 = ссылка на определяемую пользователем функцию или на метод определяемого пользователем типа (UDT) столбца неоднозначна.<br /><br /> 0 = ссылка определена однозначно, либо сущность при вызове функции может быть привязана.<br /><br /> Для привязанных к схеме ссылок всегда принимает значение 0.|  
|is_selected|**bit**|1 = объект или столбец выбран.|  
|is_updated|**bit**|1 = объект или столбец изменен.|  
|is_select_all|**bit**|1 = объект используется в предложении SELECT* (только на уровне объектов).|  
|is_all_columns_found|**bit**|1 = все зависимости столбца для объекта удалось обнаружить.<br /><br /> 0 = зависимости столбца для объекта не удалось обнаружить.|
|is_insert_all|**bit**|1 = объект используется в инструкции INSERT без списка столбцов (только на уровне объектов).<br /><br />Этот столбец был добавлен в SQL Server 2016.|  
|is_incomplete|**bit**|1 = объект или столбец содержит ошибку привязки и является неполным.<br /><br />Этот столбец был добавлен в SQL Server 2016 с пакетом обновления 2 (SP2).|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="exceptions"></a>Исключения  
 Возвращает пустой результирующий набор, если выполняется любое из следующих условий.  
  
-   Указан системный объект.  
  
-   Указанная сущность не существует в текущей базе данных.  
  
-   Указанная сущность не ссылается ни на какие сущности.  
  
-   Передан недопустимый параметр.  
  
 Выдает ошибку, если заданная ссылающаяся сущность является пронумерованной хранимой процедурой.  
  
 Возвращает ошибку 2020, когда не удается разрешить зависимости столбца. Эта ошибка не препятствует возврату запросом зависимостей на уровне объектов.  
  
## <a name="remarks"></a>Комментарии  
 Данная функция может выполняться в контексте любой базы данных и осуществляет отображение списка сущностей, ссылающихся на триггер DDL уровня сервера.  
  
 В следующей таблице перечислены типы сущностей, для которых созданы и обновляются данные о зависимостях. Данные о зависимостях не создаются и не обновляются для правил, значений по умолчанию, временных таблиц, временных хранимых процедур и системных объектов.  
  
|Тип сущности|Ссылающаяся сущность|Упоминаемая сущность|  
|-----------------|------------------------|-----------------------|  
|Таблица|Да*|Да|  
|Вид|Да|Да|  
|Хранимая процедура [!INCLUDE[tsql](../../includes/tsql-md.md)]**|Да|Да|  
|Хранимая процедура CLR|Нет|Да|  
|Определяемая пользователем функция [!INCLUDE[tsql](../../includes/tsql-md.md)]|Да|Да|  
|Определяемая пользователем функция CLR|Нет|Да|  
|Триггер CLR (DML и DDL)|Нет|Нет|  
|Триггер DML [!INCLUDE[tsql](../../includes/tsql-md.md)]|Да|Нет|  
|Триггер DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] уровня базы данных|Да|Нет|  
|Триггер DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] уровня сервера|Да|Нет|  
|Расширенные хранимые процедуры|Нет|Да|  
|Очередь|Нет|Да|  
|Синоним|Нет|Да|  
|Тип (псевдоним и определяемый пользователем тип данных CLR)|Нет|Да|  
|Коллекция схем XML|Нет|Да|  
|Функция секционирования|Нет|Да|  
| &nbsp; | &nbsp; | &nbsp; |

 \* Таблица обрабатывается как ссылающаяся сущность, только если она ссылается на [!INCLUDE[tsql](../../includes/tsql-md.md)] модуль, определяемый пользователем тип или коллекцию схем XML в определении вычисляемого столбца, проверочного ограничения или ограничения по умолчанию.  
  
 ** Пронумерованные хранимые процедуры с целочисленным значением больше 1 не отслеживаются в качестве ссылающихся или упоминаемых сущностей.  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения SELECT для функции sys.dm_sql_referenced_entities и разрешения VIEW DEFINITION для ссылающейся сущности. Разрешение SELECT по умолчанию предоставляется роли public. Требует разрешения VIEW DEFINITION для базы данных, либо, если ссылающаяся сущность является триггером DDL уровня базы данных, разрешения ALTER DATABASE DDL TRIGGER. Если указанный модуль является триггером DDL уровня сервера, то требуется разрешение VIEW ANY DEFINITION на уровне сервера.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-return-entities-that-are-referenced-by-a-database-level-ddl-trigger"></a>A. Возврат сущностей, на которые ссылается триггер DDL уровня базы данных  
 В ходе выполнения следующего примера производится отображение списка сущностей (таблиц и столбцов), упоминаемых триггером DDL уровня базы данных `ddlDatabaseTriggerLog`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT
        referenced_schema_name,
        referenced_entity_name,
        referenced_minor_name,
        referenced_minor_id,
        referenced_class_desc
    FROM
        sys.dm_sql_referenced_entities (
            'ddlDatabaseTriggerLog',
            'DATABASE_DDL_TRIGGER')
;
GO  
```  
  
### <a name="b-return-entities-that-are-referenced-by-an-object"></a>Б. Возврат сущностей, на которые ссылается объект  
 В ходе выполнения следующего примера производится отображение списка сущностей, упоминаемых в определяемой пользователем функции `dbo.ufnGetContactInformation`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT
        referenced_schema_name,
        referenced_entity_name,
        referenced_minor_name,
        referenced_minor_id,
        referenced_class_desc,
        is_caller_dependent,
        is_ambiguous
    FROM
        sys.dm_sql_referenced_entities (
            'dbo.ufnGetContactInformation',
            'OBJECT')
;
GO  
```  
  
### <a name="c-return-column-dependencies"></a>В. Возврат зависимостей столбцов  
 В ходе выполнения представленного ниже примера осуществляется создание таблицы `Table1`, в которой содержится вычисляемый столбец `c`, определяемый как сумма столбцов `a` и `b`. Затем производится вызов представления `sys.dm_sql_referenced_entities`. Представление содержит две строки, по одной для каждого столбца, определенного в вычисляемом столбце.  
  
```sql  
CREATE TABLE dbo.Table1 (a int, b int, c AS a + b);  
GO  
SELECT
        referenced_schema_name AS schema_name,  
        referenced_entity_name AS table_name,  
        referenced_minor_name  AS referenced_column,  
        COALESCE(
            COL_NAME(OBJECT_ID(N'dbo.Table1'),
            referencing_minor_id),
            'N/A') AS referencing_column_name  
    FROM
        sys.dm_sql_referenced_entities ('dbo.Table1', 'OBJECT')
;
GO

-- Remove the table.  
DROP TABLE dbo.Table1;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 schema_name table_name referenced_column referencing_column  
 ----------- ---------- ----------------- ------------------  
 dbo         Table1     a                 c  
 dbo         Table1     b                 c  
```

### <a name="d-returning-non-schema-bound-column-dependencies"></a>Г. Возврат списка зависимостей столбцов, не привязанных к схеме  
 В ходе выполнения представленного ниже примера выполняется удаление таблицы `Table1`, а также создание таблицы `Table2` и хранимой процедуры `Proc1`. Процедура ссылается на таблицу `Table2` и на несуществующую таблицу `Table1`. Представление `sys.dm_sql_referenced_entities` запускается с помощью хранимой процедуры, которая указана в качестве ссылающейся сущности. В результирующем наборе показана одна строка для таблицы `Table1` и 3 строки для таблицы `Table2`. Поскольку таблица `Table1` не существует, то не удается разрешить зависимости столбца и возвращается ошибка 2020. Столбец `is_all_columns_found` возвращает 0 для таблицы `Table1`, указывая, что существуют столбцы, которые не удалось обнаружить.  
  
```sql  
DROP TABLE IF EXISTS dbo.Table1;
GO  
CREATE TABLE dbo.Table2 (c1 int, c2 int);  
GO  
CREATE PROCEDURE dbo.Proc1 AS  
    SELECT a, b, c FROM Table1;  
    SELECT c1, c2 FROM Table2;  
GO  
SELECT
        referenced_id,
        referenced_entity_name AS table_name,
        referenced_minor_name  AS referenced_column_name,
        is_all_columns_found
    FROM
        sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1  
 935674381     Table2       C1                      1  
 935674381     Table2       C2                      1  
 NULL          Table1       NULL                    0  

Msg 2020, Level 16, State 1, Line 1
The dependencies reported for entity "dbo.Proc1" might not include
  references to all columns. This is either because the entity
  references an object that does not exist or because of an error
  in one or more statements in the entity.  Before rerunning the
  query, ensure that there are no errors in the entity and that
  all objects referenced by the entity exist.
 ```
  
### <a name="e-demonstrating-dynamic-dependency-maintenance"></a>Д. Отображение параметров обслуживания динамических зависимостей  

В этом примере E предполагается, что был запущен пример г. В примере E показано, что зависимости поддерживаются динамически. Пример выполняет следующие действия:

1. Повторно создает `Table1` , что было пропущено в примере г.
2. Затем запустите `sys.dm_sql_referenced_entities` повторно, выполнив хранимую процедуру, указанную в качестве ссылающейся сущности.

Результирующий набор показывает, что возвращаются обе таблицы и соответствующие им столбцы, определенные в хранимой процедуре. Кроме того, столбец `is_all_columns_found` возвращает значение 1 для всех объектов и столбцов.

```sql  
CREATE TABLE Table1 (a int, b int, c AS a + b);  
GO   
SELECT
        referenced_id,
        referenced_entity_name AS table_name,
        referenced_minor_name  AS column_name,
        is_all_columns_found
    FROM
        sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');
GO  
DROP TABLE Table1, Table2;  
DROP PROC Proc1;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1 
 935674381     Table2       c1                      1 
 935674381     Table2       c2                      1 
 967674495     Table1       NULL                    1 
 967674495     Table1       a                       1  
 967674495     Table1       b                       1  
 967674495     Table1       c                       1  
 ```
 
### <a name="f-returning-object-or-column-usage"></a>Е. Использование возвращения объекта или столбца  
 В следующем примере возвращаются зависимости объекта и столбца хранимой процедуры `HumanResources.uspUpdateEmployeePersonalInfo`. Эта процедура обновляет столбцы `NationalIDNumber` , `BirthDate,``MaritalStatus` и `Gender` `Employee` таблицы на основе указанного `BusinessEntityID` значения. Другая хранимая процедура, `upsLogError` определенная в инструкции try... CATCH для записи всех ошибок выполнения. Столбцы `is_selected`, `is_updated` и `is_select_all` возвращают сведения о способе использования этих объектов и столбцов в рамках ссылающегося объекта. Измененная таблица и столбцы в обновленном столбце помечаются единицей. Единственный выбранный столбец — `BusinessEntityID`, а хранимая процедура `uspLogError` не выбрана и не изменена.  

```sql  
USE AdventureWorks2012;
GO
SELECT
        referenced_entity_name AS table_name,
        referenced_minor_name  AS column_name,
        is_selected,  is_updated,  is_select_all
    FROM
        sys.dm_sql_referenced_entities(
            'HumanResources.uspUpdateEmployeePersonalInfo',
            'OBJECT')
;
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 table_name    column_name         is_selected is_updated is_select_all  
 ------------- ------------------- ----------- ---------- -------------  
 uspLogError   NULL                0           0          0  
 Employee      NULL                0           1          0  
 Employee      BusinessEntityID    1           0          0  
 Employee      NationalIDNumber    0           1          0  
 Employee      BirthDate           0           1          0  
 Employee      MaritalStatus       0           1          0  
 Employee      Gender              0           1          0
 ```
  
## <a name="see-also"></a>См. также:  
 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
