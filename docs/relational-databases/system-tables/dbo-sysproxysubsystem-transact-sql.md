---
description: dbo.sysproxysubsystem (Transact-SQL)
title: dbo.sysпроксисубсистем (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxysubsystem_TSQL
- dbo.sysproxysubsystem
- sysproxysubsystem_TSQL
- sysproxysubsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxysubsystem system table
ms.assetid: 6d7713f5-1253-4a19-b1fb-635c377c95c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 700d55e45dc153834cfb12aa9883f8faa6fac909
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098322"
---
# <a name="dbosysproxysubsystem-transact-sql"></a>dbo.sysproxysubsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит записи, используемые подсистемой агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для каждой из учетных записей-посредников. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Идентификатор подсистемы. Это значение соответствует столбцу **subsystem_id** в таблице **заполнения таблицы syssubsystems** .|  
|**proxy_id**|**int**|Идентификатор учетной записи-посредника. Это значение соответствует столбцу **proxy_id** в таблице **sysproxies** .|  
  
## <a name="remarks"></a>Комментарии  
 Только члены предопределенной роли сервера **sysadmin** могут обращаться к этой таблице.  
  
## <a name="see-also"></a>См. также:  
 [dbo.sysподсистемы &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)   
 [dbo.sysпрокси &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
  
  
