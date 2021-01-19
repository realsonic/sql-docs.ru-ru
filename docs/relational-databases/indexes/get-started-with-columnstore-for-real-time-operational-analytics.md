---
description: Начало работы с Columnstore для получения операционной аналитики в реальном времени
title: Начало работы с Columnstore для получения операционной аналитики в реальном времени | Документация Майкрософт
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: quickstart
ms.assetid: e1328615-6b59-4473-8a8d-4f360f73187d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4e62692402f33bfc42957363794bad9a698b18bb
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172766"
---
# <a name="get-started-with-columnstore-for-real-time-operational-analytics"></a>Начало работы с Columnstore для получения операционной аналитики в реальном времени
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] представляет операционную аналитику в реальном времени, дающую возможность одновременно выполнять рабочие нагрузки аналитики и OLTP в одной таблице базы данных. Кроме выполнения анализа в реальном времени, можно устранить потребность в процессе извлечения, преобразования и загрузки, а также в хранилище данных.  
  
## <a name="real-time-operational-analytics-explained"></a>Пояснения к операционной аналитике в реальном времени  
 Традиционно предприятия использовали отдельные системы для операционных (т. е. OLTP) и аналитических рабочих нагрузок. Для таких систем задания извлечения, преобразования и загрузки (ETL) регулярно перемещали данные из операционного хранилища в хранилище аналитики. Аналитические данные обычно хранятся в хранилище или киоске данных, предназначенном для выполнения аналитических запросов. Хотя такое решение являлось стандартным, у него есть три существенных недостатка:  
  
-   **Сложность.** Реализация извлечения, преобразования и загрузки может потребовать большого объема кода, особенно для загрузки только измененных строк. Определение того, какие именно строки изменены, может оказаться непростой задачей.  
  
-   **Стоимость.** Для реализации извлечения, преобразования и загрузки нужно приобрести дополнительное оборудование и лицензии на программное обеспечение.  
  
-   **Задержка данных.** Реализация извлечения, преобразования и загрузки добавляет задержку для выполнения анализа. Например, если задание ETL выполняется в конце каждого рабочего дня, аналитические запросы будут использовать данные, которые устарели по меньшей мере на день. Для многих организаций такая задержка недопустима, поскольку их бизнес зависит от анализа данных в реальном времени. Например, для выявления мошенничества требуется анализ рабочих данных в реальном времени.  
  
 ![Обзор операционной аналитики в режиме реального времени](../../relational-databases/indexes/media/real-time-operational-analytics-overview.png "Обзор операционной аналитики в режиме реального времени")  
  
Операционная аналитика в реальном времени предлагает решение этих проблем.   
При выполнении рабочих нагрузок аналитики и OLTP в одной базовой таблице задержка отсутствует.   В сценариях, где применима аналитика в реальном времени, значительно снижаются затраты и сложность за счет отказа от извлечения, преобразования и загрузки, а также от приобретения и обслуживания отдельного хранилища данных.  
  
> [!NOTE]  
>  Сценарий операционной аналитики в реальном времени ориентирован на отдельный источник данных, такой как приложение управления ресурсами предприятия (ERP), где можно выполнять как операционные, так и аналитические рабочие нагрузки. Однако отдельное хранилище данных по прежнему необходимо, если требуется интегрировать данные из нескольких источников перед выполнением аналитической рабочей нагрузки либо нужно обеспечить максимальную производительность анализа с помощью данных, прошедших предварительную статистическую обработку, таких как кубы.  
  
 Аналитика в реальном времени использует индекс columnstore для таблицы rowstore. Индекс columnstore хранит копию данных, поэтому рабочие нагрузки аналитики и OLTP выполняются для отдельных копий данных. Это сводит к минимуму влияние одновременно выполняющихся рабочих нагрузок на производительность. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически сохраняет изменения индекса, поэтому для аналитики всегда доступны актуальные изменения OLTP. Такой подход позволяет удобно анализировать актуальные данные в реальном времени. Он работает как с дисковыми, так и с оптимизированными для памяти таблицы.  
  
## <a name="get-started-example"></a>Пример для начала работы  
 Чтобы начать работу с операционной аналитикой в реальном времени, выполните следующее.  
  
1.  Определите те таблицы в рабочей схеме, которые содержат данные для анализа.  
  
2.  Для каждой таблицы удалите все индексы сбалансированного дерева, предназначенные для ускорения аналитики по рабочей нагрузке OLTP. Замените их одним индексом columnstore.  Это позволяет повысить общую производительность рабочей нагрузки OLTP, так как потребуется обрабатывать меньше индексов.  
  
    ```sql  
    --This example creates a nonclustered columnstore index on an existing OLTP table.  
    --Create the table  
    CREATE TABLE t_account (  
        accountkey int PRIMARY KEY,  
        accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int  
    );  
  
    --Create the columnstore index with a filtered condition  
    CREATE NONCLUSTERED COLUMNSTORE INDEX account_NCCI   
    ON t_account (accountkey, accountdescription, unitsold)   
    ;  
    ```  
  
     Индекс columnstore в таблице в памяти позволяет осуществлять операционную аналитику путем интеграции технологий OLTP в памяти и columnstore в памяти для получения высокой производительности рабочих нагрузок OLTP и аналитики. Индекс columnstore для таблицы в памяти должен включать все столбцы.  
  
    ```sql  
    -- This example creates a memory-optimized table with a columnstore index.  
    CREATE TABLE t_account (  
        accountkey int NOT NULL PRIMARY KEY NONCLUSTERED,  
        Accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int,  
        INDEX t_account_cci CLUSTERED COLUMNSTORE  
        )  
        WITH (MEMORY_OPTIMIZED = ON );  
    GO  
  
    ```  
  
3.  Вот и все, что нужно сделать!  

 Теперь все готово для запуска операционной аналитики в реальном времени без внесения изменений в приложение.  Аналитические запросы будут использовать индекс columnstore, а операции OLTP продолжат использовать индексы сбалансированного дерева OLTP. Рабочие нагрузки OLTP продолжат выполняться, однако появятся некоторые дополнительные издержки на обработку индекса columnstore. В следующем разделе описаны процессы оптимизации производительности.  
  
## <a name="blog-posts"></a>Записи блога  
 Ознакомьтесь со следующими записями блога, содержащими дополнительные сведения об операционной аналитике в реальном времени. Это поможет вам лучше понять приведенные ниже советы по производительности.  
  
-   [Экономическое обоснование для операционной аналитики в реальном времени (Business case for real-time operational analytics)](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)  
  
-   [Использование некластеризованного индекса columnstore для операционной аналитики в реальном времени (Using a nonclustered columnstore index for real-time operational analytics)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-using-nonclustered-columnstore-index)  
  
-   [Простой пример использования некластеризованного индекса columnstore (A simple example using a nonclustered columnstore index)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci)  
  
-   [Обработка SQL Server некластеризованного индекса columnstore в транзакционной рабочей нагрузке (How SQL Server maintains a nonclustered columnstore index  on a transactional workload)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016)  
  
-   [Минимизация влияния некластеризованного индекса с помощью отфильтрованного индекса (Minimizing the impact of nonclustered columnstore index maintenance by using a filtered index)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)  
  
-   [Минимизация влияния некластеризованного индекса с помощью задержки сжатия (Minimizing the impact of nonclustered columnstore index maintenance by using compression delay)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)  
  
-   [Минимизация влияния некластеризованного индекса с помощью задержки сжатия — показатели производительности (Minimizing impact of a nonclustered columnstore index maintenance by using compression delay - performance numbers)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance)  
  
-   [Операционная аналитика в реальном времени с таблицами, оптимизированными для памяти](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-memory-optimized-table-and-columnstore-index)  
  
-   [Индексы columnstore и политика слияния для групп строк (Columnstore index and the merge policy for rowgroups)](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)  
  
## <a name="performance-tip-1-use-filtered-indexes-to-improve-query-performance"></a>Совет для повышения производительности № 1. Для улучшения производительности запросов пользуйтесь отфильтрованными индексами  
 Выполнение операционной аналитики в реальном времени может повлиять на производительность рабочей нагрузки OLTP. Это влияние должно быть минимальным. В приведенном ниже примере показано, как использовать отфильтрованные индексы для минимизации влияния некластеризованного индекса columnstore на транзакционную рабочую нагрузку при одновременном предоставлении аналитики в реальном времени.  
  
 Чтобы свести к минимуму дополнительные издержки на ведение некластеризованного индекса columnstore для операционной рабочей нагрузки, можно использовать отфильтрованное условие для создания некластеризованного индекса columnstore только для *теплых* или медленно изменяющихся данных. Например, в приложении управления заказом можно создать некластеризованный индекс columnstore для уже доставленных заказов. Заказы редко изменяются после доставки, поэтому эти данные можно считать теплыми. При наличии отфильтрованного индекса данным в некластеризованном индексе columnstore требуется меньше обновлений, что снижает влияние на транзакционную рабочую нагрузку.  
  
 Аналитические запросы прозрачно обращаются как к теплым, так и к горячим данным для обеспечения аналитики в реальном времени. Если значительная часть операционной рабочей нагрузки связана с "горячими" данными, этим операциям не потребуется дополнительная обработка индекса columnstore. Для столбцов, используемых в определении отфильтрованного индекса, рекомендуется иметь кластеризованный индекс rowstore. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует его, чтобы быстро сканировать строки, не соответствующие отфильтрованному условию. Без такого кластеризованного индекса для сканирования этих строк потребуется полное сканирование таблицы rowstore, что может отрицательно повлиять на производительность аналитического запроса. В случае отсутствия кластеризованного индекса можно создать дополняющий отфильтрованный некластеризованный индекс сбалансированного дерева для определения таких строк, однако так поступать не рекомендуется, поскольку доступ к большому диапазону строк через некластеризованный индекс сбалансированного дерева потребляет много ресурсов.  
  
> [!NOTE]  
>  Отфильтрованный некластеризованный индекс columnstore поддерживается только для дисковых таблиц. Оптимизированные для памяти таблицы он не поддерживает.  
  
### <a name="example-a-access-hot-data-from-btree-index-warm-data-from-columnstore-index"></a>Пример А. Доступ к горячим данным из индекса сбалансированного дерева и теплым данным из индекса columnstore  
 В этом примере отфильтрованное условие (accountkey > 0) определяет, какие строки будут находиться в индексе columnstore. Требуется так спроектировать отфильтрованное условие и последующие запросы, чтобы доступ к часто изменяющимся "горячим" данным осуществлялся из индекса сбалансированного дерева, а к более стабильным "теплым" данным — из индекса columnstore.  
  
 ![Комбинированные индексы для теплых и горячих данных](../../relational-databases/indexes/media/de-columnstore-warmhotdata.png "Комбинированные индексы для теплых и горячих данных")  
  
> [!NOTE]  
>  Оптимизатор запросов учитывает возможность использования индекса columnstore для плана запроса, но не всегда пользуется ей. Когда оптимизатор запросов выбирает отфильтрованный индекс columnstore, он прозрачно объединяет строки из индекса columnstore и строки, не соответствующие отфильтрованному условию, обеспечивая аналитику в режиме реального времени. Это отличается от обычного отфильтрованного некластеризованного индекса, который можно использовать только в запросах, ограниченных представленными в индексе строками.  
  
```sql  
--Use a filtered condition to separate hot data in a rowstore table  
-- from "warm" data in a columnstore index.  
  
-- create the table  
CREATE TABLE  orders (  
         AccountKey         int not null,  
         Customername       nvarchar (50),  
        OrderNumber         bigint,  
        PurchasePrice       decimal (9,2),  
        OrderStatus         smallint not null,  
        OrderStatusDesc     nvarchar (50))  
  
-- OrderStatusDesc  
-- 0 => 'Order Started'  
-- 1 => 'Order Closed'  
-- 2 => 'Order Paid'  
-- 3 => 'Order Fullfillment Wait'  
-- 4 => 'Order Shipped'  
-- 5 => 'Order Received'  
  
CREATE CLUSTERED INDEX  orders_ci ON orders(OrderStatus)  
  
--Create the columnstore index with a filtered condition  
CREATE NONCLUSTERED COLUMNSTORE INDEX orders_ncci ON orders  (accountkey, customername, purchaseprice, orderstatus)  
where orderstatus = 5  
;  
  
-- The following query returns the total purchase done by customers for items > $100 .00  
-- This query will pick  rows both from NCCI and from 'hot' rows that are not part of NCCI  
SELECT top 5 customername, sum (PurchasePrice)  
FROM orders  
WHERE purchaseprice > 100.0   
Group By customername  
```  
  
 Аналитический запрос будет выполняться с приведенным ниже планом запроса. Видно, что доступ к не соответствующим отфильтрованному условию строкам осуществляется через кластеризованный индекс сбалансированного дерева.  
  
 ![План запроса](../../relational-databases/indexes/media/query-plan-columnstore.png "План запроса")  
  
 Дополнительные сведения см. в блоге об [отфильтрованном некластеризованном индексе columnstore.](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)  
  
## <a name="performance-tip-2-offload-analytics-to-always-on-readable-secondary"></a>Совет для повышения производительности № 2. Разгрузка аналитики во вторичную реплику для чтения AlwaysOn  
 Хотя обработку индекса columnstore можно минимизировать с помощью отфильтрованного индекса columnstore, аналитические запросы могут по-прежнему потреблять значительные вычислительные ресурсы (ЦП, ввода-вывода, памяти), что негативно влияет на производительность рабочей нагрузки. Для самых критически важных рабочих нагрузок рекомендуется использовать конфигурацию AlwaysOn. В такой конфигурации можно исключить влияние аналитики, выгрузив ее во вторичную реплику для чтения.  
  
## <a name="performance-tip-3-reducing-index-fragmentation-by-keeping-hot-data-in-delta-rowgroups"></a>Совет для повышения производительности № 3. Снижение фрагментации индексов за счет хранения горячих данных в разностных группах строк  
 Таблицы с индексом columnstore могут становиться сильно фрагментированными (т. е. иметь удаленные строки), когда рабочая нагрузка обновляет или удаляет сжатые строки. Фрагментированный индекс columnstore вызывает неэффективное использование памяти и хранилища. Кроме того, это негативно влияет на производительность аналитических запросов из-за дополнительных операций ввода-вывода и необходимости фильтрации удаленных строк из результирующего набора.  
  
 До запуска дефрагментации индекса с помощью команды `REORGANIZE` или перестроения индекса columnstore для всей таблицы или затронутых секций фактической очистки удаленных строк не происходит. Команды для работы с индексом `REORGANIZE` и `REBUILD` потребляют значительную часть ресурсов, которые в противном случае могли бы использоваться для рабочей нагрузки. Кроме того, если строки сжаты слишком рано, из-за обновлений может потребоваться их многократное повторное сжатие, что ведет к бесполезной трате ресурсов.  
Для минимизации фрагментации индекса можно использовать параметр `COMPRESSION_DELAY`.  
  
```sql  
-- Create a sample table  
CREATE TABLE t_colstor (  
               accountkey                      int not null,  
               accountdescription              nvarchar (50) not null,  
               accounttype                     nvarchar(50),  
               accountCodeAlternatekey         int)  
  
-- Creating nonclustered columnstore index with COMPRESSION_DELAY. The columnstore index will keep the rows in closed delta rowgroup for 100 minutes   
-- after it has been marked closed  
CREATE NONCLUSTERED COLUMNSTORE index t_colstor_cci on t_colstor (accountkey, accountdescription, accounttype)   
                       WITH (DATA_COMPRESSION= COLUMNSTORE, COMPRESSION_DELAY = 100);  
  
;  
```  
  
 Дополнительные сведения см. в записи блога о [задержке сжатия](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci).  
  
 Ниже приведены рекомендуемые методики.  
  
-   **Рабочая нагрузка со вставками и запросами**. Если основная часть рабочей нагрузки связана со вставкой и запросом данных, для параметра COMPRESSION_DELAY рекомендуется использовать значение по умолчанию — 0. Новые строки будут сжиматься после вставки 1 миллиона строк в отдельную разностную группу строк.  
    Примерами такой рабочей нагрузки являются: (а) обычные рабочие нагрузки хранилища данных, (б) анализ навигации, когда требуется проанализировать данные о щелчках мышью в веб-приложении.  
  
-   **Рабочая нагрузка OLTP**. При интенсивной рабочей нагрузке DML (т. е. ресурсоемком сочетании операций Update, Delete и Insert) можно определить фрагментацию индекса columnstore, проверив DMV sys. dm_db_column_store_row_group_physical_stats. Если вы видите, что более 10 % строк будут в недавно сжатых группах строк помечены как удаленные, можно использовать параметр COMPRESSION_DELAY, чтобы отсрочить сжатие этих строк. Например, если вновь вставленные для рабочей нагрузки данные остаются "горячими" (т. е. многократно обновляются), скажем, в течение 60 минут, необходимо выбрать COMPRESSION_DELAY равным 60.  
  
 Предполагается, что большинству пользователей никаких действий выполнять не потребуется. Им подойдет и значение COMPRESSION_DELAY по умолчанию.  
Опытным пользователям рекомендуется выполнить приведенный ниже запрос и собирать данные о проценте удаленных строк за последние 7 дней.  
  
```sql  
SELECT row_group_id,cast(deleted_rows as float)/cast(total_rows as float)*100 as [% fragmented], created_time  
FROM sys. dm_db_column_store_row_group_physical_stats  
WHERE object_id = object_id('FactOnlineSales2')   
             AND  state_desc='COMPRESSED'   
             AND deleted_rows>0   
             AND created_time > GETDATE() - 7  
ORDER BY created_time DESC;  
```  
  
 Если число удаленных строк в сжатых группах строк превышает 20 %, выполнив выравнивание в старых группах строк с вариативностью менее 5 % (называются холодными), задайте COMPRESSION_DELAY равным (youngest_rowgroup_created_time — current_time). Помните, что такой подход лучше всего работает для стабильной и сравнительно однородной рабочей нагрузки.  
  
## <a name="see-also"></a>См. также:  
 [Руководство по индексам columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)   
 [Загрузка данных индексов ColumnStore](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Производительность запросов индексов columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Индексы сolumnstore для хранилищ данных](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
