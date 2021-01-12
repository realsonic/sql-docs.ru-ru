---
description: MSpublicationthresholds (Transact-SQL)
title: Мспубликатионсрешолдс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- mspublicationthresholds
- mspublicationthresholds_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublicationthresholds system table
ms.assetid: 9da3879f-b1f4-4ab4-abd4-a9a8ac395eba
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3fc1c3f5290e5c3f540e070c2de5790414ffb6dd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098247"
---
# <a name="mspublicationthresholds-transact-sql"></a>MSpublicationthresholds (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **мспубликатионсрешолдс** используется для отслеживания метрик производительности репликации для публикации с одной строкой для каждого наблюдаемого порогового значения. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publication_id**|**int**|Указывает публикацию, для которой было установлено пороговое значение.|  
|**metric_id**|**int**|Определяет метрику производительности репликации, которая отслеживается в соответствии с определением в системной таблице [мсреплмонсрешолдметрикс](../../relational-databases/system-tables/msreplmonthresholdmetrics-transact-sql.md) .|  
|**value**|**sql_variant**|Пороговое значение измеряемой величины.|  
|**shouldalert**|**bit**|Значение **1** указывает, что предупреждение должно быть создано, если метрика превышает заданное пороговое значение.|  
|**IsEnabled**|**bit**|Значение **1** указывает, что мониторинг включен для этой метрики производительности репликации.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
