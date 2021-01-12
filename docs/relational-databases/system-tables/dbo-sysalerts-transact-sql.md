---
description: dbo.sysalerts (Transact-SQL)
title: Оповещения dbo.sys(Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysalerts
- sysalerts_TSQL
- dbo.sysalerts_TSQL
- sysalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysalerts system table
ms.assetid: a2c2f50d-61f3-4951-996a-add5ad092cc2
author: cawrites
ms.author: chadam
ms.openlocfilehash: ff1e70701882c1740ae91212c8d33ae5e7e3040b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094874"
---
# <a name="dbosysalerts-transact-sql"></a>dbo.sysalerts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит одну строку для каждого предупреждения. Предупреждение — это сообщение, отправляемое как реакция на событие. Предупреждения могут направлять сообщения не только в среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Например, предупреждение может быть сообщением электронной почты или пейджера. Предупреждения также могут создавать задачи.  Эта таблица хранится в базе данных **msdb** .
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Идентификатор предупреждения.|  
|**name**|**sysname**|Имя предупреждения.|  
|**event_source**|**nvarchar (100)**|Источник события: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**event_category_id**|**int**|Зарезервировано для будущего использования.|  
|**event_id**|**int**|Зарезервировано для будущего использования.|  
|**message_id**|**int**|Определяемый пользователем идентификатор сообщения или ссылка на сообщение **sysmessages** , которое запускает это оповещение.|  
|**severity**|**int**|Серьезность события, вызвавшего это предупреждение.|  
|**доступной**|**tinyint**|Состояние предупреждения:<br /><br /> **0** = отключено.<br /><br /> **1** = включено.|  
|**delay_between_responses**|**int**|Время ожидания между отправкой уведомлений для этого предупреждения (в секундах).|  
|**last_occurrence_date**|**int**|Дата последнего возникновения предупреждения.|  
|**last_occurrence_time**|**int**|Время последнего возникновения предупреждения.|  
|**last_response_date**|**int**|Дата последней отправки предупреждения.|  
|**last_response_time**|**int**|Время последней отправки предупреждения.|  
|**notification_message**|**nvarchar(512)**|Дополнительные сведения, отправляемые вместе с предупреждением.|  
|**include_event_description**|**tinyint**|Битовая маска, показывающая, отправляется ли описание события по электронной почте, по пейджеру или с помощью команды net send. Значения см. на диаграмме ниже.|  
|**database_name**|**nvarchar(512)**|База данных, в которой должно произойти предупреждение, чтобы оно сработало.|  
|**event_description_keyword**|**nvarchar (100)**|Шаблон, с которым должна совпасть ошибка для срабатывания этого предупреждения.|  
|**occurrence_count**|**int**|Количество раз, когда возникало этого предупреждение.|  
|**count_reset_date**|**int**|Значение счетчика "день (дата)" будет сброшено в **0**.|  
|**count_reset_time**|**int**|Время суток будет сброшено до **0**.|  
|**job_id**|**uniqueidentifier**|Идентификатор задачи, выполняемой, когда происходит предупреждение.|  
|**has_notification**|**int**|Количество операторов, получающих при возникновении предупреждения уведомление по электронной почте.|  
|**flags**|**int**|Зарезервировано.|  
|**performance_condition**|**nvarchar(512)**|Зарезервировано.|  
|**category_id**|**int**|Зарезервировано.|  
  
 ## <a name="remarks"></a>Комментарии

В следующей таблице показаны значения битовой маски include_event_description. Десятичное значение возвращается dbo.sysпредупреждениях. 

|Decimal | binary | значение |
|------|------|------|
|0 |0000 |нет сообщения |
|1 |0001 |email |
|2 |0010 |pager |
|3 |0011 |пейджер и электронная почта |
|4 |0100 |Net send |
|5 |0101 |NET SEND и email |
|6 |0110 |NET SEND и пейджер |
|7 |0111 |NET SEND, пейджер и электронная почта |
  
