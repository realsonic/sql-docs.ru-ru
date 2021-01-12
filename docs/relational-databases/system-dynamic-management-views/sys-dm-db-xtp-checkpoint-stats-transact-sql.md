---
title: sys.dm_db_xtp_checkpoint_stats (Transact-SQL) | Документация Майкрософт
description: Возвращает статистику об операциях контрольной точки OLTP в памяти в текущей базе данных. Узнайте, как это представление отличается для версий SQL Server.
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_stats
- dm_db_xtp_checkpoint_stats_TSQL
- sys.dm_db_xtp_checkpoint_stats
- sys.dm_db_xtp_checkpoint_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_stats dynamic management view
ms.assetid: 8d0b18ca-db4d-4376-9905-3e4457727c46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 08cd6d78f4da2d0f5dc68ad43db29bc0542411e9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092935"
---
# <a name="sysdm_db_xtp_checkpoint_stats-transact-sql"></a>sys.dm_db_xtp_checkpoint_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Возвращает статистику об операциях контрольной точки OLTP в памяти в текущей базе данных. Если база данных не имеет объектов OLTP в памяти, возвращается пустой результирующий набор.  
  
 Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
```  
USE In_Memory_db_name
SELECT * FROM sys.dm_db_xtp_checkpoint_stats;  
```  
  
**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] существенно отличается от более поздних версий и обсуждается ниже в разделе [SQL Server 2014](#bkmk_2014).**
  
## <a name="sssql15-and-later"></a>[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий  
 В следующей таблице описаны столбцы в `sys.dm_db_xtp_checkpoint_stats` , начиная с **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]** .  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|last_lsn_processed|**bigint**|Последний номер LSN, отображаемый контроллером.|  
|end_of_log_lsn|**numeric (38)**|Номер LSN конца журнала.|  
|bytes_to_end_of_log|**bigint**|Байты журнала, необработанные контроллером, соответствующие байты между `last_lsn_processed` и `end_of_log_lsn` .|  
|log_consumption_rate|**bigint**|Интенсивность использования журнала транзакций контроллером (в КБ/с).|  
|active_scan_time_in_ms|**bigint**|Время, затраченное контроллером на активное сканирование журнала транзакций.|  
|total_wait_time_in_ms|**bigint**|Совокупное время ожидания контроллера, не проверяя журнал.|  
|waits_for_io|**bigint**|Количество ожиданий операций ввода-вывода журнала, вызванных потоком контроллера.|  
|io_wait_time_in_ms|**bigint**|Совокупное время, затраченное на ожидание ввода-вывода журнала потоком контроллера.|  
|waits_for_new_log_count|**bigint**|Число ожиданий, вызванных потоком контроллера для создания нового журнала.|  
|new_log_wait_time_in_ms|**bigint**|Совокупное время, затраченное на ожидание нового журнала потоком контроллера.|  
|idle_attempts_count|**bigint**|Количество раз, когда контроллер перешел в состояние простоя.|  
|tx_segments_dispatched|**bigint**|Количество сегментов, обнаруженных контроллером и отправленных в сериализаторы. Сегмент — это непрерывная часть журнала, которая образует единицу сериализации. В настоящее время он имеет размер 1 МБ, но может измениться в будущем.|  
|segment_bytes_dispatched|**bigint**|Общее число байтов, отправленных контроллером для сериализаторов, с момента перезапуска базы данных.|  
|bytes_serialized|**bigint**|Общее число байтов, сериализованных с момента перезапуска базы данных.|  
|serializer_user_time_in_ms|**bigint**|Время, затраченное сериализаторами в пользовательском режиме.|  
|serializer_kernel_time_in_ms|**bigint**|Время, затраченное сериализаторами в режиме ядра.|  
|xtp_log_bytes_consumed|**bigint**|Общее число байтов журнала, использованных с момента перезапуска базы данных.|  
|checkpoints_closed|**bigint**|Число контрольных точек, закрытых с момента перезапуска базы данных.|  
|last_closed_checkpoint_ts|**bigint**|Метка времени последней закрытой контрольной точки.|  
|hardened_recovery_lsn|**numeric (38)**|Восстановление начнется с этого номера LSN.|  
|hardened_root_file_guid|**uniqueidentifier**|Идентификатор GUID корневого файла, зафиксированного в результате последней выполненной контрольной точки.|  
|hardened_root_file_watermark|**bigint**|**Только внутренние**. Насколько действительно можно считать корневой файл до (это внутренний релевантный тип, именуемый BSN).|  
|hardened_truncation_lsn|**numeric (38)**|Номер LSN точки усечения.|  
|log_bytes_since_last_close|**bigint**|Байтов от последнего близкого к текущему концу журнала.|  
|time_since_last_close_in_ms|**bigint**|Время с момента последнего закрытия контрольной точки.|  
|current_checkpoint_id|**bigint**|В эту контрольную точку назначаются новые сегменты. Система контрольных точек является конвейером. Текущая контрольная точка — это тот, которому назначаются сегменты из журнала. После достижения ограничения контрольная точка освобождается контроллером, а новая создается как текущая.|  
|current_checkpoint_segment_count|**bigint**|Число сегментов в текущей контрольной точке.|  
|recovery_lsn_candidate|**bigint**|**Только на внутреннем уровне**. Кандидат, который будет выбран как рековерилсн при закрытии current_checkpoint_id.|  
|outstanding_checkpoint_count|**bigint**|Число контрольных точек в конвейере, ожидающих закрытия.|  
|closing_checkpoint_id|**bigint**|Идентификатор закрывающей контрольной точки.<br /><br /> Сериализаторы работают параллельно, поэтому после завершения этих действий контрольная точка будет закрыта закрытым потоком. Но поток закрытия может закрываться только по одному и должен быть по порядку, поэтому закрывающая контрольная точка — это тот, на котором работает поток закрытия.|  
|recovery_checkpoint_id|**bigint**|Идентификатор контрольной точки, которая будет использоваться при восстановлении.|  
|recovery_checkpoint_ts|**bigint**|Метка времени контрольной точки восстановления.|  
|bootstrap_recovery_lsn|**numeric (38)**|Номер LSN восстановления для начальной загрузки.|  
|bootstrap_root_file_guid|**uniqueidentifier**|Идентификатор GUID корневого файла для начальной загрузки.|  
|internal_error_code|**bigint**|Ошибка, обнаруженная любыми потоками контроллера, сериализатора, закрытия и слияния.|
|bytes_of_large_data_serialized|**bigint**|Объем сериализованных данных. |  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 В следующей таблице описаны столбцы в `sys.dm_db_xtp_checkpoint_stats` , для **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|log_to_process_in_bytes|**bigint**|Число байтов журнала до регистрационного номера транзакции в журнале и в конце текущего потока транзакций.|  
|total_log_blocks_processed|**bigint**|Общее количество блоков журнала, обработанных с момента запуска сервера.|  
|total_log_records_processed|**bigint**|Общее количество записей журнала, обработанных с момента запуска сервера.|  
|xtp_log_records_processed|**bigint**|Общее количество записей журнала In-Memory OLTP, обработанных с момента запуска сервера.|  
|total_wait_time_in_ms|**bigint**|Совокупное время ожидания в миллисекундах.|  
|waits_for_io|**bigint**|Число ожиданий ввода-вывода для журнала.|  
|io_wait_time_in_ms|**bigint**|Совокупное время, потраченное на ожидание ввода-вывода для журнала.|  
|waits_for_new_log|**bigint**|Число ожиданий для нового журнала.|  
|new_log_wait_time_in_ms|**bigint**|Совокупное время, затраченное на новый журнал.|  
|log_generated_since_last_checkpoint_in_bytes|**bigint**|Размер журнала, созданного с момента создания последней контрольной точки In-Memory OLTP.|  
|ms_since_last_checkpoint|**bigint**|Время в миллисекундах с момента создания последней контрольной точки In-Memory OLTP.|  
|checkpoint_lsn|**numeric (38)**|Регистрационный номер транзакции в журнале восстановления (LSN), связанный с последней завершенной контрольной точкой In-Memory OLTP.|  
|current_lsn|**numeric (38)**|Регистрационный номер транзакции в журнале, который обрабатывается в данный момент.|  
|end_of_log_lsn|**numeric (38)**|Число номеров LSN в конце журнала.|  
|task_address|**varbinary(8)**|Адрес задачи SOS_Task. Соединение с sys.dm_os_tasks для поиска дополнительных сведений.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение `VIEW DATABASE STATE` на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
