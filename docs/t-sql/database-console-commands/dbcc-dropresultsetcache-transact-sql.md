---
description: DBCC DROPRESULTSETCACHE (Transact-SQL)
title: DBCC DROPRESULTSETCACHE (Transact-SQL)
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 73f598cf-b02a-4dba-8d89-9fc0b55a12b8
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 22ebd9f7d52fd15d9dbd8479962312387857b233
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641260"
---
# <a name="dbcc-dropresultsetcache--transact-sql"></a>DBCC DROPRESULTSETCACHE (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Удаляет все записи кэша результирующего набора из базы данных Azure [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DBCC DROPRESULTSETCACHE
[;]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="permissions"></a>Разрешения

Необходимо членство в предопределенной роли сервера DB_OWNER.

## <a name="remarks"></a>Remarks

- Эта команда очищает кэш результирующего набора для всех запросов.  

- При отключении функции кэша результирующего набора для базы данных также удаляются все кэшированные результаты.  

- Приостановка базы данных с кэшированием результирующего набора не приведет к удалению кэшированных результатов.  

## <a name="see-also"></a>См. также раздел

- [Performance tuning with result set caching](/azure/sql-data-warehouse/performance-tuning-result-set-caching) (Настройка производительности путем кэширования результирующего набора)</br>
- [Параметры ALTER DATABASE SET &#40;Transact-SQL&#41;](../statements/alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)</br>
- [ALTER DATABASE (Transact-SQL)](../statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)</br>
- [SET RESULT SET CACHING &#40;Transact-SQL&#41;](../statements/set-result-set-caching-transact-sql.md)</br>
- [DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)](./dbcc-showresultcachespaceused-transact-sql.md)