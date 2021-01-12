---
description: sys.dm_os_memory_cache_hash_tables (Transact-SQL)
title: sys.dm_os_memory_cache_hash_tables (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_cache_hash_tables_TSQL
- sys.dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_hash_tables dynamic management view
ms.assetid: 68b94f35-8f80-4d2b-bcde-7a21934219af
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d40340647e316a42ab7ad047502982da9fe90234
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096585"
---
# <a name="sysdm_os_memory_cache_hash_tables-transact-sql"></a>sys.dm_os_memory_cache_hash_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает строку для каждого активного кэша в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_memory_cache_hash_tables**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Адрес (первичный ключ) записи кэша. Не допускает значение NULL.|  
|**name**|**nvarchar(256)**|Имя кэша. Не допускает значение NULL.|  
|**type**|**nvarchar(60)**|Тип кэша. Не допускает значение NULL.|  
|**table_level**|**int**|Номер таблицы кэша. Частичный кэш может содержать множество хэшированных таблиц, которые соответствуют разным хэш-функциям. Не допускает значение NULL.|  
|**buckets_count**|**int**|Количество сегментов в хэш-таблице. Не допускает значение NULL.|  
|**buckets_in_use_count**|**int**|Количество сегментов, используемых в настоящий момент. Не допускает значение NULL.|  
|**buckets_min_length**|**int**|Минимальное количество вхождений кэша в сегменте. Не допускает значение NULL.|  
|**buckets_min_length**|**int**|Максимальное количество вхождений кэша в сегменте. Не допускает значение NULL.|  
|**buckets_avg_length**|**int**|Среднее количество вхождений кэша в каждом сегменте. Не допускает значение NULL.|  
|**buckets_max_length_ever**|**int**|Максимальное количество кэшированных записей в сегменте хэша для этой таблицы хэша с момента запуска сервера. Не допускает значение NULL.|  
|**hits_count**|**bigint**|Количество попаданий в кэш. Не допускает значение NULL.|  
|**misses_count**|**bigint**|Количество неудачных обращений к кэшу. Не допускает значение NULL.|  
|**buckets_avg_scan_hit_length**|**int**|Среднее количество проверенных записей в сегменте перед нахождением искомого элемента. Не допускает значение NULL.|  
|**buckets_avg_scan_miss_length**|**int**|Среднее количество проверенных записей в сегменте перед неудачным завершением поиска. Не допускает значение NULL.|  
|**pdw_node_id**|**int**|Идентификатор узла, на котором находится данное распределение.<br /><br /> **Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
## <a name="permissions"></a>Разрешения 

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также:  
 
  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


