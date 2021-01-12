---
description: sys.fn_cdc_get_max_lsn (Transact-SQL)
title: sys.fn_cdc_get_max_lsn (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_get_max_lsn
- fn_cdc_get_max_lsn
- fn_cdc_get_max_lsn_TSQL
- sys.fn_cdc_get_max_lsn_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_max_lsn
- sys.fn_cdc_get_max_lsn
ms.assetid: 93f3a4c8-b91f-4ebb-8e96-9397bb3a1c43
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 59613b93de2a086bfd4d0bac7fa0f8a8983d8ce5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095001"
---
# <a name="sysfn_cdc_get_max_lsn-transact-sql"></a>sys.fn_cdc_get_max_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает максимальный регистрационный номер транзакции в журнале (LSN) из столбца start_lsn в системной таблице [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) . Эту функцию можно использовать для возврата верхней конечной точки временной шкалы системы отслеживания измененных данных для любого экземпляра отслеживания.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_cdc_get_max_lsn ()  
```  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **binary(10)**  
  
## <a name="remarks"></a>Комментарии  
 Эта функция возвращает максимальный номер LSN в столбце start_lsn таблицы [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) . Этот последний номер LSN, обрабатываемый процессом отслеживания, когда изменения передаются в таблицы изменений базы данных. Он служит в качестве верхней конечной точки для любой временной шкалы, связанной с экземплярами отслеживания, определенными для базы данных.  
  
 Эта функция обычно используется, чтобы получить подходящую верхнюю конечную точку для интервала запроса.  
  
## <a name="permissions"></a>Разрешения  
 Требует членства в роли базы данных public.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-the-maximum-lsn-value"></a>A. Возвращение максимального значения номера LSN  
 В следующем примере возвращается максимальное значение номера LSN для всех экземпляров отслеживания в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT sys.fn_cdc_get_max_lsn()AS max_lsn;  
```  
  
### <a name="b-setting-the-high-endpoint-of-a-query-range"></a>Б. Установка верхней конечной точки запроса по диапазону  
 В следующем примере номер LSN, возвращенный функцией `sys.fn_cdc_get_max_lsn`, используется для установки верхней конечной точки запроса по диапазону для экземпляра отслеживания `HumanResources_Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn(N'HumanResources_Employee');  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SELECT * FROM cdc.fn_cdc_get_all_changes_HumanResources_Employee(@from_lsn, @to_lsn, 'all');  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.fn_cdc_get_min_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)   
 [Журнал транзакций (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)  
  
  
