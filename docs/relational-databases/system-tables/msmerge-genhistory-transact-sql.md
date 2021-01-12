---
description: MSmerge_genhistory (Transact-SQL)
title: MSmerge_genhistory (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_genhistory_TSQL
- MSmerge_genhistory
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_genhistory system table
ms.assetid: 475d08ae-eb8b-49de-afd6-33c96ab8004d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7ccd9f9b6cc2780681f1f76c0764aaf423c2af36
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100593"
---
# <a name="msmerge_genhistory-transact-sql"></a>MSmerge_genhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSmerge_genhistory** содержит по одной строке для каждого поколения, о котором известно подписчику (в течение срока хранения). Она используется, чтобы избежать отправки общих поколений при обменах и для повторной синхронизации подписчиков, восстановленных из резервных копий. Эта таблица хранится в базах данных публикации и подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**guidsrc**|**uniqueidentifier**|Глобальный идентификатор изменений, связанных с поколением на подписчике.|  
|**pubid**|**uniqueidentifier**|Идентификатор публикации.|  
|**поколения**|**bigint**|Номер поколения.|  
|**art_nick**|**int**|Псевдоним статьи.|  
|**nicknames**|**varbinary (1001)**|Список псевдонимов других подписчиков, о которых известно, что они уже содержат это поколение. Используется, чтобы избежать отправки поколения подписчику, который уже получил изменения, содержащиеся в нем. Псевдонимы в списке псевдонимов хранятся в порядке сортировки для повышения эффективности поиска. Если существует больше псевдонимов, чем может поместиться в это поле, они не получат пользы от этой оптимизации.|  
|**coldate**|**datetime**|Дата, когда текущее поколение было добавлено в таблицу.|  
|**genstatus**|**tinyint**|Состояние поколения может быть:<br /><br /> **0** = открыть.<br /><br /> **1** = закрыто.<br /><br /> **2** = закрыто и создано на другом подписчике.|  
|**changecount**|**int**|Количество изменений, отраженных в данном поколении|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
