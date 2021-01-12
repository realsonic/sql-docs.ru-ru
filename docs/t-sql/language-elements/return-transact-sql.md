---
description: RETURN (Transact-SQL)
title: RETURN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RETURN_TSQL
- RETURN
dev_langs:
- TSQL
helpviewer_keywords:
- unconditionally exiting program
- stored procedures [SQL Server], exiting
- batches [SQL Server], exiting
- statement blocks [SQL Server]
- queries [SQL Server], exiting
- exiting queries [SQL Server]
- exiting procedures [SQL Server]
- RETURN statement
ms.assetid: 1d9c8247-fd89-4544-be9c-01c95b745db0
author: cawrites
ms.author: chadam
ms.openlocfilehash: b015a7e35987e20021388f9270b6f0588e8e58cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102429"
---
# <a name="return-transact-sql"></a>RETURN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Служит для безусловного выхода из запроса или процедуры. Инструкция RETURN выполняется немедленно и полностью и может использоваться в любой точке для выхода из процедуры, пакета или блока инструкций. Инструкции, следующие после RETURN, не выполняются.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
RETURN [ integer_expression ]   
```  
  
## <a name="arguments"></a>Аргументы  
 *integer_expression*  
 Возвращаемое целочисленное значение. Хранимые процедуры могут возвращать целочисленное значение вызывающей их процедуре или приложению.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых значений
 Может возвращать тип **int**.  
  
> [!NOTE]  
>  Если в документации ничего не указано, все хранимые системные процедуры возвращают значение 0. Это указывает на успех, а ненулевое значение — на ошибку.  
  
## <a name="remarks"></a>Примечания  
 При использовании в хранимой процедуре инструкция RETURN не может возвращать значение NULL. Если процедура пытается вернуть значение NULL (например, с помощью инструкции RETURN @status, если @status равен NULL), формируется предупредительное сообщение и возвращается значение 0.  
  
 Возвращаемое значение состояния может быть включено в последующие инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] пакета или процедуры, выполняющей текущую процедуру, но должно вводиться в следующем формате: `EXECUTE @return_status = <procedure_name>`.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-from-a-procedure"></a>A. Возвращение из процедуры  
 В следующем примере показано, что, если при выполнении процедуры `findjobs` не задан параметр имени пользователя, инструкция `RETURN` заставляет процедуру завершиться после отправки сообщения на пользовательский экран. Если имя пользователя задано, имена всех объектов, созданных пользователем в текущей базе данных, извлекаются из соответствующих системных таблиц.  
  
```sql  
CREATE PROCEDURE findjobs @nm sysname = NULL  
AS   
IF @nm IS NULL  
    BEGIN  
        PRINT 'You must give a user name'  
        RETURN  
    END  
ELSE  
    BEGIN  
        SELECT o.name, o.id, o.uid  
        FROM sysobjects o INNER JOIN master..syslogins l  
            ON o.uid = l.sid  
        WHERE l.name = @nm  
    END;  
```  
  
### <a name="b-returning-status-codes"></a>Б. Возвращение кодов состояния  
 В следующем примере проверяется состояние идентификатора заданного контакта. Если значением штата является Washington (`WA`), то возвращается состояние `1`. Иначе возвращается `2` для какого-либо другого условия (значение `WA`, отличное от `StateProvince`, или `ContactID`, для которого нет соответствующей строки).  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE checkstate @param VARCHAR(11)  
AS  
IF (SELECT StateProvince FROM Person.vAdditionalContactInfo WHERE ContactID = @param) = 'WA'  
    RETURN 1  
ELSE  
    RETURN 2;  
GO  
```  
  
 В следующих примерах отображаются состояния, возвращаемые после выполнения процедуры `checkstate`. В первом показывается контакт в штате Вашингтон, во втором — контакты вне штата Вашингтон, а в третьем — недопустимый контакт. Локальная переменная `@return_status` должна быть объявлена, прежде чем ее можно будет использовать.  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '2';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status 
  
 ------------- 
  
 1
 ```  
  
 Выполните запрос повторно, указав другой номер контакта.  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '6';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
 Выполните запрос повторно, указав еще один номер контакта.  
  
```sql  
DECLARE @return_status INT  
EXEC @return_status = checkstate '12345678901';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
## <a name="see-also"></a>См. также:  
 [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)   
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)  
  
  
