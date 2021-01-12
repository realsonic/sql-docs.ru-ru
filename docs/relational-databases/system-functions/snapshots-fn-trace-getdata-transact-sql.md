---
description: snapshots.fn_trace_getdata (Transact-SQL)
title: snapshots.fn_trace_getdata (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- snapshots.fn_trace_getdata
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots.fn_trace_getdata function
ms.assetid: ac28ef48-f4f4-4bf2-ba22-d44e1be88172
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ff9e08aefac2307811d0d4398b3af208924fd747
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097464"
---
# <a name="snapshotsfn_trace_getdata-transact-sql"></a>snapshots.fn_trace_getdata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает все события, собранные для заданной трассировки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
snapshots.fn_trace_gettable ( trace_info_id, start_time, end_time )  
```  
  
## <a name="arguments"></a>Аргументы  
 *trace_info_id*  
 Уникальный идентификатор первичного ключа в таблице snapshots.trace_info в базе данных хранилища управляющих данных. *trace_info_id* имеет **тип int**.  
  
 *start_time*  
 Время начала трассировки. *start_time* имеет тип **DateTime**.  
  
 *end_time*  
 Время завершения трассировки. *end_time* имеет тип **DateTime**.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|\<All trace columns>|\<Varies>|Данные трассировки из таблицы snapshots.trace_data в базе данных хранилища данных управления.<br /><br /> Список столбцов для заданной трассировки можно получить, выполнив следующий запрос:<br /><br /> `SELECT * FROM sys.trace_columns`<br /><br /> **Примечание.** Столбцы, возвращаемые функцией snapshots.fn_trace_gettable, соответствуют значениям в столбце имя в системном представлении sys.trace_columns. Единственным различием является столбец GroupID, не возвращаемый функцией.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение SELECT для mdw_reader.  
  
## <a name="see-also"></a>См. также:  
 [Сбор данных](../../relational-databases/data-collection/data-collection.md)  
  
  
