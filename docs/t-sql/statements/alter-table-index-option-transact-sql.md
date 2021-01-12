---
description: ALTER TABLE index_option (Transact-SQL)
title: " | Документы Майкрософт"
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- index_option
ms.assetid: 8a14f12d-2fbf-4036-b8b2-8db3354e0eb7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6089ef60d4f0c4fc6338404084140f72fa3a3a19
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094707"
---
# <a name="alter-table-index_option-transact-sql"></a>ALTER TABLE index_option (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Определяет набор параметров, которые могут применяться к индексу, являющемуся частью определения ограничения, созданному с помощью инструкции [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
{   
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | IGNORE_DUP_KEY = { ON | OFF }  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF } 
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF } 
  | SORT_IN_TEMPDB = { ON | OFF }   
  | ONLINE = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = { NONE |ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }   
      [ , ...n ] ) ]  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<range> ::=   
<partition_number_expression> TO <partition_number_expression>  
  
<single_partition_rebuild__option> ::=  
{  
    SORT_IN_TEMPDB = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = {NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                           ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 PAD_INDEX **=** { ON | **OFF** }  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Определяет разреженность индекса. Значение по умолчанию — OFF.  
  
 ON  
 Процент свободного места, определяемый параметром FILLFACTOR, применяется к страницам индекса промежуточного уровня.  
  
 OFF или *fillfactor* не указан  
 Страницы промежуточного уровня заполнены почти в соответствии их производительным возможностям, на них остается место как минимум для одной строки максимального размера, какой только может иметь индекс, с заданным набором ключей на промежуточных страницах.  
  
 FILLFACTOR **=** _fillfactor_  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Определяет величину в процентах, показывающую насколько должен компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] заполнять конечный уровень каждой страницы индекса во время его создания и изменения. Заданное значение должно быть целым числом от 1 до 100. Значение по умолчанию равно 0.  
  
> [!NOTE]  
>  Значения коэффициента заполнения 0 и 100 одинаковы во всех отношениях.  
  
 IGNORE_DUP_KEY **=** { ON | **OFF** }  
 Определяет тип ответа, когда операция вставки пытается вставить в уникальный индекс повторяющиеся значения ключа. Параметр IGNORE_DUP_KEY применяется только к операциям вставки, производимым после создания или перестроения индекса. Параметр не работает во время выполнения инструкции [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) или [UPDATE](../../t-sql/queries/update-transact-sql.md). Значение по умолчанию — OFF.  
  
 ON  
 Если в уникальный индекс вставляются повторяющиеся значения ключа, выводится предупреждающее сообщение. С ошибкой завершаются только строки, нарушающие ограничение уникальности.  
  
 OFF  
 Если в уникальный индекс вставляются повторяющиеся значения ключа, выводится сообщение об ошибке. Выполняется откат всей операции INSERT.  
  
 IGNORE_DUP_KEY нельзя установить в значение ON для индексов, создаваемых для представлений, неуникальных индексов, XML-индексов, пространственных индексов и фильтруемых индексов.  
  
 Для просмотра значения IGNORE_DUP_KEY используйте [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Для обратной совместимости синтаксиса аргумент WITH IGNORE_DUP_KEY эквивалентен аргументу WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | **OFF** }  
 Указывает, выполнялся ли перерасчет статистики. Значение по умолчанию — OFF.  
  
 ON  
 Устаревшие статистики не пересчитываются автоматически.  
  
 OFF  
 Автоматическое обновление статистических данных включено.  
  
 ALLOW_ROW_LOCKS **=** { **ON** | OFF }  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает, разрешена ли блокировка строк. Значение по умолчанию — ON.  
  
 ON  
 Блокировки строк допустимы при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки строки.  
  
 OFF  
 Блокировки строк не используются.  
  
 ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает, разрешена ли блокировка страниц. Значение по умолчанию — ON.  
  
 ON  
 Блокировки страниц возможны при доступе к индексу. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяет, когда используются блокировки страниц.  
  
 OFF  
 Блокировки страниц не используются.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }

**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и более поздних версий.

Определяет, следует ли выполнять оптимизацию, связанную с состязанием при операциях вставки на последнюю страницу. Значение по умолчанию — OFF. См. подробнее раздел о [последовательных ключах](./create-index-transact-sql.md#sequential-keys) в документации по CREATE INDEX.
 
 SORT_IN_TEMPDB **=** { ON | **OFF** }  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает, сохраняются ли результаты сортировки в базе данных **tempdb**. Значение по умолчанию — OFF.  
  
 ON  
 Промежуточные результаты сортировки, которые используются при индексировании, хранятся в базе данных **tempdb**. Это может уменьшить время, необходимое для создания индекса, если база данных **tempdb** и база данных пользователя находятся на разных наборах дисков. Однако это увеличивает использование места на диске, которое используется при индексировании.  
  
 OFF  
 Промежуточные результаты сортировки хранятся в той же базе данных, где и индекс.  
  
 ONLINE **=** { ON | **OFF** }  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Определяет, будут ли базовые таблицы и связанные индексы доступны для запросов и изменения данных во время операций с индексами. Значение по умолчанию — OFF. REBUILD может выполняться как операция в режиме ONLINE.  
  
> [!NOTE]  
>  Уникальные некластеризованные индексы нельзя создавать в режиме в сети. К ним относятся индексы, создаваемые из-за ограничений UNIQUE и PRIMARY KEY.  
  
 ON  
 Долгосрочные блокировки таблицы не поддерживаются во время операций с индексами. Во время главной фазы операций с индексами только блокировка с намерением совмещаемого доступа (IS) удерживается в исходной таблице. Это включает запросы или обновления применительно к обрабатываемой базовой таблице и индексам. В начале операции совмещаемая блокировка (S) удерживается на исходном объекте в течение очень короткого времени. В конце операции на источнике на короткое время удерживается совмещаемая блокировка (S), если создается некластеризованный индекс. Если в режиме в сети создается или удаляется кластеризованный индекс и, если перестраивается кластеризованный или некластеризованный индекс, удерживается блокировка SCH-M (изменения схемы). Хотя блокировки индекса в сети — это короткие блокировки метаданных, но блокировка изменения схемы (Sch-M) должна ожидать завершения всех блокирующих транзакций для этой таблицы. Во время ожидания Sch-M блокирует все другие транзакции, ожидающие за этой блокировкой доступа к одной таблице. При создании индекса для временной локальной таблицы параметр ONLINE не может принимать значение ON.  
  
> [!NOTE]  
>  Перестроение индекса в режиме "в сети" может задать параметры *low_priority_lock_wait*, описанные ниже в этом разделе. *low_priority_lock_wait* управляет приоритетом блокировки S и Sch-M во время операции перестроения индекса в режиме "в сети".  
  
 OFF  
 Блокировки таблиц применяются во время выполнения операций с индексами. Это предотвращает доступ к базовой таблице всех пользователей во время операции. Блокировку изменения схемы (Sch-M) в таблице получает операция с индексами вне сети, которая создает, перестраивает или удаляет кластеризованный индекс либо перестраивает или удаляет некластеризованный индекс. Это предотвращает доступ к базовой таблице всех пользователей во время операции. Операция с индексами вне сети, создающая некластеризованный индекс, получает совмещаемую блокировку (S) в таблице. Это запрещает проводить обновления базовой таблицы, но разрешает проводить операции чтения, например инструкции SELECT.  
  
 Дополнительные сведения см. в разделе [Об операциях с индексами в режиме "в сети"](../../relational-databases/indexes/how-online-index-operations-work.md).  
  
> [!NOTE]
>  Операции с индексами в сети доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 MAXDOP **=** _max_degree_of_parallelism_  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Переопределяет параметр конфигурации **max degree of parallelism** на время выполнения операции с индексами. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
 Параметр *max_degree_of_parallelism* может иметь одно из следующих значений:  
  
 - 1 — подавляет формирование параллельных планов;  
 - \>1 — ограничивает указанным значением максимальное число процессоров, используемых для параллельных операций с индексами;  
 - 0 (по умолчанию) — в зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
 Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 DATA_COMPRESSION  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Задает режим сжатия данных для указанной таблицы, номера секции или диапазона секций. Существуют следующие варианты выбора.  
  
 None  
 Таблица или указанные секции не сжимаются. Применяется только к таблицам rowstore, не относится к таблицам columnstore.  
  
 ROW  
 Таблицы или указанные секции сжимаются, используя сжатие строк. Применяется только к таблицам rowstore, не относится к таблицам columnstore.  
  
 PAGE  
 Таблицы или указанные секции сжимаются, используя сжатие страниц. Применяется только к таблицам rowstore, не относится к таблицам columnstore.  
  
 COLUMNSTORE  
 **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
 Применяется только к таблицам columnstore. COLUMNSTORE указывает, что должна быть распакована секция, которая была упакована с помощью параметра COLUMNSTORE_ARCHIVE. При восстановлении данных сжатие индекса COLUMNSTORE будет продолжаться с применением сжатия columnstore, предусмотренного для всех таблиц columnstore.  
  
 COLUMNSTORE_ARCHIVE  
 **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
 Применяется только к таблицам columnstore, представляющим собой таблицы, которые хранятся с кластеризованным индексом columnstore. Параметр COLUMNSTORE_ARCHIVE обеспечивает дальнейшее сжатие указанной секции до еще меньшего размера. Это может использоваться для архивации или в других ситуациях, где требуется уменьшение объема пространства и допускается увеличение затрат времени на хранение и извлечение.  
  
 Дополнительные сведения о сжатии см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).  
  
ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...*n* ] **)** **Применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.  
  
 Указывает секции, к которым применяется параметр DATA_COMPRESSION. Если таблица не секционирована, аргумент ON PARTITIONS проводит к формированию ошибки. Если не указано предложение ON PARTITIONS, то параметр DATA_COMPRESSION применяется ко всем секциям секционированной таблицы.  
  
\<partition_number_expression> можно указать одним из следующих способов.  
  
-   Указав номер секции, например: ON PARTITIONS (2).  
-   Указав номера нескольких секций, разделив их запятыми, например ON PARTITIONS (1, 5).  
-   Указав диапазоны секций и отдельные секции, например ON PARTITIONS (2, 4, 6 TO 8).  
  
\<range> можно указать номерами секций, разделенными ключевым словом TO, например: ON PARTITIONS (6 TO 8).  
  
 Чтобы для разных секций задать разные типы сжатия данных, укажите параметр DATA_COMPRESSION несколько раз, например следующим образом.  
  
```sql  
--For rowstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
)  
  
--For columnstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = COLUMNSTORE ON PARTITIONS (1, 3, 5),   
DATA_COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (2, 4, 6 TO 8)  
)  
```  
  
**\<single_partition_rebuild__option>**  
 В большинстве случаев при перестроении индекса перестраиваются все секции секционированного индекса. Следующие параметры при применении к одному разделу не перестраивают все секции.  
  
-   SORT_IN_TEMPDB  
-   MAXDOP  
-   DATA_COMPRESSION  
  
**low_priority_lock_wait**  
 **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
 **SWITCH** или перестроение индекса в сети завершается, как только не остается блокирующих таблицу операций. *WAIT_AT_LOW_PRIORITY* указывает, что если операция **SWITCH** или перестроение индекса в сети не может быть выполнено немедленно, то эти операции будут переведены в режим ожидания. Операция удерживает блокировки с низким приоритетом, чтобы другие операции, удерживающие блокировки, конфликтующие с инструкцией DDL, могли выполняться дальше. Пропуск параметра **WAIT AT LOW PRIORITY** эквивалентен `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.  
  
MAX_DURATION = *time* [**MINUTES** ]  
 Время ожидания (целочисленное значение, указанное в минутах) при выполнении команды DDL для операции **SWITCH** или получаемой блокировки по операции перестроения индекса в режиме "в сети". Будет выполнена попытка немедленного запуска операции SWITCH или перестроения индекса в режиме «в сети». Если операция будет заблокирована на время **MAX_DURATION**, будет выполнено одно из действий **ABORT_AFTER_WAIT**. Время **MAX_DURATION** всегда указывается в минутах, и слово **MINUTES** можно опустить.  
  
ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
 None  
 Продолжение операции **SWITCH** или перестроение индекса в сети без изменения приоритета блокировки (с помощью обычного приоритета).  
  
SELF  
 Прекращение операции **SWITCH** или DDL по перестроению индекса в сети, выполняемой в данный момент, без какого-либо действия.  
  
BLOCKERS  
 Остановка всех пользовательских транзакций, в данный момент блокирующих операцию **SWITCH** или операцию DDL по перестроению индекса в сети, чтобы можно было продолжить данную операцию.  
 BLOCKERS необходимо разрешение **ALTER ANY CONNECTION**.  
  
## <a name="remarks"></a>Remarks  
 Полное описание параметров индекса см. в разделе [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_constraint (Transact-SQL)](../../t-sql/statements/alter-table-column-constraint-transact-sql.md)   
 [computed_column_definition (Transact-SQL)](../../t-sql/statements/alter-table-computed-column-definition-transact-sql.md)   
 [table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  
  
 
