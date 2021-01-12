---
description: sys.dm_os_memory_objects (Transact-SQL)
title: sys.dm_os_memory_objects (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_memory_objects
- sys.dm_os_memory_objects
- sys.dm_os_memory_objects_TSQL
- dm_os_memory_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_objects dynamic management view
ms.assetid: 5688bcf8-5da9-4ff9-960b-742b671d7096
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 274127f7fb255f6a0be4fe2cc78ca12519cae813
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101516"
---
# <a name="sysdm_os_memory_objects-transact-sql"></a>sys.dm_os_memory_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает объекты памяти, которые выделяются в настоящее время [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **Sys.dm_os_memory_objects** можно использовать для анализа использования памяти и выявления возможных утечек памяти.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**memory_object_address**|**varbinary(8)**|Адрес объекта памяти. Не допускает значение NULL.|  
|**parent_address**|**varbinary(8)**|Адрес родительского объекта памяти. Допускает значение NULL.|  
|**pages_allocated_count**|**int**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Число страниц, выделенных данным объектом. Не допускает значение NULL.|  
|**pages_in_bytes**|**bigint**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Объем памяти в байтах, выделяемый этим экземпляром объекта памяти. Не допускает значение NULL.|  
|**creation_options**|**int**|Только для внутреннего использования. Допускает значение NULL.|  
|**bytes_used**|**bigint**|Только для внутреннего использования. Допускает значение NULL.|  
|**type**|**nvarchar(60)**|Тип объекта памяти.<br /><br /> Указывает компонент, которому принадлежит данный объект памяти, либо функцию объекта памяти. Допускает значение NULL.|  
|**name**|**varchar(128)**|Только для внутреннего использования. Допускает значение NULL.|  
|**memory_node_id**|**smallint**|Идентификатор узла памяти, используемого данным объектом памяти. Не допускает значение NULL.|  
|**creation_time**|**datetime**|Только для внутреннего использования. Допускает значение NULL.|  
|**max_pages_allocated_count**|**int**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Максимальное число страниц, выделенных данным объектом. Не допускает значение NULL.|  
|**page_size_in_bytes**|**int**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Размер страниц в байтах, выделенных данным объектом. Не допускает значение NULL.|  
|**max_pages_in_bytes**|**bigint**|Максимальный объем памяти, который когда-либо использовался данным объектом памяти. Не допускает значение NULL.|  
|**page_allocator_address**|**varbinary(8)**|Адрес средства выделения страниц в памяти. Не допускает значение NULL. Дополнительные сведения см. в разделе [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**creation_stack_address**|**varbinary(8)**|Только для внутреннего использования. Допускает значение NULL.|  
|**sequence_num**|**int**|Только для внутреннего использования. Допускает значение NULL.|  
|**partition_type**|**int**|**Область применения**: [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] и более поздних версий.<br /><br /> Тип раздела:<br /><br /> 0 — объект памяти, не являющийся секционированным<br /><br /> 1 — объект секционированной памяти, который в настоящее время не секционирован<br /><br /> 2-секционный объект памяти, секционированный по узлу NUMA. В среде с одним узлом NUMA это эквивалентно 1.<br /><br /> 3-секционный объект памяти, секционированный по ЦП.|  
|**contention_factor**|**real**|**Область применения**: [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] и более поздних версий.<br /><br /> Значение, указывающее состязание за этот объект памяти, при этом 0 означает отсутствие конкуренции. Значение обновляется каждый раз, когда заданное число выделений памяти отражает состязание за этот период. Применяется только к потокобезопасным объектам памяти.|  
|**waiting_tasks_count**|**bigint**|**Область применения**: [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] и более поздних версий.<br /><br /> Число ожиданий этого объекта памяти. Этот счетчик увеличивается при выделении памяти из этого объекта памяти. Шаг приращения — это число задач, ожидающих доступа к этому объекту памяти. Применяется только к потокобезопасным объектам памяти. Это наиболее актуальное значение без гарантии правильности.|  
|**exclusive_access_count**|**bigint**|**Область применения**: [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] и более поздних версий.<br /><br /> Указывает, как часто данный объект памяти был доступен исключительно. Применяется только к потокобезопасным объектам памяти.  Это наиболее актуальное значение без гарантии правильности.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
 **partition_type**, **contention_factor**, **waiting_tasks_count** и **exclusive_access_count** еще не реализованы в [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] .  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="remarks"></a>Комментарии  
 Объекты памяти представляют собой кучи. Они обеспечивают выделение памяти с большей точностью, чем клерки памяти. Компоненты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используют вместо клерков памяти объекты памяти. Объекты памяти для выделения страниц используют интерфейс средства выделения памяти от клерка памяти. Виртуальные или общие интерфейсы памяти объектами памяти не используются. В зависимости от шаблонов выделения памяти компоненты могут создавать различные типы объектов для выделения областей памяти произвольного размера.  
  
 Стандартный размер страницы для объекта памяти равен 8 КБ. Однако для добавочных объектов памяти размеры страниц могут варьироваться от 512 байт до 8 килобайт.  
  
> [!NOTE]  
>  Размер страницы не означает максимально возможный размер выделенной памяти. Он представляет собой гранулярность выделения страниц, поддерживаемую средством выделения и реализуемую клерком памяти. От объекта памяти можно запросить выделение объема свыше 8 килобайт.  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращается объем памяти, выделенный каждым типом объектов памяти.  
  
```  
SELECT SUM (pages_in_bytes) as 'Bytes Used', type   
FROM sys.dm_os_memory_objects  
GROUP BY type   
ORDER BY 'Bytes Used' DESC;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)  
  
  


