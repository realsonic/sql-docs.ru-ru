---
description: CREATE COLUMNSTORE INDEX (Transact-SQL)
title: CREATE COLUMNSTORE INDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE INDEX
- COLUMNSTORE_INDEX_TSQL
- CREATE CLUSTERED COLUMNSTORE INDEX
- COLUMNSTORE_TSQL
- CREATE NONCLUSTERED COLUMNSTORE INDEX
- CREATE_NONCLUSTERED_COLUMNSTORE_INDEX_TSQL
- CREATE COLUMNSTORE INDEX
- CREATE_CLUSTERED_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE
dev_langs:
- TSQL
helpviewer_keywords:
- index creation [SQL Server], columnstore indexes
- columnstore index, creating
- CREATE COLUMNSTORE INDEX statement
- CREATE INDEX statement
ms.assetid: 7e1793b3-5383-4e3d-8cef-027c0c8cb5b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b650e0fa1d1d6c02e657d3d5860908164ed5cd91
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099580"
---
# <a name="create-columnstore-index-transact-sql"></a>CREATE COLUMNSTORE INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Преобразование таблицы rowstore в кластеризованный индекс columnstore или создание некластеризованного индекса columnstore. Используйте индекс columnstore для эффективного выполнения операционной аналитики в реальном времени в рабочей нагрузке OLTP или повышения производительности сжатия данных и запросов для рабочих нагрузок хранилищ данных.  
  
> [!NOTE]
> Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] таблицы можно создавать как кластеризованный индекс columnstore.   Больше не нужно сначала создавать таблицу rowstore, а затем преобразовывать ее в кластеризованный индекс columnstore.  

> [!TIP]
> Дополнительные сведения о правилах проектирования индексов см. в статье [Руководство по проектированию индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md).

Примеры:  
-   [Примеры преобразования таблицы rowstore в columnstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#convert)  
-   [Примеры некластеризованных индексов columnstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#nonclustered)  
  
Сценарии:  
-   [Индексы columnstore для получения операционной аналитики в реальном времени](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
-   [Индексы сolumnstore для хранилищ данных](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
  
Дополнительные сведения:  
-   [Руководство по индексам columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)  
-   [Сводка функций индексов columnstore](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create a clustered columnstore index on disk-based table.  
CREATE CLUSTERED COLUMNSTORE INDEX index_name  
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ] 
[ ; ]  
  
--Create a nonclustered columnstore index on a disk-based table.  
CREATE [NONCLUSTERED]  COLUMNSTORE INDEX index_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
        ( column  [ ,...n ] )  
    [ WHERE <filter_expression> [ AND <filter_expression> ] ]
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ]
[ ; ]  
  
<with_option> ::=  
      DROP_EXISTING = { ON | OFF } -- default is OFF  
    | MAXDOP = max_degree_of_parallelism 
    | ONLINE = { ON | OFF } 
    | COMPRESSION_DELAY  = { 0 | delay [ Minutes ] }  
    | DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { partition_number_expression | range } [ ,...n ] ) ]  
  
<on_option>::=  
      partition_scheme_name ( column_name )
    | filegroup_name
    | "default"
  
<filter_expression> ::=  
      column_name IN ( constant [ ,...n ]  
    | column_name { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< } constant  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    [ORDER (column [,...n] ) ]  
    [ WITH ( DROP_EXISTING = { ON | OFF } ) ] --default is OFF  
[;]  

```
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

Некоторые параметры доступны не во всех версиях ядра СУБД. В следующей таблице показаны версии, в которых параметры добавлены в индексы CLUSTERED COLUMNSTORE и NONCLUSTERED COLUMNSTORE:

|Параметр| CLUSTERED | NONCLUSTERED |
|---|---|---|
| COMPRESSION_DELAY | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] |
| DATA_COMPRESSION | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] | 
| ONLINE | [!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)] | [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] |
| WHERE, предложение | Недоступно | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] |

Все параметры доступны в Базе данных SQL Azure.

### <a name="create-clustered-columnstore-index"></a>СОЗДАНИЕ КЛАСТЕРИЗОВАННОГО ИНДЕКСА COLUMNSTORE

Создание кластеризованного индекса columnstore, в котором все данные сжаты и сохранены по столбцам. Индекс включает все столбцы в таблице и сохраняет всю таблицу. Если существующая таблица является кучей или кластеризованным индексом, она преобразуется в кластеризованный индекс columnstore. Если таблица уже хранится в виде кластеризованного индекса columnstore, то существующий индекс будет удален и перестроен.  
  
*index_name*  
Задает имя нового индекса.  
  
Если таблица уже является кластеризованным индексом columnstore, вы можете указать то же имя, что у существующего индекса, или задать новое имя с помощью параметра DROP EXISTING.  
  
ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*

Задает одно-, двух- или трехкомпонентное имя таблицы, которая должна быть сохранена как кластеризованный индекс columnstore. Если таблица является кучей или кластеризованным индексом, она преобразуется из rowstore в columnstore. Если таблица уже columnstore, эта инструкция перестраивает кластеризованный индекс columnstore. Для преобразования в упорядоченный кластеризованный индекс columnstore существующий индекс должен быть кластеризованным индексом columnstore.
  
#### <a name="with-options"></a>Параметры WITH

##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON

   `DROP_EXISTING = ON` удаляет существующий индекс и создает индекс columnstore.  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (DROP_EXISTING = ON);
```
   По умолчанию DROP_EXISTING = OFF ожидает, что имя индекса совпадает с существующим именем. Если индекс с указанным именем уже существует, выдается ошибка.  
  
##### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Переопределяет существующую конфигурацию максимальной степени параллелизма сервера на время выполнения этой операции с индексом. MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
   Значения *max_degree_of_parallelism* могут быть следующими:  
   - 1 — подавляет создание параллельных планов.  
   - \>1 — ограничивает максимальное число процессоров, используемых в параллельных операциях с индексами, заданным или меньшим числом в зависимости от текущей рабочей нагрузки системы. Например, если MAXDOP = 4, то число используемых процессоров будет равно 4 или меньше.  
   - 0 (по умолчанию) — в зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (MAXDOP = 2);
```

   Дополнительные сведения см. в статьях [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) и [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
 
###### <a name="compression_delay--0--delay--minutes-"></a>COMPRESSION_DELAY = **0** | *delay* [ мин ]  
   Для таблицы на основе диска значение *delay* указывает минимальное количество минут, в течение которых разностная группа строк в состоянии CLOSED должна оставаться в разностной группе строк до того, как SQL Server сожмет ее в сжатую группу строк. Поскольку таблицы на основе диска не отслеживают время вставки и обновления отдельных строк, SQL Server применяет задержку к разностным группам строк в состоянии CLOSED.  
   Значение по умолчанию — 0 минут.  
   
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( COMPRESSION_DELAY = 10 Minutes );
```

   Рекомендации по использованию COMPRESSION_DELAY см. в разделе [Начало работы с columnstore для получения операционной аналитики в реальном времени](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
##### <a name="data_compression--columnstore--columnstore_archive"></a>DATA_COMPRESSION = COLUMNSTORE | COLUMNSTORE_ARCHIVE  
   Задает режим сжатия данных для указанной таблицы, номера секции или диапазона секций. Существуют следующие варианты выбора.   
- `COLUMNSTORE` является значением по умолчанию и задает сжатие с использованием самого эффективного сжатия columnstore. Это обычный вариант.  
- Параметр `COLUMNSTORE_ARCHIVE` обеспечивает дальнейшее сжатие таблицы или секции до еще меньшего размера. Этот параметр можно использовать в ситуациях, где требуется уменьшение размера хранилища и допускается увеличение затрат времени на сохранение и выборку.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( DATA_COMPRESSION = COLUMNSTORE_ARCHIVE );
```
   Дополнительные сведения о сжатии см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).  

###### <a name="online--on--off"></a>ONLINE = [ON | OFF]
- Значение `ON` указывает, что индекс columnstore остается в сети и доступен во время создания новой копии индекса.
- Значение `OFF` указывает, что индекс недоступен для использования во время создания новой копии.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( ONLINE = ON );
```

#### <a name="on-options"></a>Параметры ON 
   Параметры ON позволяют задавать параметры для хранения данных, такие как схема секционирования, конкретная файловая группа или файловая группа по умолчанию. Если параметр ON не задан, индекс использует параметры секционирования или параметры файловой группы существующей таблицы.  
  
   *имя_схемы_секционирования* **(** _имя_столбца_ **)**  
   Задает схему секционирования для таблицы. Эта схема секционирования должна уже существовать в базе данных. Описание создания схемы секционирования см. в разделе [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
 
   *column_name* указывает столбец, по которому будет секционирован индекс. Столбец должен соответствовать по типу данных, длине и точности аргументу функции секционирования, используемой аргументом *partition_scheme_name*.  

   *filegroup_name*  
   Указывает файловую группу для хранения кластеризованного индекса columnstore. Если местоположение не указано и таблица не секционирована, индекс использует ту же файловую группу, что и базовая таблица или представление. Файловая группа должна существовать.  

   **"** default **"**  
   Чтобы создать индекс для файловой группы по умолчанию, используйте значение default или [ default ].  
  
   Если указано значение "default" (по умолчанию), параметр QUOTED_IDENTIFIER должен иметь значение ON для текущего сеанса. QUOTED_IDENTIFIER по умолчанию равен ON. Дополнительные сведения см. в статье [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
### <a name="create-nonclustered-columnstore-index"></a>CREATE [NONCLUSTERED] COLUMNSTORE INDEX  
Создайте выполняемый в памяти некластеризованный индекс columnstore в таблице rowstore, хранимой в виде кучи или кластеризованного индекса. Индекс может иметь условие фильтрации и не должен включать все столбцы базовой таблицы. Индекс columnstore требует достаточно места для хранения копии данных. Он может обновляться и обновляется по мере изменения базовой таблицы. Некластеризованный индекс columnstore в кластеризованном индексе допускает аналитику в реальном времени.  
  
*index_name*  
   Указывает имя индекса. Значение *index_name* должно быть уникальным в пределах таблицы, но необязательно должно быть уникальным в пределах базы данных. Имена индексов должны удовлетворять правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
 **(** _столбец_  [ **,** ...*n* ] **)**  
    Задает столбцы для хранения. Некластеризованный индекс columnstore может включать не более 1024 столбцов.  
   Каждый столбец должен иметь поддерживаемый тип данных для индексов columnstore. В разделе [Ограничения](../../t-sql/statements/create-columnstore-index-transact-sql.md#LimitRest) приводится список поддерживаемых типов данных.  

ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*  
   Указывает одно-, двух- или трехкомпонентное имя таблицы, которая содержит индекс.  

#### <a name="with-options"></a>Параметры WITH
##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON  
   DROP_EXISTING = ON Существующий индекс удаляется и перестраивается. Указанное имя индекса должно совпадать с уже существующим индексом, но определение индекса может быть изменено. Например, можно указать другие столбцы или параметры индекса.
  
   DROP_EXISTING = OFF Выдается ошибка, если индекс с указанным именем уже существует. Тип индекса не может быть изменен с помощью аргумента DROP_EXISTING. Для обратной совместимости синтаксиса аргумент WITH DROP_EXISTING эквивалентен аргументу WITH DROP_EXISTING = ON.  

###### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Переопределяет параметр конфигурации сервера [Максимальная степень параллелизма](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) на время выполнения операции с индексами. MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
   Значения *max_degree_of_parallelism* могут быть следующими:  
   - 1 — подавляет создание параллельных планов.  
   - \>1 — ограничивает максимальное число процессоров, используемых в параллельных операциях с индексами, заданным или меньшим числом в зависимости от текущей рабочей нагрузки системы. Например, если MAXDOP = 4, то число используемых процессоров будет равно 4 или меньше.  
   - 0 (по умолчанию) — в зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
   Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[msC](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
###### <a name="online--on--off"></a>ONLINE = [ON | OFF]   
- Значение `ON` указывает, что индекс columnstore остается в сети и доступен во время создания новой копии индекса.
- Значение `OFF` указывает, что индекс недоступен для использования во время создания новой копии. В некластеризованных индексах базовая таблица остается доступной. Только некластеризованный индекс columnstore не используется для обработки запросов до создания индекса. 

```sql
CREATE COLUMNSTORE INDEX ncci ON Sales.OrderLines (StockItemID, Quantity, UnitPrice, TaxRate) WITH ( ONLINE = ON );
```

##### <a name="compression_delay--0--delayminutes"></a>COMPRESSION_DELAY = **0** | \<delay>[минут]  
   Задает нижнюю границу периода, в течение которого строка должна оставаться в разностной группе строк, прежде чем сможет переместиться в сжатую группу строк. Например, клиент может запросить возможность сжатия строки в формат хранения по столбцам, если она остается неизменной в течение 120 минут. Для индекса columnstore в таблицах на диске мы не отслеживаем время вставки или обновления строки, вместо этого мы используем время закрытия разностной группы строк для получения сведений о строке. Значение по умолчанию — 0 минут. Строка переносится в хранилище по столбцам, когда в разностной группе строк накопится 1 миллион строк и она будет помечена как закрытая.  
  
###### <a name="data_compression"></a>DATA_COMPRESSION  
   Задает режим сжатия данных для указанной таблицы, номера секции или диапазона секций. Применяется только к индексам columnstore, включая некластеризованные и кластеризованные индексы columnstore. Существуют следующие варианты выбора.
   
- `COLUMNSTORE` является значением по умолчанию и задает сжатие с использованием самого эффективного сжатия columnstore. Это обычный вариант.  
- Параметр `COLUMNSTORE_ARCHIVE` обеспечивает дальнейшее сжатие таблицы или секции до еще меньшего размера. Это может использоваться для архивации или в других ситуациях, где требуется уменьшение объема пространства и допускается увеличение затрат времени на сохранение и выборку  
  
 Дополнительные сведения о сжатии см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).  
  
##### <a name="where-filter_expression--and-filter_expression-"></a>WHERE \<filter_expression> [ AND \<filter_expression> ]
  
   Называется предикатом фильтра и задает строки для включения в индекс. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает отфильтрованную статистику для строк данных отфильтрованного индекса.  
  
   Предикат фильтра использует простую логику сравнения. Сравнения с помощью литералов NULL с операторами сравнения недопустимы. Вместо этого используются операторы IS NULL и IS NOT NULL.  
  
   Далее приведено несколько примеров использования предикатов фильтра для таблицы `Production.BillOfMaterials`:  
   `WHERE StartDate > '20000101' AND EndDate <= '20000630'`    
   `WHERE ComponentID IN (533, 324, 753)`  
   `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
   
   Рекомендации по отфильтрованным индексам см. в статье [Создание отфильтрованных индексов](../../relational-databases/indexes/create-filtered-indexes.md).  
  
#### <a name="on-options"></a>Параметры ON  
   Эти параметры указывают файловые группы, для которых создается индекс.  
  
*имя_схемы_секционирования* **(** _имя_столбца_ **)**  
   Задает схему секционирования, определяющую файловые группы, по которым сопоставляются секции секционированного индекса. Схема секционирования должна быть создана в базе данных путем выполнения инструкции [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md). 
   *column_name* указывает столбец, по которому будет секционирован индекс. Столбец должен соответствовать по типу данных, длине и точности аргументу функции секционирования, используемой аргументом *partition_scheme_name*. Аргумент *column_name* необязательно должен соответствовать столбцам из определения индекса. При секционировании индекса columnstore компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] добавляет столбец секционирования как столбец индекса, если этого столбца еще нет в списке.  
   Если аргумент *partition_scheme_name* или *filegroup* не задан и таблица секционирована, индекс помещается в ту же схему секционирования и с тем же столбцом секционирования, что и для базовой таблицы.  
   Индекс columnstore для секционированной таблицы должен быть выровнен по секциям.  
   Дополнительные сведения о секционировании индексов см. в разделе [Секционированные таблицы и индексы](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  

*filegroup_name*  
   Указывает имя файловой группы, в которой создается индекс. Если имя *filegroup_name* не указано и таблица не секционирована, то индекс использует ту же файловую группу, что и базовая таблица. Файловая группа должна существовать.  
 
**"** default **"**  
Создает заданный индекс в файловой группе, используемой по умолчанию.  
  
Слово "default" в этом контексте не является ключевым. Это идентификатор установленной по умолчанию файловой группы, который должен иметь разделители, как в выражениях ON **"** default **"** или ON **[** default **]** . Если указано значение "default" (по умолчанию), параметр QUOTED_IDENTIFIER должен иметь значение ON для текущего сеанса. Это параметр по умолчанию. Дополнительные сведения см. в статье [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется разрешение ALTER на таблицу.  
  
##  <a name="general-remarks"></a><a name="GenRemarks"></a> Общие замечания  
Индекс columnstore может создаваться для временной таблицы. После удаления таблицы или окончания сеанса индекс также уничтожается.  

Упорядоченный кластеризованный индекс columnstore можно создавать для столбцов любых типов данных, поддерживаемых в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], за исключением строковых столбцов.  
 
## <a name="filtered-indexes"></a>Отфильтрованные индексы  
Отфильтрованный индекс является оптимизированным некластеризованным индексом, предназначенным для запросов, выбирающих небольшой процент строк таблицы. Чтобы проиндексировать часть данных таблицы, в нем используется предикат фильтра. Правильно составленный отфильтрованный индекс может увеличить скорость выполнения запроса, уменьшить стоимость хранения и обслуживания.  
  
### <a name="required-set-options-for-filtered-indexes"></a>Обязательные параметры SET для отфильтрованных индексов  
Параметры SET в столбце Required Value необходимы при возникновении любого из следующих условий.  
- Создание отфильтрованного индекса.  
- Операция INSERT, UPDATE, DELETE или MERGE изменяет данные в отфильтрованном индексе.  
- Отфильтрованный индекс используется оптимизатором запросов для создания плана запроса.  
  
    |Параметры SET|Обязательное значение|Значение сервера по умолчанию|По умолчанию<br /><br /> Значение OLE DB и ODBC|По умолчанию<br /><br /> Значение DB-Library|  
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|  
    |ANSI_NULLS|ON|ON|ON|OFF|  
    |ANSI_PADDING|ON|ON|ON|OFF|  
    |ANSI_WARNINGS*|ON|ON|ON|OFF|  
    |ARITHABORT|ON|ON|OFF|OFF|  
    |CONCAT_NULL_YIELDS_NULL|ON|ON|ON|OFF|  
    |NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|  
    |QUOTED_IDENTIFIER|ON|ON|ON|OFF|   
  
     *Если уровень совместимости базы данных равен 90 или более, при установке параметра ANSI_WARNINGS в состояние ON параметр ARITHABORT также устанавливается в состояние ON. Если уровень совместимости базы данных установлен в состояние 80 или более раннее, то параметр ARITHABORT необходимо явным образом установить в состояние ON.  
  
 Если параметры SET неверны, может произойти следующее.  
  
-   Отфильтрованный индекс не будет создан.  
  
-   Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] сформирует ошибку и выполнит откат любой инструкции INSERT, UPDATE, DELETE или MERGE, которая изменила значения данных в индексе.  
  
-   Оптимизатор запросов не учтет индекс в плане выполнения любой инструкции Transact-SQL.  
  
 Дополнительные сведения об отфильтрованных индексах см. в статье [Создание отфильтрованных индексов](../../relational-databases/indexes/create-filtered-indexes.md). 
  
##  <a name="limitations-and-restrictions"></a><a name="LimitRest"></a> Ограничения  

**Каждый столбец в индексе columnstore должен иметь один из следующих типов общих бизнес-данных:** 
-   datetimeoffset [ ( *n* ) ]  
-   datetime2 [ ( *n* ) ]  
-   DATETIME  
-   smalldatetime  
-   Дата  
-   time [ ( *n* ) ]  
-   float [ ( *n* ) ]  
-   real [ ( *n* ) ]  
-   decimal [ ( *precision* [ *, scale* ] **)** ]
-   numeric [ ( *precision* [ *, scale* ] **)** ]    
-   money  
-   smallmoney  
-   BIGINT  
-   INT  
-   smallint  
-   tinyint  
-   bit  
-   nvarchar [ ( *n* ) ] 
-   nvarchar(max) (применяется к [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и уровню "Премиум", уровню "Стандартный" (S3 и выше) и всем уровням предложений виртуальных ядер только в кластеризованных индексах columnstore)   
-   nchar [ ( *n* ) ]  
-   varchar [ ( *n* ) ]  
-   varchar(max) (применяется к [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и уровню "Премиум", уровню "Стандартный" (S3 и выше) и всем уровням предложений виртуальных ядер только в кластеризованных индексах columnstore)
-   char [ ( *n* ) ]  
-   varbinary [ ( *n* ) ] 
-   varbinary (max) (применяется к [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и базе данных SQL Azure на уровне "Премиум", уровне "Стандартный" (S3 и выше) и всех уровнях предложений виртуальных ядер только в кластеризованных индексах columnstore)
-   binary [ ( *n* ) ]  
-   uniqueidentifier (область применения: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздние версии)
  
Ели в базовой таблице есть столбец с типом данных, не поддерживаемым для индексов columnstore, необходимо исключить этот столбец из некластеризованного индекса columnstore.  
  
**Столбцы, которые используют следующие типы данных, не могут быть включены в индекс columnstore:**
-   ntext, text, и image  
-   nvarchar(max), varchar(max) и varbinary(max) (область применения: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и предыдущие версии и некластеризованные индексы columnstore) 
-   rowversion (и timestamp)  
-   sql_variant  
-   Типы CLR (hierarchyid и пространственные типы)  
-   Xml  
-   uniqueidentifier (область применения: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  

**Некластеризованные индексы columnstore:**
-   Не более 1024 столбцов.
-   Невозможно создать как индексы на основе ограничений. Таблица с индексом columnstore может иметь ограничения уникальности, ограничения первичного ключа и ограничения внешнего ключа. Ограничения всегда применяются с помощью индекса row-store. Ограничения невозможно применить с помощью индекса columnstore (кластеризованного или некластеризованного).
-   Не может содержать разреженный столбец.  
-   Не может быть изменено с использованием инструкции **ALTER INDEX**. Чтобы изменить некластеризованный индекс, следует удалить и повторно создать индекс columnstore. Инструкция **ALTER INDEX** позволяет отключить и перестроить индекс columnstore.  
-   Не может быть создано с использованием ключевого слова **INCLUDE**.  
-   Нельзя включать ключевые слова **ASC** и **DESC** для сортировки индексов. Индексы columnstore упорядочены в соответствии с алгоритмами сжатия. В результате сортировки можно потерять многие преимущества в производительности.  
-   Нельзя включать столбцы больших объектов (LOB) типа nvarchar(max), varchar(max) и varbinary(max) в некластеризованные индексы columnstore. Только кластеризованные индексы columnstore поддерживают типы больших объектов, начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] и базы данных SQL Azure на уровне "Премиум", уровне "Стандартный" (S3 и выше) и всех уровнях предложений виртуальных ядер. Обратите внимание, что предыдущие версии не поддерживают типы больших объектов в кластеризованных и некластеризованных индексах columnstore.


> [!NOTE]  
> Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], вы можете создавать некластеризованный индекс columnstore для индексированного представления.  


 **Индексы columnstore нельзя использовать вместе со следующими функциями:**  
-   вычисляемые столбцы; Начиная с SQL Server 2017 кластеризованный индекс columnstore может содержать нематериализованный вычисляемый столбец. Однако в SQL Server 2017 кластеризованные индексы columnstore не могут содержать материализованные вычисляемые столбцы, и невозможно создать некластеризованные индексы в вычисляемых столбцах. 
-   Формат сжатия страниц и строк, а также хранения **vardecimal** (индекс columnstore уже сжат в другом формате).  
-   Репликация  
-   Файловый поток

Нельзя использовать курсоры или триггеры в таблице с кластеризованным индексом columnstore. Это ограничение не применяется к некластеризованным индексам columnstore. Курсоры и триггеры можно использовать в таблице с некластеризованным индексом columnstore.

**Особые ограничения [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**  
Эти ограничения применяются только к [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. В этом выпуске мы представили обновляемые кластеризованные индексы columnstore. Некластеризованные индексы columnstore были доступны только для чтения.  

-   Отслеживание изменений. Отслеживание изменений невозможно использовать с индексами columnstore.  
-   Система отслеживания измененных данных. Отслеживание измененных данных невозможно включить для таблиц с кластеризованным индексом columnstore. Начиная с SQL Server 2016 его можно включить для таблиц с некластеризованным индексом columnstore.  
-   Вторичные реплики для чтения. Невозможно получить доступ к кластеризованному индексу columnstore (CCI) из вторичной реплики для чтения группы доступности AlwaysOn.  К некластеризованному индексу columnstore (NCCI) можно получить доступ из вторичной реплики для чтения.  
-   Множественный активный результирующий набор (MARS). [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] использует функцию MARS для установки подключений с доступом только для чтения с использованием таблиц с индексом columnstore. При этом [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] не поддерживает функцию MARS для параллельного выполнения операций DML в таблице с индексом columnstore. В этом случае [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] завершает подключения и прерывает выполнение транзакций.  
-  Некластеризованные индексы columnstore недопустимо создавать для представления или индексированного представления.
  
 Сведения о преимуществах в производительности и ограничениях индексов columnstore см. в статье [Общие сведения об индексах columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
##  <a name="metadata"></a><a name="Metadata"></a> Метаданные  
 Все столбцы в индексе columnstore хранятся в метаданных как включенные столбцы. Индекс columnstore не имеет ключевых столбцов. Эти системные представления предоставляют сведения об индексах columnstore.  
  
-   [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
-   [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
-   [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)  
-   [sys.column_store_segments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
-   [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
-   [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)  

##  <a name="examples-for-converting-a-rowstore-table-to-columnstore"></a><a name="convert"></a> Примеры преобразования таблицы rowstore в columnstore  
  
### <a name="a-convert-a-heap-to-a-clustered-columnstore-index"></a>A. Преобразование кучи в кластеризованный индекс columnstore  
 В этом примере создается таблица как куча, затем преобразуется в кластеризованный индекс с именем columnstore cci_Simple. В результате таблица rowstore становится таблицей columnstore.  
  
```sql  
CREATE TABLE SimpleTable(  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cci_Simple ON SimpleTable;  
GO  
```  
  
### <a name="b-convert-a-clustered-index-to-a-clustered-columnstore-index-with-the-same-name"></a>Б. Преобразование кластеризованного индекса в кластеризованный индекс columnstore с тем же именем.  
 В этом примере создается таблица с кластеризованным индексом, затем демонстрируется синтаксис преобразования кластеризованного индекса в кластеризованный индекс columnstore. В результате таблица rowstore становится таблицей columnstore.  
  
```sql  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cl_simple ON SimpleTable  
WITH (DROP_EXISTING = ON);  
GO  
```  
  
### <a name="c-handle-nonclustered-indexes-when-converting-a-rowstore-table-to-a-columnstore-index"></a>В. Обработка некластеризованных индексов при преобразовании таблицы rowstore в индекс columnstore.  
 В этом примере показано, как обрабатывать некластеризованные индексы при преобразовании таблицы rowstore в индекс columnstore. На самом деле, начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] никаких специальных действий не требуется. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически определяет и перестраивает некластеризованные индексы в новый кластеризованный индекс columnstore.  
  
 Если вы хотите удалить некластеризованные индексы, используйте инструкцию DROP INDEX до создания индекса columnstore. Параметр DROP EXISTING удаляет только преобразуемый кластеризованный индекс. Он не удаляет некластеризованные индексы.  
  
 В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] было невозможно создать некластеризованный индекс в индексе columnstore. В этом примере показано, как в предыдущих выпусках необходимо было удалить некластеризованные индексы до создания индекса columnstore.  
  
```sql  
--Create the table for use with this example.  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
  
--Create two nonclustered indexes for use with this example  
CREATE INDEX nc1_simple ON SimpleTable (OrderDateKey);  
CREATE INDEX nc2_simple ON SimpleTable (DueDateKey);   
GO  
  
--SQL Server 2012 and SQL Server 2014: you need to drop the nonclustered indexes  
--in order to create the columnstore index.   
  
DROP INDEX SimpleTable.nc1_simple;  
DROP INDEX SimpleTable.nc2_simple;  
  
--Convert the rowstore table to a columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_simple ON SimpleTable;   
GO  
```  
  
### <a name="d-convert-a-large-fact-table-from-rowstore-to-columnstore"></a>Г. Преобразование большой таблицы фактов из rowstore в columnstore  
 В этом примере показано, как преобразовать большую таблицу фактов из таблицы rowstore в таблицу columnstore.  
  
 Преобразование таблицы rowstore в таблицу columnstore.  
  
1.  Сначала создается небольшая таблица для использования в этом примере.  
  
    ```sql  
    --Create a rowstore table with a clustered index and a nonclustered index.  
    CREATE TABLE MyFactTable (  
        ProductKey [INT] NOT NULL,  
        OrderDateKey [INT] NOT NULL,  
         DueDateKey [INT] NOT NULL,  
         ShipDateKey [INT] NOT NULL )  
    )  
    WITH (  
        CLUSTERED INDEX ( ProductKey )  
    );  
  
    --Add a nonclustered index.  
    CREATE INDEX my_index ON MyFactTable ( ProductKey, OrderDateKey );  
    ```  
  
2.  Удалите все некластеризованные индексы из таблицы rowstore.  
  
    ```sql  
    --Drop all nonclustered indexes  
    DROP INDEX my_index ON MyFactTable;  
    ```  
  
3.  Удалить кластеризованный индекс.  
  
    -   Это следует делать только в том случае, если необходимо указать новое имя индекса при его преобразовании в кластеризованный индекс columnstore. Если не удалить кластеризованный индекс, новый кластеризованный индекс columnstore будет иметь то же имя.  
  
        > [!NOTE]  
        > Имя индекса может оказаться легче запомнить, если вы используете собственное имя. Все кластеризованные индексы rowstore используют имя по умолчанию — ClusteredIndex_\<GUID>.  
  
    ```sql  
    --Process for dropping a clustered index.  
    --First, look up the name of the clustered rowstore index.  
    --Clustered rowstore indexes always use the DEFAULT name 'ClusteredIndex_<GUID>'.  
    SELECT i.name   
    FROM sys.indexes i   
    JOIN sys.tables t  
    ON ( i.type_desc = 'CLUSTERED' ) WHERE t.name = 'MyFactTable';  
  
    --Drop the clustered rowstore index.  
    DROP INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09 ON MyDimTable;  
    ```  
  
4.  Преобразуйте таблицу rowstore в таблицу columnstore с кластеризованным индексом columnstore.  
  
    ```sql  
    --Option 1: Convert to columnstore and name the new clustered columnstore index MyCCI.  
    CREATE CLUSTERED COLUMNSTORE INDEX MyCCI ON MyFactTable;  
  
    --Option 2: Convert to columnstore and use the rowstore clustered   
    --index name for the columnstore clustered index name.  
    --First, look up the name of the clustered rowstore index.  
    SELECT i.name   
    FROM sys.indexes i  
    JOIN sys.tables t   
    ON ( i.type_desc = 'CLUSTERED' )  
    WHERE t.name = 'MyFactTable';  
  
    --Second, create the clustered columnstore index and   
    --Replace ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    --with the name of your clustered index.  
    CREATE CLUSTERED COLUMNSTORE INDEX   
    ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
     ON MyFactTable  
    WITH DROP_EXISTING = ON;  
    ```  
  
### <a name="e-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>Д. Преобразование таблицы columnstore в таблицу rowstore с кластеризованным индексом  
 Чтобы преобразовать таблицу columnstore в таблицу rowstore с кластеризованным индексом, используйте инструкцию CREATE INDEX с параметром DROP_EXISTING.  
  
```sql  
CREATE CLUSTERED INDEX ci_MyTable   
ON MyFactTable  
WITH ( DROP EXISTING = ON );  
```  
  
### <a name="f-convert-a-columnstore-table-to-a-rowstore-heap"></a>Е. Преобразование таблицы columnstore в кучу rowstore  
 Чтобы преобразовать таблицу columnstore в кучу rowstore, просто удалите кластеризованный индекс columnstore.  
  
```sql  
DROP INDEX MyCCI   
ON MyFactTable;  
```  
  

### <a name="g-defragment-by-rebuilding-the-entire-clustered-columnstore-index"></a>Ж. Дефрагментация путем перестроения всего кластеризованного индекса columnstore  
   Применимо к: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 Есть два способа полностью перестроить кластеризованный индекс columnstore. Можно использовать инструкцию CREATE CLUSTERED COLUMNSTORE INDEX или [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md) с параметром REBUILD. Оба метода дают одинаковые результаты.  
  
> [!NOTE]  
> Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], используйте `ALTER INDEX...REORGANIZE` вместо перестраивания с помощью методов, описанных в этом примере.  
  
```sql  
--Determine the Clustered Columnstore Index name of MyDimTable.  
SELECT i.object_id, i.name, t.object_id, t.name   
FROM sys.indexes i   
JOIN sys.tables t  
ON (i.type_desc = 'CLUSTERED COLUMNSTORE')  
WHERE t.name = 'RowstoreDimTable';  
  
--Rebuild the entire index by using CREATE CLUSTERED INDEX.  
CREATE CLUSTERED COLUMNSTORE INDEX my_CCI   
ON MyFactTable  
WITH ( DROP_EXISTING = ON );  
  
--Rebuild the entire index by using ALTER INDEX and the REBUILD option.  
ALTER INDEX my_CCI  
ON MyFactTable  
REBUILD PARTITION = ALL  
WITH ( DROP_EXISTING = ON );  
```  
  
##  <a name="examples-for-nonclustered-columnstore-indexes"></a><a name="nonclustered"></a> Примеры некластеризованных индексов columnstore  
  
### <a name="a-create-a-columnstore-index-as-a-secondary-index-on-a-rowstore-table"></a>A. Создание индекса columnstore в качестве вторичного индекса в таблице rowstore  
 В этом примере создается некластеризованный индекс в таблице rowstore. В этом случае можно создать только один индекс columnstore. Индекс columnstore требует дополнительного места, поскольку содержит копию данных из таблицы rowstore. В этом примере показано, как создать простую таблицу и кластеризованный индекс, затем демонстрируется синтаксис создания некластеризованного индекса columnstore.  
  
```sql  
CREATE TABLE SimpleTable  
(ProductKey [INT] NOT NULL,   
OrderDateKey [INT] NOT NULL,   
DueDateKey [INT] NOT NULL,   
ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey);  
GO  
```  
  
### <a name="b-create-a-simple-nonclustered-columnstore-index-using-all-options"></a>Б. Создание простого некластеризованного индекса columnstore с использованием всех параметров  
 В следующем примере демонстрируется синтаксис создания некластеризованного индекса columnstore с использованием всех параметров.  
  
```sql  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey)  
WITH (DROP_EXISTING =  ON,   
    MAXDOP = 2)  
ON "default"  
GO  
```  
  
 Более сложный пример с использованием секционированных таблиц см. в разделе [Общие сведения об индексах columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
### <a name="c-create-a-nonclustered-columnstore-index-with-a-filtered-predicate"></a>В. Создание некластеризованного индекса columnstore с предикатом фильтрации  
 В следующем примере создается отфильтрованный некластеризованный индекс columnstore в таблице Production.BillOfMaterials в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Предикат фильтра может включать столбцы, не являющиеся ключевыми в отфильтрованном индексе. Предикат в примере выбирает только те строки, где EndDate не равно NULL.  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithEndDate'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX "FIBillOfMaterialsWithEndDate"  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL;  
```  
  
###  <a name="d-change-the-data-in-a-nonclustered-columnstore-index"></a><a name="ncDML"></a> Г. Изменение данных в некластеризованном индексе columnstore  
   Применимо к: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].
  
 После создания некластеризованного индекса columnstore в таблице нельзя непосредственно изменять данные в этой таблице. Запрос с инструкциями INSERT, UPDATE, DELETE или MERGE завершится сбоем и вернет сообщение об ошибке. Для добавления или изменения данных в таблице можно воспользоваться одним из следующих способов.  
  
-   Отключить или удалить индекс columnstore. Затем можно обновлять данные в таблице. Если отключить индекс columnstore, то можно перестроить его после окончания обновления данных. Например,  
  
    ```sql  
    ALTER INDEX mycolumnstoreindex ON mytable DISABLE;  
    -- update mytable --  
    ALTER INDEX mycolumnstoreindex on mytable REBUILD  
    ```  
  
-   Загрузка данных в промежуточную таблицу, не имеющую индекса columnstore. Создание индекса columnstore в промежуточной таблице. Переключение промежуточной таблицы в пустую секцию главной таблицы.  
  
-   Переключение секции из таблицы с индексом columnstore в пустую промежуточную таблицу. Если в промежуточной таблице имеется индекс columnstore, отключите индекс columnstore. Выполните все обновления. Постройте (или перестройте) индекс columnstore. Переключитесь с промежуточной таблицы обратно на (теперь пустую) секцию главной таблицы.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-change-a-clustered-index-to-a-clustered-columnstore-index"></a>A. Преобразование кластеризованного индекса в кластеризованный индекс columnstore  
 С помощью инструкции CREATE CLUSTERED COLUMNSTORE INDEX с параметром DROP_EXISTING = ON выполняется:  
  
-   Преобразование кластеризованного индекса в кластеризованный индекс columnstore.  
  
-   Перестроение кластеризованного индекса columnstore.  
  
 В этом примере создается таблица xDimProduct в виде таблицы rowstore с кластеризованным индексом, а затем используется инструкция CREATE CLUSTERED COLUMNSTORE INDEX для преобразования таблицы из rowstore в columnstore.  
  
```sql  
-- Uses AdventureWorks  
  
IF EXISTS (SELECT name FROM sys.tables  
    WHERE name = N'xDimProduct'  
    AND object_id = OBJECT_ID (N'xDimProduct'))  
DROP TABLE xDimProduct;  
  
--Create a distributed table with a clustered index.  
CREATE TABLE xDimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey)  
WITH ( DISTRIBUTION = HASH(ProductKey),  
    CLUSTERED INDEX (ProductKey) )  
AS SELECT ProductKey, ProductAlternateKey, ProductSubcategoryKey FROM DimProduct;  
  
--Change the existing clustered index   
--to a clustered columnstore index with the same name.  
--Look up the name of the index before running this statement.  
CREATE CLUSTERED COLUMNSTORE INDEX <index_name>   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="b-rebuild-a-clustered-columnstore-index"></a>Б. Перестроение кластеризованного индекса columnstore  
 Этот пример основан на предыдущем примере. В нем используется инструкция CREATE CLUSTERED COLUMNSTORE INDEX для перестроения существующего кластеризованного индекса columnstore с именем cci_xDimProduct.  
  
```sql  
--Rebuild the existing clustered columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_xDimProduct   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="c-change-the-name-of-a-clustered-columnstore-index"></a>В. Изменение кластеризованного индекса columnstore  
 Чтобы изменить имя кластеризованного индекса columnstore, удалите существующий кластеризованный индекс columnstore, а затем заново создайте индекс с новым именем.  
  
 Рекомендуется выполнять эту операцию только с небольшими или пустыми таблицами. Удаление большого кластеризованного индекса columnstore и его перестройка с другим именем занимает много времени.  
  
 В этом примере удаляется кластеризованный индекс columnstore с именем cci_xDimProduct, взятый из предыдущего примера, а затем повторно создается кластеризованный индекс columnstore с именем mycci_xDimProduct.  
  
```sql  
--For illustration purposes, drop the clustered columnstore index.   
--The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xDimProduct;  
  
--Create a clustered index with a new name, mycci_xDimProduct.  
CREATE CLUSTERED COLUMNSTORE INDEX mycci_xDimProduct  
ON xdimProduct  
WITH ( DROP_EXISTING = OFF );  
```  
  
### <a name="d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>Г. Преобразование таблицы columnstore в таблицу rowstore с кластеризованным индексом  
 Может возникнуть ситуация, в которой необходимо удалить кластеризованный индекс columnstore и создать кластеризованный индекс. При этом таблица будет сохранена в формате rowstore. В этом примере таблица columnstore преобразуется в таблицу rowstore с кластеризованным индексом с тем же именем. Данные не будут утеряны. Все данные переносятся в таблицу rowstore, а указанные столбцы становятся ключевыми столбцами в кластеризованном индексе.  
  
```sql  
--Drop the clustered columnstore index and create a clustered rowstore index.   
--All of the columns are stored in the rowstore clustered index.   
--The columns listed are the included columns in the index.  
CREATE CLUSTERED INDEX cci_xDimProduct    
ON xdimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey, WeightUnitMeasureCode)  
WITH ( DROP_EXISTING = ON);  
```  
  
### <a name="e-convert-a-columnstore-table-back-to-a-rowstore-heap"></a>Д. Преобразование таблицы columnstore обратно в кучу rowstore  
 Используйте [DROP INDEX (SQL Server PDW)](drop-index-transact-sql.md), чтобы удалить кластеризованный индекс columnstore и преобразовать таблицу в кучу rowstore. В этом примере таблица cci_xDimProduct преобразуется в кучу rowstore. Таблица остается распределенной, но хранится в виде кучи.  
  
```sql  
--Drop the clustered columnstore index. The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xdimProduct;  
```  

### <a name="f-create-an-ordered-clustered-columnstore-index-on-a-table-with-no-index"></a>Е. Создание упорядоченного кластеризованного индекса columnstore в таблице без индекса

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
```

### <a name="g-convert-a-clustered-columnstore-index-to-an-ordered-clustered-columnstore-index"></a>Ж. Преобразование кластеризованного индекса columnstore в упорядоченный кластеризованный индекс columnstore

```sql  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
WITH (DROP_EXISTING = ON)
```

### <a name="h-add-a-column-to-the-ordering-of-an-ordered-clustered-columnstore-index"></a>З. Добавление столбца в порядок упорядоченного кластеризованного индекса columnstore

```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE column only.  Add PRODUCTKEY column to the ordering.
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE, PRODUCTKEY );
WITH (DROP_EXISTING = ON)
```
### <a name="i-change-the-ordinal-of-ordered-columns"></a>И. Изменение порядковых номеров упорядоченных столбцов  
```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE, PRODUCTKEY.  Change the ordering to PRODUCTKEY, SHIPDATE.  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( PRODUCTKEY,SHIPDATE );
WITH (DROP_EXISTING = ON)
```