---
description: sys.fn_cdc_decrement_lsn (Transact-SQL)
title: sys.fn_cdc_decrement_lsn (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_cdc_decrement_lsn
- sys.fn_cdc_decrement_lsn_TSQL
- sys.fn_cdc_decrement_lsn
- fn_cdc_decrement_lsn_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_decrement_lsn
- sys.fn_cdc_decrement_lsn
ms.assetid: 83c182ad-4713-439b-8769-9b7408aec8b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 460af637f6829940d3dce5282e2bab067a6b08c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093887"
---
# <a name="sysfn_cdc_decrement_lsn-transact-sql"></a>sys.fn_cdc_decrement_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает предыдущий регистрационный номер транзакции в журнале (LSN) в последовательности, основанной на заданном номере LSN.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_cdc_decrement_lsn ( lsn_value )  
```  
  
## <a name="arguments"></a>Аргументы  
 *lsn_value*  
 Значение LSN. *lsn_value* является **двоичным (10)**.  
  
## <a name="return-type"></a>Возвращаемый тип  
 **binary(10)**  
  
## <a name="remarks"></a>Комментарии  
 Номер LSN, возвращаемый этой функцией, всегда меньше указанного значения, и между этими двумя значениями не могут существовать другие номера LSN.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли базы данных **Public** .  
  
## <a name="examples"></a>Примеры  
 В следующем примере функция `sys.fn_cdc_decrement_lsn` используется для установки верхней границы номеров LSN в запросе, возвращающем строки информации об изменениях, имеющих номера LSN меньше максимального значения LSN.  
  
```  
Use AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HumanResources_Employee');  
SET @to_lsn = sys.fn_cdc_decrement_lsn(sys.fn_cdc_get_max_lsn());  
SELECT * FROM cdc.fn_cdc_get_all_changes_HumanResources_Employee( @from_lsn, @to_lsn, 'all');   
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.fn_cdc_increment_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md)   
 [sys.fn_cdc_get_min_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)   
 [sys.fn_cdc_get_max_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md)   
 [Журнал транзакций (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Об отслеживании измененных данных (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
