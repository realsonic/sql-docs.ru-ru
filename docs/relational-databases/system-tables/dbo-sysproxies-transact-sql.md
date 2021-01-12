---
description: dbo.sysproxies (Transact-SQL)
title: dbo.sysпрокси (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxies_TSQL
- sysproxies_TSQL
- dbo.sysproxies
- sysproxies
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxies system table
ms.assetid: a73da875-be22-45fc-b5e2-ea7ebd48e2d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3d04cd1822a9c7e0edc309a3500e800b2df58fd0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098324"
---
# <a name="dbosysproxies-transact-sql"></a>dbo.sysproxies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Определяет атрибуты учетной записи-посредника агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Идентификатор учетной записи-посредника.|  
|**name**|**sysname**|Имя учетной записи-посредника.|  
|**credential_id**|**int**|Идентификатор учетных данных, используемых учетной записью-посредником.|  
|**доступной**|**tinyint**|Состояние учетной записи-посредника:<br /><br /> **0** = отключено. **1** = включено.|  
|**description**|**nvarchar(512)**|Описание, введенное пользователем при создании учетной записи-посредника.|  
|**user_sid**|**varbinary(85)**|*Security_identifier* Microsoft Windows для пользователя или группы, связанных с учетными данными прокси-сервера.|  
|**credential_date_created**|**datetime**|Дата и время создания учетных данных.|  
  
## <a name="remarks"></a>Комментарии  
 Только члены предопределенной роли сервера **sysadmin** могут обращаться к таблице **sysproxies** .  
  
## <a name="see-also"></a>См. также:  
 [dbo.sysпроксилогин &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)   
 [dbo.sysпроксисубсистем &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [dbo.sysподсистемы &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)  
  
  
