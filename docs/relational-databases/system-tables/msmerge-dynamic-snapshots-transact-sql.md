---
description: Таблица MSmerge_dynamic_snapshots (Transact-SQL)
title: MSmerge_dynamic_snapshots (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_dynamic_snapshots_TSQL
- MSmerge_dynamic_snapshots
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_dynamic_snapshots system table
ms.assetid: a5592b3c-731b-4fc9-ae4b-2602ed78248e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 10d3e082032daa3782e15caa20d43e8eb4130bd5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091432"
---
# <a name="msmerge_dynamic_snapshots-transact-sql"></a>Таблица MSmerge_dynamic_snapshots (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В **MSmerge_dynamic_snapshots** таблице отслеживается расположение моментального снимка отфильтрованных данных для каждой секции, определенной для публикации слиянием с параметризованными фильтрами строк. Эта таблица хранится в базе данных **публикации** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**partition_id**|**int**|Идентификатор секции для слияния.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Расположение моментального снимка отфильтрованных данных секции.|  
|**last_updated**|**datetime**|Дата последнего обновления моментального снимка отфильтрованных данных.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
