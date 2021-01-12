---
description: Таблицы расширенных событий — trace_xe_action_map
title: trace_xe_action_map (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- trace_xe_action_map_TSQL
- trace_xe_action_map
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], tables
- trace_xe_action_map
ms.assetid: 208a1413-ce7f-4521-b765-d74723627302
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5eeb22a3617ffb2510c4d9d768cf2cde5e281ee0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100604"
---
# <a name="extended-events-tables---trace_xe_action_map"></a>Таблицы расширенных событий — trace_xe_action_map
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит одну строку для каждого действия из числа расширенных событий, сопоставленного с идентификатором столбца трассировки SQL. Эта таблица хранится в базе данных master в схеме sys.  
  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|trace_column_id|**smallint**|Идентификатор сопоставленного столбца трассировки SQL.|  
|package_name|**nvarchar(60)**|Имя пакета расширенных событий, в котором находится сопоставленное действие.|  
|xe_action_name|**nvarchar(60)**|Имя действия расширенных событий, которое сопоставлено со столбцом трассировки SQL.|  
  
## <a name="remarks"></a>Комментарии  
 Чтобы выявить действия расширенных событий, эквивалентные столбцам трассировки SQL, можно использовать следующий запрос.  
  
```  
SELECT tc.name AS trace_column, am.package_name, am.xe_action_name  
FROM sys.trace_columns AS tc  
INNER JOIN sys.trace_xe_action_map AS am  
   ON tc.trace_column_id = am.trace_column_id  
```  
  
 Столбцы трассировки SQL, которые не соответствуют действиям, не включаются в таблицу.  
  
## <a name="see-also"></a>См. также:  
 [trace_xe_event_map (Transact-SQL)](../../relational-databases/system-tables/extended-events-tables-trace-xe-event-map.md)  
  
  
