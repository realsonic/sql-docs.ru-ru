---
description: dbo.syssubsystems (Transact-SQL)
title: dbo.sysподсистемы (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.syssubsystems
- syssubsystems
- syssubsystems_TSQL
- dbo.syssubsystems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syssubsystems system table
ms.assetid: 114b3d55-1ad6-4777-b868-8ef0c86ba596
author: cawrites
ms.author: chadam
ms.openlocfilehash: ebd56d0ad3847aea411c27af5852d652b84e360e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098285"
---
# <a name="dbosyssubsystems-transact-sql"></a>dbo.syssubsystems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит сведения обо всех доступных подсистемах учетных записей-посредников агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Таблица **заполнения таблицы syssubsystems** хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Идентификатор подсистемы.|  
|**подсистемы**|**nvarchar(40)**|Имя подсистемы.|  
|**description_id**|**int**|Идентификатор сообщения строки в представлении каталога **sys. messages** , содержащей описание подсистемы.|  
|**subsystem_dll**|**nvarchar(255)**|Расположение подсистемы DLL.|  
|**agent_exe**|**nvarchar(255)**|Полный путь к исполняемым объектам, используемым подсистемой.|  
|**start_entry_point**|**nvarchar(30)**|Функция, которая вызывается при инициализации подсистемы.|  
|**event_entry_point**|**nvarchar(30)**|Функция, которая вызывается при запуске шага подсистемы.|  
|**stop_entry_point**|**nvarchar(30)**|Функция, которая вызывается при завершении работы подсистемы.|  
|**max_worker_threads**|**int**|Максимальное число одновременных шагов для заданной подсистемы.|  
  
## <a name="remarks"></a>Комментарии  
 Только члены предопределенной роли сервера **sysadmin** могут обращаться к этой таблице.  
  
## <a name="see-also"></a>См. также:  
 [dbo.sysпроксисубсистем &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [dbo.sysпрокси &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)  
  
  
