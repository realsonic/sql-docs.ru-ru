---
description: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)
title: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: language-reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: dc48dc319a628b389479eacb0c68b702f796a836
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644400"
---
# <a name="create-materialized-view-as-select-transact-sql"></a>CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)  

[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

В этой статье приведены сведения об инструкции CREATE MATERIALIZED VIEW AS SELECT T-SQL в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], используемой при разработке решений. Здесь также приведены примеры кодов.

Материализованное представление хранит данные, возвращенные запросом определения представления, и автоматически обновляется после изменений данных в базовых таблицах.   Это повышает производительность сложных запросов (обычно запросы с объединениями и агрегатами), а также упрощает обслуживание.   Благодаря возможности автоматического сопоставления плана выполнения материализованное представление не нужно указывать в запросе, чтобы оптимизатор учитывал его при подстановке.  Эта возможность позволяет специалистам по обработке данных реализовать материализованные представления в виде механизма повышения времени отклика запроса без необходимости изменять запросы.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE MATERIALIZED VIEW [ schema_name. ] materialized_view_name
    WITH (  
      <distribution_option>
    )
    AS <select_statement>
[;]

<distribution_option> ::=
    {  
        DISTRIBUTION = HASH ( distribution_column_name )  
      | DISTRIBUTION = ROUND_ROBIN  
    }

<select_statement> ::=
    SELECT select_criteria

```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Аргументы

*schema_name*     
 Имя схемы, которой принадлежит представление.  
  
*materialized_view_name*   
Имя представления. Имена представлений должны соответствовать требованиям, предъявляемым к идентификаторам. Указывать имя владельца представления не обязательно.  

*distribution option*     
Поддерживаются только распределения HASH и ROUND_ROBIN.

*select_statement*   
Список SELECT в определении материализованного представления должен соответствовать как минимум одному из следующих двух условий:
- Список SELECT содержит агрегатную функцию.
- В определении материализованного представления используется предложение GROUP BY, и все столбцы в предложении GROUP BY включены в список SELECT.  В предложении GROUP BY можно использовать до 32 столбцов.

В списке SELECT определения материализованного представления должны использоваться агрегатные функции.  Поддерживаемые агрегаты: MAX, MIN, AVG, COUNT, COUNT_BIG, SUM, VAR, STDEV.

Если в списке SELECT определения материализованного представления используются агрегаты MIN или MAX, применяются следующие требования:
 
- Необходимо использовать предложение FOR_APPEND.  Пример:
  ```sql 
  CREATE MATERIALIZED VIEW mv_test2  
  WITH (distribution = hash(i_category_id), FOR_APPEND)  
  AS
  SELECT MAX(i.i_rec_start_date) as max_i_rec_start_date, MIN(i.i_rec_end_date) as min_i_rec_end_date, i.i_item_sk, i.i_item_id, i.i_category_id
  FROM syntheticworkload.item i  
  GROUP BY i.i_item_sk, i.i_item_id, i.i_category_id
  ```

- Если в связанных базовых таблицах используется инструкция UPDATE или DELETE, материализованное представление отключается.  Это ограничение не относится к инструкциям INSERT.  Чтобы повторно включить материализованное представление, выполните инструкцию ALTER MATERIALIZED VIEW с параметром REBUILD.
  
## <a name="remarks"></a>Remarks

Материализованное представление в хранилище данных Azure похоже на индексированное представление SQL Server.Оно имеет практически те же ограничения, что и индексированное представление (подробности см. в статье [Создание индексированных представлений](../../relational-databases/views/create-indexed-views.md)) за исключением того, что материализованное представление поддерживает агрегатные функции.   

>[!Note]
>Несмотря на то, что CREATE MATERIALIZED VIEW не поддерживает COUNT, DISTINCT, COUNT (выражение DISTINCT) и COUNT_BIG (выражение DISTINCT), запросы SELECT с этими функциями могут по-прежнему пользоваться преимуществами материализованных представлений для повышения производительности, так как оптимизатор Synapse SQL может автоматически перезаписывать эти агрегаты в пользовательском запросе для сопоставления с существующими материализованными представлениями.  Дополнительные сведения см. в разделе с примером этой статьи. 

APPROX_COUNT_DISTINCT не поддерживается в CREATE MATERIALIZED VIEW AS SELECT.

Материализованное представление поддерживает только кластеризованный индекс columnstore. 

Материализованное представление не может ссылаться на другие представления.  

Материализованные представления невозможно создать для таблицы с динамическим маскированием данных (DDM), даже если столбец DDM не является частью материализованного представления.  Если столбец таблицы является частью активного или отключенного материализованного представления, DDM невозможно добавить в этот столбец.  

Невозможно также создать материализованное представление для таблицы с включенной безопасностью на уровне строк.

Материализованные представления можно создавать в секционированных таблицах.  Операции SPLIT и MERGE для секций поддерживаются для базовых таблиц материализованных представлений. Переключение (SWITCH) секций не поддерживается.  
 
Таблицы, связанные с материализованными представлениями, не поддерживают инструкцию ALTER TABLE SWITCH. Перед использованием инструкции ALTER TABLE SWITCH отключите или удалите все материализованные представления. Ниже приведены сценарии, в которых для создания материализованного представления в него нужно добавить новые столбцы.

|Сценарий|Новые столбцы, добавляемые к материализованному представлению|Комментировать|  
|-----------------|---------------|-----------------|
|COUNT_BIG() отсутствует в списке SELECT определения материализованного представления| COUNT_BIG (*) |Автоматически добавляется во время создания материализованного представления.  Вмешательство пользователя не требуется.|
|Пользователь указал функцию SUM(a) в списке SELECT определения материализованного представления, где "a" — это выражение, допускающее значение NULL |COUNT_BIG (a) |Пользователям необходимо добавить выражение "a" вручную в определении материализованного представления.|
|Пользователь указал функцию AVG(a) в списке SELECT определения материализованного представления, где "a" — это выражение.|SUM(a), COUNT_BIG(a)|Автоматически добавляется во время создания материализованного представления.  Вмешательство пользователя не требуется.|
|Пользователь указал функцию STDEV(a) в списке SELECT определения материализованного представления, где "a" — это выражение.|SUM(a), COUNT_BIG(a), SUM(square(a))|Автоматически добавляется во время создания материализованного представления.  Вмешательство пользователя не требуется. |
| | | |

После создания материализованные представления отображаются в SQL Server Management Studio в папке представлений экземпляра [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

Пользователи могут определить место, используемое материализованным представлением, с помощью инструкций [SP_SPACEUSED](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) и [DBCC PDW_SHOWSPACEUSED](../database-console-commands/dbcc-pdw-showspaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).  

Материализованное представление можно удалить с помощью инструкции DROP VIEW.  Отключить или перестроить материализованное представление можно с помощью инструкции ALTER MATERIALIZED VIEW.   

План EXPLAIN и графический предполагаемый план выполнения в SQL Server Management Studio предоставляют сведения о том, учитывает ли оптимизатор запросов материализованное представление во время выполнения запроса. и графический предполагаемый план выполнения в SQL Server Management Studio предоставляют сведения о том, учитывает ли оптимизатор запросов материализованное представление во время выполнения запроса.

Чтобы узнать, поддерживает ли инструкция SQL новое материализованное представление, выполните команду `EXPLAIN` со свойством `WITH_RECOMMENDATIONS`.  Дополнительные сведения см. в статье [EXPLAIN (Transact-SQL)](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).

## <a name="permissions"></a>Разрешения

Для выполнения этой инструкции требуется 1) разрешение REFERENCES и CREATE VIEW или 2) разрешение CONTROL в отношении схемы, в которой создается представление. 

## <a name="example"></a>Пример
A. В этом примере показано, как оптимизатор Synapse SQL автоматически использует материализованные представления при выполнении запроса для лучшей производительности, даже если в запросе используются функции, которые не поддерживаются в CREATE MATERIALIZED VIEW, такие как COUNT(выражение DISTINCT). Запрос, который раньше выполнялся несколько секунд, теперь выполняется за долю секунды без каких-либо изменений в пользовательском запросе.   

``` sql 

-- Create a table with ~536 million rows
create table t(a int not null, b int not null, c int not null) with (distribution=hash(a), clustered columnstore index);

insert into t values(1,1,1);

declare @p int =1;
while (@P < 30)
    begin
    insert into t select a+1,b+2,c+3 from t;  
    select @p +=1;
end

-- A SELECT query with COUNT_BIG (DISTINCT expression) took multiple seconds to complete and it reads data directly from the base table a. 
select a, count_big(distinct b) from t group by a;

-- Create two materialized views, not using COUNT_BIG(DISTINCT expression).
create materialized view V1 with(distribution=hash(a)) as select a, b from dbo.t group by a, b;

-- Clear all cache.

DBCC DROPCLEANBUFFERS;
DBCC freeproccache;

-- Check the estimated execution plan in SQL Server Management Studio.  It shows the SELECT query is first step (GET operator) is to read data from the materialized view V1, not from base table a.
select a, count_big(distinct b) from t group by a;

-- Now execute this SELECT query.  This time it took sub-second to complete because Synapse SQL engine automatically matches the query with materialized view V1 and uses it for faster query execution.  There was no change in the user query.

DECLARE @timerstart datetime2, @timerend datetime2;
SET @timerstart = sysdatetime();

select a, count_big(distinct b) from t group by a;

SET @timerend = sysdatetime()
select DATEDIFF(ms,@timerstart,@timerend);

```

  
## <a name="see-also"></a>См. также раздел

[Настройка производительности с помощью материализованного представления](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](./alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)      
[DROP VIEW](./drop-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)  
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
Представления каталога [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Системные представления, поддерживаемые в Azure[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Инструкции Т-SQL, поддерживаемые в Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)