---
description: KILL (Transact-SQL)
title: KILL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL_TSQL
- KILL
dev_langs:
- TSQL
helpviewer_keywords:
- WITH STATUSONLY option
- terminating distributed transactions
- distributed transactions [SQL Server], terminating
- in-doubt transactions
- IDs [SQL Server], user processes
- ending distributed transactions [SQL Server]
- stopping distributed transactions
- session IDs [SQL Server]
- system process termination [SQL Server]
- stopping processes
- ending processes [SQL Server]
- UOW [SQL Server]
- orphaned transactions [SQL Server]
- unit of work [SQL Server]
- process termination [SQL Server]
- KILL statement
- terminating process
ms.assetid: 071cf260-c794-4b45-adc0-0e64097938c0
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 13afe3aa357bfd968874ae1f89bf0720c39fe49c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644460"
---
# <a name="kill-transact-sql"></a>KILL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Завершает пользовательский процесс, определяемый идентификатором сеанса или единицей работы (UOW). Если для завершения процесса, указанного с помощью идентификатора сеанса или UOW, необходимо отменить большое количество операций, для выполнения инструкции KILL может понадобиться значительное время. Процесс особенно затягивается при откате длительной транзакции.  
  
Инструкция KILL завершает нормальное подключение, которое внутренне прерывает транзакции, связанные с заданным идентификатором сеанса. В некоторых случаях используется координатор распределенных транзакций [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC). Когда он используется, эту инструкцию также можно применять для завершения потерянных и сомнительных распределенных транзакций.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server  
  
KILL { session ID [ WITH STATUSONLY ] | UOW [ WITH STATUSONLY | COMMIT | ROLLBACK ] }    

```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
KILL 'session_id'  
[;]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

_Идентификатор сеанса_   
Идентификатор сеанса завершаемого процесса. `session_id` является уникальным целым числом (**int**), которое при подключении присваивается каждому соединению пользователя. Значение идентификатора сеанса остается привязанным к соединению в течение всего времени соединения. По окончании соединения данное целое значение освобождается и может быть присвоено другому соединению.  

Следующий запрос может помочь выявить `session_id`, который требуется удалить:  

 ```sql  
 SELECT conn.session_id, host_name, program_name,
     nt_domain, login_name, connect_time, last_request_end_time 
FROM sys.dm_exec_sessions AS sess
JOIN sys.dm_exec_connections AS conn
    ON sess.session_id = conn.session_id;

```  


_UOW_   
Задает идентификатор единицы работы (UOW) распределенных транзакций. _UOW_ представляет собой идентификатор GUID, который можно получить из столбца request_owner_guid динамического административного представления `sys.dm_tran_locks`. _UOW_ также может быть получен из журнала ошибок или из монитора транзакций MS DTC. Дополнительные сведения о наблюдении за распределенными транзакциями см. в руководстве пользователя MS DTC.  
  
Чтобы прервать неразрешенные распределенные транзакции, используйте инструкцию KILL \<UOW>. Эти транзакции не связаны с идентификаторами реальных сеансов, но искусственно связаны с идентификатором сеанса -2. Этот идентификатор упрощает поиск неразрешенных транзакций с помощью запроса столбца идентификатора сеанса из динамических административных представлений `sys.dm_tran_locks`, ` sys.dm_exec_sessions` или `sys.dm_exec_requests`.  

_WITH STATUSONLY_   
Создает отчет о состоянии для указанного _UOW_ или `session_id`, которые отменяются из-за выполненной ранее инструкции KILL. Инструкция KILL WITH STATUSONLY не завершает и не откатывает UOW или сеанс с определенным идентификатором. Команда только отображает текущее состояние отката.

_WITH COMMIT_   
Используется для завершения неразрешенной распределенной транзакции с фиксацией. Применяется только к распределенным транзакциям и требует указания _UOW_.  Дополнительные сведения см. в разделе о [распределенных транзакциях](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions).

_WITH ROLLBACK_   
Используется для завершения неразрешенной распределенной транзакции с откатом. Применяется только к распределенным транзакциям и требует указания _UOW_.  Дополнительные сведения см. в разделе о [распределенных транзакциях](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions).


  
## <a name="remarks"></a>Remarks  
Инструкция KILL обычно используется для прерывания процессов, блокирующих другие важные процессы или выполняющих запросы, занимающие необходимые системные ресурсы. Системные процессы и процессы, выполняющие расширенные хранимые процедуры, не могут быть прерваны.  
  
Инструкцию KILL необходимо использовать осторожно, особенно при выполнении критических процессов. Существует опасность прерывания собственного процесса. Не следует также прерывать следующие процессы:  
  
-   AWAITING COMMAND  
-   CHECKPOINT SLEEP  
-   LAZY WRITER  
-   LOCK MONITOR;  
-   SIGNAL HANDLER  
  
Аргумент @@SPID позволяет отобразить идентификатор текущего сеанса.  
  
Чтобы получить отчет о значениях идентификаторов активных сеансов, можно запросить столбец session_id из динамических административных представлений sys.dm_tran_locks, sys.dm_exec_sessions и sys.dm_exec_requests. Можно также посмотреть столбец SPID, возвращаемый системной хранимой процедурой sp_who. Если откат выполняется для конкретного SPID, столбец cmd результирующего набора процедуры sp_who для этого SPID будет содержать значение KILLED/ROLLBACK.  
  
Если какое-либо соединение имеет блокировку на ресурс базы данных и блокирует обработку остальных соединений, идентификаторы сеансов заблокированных соединений появятся в столбце `blocking_session_id` представления `sys.dm_exec_requests` или в столбце `blk`, возвращенном хранимой процедурой `sp_who`.  
  
Инструкцию KILL можно применять для решения проблем с сомнительными распределенными транзакциями. Эти транзакции представляют собой неразрешимые распределенные транзакции, возникшие в результате незапланированных повторных стартов сервера баз данных или координатора MS DTC. Дополнительные сведения о сомнительных транзакциях см. в разделе "Двухфазная фиксация" в статье [Использование помеченных транзакций для согласованного восстановления связанных баз данных (модель полного восстановления)](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="using-with-statusonly"></a>Использование предложения WITH STATUSONLY  
Инструкция KILL WITH STATUSONLY создает отчет, если для сеанса или UOW выполняется откат, вызванный введенной ранее инструкцией KILL _ИД сеанса_|_единица работы_. Отчет о состоянии отображает процесс выполнения отката (в процентах), а также выводит ожидаемое время до окончания операции (в секундах). В отчете это отображается в следующей форме:  
  
`Spid|UOW <xxx>: Transaction rollback in progress. Estimated rollback completion: <yy>% Estimated time left: <zz> seconds`  
  
Если откат единицы работы или сеанса по ИД завершается до выполнения инструкции KILL _ИД сеанса_|_UOW_ WITH STATUSONLY, выполнение инструкции KILL _ИД сеанса_|_UOW_ WITH STATUSONLY возвращает следующую ошибку:  
  
```
"Msg 6120, Level 16, State 1, Line 1"  
"Status report cannot be obtained. Rollback operation for Process ID <session ID> is not in progress."
```  
Эта ошибка также возникает, если откат сеанса по ИД или UOW не выполняется,


Аналогичный отчет о состоянии может быть получен при повторном выполнении инструкции KILL _ИД сеанса_|_UOW_ без использования параметра WITH STATUSONLY. Однако не рекомендуется повторно выполнять эту инструкцию таким образом. Повторный вызов инструкции KILL _ИД сеанса_ может прервать новый процесс в случае, если процесс отката был завершен, а значение идентификатора сеанса было присвоено другой задаче до запуска новой инструкции KILL. Указание параметра WITH STATUSONLY предотвращает указанные последствия.  
  
## <a name="permissions"></a>Разрешения  
**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:** Необходимо разрешение ALTER ANY CONNECTION. Разрешение ALTER ANY CONNECTION включено с членством в предопределенных ролях сервера sysadmin или processadmin.  
  
**[!INCLUDE[ssSDS](../../includes/sssds-md.md)]:** необходимо разрешение KILL DATABASE CONNECTION. Имя входа субъекта серверного уровня имеет разрешение KILL DATABASE CONNECTION.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-kill-to-stop-a-session"></a>A. Использование инструкции KILL для остановки сеанса  
 В следующем примере показана остановка сеанса с идентификатором `53`.  
  
```sql  
KILL 53;  
GO  
```  
  
### <a name="b-using-kill-session-id-with-statusonly-to-obtain-a-progress-report"></a>Б. Использование инструкции KILL SPID WITH STATUSONLY для получения отчета о состоянии  
В данном примере создается отчет о состоянии процесса отката указанного сеанса.  
  
```sql  
KILL 54;  
KILL 54 WITH STATUSONLY;  
GO  
  
--This is the progress report.  
spid 54: Transaction rollback in progress. Estimated rollback completion: 80% Estimated time left: 10 seconds.  
```  
  
### <a name="c-using-kill-to-stop-an-orphaned-distributed-transaction"></a>В. Применение инструкции KILL для прерывания потерянной распределенной транзакции  
Следующий пример показывает, как прервать потерянную распределенную транзакцию (идентификатор сеанса: -2) с *UOW*, равным `D5499C66-E398-45CA-BF7E-DC9C194B48CF`.  
  
```sql  
KILL 'D5499C66-E398-45CA-BF7E-DC9C194B48CF';  
```  

  
## <a name="see-also"></a>См. также:  
[KILL STATS JOB (Transact-SQL)](../../t-sql/language-elements/kill-stats-job-transact-sql.md)   
[KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
[Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
[SHUTDOWN (Transact-SQL)](../../t-sql/language-elements/shutdown-transact-sql.md)   
[@@SPID (Transact-SQL)](../../t-sql/functions/spid-transact-sql.md)   
[sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
[sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
[sys.dm_tran_locks (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
[sp_lock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
[sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
  
