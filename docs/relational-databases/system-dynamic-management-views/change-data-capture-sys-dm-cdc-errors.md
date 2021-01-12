---
description: Система отслеживания измененных данных — sys.dm_cdc_errors
title: sys.dm_cdc_errors (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_cdc_errors_TSQL
- dm_cdc_errors
- sys.dm_cdc_errors
- sys.dm_cdc_errors_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cdc_errors dynamic management view
- change data capture [SQL Server], error reporting
ms.assetid: 898f2d76-9e63-45ef-94da-8034e86004ab
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ce058600c4a912e13695817a533f8e9ec4c8f856
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100147"
---
# <a name="change-data-capture---sysdm_cdc_errors"></a>Система отслеживания измененных данных — sys.dm_cdc_errors
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает одну строку для каждой ошибки, встреченной во время сеанса просмотра журнала системы отслеживания измененных данных.  
 
 
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Идентификатор сеанса.<br /><br /> 0 = ошибка не происходила в течение сеанса просмотра журнала.|  
|**phase_number**|**int**|Номер, указывающий этап, в котором находился сеанс во время возникновения ошибки. Описание каждого этапа см. в разделе [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md).|  
|**entry_time**|**datetime**|Дата и время регистрации ошибки. Это значение соответствует отметке времени в журнале ошибок SQL.|  
|**error_number**|**int**|Идентификатор сообщения об ошибке.|  
|**error_severity**|**int**|Степень серьезности сообщения, от 1 до 25.|  
|**error_state**|**int**|Номер состояния ошибки.|  
|**error_message**|**nvarchar(1024)**|Текст сообщения ошибки.|  
|**start_lsn**|**nvarchar (23)**|Начальное значение номера LSN для строк, которые обрабатывались во время возникновения ошибки.<br /><br /> 0 = ошибка не происходила в течение сеанса просмотра журнала.|  
|**begin_lsn**|**nvarchar (23)**|Начальное значение номера LSN для транзакции, которая обрабатывалась во время возникновения ошибки.<br /><br /> 0 = ошибка не происходила в течение сеанса просмотра журнала.|  
|**sequence_value**|**nvarchar (23)**|Номер LSN для строк, которые обрабатывались во время возникновения ошибки.<br /><br /> 0 = ошибка не происходила в течение сеанса просмотра журнала.|  
  
## <a name="remarks"></a>Комментарии  
 **sys.dm_cdc_errors** содержит сведения об ошибке для предыдущих 32 сеансов.  
  
## <a name="permissions"></a>Разрешения  
 Для запроса динамического административного представления **sys.dm_cdc_errors** требуется разрешение Просмотр состояния базы данных. Дополнительные сведения о разрешениях для динамических административных представлений см. в разделе [динамические административные представления и функции &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md)   
 [sys.dm_repl_traninfo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-repl-traninfo-transact-sql.md)  
  
  

