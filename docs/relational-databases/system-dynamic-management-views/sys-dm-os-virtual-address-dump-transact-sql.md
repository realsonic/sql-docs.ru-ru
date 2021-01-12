---
description: sys.dm_os_virtual_address_dump (Transact-SQL)
title: sys.dm_os_virtual_address_dump (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_virtual_address_dump
- sys.dm_os_virtual_address_dump_TSQL
- sys.dm_os_virtual_address_dump
- dm_os_virtual_address_dump_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_virtual_address_dump dynamic management view
ms.assetid: 7b24ea55-3873-42fd-a86c-441c92eb6175
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b73703774fcee16b5090ebfa9cd1df7d18ed32c0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095132"
---
# <a name="sysdm_os_virtual_address_dump-transact-sql"></a>sys.dm_os_virtual_address_dump (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Возвращает сведения о диапазоне страниц в виртуальном адресном пространстве вызывающего процесса.  
  
> [!NOTE]  
>  Эти сведения также возвращаются API-интерфейсом **VirtualQuery** Windows.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_virtual_address_dump**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**region_base_address**|**varbinary(8)**|Указатель на базовый адрес области страниц. Не допускает значение NULL.|  
|**region_allocation_base_address**|**varbinary(8)**|Указатель на базовый адрес диапазона страниц, выделенных с помощью функции Windows API VirtualAlloc.  Страница, на которую указывает элемент BaseAddress, содержится внутри этого выделенного диапазона. Не допускает значение NULL.|  
|**region_allocation_protection**|**varbinary(8)**|Атрибуты защиты при первом выделении области. Значение может быть одним из следующих:<br /><br /> — PAGE_READONLY<br />— PAGE_READWRITE<br />— PAGE_NOACCESS<br />— PAGE_WRITECOPY<br />— PAGE_EXECUTE<br />— PAGE_EXECUTE_READ<br />— PAGE_EXECUTE_READWRITE<br />— PAGE_EXECUTE_WRITECOPY<br />— PAGE_GUARD<br />— PAGE_NOCACHE<br /><br /> Не допускает значение NULL.|  
|**region_size_in_bytes**|**bigint**|Размер области в байтах, начиная с базового адреса, в которой все страницы имеют одинаковые атрибуты. Не допускает значение NULL.|  
|**region_state**|**varbinary(8)**|Текущее состояние области. Возможны следующие варианты.<br /><br /> — MEM_COMMIT<br />— MEM_RESERVE<br />— MEM_FREE<br /><br /> Не допускает значение NULL.|  
|**region_current_protection**|**varbinary(8)**|Атрибуты защиты. Значение может быть одним из следующих:<br /><br /> — PAGE_READONLY<br />— PAGE_READWRITE<br />— PAGE_NOACCESS<br />— PAGE_WRITECOPY<br />— PAGE_EXECUTE<br />— PAGE_EXECUTE_READ<br />— PAGE_EXECUTE_READWRITE<br />— PAGE_EXECUTE_WRITECOPY<br />— PAGE_GUARD<br />— PAGE_NOCACHE<br /><br /> Не допускает значение NULL.|  
|**region_type**|**varbinary(8)**|Определяет типы страниц в области. Он может иметь одно из следующих значений:<br /><br /> — MEM_PRIVATE<br />— MEM_MAPPED<br />— MEM_IMAGE<br /><br /> Не допускает значение NULL.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


