---
description: MSmerge_identity_range_allocations (Transact-SQL)
title: MSmerge_identity_range_allocations (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_identity_range_allocations
- MSmerge_identity_range_allocations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_identity_range_allocations system table
ms.assetid: 6362e35e-0ab3-4638-855b-1ce013f5fd6d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 639652550d17b7a3864404c78ffd4ced7a1317fd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100583"
---
# <a name="msmerge_identity_range_allocations-transact-sql"></a>MSmerge_identity_range_allocations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSmerge_identity_range_allocations** используется для мониторинга журнала назначений диапазонов идентификаторов как для издателей, так и для подписчиков для опубликованных статей. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Идентификатор издателя.|  
|**publisher_db**|**nvarchar(128)**|Имя базы данных публикации.|  
|**публикации**|**nvarchar(128)**|Имя публикации.|  
|**статья**|**nvarchar(128)**|Имя статьи.|  
|**абонент**|**nvarchar(128)**|Имя подписчика.|  
|**subscriber_db**|**nvarchar(128)**|Имя базы данных подписки.|  
|**is_pub_range**|**bit**|Показывает, назначен ли диапазон идентификаторов издателю.|  
|**ranges_allocated**|**tinyint**|Число назначенных диапазонов идентификаторов.|  
|**range_begin**|**numeric (38)**|Начальное значение диапазона.|  
|**range_end**|**numeric (38)**|Конечное значение диапазона.|  
|**next_range_begin**|**numeric (38)**|Начальное значение следующего назначаемого диапазона.|  
|**next_range_end**|**numeric (38)**|Конечное значение следующего назначаемого диапазона.|  
|**max_used**|**numeric (38)**|Наибольшее использованное значение идентификатора.|  
|**time_of_allocation**|**datetime**|Время назначения.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
