---
description: ALTER TABLE (Transact-SQL)
title: ALTER TABLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WAIT_AT_LOW_PRIORITY
- ABORT_AFTER_WAIT
- ABORT_AFTER_WAIT_TSQL
- ALTER_TABLE_TSQL
- ALTER TABLE
- WAIT_AT_LOW_PRIORITY_TSQL
- ALTER_COLUMN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- columns [SQL Server], resizing
- changing column size
- MAXDOP index option, ALTER TABLE statement
- table modifications [SQL Server], ALTER TABLE
- ALTER TABLE statement
- modifying tables
- partitioned tables [SQL Server], lock escalation
- resizing columns
- removing columns
- switching partitions
- reassigning partitions
- removing constraints
- triggers [SQL Server], disabling
- columns [SQL Server], adding
- LOCK_ESCALATION option of ALTER TABLE
- constraints [SQL Server], deleting
- constraints [SQL Server], disabling
- triggers [SQL Server], enabling
- re-enabling constraints
- index modifications [SQL Server]
- disabling constraints
- columns [SQL Server], removing
- max degree of parallelism option
- locking [SQL Server], tables
- ONLINE option
- disabling triggers
- constraints [SQL Server], adding
- deleting constraints
- adding constraints
- adding columns
- SWITCH partitions
- partitioned tables [SQL Server], switching
- lock escalation [SQL Server], option of ALTER TABLE
- constraints [SQL Server], enabling
- dropping constraints
- dropping columns
- data retention policy
- table changes [SQL Server]
ms.assetid: f1745145-182d-4301-a334-18f799d361d1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d604298ca4956ef699ab763255cb2534be30679
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170636"
---
# <a name="alter-table-transact-sql"></a>ALTER TABLE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Изменяет определение таблицы путем изменения, добавления или удаления столбцов и ограничений. Также ALTER TABLE переназначает и перестраивает секции или отключает и включает ограничения и триггеры.

Дополнительные сведения о соглашениях о синтаксисе см. в статье [Соглашения о синтаксисе в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!IMPORTANT]
> Инструкция ALTER TABLE имеет разный синтаксис для таблиц на диске и таблиц, оптимизированных для памяти. Воспользуйтесь ссылками ниже, которые ведут непосредственно к описаниям соответствующих синтаксических блоков для ваших типов таблиц и соответствующим примерам:
>
> - Таблицы на диске:
>
>   - [Синтаксис](#syntax-for-disk-based-tables)
>   - [Примеры](#Example_Top)
> - Таблицы, оптимизированные для памяти
>
>   - [Синтаксис](#syntax-for-memory-optimized-tables)
>   - [Примеры](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

## <a name="syntax-for-disk-based-tables"></a>Синтаксис для таблиц на диске

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                 | max
                 | xml_schema_collection
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ] [ SPARSE ]
      | { ADD | DROP }
          { ROWGUIDCOL | PERSISTED | NOT FOR REPLICATION | SPARSE | HIDDEN }
      | { ADD | DROP } MASKED [ WITH ( FUNCTION = ' mask_function ') ]
    }
    [ WITH ( ONLINE = ON | OFF ) ]
    | [ WITH { CHECK | NOCHECK } ]
  
    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <column_set_definition>
    } [ ,...n ]
      | [ system_start_time_column_name datetime2 GENERATED ALWAYS AS ROW START
                [ HIDDEN ] [ NOT NULL ] [ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
                system_end_time_column_name datetime2 GENERATED ALWAYS AS ROW END
                   [ HIDDEN ] [ NOT NULL ][ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
        ]
       PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )
    | DROP
     [ {
         [ CONSTRAINT ][ IF EXISTS ]
         {
              constraint_name
              [ WITH
               ( <drop_clustered_constraint_option> [ ,...n ] )
              ]
          } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }
  
    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | { ENABLE | DISABLE } CHANGE_TRACKING
        [ WITH ( TRACK_COLUMNS_UPDATED = { ON | OFF } ) ]
  
    | SWITCH [ PARTITION source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]

    | SET
        (
            [ FILESTREAM_ON =
                { partition_scheme_name | filegroup | "default" | "NULL" } ]
            | SYSTEM_VERSIONING =
                  {
                      OFF
                  | ON
                      [ ( HISTORY_TABLE = schema_name . history_table_name
                          [, DATA_CONSISTENCY_CHECK = { ON | OFF } ]
                          [, HISTORY_RETENTION_PERIOD =
                          {
                              INFINITE | number {DAY | DAYS | WEEK | WEEKS
                  | MONTH | MONTHS | YEAR | YEARS }
                          }
                          ]
                        )
                      ]
                  }
            | DATA_DELETION =  
                {
                      OFF 
                    | ON  
                        [(  [ FILTER_COLUMN = column_name ]   
                            [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                                    | MONTH | MONTHS | YEAR | YEARS }}]   
                        )]
                   }
    | REBUILD
      [ [PARTITION = ALL]
        [ WITH ( <rebuild_option> [ ,...n ] ) ]
      | [ PARTITION = partition_number
           [ WITH ( <single_partition_rebuild_option> [ ,...n ] ) ]
        ]
      ]
  
    | <table_option>
    | <filetable_option>
    | <stretch_configuration>
}
[ ; ]
  
-- ALTER TABLE options
  
<column_set_definition> ::=
    column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS

<drop_clustered_constraint_option> ::=
    {
        MAXDOP = max_degree_of_parallelism
      | ONLINE = { ON | OFF }
      | MOVE TO
         { partition_scheme_name ( column_name ) | filegroup | "default" }
    }
<table_option> ::=
    {
        SET ( LOCK_ESCALATION = { AUTO | TABLE | DISABLE } )
    }
  
<filetable_option> ::=
    {
       [ { ENABLE | DISABLE } FILETABLE_NAMESPACE ]
       [ SET ( FILETABLE_DIRECTORY = directory_name ) ]
    }
  
<stretch_configuration> ::=
    {
      SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options>)
          | = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED )
          | ( <table_stretch_options> [, ...n] )
        }
            )
    }
  
<table_stretch_options> ::=
    {
     [ FILTER_PREDICATE = { null | table_predicate_function } , ]
       MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
    }
  
<single_partition_rebuild__option> ::=
{
      SORT_IN_TEMPDB = { ON | OFF }
    | MAXDOP = max_degree_of_parallelism
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE} }
    | ONLINE = { ON [( <low_priority_lock_wait> ) ] | OFF }
}
  
<low_priority_lock_wait>::=
{
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ],
        ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )
}
```

> [!NOTE]
> Дополнительные сведения см. в следующих источниках:
>
> - [ALTER TABLE ограничение_столбца](alter-table-column-constraint-transact-sql.md)
> - [ALTER TABLE определение_столбца](alter-table-column-definition-transact-sql.md)
> - [ALTER TABLE определение_вычисляемого_столбца](alter-table-computed-column-definition-transact-sql.md)
> - [ALTER TABLE вариант_индекса](alter-table-index-option-transact-sql.md)
> - [ALTER TABLE ограничения_таблицы](alter-table-table-constraint-transact-sql.md)

## <a name="syntax-for-memory-optimized-tables"></a>Синтаксис для таблиц, оптимизированных для памяти

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ]
    }

    | ALTER INDEX index_name
    {
        [ type_schema_name. ] type_name
        REBUILD
        [ [ NONCLUSTERED ] WITH ( BUCKET_COUNT = bucket_count )
        ]
    }

    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <table_index>
      | <column_index>
    } [ ,...n ]
  
    | DROP
     [ {
         CONSTRAINT [ IF EXISTS ]
         {
              constraint_name
          } [ ,...n ]
        | INDEX [ IF EXISTS ]
      {
         index_name
       } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }

    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | SWITCH [ [ PARTITION ] source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]
    
}
[ ; ]

-- ALTER TABLE options

< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   {PRIMARY KEY | UNIQUE }
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
{[ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
}

```

## <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Синтаксис для Azure Synapse Analytics и Parallel Data Warehouse

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

ALTER TABLE { database_name.schema_name.source_table_name | schema_name.source_table_name | source_table_name }
{
    ALTER COLUMN column_name
        {
            type_name [ ( precision [ , scale ] ) ]
            [ COLLATE Windows_collation_name ]
            [ NULL | NOT NULL ]
        }
    | ADD { <column_definition> | <column_constraint> FOR column_name} [ ,...n ]
    | DROP { COLUMN column_name | [CONSTRAINT] constraint_name } [ ,...n ]
    | REBUILD {
            [ PARTITION = ALL [ WITH ( <rebuild_option> ) ] ]
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_option> ] ]
      }
    | { SPLIT | MERGE } RANGE (boundary_value)
    | SWITCH [ PARTITION source_partition_number
        TO target_table_name [ PARTITION target_partition_number ] [ WITH ( TRUNCATE_TARGET = ON | OFF )
}
[;]

<column_definition>::=
{
    column_name
    type_name [ ( precision [ , scale ] ) ]
    [ <column_constraint> ]
    [ COLLATE Windows_collation_name ]
    [ NULL | NOT NULL ]
}

<column_constraint>::=
    [ CONSTRAINT constraint_name ] 
    {
        DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }
<rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]
}

<single_partition_rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*database_name*  
Имя базы данных, в которой создана таблица.

*schema_name*  
Имя схемы, которой принадлежит таблица.

*table_name*  
Имя изменяемой таблицы. Если такой таблицы нет в текущей базе данных или схеме, которой владеет текущий пользователь, их следует явным образом указать.

ALTER COLUMN  
Указывает, что именованный столбец подлежит изменению.

Не поддерживается изменение столбцов следующих типов:

- Столбец типа данных **timestamp**.
- Свойство ROWGUIDCOL для таблицы.
- Вычисляемый столбец или используемый в вычисляемом столбце.
- Используемый в статистике, созданной с помощью инструкции CREATE STATISTICS. Пользователям необходимо выполнить инструкцию DROP STATISTICS, чтобы удалить статистику, прежде чем инструкция ALTER COLUMN может быть выполнена.  Выполните этот запрос, чтобы получить все созданные пользователем статистические данные и статистические столбцы для таблицы.

``` sql

SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<table_name>'); 

```
   > [!NOTE]
   > Статистика, автоматически сформированная оптимизатором запросов, автоматически удаляется инструкцией ALTER COLUMN.

- Используется в ограничении PRIMARY KEY или [FOREIGN KEY] REFERENCES.
- Используется в ограничениях CHECK или UNIQUE. При этом допускается изменение длины столбца изменяемой длины, используемого в ограничении CHECK или UNIQUE.
- Связано с определением по умолчанию. Если же тип данных не изменяется, можно изменить длину, точность или масштаб столбца.

Тип данных в столбцах **text**, **ntext** и **image** может быть изменен только следующими способами:

- **text** на **varchar(max)** , **nvarchar(max)** или **xml**;
- **ntext** на **varchar(max)** , **nvarchar(max)** или **xml**;
- **image** на **varbinary(max)** .

Некоторые изменения типов данных могут повлечь за собой изменения в данных. Например, преобразование столбца типа **nchar** или **nvarchar** в столбец типа **char** или **varchar** может привести к преобразованию расширенных символов. См. описание [CAST и CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md). Снижение точности или масштаба столбца может привести к усечению данных.

> [!NOTE]
> Нельзя изменить тип данных для столбца секционированной таблицы.
>
> Нельзя изменить тип данных для столбцов, включенных в индекс, кроме столбцов с типами данных **varchar**, **nvarchar** или **varbinary**, если новый размер больше старого или равен ему.
>
> Для столбцов, включенных в ограничение первичного ключа, нельзя изменить условие **NOT NULL** на **NULL**.

Если при использовании функции Always Encrypted (без безопасных анклавов) изменяемый столбец зашифрован с помощью ENCRYPTED WITH, тип данных можно изменить на совместимый (например, INT на BIGINT), но нельзя изменить параметры шифрования.

При использовании функции Always Encrypted с безопасными анклавами вы можете изменять любой параметр шифрования, если используемый для защиты столбца ключ шифрования (и новый ключ шифрования столбца, если вы изменяете его) поддерживает анклавные вычисления (то есть зашифрован главными ключами столбца с поддержкой анклава). Дополнительные сведения см. в статье [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*column_name*  
Имя столбца, который требуется изменить, добавить или удалить. Длина имени *column_name* не может превышать 128 символов. Для новых столбцов, созданных с типом данных **timestamp**, аргумент *column_name* можно опустить. Если для столбца типа **timestamp** не указан аргумент *column_name*, используется имя **timestamp**.

> [!NOTE]
> Новые столбцы добавляются после изменения всех существующих столбцов в таблице.

[ _type\_schema\_name_ **.** ] _type\_name_  
Новый тип данных для изменяемого столбца либо тип данных для добавляемого столбца. Значение *type_name* нельзя задать для существующих столбцов секционированных таблиц. Тип *type_name* может иметь любое из следующих значений:

- Системным типом данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Псевдонимом типа данных, основанным на системном типе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Прежде чем использовать псевдонимы типов данных в определении таблицы, их нужно создать с помощью инструкции CREATE TYPE.
- Определяемый пользователем тип [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] и схема, к которой он принадлежит. Прежде чем использовать пользовательские типы в определении таблицы, их нужно создать с помощью инструкции CREATE TYPE.

Далее приведены критерии для аргумента *type_name* изменяемого столбца.

- Предыдущие типы данных должны быть неявно преобразуемыми в новый тип данных.
- Аргумент *type_name* не может иметь значение **timestamp**.
- По умолчанию для аргумента ANSI_NULL инструкции ALTER COLUMN всегда установлено значение ON; если не указано иное, столбец может содержать значения NULL.
- Аргумент заполнения ANSI_PADDING для инструкции ALTER COLUMN всегда принимает значение ON.
- Если изменяемый столбец является столбцом идентификаторов, то аргумент *new_data_type* должен иметь тип данных, который поддерживает свойство идентификатора.
- Текущая установка для аргумента SET ARITHABORT пропускается. Инструкция ALTER TABLE функционирует аналогично случаю, когда для аргумента ARITHABORT установлено значение ON.

> [!NOTE]
> Если предложение COLLATE не указывается, изменение типа данных для столбца приведет к изменению параметров сортировки на те, которые установлены для базы данных по умолчанию.

*precision*  
Точность указанного типа данных. Дополнительные сведения о допустимых значениях точности см. в разделе [Точность, масштаб и длина](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*масштаб*  
Масштаб указанного типа данных. Дополнительные сведения о допустимых значениях масштаба см. в разделе [Точность, масштаб и длина](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max**  
Применяется только к типам данных **varchar**, **nvarchar** и **varbinary** для хранения 2^31-1 байт символьных, двоичных данных и данных в Юникоде.

*xml_schema_collection*  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Применяется только к данным типа **xml** для связывания схемы XML с этим типом. Прежде чем включать столбец **xml** в коллекцию схемы, необходимо создать коллекцию схемы в базе данных с помощью инструкции [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).

COLLATE \< *collation_name* >  
Задает новые параметры сортировки для изменяемого столбца. Если не указано, столбцу назначаются параметры сортировки, принятые в базе данных по умолчанию. Именем параметров сортировки может быть либо имя параметров сортировки Windows, либо имя параметров сортировки SQL. Список и дополнительные сведения см. в статьях [Имя параметра сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) и [Имя параметра сортировки SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Предложение COLLATE изменяет параметры сортировки только для столбцов с типами данных **char**, **varchar**, **nchar** и **nvarchar**. Чтобы изменить параметры сортировки для столбца с пользовательским псевдонимом типа данных, с помощью отдельных инструкций ALTER TABLE преобразуйте этот столбец в системный тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Затем измените параметры сортировки и снова преобразуйте столбец в прежний тип данных.

Инструкция ALTER COLUMN не может изменить параметры сортировки, если выполняется одно или несколько из следующих условий:

- Если на изменяемый столбец ссылается ограничение CHECK, ограничение FOREIGN KEY или вычисляемые столбцы.
- Если на базе столбца создан какой-нибудь индекс, статистика или полнотекстовый индекс. Статистика, автоматически созданная на базе изменяемого столбца, удаляется, если изменяются параметры сортировки столбца.
- Если связанное со схемой представление или функция ссылаются на столбец.

Дополнительные сведения см. в описании [COLLATE](~/t-sql/statements/collations.md).

NULL | NOT NULL  
Указывает, может ли столбец принимать значения NULL. Столбцы, не допускающие значения NULL, могут быть добавлены инструкцией ALTER TABLE, только если для них указаны значения по умолчанию или если таблица пуста. Ограничение NOT NULL можно указать для вычисляемых столбцов только вместе с аргументом PERSISTED. Если новый столбец допускает значения NULL, а значение по умолчанию не задано, новый столбец получает значение NULL для каждой строки в таблице. Если новый столбец допускает значение NULL и с новым столбцом добавляется определение по умолчанию, вы можете поместить в новый столбец значения по умолчанию для каждой строки в таблице с помощью аргумента WITH VALUES.

Если новый столбец не допускает значения NULL и таблица не пуста, вместе с новым столбцом необходимо добавить определение DEFAULT. В этом случае новый столбец автоматически получит значение по умолчанию для каждой существующей строки.

Вы можете указать значение NULL в инструкции ALTER COLUMN, чтобы столбец NOT NULL мог принимать значения NULL, если он не включен в ограничения PRIMARY KEY. Условие NOT NULL можно указывать в инструкции ALTER COLUMN только для тех столбцов, которые не содержат значения NULL. Значения NULL следует обновить, присвоив некоторые значения, прежде чем разрешить инструкцию ALTER COLUMN NOT NULL, например:

```sql
UPDATE MyTable SET NullCol = N'some_value' WHERE NullCol IS NULL;
ALTER TABLE MyTable ALTER COLUMN NullCOl NVARCHAR(20) NOT NULL;
```

Когда вы создаете или изменяете таблицу с помощью инструкций CREATE TABLE или ALTER TABLE, допустимость значений NULL для типа данных, указанного в определении столбца, зависит от параметров базы данных и сеанса или даже переопределяется ими. Для всех невычисляемых столбцов явно указывайте атрибут NULL или NOT NULL.

Добавляя столбец с пользовательским типом данных, обязательно определите для этого столбца допустимость значений NULL, как указано в соответствующем пользовательском типе данных. Кроме того, укажите значение по умолчанию для этого столбца. Дополнительные сведения см. в разделе [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).

> [!NOTE]
> Если в инструкции ALTER COLUMN указано значение NULL или NOT NULL, то необходимо также указать параметры *new_data_type* [(*precision* [, *scale* ])]. Если тип данных, точность или масштаб не изменялись, укажите текущие значения столбца.

[ {ADD | DROP} ROWGUIDCOL ]  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает, что свойство ROWGUIDCOL добавляется к указанному столбцу или удаляется из него. Свойство ROWGUIDCOL указывает, что данный столбец является столбцом идентификатора GUID строки. В каждой таблице только один столбец типа **uniqueidentifier** может иметь свойство ROWGUIDCOL. Кроме того, свойство ROWGUIDCOL можно присвоить только столбцу типа **uniqueidentifier**. Вы не можете присвоить свойство ROWGUIDCOL столбцу с пользовательским типом данных.

Свойство ROWGUIDCOL не обеспечивает уникальность значений, хранимых в столбце, и не формирует автоматически значения для новых строк, вставляемых в таблицу. Чтобы создавать уникальные значения для каждого столбца, примените функцию NEWID или NEWSEQUENTIALID в инструкции INSERT. Также вы можете указать функцию NEWID или NEWSEQUENTIALID как значение по умолчанию для столбца.

[ {ADD | DROP} PERSISTED ]  
Указывает, что свойство PERSISTED добавлено к указанному столбцу или удалено из него. Этот столбец должен быть вычисляемым столбцом, который заполняется детерминированным выражением. Для столбцов, указанных как PERSISTED, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] физически хранит вычисляемые значения в таблице и обновляет значения при обновлении любого столбца, от которого зависит вычисляемый столбец. Если пометить вычисляемый столбец как PERSISTED, можно создавать индексы по вычисляемым столбцам, которые заданы, являются детерминированными, но не точными выражениями. Дополнительные сведения см. в разделе [Индексы вычисляемых столбцов](../../relational-databases/indexes/indexes-on-computed-columns.md).

Любой вычисляемый столбец, который используется как столбец секционирования для секционированной таблицы, должен быть явно помечен с помощью атрибута PERSISTED.

DROP NOT FOR REPLICATION  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает, что значения в столбцах идентификаторов увеличиваются при выполнении агентами репликации операций по вставке строк. Вы можете указать это предложение, только если *column_name* является столбцом идентификаторов.

SPARSE  
Указывает, что столбец является разреженным столбцом. Хранилище разреженных столбцов оптимизируется для значений NULL. Разреженные столбцы не могут иметь свойство NOT NULL. При преобразовании столбцов из разреженных в неразреженные или наоборот таблица блокируется на время выполнения этой команды. Возможно, потребуется использование предложения REBUILD для освобождения пространства. Дополнительные ограничения и сведения о разреженных столбцах см. в разделе [Разреженные столбцы](../../relational-databases/tables/use-sparse-columns.md).

ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает маску для динамического маскирования данных. *mask_function* — это имя функции маскирования с соответствующими параметрами. Доступны три функции:

- default()
- email()
- partial()
- random()

Чтобы удалить маску, используйте `DROP MASKED`. Параметры функции см. в разделе [Динамическое маскирование данных](../../relational-databases/security/dynamic-data-masking.md).

WITH ( ONLINE = ON | OFF) \<as applies to altering a column>  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Позволяет выполнять разные действия по изменению столбцов с сохранением доступности таблицы. По умолчанию — OFF. Изменение столбцов можно выполнять в оперативном режиме, если эти изменения связаны с типом данных, длиной или точностью столбцов, допустимостью значений NULL, разреженностью и параметрами сортировки.

Изменение столбцов в оперативном режиме позволяет использовать изменяемые столбцы в пользовательской и автоматической статистике на всем протяжении операции ALTER COLUMN, то есть не влияет на обычный режим выполнения запросов. В конце операции автоматическая статистика, которая ссылается на эти столбцы, удаляется, а созданная пользователем статистика становится недействительной. Пользователь должен вручную обновить созданную пользователем статистику после завершения операции. Если столбцы являются частью выражения фильтра для статистических данных или индексов, изменить такие столбцы нельзя.

- Во время изменения столбцов в оперативном режиме все операции, имеющие зависимость от этих столбцов (индексирование, представления и т. д.), будут блокироваться или завершаться с соответствующей ошибкой. Такое поведение гарантирует, что изменение столбцов в оперативном режиме не завершится сбоем из-за появления новых зависимостей во время выполнения этой операции.
- Изменение для столбцов свойства NOT NULL на NULL в оперативном режиме не поддерживается, если изменяемые столбцы используются в некластеризованных индексах.
- Изменение в оперативном режиме не поддерживается для столбцов, которые указаны в ограничении CHECK, если такая операция изменения ограничит точность этих столбцов (содержащего число или дату и время).
- При изменении столбцов в оперативном режиме нельзя использовать параметр `WAIT_AT_LOW_PRIORITY`.
- При изменении столбцов в оперативном режиме `ALTER COLUMN ... ADD/DROP PERSISTED` не поддерживается.
- Изменение столбцов в оперативном режиме не влияет на `ALTER COLUMN ... ADD/DROP ROWGUIDCOL/NOT FOR REPLICATION`.
- Изменение столбцов в оперативном режиме не позволяет изменить таблицу, для которой включено отслеживание изменений или которая является издателем репликации слиянием.
- Изменение столбцов в оперативном режиме не поддерживает изменение типа данных CLR на другой тип данных, или наоборот.
- Изменение столбцов в оперативном режиме не поддерживает изменение на тип данных XML, у которого коллекция схем отличается от текущей коллекции схем.
- Изменение столбцов в оперативном режиме не снижает другие ограничения, существующие для этой операции. Ссылки из индексов, статистических данных и т. п. могут привести к сбою изменения.
- Изменение в оперативном режиме для нескольких столбцов одновременно не поддерживается.
- Изменение столбцов в оперативном режиме никак не влияет на темпоральные таблицы с системным управлением версиями. Операция ALTER COLUMN никогда не выполняется в оперативном режиме, независимо от значения параметра ONLINE.

Для изменения столбцов в оперативном режиме действуют такие же ограничения и функции, что и для перестроения индекса в оперативном режиме. Среди прочего, сюда относится следующее:

- Перестроение индекса в оперативном режиме не поддерживается, если таблица содержит устаревшие столбцы с типом LOB или FILESTREAM или имеет индекс columnstore. Эти же ограничения действуют при изменении столбца в режиме «в сети».
- Для изменения существующих столбцов требуется удвоенный объем выделенного пространства — для исходного столбца и для создаваемого скрытого столбца.
- Стратегия блокировки во время операции изменения столбца в режиме «в сети» использует ту же модель блокировки, что и при перестроении индекса в режиме «в сети».

WITH CHECK | WITH NOCHECK  
Указывает, удовлетворяют ли данные в таблице недавно добавленному или повторно включенному ограничению FOREIGN KEY или CHECK. Если не указано иное, для новых ограничений предполагается условие WITH CHECK, а для повторно включенных ограничений — WITH NOCHECK.

Если вам не нужна проверка существующих данных по новым ограничениям CHECK или FOREIGN KEY, укажите WITH NOCHECK. Мы не рекомендуем применять этот вариант, за некоторыми редкими исключениями. Новое ограничение всегда проверяется при любых последующих обновлениях данных. Любые нарушения ограничения, подавленные условием WITH NOCHECK во время добавления ограничения, могут привести к сбою будущих обновлений, если новые значения строк не соответствуют этому ограничению.

> [!NOTE]
> Оптимизатор запросов не рассматривает ограничения, для которых определено условие WITH NOCHECK. Такие ограничения не учитываются до тех пор, пока они не будут повторно включены инструкцией `ALTER TABLE table WITH CHECK CHECK CONSTRAINT ALL`.

ALTER INDEX *index_name*  
Указывает, что число контейнеров для *index_name* должно быть изменено.

Синтаксис инструкции ALTER TABLE ADD/DROP/ALTER INDEX поддерживается только для таблиц, оптимизированных для памяти.

> [!IMPORTANT]
> Инструкции [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) и [PAD_INDEX](alter-table-index-option-transact-sql.md) не будут работать с индексами в таблицах, оптимизированных для памяти, если не используется инструкция ALTER TABLE.

ADD  
Указывает, что добавляется одно или несколько определений столбца, определений вычисляемого столбца или ограничений таблиц. Или же добавляются столбцы, которые система использует для системного управления версиями. Для таблиц, оптимизированных для памяти, можно добавить индекс.

> [!NOTE]
> Новые столбцы добавляются после изменения всех существующих столбцов в таблице.

> [!IMPORTANT]
> Инструкции [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) и [PAD_INDEX](alter-table-index-option-transact-sql.md) не будут работать с индексами в таблицах, оптимизированных для памяти, если не применить инструкцию ALTER TABLE.

PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает имена столбцов, которые система использует для обозначения периода действия записи. Можно указать существующие столбцы или создать новые столбцы как часть аргумента ADD PERIOD FOR SYSTEM_TIME. Настройте столбцы с типом данных datetime2 и определите для них условие NOT NULL. Если для столбца периода указать условие NULL, возвращается ошибка. Вы можете определить [column_constraint](../../t-sql/statements/alter-table-column-constraint-transact-sql.md) и (или) [указать значения по умолчанию для столбцов](../../relational-databases/tables/specify-default-values-for-columns.md) system_start_time и system_end_time. См. пример A в разделе [Системное управление версиями](#system_versioning) ниже, где показано использование значения по умолчанию для столбца system_end_time.

Используйте этот аргумент вместе с аргументом SYSTEM_VERSIONING, чтобы включить системное управление версиями для существующей таблицы. Дополнительные сведения см. в разделах [Темпоральные таблицы](../../relational-databases/tables/temporal-tables.md) и [Приступая к работе с темпоральными таблицами в базе данных SQL Azure](/azure/azure-sql/temporal-tables).

В версии [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] пользователи могут пометить один или оба столбца периода флагом **HIDDEN**, чтобы эти столбцы были неявно скрыты и инструкция **SELECT \* FROM \<table_name>** не возвращала значения этих столбцов. По умолчанию столбцы периода не скрываются. Чтобы использовать скрытые столбцы, их необходимо явно указывать во всех запросах, обращающихся к темпоральной таблице.

DROP  
Указывает, что удаляется одно или несколько определений столбца, определений вычисляемого столбца или ограничений таблиц либо удаляется спецификация столбцов, которые будут использоваться для системного управления версиями.

CONSTRAINT *constraint_name*  
Указывает, что из таблицы удалено ограничение *constraint_name*. Может быть перечислено несколько ограничений.

Вы можете выяснить имя ограничения, присвоенное пользователем или системой, с помощью запросов к представлениям каталога **sys.check_constraint**, **sys.default_constraints**, **sys.key_constraints** и **sys.foreign_keys**.

Невозможно удалить ограничение PRIMARY KEY, если для таблицы существует XML-индекс.

INDEX *index_name*  
Указывает, что *index_name* удалено из таблицы.

Синтаксис инструкции ALTER TABLE ADD/DROP/ALTER INDEX поддерживается только для таблиц, оптимизированных для памяти.

> [!IMPORTANT]
> Инструкции [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) и [PAD_INDEX](alter-table-index-option-transact-sql.md) не будут работать с индексами в таблицах, оптимизированных для памяти, если не используется инструкция ALTER TABLE.

COLUMN *column_name*  
Указывает, что *constraint_name* или *column_name* удаляется из таблицы. Можно перечислить несколько столбцов.

 Столбец невозможно удалить, если для него справедливо любое из следующих условий:

- Используется в индексе — как ключевой столбец или как INCLUDE.
- используется в ограничениях CHECK, FOREIGN KEY, UNIQUE или PRIMARY KEY;
- связан со значением по умолчанию, которое определено с ключевым словом DEFAULT или привязано к объекту по умолчанию;
- привязан к правилу.

> [!NOTE]
> При удалении столбца занимаемое им место на диске не освобождается. В том случае, если размер строк таблицы приближается к пределу или превышает его, возможен возврат места, занятого на диске удаленным столбцом. Возврат пространства осуществляется путем создания кластеризованного индекса в таблице или перестроения существующего кластеризованного индекса при помощи инструкции [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md). Дополнительные сведения о последствиях удаления типов данных больших двоичных объектов см. в этой [записи в блоге CSS](/archive/blogs/psssql/how-it-works-gotcha-varcharmax-caused-my-queries-to-be-slower).

PERIOD FOR SYSTEM_TIME  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Удаляет спецификацию для столбцов, которые будут использоваться для системного управления версиями.

WITH \<drop_clustered_constraint_option>  
Указывает, что установлен один или несколько параметров удаления кластеризованного ограничения.

MAXDOP = *max_degree_of_parallelism*  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Переопределяет параметр конфигурации **max degree of parallelism** только на время выполнения операции. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

Используйте параметр MAXDOP для ограничения числа процессоров, применяемых при выполнении параллельных планов. Максимальное число процессоров — 64.

*max_degree_of_parallelism* может принимать одно из следующих значений:

1  
Подавляет формирование параллельных планов.

\>1  
Ограничивает указанным значением максимальное число процессоров, используемых для параллельных операций с индексами.

0 (по умолчанию)  
В зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.

Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделах [Выпуски и поддерживаемые функции SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) и [Выпуски и поддерживаемые функции SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

ONLINE **=** { ON | **OFF** } \<as applies to drop_clustered_constraint_option>  
Определяет, будут ли базовые таблицы и связанные индексы доступны для запросов и изменения данных во время операций с индексами. Значение по умолчанию — OFF. Операцию REBUILD можно выполнять в оперативном режиме (ONLINE).

ON  
Долгосрочные блокировки таблицы не сохраняются на время операций с индексами. Во время главной фазы операций с индексами только блокировка с намерением совмещаемого доступа (IS) удерживается в исходной таблице. Такое поведение позволит продолжить выполнение запросов или обновлений для базовых таблиц и индексов. В начале операции на короткое время выполняется совмещаемая блокировка (S) исходного объекта. Если создается некластеризованный индекс, в конце операции на короткое время выполняется совмещаемая блокировка (S) для исходного объекта. Если в оперативном режиме создается или удаляется кластеризованный индекс либо перестраивается кластеризованный или некластеризованный индекс, запрашивается блокировка SCH-M (изменение схемы). При создании индекса для временной локальной таблицы параметр ONLINE не может принимать значение ON. Допустима только однопотоковая операция перестроения кучи.

Чтобы выполнить инструкцию DDL для операции **SWITCH** или перестроения индекса в оперативном режиме, должны быть завершены все активные блокирующие транзакции для соответствующей таблицы. Пока выполняется операция **SWITCH** или перестроение индекса, блокируется запуск новых транзакций, что может существенно повлиять на пропускную способность рабочей нагрузки и временно замедлить доступ к базовой таблице.

OFF  
Блокировки таблиц применяются на время выполнения операций с индексами. Блокировку изменения схемы (Sch-M) в таблице получает операция с индексами вне сети, которая создает, перестраивает или удаляет кластеризованный индекс либо перестраивает или удаляет некластеризованный индекс. Эта блокировка предотвращает любой доступ пользователей к базовой таблице на время операции. Операция с индексами вне сети, создающая некластеризованный индекс, получает совмещаемую блокировку (S) в таблице. Эта блокировка предотвращает обновления базовой таблицы, но разрешает операции чтения, например инструкции SELECT. Многопотоковые операции перестроения кучи разрешены.

Дополнительные сведения см. в разделе [Об операциях с индексами в режиме "в сети"](../../relational-databases/indexes/how-online-index-operations-work.md).

> [!NOTE]
> Операции с индексами в сети доступны не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделах [Выпуски и поддерживаемые функции SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) и [Выпуски и поддерживаемые функции SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ [ 1 **,** ...*n*] **)**  | *filegroup* |  **"** default **"** }  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает местоположение для перемещения строк данных, находящихся в настоящее время на конечном уровне кластеризованного индекса. Таблица перемещается на новое место. Этот параметр применяется только ограничениям, образующим кластеризованный индекс.

> [!NOTE]
> В этом контексте default не является ключевым словом. Это идентификатор файловой группы по умолчанию, и поэтому он должен быть заключен в разделители, например: MOVE TO **"** default **"** или MOVE TO **[** default **]** . Если указано значение **"** default **"** , то параметру QUOTED_IDENTIFIER для текущего сеанса должно быть присвоено значение ON. Это параметр по умолчанию. Дополнительные сведения см. в описании [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

{ CHECK | NOCHECK } CONSTRAINT  
Указывает, включено или отключено ограничение *constraint_name*. Данный параметр может использоваться только с ограничениями FOREIGN KEY и CHECK. Если указан параметр NOCHECK, то ограничение отключено и будущие вставки или обновления столбца не проверяются относительно условий ограничений. Отключить ограничения DEFAULT, PRIMARY KEY и UNIQUE нельзя.

ALL  
Указывает, что все ограничения отключаются при помощи параметра NOCHECK или включаются при помощи параметра CHECK.

{ ENABLE | DISABLE } TRIGGER  
Указывает, включено или отключено ограничение *trigger_name*. Отключенный триггер остается определенным для таблицы. Но действия триггера не будут выполняться при выполнении инструкций INSERT, UPDATE или DELETE для этой таблицы, пока триггер не будет снова включен.

ALL  
Указывает, что все триггеры в таблице включены или отключены.

*trigger_name*  
Указывает имя триггера, подлежащего включению или отключению.

{ ENABLE | DISABLE } CHANGE_TRACKING  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает, разрешено или запрещено отслеживание изменений для этой таблицы. По умолчанию отслеживание изменений запрещено.

Этот параметр доступен только в том случае, если отслеживание изменений разрешено для базы данных. Дополнительные сведения см. в описании [параметров ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md).

Чтобы разрешить отслеживание изменений, в таблице должен содержаться первичный ключ.

WITH **(** TRACK_COLUMNS_UPDATED **=** { ON | **OFF** } **)**  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает, отслеживает ли компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] обновления столбцов. Значение по умолчанию — OFF.

SWITCH [ PARTITION *source_partition_number_expression* ] TO [ _schema\_name_ **.** ] *target_table* [ PARTITION *target_partition_number_expression* ]  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Переключает блок данных одним из следующих способов.

- Переназначает все табличные данные как секцию в уже существующей секционированной таблице.
- Переключает секции из одной секционированной таблицы в другую.
- Переназначает все данные одной секции секционированной таблицы в уже существующую несекционированную таблицу.

Если таблица *table* секционирована, следует указать *source_partition_number_expression*. Если таблица *target_table* секционирована, следует указать *target_partition_number_expression*. Если происходит переназначение данных таблицы в секцию уже существующей секционированной таблицы или переключение секции из одной секционированной таблицы в другую, целевая секция должна существовать и быть пустой.

Если происходит переназначение данных из одной секции в новую отдельную таблицу, целевая таблица должна уже существовать и быть пустой. Как исходные, так и целевые таблицы или секции во всех случаях должны располагаться в одной файловой группе. Соответствующие индексы или секции индексов также должны располагаться в той же файловой группе. К переключаемым секциям применяются многие дополнительные ограничения. Значения *table* и *target_table* не могут совпадать. Значение *target_table* может быть идентификатором, состоящим из нескольких частей.

*source_partition_number_expression* и *target_partition_number_expression* являются константными выражениями, которые могут ссылаться на переменные и функции. В их число входят переменные определяемого пользователем типа и определяемые пользователем функции. Они не могут ссылаться на выражения [!INCLUDE[tsql](../../includes/tsql-md.md)].

Секционированная таблица с кластеризованным индексом columstore ведет себя как секционированная куча.

- Первичный ключ должен содержать ключ секции.
- Уникальный индекс должен содержать ключ секции. Но при этом включение ключа секции в существующий уникальный индекс может повлиять на его уникальность.
- Для переключения секций все некластеризованные индексы должны содержать ключ секции.

Сведения об ограничении **SWITCH** при использовании репликации см. в разделе [Репликация секционированных таблиц и индексов](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).

Некластеризованные индексы columnstore, созданные для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 CTP1 и для Базы данных SQL до версии 12, поддерживали формат только для чтения. Перед выполнением операций PARTITION необходимо перестроить некластеризованные индексы columnstore в текущий формат (с поддержкой обновления).

SET **(** FILESTREAM_ON = { *partition_scheme_name* \| *filestream_filegroup_name* \| **"** default **"** \| **"** NULL **"** } **)**  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не поддерживает `FILESTREAM`.

Указывает местоположения хранения данных FILESTREAM.

Инструкция ALTER TABLE с предложением SET FILESTREAM_ON будет выполнена успешно, только если в таблице отсутствуют столбцы FILESTREAM. Чтобы добавить столбцы FILESTREAM, можно создать вторую инструкцию ALTER TABLE.

Если указан аргумент *partition_scheme_name*, применяются правила для [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md). Таблица должна быть уже секционирована для строк данных, а схема секционирования должна использовать те же функции секционирования и столбцы, которые используются в схеме секционирования FILESTREAM.

Аргумент *filestream_filegroup_name* указывает имя файловой группы FILESTREAM. В файловой группе следует определить один файл для файловой группы с помощью инструкции [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) или [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md), иначе возникает ошибка.

**"** default **"** указывает файловую группу FILESTREAM с заданным свойством DEFAULT. При отсутствии файловой группы FILESTREAM возникает ошибка.

Значение **"** NULL **"** указывает на удаление всех ссылок на файловые группы FILESTREAM для таблицы. Сначала должны быть удалены все столбцы FILESTREAM. Используйте инструкцию SET FILESTREAM_ON = "**NULL**", чтобы удалить все данные FILESTREAM, связанные с таблицей.

SET **(** SYSTEM_VERSIONING **=** { OFF | ON [ ( HISTORY_TABLE = schema_name . history_table_name [ , DATA_CONSISTENCY_CHECK = { **ON** | OFF } ] ) ] } **)**  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Отключает или включает для таблицы системное управление версиями. Чтобы включить системное управление версиями в таблице, система проверяет соблюдение требований к типам данных, ограничениям допустимости значений NULL и ограничениям первичного ключа. Если аргумент HISTORY_TABLE не используется, система создает новую таблицу журнала по схеме текущей таблицы, а затем создает между ними связь и настраивает сохранение в таблице журнала истории каждой записи текущей таблицы. Таблица журнала будет называться `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Если вы укажете аргумент HISTORY_TABLE, чтобы создать связь с уже существующей таблицей журнала, система создает связь между текущей таблицей и указанной в этом аргументе таблицей. При создании связи с существующей таблицей журнала вы можете настроить проверку согласованности данных. Проверка согласованности данных гарантирует, что существующие записи не перекрываются. Выполнение проверки согласованности данных считается вариантом по умолчанию. Дополнительные сведения см. в разделе [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

HISTORY_RETENTION_PERIOD = { **INFINITE** \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS} }  
**Применимо к**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Определяет ограничение срока хранения данных журнала в темпоральной таблице или отсутствие такого ограничения. Если не указано, подразумевается неограниченный срок хранения.

SET (DATA_DELETION =  
      { OFF | ON  
                [(  [ FILTER_COLUMN = column_name ]   
                    [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS }}]   
                )] }   
**Применимо к:** *только* Azure SQL Edge

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

SET **(** LOCK_ESCALATION = { AUTO \| TABLE \| DISABLE } **)**  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает разрешенные методы укрупнения блокировки для таблицы.

AUTO  
Этот параметр позволяет [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] выбрать гранулярность укрупнения блокировки, подходящую для схемы конкретной таблицы.

- Если таблица является секционированной, то укрупнению блокировки будет разрешен доступ к куче или гранулярности сбалансированного дерева. Другими словами, укрупнение блокировки будет разрешено вплоть до уровня секции. После укрупнения блокировки до уровня HoBT блокировка не будет укрупнена позднее до гранулярности TABLE.
- Если таблица не секционирована, блокировка укрупняется до гранулярности TABLE.

TABLE  
Блокировка укрупняется до уровня гранулярности таблицы независимо от того, секционирована таблица или нет. Значение по умолчанию равно TABLE.

DISABLE  
В большинстве случаев предотвращает укрупнение блокировки. Блокировки уровня таблицы нельзя запретить полностью. Например, при сканировании таблицы, которая не имеет кластеризованного индекса на уровне изоляции SERIALIZABLE, [!INCLUDE[ssDE](../../includes/ssde-md.md)] всегда устанавливает блокировку таблицы для защиты целостности данных.

REBUILD  
Используйте синтаксис REBUILD WITH для перестроения всей таблицы, включая все секции в секционированную таблицу. Если в таблице содержится кластеризованный индекс, то параметр REBUILD перестраивает его. Операция REBUILD может выполняться в рамках операции ONLINE.

Используйте синтаксис REBUILD PARTITION для перестроения одной секции в секционированной таблице.

PARTITION = ALL  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Перестраивает все секции при изменении настройки сжатия секций.

REBUILD WITH ( \<rebuild_option> )  
Все параметры применяются к таблице с кластеризованным индексом. Если в таблице нет кластеризованного индекса, на структуру кучи влияют не все параметры.

Если для операции REBUILD не указан конкретный параметр сжатия, для секции используется текущий режим сжатия. Для возврата текущего параметра выполните запрос к столбцу **data_compression** из представления каталога **sys.partitions**.

Полное описание параметров перестроения см. в [описании index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md).

DATA_COMPRESSION  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Задает режим сжатия данных для указанной таблицы, номера секции или диапазона секций. Существуют следующие варианты выбора.

NONE — таблица или указанные секции не сжимаются. Этот вариант не применим к таблицам columnstore.

ROW — таблицы или указанные секции сжимаются, используется сжатие строк. Этот вариант не применим к таблицам columnstore.

PAGE — таблицы или указанные секции сжимаются, используется сжатие страниц. Этот вариант не применим к таблицам columnstore.

COLUMNSTORE  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Применяется только к таблицам columnstore. COLUMNSTORE указывает, что должна быть распакована секция, которая была упакована с помощью параметра COLUMNSTORE_ARCHIVE. При восстановлении данных сохраняется режим сжатия columnstore, который используется для всех таблиц columnstore.

COLUMNSTORE_ARCHIVE  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Применяется только к таблицам columnstore, представляющим собой таблицы, которые хранятся с кластеризованным индексом columnstore. Параметр COLUMNSTORE_ARCHIVE обеспечивает дальнейшее сжатие указанной секции до еще меньшего размера. Этот параметр можно использовать для архивации или в других ситуациях, когда требуется уменьшить объем пространства и допускается замедлять операции сохранения и извлечения.

Чтобы перестроить несколько секций одновременно, воспользуйтесь [описанием index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md). Если в таблице отсутствует кластеризованный индекс, при изменении сжатия данных перестраиваются некластеризованные индексы и куча. Дополнительные сведения о сжатии см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).

ONLINE **=** { ON | **OFF** } \<as applies to single_partition_rebuild_option>  
Определяет, доступна ли отдельная секция базовой таблицы и связанные индексы для запросов и изменения данных во время операций с индексами. Значение по умолчанию — OFF. Операцию REBUILD можно выполнять в оперативном режиме (ONLINE).

ON  
Долгосрочные блокировки таблицы не сохраняются на время операций с индексами. В начале перестройки индекса требуется S-блокировка таблицы, а в конце перестроения индекса в оперативном режиме — блокировка Sch-M. Обе они являются короткими блокировками метаданных, но блокировка изменения схемы (Sch-M) дополнительно ожидает завершения всех блокирующих транзакций. В этот период ожидания блокировка Sch-M блокирует все другие транзакции, которые получат доступ к этой таблице только после завершения блокировки.

> [!NOTE]
> Перестроение индекса в режиме "в сети" может задать параметры *low_priority_lock_wait*, описанные ниже в этом разделе.

OFF  
Блокировки таблиц применяются во время выполнения операций с индексами. Это предотвращает доступ к базовой таблице всех пользователей во время операции.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Имя набора столбцов. Набор столбцов представляет собой нетипизированное XML-представление, в котором все разреженные столбцы таблицы объединены в структурированные выходные данные. Набор столбцов не может быть добавлен в таблицу, если в ней содержатся разреженные столбцы. Дополнительные сведения о наборах столбцов см. в разделе [Использование наборов столбцов](../../relational-databases/tables/use-column-sets.md).

{ ENABLE | DISABLE } FILETABLE_NAMESPACE  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше).

Включает или выключает ограничения для таблицы FileTable, заданные системой. Может использоваться только для таблицы FileTable.

SET ( FILETABLE_DIRECTORY = *directory_name* )  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не поддерживает `FILETABLE`.

Указывает имя каталога таблицы FileTable, совместимое с Windows. Это имя должно быть уникальным среди всех имен каталогов FileTable в базе данных. Проверка уникальности не учитывает регистр символов независимо от параметров сортировки SQL. Может использоваться только для таблицы FileTable.

```syntaxsql
 SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options> )
          | = OFF_WITHOUT_DATA_RECOVERY
          ( MIGRATION_STATE = PAUSED ) | ( <table_stretch_options> [, ...n] )
        } )
```

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и выше).

Включает или отключает Stretch Database для таблицы. Дополнительные сведения см. в разделе [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Включение Stretch Database для таблицы**

Если вы включаете Stretch для таблицы, указывая `ON`, также нужно указать `MIGRATION_STATE = OUTBOUND`, чтобы сразу же приступить к миграции данных, или `MIGRATION_STATE = PAUSED`, чтобы отложить миграцию. Значение по умолчанию — `MIGRATION_STATE = OUTBOUND`. Более подробную информацию о включении Stretch для таблицы см. в статье [Включение Stretch Database для таблицы](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Предварительные требования**. Прежде чем включить Stretch для таблицы, необходимо включить Stretch на сервере и в базе данных. Дополнительные сведения см. в статье [Включение Stretch Database для базы данных](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Разрешения**. Чтобы включить Stretch для таблицы или базы данных, требуются права db_owner. Чтобы включить Stretch для таблицы, нужно иметь разрешения ALTER для таблицы.

**Отключение Stretch Database для таблицы**

При отключении Stretch для таблицы у вас есть два варианта управления удаленными данными, уже перенесенными в Azure. Дополнительные сведения см. в статье [Отключение Stretch Database и возврат удаленных данных](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

- Чтобы отключить Stretch для таблицы и скопировать удаленные данные для таблицы из Azure обратно в SQL Server, выполните следующую команду. Эту команду нельзя отменить.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    ```

Эта операция предусматривает расходы на передачу данных и не может быть отменена. Дополнительные сведения см. на странице [сведений о ценах на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/).

После копирования всех удаленных данных из Azure в SQL Server база данных Stretch для таблицы будет отключена.

- Чтобы отключить растяжение для таблицы и оставить удаленные данные в Azure, выполните следующую команду.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

После отключения Stretch Database для таблицы миграция данных останавливается и результаты запроса больше не включают результаты из удаленной таблицы.

Отключение Stretch Database не приводит к стиранию удаленной таблицы. Если вам нужно стереть удаленную таблицу, воспользуйтесь порталом Azure.

[ FILTER_PREDICATE = { null | *predicate* } ]  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и выше).

Дополнительно указывает предикат фильтра для выбора строк для миграции из таблицы, которая содержит данные журнала и текущие данные. Этот предикат должен вызывать детерминированную встроенную функцию с табличным значением. Дополнительные сведения см. в статьях [Включение Stretch Database для таблицы](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) и [Выбор строк для миграции с использованием функции фильтров (Stretch Database)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Если указать плохо оптимизированный предикат фильтра, перенос данных будет выполняться медленно. Stretch Database применяет предикат фильтра к таблице при помощи оператора CROSS APPLY.

Если предикат фильтра не указан, переносится вся таблица.

Если вы указываете предикат фильтра, необходимо также указать *MIGRATION_STATE*.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и выше).

- Укажите `OUTBOUND` для миграции данных с SQL Server на Azure.
- Укажите `INBOUND` для копирования удаленных данных для таблицы из Azure обратно в SQL Server с отключением Stretch для таблицы. Дополнительные сведения см. в статье [Отключение Stretch Database и возврат удаленных данных](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

    Эта операция предусматривает расходы на передачу данных и не может быть отменена.

- Укажите `PAUSED` для приостановки миграции данных. Дополнительные сведения см. в статье [Приостановка и возобновление переноса данных (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

WAIT_AT_LOW_PRIORITY  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Перестроение индекса в режиме «в сети» должно ожидать операции блокировки в этой таблице. **WAIT_AT_LOW_PRIORITY** указывает, что операция перестроения индекса в оперативном режиме будет ожидать блокировки с низким приоритетом. Другие операции могут выполняться, пока операция перестроения индекса в оперативном режиме находится в состоянии ожидания. Отсутствие параметра **WAIT AT LOW PRIORITY** эквивалентно варианту `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.

MAX_DURATION = *time* [**MINUTES** ]  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Время ожидания (целочисленное значение в минутах), в течение которого операция **SWITCH** или перестроение индекса в оперативном режиме поддерживают низкоприоритетную блокировку при выполнении команды DDL. Если операция будет заблокирована на время **MAX_DURATION**, будет выполнено одно из действий **ABORT_AFTER_WAIT**. Время **MAX_DURATION** всегда указывается в минутах, поэтому слово **MINUTES** можно опустить.

ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

None  
Продолжить ожидание блокировки с обычным приоритетом.

SELF  
Выход из операции **SWITCH** или операции DDL по перестроению индекса в оперативном режиме, которая выполняется в данный момент, без дополнительных действий.

BLOCKERS  
Остановка всех пользовательских транзакций, которые в данный момент блокируют операцию **SWITCH** или операцию DDL по перестроению индекса в оперативном режиме, чтобы эта операция могла продолжить работу.

Необходимо разрешение **ALTER ANY CONNECTION**.

IF EXISTS  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Условное удаление столбца или ограничения в том случае, если они существуют.

## <a name="remarks"></a>Remarks

Чтобы добавить новые строки данных, используйте [INSERT](../../t-sql/statements/insert-transact-sql.md). Чтобы удалить строки данных, используйте [DELETE](../../t-sql/statements/delete-transact-sql.md) или [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md). Чтобы изменить значения в существующих строках, используйте [UPDATE](../../t-sql/queries/update-transact-sql.md).

При наличии в кэше процедур каких-либо планов выполнения, ссылающихся на таблицу, инструкция ALTER TABLE помечает их как подлежащие перекомпиляции в их следующем выполнении.

## <a name="changing-the-size-of-a-column"></a>Изменение размера столбца

Длину, точность и масштаб столбца можно изменить, указав новый размер для типа данных столбца. Используйте для этого предложение ALTER COLUMN. Если в столбце содержатся данные, новый размер не может быть меньше максимального значения данных. Кроме того, вы не сможете определить столбец в индексе, если его тип данных не является **varchar**, **nvarchar** или **varbinary**, а сам индекс не является результатом ограничения PRIMARY KEY. См. пример в кратком разделе [Изменение определения столбца](#alter_column).

## <a name="locks-and-alter-table"></a>Блокировки и инструкция ALTER TABLE

Указанные в инструкции ALTER TABLE изменения применяются немедленно. Если для изменений требуется модификация строк таблицы, то инструкция ALTER TABLE обновляет эти строки. Инструкция ALTER TABLE получает для таблицы блокировку модификации схемы (SCH-M), чтобы в процессе изменения другие подключения не использовали даже метаданные этой таблицы, за исключением операций с индексами в оперативном режиме, в конце которых требуется короткая блокировка SCH-M. В операции `ALTER TABLE...SWITCH` запрашивается блокировка и исходной, и целевой таблиц. Изменения, сделанные в таблице, регистрируются в журнале и полностью обратимы. Изменения, затрагивающие все строки в больших таблицах, например удаление столбца или (в некоторых выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) добавление столбца со свойством NOT NULL и значением по умолчанию, могут занимать длительное время и создавать много записей в журнале. Выполняйте такие инструкции ALTER TABLE с осторожностью, как и любые инструкции INSERT, UPDATE или DELETE, влияющие на большое количество строк.

### <a name="adding-not-null-columns-as-an-online-operation"></a>Добавление столбцов NOT NULL в качестве операции в сети

Начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise Edition, добавление столбца NOT NULL со значением по умолчанию является операцией в сети, если значение по умолчанию является *константой времени выполнения*. Это значит, что операция выполняется почти мгновенно, независимо от количества строк в таблице. Дело в том, что существующие в таблице строки при такой операции не обновляются. Вместо этого значение по умолчанию сохраняется в метаданных таблицы и применяется по мере необходимости в запросах, обращающихся к этим строкам. Такое поведение реализуется автоматически. Вам не нужно использовать дополнительные предложения, кроме базового синтаксиса операции ADD COLUMN. Константой времени выполнения считается любое выражение, которое сохраняет во время выполнения одинаковое значение для каждой строки в таблице, независимо от ее детерминизма. Например, выражение константы «Временные данные» и системная функция GETUTCDATETIME() являются константами времени выполнения. Функции `NEWID()` и `NEWSEQUENTIALID()`, напротив, не являются константами времени выполнения, так как для каждой строки в таблице создается уникальное значение. Добавление столбца NOT NULL со значением по умолчанию, которое не является константой времени выполнения, всегда выполняется с прекращением работы и монопольной блокировкой (SCH-M) на весь период ее выполнения.

Для существующих строк используется ссылка на значение, хранящееся в метаданных, но это же значение сохраняется напрямую в каждой новой строке, которая вставляется без указания значения для этого столбца. Значение по умолчанию, хранящееся в метаданных, будет перемещено в существующую строку при ее обновлении (даже если в инструкции UPDATE не указан этот столбец), а также при перестроении таблицы или кластеризованного индекса.

Столбцы типов **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml**, **text**, **ntext**, **image**, **hierarchyid**, **geometry**, **geography** или с пользовательским типом среды CLR нельзя добавлять в оперативном режиме. Столбец нельзя добавить в оперативном режиме, если после этой операции максимально возможный размер строки превысит ограничение в 8060 байт. В этом случае столбец добавляется в рамках операции вне сети.

## <a name="parallel-plan-execution"></a>Выполнение параллельного плана

В версии [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] и выше количество процессоров, задействованных в выполнении одной инструкции ALTER TABLE ADD (на базе индекса) CONSTRAINT или DROP (кластеризованный индекс) CONSTRAINT, определяется по параметру конфигурации **max degree of parallelism** и характеристикам текущей рабочей нагрузки. Если компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, что система занята, то перед началом выполнения инструкции степень параллелизма операции автоматически понижается. Можно вручную настроить число процессоров, применяемых для запуска инструкции, указав параметр MAXDOP. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

## <a name="partitioned-tables"></a>Секционированные таблицы

Кроме выполнения операций SWITCH, затрагивающих секционированные таблицы, инструкция ALTER TABLE позволяет изменять состояния столбцов, ограничений и триггеров секционированной таблицы точно так же, как и для несекционированных таблиц. Но эта инструкция не может изменить способ секционирования таблицы. Чтобы заново секционировать секционированную таблицу, используйте [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) и [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md). Кроме того, невозможно изменить тип данных для столбца секционированной таблицы.

## <a name="restrictions-on-tables-with-schema-bound-views"></a>Ограничения в таблицах с представлениями, привязанными к схемам

К инструкциям ALTER TABLE в таблицах с представлениями, привязанными к схемам, применяются те же ограничения, которые применяются в текущем времени для изменения таблиц с простым индексом. Добавление столбца разрешено. Но не разрешается удаление или изменение столбца, участвующего в любом из представлений, привязанных к схемам. Если инструкция ALTER TABLE требует изменения столбца, используемого в привязанном к схеме представлении, то происходит сбой инструкции ALTER TABLE и компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдает сообщение об ошибке. Дополнительные сведения о привязке к схеме и индексированных представлениях см. в описании [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md).

Создание или удаление триггеров для базовых таблиц не зависит от создания привязанного к схеме представления, в котором указаны эти таблицы.

## <a name="indexes-and-alter-table"></a>Индексы и инструкция ALTER TABLE

При удалении ограничений индексы, создаваемые как часть ограничения, удаляются. Индексы, создаваемые при помощи инструкции CREATE INDEX, должны удаляться при помощи инструкции DROP INDEX. Используйте инструкцию ALTER INDEX, чтобы перестроить индексную часть определения ограничения. Не обязательно удалять и заново добавлять ограничение с помощью инструкции ALTER TABLE.

Перед удалением столбца необходимо удалить все индексы и ограничения, основанные на столбце.

После удаления ограничения, создавшего кластеризованный индекс, строки данных, хранившиеся на конечном уровне кластеризованного индекса, хранятся в некластеризованной таблице. Можно удалить кластеризованный индекс и переместить полученную в результате таблицу в другую файловую группу или схему секционирования в одной транзакции, указав параметр MOVE TO. Параметр MOVE TO обладает следующими ограничениями.

- Параметр MOVE TO недопустим для индексированных представлений и некластеризованных индексов.
- Схема секционирования или файловая группа уже должна существовать.
- Если не указан аргумент MOVE TO, таблица будет размещена в той же схеме секционирования или файловой группе, которая была определена для кластеризованного индекса.

При удалении кластеризованного индекса укажите параметр ONLINE **=** ON, чтобы транзакция DROP INDEX не блокировала запросы и изменения в отношении базовых данных и связанных с ними некластеризованных индексов.

Параметр ONLINE **=** ON имеет следующие ограничения.

- Параметр ONLINE **=** ON неприменим для кластеризованных индексов, которые отключены. Отключенные индексы должны удаляться при помощи параметра ONLINE **=** OFF.
- Только один индекс может удаляться единовременно.
- Параметр ONLINE **=** ON неприменим для индексированных представлений, некластеризованных индексов или индексов в локальных временных таблицах.
- Параметр ONLINE **=** ON неприменим для индексов columnstore.

Для удаления кластеризованного индекса временно требуется место на диске, равное размеру существующего кластеризованного индекса. Это дополнительное пространство освобождается сразу после завершения операции.

> [!NOTE]
> Параметры, перечисленные в *\<drop_clustered_constraint_option>* , применяются к кластеризованным индексам в таблицах и не могут применяться к кластеризованным индексам в представлениях или к некластеризованным индексам.

## <a name="replicating-schema-changes"></a>Репликация изменений схемы

Когда команда ALTER TABLE выполняется для таблицы, опубликованной в издателе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], все изменения по умолчанию распространяются для всех подписчиков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Эта функция имеет ряд ограничений. Вы можете отключить ее. Дополнительные сведения см. в статье [Внесение изменений в схемы баз данных публикации](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).

## <a name="data-compression"></a>Сжатие данных

В системных таблицах не может быть включено сжатие. Если таблица является кучей, то операция перестроения в режиме ONLINE будет однопотоковой. Используйте режим OFFLINE для выполнения многопотоковых операций перестроения кучи. Дополнительную информацию о сжатии данных см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).

Оценить состояние сжатия таблицы, индекса или секции можно с помощью хранимой процедуры [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).

На секционированные таблицы налагаются следующие ограничения.

- Если в таблице есть невыровненные индексы, настройку сжатия для отдельной секции изменить нельзя.
- Использование синтаксиса ALTER TABLE \<table> REBUILD PARTITION ... приводит к перестроению указанной секции.
- Использование синтаксиса ALTER TABLE \<table> REBUILD WITH ... приводит к перестроению всех секций.

## <a name="dropping-ntext-columns"></a>Удаление столбцов NTEXT

При удалении столбцов NTEXT очистка удаленных данных выполняется как сериализованная операция для всех строк. Очистка может занимать много времени. Перед удалением столбца NTEXT из таблицы с большим количеством строк сначала заполните столбец NTEXT значениями NULL. Это действие можно выполнять в параллельном режиме, чтобы значительно ускорить его.

## <a name="online-index-rebuild"></a>Перестроение индексов в режиме «в сети».

Чтобы выполнить инструкцию DDL для перестроения индекса в оперативном режиме, необходимо завершить все активные блокирующие транзакции, выполняемые для соответствующей таблицы. Запуск перестроения индекса в оперативном режиме блокирует все новые транзакции, которые готовы к выполнению для этой таблицы. Хотя продолжительность блокировки для перестроения индекса в оперативном режиме очень невелика, ожидание завершения всех открытых транзакций по таблице и блокировка новых транзакций может заметно отразиться на пропускной способности. Это может замедлить выполнение рабочей нагрузки или привести к превышению времени ожидания, существенно ограничивая доступ к базовой таблице. Параметр **WAIT_AT_LOW_PRIORITY** позволяет администратору базы данных управлять блокировками S и Sch-M, которые используются для перестроения индекса в оперативном режиме. Доступны три варианта. Во всех трех вариантах, если во время ожидания (`(MAX_DURATION =n [minutes])`) нет блокирующих действий, то перестроение индекса в оперативном режиме начинается немедленно и не ожидает завершения инструкции DDL.

## <a name="compatibility-support"></a>Поддержка совместимости

Инструкция ALTER TABLE позволяет использовать только имена таблиц, состоящие из двух частей — "схема.объект". В [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] при задании имени таблицы в приведенных далее форматах во время компиляции возникает ошибка 117.

- «сервер.база_данных.схема.таблица»
- «.база_данных.схема.таблица»
- «..схема.таблица»

В предыдущих версиях определение формата "сервер.база_данных.схема.таблица" приводило к ошибке 4902. Формат «.база_данных.схема.таблица» или «..схема.таблица» обрабатывался успешно.

Чтобы устранить эту проблему, откажитесь от использования четырехкомпонентного префикса.

## <a name="permissions"></a>Разрешения

Требуется разрешение ALTER на таблицу.

Разрешения ALTER TABLE применяются к обеим таблицам, затронутым инструкцией ALTER TABLE SWITCH. Любые переключенные данные наследуют защиту от целевой таблицы.

Если вы определили какой-либо из столбцов в инструкции ALTER TABLE с пользовательским типом для среды CLR или с псевдонимом типа данных, вам потребуется разрешение REFERENCES для этого типа.

Для добавления или изменения столбца, который обновляет строки таблицы, требуется разрешение **UPDATE** для этой таблицы. Например, добавление столбца **NOT NULL** со значением по умолчанию или добавление столбца идентификаторов, если таблица не пуста.

## <a name="examples"></a><a name="Example_Top"></a> Примеры

|Категория|Используемые элементы синтаксиса|
|--------------|------------------------------|
|[Добавление столбцов и ограничений](#add)|ADD * PRIMARY KEY с параметрами индекса * разреженные столбцы и наборы столбцов *|
|[Удаление столбцов и ограничений](#Drop)|DROP|
|[Изменение определения столбца](#alter_column)|изменение типа данных * изменение размера столбца * параметры сортировки|
|[Изменение определения таблицы](#alter_table)|DATA_COMPRESSION * SWITCH PARTITION * LOCK ESCALATION * отслеживание изменений|
|[Отключение и включение ограничений и триггеров](#disable_enable)|CHECK * NO CHECK * ENABLE TRIGGER * DISABLE TRIGGER|
| &nbsp; | &nbsp; |

### <a name="adding-columns-and-constraints"></a><a name="add"></a>Добавление столбцов и ограничений

В примерах из этого раздела показано добавление в таблицу столбцов и ограничений.

#### <a name="a-adding-a-new-column"></a>A. Добавление нового столбца

Следующий пример показывает добавление столбца, который допускает значения NULL и не имеет значений, предоставленных через определение DEFAULT. В новом столбце в каждой строке будет значение `NULL`.

```sql
CREATE TABLE dbo.doc_exa (column_a INT) ;
GO
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL ;
GO
```

#### <a name="b-adding-a-column-with-a-constraint"></a>Б. Добавление столбца с ограничением

В следующем примере показано добавление нового столбца с ограничением `UNIQUE`.

```sql
CREATE TABLE dbo.doc_exc (column_a INT) ;
GO
ALTER TABLE dbo.doc_exc ADD column_b VARCHAR(20) NULL
    CONSTRAINT exb_unique UNIQUE ;
GO
EXEC sp_help doc_exc ;
GO
DROP TABLE dbo.doc_exc ;
GO
```

#### <a name="c-adding-an-unverified-check-constraint-to-an-existing-column"></a>В. Добавление непроверяемого ограничения CHECK к существующему столбцу

В следующем примере к существующему столбцу в таблице добавляется ограничение. Столбец имеет значение, нарушающее это ограничение. Поэтому во избежание проверки ограничения относительно существующих строк, а также для того, чтобы разрешить добавление ограничения, применяется `WITH NOCHECK`.

```sql
CREATE TABLE dbo.doc_exd (column_a INT) ;
GO
INSERT INTO dbo.doc_exd VALUES (-1) ;
GO
ALTER TABLE dbo.doc_exd WITH NOCHECK
ADD CONSTRAINT exd_check CHECK (column_a > 1) ;
GO
EXEC sp_help doc_exd ;
GO
DROP TABLE dbo.doc_exd ;
GO
```

#### <a name="d-adding-a-default-constraint-to-an-existing-column"></a>Г. Добавление ограничения DEFAULT к существующему столбцу

Следующий пример показывает создание таблицы с двумя столбцами и заполнение значениями первого столбца; в другом столбце остается NULL. В таком случае во второй столбец добавляется ограничение `DEFAULT`. Чтобы проверить применение значения по умолчанию, в первый столбец вставляется другое значение и создается запрос к таблице.

```sql
CREATE TABLE dbo.doc_exz (column_a INT, column_b INT) ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (7) ;
GO
ALTER TABLE dbo.doc_exz
  ADD CONSTRAINT col_b_def
  DEFAULT 50 FOR column_b ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (10) ;
GO
SELECT * FROM dbo.doc_exz ;
GO
DROP TABLE dbo.doc_exz ;
GO
```

#### <a name="e-adding-several-columns-with-constraints"></a>Д. Добавление нескольких столбцов с ограничениями

Следующий пример показывает добавление нескольких столбцов с ограничениями, которые определяются с помощью нового столбца. Первый новый столбец имеет свойство `IDENTITY`. Каждая строка таблицы имеет новые добавочные значения в столбце идентификаторов.

```sql
CREATE TABLE dbo.doc_exe (column_a INT CONSTRAINT column_a_un UNIQUE) ;
GO
ALTER TABLE dbo.doc_exe ADD

-- Add a PRIMARY KEY identity column.
column_b INT IDENTITY
CONSTRAINT column_b_pk PRIMARY KEY,

-- Add a column that references another column in the same table.
column_c INT NULL
CONSTRAINT column_c_fk
REFERENCES doc_exe(column_a),

-- Add a column with a constraint to enforce that
-- nonnull data is in a valid telephone number format.
column_d VARCHAR(16) NULL
CONSTRAINT column_d_chk
CHECK
(column_d LIKE '[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]' OR
column_d LIKE
'([0-9][0-9][0-9]) [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'),

-- Add a nonnull column with a default.
column_e DECIMAL(3,3)
CONSTRAINT column_e_default
DEFAULT .081 ;
GO
EXEC sp_help doc_exe ;
GO
DROP TABLE dbo.doc_exe ;
GO
```

#### <a name="f-adding-a-nullable-column-with-default-values"></a>Е. Добавление столбца, допускающего значения NULL, со значениями по умолчанию

Следующий пример показывает добавление столбца, допускающего значения NULL, с определением `DEFAULT` и использование `WITH VALUES` для предоставления значений каждой строке таблицы. Если аргумент WITH VALUES не используется, каждая строка имеет значение NULL в новом столбце.

```sql
CREATE TABLE dbo.doc_exf (column_a INT) ;
GO
INSERT INTO dbo.doc_exf VALUES (1) ;
GO
ALTER TABLE dbo.doc_exf
ADD AddDate smalldatetime NULL
CONSTRAINT AddDateDflt
DEFAULT GETDATE() WITH VALUES ;
GO
DROP TABLE dbo.doc_exf ;
GO
```

#### <a name="g-creating-a-primary-key-constraint-with-index-or-data-compression-options"></a>Ж. Создание ограничения PRIMARY KEY с параметрами индекса или сжатия данных

Следующий пример показывает создание ограничения PRIMARY KEY `PK_TransactionHistoryArchive_TransactionID` и установку параметров `FILLFACTOR`, `ONLINE` и `PAD_INDEX`. Результирующий кластеризованный индекс будет иметь такое же имя, что и ограничение.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (FILLFACTOR = 75, ONLINE = ON, PAD_INDEX = ON);
GO
```

В этом аналогичном примере применяется сжатие страниц и кластеризованный первичный ключ.

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (DATA_COMPRESSION = PAGE);
GO
```

#### <a name="h-adding-a-sparse-column"></a>З. Добавление разреженного столбца

В следующих примерах показывается добавление и изменение разреженных столбцов в таблице T1. Код для создания таблицы `T1`:

```sql
CREATE TABLE T1 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) SPARSE NULL,
  C3 INT SPARSE NULL,
  C4 INT) ;
GO
```

Чтобы добавить дополнительный разреженный столбец `C5`, выполните следующую инструкцию.

```sql
ALTER TABLE T1
ADD C5 CHAR(100) SPARSE NULL ;
GO
```

Чтобы преобразовать неразреженный столбец `C4` в разреженный, выполните следующую инструкцию.

```sql
ALTER TABLE T1
ALTER COLUMN C4 ADD SPARSE ;
GO
```

Чтобы преобразовать разреженный столбец `C4`в неразреженный, выполните следующую инструкцию.

```sql
ALTER TABLE T1
ALTER COLUMN C4 DROP SPARSE ;
GO
```

#### <a name="i-adding-a-column-set"></a>И. Добавление набора столбцов

В следующих примерах показано добавление столбца к таблице `T2`. Набор столбцов не может быть добавлен в таблицу, если в ней уже содержатся разреженные столбцы. Код для создания таблицы `T2`:

```sql
CREATE TABLE T2 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

В следующих трех инструкциях добавляется набор столбцов с именем `CS`, после чего изменяются столбцы `C2` и `C3` на `SPARSE`.

```sql
ALTER TABLE T2
ADD CS XML COLUMN_SET FOR ALL_SPARSE_COLUMNS ;
GO

ALTER TABLE T2
ALTER COLUMN C2 ADD SPARSE ;
GO

ALTER TABLE T2
ALTER COLUMN C3 ADD SPARSE ;
GO
```

#### <a name="j-adding-an-encrypted-column"></a>К. Добавление зашифрованного столбца

Следующая инструкция добавляет зашифрованный столбец с именем `PromotionCode`.

```sql
ALTER TABLE Customers ADD
    PromotionCode nvarchar(100)
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,
    ENCRYPTION_TYPE = RANDOMIZED,
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') ;
```

### <a name="dropping-columns-and-constraints"></a><a name="Drop"></a>Удаление столбцов и ограничений

Приведенные в этом разделе примеры демонстрируют удаление столбцов и ограничений.

#### <a name="a-dropping-a-column-or-columns"></a>A. Удаление столбца или столбцов

В первом примере для удаления столбца изменяется таблица. Во втором примере удаляется несколько столбцов.

```sql
CREATE TABLE dbo.doc_exb (
     column_a INT,
     column_b VARCHAR(20) NULL,
     column_c DATETIME,
     column_d INT) ;
GO  
-- Remove a single column.
ALTER TABLE dbo.doc_exb DROP COLUMN column_b ;
GO
-- Remove multiple columns.
ALTER TABLE dbo.doc_exb DROP COLUMN column_c, column_d;
```

#### <a name="b-dropping-constraints-and-columns"></a>Б. Удаление ограничений и столбцов

В первом примере из таблицы удаляется ограничение `UNIQUE`. Во втором примере удаляется 2 ограничения и один столбец.

```sql
CREATE TABLE dbo.doc_exc (column_a INT NOT NULL CONSTRAINT my_constraint UNIQUE) ;
GO

-- Example 1. Remove a single constraint.
ALTER TABLE dbo.doc_exc DROP my_constraint ;
GO

DROP TABLE dbo.doc_exc;
GO

CREATE TABLE dbo.doc_exc ( column_a INT
                          NOT NULL CONSTRAINT my_constraint UNIQUE
                          ,column_b INT
                          NOT NULL CONSTRAINT my_pk_constraint PRIMARY KEY) ;
GO

-- Example 2. Remove two constraints and one column
-- The keyword CONSTRAINT is optional. The keyword COLUMN is required.
ALTER TABLE dbo.doc_exc
DROP CONSTRAINT my_constraint, my_pk_constraint, COLUMN column_b ;
GO
```

#### <a name="c-dropping-a-primary-key-constraint-in-the-online-mode"></a>В. Удаление ограничения PRIMARY KEY в режиме ONLINE

В следующем примере удаляется ограничение PRIMARY KEY с параметром `ONLINE`, имеющим значение `ON`.

```sql
ALTER TABLE Production.TransactionHistoryArchive
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID
WITH (ONLINE = ON) ;
GO
```

#### <a name="d-adding-and-dropping-a-foreign-key-constraint"></a>Г. Добавление и удаление ограничения FOREIGN KEY

Следующий пример показывает создание таблицы `ContactBackup`, а затем ее изменение сначала добавлением ограничения `FOREIGN KEY`, ссылающегося на таблицу `Person.Person`, затем удалением ограничения `FOREIGN KEY`.

```sql
CREATE TABLE Person.ContactBackup
    (ContactID INT) ;
GO

ALTER TABLE Person.ContactBackup
ADD CONSTRAINT FK_ContactBackup_Contact FOREIGN KEY (ContactID)
    REFERENCES Person.Person (BusinessEntityID) ;
GO

ALTER TABLE Person.ContactBackup
DROP CONSTRAINT FK_ContactBackup_Contact ;
GO

DROP TABLE Person.ContactBackup ;
```

![Значок стрелки, используемый со ссылкой В начало](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Значок стрелки, используемый со ссылкой В начало") [Примеры](#Example_Top)

### <a name="altering-a-column-definition"></a><a name="alter_column"></a> Изменение определения столбца

#### <a name="a-changing-the-data-type-of-a-column"></a>A. изменение типа данных столбца.

В следующем примере столбец таблицы изменяется с `INT` на `DECIMAL`.

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;
GO
DROP TABLE dbo.doc_exy ;
GO
```

#### <a name="b-changing-the-size-of-a-column"></a>Б. Изменение размера столбца

В следующем примере выполняется увеличение размера столбца **varchar**, а также точности и масштаба столбца **decimal**. Поскольку столбцы содержат данные, их размер можно только увеличить. Также обратите внимание, что столбец `col_a` определяется в уникальном индексе. Размер столбца `col_a` можно дополнительно увеличить, так как он имеет тип данных **varchar**, а индекс не является результатом ограничения PRIMARY KEY.

```sql
-- Create a two-column table with a unique index on the varchar column.
CREATE TABLE dbo.doc_exy (col_a varchar(5) UNIQUE NOT NULL, col_b decimal (4,2)) ;
GO
INSERT INTO dbo.doc_exy VALUES ('Test', 99.99) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
GO
-- Increase the size of the varchar column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_a varchar(25) ;
GO
-- Increase the scale and precision of the decimal column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_b decimal (10,4) ;
GO
-- Insert a new row.
INSERT INTO dbo.doc_exy VALUES ('MyNewColumnSize', 99999.9999) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
```

#### <a name="c-changing-column-collation"></a>В. Изменение параметров сортировки столбца

В следующем примере демонстрируется изменение параметров сортировки столбца. Сначала создается таблица с параметрами сортировки пользователей по умолчанию.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Затем параметры сортировки столбца `C2` изменяются на Latin1_General_BIN. Тип данных обязательно нужно указать, хотя он не изменяется.

```sql
ALTER TABLE T3
ALTER COLUMN C2 varchar(50) COLLATE Latin1_General_BIN ;
GO
```

#### <a name="d-encrypting-a-column"></a>Г. Шифрование столбца

В следующем примере показано, как зашифровать столбец с помощью [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

Первым делом создается таблица без зашифрованных столбцов.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

После этого столбец C2 шифруется с помощью ключа шифрования CEK1 методом случайного шифрования. Чтобы следующая инструкция была выполнена успешно, должны соблюдаться следующие условия:

- Ключ шифрования столбцов должен поддерживать анклав. Это означает, что он должен быть зашифрован главным ключом столбца, который допускает анклавные вычисления.
- Целевой экземпляр SQL Server должен поддерживать Always Encrypted с безопасными анклавами.
- Оператор следует передавать через подключение, настроенное для Always Encrypted с безопасными анклавами и применяемое поддерживаемый драйвер клиента.
- Вызывающему приложению нужен доступ к главному ключу столбца, который защищает ключ CEK1.

```sql
ALTER TABLE T3
ALTER COLUMN C2 VARCHAR(50) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL;
GO
```

### <a name="altering-a-table-definition"></a><a name="alter_table"></a> Изменение определения таблицы

В приведенных в этом разделе примерах показано, как изменить определение таблицы.

#### <a name="a-modifying-a-table-to-change-the-compression"></a>A. Изменение таблицы для изменения режима сжатия

В следующем примере изменяется режим сжатия несекционированной таблицы. Куча или кластеризованный индекс будет перестроен. Если таблица является кучей, то все некластеризованные индексы будут перестроены.

```sql
ALTER TABLE T1
REBUILD WITH (DATA_COMPRESSION = PAGE) ;
```

В следующем примере изменяется режим сжатия секционированной таблицы. Инструкция `REBUILD PARTITION = 1` вызывает перестройку только секции с номером `1`.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =NONE) ;
GO
```

Та же операция, использующая следующий альтернативный синтаксис, вызывает повторное построение всех секций в таблице.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = ALL
WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(1)) ;
```

Дополнительные примеры сжатия данных см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).

#### <a name="b-modifying-a-columnstore-table-to-change-archival-compression"></a>Б. Модификация таблицы columnstore для изменения архивного сжатия

Следующий пример показывает, как дополнительно сжать секцию таблицы columnstore, применяя дополнительный алгоритм сжатия. Такое сжатие уменьшает размер таблицы, но при этом увеличивает время, требуемое для хранения и получения данных. Это может использоваться для архивации или в тех ситуациях, где требуется уменьшение объема пространства и допускается увеличение затрат времени на сохранение и выборку.

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE) ;
GO
```

В следующем примере показана распаковка секции таблицы columnstore, которая была упакована с параметром COLUMNSTORE_ARCHIVE. При восстановлении данных сохранится режим сжатия columnstore, который используется для всех таблиц columnstore.

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE) ;
GO
```

#### <a name="c-switching-partitions-between-tables"></a>В. Переключение секций между таблицами

В следующем примере демонстрируется создание секционированной таблицы, исходя из предположения, что схема секционирования `myRangePS1` уже создана в базе данных. Затем создается несекционированная таблица с такой же структурой, что и секционированная таблица, и в той же файловой группе, что и `PARTITION 2` таблицы `PartitionTable`. В таком случае данные `PARTITION 2` таблицы `PartitionTable` переключаются в таблицу `NonPartitionTable`.

```sql
CREATE TABLE PartitionTable (col1 INT, col2 CHAR(10))
ON myRangePS1 (col1) ;
GO
CREATE TABLE NonPartitionTable (col1 INT, col2 CHAR(10))
ON test2fg ;
GO
ALTER TABLE PartitionTable SWITCH PARTITION 2 TO NonPartitionTable ;
GO
```

#### <a name="d-allowing-lock-escalation-on-partitioned-tables"></a>Г. Разрешение укрупнения блокировки для секционированных таблиц

В следующем примере укрупнение блокировки разрешается на уровне секции в секционированной таблице. Если таблица не секционирована, блокировка будет укрупняться до уровня TABLE.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE dbo.T1 SET (LOCK_ESCALATION = AUTO) ;
GO
```

#### <a name="e-configuring-change-tracking-on-a-table"></a>Д. Настройка отслеживания изменений для таблицы

В следующем примере в таблице `Person.Person` включается отслеживание изменений.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING ;
```

В следующем примере разрешается отслеживание изменений и отслеживание столбцов, которые обновляются при внесении изменений.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING
WITH (TRACK_COLUMNS_UPDATED = ON)
```

В следующем примере в таблице `Person.Person` отключается отслеживание изменений.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
DISABLE CHANGE_TRACKING ;
```

### <a name="disabling-and-enabling-constraints-and-triggers"></a><a name="disable_enable"></a> Отключение и включение ограничений и триггеров

#### <a name="a-disabling-and-re-enabling-a-constraint"></a>A. Отключение и повторное включение ограничения

В следующем примере отключается ограничение на зарплату. Параметр `NOCHECK CONSTRAINT` используется в инструкции `ALTER TABLE` для отключения ограничения и обеспечения возможности вставки, противоречащей указанному ограничению. `CHECK CONSTRAINT` повторно включает ограничение.

```sql
CREATE TABLE dbo.cnst_example (
  id INT NOT NULL,
  name VARCHAR(10) NOT NULL,
  salary MONEY NOT NULL
  CONSTRAINT salary_cap CHECK (salary < 100000)) ;

-- Valid inserts
INSERT INTO dbo.cnst_example VALUES (1,'Joe Brown',65000) ;
INSERT INTO dbo.cnst_example VALUES (2,'Mary Smith',75000) ;

-- This insert violates the constraint.
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Disable the constraint and try again.
ALTER TABLE dbo.cnst_example NOCHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Re-enable the constraint and try another insert; this will fail.
ALTER TABLE dbo.cnst_example CHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (4,'Eric James',110000) ;
```

#### <a name="b-disabling-and-re-enabling-a-trigger"></a>Б. Отключение и повторное включение триггера

В следующем примере показывается использование параметра `DISABLE TRIGGER` инструкции `ALTER TABLE` для отключения триггера и обеспечения возможности вставки, которая в обычных условиях нарушает триггер. Затем инструкция `ENABLE TRIGGER` используется для повторного включения триггера.

```sql
CREATE TABLE dbo.trig_example (
  id INT,
  name VARCHAR(12),
  salary MONEY) ;
GO
-- Create the trigger.
CREATE TRIGGER dbo.trig1 ON dbo.trig_example FOR INSERT
AS
IF (SELECT COUNT(*) FROM INSERTED
WHERE salary > 100000) > 0
BEGIN
    print 'TRIG1 Error: you attempted to insert a salary > $100,000'
    ROLLBACK TRANSACTION
END ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (1,'Pat Smith',100001) ;
GO
-- Disable the trigger.
ALTER TABLE dbo.trig_example DISABLE TRIGGER trig1 ;
GO
-- Try an insert that would typically violate the trigger.
INSERT INTO dbo.trig_example VALUES (2,'Chuck Jones',100001) ;
GO
-- Re-enable the trigger.
ALTER TABLE dbo.trig_example ENABLE TRIGGER trig1 ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (3,'Mary Booth',100001) ;
GO
```

### <a name="online-operations"></a><a name="online"></a> Операции в сети

#### <a name="a-online-index-rebuild-using-low-priority-wait-options"></a>A. Перестроение индекса в оперативном режиме с низким приоритетом ожидания.

В следующем примере показано, как выполнить перестроение индекса в оперативном режиме с низким приоритетом ожидания.

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE T1
REBUILD WITH
(
    PAD_INDEX = ON,
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES,
                                         ABORT_AFTER_WAIT = BLOCKERS ) )
) ;
```

#### <a name="b-online-alter-column"></a>Б. Изменение столбца в режиме «в сети»

В следующем примере показано, как выполнить операцию изменения столбца с параметром ONLINE.

**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy
    ALTER COLUMN column_a DECIMAL (5, 2) WITH (ONLINE = ON) ;
GO
sp_help doc_exy;
DROP TABLE dbo.doc_exy ;
GO
```

### <a name="system-versioning"></a><a name="system_versioning"></a> Системное управление версиями

Следующие четыре примера помогут ознакомиться с синтаксисом использования системного управления версиями. Дополнительные сведения см. в разделе [Приступая к работе c темпоральными таблицами с системным управлением версиями](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md).

**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

#### <a name="a-add-system-versioning-to-existing-tables"></a>A. Добавление системного управления версиями в существующие таблицы

В следующем примере показано добавление системного управления версиями в существующую таблицу и создание будущей таблицы журнала. В этом примере допускается существование таблицы `InsurancePolicy`, для которой определен первичный ключ. Этот пример заполняет только что созданные столбцы периода для системного управления версиями значениями по умолчанию для времени начала и окончания, так как эти значения не могут быть NULL. В этом примере используется предложение HIDDEN, чтобы не затрагивать существующие приложения, взаимодействующие с текущей таблицей. В нем также используется HISTORY_RETENTION_PERIOD, доступный только в [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

```sql
--Alter non-temporal table to define periods for system versioning
ALTER TABLE InsurancePolicy
ADD PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime),
SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
    DEFAULT SYSUTCDATETIME(),
SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
    DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.99999999') ;

--Enable system versioning with 1 year retention for historical data
ALTER TABLE InsurancePolicy
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 1 YEAR)) ;
```

#### <a name="b-migrate-an-existing-solution-to-use-system-versioning"></a>Б. Перенос существующего решения для использования системного управления версиями

В следующем примере показан переход на системное управление версиями из решения, использующего триггеры для имитации временной поддержкой. В примере предполагается, что есть решение, использующее таблицу `ProjectTask`, и таблица `ProjectTaskHistory` для существующего решения, в которой для периодов используются столбцы `Changed Date` и `Revised Date`, и эти столбцы не используют тип данных `datetime2`, а в таблице `ProjectTask` определен первичный ключ.

```sql
-- Drop existing trigger
DROP TRIGGER ProjectTask_HistoryTrigger;

-- Adjust the schema for current and history table
-- Change data types for existing period columns
ALTER TABLE ProjectTask ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTask ALTER COLUMN [Revised Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Revised Date] datetime2 NOT NULL ;

-- Add SYSTEM_TIME period and set system versioning with linking two existing tables
-- (a certain set of data checks happen in the background)
ALTER TABLE ProjectTask
ADD PERIOD FOR SYSTEM_TIME ([Changed Date], [Revised Date])

ALTER TABLE ProjectTask
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON))
```

#### <a name="c-disabling-and-re-enabling-system-versioning-to-change-table-schema"></a>В. Отключение и повторное включение системного управления версиями для изменения схемы таблицы

В этом примере показано, как отключить системное управление версиями в таблице `Department`, добавить столбец и повторно включить системное управление версиями. Для изменения схемы таблицы требуется отключение системного управления версиями. Выполняйте указанные действия в рамках транзакции, чтобы избежать применения обновлений к обеим таблицам во время обновления схемы таблицы. Это позволит администратору базы данных повысить производительность, пропустив проверки согласованности данных при возобновлении системного управления версиями. Для выполнения таких задач, как создание статистики, переключение секций или применение сжатия к одной или обеим таблицам, не требуется отключать системное управление версиями.

```sql
BEGIN TRAN
/* Takes schema lock on both tables */
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
/* expand table schema for temporal table */
ALTER TABLE Department  
     ADD Col5 int NOT NULL DEFAULT 0 ;
/* Expand table schema for history table */
ALTER TABLE DepartmentHistory
    ADD Col5 int NOT NULL DEFAULT 0 ;
/* Re-establish versioning again*/
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE=dbo.DepartmentHistory,
                                 DATA_CONSISTENCY_CHECK = OFF)) ;
COMMIT
```

#### <a name="d-removing-system-versioning"></a>Г. Удаление системного управления версиями

В этом примере показано, как полностью удалить системное управление версиями из таблицы Department и удалить таблицу `DepartmentHistory`. При необходимости можно также удалить столбцы периода, используемые системой для записи сведений о системном управлении версиями. При включенном системном управлении версиями удалить таблицы `Department` и `DepartmentHistory` нельзя.

```sql
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
ALTER TABLE Department
DROP PERIOD FOR SYSTEM_TIME ;
DROP TABLE DepartmentHistory ;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Во всех последующих примерах с А по В используется таблица `FactResellerSales` базы данных [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].

### <a name="a-determining-if-a-table-is-partitioned"></a>A. Определение секционирования таблицы

Следующий запрос возвращает одну или несколько строк, если таблица `FactResellerSales` секционирована. Если таблица не секционирована, строки не возвращаются.

```sql
SELECT * FROM sys.partitions AS p
JOIN sys.tables AS t
    ON p.object_id = t.object_id
WHERE p.partition_id IS NOT NULL
    AND t.name = 'FactResellerSales' ;
```

### <a name="b-determining-boundary-values-for-a-partitioned-table"></a>Б. Определение граничных значений для секционированной таблицы

Следующий запрос возвращает граничные значения для каждой секции в таблице `FactResellerSales` .

```sql
SELECT t.name AS TableName, i.name AS IndexName, p.partition_number,
    p.partition_id, i.data_space_id, f.function_id, f.type_desc,
    r.boundary_id, r.value AS BoundaryValue
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.partitions AS p
    ON i.object_id = p.object_id AND i.index_id = p.index_id
JOIN  sys.partition_schemes AS s
    ON i.data_space_id = s.data_space_id
JOIN sys.partition_functions AS f
    ON s.function_id = f.function_id
LEFT JOIN sys.partition_range_values AS r
    ON f.function_id = r.function_id and r.boundary_id = p.partition_number
WHERE t.name = 'FactResellerSales' AND i.type <= 1
ORDER BY p.partition_number ;
```

### <a name="c-determining-the-partition-column-for-a-partitioned-table"></a>В. Определение столбца секционирования секционированной таблицы

Следующий запрос возвращает имя столбца секционирования таблицы. `FactResellerSales`.

```sql
SELECT t.object_id AS Object_ID, t.name AS TableName,
    ic.column_id as PartitioningColumnID, c.name AS PartitioningColumnName
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.columns AS c
    ON t.object_id = c.object_id
JOIN sys.partition_schemes AS ps
    ON ps.data_space_id = i.data_space_id
JOIN sys.index_columns AS ic
    ON ic.object_id = i.object_id
    AND ic.index_id = i.index_id AND ic.partition_ordinal > 0
WHERE t.name = 'FactResellerSales'
AND i.type <= 1
AND c.column_id = ic.column_id ;
```

### <a name="d-merging-two-partitions"></a>Г. Объединение двух секций

В следующем примере объединяются две секции в таблице.

Таблица `Customer` имеет следующее определение:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100))) ;
```

Следующая команда объединяет границы секций 10 и 25.

```sql
ALTER TABLE Customer MERGE RANGE (10);
```

Новый DDL для таблицы:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 25, 50, 100))) ;
```

### <a name="e-splitting-a-partition"></a>Д. Разбиение секции

В следующем примере показано разбиение секции в таблице.

Таблица `Customer` имеет следующий DDL:

```sql
DROP TABLE Customer;

CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100 ))) ;
```

Следующая команда создает новую секцию, ограниченную значением 75, от 50 до 100.

```sql
ALTER TABLE Customer SPLIT RANGE (75);
```

Новый DDL для таблицы:

```sql
CREATE TABLE Customer (
   id INT NOT NULL,
   lastName VARCHAR(20),
   orderCount INT,
   orderDate DATE)
   WITH DISTRIBUTION = HASH(id),
   PARTITION ( orderCount (RANGE LEFT
      FOR VALUES (1, 5, 10, 25, 50, 75, 100))) ;
```

### <a name="f-using-switch-to-move-a-partition-to-a-history-table"></a>Е. Перемещение секции в таблицу журнала с помощью SWITCH

В следующем примере выполняется перемещение данных в секции таблицы `Orders` в секцию в таблице `OrdersHistory`.

Таблица `Orders` имеет следующий DDL:

```sql
CREATE TABLE Orders (
    id INT,
    city VARCHAR (25),
    lastUpdateDate DATE,
    orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01', '2005-01-01', '2006-01-01', '2007-01-01'))) ;
```

В этом примере таблица `Orders` содержит следующие разделы. Каждая секция содержит данные.

|Секция|Содержит данные?|Диапазон границ|
|---------------|---------------|--------------------|
|1|Да|OrderDate < '2004-01-01'|
|2|Да|'2004-01-01' <= OrderDate < '2005-01-01'|
|3|Да|'2005-01-01' <= OrderDate< '2006-01-01'|
|4|Да|'2006-01-01'<= OrderDate < '2007-01-01'|
|5|Да|'2007-01-01' <= OrderDate|
| &nbsp; | &nbsp; | &nbsp; |

- Секция 1 (содержит данные): OrderDate < '2004-01-01'
- Секция 2 (содержит данные): '2004-01-01' <= OrderDate < '2005-01-01'
- Секция 3 (содержит данные): '2005-01-01' <= OrderDate< '2006-01-01'
- Секция 4 (содержит данные): '2006-01-01'<= OrderDate < '2007-01-01'
- Секция 5 (содержит данные): '2007-01-01' <= OrderDate

Таблица `OrdersHistory` имеет следующий DDL со столбцами и именами столбцов, идентичными столбцам и именам столбцов в таблице `Orders`. Они имеют распределенный хэш в столбце `id`.

```sql
CREATE TABLE OrdersHistory (
   id INT,
   city VARCHAR (25),
   lastUpdateDate DATE,
   orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01'))) ;
```

Хотя форматы и имена столбцов должны быть одинаковыми, границы секций могут не совпадать. В этом примере таблица `OrdersHistory` содержит следующие две секции, которые пусты:

- Секция 1 (не содержит данные): OrderDate < '2004-01-01'
- Секция 2 (пустая): '2004-01-01' <= OrderDate

Для предыдущих двух таблиц следующая команда перемещает все строки с `OrderDate < '2004-01-01'` из таблицы `Orders` в таблицу `OrdersHistory`.

```sql
ALTER TABLE Orders SWITCH PARTITION 1 TO OrdersHistory PARTITION 1;
```

В результате первая секция в `Orders` пуста, а первая секция в `OrdersHistory` содержит данные. Теперь таблицы отображаются следующим образом:

 Таблица `Orders`

- Секция 1 (пустая): OrderDate < '2004-01-01'
- Секция 2 (содержит данные): '2004-01-01' <= OrderDate < '2005-01-01'
- Секция 3 (содержит данные): '2005-01-01' <= OrderDate< '2006-01-01'
- Секция 4 (содержит данные): '2006-01-01'<= OrderDate < '2007-01-01'
- Секция 5 (содержит данные): '2007-01-01' <= OrderDate

Таблица `OrdersHistory`

- Секция 1 (содержит данные): OrderDate < '2004-01-01'
- Секция 2 (пустая): '2004-01-01' <= OrderDate

Чтобы очистить таблицу `Orders`, можно удалить пустую секцию, объединив секции 1 и 2 следующим образом:

```sql
ALTER TABLE Orders MERGE RANGE ('2004-01-01');
```

После объединения таблица `Orders` содержит следующие разделы.

Таблица `Orders`

- Секция 1 (содержит данные): OrderDate < '2005-01-01'
- Секция 2 (содержит данные): '2005-01-01' <= OrderDate< '2006-01-01'
- Секция 3 (содержит данные): '2006-01-01'<= OrderDate < '2007-01-01'
- Секция 4 (содержит данные): '2007-01-01' <= OrderDate

Проходит еще один год, и теперь вы готовы к архивации данных за 2005 г. Для 2005 года можно выделить пустую секцию в таблице `OrdersHistory`, разделив пустую секцию следующим образом:

```sql
ALTER TABLE OrdersHistory SPLIT RANGE ('2005-01-01');
```

После разделения таблица `OrdersHistory` содержит следующие секции.

 Таблица `OrdersHistory`

- Секция 1 (содержит данные): OrderDate < '2004-01-01'
- Секция 2 (пустая): '2004-01-01' < '2005-01-01'
- Секция 3 (пустая): '2005-01-01' <= OrderDate

## <a name="see-also"></a>См. также:

- [sys.tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md)
- [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
