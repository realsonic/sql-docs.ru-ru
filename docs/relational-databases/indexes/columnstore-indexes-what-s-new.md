---
description: Новые возможности индексов columnstore
title: Новые возможности индексов columnstore | Документация Майкрософт
ms.custom: ''
ms.date: 05/11/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9fc87cef0244afc4459c789f94855c0c0530ddd
ms.sourcegitcommit: e5664d20ed507a6f1b5e8ae7429a172a427b066c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697117"
---
# <a name="columnstore-indexes---what39s-new"></a>Новые возможности индексов columnstore
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Сводка функций индексов columnstore, доступных для каждой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и для последних выпусков [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

 > [!NOTE]
 > Для [!INCLUDE[ssSDS](../../includes/sssds-md.md)] индексы columnstore доступны на уровнях "Премиум", уровнях "Стандартный" (S3 и выше) и всех уровнях "Виртуальное ядро" для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Для [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 1 (SP1) и более поздних версий индексы columnstore доступны во всех выпусках. Для [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] (до пакета обновления 1 (SP1)) и более ранних версий индексы columnstore доступны только в выпуске Enterprise Edition.
 
## <a name="feature-summary-for-product-releases"></a>Сводка функций по выпускам  
 В следующей таблице перечислены основные функции для индексов columnstore и продукты, в которых они доступны.  

|Функция индекса columnstore|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]|[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]|[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]|  
|-------------------------------|---------------------------|---------------------------|---------------------------|---------------------------|--------------------------------------------|-------------------------|---|  
|Пакетный режим выполнения для многопоточных запросов|да|да|да|да|да|да|да| 
|Пакетный режим выполнения для однопоточных запросов|||да|да|да|да|да|  
|Параметр сжатия архивации||да|да|да|да|да|да|  
|Изоляция моментальных снимков и изоляция моментальных снимков read committed.|||да|да|да|да|да| 
|Указание индекса columnstore при создании таблицы|||да|да|да|да|да|  
|AlwaysOn поддерживает индексы columnstore|да|да|да|да|да|да|да| 
|Вторичная реплика для чтения AlwaysOn поддерживает некластеризованные индексы columnstore только для чтения|да|да|да|да|да|да|да|  
|Вторичная реплика для чтения AlwaysOn поддерживает обновляемые индексы columnstore|||да||да|||  
|Некластеризованный индекс columnstore только для чтения в куче или сбалансированном дереве|да|да|да <sup>1</sup>|да <sup>1</sup>|да <sup>1</sup>|да <sup>1</sup>|да <sup>1</sup>|  
|Обновляемый некластеризованный индекс columnstore в куче или сбалансированном дереве|||да|да|да|да|да|  
|Разрешены дополнительные индексы сбалансированного дерева в куче или сбалансированном дереве с некластеризованным индексом columnstore|да|да|да|да|да|да|да|  
|Обновляемый кластеризованный индекс columnstore||да|да|да||да|да|  
|Индекс сбалансированного дерева в кластеризованном индексе columnstore|||да|да||да|да|  
|Индекс columnstore в таблице, оптимизированной для памяти|||да|да||да|да|  
|Определение некластеризованного индекса columnstore поддерживает использование отфильтрованных условий|||да|да|да|да|да|  
|Параметр задержки сжатия для индексов columnstore в инструкциях `CREATE TABLE` и `ALTER TABLE`|||да|да|да|да|да|
|Индекс columnstore может содержать нематериализованный вычисляемый столбец||||да|да|||   
|Поддержка фонового слияния для переноса кортежей||||||да|да|да|
  
 <sup>1</sup> Чтобы создать некластеризованный индекс columnstore, доступный только для чтения, сохраните индекс в файловой группе только для чтения.  
 
> [!NOTE]
> Степень параллелизма (DOP) для операций [пакетного режима](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) ограничена 2 для выпуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard и 1 для выпусков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Web и Express. Это относится к индексам columnstore, созданным на основе таблиц на диске и оптимизированных для памяти таблиц.

## [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 
 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] добавляет указанные новые возможности.

### <a name="functional"></a>Функциональные
- Начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] задача переноса кортежей выполняется вместе с задачей фонового объединения. Последняя автоматически сжимает небольшие разностные группы строк с состоянием OPEN, которые существовали некоторое время в соответствии с внутренним пороговым значением, или объединяет группы строк с состоянием COMPRESSED, из которых было удалено большое количество строк. Ранее операция реорганизации индекса требовалась для объединения групп строк с частично удаленными данными. Это со временем повышает качество индекса columnstore. 

## [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 
 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] добавляет указанные новые возможности.

### <a name="functional"></a>Функциональные
- [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] поддерживает нематериализованные вычисляемые столбцы в кластеризованных индексах columnstore. Не поддерживает материализованные вычисляемые столбцы в кластеризованных индексах columnstore. Невозможно создать некластеризованный индекс в индексе columnstore, где есть вычисляемый столбец. 

## [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
 В[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] реализованы ключевые усовершенствования для повышения производительности и гибкости индексов columnstore. Эти улучшения расширяют возможности хранения данных и обеспечивают поддержку операционной аналитики в реальном времени.  
  
### <a name="functional"></a>Функциональные  
  
-   Таблица rowstore может включать один обновляемый некластеризованный индекс columnstore. Ранее некластеризованный индекс columnstore был доступен только для чтения.  
  
-   Определение некластеризованного индекса columnstore поддерживает использование отфильтрованных условий. Чтобы свести к минимуму негативное влияние на производительность, вызванное добавлением некластеризованного индекса columnstore для таблицы OLTP, можно использовать отфильтрованное условие для создания некластеризованного индекса columnstore только для холодных данных операционной рабочей нагрузки. 
  
-   Таблица в памяти может включать один индекс columnstore. Его можно создать при создании таблицы или добавить позже с помощью процедуры [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md). Ранее создание индекса columnstore допускалось только в таблицах на дисках.  
  
-   Кластеризованный индекс columnstore может включать один или несколько некластеризованных индексов rowstore. Ранее индекс columnstore не поддерживал некластеризованные индексы. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически обслуживает некластеризованные индексы для операций DML.  
  
-   Поддержка первичных и внешних ключей путем использования индекса сбалансированного дерева для применения этих ограничений в кластеризованном индексе columnstore.  
  
-   Индексы columnstore включают параметр задержки сжатия, который сводит к минимуму влияние транзакционной рабочей нагрузки на операционную аналитику в реальном времени.  Этот параметр позволяет стабилизировать часто изменяющиеся строки перед их сжатием в индекс columnstore. Дополнительные сведения см. в статьях [CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md) и [Начало работы с Columnstore для получения операционной аналитики в реальном времени](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="performance-for-database-compatibility-level-120-or-130"></a>Производительность для уровня совместимости базы данных 120 или 130  
  
-   Индексы columnstore поддерживают уровень изоляции моментальных снимков read committed (RCSI) и изоляцию моментальных снимков (SI). Это позволяет выполнять запросы аналитики, согласованные с транзакциями, без блокировки.  
  
-   Индекс columnstore поддерживает дефрагментацию индексов путем удаления удаленных строк без необходимости явного перестроения индекса. Инструкция `ALTER INDEX ... REORGANIZE` удаляет удаленные строки (на основе внутренней политики) из индекса columnstore в оперативном режиме.  
  
-   Вторичная реплика для чтения AlwaysOn может получать доступ к индексам columnstore. За счет переноса запросов аналитики во вторичную реплику AlwaysOn можно повысить производительность оперативной аналитики.  
  
-   Передача агрегата (Aggregate Pushdown) вычисляет агрегатные функции `MIN`, `MAX`, `SUM`, `COUNT` и `AVG` во время сканирования таблицы, если тип данных использует не более 8 байт и не относится к типу string. Передача агрегата поддерживается с предложением `GROUP BY` и без него для кластеризованных и некластеризованных индексов columnstore. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это улучшение зарезервировано для выпуска Enterprise.
  
-   Передача предиката для строк ускоряет выполнение запросов, которые сравнивают строки типа VARCHAR/CHAR или NVARCHAR/NCHAR. Это относится к общим операторам сравнения, включая такие операторы, как `LIKE`, которые используют фильтры битовой карты. Это работает со всеми поддерживаемыми параметрами сортировки. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это улучшение зарезервировано для выпуска Enterprise. 

-   Улучшения для операций пакетного режима за счет использования аппаратных возможностей на основе векторов. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] определяет уровень поддержки ЦП для аппаратных расширений AVX 2 (Advanced Vector Extensions) и SSE 4 (Streaming SIMD Extensions 4) и использует их, если они поддерживаются. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это улучшение зарезервировано для выпуска Enterprise.
  
### <a name="performance-for-database-compatibility-level-130"></a>Производительность для уровня совместимости базы данных 130  
  
-   Поддержка нового пакетного режима для запросов с помощью любой из следующих операций:  
    -   `SORT`  
    -   агрегаты с несколькими четко различимыми функциями некоторые примеры: `COUNT/COUNT`, `AVG/SUM`, `CHECKSUM_AGG`, `STDEV/STDEVP`;  
    -   агрегатные оконные функции: `COUNT`, `COUNT_BIG`, `SUM`, `AVG`, `MIN`, `MAX` и `CLR`;  
    -   определяемые пользователем оконные агрегаты: `CHECKSUM_AGG`, `STDEV`, `STDEVP`, `VAR`, `VARP` и `GROUPING`;  
    -   аналитические агрегатные оконные функции: `LAG`, `LEAD`, `FIRST_VALUE`, `LAST_VALUE`, `PERCENTILE_CONT`, `PERCENTILE_DISC`, `CUME_DIST` и `PERCENT_RANK`.  

-   Однопоточные запросы, выполняемые с `MAXDOP 1` или последовательным планом запроса, выполняются в пакетном режиме. Ранее в пакетном режиме могли выполняться только многопоточные запросы.  

-   Запросы к таблицам, оптимизированным для памяти, могут иметь параллельные планы в режиме взаимодействия SQL при доступе к данным как в индексе rowstore, так и в индексе columnstore.
  
### <a name="supportability"></a>Возможности поддержки  
Следующие системные представления являются новыми для columnstore:  
  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
Следующие представления DMV на основе OLTP в памяти содержат обновления для columnstore:  

:::row:::
    :::column:::
        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="limitations"></a>Ограничения    
-   Для таблиц в памяти индекс columnstore должен включать все столбцы; индекс columnstore не может включать отфильтрованное условие.  
-   Для таблиц в памяти запросы к индексу columnstore выполняются только в режиме взаимодействия, а не в собственном режиме в памяти. Поддерживается параллельное выполнение.  
  
## [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 В [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] кластеризованный индекс columnstore был представлен в качестве основного формата хранилища. Это позволило выполнять регулярные операции загрузки, а также операции обновления, удаления и вставки.  
  
-   Таблица может использовать кластеризованный индекс columnstore в качестве основного хранилища таблиц. Использование других индексов не допускается в таблице, однако кластеризованный индекс columnstore является обновляемым, что позволяет выполнять регулярные операции загрузки и вносить изменения в отдельные строки.  
-   Некластеризованный индекс columnstore имеет те же функции, что и в [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], за исключением того, что теперь дополнительные операторы могут выполняться в пакетном режиме. Он по-прежнему не поддерживает обновление, помимо перестроения и переключения секций. Некластеризованный индекс columnstore поддерживается только в таблицах на диске, но не в таблицах в памяти.  
-   Кластеризованный и некластеризованный индекс columnstore включает параметр сжатия архивации, обеспечивающий дополнительное сжатие данных. Параметр архивации удобен для сокращения объема данных в памяти и на диске, однако он снижает производительность запросов. Его можно рекомендовать для редко используемых данных.  
-   Кластеризованный и некластеризованный индексы columnstore функционируют весьма сходным образом; они используют одинаковый формат хранения по столбцам, подсистему обработки запросов и набор динамических административных представлений. Различие заключается в типе индекса (основной и дополнительный), кроме того, некластеризованный индекс columnstore доступен только для чтения.  
-   Следующие операторы выполняются в пакетном режиме для многопоточных запросов: SCAN, FILTER, PROJECT, JOIN, GROUP BY и UNION ALL.  
  
## [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
 В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] был представлен некластеризованный индекс columnstore как еще один тип индекса для таблиц rowstore и пакетной обработки запросов к данным columnstore.  
  
-   Таблица rowstore может включать один некластеризованный индекс columnstore.  
-   Индекс columnstore доступен только для чтения. После создания индекса columnstore нельзя обновлять таблицу с помощью операций `INSERT`, `DELETE` и `UPDATE`. Для выполнения этих операций необходимо удалить индекс, обновить таблицу и перестроить индекс columnstore. Дополнительные данные в таблицу можно загрузить с помощью переключения секций. Преимущество переключения секций заключается в том, что можно загрузить данные без удаления и перестроения индекса columnstore.  
-   Индекс columnstore всегда требует дополнительного места в хранилище, как правило, на 10 % больше, чем rowstore, поскольку в нем хранятся копии данных.  
-   Пакетная обработка обеспечивает двукратное (и более) повышение производительности, но она доступна только для параллельного выполнения запросов.  
  
## <a name="see-also"></a>См. также:  
 [Руководство по проектированию индексов columnstore](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Индексы columnstore. Руководство по загрузке данных](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Производительность запросов индексов columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Начало работы с Columnstore для получения операционной аналитики в реальном времени](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Индексы сolumnstore для хранилищ данных](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
  
