---
description: sys.dm_exec_distributed_requests (Transact-SQL)
title: sys.dm_exec_distributed_requests (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_DISTRIBUTED_REQUESTS
- DM_EXEC_DISTRIBUTED_REQUESTS_TSQL
- SYS.DM_EXEC_DISTRIBUTED_REQUESTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- sys.dm_exec_distributed_sql_requests management view
- PolyBase
- dm_exec_distributed_sql_requests management view
ms.assetid: c041d416-d8c6-435e-a563-6a310abd33e3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7c201260484919e307b96cd37240179a4e1ddbec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094072"
---
# <a name="sysdm_exec_distributed_requests-transact-sql"></a>sys.dm_exec_distributed_requests (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Содержит сведения обо всех запросах, которые в данный момент или недавно были активны в запросах Polybase. В нем отображается одна строка для каждого запроса или запроса.  
  
 На основе идентификатора сеанса и запроса пользователь может получить фактические распределенные запросы, созданные для выполнения, с помощью sys.dm_exec_distributed_requests. Например, запрос, включающий обычные SQL и внешние таблицы SQL, будет разноситься в различные инструкции и запросы, выполняемые на различных узлах вычислений. Для трассировки распределенных шагов на всех вычислительных узлах мы представляем глобальный идентификатор выполнения, который можно использовать для трассировки всех операций на вычислительных узлах, связанных с одним конкретным запросом и оператором соответственно.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|sql_handle|**varbinary (64)**|Ключ для этого представления. Уникальный числовой идентификатор, связанный с запросом.|Уникальный для всех запросов в системе.|  
|execution_id|**nvarchar (32**|Уникальный числовой идентификатор, связанный с сеансом, в котором выполнялся этот запрос.||  
|status|**nvarchar (32**|Текущее состояние запроса.|"Pending", "Авторизация", "Аккуиресистемресаурцес", "Инициализация", "Plan", "анализ", "Акуирересаурцес", "выполняется", "Отмена", "завершение", "сбой", "отменено".|  
|error_id|**nvarchar (36)**|Уникальный идентификатор ошибки, связанной с запросом, если он есть.|Если ошибка не возникала, задайте значение NULL.|  
|start_time|**datetime**|Время начала выполнения запроса.|0 для запросов в очереди; в противном случае допустимое значение DateTime меньше или равно текущему времени.|  
|end_time|**datetime**|Время, когда обработчик завершил компиляцию запроса.|Значение NULL для очереди или активных запросов; в противном случае допустимое значение DateTime меньше или равно текущему времени.|  
|total_elapsed_time|**int**|Время, затраченное на выполнение с момента запуска запроса, в миллисекундах.|Между 0 и разностью между start_time и end_time. Если total_elapsed_time превышает максимальное значение для целого числа, total_elapsed_time будет продолжать быть максимальным значением. Это условие выдаст предупреждение "превышено максимальное значение". Максимальное значение в миллисекундах эквивалентно 24,8 дням.|  
  
## <a name="see-also"></a>См. также:  
 [Устранение неполадок в Polybase с помощью динамических административных представлений](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные с базами данных &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
