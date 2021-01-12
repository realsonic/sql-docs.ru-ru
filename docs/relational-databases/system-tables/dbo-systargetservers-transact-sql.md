---
description: dbo.systargetservers (Transact-SQL)
title: dbo.sysтаржетсерверс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.systargetservers_TSQL
- dbo.systargetservers
- systargetservers_TSQL
- systargetservers
dev_langs:
- TSQL
helpviewer_keywords:
- systargetservers system table
ms.assetid: 479d1314-be37-4d19-ac9c-419fc9110e53
author: cawrites
ms.author: chadam
ms.openlocfilehash: 00e90f2c6911e1b80b37169fab67382f15997f03
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098254"
---
# <a name="dbosystargetservers-transact-sql"></a>dbo.systargetservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Записи, занесенные целевыми серверами в этот многосерверный операционный домен.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Идентификатор сервера.|  
|**server_name**|**sysname**|Имя сервера.|  
|**расположение**|**nvarchar(200)**|Расположение указанного целевого сервера.|  
|**time_zone_adjustment**|**int**|Интервал смещения времени, в часах, от среднего времени по Гринвичу (GMT).|  
|**enlist_date**|**datetime**|Дата и время, когда указанный сервер был занесен в список.|  
|**last_poll_date**|**datetime**|Дата и время последнего опроса **sysdownloadlist** системной таблицы многосерверной системы для выполнения заданий.|  
|**status**|**int**|Состояние целевого сервера:<br /><br /> **1** = обычная<br /><br /> **2** = ожидается повторная синхронизация<br /><br /> **4** = предположительно вне сети|  
|**local_time_at_last_poll**|**datetime**|Дата и время последнего обращения к целевому серверу за операциями заданий.|  
|**enlisted_by_nt_user**|**nvarchar (100)**|Имя пользователя, выполняющего **sp_msx_enlist** на целевом сервере.|  
|**poll_internal**|**int**|Количество секунд, которое должно пройти, прежде чем целевой сервер обратится к главному серверу за новыми инструкциями по загрузке.|  
  
  
