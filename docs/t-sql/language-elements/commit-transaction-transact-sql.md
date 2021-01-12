---
description: COMMIT TRANSACTION (Transact-SQL)
title: COMMIT TRANSACTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/09/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COMMIT
- COMMIT TRANSACTION
- COMMIT_TSQL
- COMMIT_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending transactions [SQL Server]
- user-defined transactions [SQL Server]
- committed transactions
- transactions [SQL Server], ending
- marking end of transactions [SQL Server]
- implicit transactions
- distributed transactions [SQL Server], committed
- transactions [SQL Server], committed
- COMMIT TRANSACTION statement
- rolling back transactions, COMMIT TRANSACTION
ms.assetid: f8fe26a9-7911-497e-b348-4e69c7435dc1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a322d2c707c819bb1d209965fef1aa5f747d0fac
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092228"
---
# <a name="commit-transaction-transact-sql"></a>COMMIT TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Отмечает успешное завершение явной или неявной транзакции. Если значение @@TRANCOUNT равно 1, то инструкция COMMIT TRANSACTION делает все изменения, произведенные с начала транзакции, постоянной частью базы данных, освобождает ресурсы транзакции и уменьшает значение параметра @@TRANCOUNT до 0. Если значение @@TRANCOUNT больше 1, инструкция COMMIT TRANSACTION уменьшает значение @@TRANCOUNT только на 1 и оставляет транзакцию активной.  
  
 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Applies to SQL Server (starting with 2008) and Azure SQL Database
  
COMMIT [ { TRAN | TRANSACTION }  [ transaction_name | @tran_name_variable ] ] [ WITH ( DELAYED_DURABILITY = { OFF | ON } ) ]  
[ ; ]  
```  
 
```syntaxsql
-- Applies to Azure Synapse Analytics and Parallel Data Warehouse Database
  
COMMIT [ TRAN | TRANSACTION ] 
[ ; ]  
``` 
 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *transaction_name*  
 **ПРИМЕНИМО К:** SQL Server и база данных SQL Azure
 
 Не учитывается компонентом [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. *transaction_name* указывает имя транзакции, присвоенное предыдущей инструкцией BEGIN TRANSACTION. Аргумент *transaction_name* должен соответствовать правилам для идентификаторов, но его длина не может превышать 32 символа. *transaction_name* сообщает программистам, с какой вложенной инструкцией BEGIN TRANSACTION связана инструкция COMMIT TRANSACTION.  
  
 *\@tran_name_variable*  
 **ПРИМЕНИМО К:** SQL Server и база данных SQL Azure  
 
Имя определенной пользователем переменной, содержащей допустимое имя транзакции. Переменная должна быть объявлена с типом данных char, varchar, nchar или nvarchar. Если переменной присваивается значение длиной более 32 символов, используются только первые 32, остальные усекаются.  
  
 DELAYED_DURABILITY  
 **ПРИМЕНИМО К:** SQL Server и база данных SQL Azure   

 Параметр, который запрашивает эту транзакцию, должен фиксироваться с задержанной устойчивостью. Этот запрос пропускается, если база данных была изменена с использованием `DELAYED_DURABILITY = DISABLED` или `DELAYED_DURABILITY = FORCED`. Дополнительные сведения см. в разделе [Управление устойчивостью транзакций](../../relational-databases/logs/control-transaction-durability.md).  
  
## <a name="remarks"></a>Remarks  
 Обязанностью программиста на языке [!INCLUDE[tsql](../../includes/tsql-md.md)] является вызов инструкции COMMIT TRANSACTION только в том случае, когда все данные, относящиеся к транзакции, логически верны.  
  
 Если зафиксированная транзакция является распределенной транзакцией [!INCLUDE[tsql](../../includes/tsql-md.md)], инструкция COMMIT TRANSACTION вызывает координатор MS DTC для использования двухфазного протокола фиксации на всех серверах, участвующих в транзакции. Если локальная транзакция охватывает две или более базы данных одного и того же экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], то экземпляр использует встроенную двухфазную фиксацию для всех баз данных, вызванных в транзакции.  
  
 При использовании вложенных транзакций фиксация внутренних транзакций не освобождает ресурсы и не делает их изменения постоянными. Изменения данных становятся постоянными и ресурсы освобождаются только при фиксации внешней транзакции. Вызов каждой инструкции COMMIT TRANSACTION приводит к тому, что если значение @@TRANCOUNT больше 1, то значение переменной @@TRANCOUNT просто уменьшается на 1. Когда значение @@TRANCOUNT уменьшится до нуля, вся внешняя транзакция фиксируется. Так как аргумент *transaction_name* не учитывается компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)], вызов инструкции COMMIT TRANSACTION, содержащей ссылку на имя внешней транзакции, приводит к тому, что если существуют необработанные внутренние транзакции, то значение @@TRANCOUNT уменьшается только на 1.  
  
 Вызов инструкции COMMIT TRANSACTION, когда значение параметра @@TRANCOUNT равно нулю, приводит к ошибке, так как нет соответствующей инструкции BEGIN TRANSACTION.  
  
 Нельзя произвести откат транзакции после вызова инструкции COMMIT TRANSACTION, так как измененные данные уже стали частью базы данных.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] увеличивает счетчик транзакций внутри выражения, только когда счетчик транзакций равен нулю при запуске инструкции.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-committing-a-transaction"></a>A. Фиксация транзакции  
**ПРИМЕНИМО К:** SQL Server, База данных SQL Azure, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]   

В следующем примере удаляется кандидат на вакансию. В нем используется база данных AdventureWorks. 
  
```sql   
BEGIN TRANSACTION;   
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;   
COMMIT TRANSACTION;   
```  
  
### <a name="b-committing-a-nested-transaction"></a>Б. Фиксация вложенной транзакции  
**ПРИМЕНИМО К:** SQL Server и база данных SQL Azure    

В следующем примере создается таблица и формируется три уровня вложенных транзакций, которые затем фиксируются. Хотя каждая инструкция `COMMIT TRANSACTION` имеет параметр *transaction_name*, связи между инструкциями `COMMIT TRANSACTION` и `BEGIN TRANSACTION` не существует. Параметры *transaction_name* позволяют программисту удостовериться в том, что закодировано правильное количество фиксаций, необходимое для того, чтобы уменьшить значение `@@TRANCOUNT` до 0 и таким образом зафиксировать внешнюю транзакцию. 
  
```sql   
IF OBJECT_ID(N'TestTran',N'U') IS NOT NULL  
    DROP TABLE TestTran;  
GO  
CREATE TABLE TestTran (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
-- This statement sets @@TRANCOUNT to 1.  
BEGIN TRANSACTION OuterTran;  
  
PRINT N'Transaction count after BEGIN OuterTran = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
 
INSERT INTO TestTran VALUES (1, 'aaa');  
 
-- This statement sets @@TRANCOUNT to 2.  
BEGIN TRANSACTION Inner1;  
 
PRINT N'Transaction count after BEGIN Inner1 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
INSERT INTO TestTran VALUES (2, 'bbb');  
  
-- This statement sets @@TRANCOUNT to 3.  
BEGIN TRANSACTION Inner2;  
  
PRINT N'Transaction count after BEGIN Inner2 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
INSERT INTO TestTran VALUES (3, 'ccc');  
  
-- This statement decrements @@TRANCOUNT to 2.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner2;  
 
PRINT N'Transaction count after COMMIT Inner2 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
 
-- This statement decrements @@TRANCOUNT to 1.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner1;  
 
PRINT N'Transaction count after COMMIT Inner1 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
-- This statement decrements @@TRANCOUNT to 0 and  
-- commits outer transaction OuterTran.  
COMMIT TRANSACTION OuterTran;  
  
PRINT N'Transaction count after COMMIT OuterTran = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
```  
  
## <a name="see-also"></a>См. также:  
 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION (Transact-SQL)](../../t-sql/language-elements/save-transaction-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
