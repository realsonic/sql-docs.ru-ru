---
description: TRY...CATCH (Transact-SQL)
title: TRY...CATCH (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BEGIN_TRY_TSQL
- BEGIN_CATCH_TSQL
- TRY
- BEGIN TRY
- TRY_TSQL
- BEGIN CATCH
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CATCH statement
- uncommittable transactions
- errors [SQL Server], TRY...CATCH
- TRY block [SQL Server]
- XACT_STATE function
- TRY...CATCH [SQL Server]
- BEGIN TRY statement
- CATCH block
ms.assetid: 248df62a-7334-4bca-8262-235a28f4b07f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d31f8951dbf878a6ae7b6895fb060af91ad1c27
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096005"
---
# <a name="trycatch-transact-sql"></a>TRY...CATCH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  Реализация обработчика ошибок на языке [!INCLUDE[tsql](../../includes/tsql-md.md)] похожа на обработку исключений в языках [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# и [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++. Группа инструкций на языке [!INCLUDE[tsql](../../includes/tsql-md.md)] может быть заключена в блок TRY. Если ошибка возникает в блоке TRY, управление передается следующей группе инструкций, заключенных в блок CATCH.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
BEGIN TRY  
     { sql_statement | statement_block }  
END TRY  
BEGIN CATCH  
     [ { sql_statement | statement_block } ]  
END CATCH  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *sql_statement*  
 Любая из инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 *statement_block*  
 Любая группа инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)] в пакете или заключенная в блок BEGIN...END.  
  
## <a name="remarks"></a>Remarks  
 Конструкция TRY...CATCH перехватывает все ошибки исполнения с кодом серьезности, большим чем 10, которые не закрывают подключение к базе данных.  
  
 За блоком TRY сразу же должен следовать блок CATCH. Размещение каких-либо инструкций между инструкциями END TRY и BEGIN CATCH вызовет синтаксическую ошибку.  
  
 Конструкция TRY...CATCH не может охватывать несколько пакетов. Конструкция TRY...CATCH не может охватывать множество блоков инструкций на языке [!INCLUDE[tsql](../../includes/tsql-md.md)]. Например: конструктор TRY...CATCH не может охватывать два блока BEGIN...END из инструкций на языке [!INCLUDE[tsql](../../includes/tsql-md.md)] и не может охватывать конструкцию IF...ELSE.  
  
 Если ошибки в блоке TRY не возникают, то после выполнения последней инструкции в блоке TRY управление передается инструкции, расположенной сразу после инструкции END CATCH.
 
 Если же в коде, заключенном в блоке TRY, происходит ошибка, управление передается первой инструкции в соответствующем блоке CATCH. Когда код в блоке CATCH завершен, управление передается инструкции, стоящей сразу после инструкции END CATCH. 
 
 > [!NOTE] 
 > Если инструкция END CATCH является последней инструкцией хранимой процедуры или триггера, управление передается обратно инструкции, вызвавшей эту хранимую процедуру или триггер. 
 
 Ошибки, обнаруженные в блоке CATCH, не передаются в вызывающее приложение. Если какие-либо сведения об ошибке должны быть возвращены в приложение, код в блоке CATCH должен выполнить передачу этой ошибки, используя любые доступные механизмы, такие как результирующие наборы инструкции SELECT либо инструкции RAISERROR и PRINT.  
  
 Конструкция TRY...CATCH может быть вложенной. Либо блок TRY, либо блок CATCH могут содержать вложенные конструкции TRY...CATCH. Например: блок CATCH может содержать внутри себя внедренную TRY...CATCH для управления ошибками, возникающими в коде CATCH.  
  
 Ошибки, обнаруженные в блоке CATCH, обрабатываются так же, как и ошибки, возникшие в любом другом месте. Если блок CATCH содержит внутри себя конструкцию TRY...CATCH, то любая ошибка во вложенном блоке TRY передаст управление во вложенный блок CATCH. Если нет вложенной конструкции TRY...CATCH, то ошибка передается обратно в то место, откуда этот блок с ошибкой был вызван.  
  
 Конструкции TRY...CATCH ловят неуправляемые ошибки из хранимых процедур или триггеров, исполняемых кодом в блоке TRY. Дополнительно хранимые процедуры или триггеры могут содержать свои собственные конструкции TRY...CATCH для обработки ошибок, возникающих в их коде. Например, когда блок TRY выполняет хранимую процедуру и в хранимой процедуре возникла ошибка, то ошибка может быть обработана следующими способами:  
  
-   если хранимая процедура не содержит своей собственной конструкции TRY...CATCH, то ошибка передаст управление в блок CATCH, связанный с блоком TRY, содержащим инструкцию EXECUTE;  
  
-   если хранимая процедура содержит конструкцию TRY...CATCH, то ошибка передаст управление в блок CATCH в хранимой процедуре. Когда блок CATCH завершится, управление перейдет к инструкции, стоящей сразу после инструкции EXECUTE, вызвавшей эту хранимую процедуру.  
  
 Инструкция GOTO не может быть использована для входа в блоки TRY или CATCH. Оператор GOTO может быть использован для перехода к метке внутри блока TRY или CATCH или для выхода из блоков TRY или CATCH.  
  
 Конструкция TRY...CATCH не может использоваться в пользовательских функциях.  
  
## <a name="retrieving-error-information"></a>Получение информации об ошибке  
 В области блока CATCH для получения сведений об ошибке, приведшей к выполнению данного блока CATCH, можно использовать следующие системные функции:  
  
-   функция [ERROR_NUMBER()](../../t-sql/functions/error-number-transact-sql.md) возвращает номер ошибки;  
  
-   функция [ERROR_SEVERITY()](../../t-sql/functions/error-severity-transact-sql.md) возвращает степень серьезности ошибки;  
  
-   функция [ERROR_STATE()](../../t-sql/functions/error-state-transact-sql.md) возвращает код состояния ошибки;  
  
-   функция [ERROR_PROCEDURE()](../../t-sql/functions/error-procedure-transact-sql.md) возвращает имя хранимой процедуры или триггера, в котором произошла ошибка;  
  
-   функция [ERROR_LINE()](../../t-sql/functions/error-line-transact-sql.md) возвращает номер строки, которая вызвала ошибку, внутри подпрограммы;  
  
-   функция [ERROR_MESSAGE()](../../t-sql/functions/error-message-transact-sql.md) возвращает полный текст сообщения об ошибке. Текст содержит значения подставляемых параметров, таких как длина, имена объектов или время.  
  
Эти функции возвращают значение NULL, если их вызов происходит вне области блока CATCH. С помощью этих функций сведения об ошибке могут быть получены из любого места внутри блока CATCH. Например, следующий скрипт демонстрирует хранимую процедуру, которая содержит функции обработки ошибок. В блоке `CATCH` конструкции `TRY...CATCH` вызывается хранимая процедура и возвращаются сведения об ошибке.  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_GetErrorInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
SELECT  
    ERROR_NUMBER() AS ErrorNumber  
    ,ERROR_SEVERITY() AS ErrorSeverity  
    ,ERROR_STATE() AS ErrorState  
    ,ERROR_PROCEDURE() AS ErrorProcedure  
    ,ERROR_LINE() AS ErrorLine  
    ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
BEGIN TRY  
    -- Generate divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
END CATCH;   
```  
  
 Функции ERROR\_\* также работают в блоке `CATCH` внутри [хранимой процедуры, скомпилированной в собственном коде](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
## <a name="errors-unaffected-by-a-trycatch-construct"></a>Ошибки, не обрабатываемые конструкцией TRY...CATCH  
 Конструкции TRY...CATCH не обрабатывают следующие условия.  
  
-   Предупреждения и информационные сообщения с уровнем серьезности 10 или ниже.  
  
-   Ошибки с уровнем серьезности 20 или выше, которые приводят к завершению обработки задачи компонентом [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] для сеанса. Если возникла ошибка с уровнем серьезности 20 или выше, а подключение к базе данных не разорвано, конструкция TRY...CATCH обработает эту ошибку.  
  
-   Такие запросы, как прерывания от клиента или разрыв соединения, вызванный с клиента.  
  
-   Завершение сеанса системным администратором с помощью инструкции KILL.  
  
Следующие типы ошибок не обрабатываются блоком CATCH, если они возникают на том же самом уровне выполнения, что и конструкция TRY...CATCH.  
  
-   Ошибки компиляции, такие как ошибки синтаксиса, в результате которых пакет не будет выполнен.  
  
-   Ошибки, происходящие во время повторной компиляции уровня инструкций, такие как ошибки разрешения имен объектов, которые происходят после компиляции из-за отложенного разрешения имен.  
-   Ошибки разрешения имен объектов   

  
Эти ошибки возвращаются на уровень, на котором запускались пакеты, хранимые процедуры или триггеры.  
  
Если ошибка возникает во время компиляции или перекомпиляции уровня инструкций на нижнем уровне исполнения (например, при выполнении процедуры sp_executesql или определенной пользователем хранимой процедуры) внутри блока TRY, эта ошибка возникнет на уровне, более низком, чем конструкция TRY...CATCH, и будет обрабатываться соответствующим блоком CATCH.  
  
Следующий пример показывает, как ошибка разрешения имени объекта, формируемая инструкцией `SELECT`, не отлавливается конструкцией `TRY...CATCH`, но отлавливается блоком `CATCH`, когда та же самая инструкция `SELECT` выполняется внутри хранимой процедуры.  
  
```sql  
BEGIN TRY  
    -- Table does not exist; object name resolution  
    -- error not caught.  
    SELECT * FROM NonexistentTable;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
       ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH  
```  
  
 Эта ошибка не отлавливается, а управление передается за пределы конструкции `TRY...CATCH` на уровень выше.  
  
 Выполнение инструкции `SELECT` внутри хранимой процедуры приведет к ошибке, которая возникнет на уровне ниже, чем блок `TRY`. Такая ошибка будет обработана конструкцией `TRY...CATCH`.  
  
```sql  
-- Verify that the stored procedure does not exist.  
IF OBJECT_ID ( N'usp_ExampleProc', N'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that will cause an   
-- object resolution error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT * FROM NonexistentTable;  
GO  
  
BEGIN TRY  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
```  
  
## <a name="uncommittable-transactions-and-xact_state"></a>Нефиксируемые транзакции и XACT_STATE  
 Если ошибка, возникшая в блоке TRY, приведет к неправильному состоянию транзакции, то транзакция будет классифицироваться как нефиксированная транзакция. Ошибка, которая обычно останавливает выполнение транзакции за пределами блока TRY, приводит к тому, что транзакция входит в нефиксируемое состояние, когда ошибка возникает внутри блока TRY. Нефиксированные транзакции могут только выполнять операции чтения или ROLLBACK TRANSACTION. Транзакция не может выполнить инструкцию на языке [!INCLUDE[tsql](../../includes/tsql-md.md)], которая будет выполнять операции записи для COMMIT TRANSACTION. Функция XACT_STATE возвращает значение -1, если транзакция была классифицирована как нефиксированная транзакция. Когда выполнение пакета заканчивается, компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)], будет выполнен откат любых активных нефиксируемых транзакций. Если при переходе транзакции в нефиксируемое состояние не было отправлено сообщение об ошибке, после завершения выполнения пакета сообщение об ошибке будет отправлено клиентскому приложению. Это указывает на то, что была обнаружена нефиксируемая транзакция и выполнен ее откат.  
  
 Дополнительные сведения о нефиксированных транзакциях и функции XACT_STATE см. в разделе [XACT_STATE (Transact-SQL)](../../t-sql/functions/xact-state-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-trycatch"></a>A. Использование конструкции TRY...CATCH  
 В следующем примере приведена инструкция `SELECT`, вызывающая ошибку деления на нуль. Эта ошибка приводит к передаче управления связанному блоку `CATCH`.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
### <a name="b-using-trycatch-in-a-transaction"></a>Б. Использование конструкции TRY...CATCH внутри транзакции  
 В следующем примере показано использование блока `TRY...CATCH` внутри транзакции. Инструкция внутри блока `TRY` приводит к ошибке нарушения ограничения.  
  
```sql  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Generate a constraint violation error.  
    DELETE FROM Production.Product  
    WHERE ProductID = 980;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
### <a name="c-using-trycatch-with-xact_state"></a>В. Использование TRY...CATCH с XACT_STATE  
 В следующем примере показано, как использовать конструкцию `TRY...CATCH` для обработки ошибок, возникших внутри транзакции. Функция `XACT_STATE` определяет, должна ли транзакция быть зафиксирована или откачена. В данном примере параметр `SET XACT_ABORT` находится в состоянии `ON`. В результате, если произойдет ошибка нарушения ограничения, транзакция станет нефиксируемой.  
  
```sql  
-- Check to see whether this stored procedure exists.  
IF OBJECT_ID (N'usp_GetErrorInfo', N'P') IS NOT NULL  
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_LINE () AS ErrorLine  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
-- SET XACT_ABORT ON will cause the transaction to be uncommittable  
-- when the constraint violation occurs.   
SET XACT_ABORT ON;  
  
BEGIN TRY  
    BEGIN TRANSACTION;  
        -- A FOREIGN KEY constraint exists on this table. This   
        -- statement will generate a constraint violation error.  
        DELETE FROM Production.Product  
            WHERE ProductID = 980;  
  
    -- If the DELETE statement succeeds, commit the transaction.  
    COMMIT TRANSACTION;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
  
    -- Test XACT_STATE:  
        -- If 1, the transaction is committable.  
        -- If -1, the transaction is uncommittable and should   
        --     be rolled back.  
        -- XACT_STATE = 0 means that there is no transaction and  
        --     a commit or rollback operation would generate an error.  
  
    -- Test whether the transaction is uncommittable.  
    IF (XACT_STATE()) = -1  
    BEGIN  
        PRINT  
            N'The transaction is in an uncommittable state.' +  
            'Rolling back transaction.'  
        ROLLBACK TRANSACTION;  
    END;  
  
    -- Test whether the transaction is committable.
    -- You may want to commit a transaction in a catch block if you want to commit changes to statements that ran prior to the error.
    IF (XACT_STATE()) = 1  
    BEGIN  
        PRINT  
            N'The transaction is committable.' +  
            'Committing transaction.'  
        COMMIT TRANSACTION;     
    END;  
END CATCH;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-trycatch"></a>Г. Использование конструкции TRY...CATCH  
 В следующем примере приведена инструкция `SELECT`, вызывающая ошибку деления на нуль. Эта ошибка приводит к передаче управления связанному блоку `CATCH`.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [THROW (Transact-SQL)](../../t-sql/language-elements/throw-transact-sql.md)   
 [Степени серьезности ошибок ядра СУБД](../../relational-databases/errors-events/database-engine-error-severities.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)   
 [GOTO &#40;Transact-SQL&#41;](../../t-sql/language-elements/goto-transact-sql.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md)   
 [SET XACT_ABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-xact-abort-transact-sql.md)  
  
