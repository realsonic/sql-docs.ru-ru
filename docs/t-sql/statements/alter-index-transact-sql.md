---
description: ALTER INDEX (Transact-SQL)
title: ALTER INDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER INDEX
- ALTER_INDEX_TSQL
dev_langs:
- t-sql
helpviewer_keywords:
- indexes [SQL Server], reorganizing
- ALTER INDEX statement
- indexes [SQL Server], disabling
- online index operations
- index reorganization [SQL Server]
- ALLOW_ROW_LOCKS option
- ALL keyword
- reorganizing indexes
- constraints [SQL Server], indexes
- row locks [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- locking [SQL Server], indexes
- partitioned indexes [SQL Server], rebuilding
- defragmenting indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- index modifications [SQL Server]
- indexes [SQL Server], modifying
- index options [SQL Server]
- modifying indexes
- index disabling [SQL Server]
- MAXDOP index option, ALTER INDEX statement
- spatial indexes [SQL Server], modifying
- indexes [SQL Server], options
- ALLOW_PAGE_LOCKS option
- page locks [SQL Server]
- index rebuild [SQL Server]
- index reorganize [SQL Server]
ms.assetid: b796c829-ef3a-405c-a784-48286d4fb2b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7854c78f0643294b1b5111c1b6f2e0b0ef07afa6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099608"
---
# <a name="alter-index-transact-sql"></a>ALTER INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Изменяет существующий индекс таблицы или представления (rowstore, columnstore или XML) посредством его отключения, перестройки или реорганизации либо посредством настройки параметров индекса.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
ALTER INDEX { index_name | ALL } ON <object>  
{  
      REBUILD {  
            [ PARTITION = ALL ] [ WITH ( <rebuild_index_option> [ ,...n ] ) ]   
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> ) [ ,...n ] ]  
      }  
    | DISABLE  
    | REORGANIZE  [ PARTITION = partition_number ] [ WITH ( <reorganize_option>  ) ]  
    | SET ( <set_index_option> [ ,...n ] )   
    | RESUME [WITH (<resumable_index_options>,[...n])]
    | PAUSE
    | ABORT
}  
[ ; ]  
  
<object> ::=   
{  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
}  
  
<rebuild_index_option > ::=  
{  
      PAD_INDEX = { ON | OFF }  
    | FILLFACTOR = fillfactor   
    | SORT_IN_TEMPDB = { ON | OFF }  
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | STATISTICS_INCREMENTAL = { ON | OFF }  
    | ONLINE = {   
          ON [ ( <low_priority_lock_wait> ) ]   
        | OFF } 
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }   
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]  
}  
  
<single_partition_rebuild_index_option> ::=  
{  
      SORT_IN_TEMPDB = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
    | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<reorganize_option>::=  
{  
       LOB_COMPACTION = { ON | OFF }  
    |  COMPRESS_ALL_ROW_GROUPS =  { ON | OFF}  
}  
  
<set_index_option>::=  
{  
      ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | COMPRESSION_DELAY= { 0 | delay [Minutes] }  
}  

<resumable_index_option> ::=
 { 
    MAXDOP = max_degree_of_parallelism
    | MAX_DURATION =<time> [MINUTES]
    | <low_priority_lock_wait>  
 }
 
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                          ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )  
}  

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse 
  
ALTER INDEX { index_name | ALL }  
    ON   [ schema_name. ] table_name  
{  
      REBUILD {  
            [ PARTITION = ALL [ WITH ( <rebuild_index_option> ) ] ] 
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> )] ] 
      }  
    | DISABLE  
    | REORGANIZE [ PARTITION = partition_number ]  
}  
[;]  

<rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]   
}

<single_partition_rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
}  
  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

 *index_name*  
 Имя индекса. Имена индексов должны быть уникальными в пределах таблицы или представления, но необязательно должны быть уникальными в пределах базы данных. Имена индексов должны удовлетворять правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
 ALL  
 Указывает все индексы, связанные с таблицей или представлением, независимо от типа индекса. Если указывается ключевое слово ALL, то инструкция не будет выполнена, если один или несколько индексов находятся вне сети или предназначенной только для чтения файловой группе или указанная операция запрещена для одного или нескольких типов индекса. В следующей таблице перечислены операции с индексами и запрещенные типы индексов.  
  
|Использование ключевого слова ALL с этой операцией|Отказывает, если в таблице имеется один или несколько|  
|----------------------------------------|----------------------------------------|  
|REBUILD WITH ONLINE = ON|XML-индекс<br /><br /> Пространственный индекс<br /><br /> Индекс columnstore: **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|REBUILD PARTITION = *partition_number*|Несекционированный, пространственный, отключенный индекс или XML-индекс|  
|REORGANIZE|Индексы с параметром ALLOW_PAGE_LOCKS, равным OFF|  
|REORGANIZE PARTITION = *partition_number*|Несекционированный, пространственный, отключенный индекс или XML-индекс|  
|IGNORE_DUP_KEY = ON|XML-индекс<br /><br /> Пространственный индекс<br /><br /> Индекс columnstore: **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|ONLINE = ON|XML-индекс<br /><br /> Пространственный индекс<br /><br /> Индекс columnstore: **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|RESUMABLE = ON| Возобновляемые индексы не поддерживаются с ключевым словом **All**. <br /><br /> **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. |   
  
> [!WARNING]
> Более подробные сведения об операциях с индексами, которые можно выполнить в сети, см. в разделе [Рекомендации по операциям с индексами в сети](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

 Если ключевое слово ALL указывается вместе с PARTITION = *partition_number*, то все индексы должны быть выровнены. Следовательно, они секционируются на основе эквивалентных функций секционирования. При использовании ключевого слова ALL вместе с PARTITION все индексные секции с одинаковым аргументом *partition_number* будут перестроены или реорганизованы. Дополнительные сведения о секционированных индексах см. в разделе [Секционированные таблицы и индексы](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 *database_name*  
 Имя базы данных.  
  
 *schema_name*  
 Имя схемы, которой принадлежит таблица или представление.  
  
 *table_or_view_name*  
 Имя таблицы или представления, связанного с индексом. Чтобы отобразить отчет по индексам объекта, следует воспользоваться представлением каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] поддерживает трехкомпонентный формат имени database_name.[schema_name].table_or_view_name, где database_name — текущая база данных или база данных tempdb, а имя таблицы или представления table_or_view_name начинается с #.  
  
 REBUILD [ WITH **(** \<rebuild_index_option> [ **,** ... *n*] **)** ]  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

Указывает, что индекс будет перестроен с использованием тех же столбцов, типов индекса, атрибута уникальности и порядка сортировки. Это предложение эквивалентно [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md). REBUILD включает отключенный индекс. При перестройке кластеризованного индекса не перестраиваются ассоциированные некластеризованные индексы, если только не указано ключевое слово ALL. Если параметры индекса не заданы, то применяется существующий параметр индекса, который хранится в таблице [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md). Для любого параметра индекса, значение которого не хранится в таблице **sys.indexes**, применяется значение по умолчанию, указанное в определении аргумента.  
  
 Если указано ключевое слово ALL, а базовая таблица реализована в виде кучи, операция перестроения не воздействует на таблицу. Перестраиваются все некластеризованные индексы, ассоциированные с таблицей.  
  
 Возможно минимальное протоколирование операции перестроения, если модель восстановления базы данных настроена на массовый или простой режим.  
  
> [!NOTE]
> При перестроении первичного XML-индекса индексированная пользовательская таблица недоступна в течение действия операции с индексами.  
 
Для индексов columnstore операция перестроения:  
  
-  Не использует порядок сортировки.  
-  Приобретает монопольную блокировку на таблице или секции на то время, как происходит перестроение.  Во время перестроения данные находятся в автономном режиме и недоступны даже при использовании NOLOCK, RCSI или SI.  
-  Повторно сжимает все данные в columnstore. Во время перестроения существуют две копии индекса columnstore. После завершения перестроения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] удаляет исходный индекс columnstore.  

Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
PARTITION  

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Указывает, что только одна секция индекса будет перестроена или реорганизована. PARTITION не может быть указана, если аргумент *index_name* — несекционированный индекс.  
  
 PARTITION = ALL, перестроение всех секций.  
  
> [!WARNING]
> Создание и перестройка невыровненных индексов для таблицы, количество секций в которой превышает 1000, возможны, но не поддерживаются. Это может привести к снижению производительности или чрезмерному потреблению памяти во время таких операций. Если количество секций превышает 1000, рекомендуется использовать только выровненные индексы.  
  
 *partition_number*  
   
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Количество секций секционированного индекса, который необходимо перестроить или реорганизовать. Аргумент *partition_number* является постоянным выражением, которое может обращаться к переменным. К ним относятся переменные определяемых пользователем типов или функции и определяемые пользователем функции, но не ссылки на инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. *partition_number* должен существовать, или выполнение инструкции завершится с ошибкой.  
  
 WITH **(** \<single_partition_rebuild_index_option> **)**  
   
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 `SORT_IN_TEMPDB`, `MAXDOP` и `DATA_COMPRESSION` — это параметры, которые могут быть указаны при перестроении одиночной секции (PARTITION = *номер_секции*). XML-индексы не могут быть указаны в операции перестроения одиночной секции.  
  
 DISABLE  
 Помечает индекс как отключенный и недоступный для использования компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Любой индекс может быть отключен. Определение отключенного индекса остается в системном каталоге без базовых индексных данных. Отключение кластеризованного индекса блокирует доступ пользователя к данным базовой таблицы. Чтобы активировать индекс, следует использовать инструкцию ALTER INDEX REBUILD или CREATE INDEX WITH DROP_EXISTING. Дополнительные сведения см. в разделах [Отключение индексов и ограничений](../../relational-databases/indexes/disable-indexes-and-constraints.md) и [Включение индексов и ограничений](../../relational-databases/indexes/enable-indexes-and-constraints.md).  
  
 Операция REORGANIZE для индекса **rowstore**  
 Для индексов rowstore REORGANIZE указывает необходимость реорганизации конечного уровня индекса. Операция REORGANIZE:  
  
-   всегда выполняется в сети. Это означает, что долгосрочные блокировки таблицы не удерживаются и запросы или обновления базовой таблицы могут продолжаться во время выполнения транзакции ALTER INDEX REORGANIZE.
-   не разрешается для отключенного индекса;
-   не разрешается, если параметру ALLOW_PAGE_LOCKS задано значение OFF;
-   при выполнении в транзакции не откатывается при откате транзакции.

> [!NOTE]
> Если инструкция ALTER INDEX REORGANIZE использует явные транзакции (например, ALTER INDEX внутри инструкции BEGIN TRAN... COMMIT/ROLLBACK) вместо режима неявных транзакций по умолчанию, поведение блокировки REORGANIZE становится более строгим, что может привести к блокировке. Дополнительные сведения о неявных транзакциях см. в статье [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md).

Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

REORGANIZE WITH **(** LOB_COMPACTION = { **ON** | OFF } **)**  
 Применяется к индексам rowstore.  
  
LOB_COMPACTION = ON  
  
-   Указывает сжатие всех страниц, содержащих данные следующих типов данных (LOB): image, text, ntext, varchar(max), nvarchar(max), varbinary(max) и xml. Сжатие этих данных может привести к уменьшению размера данных на диске.  
-   Для кластеризованного индекса сжимаются все столбцы LOB, содержащиеся в таблице.  
-   Для некластеризованного индекса сжимаются все столбцы LOB, являющиеся неключевыми столбцами, включенными в индекс.  
-   REORGANIZE ALL выполняет операцию LOB_COMPACTION для всех индексов. Для каждого индекса сжимаются все столбцы LOB в кластеризованном индексе, базовой таблице или включенные столбцы в некластеризованном индексе.  
  
LOB_COMPACTION = OFF  
  
-   Все страницы, содержащие данные большого объекта, не сжимаются.  
-   Параметр OFF не влияет на кучу.  
  
 Операция REORGANIZE для индекса **columnstore**  
 Для индексов columnstore REORGANIZE сжимает каждую разностную группу строк CLOSED в columnstore в виде сжатой группы строк. Операция REORGANIZE всегда выполняется в сети. Это означает, что долгосрочные блокировки таблицы не удерживаются и запросы или обновления базовой таблицы могут продолжаться во время выполнения транзакции ALTER INDEX REORGANIZE. Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
-   REORGANIZE не требуется для перемещения разностных групп строк CLOSED в сжатые группы строк. Для сжатия разностных групп строк CLOSE периодически активируется фоновый процесс перемещения кортежей (TM). REORGANIZE рекомендуется использовать при отставании процесса перемещения кортежей. REORGANIZE может сжимать группы строк более агрессивно.  
-   Сведения о сжатии всех групп строк OPEN и CLOSED см. далее в разделе о параметре `REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS)`.  
  
Для индексов columnstore в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] инструкция REORGANIZE выполняет следующие дополнительные оптимизации дефрагментации с подключением к сети:  
  
-   Физически удаляет строки из группы строк, если были логически удалено 10 % или более строк. Удаленные байты освобождают место на физическом носителе. Например, если в сжатой группе из одного миллиона строк удалено 100 тысяч строк, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] удалит эти строки и выполнит повторное сжатие группы с 900 тысяч строк. Группа будет сохранена в хранилище за счет удаления удаленных строк.  
  
-   Объединяет одну или несколько сжатых групп строк для увеличения числа строк для каждой группы до максимального значения, составляющего 1 048 576 строк. Например, при массовом импорте 5 пакетов с 102 400 строками вы получите 5 сжатых групп строк. При выполнении команды REORGANIZE эти групп строк будут объединены в 1 сжатую группу строк, содержащую 512 000 строк. Предполагается отсутствие ограничений на размер словаря или объем памяти.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] попытается объединить группу строк, в которой 10 % строк или больше были логически удалены, с одной или несколькими группами строк. Например, группа строк 1 сжимается с 500 000 строками, а группа строк 21 сжимается с максимум 1 048 576 строками. В группе строк 21 удалено 60 % строк и осталось 409 830 строк. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] объединяет эти две группы строк для сжатия новой группы строк, содержащей 909 830 строк.  
  
REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = { ON | **OFF** } )  
 Применимо к индексам columnstore. 

 **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

COMPRESS_ALL_ROW_GROUPS позволяет принудительно отправлять разностные группы строк OPEN или CLOSED в columnstore. При использовании этого параметра не требуется перестраивать индекс columnstore для очистки разностных групп строк.  Это, в сочетании и другими функциями дефрагментации удаления и слияния, отменяет необходимость перестроения индекса в большинстве случаев.    

-   ON принудительно отправляет все группы строк в columnstore независимо от размера и состояния (CLOSED или OPEN).  
-   OFF принудительно отправляет все группы строк CLOSED в columnstore.  

Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

SET **(** \<set_index option> [ **,** ... *n*] **)**  
 Указывает параметры индекса без перестройки или реорганизации индекса. SET нельзя указать для отключенного индекса.  
  
PAD_INDEX = { ON | OFF }  
   
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

 Определяет разреженность индекса. Значение по умолчанию — OFF.  
  
 ON  
 Процент свободного места, определяемый параметром FILLFACTOR, применяется к страницам индекса промежуточного уровня. Если параметр FILLFACTOR не указан и при этом параметру PAD_INDEX задано значение ON, то используется значение коэффициента заполнения, хранимое в таблице [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 OFF или *fillfactor* не указан  
 Страницы промежуточного уровня заполняются почти полностью. При этом остается достаточно места по крайней мере для одной строки максимального размера, которого может достигать индекс, в зависимости от набора ключей в промежуточных страницах.  
  
 Дополнительные сведения см. в разделе [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
FILLFACTOR = *fillfactor*  
 
 **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Определяет величину в процентах, показывающую насколько должен компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] заполнять конечный уровень каждой страницы индекса во время его создания и изменения. Значение *fillfactor* должно быть целым числом от 1 до 100. Значение по умолчанию равно 0. Значения коэффициентов заполнения 0 и 100 идентичны.  
  
 Явный параметр FILLFACTOR применяется, только если индекс создается впервые или перестраивается. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] не сохраняет динамически указанный процентный объем свободного места на страницах. Дополнительные сведения см. в разделе [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
 Увидеть коэффициент заполнения можно в таблице **sys.indexes**.  
  
> [!IMPORTANT]
> Создание или замена кластеризованного индекса со значением FILLFACTOR влияет на пространство памяти, занимаемое данными, так как компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] перераспределяет данные при создании кластеризованного индекса.  
  
 SORT_IN_TEMPDB = { ON | **OFF** }  
 
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Указывает, следует ли сохранять результаты сортировки в базе данных **tempdb**. Значение по умолчанию — OFF, за исключением функции гипермасштабирования базы данных SQL Azure. Для всех операций построения индекса в режиме гипермасштабирования параметр SORT_IN_TEMPDB всегда включен независимо от указанного параметра, если не используется возобновляемое перестроение индекса.  
  
 ON  
 Промежуточные результаты сортировки, которые используются при индексировании, хранятся в базе данных **tempdb**. Это может сократить время, требуемое для создания индекса, если база данных **tempdb** размещена на иных дисках, нежели пользовательская база данных. Однако это увеличивает использование места на диске, которое используется при индексировании.  
  
 OFF  
 Промежуточные результаты сортировки хранятся в той же базе данных, где и индекс.  
  
 Если выполнение сортировки не требуется или если сортировка может быть выполнена в памяти, параметр SORT_IN_TEMPDB пропускается.  
  
 Дополнительные сведения см. в разделе [Параметр SORT_IN_TEMPDB для индексов](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=** { ON | OFF }  
 Определяет ответ на ошибку, случающуюся, когда операция вставки пытается вставить в уникальный индекс повторяющиеся значения ключа. Параметр IGNORE_DUP_KEY применяется только к операциям вставки, производимым после создания или перестроения индекса. Значение по умолчанию — OFF.  
  
 ON  
 Если в уникальный индекс вставляются повторяющиеся значения ключа, выводится предупреждающее сообщение. С ошибкой завершаются только строки, нарушающие ограничение уникальности.  
  
 OFF  
 Если в уникальный индекс вставляются повторяющиеся значения ключа, выводится сообщение об ошибке. Будет выполнен откат всей операции INSERT.  
  
 IGNORE_DUP_KEY нельзя установить в значение ON для индексов, создаваемых для представлений, неуникальных индексов, XML-индексов, пространственных индексов и фильтруемых индексов.  
  
 Для просмотра значения IGNORE_DUP_KEY используйте [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Для обратной совместимости синтаксиса аргумент WITH IGNORE_DUP_KEY эквивалентен аргументу WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | OFF }  
 Указывает, выполнялся ли перерасчет статистики распределения. Значение по умолчанию — OFF.  
  
 ON  
 Устаревшие статистики не пересчитываются автоматически.  
  
 OFF  
 Автоматическое обновление статистических данных включено.  
  
 Чтобы восстановить автоматическое обновление статистики, следует установить STATISTICS_NORECOMPUTE в значение OFF или выполнить UPDATE STATISTICS без предложения NORECOMPUTE.  
  
> [!IMPORTANT]
> Отключение автоматического перерасчета статистики распределения может помешать оптимизатору запросов выбрать оптимальные планы выполнения запросов, обращенных к таблице.  
  
STATISTICS_INCREMENTAL = { ON | **OFF** }  

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

При значении **ON** статистики создаются как статистики отдельно по секциям. При значении **OFF** дерево статистик удаляется и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] повторно вычисляет статистики. Значение по умолчанию — **OFF**.  
  
 Если статистики по секциям не поддерживаются, параметр пропускается и выводится предупреждение. Добавочные статистики не поддерживаются для следующих типов статистических данных.  
  
-   Статистика, созданная с индексами, не выровненными по секциям для базовой таблицы.  
-   Статистика, созданная в доступных для чтения базах данных-получателях AlwaysOn.  
-   Статистика, созданная в базах данных, доступных только для чтения.  
-   Статистика, созданная по фильтрованным индексам.  
-   Статистика, созданная по представлениям.  
-   Статистика, созданная по внутренним таблицам.  
-   Статистика, созданная с пространственными индексами или XML-индексами.  
  
 ONLINE **=** { ON | **OFF** } \<as applies to rebuild_index_option>  
 Определяет, будут ли базовые таблицы и связанные индексы доступны для запросов и изменения данных во время операций с индексами. Значение по умолчанию — OFF.  
  
 Для XML-индекса или пространственного индекса поддерживается только значение `ONLINE = OFF`. Если для ONLINE задано состояние ON, возникает ошибка.  
  
> [!IMPORTANT]
> Операции с индексами в сети доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статьях [Возможности, поддерживаемые различными выпусками [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]](../../sql-server/editions-and-supported-features-for-sql-server-2016.md) и [Возможности, поддерживаемые различными выпусками SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
 ON  
 Долгосрочные блокировки таблицы не поддерживаются во время операций с индексами. Во время главной фазы операций с индексами только блокировка с намерением совмещаемого доступа (IS) удерживается в исходной таблице. Это позволяет продолжить выполнение запросов или обновлений для базовых таблиц и индексов. В начале операции совмещаемая блокировка (S) исходного объекта поддерживается в течение очень короткого времени. Если создается некластеризованный индекс, то по завершении операции на короткое время создается блокировка типа S (совмещаемая) для источника. Блокировка типа SCH-M (изменения схемы) запрашивается, если кластеризованный индекс создается или удаляется в режиме в сети либо, происходит перестроение кластеризованного или некластеризованного индекса. При создании индекса для временной локальной таблицы параметр ONLINE не может принимать значение ON.  
  
 OFF  
 Блокировки таблиц применяются во время выполнения операций с индексами. Операция с индексами в режиме «вне сети», которая создает, перестраивает или удаляет кластеризованный, пространственный или XML-индекс либо перестраивает или удаляет некластеризованный индекс, получает блокировку изменения схемы (Sch-M) для этой таблицы. Это предотвращает доступ к базовой таблице всех пользователей во время операции. Операция с индексами вне сети, создающая некластеризованный индекс, получает совмещаемую блокировку (S) в таблице. Это запрещает проводить обновления базовой таблицы, но разрешает проводить операции чтения, например инструкции SELECT.  
  
Дополнительные сведения см. в статье [Выполнение операции с индексами в сети](../../relational-databases/indexes/perform-index-operations-online.md).  
  
Индексы, в том числе индексы глобальных временных таблиц, могут быть перестроены при подключении к сети, за исключением следующего:
- XML-индекс
- Индекс локальной временной таблицы
- Исходные уникальные кластеризованные индексы представлений.
- Индексы columnstore
- Кластеризованные индексы, если базовая таблица содержит типы данных LOB: **image**, **ntext**, **text** и пространственные типы данных.
- Столбцы **varchar(max)** и **varbinary(max)** не могут быть частью индекса. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], если таблица содержит столбец **varchar(max)** или **varbinary(max)** , кластеризованный индекс, содержащий другие столбцы, можно построить или перестроить с использованием параметра **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не разрешает использовать параметр **ONLINE**, если базовая таблица содержит столбец **varchar(max)** или **varbinary(max)** .

Дополнительные сведения см. в разделе [Об операциях с индексами в режиме "в сети"](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

 Указывает, является ли операция с индексами в режиме "в сети" возобновляемой.

 Операция ON с индексами является возобновляемой.

 Операция OFF с индексами является невозобновляемой.

MAX_DURATION **=** *time* [**MINUTES**] используется с **RESUMABLE = ON** (требуется **ONLINE = ON**).

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

Указывает время (целочисленное значение минутах), в течение которого выполняется возобновляемая операция с индексами в сети до приостановки. 

> [!IMPORTANT]
> Более подробные сведения об операциях с индексами, которые можно выполнить в сети, см. в разделе [Рекомендации по операциям с индексами в сети](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Возобновляемое перестроение индекса с подключением к сети не поддерживается для индексов columnstore.

ALLOW_ROW_LOCKS **=** { **ON** | OFF }  

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Указывает, разрешена ли блокировка строк. Значение по умолчанию — ON.  
  
 ON  
 Блокировки строк допустимы при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки.  
  
 OFF  
 Блокировки строк не используются.  
  
ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Указывает, разрешена ли блокировка страниц. Значение по умолчанию — ON.  
  
 ON  
 Блокировки страниц допустимы при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц.  
  
 OFF  
 Блокировки страниц не используются.  
  
> [!NOTE]
> Индекс не может быть реорганизован, если ALLOW_PAGE_LOCKS установлен в состояние OFF.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Определяет, следует ли выполнять оптимизацию, связанную с состязанием при операциях вставки на последнюю страницу. Значение по умолчанию — OFF. См. подробнее раздел о [последовательных ключах](./create-index-transact-sql.md#sequential-keys) в документации по CREATE INDEX.

 MAXDOP **=** max_degree_of_parallelism  
 
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Переопределяет параметр конфигурации **max degree of parallelism** на время выполнения операции с индексами. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
> [!IMPORTANT]
>  Хотя параметр MAXDOP синтаксически поддерживается для всех индексов XML, для пространственного или первичного XML-индекса инструкция ALTER INDEX в настоящее время использует только один процессор.  
  
 Параметр *max_degree_of_parallelism* может иметь одно из следующих значений:  
  
 1  
 Подавляет формирование параллельных планов.  
  
 \>1  
 Ограничивает указанным значением максимальное число процессоров, используемых для параллельных операций с индексами.  
  
 0 (по умолчанию)  
 В зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
 Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
COMPRESSION_DELAY **=** { **0** |*duration [Minutes]* }  

**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])  
  
 Для таблицы на основе диска значение delay указывает минимальное количество минут, в течение которых разностная группа строк в состоянии CLOSED должна оставаться в разностной группе строк до того, как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сможет преобразовать ее в сжатую группу строк. Так как таблицы на основе диска не отслеживают время вставки и обновления отдельных строк, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] применяется задержка к разностным группам строк в состоянии CLOSED.  
  
 Значение по умолчанию — 0 минут.  
  
 Рекомендации по использованию COMPRESSION_DELAY см. в разделе [Начало работы с columnstore для получения операционной аналитики в реальном времени](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
 DATA_COMPRESSION  
 Задает режим сжатия данных для указанного индекса, номера секции или диапазона секций. Существуют следующие варианты выбора.  
  
 None  
 Индекс или заданные секции не сжимаются. Это не относится к индексам columnstore.  
  
 ROW  
 Для индекса или заданных секций производится сжатие строк. Это не относится к индексам columnstore.  
  
 PAGE  
 Для индекса или заданных секций производится сжатие страниц. Это не относится к индексам columnstore.  
  
 COLUMNSTORE  
   
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Применяется только к индексам columnstore, включая некластеризованные и кластеризованные индексы columnstore. COLUMNSTORE указывает, что должны быть распакованы индекс или конкретные секции, которые были упакованы с помощью параметра COLUMNSTORE_ARCHIVE. При восстановлении данных сжатие будет продолжаться с применением сжатия columnstore, предусмотренного для всех индексов columnstore.  
  
 COLUMNSTORE_ARCHIVE  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Применяется только к индексам columnstore, включая некластеризованные и кластеризованные индексы columnstore. Параметр COLUMNSTORE_ARCHIVE обеспечивает дальнейшее сжатие указанной секции до еще меньшего размера. Это может использоваться для архивации или в других ситуациях, где требуется уменьшение объема пространства и допускается увеличение затрат времени на сохранение и выборку  
  
 Дополнительные сведения о сжатии см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).  
  
 ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...n] **)**  
    
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
 Указывает секции, к которым применяется параметр DATA_COMPRESSION. Если индекс не секционирован, аргумент ON PARTITIONS создаст ошибку. Если не указано предложение ON PARTITIONS, то параметр DATA_COMPRESSION применяется ко всем секциям секционированного индекса.  
  
 \<partition_number_expression> можно указать одним из следующих способов:  
  
-   указав номер секции, например `ON PARTITIONS (2)`;  
-   указав номера нескольких секций, разделив их запятыми, например `ON PARTITIONS (1, 5)`;  
-   указав как диапазоны секций, так и отдельные секции, например `ON PARTITIONS (2, 4, 6 TO 8)`.  
  
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
  
 ONLINE **=** { ON  | **OFF** } \<as applies to single_partition_rebuild_index_option>  
 Указывает, может ли быть перестроен индекс или секция индекса базовой таблицы в режиме "в сети" или "вне сети". Если **REBUILD** выполняется в режиме "в сети" (**ON**), то данные таблицы доступны для запросов и изменения данных во время операций с индексами.  Значение по умолчанию — **OFF**.  
  
 ON  
 Долгосрочные блокировки таблицы не поддерживаются во время операций с индексами. Во время главной фазы операций с индексами только блокировка с намерением совмещаемого доступа (IS) удерживается в исходной таблице. Необходимо наличие S-блокировки таблицы в начале перестройки индекса и блокировки Sch-M на таблице в конце перестроения индекса в режиме "в сети". Обе блокировки являются короткими блокировками метаданных, но при этом блокировка изменения схемы (Sch-M) должна ожидать завершения всех блокирующих транзакций. Во время ожидания Sch-M блокирует все другие транзакции, ожидающие за этой блокировкой доступа к одной таблице.  
  
> [!NOTE]
>  Перестроение индекса в режиме "в сети" может задать параметры *low_priority_lock_wait*, описанные ниже в этом разделе.  
  
 OFF  
 Блокировки таблиц применяются во время выполнения операций с индексами. Это предотвращает доступ к базовой таблице всех пользователей во время операции.  
  
 WAIT_AT_LOW_PRIORITY используется только с **ONLINE=ON**.  
 
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Перестроение индекса в режиме «в сети» должно ожидать операции блокировки в этой таблице. **WAIT_AT_LOW_PRIORITY** указывает, что операция перестроения индекса в режиме "в сети" будет ожидать блокировки с низким приоритетом, позволяя выполняться другим операциям, пока операция перестроения индекса в режиме "в сети" находится в состоянии ожидания. Пропуск параметра **WAIT AT LOW PRIORITY** равнозначен использованию параметра WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE). Дополнительные сведения см. в разделе [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 
  
 MAX_DURATION = *time* [**MINUTES**]  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Время ожидания (целочисленное значение, указанное в минутах) в течение которого блокировки для операции перестроения индекса в режиме «в сети» будут ожидать с низким приоритетом при выполнении команды DDL. Если операция будет заблокирована на время **MAX_DURATION**, будет выполнено одно из действий **ABORT_AFTER_WAIT**. Время **MAX_DURATION** всегда указывается в минутах, и слово **MINUTES** можно опустить.  
 
 ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
   
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 None  
 Продолжить ожидание блокировки с обычным приоритетом.  
  
 SELF  
 Прекратить операцию DDL по перестроению индекса в режиме «в сети», выполняемую в данный момент без выполнения какого-либо действия.  
  
 BLOCKERS  
 Остановить все пользовательские транзакции, в данный момент блокирующие операцию DDL по перестроению индекса в режиме «в сети», чтобы можно было продолжить данную операцию. Параметр **BLOCKERS** требует, чтобы у имени входа было разрешение **ALTER ANY CONNECTION**.  
 
 RESUME 
 
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

Возобновить операцию с индексами, приостановленную вручную или из-за сбоя.

MAX_DURATION используется с **RESUMABLE=ON**
 
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Время (целочисленное значение минутах), в течение которого выполняется возобновляемая операция с индексами с подключением к сети после приостановки. После истечения этого времени возобновляемая операция приостанавливается, если она все еще выполнялась.

WAIT_AT_LOW_PRIORITY используется с **RESUMABLE=ON** и **ONLINE = ON**.  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
 Возобновление перестроения индекса в режиме "в сети" после приостановки должно ожидать операции блокировки в этой таблице. **WAIT_AT_LOW_PRIORITY** указывает, что операция перестроения индекса в режиме "в сети" будет ожидать блокировки с низким приоритетом, позволяя выполняться другим операциям, пока операция перестроения индекса в режиме "в сети" находится в состоянии ожидания. Пропуск параметра **WAIT AT LOW PRIORITY** равнозначен использованию параметра WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE). Дополнительные сведения см. в разделе [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 

PAUSE
 
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
Приостановить возобновляемую операцию перестроения индексов в режиме "в сети".

ABORT

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

Прервать выполняющуюся или приостановленную операцию с индексами, объявленную как возобновляемая. Чтобы прекратить возобновляемую операцию перестроения индексов, необходимо явно выполнить команду **ABORT**. Сбой или приостановка возобновляемой операции с индексами не прекращает ее выполнение. Операция остается в неопределенном состоянии приостановки.
  
## <a name="remarks"></a>Remarks  
Инструкция ALTER INDEX не может использоваться для повторного секционирования индекса или его перемещения в другую файловую группу. Эта инструкция не может использоваться для изменения определения индекса, в том числе добавления или удаления столбцов или изменения порядка столбцов. Для выполнения этих операций следует использовать инструкцию CREATE INDEX с предложением DROP_EXISTING.  
  
Если параметр не указан явно, то применяется текущий параметр. Например, если параметр FILLFACTOR не указан в предложении REBUILD, то коэффициент заполнения, сохраненный в системном каталоге, будет использоваться в процессе перестроения. Для просмотра текущего параметра индекса следует использовать таблицу [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
Значения `ONLINE`, `MAXDOP` и `SORT_IN_TEMPDB` не хранятся в системном каталоге. Если значение некоторого параметра не указано в инструкции индекса, то используется значение по умолчанию.
  
На компьютерах с несколькими процессорами инструкция `ALTER INDEX REBUILD`, как и другие запросы, использует больше процессоров для операций просмотра и сортировки, связанных с изменением индекса. При выполнении инструкции `ALTER INDEX REORGANIZE` без предложения LOB_COMPACTION или с ним значение аргумента **max degree of parallelism** представляет собой однопотоковую операцию. Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!IMPORTANT]
> Индекс нельзя реорганизовать или перестроить, если файловая группа, в которой он расположен, находится в автономном режиме или предназначена только для чтения. Если указывается ключевое слово ALL, а один или несколько индексов размещены в файловой группе, которая находится в автономном режиме или предназначена только для чтения, то выполнить инструкцию не удастся.  
  
## <a name="rebuilding-indexes"></a><a name="rebuilding-indexes"></a> Перестроение индексов  
При перестроении старый индекс удаляется, и создается новый. Таким образом, устраняется фрагментация, восстанавливается место на диске путем сжатия страниц с учетом указанного или существующего коэффициента заполнения, переупорядочиваются индексные строки в последовательных страницах. Если указывается ключевое слово ALL, то все индексы для таблицы удаляются и перестраиваются в одной транзакции. Ограничения FOREIGN KEY не обязательно отменять заранее. Если перестраиваются индексы с 128 или большим числом экстентов, то компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] откладывает процедуры освобождения страниц и связанные с ними блокировки до фиксации транзакции.  
 
Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
## <a name="reorganizing-indexes"></a><a name="reorganizing-indexes"></a> Реорганизация индексов
Для реорганизации индекса требуется минимальный объем системных ресурсов. При реорганизации концевой уровень кластеризованных и некластеризованных индексов на таблицах и представлениях дефрагментируется путем физической реорганизации страниц конечного уровня, в результате чего они выстраиваются в соответствии с логическим порядком конечных узлов (слева направо). Кроме того, реорганизация сжимает страницы индекса. Их сжатие производится в соответствии с текущим значением коэффициента заполнения. 
  
Если указывается ключевое слово `ALL`, то реляционные индексы, как кластеризованные, так и некластеризованные, и XML-индексы для таблицы реорганизуются. Существуют некоторые ограничения при указаниии ALL. См. определение ALL в разделе "Аргументы" этой статьи.  
  
Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  

> [!IMPORTANT]
> Для таблицы [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] с упорядоченным кластеризованным индексом columnstore `ALTER INDEX REORGANIZE` не будет выполнять повторную сортировку данных. Для повторной сортировки данных используйте `ALTER INDEX REBUILD`.
  
## <a name="disabling-indexes"></a><a name="disabling-indexes"></a> Отключение индексов  
Отключение индексов предотвращает доступ пользователя к индексам в случае использования кластеризованных индексов к данным базовой таблицы. Определение индекса остается в системном каталоге. Отключение некластеризованных индексов или кластеризованных индексов в представлении физически удаляет данные индекса. При отключении кластеризованного индекса блокируется доступ к данным, но данные остаются необслуживаемыми в сбалансированном дереве до тех пор, пока индекс не будет удален или перестроен. Для просмотра состояния включенного или отключенного индекса следует направить запрос в столбец **is_disabled** в представлении каталога **sys.indexes**.  
  
Если таблица входит в публикацию репликации транзакций, то нельзя отключить никакие индексы, связанные с первичными ключевыми столбцами. Эти индексы необходимы для репликации. Чтобы отключить индексы, сначала необходимо удалить таблицу из публикации. Дополнительные сведения см. в статье [Публикация данных и объектов базы данных](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
С помощью инструкции `ALTER INDEX REBUILD` или `CREATE INDEX WITH DROP_EXISTING` включите индекс. Перестроить отключенный кластеризованный индекс нельзя, если параметр ONLINE установлен в ON. Дополнительные сведения см. в статье [Отключение индексов и ограничений](../../relational-databases/indexes/disable-indexes-and-constraints.md).  
  
## <a name="setting-options"></a>Настройка параметров  
Вы можете задать параметры `ALLOW_ROW_LOCKS`, `ALLOW_PAGE_LOCKS`, `OPTIMIZE_FOR_SEQUENTIAL_KEY`, `IGNORE_DUP_KEY` и `STATISTICS_NORECOMPUTE` для определенного индекса без перестройки или реорганизации этого индекса. Измененные значения немедленно применяются к индексу. Для просмотра этих установок следует использовать таблицу **sys.indexes**. Дополнительные сведения см. в разделе [Установка параметров индекса](../../relational-databases/indexes/set-index-options.md).  
  
### <a name="row-and-page-locks-options"></a>Параметры блокировок строк и страниц  
Когда присвоены значения `ALLOW_ROW_LOCKS = ON` и `ALLOW_PAGE_LOCK = ON`, при доступе к индексу допустимы блокировки на уровне строк, страниц и таблиц. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выбирает соответствующую блокировку и может повышать уровень с блокировки строки или страницы до блокировки таблицы.  
  
Если присвоены значения `ALLOW_ROW_LOCKS = OFF` и `ALLOW_PAGE_LOCK = OFF`, при доступе к индексу допустима только блокировка на уровне таблиц.  
  
Если при установке параметров блокировки строки или страницы указывается ключевое слово ALL, то установки применяются ко всем индексам. Если базовая таблица представляет собой кучу, установки применяются следующими способами:  
  
|Параметр|Сведения|
|------|-------|
|ALLOW_ROW_LOCKS = ON или OFF|Для кучи и любых соответствующих некластеризованных индексов.|  
|ALLOW_PAGE_LOCKS = ON|Для кучи и любых соответствующих некластеризованных индексов.|  
|ALLOW_PAGE_LOCKS = OFF|Полностью для некластеризованных индексов. Это означает, что все блокировки страниц запрещаются для некластеризованных индексов. В куче запрещены только общая блокировка (S), блокировка обновления (U) и монопольная блокировка (X) для страниц. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] может запросить намеренную блокировку страницы (IS, IU или IX) для внутренних целей.|  
  
## <a name="online-index-operations"></a><a name="online-index-operations"></a> Операции с индексами в режиме "в сети"  
Если при перестройке индекса параметр ONLINE установлен в значение ON, то базовые объекты, таблицы и связанные с ними индексы доступны для запросов и изменения данных. Можно также перестроить в режиме «в сети» часть индекса, находящегося в одной секции. Монопольные блокировки таблиц удерживаются лишь на очень короткое время в процессе изменения.  
  
Реорганизация индекса всегда выполняется в режиме в сети. Процесс не удерживает блокировку в течение долгого времени и поэтому не блокирует выполняемые запросы и обновления.  
  
Параллельные операции с индексами в режиме «в сети» для одной таблицы или секции можно выполнять лишь при выполнении следующих действий:  
  
-   создание нескольких некластеризованных индексов;  
-   реорганизация различных индексов в одной таблице;  
-   реорганизация различных индексов при перестройке неперекрывающихся индексов в одной таблице.  
  
Все остальные попытки выполнения операций с индексами в сети завершаются ошибкой. Например, нельзя одновременно перестроить два или несколько индексов в одной таблице или создать новый индекс в процессе перестройки существующего индекса для этой таблицы.  

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a> Возобновляемые операции с индексами

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

Операция ONLINE INDEX REBUILD указывается как возобновляемая с помощью параметра RESUMABLE = ON. 
-  Параметр RESUMABLE не сохраняется в метаданных для указанного индекса и применяется только на время выполнения текущей инструкции DDL. Таким образом, для включения возобновляемости предложение RESUMABLE=ON должно быть указано явным образом.
-  Параметр MAX_DURATION поддерживается для параметра RESUMABLE=ON или параметра аргумента **low_priority_lock_wait**. 
   -  Параметр MAX_DURATION для RESUMABLE задает временной интервал для перестраиваемого индекса. После окончания этого времени операция перестроения индекса приостанавливается или завершает выполнение. Пользователь решает, когда можно возобновить перестроение приостановленного индекса. Значение **time** в минутах для MAX_DURATION должно быть больше 0 минут и меньше или равно 1 неделе (7 \* 24 \* 60 = 10 080 минут). Длинная пауза в операции с индексами может повлиять на производительность DML в конкретной таблице, а также на емкость диска базы данных, поскольку они оба индексируют исходное и только что созданное требуемое место на диске и должны быть обновлены во время операций DML. Если параметр MAX_DURATION пропускается, операция с индексами будет продолжаться вплоть до ее завершения или до момента возникновения сбоя. 
   -  Параметр аргумента \<low_priority_lock_wait> позволяет решить, каким образом будет продолжена операция с индексами при SCH-M-блокировке.
 
-  Повторное выполнение исходной инструкции ALTER INDEX REBUILD с теми же параметрами возобновляет приостановленную операцию перестроение индексов. Возобновить приостановленную операцию перестроения индексов можно также путем выполнения инструкции ALTER INDEX RESUME.
-  Параметр SORT_IN_TEMPDB = ON не поддерживается для возобновляемых индексов. 
-  Команду DDL с параметром RESUMABLE=ON невозможно выполнить внутри явной транзакции (она не может быть частью блока BEGIN TRAN ... COMMIT).
-  Возобновляемыми являются только приостановленные операции с индексами.
-  При возобновлении приостановленной операции с индексами можно изменить значение MAXDOP на новое.  Если параметр MAXDOP не указан при возобновлении приостановленной операции с индексами, берется последнее значение MAXDOP. Если параметр MAXDOP вообще не указан для операции перестроения индексов, берется значение по умолчанию.
- Чтобы немедленно приостановить операцию с индексами, можно остановить текущую команду (CTRL+C) либо выполнить команду ALTER INDEX PAUSE INDEX или KILL *session_id*. После приостановки команду можно возобновить с помощью параметра RESUME.
-  Команда ABORT разрывает сеанс, размещающий исходное перестроение индекса, и прерывает выполнение операции с индексами.  
-  Для возобновляемого перестроения индекса не требуются дополнительные ресурсы, за исключением перечисленных ниже.
   -    Дополнительное место для хранения создаваемого индекса, включая время, когда индекс будет приостановлен.
   -    Состояние DDL, запрещающее изменения DDL.
-  На этапе приостановки индекса будет выполняться процесс очистки фантомных записей, но он будет приостановлен во время выполнения индекса.   
Для возобновляемых операций перестроения индексов отключены следующие функциональные возможности.
   -    Перестроение отключенного индекса не поддерживается с RESUMABLE = ON.
   -    Команда ALTER INDEX REBUILD ALL.
   -    ALTER TABLE с командой DDL для перестроения индекса  
   -    Команду DDL с параметром "RESUMABLE=ON" невозможно выполнить внутри явной транзакции (она не может быть частью блока BEGIN TRAN ... COMMIT)
   -    Перестроение индекса, который содержит вычисляемые столбцы или столбцы TIMESTAMP в качестве ключевых столбцов.
-   Если базовая таблица содержит столбцы LOB, для возобновляемого перестроения кластеризованных индексов требуется Sch-M-блокировка в начале этой операции. 

> [!NOTE]
> Команда DDL выполняется вплоть до завершения, приостанавливается или завершается ошибкой. Если команда приостанавливается, возникнет ошибка, указывающая на приостановку операции и невозможность завершения создания индекса. Дополнительные сведения о текущем состоянии индекса можно получить из [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Как и в случае выше, при сбое также будет выведено сообщение об ошибке. 

 Дополнительные сведения см. в статье [Выполнение операции с индексами в сети](../../relational-databases/indexes/perform-index-operations-online.md).  
  
 ### <a name="wait_at_low_priority-with-online-index-operations"></a>WAIT_AT_LOW_PRIORITY с операциями с индексами в режиме "в сети"  
  
 Для выполнения инструкции DDL для перестроения индекса в режиме «в сети» все активные блокирующие транзакции, выполняемые для конкретной таблицы, должны быть завершены. Если выполняется перестроение индекса в режиме «в сети», то все новые транзакции, готовые к выполнению на данной таблице, блокируются. Хотя продолжительность блокировки для перестроения индекса в режиме «в сети» очень коротка, ожидание завершения всех открытых транзакций на данной таблице и блокировка новых запускаемых транзакций может значительно отразиться на пропускной способности и времени выполнения операции, а также значительно ограничить доступ к базовой таблице. Параметр **WAIT_AT_LOW_PRIORITY** позволяет администратору базы данных управлять S и Sch-M блокировками, необходимыми для перестроения индекса в режиме "в сети". Доступны 3 варианта. Во всех 3 случаях, если во время ожидания ( (MAX_DURATION = n [minutes]) ) нет блокирующих действий, перестроение индекса с подключением выполняется немедленно и без ожидания завершения инструкции DDL.  
  
## <a name="spatial-index-restrictions"></a>Ограничения пространственного индекса  
 При перестроении пространственного индекса базовая пользовательская таблица недоступна на протяжении выполнения операции с индексом, поскольку пространственный индекс блокирует схему.  
  
 Ограничение PRIMARY KEY в пользовательской таблице не может быть изменено, пока для столбца этой таблицы определен пространственный индекс. Для изменения ограничения PRIMARY KEY сначала необходимо удалить все пространственные индексы таблицы. После изменения ограничения PRIMARY KEY все пространственные индексы можно создать повторно.  
  
 В отдельной операции перестроения секции невозможно указать пространственные индексы. Однако пространственные индексы можно указать при полном перестроении секции.  
  
 Чтобы изменить параметры, характерные для пространственного индекса (такие как BOUNDING_BOX или GRID), необходимо либо применить инструкцию CREATE SPATIAL INDEX с параметром DROP_EXISTING = ON, либо удалить пространственный индекс и создать новый. Пример см. в разделе [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
## <a name="data-compression"></a>Сжатие данных  
 Дополнительную информацию о сжатии данных см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).  
  
 Чтобы оценить, как изменение параметров сжатия PAGE и ROW повлияет на таблицу, индекс или секцию, используйте хранимую процедуру [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).  
  
На секционированные индексы налагаются следующие ограничения.  
  
-   Если у таблицы есть невыровненные индексы, то изменить настройку сжатия отдельной секции с помощью инструкции ALTER INDEX ALL невозможно.  
-   Инструкция ALTER INDEX \<index> ... REBUILD PARTITION ... производит перестроение указанной секции индекса.  
-   Инструкция ALTER INDEX \<index> ... REBUILD WITH ... производит перестроение всех секций индекса.  
  
## <a name="statistics"></a>Статистика  
 При применении инструкции **ALTER INDEX ALL ...** к таблице происходит обновление только тех статистических данных, которые связаны с индексами. Автоматические или созданные вручную статические данные таблицы (вместо индекса) не обновляются.  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения ALTER INDEX необходимо иметь как минимум разрешение ALTER для таблицы или представления.  
  
## <a name="version-notes"></a>Заметки о версии  
  
-  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] не использует параметры файловой группы и файлового потока.  
-  Индексы columnstore недоступны до [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. 
-  Возобновляемые операции с индексами будут доступны начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
  
## <a name="basic-syntax-example"></a>Пример основного синтаксиса:   
  
```sql 
ALTER INDEX index1 ON table1 REBUILD;  
  
ALTER INDEX ALL ON table1 REBUILD;  
  
ALTER INDEX ALL ON dbo.table1 REBUILD;  
```

## <a name="examples-columnstore-indexes"></a>Примеры: Индексы columnstore  
 Эти примеры относятся к индексам columnstore.  
  
### <a name="a-reorganize-demo"></a>A. Демонстрация REORGANIZE  
 В этом примере показана работа команды ALTER INDEX REORGANIZE.  В нем создается таблица с несколькими группами строк и показано использование команды REORGANIZE для объединения этих групп строк.  
  
```sql  
-- Create a database   
CREATE DATABASE [ columnstore ];  
GO  
  
-- Create a rowstore staging table  
CREATE TABLE [ staging ] (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey     int  
     )  
  
-- Insert 10 million rows into the staging table.   
DECLARE @loop int  
DECLARE @AccountDescription varchar(50)  
DECLARE @AccountKey int  
DECLARE @AccountType varchar(50)  
DECLARE @AccountCode int  
  
SELECT @loop = 0  
BEGIN TRAN  
    WHILE (@loop < 300000)   
      BEGIN  
        SELECT @AccountKey = CAST (RAND()*10000000 as int);  
        SELECT @AccountDescription = 'accountdesc ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountType = 'AccountType ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountCode =  CAST (RAND()*10000000 as int);  
  
        INSERT INTO  staging VALUES (@AccountKey, @AccountDescription, @AccountType, @AccountCode);  
  
        SELECT @loop = @loop + 1;  
    END  
COMMIT  
  
-- Create a table for the clustered columnstore index  
  
CREATE TABLE cci_target (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey int  
     )  
  
-- Convert the table to a clustered columnstore index named inxcci_cci_target;  
CREATE CLUSTERED COLUMNSTORE INDEX idxcci_cci_target ON cci_target;  
```  
  
 Используйте параметр TABLOCK для параллельной вставки строк. Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], операция INSERT INTO может выполняться параллельно с TABLOCK.  
  
```sql  
INSERT INTO cci_target WITH (TABLOCK) 
SELECT TOP 300000 * FROM staging;  
```  
  
 Выполните эту команду, чтобы увидеть разностные группы строк OPEN. Количество групп строк зависит от уровня параллелизма.  
  
```sql  
SELECT *   
FROM sys.dm_db_column_store_row_group_physical_stats   
WHERE object_id  = object_id('cci_target');  
```  
  
 Выполните эту команду для принудительной отправки всех групп строк CLOSED и OPEN в columnstore.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
 Выполните эту команду еще раз, и вы увидите, что небольшие группы строк объединены в одну сжатую группу строк.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="b-compress-closed-delta-rowgroups-into-the-columnstore"></a>Б. Сжатие разностных групп строк CLOSED в columnstore  
 В этом примере используется параметр REORGANIZE для сжатия каждой разностной группы строк CLOSED в columnstore в виде сжатой группы строк.   Это делать необязательно, но полезно, когда процесс перемещения кортежей сжимает группы строк CLOSED недостаточно быстро.  
  
```sql  
-- Uses AdventureWorksDW  
-- REORGANIZE all partitions  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
  
-- REORGANIZE a specific partition  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0;  
```  
  
### <a name="c-compress-all-open-and-closed-delta-rowgroups-into-the-columnstore"></a>В. Сжатие всех разностных групп строк OPEN и CLOSED в columnstore  
 **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
 Команда REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON) сжимает каждую разностную группу строк OPEN и CLOSED в columnstore как сжатую группу строк. При этом очищается хранилище deltastore и все строки принудительно сжимаются в columnstore. Это особенно полезно после выполнения множества операций вставки, так как они хранят строки в одном или нескольких разностных группах строк.  
  
 REORGANIZE объединяет группы строк для заполнения групп строк до максимального числа строк \<= 1 024 576. Таким образом, при сжатии всех групп строк OPEN и CLOSED у вас не будет большого количества сжатых групп строк, содержащих небольшое количество строк. Чтобы сократить размер в сжатом виде и повысить производительность запросов, группы строк следует заполнить как можно плотнее.  
  
```sql  
-- Uses AdventureWorksDW2016  
-- Move all OPEN and CLOSED delta rowgroups into the columnstore.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
  
-- For a specific partition, move all OPEN AND CLOSED delta rowgroups into the columnstore  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0 WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="d-defragment-a-columnstore-index-online"></a>Г. Дефрагментация индекса columnstore в режиме "в сети"  
 Не применяется к [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], REORGANIZE позволяет не только сжимать разностные групп строк в columnstore. Эта команда также выполняет дефрагментацию в режиме "в сети". Сначала она уменьшает размер columnstore путем физического удаления удаленных строк, если в группе строк было удалено 10 % или более строк.  Затем она объединяет группы строк для формирования более крупных групп, каждая из которых может содержать до 1 024 576 строк.  Все измененные группы строк проходят повторное сжатие.  
  
> [!NOTE]
> Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], в большинстве случаев больше не требуется перестраивать индекса columnstore, так как REORGANIZE физически удаляет удаленные строки и объединяет группы строк. Параметр COMPRESS_ALL_ROW_GROUPS принудительно отправляет все разностные группы строк OPEN или CLOSED в columnstore. Ранее это можно было сделать только с помощью перестроения. REORGANIZE работает в режиме "в сети" и выполняется в фоне, поэтому запросы могут продолжаться по мере выполнения операции.  
  
```sql  
-- Uses AdventureWorks  
-- Defragment by physically removing rows that have been logically deleted from the table, and merging rowgroups.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
```  
  
### <a name="e-rebuild-a-clustered-columnstore-index-offline"></a>Д. Перестроение кластеризованного индекса columnstore в режиме "вне сети"  
Область применения: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])   
  
> [!TIP]
> Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], рекомендуется использовать инструкцию ALTER INDEX REORGANIZE вместо инструкции ALTER INDEX REBUILD.  
  
> [!NOTE]
> В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] REORGANIZE используется только для сжатия групп строк CLOSED в columnstore. Единственным способом выполнения операций дефрагментация и принудительной отправки всех разностных групп строк в columnstore является перестроение индекса.  
  
 В этом примере показано, как перестроить кластеризованный индекс columnstore и принудительно отправить все разностные группы строк в columnstore. В этом первом шаге подготавливается таблица FactInternetSales2 с кластеризованным индексом columnstore и происходит вставка данных из первых четырех столбцов.  
  
```sql  
-- Uses AdventureWorksDW  
  
CREATE TABLE dbo.FactInternetSales2 (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_FactInternetSales2  
ON dbo.FactInternetSales2;  
  
INSERT INTO dbo.FactInternetSales2  
SELECT ProductKey, OrderDateKey, DueDateKey, ShipDateKey  
FROM dbo.FactInternetSales;  
  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Как здесь показано, результатом становится одна группа строк OPEN, а это означает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет ожидать добавления большего количества строк и только после этого закроет группу строк и переместит данные в columnstore. Следующая инструкция перестраивает кластеризованный индекс columnstore, что приводит к принудительной отправке всех строк в columnstore.  
  
```sql  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REBUILD;  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Результаты инструкции SELECT показывают, что группа строк имеет атрибут COMPRESSED, а это означает, что сегменты столбца этой группы строк теперь упакованы и хранятся в columnstore.  
  
### <a name="f-rebuild-a-partition-of-a-clustered-columnstore-index-offline"></a>Е. Перестроение секции кластеризованного индекса columnstore в режиме "вне сети"  
 **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  
 
 Для перестроения секции большого кластеризованного индекса columnstore используйте инструкцию ALTER INDEX REBUILD с параметром секции. В этом примере перестраивается секция 12. Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], рекомендуется заменить REBUILD на REORGANIZE.  
  
```sql  
ALTER INDEX cci_fact3   
ON fact3  
REBUILD PARTITION = 12;  
```  
  
### <a name="g-change-a-clustered-columstore-index-to-use-archival-compression"></a>Ж. Изменение кластеризованного индекса columnstore для использования архивного сжатия  
 Не применяется к [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 Используя параметр сжатия данных COLUMNSTORE_ARCHIVE, можно еще больше уменьшить размер кластеризованного индекса columnstore. Это целесообразно для более старых данных, которые будут храниться в дешевом хранилище. Рекомендуется применять этот параметр только для редко используемых данных, поскольку распаковка выполняется медленнее, чем обычное сжатие COLUMNSTORE.  
  
 В следующем примере перестраивается кластеризованный индекс columnstore в целях применения архивного сжатия, затем показано, как удалить архивное сжатие. Конечным результатом становится использование только сжатия columnstore.  
  
```sql  
--Prepare the example by creating a table with a clustered columnstore index.  
CREATE TABLE SimpleTable (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL  
);  
  
CREATE CLUSTERED INDEX cci_SimpleTable ON SimpleTable (ProductKey);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_SimpleTable  
ON SimpleTable  
WITH (DROP_EXISTING = ON);  
  
--Compress the table further by using archival compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE);  
  
--Remove the archive compression and only use columnstore compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE);  
GO  
```  
  
## <a name="examples-rowstore-indexes"></a>Примеры: индексы rowstore  
  
### <a name="a-rebuilding-an-index"></a>A. Перестроение индекса  
 В следующем примере показано, как перестроить единственный индекс на таблице `Employee` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX PK_Employee_EmployeeID ON HumanResources.Employee REBUILD;  
```  
  
### <a name="b-rebuilding-all-indexes-on-a-table-and-specifying-options"></a>Б. Перестроение всех индексов по таблице и указание параметров  
 В следующем примере указывается ключевое слово ALL. Это приводит к перестроению всех индексов, связанных с таблицей Production.Product базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Указываются три параметра.  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH (FILLFACTOR = 80, SORT_IN_TEMPDB = ON, STATISTICS_NORECOMPUTE = ON);  
```  
  
В следующем примере добавляется параметр ONLINE, содержащий параметры блокировки с низким приоритетом, и добавляется параметр сжатия строк.  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH   
(  
    FILLFACTOR = 80,   
    SORT_IN_TEMPDB = ON,  
    STATISTICS_NORECOMPUTE = ON,  
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES, ABORT_AFTER_WAIT = BLOCKERS ) ),   
    DATA_COMPRESSION = ROW  
);  
```  
  
### <a name="c-reorganizing-an-index-with-lob-compaction"></a>В. Реорганизация индекса со сжатием данных LOB  
 В следующем примере показано, как реорганизовать единственный кластеризованный индекс в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Поскольку индекс содержит тип данных LOB на конечном уровне, инструкция также подвергает сжатию все страницы, в которых содержатся данные больших объектов. Следует отметить, что указывать параметр WITH (LOB_COMPACTION = ON) не требуется, так как значение по умолчанию — ON.  
  
```sql  
ALTER INDEX PK_ProductPhoto_ProductPhotoID ON Production.ProductPhoto REORGANIZE WITH (LOB_COMPACTION = ON);  
```  
  
### <a name="d-setting-options-on-an-index"></a>Г. Установка параметров для индекса  
 В следующем примере задается несколько параметров индекса `AK_SalesOrderHeader_SalesOrderNumber` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
ALTER INDEX AK_SalesOrderHeader_SalesOrderNumber ON  
    Sales.SalesOrderHeader  
SET (  
    STATISTICS_NORECOMPUTE = ON,  
    IGNORE_DUP_KEY = ON,  
    ALLOW_PAGE_LOCKS = ON  
    ) ;  
GO
```  
  
### <a name="e-disabling-an-index"></a>Д. Отключение индекса  
 В следующем примере показано отключение некластеризованного индекса на таблице `Employee` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX IX_Employee_ManagerID ON HumanResources.Employee DISABLE;
```  
  
### <a name="f-disabling-constraints"></a>Е. Отключение ограничений  
 В следующем примере показано отключение ограничения PRIMARY KEY путем отключения индекса PRIMARY KEY в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Ограничение FOREIGN KEY в базовой таблице автоматически отключается, и выводится предупредительное сообщение.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department DISABLE;  
```  
  
Результирующий набор возвращает это предупреждающее сообщение.  
  
```  
Warning: Foreign key 'FK_EmployeeDepartmentHistory_Department_DepartmentID'  
on table 'EmployeeDepartmentHistory' referencing table 'Department'  
was disabled as a result of disabling the index 'PK_Department_DepartmentID'.
```  
  
### <a name="g-enabling-constraints"></a>Ж. Включение ограничений  
 В следующем примере активируются ограничения PRIMARY KEY и FOREIGN KEY, снятые в примере Е.  
  
Ограничение PRIMARY KEY активируется путем перестройки индекса PRIMARY KEY.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department REBUILD;  
```  
  
Затем активируется ограничение FOREIGN KEY.  
  
```sql  
ALTER TABLE HumanResources.EmployeeDepartmentHistory  
CHECK CONSTRAINT FK_EmployeeDepartmentHistory_Department_DepartmentID;  
GO  
```  
  
### <a name="h-rebuilding-a-partitioned-index"></a>З. Перестроение секционированного индекса  
 В следующем примере перестраивается единственная секция с номером `5` секционированного индекса `IX_TransactionHistory_TransactionDate` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Секция 5 перестраивается в сети, 10 минут времени ожидания для блокировки с низким приоритетом применяется отдельно к каждой полученной блокировке операции перестроения индекса. Если в течение этого времени не удается получить блокировку для завершения перестроения индекса, инструкция по перестроению прерывается.  
  
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
-- Verify the partitioned indexes.  
SELECT *  
FROM sys.dm_db_index_physical_stats (DB_ID(),OBJECT_ID(N'Production.TransactionHistory'), NULL , NULL, NULL);  
GO  
--Rebuild only partition 5.  
ALTER INDEX IX_TransactionHistory_TransactionDate  
ON Production.TransactionHistory  
REBUILD Partition = 5   
   WITH (ONLINE = ON (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 10 minutes, ABORT_AFTER_WAIT = SELF)));  
GO  
```  
  
### <a name="i-changing-the-compression-setting-of-an-index"></a>И. Изменение настроек сжатия индекса  
 В следующем примере перестраивается индекс на несекционированной таблице rowstore.  
  
```sql
ALTER INDEX IX_INDEX1   
ON T1  
REBUILD   
WITH (DATA_COMPRESSION = PAGE);  
GO  
```  
  
Дополнительные примеры сжатия данных см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).  
 
### <a name="j-online-resumable-index-rebuild"></a>К. Возобновляемое перестроение индексов в режиме "в сети"

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

 В следующих примерах показано использование возобновляемого перестроения индексов в режиме "в сети". 

1. Выполните перестроение индекса в режиме "в сети" в качестве возобновляемой операции с параметром MAXDOP = 1.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, MAXDOP=1, RESUMABLE=ON) ;
   ```

2. При повторном выполнении этой команды (см. выше) после приостановки операции индекса автоматически возобновляется операция перестроения индекса.

3. Выполните перестроение индекса в режиме "в сети" в качестве возобновляемой операции с параметром MAX_DURATION = 240.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, RESUMABLE=ON, MAX_DURATION=240) ; 
   ```
4. Приостановите выполняющееся возобновляемое перестроение индексов в режиме "в сети".

   ```sql
   ALTER INDEX test_idx on test_table PAUSE ;
   ```   
5. Возобновите перестроение индекса в режиме "в сети" для перестроения индекса, который был выполнен как возобновляемая операция, указав новое значение для MAXDOP, равное 4.

   ```sql
   ALTER INDEX test_idx on test_table RESUME WITH (MAXDOP=4) ;
   ```
6. Возобновите операцию перестроения индекса в режиме "в сети" для перестроения индекса в режиме "в сети", которое было выполнено как возобновляемое. Задайте параметру MAXDOP значение 2, установите время для выполняющегося в качестве возобновляемого индекса равным 240 минутам, а в случае блокировки индекса задайте время ожидания 10 минут, после чего все блокировщики должны быть удалены. 

   ```sql
      ALTER INDEX test_idx on test_table  
         RESUME WITH (MAXDOP=2, MAX_DURATION= 240 MINUTES, 
         WAIT_AT_LOW_PRIORITY (MAX_DURATION=10, ABORT_AFTER_WAIT=BLOCKERS)) ;
   ```      
7. Прервите возобновляемую операцию перестроения индекса, которая выполняется или приостановлена.

   ```sql
   ALTER INDEX test_idx on test_table ABORT ;
   ``` 
  
## <a name="see-also"></a>См. также:  
[Руководство по архитектуре и разработке индексов SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Выполнение операций с индексами в режиме "в сети"](../../relational-databases/indexes/perform-index-operations-online.md)    
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)     
[CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)   
[CREATE XML INDEX (Transact-SQL)](../../t-sql/statements/create-xml-index-transact-sql.md)   
[DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)   
[Отключение индексов и ограничений](../../relational-databases/indexes/disable-indexes-and-constraints.md)   
[XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)   
[Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)   
[sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
[EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)    
