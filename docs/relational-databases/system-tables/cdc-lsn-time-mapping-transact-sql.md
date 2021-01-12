---
description: cdc.lsn_time_mapping (Transact-SQL)
title: cdc.lsn_time_mapping (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc.lsn_time_mapping
- cdc.lsn_time_mapping_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.lsn_time_mapping
ms.assetid: 1cb7aedc-48a4-486e-9b91-d30c4bd4084e
author: cawrites
ms.author: chadam
ms.openlocfilehash: edb2732da4b3d2dba2c8dfa0947eb0c5d135ffb6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094918"
---
# <a name="cdclsn_time_mapping-transact-sql"></a>cdc.lsn_time_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по одной строке для каждой транзакции, имеющей строки в таблице изменений. Эта таблица используется, чтобы сопоставить значения номера LSN фиксации и время фиксации транзакции. Записи также могут регистрироваться. Для этого нет необходимости создавать таблицы изменений. Это позволяет фиксировать завершение процесса обработки номеров LSN в периоды низкой или неизменяющейся нагрузки.  
  
 Не рекомендуется непосредственно запрашивать системные таблицы. Вместо этого выполните [sys.fn_cdc_map_lsn_to_time &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md) и sys.fn_cdc_map_time_to_lsn &#40;системным функциям [&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) .  
    
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**start_lsn**|**binary(10)**|Номер LSN проводимой транзакции в журнале.|  
|**tran_begin_time**|**datetime**|Время начала транзакции с заданным номером LSN.|  
|**tran_end_time**|**datetime**|Время завершения транзакции.|  
|**tran_id**|**varbinary (10)**|Идентификатор транзакции.|  
  
## <a name="see-also"></a>См. также:  
 [Журнал транзакций (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [CDC. &#60;capture_instance&#62;_CT &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)  
  
  
