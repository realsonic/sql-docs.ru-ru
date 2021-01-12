---
description: IHpublishers (Transact-SQL)
title: Ихпублишерс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublishers
- IHpublishers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishers system table
ms.assetid: 77007246-f10b-4b87-8edf-7afc3c2096af
author: cawrites
ms.author: chadam
ms.openlocfilehash: 037778000e56bf49e99d46d560fd6fda8f4ce484
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092456"
---
# <a name="ihpublishers-transact-sql"></a>IHpublishers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Системная таблица **ихпублишерс** содержит по одной строке для каждого издателя, не являющегося SQL Server, с использованием текущего распространителя. Эта таблица хранится в базе данных распространителя.  
  
## <a name="definition"></a>Определение  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Определяет издателя, не являющегося SQL Server.|  
|**разработчика**|**sysname**|Имя производителя базы данных, не являющейся SQL Server.|  
|**publisher_guid**|**uniqueidentifier**|Идентификатор GUID издателя, не являющегося SQL Server.|  
|**flush_request_time**|**datetime**|Содержит дату и время последнего изменения метаданных статьи, потребовавшего от агента чтения журнала обновления кэша метаданных.|  
|**version**|**sysname**|Текстовая строка, характеризующая версию издателя, не являющегося SQL Server.|  
  
## <a name="see-also"></a>См. также  
 [Разнородная репликация базы данных](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_helpdistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)  
  
  
