---
description: IHsubscriptions (Transact-SQL)
title: Ихсубскриптионс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHsubscriptions_TSQL
- IHsubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- IHsubscriptions system table
ms.assetid: 9ec21119-35f1-4e39-abaa-b2c790c485b1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 163bf870e26fefc58181a5d3abcd28960195e2e5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097449"
---
# <a name="ihsubscriptions-transact-sql"></a>IHsubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Системная таблица **ихсубскриптионс** содержит по одной строке для каждой подписки на публикацию от издателя, не являющегося SQL Server, с помощью текущего распространителя. Эта таблица хранится в базе данных распространителя.  
  
## <a name="definition"></a>Определение  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|Однозначно идентифицирует опубликованную статью.|  
|**srvid**|**smallint**|Идентификатор сервера подписчика.|  
|**dest_db**|**sysname**|Имя целевой базы данных|  
|**login_name**|**sysname**|Имя входа, использованное при добавлении подписки.|  
|**distribution_jobid**|**двоичный (16)**|Идентификатор задания для агента распространителя|  
|**timestamp**|**timestamp**|Дата и время создания подписки.|  
|**queued_reinit**|**bit**|Определяет, помечена ли статья для инициализации или повторной инициализации. Значение **1** указывает, что статья, на которую оформлена подписка, помечена как для инициализации, так и для повторной инициализации.|  
|**status**|**tinyint**|Состояние подписки.<br /><br /> **0** = неактивно.<br /><br /> **1** = подписка выполнена.<br /><br /> **2** = активно.|  
|**sync_type**|**tinyint**|Тип начальной синхронизации.<br /><br /> **1** = автоматический.<br /><br /> **2** = нет.|  
|**subscription_type**|**int**|Тип подписки.<br /><br /> **0** = Push-агент распространителя запускается на подписчике.<br /><br /> **1** = по запросу — агент распространителя запускается на распространителе.|  
|**update_mode**|**tinyint**|Режим обновления.<br /><br /> **0** = только для чтения.<br /><br /> **1** = немедленное обновление.|  
|**loopback_detection**|**bit**|Применяется к подпискам, которые являются частью двунаправленной топологии репликации транзакций. Механизм распознавания обратной связи определяет, отправляет ли агент распространителя транзакции, созданные в подписчике, обратно подписчику:<br /><br /> **0** = отправляет обратно.<br /><br /> **1** = не отправляет обратно.|  
  
## <a name="see-also"></a>См. также  
 [Разнородная репликация базы данных](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
