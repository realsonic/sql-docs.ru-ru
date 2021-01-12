---
description: sys.resource_governor_workload_groups (Transact-SQL)
title: sys.resource_governor_workload_groups (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_workload_groups
- resource_governor_workload_groups_TSQL
- sys.resource_governor_workload_groups_TSQL
- resource_governor_workload_groups
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_workload_groups catalog view
ms.assetid: 619ba4b7-868f-4784-b527-ec1dfd703c4f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 83d699e2da5e1de1e25a2618171924843c569188
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101767"
---
# <a name="sysresource_governor_workload_groups-transact-sql"></a>sys.resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает хранимую в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] конфигурацию группы рабочей нагрузки. Каждая группа рабочей нагрузки может иметь подписку только на один и только один пул ресурсов.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|group_id|**int**|Уникальный идентификатор группы рабочей нагрузки. Не допускает значение NULL.|  
|name|**sysname**|Имя группы рабочей нагрузки. Не допускает значение NULL.|  
|importance|**sysname**|**Примечание.** Важность применяется только к группам рабочей нагрузки в том же пуле ресурсов.<br /><br /> Относительная важность запроса в данной группе рабочей нагрузки. Параметр важность имеет одно из следующих значений, где MEDIUM — это значение по умолчанию: низкий, средний, высокий.<br /><br /> Не допускает значение NULL.|  
|request_max_memory_grant_percent|**int**|Максимальный объем предоставляемой памяти, в процентах, для отдельного запроса. По умолчанию используется значение 25. Не допускает значение NULL.<br /><br /> **Примечание.** Если этот параметр имеет значение выше 50%, крупные запросы будут выполняться по одному за раз. Поэтому повышается риск возникновения ошибки нехватки памяти при выполнении запроса.|  
|request_max_cpu_time_sec|**int**|Максимально допустимое использование ЦП, в секундах, для отдельного запроса. Значение по умолчанию равно 0 и означает отсутствие ограничений. Не допускает значение NULL.<br /><br /> **Примечание.** Дополнительные сведения см. в разделе [класс событий ЦП с превышением порогового значения](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|request_memory_grant_timeout_sec|**int**|Время ожидания предоставления памяти, в секундах, для отдельного запроса. Для значения по умолчанию 0 используется внутреннее вычисление, основанное на стоимости запроса. Не допускает значение NULL.|  
|max_dop|**int**|Максимальная степень параллелизма для группы рабочей нагрузки. Для значения по умолчанию 0 используются глобальные параметры. Не допускает значение NULL.<br /><br /> **Узел:** Этот параметр переопределит параметр запроса **MAXDOP**.|  
|group_max_requests|**int**|Максимальное число параллельных запросов. Значение по умолчанию равно 0 и означает отсутствие ограничений. Не допускает значение NULL.|  
|pool_id|**int**|Идентификатор пула ресурсов, используемого данной группой рабочей нагрузки.|  
|external_pool_id|**int**|**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.<br /><br /> ИДЕНТИФИКАТОР внешнего пула ресурсов, который используется этой группой рабочей нагрузки.|  
  
## <a name="remarks"></a>Комментарии  
 Представление каталога отображает хранимые метаданные. Чтобы просмотреть конфигурацию в памяти, используйте соответствующее динамическое административное представление [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md).  
  
 Сохраненная конфигурация и конфигурация, хранимая в памяти, могут различаться, если конфигурация регулятора ресурсов была изменена, но инструкция ALTER RESOURCE GOVERNOR RECONFIGURE не применялась.  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешение VIEW ANY DEFINITION для просмотра содержимого и разрешение CONTROL SERVER для изменения содержимого.  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_resource_governor_workload_groups (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Resource Governor представления каталога &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)  
  
  
