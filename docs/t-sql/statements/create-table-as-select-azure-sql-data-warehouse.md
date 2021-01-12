---
description: CREATE TABLE AS SELECT (Azure Synapse Analytics)
title: CREATE TABLE AS SELECT (Azure Synapse Analytics) | Документация Майкрософт
ms.custom: ''
ms.date: 10/07/2016
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: d1e08f88-64ef-4001-8a66-372249df2533
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 55b57802b33ed1e0187ba2f83f3dd9a20ffdd3c1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102358"
---
# <a name="create-table-as-select-azure-synapse-analytics"></a>CREATE TABLE AS SELECT (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

CREATE TABLE AS SELECT (CTAS) — одна из наиболее важных доступных функций T-SQL. Эта операция выполняется полностью параллельно и создает новую таблицу на основе выходных данных инструкции SELECT. CTAS позволяет быстро и легко создать копию таблицы.   
 
 CTAS можно использовать, например, для решения следующих задач:  
  
-   Повторное создание таблицы с другим столбцом распределения хэша.
-   Повторное создание таблицы в виде реплицированной таблицы.   
-   Создание индекса columnstore для всех столбцов таблицы.  
-   Отправка запроса к внешним данным или импорт внешних данных.  

> [!NOTE]  
> Поскольку с помощью CTAS можно создавать таблицы, в этом разделе мы не будем повторять содержимое раздела CREATE TABLE. Вместо этого мы опишем различия между инструкциями CTAS и CREATE TABLE. Сведения об инструкции CREATE TABLE см. в разделе [Инструкция CREATE TABLE (Azure Synapse Analytics)](./create-table-azure-sql-data-warehouse.md). 
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
<a name="syntax-bk"></a>

## <a name="syntax"></a>Синтаксис   

```syntaxsql
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    WITH ( 
      <distribution_option> -- required
      [ , <table_option> [ ,...n ] ]    
    )  
    AS <select_statement>  
    OPTION <query_hint> 
[;]  

<distribution_option> ::=
    { 
        DISTRIBUTION = HASH ( distribution_column_name ) 
      | DISTRIBUTION = ROUND_ROBIN 
      | DISTRIBUTION = REPLICATE
    }   

<table_option> ::= 
    {   
        CLUSTERED COLUMNSTORE INDEX --default for Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column[,...n])
      | HEAP --default for Parallel Data Warehouse   
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) --default is ASC 
    }  
      | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] --default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) ) 
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT select_criteria  

<query_hint> ::=
    {
        MAXDOP 
    }
```  

<a name="arguments-bk"></a>
  
## <a name="arguments"></a>Аргументы  
Дополнительные сведения см. в [подразделе "Аргументы"](./create-table-azure-sql-data-warehouse.md#Arguments) раздела, посвященного инструкции CREATE TABLE.  

<a name="column-options-bk"></a>

### <a name="column-options"></a>Параметры столбцов
`column_name` [ ,...`n` ]   
 В именах столбцов не могут использоваться [параметры столбцов](./create-table-azure-sql-data-warehouse.md#ColumnOptions), указанные в разделе, посвященном инструкции CREATE TABLE.  Вместо этого можно указать необязательный список из одного или нескольких имен столбцов для новой таблицы. Указанные имена будут использованы для столбцов в новой таблице. При указании имен столбцов число столбцов в списке столбцов должно совпадать с числом столбцов в результатах выборки. Если имена столбцов не указаны, то в новой целевой таблице будут использованы имена столбцов в результатах выборки. 
  
 Вы не можете указать другие параметры столбцов, например типы данных, параметры сортировки и допустимость значений NULL. Каждый из этих атрибутов определяется на основе результатов выполнения инструкции `SELECT`. Тем не менее инструкцию SELECT можно использовать для изменения атрибутов. Пример см. в разделе [Использование инструкции CTAS для изменения атрибутов столбца](#ctas-change-column-attributes-bk).   

<a name="table-distribution-options-bk"></a>

### <a name="table-distribution-options"></a>Параметры распространения таблицы

`DISTRIBUTION` = `HASH` ( *имя_столбца_распределения* ) | ROUND_ROBIN | REPLICATE      
Для инструкции CTAS требуется указать параметр распространения, и у этой инструкции нет параметров по умолчанию. Это отличает ее от инструкции CREATE TABLE, у которой есть параметры по умолчанию. 

Дополнительные сведения и инструкции по выбору столбца распределения см. в подразделе [Параметры распределения таблицы](./create-table-azure-sql-data-warehouse.md#TableDistributionOptions) раздела, посвященного инструкции CREATE TABLE. 

<a name="table-partition-options-bk"></a>

### <a name="table-partition-options"></a>Параметры секционирования таблицы
По умолчанию инструкция CTAS создает несекционированную таблицу, даже если исходная таблица является секционированной. Чтобы создать секционированную таблицу с помощью инструкции CTAS, необходимо указать параметр секционирования. 

Дополнительные сведения см. в подразделе [Параметры секционирования таблицы](./create-table-azure-sql-data-warehouse.md#TablePartitionOptions) раздела, посвященного инструкции CREATE TABLE.

<a name="select-options-bk"></a>

### <a name="select-statement"></a>Select, инструкция
Инструкция SELECT принципиально отличает инструкцию CTAS от инструкции CREATE TABLE.  

 `WITH` *common_table_expression*  
 Задается временно именованный результирующий набор, называемый обобщенным табличным выражением (ОТВ). Дополнительные сведения см. в разделе [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 `SELECT` *select_criteria*  
 Заполняет новую таблицу результатами выполнения инструкции SELECT. *select_criteria* являются основной частью инструкции SELECT и определяют, какие данные нужно скопировать в новую таблицу. Сведения об инструкциях SELECT см. в разделе [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md).  
 
### <a name="query-hint"></a>Указание запроса
Пользователи могут задать для параметра MAXDOP целое значение, чтобы управлять максимальной степенью параллелизма.  Если параметр MAXDOP имеет значение 1, запрос выполняется одним потоком.

 
<a name="permissions-bk"></a>  
  
## <a name="permissions"></a>Разрешения  
Инструкции CTAS требуется разрешение `SELECT` для всех объектов, указанных в *критериях_выборки*.

Разрешения на создание таблицы см. в подразделе [Разрешения](./create-table-azure-sql-data-warehouse.md#Permissions) раздела, посвященного инструкции CREATE TABLE. 
  
<a name="general-remarks-bk"></a>
  
## <a name="general-remarks"></a>Общие замечания
Дополнительные сведения см. в подразделе [Общие замечания](./create-table-azure-sql-data-warehouse.md#GeneralRemarks) раздела, посвященного инструкции CREATE TABLE.

<a name="limitations-bk"></a>

## <a name="limitations-and-restrictions"></a>Ограничения  

Упорядоченный кластеризованный индекс columnstore можно создавать для столбцов любых типов данных, поддерживаемых в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], за исключением строковых столбцов.  

[SET ROWCOUNT (Transact-SQL)](../../t-sql/statements/set-rowcount-transact-sql.md) не влияет на инструкцию CTAS. Чтобы обеспечить аналогичное поведение, воспользуйтесь инструкцией [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md).  
 
Дополнительные сведения см. в подразделе [Ограничения](./create-table-azure-sql-data-warehouse.md#LimitationsRestrictions) раздела, посвященного инструкции CREATE TABLE.

<a name="locking-behavior-bk"></a>
  
## <a name="locking-behavior"></a>Режим блокировки  
 Дополнительные сведения см. в подразделе [Режим блокировки](./create-table-azure-sql-data-warehouse.md#LockingBehavior) раздела, посвященного инструкции CREATE TABLE.
 
<a name="performance-bk"></a>
 
 ## <a name="performance"></a>Производительность 

Для таблиц с распределенным хэшем с помощью инструкции CTAS можно выбрать другой столбец распределения, что позволит повысить производительность операций соединения и агрегатов. Если выбор другого столбца распределения не является вашей целью, то для достижения наилучшей производительности CTAS укажите тот же столбец распределения, так как это позволит избежать повторного распределения строк. 

Если вы используете инструкцию CTAS для создания таблицы и производительность не имеет значения, вы можете указать `ROUND_ROBIN`, чтобы не выбирать столбец распределения.

Чтобы избежать перемещения данных в последующих запросах, можно указать `REPLICATE`, что приведет к увеличению размера хранилища для загрузки полной копии таблицы на каждом вычислительном узле.  


<a name="examples-copy-table-bk"></a>

## <a name="examples-for-copying-a-table"></a>Примеры копирования таблицы

<a name="ctas-copy-table-bk"></a>

### <a name="a-use-ctas-to-copy-a-table"></a>A. Использование инструкции CTAS для копирования таблицы 
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Одно из наиболее частых применений `CTAS` — создание копии таблицы для изменения DDL. Например, если вы изначально создали таблицу как `ROUND_ROBIN` и теперь хотите изменить ее на таблицу с распределением по столбцу, вы можете изменить столбец распределения с помощью `CTAS`. `CTAS` также может использоваться для изменения секционирования, индексирования и типов столбцов.

Предположим, что вы создали эту таблицу с использованием типа распределения по умолчанию `ROUND_ROBIN`, так как в `CREATE TABLE` не был указан столбец распределения.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey INT NOT NULL,
    OrderDateKey INT NOT NULL,
    DueDateKey INT NOT NULL,
    ShipDateKey INT NOT NULL,
    CustomerKey INT NOT NULL,
    PromotionKey INT NOT NULL,
    CurrencyKey INT NOT NULL,
    SalesTerritoryKey INT NOT NULL,
    SalesOrderNumber NVARCHAR(20) NOT NULL,
    SalesOrderLineNumber TINYINT NOT NULL,
    RevisionNumber TINYINT NOT NULL,
    OrderQuantity SMALLINT NOT NULL,
    UnitPrice MONEY NOT NULL,
    ExtendedAmount MONEY NOT NULL,
    UnitPriceDiscountPct FLOAT NOT NULL,
    DiscountAmount FLOAT NOT NULL,
    ProductStandardCost MONEY NOT NULL,
    TotalProductCost MONEY NOT NULL,
    SalesAmount MONEY NOT NULL,
    TaxAmt MONEY NOT NULL,
    Freight MONEY NOT NULL,
    CarrierTrackingNumber NVARCHAR(25),
    CustomerPONumber NVARCHAR(25)
);
```

Теперь вы хотите создать новую копию этой таблицы с кластеризованным индексом columnstore, чтобы получить преимущества производительности таблиц с кластеризованными индексами columnstore. Вы также хотите распределить эту таблицу по столбцу ProductKey, так как к этому столбцу будет применяться операция соединения и вы хотите избежать перемещения данных при соединении по столбцу ProductKey. Наконец, вы также хотите добавить секционирование по столбцу OrderDateKey, чтобы быстро удалять старые данные путем удаления старых секций. Ниже приведена инструкция CTAS, которая скопирует старую таблицу в новую.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Наконец, вы можете переименовать таблицы, чтобы поместить старую таблицу на место новой и удалить старую.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

<a name="examples-column-bk"></a>

## <a name="examples-for-column-options"></a>Примеры параметров столбцов

<a name="ctas-change-column-attributes-bk"></a>

### <a name="b-use-ctas-to-change-column-attributes"></a>Б. Использование инструкции CTAS для изменения атрибутов столбца 
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В этом примере инструкция CTAS используется для изменения типов данных, допустимости значений NULL и параметров сортировки для нескольких столбцов в таблице DimCustomer2.  
  
```sql  
-- Original table 
CREATE TABLE [dbo].[DimCustomer2] (  
    [CustomerKey] INT NOT NULL,  
    [GeographyKey] INT NULL,  
    [CustomerAlternateKey] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL  
)  
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerKey]));  
  
-- CTAS example to change data types, nullability, and column collations  
CREATE TABLE test  
WITH (HEAP, DISTRIBUTION = ROUND_ROBIN)  
AS  
SELECT  
    CustomerKey AS CustomerKeyNoChange,  
    CustomerKey*1 AS CustomerKeyChangeNullable,  
    CAST(CustomerKey AS DECIMAL(10,2)) AS CustomerKeyChangeDataTypeNullable,  
    ISNULL(CAST(CustomerKey AS DECIMAL(10,2)),0) AS CustomerKeyChangeDataTypeNotNullable,  
    GeographyKey AS GeographyKeyNoChange,  
    ISNULL(GeographyKey,0) AS GeographyKeyChangeNotNullable,  
    CustomerAlternateKey AS CustomerAlternateKeyNoChange,  
    CASE WHEN CustomerAlternateKey = CustomerAlternateKey 
        THEN CustomerAlternateKey END AS CustomerAlternateKeyNullable,  
    CustomerAlternateKey COLLATE Latin1_General_CS_AS_KS_WS AS CustomerAlternateKeyChangeCollation  
FROM [dbo].[DimCustomer2]  
  
-- Resulting table 
CREATE TABLE [dbo].[test] (
    [CustomerKeyNoChange] INT NOT NULL, 
    [CustomerKeyChangeNullable] INT NULL, 
    [CustomerKeyChangeDataTypeNullable] DECIMAL(10, 2) NULL, 
    [CustomerKeyChangeDataTypeNotNullable] DECIMAL(10, 2) NOT NULL, 
    [GeographyKeyNoChange] INT NULL, 
    [GeographyKeyChangeNotNullable] INT NOT NULL, 
    [CustomerAlternateKeyNoChange] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL, 
    [CustomerAlternateKeyNullable] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [CustomerAlternateKeyChangeCollation] NVARCHAR(15) COLLATE Latin1_General_CS_AS_KS_WS NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN);
```
 
В качестве последнего шага можно изменить имена таблиц с помощью инструкции [RENAME (Transact-SQL)](../../t-sql/statements/rename-transact-sql.md). После этого DimCustomer2 станет новой таблицей.

```sql
RENAME OBJECT DimCustomer2 TO DimCustomer2_old;
RENAME OBJECT test TO DimCustomer2;

DROP TABLE DimCustomer2_old;
```

<a name="examples-table-distribution-bk"></a>

## <a name="examples-for-table-distribution"></a>Примеры распределения таблиц

<a name="ctas-change-distribution-method-bk"></a>

### <a name="c-use-ctas-to-change-the-distribution-method-for-a-table"></a>В. Использование инструкции CTAS для изменения метода распределения таблицы
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В этом простом примере показано, как изменить метод распределения таблицы. Для иллюстрации механизма изменения в этом примере таблица с распределенным хэшем изменяется на таблицу с циклическим распределением и затем обратно на таблицу с распределенным хэшем. Окончательная таблица совпадает с исходной таблицей. 

В большинстве случаев изменять таблицу с распределенным хэшем на таблицу с циклическим распределением не требуется. Чаще требуется изменить таблицу с циклическим распределением на таблицу с распределенным хэшем. Например, вы можете изначально загрузить новую таблицу как таблицу с циклическим распределением и затем изменить ее на таблицу с распределенным хэшем для повышения производительности операций соединения.

В этом примере используется образец базы данных AdventureWorksDW. Сведения о том, как загрузить версию [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], см. в статье [Загрузка примера данных в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-load-sample-databases/)
 
```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a round-robin table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  
Затем измените таблицу обратно на таблицу с распределенным хэшем.

```sql
-- You just made DimSalesTerritory a round-robin table.
-- Change it back to the original hash-distributed table. 
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH(SalesTerritoryKey) 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
<a name="ctas-change-to-replicated-bk"></a>

### <a name="d-use-ctas-to-convert-a-table-to-a-replicated-table"></a>Г. Использование инструкции CTAS для преобразования таблицы в реплицированную таблицу  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Этот пример относится к преобразованию таблицы с циклическим распределением или таблицы с распределенным хэшем в реплицированную таблицу. В этом примере предыдущий способ изменения типа распределения продвигается на один шаг дальше.  Поскольку DimSalesTerritory является измерением и скорее всего таблицей меньшего размера, вы можете создать таблицу повторно в виде реплицированной таблицы, чтобы избежать перемещения данных при соединении с другими таблицами. 

```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a replicated table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
### <a name="e-use-ctas-to-create-a-table-with-fewer-columns"></a>Д. Использование инструкции CTAS для создания таблицы с меньшим количеством столбцов
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В следующем примере создается таблица `myTable (c, ln)` с циклическим распределением. Новая таблица содержит только два столбца. В этом примере в инструкции SELECT в качестве имен столбцов используются псевдонимы столбцов.  
  
```sql  
CREATE TABLE myTable  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT CustomerKey AS c, LastName AS ln  
    FROM dimCustomer; 
```  

<a name="examples-query-hints-bk"></a>

## <a name="examples-for-query-hints"></a>Примеры указания запросов

<a name="ctas-query-hint-bk"></a>

### <a name="f-use-a-query-hint-with-create-table-as-select-ctas"></a>Е. Использование указания запроса с инструкцией CREATE TABLE AS SELECT (CTAS)  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
В этом запросе показан базовый синтаксис для указания запроса на соединение с инструкцией CTAS. После отправки запроса [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] применяет стратегию хэш-соединения при создании плана запроса для каждого отдельного распределения. Дополнительные сведения об указании запроса на хэш-соединение см. в разделе [Предложение OPTION (Transact-SQL)](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql  
CREATE TABLE dbo.FactInternetSalesNew  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN   
  )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```  

<a name="examples-external-tables"></a>

## <a name="examples-for-external-tables"></a>Примеры внешних таблиц

<a name="ctas-azure-blob-storage-bk"></a>

### <a name="g-use-ctas-to-import-data-from-azure-blob-storage"></a>Ж. Использование инструкции CTAS для импорта данных из хранилища BLOB-объектов Azure  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Чтобы импортировать данные из внешней таблицы, используйте инструкцию CREATE TABLE AS SELECT для выбора данных из внешней таблицы. Синтаксис для выбора данных из внешней таблицы в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] является таким же, как синтаксис для выбора данных из обычной таблицы.  
  
 В следующем примере определяется внешняя таблица для данных в учетной записи хранилища BLOB-объектов Azure. Затем используется инструкция CREATE TABLE AS SELECT для выбора данных из внешней таблицы. Она импортирует данные из текстовых файлов с разделителями хранилища BLOB-объектов Azure и сохраняет данные в новой таблице [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
```sql  
--Use your own processes to create the text-delimited files on Azure blob storage.  
--Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION='/logs/clickstream/2015/',  
    DATA_SOURCE = MyAzureStorage,  
    FILE_FORMAT = TextFileFormat)  
;  
  
--Use CREATE TABLE AS SELECT to import the Azure blob storage data into a new   
--Synapse Analytics table called ClickStreamData  
CREATE TABLE ClickStreamData   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;  
```  

<a name="ctas-import-Hadoop-bk"></a>
  
### <a name="h-use-ctas-to-import-hadoop-data-from-an-external-table"></a>З. Использование инструкции CTAS для импорта данных Hadoop из внешней таблицы  
Применимо к: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
Чтобы импортировать данные из внешней таблицы, используйте инструкцию CREATE TABLE AS SELECT для выбора данных из внешней таблицы. Синтаксис для выбора данных из внешней таблицы в [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] является таким же, как синтаксис для выбора данных из обычной таблицы.  
  
 В следующем примере определяется внешняя таблица в кластере Hadoop. Затем используется инструкция CREATE TABLE AS SELECT для выбора данных из внешней таблицы. Она импортирует данные из текстовых файлов с разделителями Hadoop и сохраняет данные в новую таблицу [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
```sql  
-- Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION = 'hdfs://MyHadoop:5000/tpch1GB/employee.tbl',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|')  
)  
;  
  
-- Use your own processes to create the Hadoop text-delimited files 
-- on the Hadoop Cluster.  
  
-- Use CREATE TABLE AS SELECT to import the Hadoop data into a new 
-- table called ClickStreamPDW  
CREATE TABLE ClickStreamPDW   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;   
```  
 
<a name="examples-workarounds-bk"></a>
 
## <a name="examples-using-ctas-to-replace-sql-server-code"></a>Примеры использования инструкции CTAS для замены кода SQL Server

Используйте инструкцию CTAS, чтобы обойти некоторые неподдерживаемые функции. Наряду с возможностью запуска кода в хранилище данных переработка существующего кода для использования инструкции CTAS обычно повышает производительность. Это является результатом полностью параллельной структуры инструкции CTAS. 

> [!NOTE]
> Попробуйте решить задачу "в первую очередь с инструкцией CTAS". Если вы считаете, что можете решить задачу с помощью `CTAS`, то обычно это лучший способ ее решения — даже если в результате будут созданы дополнительные данные.
>

<a name="ctas-replace-select-into-bk"></a>

### <a name="i-use-ctas-instead-of-selectinto"></a>И. Использование инструкции CTAS вместо SELECT..INTO  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В коде SQL Server для заполнения таблицы результатами выполнения инструкции SELECT обычно используется SELECT..INTO. Ниже приведен пример использования инструкции SQL Server SELECT..INTO.

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Этот синтаксис не поддерживается в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и Parallel Data Warehouse. В этом примере показано, как переписать предыдущую инструкцию SELECT..INTO в виде инструкции CTAS. Вы можете выбрать любой из параметров DISTRIBUTION, описанных в синтаксисе CTAS. В этом примере используется метод распределения ROUND_ROBIN.

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<a name="ctas-replace-implicit-joins-bk"></a>

### <a name="j-use-ctas-and-implicit-joins-to-replace-ansi-joins-in-the-from-clause-of-an-update-statement"></a>К. Использование инструкции CTAS и неявного соединения для замены соединения ANSI в предложении `FROM` инструкции `UPDATE`  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Может оказаться, что у вас есть сложная операция обновления, которая соединяет вместе несколько таблиц с использованием синтаксиса соединения ANSI для выполнения операции UPDATE или DELETE.

Представьте, что вам необходимо обновить следующую таблицу:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]          SMALLINT    NOT NULL
,   [TotalSalesAmount]      MONEY       NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Исходный запрос может выглядеть примерно так:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Так как [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] не поддерживает соединения ANSI в предложении `FROM` инструкции `UPDATE`, потребуется немного изменить этот код SQL Server.

Чтобы заменить этот код, можно использовать сочетание `CTAS` и неявного соединения:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

<a name="ctas-replace-ansi-joins-bk"></a>

### <a name="k-use-ctas-to-specify-which-data-to-keep-instead-of-using-ansi-joins-in-the-from-clause-of-a-delete-statement"></a>Л. Использование инструкции CTAS, чтобы указать, какие данные следует сохранить вместо использования соединений ANSI в предложении FROM инструкции DELETE  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Иногда для удаления данных лучше всего использовать `CTAS`. Вместо удаления данных просто выберите данные, которые требуется сохранить. Это особенно справедливо для инструкций `DELETE`, в которых используется синтаксис соединения ANSI, так как [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] не поддерживает соединения ANSI в предложении `FROM` инструкции `DELETE`.

Пример преобразованной инструкции DELETE приведен ниже:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

<a name="ctas-simplify-merge-bk"></a>

### <a name="l-use-ctas-to-simplify-merge-statements"></a>М. Использование инструкции CTAS для упрощения инструкций слияния  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Операторы слияния, по крайней мере, частично могут быть заменены на `CTAS`. Вы можете объединить `INSERT` и `UPDATE` в одну инструкцию. Любые удаленные записи потребуется включить во вторую инструкцию.

Пример `UPSERT` приведен ниже:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];

```

<a name="ctas-data-type-and-nullability-bk"></a>

### <a name="m-explicitly-state-data-type-and-nullability-of-output"></a>Н. явно указывайте тип данных и допустимость нулевого результата.  
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

При переносе кода SQL Server в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] вы можете столкнуться со следующим шаблоном написания кода:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Инстинктивно вы можете подумать, что этот код следует перенести в CTAS, и будете правы. Однако здесь есть скрытая проблема.

Следующий код не позволяет получить тот же самый результат:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Обратите внимание, что столбец "result" переносит тип данных и допустимость значений NULL для выражения. Это может привести к некоторым различиям в значениях, если не соблюдать осторожность.

Попробуйте следующий код в качестве примера:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Значение, сохраненное для результата, отличается. Так как сохраненное значение в столбце результатов используется в других выражениях, ошибка становится еще более значительной.

![Результаты выполнения инструкции CREATE TABLE AS SELECT](../../t-sql/statements/media/create-table-as-select-results.png)

Это особенно важно для перемещения данных. Несмотря на то, что второй запрос является более точным, здесь есть проблема. Данные будут отличаться по сравнению с исходной системой, и это приведет к вопросам целостности при миграции. Это один из тех редких случаев, когда "неправильный" ответ на самом деле является правильным!

Причина несоответствия между двумя результатами заключается в неявном преобразовании типов. В первом примере в таблице приводится определение столбца. При вставке строки происходит неявное преобразование типов. Во втором примере отсутствует неявное преобразование типов, так как выражение определяет тип данных столбца. Обратите внимание, во втором примере в столбце допускаются значения NULL, а в первом примере — нет. При создании таблицы в первом примере была явно указана допустимость значений NULL для столбца. Во втором примере допустимость значений NULL определялась с помощью выражения, и по умолчанию это привело бы к определению, равному NULL.  

Для устранения этих проблем необходимо явно задать преобразование типов и допустимость значений NULL в части `SELECT` инструкции `CTAS`. Эти свойства невозможно установить в части создания таблицы.

В приведенном ниже примере показано, как исправить код:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Следует отметить следующее.
- Можно было бы использовать CAST или CONVERT
- Функция ISNULL используется для принудительной установки допустимости значений NULL, а не для COALESCE
- Функция ISNULL является внешней функцией
- Вторая часть функции ISNULL является константой, например 0

> [!NOTE]
> Для правильной установки допустимости значений NULL обязательно используйте `ISNULL`, а не `COALESCE`. `COALESCE` не является детерминированной функцией, и поэтому результат выражения всегда может иметь значение NULL. Функция `ISNULL` отличается от нее. Она является детерминированной. Поэтому когда вторая часть функции `ISNULL` является константой или литералом, результирующим значением будет NOT NULL.

Этот совет помогает не только обеспечить целостность вычислений. Он также важен для переключения секций таблицы. Представьте, что в качестве факта определена следующая таблица:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Однако значение поля является вычисляемым выражением и не является частью исходных данных.

Для создания набора секционированных данных может потребоваться сделать следующее:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Запрос выполняется без ошибок. Проблема возникает при попытке выполнить переключение секций. Определения таблиц не совпадают. Чтобы определения таблиц совпадали, необходимо изменить инструкцию CTAS.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Таким образом, рекомендуется использовать инструкцию CTAS для обеспечения согласованности типов и поддержания допустимости значений NULL. Это помогает поддерживать целостность вычислений и обеспечивает возможность переключения секций.

### <a name="n-create-an-ordered-clustered-columnstore-index-with-maxdop-1"></a>О. Создание упорядоченного кластеризованного индекса columnstore при помощи MAXDOP 1  
```sql
CREATE TABLE Table1 WITH (DISTRIBUTION = HASH(c1), CLUSTERED COLUMNSTORE INDEX ORDER(c1) )
AS SELECT * FROM ExampleTable
OPTION (MAXDOP 1);
```

 
## <a name="see-also"></a>См. также:  
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-azure-sql-data-warehouse.md) [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [DROP EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/drop-external-table-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER EXTERNAL TABLE (Transact-SQL)](./create-external-table-transact-sql.md)  
