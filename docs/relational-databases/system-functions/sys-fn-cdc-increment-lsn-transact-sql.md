---
description: sys.fn_cdc_increment_lsn (Transact-SQL)
title: sys.fn_cdc_increment_lsn (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn
- fn_cdc_increment_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_increment_lsn
- sys.fn_cdc_increment_lsn
ms.assetid: e53b6703-358b-4c9a-912a-8f7c7331069b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee5d44cbe53d31609a454eb5acbed52a53ae194e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099696"
---
# <a name="sysfn_cdc_increment_lsn-transact-sql"></a>sys.fn_cdc_increment_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает следующий регистрационный номер транзакции в журнале (LSN) в последовательности относительно указанного номера.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_cdc_increment_lsn ( lsn_value )  
```  
  
## <a name="arguments"></a>Аргументы  
 *lsn_value*  
 Значение LSN. *lsn_value* является **двоичным (10)**.  
  
## <a name="return-type"></a>Возвращаемый тип  
 **binary(10)**  
  
## <a name="remarks"></a>Комментарии  
 Номер LSN, возвращаемый функцией, всегда больше заданного значения. Между указанными двумя значениями не может быть промежуточных номеров LSN.  
  
 Для систематического отображения информации об изменениях данных с течением времени можно периодически вызывать функцию запроса, каждый раз указывая новые значения границ интервала. Чтобы исключить потерю данных, необходимо в качестве нижней границы последующего запроса использовать верхнюю границу предыдущего запроса. Так как интервал запроса является замкнутым, то новое значение нижней границы должно быть больше, чем значение верхней границы предыдущего запроса. Однако необходимо убедиться, что разница между указанными значениями границ достаточно мала и не включает номеров LSN каких-либо изменений. Для получения этого значения используется функция sys.fn_cdc_increment_lsn.  
  
## <a name="permissions"></a>Разрешения  
 Требует членства в роли базы данных public.  
  
## <a name="examples"></a>Примеры  
 В следующем примере новое значение нижней границы интервала системы отслеживания измененных данных формируется с помощью функции `sys.fn_cdc_increment_lsn`. При этом используется значение верхней границы предыдущего запроса, а результат записывается в переменную `@save_to_lsn`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10), @save_to_lsn binary(10);  
SET @save_to_lsn = <previous_upper_bound_value>;  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SELECT * from cdc.fn_cdc_get_all_changes_HumanResources_Employee( @from_lsn, @to_lsn, 'all' );  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.fn_cdc_decrement_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-decrement-lsn-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [Журнал транзакций (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Об отслеживании измененных данных (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
