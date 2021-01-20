---
description: sp_data_source_objects (Transact-SQL)
title: sp_data_source_objects | Документация Майкрософт
ms.custom: ''
ms.date: 11/14/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_objects
helpviewer_keywords:
- PolyBase
ms.assetid: 48066431-fed2-4a8a-85af-ac704689e183
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4c76026dcd24572b4d52c6b82246e5293c22e713
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596692"
---
# <a name="sp_data_source_objects-transact-sql"></a>sp_data_source_objects (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Возвращает список объектов таблицы, доступных для виртуализации.

> [!NOTE]
> Эта процедура введена в [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Синтаксис  

```syntaxsql
sp_data_source_objects  
         [ @data_source = ] 'data_source'
     [ , [ @object_root_name = ] 'object_root_name' ]
     [ , [ @max_search_depth = ] max_search_depth ]
     [ , [ @search_options = ] 'search_options' ]
[ ; ]
```  
  
## <a name="arguments"></a>Аргументы  

`[ @data_source = ] 'data_source'`   
Имя внешнего источника данных, из которого необходимо получить метаданные. Параметр `@data_source` равен `sysname`.  

`[ @object_root_name = ] 'object_root_name'`   
Этот параметр является корнем имен объектов для поиска. `@object_root_name` имеет значение `nvarchar(max)` и значение по умолчанию `NULL` .

Этот вызов возвращает только внешние объекты, которые начинаются со значения, заданного для `@object_root_name` .

Если источник данных ODBC подключается к системе управления реляционными базами данных (РСУБД), использующей имена из трех частей, `@object_root_name` не может содержать частичное имя базы данных. В этих случаях параметр `@object_root_name` должен содержать все три части, а третья часть — имя искомого объекта.
> [!CAUTION]
> Из-за различий между платформами внешних данных некоторые платформы не возвращают никаких результатов, если предоставлено значение по умолчанию `NULL` . Некоторые обрабатываются `NULL` как отсутствие фильтра. Например, Oracle РДМБС не возвращает результаты, если `NULL` предоставляется для `@object_root_name` .

`[ @max_search_depth = ] max_search_depth`   
Это значение указывает максимальную глубину (в частях), за которой следует `@object_root_name` искать. `@max_search_depth` параметр имеет значение `int` со значением по умолчанию 1.

Например, значение, `@max_search_depth` равное 1, с `@object_root_name` именем SQL Server базы данных, вернет Schemata, содержащийся в базе данных.

Объект `@max_search_depth` `NULL` будет возвращать сведения о том `@object_root_name` , существует ли он и не является пустым, в случае каталога или схемы.

`[ @search_options = ] 'search_options'`   
`search_options`Параметр имеет тип nvarchar (max) и значение по умолчанию `NULL` .

Этот параметр не используется, но может быть реализован в будущем.

## <a name="result-sets"></a>Наборы результатов

| Имя столбца | Тип данных | Описание |
|--|--|--|
| Object_Type | nvarchar(200) | Тип объекта (пример: таблица или база данных). |
| OBJECT_NAME | nvarchar(max) | Полное имя объекта. Экранирование с использованием символа кавычки, зависящего от внутреннего сервера. |
| OBJECT_LEAF_NAME | nvarchar(max) | Неполное имя объекта. |
| TABLE_LOCATION | nvarchar(max) | Допустимая строка расположения таблицы, которую можно использовать для инструкции CREATE EXTERNAL TABLE. Будет `NULL` ли это неприменимо. |
  
## <a name="permissions"></a>Разрешения

Требуется разрешение ALTER ANY EXTERNAL DATA SOURCE.  

## <a name="remarks"></a>Remarks  

На экземпляре SQL Server должен быть установлен компонент  [polybase](../../relational-databases/polybase/polybase-guide.md) .

Эта хранимая процедура поддерживает соединители для:

- SQL Server
- Oracle;
- Teradata
- MongoDB
- Cosmos DB

Хранимая процедура не поддерживает универсальные соединители источника ODBC dta.

Понятие пустое и непустое относится к поведению драйвера ODBC и [ `SQLTables` функции](../native-client-odbc-api/sqltables.md). Не пусто указывает, что объект содержит таблицы, а не строки. Например, пустая схема не содержит таблиц в SQL Server. Пустая база данных содержит без таблиц в Teradata.

Типы объектов определяются драйвером ODBC для внешнего источника данных. Каждый внешний источник данных определяет, какой именно объект является таблицей. Это могут быть объекты базы данных, такие как функции в TeraData, или синонимы в Oracle. Polybase не может подключиться к некоторым объектам ODBC как к внешним таблицам, поэтому в столбце TABLE_LOCATION не будет значения. Несмотря на то, что наличие одного из этих объектов может сделать базу данных или схему не пустой.

Используйте `sp_data_source_objects` и [`sp_data_source_table_columns`](sp-data-source-table-columns.md) для обнаружения внешних объектов. Эти системные хранимые процедуры возвращают схему таблиц, которые доступны для виртуализации. Azure Data Studio использует эти две хранимые процедуры для поддержки [виртуализации данных](../../azure-data-studio/extensions/data-virtualization-extension.md). Используйте [sp_data_source_table_columns](sp-data-source-table-columns.md) для обнаружения схем внешней таблицы, представленных в SQL Server типах данных.

### <a name="data-source-type-specific-remarks"></a>Замечания по конкретным типам источников данных

* Teradata

   Системные представления Teradata не используют безопасность на уровне строк (RLS), поэтому пользователи могут видеть наличие таблиц, которые они не могут запрашивать.

* MongoDB

   Некоторые более ранние версии MongoDB ограничивают возможность перечислять все базы данных для пользователей, имеющих права администратора. Пользователи без этого разрешения могут получить ошибки проверки подлинности, пытающиеся выполнить эту процедуру с нулевым значением `@object_root_name` .

## <a name="examples"></a>Примеры  

### <a name="sql-server"></a>SQL Server

В следующем примере возвращаются все базы данных, Schemata, таблицы и представления.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 3;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | База данных | NULL |
| SCHEMA | "база данных". " dbo | dbo | NULL |
| TABLE | "база данных". " dbo "." пользовательских | клиент | [база данных]. [dbo]. пользовательских |
| TABLE | "база данных". " dbo "." детализирован | item | [база данных]. [dbo]. детализирован |
| TABLE | "база данных". " dbo "." Nation | Nation | [база данных]. [dbo]. Nation |

В следующем примере возвращаются все базы данных.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "Усердатабасе" | усердатабасе | NULL |
| DATABASE | "master" | master | NULL |
| DATABASE | msdb | msdb | NULL |
| DATABASE | базе | tempdb | NULL |
| DATABASE | "database" | База данных | NULL |

В следующем примере возвращаются все Schemata в базе данных.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| SCHEMA | "база данных". " dbo | dbo | NULL |
| SCHEMA | "база данных". " INFORMATION_SCHEMA " | INFORMATION_SCHEMA | NULL |
| SCHEMA | "база данных". " представления | sys | NULL |

В следующем примере возвращаются все таблицы в схеме. 

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database].[dbo]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| TABLE | "база данных". " dbo "." пользовательских | клиент | [база данных]. [dbo]. пользовательских |
| TABLE | "база данных". " dbo "." детализирован | item | [база данных]. [dbo]. детализирован |
| TABLE | "база данных". " dbo "." Nation | Nation | [база данных]. [dbo]. Nation |
| TABLE | "база данных". " dbo "." перемещен | заказы | [база данных]. [dbo]. перемещен |
| TABLE | "база данных". " dbo "." блок | блок | [база данных]. [dbo]. блок |

### <a name="oracle"></a>Oracle;

В следующем примере возвращается полный Schemata и таблицы, функции, представления и т. д.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[OracleObjectRoot]'; 
DECLARE @max_search_depth INT = 2; 
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| VIEW | "SYS". ALL_SQLSET_STATEMENTS " | ALL_SQLSET_STATEMENTS | [ОРАКЛЕОБЖЕКТРУТ]. [SYS]. [ALL_SQLSET_STATEMENTS] |
| SYSTEM TABLE | "SYS". НАЧАЛЬная Загрузка $ " | НАЧАЛЬная Загрузка $ | [ОРАКЛЕОБЖЕКТРУТ]. [SYS]. [НАЧАЛЬная Загрузка $] |
| SYNONYM | "PUBLIC". ALL_ALL_TABLES " | ALL_ALL_TABLES | NULL |
| SCHEMA | "database" | База данных | NULL |
| TABLE | "база данных". " пользовательских | клиент | [ОРАКЛЕОБЖЕКТРУТ]. [база данных]. пользовательских |

### <a name="teradata"></a>Teradata

В следующем примере возвращаются все базы данных и таблицы, функции, представления и т. д.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |  
|--|--|--|--|
| FUNCTION | "СИСЛИБ". Екстрактролес " | екстрактролес | NULL |  
| SYSTEM TABLE | "DBC". Удткаст " | удткаст | [DBC]. [Удткаст] |  
| TYPE | "СИСУДТЛИБ". КОД | XML | NULL |  
| DATABASE | "database" | База данных | NULL |
| TABLE | "база данных". " пользовательских | клиент | [база данных]. пользовательских |  

### <a name="mongo-db"></a>MongoDB

В следующем примере возвращаются все базы данных и таблицы.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | База данных | NULL |
| TABLE | "база данных". " пользовательских | клиент | [база данных]. пользовательских |
| TABLE | "база данных". " детализирован | item | [база данных]. детализирован |
| TABLE | "база данных". " Nation | Nation | [база данных]. Nation |
| TABLE | "база данных". " перемещен | заказы | [база данных]. перемещен |

## <a name="see-also"></a>См. также раздел

- [sp_data_source_columns](./sp-data-source-table-columns.md)   
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)
- [Расширение Data Virtualization для Azure Data Studio](../../azure-data-studio/extensions/data-virtualization-extension.md)   
- [Приступая к работе с PolyBase](../polybase/polybase-guide.md)   
- [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)