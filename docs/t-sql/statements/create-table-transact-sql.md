---
description: Инструкция CREATE TABLE (Transact-SQL)
title: Инструкция CREATE TABLE (Transact-SQL)
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILESTREAM_TSQL
- TABLE
- CREATE_TABLE_TSQL
- CREATE TABLE
- FILESTREAM
- TABLE_TSQL
- FILESTREAM_ON
- FILESTREAM_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHECK constraints
- global temporary tables [SQL Server]
- local temporary tables [SQL Server]
- size [SQL Server], tables
- UNIQUE constraints [SQL Server], creating
- constraints [SQL Server], columns
- maximum number of indexes per table
- number of tables per database
- number of indexes per table
- table creation [SQL Server], CREATE TABLE
- temporary tables [SQL Server], creating
- table size [SQL Server]
- nullability [SQL Server]
- partitioned tables [SQL Server], creating
- DEFAULT definition
- null values [SQL Server], columns
- number of rows per table
- maximum number of columns per table
- FOREIGN KEY constraints, creating
- PRIMARY KEY constraints
- number of bytes per row
- CREATE TABLE statement
- number of columns per table
- maximum number of bytes per row
- data retention policy
ms.assetid: 1e068443-b9ea-486a-804f-ce7b6e048e8b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6a16e0db4d25dabd0d364eb4d6ec1efd22386f92
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099542"
---
# <a name="create-table-transact-sql"></a>Инструкция CREATE TABLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Создает новую таблицу в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

> [!NOTE]
> Синтаксис [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] см. в статье [CREATE TABLE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/statements/create-table-azure-sql-data-warehouse.md).

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="simple-syntax"></a>Простой синтаксис

```syntaxsql
-- Simple CREATE TABLE Syntax (common if not using options)
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } [ ,...n ] )
[ ; ]
```

## <a name="full-syntax"></a>Полный синтаксис

```syntaxsql
-- Disk-Based CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ AS FileTable ]
    ( {   <column_definition>
        | <computed_column_definition>
        | <column_set_definition>
        | [ <table_constraint> ] [ ,... n ]
        | [ <table_index> ] }
          [ ,...n ]
          [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
             , system_end_time_column_name ) ]
      )
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
    [ TEXTIMAGE_ON { filegroup | "default" } ]
    [ FILESTREAM_ON { partition_scheme_name
           | filegroup
           | "default" } ]
    [ WITH ( <table_option> [ ,...n ] ) ]
[ ; ]
  
<column_definition> ::=
column_name <data_type>
    [ FILESTREAM ]
    [ COLLATE collation_name ]
    [ SPARSE ]
    [ MASKED WITH ( FUNCTION = ' mask_function ') ]
    [ [ CONSTRAINT constraint_name ] DEFAULT constant_expression ]
    [ IDENTITY [ ( seed,increment ) ]
    [ NOT FOR REPLICATION ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
    [ ROWGUIDCOL ]
    [ ENCRYPTED WITH
        ( COLUMN_ENCRYPTION_KEY = key_name ,
          ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,
          ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ) ]
    [ <column_constraint> [, ...n ] ]
    [ <column_index> ]
  
<data_type> ::=
[ type_schema_name . ] type_name
    [ ( precision [ , scale ] | max |
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]
  
<column_constraint> ::=
[ CONSTRAINT constraint_name ]
{     { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( < index_option > [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
            | filegroup | "default" } ]
  
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
  
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
}
  
<column_index> ::=
 INDEX index_name [ CLUSTERED | NONCLUSTERED ]
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
<computed_column_definition> ::=
column_name AS computed_column_expression
[ PERSISTED [ NOT NULL ] ]
[
    [ CONSTRAINT constraint_name ]
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
        | filegroup | "default" } ]
  
    | [ FOREIGN KEY ]
        REFERENCES referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE } ]
        [ ON UPDATE { NO ACTION } ]
        [ NOT FOR REPLICATION ]
  
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
]
  
<column_set_definition> ::=
column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
  
< table_constraint > ::=
[ CONSTRAINT constraint_name ]
{
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        (column [ ASC | DESC ] [ ,...n ] )
        [
            WITH FILLFACTOR = fillfactor
           |WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name (partition_column_name)
            | filegroup | "default" } ]
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )

< table_index > ::=
{  
    {  
      INDEX index_name  [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ]
         (column_name [ ASC | DESC ] [ ,... n ] )
    | INDEX index_name CLUSTERED COLUMNSTORE
    | INDEX index_name [ NONCLUSTERED ] COLUMNSTORE (column_name [ ,... n ] )
    }
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
}

<table_option> ::=
{  
    [DATA_COMPRESSION = { NONE | ROW | PAGE }
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }
      [ , ...n ] ) ]]
    [ FILETABLE_DIRECTORY = <directory_name> ]
    [ FILETABLE_COLLATE_FILENAME = { <collation_name> | database_default } ]
    [ FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ] ]
    [ REMOTE_DATA_ARCHIVE =
      {
          ON [ ( <table_stretch_options> [,...n] ) ]
        | OFF ( MIGRATION_STATE = PAUSED )
      }
    ]   
    [ DATA_DELETION = ON  
          {( 
             FILTER_COLUMN = column_name,   
             RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                              | MONTH | MONTHS | YEAR | YEARS }
        )}  
     ]
}
  
<table_stretch_options> ::=
{  
    [ FILTER_PREDICATE = { null | table_predicate_function } , ]
      MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
 }   
  
<index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | COMPRESSION_DELAY= {0 | delay [Minutes]}
  | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }
       [ ON PARTITIONS ( { <partition_number_expression> | <range> }
       [ , ...n ] ) ]
}
<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

```
-- Memory optimized CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition>
    | [ <table_constraint> ] [ ,... n ]
    | [ <table_index> ]
      [ ,... n ] }
      [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
        , system_end_time_column_name ) ]
)
    [ WITH ( <table_option> [ ,... n ] ) ]
 [ ; ]

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
[
    [ CONSTRAINT constraint_name ] DEFAULT memory_optimized_constant_expression ]
    | [ IDENTITY [ ( 1, 1 ) ]
]
    [ <column_constraint> ]
    [ <column_index> ]
  
<data_type> ::=
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]

<column_constraint> ::=
 [ CONSTRAINT constraint_name ]
{
  { PRIMARY KEY | UNIQUE }
      {   NONCLUSTERED
        | NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count)
      }
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
  | CHECK ( logical_expression )
}
  
< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   { PRIMARY KEY | UNIQUE }
     {
       NONCLUSTERED (column [ ASC | DESC ] [ ,... n ])
       | NONCLUSTERED HASH (column [ ,... n ] ) WITH ( BUCKET_COUNT = bucket_count )
                    }
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
    | CHECK ( logical_expression )
}

<column_index> ::=
  INDEX index_name
{ [ NONCLUSTERED ] | [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count)}

<table_index> ::=
  INDEX index_name
{   [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
  
}
  
<table_option> ::=
{  
    MEMORY_OPTIMIZED = ON
  | DURABILITY = {SCHEMA_ONLY | SCHEMA_AND_DATA}
  | SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ]
  
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*database_name* — имя базы данных, в которой создается таблица. Параметр *database_name* должен указывать имя существующей базы данных. Если не указано, в качестве *database_name* по умолчанию выбирается текущая база данных. Имя входа для текущего соединения должно быть связано с идентификатором пользователя, существующего в базе данных, указанной аргументом *database_name*, а этот пользователь должен обладать разрешениями CREATE TABLE.

*schema_name* — имя схемы, которой принадлежит новая таблица.

*table_name* — имя новой таблицы. Имена таблиц должны соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Аргумент *table_name* может состоять не более чем из 128 символов, за исключением имен локальных временных таблиц (имена с префиксом из одного символа решетки #), длина которых не должна превышать 116 символов.

AS FileTable **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше).

Создает новую таблицу FileTable. Нет необходимости указывать столбцы, так как таблица FileTable имеет фиксированное схему. Дополнительные сведения см. в статье [Таблицы FileTable](../../relational-databases/blob/filetables-sql-server.md).

*column_name*
*computed_column_expression* — выражение, определяющее значение вычисляемого столбца. Вычисляемый столбец представляет собой виртуальный столбец, физически не хранящийся в таблице, если для него не установлен признак PERSISTED. Значение столбца вычисляется на основе выражения, использующего другие столбцы той же таблицы. Например, вычисляемый столбец может иметь определение **cost** AS **price** \* **qty**. Выражение может быть именем невычисляемого столбца, константой, функцией, переменной или любой их комбинацией, соединенной одним или несколькими операторами. Выражение не может быть вложенным запросом или содержать псевдонимы типов данных.

Вычисляемые столбцы могут использоваться в списках выбора, предложениях WHERE, ORDER BY и в любых других местах, в которых могут использоваться обычные выражения, за исключением следующих случаев.

- Для использования в ограничениях FOREIGN KEY или CHECK вычисляемые столбцы должны быть помечены как PERSISTED.
- Вычисляемый столбец может использоваться в качестве ключевого столбца в индексе или в качестве компонента какого-либо ограничения PRIMARY KEY или UNIQUE, если значение этого вычисляемого столбца определено детерминистическим выражением, а тип данных результата разрешен в столбцах индекса.

   Например, если таблица содержит целочисленные столбцы **a** и **b**, вычисляемый столбец **a+b** может быть включен в индекс, а вычисляемый столбец **a+DATEPART(dd, GETDATE())**  — не может, так как его значение может изменяться при последующих вызовах.

- Вычисляемый столбец не может быть целевым столбцом инструкций INSERT или UPDATE.

> [!NOTE]
> Каждая строка таблицы может содержать различные значения столбцов, задействованных в вычисляемом столбце; таким образом, значение вычисляемого столбца не будет одним и тем же в каждой строке.

Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] автоматически определяет для вычисляемых столбцов допустимость значений NULL на основе использованных выражений. Считается, что результат большинства выражений допускает значение NULL, даже если используются только столбцы, для которых значение NULL запрещено, так как при возможном переполнении или потере точности может получаться значение NULL. Для выяснения допустимости значения NULL в вычисляемом столбце таблицы используйте функцию `COLUMNPROPERTY` со свойством **AllowsNull**. Добиться того, чтобы выражение не допускало значения NULL, можно, указав `ISNULL` с константой *check_expression*, где константа представляет собой ненулевое значение, заменяющее любое значение NULL. Для вычисляемых столбцов, основанных на выражениях, содержащих определяемые пользователем типы среды CLR, требуется разрешение REFERENCES на тип.

PERSISTED — указывает, что компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] будет физически хранить вычисляемые значения в таблице и обновлять их при изменении любого столбца, от которого зависит вычисляемый столбец. Указание `PERSISTED` для вычисляемого столбца позволяет создать индекс по вычисляемому столбцу, который будет детерминированным, но не точным. Дополнительные сведения см. в разделе [Индексы вычисляемых столбцов](../../relational-databases/indexes/indexes-on-computed-columns.md). Все вычисляемые столбцы, используемые как столбцы секционирования в секционированной таблице, должны быть явно помечены как `PERSISTED`. Если указан параметр `PERSISTED`, значение *computed_column_expression* должно быть детерминированным.

ON { *partition_scheme* | *filegroup* |  **"default"** } Указывает схему секционирования или файловую группу, в которой хранится таблица. Если аргумент *partition_scheme* указан, таблица будет разбита на секции, хранимые в одной или нескольких файловых группах, указанных аргументом *partition_scheme*. Если указан аргумент *filegroup*, таблица сохраняется в файловой группе с таким именем. Это должна быть существующая файловая группа в базе данных. Если указано значение **"default"** или параметр ON не определен вообще, таблица сохраняется в файловой группе по умолчанию. Механизм хранения таблицы, указанный в инструкции CREATE TABLE, изменить в дальнейшем невозможно.

Параметр ON {*partition_scheme* | *filegroup* |  **"default"** } может также указываться в ограничении PRIMARY KEY или UNIQUE. С помощью этих ограничений создаются индексы. Если указан аргумент *filegroup*, индекс сохраняется в файловой группе с таким именем. Если указано значение **"default"** или параметр ON не определен вообще, индекс сохраняется в той же файловой группе, что и таблица. Если ограничение `PRIMARY KEY` или `UNIQUE` создает кластеризованный индекс, страницы данных таблицы сохраняются в той же файловой группе, что и индекс. Если ограничение создает кластеризованный индекс (с помощью параметра `CLUSTERED` или другим способом), а указанный аргумент *partition_scheme* отличается от аргументов *partition_scheme* и *filegroup* из определения таблицы либо наоборот, принимается во внимание только определение ограничения, а все остальное не учитывается.

> [!NOTE]
> В этом контексте *default* не является ключевым словом. Это идентификатор файловой группы по умолчанию, который должен иметь разделители, как в выражениях ON **"default"** или ON **[** default **]** . Если указано значение **"default"** , параметр `QUOTED_IDENTIFIER` должен иметь значение ON для текущего сеанса. Это параметр по умолчанию. Дополнительные сведения см. в описании [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).
>
> После создания секционированной таблицы рекомендуется присвоить параметру `LOCK_ESCALATION` для таблицы значения `AUTO`. При этом можно усовершенствовать параллелизм, разрешив укрупнение блокировок до уровня секции (HoBT) вместо таблицы. Дополнительные сведения см. в разделе [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).

TEXTIMAGE_ON { *filegroup*|  **"default"** } Указывает, что столбцы с типами **text**, **ntext**, **image**, **xml**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** и с определяемыми пользователем типами данных CLR (включая geometry и geography) хранятся в указанной файловой группе.

Параметр `TEXTIMAGE_ON` недопустим, если в таблице нет столбцов с большими значениями. Нельзя указывать параметр `TEXTIMAGE_ON` одновременно с параметром *partition_scheme*. Если указано значение **"default"** или параметр `TEXTIMAGE_ON` не определен вообще, столбцы с большими значениями сохраняются в файловой группе по умолчанию. Способ хранения любых данных столбцов с большими значениями, определенный инструкцией `CREATE TABLE`, изменить в дальнейшем невозможно.

> [!NOTE]
> Varchar(max), nvarchar(max), varbinary(max), xml и большие значения UDT хранятся прямо в строке данных до предельного размера в 8000 байт и пока значение умещается в записи. Если значение не умещается в записи, то указатель хранится в строке, а все остальное хранится вне строки в области хранения объектов LOB. 0 — это значение по умолчанию, указывающее, что все значения сохраняются прямо в строке данных.
>
> Параметр `TEXTIMAGE_ON` изменяет только расположение "пространства хранения объектов LOB", но не влияет на данные, хранящиеся в строке. Используйте параметр LARGE VALUE TYPES OUT OF ROW в sp_tableoption, чтобы хранить все значения LOB за пределами строки.
>
> В этом контексте default не является ключевым словом. Это идентификатор файловой группы по умолчанию, который должен иметь разделители, как в выражении `TEXTIMAGE_ON "default"` или `TEXTIMAGE_ON [default]`. Если указано значение **"default"** , параметр `QUOTED_IDENTIFIER` должен иметь значение ON для текущего сеанса. Это параметр по умолчанию. Дополнительные сведения см. в описании [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

FILESTREAM_ON { *partition_scheme_name* | filegroup | **"** default **"** } **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] и более поздних версий). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не поддерживает `FILESTREAM`.

Задает файловую группу для данных FILESTREAM.

Если таблица содержит данные FILESTREAM и является секционированной, необходимо включить предложение FILESTREAM_ON и указать схему секционирования файловых групп файлового потока. В этой схеме секционирования должны использоваться те же функции и столбцы секционирования, что и в схеме секционирования для таблицы; в противном случае возникает ошибка.

Если таблица не секционирована, столбец FILESTREAM не может быть секционирован. Данные FILESTREAM для таблицы должны храниться в отдельной файловой группе. Эта файловая группа указывается в предложении FILESTREAM_ON.

Если таблица не является секционированной и предложение `FILESTREAM_ON` не указано, используется файловая группа FILESTREAM, для которой задано свойство `DEFAULT`. При отсутствии файловой группы FILESTREAM возникает ошибка.

Как и в случае с предложениями ON и `TEXTIMAGE_ON`, значение, указанное с помощью инструкции `CREATE TABLE` для предложения `FILESTREAM_ON`, не может быть изменено, за исключением описанных ниже ситуаций.

- Инструкция [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) преобразует кучу в кластеризованный индекс. В этом случае можно указать другую файловую группу FILESTREAM, схему секционирования или значение NULL.
- Инструкция [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) преобразует кластеризованный индекс в кучу. В этом случае можно указать другую файловую группу FILESTREAM, схему секционирования или значение **"default"** .

Для файловой группы в предложении `FILESTREAM_ON <filegroup>` либо для каждой файловой группы FILESTREAM, упомянутой в схеме секционирования, должен быть определен файл. Этот файл должен быть определен с помощью инструкции[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) или [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md), иначе возникает ошибка.

Дополнительные сведения о FILESTREAM см. в [этой статье](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).

[ _type\_schema\_name_ **.** ] *type_name* — указывает тип данных столбца и схему, к которой он принадлежит. Дисковые таблицы могут иметь один из следующих типов данных:

- Системный тип данных.
- Псевдонимы типа на основе системного типа данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Прежде чем псевдонимы типов данных можно будет использовать в определении таблицы, их нужно создать с помощью инструкции `CREATE TYPE`. Состояние признака NULL или NOT NULL для псевдонима типа данных можно переопределить с помощью инструкции `CREATE TABLE`. Однако его длину изменить нельзя; длина типа данных-псевдонима не определяется инструкцией `CREATE TABLE`.
- Определяемый пользователем тип данных CLR. Прежде чем определяемые пользователем типы данных CLR можно будет использовать в определении таблицы, их нужно создать с помощью инструкции `CREATE TYPE`. Для создания столбца с определяемым пользователем типом данных CLR требуется разрешение REFERENCES на этот тип.

Если аргумент *type_schema_name* не указан, компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ссылается на аргумент *type_name* в следующем порядке:

- системный тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];
- в установленной по умолчанию для текущего пользователя схеме в текущей базе данных;
- Схема **dbo** в текущей базе данных.

Список системных типов данных, поддерживаемых оптимизированными для памяти таблицами, см. в разделе [Поддерживаемые типы данных для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).

*precision* — точность указанного типа данных. Дополнительные сведения о допустимых значениях точности см. в разделе [Точность, масштаб и длина](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale* — масштаб указанного типа данных. Дополнительные сведения о допустимых значениях масштаба см. в разделе [Точность, масштаб и длина](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max** — применяется только к типам данных **varchar**, **nvarchar** и **varbinary** для хранения 2^31 байт символьных и двоичных данных или 2^30 байт данных в Юникоде.

CONTENT — определяет, что каждый экземпляр типа данных **xml** в *column_name* может включать в себя несколько элементов верхнего уровня. Аргумент CONTENT применим только к данным типа **xml** и может быть указан только в том случае, если одновременно указан аргумент *xml_schema_collection*. Если этот параметр не указан, CONTENT принимается в качестве поведения по умолчанию.

DOCUMENT — определяет, что каждый экземпляр типа данных **xml** в *column_name* может включать в себя только один элемент верхнего уровня. Аргумент DOCUMENT применим только к данным типа **xml** и может быть указан только в том случае, если одновременно указан аргумент *xml_schema_collection*.

*xml_schema_collection* — применяется только к типу данных **xml** для коллекции схем XML, связанной с этим типом. Перед помещением столбца **xml** в схему она должна быть создана в базе данных при помощи инструкции [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).

DEFAULT — указывает значение, присваиваемое столбцу, если значение явно не задано при вставке. Определения DEFAULT могут применяться к любым столбцам, кроме имеющих тип **timestamp** или обладающих свойством `IDENTITY`. Если значение по умолчанию указывается для столбца определяемого пользователем типа, этот тип должен поддерживать неявное преобразование выражения *constant_expression* в определяемый пользователем тип. Определения DEFAULT удаляются, когда таблица удаляется из памяти. В качестве значения по умолчанию могут использоваться только константы (например, символьные строки), скалярные функции (системные, определяемые пользователем или функции CLR) или значение NULL. Для сохранения совместимости с более ранними версиями сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] значению DEFAULT может быть присвоено имя ограничения.

*constant_expression* — константа, значение NULL или системная функция, используемая в качестве значения столбца по умолчанию.

*memory_optimized_constant_expression* — константа, значение NULL или системная функция, которая поддерживается в качестве значения столбца по умолчанию. Должно поддерживаться для хранимых процедур, скомпилированных в собственном коде. Дополнительные сведения о встроенных функциях в хранимых процедурах, компилируемых в собственном коде, см. в разделе [Поддерживаемые функции для модулей, скомпилированных в собственном коде T-SQL](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).

IDENTITY — указывает, что новый столбец является столбцом идентификаторов. При добавлении в таблицу новой строки компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] формирует для этого столбца уникальное последовательное значение. Столбцы идентификаторов обычно используются с ограничением PRIMARY KEY для поддержания уникальности идентификаторов строк в таблице. Свойство `IDENTITY` может назначаться столбцам типа **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** или **numeric(p,0)** . Для каждой таблицы можно создать только один столбец идентификаторов. Ограниченные значения по умолчанию и ограничения DEFAULT не могут использоваться в столбце идентификаторов. Необходимо указать как начальное значение, так и приращение, или же не указывать ничего. Если ничего не указано, применяется значение по умолчанию (1,1).

*seed* — значение, которое будет использовано для первой загруженной в таблицу строки.

*increment* — значение приращения, добавляемое к значению идентификатора предыдущей загруженной строки.

NOT FOR REPLICATION В инструкции `CREATE TABLE` предложение `NOT FOR REPLICATION` может указываться для свойства IDENTITY, а также ограничений FOREIGN KEY и CHECK. Если это предложение указано для свойства `IDENTITY`, значения в столбцах идентификаторов не приращиваются, если вставку выполняют агенты репликации. Если ограничение сопровождается этим предложением, оно не выполняется, когда агенты репликации выполняют операции вставки, обновления или удаления.

GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] [ NOT NULL ] **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Определяет, что указанный столбец типа `datetime2` будет использоваться системой для записи времени начала или окончания действия записи. Столбец должен быть определен как `NOT NULL`. Если вы попытаетесь указать `NULL`, система выдаст ошибку. Если вы явно не укажете NOT NULL для столбца периода, система определит столбец как `NOT NULL` по умолчанию. Используйте этот аргумент в сочетании с аргументами `PERIOD FOR SYSTEM_TIME` и `WITH SYSTEM_VERSIONING = ON`, чтобы выключить системное управление версиями в таблице. Дополнительные сведения см. в разделе [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

Вы можете пометить один или оба столбца периода флагом **HIDDEN**, чтобы эти столбцы были скрыты и инструкция **SELECT \* FROM** _`<table>`_ не возвращала значения этих столбцов. По умолчанию столбцы периода не скрыты. Чтобы использовать скрытые столбцы, их необходимо явно указывать во всех запросах, обращающихся к темпоральной таблице. Чтобы изменить атрибут **HIDDEN** для существующего столбца периода, необходимо отбросить атрибут **PERIOD** и заново создать его с другим флагом HIDDEN.

INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] ) **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Задает создание индекса для таблицы. Это может быть кластеризованный или некластеризованный индекс. Индекс будет содержать указанные столбцы и сортировать данные по возрастанию или убыванию.

INDEX *index_name* CLUSTERED COLUMNSTORE **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Задает сохранение всей таблицы в виде столбцов с кластеризованным индексом columnstore. Сюда всегда входят все столбцы в таблице. Данные не сортируются в алфавитном или числовом порядке, поскольку организация строк позволяет использовать преимущества сжатия columnstore.

INDEX *index_name* [ NONCLUSTERED ] COLUMNSTORE (*column_name* [ ,... *n* ] ) **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Задает создание некластеризованного индекса columnstore в таблице. Базовая таблица может быть кучей rowstore или кластеризованным индексом или кластеризованным индексом columnstore. В любом случае при создании некластеризованного индекса columnstore в таблице сохраняется вторая копия данных для столбцов в индексе.

Некластеризованный индекс columnstore хранится и управляется как кластеризованный индекс columnstore. Он называется некластеризованным индексом columnstore, потому что столбцы могут быть ограничены и он существует как дополнительный индекс в таблице.

ON _partition\_scheme\_name_ **(** _column\_name_ **)**  — задает схему секционирования, которая определяет файловые группы, соответствующие секциям секционированного индекса. Схема секционирования должна быть создана в базе данных путем выполнения инструкции [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) или [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). *column_name* указывает столбец, по которому будет секционирован индекс. Столбец должен соответствовать по типу данных, длине и точности аргументу функции секционирования, используемой аргументом *partition_scheme_name*. Аргумент *column_name* необязательно должен соответствовать столбцам из определения индекса. Можно указать любой столбец базовой таблицы, за исключением случая секционирования индекса UNIQUE, когда аргумент *column_name* должен быть выбран из используемых в качестве уникального ключа. Это ограничение дает возможность компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] проверять уникальность значений ключа только в одной секции.

> [!NOTE]
> При секционировании неуникального кластеризованного индекса компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] по умолчанию добавляет столбец секционирования в список ключей кластеризованного индекса, если этого столбца еще нет в списке. При секционировании неуникального некластеризованного индекса компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] добавляет столбец секционирования как неключевой (включенный) столбец индекса, если этого столбца еще нет в списке.

Если аргумент *partition_scheme_name* или *filegroup* не задан и таблица секционирована, индекс помещается в ту же схему секционирования и с тем же столбцом секционирования, что и для базовой таблицы.

> [!NOTE]
> Для XML-индекса задать схему секционирования невозможно. Если базовая таблица секционирована, XML-индекс использует ту же схему секционирования, что и таблица.

Дополнительные сведения об индексах секционирования см. в разделе [Секционированные таблицы и индексы](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON *filegroup_name* — позволяет создать заданный индекс в указанной файловой группе. Если местоположение не указано и таблица или представление не секционированы, индекс использует ту же файловую группу, что и базовая таблица или базовое представление. Файловая группа должна существовать.

ON **"default"**  — позволяет создать заданный индекс в файловой группе, используемой по умолчанию.

Слово "default" в этом контексте не является ключевым. Это идентификатор файловой группы по умолчанию, который должен иметь разделители, как в выражениях ON **"default"** или ON **[default]** . Если указано значение "default", параметр `QUOTED_IDENTIFIER` должен иметь значение ON для текущего сеанса. Это параметр по умолчанию. Дополнительные сведения см. в описании [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ] **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] и более поздних версий).

Указывает размещение данных FILESTREAM для таблицы при создании кластеризованного индекса. Предложение FILESTREAM_ON позволяет перемещать данные FILESTREAM в другую файловую группу FILESTREAM или схему секционирования.

Аргумент *filestream_filegroup_name* указывает имя файловой группы FILESTREAM. В файловой группе должен быть определен один файл для файловой группы с помощью инструкции [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) или [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md), иначе возникает ошибка.

Если таблица секционирована, должно быть включено предложение `FILESTREAM_ON` и указана схема секционирования файловых групп FILESTREAM, использующая ту же функцию и те же столбцы секционирования, что и схема секционирования для таблицы. В противном случае произойдет ошибка.

Если таблица не секционирована, столбец FILESTREAM не может быть секционирован. Данные FILESTREAM для этой таблицы необходимо хранить в отдельной файловой группе, указанной в предложении `FILESTREAM_ON`.

Предложение `FILESTREAM_ON NULL` можно указать в инструкции `CREATE INDEX`, если создается кластеризованный индекс и таблица не содержит столбец FILESTREAM.

Дополнительные сведения см. в статье [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md).

ROWGUIDCOL — указывает, что новый столбец является столбцом идентификатора GUID строки. Только один столбец типа **uniqueidentifier** в таблице может быть назначен в качестве столбца ROWGUIDCOL. Применение свойства ROWGUIDCOL позволяет ссылаться на столбец с помощью ключевого слова `$ROWGUID`. Свойство ROWGUIDCOL может быть присвоено только столбцу типа **uniqueidentifier**. Ключевым словом ROWGUIDCOL нельзя обозначать столбцы определяемых пользователем типов данных.

Свойство ROWGUIDCOL не обеспечивает уникальности значений, хранимых в столбце. Кроме того, при указании данного свойства автоматического формирования значений для новых строк, вставляемых в таблицу, не выполняется. Для создания уникальных значений в каждом столбце следует использовать функцию [NEWID](../../t-sql/functions/newid-transact-sql.md) или [NEWSEQUENTIALID](../../t-sql/functions/newsequentialid-transact-sql.md) в инструкциях [INSERT](../../t-sql/statements/insert-transact-sql.md) либо использовать эти функции по умолчанию для столбца.

ENCRYPTED WITH — указывает столбцы шифрования с помощью функции [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

COLUMN_ENCRYPTION_KEY = *key_name* — указывает ключ шифрования столбца. Дополнительные сведения см. в [этой статье](../../t-sql/statements/create-column-encryption-key-transact-sql.md).

ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } — для **детерминированного шифрования** используется метод, с помощью которого всегда создается одно и то же зашифрованное значение для конкретного текстового значения. Это позволяет выполнять поиск с помощью сравнения на равенство, группирование и объединение таблиц по зашифрованным значениям. При этом несанкционированные пользователи могут определять некоторую информацию о зашифрованных значениях путем анализа повторов в зашифрованном столбце. Соединить две таблицы по столбцам с детерминированным шифрованием можно только в том случае, если оба столбца шифруются с помощью одного ключа шифрования столбца. При использовании детерминированного шифрования необходимо указать порядок сортировки binary2 в параметрах сортировки для символьных столбцов.

**Случайное шифрование** использует метод, который шифрует данные менее предсказуемым образом. Случайное шифрование более безопасное, но не предусматривает вычисления и индексацию в зашифрованных столбцах, если экземпляр SQL Server не поддерживает функцию Always Encrypted с безопасными анклавами. См. сведения о функции [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

Если вы используете функцию Always Encrypted (без безопасных анклавов), к столбцам, поиск которых осуществляется на основе параметров или параметров группирования, например номер внутреннего паспорта, следует применять детерминированное шифрование. Используйте случайное шифрование для таких данных, как номер кредитной карты, которые не группируются с другими записями, не используются для соединения таблиц и не могут быть параметром поиска, так как для поиска нужной строки с зашифрованным столбцом используются другие столбцы (например, номер транзакции).

При использовании функции Always Encrypted с безопасными анклавами советуем использовать случайное шифрование.

Столбцы должны иметь подходящий тип данных.

ALGORITHM **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий).

Должно быть **'AEAD_AES_256_CBC_HMAC_SHA_256'** .

Дополнительные сведения о функции Always Encrypted, в том числе об ограничениях, см. в [этой статье](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

SPARSE — указывает, что столбец является разреженным столбцом. Хранилище разреженных столбцов оптимизируется для значений NULL. Для разреженных столбцов нельзя указать параметр NOT NULL. Дополнительные ограничения и сведения о разреженных столбцах см. в разделе [Разреженные столбцы](../../relational-databases/tables/use-sparse-columns.md).

MASKED WITH ( FUNCTION = ' *mask_function* ') **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий).

Указывает маску для динамического маскирования данных. *mask_function* — это имя функции маскирования с соответствующими параметрами. Доступны четыре функции:

- default()
- email()
- partial()
- random()

Параметры функции см. в разделе [Динамическое маскирование данных](../../relational-databases/security/dynamic-data-masking.md).

FILESTREAM **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] и более поздних версий)

Допустимо только для столбцов типа **varbinary(max)** . Указывает хранилище FILESTREAM для данных больших двоичных объектов типа **varbinary(max)** .

Таблица также должна содержать столбец данных типа **uniqueidentifier** с атрибутом ROWGUIDCOL. Этот столбец не должен допускать значений NULL и должен иметь относящееся к одному столбцу ограничение UNIQUE или PRIMARY KEY. Значение идентификатора GUID для столбца должно быть предоставлено приложением во время вставки данных или ограничением DEFAULT, в котором используется функция NEWID ().

Столбец ROWGUIDCOL нельзя удалить, а связанные ограничения нельзя изменить, если в таблице определен столбец FILESTREAM. Столбец ROWGUIDCOL можно удалить только после удаления последнего столбца FILESTREAM.

Если для столбца задан атрибут хранилища FILESTREAM, то все значения для этого столбца хранятся в контейнере данных FILESTREAM в файловой системе.

COLLATE *collation_name* — задает параметры сортировки для столбца. Могут использоваться параметры сортировки Windows или параметры сортировки SQL. Аргумент *collation_name* применим только к столбцам типа **char**, **varchar**, **text**, **nchar**, **nvarchar** и **ntext**. Если этот аргумент не указан, столбцу назначаются либо параметры сортировки определяемого пользователем типа, если столбец принадлежит к определяемому пользователем типу данных, либо установленные по умолчанию параметры сортировки для базы данных.

Дополнительные сведения об именах параметров сортировки Windows и SQL см. в разделах [Имя параметров сортировки Windows](../../t-sql/statements/windows-collation-name-transact-sql.md) и [Имя параметров сортировки SQL](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Дополнительные сведения см. в описании [COLLATE](~/t-sql/statements/collations.md).

CONSTRAINT — необязательное ключевое слово, указывающее на начало определения для ограничения PRIMARY KEY, NOT NULL, UNIQUE, FOREIGN KEY или CHECK.

*constraint_name* — имя ограничения. Имена ограничений должны быть уникальными в пределах схемы, к которой принадлежит таблица.

NULL | NOT NULL — определяет, допустимы ли для столбца значения NULL. Параметр NULL не является ограничением в строгом смысле слова, но может быть указан так же, как и NOT NULL. Ограничение NOT NULL может быть указано для вычисляемых столбцов только в случае, если одновременно указан параметр PERSISTED.

PRIMARY KEY — ограничение, которое обеспечивает целостность сущностей для указанного столбца или столбцов с помощью уникального индекса. Можно создать только одно ограничение PRIMARY KEY для таблицы.

UNIQUE — ограничение, которое обеспечивает целостность сущностей для указанного столбца или столбцов с помощью уникального индекса. В таблице может быть несколько ограничений UNIQUE.

CLUSTERED | NONCLUSTERED — указывает, что для ограничения PRIMARY KEY или UNIQUE создается кластеризованный или некластеризованный индекс. Для ограничений PRIMARY KEY по умолчанию создается кластеризованный индекс (CLUSTERED), а для ограничений UNIQUE — некластеризованный (NONCLUSTERED).

В инструкции `CREATE TABLE` параметр CLUSTERED можно задать только для одного ограничения. Если для ограничения UNIQUE указан параметр CLUSTERED, и, кроме того, указано ограничение PRIMARY KEY, то для PRIMARY KEY применяется по умолчанию значение NONCLUSTERED.

FOREIGN KEY REFERENCES — ограничение, которое обеспечивает ссылочную целостность данных в этом столбце или столбцах. Ограничения FOREIGN KEY требуют, чтобы каждое значение в столбце существовало в соответствующем связанном столбце или столбцах в связанной таблице. Ограничения FOREIGN KEY могут ссылаться только на столбцы, являющиеся ограничениями PRIMARY KEY или UNIQUE в связанной таблице или на столбцы, на которые имеются ссылки в индексе UNIQUE INDEX связанной таблицы. Внешние ключи в вычисляемых столбцах должны быть также помечены как PERSISTED.

[ _schema\_name_ **.** ] *referenced_table_name*] — имя таблицы, на которую ссылается ограничение FOREIGN KEY, и схема, к которой она принадлежит.

**(** *ref_column* [ **,** ... *n* ] **)**  — столбец или список столбцов из таблицы, на которую ссылается ограничение FOREIGN KEY.

ON DELETE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT } — определяет операцию, выполняемую со строками создаваемой таблицы, если эти строки имеют ссылочную связь, а строка, на которую добавлены ссылки, удаляется из родительской таблицы. Параметр по умолчанию — NO ACTION.

NO ACTION — компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] формирует ошибку, и выполняется откат операции удаления строки из родительской таблицы.

CASCADE — если из родительской таблицы удаляется строка, соответствующие ей строки удаляются и из таблицы со ссылкой.

SET NULL — все значения, составляющие внешний ключ, при удалении соответствующей строки родительской таблицы устанавливаются в NULL. Для выполнения этого ограничения внешние ключевые столбцы должны допускать значения NULL.

SET DEFAULT — все значения, составляющие внешний ключ, при удалении соответствующей строки родительской таблицы устанавливаются в значение по умолчанию. Для выполнения этого ограничения все внешние ключевые столбцы должны иметь определения по умолчанию. Если столбец допускает значения NULL и значение по умолчанию явно не определено, значением столбца по умолчанию становится NULL.

Не следует использовать параметр CASCADE, если таблица будет включена в публикацию слиянием, в которой используются логические записи. Дополнительные сведения о логических записях см. в статье [Группирование изменений в связанных строках с помощью логических записей](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

Параметр `ON DELETE CASCADE` нельзя указывать, если в таблице уже существует триггер `INSTEAD OF``ON DELETE`.

Например, в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] таблица **ProductVendor** имеет ссылочную связь с таблицей **Vendor**. Внешний ключ **ProductVendor.BusinessEntityID** ссылается на первичный ключ **Vendor.BusinessEntityID**.

Если для строки в таблице **Vendor** выполняется инструкция `DELETE` и для внешнего ключа **ProductVendor.BusinessEntityID** указано действие `ON DELETE CASCADE`, то компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] проверит наличие одной зависимой строки или нескольких в таблице **ProductVendor**. Если такие существуют, то кроме строки в таблице **Vendor** будут удалены также и все зависимые строки из таблицы **ProductVendor**.

В противном случае, если задан параметр `NO ACTION`, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдает ошибку и производит откат операции по удалению строки из таблицы **Vendor**, если в таблице **ProductVendor** существует хотя бы одна строка, ссылающаяся на нее.

ON UPDATE { **NO ACTION** \| CASCADE \ SET NULL \| SET DEFAULT } — определяет операцию, которая выполняется со строками изменяемой таблицы, если эти строки имеют ссылочную связь, а строка, на которую добавлены ссылки, изменяется в родительской таблице. Параметр по умолчанию — NO ACTION.

NO ACTION — компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] возвращает ошибку, а обновление строки родительской таблицы откатывается.

CASCADE — соответствующие строки обновляются в ссылающейся таблице, если эта строка обновляется в родительской таблице.

SET NULL — всем значениям, составляющим внешний ключ, присваивается значение NULL, когда обновляется соответствующая строка в родительской таблице. Для выполнения этого ограничения внешние ключевые столбцы должны допускать значения NULL.

SET DEFAULT — всем значениям, составляющим внешний ключ, присваивается их значение по умолчанию, когда обновляется соответствующая строка в родительской таблице. Для выполнения этого ограничения все внешние ключевые столбцы должны иметь определения по умолчанию. Если столбец допускает значения NULL и значение по умолчанию явно не определено, значением столбца по умолчанию становится NULL.

Не указывайте параметр `CASCADE`, если таблица будет включена в публикацию слиянием, в которой используются логические записи. Дополнительные сведения о логических записях см. в статье [Группирование изменений в связанных строках с помощью логических записей](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

Действия `ON UPDATE CASCADE`, `SET NULL` и `SET DEFAULT` нельзя определить, если в изменяемой таблице уже существует триггер `INSTEAD OF` для условия `ON UPDATE`.

Например, в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] таблица **ProductVendor** имеет ссылочную связь с таблицей **Vendor**. Внешний ключ **ProductVendor.BusinessEntity** ссылается на первичный ключ **Vendor.BusinessEntityID**.

Если при выполнении инструкции UPDATE для строки в таблице **Vendor** указано ON UPDATE CASCADE для столбца **ProductVendor.BusinessEntityID**, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] проверяет зависимые строи в таблице **ProductVendor**. Если такие существуют, то кроме строки в таблице **Vendor** будут обновлены также и все зависимые строки из таблицы **ProductVendor**.

В противном случае, если задан параметр NO ACTION, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдает ошибку и производит откат операции по обновлению строки из таблицы **Vendor**, если в таблице **ProductVendor** существует хотя бы одна строка, ссылающаяся на нее.

CHECK — ограничение, обеспечивающее целостность домена путем ограничения возможных значений, которые можно ввести в столбец или столбцы. Ограничения CHECK в вычисляемых столбцах должны быть также помечены как PERSISTED.

*logical_expression* — логическое выражение, возвращающее значения TRUE или FALSE. Псевдонимы типа данных частью выражения быть не могут.

*column* — столбец или список столбцов (в скобках), используемый в ограничениях таблицы для указания столбцов, которые применяются в определении ограничения.

[ **ASC** | DESC ] —указывает порядок сортировки столбца или столбцов, участвующих в ограничениях таблицы. Значение по умолчанию — ASC.

*partition_scheme_name* — имя схемы секционирования, определяющей файловые группы, с которыми сопоставляются секции секционированной таблицы. Эта схема секционирования должна существовать в базе данных.

[ _имя\_столбца\_раздела_ **.** ] — указывает столбец, по которому будет секционирована таблица. Столбец должен совпадать с указанным в функции секционирования, которая используется аргументом *partition_scheme_name*, по типу данных, длине и точности. Вычисляемый столбец, участвующий в функции секционирования, должен быть явно обозначен ключевым словом PERSISTED.

> [!IMPORTANT]
> Рекомендуется указывать параметр NOT NULL для столбца секционирования секционированных таблиц, а также для несекционированных таблиц, являющихся источниками или целями для операций ALTER TABLE...SWITCH. В результате любые ограничения CHECK, заданные для столбцов секционирования, не будут проверять значения NULL.

WITH FILLFACTOR **=** _fillfactor_ — указывает, насколько плотно компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен заполнять каждую страницу индекса, используемую для хранения данных индекса. Пользовательские значения аргумента *fillfactor* могут быть в диапазоне от 1 до 100. Если значение не задано, по умолчанию принимается значение 0. Значения коэффициентов заполнения 0 и 100 идентичны.

> [!IMPORTANT]
> Описание выражения WITH FILLFACTOR = *fillfactor* как единственного параметра индекса, применимого к ограничениям PRIMARY KEY или UNIQUE, сохранено для обеспечения обратной совместимости, но в будущих выпусках это выражение документировано не будет.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS — имя набора столбцов. Набор столбцов представляет собой нетипизированное XML-представление, в котором все разреженные столбцы таблицы объединены в структурированные выходные данные. Дополнительные сведения о наборах столбцов см. в разделе [Использование наборов столбцов](../../relational-databases/tables/use-column-sets.md).

PERIOD FOR SYSTEM_TIME (*system_start_time_column_name* , *system_end_time_column_name* ) **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает имена столбцов, которые система будет использовать для обозначения периода действия записи. Используйте этот аргумент в сочетании с аргументами GENERATED ALWAYS AS ROW { START | END } и WITH SYSTEM_VERSIONING = ON, чтобы выключить системное управление версиями в таблице. Дополнительные сведения см. в разделе [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

COMPRESSION_DELAY **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Если используется оптимизация для памяти, указывается минимальное количество минут задержки, в течение которых строка должна оставаться в таблице без изменений до сжатия в индекс columnstore. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выбирает определенные строки для сжатия в зависимости от времени их последнего обновления. Например, если строки часто меняются в течение двухчасового периода, установите `COMPRESSION_DELAY = 120 Minutes`, чтобы обновления были завершены до того, как SQL Server сожмет строку.

Для таблицы на основе диска значение delay указывает минимальное количество минут, в течение которых разностная группа строк в состоянии CLOSED должна оставаться в разностной группе строк до того, как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сможет преобразовать ее в сжатую группу строк. Так как таблицы на основе диска не отслеживают время вставки и обновления отдельных строк, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] применяется задержка к разностным группам строк в состоянии CLOSED.

Значение по умолчанию — 0 минут.

Рекомендации по использованию `COMPRESSION_DELAY` см. в статье [Начало работы с Columnstore для получения операционной аналитики в реальном времени](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).

\< table_option> ::= Указывает один или более параметров таблицы.

DATA_COMPRESSION — задает режим сжатия данных для указанной таблицы, номера секции или диапазона секций. Существуют следующие варианты выбора.

NONE — таблица или указанные секции не сжимаются.

ROW — таблицы или указанные секции сжимаются, используется сжатие строк.

PAGE — таблицы или указанные секции сжимаются, используется сжатие страниц.

COLUMNSTORE

**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Применяется только к индексам columnstore, включая некластеризованные и кластеризованные индексы columnstore. COLUMNSTORE задает сжатие с использованием самого эффективного сжатия columnstore. Это обычный вариант.

COLUMNSTORE_ARCHIVE **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Применяется только к индексам columnstore, включая некластеризованные и кластеризованные индексы columnstore. Параметр COLUMNSTORE_ARCHIVE обеспечивает дальнейшее сжатие таблицы или секции до еще меньшего размера. Это может использоваться для архивации или в других ситуациях, где требуется уменьшение объема пространства и допускается увеличение затрат времени на сохранение и выборку

Дополнительные сведения см. в разделе [Data Compression](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { `<partition_number_expression>` | [ **,** ...*n* ] **)**  — указывает секции, к которым применяется параметр DATA_COMPRESSION. Если таблица не секционирована, аргумент `ON PARTITIONS` приведет к возникновению ошибки. Если не указано предложение `ON PARTITIONS`, параметр `DATA_COMPRESSION` применяется ко всем секциям секционированной таблицы.

*partition_number_expression* можно указать одним из следующих способов:

- Указав номер секции, например: `ON PARTITIONS (2)`
- Указав номера нескольких секций, разделив их запятыми, например `ON PARTITIONS (1, 5)`
- Указав диапазоны секций и отдельные секции, например `ON PARTITIONS (2, 4, 6 TO 8)`

`<range>` можно указать в виде номеров секций, разделенных ключевым словом TO, например: `ON PARTITIONS (6 TO 8)`.

Чтобы для разных секций задать разные типы сжатия данных, укажите параметр `DATA_COMPRESSION` несколько раз, например следующим образом:

```sql
WITH
(
    DATA_COMPRESSION = NONE ON PARTITIONS (1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
    DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
)
```

\<index_option> ::= Указывает один или более параметров индекса. Полное описание этих параметров см. в [этой статье](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** } — если указано значение ON, процент свободного места, определяемый параметром FILLFACTOR, применяется к страницам индекса промежуточного уровня. Если указано значение OFF или значение FILLFACTOR не указано, страницы промежуточного уровня заполняются до приблизительного объема, оставляющего достаточно места для, как минимум, одной строки максимального размера, которого может достигать индекс, при этом учитывается набор ключей на промежуточных страницах. Значение по умолчанию — OFF.

FILLFACTOR **=** _fillfactor_ — определяет величину в процентах, показывающую насколько должен компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] заполнять конечный уровень каждой страницы индекса во время его создания и изменения. Значение *fillfactor* должно быть целым числом от 1 до 100. Значение по умолчанию равно 0. Значения коэффициентов заполнения 0 и 100 идентичны.

IGNORE_DUP_KEY = { ON | **OFF** } — определяет реакцию на ошибку, возникающую при попытке вставки повторяющихся ключевых значений в уникальный индекс. Параметр IGNORE_DUP_KEY применяется только к операциям вставки, производимым после создания или перестроения индекса. Параметр не работает во время выполнения инструкции [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) или [UPDATE](../../t-sql/queries/update-transact-sql.md). Значение по умолчанию — OFF.

ON — если в уникальный индекс вставляются повторяющиеся значения ключа, выводится предупреждающее сообщение. С ошибкой завершаются только строки, нарушающие ограничение уникальности.

OFF — если в уникальный индекс вставляются повторяющиеся значения ключа, выводится сообщение об ошибке. Будет выполнен откат всей операции INSERT.

`IGNORE_DUP_KEY` нельзя установить в значение ON для индексов, создаваемых для представлений, неуникальных индексов, XML-индексов, пространственных индексов и фильтруемых индексов.

Для просмотра значения `IGNORE_DUP_KEY` используйте [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

Для обратной совместимости синтаксиса аргумент `WITH IGNORE_DUP_KEY` эквивалентен `WITH IGNORE_DUP_KEY = ON`.

STATISTICS_NORECOMPUTE **=** { ON | **OFF** } — если указано значение ON, устаревшая статистика индекса не вычисляется повторно автоматически. Если указано значение OFF, включается автоматическое обновление статистик. Значение по умолчанию — OFF.

ALLOW_ROW_LOCKS **=** { **ON** | OFF } — если указано значение ON, при доступе к индексу допустимы блокировки строк. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки. При значении OFF блокировки строк не используются. Значение по умолчанию — ON.

ALLOW_PAGE_LOCKS **=** { **ON** | OFF } — если указано значение ON, при доступе к индексу допустимы блокировки страниц. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц. При значении OFF блокировки страниц не используются. Значение по умолчанию — ON.

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** } **Применимо к**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и более поздних версий. <BR>
Определяет, следует ли выполнять оптимизацию, связанную с состязанием при операциях вставки на последнюю страницу. Значение по умолчанию — OFF. См. подробнее раздел о [последовательных ключах](./create-index-transact-sql.md#sequential-keys) в документации по CREATE INDEX.

FILETABLE_DIRECTORY = *directory_name*

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше).

Указывает имя каталога таблицы FileTable, совместимое с Windows. Это имя должно быть уникальным среди всех имен каталогов FileTable в базе данных. Проверка уникальности выполняется без учета регистра, независимо от параметров сортировки. Если это значение не задано, то используется имя таблицы FileTable.

FILETABLE_COLLATE_FILENAME = { *collation_name* | database_default }

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не поддерживает `FILETABLE`.

Указывает имя параметров сортировки, применяемых к столбцу **Name** в таблице FileTable. Для соответствия семантике именования файлов в операционной системе Windows параметры сортировки не должны учитывать регистр. Если это значение не задано, то используются параметры сортировки по умолчанию базы данных. Если в параметрах сортировки по умолчанию базы данных учитывается регистр, то выдается ошибка и операция CREATE TABLE оканчивается неуспешно.

*collation_name* — имя параметров сортировки без учета регистра.

database_default — указывает, что для базы данных следует использовать параметры сортировки по умолчанию. Эти параметры сортировки не должны учитывать регистр символов.

FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = *constraint_name*
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий).

Указывает имя, которое должно использоваться для ограничения первичного ключа, автоматически создаваемого в FileTable. Если это значение не задано, то имя для ограничения формируется системой.

FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий).

Указывает имя, которое должно использоваться для ограничения уникальности, автоматически создаваемого в столбце **stream_id** в FileTable. Если это значение не задано, то имя для ограничения формируется системой.

FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий).

Указывает имя, которое должно использоваться для ограничения уникальности, автоматически создаваемого в столбцах **parent_path_locator** и **name** в FileTable. Если это значение не задано, то имя для ограничения формируется системой.

SYSTEM_VERSIONING **=** ON [ ( HISTORY_TABLE **=** *schema_name* .*history_table_name* [, DATA_CONSISTENCY_CHECK **=** { **ON** | OFF } ] ) ] **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

Допускает системное управление версиями таблицы, если выполнены требования по типу данных, ограничении допустимости значений NULL и ограничении первичного ключа. Если аргумент `HISTORY_TABLE` не используется, система создает новую таблицу журнала, соответствующую схеме текущей таблицы, в той же файловой группе, что и текущая таблица. Между двумя таблицами создается связь, и система записывает журнал каждой записи текущей таблицы в таблице журнала. Таблица журнала будет называться `MSSQL_TemporalHistoryFor<primary_table_object_id>`. По умолчанию таблица журнала сжимается с использованием метода **PAGE** . Если аргумент `HISTORY_TABLE` используется для создания ссылки и применения существующей таблицы журнала, ссылка создается между текущей таблицей и указанной таблицей. Если текущая таблица секционирована, таблица журнала создается в файловой группе по умолчанию, так как конфигурация секционирования не реплицируется автоматически из текущей таблицы в таблицу журнала. Если при создании таблицы журнала указывается ее имя, следует также указать имя схемы и таблицы. При создании ссылки на существующую таблицу журнала вы можете указать необходимость проверки согласованности данных. Проверка согласованности данных гарантирует, что существующие записи не будут перекрываться. Проверка согласованности данных является проверкой по умолчанию. Используйте этот аргумент в сочетании с аргументами `PERIOD FOR SYSTEM_TIME` и `GENERATED ALWAYS AS ROW { START | END }`, чтобы выключить системное управление версиями в таблице. Дополнительные сведения см. в разделе [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

REMOTE_DATA_ARCHIVE = { ON [ ( *table_stretch_options* [,...n] ) ] | OFF ( MIGRATION_STATE = PAUSED ) }   
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше).

Создание новой таблицы, для которой включена или отключена Stretch Database. Дополнительные сведения см. в разделе [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Включение Stretch Database для таблицы**

Если вы включаете Stretch для таблицы, указывая `ON`, вы можете дополнительно указать `MIGRATION_STATE = OUTBOUND`, чтобы сразу же приступить к переносу данных, или `MIGRATION_STATE = PAUSED`, чтобы отложить его. Значение по умолчанию — `MIGRATION_STATE = OUTBOUND`. Более подробную информацию о включении Stretch для таблицы см. в разделе [Включение Stretch Database для таблицы](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Предварительные требования**. Прежде чем включить Stretch для таблицы, необходимо включить Stretch на сервере и в базе данных. Дополнительные сведения см. в разделе [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Разрешения**. Чтобы включить Stretch для таблицы или базы данных, требуются права db_owner. Чтобы включить Stretch для таблицы, нужно иметь разрешения ALTER для таблицы.

[ FILTER_PREDICATE = { null | *predicate* } ] **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий).

Дополнительно указывает предикат фильтра для выбора строк для миграции из таблицы, которая содержит данные журнала и текущие данные. Этот предикат должен вызывать детерминированную встроенную функцию с табличным значением. Более подробную информацию см. в разделе [Включение Stretch Database для таблицы](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) и [Выбор строк для миграции с помощью функции фильтра](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Если указать плохо оптимизированный предикат фильтра, перенос данных будет выполняться медленно. Stretch Database применяет предикат фильтра к таблице при помощи оператора CROSS APPLY.

Если предикат фильтра не указан, переносится вся таблица.

Если вы указываете предикат фильтра, необходимо также указать *MIGRATION_STATE*.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED } **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

- Укажите `OUTBOUND` для миграции данных с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
- Укажите `INBOUND` для копирования удаленных данных для таблицы из [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] обратно в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с отключением Stretch для таблицы. Дополнительные сведения см. в разделе [Отключение Stretch Database и возврат удаленных данных](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

   Эта операция предусматривает расходы на передачу данных и не может быть отменена.

- Укажите `PAUSED` для приостановки миграции данных. Дополнительные сведения см. в разделе [Приостановка и возобновление переноса данных (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

[DATA_DELETION = ON  
    {( FILTER_COLUMN = column_name,   
           RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS } )}] **Применимо к:** *только* Azure SQL Edge

Включает очистку старых или устаревших данных из таблиц в базе данных на основе политики хранения. Дополнительные сведения см. в статье [Включение и отключение хранения данных](/azure/azure-sql-edge/data-retention-enable-disable). Для включения хранения данных необходимо указать следующие параметры. 

- FILTER_COLUMN = { column_name }  
Указывает столбец, который должен использоваться для определения того, являются ли строки в таблице устаревшими. Для столбца фильтра разрешены следующие типы данных.
  - Дата
  - Дата и время
  - datetime2
  - SmallDateTime
  - DateTimeOffset

- RETENTION_PERIOD = { INFINITE \| число {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS }}       
  Указывает политику периода хранения для таблицы. Период хранения указывается как сочетание положительного целого значения и единицы измерения даты.   

MEMORY_OPTIMIZED **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]). Управляемый экземпляр SQL Azure не поддерживает оптимизированные для памяти таблицы.

Значение ON указывает, что таблица оптимизирована для памяти. Таблицы, оптимизированные для памяти, входят в функцию выполняющейся в памяти OLTP, которая используется для оптимизации производительности обработки транзакций. Чтобы приступить к работе с OLTP в памяти, см. статью [Краткое руководство 1. Технологии выполнения OLTP в памяти для повышения производительности службы Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md). Дополнительные сведения об оптимизированных для памяти таблицах см. в разделе [Таблицы, оптимизированные для памяти](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md).

Значение по умолчанию OFF указывает, что таблица основана на диске.

DURABILITY **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Значение `SCHEMA_AND_DATA` указывает на устойчивость таблицы. Это означает, что изменения сохраняются на диске даже после перезагрузки или отработки отказа. SCHEMA_AND_DATA является значением по умолчанию.

Значение `SCHEMA_ONLY` указывает, что таблица не является устойчивой. При перезапуске или отработке отказа в базе данных схема таблицы сохраняется, а обновления данных — нет. Аргумент `DURABILITY = SCHEMA_ONLY` может использоваться только совместно с аргументом `MEMORY_OPTIMIZED = ON`.

> [!WARNING]
> Если таблица создается с аргументом **DURABILITY = SCHEMA_ONLY**, а затем **READ_COMMITTED_SNAPSHOT** меняется с помощью **ALTER DATABASE**, данные в таблице будут утеряны.

BUCKET_COUNT **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Отображает число контейнеров, которые необходимо создать в хэш-индексе. Максимальное значение для параметра BUCKET_COUNT в хэш-индексах составляет 1 073 741 824. Дополнительные сведения о числах контейнеров см. в разделе [Индексы для таблиц, оптимизированных для памяти](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

Bucket_count — это обязательный аргумент.

INDEX **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

Индексы столбцов и таблиц необходимо указывать в составе инструкции CREATE TABLE. Дополнительные сведения о добавлении и удалении индексов в таблицах, оптимизированных для памяти, см. в следующей статье: [Изменение таблиц с оптимизацией для памяти](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

HASH **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает, что был создан индекс HASH.

Хэш-индексы поддерживаются только в таблицах, оптимизированных для памяти.

## <a name="remarks"></a>Remarks

Сведения о допустимом количестве таблиц, столбцов, ограничений и индексов см. в разделе [Спецификации максимально допустимых параметров SQL Server](../../sql-server/maximum-capacity-specifications-for-sql-server.md).

Пространство таблицам и индексам обычно выделяется по одному экстенту за раз. Если для параметра `SET MIXED_PAGE_ALLOCATION` в инструкции `ALTER DATABASE` установлено значение TRUE или всегда до [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], при создании таблицы или индекса они будут представлять собой выделенные страницы из разных экстентов, пока не наберется достаточно страниц для заполнения одного экстента. Каждый раз, когда число страниц достигает размера однородного экстента, и текущие выделенные экстенты становятся заполненными, выделяется новый экстент. Получить отчет об объеме выделенного и используемого таблицей пространства можно с помощью процедуры `sp_spaceused`.

Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не требует указания параметров DEFAULT, IDENTITY, ROWGUIDCOL или ограничения столбцов в определенном порядке при определении столбца.

После создания таблицы параметр QUOTED IDENTIFIER всегда сохраняется в метаданных таблицы в состоянии ON, даже если он был установлен в состояние OFF при создании таблицы.

## <a name="temporary-tables"></a>Временные таблицы

Можно создавать локальные и глобальные временные таблицы. Локальные временные таблицы видимы только во время текущего сеанса, а глобальные — во всех сеансах. Временные таблицы не подлежат секционированию.

Имени локальной временной таблицы должен предшествовать символ решетки (#*table_name*), а имени глобальной временной таблицы — двойной символ решетки (##*table_name*).

Инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] могут обращаться к временной таблице по заданному в инструкции `CREATE TABLE` значению аргумента *table_name*, например:

```sql
CREATE TABLE #MyTempTable (
    col1 INT PRIMARY KEY
);

INSERT INTO #MyTempTable
VALUES (1);
```

Если в пределах одной хранимой процедуры или пакета создается более одной временной таблицы, им должны быть присвоены разные имена.

Параметр *schema_name* игнорируется при создании или временной таблице или обращении к ней. Все временные таблицы создаются в схеме dbo.

Если локальная временная таблица создается хранимой процедурой или приложением, которые одновременно могут выполняться несколькими пользователями, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен иметь возможность различать таблицы, созданные разными пользователями. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] делает это путем внутреннего присоединения числового суффикса к имени каждой локальной временной таблицы. Полное имя временной таблицы, хранящееся в таблице **sysobjects** базы данных **tempdb**, состоит из имени таблицы, заданного инструкцией CREATE TABLE, и сформированного системой числового суффикса. Для обеспечения возможности добавления суффикса значение параметра *table_name*, определенного как имя локальной временной таблицы, не должно содержать более 116 символов.

Временные таблицы автоматически удаляются при выходе за пределы области определения, если не удалять их явно с помощью инструкции DROP TABLE.

- Локальная временная таблица, созданная хранимой процедурой, удаляется автоматически при завершении хранимой процедуры. К этой таблице могут обращаться любые вложенные хранимые процедуры, выполняемые хранимой процедурой, создавшей таблицу. Процесс, вызвавший хранимую процедуру, создавшую таблицу, к этой таблице обращаться не может.
- Все прочие локальные временные таблицы удаляются автоматически в конце текущего сеанса.
- Глобальные временные таблицы автоматически удаляются при завершении сеанса, создавшего таблицу, и прекращении обращения к ним всех прочих задач. Взаимосвязь между задачей и таблицей поддерживается только на время выполнения отдельной инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Это означает, что глобальная временная таблица удаляется после выполнения последней инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)], активно обращавшейся к ней во время завершения создавшего таблицу сеанса.

Локальная временная таблица, созданная хранимой процедурой или триггером, может иметь то же имя, что и временная таблица, созданная до вызова хранимой процедуры или триггера. Однако если запрос обращается к временной таблице и одновременно существует две таблицы с одинаковым именем, не определено, к какой из таблиц будет направлен запрос. Вложенные хранимые процедуры могут также создавать временные таблицы с тем же именем, что и временная таблица, созданная вызывающей хранимой процедурой. Однако для применения изменений к таблице, созданной во вложенной процедуре, эта таблица должна иметь ту же структуру с теми же именами столбцов, что и таблица, созданная в вызывающей процедуре. Это показано в следующем примере.

```sql
CREATE PROCEDURE dbo.Test2
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (2);
    SELECT Test2Col = x FROM #t;
GO

CREATE PROCEDURE dbo.Test1
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (1);
    SELECT Test1Col = x FROM #t;
    EXEC Test2;
GO

CREATE TABLE #t(x INT PRIMARY KEY);
INSERT INTO #t VALUES (99);
GO

EXEC Test1;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
(1 row(s) affected)
Test1Col
-----------
1

(1 row(s) affected)
 Test2Col
 -----------
 2
 ```

При создании локальных или глобальных временных таблиц синтаксис инструкции `CREATE TABLE` поддерживает определение всех ограничений, кроме FOREIGN KEY. Если во временной таблице указано ограничение FOREIGN KEY, инструкция возвращает предупредительное сообщение, указывающее на то, что ограничение было пропущено. При этом таблица создается без ограничений FOREIGN KEY. В ограничениях FOREIGN KEY обращение к временным таблицам недопустимо.

При создании таблицы с именованным ограничением внутри области, определяемой пользователем транзакции, возможность выполнения инструкций, формирующих временные таблицы, ограничивается одним пользователем единовременно. Например, если хранимая процедура формирует временную таблицу с именованным ограничением первичного ключа, то она не может быть выполнена несколькими пользователями одновременно.

## <a name="database-scoped-global-temporary-tables-azure-sql-database"></a>Глобальные временные таблицы (база данных SQL Azure) в области базы данных

Глобальные временные таблицы для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с префиксом ##) хранятся в базе данных tempdb и являются общими для всех сеансов пользователей во всем экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительную информацию о типах таблиц SQL см. в предыдущем разделе о создании таблиц.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] поддерживает глобальные временные таблицы, которые хранятся в базе данных tempdb и областью действия которых является база данных. Это означает, что глобальные временные таблицы являются общими для всех сеансов пользователей в рамках одной [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Сеансы пользователей, связанные с другими базами данных, не имеют доступа к глобальным временным таблицам.

В глобальных временных таблицах для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] используются те же синтаксис и семантика, что и для временных таблиц в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Точно так же временные хранимые процедуры действуют в области базы данных в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Локальные временные таблицы (с префиксом #) также поддерживаются [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и имеют тот же синтаксис и семантику, что и в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. См. предыдущий раздел [Временные таблицы](#temporary-tables).  

> [!IMPORTANT]
> Эта возможность доступна для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

### <a name="troubleshooting-global-temporary-tables-for-azure-sql-database"></a>Устранение неполадок с глобальными временными таблицами в Базе данных SQL Azure

Сведения об устранении неполадок с базой данных tempdb см. в разделе [Мониторинг использования базы данных tempdb](../../relational-databases/databases/tempdb-database.md#monitoring-tempdb-use).

> [!NOTE]
> Только администратор сервера может получить доступ к динамическим административным представлениям для устранения неполадок в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

### <a name="permissions"></a>Разрешения

Любой пользователь может создавать глобальные временные объекты. Если не предоставлены какие-либо дополнительные разрешения, то пользователи могут производить доступ только к тем объектам, которыми они владеют.

## <a name="partitioned-tables"></a>Секционированные таблицы

Перед созданием секционированной таблицы с помощью инструкции CREATE TABLE следует вначале создать функцию секционирования, чтобы указать, как должна быть секционирована таблица. Функция секционирования создается с помощью инструкции [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md). Затем необходимо создать схему секционирования, чтобы указать файловые группы, которые будут содержать указанные функцией секционирования секции. Схема секционирования создается с помощью инструкции [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md). Для секционированных таблиц нельзя указать ограничения PRIMARY KEY или UNIQUE для разделения файловых групп. Дополнительные сведения см. в разделе [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

## <a name="primary-key-constraints"></a>Ограничения PRIMARY KEY

- В таблице возможно наличие только одного ограничения по первичному ключу.
- Индекс, формируемый ограничением PRIMARY KEY, не может привести к выходу количества индексов в таблице за пределы в 999 некластеризованных индексов и 1 кластеризованный.
- Если для ограничения PRIMARY KEY не указан параметр CLUSTERED или NONCLUSTERED, применяется параметр CLUSTERED, если для ограничения UNIQUE не определено кластеризованных индексов.
- Все столбцы с ограничением PRIMARY KEY должны иметь признак NOT NULL. Если допустимость значения NULL не указана, то для всех столбцов c ограничением PRIMARY KEY устанавливается признак NOT NULL.

    > [!NOTE]
    > В таблицах, оптимизированных для памяти, допускается ключевой столбец, способный принимать значение NULL.

- Если первичный ключ определен на столбце определяемого пользователем типа данных CLR, реализация этого типа должна поддерживать двоичную сортировку. Дополнительные сведения об определяемых пользователем типах данных CLR см. в разделе [Определяемые пользователем типы данных CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="unique-constraints"></a>Ограничения UNIQUE

- Если для ограничения UNIQUE не указан параметр CLUSTERED или NONCLUSTERED, по умолчанию применяется параметр NONCLUSTERED.
- Каждое ограничение уникальности создает индекс. Количество ограничений UNIQUE не может привести к выходу количества индексов в таблице за пределы в 999 некластеризованных индексов и 1 кластеризованный.
- Если ограничение уникальности определено на столбце определяемого пользователем типа данных CLR, реализация этого типа должна поддерживать двоичную сортировку или сортировку на основе оператора. Дополнительные сведения об определяемых пользователем типах данных CLR см. в разделе [Определяемые пользователем типы данных CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="foreign-key-constraints"></a>Ограничения FOREIGN KEY

- Если столбцу, имеющему ограничение внешнего ключа, задается значение, отличное от NULL, такое же значение должно существовать и в указываемом столбце; в противном случае будет возвращено сообщение о нарушении внешнего ключа.
- Если не указаны исходные столбцы, ограничения FOREIGN KEY применяются к предшествующему столбцу.
- Ограничения FOREIGN KEY могут ссылаться только на таблицы в пределах той же базы данных на том же сервере. Межбазовую ссылочную целостность необходимо реализовать посредством триггеров. Дополнительные сведения см. в статье об инструкции [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- Ограничения FOREIGN KEY могут ссылаться на другие столбцы той же таблицы. Это называется самовызовом.
- Предложение REFERENCES ограничения внешнего ключа на уровне столбца может содержать только один ссылочный столбец. Этот столбец должен принадлежать к тому же типу данных, что и столбец, для которого определяется ограничение.
- Предложение REFERENCES ограничения внешнего ключа на уровне таблицы должно содержать такое же число ссылочных столбцов, какое содержится в списке столбцов в ограничении. Тип данных каждого ссылочного столбца должен также совпадать с типом соответствующего столбца в списке столбцов. Ссылочные столбцы должны быть указаны в том же порядке, который использовался при указании столбцов первичного ключа или уникального ограничения в упоминаемой таблице.
- Если частью внешнего ключа или ключа, на который указывает ссылка, является столбец типа **timestamp**, ключевые слова CASCADE, SET NULL и SET DEFAULT указывать нельзя.
- Ключевые слова CASCADE, SET NULL, SET DEFAULT и NO ACTION можно сочетать в таблицах, имеющих взаимные ссылочные связи. Если компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] обнаруживает ключевое слово NO ACTION, оно остановит и произведет откат связанных операций CASCADE, SET NULL и SET DEFAULT. Если инструкция DELETE содержит сочетание ключевых слов CASCADE, SET NULL, SET DEFAULT и NO ACTION, то все операции CASCADE, SET NULL и SET DEFAULT выполняются перед поиском компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] операции NO ACTION.
- Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не имеет стандартного предела на количество ограничений FOREIGN KEY, содержащихся в таблице, ссылающейся на другие таблицы, или на количество ограничений FOREIGN KEY в других таблицах, ссылающихся на указанную таблицу.

  Тем не менее фактическое количество ограничений FOREIGN KEY, доступных для использования, ограничивается конфигурацией оборудования, базы данных и приложения. Рекомендуется, чтобы таблица содержала не более 253 ограничений FOREIGN KEY, а также чтобы на нее ссылалось не более 253 ограничений FOREIGN KEY. Предел эффективности в конкретном случае может более или менее зависеть от приложения и оборудования. При разработке базы данных и приложений следует учитывать стоимость принудительных ограничений FOREIGN KEY.

- Ограничения FOREIGN KEY не применяются к временным таблицам.
- Ограничения FOREIGN KEY могут ссылаться только на столбцы с ограничениями PRIMARY KEY или UNIQUE в таблице, на которую указывает ссылка, или на столбцы уникального индекса (UNIQUE INDEX) такой таблицы.
- Если внешний ключ определен на столбце определяемого пользователем типа данных CLR, реализация этого типа должна поддерживать двоичную сортировку. Дополнительные сведения об определяемых пользователем типах данных CLR см. в разделе [Определяемые пользователем типы данных CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Столбцы, участвующие в связи по внешнему ключу, должны иметь одинаковую длину и масштаб.

## <a name="default-definitions"></a>DEFAULT, определения

- Столбец может иметь только определение DEFAULT.
- Ограничение DEFAULT может содержать значения констант, функции, стандартные функции без параметров SQL или значение NULL. В следующей таблице приведены функции без параметров и возвращаемые ими по умолчанию значения в процессе выполнения инструкции INSERT.

    |Функция без параметров SQL-92|Возвращенное значение|
    |------------------------------|--------------------|
    |CURRENT_TIMESTAMP|Текущие дата и время.|
    |CURRENT_USER|Имя пользователя, выполняющего вставку.|
    |SESSION_USER|Имя пользователя, выполняющего вставку.|
    |SYSTEM_USER|Имя пользователя, выполняющего вставку.|
    |Пользователь|Имя пользователя, выполняющего вставку.|
- Выражение *constant_expression* в определении DEFAULT не может ссылаться на другой столбец таблицы, а также на другие таблицы, представления или хранимые процедуры.
- Определения DEFAULT нельзя создавать для столбцов с типом данных **timestamp** или столбцов со свойством IDENTITY.
- Определения DEFAULT нельзя создавать для столбцов с псевдонимами типов данных, если такой тип привязан к определенному по умолчанию объекту.

## <a name="check-constraints"></a>Ограничения CHECK

- Столбец может содержать любое количество ограничений CHECK, а условие может включать несколько логических выражений, соединенных операторами AND и OR. При указании нескольких ограничений CHECK для столбца их проверка производится в порядке создания.
- Условие поиска должно возвращать логическое выражение и не может ссылаться на другую таблицу.
- Ограничение CHECK уровня столбца может ссылаться только на ограничиваемый столбец, а ограничение CHECK уровня таблицы — только на столбцы этой таблицы.

  Правила и ограничения CHECK выполняют одну и ту же функцию проверки данных при выполнении инструкций INSERT и UPDATE.

- Если для столбца или столбцов задано правило либо одно или несколько ограничений CHECK, применяются все ограничения.
- Ограничения CHECK нельзя определять для столбцов типов **text**, **ntext** или **image**.

## <a name="additional-constraint-information"></a>Дополнительные сведения об ограничениях

- Индекс, созданный для ограничения, нельзя удалить с помощью инструкции `DROP INDEX`. Вам нужно удалить ограничение с помощью инструкции `ALTER TABLE`. Индекс, созданный для ограничения и используемый им, можно перестроить с помощью инструкции `ALTER INDEX ... REBUILD`. Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).
- Имена ограничений должны подчиняться правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md), за исключением тех, которые не могут начинаться с символа решетки (#). Если аргумент *constraint_name* не указан, то ограничению присваивается имя, формируемое системой. Имя ограничения отображается в любых сообщениях об ошибках, связанных с нарушением ограничения.
- При нарушении ограничения в инструкции `INSERT`, `UPDATE` или `DELETE` выполнение инструкции прекращается. Однако если параметр `SET XACT_ABORT` установлен в значение OFF, а инструкция является частью явной транзакции, выполнение этой транзакции продолжается. Если параметр `SET XACT_ABORT` установлен в значение ON, производится откат всей транзакции. С определением транзакции можно также использовать инструкцию `ROLLBACK TRANSACTION`, установив флажок для системной функции `@@ERROR`.
- Когда присвоены значения `ALLOW_ROW_LOCKS = ON` и `ALLOW_PAGE_LOCK = ON`, при доступе к индексу допустимы блокировки на уровне строк, страниц и таблиц. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выбирает соответствующую блокировку и может повышать уровень с блокировки строки или страницы до блокировки таблицы. Если присвоены значения `ALLOW_ROW_LOCKS = OFF` и `ALLOW_PAGE_LOCK = OFF`, при доступе к индексу допустима только блокировка на уровне таблиц.
- Если в таблице содержатся ограничения FOREIGN KEY или CHECK и триггеры, условия ограничений вычисляются перед выполнением триггера.

Получить отчет о таблице и ее столбцах можно с помощью процедуры `sp_help` или `sp_helpconstraint`. Для переименования таблицы используется процедура `sp_rename`. Чтобы получить сведения о представлениях и хранимых процедурах, зависящих от таблицы, используйте функции [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) и [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).

## <a name="nullability-rules-within-a-table-definition"></a>Правила допустимости значения NULL в рамках определения таблицы

Допустимость значения NULL для столбца зависит от того, разрешено ли значение NULL в качестве допустимого значения данных этого столбца. Значение NULL не равнозначно нулю или пустой строке: оно указывает, что запись не была произведена или было явно указано значение NULL; обычно оно означает, что значение неизвестно либо неприменимо.

При создании или изменении таблицы с помощью инструкции `CREATE TABLE` или `ALTER TABLE` параметры базы данных и сеанса влияют на допустимость значений NULL для типа данных, указанного в определении столбца, и могут переопределять ее. Рекомендуется всегда явно определять столбец как NULL или NOT NULL для невычисляемых столбцов или, если используется пользовательский тип данных, разрешать, чтобы для столбца применялась возможность, установленная для этого типа по умолчанию. Для разреженных столбцов всегда должно быть разрешено значение NULL.

Если возможность столбца принимать значение NULL не задана явно, она определяется согласно правилам, указанным в следующей таблице.

|Тип данных столбца|Правило|
|----------------------|----------|
|Псевдоним типа данных|Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] использует допустимость значений NULL, указанную при создании типа данных. Чтобы выяснить допустимость значений NULL по умолчанию для типа данных, используется процедура **sp_help**.|
|CLR, определяемый пользователем тип данных|Допустимость значения NULL определяется в соответствии с определением столбца.|
|Системный тип данных|Если для системного типа данных предусмотрен только один вариант, он и применяется. Для столбцов типа **timestamp** должен быть указан параметр NOT NULL. Если любые параметры сеанса с помощью инструкции SET установлены в ON:<br />**ANSI_NULL_DFLT_ON** = ON — применяется NULL. <br />**ANSI_NULL_DFLT_OFF** = ON — применяется NOT NULL.<br /><br /> Если настроены какие-либо параметры базы данных с помощью инструкции ALTER DATABASE:<br />**ANSI_NULL_DEFAULT_ON** = ON — применяется NULL. <br />**ANSI_NULL_DEFAULT_OFF** = ON — применяется NOT NULL.<br /><br /> Просмотреть параметр базы данных ANSI_NULL_DEFAULT можно в представлении каталога **sys.databases**|

Если для сеанса не установлен ни один из параметров ANSI_NULL_DFLT, а база данных настроена по умолчанию (ANSI_NULL_DEFAULT = OFF), применяется установленное по умолчанию значение NOT NULL.

Если столбец является вычисляемым, допустимость значения NULL для него всегда определяется компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] автоматически. Определить допустимость значения NULL для этого типа столбцов можно с помощью функции `COLUMNPROPERTY` со свойством **AllowsNull**.

> [!NOTE]
> Как драйвер ODBC для SQL Server, так и драйвер OLE DB для SQL Server предусматривают по умолчанию значение параметра ANSI_NULL_DFLT_ON, равное ON. Пользователи ODBC и OLE DB могут настраивать этот параметр в источниках данных ODBC или с помощью установки атрибутов или свойств соединения в приложении.

## <a name="data-compression"></a>Сжатие данных

В системных таблицах не может быть включено сжатие. При создании таблицы параметру сжатия данных присваивается значение NONE, если не указано иное. При указании списка секций или секции, выходящей за пределы диапазона, будет сформирована ошибка. Дополнительную информацию о сжатии данных см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).

Оценить состояние сжатия таблицы, индекса или секции можно с помощью хранимой процедуры [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).

## <a name="permissions"></a>Разрешения

Требуется разрешение `CREATE TABLE` в базе данных и разрешение `ALTER` для схемы, в которой создается таблица.

Если какие-либо столбцы в инструкции `CREATE TABLE` определены как принадлежащие к пользовательскому типу, необходимо иметь разрешение `REFERENCES` для этого типа.

Если какие-либо столбцы в инструкции `CREATE TABLE` определены как принадлежащие к определяемому пользователем типу данных CLR, необходимо быть владельцем этого типа либо иметь разрешение `REFERENCES` для него.

Если какие-либо столбцы в инструкции `CREATE TABLE` имеют связанную коллекцию схем XML, необходимо быть владельцем этой коллекции схем или иметь разрешение `REFERENCES` для нее.

Временные таблицы в базе данных tempdb может создавать любой пользователь.

## <a name="examples"></a>Примеры

### <a name="a-create-a-primary-key-constraint-on-a-column"></a>A. Создание ограничения PRIMARY KEY для столбца

В следующем примере показано определение ограничения PRIMARY KEY с кластеризованным индексом для столбца `EmployeeID` таблицы `Employee`. Поскольку имя ограничения не указано, оно будет подставлено системой.

```sql
CREATE TABLE dbo.Employee (
    EmployeeID INT PRIMARY KEY CLUSTERED
);
```

### <a name="b-using-foreign-key-constraints"></a>Б. Использование ограничений FOREIGN KEY

Ограничение FOREIGN KEY используется для ссылки на другую таблицу. Внешние ключи могут включать один или несколько столбцов. В следующем примере показано ограничение FOREIGN KEY с одним столбцом в таблице `SalesOrderHeader`, ссылающееся на таблицу `SalesPerson`. Для ограничения FOREIGN KEY с одним столбцом требуется только предложение REFERENCES.

```sql
SalesPersonID INT NULL REFERENCES SalesPerson(SalesPersonID)
```

Кроме того, предложение FOREIGN KEY можно применить явно и заново определить атрибут столбца. Обратите внимание, что имена столбцов в обеих таблицах могут различаться.

```sql
FOREIGN KEY (SalesPersonID) REFERENCES SalesPerson(SalesPersonID)
```

Ограничения по ключам с несколькими столбцами создаются в виде табличных ограничений. В базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] таблица `SpecialOfferProduct` включает ограничение PRIMARY KEY с несколькими столбцами. В следующем примере показано, как обращаться к этому ключу из другой таблицы; задавать имя ограничения явно необязательно.

```sql
CONSTRAINT FK_SpecialOfferProduct_SalesOrderDetail
    FOREIGN KEY (ProductID, SpecialOfferID)
    REFERENCES SpecialOfferProduct (ProductID, SpecialOfferID)
```

### <a name="c-using-unique-constraints"></a>В. Использование ограничений UNIQUE

Ограничения UNIQUE используются для указания уникальности непервичных ключевых столбцов. В следующем примере применяется ограничение уникальности столбца `Name` таблицы `Product`.

```sql
Name NVARCHAR(100) NOT NULL
UNIQUE NONCLUSTERED
```

### <a name="d-using-default-definitions"></a>Г. Использование определений DEFAULT

Определения DEFAULT (вместе с инструкциями INSERT и UPDATE) позволяют указать значение по умолчанию, используемое, если значение не задано. Например, база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] может включать таблицу уточняющих запросов, содержащую различные должности, которые могут занимать сотрудники компании. В столбце, описывающем каждую должность, значение символьной строки по умолчанию может содержать описание, отображаемое, если фактическое описание должности не было введено явно.

```sql
DEFAULT 'New Position - title not formalized yet'
```

Кроме констант, определения DEFAULT могут включать функции. Следующий пример позволяет получить текущую дату для той или иной записи.

```sql
DEFAULT (GETDATE())
```

Обработка функциями без параметров также может повысить целостность данных. Чтобы определить пользователя, вставившего строку, используйте функцию без параметров для USER. Не заключайте функции без параметров в скобки.

```sql
DEFAULT USER
```

### <a name="e-using-check-constraints"></a>Д. Использование ограничений CHECK

В следующем примере показано ограничение, применяемое к значениям, вводимым в столбец `CreditRating` таблицы `Vendor`. Ограничение не имеет имени.

```sql
CHECK (CreditRating >= 1 and CreditRating <= 5)
```

В этом примере показано именованное ограничение вводимых в столбец таблицы символьных данных по шаблону.

```sql
CONSTRAINT CK_emp_id CHECK (
    emp_id LIKE '[A-Z][A-Z][A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
    OR emp_id LIKE '[A-Z]-[A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
)
```

В этом примере указывается, что значения должны входить в заданный список или соответствовать заданному шаблону.

```sql
CHECK (
    emp_id IN ('1389', '0736', '0877', '1622', '1756')
    OR emp_id LIKE '99[0-9][0-9]'
)
```

### <a name="f-showing-the-complete-table-definition"></a>Е. Вывод на экран полного определения таблицы

В следующем примере выводятся полные определения таблицы со всеми определениями ограничений для таблицы `PurchaseOrderDetail`, созданной в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Обратите внимание, что для выполнения этого образца схема таблицы заменяется на схему `dbo`.

```sql
CREATE TABLE dbo.PurchaseOrderDetail
(
    PurchaseOrderID int NOT NULL
        REFERENCES Purchasing.PurchaseOrderHeader(PurchaseOrderID),
    LineNumber smallint NOT NULL,
    ProductID int NULL
        REFERENCES Production.Product(ProductID),
    UnitPrice money NULL,
    OrderQty smallint NULL,
    ReceivedQty float NULL,
    RejectedQty float NULL,
    DueDate datetime NULL,
    rowguid uniqueidentifier ROWGUIDCOL NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_rowguid DEFAULT (NEWID()),
    ModifiedDate datetime NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_ModifiedDate DEFAULT (GETDATE()),
    LineTotal AS ((UnitPrice*OrderQty)),
    StockedQty AS ((ReceivedQty-RejectedQty)),
    CONSTRAINT PK_PurchaseOrderDetail_PurchaseOrderID_LineNumber
               PRIMARY KEY CLUSTERED (PurchaseOrderID, LineNumber)
               WITH (IGNORE_DUP_KEY = OFF)
)
ON PRIMARY;
```

### <a name="g-creating-a-table-with-an-xml-column-typed-to-an-xml-schema-collection"></a>Ж. Создание таблицы со столбцом, приведенным к типу коллекции схем XML

В следующем примере создается таблица со столбцом `xml`, приведенным к типу коллекции схем XML `HRResumeSchemaCollection`. Ключевое слово `DOCUMENT` указывает, что каждый экземпляр типа данных `xml` в столбце *column_name* может содержать только один элемент верхнего уровня.

```sql
CREATE TABLE HumanResources.EmployeeResumes
(
    LName nvarchar(25),
    FName nvarchar(25),
    Resume xml(DOCUMENT HumanResources.HRResumeSchemaCollection)
);
```

### <a name="h-creating-a-partitioned-table"></a>З. Создание секционированной таблицы

В следующем примере создается функция секционирования для разделения таблицы или индекса на четыре секции. Затем создается схема секционирования, определяющая файловые группы, в которых содержится каждая из четырех секций. Наконец, создается таблица, использующая схему секционирования. В примере предполагается, что в базе данных уже существуют файловые группы.

```sql
CREATE PARTITION FUNCTION myRangePF1 (int)
    AS RANGE LEFT FOR VALUES (1, 100, 1000);
GO

CREATE PARTITION SCHEME myRangePS1
    AS PARTITION myRangePF1
    TO (test1fg, test2fg, test3fg, test4fg);
GO  
  
CREATE TABLE PartitionTable (col1 int, col2 char(10))
    ON myRangePS1 (col1);
GO
```

Секции назначаются на основе столбца `col1` таблицы `PartitionTable` следующими способами.

|Файловая группа|test1fg|test2fg|test3fg|test4fg|
|---------------|-------------|-------------|-------------|-------------|
|**Секция**|1|2|3|4|
|**Значения**|col 1 \<= 1|col1 > 1 AND col1 \<= 100|col1 > 100 AND col1 \<= 1000|col1 > 1000|

### <a name="i-using-the-uniqueidentifier-data-type-in-a-column"></a>И. Использование типа данных uniqueidentifier в столбце

В следующем примере создается таблица со столбцом типа `uniqueidentifier`. В этом примере используется ограничение PRIMARY KEY для защиты таблицы от вставки пользователями повторяющихся значений, а также функция `NEWSEQUENTIALID()` в ограничении `DEFAULT` для указания значений для новых строк. К столбцу `uniqueidentifier` применяется свойство ROWGUIDCOL, чтобы на столбец можно было ссылаться с помощью ключевого слова $ROWGUID.

```sql
CREATE TABLE dbo.Globally_Unique_Data
(
    GUID UNIQUEIDENTIFIER
        CONSTRAINT Guid_Default DEFAULT
        NEWSEQUENTIALID() ROWGUIDCOL,
    Employee_Name VARCHAR(60)
    CONSTRAINT Guid_PK PRIMARY KEY (GUID)
);
```

### <a name="j-using-an-expression-for-a-computed-column"></a>К. Использование выражения для вычисляемого столбца

В следующем примере показано использование выражения (`(low + high)/2`) для вычисления столбца `myavg`.

```sql
CREATE TABLE dbo.mytable
(
    low INT,
    high INT,
    myavg AS (low + high)/2
);
```

### <a name="k-creating-a-computed-column-based-on-a-user-defined-type-column"></a>Л. Создание вычисляемого столбца на основе столбца определяемого пользователем типа

В следующем примере создается таблица с одним столбцом, имеющим определяемый пользовательским тип `utf8string`, и предполагается, что как сборка, содержащая данный тип, так и сам тип, уже созданы в текущей базе данных. Второй столбец определяется на основе типа `utf8string` и использует метод `ToString()` типа **type(class)** `utf8string` для вычисления значения столбца.

```sql
CREATE TABLE UDTypeTable
(
    u UTF8STRING,
    ustr AS u.ToString() PERSISTED
);
```

### <a name="l-using-the-user_name-function-for-a-computed-column"></a>М. Использование функции USER_NAME для вычисляемого столбца

В следующем примере используется функция `USER_NAME()` в столбце `myuser_name`.

```sql
CREATE TABLE dbo.mylogintable
(
    date_in DATETIME,
    user_id INT,
    myuser_name AS USER_NAME()
);
```

### <a name="m-creating-a-table-that-has-a-filestream-column"></a>Н. Создание таблицы со столбцом FILESTREAM

В следующем примере создается таблица со столбцом `FILESTREAM``Photo`. Если таблица содержит один или более столбцов `FILESTREAM`, она должна содержать столбец `ROWGUIDCOL`.

```sql
CREATE TABLE dbo.EmployeePhoto
(
    EmployeeId INT NOT NULL PRIMARY KEY,
    Photo VARBINARY(MAX) FILESTREAM NULL,
    MyRowGuidColumn UNIQUEIDENTIFIER NOT NULL ROWGUIDCOL UNIQUE DEFAULT NEWID()
);
```

### <a name="n-creating-a-table-that-uses-row-compression"></a>О. О. Создание таблицы, использующей сжатие строк

В следующем примере создается таблица, использующая сжатие строк.

```sql
CREATE TABLE dbo.T1
(
    c1 INT,
    c2 NVARCHAR(200)
)
WITH (DATA_COMPRESSION = ROW);
```

Дополнительные примеры сжатия данных см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).

### <a name="o-creating-a-table-that-has-sparse-columns-and-a-column-set"></a>П. Создание таблицы с разреженными столбцами и набором столбцов

В следующих примерах показано создание таблицы с разреженным столбцом и таблицы с двумя разреженными столбцами и набором столбцов. В примерах используется основной синтаксис. Более сложные примеры см. в разделе [Использование разреженных столбцов](../../relational-databases/tables/use-sparse-columns.md) и [Использование наборов столбцов](../../relational-databases/tables/use-column-sets.md).

В следующем примере создается таблица с разреженным столбцом.

```sql
CREATE TABLE dbo.T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL
);
```

В этом примере создается таблица с двумя разреженными столбцами и набором столбцов с именем `CSet`.

```sql
CREATE TABLE T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL,
    c3 INT SPARSE NULL,
    CSet XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
);
```

### <a name="p-creating-a-system-versioned-disk-based-temporal-table"></a>Т. Создание темпоральной таблицы на основе диска с системным управлением версиями

**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

В следующих примерах показано, как создать темпоральную таблицу, привязанную к новой таблице журнала, и как создать темпоральную таблицу, привязанную к существующей таблице журнала. Для темпоральной таблицы должен быть определен первичный ключ, чтобы включить системное управление версиями. Примеры добавления или удаления системного управления версиями в существующей таблице см. в главе [Примеры](../../t-sql/statements/alter-table-transact-sql.md#Example_Top) в разделе "Системное управление версиями". Варианты использования описаны в разделе [Темпоральные таблицы](../../relational-databases/tables/temporal-tables.md).

В этом примере создается новая темпоральная таблица, привязанная к новой таблице журнала.

```sql
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON);
```

В этом примере создается новая темпоральная таблица, привязанная к существующей таблице журнала.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON));
```

### <a name="q-creating-a-system-versioned-memory-optimized-temporal-table"></a>У. Создание оптимизированной для памяти темпоральной таблицы с системным управлением версиями

**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

В следующем примере показано, как создать оптимизированную для памяти темпоральную таблицу с системным управлением версиями, привязанную к новой таблице журнала на диске.

В этом примере создается новая темпоральная таблица, привязанная к новой таблице журнала.

```sql
CREATE SCHEMA History;
GO

CREATE TABLE dbo.Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY NONCLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    MEMORY_OPTIMIZED = ON,
    DURABILITY = SCHEMA_AND_DATA,
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)
);
```

В этом примере создается новая темпоральная таблица, привязанная к существующей таблице журнала.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON)
);
```

### <a name="r-creating-a-table-with-encrypted-columns"></a>Ф. Создание таблицы с зашифрованными столбцами

В следующем примере создается таблица с двумя зашифрованными столбцами. Дополнительные сведения см. в разделе [Постоянное шифрование](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

```sql
CREATE TABLE Customers (
    CustName NVARCHAR(60)
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = RANDOMIZED,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    SSN VARCHAR(11) COLLATE Latin1_General_BIN2
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = DETERMINISTIC ,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    Age INT NULL
);
```

### <a name="s-create-an-inline-filtered-index"></a>Х. Создание встроенного фильтруемого индекса

В этом примере создается таблица со встроенным фильтруемым индексом.

```sql
CREATE TABLE t1
(
    c1 INT,
    index IX1 (c1) WHERE c1 > 0
);
```

### <a name="t-create-an-inline-index"></a>T. Создание встроенного индекса

В приведенном ниже примере показано использование встроенного параметра NONCLUSTERED для дисковых таблиц.

```sql
CREATE TABLE t1
(
    c1 INT,
    INDEX ix_1 NONCLUSTERED (c1)
);

CREATE TABLE t2
(
    c1 INT,
    c2 INT INDEX ix_1 NONCLUSTERED
);

CREATE TABLE t3
(
    c1 INT,
    c2 INT,
    INDEX ix_1 NONCLUSTERED (c1,c2)
);
```

### <a name="u-create-a-temporary-table-with-an-anonymously-named-compound-primary-key"></a>Ф. Создание временной таблицы с анонимным составным первичным ключом.

В примере создается таблица с анонимным составным первичным ключом. Это полезно для предотвращения конфликтов во время выполнения, если две временные таблицы, областью действия которых являются сеансы (каждая в отдельном сеансе), используют одно и то же имя для ограничения.

```sql
CREATE TABLE #tmp
(
    c1 INT,
    c2 INT,
    PRIMARY KEY CLUSTERED ([c1], [c2])
);
GO
```

Если вы явным образом именуете ограничение, во втором сеансе возникнет ошибка, например:

```
Msg 2714, Level 16, State 5, Line 1
There is already an object named 'PK_#tmp' in the database.
Msg 1750, Level 16, State 1, Line 1
Could not create constraint or index. See previous errors.
```

Причина проблемы в том, что имя временной таблицы уникально, а имена ограничений — нет.

### <a name="v-using-global-temporary-tables-in-azure-sql-database"></a>V. Использование глобальных временных таблиц в базе данных SQL Azure

В сеансе A создается глобальная временная таблица ##test в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1 и добавляется 1 строка.

```sql
CREATE TABLE ##test (
    a INT,
    b INT
);

INSERT INTO ##test
VALUES (1, 1);

-- Obtain object ID for temp table ##test
SELECT OBJECT_ID('tempdb.dbo.##test') AS 'Object ID';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
1253579504
```

Получаем имя глобальной временной таблицы для идентификатора объекта 1253579504 в tempdb (2)

```sql
SELECT name FROM tempdb.sys.objects WHERE object_id = 1253579504;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
##test
```

В сеансе B устанавливается подключение к [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1 и доступ к таблице ##test, созданной в сеансе A.

```sql
SELECT * FROM ##test;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
1, 1
```

В сеансе C устанавливается подключение к другой базе данных [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb2 и выполняется попытка получить доступ к таблице ##test, созданной в базе данных testdb1. Это невозможно, поскольку глобальные временные таблицы существуют в области базы данных

```sql
SELECT * FROM ##test
```

В результате выдается следующая ошибка:

```
Msg 208, Level 16, State 0, Line 1
Invalid object name '##test'
```

Обращение к системному объекту в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] tempdb из текущей базы данных пользователя testdb1.

```sql
SELECT * FROM tempdb.sys.objects;
SELECT * FROM tempdb.sys.columns;
SELECT * FROM tempdb.sys.database_files;
```   

### <a name="w-enable-data-retention-policy-on-a-table"></a>Ц. Включение политики хранения данных в таблице

В следующем примере создается таблица с включенным хранением данных и периодом хранения, равным 1 неделе. Этот пример применяется только к **SQL Azure для пограничных вычислений**.

```sql
CREATE TABLE [dbo].[data_retention_table] 
(
  [dbdatetime2] datetime2(7), 
  [product_code] int, 
  [value] char(10)
) 
WITH (DATA_DELETION = ON ( FILTER_COLUMN = [dbdatetime2], RETENTION_PERIOD = 1 WEEKS ))
```

## <a name="next-steps"></a>Дальнейшие действия

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)
- [Типы данных](../../t-sql/data-types/data-types-transact-sql.md)
- [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)
- [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)
- [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)
- [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [sp_helpconstraint](../../relational-databases/system-stored-procedures/sp-helpconstraint-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
