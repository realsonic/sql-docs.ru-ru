---
description: dbo.sysproxylogin (Transact-SQL)
title: dbo.sysпроксилогин (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxylogin_TSQL
- sysproxylogin_TSQL
- dbo.sysproxylogin
- sysproxylogin
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxylogin system table
ms.assetid: 433d33cb-bdf2-47bb-af78-2a40b7c8dfce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7e1115caf380e82ee004a460f29a7b486a70affc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098314"
---
# <a name="dbosysproxylogin-transact-sql"></a>dbo.sysproxylogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Регистрирует текущее соответствие учетных записей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] учетным записям-посредникам агента SQL Server. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Идентификатор учетной записи-посредника агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это значение соответствует столбцу **proxy_id** в таблице **sysproxies** .|  
|**трансляцию**|**varbinary(85)**|*Security_identifier* Microsoft Windows для SQL Server имени входа.|  
|**principal_id**|**int**|Идентификатор пользователя или группы, которая имеет разрешение на использование учетной записи-посредника для указанного шага подсистемы.|  
|**flags**|**int**|Тип имени входа:<br /><br /> **0** = пользователь или группа Windows, а также [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имя входа.<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Фиксированная системная роль<br /><br /> **2**  =  роль базы данных **msdb**|  
  
## <a name="remarks"></a>Комментарии  
 Только члены предопределенной роли сервера **sysadmin** могут обращаться к этой таблице.  
  
## <a name="see-also"></a>См. также:  
 [dbo.sysпрокси &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
  
  
