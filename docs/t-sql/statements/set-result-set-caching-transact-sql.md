---
description: SET RESULT_SET_CACHING (Transact-SQL)
title: SET RESULT_SET_CACHING (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords: ''
dev_langs:
- TSQL
helpviewer_keywords: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: d938f8523284c1c814573e70ebf2e20a50a4d089
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644388"
---
# <a name="set-result-set-caching-transact-sql"></a>SET RESULT SET CACHING (Transact-SQL) 

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Определяет поведение кэширования результирующего набора для текущего сеанса клиента.  

Применяется к [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис

```syntaxsql
SET RESULT_SET_CACHING { ON | OFF };
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="remarks"></a>Remarks  

Выполните эту команду при подключении к пользовательской базе данных, для которой требуется настроить параметр result_set_caching.

**ON**   
Включает кэширование результирующего набора для текущего сеанса клиента.  Для кэширования результирующих наборов невозможно указать значение ON для сеанса, если на уровне базы данных указано значение OFF.

**OFF**   
Отключает кэширование результирующего набора для текущего сеанса клиента.

## <a name="examples"></a>Примеры

В [sys.dm_pdw_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md) запросите столбец result_cache_hit, используя request_id запроса, чтобы определить, был ли запрос результатов выполнен с попаданием в кэше или промахом кэша.

```sql
SELECT result_cache_hit
FROM sys.dm_pdw_exec_requests
WHERE request_id = 'QID58286'
```

## <a name="permissions"></a>Разрешения

Требуется членство в роли public.

## <a name="see-also"></a>См. также раздел

- [Performance tuning with result set caching](/azure/sql-data-warehouse/performance-tuning-result-set-caching) (Настройка производительности путем кэширования результирующего набора)
- [Параметры ALTER DATABASE SET &#40;Transact-SQL&#41;](./alter-database-transact-sql-set-options.md?preserve-view=true&view=azure-sqldw-latest&preserve-view=true)
- [ALTER DATABASE (Transact-SQL)](./alter-database-transact-sql.md?preserve-view=true&view=azure-sqldw-latest&preserve-view=true)
- [DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)](../database-console-commands/dbcc-showresultcachespaceused-transact-sql.md)
- [DBCC DROPRESULTSETCACHE (Transact-SQL)](../database-console-commands/dbcc-dropresultsetcache-transact-sql.md)