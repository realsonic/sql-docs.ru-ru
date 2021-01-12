---
description: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
title: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_active_snapshot_database_transactions_TSQL
- dm_tran_active_snapshot_database_transactions
- sys.dm_tran_active_snapshot_database_transactions
- dm_tran_active_snapshot_database_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_snapshot_database_transactions dynamic management view
ms.assetid: 55b83f9c-da10-4e65-9846-f4ef3c0c0f36
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24259c4e5842243545b9fc02d07c6b7a33387289
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094012"
---
# <a name="sysdm_tran_active_snapshot_database_transactions-transact-sql"></a>sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это динамическое административное представление возвращает виртуальную таблицу всех активных транзакций, формирующих или потенциально получающих доступ к версиям строк. Транзакции включаются для одного или нескольких из следующих условий.  
  
-   Если одному или обоим параметрам базы данных ALLOW_SNAPSHOT_ISOLATION и READ_COMMITTED_SNAPSHOT присвоены значения ON.  
  
    -   Для каждой транзакции имеется одна строка, запускающаяся на уровне изоляции моментальных снимков либо на уровне READ COMMITTED с использованием управления версиями строк.  
  
    -   Для каждой транзакции имеется одна строка, вызывающая создание версии строки в текущей базе данных. Например, транзакция формирует версию строки путем обновления или удаления строки из текущей базы данных.  
  
-   При срабатывании триггера имеется одна строка для транзакции, в которой выполняется триггер.  
  
-   При запуске процедуры индексирования в сети имеется одна строка для транзакции, которая создает индекс.  
  
-   При включенном режиме MARS имеется одна строка для каждой транзакции, которая получает доступ к версиям строк.  
  
 Это динамическое административное представление не включает в себя системные транзакции.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_tran_active_snapshot_database_transactions**.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.dm_tran_active_snapshot_database_transactions  
```  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|Уникальный идентификатор, присвоенный транзакции. Идентификатор транзакции используется главным образом для определения транзакции при операциях блокировки.|  
|**transaction_sequence_num**|**bigint**|Порядковый номер транзакции. Порядковый номер транзакции является уникальным и присваивается транзакции в момент ее запуска. Транзакции, которые не создают записи версий и не используют сканирование моментальных снимков, не получат порядковый номер.|  
|**commit_sequence_num**|**bigint**|Порядковый номер, который указывает, когда транзакция заканчивается (фиксируется или останавливается). Для активных транзакций значение равно NULL.|  
|**is_snapshot**|**int**|0 = транзакция, отличная от изоляции моментального снимка.<br /><br /> 1 = транзакция изоляции моментального снимка.|  
|**session_id**|**int**|Идентификатор сеанса, в рамках которого была инициирована транзакция.|  
|**first_snapshot_sequence_num**|**bigint**|Наименьший порядковый номер транзакции, которая была активна при получении моментального снимка. При выполнении транзакции моментального снимка она формирует моментальный снимок активных в этот момент транзакций. Для транзакций, не связанных с моментальными снимками, в этом столбце отображается 0.|  
|**max_version_chain_traversed**|**int**|Максимальная длина цепочки версий, пройденной в поисках транзакционно согласованной версии.|  
|**average_version_chain_traversed**|**real**|Среднее число версий строк по всем пройденным цепочкам версий.|  
|**elapsed_time_seconds**|**bigint**|Время, истекшее с момента, когда транзакция получила свой порядковый номер.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="remarks"></a>Комментарии  
 **sys.dm_tran_active_snapshot_database_transactions** сообщает транзакции, которым назначен последовательный номер транзакции (XSN). Порядковый номер XSN назначается при первом доступе транзакции к хранилищу версий. В следующих примерах показано, как в базе данных, для которой включена изоляция моментальных снимков или READ COMMITTED с использованием управления версиями строк, транзакции назначается номер XSN.  
  
-   Если транзакция выполняется на упорядочиваемом уровне изоляции, номер XSN назначается при первом выполнении транзакцией какой-либо инструкции, например операции UPDATE, в ходе которой создается версия строки.  
  
-   Если транзакция выполняется на уровне изоляции моментальных снимков, номер XSN назначается при выполнении какой-либо инструкции языка обработки данных (DML), включая операцию SELECT.  
  
 Порядковые номера транзакций увеличиваются последовательно для каждой транзакции, создаваемой в экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Примеры  
 Следующий пример использует тестовый сценарий, содержащий четыре параллельные транзакции, идентифицированные порядковыми номерами (XSN), который выполняется в базе данных с параметрами ALLOW_SNAPSHOT_ISOLATION и READ_COMMITTED_SNAPSHOT, установленными в значение ON. Следующие транзакции запущены:  
  
-   XSN-57 является операцией обновления с сериализуемой изоляцией.  
  
-   XSN-58 аналогична XSN-57.  
  
-   XSN-59 — операция выборки с уровнем изоляции моментальных снимков.  
  
-   XSN-60 — то же, что и XSN-59.  
  
 Выполнен следующий запрос.  
  
```  
SELECT   
    transaction_id,  
    transaction_sequence_num,  
    commit_sequence_num,  
    is_snapshot session_id,  
    first_snapshot_sequence_num,  
    max_version_chain_traversed,  
    average_version_chain_traversed,  
    elapsed_time_seconds  
  FROM sys.dm_tran_active_snapshot_database_transactions;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_id  transaction_sequence_num  commit_sequence_num  
--------------  ------------------------  -------------------  
9295            57                        NULL  
9324            58                        NULL  
9387            59                        NULL  
9400            60                        NULL  
  
is_snapshot  session_id   first_snapshot_sequence_num  
-----------  -----------  ---------------------------  
0            54           0  
0            53           0  
1            52           57  
1            51           57  
  
max_version_chain_traversed  average_version_chain_traversed  
---------------------------  -------------------------------  
0                            0  
0                            0  
1                            1  
1                            1  
  
elapsed_time_seconds  
--------------------  
419  
397  
359  
333  
```  
  
 Следующая информация оценивает результаты **sys.dm_tran_active_snapshot_database_transactions**:  
  
-   XSN-57: так как эта транзакция не выполняется при изоляции моментального снимка, это `is_snapshot` значение и `first_snapshot_sequence_num` `0` . Аргумент `transaction_sequence_num` показывает, что данной транзакции был присвоен порядковый номер, поскольку параметр READ_COMMITTED_SNAPSHOT или ALLOW_SNAPSHOT_ISOLATION (или оба) имеют значение ON.  
  
-   XSN-58: Эта транзакция не выполняется при изоляции моментального снимка и применяется та же информация для XSN-57.  
  
-   XSN-59: это первая активная транзакция, которая выполняется в режиме изоляции моментального снимка. Транзакция считывает данные, зафиксированные транзакцией XSN-57, на что указывает параметр `first_snapshot_sequence_num`. Вывод этой транзакции также указывает на то, что максимальная цепочка версий, пройденная для строки, равна `1` и что для каждой строки, к которой был получен доступ, пройдена в среднем `1` версия. Это означает, что транзакции XSN-57, XSN-58 и XSN-60 не изменяли строки и были зафиксированы.  
  
-   XSN-60: это вторая транзакция, выполняемая при изоляции моментального снимка. Вывод содержит такие же сведения, что и для транзакции XSN-59.  
  
## <a name="see-also"></a>См. также:  
 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с транзакциями (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


