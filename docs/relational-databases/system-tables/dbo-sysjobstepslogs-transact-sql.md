---
description: dbo.sysjobstepslogs (Transact-SQL)
title: dbo.sysжобстепслогс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobstepslogs_TSQL
- sysjobstepslogs_TSQL
- sysjobstepslogs
- dbo.sysjobstepslogs
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobstepslogs system table
ms.assetid: 128c25db-0b71-449d-bfb2-38b8abcf24a0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 815133f093a108d7f1e4b9841ffbd3310b3dd758
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097446"
---
# <a name="dbosysjobstepslogs-transact-sql"></a>dbo.sysjobstepslogs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит журнал шагов задания для всех шагов задания агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые настроены для записи выхода шага задания в таблицу. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**log_id**|**int**|Идентификатор журнала шагов задания.|  
|**Журналь**|**nvarchar(max)**|Содержимое журнала шагов задания.|  
|**date_created**|**datetime**|Дата и время создания журнала шагов задания.|  
|**date_modified**|**datetime**|Дата и время последнего изменения журнала шагов задания.|  
|**log_size**|**int**|Размер журнала шагов задания в байтах.|  
|**step_uid**|**uniqueidentifier**|Уникальный идентификатор шага задания.|  
  
## <a name="see-also"></a>См. также:  
 [sp_help_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [sp_delete_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobsteplog-transact-sql.md)  
  
  
