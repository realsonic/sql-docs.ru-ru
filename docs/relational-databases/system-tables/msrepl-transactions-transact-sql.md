---
description: MSrepl_transactions (Transact-SQL)
title: MSrepl_transactions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSrepl_transactions_TSQL
- MSrepl_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_transactions system table
ms.assetid: d325288d-47ae-4488-8799-122f7ab43459
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6e9ca4af77c9cab4e64a515041e68e4eb3dae3b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092417"
---
# <a name="msrepl_transactions-transact-sql"></a>MSrepl_transactions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSrepl_transactions** содержит по одной строке для каждой реплицированной транзакции. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|Идентификатор базы данных издателя.|  
|**xact_id**|**varbinary (16)**|Идентификатор транзакции.|  
|**xact_seqno**|**varbinary (16)**|Порядковый номер транзакции.|  
|**entry_time**|**datetime**|Время входа транзакции в базу данных распространителя.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
