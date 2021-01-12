---
description: ROLLBACK TRANSACTION (Transact-SQL)
title: ROLLBACK TRANSACTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ROLLBACK TRANSACTION
- ROLLBACK
- ROLLBACK_TSQL
- ROLLBACK_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- ROLLBACK TRANSACTION statement
- erasing data modifications [SQL Server]
- rolling back transactions, ROLLBACK TRANSACTION
- roll back transactions [SQL Server]
- savepoints [SQL Server]
ms.assetid: 6882c5bc-ff74-476a-984b-164aeb036c66
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c785699c469ca1337eaa1a6b27fbab9aed149a5e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097053"
---
# <a name="rollback-transaction-transact-sql"></a>ROLLBACK TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Откатывает явные или неявные транзакции до начала или до точки сохранения транзакции. ROLLBACK TRANSACTION можно использовать для отмены всех изменений данных, произведенных с начала транзакции или до точки сохранения. Она также освобождает ресурсы, используемые транзакцией.  
  
  Сюда не входят изменения, внесенные в локальные или табличные переменные. Они не удаляются с помощью этой инструкции.
  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ROLLBACK { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable  
     | savepoint_name | @savepoint_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *transaction_name*  
 Имя, присвоенное транзакции в BEGIN TRANSACTION. Аргумент *transaction_name* должен соответствовать правилам для идентификаторов, однако используются только первые 32 символа имени транзакции. При вложении транзакций аргумент *transaction_name* должен быть именем транзакции из самой внешней инструкции BEGIN TRANSACTION. Аргумент *transaction_name* всегда учитывает регистр, даже если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] регистр не учитывает.  
  
 **@** *tran_name_variable*  
 Имя определенной пользователем переменной, содержащей допустимое имя транзакции. Переменная должна быть объявлена с типом данных **char**, **varchar**, **nchar** или **nvarchar**.  
  
 *savepoint_name*  
 Аргумент *savepoint_name* из инструкции SAVE TRANSACTION. Аргумент *savepoint_name* должен соответствовать требованиям, предъявляемым к идентификаторам. Используйте аргумент *savepoint_name*, если откат по условию должен влиять только на часть транзакции.  
  
 **@** *savepoint_variable*  
 Имя пользовательской переменной, содержащей допустимое имя точки сохранения. Переменная должна быть объявлена с типом данных **char**, **varchar**, **nchar** или **nvarchar**.  
  
## <a name="error-handling"></a>Обработка ошибок  
 Инструкция ROLLBACK TRANSACTION не выдает никаких сообщений пользователю. Если нужны предупреждения в хранимой процедуре или триггере, используйте инструкции RAISERROR или PRINT. Инструкция RAISERROR предпочтительна для отображения ошибок.  
  
## <a name="general-remarks"></a>Общие замечания  
 Инструкция ROLLBACK TRANSACTION без аргумента *savepoint_name* или *transaction_name* откатывает изменения на начало транзакции. При наличии вложенных транзакций эта инструкция откатывает все внутренние транзакции к началу самой внешней инструкции BEGIN TRANSACTION. В обоих случаях инструкция ROLLBACK TRANSACTION уменьшает системную функцию @@TRANCOUNT до 0. Инструкция ROLLBACK TRANSACTION *savepoint_name* не уменьшает @@TRANCOUNT.  
  
 Инструкция ROLLBACK TRANSACTION не может ссылаться на аргумент *savepoint_name* в распределенных транзакциях, запущенных явно с помощью инструкции BEGIN DISTRIBUTED TRANSACTION или вызванных из локальной транзакции.  
  
 Нельзя выполнить откат транзакции после выполнения инструкции COMMIT TRANSACTION, кроме случая, когда инструкция COMMIT TRANSACTION связана с вложенной транзакцией, которая содержится внутри откатываемой транзакции. В этом случае будет выполнен откат вложенной транзакции, даже если для нее была выполнена инструкция COMMIT TRANSACTION.  
  
 Внутри транзакции допускается использование повторяющихся имен точки сохранения, но инструкция ROLLBACK TRANSACTION, использующая повторяющееся имя точки сохранения, откатывает транзакцию лишь к самой последней точке, установленной с помощью инструкции SAVE TRANSACTION для этого имени.  
  
## <a name="interoperability"></a>Совместимость  
 В хранимых процедурах инструкция ROLLBACK TRANSACTION без аргументов *savepoint_name* или *transaction_name* откатывает все инструкции к самой внешней инструкции BEGIN TRANSACTION. Вызов инструкции ROLLBACK TRANSACTION в хранимой процедуре является причиной того, что значение @@TRANCOUNT после завершения хранимой процедуры отличается от значения @@TRANCOUNT при выдаче хранимой процедурой информационного сообщения. Это сообщение не влияет на последующую обработку.  
  
 Если инструкция ROLLBACK TRANSACTION запускается в триггере, происходит следующее:  
  
-   Все изменения данных, сделанные к настоящему времени в текущей базе данных, откатываются, включая изменения, сделанные триггером.  
  
-   Триггер продолжает выполнять все оставшиеся инструкции после инструкции ROLLBACK. Если какая-нибудь из инструкций изменит данные, откат этих изменений выполнен не будет. Вложенные триггеры не выполняются при выполнении оставшихся инструкций.  
  
-   Инструкции в пакете, следующие за инструкцией, вызвавшей срабатывание триггера, не выполняются.  
  
Значение @@TRANCOUNT увеличивается на единицу при срабатывании триггера даже в режиме автоматической фиксации. (Система обрабатывает триггер как неявную вложенную транзакцию.)  
  
Инструкция ROLLBACK TRANSACTION в хранимой процедуре не влияет на последующие инструкции в пакете, вызвавшем процедуру; последующие инструкции в пакете выполняются. Инструкции ROLLBACK TRANSACTION в триггерах уничтожают пакет, содержащий инструкцию, вызвавшую триггер; последующие инструкции в пакете не выполняются.  
  
Эффект, оказываемый инструкцией ROLLBACK на курсоры, определяется тремя правилами:  
  
1.  Если параметр CURSOR_CLOSE_ON_COMMIT установлен в ON, инструкция ROLLBACK закрывает, но не освобождает все открытые курсоры.  
  
2.  Если параметр CURSOR_CLOSE_ON_COMMIT установлен в OFF, инструкция ROLLBACK не влияет на открытые синхронные курсоры типа STATIC или INSENSITIVE или асинхронные курсоры типа STATIC, которые были полностью заполнены. Открытые курсоры любого другого типа закрываются, но не освобождаются.  
  
3.  Ошибка, которая уничтожает пакет и формирует внутренний откат, освобождает все курсоры, которые были объявлены в пакете, содержащем ошибочную инструкцию. Все курсоры освобождаются в зависимости от их типа или установок параметра CURSOR_CLOSE_ON_COMMIT. Это относится и к курсорам, объявленным в хранимых процедурах, вызываемых ошибочным пакетом. Курсоры, объявленные в пакете перед ошибочным, подчиняются правилам 1 и 2. Ошибка взаимоблокировки является примером ошибки такого типа. Инструкция ROLLBACK в триггере также автоматически формирует этот тип ошибки.  
  
## <a name="locking-behavior"></a>Режим блокировки  
 Инструкция ROLLBACK TRANSACTION с параметром *savepoint_name* освобождает все блокировки, полученные после точки сохранения, за исключением укрупненных блокировок и блокировок преобразования. Такие блокировки не освобождаются и не переводятся в прежний режим.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере демонстрируется эффект отката именованной транзакции: После создания таблицы следующие инструкции запускают именованную транзакцию, вставляют две строки и откатывают транзакцию, именованную в переменной @TransactionName. Другой оператор вне именованной транзакции вставляет две строки. Запрос возвращает результаты предыдущих инструкций.   
  
```sql    
USE tempdb;  
GO  
CREATE TABLE ValueTable ([value] INT);  
GO  
  
DECLARE @TransactionName VARCHAR(20) = 'Transaction1';  
  
BEGIN TRAN @TransactionName  
       INSERT INTO ValueTable VALUES(1), (2);  
ROLLBACK TRAN @TransactionName;  
  
INSERT INTO ValueTable VALUES(3),(4);  
  
SELECT [value] FROM ValueTable;  
  
DROP TABLE ValueTable;  
```  
[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]  
```  
value  
-----   
3    
4  
```  
  
  
## <a name="see-also"></a>См. также:  
 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION (Transact-SQL)](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
