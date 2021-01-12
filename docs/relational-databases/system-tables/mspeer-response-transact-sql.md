---
description: MSpeer_response (Transact-SQL)
title: MSpeer_response (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpeer_response
- MSpeer_response_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_response system table
ms.assetid: 510e24cf-0292-47a9-b1d9-71a30fef030f
author: cawrites
ms.author: chadam
ms.openlocfilehash: fdf2ec05835bc63f30e5304e8b8226b54be6becb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096194"
---
# <a name="mspeer_response-transact-sql"></a>MSpeer_response (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSpeer_response** таблица используется в одноранговой репликации для хранения ответа каждого узла на запрос состояния публикации. Эта таблица хранится в базе данных публикации.  
  
## <a name="definition"></a>Определение  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**request_id**|**int**|Определяет запись запроса состояния в таблице [MSpeer_request](../../relational-databases/system-tables/mspeer-request-transact-sql.md) .|  
|**класс**|**sysname**|Одноранговый узел, сформировавший ответ.|  
|**peer_db**|**sysname**|База данных подписки однорангового узла, сформировавшего ответ.|  
|**received_date**|**datetime**|Дата и время получения однорангового запроса.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
