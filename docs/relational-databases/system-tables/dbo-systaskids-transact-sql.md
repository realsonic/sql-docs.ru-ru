---
description: dbo.systaskids (Transact-SQL)
title: dbo.sysтаскидс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- systaskids_TSQL
- dbo.systaskids
- systaskids
- dbo.systaskids_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- systaskids system table
ms.assetid: 45c56d89-4160-4d84-80bf-a7a05488792d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1e98a4a8a7e41b584fa9a397aa2aa98906d45bd1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093784"
---
# <a name="dbosystaskids-transact-sql"></a>dbo.systaskids (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит сопоставление задач, созданных в более ранних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с заданиями среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] текущей версии. Эта таблица хранится в базе данных **msdb** .  
  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**task_id**|**int**|Идентификатор задачи.|  
|**job_id**|**uniqueidentifier**|Идентификатор задания, с которым сопоставляется задача.|  
  
  
