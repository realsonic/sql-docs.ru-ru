---
description: MSsubscription_agents (Transact-SQL)
title: MSsubscription_agents (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_agents
- MSsubscription_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_agents system table
ms.assetid: 86ad5891-0bef-4963-9381-7d5b45245a0c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e8569eba1925879f2f757953f36e4ae632292100
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094730"
---
# <a name="mssubscription_agents-transact-sql"></a>MSsubscription_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSsubscription_agents** таблица используется агент распространения и триггерами обновляемых подписок для наблюдения за свойствами подписки. Эта таблица хранится в базе данных подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Идентификатор строки.|  
|**publisher**|**sysname**|Имя издателя.|  
|**publisher_db**|**sysname**|Имя базы данных публикации.|  
|**публикации**|**sysname**|Имя публикации.|  
|**subscription_type**|**int**|Тип подписки:<br /><br /> 0 = принудительная<br /><br /> 1 = по запросу;<br /><br /> 2 = анонимная по запросу.|  
|**queue_id**|**sysname**|Идентификатор [!INCLUDE[msCoName](../../includes/msconame-md.md)] очереди сообщений на издателе. для *queue_id* задано значение **SQL** для обновления посредством очередей на основе SQL.|  
|**update_mode**|**tinyint**|Тип обновления:<br /><br /> **0** = только для чтения.<br /><br /> **1** = немедленное обновление.<br /><br /> **2** = обновление в очереди с помощью очереди сообщений.<br /><br /> **3** = немедленное обновление с очередью обновлений как отработка отказа с помощью очереди сообщений.<br /><br /> **4** = обновление в очереди с использованием очереди SQL Server.<br /><br /> **5** = немедленное обновление с отработкой отказа обновления посредством очередей с помощью очереди SQL Server.|  
|**failover_mode**|**bit**|Если выбрано обновление с отработкой отказа, может быть выбран следующий тип отработки отказа:<br /><br /> **0** = используется немедленное обновление. Отработка отказа не включена.<br /><br /> **1** = используется обновление в очереди. Отработка отказа включена. Очередь, используемая для отработки отказа, указывается в значении *update_mode* .|  
|**spid**|**int**|Идентификатор системного процесса для подключения, используемый агентом распространителя, выполняющимся в настоящее время или только что запущенным.|  
|**login_time**|**datetime**|Дата и время подключения агента распространителя, выполняющегося в настоящее время или только что запущенного.|  
|**allow_subscription_copy**|**bit**|Указывает, допускается ли возможность копирования базы данных подписки.|  
|**attach_state**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**attach_version**|**двоичный (16)**|Уникальный идентификатор, представляющий версию вложенной подписки.|  
|**last_sync_status**|**int**|Последнее состояние выполнения агента распространителя, выполняющегося в настоящее время или только что запущенного. Состояние может быть следующим:<br /><br /> **1** = запущено.<br /><br /> **2** = успех.<br /><br /> **3** = выполняется.<br /><br /> **4** = бездействие.<br /><br /> **5** = повторная попытка.<br /><br /> **6** = ошибка.|  
|**last_sync_summary**|**sysname**|Последнее сообщение агента распространителя, выполняющегося в настоящее время или только что запущенного. Состояние может быть следующим:<br /><br /> **Начинать.**<br /><br /> **Успешно.**<br /><br /> **Выполняется.**<br /><br /> **Выключен.**<br /><br /> **Повторите.**<br /><br /> **Cчетчик.**|  
|**last_sync_time**|**datetime**|Дата и время обновления столбцов *last_sync_summary* и *last_sync_status* . Агенты распространителя по запросу или анонимные, выполняющиеся как задания службы агента SqlServer, не обновляют эти столбцы. Вместо этого данные журнала заносятся в таблицу журнала заданий.|  
|**queue_server**|**sysname**|Только для внутреннего использования.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)  
  
  
