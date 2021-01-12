---
description: sys.dm_tran_transactions_snapshot (Transact-SQL)
title: sys.dm_tran_transactions_snapshot (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_transactions_snapshot
- dm_tran_transactions_snapshot
- sys.dm_tran_transactions_snapshot_TSQL
- dm_tran_transactions_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_transactions_snapshot dynamic management view
ms.assetid: 03f64883-07ad-4092-8be0-31973348c647
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8c6596b92ecd000b0742886ab6565caf2521f701
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101439"
---
# <a name="sysdm_tran_transactions_snapshot-transact-sql"></a>sys.dm_tran_transactions_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает виртуальную таблицу для **sequence_number** транзакций, которые активны при запуске каждой транзакции моментального снимка. Данные, возвращаемые этим представлением, могут помочь:  
  
-   найти номера активных в настоящее время транзакций моментальных снимков;  
  
-   определить изменения данных, пропущенные обычной транзакцией моментальных снимков. Для транзакции, активной при запуске транзакции моментальных снимков, все измененные этой транзакцией данные (даже после того, как она будет зафиксирована) будут пропущены транзакцией моментальных снимков.  
  
 Например, рассмотрим следующие выходные данные **sys.dm_tran_transactions_snapshot**:  
  
```  
transaction_sequence_num snapshot_id snapshot_sequence_num  
------------------------ ----------- ---------------------  
59                       0           57  
59                       0           58  
60                       0           57  
60                       0           58  
60                       0           59  
60                       3           57  
60                       3           58  
60                       3           59  
60                       3           60  
```  
  
 Столбец `transaction_sequence_num` содержит последовательные номера транзакции (XSN) текущих транзакций моментальных снимков. В выходных данных их два: `59` и `60`. Столбец `snapshot_sequence_num` содержит последовательные номера транзакций, активных при запуске каждой транзакции моментальных снимков.  
  
 Видно, что запуск транзакции моментальных снимков XSN-59 произошел, когда были активны транзакции XSN-57 и XSN-58. Если в транзакциях XSN-57 или XSN-58 выполняется изменение данных, транзакция XSN-59 пропустит эти изменения и будет использовать управление версиями строк для поддержки транзакционно согласованного представления базы данных.  
  
 Транзакция моментальных снимков XSN-60 пропустит изменения данных, сделанные в транзакциях XSN-57, XSN-58 и XSN 59.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Последовательный номер транзакции моментальных снимков.|  
|**snapshot_id**|**int**|Идентификатор моментального снимка для каждой инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], вызванной в контексте чтения зафиксированных данных с помощью управления версиями строк. Этот идентификатор используется для формирования согласованного на уровне транзакций представления базы данных, поддерживающего выполнение каждого запроса в контексте управления версиями строк с чтением только зафиксированных данных.|  
|**snapshot_sequence_num**|**bigint**|Последовательный номер транзакции, которая была активна при запуске транзакции моментальных снимков.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="remarks"></a>Комментарии  
 При запуске транзакции моментальных снимков компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] регистрирует все активные транзакции. **sys.dm_tran_transactions_snapshot** сообщает эти сведения обо всех активных транзакциях моментальных снимков.  
  
 Каждая транзакция имеет последовательный номер, который назначается ей при запуске. Запуск транзакций начинается с вызова инструкции BEGIN TRANSACTION или BEGIN WORK. Однако последовательный номер транзакции назначается компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] при выполнении первой инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], следующей за инструкцией BEGIN TRANSACTION или BEGIN WORK. Последовательные номера транзакций увеличиваются с единичным интервалом.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с транзакциями (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  

