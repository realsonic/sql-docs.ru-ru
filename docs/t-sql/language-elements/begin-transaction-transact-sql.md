---
description: BEGIN TRANSACTION (Transact-SQL)
title: BEGIN TRANSACTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BEGIN_TRANSACTION_TSQL
- TRANSACTION_TSQL
- TRANSACTION
- BEGIN TRANSACTION
- BEGIN TRAN
- BEGIN_TRAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction logs [SQL Server], BEGIN TRANSACTION statement
- marked transactions [SQL Server], BEGIN TRANSACTION statement
- BEGIN TRANSACTION statement
- local transactions [SQL Server]
- marking transactions [SQL Server]
- transactions [SQL Server], starting
- transaction names [SQL Server]
- starting point marked for transactions
- starting transactions
ms.assetid: c6258df4-11f1-416a-816b-54f98c11145e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e36a40d8db05de839f1fedc6844c1d6cd907e885
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102546"
---
# <a name="begin-transaction-transact-sql"></a>BEGIN TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Отмечает начальную точку явной локальной транзакции. Явные транзакции начинаются с инструкции BEGIN TRANSACTION и заканчиваются инструкцией COMMIT или ROLLBACK.  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
--Applies to SQL Server and Azure SQL Database
 
BEGIN { TRAN | TRANSACTION }   
    [ { transaction_name | @tran_name_variable }  
      [ WITH MARK [ 'description' ] ]  
    ]  
[ ; ]  
```  
 
```syntaxsql
--Applies to Azure Synapse Analytics and Parallel Data Warehouse
 
BEGIN { TRAN | TRANSACTION }   
[ ; ]  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *transaction_name*  
 **ПРИМЕНИМО К**: SQL Server (начиная с версии 2008), база данных SQL Azure
 
 Имя, присвоенное транзакции. Аргумент *transaction_name* должен соответствовать правилам для идентификаторов, однако не допускаются идентификаторы длиннее 32 символов. Имена транзакций используются только для самых внешних вложенных инструкций BEGIN...COMMIT или BEGIN...ROLLBACK. Аргумент *transaction_name* всегда учитывает регистр, даже если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] регистр не учитывает.  
  
 @*tran_name_variable*  
 **ПРИМЕНИМО К**: SQL Server (начиная с версии 2008), база данных SQL Azure
 
 Имя определенной пользователем переменной, содержащей допустимое имя транзакции. Переменная должна быть объявлена с типом данных **char**, **varchar**, **nchar** или **nvarchar**. Если переменной передается больше 32 символов, используются только 32 первых символа, а остальные усекаются.  
  
 WITH MARK [ '*description*' ]  
**ПРИМЕНИМО К**: SQL Server (начиная с версии 2008), база данных SQL Azure

Указывает, что транзакция отмечается в журнале. Значение аргумента *description* — это строка, описывающая отметку. Значения аргумента *description* длиннее 128 символов усекаются до 128 символов перед сохранением в таблице msdb.dbo.logmarkhistory.  
  
 Если используется предложение WITH MARK, необходимо указать имя транзакции. Предложение WITH MARK позволяет восстановить журнал транзакций до именованной отметки.  
  
## <a name="general-remarks"></a>Общие замечания
Инструкция BEGIN TRANSACTION увеличивает значение @@TRANCOUNT на 1.
  
Инструкция BEGIN TRANSACTION предоставляет точку, где гарантируется логическая и физическая согласованность данных, на которые ссылается соединение. При возникновении ошибок все изменения, внесенные в данные после выполнения инструкции BEGIN TRANSACTION, можно откатить, чтобы вернуть данные в известное согласованное состояние. Каждая транзакция продолжается до тех пор, пока она не завершается без ошибок и выполняется инструкция COMMIT TRANSACTION, чтобы внести изменения в базу данных. В случае возникновения ошибок все изменения удаляются с помощью инструкции ROLLBACK TRANSACTION.  
  
Инструкция BEGIN TRANSACTION запускает локальную транзакцию для соединения, выполняющего эту инструкцию. В зависимости от текущих параметров уровня изоляции транзакции многие ресурсы, необходимые для поддержки выполняемых соединением инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], блокируются транзакцией до ее завершения с помощью инструкций COMMIT TRANSACTION и ROLLBACK TRANSACTION. Транзакции, долгое время ожидающие обработки, могут препятствовать доступу других пользователей к заблокированным ресурсам и усечения транзакций в журнале.  
  
 Хотя инструкция BEGIN TRANSACTION запускает локальную транзакцию, она не записывается в журнал транзакций, пока приложение не выполнит действие, которое должно быть записано в журнал, например инструкцию INSERT, UPDATE или DELETE. Приложение может выполнять такие действия, как получение блокировки для защиты уровня изоляции транзакции инструкции SELECT, но ни одно действие не записывается в журнал, пока приложение не выполнит действие, изменяющее данные.  
  
 Присвоение имен нескольким транзакциям в последовательности вложенных транзакций мало влияет на транзакцию. Системой регистрируется только первое (самое внешнее) имя транзакции. Откат к другому имени (не считая допустимого имени точки сохранения) приводит к формированию ошибки. В момент возникновения такой ошибки на самом деле не выполняется откат инструкций. Для этих инструкций выполняется откат только при откате внешней транзакции.  
  
 Локальная транзакция, запущенная инструкцией BEGIN TRANSACTION, повышается до распределенной транзакции, если до фиксации или отката инструкции выполняются следующие действия.  
  
-   Выполняется инструкция INSERT, DELETE или UPDATE, ссылающаяся на удаленную таблицу или связанный сервер. Инструкция INSERT, UPDATE или DELETE завершается неудачно, если поставщик OLE DB, который используется для доступа к связанному серверу, не поддерживает интерфейс ITransactionJoin.  
  
-   Вызывается удаленная хранимая процедура, если для параметра REMOTE_PROC_TRANSACTIONS установлено значение ON.  
  
 Локальная копия [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] становится контроллером транзакции и использует координатор распределенных транзакций [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC) для управления распределенной транзакцией.  
  
 Транзакция может выполняться явно как распределенная с помощью инструкции BEGIN DISTRIBUTED TRANSACTION. Дополнительные сведения см. в статье [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
 Если SET IMPLICIT_TRANSACTIONS задано как ON, то инструкция BEGIN TRANSACTION создает две вложенные транзакции. Дополнительные сведения см. в статье [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md).  
  
## <a name="marked-transactions"></a>Помеченные транзакции  
 Если используется параметр WITH MARK, имя транзакции помещается в журнал транзакций. При восстановлении базы данных из копии до прежнего состояния вместо даты и времени может использоваться помеченная транзакция. Дополнительные сведения см. в статьях [Использование помеченных транзакций для согласованного восстановления связанных баз данных (модель полного восстановления)](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md) и [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 Кроме того, отметки в журнале транзакций необходимы, если нужно восстановить набор взаимосвязанных баз данных до логически согласованного состояния. Отметки можно записывать в журналы транзакций взаимосвязанных баз данных с помощью распределенной транзакции. Восстановление набора взаимосвязанных баз данных по журналу до этих отметок приводит к транзакционной согласованности этого набора баз данных. Размещение отметок в связанных базах данных требует использования специальных процедур.  
  
 Отметка записывается в журнал транзакций только в том случае, если база данных обновляется помеченной транзакцией. Транзакции, не изменяющие данные, не отмечаются.  
  
 Инструкцию BEGIN TRAN *new_name* WITH MARK можно вложить в уже существующую непомеченную транзакцию. При этом значением параметра *new_name* становится имя метки транзакции несмотря на то, что транзакция может иметь имя. В следующем примере `M2` является именем метки.  
  
```sql  
BEGIN TRAN T1;  
UPDATE table1 ...;  
BEGIN TRAN M2 WITH MARK;  
UPDATE table2 ...;  
SELECT * from table1;  
COMMIT TRAN M2;  
UPDATE table3 ...;  
COMMIT TRAN T1;  
```  
  
 При существовании вложенности транзакций попытка отметить транзакцию, уже имеющую метку, приводит к следующему предупреждению (не ошибке):  
  
 BEGIN TRAN T1 WITH MARK ...;  
  
 UPDATE table1 ...;  
  
 BEGIN TRAN M2 WITH MARK ...;  
  
 "Сервер: сообщение 3920, уровень 16, состояние 1, строка 3"  
  
 «Параметр WITH MARK применяется только к первой инструкции BEGIN TRAN WITH MARK.»  
  
 «Параметр не обрабатывается.»  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли public.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-an-explicit-transaction"></a>A. Использование явной транзакции
**ПРИМЕНИМО К:** SQL Server (начиная с версии 2008), База данных SQL Azure, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Parallel Data Warehouse

В примере используется база данных AdventureWorks. 

```sql
BEGIN TRANSACTION;  
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT;  
```

### <a name="b-rolling-back-a-transaction"></a>Б. Откат транзакции
**ПРИМЕНИМО К:** SQL Server (начиная с версии 2008), База данных SQL Azure, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Parallel Data Warehouse

В приведенном ниже примере демонстрируется результат отката транзакции. В этом примере инструкция ROLLBACK приведет к откату инструкции INSERT, но созданная таблица будет по-прежнему существовать.

```sql
CREATE TABLE ValueTable (id INT);  
BEGIN TRANSACTION;  
       INSERT INTO ValueTable VALUES(1);  
       INSERT INTO ValueTable VALUES(2);  
ROLLBACK;  

```

### <a name="c-naming-a-transaction"></a>В. Присвоение транзакции имени 
**ПРИМЕНИМО К**: SQL Server (начиная с версии 2008), база данных SQL Azure

В следующем примере показано, как присвоить транзакции имя.  
  
```sql
DECLARE @TranName VARCHAR(20);  
SELECT @TranName = 'MyTransaction';  
  
BEGIN TRANSACTION @TranName;  
USE AdventureWorks2012;  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
  
COMMIT TRANSACTION @TranName;  
GO  
```  
  
### <a name="d-marking-a-transaction"></a>Г. Пометка транзакции  
**ПРИМЕНИМО К**: SQL Server (начиная с версии 2008), база данных SQL Azure

В следующем примере показано, как пометить транзакцию. Транзакция `CandidateDelete` помечена.  
  
```sql  
BEGIN TRANSACTION CandidateDelete  
    WITH MARK N'Deleting a Job Candidate';  
GO  
USE AdventureWorks2012;  
GO  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
GO  
COMMIT TRANSACTION CandidateDelete;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION (Transact-SQL)](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
