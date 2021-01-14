---
title: Предложение FROM и JOIN, APPLY, PIVOT (T-SQL)
description: Предложение FROM и JOIN, APPLY, PIVOT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- JOIN
- FROM_TSQL
- FROM
- JOIN_TSQL
- OUTER_JOIN_TSQL
- INNER_JOIN_TSQL
- CROSS_TSQL
- CROSS_APPLY_TSQL
- APPLY_TSQL
- CROSS_JOIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OUTER APPLY operator
- hints [SQL Server], FROM clause
- SELECT statement [SQL Server], FROM clause
- ISO syntax
- DELETE statement [SQL Server], FROM clause
- CROSS APPLY operator
- FROM clause
- APPLY operator
- joins [SQL Server], FROM clause
- UPDATE statement [SQL Server], FROM clause
- derived tables
ms.assetid: 36b19e68-94f6-4539-aeb1-79f5312e4263
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 06/01/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 70cda7e45f17bb1dbeeaa69178e0538296572ae7
ms.sourcegitcommit: b652ff2f0f7edbb5bd2f8fdeac56348e4d84f8fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112676"
---
# <a name="from-clause-plus-join-apply-pivot-transact-sql"></a>Предложение FROM и JOIN, APPLY, PIVOT (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

В Transact-SQL предложение FROM доступно для следующих инструкций:

- [DELETE](../statements/delete-transact-sql.md)
- [UPDATE](update-transact-sql.md)
- [SELECT](select-transact-sql.md)

Как правило, предложение FROM требуется в инструкции SELECT. Исключением является случай, когда не указаны столбцы таблицы, а заданы только литералы или переменные и арифметические выражения.

В этой статье также рассматриваются следующие ключевые слова, которые могут использоваться в предложении FROM.

- [JOIN](../../relational-databases/performance/joins.md)
- APPLY
- [PIVOT](from-using-pivot-and-unpivot.md)

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ FROM { <table_source> } [ ,...n ] ]   
<table_source> ::=   
{  
    table_or_view_name [ FOR SYSTEM_TIME <system_time> ] [ AS ] table_alias ]   
        [ <tablesample_clause> ]   
        [ WITH ( < table_hint > [ [ , ]...n ] ) ]   
    | rowset_function [ [ AS ] table_alias ]   
        [ ( bulk_column_alias [ ,...n ] ) ]   
    | user_defined_function [ [ AS ] table_alias ]  
    | OPENXML <openxml_clause>   
    | derived_table [ [ AS ] table_alias ] [ ( column_alias [ ,...n ] ) ]   
    | <joined_table>   
    | <pivoted_table>   
    | <unpivoted_table>  
    | @variable [ [ AS ] table_alias ]  
    | @variable.function_call ( expression [ ,...n ] )   
        [ [ AS ] table_alias ] [ (column_alias [ ,...n ] ) ]     
}  
<tablesample_clause> ::=  
    TABLESAMPLE [SYSTEM] ( sample_number [ PERCENT | ROWS ] )   
        [ REPEATABLE ( repeat_seed ) ]   
  
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON <search_condition>   
    | <table_source> CROSS JOIN <table_source>   
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
<join_type> ::=   
    [ { INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } [ <join_hint> ] ]  
    JOIN  
  
<pivoted_table> ::=  
    table_source PIVOT <pivot_clause> [ [ AS ] table_alias ]  
  
<pivot_clause> ::=  
        ( aggregate_function ( value_column [ [ , ]...n ])   
        FOR pivot_column   
        IN ( <column_list> )   
    )   
  
<unpivoted_table> ::=  
    table_source UNPIVOT <unpivot_clause> [ [ AS ] table_alias ]  
  
<unpivot_clause> ::=  
    ( value_column FOR pivot_column IN ( <column_list> ) )   
  
<column_list> ::=  
    column_name [ ,...n ]   
  
<system_time> ::=  
{  
       AS OF <date_time>  
    |  FROM <start_date_time> TO <end_date_time>  
    |  BETWEEN <start_date_time> AND <end_date_time>  
    |  CONTAINED IN (<start_date_time> , <end_date_time>)   
    |  ALL  
}  
  
    <date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <start_date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <end_date_time>::=  
        <date_time_literal> | @date_time_variable  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
FROM { <table_source> [ ,...n ] }  
  
<table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [<tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
    | <joined_table>  
}  
  
<tablesample_clause> ::=
    TABLESAMPLE ( sample_number [ PERCENT ] ) -- Azure Synapse Analytics only  
 
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON search_condition   
    | <table_source> CROSS JOIN <table_source> 
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
  
<join_type> ::=   
    [ INNER ] [ <join hint> ] JOIN  
    | LEFT  [ OUTER ] JOIN  
    | RIGHT [ OUTER ] JOIN  
    | FULL  [ OUTER ] JOIN  
  
<join_hint> ::=   
    REDUCE  
    | REPLICATE  
    | REDISTRIBUTE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
\<table_source>  
 Указывает таблицу, представление, табличную переменную или источник производной таблицы с указанием или без указания псевдонима для использования в инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. В инструкции можно использовать до 256 источников таблиц, хотя предел изменяется в зависимости от доступной памяти и сложности других выражений в запросе. Отдельные запросы могут не поддерживать 256 источников таблиц.  
  
> [!NOTE]  
>  Производительность выполнения запросов может снизиться из-за большого количества таблиц, указанных в запросе. На время компиляции и оптимизации также влияют дополнительные факторы. Они включают в себя наличие индексов и индексированных представлений в каждом \<table_source> и размер \<select_list> в инструкции SELECT.  
  
 Порядок источников таблицы после ключевого слова FROM не влияет на возвращаемый результирующий набор. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибки, если в предложении FROM появляются повторяющиеся имена.  
  
 *table_or_view_name*  
 Имя таблицы или представления.  
  
 Если таблица или представление существует в другой базе данных одного и того же экземпляра службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], используйте полностью уточненное имя в форме *база данных*.*схема*.*имя_объекта*.  
  
 Если таблица или представление существует вне экземпляра службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]l, используйте четырехчастное имя в форме *связанный_свервер*.*каталог*.*схема*.*объект*. Дополнительные сведения см. в статье [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Для указания удаленного источника таблицы также можно использовать четырехкомпонентное имя, где в качестве компонента сервера используется функция [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Если указана функция OPENDATASOURCE, то аргументы *database_name* и *schema_name* могут применяться не ко всем источникам данных, а в зависимости от возможностей поставщика OLE DB, который обращается к удаленному объекту.  
  
 [AS] *table_alias*  
 Псевдоним для *table_source*, который может использоваться как для удобства, так и для различения таблицы или представления в самосоединении или во вложенном запросе. Псевдоним часто является сокращенным именем таблицы, использующимся для соотнесения с определенными столбцами таблиц в соединении. Если имя столбца существует более чем в одной таблице соединения, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребует, чтобы имя столбца было уточнено именем таблицы, представления или псевдонима. Если определен псевдоним, нельзя использовать имя таблицы.  
  
 При использовании производной таблицы, набора строк или функции с табличным значением либо предложения оператора (как PIVOT или UNPIVOT) требуемый аргумент *table_alias* в конце предложения является соответствующим именем таблицы для всех возвращаемых столбцов, включая группирующие столбцы.  
  
 WITH (\<table_hint> )  
 Указывает на то, что с данной таблицей и для данной инструкции оптимизатор запросов использует стратегию оптимизации или блокировки. Дополнительные сведения см. в разделе [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md).  
  
*rowset_function*  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Указывает одну из функций набора строк, например OPENROWSET, возвращающую объект, который можно использовать вместо ссылки на таблицу. Дополнительные сведения о списке функций набора строк см. в разделе [Функции набора строк (Transact-SQL)](../functions/opendatasource-transact-sql.md).  
  
 Использование функций OPENROWSET и OPENQUERY для задания удаленного объекта зависит от возможностей поставщика OLE DB, который обращается к удаленному объекту.  
  
 *bulk_column_alias*  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
 Дополнительный псевдоним для замены имени столбца в результирующем наборе. Псевдонимы столбца разрешены только в инструкциях SELECT, использующих функцию OPENROWSET с параметром BULK. При использовании аргумента *bulk_column_alias* необходимо указать псевдоним для каждого столбца таблицы в том же порядке, что и в файле.  
  
> [!NOTE]  
>  Данный псевдоним, если он присутствует, переопределяет атрибут NAME в элементах COLUMN файла XML.  
  
 *user_defined_function*  
 Указывает функцию с табличным значением.  
  
OPENXML \<openxml_clause>  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Обеспечивает представление XML-документа в виде набора строк. Дополнительные сведения см в разделе [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md).  
  
 *derived_table*  
 Это вложенный запрос, который извлекает строки из базы данных. *derived_table* используется в качестве входных данных для внешнего запроса.  
  
 Чтобы указать несколько строк в параметре *derived_table*, можно воспользоваться конструктором табличных значений [!INCLUDE[tsql](../../includes/tsql-md.md)]. Например, `SELECT * FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);`. Дополнительные сведения см. в разделе [Конструктор табличных значений (Transact-SQL)](../../t-sql/queries/table-value-constructor-transact-sql.md).  
  
 *column_alias*  
 Дополнительный псевдоним для замены имени столбца в результирующем наборе производной таблицы. Для каждого столбца в списке выбора следует включить по одному псевдониму столбца и заключить весь список псевдонимов столбцов в скобки.  
  
 *имя_таблицы_или_представления* FOR SYSTEM_TIME \<system_time>
**Применяется к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздней версии и [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Указывает, что конкретная версия данных возвращается из указанной темпоральной таблицы и связывается с таблицей журнала с системным управлением версиями.  
  
### <a name="tablesample-clause"></a>Предложение Tablesample
**Применимо к:** SQL Server, База данных SQL. Указывает, что из таблицы возвращается выборка данных. Выборка может быть приблизительной. Это предложение может использоваться в инструкциях SELECT или UPDATE в отношении любой первичной или присоединенной таблицы. TABLESAMPLE не может быть указано для представлений.  
  
> [!NOTE]  
>  При использовании предложения TABLESAMPLE к базам данных, которые были обновлены до [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с уровнем совместимости базы данных 110 или больше, оператор PIVOT нельзя использовать в запросах рекурсивного обобщенного табличного выражения (CTE). Дополнительные сведения см. в разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
 SYSTEM  
 Зависящий от реализации метод выборки, определенный стандартами ISO. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это единственно доступный метод выборки, и он применяется по умолчанию. SYSTEM использует основанный на страницах метод выборки со случайным набором страниц, все строки которых возвращаются как подмножество выборки.  
  
 *sample_number*  
 Точное или приближенное константное числовое выражение, представляющее процент или количество строк. При указании PERCENT аргумент *sample_number* неявно преобразуется в тип **float**; в противном случае он преобразуется в тип **bigint**. PERCENT является параметром по умолчанию.  
  
 PERCENT  
 Указывает, что из таблицы должен быть извлечен процент строк таблицы, равный значению аргумента *sample_number*. При указании PERCENT [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает приближенное значение указанного процента. При указании PERCENT выражение *sample_number* должно иметь значение от 0 до 100.  
  
 ROWS  
 Указывает, что будет извлечено количество строк, приблизительно равное значению *sample_number*. При указании ROWS [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает приближенное значение количества указанных строк. При указании ROWS результатом выражения *sample_number* должно быть целочисленное значение больше нуля.  
  
 REPEATABLE  
 Указывает, что заданная выборка может быть возвращена снова. При указании такого же значения *repeat_seed*[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет возвращать то же подмножество строк до тех пор, пока не будут внесены изменения в какую-либо строку таблицы. При указании другого значения *repeat_seed*[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], скорее всего, вернет другую выборку строк таблицы. Изменениями считаются следующие действия над таблицей: вставка, обновление, удаление, перестроение или дефрагментация индекса, а также восстановление или присоединение базы данных.  
  
 *repeat_seed*  
 Константное целочисленное выражение, используемое [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для формирования случайного числа. *repeat_seed* имеет тип **bigint**. Если аргумент *repeat_seed* не указан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] присваивает значение случайным образом. Для определенного значения аргумента *repeat_seed* результат выборки всегда тот же, если в таблице не было произведено никаких изменений. Результат выражения *repeat_seed* должен быть целочисленным значением больше нуля.  
  
### <a name="tablesample-clause"></a>Предложение Tablesample
**Применимо к:** [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

 Указывает, что из таблицы возвращается выборка данных. Выборка может быть приблизительной. Это предложение может использоваться в инструкциях SELECT или UPDATE в отношении любой первичной или присоединенной таблицы. TABLESAMPLE не может быть указано для представлений. 

 PERCENT  
 Указывает, что из таблицы должен быть извлечен процент строк таблицы, равный значению аргумента *sample_number*. При указании PERCENT [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] возвращает приближенное значение указанного процента. При указании PERCENT выражение *sample_number* должно иметь значение от 0 до 100.  


### <a name="joined-table"></a>Соединяемая таблица 
Соединяемая таблица — это результирующий набор, полученный из двух или более таблиц. Для нескольких соединений следует использовать скобки, чтобы изменить естественный порядок соединений.  
  
### <a name="join-type"></a>Тип соединения
Указание типа операции соединения.  
  
 INNER  
 Указывает, что возвращаются все совпадающие пары строк. Отмена несовпадающих строк из обеих таблиц. Если тип соединения не указан, этот тип задается по умолчанию.  
  
 FULL [ OUTER ]  
 Указывает, что в результирующий набор включаются строки как из левой, так и из правой таблицы, несоответствующие условиям соединения, а выходные столбцы, соответствующие оставшейся таблице, устанавливаются в значение NULL. Этим дополняются все строки, обычно возвращаемые при помощи INNER JOIN.  
  
 LEFT [ OUTER ]  
 Указывает, что все строки из левой таблицы, не соответствующие условиям соединения, включаются в результирующий набор, а выходные столбцы из оставшейся таблицы устанавливаются в значение NULL в дополнение ко всем строкам, возвращаемым внутренним соединением.  
  
 RIGHT [ OUTER ]  
 Указывает, что все строки из правой таблицы, не соответствующие условиям соединения, включаются в результирующий набор, а выходные столбцы, соответствующие оставшейся таблице, устанавливаются в значение NULL в дополнение ко всем строкам, возвращаемым внутренним соединением.  
  
### <a name="join-hint"></a>Указание соединения  
Указывает, что оптимизатор запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../includes/sssds-md.md)] использует одно указание соединения, или алгоритм выполнения, для каждого соединения, указанного в предложении FROM. Дополнительные сведения см. в разделе [Указания соединений (Transact-SQL)](../../t-sql/queries/hints-transact-sql-join.md).  
  
 Для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] эти указания соединения применяются к соединениям INNER по двум несовместимым столбцам распределения. Они могут повысить производительность запросов, ограничивая объем перемещаемых данных, который происходит во время обработки запросов. Допустимые указания соединения для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] показаны ниже.  
  
 REDUCE  
 Уменьшает количество подлежащих перемещению строк для таблицы в правой части соединения, чтобы сделать совместимыми два несовместимых столбца распределения. Указание REDUCE также называется указанием полусоединения.  
  
 REPLICATE  
 Приводит к репликации значений в столбце соединения из таблицы в левой части соединения на всех узлах. Таблица в правой части соединяется с реплицированной версией этих столбцов.  
  
 REDISTRIBUTE  
 Принудительно распространяет два источника данных на столбцы, указанные в предложении JOIN. Для распределенной таблицы [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] выполнит перемещение в случайном порядке. Для реплицированной таблицы [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] выполнит обрезанное перемещение. Сведения об этих типах перемещения см. в подразделе об операциях с планами запросов DMS в разделе с основными сведениями о планах запросов в [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Это указание может повысить производительность в случае, когда план запроса использует широковещательное перемещение для разрешения несовместимого соединения распределения.  
  
 JOIN  
 Указывает, что данная операция соединения должна произойти между указанными источниками или представлениями таблицы.  
  
 ON \<search_condition>  
 Задает условие, на котором основывается соединение. Условие может указывать любой предикат, хотя чаще используются столбцы и операторы сравнения, например:  
  
```sql
SELECT p.ProductID, v.BusinessEntityID  
FROM Production.Product AS p   
JOIN Purchasing.ProductVendor AS v  
ON (p.ProductID = v.ProductID);  
  
```  
  
 Если условие указывает столбцы, их имена и типы данных могут не совпадать, однако, если типы данных не совпадают, столбцы должны либо быть совместимыми, либо иметь типы, которые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может неявно преобразовать. Если типы данных не могут быть преобразованы неявно, условие должно проводить явное преобразование типа данных при помощи функции CONVERT.  
  
 Могут существовать предикаты, использующие в предложении ON только одну из соединяемых таблиц. Такие предикаты также могут присутствовать в предложении WHERE запроса. Хотя размещение таких предикатов не оказывает влияния в случае внутренних соединений (INNER), при использовании внешних соединений (OUTER) они могут привести к другому результату. Это происходит потому, что предикаты в предложении ON применяются к таблице до соединения, в то время как предложение WHERE семантически применяется к результату соединения.  
  
 Дополнительные сведения об условиях поиска и предикатах см. в статье [Условие поиска (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CROSS JOIN  
 Указывает перекрестное произведение двух таблиц. Возвращает те же строки, что и соединение без предложения WHERE в старом режиме, не совместимом с SQL-92.  
  
 *left_table_source* { CROSS | OUTER } APPLY *right_table_source*  
 Указывает, что *right_table_source* оператора APPLY определяется для каждой строки *left_table_source*. Данная функциональность полезна в том случае, когда *right_table_source* содержит функцию с табличным значением, которая принимает значения столбцов *left_table_source* в качестве одного из своих аргументов.  
  
 Вместе с ключевым словом APPLY должно быть указано либо CROSS, либо OUTER. Если указано CROSS, то при вычислении *right_table_source* для определенной строки *left_table_source* не создается ни одной строки и возвращается пустой результирующий набор.  
  
 При указании OUTER для каждой строки *left_table_source* создается одна строка, даже когда *right_table_source* вычисляется для этой строки и возвращается пустой результирующий набор.  
  
 Дополнительные сведения см. в разделе «Примечания».  
  
 *left_table_source*  
 Источник таблицы, определенный в предыдущем аргументе. Дополнительные сведения см. в разделе «Примечания».  
  
 *right_table_source*  
 Источник таблицы, определенный в предыдущем аргументе. Дополнительные сведения см. в разделе «Примечания».  
  
### <a name="pivot-clause"></a>Предложение PIVOT

 *table_source* PIVOT \<pivot_clause>  
 Указывает, что значение *table_source* основано на *pivot_column*. *table_source* представляет собой таблицу или табличное выражение. Выходными данными является таблица, содержащая все столбцы *table_source*, за исключением *pivot_column* и *value_column*. Столбцы *table_source*, кроме *pivot_column* и *value_column*, называются столбцами группирования оператора PIVOT. Дополнительные сведения о PIVOT и UNPIVOT см. в разделе [Использование операторов PIVOT и UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
 Оператор PIVOT применяет операцию группирования к входной таблице по отношению к столбцам группирования и возвращает одну строку для каждой группы. Кроме того, вывод содержит один столбец для каждого значения, указанного в *column_list*, который отображается в *pivot_column* в *input_table*.  
  
 Дополнительные сведения см. в разделе «Замечания» далее.  
  
 *aggregate_function*  
 Системная или определяемая пользователем агрегатная функция, имеющая один или более входов. Агрегатная функция должна быть инвариантной относительно значений NULL. Инвариантная относительно нулевых значений агрегатная функция при определении статистического значения не учитывает нулевые значения в группе.  
  
 Системная агрегатная функция COUNT(*) недопустима.  
  
 *value_column*  
 Столбец значений оператора PIVOT. При использовании вместе с оператором UNPIVOT аргумент *value_column* не может быть именем существующего столбца во входном *table_source*.  
  
 FOR *pivot_column*  
 Столбец сведения оператора PIVOT. Аргумент *pivot_column* должен иметь тип данных, который может быть явно или неявно преобразован в тип данных **nvarchar()** . Этот столбец не может иметь тип **image** или **rowversion**.  
  
 При использовании оператора UNPIVOT аргумент *pivot_column* является именем выходного столбца, полученного из *table_source*. В *table_source* не может быть существующего столбца с таким именем.  
  
 IN (*column_list* )  
 В предложении PIVOT представлены все значения в аргументе *pivot_column*, которые станут именами столбцов выходной таблицы. В списке не могут быть указаны какие-либо имена столбцов, которые уже существуют во входном *table_source*, к которому применяется сведение.  
  
 В предложении UNPIVOT представлены столбцы в *table_source*, которые будут сведены в один столбец *pivot_column*.  
  
 *table_alias*  
 Псевдоним выходной таблицы. Аргумент *pivot_table_alias* должен быть указан.  
  
 UNPIVOT \<unpivot_clause>  
 Указывает, что входная таблица сведена из нескольких столбцов в *column_list* в один столбец под названием *pivot_column*. Дополнительные сведения о PIVOT и UNPIVOT см. в разделе [Использование операторов PIVOT и UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
AS OF \<date_time>  
**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, а также [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Возвращает таблицу с одной записью для каждой строки, содержащей значения, которые были фактическими (текущими) в указанный момент времени в прошлом. На внутреннем уровне объединение выполняется между темпоральной таблицей и соответствующей таблицей журнала и результаты отфильтровываются так, чтобы возвращать значения в строке, которая была действительной на момент времени, определяемый параметром *\<date_time>* . Значение для строки считается действительным, если значение *system_start_time_column_name* меньше или равно значению параметра *\<date_time>* , а значение *system_end_time_column_name* больше значения параметра *\<date_time>* .   
  
FROM \<start_date_time> TO \<end_date_time>
**Применяется к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздней версии и [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
Возвращает таблицу, содержащую значения для всех версий записей, которые были активны в течение указанного интервала времени независимо от того, начали ли они быть активными до значения параметра *\<start_date_time>* аргумента FROM или перестали быть активными после значения параметра *\<end_date_time>* аргумента TO. На внутреннем уровне объединение выполняется между темпоральной таблицей и соответствующей таблицей журнала и результаты отфильтровываются так, чтобы возвращать значения для всех версий строк, которые были активными в течение указанного временного диапазона. В эти строки включаются те, которые стали активными точно в нижнюю границу периода времени, определяемую конечной точкой FROM, и не включаются те, которые стали активными точно в верхнюю границу периода времени, определяемую конечной точкой TO.  
  
BETWEEN \<start_date_time> AND \<end_date_time>  
**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, а также [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Аналогично приведенному выше описанию для **FROM \<start_date_time> TO \<end_date_time>** за исключением того, что таблица возвращаемых строк включает строки, которые стали активными на верхней границе периода времени, определяемой конечной точкой \<end_date_time>.  
  
CONTAINED IN (\<start_date_time> , \<end_date_time>)  
**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, а также [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Возвращает таблицу, содержащую значения для всех версий записей, которые были открыты и закрыты в течение указанного интервала времени, определяемого двумя значениями даты и времени в аргументе CONTAINED IN. В эти строки включаются те, которые стали активными точно в нижнюю границу периода времени, и те, которые перестали быть активными точно в верхнюю границу периода времени.  
  
ALL  
Возвращает таблицу, содержащую значения из всех строк из текущей таблицы и таблицы журнала.  
  
## <a name="remarks"></a>Remarks  
 Предложение FROM поддерживает синтаксис SQL-92-SQL для соединенных и производных таблиц. Синтаксис SQL-92 предусматривает операторы соединения INNER, LEFT OUTER, RIGHT OUTER, FULL OUTER и CROSS.  
  
 UNION и JOIN в предложении FROM поддерживаются в представлениях и в производных таблицах и вложенных запросах.  
  
 Самосоединение — это таблица, соединенная сама с собой. Операции вставки или обновления, основанные на самосоединении, следуют порядку, указанному в предложении FROM.  
  
 Так как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] учитывает статистику распределения и количества элементов со связанных серверов, предоставляющих статистику распределения столбцов, то указание в соединении REMOTE не требуется для принудительной удаленной оценки соединения. Обработчик запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] учитывает удаленную статистику и определяет, является ли стратегия удаленного соединения подходящей. Указание соединения REMOTE удобно для поставщиков, которые не предоставляют статистику распределения столбцов.  
  
## <a name="using-apply"></a>Использование оператора APPLY  
 Как левый, так и правый операнды оператора APPLY являются табличными выражениями. Главное различие между этими операндами состоит в том, что *right_table_source* может использовать функцию с табличным значением, которая принимает столбец из *left_table_source* в качестве одного из аргументов функции. *left_table_source* может включать функции с табличным значением, но не может содержать аргументы, которые являются столбцами из *right_table_source*.  
  
Для предоставления табличного источника для предложения FROM оператор APPLY выполняет следующее:  
  
1.  Оценивает *right_table_source* для каждой строки а *left_table_source* для создания наборов строк.  
  
    Значения в *right_table_source* зависят от *left_table_source*. *right_table_source* может быть представлено примерно в следующем виде: `TVF(left_table_source.row)`, где `TVF` является функцией с табличным значением.  
  
2.  Объединяет результирующие наборы, предоставляемые для каждой строки при оценке *right_table_source* с left_table_source *left_table_source*, посредством выполнения операции UNION ALL.  
  
    Список столбцов, полученный в результате выполнения оператора APPLY, представляет собой набор столбцов из *left_table_source*, объединенный со списком столбцов из *right_table_source*.  
  
## <a name="using-pivot-and-unpivot"></a>Использование операторов PIVOT и UNPIVOT  
 Аргументы *pivot_column* и *value_column* являются столбцами группирования, используемыми оператором PIVOT. Для получения выходного результирующего набора оператор PIVOT выполняет следующее:  
  
1.  Применяет GROUP BY к *input_table* к столбцам группирования и предоставляет одну выходную строку для каждой группы.  
  
     Столбцы группирования в выходной строке получают соответствующие значения столбцов этой группы в *input_table*.  
  
2.  Формирует значения столбцов в списке столбцов для каждой выходной строки, для чего выполняет следующее:  
  
    1.  Дополнительно группирует строки, созданные в GROUP BY для аргумента *pivot_column* в предыдущем шаге.  
  
         Для каждого выходного столбца в *column_list* выбирает подгруппу, которая удовлетворяет следующим условиям:  
  
         `pivot_column = CONVERT(<data type of pivot_column>, 'output_column')`  
  
    2.  *aggregate_function* вычисляется по отношению к *value_column* в этой подгруппе, и этот результат возвращается как значение соответствующего *output_column*. Если подгруппа пуста, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определяет для такого *output_column* значение NULL. Если используется агрегатная функция COUNT, а подгруппа пуста, то возвращается значение (0).  

> [!NOTE]
> Идентификаторы столбцов в предложении `UNPIVOT` следуют параметрам сортировки каталога. Для [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] параметры сортировки всегда соответствуют `SQL_Latin1_General_CP1_CI_AS`. Для частично автономных баз данных [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] параметры сортировки всегда соответствуют `Latin1_General_100_CI_AS_KS_WS_SC`. Если столбец используется в сочетании с другими столбцами, для предотвращения конфликтов требуется предложение collate (`COLLATE DATABASE_DEFAULT`).   
  
 Дополнительные сведения о PIVOT и UNPIVOT, включая примеры, см. в разделе [Использование операторов PIVOT и UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения для инструкции DELETE, SELECT или UPDATE.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-a-simple-from-clause"></a>A. Использование простого предложения FROM  
 В следующем примере извлекаются столбцы `TerritoryID` и `Name` из таблицы `SalesTerritory` в образце базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql    
SELECT TerritoryID, Name  
FROM Sales.SalesTerritory  
ORDER BY TerritoryID ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
TerritoryID Name                            
----------- ------------------------------  
1           Northwest                       
2           Northeast                       
3           Central                         
4           Southwest                       
5           Southeast                       
6           Canada                          
7           France                          
8           Germany                         
9           Australia                       
10          United Kingdom                  
(10 row(s) affected)  
```  
  
### <a name="b-using-the-tablock-and-holdlock-optimizer-hints"></a>Б. Использование подсказок оптимизатора TABLOCK и HOLDLOCK  
 Следующая частичная транзакция показывает, как явно указать совмещаемую блокировку на таблицу `Employee` и как прочитать индекс. Блокировка удерживается на протяжении всей транзакции.  
  
```sql    
BEGIN TRAN  
SELECT COUNT(*)   
FROM HumanResources.Employee WITH (TABLOCK, HOLDLOCK) ;  
```  
  
### <a name="c-using-the-sql-92-cross-join-syntax"></a>В. Использование синтаксиса SQL-92 для CROSS JOIN  
 В следующем примере возвращается векторное произведение двух таблиц `Employee` и `Department` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Возвращается список всех возможных сочетаний строк `BusinessEntityID` и все строки имен `Department`.  
  
```sql    
SELECT e.BusinessEntityID, d.Name AS Department  
FROM HumanResources.Employee AS e  
CROSS JOIN HumanResources.Department AS d  
ORDER BY e.BusinessEntityID, d.Name ;  
```  
  
### <a name="d-using-the-sql-92-full-outer-join-syntax"></a>Г. Использование синтаксиса SQL-92 для FULL OUTER JOIN  
 В следующем примере возвращается название продукта и любые соответствующие заказы на продажу в таблице `SalesOrderDetail` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. В примере также возвращаются все заказы на продажу, продукты для которых не представлены в таблице `Product`, и все продукты с заказом на продажу, отличные от тех, которые представлены в таблице `Product`.  
  
```sql  
-- The OUTER keyword following the FULL keyword is optional.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
FULL OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="e-using-the-sql-92-left-outer-join-syntax"></a>Д. Использование синтаксиса SQL-92 для LEFT OUTER JOIN  
 Следующий пример соединяет две таблицы по столбцу `ProductID` и сохраняет несовпадающие строки из левой таблицы. Таблица `Product` сопоставляется с таблицей `SalesOrderDetail` по столбцам `ProductID` в каждой таблице. В результирующем наборе отражаются все продукты (как входящие в заказы, так и не входящие).  
  
```sql    
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
LEFT OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="f-using-the-sql-92-inner-join-syntax"></a>Е. Использование синтаксиса SQL-92 для INNER JOIN  
 Следующий пример возвращает все названия продуктов и идентификаторы заказов.  
  
```sql    
-- By default, SQL Server performs an INNER JOIN if only the JOIN   
-- keyword is specified.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
INNER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="g-using-the-sql-92-right-outer-join-syntax"></a>Ж. Использование синтаксиса SQL-92 для RIGHT OUTER JOIN  
 Следующий пример соединяет две таблицы по столбцу `TerritoryID` и сохраняет несовпадающие строки из правой таблицы. Таблица `SalesTerritory` сопоставляется с таблицей `SalesPerson` по столбцу `TerritoryID` каждой таблицы. В результирующем наборе отображаются все представители отдела продаж независимо от того, назначена им обслуживаемая территория или нет.  
  
```sql    
SELECT st.Name AS Territory, sp.BusinessEntityID  
FROM Sales.SalesTerritory AS st   
RIGHT OUTER JOIN Sales.SalesPerson AS sp  
ON st.TerritoryID = sp.TerritoryID ;  
```  
  
### <a name="h-using-hash-and-merge-join-hints"></a>З. Использование подсказок соединения HASH и MERGE  
 Следующий пример выполняет соединение трех таблиц — `Product`, `ProductVendor` и `Vendor` — для формирования списка продуктов и их поставщиков. Оптимизатор запросов соединяет таблицы `Product` и `ProductVendor` (`p` и `pv`) с помощью соединения слиянием (MERGE). Затем результаты соединения слиянием таблиц `Product` и `ProductVendor` (`p` и `pv`) соединяются при помощи HASH в таблицу `Vendor` для формирования (`p` и `pv`) и `v`.  
  
> [!IMPORTANT]  
>  После того как задано указание соединения, ключевое слово INNER более не является необязательным и должно быть задано в явном виде для выполнения INNER JOIN.  
  
```sql    
SELECT p.Name AS ProductName, v.Name AS VendorName  
FROM Production.Product AS p   
INNER MERGE JOIN Purchasing.ProductVendor AS pv   
ON p.ProductID = pv.ProductID  
INNER HASH JOIN Purchasing.Vendor AS v  
ON pv.BusinessEntityID = v.BusinessEntityID  
ORDER BY p.Name, v.Name ;  
```  
  
### <a name="i-using-a-derived-table"></a>И. Использование производной таблицы  
 Следующий пример использует производную таблицу, инструкцию `SELECT` после предложения `FROM`, для возврата имен и фамилий сотрудников и городов, в которых они проживают.  
  
```sql    
SELECT RTRIM(p.FirstName) + ' ' + LTRIM(p.LastName) AS Name, d.City  
FROM Person.Person AS p  
INNER JOIN HumanResources.Employee e ON p.BusinessEntityID = e.BusinessEntityID   
INNER JOIN  
   (SELECT bea.BusinessEntityID, a.City   
    FROM Person.Address AS a  
    INNER JOIN Person.BusinessEntityAddress AS bea  
    ON a.AddressID = bea.AddressID) AS d  
ON p.BusinessEntityID = d.BusinessEntityID  
ORDER BY p.LastName, p.FirstName;  
```  
  
### <a name="j-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>К. Использование TABLESAMPLE для чтения данных из выборки строк в таблице  
 В следующем примере используется `TABLESAMPLE` в предложении `FROM` для возврата около `10` процентов всех строк из таблицы `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;  
```  
  
### <a name="k-using-apply"></a>Л. Использование оператора APPLY  
Следующий пример предполагает, что в базе данных существуют следующие таблицы и функция с табличным значением:  

|Имени объекта|Имена столбцов|      
|---|---|   
|Departments|DeptID, DivisionID, DeptName, DeptMgrID|      
|EmpMgr|MgrID, EmpID|     
|Employees|EmpID, EmpLastName, EmpFirstName, EmpSalary|  
|GetReports(MgrID)|EmpID, EmpLastName, EmpSalary|     
  
Функция с табличным значением `GetReports` возвращает список всех сотрудников, которые находятся в прямом или косвенном подчинении указанного менеджера `MgrID`.  
  
В этом примере используется `APPLY` для возврата всех отделов и всех сотрудников этих отделов. Если в каком-либо отделе нет сотрудников, для этого отдела не будет возвращено никаких строк.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d    
CROSS APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
Если необходимо, чтобы запрос предоставил строки для тех отделов без сотрудников, в которых будут выданы значения NULL для столбцов `EmpID`, `EmpLastName` и `EmpSalary`, нужно вместо APPLY применить `OUTER APPLY`.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d   
OUTER APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
### <a name="l-using-cross-apply"></a>М. Использование CROSS APPLY  
В следующем примере показано получение моментального снимка всех планов запросов, находящихся в кэше планов, путем получения дескрипторов планов для всех планов запросов в кэше запросом динамического административного представления `sys.dm_exec_cached_plans`. Затем оператор `CROSS APPLY` передает дескрипторы планов в `sys.dm_exec_query_plan`. Вывод инструкции Showplan в формате XML для каждого плана, находящегося в кэше планов, находится в столбце `query_plan` возвращаемой таблицы.  
  
```sql
USE master;  
GO  
SELECT dbid, object_id, query_plan   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);   
GO  
```  
  
### <a name="m-using-for-system_time"></a>Н. Использование FOR SYSTEM_TIME  
  
**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, а также [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
В следующем примере используется аргумент SYSTEM_TIME AS OF date_time_literal_or_variable для возврата строк таблицы, которые были актуальны по состоянию на 1 января 2014 г.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME AS OF '2014-01-01'  
WHERE ManagerID = 5;
```  
  
В следующем примере используется аргумент SYSTEM_TIME FROM date_time_literal_or_variable TO date_time_literal_or_variable для возврата всех строк, которые были активны в течение периода с 1 января 2013 г. по 1 января 2014 г., исключая верхнее значение.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM '2013-01-01' TO '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 В следующем примере используется аргумент SSYSTEM_TIME BETWEEN date_time_literal_or_variable AND date_time_literal_or_variabl для возврата всех строк, которые были активны в течение периода с 1 января 2013 г. по 1 января 2014 г., включая верхнее значение.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME BETWEEN '2013-01-01' AND '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 В следующем примере используется аргумент SYSTEM_TIME CONTAINED IN (date_time_literal_or_variable, date_time_literal_or_variable) для возврата всех строк, которые были открыты и закрыты в течение периода с 1 января 2013 г. по 1 января 2014 г.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME CONTAINED IN ( '2013-01-01', '2014-01-01' )  
WHERE ManagerID = 5;
```  
  
 В следующем примере для предоставления граничных значений даты для запроса используется переменная, а не литерал.  
  
```sql
DECLARE @AsOfFrom datetime2 = dateadd(month,-12, sysutcdatetime());
DECLARE @AsOfTo datetime2 = dateadd(month,-6, sysutcdatetime());
  
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM @AsOfFrom TO @AsOfTo  
WHERE ManagerID = 5;
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="n-using-the-inner-join-syntax"></a>О. Использование синтаксиса INNER JOIN  
 В следующем примере возвращаются столбцы `SalesOrderNumber`, `ProductKey` и `EnglishProductName` из таблиц `FactInternetSales` и `DimProduct` с одинаковым ключом соединения `ProductKey` в обеих таблицах. Каждый столбец `SalesOrderNumber` и `EnglishProductName` существует только в одной из таблиц, поэтому не нужно указывать псевдоним таблицы с этими столбцами, как показано. Эти псевдонимы приводятся исключительно для удобства чтения. Слово **AS** перед именем псевдонима не является обязательным, но рекомендуется для удобства чтения и соответствия стандарту ANSI.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
INNER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Поскольку для внутренних соединений не требуется ключевое слово `INNER`, этот же запрос можно записать так:  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
ON dp.ProductKey = fis.ProductKey;  
```  
  
 Для ограничения результатов в этом запросе также можно использовать предложение `WHERE`. В этом примере результаты ограничиваются `SalesOrderNumber` значениями, выше чем "SO5000":  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey  
WHERE fis.SalesOrderNumber > 'SO50000'  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="o-using-the-left-outer-join-and-right-outer-join-syntax"></a>П. Использование синтаксиса LEFT OUTER JOIN и RIGHT OUTER JOIN  
 В следующем примере соединяются таблицы `FactInternetSales` и `DimProduct` по столбцам `ProductKey`. Синтаксис левого внешнего соединения сохраняет несовпадающие строки из левой (`FactInternetSales`) таблицы. Поскольку таблица `FactInternetSales` не содержит значения `ProductKey`, которые не соответствуют значениям в таблице `DimProduct`, этот запрос возвращает те же строки, что и в первом примере внутреннего соединения выше.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
LEFT OUTER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Этот запрос можно написать без ключевого слова `OUTER`.  
  
 В правых внешних соединениях сохраняются несовпадающие строки из правой таблицы. В следующем примере возвращаются те же строки, что и в приведенном выше примере левого внешнего соединения.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM DimProduct AS dp 
RIGHT OUTER JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Следующий запрос использует таблицу `DimSalesTerritory` в качестве левой таблицы в левом внешнем соединении. Он извлекает значения `SalesOrderNumber` из таблицы `FactInternetSales`. Если заказы по конкретному `SalesTerritoryKey` отсутствуют, запрос вернет значение NULL для `SalesOrderNumber` для этой строки. Этот запрос упорядочен по столбцу `SalesOrderNumber`, чтобы все значения NULL в этом столбце отображались в верхней части результатов.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
LEFT OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Этот запрос можно переписать с правым внешним соединением, чтобы получать те же результаты:  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM FactInternetSales AS fis 
RIGHT OUTER JOIN DimSalesTerritory AS dst  
    ON fis.SalesTerritoryKey = dst.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="p-using-the-full-outer-join-syntax"></a>Т. Использование синтаксиса FULL OUTER JOIN  
 В следующем примере показано полное внешнее соединение, которое возвращает все строки из обеих соединенных таблиц, но возвращает NULL для значений, которые не соответствуют значениям из другой таблицы.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Этот запрос можно написать без ключевого слова `OUTER`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="q-using-the-cross-join-syntax"></a>У. Использование синтаксиса CROSS JOIN  
 В следующем примере возвращается векторное произведение двух таблиц `FactInternetSales` и `DimSalesTerritory`. Возвращается список всех возможных сочетаний `SalesOrderNumber` и `SalesTerritoryKey`. Обратите внимание на отсутствие предложения `ON` в запросе перекрестного соединения.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
CROSS JOIN FactInternetSales AS fis  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="r-using-a-derived-table"></a>Ф. Использование производной таблицы  
 В следующем примере используется производная таблица (инструкция `SELECT`после предложения `FROM`) для возврата столбцов `CustomerKey` и `LastName` всех клиентов в таблице `DimCustomer` со значениями `BirthDate` позже 1 января 1970 г. и фамилией Smith.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, LastName  
FROM  
   (SELECT * FROM DimCustomer  
    WHERE BirthDate > '01/01/1970') AS DimCustomerDerivedTable  
WHERE LastName = 'Smith'  
ORDER BY LastName;  
```  
  
### <a name="s-reduce-join-hint-example"></a>Х. Пример указания соединения REDUCE  
 В следующем примере используется указание соединения `REDUCE` для изменения обработки производной таблицы в запросе. При использовании указания соединения `REDUCE` в этом запросе выполняется проецирование, репликация и разделение `fis.ProductKey` и последующее соединение с `DimProduct` во время случайного перемещения `DimProduct` в `ProductKey`. Результирующая производная таблица распространяется на `fis.ProductKey`.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REDUCE JOIN FactInternetSales AS fis   
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="t-replicate-join-hint-example"></a>T. Пример указания соединения REPLICATE  
 В следующем примере показан тот же запрос, что и в предыдущем примере, за исключением того, что вместо указания соединения `REDUCE` используется указание соединения `REPLICATE`. Использование указания `REPLICATE` приводит к репликации значений в столбце соединения `ProductKey` из таблицы `FactInternetSales`на всех узлах. Таблица `DimProduct` соединяется с реплицированной версией этих значений.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REPLICATE JOIN FactInternetSales AS fis  
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="u-using-the-redistribute-hint-to-guarantee-a-shuffle-move-for-a-distribution-incompatible-join"></a>Ф. Использование указания REDISTRIBUTE для перемещения в случайном порядке для соединения несовместимого распределения  
 В следующем запросе используется указание запроса REDISTRIBUTE в соединении несовместимого распределения. Это гарантирует, что оптимизатор запросов будет использовать в плане запроса перемещение в случайном порядке. Кроме того, план запроса не будет использовать широковещательное перемещение, при котором распределенная таблица переносится в реплицированную таблицу.  
  
 В следующем примере указание REDISTRIBUTE принудительно вызывает перемещение в случайном порядке в таблице FactInternetSales, поскольку ProductKey является столбцом распределения для DimProduct и не является столбцом распределения для FactInternetSales.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN  
SELECT dp.ProductKey, fis.SalesOrderNumber, fis.TotalProductCost  
FROM DimProduct AS dp 
INNER REDISTRIBUTE JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  

### <a name="v-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>V. Использование TABLESAMPLE для чтения данных из выборки строк в таблице  
 В следующем примере используется `TABLESAMPLE` в предложении `FROM` для возврата около `10` процентов всех строк из таблицы `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;
```
  
## <a name="see-also"></a>См. также:  
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [OPENQUERY (Transact-SQL)](../../t-sql/functions/openquery-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)
