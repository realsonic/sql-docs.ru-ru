---
description: Указания (Transact-SQL) — запросы
title: Указания запросов (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Query_Hint_TSQL
- Query_TSQL
- Query
- Query Hint
- MAX_GRANT_PERCENT
- MAX_GRANT_PERCENT_TSQL
- MIN_GRANT_PERCENT
- MIN_GRANT_PERCENT_TSQL
- EXTERNALPUSHDOWN
- EXTERNALPUSHDOWN_TSQL
- NOLOCK_TSQL
- MAXDOP_TSQL
- USE_HINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REPORT PLAN query hint
- FORCE ORDER query hint
- HASH JOIN query hint
- query hints [SQL Server]
- OPTIMIZE FOR query hint
- FORCESCAN query hint
- RECOMPILE query hint
- MAXRECURSION query hint
- MERGE JOIN query hint
- HASH GROUP query hint
- KEEP PLAN query hint
- UNION query hint
- FORCESEEK query hint
- ORDER GROUP query hint
- LOOP JOIN query hint
- KEEPFIXED PLAN query hint
- FAST query hint
- MAXDOP query hint
- PARAMETERIZATION query hint
- hints [SQL Server], query
- JOIN query hint
- USE PLAN query hint
- EXPAND VIEWS query hint
- MAX_GRANT_PERCENT query hint
- MIN_GRANT_PERCENT query hint
- EXTERNALPUSHDOWN query hint
- USE HINT query hint
- QUERY_PLAN_PROFILE query hint
ms.assetid: 66fb1520-dcdf-4aab-9ff1-7de8f79e5b2d
author: pmasl
ms.author: pelopes
ms.openlocfilehash: eda240576ebb819e387e3fb93d55a06b6a1c1d9e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102386"
---
# <a name="hints-transact-sql---query"></a>Указания (Transact-SQL) — запросы
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Указания запросов определяют, что выбранные указания используются в области запроса. Они влияют на все операторы в инструкции. Если в основном запросе используется операция UNION, только последний запрос, использующий ее, может содержать предложение OPTION. Подсказки в запросе указываются как часть предложения [OPTION](../../t-sql/queries/option-clause-transact-sql.md). Если оптимизатор запросов не сформирует допустимый план из-за одного или нескольких указаний запроса, возникает ошибка 8622.  
  
> [!CAUTION]  
> Поскольку оптимизатор запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обычно выбирает лучший план выполнения запроса, использовать подсказки рекомендуется только опытным разработчикам и администраторам баз данных в самом крайнем случае.  
  
**Применимо к:**  
  
[DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
[INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
[SELECT](../../t-sql/queries/select-transact-sql.md)  
  
[UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
[MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
<query_hint> ::=   
{ { HASH | ORDER } GROUP   
  | { CONCAT | HASH | MERGE } UNION   
  | { LOOP | MERGE | HASH } JOIN   
  | EXPAND VIEWS   
  | FAST <integer_value>   
  | FORCE ORDER   
  | { FORCE | DISABLE } EXTERNALPUSHDOWN
  | { FORCE | DISABLE } SCALEOUTEXECUTION
  | IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX  
  | KEEP PLAN   
  | KEEPFIXED PLAN  
  | MAX_GRANT_PERCENT = <numeric_value>  
  | MIN_GRANT_PERCENT = <numeric_value>  
  | MAXDOP <integer_value>   
  | MAXRECURSION <integer_value>   
  | NO_PERFORMANCE_SPOOL   
  | OPTIMIZE FOR ( @variable_name { UNKNOWN | = <literal_constant> } [ , ...n ] )  
  | OPTIMIZE FOR UNKNOWN  
  | PARAMETERIZATION { SIMPLE | FORCED }   
  | QUERYTRACEON <integer_value>   
  | RECOMPILE  
  | ROBUST PLAN   
  | USE HINT ( <use_hint_name> [ , ...n ] )
  | USE PLAN N'<xml_plan>'  
  | TABLE HINT ( <exposed_object_name> [ , <table_hint> [ [, ]...n ] ] )  
}  
  
<table_hint> ::=  
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ]  
  | FORCESCAN  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT  
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK  
}  

<use_hint_name> ::=
{ 'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS'
  | 'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES'
  | 'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'
  | 'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_DEFERRED_COMPILATION_TV'
  | 'DISABLE_INTERLEAVED_EXECUTION_TVF'
  | 'DISABLE_OPTIMIZED_NESTED_LOOP'
  | 'DISABLE_OPTIMIZER_ROWGOAL'
  | 'DISABLE_PARAMETER_SNIFFING'
  | 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_TSQL_SCALAR_UDF_INLINING'
  | 'DISALLOW_BATCH_MODE'
  | 'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'
  | 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'
  | 'FORCE_DEFAULT_CARDINALITY_ESTIMATION'
  | 'FORCE_LEGACY_CARDINALITY_ESTIMATION'
  | 'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'
  | 'QUERY_PLAN_PROFILE' 
}
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
{ HASH |ORDER } GROUP  
Указывает, что агрегаты, описываемые в предложениях GROUP BY или DISTINCT в запросе, должны использовать хэширование или упорядочивание.  
  
{ MERGE | HASH | CONCAT } UNION  
Указывает, что все операции UNION выполняются путем слияния, хэширования или объединения наборов UNION. Если задано несколько указаний UNION, оптимизатор запросов выбирает наименее затратную стратегию из указанных.  
  
{ LOOP | MERGE | HASH } JOIN  
Указывает, что все операции соединения во всем запросе выполняются с помощью LOOP JOIN, MERGE JOIN или HASH JOIN. Если задано больше одного указания соединения, оптимизатор запросов выбирает наименее затратную стратегию из допустимых.  
  
Если в предложении FROM для определенной пары таблиц в том же запросе есть указание соединения, оно имеет приоритет при соединении двух таблиц. Но указания запроса при этом все равно должны соблюдаться. Указание соединения для пары таблиц может только ограничивать выбор методов соединения, разрешенных в указании запроса. Дополнительные сведения см. в разделе [Указания соединений (Transact-SQL)](../../t-sql/queries/hints-transact-sql-join.md).  
  
EXPAND VIEWS  
Указывает, что индексированные представления разворачиваются. Также указывает, что оптимизатор запросов не будет рассматривать индексированные представления в качестве замены для любой части запроса. Представление разворачивается при замене имени представления на определение представления в тексте запроса.  
  
Это указание запроса виртуально запрещает прямое использование индексированных представлений и индексов для индексированных представлений в плане запроса.  
  
> [!NOTE]
> Индексированное представление сохраняет сокращенный вид, если на это представление есть прямая ссылка в части SELECT запроса. Представление также останется сокращенным, если указаны предложения WITH (NOEXPAND) или WITH (NOEXPAND, INDEX( _<index\_value>_ [ **,** _...n_ ] ) ). Дополнительные сведения об указании запроса NOEXPAND см. в разделе [Использование NOEXPAND](../../t-sql/queries/hints-transact-sql-table.md#using-noexpand).  
  
Это указание действует только на представления в части SELECT инструкции, в том числе в инструкциях INSERT, UPDATE, MERGE и DELETE.  
  
FAST _<integer\_value>_  
Определяет, что запрос оптимизирован для быстрого получения первых строк ( _<integer\_value>_ ). Это неотрицательное целое число. После возврата первых строк ( _<integer\_value>_ ) запрос продолжает выполняться и возвращает полный результирующий набор.  
  
FORCE ORDER  
Указывает, что при оптимизации запроса сохраняется порядок соединения, заданный синтаксисом запроса. Использование FORCE ORDER не влияет на возможный реверс ролей в оптимизаторе запросов.  
  
> [!NOTE]  
> Инструкция MERGE получает доступ вначале к исходной таблице, затем к целевой в порядке соединения, принятом по умолчанию, если не задано предложение WHEN SOURCE NOT MATCHED. Если указать FORCE ORDER, сохраняется поведение по умолчанию.  
  
{ FORCE | DISABLE } EXTERNALPUSHDOWN  
Принудительная передача или отключение передачи вычислений соответствующих выражений в Hadoop. Применяется только к запросам, использующим PolyBase. Не выполняет отправку в хранилище Azure.  

{ FORCE | DISABLE } SCALEOUTEXECUTION Принудительное применение или отключение горизонтального увеличения масштаба выполнения запросов Polybase с использованием внешних таблиц в кластерах больших данных SQL Server 2019. Это указание будет учитываться только запросом, использующим главный экземпляр кластера больших данных SQL. Горизонтальное увеличение масштаба будет выполняться в пределах пула вычислений кластера больших данных. 

KEEP PLAN  
Заставляет оптимизатор запросов снизить приблизительный порог повторной компиляции для запроса. Предполагаемое пороговое значение повторной компиляции запускает автоматическую перекомпиляцию запроса, если в таблице изменилось ожидаемое количество индексированных столбцов при выполнении одной из следующих инструкций.

* UPDATE
* DELETE
* MERGE
* INSERT

Указание KEEP PLAN гарантирует, что запрос не будет часто перекомпилироваться при выполнении множественных обновлений в таблице.  
  
KEEPFIXED PLAN  
Принуждает оптимизатор запросов не перекомпилировать запрос при изменении статистики. Указание KEEPFIXED PLAN гарантирует, что запрос будет перекомпилирован только при изменении схемы базовых таблиц или при выполнении для них процедуры **sp_recompile**.  
  
IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX       
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  
  
Предотвращает использование в запросе некластеризованного индекса columnstore с оптимизацией для памяти. Если в запросе содержится указание запроса, исключающее использование индекса columnstore, а также указание индекса для использования индекса columnstore, то данные указания будут конфликтовать между собой, и запрос вернет ошибку.  
  
MAX_GRANT_PERCENT = <numeric_value>     
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

Максимальный объем предоставленной памяти (PERCENT) от заданного ограничения. Запрос гарантированно не превышает это ограничение. Реальное ограничение может быть ниже, если значение параметра Resource Governor ниже значения в этом указании. Допустимые значения — от 0 до 100.  
  
MIN_GRANT_PERCENT = <numeric_value>        
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

Минимальный объем предоставленной памяти (PERCENT) от заданного ограничения. Запрос гарантированно получает `MAX(required memory, min grant)`, так как для запуска запроса требуется определенный минимальный объем памяти. Допустимые значения — от 0 до 100.  
 
MAXDOP <integer_value>      
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Переопределяет параметр конфигурации, задающий **максимальный уровень параллелизма**, в **sp_configure**. Также переопределяет Resource Governor для запроса, в котором указан этот параметр. Указание запроса MAXDOP может превысить значение, настроенное с помощью sp_configure. Если MAXDOP превышает значение, настроенное с помощью Resource Governor, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] использует значение MAXDOP из Resource Governor, как описано в статье [ALTER WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md). Все семантические правила, используемые параметром конфигурации **max degree of parallelism**, применимы при использовании подсказки в запросе MAXDOP. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!WARNING]     
> Если значение MAXDOP равно нулю, сервер выбирает максимальную степень параллелизма.  
  
MAXRECURSION <integer_value>     
Указывает максимальное число рекурсий, допустимых для данного запроса. _number_ представляет собой неотрицательное целое число от 0 до 32 767. Если указано значение 0, ограничения не применяются. Если этот параметр не указан, для сервера используется ограничение по умолчанию 100.  
  
Если в процессе выполнения запроса достигнут указанный уровень MAXRECURSION (или уровень по умолчанию), выполнение запроса завершается и возвращается ошибка.  
  
Из-за этой ошибки все действия инструкции откатываются. Если это инструкция SELECT, может быть возвращена часть результатов или не возвращено ничего. Любые возвращенные частичные результаты могут не включать всех строк на рекурсивных уровнях, расположенных за указанным максимальным уровнем рекурсии.  
  
Дополнительные сведения см. в разделе [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md).     
  
NO_PERFORMANCE_SPOOL    
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
  
Запрещает добавление оператора очередей в планы запроса (за исключением тех планов, когда очередь необходима для гарантированного обеспечения допустимой семантики обновления). В некоторых сценариях оператор очередей может снизить производительность. Например, очередь использует базу данных tempdb, и за нее может возникнуть состязание при наличии множества параллельных запросов, выполняющихся с операциями очереди.  
  
OPTIMIZE FOR ( _\@variable\_name_ { UNKNOWN | = <literal_constant> }_ [ **,** ..._n_ ] )     
Указывает оптимизатору запросов, что при компиляции и оптимизации запросов нужно использовать конкретное значение для локальной переменной. Значение используется только в процессе оптимизации запроса, но не в процессе выполнения.  
  
_\@variable\_name_  
Имя локальной переменной, используемой в запросе, которой может быть присвоено значение для использования с указанием запроса OPTIMIZE FOR.  
  
_UNKNOWN_  
Указывает, что оптимизатор запросов использует статистические данные вместо начального значения, чтобы определить значение локальной переменной при оптимизации запросов.  
  
_literal\_constant_  
Значению литеральной константы присваивается имя _\@variable\_name_ для использования в указании запроса OPTIMIZE FOR. _literal\_constant_ используется только в процессе оптимизации запроса, а не как значение _\@variable\_name_ во время выполнения запроса. _literal\_constant_ может иметь любой системный тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], который может быть выражен как литеральная константа. Тип данных значения _literal\_constant_ должен неявно преобразовываться в тип данных, на который ссылается аргумент _\@variable\_name_ в запросе.  
  
Указание OPTIMIZE FOR может изменить поведение оптимизатора по обнаружению параметра по умолчанию. При создании структур плана также используйте OPTIMIZE FOR. Дополнительные сведения см. в разделе [Перекомпиляция хранимой процедуры](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md).  
  
OPTIMIZE FOR UNKNOWN  
Указывает, что во время компиляции и оптимизации запроса оптимизатор запросов должен использовать среднюю избирательность предиката по всем значениям столбцов вместо значения параметра времени выполнения.  
  
Если вы используете OPTIMIZE FOR @variable_name = _literal\_constant_ и OPTIMIZE FOR UNKNOWN в одном указании запроса, оптимизатор запросов будет использовать указанный аргумент _literal\_constant_ для конкретного значения. Для всех остальных значений переменной оптимизатор запросов будет использовать вариант UNKNOWN. Значения используются только в процессе оптимизации запроса, но не в процессе выполнения.  

PARAMETERIZATION { SIMPLE | FORCED }     
Указывает правила параметризации, которые оптимизатор запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] применяет к запросу при его компиляции.  
  
> [!IMPORTANT]  
> Указание запроса PARAMETERIZATION может быть задано только в структуре плана для переопределения текущего значения параметра SET базы данных PARAMETERIZATION. Оно не может быть определено напрямую в запросе.    
> Дополнительные сведения см. в разделе [Указание механизма параметризации запросов с помощью структур плана](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).
  
Значение SIMPLE дает оптимизатору запросов указание использовать простую параметризацию. Значение FORCED предписывает оптимизатору запросов использовать принудительную параметризацию. Дополнительные сведения см. в разделах [Принудительная параметризация](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) и [Простая параметризация](../../relational-databases/query-processing-architecture-guide.md#SimpleParam) в руководстве по архитектуре обработки запросов.  

QUERYTRACEON <integer_value>    
Этот параметр позволяет включить флаг трассировки, влияющий на план, только во время компиляции с одним запросом. Как и другие параметры уровня запроса, его можно использовать вместе со структурами плана, чтобы обеспечить соответствие тексту запроса, выполняемого из любого сеанса, и автоматически применять флаг трассировки, влияющий на план, при компиляции этого запроса. Параметр QUERYTRACEON поддерживается только для флагов трассировки оптимизатора запросов. Дополнительные сведения см. в статье [о флагах трассировки](../database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). 

> [!NOTE]  
> Но этот параметр не возвращает сообщение об ошибке или предупреждение, если используется неподдерживаемый номер флага трассировки. Если указанный флаг трассировки не относится к плану выполнения запроса, параметр будет игнорироваться без уведомления.

> [!NOTE]  
> Чтобы использовать в запросе несколько флагов трассировки, укажите одно указание QUERYTRACEON для каждого номера флага трассировки.

RECOMPILE  
Указывает [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] создать временный план для запроса, который будет немедленного удален после выполнения запроса. Созданный план запроса не заменяет план, хранимый в кэше, когда тот же запрос выполняется без указания RECOMPILE. Без указания подсказки RECOMPILE компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] кэширует планы запросов и использует их повторно. При компиляции планов запроса указание запроса RECOMPILE использует в запросе текущие значения локальных переменных. Если запрос находится в хранимой процедуре, всем параметрам присваиваются текущие значения.  
  
RECOMPILE — полезная альтернатива созданию хранимой процедуры. RECOMPILE использует предложение WITH RECOMPILE в тех случаях, когда нужно перекомпилировать лишь часть запросов в хранимой процедуре, а не всю хранимую процедуру. Дополнительные сведения см. в разделе [Перекомпиляция хранимой процедуры](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md). Подсказка RECOMPILE также полезна для создания структур планов.  
  
ROBUST PLAN  
Заставляет оптимизатор запросов использовать план, который работает со строками наибольшего потенциального размера, возможно, с потерей производительности. При обработке запроса промежуточным таблицам и операторам может понадобиться сохранять и обрабатывать строки, которые шире, чем любые из входных строк. Строки могут быть настолько широки, что некоторые операторы не смогут их обработать. Для таких широких строк компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] возвращает ошибку при выполнении запроса. ROBUST PLAN сообщает оптимизатору запросов, что следует игнорировать все планы запросов, в которых может возникнуть эта проблема.  
  
Если такой план невозможен, оптимизатор запросов возвращает ошибку сразу, не откладывая обнаружение ошибок на момент выполнения запроса. Строки могут содержать столбцы переменной длины; компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] позволяет указать для строк максимальный потенциальный размер, при превышении которого компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] может не суметь обработать их. В основном, несмотря на максимальный потенциальный размер, приложение сохраняет строки, имеющие актуальные размеры с ограничениями, которые компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] может обработать. Если компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] встречает слишком длинную строку, возвращается ошибка выполнения.  
 
<a name="use_hint"></a> USE HINT ( **'** _hint\_name_ **'** )    
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 1 (SP1)) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
 
Предоставляет обработчику запросов одно или несколько дополнительных указаний. Дополнительные указания определяются именем указания **в одинарных кавычках**.   

Поддерживаются следующие имена подсказок:    
 
*  'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS' <a name="use_hint_join_containment"></a>       
   Побуждает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создать план запроса с допущением простого вложения вместо допущения базового вложения по умолчанию для соединений в модели [оценки кратности](../../relational-databases/performance/cardinality-estimation-sql-server.md) оптимизатора запросов версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] или более поздних. Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9476. 
*  'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES' <a name="use_hint_correlation"></a>      
   Заставляет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создавать план с минимальной избирательностью при оценке предикатов AND для фильтров в случае корреляции. Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4137 при использовании модели оценки кратности в [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более ранних версиях и имеет тот же эффект, что и при использовании [флага трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9471 с моделью оценки кратности в [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] или более поздних версиях.
*  "DISABLE_BATCH_MODE_ADAPTIVE_JOINS"       
   Отключает адаптивные соединения в пакетном режиме. Дополнительные сведения: [Адаптивные соединения в пакетном режиме](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins).     
   **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  "DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK"       
   Отключает обратную связь по временно предоставляемому буферу памяти в пакетном режиме. Дополнительные сведения см. в разделе [Обратная связь по временно предоставляемому буферу памяти в пакетном режиме](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-memory-grant-feedback).     
   **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
* "DISABLE_DEFERRED_COMPILATION_TV"    
  Отключает отложенную компиляцию табличных переменных. См. дополнительные сведения об [отложенной компиляции табличных переменных](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).     
  **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  "DISABLE_INTERLEAVED_EXECUTION_TVF"      
   Отключает выполнение с чередованием для функций с табличным значением с несколькими инструкциями. Дополнительные сведения см. в разделе о [выполнении с чередованием для функций с табличным значением с несколькими инструкциями](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).     
   **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_OPTIMIZED_NESTED_LOOP'      
   Заставляет обработчик запросов не использовать операцию сортировки (сортировки пакетов) для оптимизации соединений вложенного цикла при формировании плана запроса. Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2340.
*  'DISABLE_OPTIMIZER_ROWGOAL' <a name="use_hint_rowgoal"></a>      
   Указывает, что SQL Server должен создать план без использования изменений целей строк с запросами, содержащими следующие ключевые слова: 
   
   * В начало
   * OPTION (FAST N);
   * IN
   * EXISTS
   
   Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4138.
*  'DISABLE_PARAMETER_SNIFFING'      
   Указывает, что оптимизатор запросов должен использовать среднее распределение данных при компиляции запроса с одним или несколькими параметрами. Эта инструкция делает план запроса независимым от значения параметра, которое было использовано при первой компиляции запроса. Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4136 или параметру [конфигурации области баз данных](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`PARAMETER_SNIFFING = OFF`.
* "DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK"    
  Отключает обратную связь по временно предоставляемому буферу памяти в строковом режиме. Дополнительные сведения см. в разделе [Обратная связь по временно предоставляемому буферу памяти в строковом режиме](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).      
  **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
* "DISABLE_TSQL_SCALAR_UDF_INLINING"    
  Отключает встраивание скалярных пользовательских функций. Дополнительные сведения: [Встраивание скалярной функции, определяемой пользователем](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).     
  **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]).    
* "DISALLOW_BATCH_MODE"    
  Отключает выполнение в пакетном режиме. Дополнительные сведения см. в разделе [Режимы выполнения](../../relational-databases/query-processing-architecture-guide.md#execution-modes).     
  **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
*  'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'      
   Позволяет использовать автоматически созданную быструю статистику (поправку к гистограмме) для любого начального столбца индекса, для которого требуется оценить кратность. Гистограмма, используемая для оценки кратности, будет так откорректирована во время компиляции запроса, чтобы учитывать фактическое максимальное или минимальное значение этого столбца. Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4139. 
*  'ENABLE_QUERY_OPTIMIZER_HOTFIXES'     
   Включает исправления в оптимизаторе запросов, выпущенные в накопительных пакетах обновления и пакетах обновления SQL Server. Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4199 или параметру [конфигурации области баз данных](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`QUERY_OPTIMIZER_HOTFIXES = ON`.
*  'FORCE_DEFAULT_CARDINALITY_ESTIMATION'      
   Заставляет оптимизатор запросов использовать модель [оценки кратности](../../relational-databases/performance/cardinality-estimation-sql-server.md), которая соответствует текущему уровню совместимости базы данных. Используйте это указание для переопределения параметра [конфигурации области баз данных](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`LEGACY_CARDINALITY_ESTIMATION = ON` или [флага трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481.
*  'FORCE_LEGACY_CARDINALITY_ESTIMATION' <a name="use_hint_ce70"></a>      
   Заставляет оптимизатор запросов использовать модель [оценки кратности](../../relational-databases/performance/cardinality-estimation-sql-server.md) для [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более ранних версий. Это указание эквивалентно [флагу трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481 или параметру [конфигурации области баз данных](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`LEGACY_CARDINALITY_ESTIMATION = ON`.
*  'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'          
 Принудительно изменяет поведение оптимизатора запросов на уровне запроса. Оптимизация выполняется так, как если бы запрос компилировался с уровнем совместимости базы данных _n_, где _n_ — максимальный поддерживаемый уровень совместимости базы данных (например, 100, 130 и т. д.). Список значений, поддерживаемых сейчас для _n_, см. здесь: [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md).      
   **Применимо к** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления CU10).    

   > [!NOTE]
   > Указание QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n не переопределяет значение по умолчанию или унаследованное значение параметра оценки кратности, если оно указано в конфигурации области базы данных, с помощью флага трассировки или другого указания запроса, например QUERYTRACEON.   
   > Это указание влияет только на поведение оптимизатора запросов. Оно не влияет на другие функции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые могут зависеть от [уровня совместимости базы данных](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md), в том числе на доступность определенных функций базы данных.  
   > Дополнительные сведения об этом указании см. в разделе [Developer’s Choice: Hinting Query Execution model](/archive/blogs/sql_server_team/developers-choice-hinting-query-execution-model) (Выбор разработчика: модель выполнения запроса указания).
    
*  'QUERY_PLAN_PROFILE'      
 Включает упрощенное профилирование для запроса. Когда завершается запрос, содержащий это новое указание, вызывается новое расширенное событие query_plan_profile. Это расширенное событие предоставляет статистику выполнения и фактический план выполнения XML (подобно расширенному событию query_post_execution_showplan, но только для запросов, содержащих новое указание).    
   **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 2 (SP2) и накопительным пакетом обновления 3 (CU3) и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления 11 (CU11)). 

   > [!NOTE]
   > Если включен сбор расширенных событий query_post_execution_showplan, в каждый запрос, который выполняется на сервере, будет добавлена стандартная инфраструктура профилирования. Это может повлиять на общую производительность сервера.      
   > Если вместо этого вы включите сбор расширенных событий *query_thread_profile* для использования упрощенной инфраструктуры профилирования, издержки производительности будут гораздо ниже, но по-прежнему могут влиять на производительность сервера.       
   > Если вы включите расширенное событие query_plan_profile, упрощенная инфраструктура профилирования будет применяться только к запросам, которые выполняются с указанием QUERY_PLAN_PROFILE, и не повлияет на другие рабочие нагрузки на сервере. Используйте это указание для профилирования конкретного запроса, не влияя на другие части рабочей нагрузки сервера.
   > Дополнительные сведения об облегченном профилировании см. в разделе [Инфраструктура профилирования запросов](../../relational-databases/performance/query-profiling-infrastructure.md).
 
Список всех поддерживаемых имен USE HINT можно запросить с помощью динамического административного представления [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md).    

> [!TIP]
> В именах указаний учитывается регистр.   
  
> [!IMPORTANT] 
> Некоторые указания USE HINT могут конфликтовать с флагами трассировки, включенными на глобальном уровне или уровне сеанса, или параметрами конфигурации области баз данных. В этом случае приоритет всегда имеет указание уровня запроса (USE HINT). Если USE HINT конфликтует с другим указанием запроса или флагом трассировки, включенным на уровне запроса (например, с помощью QUERYTRACEON), при попытке выполнить запрос [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выведет ошибку. 

<a name="use-plan"></a> USE PLAN N' _<xml\_plan>_ '  
 Указывает оптимизатору запросов использовать существующий план запроса для запроса, определенного параметром **'** _xml\_plan_ **'** . Подсказку USE PLAN нельзя указывать в инструкциях INSERT, UPDATE MERGE и DELETE.  
  
TABLE HINT **(** _<exposed\_object\_name>_ [ **,** \<table_hint> [ [ **,** ]..._n_ ] ] **)** Позволяет применить заданное табличное указание к таблице или представлению, которые соответствуют _exposed\_object\_name_. Табличные указания рекомендуется использовать в качестве подсказок в запросах только в контексте [структуры плана](../../relational-databases/performance/plan-guides.md).  
  
 _<exposed\_object\_name>_ может представлять одну из следующих ссылок:  
  
-   Если в предложении [FROM](../../t-sql/queries/from-transact-sql.md) используется псевдоним таблицы или представления, _exposed\_objeсt\_name_ совпадает с этим псевдонимом.  
  
-   Если псевдоним не используется, _exposed\_object\_name_ будет точным соответствием таблицы или представления, на которые ссылается предложение FROM. Например, если ссылка на таблицу или представление является двухкомпонентным именем, аргумент _exposed\_object\_name_ будет тем же двухкомпонентным именем.  
  
 Если вы укажете _exposed\_object\_name_ без табличного указания, любые индексы, которые указаны в составе табличного указания для этого объекта в запросе, будут игнорироваться. Затем оптимизатор запросов определяет использование индексов. Эта методика позволяет устранить влияние табличного указания INDEX, если нет возможности изменить первоначальный запрос. См. пример К.  
  
**\<table_hint> ::=** { NOEXPAND [ , INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) ] \| INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) \| FORCESEEK [ **(** _<index\_value>_ **(** _<index\_column\_name>_ [ **,** ... ] **))** ] \| FORCESCAN \| HOLDLOCK \| NOLOCK \| NOWAIT \| PAGLOCK \| READCOMMITTED \| READCOMMITTEDLOCK \| READPAST \| READUNCOMMITTED \| REPEATABLEREAD \| ROWLOCK \| SERIALIZABLE \| SNAPSHOT \| SPATIAL_WINDOW_MAX_CELLS = _<integer\_value>_ \| TABLOCK \| TABLOCKX \| UPDLOCK \| XLOCK }    
Табличное указание, применяемое в качестве указания запроса к таблице или представлению, которые соответствует *exposed_object_name*. Описание этих указаний см. в разделе [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Табличные указания, за исключением INDEX, FORCESCAN и FORCESEEK, не могут использоваться как указания запроса, кроме тех случаев, когда в запросе уже содержится предложение WITH, задающее табличное указание. Дополнительные сведения см. в разделе с [примечаниями](#remarks).  
  
> [!CAUTION]
> Указав FORCESEEK с параметрами, вы ограничите число планов, которые может использовать оптимизатор запросов. Из-за этого может чаще возникать ошибка "Невозможно сформировать план". В будущем выпуске внутренние изменения оптимизатора запросов могут привести к увеличению числа этих планов.  
  
## <a name="remarks"></a>Remarks  
 Указания запросов нельзя задавать в инструкции INSERT, кроме случая, когда внутри инструкции используется предложение SELECT.  
  
 Указания запросов можно задавать только в запросах верхнего уровня, но не во вложенных запросах. Если табличное указание задается в качестве указания запроса, его можно определить в запросе верхнего уровня или во вложенном запросе. При этом значение _<exposed\_object\_name>_ в предложении TABLE HINT должно точно соответствовать имени, предоставленному в запросе или вложенном запросе.  
  
## <a name="specifying-table-hints-as-query-hints"></a>Определение табличных указаний как указаний запроса  
 Табличные указания INDEX, FORCESCAN или FORCESEEK рекомендуется использовать в качестве указаний запроса только в контексте [структуры плана](../../relational-databases/performance/plan-guides.md). Структуры планов полезны, когда нет возможности изменить первоначальный запрос, например, если он является приложением стороннего разработчика. Указание запроса, заданное в структуре плана, добавляется к запросу перед его компиляцией и оптимизацией. В автоматизированных запросах предложение TABLE HINT используется только при тестировании инструкций структуры планов. Для всех других нерегламентированных запросов рекомендуется задавать эти указания только как табличные.  
  
 Табличные указания INDEX, FORCESCAN и FORCESEEK, определенные в качестве указаний запроса, допустимы для следующих объектов:  
  
-   Таблицы  
-   Представления  
-   Индексированные представления  
-   Обобщенные табличные выражения (подсказку необходимо указывать в инструкции SELECT, результирующий набор которой заполняет обобщенное табличное выражение)  
-   Динамические административные представления
-   Именованные вложенные запросы  
  
Вы можете указать табличные указания INDEX, FORCESCAN и FORCESEEK как указания запроса, если в этом запросе не существует табличных указаний. Кроме того, их можно использовать для замены в запросе существующих указаний INDEX, FORCESCAN или FORCESEEK соответственно. 

Табличные указания, за исключением INDEX, FORCESCAN и FORCESEEK, не могут использоваться как указания запроса, кроме тех случаев, когда в запросе уже содержится предложение WITH, задающее табличное указание. В этом случае следует создать аналогичное указание в качестве указания запроса. Чтобы задать аналогичное указание в качестве указания запроса, включите TABLE HINT в предложение OPTION. Эта спецификация сохраняет семантику запроса. Например, если запрос содержит табличное указание NOLOCK, то предложение OPTION в параметре **\@hints** структуры плана также должно содержать указание NOLOCK. См. пример Л. 
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-merge-join"></a>A. Использование MERGE JOIN  
 Следующий пример указывает, что операция JOIN в запросе выполняется с помощью MERGE JOIN. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *   
FROM Sales.Customer AS c  
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID  
WHERE TerritoryID = 5  
OPTION (MERGE JOIN);  
GO    
```  
  
### <a name="b-using-optimize-for"></a>Б. Использование OPTIMIZE FOR  
 В следующем примере оптимизатору запросов дается указание, что при оптимизации запроса следует использовать значение `'Seattle'` для `@city_name` и среднюю избирательность предиката по всем значениям столбцов для `@postal_code`. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
CREATE PROCEDURE dbo.RetrievePersonAddress
@city_name NVARCHAR(30),  
 @postal_code NVARCHAR(15)
AS
SELECT * FROM Person.Address  
WHERE City = @city_name AND PostalCode = @postal_code  
OPTION ( OPTIMIZE FOR (@city_name = 'Seattle', @postal_code UNKNOWN) );  
GO
```  
  
### <a name="c-using-maxrecursion"></a>В. Использование MAXRECURSION  
 Подсказка MAXRECURSION может использоваться для предотвращения входа в бесконечный цикл из-за неверно сформированного рекурсивного обобщенного табличного выражения. В следующем примере преднамеренно формируется бесконечный цикл и используется указание MAXRECURSION для ограничения числа уровней рекурсии двумя. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
--Creates an infinite loop  
WITH cte (CustomerID, PersonID, StoreID) AS  
(  
    SELECT CustomerID, PersonID, StoreID  
    FROM Sales.Customer  
    WHERE PersonID IS NOT NULL  
  UNION ALL  
    SELECT cte.CustomerID, cte.PersonID, cte.StoreID  
    FROM cte   
    JOIN  Sales.Customer AS e   
        ON cte.PersonID = e.CustomerID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT CustomerID, PersonID, StoreID  
FROM cte  
OPTION (MAXRECURSION 2);  
GO  
```  
  
 После исправления ошибки в коде подсказка MAXRECURSION больше не нужна.  
  
### <a name="d-using-merge-union"></a>Г. Использование MERGE UNION  
 В следующем примере используется указание запроса MERGE UNION. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *  
FROM HumanResources.Employee AS e1  
UNION  
SELECT *  
FROM HumanResources.Employee AS e2  
OPTION (MERGE UNION);  
GO  
```  
  
### <a name="e-using-hash-group-and-fast"></a>Д. Использование HASH GROUP и FAST  
 В следующем примере используется указания запросов HASH GROUP и FAST. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO    
```  
  
### <a name="f-using-maxdop"></a>Е. Использование MAXDOP  
 В следующем примере используется указание запроса MAXDOP. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (MAXDOP 2);    
GO
```  
  
### <a name="g-using-index"></a>Ж. Использование INDEX  
 В следующем примере используется указание в запросе INDEX. В первом примере задан один индекс. Во втором примере указывается несколько индексов для одной табличной ссылки. К таблице с псевдонимом применено указание INDEX, поэтому в обоих примерах необходимо указать тот же псевдоним в предложении TABLE HINT в качестве имени видимого объекта. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX (IX_Employee_ManagerID)))';  
GO  
EXEC sp_create_plan_guide   
    @name = N'Guide2',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX(PK_Employee_EmployeeID, IX_Employee_ManagerID)))';  
GO    
```  
  
### <a name="h-using-forceseek"></a>З. Использование FORCESEEK  
 В следующем примере используется табличное указание FORCESEEK. В предложении TABLE HINT также необходимо указать двухкомпонентное имя, которое совпадает с именем видимого объекта. Укажите имя при применении указания INDEX к таблице, которая использует двухкомпонентное имя. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide3',   
    @stmt = N'SELECT c.LastName, c.FirstName, HumanResources.Employee.Title  
              FROM HumanResources.Employee  
              JOIN Person.Contact AS c ON HumanResources.Employee.ContactID = c.ContactID  
              WHERE HumanResources.Employee.ManagerID = 3  
              ORDER BY c.LastName, c.FirstName;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT( HumanResources.Employee, FORCESEEK))';  
GO    
```  
  
### <a name="i-using-multiple-table-hints"></a>И. Использование нескольких табличных указаний  
 В следующем примере к одной таблице применяется указание INDEX, а к другой — указание FORCESEEK. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide4',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX( IX_Employee_ManagerID))   
                       , TABLE HINT (c, FORCESEEK))';  
GO  
```  
  
### <a name="j-using-table-hint-to-override-an-existing-table-hint"></a>К. Использование TABLE HINT для замещения существующего табличного указания  
В следующем примере показано, как использовать указание TABLE HINT. Это указание можно использовать без дополнительного указания, переопределяющего поведение табличного указания INDEX, которое указано в предложении FROM в запросе. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide5',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e WITH (INDEX (IX_Employee_ManagerID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e))';  
GO    
```  
  
### <a name="k-specifying-semantics-affecting-table-hints"></a>Л. Определение табличных указаний, влияющих на семантику  
В следующем примере в запросе содержатся два указания таблицы: NOLOCK, которое изменяет семантику, и INDEX, которое не изменяет семантику. Чтобы сохранить семантику запроса, указание NOLOCK задается в предложении OPTIONS структуры плана. Наряду с указанием NOLOCK задайте указания INDEX и FORCESEEK, а также замените не влияющее на семантику указание INDEX в запросе при компиляции и оптимизации инструкции. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide6',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX(IX_Employee_ManagerID), NOLOCK, FORCESEEK))';  
GO    
```  
  
В следующем примере показан альтернативный метод сохранения семантики запроса, позволяющий оптимизатору выбрать другой индекс, в отличие от заданного в табличном указании. Предоставьте этот выбор оптимизатору, задав указание NOLOCK в предложении OPTIONS. Указание нужно для того, чтобы изменить семантику. Также укажите ключевое слово TABLE HINT со ссылкой на таблицу, но без указания INDEX. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide7',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, NOLOCK))';  
GO  
```  

### <a name="l-using-use-hint"></a>М. Указание USE HINT  
 В следующем примере используются указания запросов RECOMPILE и USE HINT. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (RECOMPILE, USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES', 'DISABLE_PARAMETER_SNIFFING')); 
GO  
```  

### <a name="m-using-querytraceon-hint"></a>Н. Использование QUERYTRACEON HINT  
 В следующем примере используются указания запроса QUERYTRACEON. В этом примере используется база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Можно включить все исправления, влияющие на план, которыми управляет флаг трассировки 4199, для конкретного запроса, используя следующий запрос:
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (QUERYTRACEON 4199);
```  

 Кроме того, вы можете использовать несколько флагов трассировки, как в следующем запросе:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION  (QUERYTRACEON 4199, QUERYTRACEON 4137);
```

## <a name="see-also"></a>См. также:  
[Указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)   
[sp_create_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
[sp_control_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
[Флаги трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)       
[Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)      
