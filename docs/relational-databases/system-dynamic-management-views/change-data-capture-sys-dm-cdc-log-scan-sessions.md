---
description: Система отслеживания измененных данных — sys.dm_cdc_log_scan_sessions
title: sys.dm_cdc_log_scan_sessions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_cdc_log_scan_sessions
- dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], log scan reporting
- sys.dm_cdc_log_scan_sessions dynamic management view
ms.assetid: d337e9d0-78b1-4a07-8820-2027d0b9f87c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 312a0577d6ac2d092b01b34fcdb2aa034d36995d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100078"
---
# <a name="change-data-capture---sysdm_cdc_log_scan_sessions"></a>Система отслеживания измененных данных — sys.dm_cdc_log_scan_sessions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает одну строку для каждого сеанса просмотра журнала в текущей базе данных. Последняя строка соответствует текущему сеансу. Данное представление можно использовать, чтобы получить сведения о состоянии текущего сеанса просмотра журнала либо статистические сведения обо всех сеансах с момента последнего запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Идентификатор сеанса.<br /><br /> Значение 0 в данной строке означает, что возвращаемые данные представляют собой статистику всех сеансов с момента последнего запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**start_time**|**datetime**|Время начала сеанса.<br /><br /> Если **session_id** = 0, то время сбора агрегированных данных началось.|  
|**end_time**|**datetime**|Время окончания сеанса.<br /><br /> NULL = сеанс активен.<br /><br /> Если **session_id** = 0, время последнего сеанса завершено.|  
|**duration**|**bigint**|Продолжительность сеанса в секундах.<br /><br /> Значение 0 означает, что сеанс не содержит транзакций системы отслеживания измененных данных.<br /><br /> Если **session_id** = 0, то сумма длительности (в секундах) всех сеансов с транзакциями системы отслеживания измененных данных.|  
|**scan_phase**|**nvarchar(200)**|Текущая стадия сеанса. Ниже приведены возможные значения и их описания.<br /><br /> 1: чтение конфигурации<br />2: первое сканирование, создание хэш-таблицы<br />3: второе сканирование<br />4: второе сканирование<br />5: второе сканирование<br />6: Управление версиями схемы<br />7: Последнее сканирование<br />8: готово<br /><br /> Если **session_id** = 0, это значение всегда равно "Aggregate".|  
|**error_count**|**int**|Количество обнаруженных ошибок.<br /><br /> Если **session_id** = 0, общее количество ошибок во всех сеансах.|  
|**start_lsn**|**nvarchar (23)**|Начальный номер LSN для сеанса.<br /><br /> Если **session_id** = 0, начальный номер LSN для последнего сеанса.|  
|**current_lsn**|**nvarchar (23)**|Текущий номер LSN, который был просмотрен.<br /><br /> Если **session_id** = 0, текущий номер LSN равен 0.|  
|**end_lsn**|**nvarchar (23)**|Конечный номер LSN сеанса.<br /><br /> NULL = сеанс активен.<br /><br /> Если **session_id** = 0, конечный номер LSN для последнего сеанса.|  
|**tran_count**|**bigint**|Количество проведенных транзакций системы отслеживания измененных данных. Этот счетчик заполняется на второй стадии.<br /><br /> Если **session_id** = 0, количество обработанных транзакций во всех сеансах.|  
|**last_commit_lsn**|**nvarchar (23)**|Номер LSN последней обработанной записи в журнале фиксирования.<br /><br /> Если **session_id** = 0, последний номер LSN записи журнала фиксации для любого сеанса.|  
|**last_commit_time**|**datetime**|Время последней обработки записи в журнале фиксирования.<br /><br /> Если **session_id** = 0, то время последней записи журнала фиксации для любого сеанса.|  
|**log_record_count**|**bigint**|Количество просмотренных записей журнала.<br /><br /> Если **session_id** = 0, число записей, просмотренных для всех сеансов.|  
|**schema_change_count**|**int**|Количество обнаруженных операций языка DDL. Значение данного счетчика заполняется на шестой стадии.<br /><br /> Если **session_id** = 0, количество операций DDL, обработанных во всех сеансах.|  
|**command_count**|**bigint**|Количество выполненных команд.<br /><br /> Если **session_id** = 0, количество команд, обрабатываемых во всех сеансах.|  
|**first_begin_cdc_lsn**|**nvarchar (23)**|Первый номер LSN, содержащий транзакции системы отслеживания измененных данных.<br /><br /> Если **session_id** = 0, первый номер LSN, содержащий транзакции системы отслеживания измененных данных.|  
|**last_commit_cdc_lsn**|**nvarchar (23)**|Номер LSN последней записи в журнале фиксирования, содержащей транзакции системы отслеживания измененных данных.<br /><br /> Если **session_id** = 0, последний номер LSN записи журнала фиксации для любого сеанса, содержащего транзакции системы отслеживания измененных данных|  
|**last_commit_cdc_time**|**datetime**|Время обработки последней записи в журнале фиксирования, содержащей транзакции системы отслеживания измененных данных.<br /><br /> Если **session_id** = 0, то время последней записи журнала фиксации для любого сеанса, содержащего транзакции системы отслеживания измененных данных.|  
|**явно**|**int**|Разница между **end_time** и **last_commit_cdc_time** в сеансе в секундах. Этот счетчик заполняется в конце седьмой стадии.<br /><br /> Если **session_id** = 0, Последнее ненулевое значение задержки, записанное сеансом.|  
|**empty_scan_count**|**int**|Количество последовательных сеансов, не содержащих транзакций системы отслеживания измененных данных.|  
|**failed_sessions_count**|**int**|Число сеансов, завершившихся неудачно.|  
  
## <a name="remarks"></a>Комментарии  
 Значения в этом динамическом административном представлении сбрасываются при каждом запуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Разрешения  
 Для запроса динамического административного представления **sys.dm_cdc_log_scan_sessions** требуется разрешение Просмотр состояния базы данных. Дополнительные сведения о разрешениях для динамических административных представлений см. в разделе [динамические административные представления и функции &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращается информация о самом последнем сеансе.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT session_id, start_time, end_time, duration, scan_phase,  
    error_count, start_lsn, current_lsn, end_lsn, tran_count,  
    last_commit_lsn, last_commit_time, log_record_count, schema_change_count,  
    command_count, first_begin_cdc_lsn, last_commit_cdc_lsn,   
    last_commit_cdc_time, latency, empty_scan_count, failed_sessions_count  
FROM sys.dm_cdc_log_scan_sessions  
WHERE session_id = (SELECT MAX(b.session_id) FROM sys.dm_cdc_log_scan_sessions AS b);  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_cdc_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)  
  
  

