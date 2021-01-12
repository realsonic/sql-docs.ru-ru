---
description: sys.dm_os_memory_cache_counters (Transact-SQL)
title: sys.dm_os_memory_cache_counters (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_cache_counters_TSQL
- dm_os_memory_cache_counters_TSQL
- sys.dm_os_memory_cache_counters
- dm_os_memory_cache_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_counters dynamic management view
ms.assetid: ca7bd036-d661-4c17-b00a-e1a975bd8932
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b35568bdef71d81918a3c672682e1e6df1fb9da1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092774"
---
# <a name="sysdm_os_memory_cache_counters-transact-sql"></a>sys.dm_os_memory_cache_counters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает моментальный снимок исправности кэша в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **sys.dm_os_memory_cache_counters** предоставляет сведения времени выполнения о выделенных записях кэша, их использовании и источнике памяти для записей кэша.  
  
> **Примечание.** Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_memory_cache_counters**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Указывает адрес (первичный ключ) счетчиков, связанных с указанным кэшем. Не допускает значение NULL.|  
|**name**|**nvarchar(256)**|Указывает имя кэша. Не допускает значение NULL.|  
|**type**|**nvarchar(60)**|Указывает тип кэша, связанного с этой записью. Не допускает значение NULL.|  
|**single_pages_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Размер одной выделенной страницы памяти в килобайтах. Объем памяти, выделенный с помощью одностраничного блока распределения. Это относится к 8-килобайтным страницам, взятым прямо из буферного пула для этого кэша. Не допускает значение NULL.|  
|**pages_kb**|**bigint**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Указывает объем (в килобайтах) памяти, выделенной в кэш. Не допускает значение NULL.|  
|**multi_pages_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Размер выделенной многостраничной памяти в килобайтах. Это объем памяти, выделенной с помощью многостраничного блока распределения узла памяти. Эта память выделена вне буферного пула и использует преимущества виртуального блока распределения узлов памяти. Не допускает значение NULL.|  
|**pages_in_use_kb**|**bigint**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Указывает объем (в килобайтах) памяти, выделенной и используемой в кэше. Допускает значение NULL.  Значения для объектов типа `USERSTORE_<*>` не отслеживаются.  Для них выводится значение NULL.|  
|**single_pages_in_use_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Размер используемой одностраничной памяти в килобайтах. Допускает значение NULL. Эти сведения не отписываются для объектов типа USERSTORE_ \<*> и эти значения будут иметь значение null.|  
|**multi_pages_in_use_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Размер используемой многостраничной памяти в килобайтах. Допускает значение NULL. Эти сведения не отписываются для объектов типа USERSTORE_ \<*> , и эти значения будут иметь значение null.|  
|**entries_count**|**bigint**|Указывает количество записей в кэше. Не допускает значение NULL.|  
|**entries_in_use_count**|**bigint**|Указывает количество записей в используемом кэше. Не допускает значение NULL.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения 

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также:  
  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


