---
description: IHpublisherindexes (Transact-SQL)
title: Ихпублишериндексес (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublisherindexes
- IHpublisherindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublisherindexes system table
ms.assetid: 6008ef89-eeb9-46dc-93a2-f7623298cf0f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 03915f08ecf8aeb222f45e86af3988c64cab575b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092463"
---
# <a name="ihpublisherindexes-transact-sql"></a>IHpublisherindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Системная таблица **ихпублишериндексес** содержит по одной строке для каждого индекса, реплицированного из издателей, отличных от SQL Server, использующих текущий распространитель. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisherindex_id**|**int**|Идентифицирует опубликованный индекс.|  
|**table_id**|**int**|Определяет таблицу из [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md) , к которой принадлежит индекс.|  
|**publisher_id**|**smallint**|Идентифицирует издатель, отличный от SQL Server и публикующий данный индекс.|  
|**name**|**sysname**|Имя опубликованного индекса.|  
|**type**|**nvarchar(255)**|Поддерживаемый тип индекса из системной таблицы [ихиндекстипес](../../relational-databases/system-tables/ihindextypes-transact-sql.md) .|  
  
## <a name="see-also"></a>См. также  
 [Разнородная репликация базы данных](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
