---
description: sys.dm_os_cluster_nodes (Transact-SQL)
title: sys.dm_os_cluster_nodes (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes
- sys.dm_os_cluster_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_cluster_nodes dynamic management view
ms.assetid: 92fa804e-2d08-42c6-a36f-9791544b1d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5276063293d8356902f0afcbf741281bedc24f6d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101541"
---
# <a name="sysdm_os_cluster_nodes-transact-sql"></a>sys.dm_os_cluster_nodes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает строку для каждого узла в конфигурации экземпляра отказоустойчивого кластера. Если текущий экземпляр является экземпляром отказоустойчивого кластера, то возвращается список узлов, в которых определен этот экземпляр отказоустойчивого кластера (прежде «виртуальный сервер»). Если текущий экземпляр сервера не является кластеризованным экземпляром отработки отказа, то возвращается пустой набор строк.  
  
> **Примечание.** Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_cluster_nodes**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**NodeName**|**sysname**|Имя узла в конфигурации экземпляра отказоустойчивого кластера (виртуального сервера) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|status|**int**|Состояние узла в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] экземпляре отказоустойчивого кластера: 0, 1, 2, 3,-1. Дополнительные сведения см. в разделе [функция жетклустернодестате](/windows/win32/api/clusapi/nf-clusapi-getclusternodestate).|  
|status_description|**nvarchar (20)**|Описание состояния узла отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = работает<br /><br /> 1 = остановлен<br /><br /> 2 = приостановлен<br /><br /> 3 = соединение<br /><br /> -1 = неизвестно|  
|is_current_owner|bit|1 означает, что этот узел является текущим владельцем ресурса отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="remarks"></a>Комментарии  
 Когда отказоустойчивый кластер включен, экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может работать на любом из узлов отказоустойчивого кластера, входящих в конфигурацию экземпляра отказоустойчивого кластера (виртуального сервера) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> **Примечание.** Это представление заменяет функцию fn_virtualservernodes, которая будет считаться устаревшей в будущем выпуске.  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения VIEW SERVER STATE на экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Примеры  
 В следующем примере sys. dm_os_cluster_nodes используется для возврата узлов экземпляра кластерного сервера.  
  
```  
SELECT NodeName, status, status_description, is_current_owner   
FROM sys.dm_os_cluster_nodes;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|NodeName|status|status_description|is_current_owner|  
|--------------|------------|-------------------------|------------------------|  
|node1|0|up|1|  
|node2|0|up|0|  
|Node3|1|работу|0|  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_os_cluster_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-properties-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [sys.fn_virtualservernodes &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-virtualservernodes-transact-sql.md)   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
