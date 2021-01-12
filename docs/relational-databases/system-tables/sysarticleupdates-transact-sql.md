---
description: sysarticleupdates (Transact-SQL)
title: sysarticleupdates (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysarticleupdates_TSQL
- sysarticleupdates
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticleupdates system table
ms.assetid: 11a53bcd-a215-4d0b-9db8-233981d3ef5d
author: cawrites
ms.author: chadam
ms.openlocfilehash: a54ffa1056efb88336a776cf2a243c4e495a2d8d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096101"
---
# <a name="sysarticleupdates-transact-sql"></a>sysarticleupdates (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит одну строку для каждой статьи, поддерживающей немедленно обновляемые подписки. Эта таблица хранится в реплицируемой базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Столбец идентификаторов, который предоставляет уникальный идентификационный номер для статьи.|  
|**pubid**|**int**|Идентификатор публикации, к которой принадлежит статья.|  
|**sync_ins_proc**|**int**|Идентификатор хранимой процедуры, обрабатывающей синхронизирующие транзакции вставки. |  
|**sync_upd_proc**|**int**|Идентификатор хранимой процедуры, обрабатывающей синхронизирующие транзакции обновления. |  
|**sync_del_proc**|**int**|Идентификатор хранимой процедуры, обрабатывающей синхронизирующие транзакции удаления. |  
|**аутожен**|**bit**|Показывает, что хранимые процедуры автоматически созданы.<br /><br /> **0** = false, не автоматически.<br /><br /> **1** = true, автоматически.|  
|**sync_upd_trig**|**int**|Идентификатор триггера автоматического управления версиями в таблице статьи. |  
|**conflict_tableid**|**int**|Идентификатор таблицы конфликтов.|  
|**ins_conflict_proc**|**int**|Идентификатор процедуры, используемой для записи конфликта в **conflict_table**.|  
|**identity_support**|**bit**|Определяет, включена ли автоматическая обработка диапазона идентификаторов при использовании очереди обновления.  **0** означает, что диапазон идентификаторов не поддерживается.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
