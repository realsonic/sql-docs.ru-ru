---
description: sys.dm_xe_session_targets (Transact-SQL)
title: sys.dm_xe_session_targets (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_xe_session_targets
- dm_xe_session_targets_TSQL
- dm_xe_session_targets
- sys.dm_xe_session_targets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_session_targets dynamic management view
- extended events [SQL Server], views
ms.assetid: 76fbc3e1-ad88-4a47-8bf1-471c3bee5ad8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 041cf14bcefff0d294cd7c4f4f037870d1513552
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098881"
---
# <a name="sysdm_xe_session_targets-transact-sql"></a>sys.dm_xe_session_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о целевых объектах сеанса.  
  
  |Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8)**|Адрес сеанса событий в памяти. Обеспечивает связь «многие к одному» со столбцом sys.dm_xe_sessions.address. Не допускает значение NULL.|  
|target_name|**nvarchar(60)**|Имя цели в сеансе. Не допускает значение NULL.|  
|target_package_guid|**uniqueidentifier**|Идентификатор GUID пакета, в котором содержится цель. Не допускает значение NULL.|  
|execution_count|**bigint**|Количество выполнений цели для сеанса. Не допускает значение NULL.|  
|execution_duration_ms|**bigint**|Общее время в миллисекундах, в течение которого выполнялась цель. Не допускает значение NULL.|  
|target_data|**nvarchar(max)**|Данные, предоставляемые целью, такие как сведения статистической обработки событий. Допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
### <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_session_targets.event_session_address|sys.dm_xe_sessions.address|«многие к одному»|  
  
## <a name="change-history"></a>Журнал изменений  
  
|Обновленное содержимое|  
|---------------------|  
|Исправлен тип данных для столбца target_data.|  
|Исправлено описание для столбца target_data, указывающее, что он допускает значения NULL.|  
|Исправлена таблица «Количество элементов связей».|  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

