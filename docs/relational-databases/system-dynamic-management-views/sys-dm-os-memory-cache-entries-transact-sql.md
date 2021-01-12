---
description: sys.dm_os_memory_cache_entries (Transact-SQL)
title: sys.dm_os_memory_cache_entries (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_memory_cache_entries
- sys.dm_os_memory_cache_entries
- dm_os_memory_cache_entries_TSQL
- sys.dm_os_memory_cache_entries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_entries dynamic management view
ms.assetid: dd32be6b-10d1-4059-b4fd-0bf817f40d54
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bbab29fa5ed1080b52c149919e1d364969f962ef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101505"
---
# <a name="sysdm_os_memory_cache_entries-transact-sql"></a>sys.dm_os_memory_cache_entries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения обо всех записях в кэше в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используйте это представление, чтобы трассировать связь записей из кэша и ассоциированных с ними объектов. Кроме того, это представление можно использовать для получения статистики по записям в кэше.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_memory_cache_entries**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Адрес кэша. Не допускает значение NULL.|  
|**name**|**nvarchar(256)**|Имя кэша. Не допускает значение NULL.|  
|**type**|**varchar(60)**|Тип кэша. Не допускает значение NULL.|  
|**entry_address**|**varbinary(8)**|Адрес дескриптора записи кэша. Не допускает значение NULL.|  
|**entry_data_address**|**varbinary(8)**|Адрес данных пользователя в записи кэша.<br /><br /> 0x00000000 = адрес данных записи не доступен.<br /><br /> Не допускает значение NULL.|  
|**in_use_count**|**int**|Число пользователей, одновременно использующих эту запись кэша. Не допускает значение NULL.|  
|**is_dirty**|**bit**|Указывает, помечена ли эта запись кэша для удаления. 1 — помечена для удаления. Не допускает значение NULL.|  
|**disk_ios_count**|**int**|Число операций ввода-вывода в момент создания этой записи. Не допускает значение NULL.|  
|**context_switches_count**|**int**|Число переключателей контекста в момент создания этой записи. Не допускает значение NULL.|  
|**original_cost**|**int**|Исходная стоимость записи. Это значение представляет собой приблизительное число вызванных операций ввода-вывода, стоимость инструкции ЦП и объем памяти, потребляемой каждой записью. Чем выше стоимость, тем меньше вероятность того, что элемент будет удален из кэша. Не допускает значение NULL.|  
|**current_cost**|**int**|Текущая стоимость записи кэша. Это значение обновляется в процессе очистки записи. При повторном использовании записи текущая стоимость сбрасывается на исходное значение. Не допускает значение NULL.|  
|**memory_object_address**|**varbinary(8)**|Адрес ассоциированного объекта памяти. Допускает значение NULL.|  
|**pages_allocated_count**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Количество восьмикилобайтных страниц для хранения в этой записи кэша. Не допускает значение NULL.|  
|**pages_kb**|**bigint**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Объем памяти (в килобайтах), используемый этой записью кэша.  Не допускает значение NULL.|  
|**entry_data**|**nvarchar (2048)**|Сериализованное представление кэшированной записи. Эти сведения зависят от хранения кэша. Допускает значение NULL.|  
|**pool_id**|**int**|**Область применения**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и более поздних версий.<br /><br /> Пул ресурсов связан с записью. Допускает значение NULL.<br /><br /> не katmai|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения 

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также:  
 
  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


