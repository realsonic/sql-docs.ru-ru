---
description: dbo.sysjobhistory (Transact-SQL)
title: dbo.sysjobhistory (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/24/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobhistory_TSQL
- dbo.sysjobhistory
- sysjobhistory
- sysjobhistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobhistory system table
ms.assetid: 1b1fcdbb-2af2-45e6-bf3f-e8279432ce13
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6fa83e3df6cf6f05420341c497c8b3bfd2a481df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098754"
---
# <a name="dbosysjobhistory-transact-sql"></a>dbo.sysjobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Содержит сведения о выполнении назначенных заданий агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
> [!NOTE]
> В большинстве случаев данные обновляются только после завершения шага задания, а таблица обычно не содержит записей для шагов задания, которые в настоящее время выполняются, но в некоторых случаях *базовые процессы предоставляют* сведения о выполняемых шагах задания.

Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Уникальный идентификатор строки.|  
|**job_id**|**uniqueidentifier**|Идентификатор задания.|  
|**step_id**|**int**|Идентификатор этапа в задании.|  
|**step_name**|**sysname**|Имя этапа.|  
|**sql_message_id**|**int**|Идентификатор любого сообщения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] об ошибке, возвращенного при сбое задания.|  
|**sql_severity**|**int**|Уровень серьезности любой ошибки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**message**|**nvarchar(4000)**|Текст ошибки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если он имеется.|  
|**run_status**|**int**|Состояние выполнения задания.<br /><br /> **0** = сбой<br /><br /> **1** = успех<br /><br /> **2** = повторная попытка<br /><br /> **3** = отменено<br /><br />**4** = выполняется|  
|**run_date**|**int**|Дата начала выполнения задания или этапа. Для внутрипроцессного журнала это дата и время записи журнала.|  
|**run_time**|**int**|Время начала задания или шага в формате **ЧЧММСС** .|  
|**run_duration**|**int**|Время, затраченное на выполнение задания или шага в формате **ЧЧММСС** .|  
|**operator_id_emailed**|**int**|Идентификатор оператора, уведомленного о завершении задания.|  
|**operator_id_netsent**|**int**|Идентификатор оператора, уведомленного при помощи сообщения о завершении задания.|  
|**operator_id_paged**|**int**|Идентификатор оператора, уведомленного по пейджеру о завершении задания.|  
|**retries_attempted**|**int**|Количество повторных попыток выполнения задания или этапа.|  
|**server**|**sysname**|Имя сервера, на котором выполнялось задание.|  
  
  ## <a name="example"></a>Пример
 Следующий [!INCLUDE[tsql](../../includes/tsql-md.md)] запрос преобразует **run_time** и **run_duration** столбцы в более понятный формат.  Выполните скрипт в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .
 
 ```sql
 SET NOCOUNT ON;
 
 SELECT sj.name,
        sh.run_date,
        sh.step_name,
        STUFF(STUFF(RIGHT(REPLICATE('0', 6) +  CAST(sh.run_time as varchar(6)), 6), 3, 0, ':'), 6, 0, ':') 'run_time',
        STUFF(STUFF(STUFF(RIGHT(REPLICATE('0', 8) + CAST(sh.run_duration as varchar(8)), 8), 3, 0, ':'), 6, 0, ':'), 9, 0, ':') 'run_duration (DD:HH:MM:SS)  '
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobhistory sh
ON sj.job_id = sh.job_id
```
