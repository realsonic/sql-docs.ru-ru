---
description: CREATE STATISTICS (Transact-SQL)
title: CREATE STATISTICS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STATISTICS
- STATISTICS_TSQL
- CREATE STATISTICS
- CREATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], creating
- indexed views [SQL Server], statistics
- FULLSCAN option
- CREATE STATISTICS statement
- filtered statistics [SQL Server]
- creating statistics [SQL Server]
- NORECOMPUTE clause
ms.assetid: b23e2f6b-076c-4e6d-9281-764bdb616ad2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f830b33303c626afe27e6e509348d13336a5daea
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102394"
---
# <a name="create-statistics-transact-sql"></a>CREATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Создает статистику по оптимизации запроса для одного или нескольких столбцов таблицы, индексированного представления или внешней таблицы. Для большинства запросов оптимизатор уже создает достаточно статистики для формирования высококачественного плана запроса, но в некоторых случаях для повышения производительности запросов нужно создать дополнительные статистические данные с помощью инструкции CREATE STATISTICS или изменить структуру запроса.  
  
 Дополнительные сведения см. в разделе [Статистика](../../relational-databases/statistics/statistics.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create statistics on an external table  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WITH FULLSCAN ] ;  
  
-- Create statistics on a regular table or indexed view  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH   
        [ [ FULLSCAN   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | SAMPLE number { PERCENT | ROWS }   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | <update_stats_stream_option> [ ,...n ]    
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ]
    ] ;  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
    
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ] 
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE STATISTICS statistics_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( column_name  [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH {  
           FULLSCAN   
           | SAMPLE number PERCENT   
      }  
    ]  
[;]  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *statistics_name*  
 Имя создаваемой статистики.  
  
 *table_or_indexed_view_name*  
 Имя таблицы, индексированного представления или внешней таблицы, для которого требуется создать статистику. Чтобы создать статистику для другой базы данных, укажите полное имя таблицы.  
  
 *column [ ,...n]*  
 Один или несколько столбцов, которые будут включены в статистику. Столбцы должны быть указаны в порядке приоритета слева направо. Для создания гистограммы используется только первый столбец. Для статистики корреляции между столбцами, называемой плотностью, используются все столбцы.  
  
 Можно указать любой столбец, который может указываться в качестве ключевого столбца индекса, за исключением следующих столбцов.  
  
-   Нельзя указывать столбцы **xml**, полнотекстовые столбцы и столбцы FILESTREAM.  
  
-   Вычисляемые столбцы могут быть указаны только в том случае, если параметры ARITHABORT и QUOTED_IDENTIFIER базы данных имеют значение ON.  
  
-   Столбцы, имеющие определяемый пользователем тип данных CLR, могут быть указаны, если этот тип поддерживает двоичное упорядочение. Вычисляемые столбцы, определенные как вызовы методов для столбца определяемого пользователем типа, могут быть указаны, если эти методы отмечены как детерминированные.  
  
 WHERE \<filter_predicate> Указывает выражение для выбора подмножества включаемых строк при создании объекта статистики. Статистика, создаваемая с предикатом фильтра, называется отфильтрованной. Предикат фильтра использует простую логику сравнения и не может ссылаться на вычисляемый столбец, столбец определяемого пользователем типа, столбец типа пространственных данных или столбец типа **hierarchyID**. Сравнения с помощью литералов NULL с операторами сравнения недопустимы. Вместо этого используются операторы IS NULL и IS NOT NULL.  
  
 Далее приведено несколько примеров использования предикатов фильтра для таблицы Production.BillOfMaterials:  
  
 * `WHERE StartDate > '20000101' AND EndDate <= '20000630'`  
  
 * `WHERE ComponentID IN (533, 324, 753)`  
  
 * `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
  
 Дополнительные сведения о предикатах фильтра см. в статье [Создание отфильтрованных индексов](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 FULLSCAN  
 Вычисляет статистику путем сканирования всех строк. FULLSCAN и SAMPLE 100 PERCENT имеют одинаковые результаты. FULLSCAN не может быть использован с параметром SAMPLE.  
  
 Если этот параметр опущен, SQL Server использует выборку для создания статистики и определяет размер выборки, необходимый для создания плана запроса высокого качества.  
  
 SAMPLE *number* { PERCENT | ROWS }  
 Указывает приблизительное процентное соотношение или число строк в таблице или индексированном представлении для оптимизатора запросов, которые используются при создании статистики. Аргумент *number* для параметра PERCENT может иметь значение от 0 до 100, а для параметра ROWS аргумент *number* может иметь значение от 0 до общего числа строк. Фактическое процентное соотношение или число строк, отбираемых оптимизатором запросов, может не совпадать с заданным значением. Например, оптимизатор запросов просматривает все строки на странице данных.  
  
 Команда SAMPLE полезна в особых случаях, в которых план запроса на основе выборки по умолчанию не является оптимальным. В большинстве ситуаций нет необходимости указывать параметр SAMPLE, поскольку по умолчанию оптимизатор запросов применяет такие правила отбора, чтобы сформировать выборку статистически значимого размера, что необходимо для создания высококачественных планов запросов.  
  
 Параметр SAMPLE нельзя использовать вместе с параметром FULLSCAN. Если не указана ни одна из команд SAMPLE или FULLSCAN, оптимизатор запросов использует выбранные данные и вычисляет размер выборки по умолчанию.  
  
 Не рекомендуется указывать значения 0 PERCENT и 0 ROWS. Если для PERCENT или ROWS указано значение 0, объект статистики будет создан без статистических данных.  
 
 PERSIST_SAMPLE_PERCENT = { ON | OFF }  
 Если установлено значение **ON**, статистика будет сохранять заданный процент выборки для последующих обновлений, где явно не указан процент выборки. Если установлено значение **OFF**, процент выборки будет сбрасываться на значение по умолчанию при последующих обновлениях, где явно не указан процент выборки. Значение по умолчанию — **OFF**. 
 
 > [!NOTE]
 > Если таблица усечена, вся статистика, построенная на усеченном HoBT, вернется к использованию процентного соотношения выборки по умолчанию.

 **Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 CU4) и выше (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]CU1).    
  
 STATS_STREAM **=** _stats_stream_  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 NORECOMPUTE  
 Отключите автоматический параметр обновления статистики AUTO_STATISTICS_UPDATE для *statistics_name*. Если данный параметр определен, оптимизатор запросов завершит любое выполняемое обновление статистики для *statistics_name* и отключит будущие обновления.  
  
 Чтобы вновь включить обновление статистики, удалите статистику с помощью инструкции [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md), а затем выполните инструкцию CREATE STATISTICS без параметра NORECOMPUTE.  
  
> [!WARNING]  
> Использование этого параметра может привести к созданию неоптимальных планов запросов. Рекомендуется ограничить использование этого параметра, причем использовать его надлежит только опытным системным администраторам.  
  
 Дополнительные сведения о параметре AUTO_STATISTICS_UPDATE см. в разделе [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md). Дополнительные сведения об отключении и повторном включении обновления статистики см. в разделе [Статистика](../../relational-databases/statistics/statistics.md).  
  
 INCREMENTAL = { ON | OFF }  
 При значении **ON** статистики создаются как статистики отдельно по секциям. При значении **OFF** статистика для всех секций комбинируется. Значение по умолчанию — **OFF**.  
  
 Если статистики по секциям не поддерживаются, возвращается ошибка. Добавочные статистики не поддерживаются для следующих типов статистических данных.  
  
-   Статистики, созданные с индексами, не выровненными по секциям для базовой таблицы.  
-   Статистики, созданные в доступных для чтения базах данных-получателях AlwaysOn.  
-   Статистики, созданные в базах данных, доступных только для чтения.  
-   Статистики, созданные по фильтрованным индексам.  
-   Статистика, созданная по представлениям.  
-   Статистики, созданные по внутренним таблицам.  
-   Статистики, созданные с пространственными индексами или XML-индексами.  
  
**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
MAXDOP = *max_degree_of_parallelism*  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 2 (SP2) и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления 3 (CU3)).  
  
 Переопределяет параметр конфигурации **max degree of parallelism** на время выполнения операции со статистикой. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
 Параметр *max_degree_of_parallelism* может иметь одно из следующих значений:  
  
 1  
 Подавляет формирование параллельных планов.  
  
 \>1  
 Ограничивает максимальное количество процессоров, используемых в параллельных операциях со статистиками, заданным или меньшим числом в зависимости от текущей рабочей нагрузки системы.  
  
 0 (по умолчанию)  
 В зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
 \<update_stats_stream_option> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="permissions"></a>Разрешения  
 Требуются одно из указанных далее разрешений.  
  
-   ALTER TABLE  
-   Пользователь является владельцем таблицы  
-   Членство в предопределенной роли базы данных **db_securityadmin**  
  
## <a name="general-remarks"></a>Общие замечания  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может использовать базу данных tempdb для сортировки выбранных строк до создания статистики.  
  
### <a name="statistics-for-external-tables"></a>Статистика для внешних таблиц  
 При создании статистики внешней таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] импортирует внешнюю таблицу во временную таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а затем создает статистику. Для примеров статистики импортируются только строки с выборкой. При наличии большой внешней таблицы выборка по умолчанию будет работать гораздо быстрее полной проверки.  
  
### <a name="statistics-with-a-filtered-condition"></a>Статистика с отфильтрованным условием  
 Отфильтрованная статистика может повысить производительность запросов, которые выполняют выборку из четко определенных подмножеств данных. Отфильтрованная статистика использует предикат фильтра в предложении WHERE для выбора подмножества данных, включенных в статистику.  
  
### <a name="when-to-use-create-statistics"></a>Условия использования инструкции CREATE STATISTICS  
 Дополнительные сведения об условиях использования CREATE STATISTICS см. в разделе [Статистика](../../relational-databases/statistics/statistics.md).  
  
### <a name="referencing-dependencies-for-filtered-statistics"></a>Упоминаемые зависимости для отфильтрованной статистики  
 Представление каталога [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) представляет каждый столбец в предикате отфильтрованной статистики как ссылаемую зависимость. Обратите внимание на операции, которые выполняются на столбцах таблицы, созданной до создания отфильтрованной статистики, потому что определение столбца таблицы, указанного в предикате отфильтрованной статистики, невозможно удалить, переименовать или изменить.  
  
## <a name="limitations-and-restrictions"></a>Ограничения  
* Обновление статистики во внешних таблицах не поддерживается. Для обновления статистики во внешней таблице удалите и повторно создайте статистику.  
* Каждый объект статистики может содержать до 64 столбцов.
* Параметр MAXDOP несовместим с параметрами STATS_STREAM, ROWCOUNT и PAGECOUNT.
* Параметр MAXDOP ограничивается параметром MAX_DOP группы рабочей нагрузки Resource Governor (если применимо).
* Инструкции CREATE и DROP STATISTICS для внешних таблиц не поддерживаются в базе данных SQL Azure.
  
## <a name="examples"></a>Примеры  

### <a name="examples-use-the-adventureworks-database"></a>В примерах используется база данных AdventureWorks.  

### <a name="a-using-create-statistics-with-sample-number-percent"></a>A. Использование инструкции CREATE STATISTICS с аргументом «SAMPLE число PERCENT»  
 В следующем примере создается статистика `ContactMail1`, использующая случайную 5-процентную выборку из столбцов `BusinessEntityID` и `EmailPromotion` таблицы `Person` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
```  
  
### <a name="b-using-create-statistics-with-fullscan-and-norecompute"></a>Б. Использование инструкции CREATE STATISTICS с аргументами FULLSCAN и NORECOMPUTE  
 В следующем примере создается статистика `NamePurchase` по всем строкам для столбцов `BusinessEntityID` и `EmailPromotion` таблицы `Person`, при этом автоматический перерасчет статистики блокируется.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, NORECOMPUTE;  
```  
  
### <a name="c-using-create-statistics-to-create-filtered-statistics"></a>В. Создание отфильтрованной статистики с помощью инструкции CREATE STATISTICS  
 В следующем примере создается отфильтрованная статистика `ContactPromotion1`. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] просматривает 50 процентов данных, а затем выбирает все строки с `EmailPromotion`, равным 2.  
  
```sql  
CREATE STATISTICS ContactPromotion1  
    ON Person.Person (BusinessEntityID, LastName, EmailPromotion)  
WHERE EmailPromotion = 2  
WITH SAMPLE 50 PERCENT;  
GO  
```  
  
### <a name="d-create-statistics-on-an-external-table"></a>Г. Создание статистики для внешней таблицы  
 Помимо указания списка столбцов при создании статистики для внешней таблицы необходимо определить, следует ли создавать статистику с помощью выборки строк или путем сканирования всех строк. Инструкции CREATE и DROP STATISTICS для внешних таблиц не поддерживаются в базе данных SQL Azure.
  
 Поскольку [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] импортирует данные из внешней таблицы во временную таблицу для создания статистики, полное сканирование займет больше времени. Для большой таблицы обычно достаточно выполнить выборку по умолчанию.  
  
```sql  
--Create statistics on an external table and use default sampling.  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
  
--Create statistics on an external table and scan all the rows  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  

### <a name="e-using-create-statistics-with-fullscan-and-persist_sample_percent"></a>Д. Использование инструкции CREATE STATISTICS с аргументами FULLSCAN и PERSIST_SAMPLE_PERCENT  
 В приведенном ниже примере создается статистика `NamePurchase` для всех строк в столбцах `BusinessEntityID` и `EmailPromotion` таблицы `Person` и задается 100-процентная выборка для всех последующих обновлений, которые неявно указывают процент выборки.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, PERSIST_SAMPLE_PERCENT = ON;  
```  
  
### <a name="examples-using-adventureworksdw-database"></a>В примерах используется база данных AdventureWorksDW. 
  
### <a name="f-create-statistics-on-two-columns"></a>Е. Создание статистики по двум столбцам  
 В следующем примере создается статистика `CustomerStats1` на основе столбцов `CustomerKey` и `EmailAddress` таблицы `DimCustomer`. Статистика создается на базе статистически значимой выборки строк в таблице `Customer`.  
  
```sql  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
```  
  
### <a name="g-create-statistics-by-using-a-full-scan"></a>Ж. Создание статистики с помощью полной проверки  
 В следующем примере создается статистика `CustomerStatsFullScan` на основе проверки всех строк в таблице `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsFullScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  
  
### <a name="h-create-statistics-by-specifying-the-sample-percentage"></a>З. Создание статистики путем указания процента выборки  
 В следующем примере создается статистика `CustomerStatsSampleScan` на основе проверки 50 % строк в таблице `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsSampleScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH SAMPLE 50 PERCENT;  
```  
  
## <a name="see-also"></a>См. также:  
 [Статистика](../../relational-databases/statistics/statistics.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

