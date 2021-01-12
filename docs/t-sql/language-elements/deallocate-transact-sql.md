---
description: DEALLOCATE (Transact-SQL)
title: DEALLOCATE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DEALLOCATE
- DEALLOCATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], cursors
- DEALLOCATE statement
- deallocations [SQL Server]
- deleting cursor references
- removing cursor references
ms.assetid: c75cf73d-0268-4c57-973d-b8a84ff801fa
author: cawrites
ms.author: chadam
ms.openlocfilehash: af7031fbcfaf4efacbe4f3692547635030049c64
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093531"
---
# <a name="deallocate-transact-sql"></a>DEALLOCATE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Удаляет ссылку курсора. Когда удаляется последняя ссылка курсора, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] освобождает структуры данных, составляющие курсор.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DEALLOCATE { { [ GLOBAL ] cursor_name } | @cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *cursor_name*  
 Имя объявленного курсора. Когда имеется как глобальный, так и локальный курсор с именем *cursor_name*, то *cursor_name* ссылается на глобальный курсор, если задано `GLOBAL`, и на локальный, если `GLOBAL` не задано.  
  
 @*cursor_variable_name*  
 Имя переменной **курсора**.Переменная  @*cursor_variable_name* должна иметь тип **cursor**.  
  
## <a name="remarks"></a>Комментарии  
Инструкции, обрабатывающие курсоры, используют для ссылки имя курсора или имя переменной курсора. Инструкция `DEALLOCATE` удаляет связь между курсором и его именем или переменной. Если это последнее имя или переменная, ссылающаяся на курсор, сам курсор удаляется и освобождаются все используемые им ресурсы. `DEALLOCATE` освобождает все блокировки прокрутки, которые используются для защиты изоляции выборки. Блокировки транзакций, которые используются для защиты обновлений, включая позиционные обновления в курсоре, удерживаются до завершения транзакции.  
  
Инструкция `DECLARE CURSOR` присваивает курсору имя и связывает его с этим именем.  
  
```sql  
DECLARE abc SCROLL CURSOR FOR  
SELECT * FROM Person.Person;  
```  
  
После того как имя курсора связано с курсором, это имя нельзя использовать для другого курсора той же области действия (глобальной или локальной), пока этот курсор не будет освобожден.  
  
 Переменная курсора связывается с курсором двумя способами.  
  
-   С помощью имени, используя инструкцию `SET`, которая устанавливает курсору переменную курсора.  
  
    ```sql  
    DECLARE @MyCrsrRef CURSOR;  
    SET @MyCrsrRef = abc;  
    ```  
  
-   Курсор можно также создать и связать с переменной, не определяя имя курсора.  
  
    ```sql  
    DECLARE @MyCursor CURSOR;  
    SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Person.Person;  
    ```  
  
 Инструкция `DEALLOCATE <@cursor_variable_name>` удаляет только ссылку именованной переменной на курсор. Эта переменная не освобождается, пока не выходит за область действия в конце пакета, хранимой процедуры или триггера. После выполнения инструкции `DEALLOCATE <@cursor_variable_name>` эту переменную можно связать с другим курсором, используя инструкцию SET.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
  
DEALLOCATE @MyCursor;  
  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesTerritory;  
GO  
```  
  
Переменная курсора не обязательно освобождается явно. Эта переменная освобождается неявно, если выходит за область действия.  
  
## <a name="permissions"></a>Разрешения  
 Разрешения `DEALLOCATE` по умолчанию предоставляются всем допустимым пользователям.  
  
## <a name="examples"></a>Примеры  
 Следующий скрипт показывает, что курсор сохраняется, пока не освобождается его последнее имя или ссылка на него.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create and open a global named cursor that  
-- is visible outside the batch.  
DECLARE abc CURSOR GLOBAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
OPEN abc;  
GO  
-- Reference the named cursor with a cursor variable.  
DECLARE @MyCrsrRef1 CURSOR;  
SET @MyCrsrRef1 = abc;  
-- Now deallocate the cursor reference.  
DEALLOCATE @MyCrsrRef1;  
-- Cursor abc still exists.  
FETCH NEXT FROM abc;  
GO  
-- Reference the named cursor again.  
DECLARE @MyCrsrRef2 CURSOR;  
SET @MyCrsrRef2 = abc;  
-- Now deallocate cursor name abc.  
DEALLOCATE abc;  
-- Cursor still exists, referenced by @MyCrsrRef2.  
FETCH NEXT FROM @MyCrsrRef2;  
-- Cursor finally is deallocated when last referencing  
-- variable goes out of scope at the end of the batch.  
GO  
-- Create an unnamed cursor.  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
SELECT * FROM Sales.SalesTerritory;  
-- The following statement deallocates the cursor  
-- because no other variables reference it.  
DEALLOCATE @MyCursor;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [Курсоры](../../relational-databases/cursors.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN (Transact-SQL)](../../t-sql/language-elements/open-transact-sql.md)  
  
  
