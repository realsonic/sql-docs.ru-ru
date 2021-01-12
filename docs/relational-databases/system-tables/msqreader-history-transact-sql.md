---
description: MSqreader_history (Transact-SQL)
title: MSqreader_history (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSqreader_history
- MSqreader_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSqreader_history system table
ms.assetid: c5c91d39-513c-4a77-870b-c8ef74a1cd6b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 22053b5cae7f87c4cf2544ca0ed3ecc39dbda6a9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100591"
---
# <a name="msqreader_history-transact-sql"></a>MSqreader_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSqreader_history** содержит строки журнала для агентов чтения очереди, связанных с локальным распространителем. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|Идентификатор агента чтения очереди.|  
|**publication_id**|**int**|Идентификатор публикации.|  
|**runstatus**|**int**|Состояние выполнения агента:<br /><br /> **1** = запуск.<br /><br /> **2** = выполнена.<br /><br /> **3** = выполняется.<br /><br /> **4** = бездействие.<br /><br /> **5** = повторная попытка.<br /><br /> **6** = ошибка.|  
|**start_time**|**datetime**|Дата и время начала сеанса агента.|  
|**time**|**datetime**|Дата и время последнего сообщения, записанного в журнал.|  
|**duration**|**int**|Продолжительность зарегистрированной активности сеанса в секундах.|  
|**обсуждения**|**nvarchar(255)**|Описание.|  
|**transaction_id**|**nvarchar(40)**|Идентификатор транзакции, сохраненный с сообщением (если применимо).|  
|**transaction_status**|**int**|Состояние транзакции.|  
|**transactions_processed**|**int**|Совокупное число транзакций, обработанных за время сеанса.|  
|**commands_processed**|**int**|Совокупное число команд, обработанных за время сеанса.|  
|**delivery_rate**|**float(53)**|Среднее число доставленных команд в секунду.|  
|**transaction_rate**|**float(53)**|Скорость обработки транзакций.|  
|**абонент**|**sysname**|Имя подписчика.|  
|**SubscriberDB**|**sysname**|Имя базы данных подписки.|  
|**error_id**|**int**|Если не равен нулю, то число представляет [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сообщение об ошибке.|  
|**timestamp**|**timestamp**|Столбец отметок времени для данной таблицы.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
