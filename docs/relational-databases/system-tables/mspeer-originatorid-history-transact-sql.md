---
description: MSpeer_originatorid_history (Transact-SQL)
title: MSpeer_originatorid_history (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpeer_originatorid_history_TSQL
- MSpeer_originatorid_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_originatorid_history
ms.assetid: c1f53d0f-4080-43ff-be38-2b10395c68c9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8b0056305ece07f6dae6f1dc9f07563a8ff5c9dd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097373"
---
# <a name="mspeer_originatorid_history-transact-sql"></a>MSpeer_originatorid_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого идентификатора инициатора, определенного в топологии. Сюда входят и идентификаторы для узлов, которые уже неактивны. Таблица используется при настройке нового узла для обнаружения конфликтов и позволяет гарантировать, что указанный идентификатор инициатора еще не использовался. Эта таблица хранится в базе данных публикации. Дополнительные сведения об обнаружении конфликтов см. [в разделе Обнаружение конфликтов в](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)одноранговой репликации.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|originator_publication|**sysname**|Публикация, для которой задан идентификатор инициатора.|  
|originator_id|**int**|Число, идентифицирующее каждый из узлов топологии в целях обнаружения конфликтов.|  
|originator_node|**sysname**|Экземпляр сервера, для которого был задан идентификатор инициатора.|  
|originator_db|**sysname**|База данных публикации, для которой был задан идентификатор инициатора.|  
|originator_db_version|**int**|Показывает номер версии исходной базы данных.|  
|originator_version|**int**|Определяет номер версии издателя.|  
|inserted_date|**datetime**|Дата и время вставки в эту таблицу строки для идентификатора инициатора.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
