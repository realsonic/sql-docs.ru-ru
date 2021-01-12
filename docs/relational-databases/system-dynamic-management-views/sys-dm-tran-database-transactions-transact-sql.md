---
description: sys.dm_tran_database_transactions (Transact-SQL)
title: sys.dm_tran_database_transactions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_database_transactions
- sys.dm_tran_database_transactions_TSQL
- dm_tran_database_transactions_TSQL
- sys.dm_tran_database_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_database_transactions dynamic management view
ms.assetid: 82a44295-4cbc-4a5b-891a-8ebaf307b8f5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9932a3f57c02307699ff75d53efaf81f25594e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097514"
---
# <a name="sysdm_tran_database_transactions-transact-sql"></a>sys.dm_tran_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения о транзакциях на уровне базы данных.  
  
> [!NOTE]  
>  Чтобы вызвать это динамическое административное представление из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_tran_database_transactions**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|Идентификатор транзакции на уровне экземпляра, а не на уровне базы данных. Уникален в пределах баз данных экземпляра, но не уникален в пределах экземпляров сервера.|  
|database_id|**int**|Идентификатор базы данных, связанной с транзакцией.|  
|database_transaction_begin_time|**datetime**|Момент времени, с которого база данных задействована в транзакции. Точнее, это время первой записи журнала в базе данных для данной транзакции.|  
|database_transaction_type|**int**|1 = транзакция чтения-записи<br /><br /> 2 = транзакция только чтения<br /><br /> 3 = системная транзакция|  
|database_transaction_state|**int**|1 = Транзакция не инициализирована.<br /><br /> 3 = Транзакция инициализирована, но в ней еще не сформировано ни одной записи журнала.<br /><br /> 4 = В транзакции имеются сформированные записи журнала.<br /><br /> 5 = Транзакция подготовлена.<br /><br /> 10 = Транзакция зафиксирована.<br /><br /> 11 = Транзакция находится в процессе отката.<br /><br /> 12 = Транзакция находится в стадии фиксации. (Запись журнала создается, но не была материализованным или материализованным.)|  
|database_transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_log_record_count|**bigint**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Число записей журнала, сформированных в базе данных для этой транзакции.|  
|database_transaction_replicate_record_count|**int**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Количество записей журнала, сформированных в базе данных для реплицируемой транзакции.|  
|database_transaction_log_bytes_used|**bigint**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Число байтов, используемых журналом базы данных для данной транзакции.|  
|database_transaction_log_bytes_reserved|**bigint**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Число байтов, зарезервированных в журнале базы данных для данной транзакции.|  
|database_transaction_log_bytes_used_system|**int**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Число байтов, занятых в журнале базы данных для системных транзакций от имени данной транзакции.|  
|database_transaction_log_bytes_reserved_system|**int**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Число байтов, зарезервированных в журнале базы данных для системных транзакций от имени данной транзакции.|  
|database_transaction_begin_lsn|**numeric(25,0)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Регистрационный номер транзакции в журнале (номер LSN) начальной записи для данной транзакции в журнале базы данных.|  
|database_transaction_last_lsn|**numeric(25,0)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Номер LSN последней сохраненной записи для данной транзакции в журнале базы данных.|  
|database_transaction_most_recent_savepoint_lsn|**numeric(25,0)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Номер LSN самой последней точки сохранения для данной транзакции в журнале базы данных.|  
|database_transaction_commit_lsn|**numeric(25,0)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Номер LSN записи фиксации для данной транзакции в журнале базы данных.|  
|database_transaction_last_rollback_lsn|**numeric(25,0)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Номер LSN транзакции в журнале, до которой произошел последний откат. Если откат не выполнен, используется значение Макслсн.|  
|database_transaction_next_undo_lsn|**numeric(25,0)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Номер LSN следующей записи для отката.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также:  
 [sys.dm_tran_active_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-transactions-transact-sql.md)   
 [sys.dm_tran_session_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с транзакциями (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


