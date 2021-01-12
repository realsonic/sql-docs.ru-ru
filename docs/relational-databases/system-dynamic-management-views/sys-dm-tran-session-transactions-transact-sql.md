---
description: sys.dm_tran_session_transactions (Transact-SQL)
title: sys.dm_tran_session_transactions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_session_transactions
- sys.dm_tran_session_transactions
- sys.dm_tran_session_transactions_TSQL
- dm_tran_session_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_session_transactions dynamic management view
ms.assetid: c7157491-58c2-49fe-87d7-0c9723113adf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5dcb5a90c960d616ced6852e3b42bc29f5e7e92d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101466"
---
# <a name="sysdm_tran_session_transactions-transact-sql"></a>sys.dm_tran_session_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения о взаимосвязях связанных транзакций и сеансов.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_tran_session_transactions**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Идентификатор сеанса, в котором выполняется транзакция.|  
|transaction_id|**bigint**|Идентификатор транзакции.|  
|transaction_descriptor|**Binary (8)**|Идентификатор транзакции, который используется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для связи с драйвером клиента.|  
|enlist_count|**int**|Количество активных запросов транзакции в сеансе.|  
|is_user_transaction|**bit**|1 = транзакция была инициирована запросом пользователя.<br /><br /> 0 = системная транзакция.|  
|is_local|**bit**|1 = локальная транзакция.<br /><br /> 0 = распределенная транзакция или прикрепленная транзакция связанного сеанса.|  
|is_enlisted|**bit**|1 = является прикрепленной распределенной транзакцией.<br /><br /> 0 = не является прикрепленной распределенной транзакцией.|  
|is_bound|**bit**|1 = транзакция активна в сеансе через связанные сеансы.<br /><br /> 0 = транзакция не активна в сеансе через связанные сеансы.|  
|open_transaction_count||Количество открытых транзакций для каждого сеанса.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="remarks"></a>Комментарии  
 Через связанные сеансы и распределенные транзакции транзакция может запускаться в нескольких сеансах. В этом случае представление sys.dm_tran_session_transactions покажет несколько строк для одного идентификатора transaction_id — одну для каждого сеанса, в котором выполняется эта транзакция.  
  
 Посредством выполнения множественных запросов в режиме автофиксации с помощью режима MARS, при этом можно иметь несколько транзакций в одном сеансе. В этом случае представление sys.dm_tran_session_transactions покажет несколько строк для одного идентификатора session_id — одну для каждой транзакции, которая выполняется в этом сеансе.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с транзакциями (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


