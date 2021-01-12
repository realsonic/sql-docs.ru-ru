---
description: MSreplication_subscriptions (Transact-SQL)
title: MSreplication_subscriptions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_subscriptions
- MSreplication_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_subscriptions system table
ms.assetid: fd0c5843-4e9b-4448-8bfb-0a4067d1d8d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8d5d0b8b450102483c844ec53472ff509b75cd0d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092387"
---
# <a name="msreplication_subscriptions-transact-sql"></a>MSreplication_subscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSreplication_subscriptions** содержит одну строку сведений о репликации для каждого агент распространения обслуживания базы данных локального подписчика. Эта таблица хранится в базе данных подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Имя издателя.|  
|**publisher_db**|**sysname**|Имя базы данных издателя.|  
|**публикации**|**sysname**|Имя публикации.|  
|**independent_agent**|**bit**|Указывает, имеется ли для данной публикации изолированный агент распространителя.|  
|**subscription_type**|**int**|Тип подписки.<br /><br /> 0 = принудительная<br /><br /> 1 = по запросу<br /><br /> 2 = анонимная|  
|**distribution_agent**|**sysname**|Имя агента распространителя.|  
|**Time**|**smalldatetime**|Время последнего обновления агентом распространителя.|  
|**description**|**nvarchar(255)**|Описание подписки.|  
|**transaction_timestamp**|**varbinary (16)**|Только для внутреннего использования.|  
|**update_mode**|**tinyint**|Тип обновления.|  
|**agent_id**|**двоичный (16)**|Идентификатор агента.|  
|**subscription_guid**|**двоичный (16)**|Глобальный идентификатор для версии подписки на эту публикацию.|  
|**subid**|**двоичный (16)**|Глобальный идентификатор для анонимной подписки.|  
|**immediate_sync**|**bit**|Указывает, создаются ли повторно файлы синхронизации при каждом запуске агента моментальных снимков.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
