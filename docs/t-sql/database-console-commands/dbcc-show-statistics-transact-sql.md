---
description: Инструкция DBCC SHOW_STATISTICS (Transact-SQL)
title: Инструкция DBCC SHOW_STATISTICS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHOW_STATISTICS_TSQL
- DBCC SHOW_STATISTICS
- SHOW_STATISTICS
- DBCC_SHOW_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], densities
- histograms [SQL Server]
- statistical information [SQL Server], viewing statistics objects
- current distribution statistics
- query optimization statistics [SQL Server], histograms
- DBCC SHOW_STATISTICS statement
- distribution statistics
- statistical information [SQL Server], densities
- statistics objects
- statistical information [SQL Server], histograms
- query optimization statistics [SQL Server], viewing statistics objects
- statistical information [SQL Server], distribution
- densities [SQL Server]
- displaying distribution statistics
ms.assetid: 12be2923-7289-4150-b497-f17e76a50b2e
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 710a6211b588ab0dd4745b41af1f330c81241294
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170506"
---
# <a name="dbcc-show_statistics-transact-sql"></a>Инструкция DBCC SHOW_STATISTICS (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Инструкция DBCC SHOW_STATISTICS отображает статистику оптимизации текущего запроса для таблицы или индексированного представления. Оптимизатор запросов использует статистику, чтобы оценить кратность или число строк в результате запроса. Это позволяет ему создать план запросов высокого качества. Например, на основе оценки количества элементов оптимизатор запросов может выбрать в плане запроса оператор поиска по индексу, а не оператор просмотра индекса, повышая производительность запроса за счет использования менее ресурсоемкого поиска по индексу.
  
Оптимизатор запросов хранит статистические данные по таблице или индексированному представлению в объекте статистики. Объект статистики для таблицы создается по индексу или списку столбцов таблицы. Объект статистики включает заголовок, содержащий метаданные о статистике, гистограмму, содержащую распределение значений в первом ключевом столбце объекта статистики, и вектор плотностей для измерения корреляции с охватом нескольких столбцов. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] позволяет вычислять оценки количества элементов с применением любых данных в объекте статистики. Дополнительные сведения см. в разделе [Статистика](../../relational-databases/statistics/statistics.md) и [Оценка кратности (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md).
  
DBCC SHOW_STATISTICS отображает заголовок, гистограмму и вектор плотностей на основе данных, хранящихся в объекте статистики. Применяемый синтаксис позволяет указывать таблицу или индексированное представление вместе с именем целевого индекса, именем статистики или именем столбца. В этом разделе описывается, как отображать статистику и трактовать отображаемые результаты.

> [!IMPORTANT]
> Начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] с пакетом обновления 1 (SP1), DMV [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) позволяет программно получать сведения о заголовке, содержащиеся в объекте статистики, для ненакопительных статистических данных.

> [!IMPORTANT]
> Начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 1 (SP1) и [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] с пакетом обновления 2 (SP2), DMV [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md) позволяет программно получать сведения о заголовке, содержащиеся в объекте статистики, для накопительных статистических данных.

> [!IMPORTANT]
> Начиная с накопительного обновления 2 (CU2) [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1), DMV [sys.dm_db_stats_histogram](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) позволяет программно получать данные гистограммы, содержащиеся в объекте статистики.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DBCC SHOW_STATISTICS ( table_or_indexed_view_name , target )   
[ WITH [ NO_INFOMSGS ] < option > [ , n ] ]  
< option > :: =  
    STAT_HEADER | DENSITY_VECTOR | HISTOGRAM | STATS_STREAM  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

DBCC SHOW_STATISTICS ( table_name , target )   
    [ WITH { STAT_HEADER | DENSITY_VECTOR | HISTOGRAM } [ ,...n ] ]  
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

 *table_or_indexed_view_name*  
 Имя таблицы или индексированного представления, для которого должны быть отображены статистические данные.  
  
 *table_name*  
 Имя таблицы, содержащей статистику для отображения. Таблица не может быть внешней таблицей.  
  
 *target*  
 Имя индекса, статистики или столбца, для которого отображаются статистические данные. *target* заключается в круглые скобки, одинарные кавычки, двойные кавычки или не имеет кавычек. Если аргумент *target* является именем существующего индекса или статистики по таблице или индексированному представлению, будут возвращены статистические данные об этом адресате. Если *target* представляет собой имя существующего столбца и имеется статистика, автоматически созданная по данным этого столбца, возвращаются сведения об этой автоматически созданной статистике. Если автоматически созданная статистика для целевого столбца отсутствует, возвращается сообщение об ошибке 2767.  
 В [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]*target* не может быть именем столбца.  
  
 NO_INFOMSGS  
 Подавляет все информационные сообщения со степенями серьезности от 0 до 10.  
  
 STAT_HEADER \| DENSITY_VECTOR \| HISTOGRAM \| STATS_STREAM [ **,** _n_ ] Указание одного или более из этих параметров ограничивает результирующие наборы, возвращаемые выражением, указанным параметром или параметрами. Если параметры не указаны, то возвращаются все статистические данные.  

 Аргумент STATS_STREAM имеет тип [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="result-sets"></a>Результирующие наборы

В следующей таблице описываются столбцы, возвращаемые в результирующем наборе, если указан параметр STAT_HEADER.
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|Имя|Имя объекта статистики.|  
|Обновлен|Дата и время последнего обновления статистики. Функция [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) представляет собой альтернативный способ получения этих данных. Дополнительные сведения см. в подразделе [Примечания](#Remarks) на этой странице.|  
|Строки|Общее число строк в таблице или индексированном представлении при последнем обновлении статистики. Если статистика отфильтрована или соответствует отфильтрованному индексу, количество строк может быть меньше, чем количество строк в таблице. Дополнительные сведения см. в разделе [Статистика](../../relational-databases/statistics/statistics.md).|  
|Rows Sampled|Общее количество строк, выбранных для статистических вычислений. Если имеет место условие «количество строк выборки < количество строк таблицы», то отображаемые результаты определения гистограммы и вычисления плотности представляют собой оценки, основанные на строках выборки.|  
|Шаги|Число шагов в гистограмме. Каждый шаг охватывает диапазон значений столбцов, за которым следует значение столбца, представляющее собой верхнюю границу. Шаги гистограммы определяются в первом ключевом столбце статистики. Максимальное число шагов — 200.|  
|Плотность|Рассчитывается как 1 / *различающиеся значения* для всех значений в первом ключевом столбце объекта статистики, исключая возможные значения гистограммы. Это значение плотности не используется оптимизатором запросов и отображается для обратной совместимости с версиями, выпущенными до [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].|  
|Средняя длина ключа|Среднее число байтов на значение для всех ключевых столбцов в объекте статистики.|  
|String Index|Значение «Да» указывает, что объект статистики содержит сводную строковую статистику, позволяющую уточнить оценку количества элементов для предикатов запроса, использующих оператор LIKE, например `WHERE ProductName LIKE '%Bike'`. Сводная строковая статистика хранится отдельно от гистограммы и создается в первом ключевом столбце объекта статистики, если он имеет тип **char**, **varchar**, **nchar**, **nvarchar**, **varchar(max)** , **nvarchar(max)** , **text** или **ntext**.|  
|Критерий фильтра|Предикат для подмножества строк таблицы, включенных в объект статистики. NULL — неотфильтрованная статистика. Дополнительные сведения об отфильтрованных предикатах см. в статье [Создание отфильтрованных индексов](../../relational-databases/indexes/create-filtered-indexes.md). Дополнительные сведения об отфильтрованной статистике см. в разделе [Статистика](../../relational-databases/statistics/statistics.md).|  
|Unfiltered Rows|Общее количество строк в таблице перед применением критерия фильтра. Если Filter Expression имеет значение NULL, то столбец Unfiltered Rows совпадает со столбцом Rows.|  
|Процент материализованной выборки|Процент материализованной выборки используется для обновлений статистики, где явно не указан процент выборки. Если значение равно нулю, процент материализованной выборки не устанавливается для этой статистики.<br /><br /> **Применимо к:** [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1) и накопительным обновлением 4| 
  
Следующая таблица описывает столбцы, возвращаемые в результирующий набор, если указан параметр DENSITY_VECTOR.
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|Общая плотность|Плотность равна 1 / *различающиеся значения*. В результатах отображаются плотности для каждого префикса столбцов объекта статистики, по одной строке на плотность. Различающееся значение — это отдельный список значений столбцов на строку и на префикс столбцов. Например, если объект статистики содержит ключевые столбцы (A, B, C), то в результатах приводится плотность отдельных списков значений в каждом из следующих префиксов столбцов: (A), (A, B) и (A, B, C). При использовании префикса (A, B, C) каждый из этих списков является отдельным списком значений: (3, 5, 6), (4, 4, 6), (4, 5, 6), (4, 5, 7). При использовании префикса (A, B) одинаковые значения столбцов имеют следующие отдельные списки значений: (3, 5), (4, 4) и (4, 5).|  
|Средняя длина|Средняя длина (в байтах) для хранения списка значений столбца для данного префикса столбца. Если каждому значению в списке (3, 5, 6), например, требуется по 4 байта, то длина составляет 12 байт.|  
|Столбцы|Имена столбцов в префиксе, для которых отображаются значения «Общая плотность» и «Средняя длина».|  
  
Следующая таблица описывает столбцы, возвращаемые в результирующий набор, если указан параметр HISTOGRAM.
  
|Имя столбца|Описание|  
|---|---|
|RANGE_HI_KEY|Верхнее граничное значение столбца для шага гистограммы. Это значение столбца называется также ключевым значением.|  
|RANGE_ROWS|Предполагаемое количество строк, значение столбцов которых находится в пределах шага гистограммы, исключая верхнюю границу.|  
|EQ_ROWS|Предполагаемое количество строк, значение столбцов которых равно верхней границе шага гистограммы.|  
|DISTINCT_RANGE_ROWS|Предполагаемое количество строк с различающимся значением столбца в пределах шага гистограммы, исключая верхнюю границу.|  
|AVG_RANGE_ROWS|Среднее количество строк с повторяющимися значениями столбца в пределах шага гистограммы, исключая верхнюю границу. Если значение DISTINCT_RANGE_ROWS больше 0, AVG_RANGE_ROWS вычисляется делением RANGE_ROWS на DISTINCT_RANGE_ROWS. Если значение DISTINCT_RANGE_ROWS равно 0, AVG_RANGE_ROWS возвращает значение 1 для шага гистограммы.| 
  
## <a name="remarks"></a><a name="Remarks"></a> Замечания 

Дата обновления статистики хранится в [большом двоичном объекте статистики](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) вместе с [гистограммой](#histogram) и [вектором плотности](#density), а не в метаданных. Если нет данных для создания статистических данных, большой двоичный объект статистики не создается, дата недоступна и *обновленный* столбец имеет значение NULL. Это происходит с отфильтрованной статистикой, для которой предикат не возвращает ни одной строки, или с новыми пустыми таблицами.
  
## <a name="histogram"></a><a name="histogram"></a> Гистограмма

Гистограмма измеряет частоту появления каждого различающегося значения в наборе данных. Оптимизатор запросов вычисляет гистограмму для значений столбца в первом ключевом столбце объекта статистики, выбирая значения столбцов путем статистической выборки строк или при помощи полного просмотра всех строк в таблице или представлении. Если гистограмма создается на основе выбранного набора строк, то сохраняемые итоговые значения количества строк и количества различающихся значений являются приблизительными и не всегда выражаются целыми числами.
  
Чтобы создать гистограмму, оптимизатор запросов сортирует значения столбцов, вычисляет количество значений, совпадающих с каждым различающимся значением столбца, а затем осуществляет статистическую обработку значений столбцов с получением непрерывных шагов гистограммы, максимальное количество которых составляет 200. Каждый шаг включает диапазон значений столбцов, за которым следует значение столбца, представляющее собой верхнюю границу. В этот диапазон входят все возможные значения столбца между граничными значениями, за исключением самих граничных значений. Наименьшим из отсортированных значений столбца является верхнее граничное значение первого шага гистограммы.
  
На следующей диаграмме показана гистограмма с шестью шагами. Первый шаг — это область слева от первого верхнего граничного значения.
  
![Гистограмма](../../relational-databases/system-dynamic-management-views/media/a0ce6714-01f4-4943-a083-8cbd2d6f617a.gif "a0ce6714-01f4-4943-a083-8cbd2d6f617a")
  
В каждом шаге гистограммы:
-   полужирной линией обозначено верхнее граничное значение (RANGE_HI_KEY) и количество его вхождений (EQ_ROWS);  
-   закрашенная область слева от RANGE_HI_KEY обозначает диапазон значений столбца и среднее количество вхождений каждого значения столбца (AVG_RANGE_ROWS). В первом шаге гистограммы значение AVG_RANGE_ROWS всегда равно 0;  
-   пунктирные линии обозначают выбранные значения, которые используются для оценки общего числа различающихся значений (DISTINCT_RANGE_ROWS) и общего числа значений в диапазоне (RANGE_ROWS). Оптимизатор запросов использует RANGE_ROWS и DISTINCT_RANGE_ROWS для вычисления AVG_RANGE_ROWS и не хранит выбранные значения.  
  
Оптимизатор запросов определяет шаги гистограммы согласно их статистической значимости. Он использует алгоритм максимальной разности для сведения к минимуму числа шагов в гистограмме и вместе с тем максимального увеличения разницы между граничными значениями. Максимальное число шагов — 200. Число шагов гистограммы может быть меньше, чем количество различающихся значений, даже для столбцов, в которых число граничных точек меньше 200. Например, столбец со 100 различающимися значениями может иметь гистограмму, число граничных точек в которой меньше 100.
  
## <a name="density-vector"></a><a name="density"></a> Вектор плотностей  
Оптимизатор запросов использует плотности для улучшения оценок количества элементов для запросов, которые возвращают данные нескольких столбцов из одной таблицы или индексированного представления. Вектор плотностей содержит по одной плотности для каждого префикса столбцов объекта статистики. Например, если объект статистики имеет ключевые столбцы `CustomerId`, `ItemId` и `Price`, плотность вычисляется для каждого из следующих префиксов столбцов.
  
|Префикс столбца|Префикс, по которому вычисляется плотность|  
|---|---|
|(CustomerId)|Строки с совпадающими значениями CustomerId|  
|(CustomerId, ItemId)|Строки с совпадающими значениями CustomerId и ItemId.|  
|(CustomerId, ItemId, Price)|Строки с совпадающими значениями CustomerId, ItemId и Price.|  
  
## <a name="restrictions"></a>Ограничения  
 Инструкция DBCC SHOW_STATISTICS не предоставляет статистических данных для пространственного или оптимизированного для памяти индексов columnstore.  
  
## <a name="permissions-for-ssnoversion-and-sssds"></a>Разрешения для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
Для просмотра объекта статистики у пользователя должно быть разрешение `SELECT` для таблицы.
Обратите внимание, что для того, чтобы разрешение SELECT было достаточным для выполнения команды, существуют следующие требования.
-   Пользователь должен иметь разрешение для всех столбцов в статистическом объекте.  
-   Пользователь должен иметь разрешение для всех столбцов в условии фильтра (если фильтр задан).  
-   Таблица не может иметь политику безопасности на уровне строк.
-   Если какие-либо столбцы в объекте статистики замаскированы с помощью правил динамического маскирования данных, то помимо разрешения `SELECT` у пользователя должно быть разрешение `UNMASK`.

В версиях, предшествующих [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] с пакетом обновления 1 (SP1), пользователь должен быть владельцем таблицы или членом предопределенной роли сервера `sysadmin`, предопределенной роли базы данных `db_owner` или предопределенной роли базы данных `db_ddladmin`.

 > [!NOTE]
 > Чтобы восстановить поведение, доступное в версиях, предшествующих [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] с пакетом обновления 1 (SP1), используйте флаг трассировки 9485.
  
## <a name="permissions-for-sssdw-and-sspdw"></a>Разрешения для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC SHOW_STATISTICS требует разрешения `SELECT` для таблицы или членства в предопределенной роли сервера `sysadmin`, предопределенной роли базы данных `db_owner` или предопределенной роли базы данных `db_ddladmin`.  
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Ограничения для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Инструкция DBCC SHOW_STATISTICS показывает статистику, хранящуюся в базе данных оболочки на уровне управляющего узла. Она не показывает статистику, автоматически созданную [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на вычислительных узлах.
  
DBCC SHOW_STATISTICS не поддерживается во внешних таблицах.
  
## <a name="examples-ssnoversion-and-sssds"></a>Примеры: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
### <a name="a-returning-all-statistics-information"></a>A. Возвращение всех статистических данных  
Следующий пример отображает все статистические данные для индекса `AK_Address_rowguid` таблицы `Person.Address` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);  
GO  
```  
  
### <a name="b-specifying-the-histogram-option"></a>Б. Задание параметра HISTOGRAM  
Это ограничивает статистические данные, отображаемые для Customer_LastName, до данных HISTOGRAM.
  
```sql
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName) WITH HISTOGRAM;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Примеры: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="c-display-the-contents-of-one-statistics-object"></a>В. Отображение содержимого одного объекта статистики  
 В следующем примере отображается содержимое статистики Customer_LastName в таблице DimCustomer.  
  
```sql
-- Uses AdventureWorks  
--First, create a statistics object  
CREATE STATISTICS Customer_LastName   
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName);  
GO  
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName);  
GO  
```  
  
В результатах показан заголовок, вектор плотности и части гистограммы.
  
![Результаты DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/media/aps-sql-dbccshow-statistics.JPG "Результаты DBCC SHOW_STATISTICS")
  
## <a name="see-also"></a>См. также:  
[Статистика](../../relational-databases/statistics/statistics.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)  
[DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)  
[sp_autostats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)  
[sp_createstats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)  
[STATS_DATE (Transact-SQL)](../../t-sql/functions/stats-date-transact-sql.md)  
[UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)  
[sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)  
[sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)   
[sys.dm_db_incremental_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)   
