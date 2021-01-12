---
description: sys.dm_db_index_operational_stats (Transact-SQL)
title: sys.dm_db_index_operational_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_index_operational_stats
- sys.dm_db_index_operational_stats_TSQL
- sys.dm_db_index_operational_stats
- dm_db_index_operational_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_operational_stats dynamic management function
ms.assetid: 13adf2e5-2150-40a6-b346-e74a33ce29c6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a6eb4083361d07fee44557d20dd4be4625cbdb12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095203"
---
# <a name="sysdm_db_index_operational_stats-transact-sql"></a>sys.dm_db_index_operational_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает текущие действия ввода-вывода нижнего уровня, блокировки, кратковременной блокировки и метода доступа для каждой секции таблицы или индекса в базе данных.    
    
 Индексы с оптимизацией для памяти в данном DMV не отображаются.    
    
> [!NOTE]    
>  **sys.dm_db_index_operational_stats** не возвращает сведения о индексах, оптимизированных для памяти. Сведения об использовании индексов, оптимизированных для памяти, см. в разделе [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).    
        
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Синтаксис    
    
```    
sys.dm_db_index_operational_stats (    
    { database_id | NULL | 0 | DEFAULT }    
  , { object_id | NULL | 0 | DEFAULT }    
  , { index_id | 0 | NULL | -1 | DEFAULT }    
  , { partition_number | NULL | 0 | DEFAULT }    
)    
```    
    
## <a name="arguments"></a>Аргументы    

*database_id* | NULL | 0 | ПАРАМЕТРЫ

  Идентификатор базы данных. *database_id* имеет **smallint**. Допустимыми входными значениями являются идентификатор базы данных, NULL, 0 или DEFAULT. Значение по умолчанию — 0. В данном контексте значения NULL, 0 и DEFAULT эквивалентны.    
    
 Укажите значение NULL, чтобы вернуть сведения для всех баз данных в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если для *database_id* указано значение null, необходимо также указать значение null для *object_id*, *index_id* и *partition_number*.    
    
 Может быть указана встроенная функция [DB_ID](../../t-sql/functions/db-id-transact-sql.md).    

*object_id* | NULL | 0 | ПАРАМЕТРЫ

 Идентификатор объекта таблицы или представления, которые содержат индекс. *object_id* имеет **тип int**.    
    
 Допустимыми входными значениями являются идентификатор таблицы, NULL, 0 или DEFAULT. Значение по умолчанию — 0. В данном контексте значения NULL, 0 и DEFAULT эквивалентны.    
    
 Укажите значение NULL, чтобы вернуть кэшированные данные для всех таблиц и представлений в указанной базе данных. Если для *object_id* указано значение null, необходимо также указать значение null для *index_id* и *partition_number*.    

*index_id* | 0 | NULL | -1 | ПАРАМЕТРЫ

 Идентификатор индекса. *index_id* имеет **тип int**. Допустимые входные данные — это ИДЕНТИФИКАЦИОНный номер индекса, 0, если *object_id* является КУЧЕЙ, null,-1 или ЗНАЧЕНИЕМ по умолчанию. Значение по умолчанию равно -1. Значения NULL, -1 и DEFAULT в данном контексте эквивалентны.    
    
 Укажите значение NULL, чтобы вернуть кэшированные данные для всех индексов базовой таблицы или представления. Если для *index_id* указано значение null, необходимо также указать значение null для *partition_number*.    

*partition_number* | NULL | 0 | ПАРАМЕТРЫ

 Номер секции в объекте. *partition_number* имеет **тип int**. Допустимыми входными значениями являются *partion_number* индекса, КУЧИ, null, 0 или Default. Значение по умолчанию — 0. В данном контексте значения NULL, 0 и DEFAULT эквивалентны.    
    
 Укажите NULL, чтобы возвратить кэшированные данные для всех секций индекса или кучи.    
    
 *partition_number* , основанный на 1. Несекционированный индекс или куча имеет *partition_number* значение 1.    
    
## <a name="table-returned"></a>Возвращаемая таблица    
    
|Имя столбца|Тип данных|Описание|    
|-----------------|---------------|-----------------|    
|**database_id**|**smallint**|Идентификатор базы данных.|    
|**object_id**|**int**|Идентификатор таблицы или представления.|    
|**index_id**|**int**|Идентификатор индекса или кучи.<br /><br /> 0 = куча;| 
|**partition_number**|**int**|Номер секции внутри индекса или кучи (нумерация начинается с 1).| 
|**hobt_id**|**bigint**|**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](../../sql-server/what-s-new-in-sql-server-2016.md)), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .<br /><br /> Идентификатор набора строк куча или сбалансированное дерево данных, который отслеживает внутренние данные для индекса columnstore.<br /><br /> NULL — это не внутренний набор строк columnstore.<br /><br /> Дополнительные сведения см. в разделе [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)|       
|**leaf_insert_count**|**bigint**|Совокупное количество вставок конечного уровня.|    
|**leaf_delete_count**|**bigint**|Совокупное количество удалений конечного уровня. leaf_delete_count увеличивается только для удаленных записей, которые сначала не помечены как фантомные. Для удаленных записей, которые сначала являются фантомными, **leaf_ghost_count** увеличивается.|    
|**leaf_update_count**|**bigint**|Совокупное количество обновлений конечного уровня.|    
|**leaf_ghost_count**|**bigint**|Совокупное количество строк конечного уровня, которые помечены как удаленные, но еще не удалены. Этот счетчик не включает записи, которые немедленно удаляются без пометки как фантомы. Эти строки будут удалены потоком очистки в установленные интервалы времени. Это значение не включает строки, которые сохранены, из-за необработанной транзакции изоляции моментальных снимков.|    
|**nonleaf_insert_count**|**bigint**|Совокупное количество вставок выше конечного уровня.<br /><br /> 0 = куча или columnstore|    
|**nonleaf_delete_count**|**bigint**|Совокупное количество удалений выше конечного уровня.<br /><br /> 0 = куча или columnstore|    
|**nonleaf_update_count**|**bigint**|Совокупное количество обновлений выше конечного уровня.<br /><br /> 0 = куча или columnstore|    
|**leaf_allocation_count**|**bigint**|Совокупное количество размещений страниц конечного уровня в индексе или куче.<br /><br /> Для индекса размещение страницы соответствует разбиению страницы.|    
|**nonleaf_allocation_count**|**bigint**|Совокупное количество размещений страниц, вызванных разбиениями страниц выше конечного уровня.<br /><br /> 0 = куча или columnstore|    
|**leaf_page_merge_count**|**bigint**|Совокупное количество слияний страниц на конечном уровне. Всегда 0 для индекса columnstore.|    
|**nonleaf_page_merge_count**|**bigint**|Совокупное количество слияний страниц выше конечного уровня.<br /><br /> 0 = куча или columnstore|    
|**range_scan_count**|**bigint**|Совокупное количество просмотров диапазонов и таблиц, запущенных на индексе или куче.|    
|**singleton_lookup_count**|**bigint**|Совокупное количество извлечений одиночных строк из индекса или кучи.|    
|**forwarded_fetch_count**|**bigint**|Число строк, выбранных через перенаправляющую запись.<br /><br /> 0 = индексы|    
|**lob_fetch_in_pages**|**bigint**|Совокупное количество страниц больших объектов (LOB), извлеченных из единицы распределения LOB_DATA. Эти страницы содержат данные, которые хранятся в столбцах типа **Text**, **ntext**, **Image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)** и **XML**. Дополнительные сведения см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).|    
|**lob_fetch_in_bytes**|**bigint**|Совокупное количество извлеченных байтов данных LOB.|    
|**lob_orphan_create_count**|**bigint**|Совокупное количество потерянных значений LOB, созданных для массовых операций.<br /><br /> 0 = некластеризованный индекс|    
|**lob_orphan_insert_count**|**bigint**|Совокупное количество потерянных значений LOB, вставленных во время массовых операций.<br /><br /> 0 = некластеризованный индекс|    
|**row_overflow_fetch_in_pages**|**bigint**|Совокупное количество превышающих размер страницы данные строки, извлеченных из единицы распределения ROW_OVERFLOW_DATA.<br /><br /> Эти страницы содержат данные, хранящиеся в столбцах типов **varchar (n)**, **nvarchar (n)**, **varbinary (n)** и **sql_variant** , которые были переданы вне строки.|    
|**row_overflow_fetch_in_bytes**|**bigint**|Совокупное количество извлеченных байтов, превышающих размер страницы данные строки.|    
|**column_value_push_off_row_count**|**bigint**|Совокупное количество значений столбца для данных LOB и превышающих размер страницы данные строки, которые вытесняются из строки, чтобы вместить на странице вставленную или обновленную строку.|    
|**column_value_pull_in_row_count**|**bigint**|Совокупное количество значений столбцов для данных LOB и превышающих размер страницы данные строки, которые помещаются в строку. Это происходит, когда операция обновления освобождает пространство в записи и предоставляет возможность поместить одно или несколько выходящих за пределы строки значений из единиц распределения LOB_DATA или ROW_OVERFLOW_DATA в единицу распределения IN_ROW_DATA.|    
|**row_lock_count**|**bigint**|Совокупное количество запрошенных блокировок строк.|    
|**row_lock_wait_count**|**bigint**|Совокупное количество раз, когда компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал блокировки строки.|    
|**row_lock_wait_in_ms**|**bigint**|Общее время в миллисекундах, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал блокировки строки.|    
|**page_lock_count**|**bigint**|Совокупное количество запрошенных блокировок страниц.|    
|**page_lock_wait_count**|**bigint**|Совокупное количество раз, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал блокировки страницы.|    
|**page_lock_wait_in_ms**|**bigint**|Общее время в миллисекундах, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал блокировки страницы.|    
|**index_lock_promotion_attempt_count**|**bigint**|Совокупное количество раз, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] пытался повышать уровень блокировок.|    
|**index_lock_promotion_count**|**bigint**|Совокупное количество раз, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] повышал уровень блокировок.|    
|**page_latch_wait_count**|**bigint**|Совокупное количество раз, когда компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал из-за конфликтов кратковременной блокировки.|    
|**page_latch_wait_in_ms**|**bigint**|Совокупное количество миллисекунд, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал из-за конфликтов кратковременной блокировки.|    
|**page_io_latch_wait_count**|**bigint**|Совокупное количество раз, когда компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал кратковременной блокировки страницы ввода-вывода.|    
|**page_io_latch_wait_in_ms**|**bigint**|Совокупное количество миллисекунд, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] ожидал кратковременной блокировки страницы ввода-вывода.|    
|**tree_page_latch_wait_count**|**bigint**|Подмножество множества **page_latch_wait_count**, которое включает только страницы сбалансированного дерева верхнего уровня. Всегда 0 для кучи или индекса columnstore.|    
|**tree_page_latch_wait_in_ms**|**bigint**|Подмножество множества **page_latch_wait_in_ms**, которое включает только страницы сбалансированного дерева верхнего уровня. Всегда 0 для кучи или индекса columnstore.|    
|**tree_page_io_latch_wait_count**|**bigint**|Подмножество множества **page_io_latch_wait_count**, которое включает только страницы сбалансированного дерева верхнего уровня. Всегда 0 для кучи или индекса columnstore.|    
|**tree_page_io_latch_wait_in_ms**|**bigint**|Подмножество множества **page_io_latch_wait_in_ms**, которое включает только страницы сбалансированного дерева верхнего уровня. Всегда 0 для кучи или индекса columnstore.|    
|**page_compression_attempt_count**|**bigint**|Количество страниц, которые были оценены как пригодные для сжатия на уровне страницы для конкретных секций таблицы, индекса или индексированного представления. Включает несжатые страницы, поскольку это не привело бы к значительной экономии. Всегда 0 для индекса columnstore.|    
|**page_compression_success_count**|**bigint**|Количество страниц данных, которые были сжаты с помощью сжатия PAGE для конкретной секции таблицы, индекса или индексированного представления. Всегда 0 для индекса columnstore.|    
    
## <a name="remarks"></a>Комментарии    
 Этот объект динамического управления не принимает Коррелированные параметры от `CROSS APPLY` и `OUTER APPLY` .    
    
 Для отслеживания продолжительности ожидания пользователями считывания из таблицы, индекса или секции и записи в таблицу, индекс или секцию, а также для определения таблиц или индексов, в которых наблюдается значительная интенсивность операций ввода-вывода или присутствуют перегруженные участки, можно использовать представление **sys.dm_db_index_operational_stats**.    
    
 Используйте следующие столбцы для идентификации областей состязаний.    
    
 **Анализ типичного шаблона доступа к секции таблицы или индекса**    
    
-   **leaf_insert_count**    
    
-   **leaf_delete_count**    
    
-   **leaf_update_count**    
    
-   **leaf_ghost_count**    
    
-   **range_scan_count**    
    
-   **singleton_lookup_count**    
    
 Чтобы идентифицировать состязание кратковременных и обычных блокировок, используйте следующие столбцы.    
    
-   **page_latch_wait_count** и **page_latch_wait_in_ms**    
    
     Эти столбцы показывают наличие состязания кратковременной блокировки в индексе или куче и значимость конфликта.    
    
-   **row_lock_count** и **page_lock_count**    
    
     Эти столбцы указывают, сколько раз компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] пытался получить блокировки строк и страниц.    
    
-   **row_lock_wait_in_ms** и **page_lock_wait_in_ms**    
    
     Эти столбцы показывают наличие состязания блокировок в индексе или куче и значимость состязания.    
    
 **Анализ статистики физического ввода-вывода на индексе или секции кучи**    
    
-   **page_io_latch_wait_count** и **page_io_latch_wait_in_ms**    
    
     Эти столбцы указывают, были ли произведены физические операции ввода-вывода, чтобы занести страницы индекса или кучи в память, и сколько операций ввода-вывода было произведено.    
    
## <a name="column-remarks"></a>Примечания по столбцам    
 Значения в **lob_orphan_create_count** и **lob_orphan_insert_count** всегда должны быть равны.    
    
 Значение в столбцах **lob_fetch_in_pages** и **lob_fetch_in_bytes** может быть больше нуля для некластеризованных индексов, содержащих один или более LOB-столбцов в качестве включенных. Дополнительные сведения см. в статье [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md). Аналогично значение в столбцах **row_overflow_fetch_in_pages** и **row_overflow_fetch_in_bytes** может быть больше 0 для некластеризованных индексов, если индекс содержит столбцы, которые могут быть принудительно отправлены вне строки.    
    
## <a name="how-the-counters-in-the-metadata-cache-are-reset"></a>Сброс счетчиков в кэше метаданных    
 Данные, возвращенные **sys.dm_db_index_operational_stats**, существуют до тех пор, пока объект кэша метаданных, представляющий кучу или индекс, является доступным. Эти данные не являются постоянными и не согласованы на уровне транзакций. Это означает, что эти счетчики не позволяют определить факт использования индекса или время, когда индекс применялся последний раз. Дополнительные сведения см. в разделе [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md).    
    
 Значения для каждого столбца устанавливаются в нуль всякий раз, когда метаданные для кучи или индекса заносятся в кэш метаданных, и статистические данные накапливаются, пока объект кэша не удаляется из кэша метаданных. Поэтому активная куча или индекс будут, вероятно, всегда иметь эти метаданные в кэше, и совокупные значения количества могут отражать активность с момента последнего запуска экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Метаданные для менее активной кучи или индекса будут перемещаться в кэш и из него по мере их использования. В результате метаданные могут иметь или не иметь действительных значений. Удаление индекса приведет к удалению соответствующих статистических данных из памяти, и они больше не будут передаваться функцией. При других DDL-операциях с индексом может произойти обнуление статистических данных.    
    
## <a name="using-system-functions-to-specify-parameter-values"></a>Использование системных функций для указания значений параметров    
 Можно использовать [!INCLUDE[tsql](../../includes/tsql-md.md)] функции [DB_ID](../../t-sql/functions/db-id-transact-sql.md) и [object_id](../../t-sql/functions/object-id-transact-sql.md) , чтобы указать значения для параметров *database_id* и *object_id* . Однако передавая значения, которые не допустимы для этих функций, можно получить неожиданные результаты. Всегда следует проверять, что функции DB_ID и OBJECT_ID возвращают допустимый идентификатор. Дополнительные сведения см. в подразделе "Примечания" раздела [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).    
    
## <a name="permissions"></a>Разрешения    
 Необходимы следующие разрешения:    
    
-   `CONTROL` разрешение на указанный объект в базе данных    
    
-   `VIEW DATABASE STATE` разрешение на возврат сведений обо всех объектах в указанной базе данных с помощью шаблона объекта @*object_id* = null.    
    
-   `VIEW SERVER STATE` разрешение на возврат сведений обо всех базах данных с помощью шаблона базы данных @*database_id* = null    
    
 Предоставление `VIEW DATABASE STATE` позволяет вернуть все объекты в базе данных независимо от того, запрещены ли разрешения на управление для конкретных объектов.    
    
 Запрет запрещает `VIEW DATABASE STATE` возвращение всех объектов в базе данных независимо от любых разрешений на управление, предоставленных определенным объектам. Кроме того, если указан шаблон базы данных `@database_id=NULL` , то база данных опускается.    
    
 Дополнительные сведения см. в разделе [динамические административные представления и функции &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).    
    
## <a name="examples"></a>Примеры    
    
### <a name="a-returning-information-for-a-specified-table"></a>A. Возвращение данных для указанной таблицы    
 В следующем примере возвращаются сведения по всем индексам и секциям таблицы `Person.Address` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Выполнение этого запроса требует как минимум разрешения CONTROL на таблицу `Person.Address`.    
    
> [!IMPORTANT]    
> При использовании [!INCLUDE[tsql](../../includes/tsql-md.md)] функций DB_ID и OBJECT_ID для возврата значения параметра необходимо убедиться в правильности возвращаемого идентификатора. Если имя базы данных или объекта не может быть найдено, например если база данных или объект не существуют или неправильно записаны, то обе функции возвратят значение NULL. Функция **sys.dm_db_index_operational_stats** интерпретирует значение NULL как значение шаблона, указывающего все базы данных или все объекты. Так как эта операция может быть непреднамеренной, примеры в этом разделе демонстрируют безопасный способ определения идентификаторов базы данных и объекта.    
    
```sql    
DECLARE @db_id int;    
DECLARE @object_id int;    
SET @db_id = DB_ID(N'AdventureWorks2012');    
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');    
IF @db_id IS NULL     
  BEGIN;    
    PRINT N'Invalid database';    
  END;    
ELSE IF @object_id IS NULL    
  BEGIN;    
    PRINT N'Invalid object';    
  END;    
ELSE    
  BEGIN;    
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);    
  END;    
GO    
```    
    
### <a name="b-returning-information-for-all-tables-and-indexes"></a>Б. Возвращение сведений для всех таблиц и индексов    
 В следующем примере возвращаются сведения по всем таблицам и индексам в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Выполнение этого запроса требует разрешения VIEW SERVER STATE.    
    
```sql    
SELECT * FROM sys.dm_db_index_operational_stats( NULL, NULL, NULL, NULL);    
GO        
```    
    
## <a name="see-also"></a>См. также:    
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
 [Динамические административные представления и функции, связанные с индексами &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)     
 [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)     
 [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)     
 [sys.dm_os_latch_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)     
 [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)     
 [sys.allocation_units (Transact-SQL)](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)    
    
