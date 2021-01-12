---
description: CREATE INDEX (Transact-SQL)
title: CREATE INDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE INDEX
- INDEX
- INDEX_TSQL
- CREATE_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- PRIMARY XML INDEX statement
- indexes [SQL Server], creating
- online index operations
- index keys [SQL Server]
- PROPERTY INDEX statement
- computed columns, index creation
- clustered indexes, creating
- indexed views [SQL Server], index creation
- partitioned indexes [SQL Server], creating
- VALUE INDEX statement
- index creation [SQL Server], CREATE INDEX statement
- DROP_EXISTING clause
- row locks [SQL Server]
- composite indexes
- MAXDOP index option, CREATE INDEX statement
- filtered indexes [SQL Server], creating
- PATH INDEX statement
- locking [SQL Server], indexes
- unique indexes, creating
- primary indexes [SQL Server]
- CREATE PRIMARY XML INDEX statement
- SET statement, index creation
- index options [SQL Server]
- included columns
- ONLINE option
- nonclustered indexes [SQL Server], creating
- CREATE INDEX statement
- IGNORE_DUP_KEY option
- deterministic functions
- keys [SQL Server], index
- SECONDARY XML INDEX statement
- page locks [SQL Server]
- secondary indexes [SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: d2297805-412b-47b5-aeeb-53388349a5b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 01a80dd71397a4528c1d56882cec5934d750bb8f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093526"
---
# <a name="create-index-transact-sql"></a>CREATE INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Создает реляционный индекс для таблицы или представления. Он также называется индексом rowstore, так как является кластеризованным или некластеризованным индексом сбалансированного дерева. Индекс rowstore можно создать до заполнения таблицы данными. Индекс rowstore позволяет повысить производительность запросов, особенно в том случае, если запросы выбирают определенные столбцы или им требуются значения, которые должны быть отсортированы в определенном порядке.

> [!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] в настоящее время не поддерживает ограничения уникальности. Примеры, ссылающиеся на ограничения уникальности применимы только к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> [!TIP]
> Дополнительные сведения о правилах проектирования индексов см. в статье [Руководство по проектированию индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md).

 **Простые примеры:**

```sql
-- Create a nonclustered index on a table or view
CREATE INDEX i1 ON t1 (col1);

-- Create a clustered index on a table and use a 3-part name for the table
CREATE CLUSTERED INDEX i1 ON d1.s1.t1 (col1);

-- Syntax for SQL Server and Azure SQL Database
-- Create a nonclustered index with a unique constraint
-- on 3 columns and specify the sort order for each column
CREATE UNIQUE INDEX i1 ON t1 (col1 DESC, col2 ASC, col3 DESC);
```

**Основной сценарий.**

Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и [!INCLUDE[ssSDS](../../includes/sssds-md.md)], некластеризованный индекс используется в индексе columnstore для повышения производительности запросов хранилища данных. Дополнительные сведения см. в статье [Хранилище данных для индексов columnstore](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md).

Дополнительные типы индексов описаны в следующих статьях:

- [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)
- [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)
- [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md)

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

### <a name="syntax-for-sql-server-and-azure-sql-database"></a>Синтаксис для SQL Server и базы данных SQL Azure

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column [ ASC | DESC ] [ ,...n ] )
    [ INCLUDE ( column_name [ ,...n ] ) ]
    [ WHERE <filter_predicate> ]
    [ WITH ( <relational_index_option> [ ,...n ] ) ]
    [ ON { partition_scheme_name ( column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
[ ; ]
  
<object> ::=
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }

<relational_index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB = { ON | OFF }
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | DROP_EXISTING = { ON | OFF }
  | ONLINE = { ON | OFF }
  | RESUMABLE = { ON | OFF }
  | MAX_DURATION = <time> [MINUTES]
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | MAXDOP = max_degree_of_parallelism
  | DATA_COMPRESSION = { NONE | ROW | PAGE }
     [ ON PARTITIONS ( { <partition_number_expression> | <range> }
     [ , ...n ] ) ]
}

<filter_predicate> ::=
    <conjunct> [ AND <conjunct> ]

<conjunct> ::=
    <disjunct> | <comparison>

<disjunct> ::=
        column_name IN (constant ,...n)

<comparison> ::=
        column_name <comparison_op> constant

<comparison_op> ::=
    { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< }

<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

### <a name="backward-compatible-relational-index"></a>Реляционный индекс с обратной совместимостью

> [!IMPORTANT]
> Структура синтаксиса реляционного индекса с обратной совместимостью будет исключена из следующей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
> Избегайте ее использования в новых разработках и запланируйте изменение приложений, которые пользуются ею сейчас.
> Вместо нее используйте структуру синтаксиса, указанную в <relational_index_option>.

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column_name [ ASC | DESC ] [ ,...n ] )
    [ WITH <backward_compatible_index_option> [ ,...n ] ]
    [ ON { filegroup_name | "default" } ]

<object> ::=
{
    [ database_name. [ owner_name ] . | owner_name. ]
    table_or_view_name
}

<backward_compatible_index_option> ::=
{
    PAD_INDEX
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB
  | IGNORE_DUP_KEY
  | STATISTICS_NORECOMPUTE
  | DROP_EXISTING
}
```

### <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Синтаксис для Azure Synapse Analytics и Parallel Data Warehouse

```syntaxsql

CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
    [ORDER (column[,...n])]
    [WITH ( DROP_EXISTING = { ON | OFF } )]
[;]


CREATE [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
        ( { column [ ASC | DESC ] } [ ,...n ] )
    WITH ( DROP_EXISTING = { ON | OFF } )
[;]


```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

UNIQUE      
Создает уникальный индекс для таблицы или представления. Уникальным является индекс, в котором не может быть двух строк с одним и тем же значением ключа индекса. Кластеризованный индекс представления должен быть уникальным.

Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не позволяет создать уникальный индекс по столбцам, уже содержащим повторяющиеся значения, даже если параметру IGNORE_DUP_KEY присвоено значение ON. При попытке создания такого индекса компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдает сообщение об ошибке. Прежде чем создавать уникальный индекс по такому столбцу или столбцам, необходимо удалить все повторяющиеся значения. Столбцы, используемые в уникальном индексе, должны иметь свойство NOT NULL, т. к. при создании индекса значения NULL рассматриваются как повторяющиеся.

CLUSTERED      
Создает индекс, в котором логический порядок значений ключа определяет физический порядок соответствующих строк в таблице. На нижнем (конечном) уровне кластеризованного индекса хранятся действительные строки данных таблицы. Для таблицы или представления в каждый момент времени может существовать только один кластеризованный индекс.

Представление с уникальным кластеризованным индексом называется индексированным. Создание уникального кластеризованного индекса физически материализует представление. Уникальный кластеризованный индекс для представления должен быть создан до того, как для этого же представления будут определены какие-либо другие индексы. Дополнительные сведения см. в разделе [Создание индексированных представлений](../../relational-databases/views/create-indexed-views.md).

Создавайте кластеризованные индексы до создания любых некластеризованных. При создании кластеризованного индекса все существующие некластеризованные индексы таблицы перестраиваются.

Если аргумент `CLUSTERED` не указан, создается некластеризованный индекс.

> [!NOTE]
> Поскольку конечный уровень кластеризованного индекса и страницы данных — это по определению одно и то же, создание кластеризованного индекса и использование предложения ON *partition_scheme_name* или ON *filegroup_name* приводят к перемещению таблицы из файловой группы, в которой она была создана, в новую схему секционирования или файловую группу. Прежде чем создавать таблицы или индексы в определенных файловых группах, проверьте, какие группы доступны, и убедитесь в том, что в этих группах достаточно свободного места для индекса.

В некоторых случаях создание кластеризованного индекса может привести к включению ранее отключенных индексов. Дополнительные сведения см. в разделах [Включение индексов и ограничений](../../relational-databases/indexes/enable-indexes-and-constraints.md) и [Отключение индексов и ограничений](../../relational-databases/indexes/disable-indexes-and-constraints.md).

NONCLUSTERED      
Создает индекс, задающий логическое упорядочение для таблицы. Логический порядок строк в некластеризованном индексе не влияет на их физический порядок.

Каждая таблица может содержать до 999 некластеризованных индексов независимо от способа их создания: неявно с помощью ограничений PRIMARY KEY и UNIQUE или явно с помощью инструкции CREATE INDEX.

Для индексированных представлений некластеризованные индексы могут создаваться только в случае, если уже определен уникальный кластеризованный индекс.

Если не указано иное, типом индекса по умолчанию является NONCLUSTERED.

*index_name*      
 Имя индекса. Имена индексов должны быть уникальными в пределах таблицы или представления, но необязательно должны быть уникальными в пределах базы данных. Имена индексов должны удовлетворять правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).

*column*      
 Столбец или столбцы, на которых основан индекс. Имена одного или нескольких столбцов для создания комбинированного индекса. Столбцы, которые должны быть включены в составной индекс, указываются в скобках за аргументом *table_or_view_name* в порядке сортировки.

В один ключ составного индекса могут входить до 32 столбцов. Все столбцы ключа составного индекса должны находиться в одной таблице или одном и том же представлении. Максимально допустимый размер значений составного индекса составляет 900 байтов для кластеризованного индекса или 1700 для некластеризованного индекса. Ограничениями являются 16 столбцов и 900 байт для версий до [!INCLUDE[ssSDS](../../includes/sssds-md.md)] и [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)].

Столбцы с типами данных LOB **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** или **image** нельзя указать в качестве столбцов для индекса. Кроме того, определение представления не может включать столбцы типов **ntext**, **text** или **image**, даже если они не указаны в инструкции CREATE INDEX.

Можно создавать индексы на столбцах с определяемым пользователем типом данных CLR, если этот тип поддерживает двоичное упорядочение. Можно также создавать индексы на вычисляемых столбцах, определенных как вызовы методов для столбцов с определяемыми пользователем типами данных, если эти методы помечены как детерминированные и не выполняют операции доступа к данным. Дополнительные сведения об индексировании столбцов с определяемыми пользователем типами данных CLR см. в разделе [Определяемые пользователем типы данных CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

[ **ASC** | DESC ]      
Определяет сортировку значений заданного столбца индекса: по возрастанию или по убыванию. Значение по умолчанию — ASC.

1INCLUDE **(** _column_ [ **,** ... *n* ] **)**       
Указывает неключевые столбцы, добавляемые на конечный уровень некластеризованного индекса. Некластеризованный индекс может быть уникальным или неуникальным.

Имена столбцов в списке INCLUDE не могут повторяться и не могут использоваться одновременно как ключевые и неключевые. Некластеризованные индексы всегда содержат столбцы кластеризованного индекса, если для таблицы определен кластеризованный индекс. Дополнительные сведения см. в статье [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md).

Допускаются данные всех типов, за исключением **text**, **ntext** и **image**. Индекс должен создаваться или перестраиваться в режиме "вне сети" (ONLINE = OFF), если любой из заданных неключевых столбцов имеет тип данных **varchar(max)** , **nvarchar(max)** или **varbinary(max)** .

Вычисляемые столбцы, являющиеся детерминированными и точными или неточными, могут быть включенными столбцами. Вычисляемые столбцы, производные от типов данных **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** и **xml**, могут быть включены в неключевые столбцы, если типы данных вычисляемого столбца допускаются в качестве столбца для включения. Дополнительные сведения см. в разделе [Индексы вычисляемых столбцов](../../relational-databases/indexes/indexes-on-computed-columns.md).

Сведения о создании XML-индекса см. в описании [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md).

WHERE \<filter_predicate>      
Создает отфильтрованный индекс путем указания строк для включения в индекс. Отфильтрованный индекс должен быть некластеризованным индексом для таблицы. Создается отфильтрованная статистика для строк данных отфильтрованного индекса.

Предикат фильтра использует простую логику сравнения и не может ссылаться на вычисляемый столбец, столбец определяемого пользователем типа, столбец типа пространственных данных или столбец типа hierarchyID. Сравнения с помощью литералов NULL с операторами сравнения недопустимы. Вместо этого используются операторы IS NULL и IS NOT NULL.

Далее приведено несколько примеров использования предикатов фильтра для таблицы `Production.BillOfMaterials`:

```sql
WHERE StartDate > '20000101' AND EndDate <= '20000630'

WHERE ComponentID IN (533, 324, 753)

WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL
```

Отфильтрованные индексы не применяются к XML-индексам и полнотекстовым индексам. Для индексов UNIQUE только выбранные строки должны иметь уникальные значения индексов. Отфильтрованные индексы не поддерживают параметр IGNORE_DUP_KEY.

ON *partition_scheme_name* **( _column_name_ )**      

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Задает схему секционирования, которая определяет файловые группы, соответствующие секциям секционированного индекса. Схема секционирования должна быть создана в базе данных путем выполнения инструкции [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) или [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). *column_name* указывает столбец, по которому будет секционирован индекс. Столбец должен соответствовать по типу данных, длине и точности аргументу функции секционирования, используемой аргументом *partition_scheme_name*. Аргумент *column_name* необязательно должен соответствовать столбцам из определения индекса. Можно указать любой столбец базовой таблицы, за исключением случая секционирования индекса UNIQUE, когда аргумент *column_name* должен быть выбран из используемых в качестве уникального ключа. Это ограничение дает возможность компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] проверять уникальность значений ключа только в одной секции.

> [!NOTE]
> При секционировании неуникального кластеризованного индекса компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] по умолчанию добавляет столбец секционирования в список ключей кластеризованного индекса, если этого столбца еще нет в списке. При секционировании неуникального некластеризованного индекса компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] добавляет столбец секционирования как неключевой (включенный) столбец индекса, если этого столбца еще нет в списке.

Если аргумент _partition_scheme_name_ или _filegroup_ не задан и таблица секционирована, индекс помещается в ту же схему секционирования и с тем же столбцом секционирования, что и для базовой таблицы.

> [!NOTE]
> Для XML-индекса задать схему секционирования невозможно. Если базовая таблица секционирована, XML-индекс использует ту же схему секционирования, что и таблица.

Дополнительные сведения об индексах секционирования см. в разделе [Секционированные таблицы и индексы](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON _filegroup_name_      

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше)

Создает заданный индекс в указанной файловой группе. Если местоположение не указано и таблица или представление не секционированы, индекс использует ту же файловую группу, что и базовая таблица или базовое представление. Файловая группа должна существовать.

ON **"** default **"**      

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Создает указанный индекс для той же файловой группы или схемы секционирования, к которой относится таблица или представление.

Слово "default" в этом контексте не является ключевым. Это идентификатор установленной по умолчанию файловой группы, который должен иметь разделители, как в выражениях ON **"** default **"** или ON **[** default **]** . Если указано значение "default" (по умолчанию), параметр QUOTED_IDENTIFIER должен иметь значение ON для текущего сеанса. Это параметр по умолчанию. Дополнительные сведения см. в описании [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

> [!NOTE]
> "default" не указывает файловую группу по умолчанию для базы данных в контексте CREATE INDEX. В случае с инструкцией CREATE TABLE поведение иное: значение "default" указывает расположение таблицы в файловой группе по умолчанию для базы данных.

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ]      

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше)

Указывает размещение данных FILESTREAM для таблицы при создании кластеризованного индекса. Предложение FILESTREAM_ON позволяет перемещать данные FILESTREAM в другую файловую группу FILESTREAM или схему секционирования.

Аргумент _filestream_filegroup_name_ указывает имя файловой группы FILESTREAM. В файловой группе должен быть определен один файл для файловой группы с помощью инструкции [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) или [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md), иначе возникает ошибка.

Если таблица секционирована, должно быть включено предложение FILESTREAM_ON и указана схема секционирования файловых групп FILESTREAM, использующая те же функции и столбцы секционирования, что и схема секционирования для таблицы. В противном случае произойдет ошибка.

Если таблица не секционирована, столбец FILESTREAM не может быть секционирован. Данные FILESTREAM для этой таблицы необходимо хранить в отдельной файловой группе, указанной в предложении FILESTREAM_ON.

Предложение `FILESTREAM_ON NULL` можно указать в инструкции CREATE INDEX, если создается кластеризованный индекс и таблица не содержит столбец FILESTREAM.

Дополнительные сведения см. в разделе [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md).

**\<object>::=**      

Полное или неполное имя индексируемого объекта.

_database_name_      
Имя базы данных.

*schema_name*      
Имя схемы, которой принадлежит таблица или представление.

*table_or_view_name*      
Имя индексируемой таблицы или представления.

Чтобы создать индекс для представления, это представление оно должно быть определено с параметром SCHEMABINDING. Прежде чем создавать любой некластеризованный индекс для представления, необходимо создать уникальный кластеризованный индекс. Дополнительные сведения об индексированных представлениях см. в разделе "Примечания".

Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], объект может быть таблицей, хранящейся в кластеризованном индексе.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] поддерживает формат трехкомпонентного имени _database_name_.[_schema_name_]._object_name_, если *database_name* обозначает текущую базу данных или _database_name_ имеет значение `tempdb` и _object_name_ начинается с символа #.

**\<relational_index_option\>::=**       
Указывает параметры, которые должны использоваться при создании индекса.

PAD_INDEX = { ON | **OFF** }      

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Определяет разреженность индекса. Значение по умолчанию — OFF.

ON      
Процент свободного места, определяемый параметром *fillfactor*, применяется к страницам индекса промежуточного уровня.

OFF или _fillfactor_ не указан      
Страницы промежуточного уровня заполняются почти полностью, при этом остается достаточно места по крайней мере для одной строки максимального размера, возможного в этом индексе при заданном наборе ключей на промежуточных страницах.

Параметр PAD_INDEX имеет смысл только в случае, если указан параметр FILLFACTOR, так как использует процентное значение, указанное в нем. Если процент, заданный аргументом FILLFACTOR, недостаточно велик для размещения одной строки, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] внутренне переопределит это значение, чтобы обеспечить минимум. Количество строк на странице индекса промежуточного уровня никогда не бывает менее двух даже при самых малых значениях аргумента _fillfactor_.

Для обратной совместимости синтаксиса аргумент WITH PAD_INDEX эквивалентен аргументу WITH PAD_INDEX = ON.

FILLFACTOR **=** _fillfactor_      

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Определяет величину в процентах, показывающую, насколько компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен заполнять конечный уровень каждой страницы индекса во время его создания или перестроения. Значение *fillfactor* должно быть целым числом от 1 до 100. Если параметр *fillfactor* равен 100, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] создает индексы с полностью заполненными страницами конечного уровня.

Аргумент FILLFACTOR действует только при создании или перестройке индекса. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не сохраняет динамически указанный процентный объем свободного места на страницах. Значение коэффициента заполнения можно увидеть в представлении каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

> [!IMPORTANT]
> Создание кластеризованного индекса с аргументом FILLFACTOR меньше 100 влияет на объем пространства хранения, занимаемого данными, т. к. компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] перераспределяет данные, когда создает кластеризованный индекс.

Дополнительные сведения см. в статье [Указание коэффициента заполнения для индекса](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).

SORT_IN_TEMPDB = { ON | **OFF** }      

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Указывает, сохранять ли временные результаты сортировки в базе данных **tempdb**. Значение по умолчанию — OFF, за исключением функции гипермасштабирования базы данных SQL Azure. Для всех операций построения индекса в режиме гипермасштабирования параметр SORT_IN_TEMPDB всегда включен независимо от указанного параметра, если не используется возобновляемое перестроение индекса.

ON      
Промежуточные результаты сортировки, которые используются при индексировании, хранятся в базе данных **tempdb**. Это может уменьшить время, необходимое для создания индекса, если база данных **tempdb** и база данных пользователя находятся на разных наборах дисков. Однако это увеличивает использование места на диске, которое используется при индексировании.

OFF      
Промежуточные результаты сортировки хранятся в той же базе данных, где и индекс.

Кроме места в базе данных пользователя, необходимого для создания индекса, требуется примерно столько же дополнительного места в базе данных **tempdb** для хранения промежуточных результатов сортировки. Дополнительные сведения см. в разделе [Параметр SORT_IN_TEMPDB для индексов](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).

Для обратной совместимости синтаксиса аргумент WITH SORT_IN_TEMPDB эквивалентен аргументу WITH SORT_IN_TEMPDB = ON.

IGNORE_DUP_KEY = { ON | **OFF** }      
Определяет ответ на ошибку, случающуюся, когда операция вставки пытается вставить в уникальный индекс повторяющиеся значения ключа. Параметр IGNORE_DUP_KEY применяется только к операциям вставки, производимым после создания или перестроения индекса. Параметр не работает во время выполнения инструкции [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) или [UPDATE](../../t-sql/queries/update-transact-sql.md). Значение по умолчанию — OFF.

ON      
Если в уникальный индекс вставляются повторяющиеся значения ключа, выводится предупреждающее сообщение. С ошибкой завершаются только строки, нарушающие ограничение уникальности.

OFF      
Если в уникальный индекс вставляются повторяющиеся значения ключа, выводится сообщение об ошибке. Будет выполнен откат всей операции INSERT.

IGNORE_DUP_KEY нельзя установить в значение ON для индексов, создаваемых для представлений, неуникальных индексов, XML-индексов, пространственных индексов и фильтруемых индексов.

Для просмотра значения IGNORE_DUP_KEY используйте [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

Для обратной совместимости синтаксиса аргумент WITH IGNORE_DUP_KEY эквивалентен аргументу WITH IGNORE_DUP_KEY = ON.

STATISTICS_NORECOMPUTE = { ON | **OFF**}      
Указывает, выполнялся ли перерасчет статистики распределения. Значение по умолчанию — OFF.

ON      
Устаревшие статистики не пересчитываются автоматически.

OFF      
Автоматическое обновление статистических данных включено.

Чтобы восстановить автоматическое обновление статистики, следует установить STATISTICS_NORECOMPUTE в значение OFF или выполнить UPDATE STATISTICS без предложения NORECOMPUTE.

> [!IMPORTANT]
> Отключение автоматического пересчета статистики распределения может помешать оптимизатору запросов выбрать оптимальные планы выполнения запросов, обращенных к таблице.

Для обратной совместимости синтаксиса аргумент WITH STATISTICS_NORECOMPUTE эквивалентен аргументу WITH STATISTICS_NORECOMPUTE = ON.

STATISTICS_INCREMENTAL = { ON | **OFF** }     

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

При значении **ON** статистики создаются как статистики отдельно по секциям. При значении **OFF** дерево статистик удаляется и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] повторно вычисляет статистики. Значение по умолчанию — **OFF**.

Если статистики по секциям не поддерживаются, параметр пропускается и выводится предупреждение. Добавочные статистики не поддерживаются для следующих типов статистических данных.

- Статистики, созданные с индексами, не выровненными по секциям для базовой таблицы.
- Статистики, созданные в доступных для чтения базах данных-получателях AlwaysOn.
- Статистики, созданные в базах данных, доступных только для чтения.
- Статистики, созданные по фильтрованным индексам.
- Статистика, созданная по представлениям.
- Статистики, созданные по внутренним таблицам.
- Статистики, созданные с пространственными индексами или XML-индексами.

DROP_EXISTING = { ON | **OFF** }      
Параметр для удаления и перестроения существующего кластеризованного или некластеризованного индекса с измененными спецификациями столбцов и сохранения того же имени для индекса. Значение по умолчанию — OFF.

ON      
Указывает удалить и перестроить существующий индекс, который должен иметь имя, совпадающее с именем параметра *index_name*.

OFF      
Указывает не удалять и перестраивать существующий индекс. SQL Server отображает ошибку, если индекс с указанным именем уже существует.

С помощью инструкции DROP_EXISTING можно изменить:

- некластеризованный индекс rowstore на кластеризованный индекс rowstore.

С помощью инструкции DROP_EXISTING нельзя изменить:

- кластеризованный индекс rowstore на некластеризованный индекс rowstore;
- кластеризованный индекс columnstore на индекс rowstore любого типа.

Для обратной совместимости синтаксиса аргумент WITH DROP_EXISTING эквивалентен аргументу WITH DROP_EXISTING = ON.

ONLINE = { ON | **OFF** }      
Определяет, будут ли базовые таблицы и связанные индексы доступны для запросов и изменения данных во время операций с индексами. Значение по умолчанию — OFF.

> [!IMPORTANT]
> Операции с индексами в сети доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).

ON      
Долгосрочные блокировки таблицы не поддерживаются во время операций с индексами. Во время главной фазы операций с индексами только блокировка с намерением совмещаемого доступа (IS) удерживается в исходной таблице. Это включает запросы или обновления применительно к обрабатываемой базовой таблице и индексам. В начале операции совмещаемая блокировка (S) удерживается на исходном объекте в течение очень короткого времени. В конце операции на источнике на короткое время удерживается совмещаемая блокировка (S), если создается некластеризованный индекс. Если в режиме в сети создается или удаляется кластеризованный индекс и, если перестраивается кластеризованный или некластеризованный индекс, удерживается блокировка SCH-M (изменения схемы). При создании индекса для временной локальной таблицы параметр ONLINE не может принимать значение ON.

OFF      
Блокировки таблиц применяются во время выполнения операций с индексами. Блокировку изменения схемы (Sch-M) в таблице получает операция с индексами вне сети, которая создает, перестраивает или удаляет кластеризованный индекс либо перестраивает или удаляет некластеризованный индекс. Это предотвращает доступ к базовой таблице всех пользователей во время операции. Операция с индексами вне сети, создающая некластеризованный индекс, получает совмещаемую блокировку (S) в таблице. Это запрещает проводить обновления базовой таблицы, но разрешает проводить операции чтения, например инструкции SELECT.

Дополнительные сведения см. в статье [Выполнение операции с индексами в сети](../../relational-databases/indexes/perform-index-operations-online.md).  

Индексы, в том числе индексы глобальных временных таблиц, могут быть созданы при подключении к сети, за исключением следующего:

- XML-индекс
- Индекс локальной временной таблицы
- Исходные уникальные кластеризованные индексы представлений.
- Отключенные кластеризованные индексы.
- Индексы columnstore
- Кластеризованные индексы, если базовая таблица содержит типы данных LOB: **image**, **ntext**, **text** и пространственные типы данных.
- Столбцы **varchar(max)** и **varbinary(max)** не могут быть частью индекса. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], если таблица содержит столбец **varchar(max)** или **varbinary(max)** , кластеризованный индекс, содержащий другие столбцы, можно построить или перестроить с использованием параметра **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не разрешает использовать параметр **ONLINE**, если базовая таблица содержит столбец **varchar(max)** или **varbinary(max)** .

Дополнительные сведения см. в разделе [Об операциях с индексами в режиме "в сети"](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}      

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

 Указывает, является ли операция с индексами в режиме "в сети" возобновляемой.

 ON      
Операция с индексами является возобновляемой.

 OFF      
Операция с индексами является невозобновляемой.

MAX_DURATION **=** *time* [**MINUTES**] используется с **RESUMABLE = ON** (требуется **ONLINE = ON**)   

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает время (целочисленное значение минутах), в течение которого выполняется возобновляемая операция с индексами в сети до приостановки.

> [!IMPORTANT]
> Более подробные сведения об операциях с индексами, которые можно выполнить в сети, см. в разделе [Рекомендации по операциям с индексами в сети](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Возобновляемое перестроение индекса с подключением к сети не поддерживается для индексов columnstore.

ALLOW_ROW_LOCKS = { **ON** | OFF }      
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Указывает, разрешена ли блокировка строк. Значение по умолчанию — ON.

ON      
Блокировки строк допустимы при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки.

OFF      
Блокировки строк не используются.

ALLOW_PAGE_LOCKS = { **ON** | OFF }      
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Указывает, разрешена ли блокировка страниц. Значение по умолчанию — ON.

ON      
Блокировки страниц возможны при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц.

OFF      
Блокировки страниц не используются.

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }      
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Определяет, следует ли выполнять оптимизацию, связанную с состязанием при операциях вставки на последнюю страницу. Значение по умолчанию — OFF. См. подробнее раздел о [последовательных ключах](#sequential-keys).

MAXDOP = *max_degree_of_parallelism*      
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Переопределяет параметр конфигурации **max degree of parallelism** на время выполнения операции с индексами. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.

Параметр *max_degree_of_parallelism* может иметь одно из следующих значений:

1      
Подавляет формирование параллельных планов.

\>1      
Ограничивает максимальное количество процессоров, используемых в параллельных операциях с индексами, заданным или меньшим числом в зависимости от текущей рабочей нагрузки системы.

0 (по умолчанию)      
В зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.

 Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статьях [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) и [Возможности, поддерживаемые различными выпусками SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

DATA_COMPRESSION      
Задает режим сжатия данных для указанного индекса, номера секции или диапазона секций. Существуют следующие варианты выбора.

None      
Индекс или заданные секции не сжимаются.

ROW      
Для индекса или заданных секций производится сжатие строк.

PAGE      
Для индекса или заданных секций производится сжатие страниц.

Дополнительные сведения о сжатии см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ..._n_ ] **)**       
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Указывает секции, к которым применяется параметр DATA_COMPRESSION. Если индекс не секционирован, аргумент ON PARTITIONS создаст ошибку. Если не указано предложение ON PARTITIONS, то параметр DATA_COMPRESSION применяется ко всем секциям секционированного индекса.

\<partition_number_expression> можно указать одним из следующих способов.

- Указав номер секции, например ON PARTITIONS (2).
- Указав номера нескольких секций, разделив их запятыми, например ON PARTITIONS (1, 5).
- Указав диапазоны секций и отдельные секции, например ON PARTITIONS (2, 4, 6 TO 8).

\<range> можно указать в виде номеров секций, разделенных ключевым словом TO, например: `ON PARTITIONS (6 TO 8)`.

 Чтобы для разных секций задать разные типы сжатия данных, укажите параметр DATA_COMPRESSION несколько раз, например следующим образом.

```sql
REBUILD WITH
(
  DATA_COMPRESSION = NONE ON PARTITIONS (1),
  DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
  DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
);
```

## <a name="remarks"></a>Remarks
Инструкция CREATE INDEX оптимизируется, как и любой другой запрос. Для уменьшения числа операций ввода-вывода обработчик запросов может вместо таблицы просматривать другой индекс. В некоторых ситуациях можно отказаться от операций сортировки. На многопроцессорных компьютерах инструкция CREATE INDEX, как и другие запросы, может использовать больше процессоров для операций просмотра и сортировки, связанных с созданием индекса. Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).

Если в качестве модели восстановления базы данных используется модель с неполным протоколированием или простая модель, операция создания индекса может выполняться с минимальным протоколированием.

Индексы могут создаваться для временной таблицы. При удалении таблицы или в конце сеанса такие индексы удаляются.

Кластеризованный индекс можно создать на основе табличной переменной при создании первичного ключа. После завершения выполнения запроса или окончания сеанса индекс уничтожается.

Индексы поддерживают расширенные свойства.

## <a name="clustered-indexes"></a>Кластеризованные индексы
Чтобы создать кластеризованный индекс для таблицы (кучи) или удалить и повторно создать существующий кластеризованный индекс, требуется дополнительная рабочая область в базе данных для сортировки и временного копирования данных исходной таблицы или существующего кластеризованного индекса. Дополнительные сведения о кластеризованных индексах см. в статьях [Создание кластеризованных индексов](../../relational-databases/indexes/create-clustered-indexes.md) и [Руководство по архитектуре и разработке индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="nonclustered-indexes"></a>Некластеризованные индексы
Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], вы можете создавать некластеризованный индекс в таблице, сохраненной в виде кластеризованного индекса columnstore. Если сначала создать некластеризованный индекс в таблице, сохраненной в виде кучи или кластеризованного индекса, индекс сохранится после дальнейшего преобразования таблицы в кластеризованный индекс columnstore. Кроме того, необязательно удалять некластеризованный индекс при перестройке кластеризованного индекса columnstore.

Ограничения

- Параметр `FILESTREAM_ON` является недопустимым при создании некластеризованного индекса в таблице, сохраненной в виде кластеризованного индекса columnstore.

## <a name="unique-indexes"></a>Уникальные индексы
Если существует уникальный индекс, то каждый раз при добавлении данных с помощью операции вставки компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] делает проверку на появление повторяющихся значений. Производится откат операций вставки, которые могли бы создать повторяющиеся значения ключей, и компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдает сообщение об ошибке. Это происходит даже в случае, если операция вставки изменяет несколько строк, а повторяющееся значение может появиться всего одно. Если делается попытка ввести данные, для которых существует уникальный индекс, и предложение IGNORE_DUP_KEY имеет значение ON, сбоем завершаются операции только с теми строками, где нарушается свойство уникальности индекса.

## <a name="partitioned-indexes"></a>Секционированные индексы
Секционированные индексы создаются и поддерживаются так же, как и секционированные таблицы, но обрабатываются как отдельные объекты базы данных подобно обычным индексам. Можно создать секционированный индекс для несекционированной таблицы и несекционированный индекс для секционированной таблицы.

Если создается индекс для секционированной таблицы и не указывается файловая группа, в которую должен быть помещен индекс, индекс секционируется так же, как и базовая таблица. Дело в том, что по умолчанию индексы помещаются в те же файловые группы, что и их базовые таблицы, а в случае секционированной таблицы — в схему секционирования, использующую те же самые столбцы секционирования. Когда индекс использует ту же схему и столбец секционирования, что и таблица, индекс *выравнивается* с таблицей.

> [!WARNING]
> Создание и перестройка невыровненных индексов для таблицы, количество секций в которой превышает 1000, возможны, но не поддерживаются. Это может привести к снижению производительности или чрезмерному потреблению памяти во время таких операций. Если количество секций превышает 1000, рекомендуется использовать только выровненные индексы.

Если секционируется неуникальный кластеризованный индекс, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] по умолчанию добавляет столбцы секционирования в список кластеризованных ключей индекса, если они еще не заданы.

Индексированные представления могут создаваться для секционированных таблиц таким же образом, как и индексы для таблиц. Дополнительные сведения о секционированных индексах см. в статьях [Секционированные таблицы и индексы](../../relational-databases/partitions/partitioned-tables-and-indexes.md) и [Руководство по архитектуре и разработке индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md).

Статистические данные в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] не создаются путем сканирования всех строк таблицы при создании или перестроении секционированного индекса. Вместо этого оптимизатор запросов использует для создания статистики алгоритм выборки по умолчанию. Для получения статистики по секционированным индексам путем сканирования всех строк таблицы используйте инструкции `CREATE STATISTICS` или `UPDATE STATISTICS` с предложением `FULLSCAN`.

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
  
     * Если уровень совместимости базы данных равен 90 или более, при установке параметра ANSI_WARNINGS в состояние ON параметр ARITHABORT также устанавливается в состояние ON. Если уровень совместимости базы данных установлен в состояние 80 или более раннее, то параметр ARITHABORT необходимо явным образом установить в состояние ON.

Если параметры SET неверны, может произойти следующее.

- Отфильтрованный индекс не будет создан.
- Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] сформирует ошибку и выполнит откат любой инструкции INSERT, UPDATE, DELETE или MERGE, которая изменила значения данных в индексе.
- Оптимизатор запросов не учтет индекс в плане выполнения любой инструкции Transact-SQL.

 Дополнительные сведения об отфильтрованных индексах см. в статьях [Создание отфильтрованных индексов](../../relational-databases/indexes/create-filtered-indexes.md) и [Руководство по архитектуре и разработке индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="spatial-indexes"></a>Пространственные индексы
Сведения о пространственных индексах см. в описании [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md) и статье [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).

## <a name="xml-indexes"></a>XML-индексы
Сведения об XML-индексах см. в описании [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md) и статье [XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md).

## <a name="index-key-size"></a>Размер ключа индекса
Максимальный размер ключа индекса составляет 900 байт для кластеризованного индекса и 1700 байт для некластеризованного индекса. (До [!INCLUDE[ssSDS](../../includes/sssds-md.md)] и [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] это ограничение всегда составляло 900 байт.) Индексы в столбцах **varchar**, размер которых превышает максимальный, могут быть созданы, если в момент создания индекса объем существующих данных в столбцах не превышает максимальный, но последующие операции вставки или обновления, вызывающие превышение общего максимального размера, будут заканчиваться ошибкой. Ключ кластеризованного индекса не может включать в себя столбцы **varchar** , для которых существуют данные в единице размещения ROW_OVERFLOW_DATA. Если кластеризованный индекс создается для столбца типа **varchar** и существующие данные располагаются в единице размещения IN_ROW_DATA, то все последующие операции вставки или обновления для данного столбца, выталкивающие данные за пределы строки, будут завершаться ошибкой.

Некластеризованные индексы могут включать неключевые столбцы на конечном уровне индекса. При вычислении размера ключа индекса компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] эти столбцы не рассматривает. Дополнительные сведения см. в статьях [Создание индексов с включенными столбцами](../../relational-databases/indexes/create-indexes-with-included-columns.md) и [Руководство по архитектуре и разработке индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md).

> [!NOTE]
> Если ключевые столбцы секционирования не представлены в неуникальном кластеризованном индексе при секционировании таблиц, то они добавляются в индекс службами [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Объединенный размер индексированных столбцов (без учета включенных столбцов) и любых добавленных столбцов секционирования в неуникальном кластеризованном индексе не может превышать 1800 байт.

## <a name="computed-columns"></a>Вычисляемые столбцы
Индексы могут создаваться в вычисляемых столбцах. Кроме того, вычисляемые столбцы могут иметь свойство PERSISTED. Это значит, что компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] хранит вычисляемые значения в таблице и обновляет их при изменении любого столбца, от которого зависит вычисляемый столбец. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] использует эти сохраненные значения при создании индекса столбца и при появлении ссылки на этот столбец в запросе.

Для индексации вычисляемого столбца этот вычисляемый столбец должен быть детерминированным и точным. Если используется свойство PERSISTED, список типов индексируемых вычисляемых столбцов расширяется и включает следующее.

- Вычисляемые столбцы, основанные на выражениях языка [!INCLUDE[tsql](../../includes/tsql-md.md)], функциях CLR и методах определяемых пользователем типов данных CLR, помеченных пользователем как детерминированные.
- Вычисляемые столбцы, основанные на выражениях, которые определены компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] как детерминированные, но не являются точными.

Для материализованных вычисляемых столбцов необходимо, чтобы следующие параметры SET имели значения, указанные выше в разделе [Обязательные параметры SET для отфильтрованных индексов](#required-set-options-for-filtered-indexes).

Ограничения UNIQUE или PRIMARY KEY могут содержать вычисляемый столбец, если он удовлетворяет всем условиям для индексирования. Вычисляемый столбец должен быть детерминированным и точным или детерминированным и сохраняемым. Дополнительные сведения о детерминизме см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

Вычисляемые столбцы, производные от типов данных **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** и **xml**, могут индексироваться как ключевой или неключевой столбец, если тип данных вычисляемого столбца допускается в качестве ключевого или неключевого столбца индекса. Например, нельзя создать первичный XML-индекс для вычисляемого столбца типа **xml**. Если размер ключа индекса превышает 900 байт, выдается предупреждение.

Создание индекса на вычисляемом столбце может привести к ошибке в операциях вставки или обновления, которые до этого успешно выполнялись. Такое неуспешное завершение возможно, если вычисляемый столбец вызывает арифметическую ошибку. Например, вычисляемый столбец `c` в следующей таблице приводит к арифметической ошибке, но инструкция INSERT работает нормально.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
INSERT INTO t1 VALUES (1, 0);
```

Если же после создания таблицы создать индекс на вычисляемом столбце `c`, та же инструкция `INSERT` будет заканчиваться ошибкой.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
CREATE UNIQUE CLUSTERED INDEX Idx1 ON t1(c);
INSERT INTO t1 VALUES (1, 0);
```

Дополнительные сведения см. в разделе [Индексы вычисляемых столбцов](../../relational-databases/indexes/indexes-on-computed-columns.md).

## <a name="included-columns-in-indexes"></a>Включенные столбцы в индексах
Неключевые столбцы, называемые "включенными столбцами", могут добавляться на конечный уровень некластеризованного индекса для повышения производительности запроса благодаря тому, что индекс включает все необходимые данные для запроса. Т. е. все столбцы, указанные в запросе, включаются в индекс в качестве ключевых или неключевых столбцов. Таким образом, оптимизатор запросов может найти все необходимые данные путем просмотра индекса, не обращаясь к данным таблицы или кластеризованного индекса. Дополнительные сведения см. в статьях [Создание индексов с включенными столбцами](../../relational-databases/indexes/create-indexes-with-included-columns.md) и [Руководство по архитектуре и разработке индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="specifying-index-options"></a>Установка параметров индекса
На сервере [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] представлены новые параметры индексов и изменен способ установки параметров. Для обратной совместимости синтаксиса WITH *option_name* эквивалентно WITH **(** \<option_name> **= ON )** . Устанавливая параметры индекса, необходимо соблюдать следующие правила.

- Новые параметры индекса могут быть заданы только с помощью аргумента WITH (**_option\_name_ = ON | OFF**).
- Нельзя задавать параметры с помощью нового синтаксиса и совместимого старого в одной и той же инструкции. Например, инструкция с WITH (**DROP_EXISTING, ONLINE = ON**) вызовет ошибку.
- При создании XML-индекса параметры должны указываться с помощью аргумента WITH (**_option_name_= ON | OFF**).

## <a name="drop_existing-clause"></a>Предложение DROP_EXISTING
Предложение DROP_EXISTING может использоваться для перестроения индекса, добавления или удаления столбцов, изменения параметров, изменения порядка сортировки столбцов, а также изменения схемы секционирования или файловой группы.

Если индекс принудительно налагает ограничение PRIMARY KEY или UNIQUE и его определение никак не меняется, он удаляется и создается вновь с сохранением существующих ограничений. Но если изменить определение индекса, инструкция вызовет ошибку. Чтобы изменить ограничение PRIMARY KEY или UNIQUE, удалите ограничение и добавьте ограничение вместе с новым определением.

Предложение DROP_EXISTING повышает производительность, если повторно создается кластеризованный индекс с тем же самым или другим набором ключей на таблице, имеющей также некластеризованные индексы. Предложение DROP_EXISTING заменяет удаление старого кластеризованного индекса с помощью инструкции DROP INDEX и последующее создание нового кластеризованного индекса с помощью инструкции CREATE INDEX. Некластеризованные индексы перестраиваются один раз, а после этого только в случае, если меняется определение индекса. Предложение DROP_EXISTING не перестраивает некластеризованные индексы, если определение индекса содержит то же самое имя индекса, ключевые столбцы, столбцы секционирования, атрибут уникальности и порядок сортировки, что и исходный индекс.

Независимо от того, перестраиваются ли некластеризованные индексы, они всегда остаются в своих исходных файловых группах или схемах секционирования и используют исходные функции секционирования. Если кластеризованный индекс перестраивается в другой файловой группе или схеме секционирования, некластеризованные индексы не перемещаются вместе с кластеризованным индексом. Поэтому даже если некластеризованные индексы раньше были выровнены по кластеризованному, теперь это может быть не так. Дополнительные сведения о выравнивании секционированных индексов см. в разделе [Секционированные таблицы и индексы](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

Предложение DROP_EXISTING не сортирует данные заново, если те же ключевые столбцы индекса используются в том же порядке с тем же порядком сортировки по возрастанию или убыванию, за исключением случаев, когда инструкция индекса задает некластеризованный индекс и параметр ONLINE равен OFF. Если кластеризованный индекс отключен, операция CREATE INDEX WITH DROP_EXISTING должна выполняться с параметром ONLINE в значении OFF. Если некластеризованный индекс отключен и не связан с отключенным кластеризованным индексом, операция CREATE INDEX WITH DROP_EXISTING может выполняться с параметром ONLINE в значении OFF или ON.

> [!NOTE]
> Если удаляются или перестраиваются индексы со 128 или более экстентами, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] откладывает фактическое освобождение страниц и связанных с ними блокировок до фиксации транзакции.

## <a name="online-option"></a>Параметр ONLINE
Следующие правила применяются к операциям с индексами в режиме в сети.

- Во время выполнения операций с индексами в сети базовая таблица не может изменяться, усекаться или удаляться.
- Для операций с индексами требуется дополнительное временное место на диске.
- Обработка индексов в сети может выполняться для секционированных индексов, содержащих материализованные вычисляемые столбцы или включенные столбцы.

Дополнительные сведения см. в статье [Выполнение операции с индексами в сети](../../relational-databases/indexes/perform-index-operations-online.md).

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a> Возобновляемые операции с индексами
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Следующие правила применяются к операциям с возобновляемыми индексами.

- Операция создания индексов в подключенном режиме указывается как возобновляемая с помощью параметра `RESUMABLE = ON`.
- Параметр RESUMABLE не сохраняется в метаданных для указанного индекса и применяется только на время выполнения текущей инструкции DDL. Таким образом, для включения возобновляемости предложение `RESUMABLE = ON` должно быть указано явным образом.
- Параметр MAX_DURATION поддерживается только в том случае, если `RESUMABLE = ON`.
- Параметр MAX_DURATION при включенном параметре RESUMABLE задает интервал времени для создания индекса. По истечении этого времени операция создания индекса приостанавливается или завершается. Пользователь решает, когда можно будет возобновить создание приостановленного индекса. Значение **time** в минутах для MAX_DURATION должно быть больше 0 минут и меньше или равно 1 неделе (7 \* 24 \* 60 = 10 080 минут). Длинная пауза в операции с индексами может повлиять на производительность DML в конкретной таблице, а также на емкость диска базы данных, поскольку они оба индексируют исходное и только что созданное требуемое место на диске и должны быть обновлены во время операций DML. Если параметр MAX_DURATION пропускается, операция с индексами будет продолжаться вплоть до ее завершения или до момента возникновения сбоя.
- Чтобы немедленно приостановить операцию создания индекса, можно остановить текущую команду сочетанием клавиш CTRL+C либо выполнить команду [ALTER INDEX](alter-index-transact-sql.md) PAUSE или команду `KILL <session_id>`. Приостановленную команду можно возобновить командой [ALTER INDEX](alter-index-transact-sql.md).
- Повторное выполнение исходной инструкции CREATE INDEX для возобновляемого индекса автоматически возобновляет приостановленную операцию создания индекса.
- Параметр `SORT_IN_TEMPDB = ON` не поддерживается для возобновляемых индексов.
- Команду DDL с параметром `RESUMABLE = ON` невозможно выполнить внутри явной транзакции (она не может быть частью блока BEGIN TRAN… COMMIT).
- Чтобы прервать или возобновить создание либо перестроение индекса, используйте синтаксис T-SQL [ALTER INDEX](alter-index-transact-sql.md).

> [!NOTE]
> Команда DDL выполняется вплоть до завершения, приостанавливается или завершается ошибкой. Если команда приостанавливается, возникнет ошибка, указывающая на приостановку операции и невозможность завершения создания индекса. Дополнительные сведения о текущем состоянии индекса можно получить из [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Как и в случае выше, при сбое также будет выведено сообщение об ошибке.

Чтобы указать, что создание индекса выполняется как возобновляемая операция, и проверить текущее состояние выполнения, см. статью [index_resumable_operations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md).

#### <a name="resources"></a>Ресурсы
Для операции создания возобновляемого индекса в сети необходимы следующие ресурсы.

- Дополнительное место для хранения создаваемого индекса, включая время, когда индекс будет приостановлен.
- Дополнительная пропускная способность для журналов на период сортировки. Общее потребление пространства для журналов у возобновляемого индекса меньше по сравнению с обычной операцией создания индекса в сети. Кроме того, эта операция поддерживает усечение журнала во время выполнения.
- Состояние DDL, запрещающее изменения DDL.
- Очистка фантомных записей блокируется для встроенных индексов на весь период операции, в том числе пока она приостановлена.

#### <a name="current-functional-limitations"></a>Существующие функциональные ограничения
Для возобновляемых операций создания индексов отключены следующие функциональные возможности.

- После приостановки возобновляемой операции создания индекса в сети нельзя изменить исходное значение MAXDOP.
- Создание индекса, который содержит следующее:

  - вычисляемые столбцы или столбцы TIMESTAMP в качестве ключевых столбцов;
  - столбец LOB в качестве включенного столбца для создания возобновляемого индекса.
  - Фильтруемый индекс

## <a name="row-and-page-locks-options"></a>Параметры блокировок строк и страниц
Когда присвоены значения `ALLOW_ROW_LOCKS = ON` и `ALLOW_PAGE_LOCK = ON`, при доступе к индексу допустимы блокировки на уровне строк, страниц и таблиц. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выбирает соответствующую блокировку и может повышать уровень с блокировки строки или страницы до блокировки таблицы.

Если присвоены значения `ALLOW_ROW_LOCKS = OFF` и `ALLOW_PAGE_LOCK = OFF`, при доступе к индексу допустима только блокировка на уровне таблиц.

## <a name="sequential-keys"></a>Последовательные ключи
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

Состязание операций вставки на последнюю страницу — это распространенная проблема, влияющая на производительность. Она возникает, когда множество параллельных запросов пытается вставить строки в индекс с последовательным ключом. Индекс считается последовательным, если ведущий ключевой столбец содержит значения, которые всегда увеличиваются (или уменьшаются), например столбец идентификаторов или даты, в котором по умолчанию используется текущее значение даты и времени. Так как ключи вставляются последовательно, все новые строки будут вставляться в конце структуры индекса, т. е. на одной и той же странице. Это приводит к состязанию за страницы в памяти. В этой ситуации несколько потоков ожидают PAGELATCH_EX для указанной страницы.

Включение параметра OPTIMIZE_FOR_SEQUENTIAL_KEY для индекса оптимизирует ядро СУБД, позволяя повысить пропускную способность для операций вставки в индекс с высокой степенью параллелизма. Этот параметр предназначен для индексов с последовательным ключом, из-за которого возникает состояние состязания при операциях вставки на последнюю страницу. Но его также можно применять для индексов с критическими участками в других областях структуры индекса в виде сбалансированного дерева.

## <a name="viewing-index-information"></a>Просмотр сведений об индексах
Получить данные об индексах можно с помощью представлений каталогов, системных функций и системных хранимых процедур.

## <a name="data-compression"></a>Сжатие данных
Сжатие данных описывается в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md). Необходимо учесть следующие основные моменты.

- С помощью сжатия можно хранить больше строк в странице, максимальный размер строки при этом не изменяется.
- Неконечные страницы индекса не сжаты на уровне страниц, но могут быть сжаты на уровне строк.
- У каждого некластеризованного индекса индивидуальные настройки сжатия, которые не наследуются от базовой таблицы.
- При создании кластеризованного индекса в куче кластеризованный индекс наследует состояние сжатия кучи, если не указано другое состояние сжатия.

На секционированные индексы налагаются следующие ограничения.

- Если у таблицы есть невыровненные индексы, изменить настройку сжатия отдельной секции невозможно.
- Инструкция ALTER INDEX \<index> ... REBUILD PARTITION ... производит перестроение указанной секции индекса.
- Инструкция ALTER INDEX \<index> ... REBUILD WITH ... производит перестроение всех секций индекса.

Оценить состояние сжатия таблицы, индекса или секции можно с помощью хранимой процедуры [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).

## <a name="permissions"></a>Разрешения
Необходимо разрешение `ALTER` для таблицы или представления. Пользователь должен быть членом предопределенной роли сервера `sysadmin` или предопределенных ролей баз данных `db_ddladmin` или `db_owner`.

## <a name="limitations-and-restrictions"></a>Ограничения
В [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] невозможно создать:

- кластеризованный или некластеризованный индекс rowstore в таблице хранилища данных, если индекс columnstore уже существует. Это поведение отличается от SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], разрешающее сосуществование индексов rowstore и columnstore в одной таблице.
- невозможно создать индекс для представления.

## <a name="metadata"></a>Метаданные
Чтобы просмотреть сведения о существующих индексах, можно выполнить запрос к представлению каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

## <a name="version-notes"></a>Заметки о версии

[!INCLUDE[ssSDS](../../includes/sssds-md.md)] не поддерживает параметры файловой группы и файлового потока.

## <a name="examples-all-versions-uses-the-adventureworks-database"></a>Примеры: все версии Используется база данных AdventureWorks

### <a name="a-create-a-simple-nonclustered-rowstore-index"></a>A. Создание простого некластеризованного индекса rowstore
В следующем примере создается некластеризованный индекс для столбца `VendorID` таблицы `Purchasing.ProductVendor`.

```sql
CREATE INDEX IX_VendorID ON ProductVendor (VendorID);
CREATE INDEX IX_VendorID ON dbo.ProductVendor (VendorID DESC, Name ASC, Address DESC);
CREATE INDEX IX_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="b-create-a-simple-nonclustered-rowstore-composite-index"></a>Б. Создание простого некластеризованного составного индекса rowstore
В следующем примере создается некластеризованный составной индекс в столбцах `SalesQuota` и `SalesYTD` таблицы `Sales.SalesPerson`.

```sql
CREATE NONCLUSTERED INDEX IX_SalesPerson_SalesQuota_SalesYTD ON Sales.SalesPerson (SalesQuota, SalesYTD);
```

### <a name="c-create-an-index-on-a-table-in-another-database"></a>В. Создание индекса в таблице из другой базы данных
В следующем примере создается кластеризованный индекс для столбца `VendorID` таблицы `ProductVendor` в базе данных `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="d-add-a-column-to-an-index"></a>Г. Добавление столбца в индекс
В следующем примере создается индекс IX_FF с двумя столбцами из таблицы dbo.FactFinance. Следующая инструкция перестраивает индекс с еще одним столбцом и сохраняет существующее имя.

```sql
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey ASC, DateKey ASC);

-- Rebuild and add the OrganizationKey
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey, DateKey, OrganizationKey DESC)
  WITH (DROP_EXISTING = ON);
```

## <a name="examples-sql-server-azure-sql-database"></a>Примеры: SQL Server, база данных SQL Azure

### <a name="e-create-a-unique-nonclustered-index"></a>Д. Создание уникального некластеризованного индекса
В следующем примере создается уникальный некластеризованный индекс в столбце `Name` таблицы `Production.UnitMeasure` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Индекс требует уникальности данных, вставляемых в столбец `Name`.

```sql
CREATE UNIQUE INDEX AK_UnitMeasure_Name
  ON Production.UnitMeasure(Name);
```

Следующий запрос проверяет ограничение уникальности данных при попытке вставить строку с тем же значением, что и в уже существующей строке.

```sql
-- Verify the existing value.
SELECT Name FROM Production.UnitMeasure WHERE Name = N'Ounces';
GO

INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name, ModifiedDate)
  VALUES ('OC', 'Ounces', GETDATE());
```

В результате выдается сообщение об ошибке:

```
Server: Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object 'UnitMeasure' with unique index 'AK_UnitMeasure_Name'. The statement has been terminated.
```

### <a name="f-use-the-ignore_dup_key-option"></a>Е. Использование параметра IGNORE_DUP_KEY
В следующем примере демонстрируется влияние параметра `IGNORE_DUP_KEY` со значениями `ON` и `OFF` на операцию вставки нескольких строк во временную таблицу. В таблицу `#Test` вставляется одна строка, которая намеренно приведет к появлению повторяющихся значений при выполнении второй многострочной операции вставки `INSERT`. Счетчик строк таблицы возвращает количество вставленных строк.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = ON);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Ниже приведены результаты второй инструкции `INSERT`.

```cmd
Server: Msg 3604, Level 16, State 1, Line 5 Duplicate key was ignored.

Number of rows
--------------
38
```

Обратите внимание, что строки из таблицы `Production.UnitMeasure`, не нарушающие ограничение уникальности, были успешно вставлены. Было выдано предупреждение, и строка с повторяющимся значением не была вставлена, но отката всей транзакции не произошло.

Те же инструкции выполняются вновь, но теперь с аргументом `IGNORE_DUP_KEY`, равным `OFF`.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = OFF);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Ниже приведены результаты второй инструкции `INSERT`.

```
Server: Msg 2601, Level 14, State 1, Line 5
Cannot insert duplicate key row in object '#Test' with unique index
'AK_Index'. The statement has been terminated.

Number of rows
--------------
1
```

Обратите внимание, что ни одна из строк таблицы `Production.UnitMeasure` не была вставлена, хотя ограничение индекса `UNIQUE` было нарушено только одной строкой.

### <a name="g-using-drop_existing-to-drop-and-re-create-an-index"></a>Ж. Использование предложения DROP_EXISTING для удаления и повторного создания индекса
В следующем примере удаляется и создается повторно существующий индекс для столбца `ProductID` таблицы `Production.WorkOrder` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] с использованием параметра `DROP_EXISTING`. Указываются также параметры `FILLFACTOR` и `PAD_INDEX`.

```sql
CREATE NONCLUSTERED INDEX IX_WorkOrder_ProductID
  ON Production.WorkOrder(ProductID)
    WITH (FILLFACTOR = 80,
      PAD_INDEX = ON,
      DROP_EXISTING = ON);
GO
```

### <a name="h-create-an-index-on-a-view"></a>З. Создание индекса для представления
В следующем примере создаются представление и индекс этого представления. Включено два запроса, использующих созданное индексированное представление.

```sql
-- Set the options to support indexed views
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,
  QUOTED_IDENTIFIER, ANSI_NULLS ON;
GO

-- Create view with schemabinding
IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL
  DROP VIEW Sales.vOrders;
GO

CREATE VIEW Sales.vOrders
  WITH SCHEMABINDING
AS
  SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Revenue,
    OrderDate, ProductID, COUNT_BIG(*) AS COUNT
  FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o
  WHERE od.SalesOrderID = o.SalesOrderID
  GROUP BY OrderDate, ProductID;
GO

-- Create an index on the view
CREATE UNIQUE CLUSTERED INDEX IDX_V1
  ON Sales.vOrders (OrderDate, ProductID);
GO

-- This query can use the indexed view even though the view is
-- not specified in the FROM clause.
SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev,
  OrderDate, ProductID
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND ProductID BETWEEN 700 AND 800
    AND OrderDate >= CONVERT(DATETIME, '05/01/2002', 101)
GROUP BY OrderDate, ProductID
ORDER BY Rev DESC;
GO

-- This query can use the above indexed view
SELECT OrderDate, SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND DATEPART(mm, OrderDate) = 3
  AND DATEPART(yy, OrderDate) = 2002
GROUP BY OrderDate
ORDER BY OrderDate ASC;
GO
```

### <a name="i-create-an-index-with-included-non-key-columns"></a>И. Создание индекса с включенными (неключевыми) столбцами
В следующем примере создается некластеризованный индекс с одним ключевым столбцом (`PostalCode`) и четырьмя неключевыми столбцами (`AddressLine1`, `AddressLine2`, `City`, `StateProvinceID`). Далее следует запрос, все данные для которого есть в индексе. Прежде чем выводить индекс, выбранный оптимизатором запросов, выберите в меню **Запрос** среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] команду **Показать действительный план выполнения**.

```sql
CREATE NONCLUSTERED INDEX IX_Address_PostalCode
  ON Person.Address (PostalCode)
  INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);
GO

SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode
FROM Person.Address
WHERE PostalCode BETWEEN N'98000' and N'99999';
GO
```

### <a name="j-create-a-partitioned-index"></a>К. Создание секционированного индекса
В следующем примере создается некластеризованный секционированный индекс для `TransactionsPS1`, существующей схемы секционирования в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. В данном примере подразумевается, что образец секционированного индекса установлен.

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
CREATE NONCLUSTERED INDEX IX_TransactionHistory_ReferenceOrderID
  ON Production.TransactionHistory (ReferenceOrderID)
  ON TransactionsPS1 (TransactionDate);
GO
```

### <a name="k-creating-a-filtered-index"></a>Л. Создание отфильтрованного индекса
В следующем примере создается фильтрованный индекс для таблицы Production.BillOfMaterials в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Предикат фильтра может включать столбцы, не являющиеся ключевыми в отфильтрованном индексе. Предикат в примере выбирает только те строки, где EndDate не равно NULL.

```sql
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithEndDate"
  ON Production.BillOfMaterials (ComponentID, StartDate)
  WHERE EndDate IS NOT NULL;
```

### <a name="l-create-a-compressed-index"></a>М. Создание сжатого индекса
Следующий пример демонстрирует создание индекса для несекционированной таблицы с помощью сжатия строк.

```sql
CREATE NONCLUSTERED INDEX IX_INDEX_1
  ON T1 (C2)
  WITH (DATA_COMPRESSION = ROW);
GO
```

Следующий пример демонстрирует создание индекса для секционированной таблицы с помощью сжатия строк во всех секциях индекса.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (DATA_COMPRESSION = ROW);
GO
```

 Следующий пример демонстрирует создание индекса для секционированной таблицы с помощью сжатия страниц для секции `1` индекса и сжатия строк для секций индекса со `2` по `4`.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (
    DATA_COMPRESSION = PAGE ON PARTITIONS(1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2 TO 4)
  );
GO
```

### <a name="m-create-resume-pause-and-abort-resumable-index-operations"></a>Н. Создание, возобновление, приостановка и прерывание операций с возобновляемыми индексами
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx1 ON test_table (col1) WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx2 ON test_table (col2) WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx1 ON test_table PAUSE;
ALTER INDEX test_idx2 ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx1 ON test_table RESUME;
ALTER INDEX test_idx2 ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx1 ON test_table ABORT;
ALTER INDEX test_idx2 ON test_table ABORT;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="n-basic-syntax"></a>О. Базовый синтаксис
Создание, возобновление, приостановка и прерывание операций с возобновляемыми индексами       

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx ON test_table ABORT;
```

### <a name="o-create-a-nonclustered-index-on-a-table-in-the-current-database"></a>П. Создание некластеризованного индекса для таблицы в текущей базе данных
В приведенном ниже примере создается некластеризованный индекс по столбцу `VendorID` таблицы `ProductVendor`.

```sql
CREATE INDEX IX_ProductVendor_VendorID
  ON ProductVendor (VendorID);
```

### <a name="p-create-a-clustered-index-on-a-table-in-another-database"></a>Т. Создание индекса для таблицы из другой базы данных
В приведенном ниже примере создается некластеризованный индекс по столбцу `VendorID` таблицы `ProductVendor` в базе данных `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID
  ON Purchasing..ProductVendor (VendorID);
```
### <a name="q-create-an-ordered-clustered-index-on-a-table"></a>У. Создание кластеризованного индекса для таблицы  
В следующем примере создается упорядоченный кластеризованный индекс на столбцах `c1` и `c2` таблицы `T1` в базе данных `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T1 
ORDER (c1, c2);

```

### <a name="r-convert-a-cci-to-an-ordered-clustered-index-on-a-table"></a>Ф. Преобразование кластеризованного индекса columnstore в упорядоченный кластеризованный индекс таблицы  
В следующем примере существующий кластеризованный индекс columnstore преобразуется в упорядоченный кластеризованный индекс columnstore с именем `MyOrderedCCI` для столбцов `c1` и `c2` таблицы `T2` в базе данных `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T2
ORDER (c1, c2)
WITH (DROP_EXISTING = ON);

```

## <a name="see-also"></a>См. также:
[Руководство по архитектуре и разработке индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Выполнение операций с индексами в оперативном режиме](../../relational-databases/indexes/perform-index-operations-online.md)  
[Индексы и инструкция ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#indexes-and-alter-table)     
[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)     
[CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)     
[CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)     
[CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)      
[CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)     
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)     
[Data Types](../../t-sql/data-types/data-types-transact-sql.md)    
[DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)    
[DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)    
[XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)     
[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)     
[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)    
[sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)     
[EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)