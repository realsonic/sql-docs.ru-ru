---
title: MSpeer_conflictdetectionconfigresponse (T-SQL)
description: Описывает MSPeer_conflictdetectionconfigureresponse хранимую процедуру, используемую в одноранговой репликации для хранения ответа каждого узла на запрос конфигурации уровня топологии.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpeer_conflictdetectionconfigresponse
- MSpeer_conflictdetectionconfigresponse_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_conflictdetectionconfigureresponse
ms.assetid: 2685fb66-731d-40f7-af4b-596b9222c5d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 02c82643c290e3b751ced451dd7415004272a2b0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097400"
---
# <a name="mspeer_conflictdetectionconfigresponse-transact-sql"></a>MSpeer_conflictdetectionconfigresponse (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Используется в одноранговой репликации для сохранения ответа каждого из узлов на запрос настройки на уровне топологии. Эта таблица хранится в базе данных публикации.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|request_id|**int**|Определяет запись запроса конфигурации конфликтов в [MSpeer_conflictdetectionconfigrequest](../../relational-databases/system-tables/mspeer-conflictdetectionconfigrequest-transact-sql.md) таблице.|  
|peer_node|**sysname**|Имя экземпляра сервера, сформировавшего ответ.|  
|peer_db|**sysname**|База данных подписки на одноранговом узле, сформировавшем ответ.|  
|peer_version|**sysname**|Определяет номер версии издателя.|  
|peer_db_version|**sysname**|Указывает номер версии одноранговой базы данных.|  
|is_peer|**bit**|Указывает, является ли узел подписчиком, доступным только для чтения. Значение **0** указывает на подписчик только для чтения.|  
|conflict_detection_enabled|**bit**|Указывает, включено ли обнаружение конфликтов для топологии.|  
|originator_id|**varbinary (16)**|Указывает каждый узел в топологии с целью обнаружения конфликтов. Дополнительные сведения см. в разделе [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
|peer_conflict_retention|**int**|Срок (в днях), в течение которого метаданные хранятся в таблицах конфликтов.|  
|peer_subscriptions|**XML**|Сведения об узле, сформировавшем ответ на запрос.|  
|progress_phase|**nvarchar(32)**|Определяет текущую стадию обработки как одно из следующих значений:<br /><br /> Запуск<br /><br /> Собраны данные о версии однорангового узла<br /><br /> Собранные данные о состоянии|  
|modified_date|**datetime**|Дата и время завершения стадии.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
