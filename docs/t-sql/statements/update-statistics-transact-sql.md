---
description: UPDATE STATISTICS (Transact-SQL)
title: UPDATE STATISTICS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UPDATE STATISTICS
- UPDATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating statistics
- query optimization statistics [SQL Server], updating
- UPDATE STATISTICS statement
- statistical information [SQL Server], updating
ms.assetid: 919158f2-38d0-4f68-82ab-e1633bd0d308
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c8d9cc3f1218ab0e374359a57dce6cb13dc0b8a6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099445"
---
# <a name="update-statistics-transact-sql"></a>UPDATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Обновляет статистику оптимизации запросов для таблицы или индексированного представления. По умолчанию оптимизатор запросов обновляет статистику по мере необходимости для усовершенствования плана запроса. В некоторых случаях можно повысить производительность запроса, выполняя обновление статистики с помощью инструкции `UPDATE STATISTICS` или хранимой процедуры [sp_updatestats](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md) чаще, чем это происходит по умолчанию.  
  
Обновление статистики гарантирует, что запросы будут компилироваться с актуальной статистикой. Однако обновление статистики вызывает перекомпиляцию запросов. Рекомендуется не обновлять статистику слишком часто, поскольку необходимо найти баланс между выигрышем в производительности за счет усовершенствованных планов запросов и потерей времени на перекомпиляцию запросов. Критерии выбора компромиссного решения зависят от приложения. `UPDATE STATISTICS` может использовать базу данных tempdb для сортировки образцов строк для построения статистики.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
UPDATE STATISTICS table_or_indexed_view_name   
    [   
        {   
            { index_or_statistics__name }  
          | ( { index_or_statistics_name } [ ,...n ] )   
                }  
    ]   
    [    WITH   
        [  
            FULLSCAN   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | SAMPLE number { PERCENT | ROWS }   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | RESAMPLE   
              [ ON PARTITIONS ( { <partition_number> | <range> } [, ...n] ) ]  
            | <update_stats_stream_option> [ ,...n ]  
        ]   
        [ [ , ] [ ALL | COLUMNS | INDEX ]   
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ] 
    ] ;  
  
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
UPDATE STATISTICS [ schema_name . ] table_name   
    [ ( { statistics_name | index_name } ) ]  
    [ WITH   
       {  
              FULLSCAN   
            | SAMPLE number PERCENT   
            | RESAMPLE   
        }  
    ]  
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Аргументы
 *table_or_indexed_view_name*  
 Имя таблицы или индексированного представления, содержащего статистический объект.  
  
 *index_or_statistics_name*  
 Имя индекса, для которого обновляется статистика, или имя обновляемой статистики. Если аргумент *index_or_statistics_name* не указан, то оптимизатор запросов обновляет всю статистику для таблицы или индексированного представления. Сюда входит статистика, созданная инструкцией CREATE STATISTICS, статистика по отдельным столбцам, созданная при включенном параметре AUTO_CREATE_STATISTICS, и статистика, созданная для индексов.  
  
 Дополнительные сведения об AUTO_CREATE_STATISTICS см. в разделе [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md). Просмотреть все индексы для таблицы или представления можно с помощью процедуры [sp_helpindex](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md).  
  
 FULLSCAN  
 Вычисляет статистику путем просмотра всех строк в таблице или индексированном представлении. FULLSCAN и SAMPLE 100 PERCENT имеют одинаковые результаты. FULLSCAN не может быть использован с параметром SAMPLE.  
  
 SAMPLE *number* { PERCENT | ROWS }  
 Указывает приблизительное процентное соотношение или число строк в таблице или индексированном представлении для оптимизатора запросов, которые используются при обновлении статистики. Аргумент *number* для параметра PERCENT может иметь значение от 0 до 100, а для параметра ROWS аргумент *number* может иметь значение от 0 до общего числа строк. Фактическое процентное соотношение или число строк, отбираемых оптимизатором запросов, может не совпадать с заданным значением. Например, оптимизатор запросов просматривает все строки на странице данных.  
  
 Команда SAMPLE полезна в особых случаях, в которых план запроса на основе выборки по умолчанию не является оптимальным. В большинстве случаев нет необходимости использовать команду SAMPLE, так как оптимизатор запросов делает выборку и определяет размер статистически значимой выборки по умолчанию, что требуется для создания высококачественных планов запроса. 
 
Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], выборка данных для формирования статистики выполняется в параллельном режиме при использовании уровня совместимости 130, что повышает производительность сбора статистики. Оптимизатор запросов будет использовать статистику параллельной выборки каждый раз, когда размер таблицы превышает определенный порог. 
   
 Параметр SAMPLE нельзя использовать вместе с параметром FULLSCAN. Если не указана ни одна из команд SAMPLE или FULLSCAN, оптимизатор запросов использует выбранные данные и вычисляет размер выборки по умолчанию.  
  
 Не рекомендуется указывать значения 0 PERCENT и 0 ROWS. Если для PERCENT или ROWS указано значение 0, объект статистики будет обновлен без статистических данных.  
  
 Для большинства значений рабочих нагрузок полная проверка не требуется, достаточно выборки по умолчанию.  
Тем не менее для некоторых рабочих нагрузок, чувствительных к разнящимся распределениям данных, может потребоваться выборка большего размера или даже полная проверка.  
Дополнительные сведения см. в блоге [Службы эскалации CSS SQL](/archive/blogs/psssql/sampling-can-produce-less-accurate-statistics-if-the-data-is-not-evenly-distributed).  
  
 RESAMPLE  
 Обновить каждый объект статистики, используя последнее значение частоты выборки.  
  
 Использование RESAMPLE может вызвать просмотр полной таблицы. Например, статистика для индексов использует для частоты выборки просмотр полной таблицы. Если не указан ни один из параметров выборки (SAMPLE, FULLSCAN, RESAMPLE), оптимизатор запросов выполняет выборку данных и вычисляет размер выборки по умолчанию.  

PERSIST_SAMPLE_PERCENT = { ON | OFF }  
Если установлено значение **ON**, статистика будет сохранять заданный процент выборки для последующих обновлений, где явно не указан процент выборки. Если установлено значение **OFF**, процент выборки будет сбрасываться на значение по умолчанию при последующих обновлениях, где явно не указан процент выборки. Значение по умолчанию — **OFF**. 
 
 > [!NOTE]
 > Если выполняется инструкция AUTO_UPDATE_STATISTICS, используется сохраненный процент выборки, если он указан, или процент выборки по умолчанию, если нет.
 > Этот параметр не влияет на реакцию RESAMPLE.
 
 > [!NOTE]
 > Если таблица усечена, вся статистика, построенная на усеченном HoBT, вернется к использованию процентного соотношения выборки по умолчанию.
 
 > [!TIP] 
 > Значение сохраненного процента выборки для выбранной статистики отображается в [DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) и [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md).
 
 **Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 CU4) и выше (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]CU1).  
 
 ON PARTITIONS ( { \<partition_number> | \<range> } [, ...n] ) ]. Задает принудительное повторное вычисление статистик конечного уровня, посвященных секциям в предложении ON PARTITIONS, с последующим их объединением для создания глобальных статистик. WITH RESAMPLE обязательно, потому что статистики секции, построенные с различной частотой выборки, нельзя объединить.  
  
**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий
  
 ALL | COLUMNS | INDEX  
 Обновить всю существующую статистику, созданную по одному или нескольким столбцам, или статистику, созданную для индексов. Если не указан ни один параметр, инструкция UPDATE STATISTICS обновляет всю статистику для таблицы или индексированного представления.  
  
 NORECOMPUTE  
 Отключить параметр автоматического обновления статистики AUTO_UPDATE_STATISTICS для указанной статистики. Если указан этот параметр, оптимизатор запросов завершает текущее обновление статистики и отключает обновление в будущем.  
  
 Чтобы возобновить действие параметра AUTO_UPDATE_STATISTICS, снова выполните инструкцию UPDATE STATISTICS без параметра NORECOMPUTE или выполните процедуру **sp_autostats**.  
  
> [!WARNING]  
> Использование этого параметра может привести к созданию неоптимальных планов запросов. Рекомендуется ограничить использование этого параметра, причем использовать его надлежит только опытным системным администраторам.  
  
 Дополнительные сведения о параметре AUTO_STATISTICS_UPDATE см. в разделе [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 INCREMENTAL = { ON | OFF }  
 При значении **ON** статистики повторно создаются как статистики отдельно по секциям. При значении **OFF** дерево статистик удаляется и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] повторно вычисляет статистики. Значение по умолчанию — **OFF**.  
  
 Если статистики по секциям не поддерживаются, возвращается ошибка. Добавочные статистики не поддерживаются для следующих типов статистических данных.  
  
-   Статистики, созданные с индексами, не выровненными по секциям для базовой таблицы.  
-   Статистики, созданные в доступных для чтения базах данных-получателях AlwaysOn.  
-   Статистики, созданные в базах данных, доступных только для чтения.  
-   Статистики, созданные по фильтрованным индексам.  
-   Статистика, созданная по представлениям.  
-   Статистики, созданные по внутренним таблицам.  
-   Статистики, созданные с пространственными индексами или XML-индексами.  
  
**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий

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
  
 \<update_stats_stream_option> 
 
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="remarks"></a>Remarks  
  
### <a name="when-to-use-update-statistics"></a>Условия использования инструкции UPDATE STATISTICS  
 Дополнительные сведения об условиях использования `UPDATE STATISTICS` см. в разделе [Статистика](../../relational-databases/statistics/statistics.md).  

### <a name="limitations-and-restrictions"></a>Ограничения  
* Обновление статистики во внешних таблицах не поддерживается. Для обновления статистики во внешней таблице удалите и повторно создайте статистику.  
* Параметр `MAXDOP` несовместим с параметрами `STATS_STREAM`, `ROWCOUNT` и `PAGECOUNT`.
* Параметр `MAXDOP` ограничивается параметром `MAX_DOP` группы рабочей нагрузки Resource Governor (если применимо).

### <a name="updating-all-statistics-with-sp_updatestats"></a>Обновление всей статистики с помощью процедуры sp_updatestats  
Сведения об обновлении статистики по всем определяемым пользователем таблицам и внутренним таблицам в базе данных см. в описании хранимой процедуры [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md). Например, следующая команда вызывает процедуру sp_updatestats для обновления всей статистики для базы данных.  
  
```sql  
EXEC sp_updatestats;  
```  

### <a name="automatic-index-and-statistics-management"></a>Автоматическое управление индексами и статистикой
Используйте такие решения, как [Адаптивная дефрагментация индексов](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag), чтобы автоматически управлять дефрагментацией индексов и обновлениями статистики для одной базы данных или нескольких. Эта процедура автоматически выбирает, следует ли перестроить или реорганизовать индекс, сверяясь с уровнем фрагментации и другими параметрами, и обновляет статистику на основе линейных пороговых значений.
  
### <a name="determining-the-last-statistics-update"></a>Определение времени последнего обновления статистики  
 Чтобы определить время последнего обновления статистики, используйте функцию [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) .  
  
### <a name="pdw--azure-synapse-analytics"></a>PDW/Azure Synapse Analytics  
 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] / [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] не поддерживают следующий синтаксис.  
  
```sql
UPDATE STATISTICS t1 (a,b);   
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH SAMPLE 10 ROWS;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH NORECOMPUTE;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH INCREMENTAL = ON;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH stats_stream = 0x01;  
```  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение `ALTER` для таблицы или представления.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-update-all-statistics-on-a-table"></a>A. Обновление всей статистики для таблицы  
 В следующем примере обновляется статистика для всех индексов в таблице `SalesOrderDetail`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail;  
GO  
```  
  
### <a name="b-update-the-statistics-for-an-index"></a>Б. Обновление статистики для индекса  
 В следующем примере обновляется статистика для индекса `AK_SalesOrderDetail_rowguid` в таблице `SalesOrderDetail`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail AK_SalesOrderDetail_rowguid;  
GO  
```  
  
### <a name="c-update-statistics-by-using-50-percent-sampling"></a>В. Обновление статистики с применением 50-процентной выборки  
 В следующем примере создается, а затем обновляется статистика для столбцов `Name` и `ProductNumber` в таблице `Product`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE STATISTICS Products  
    ON Production.Product ([Name], ProductNumber)  
    WITH SAMPLE 50 PERCENT  
-- Time passes. The UPDATE STATISTICS statement is then executed.  
UPDATE STATISTICS Production.Product(Products)   
    WITH SAMPLE 50 PERCENT;  
```  
  
### <a name="d-update-statistics-by-using-fullscan-and-norecompute"></a>Г. Обновление статистики с использованием параметров FULLSCAN и NORECOMPUTE  
 В следующем примере обновляется статистика `Products` в таблице `Product`, принудительно запускается полный просмотр всех строк таблицы `Product` и отключается автоматическое обновление статистики `Products`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Production.Product(Products)  
    WITH FULLSCAN, NORECOMPUTE;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-update-statistics-on-a-table"></a>Д. Обновление статистики для таблицы  
 В следующем примере обновляется статистика `CustomerStats1` в таблице `Customer`.  
  
```sql  
UPDATE STATISTICS Customer ( CustomerStats1 );  
```  
  
### <a name="f-update-statistics-by-using-a-full-scan"></a>Е. Обновление статистики с помощью полной проверки  
 В следующем примере обновляется статистика `CustomerStats1` на основе проверки всех строк в таблице `Customer`.  
  
```sql  
UPDATE STATISTICS Customer (CustomerStats1) WITH FULLSCAN;  
```  
  
### <a name="g-update-all-statistics-on-a-table"></a>Ж. Обновление всей статистики для таблицы  
 В следующем примере обновляется вся статистика в таблице `Customer`.  
  
```sql  
UPDATE STATISTICS Customer;  
```  
  
## <a name="see-also"></a>См. также:  
 [Статистика](../../relational-databases/statistics/statistics.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_autostats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [STATS_DATE (Transact-SQL)](../../t-sql/functions/stats-date-transact-sql.md)  
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)    
 [sys.dm_db_stats_histogram &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)