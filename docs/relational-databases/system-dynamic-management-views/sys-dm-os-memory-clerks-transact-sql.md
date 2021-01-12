---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
ms.assetid: 1d556c67-5c12-46d5-aa8c-7ec1bb858df7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8f6942f95ff8d0f229d52921a692ae7ec74c18f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101560"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает набор всех клерков памяти, активных в данный момент в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_memory_clerks**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary(8)**|Указывает уникальный адрес клерка памяти. Это первичный ключевой столбец. Не допускает значение NULL.|  
|**type**|**nvarchar(60)**|Указывает тип клерка памяти. Каждый клерк принадлежит к определенному типу, такому как CLR Clerks MEMORYCLERK_SQLCLR. Не допускает значение NULL.|  
|**name**|**nvarchar(256)**|Указывает внутреннее имя, назначенное данному клерку памяти. Компонент может иметь несколько клерков памяти определенного типа. Компонент может использовать определенные имена для идентификации клерков памяти одного и того же типа. Не допускает значение NULL.|  
|**memory_node_id**|**smallint**|Указывает идентификатор узла памяти. Не допускает значения NULL.|  
|**single_pages_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].|  
|**pages_kb**|**bigint**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Устанавливает объем страничной памяти (в килобайтах), выделяемый для этого клерка памяти. Не допускает значение NULL.|  
|**multi_pages_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Объем выделенной многостраничной памяти в КБ. Это объем памяти, выделенной с помощью механизма распределения множества страниц узлов памяти. Эта память выделена вне буферного пула и использует преимущества виртуального блока распределения узлов памяти. Не допускает значение NULL.|  
|**virtual_memory_reserved_kb**|**bigint**|Указывает объем виртуальной памяти, зарезервированной клерком памяти. Не допускает значение NULL.|  
|**virtual_memory_committed_kb**|**bigint**|Указывает объем виртуальной памяти, зафиксированной клерком памяти. Объем зафиксированной памяти должен всегда быть меньше объема зарезервированной памяти. Не допускает значение NULL.|  
|**awe_allocated_kb**|**bigint**|Указывает объем физической памяти (в КБ), заблокированной в физической памяти и не выгруженной операционной системой. Не допускает значение NULL.|  
|**shared_memory_reserved_kb**|**bigint**|Указывает объем общей памяти, зарезервированной клерком памяти. Объем памяти, зарезервированной для использования при сопоставлении общей памяти и файлов. Не допускает значение NULL.|  
|**shared_memory_committed_kb**|**bigint**|Указывает объем общей памяти, зафиксированной клерком памяти. Не допускает значение NULL.|  
|**page_size_in_bytes**|**bigint**|Указывает гранулярность выделения страниц для этого клерка памяти. Не допускает значение NULL.|  
|**page_allocator_address**|**varbinary(8)**|Указывает адрес средства выделения страниц. Этот адрес уникален для клерка памяти и может использоваться в **sys.dm_os_memory_objects** для нахождение объектов памяти, привязанных к этому Клерку. Не допускает значение NULL.|  
|**host_address**|**varbinary(8)**|Указывает адрес памяти, по которому размещается данный клерк памяти. Дополнительные сведения см. в разделе [sys.dm_os_hosts &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md). Компоненты, такие как [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент, обращаются к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ресурсам памяти через интерфейс узла.<br /><br /> 0x00000000 = Клерк памяти принадлежит [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Не допускает значение NULL.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения 

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="remarks"></a>Комментарии  
 Диспетчер памяти [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имеет трехуровневую иерархию. В нижней части иерархии располагаются узлы памяти. Средний уровень содержит клерки, кэш и пулы памяти. Верхний уровень содержит объекты памяти. Эти объекты обычно используются для выделения памяти в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Узлы памяти обеспечивают интерфейс и реализацию низкоуровневых механизмов выделения. В пределах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] доступ к узлам памяти имеют только клерки памяти. Клерки памяти получают доступ к интерфейсам узлов памяти для ее выделения. Узлы памяти также ведут слежение за выделяемой клерками памятью в целях диагностики. Каждый компонент, выделяющий значительный объем памяти, должен создать свой клерк памяти и выделить необходимую ему память с помощью интерфейсов клерка. Часто компоненты создают соответствующие им клерки во время запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>См. также:  

 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  
  


