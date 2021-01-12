---
description: MSreplication_options (Transact-SQL)
title: MSreplication_options (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_options
- MSreplication_options_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_options system table
ms.assetid: 23cf10d7-8bc1-4368-b5eb-e5576421e776
author: cawrites
ms.author: chadam
ms.openlocfilehash: 263436f5bd3ff691191d74b29c7bced426ba4599
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098150"
---
# <a name="msreplication_options-transact-sql"></a>MSreplication_options (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В **MSreplication_options** таблице хранятся метаданные, которые используются для внутренних целей репликации. Эта таблица хранится в базе данных **master** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**optname**|**sysname**|Только для внутреннего использования.|  
|**value**|**bit**|Только для внутреннего использования.|  
|**major_version**|**int**|Только для внутреннего использования.|  
|**minor_version**|**int**|Только для внутреннего использования.|  
|**revision**|**int**|Только для внутреннего использования.|  
|**install_failures**|**int**|Только для внутреннего использования.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
