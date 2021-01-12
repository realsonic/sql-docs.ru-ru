---
description: ALTER QUEUE (Transact-SQL)
title: ALTER QUEUE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/01/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_QUEUE_TSQL
- ALTER QUEUE
dev_langs:
- TSQL
helpviewer_keywords:
- number of queue readers
- modifying queues
- ALTER QUEUE statement
- queue readers [SQL Server]
- queues [Service Broker], modifying
- unavailable queues [SQL Server]
- activation stored procedures [Service Broker]
ms.assetid: d54aa325-8761-4cd4-8da7-acf33df12296
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 385a84a0f92c41cbf661abe327d244a2e9b6ebd3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092095"
---
# <a name="alter-queue-transact-sql"></a>ALTER QUEUE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Изменяет свойства очереди.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER QUEUE <object>   
   queue_settings  
   | queue_action  
[ ; ]  
  
<object> : :=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
  
<queue_settings> : :=  
WITH  
   [ STATUS = { ON | OFF } [ , ] ]  
   [ RETENTION = { ON | OFF } [ , ] ]  
   [ ACTIVATION (  
       { [ STATUS = { ON | OFF } [ , ] ]   
         [ PROCEDURE_NAME = <procedure> [ , ] ]  
         [ MAX_QUEUE_READERS = max_readers [ , ] ]  
         [ EXECUTE AS { SELF | 'user_name'  | OWNER } ]  
       |  DROP }  
          ) [ , ]]  
         [ POISON_MESSAGE_HANDLING (  
          STATUS = { ON | OFF } )  
         ]   
  
<queue_action> : :=  
   REBUILD [ WITH <query_rebuild_options> ]  
   | REORGANIZE [ WITH (LOB_COMPACTION = { ON | OFF } ) ]  
   | MOVE TO { file_group | "default" }  
  
<procedure> : :=  
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }
  
<queue_rebuild_options> : :=  
{  
   ( MAXDOP = max_degree_of_parallelism )  
}  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *database_name* (объект)  
 Название базы данных, которая содержит изменяемую очередь. Если аргумент *database_name* не указан, по умолчанию используется текущая база данных.  
  
 *schema_name* (объект)  
 Имя схемы, которой принадлежит новая очередь. Если аргумент *schema_name* не указан, по умолчанию используется схема по умолчанию текущего пользователя.  
  
 *queue_name*  
 Имя изменяемой очереди.  
  
 STATUS (очередь)  
 Указывает, доступна очередь (ON) или нет (OFF). Когда очередь недоступна, нельзя ни добавлять в нее сообщения, ни удалять их из нее.  
  
 RETENTION  
 Указывает параметр хранения для очереди. Если указано RETENTION = ON, все сообщения, посылаемые или отправляемые во время диалогов, которые используют данную очередь, хранятся в очереди до окончания этих диалогов. Это позволяет сохранять сообщения для целей ревизии или для выполнения компенсирующих транзакций, если возникают ошибки  
  
> [!NOTE]  
>  Установка RETENTION = ON может уменьшить производительность. Эта установка должна использоваться только тогда, когда необходимо достичь соглашения уровня службы для приложения.  
  
 ACTIVATION  
 Указывает сведения о хранимой процедуре, которая активизируется для обработки сообщений, которые поступают в данную очередь.  
  
 STATUS (активация)  
 Указывает, активирует ли очередь хранимую процедуру. Если параметр STATUS = ON, то очередь запускает хранимую процедуру, указанную параметром PROCEDURE_NAME, если количество выполняемых в настоящий момент хранимых процедур меньше, чем значение MAX_QUEUE_READERS, и если сообщения прибывают в очередь быстрее, чем хранимые процедуры получают сообщения. Если параметр STATUS = OFF, очередь не активирует хранимую процедуру.  
  
 REBUILD [ WITH \<queue_rebuild_options> ]  
 **Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.  
  
 Перестраивает все индексы внутренней таблицы очереди. Используйте эту возможность в случае проблем с фрагментацией из-за высокой нагрузки. MAXDOP — это единственный поддерживаемый параметр перестройки очереди. Операция REBUILD всегда выполняется в автономном режиме.  
  
 REORGANIZE [ WITH ( LOB_COMPACTION = { ON | OFF } ) ]  
 **Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.  
  
 Реорганизует все индексы внутренней таблицы очереди.   
В отличие от операции REORGANIZE в пользовательских таблицах, операция REORGANIZE в очереди всегда выполняется в автономном режиме, потому что в очередях явно отключена блокировка на уровне страницы.  
  
> [!TIP]  
>  Как правило, если фрагментация составляет от 5 до 30 %, необходима реорганизация индекса. Если фрагментация больше 30 %, необходима перестройка индекса. Однако эти числа приводятся только в виде общих рекомендаций в качестве отправной точки для вашей среды. Чтобы определить степень фрагментации индекса, используйте [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) — см. пример Ж в этой статье.  
  
 MOVE TO { *file_group* | "default" }  
 **Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.  
  
 Перемещает внутреннюю таблицу очереди (с индексами) в указанную пользователем файловую группу.  Новая файловая группа не должна быть доступна только для чтения.  
  
 PROCEDURE_NAME = \<procedure>  
 Указывает имя активируемой хранимой процедуры, если очередь содержит сообщения, которые нужно обработать. Это значение должно являться идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *database_name* (процедура)  
 Имя базы данных, которая содержит хранимую процедуру.  
  
 *schema_name* (процедура)  
 Имя схемы, которой принадлежит хранимая процедура.  
  
 *stored_procedure_name*  
 Имя хранимой процедуры.  
  
 MAX_QUEUE_READERS =*max_reader*  
 Указывает максимальное число экземпляров хранимой процедуры активации, которые очередь одновременно может запустить. Значение аргумента *max_readers* должно быть числом от 0 до 32767.  
  
 EXECUTE AS  
 Указывает учетную запись пользователя базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], от имени которого выполняется хранимая процедура активации. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен иметь возможность проверить разрешения для этого пользователя в момент, когда очередь активирует хранимую процедуру. Для пользователя домена Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен быть подключен к домену и иметь возможность проверить разрешения указанного пользователя при активизации процедуры или неудачной попытке запуска процедуры. Для пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сервер всегда в состоянии проверить разрешения.  
  
 SELF  
 Указывает, что хранимая процедура выполняется как текущий пользователь. (Участник базы данных, выполняющий эту инструкцию ALTER QUEUE)  
  
 '*user_name*'  
 Имя пользователя, от имени которого выполняется хранимая процедура. Аргумент *user_name* должен быть именем допустимого пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], указанным в виде идентификатора [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пользователь должен иметь разрешение IMPERSONATE на указанного аргументом *user_name* пользователя.  
  
 OWNER  
 Указывает, что хранимая процедура выполняется в контексте владельца очереди.  
  
 DROP  
 Удаляет все сведения об активации, ассоциированные с очередью.  
  
 POISON_MESSAGE_HANDLING  
 Указывает, включена ли обработка сообщений о сбое. Значение по умолчанию — ON.  
  
 Очередь, в которой параметру обработки сообщений о сбое задано значение OFF, не будет отключена после пяти последовательных откатов транзакций. Это позволяет приложению определить пользовательскую систему обработки опасных сообщений.  
  
## <a name="remarks"></a>Remarks  
 Если очередь с указанной хранимой процедурой активации содержит сообщения, которые изменяют состояние активации с OFF на ON, тут же активирует хранимую процедуру активации. Изменение состояния активации с ON на OFF прекращает активацию экземпляров хранимой процедуры брокером, но не останавливает экземпляры хранимой процедуры, которые уже запущены.  
  
 Изменение очереди для добавления хранимой процедуры активации не изменяет состояния активации очереди. Изменение хранимой процедуры активации для очереди не влияет на экземпляры хранимых процедур активации, которые уже запущены.  
  
 Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] проверяет максимальное число агентов чтения очереди для очереди в качестве части процесса активации. Поэтому, изменение очереди с целью увеличения максимального числа агентов чтения очереди позволяет компоненту [!INCLUDE[ssSB](../../includes/sssb-md.md)] немедленно запустить больше экземпляров хранимой процедуры активации. Изменение очереди с целью уменьшения максимального числа агентов чтения очереди не влияет на экземпляры хранимой процедуры активации, которые уже запущены. Однако компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] не запускает новый экземпляр хранимой процедуры до тех пор, пока число экземпляров хранимой процедуры активации не уменьшится до заданного максимального числа.  
  
 Если очередь недоступна, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] сохраняет сообщения для служб, пользующихся данной очередью, в очереди передачи для базы данных. Представление каталога [sys.transmission_queue](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md) содержит представление очереди передачи.  
  
 Если инструкция RECEIVE или инструкция GET CONVERSATION GROUP указывает недоступную очередь, выполнение инструкции завершается ошибкой языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешением на изменения значений очереди обладает владелец типа сообщений, члены предопределенной роли базы данных db_ddladmin или db_owner и члены предопределенной роли сервера sysadmin.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-making-a-queue-unavailable"></a>A. Как сделать очередь недоступной  
 В этом примере очередь `ExpenseQueue` делается недоступной для приема сообщений.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH STATUS = OFF ;  
```  
  
### <a name="b-changing-the-activation-stored-procedure"></a>Б. Изменение хранимой процедуры активации  
 Следующий пример изменяет хранимую процедуру, которую запускает очередь. Хранимая процедура выполняется в контексте пользователя, выполнившего инструкцию `ALTER QUEUE`.  
  
```sql  
ALTER QUEUE ExpenseQueue  
    WITH ACTIVATION (  
        PROCEDURE_NAME = new_stored_proc,  
        EXECUTE AS SELF) ;  
```  
  
### <a name="c-changing-the-number-of-queue-readers"></a>В. Изменение числа агентов чтения очереди  
 Следующий пример устанавливает максимальное число экземпляров хранимых процедур, запускаемых компонентом [!INCLUDE[ssSB](../../includes/sssb-md.md)] для этой очереди, равное `7`.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH ACTIVATION (MAX_QUEUE_READERS = 7) ;  
```  
  
### <a name="d-changing-the-activation-stored-procedure-and-the-execute-as-account"></a>Г. Изменение хранимой процедуры активации и учетной записи EXECUTE AS  
 Следующий пример изменяет хранимую процедуру, которую запускает компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Хранимая процедура выполняется в контексте пользователя `SecurityAccount`.  
  
```sql  
ALTER QUEUE ExpenseQueue  
    WITH ACTIVATION (  
        PROCEDURE_NAME = AdventureWorks2012.dbo.new_stored_proc ,  
        EXECUTE AS 'SecurityAccount') ;  
```  
  
### <a name="e-setting-the-queue-to-retain-messages"></a>Д. Настройка очереди на хранение сообщений  
 Следующий пример настраивает очередь на хранение сообщений. Очередь хранит все сообщения, отправленные службам или службами, которые используют эту очередь, до тех пор, пока не завершится диалог, который содержит сообщения.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH RETENTION = ON ;  
```  
  
### <a name="f-removing-activation-from-a-queue"></a>Е. Удаление активации из очереди  
 Следующий пример удаляет все сведения активации из очереди.  
  
```  
ALTER QUEUE ExpenseQueue WITH ACTIVATION (DROP) ;  
```  
  
### <a name="g-rebuilding-queue-indexes"></a>Ж. Перестроение индексов очереди  
  
**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.  
  
 В следующем примере производится перестроение индексов очереди  
  
```sql  
ALTER QUEUE ExpenseQueue REBUILD WITH (MAXDOP = 2)   
```  
  
### <a name="h-reorganizing-queue-indexes"></a>З. Реорганизация индексов очереди  
  
**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.  
  
 В следующем примере производится реорганизация индексов очереди  
  
```sql  
ALTER QUEUE ExpenseQueue REORGANIZE   
```  
  
### <a name="i-moving-queue-internal-table-to-another-filegroup"></a>И. Перемещение внутренней таблицы очереди в другую файловую группу  
  
**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.  
  
```sql  
ALTER QUEUE ExpenseQueue MOVE TO [NewFilegroup]   
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE QUEUE (Transact-SQL)](../../t-sql/statements/create-queue-transact-sql.md)   
 [DROP QUEUE (Transact-SQL)](../../t-sql/statements/drop-queue-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
  
  
