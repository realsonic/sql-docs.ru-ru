---
description: MSmerge_replinfo (Transact-SQL)
title: MSmerge_replinfo (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_replinfo_TSQL
- MSmerge_replinfo
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_replinfo system table
ms.assetid: b0924094-c0cc-49c1-869a-65be0d0465a0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6d7ba27eb3b01fcc34e5f53d954b12d120542116
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098637"
---
# <a name="msmerge_replinfo-transact-sql"></a>MSmerge_replinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSmerge_replinfo** содержит по одной строке для каждой подписки. Эта таблица отслеживает сведения о подписках. Эта таблица хранится в базах данных публикации и подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**repid**|**uniqueidentifier**|Уникальный идентификатор реплики.|  
|**use_interactive_resolver**|**bit**|Показывает, используется ли интерактивный сопоставитель во время проверки согласованности.<br /><br /> **0** = не использовать интерактивный сопоставитель.<br /><br /> **1** = использовать интерактивный сопоставитель.|  
|**validation_level**|**int**|Тип проверки подписки. Уровень проверки может иметь одно из следующих значений:<br /><br /> **0** = проверка не производится.<br /><br /> **1** = проверка только количества строк.<br /><br /> **2** = проверка количества строк и контрольной суммы.<br /><br /> **3** = проверка количества строк и двоичной контрольной суммы.|  
|**resync_gen**|**bigint**|Номер поколения, используемый при повторной синхронизации подписки. Значение **-1** указывает, что подписка не помечена для повторной синхронизации.|  
|**login_name**|**sysname**|Имя пользователя, создавшего подписку.|  
|**hostname**|**sysname**|Значение, используемое параметризованным фильтром строк при формировании секции для подписки.|  
|**merge_jobid**|**двоичный (16)**|Идентификатор задания слияния, соответствующий данной подписке.|  
|**sync_info**|**int**|Только для внутреннего использования.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
