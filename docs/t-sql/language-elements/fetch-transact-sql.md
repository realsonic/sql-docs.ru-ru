---
description: FETCH (Transact-SQL)
title: FETCH (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FETCH
- FETCH_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FETCH statement
- cursors [SQL Server], fetching
- Transact-SQL cursors, fetching and scrolling
- retrieving rows
- fetching [SQL Server]
- SCROLL option
- row fetching [SQL Server]
ms.assetid: 5d68dac2-f91b-4342-bb4e-209ee132665f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 43ce2fb10cf07ec766d29b4b3e96e63404c0fbb1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100367"
---
# <a name="fetch-transact-sql"></a>FETCH (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Получает определенную строку из серверного курсора [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
FETCH   
          [ [ NEXT | PRIOR | FIRST | LAST   
                    | ABSOLUTE { n | @nvar }   
                    | RELATIVE { n | @nvar }   
               ]   
               FROM   
          ]   
{ { [ GLOBAL ] cursor_name } | @cursor_variable_name }   
[ INTO @variable_name [ ,...n ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 NEXT  
 Возвращает строку результата сразу же за текущей строкой и перемещает указатель текущей строки на возвращенную строку. Если инструкция `FETCH NEXT` выполняет первую выборку в отношении курсора, она возвращает первую строку в результирующем наборе. `NEXT` является параметром по умолчанию выборки из курсора.  
  
 PRIOR  
 Возвращает строку результата, находящуюся непосредственно перед текущей строкой и перемещает указатель текущей строки на возвращенную строку. Если инструкция `FETCH PRIOR` выполняет первую выборку из курсора, не возвращается никакая строка и положение курсора остается перед первой строкой.  
  
 FIRST  
 Возвращает первую строку в курсоре и делает ее текущей.  
  
 LAST  
 Возвращает последнюю строку в курсоре, и делает ее текущей.  
  
 ABSOLUTE { *n*| \@*nvar*}  
 Если *n* или \@*nvar* является положительным, возвращает строку *n* строк из начала курсора и делает возвращенную строку новой текущей строкой. Если *n* или \@*nvar* является отрицательным, возвращает строку *n* строк в конце курсора и делает возвращенную строку новой текущей строкой. Если *n* или \@*nvar* равно 0, строки не возвращаются. *n* должен быть целочисленной константой, а \@*nvar* должен иметь тип данных **smallint**, **tinyint** или **int**.  
  
 RELATIVE { *n*| \@*nvar*}  
 Если *n* или \@*nvar* является положительным, возвращает строку *n* строк после текущей строки и делает возвращенную строку новой текущей строкой. Если *n* или \@*nvar* является отрицательным, возвращает строку *n* строк перед текущей строкой и делает возвращенную строку новой текущей строкой. Если *n* или \@*nvar* равно 0, возвращает текущую строку. Если `FETCH RELATIVE` указано с *n* или \@*nvar*, заданными с отрицательными числами или 0 в первой выборке, выполненной с курсором, строки не возвращаются. *n* должен быть целочисленной константой, а \@*nvar* должен иметь тип данных **smallint**, **tinyint** или **int**.  
  
 GLOBAL  
 Указывает, что аргумент *cursor_name* ссылается на глобальный курсор.  
  
 *cursor_name*  
 Имя открытого курсора, из которого должна быть произведена выборка. Когда имеется как глобальный, так и локальный курсор с именем *cursor_name*, то *cursor_name* ссылается на глобальный курсор, если задано GLOBAL, и на локальный, если GLOBAL не задано.  
  
 \@*cursor_variable_name*  
 Имя переменной курсора, ссылающейся на открытый курсор, из которого должна быть произведена выборка.  
  
 INTO \@*variable_name*[ ,...*n*]  
 Позволяет поместить данные из столбцов выборки в локальные переменные. Каждая переменная из списка, слева направо, связывается с соответствующим столбцом в результирующем наборе курсора. Тип данных каждой переменной должен соответствовать типу данных соответствующего столбца результирующего набора, или должна обеспечиваться поддержка неявного преобразования в тип данных этого столбца. Количество переменных должно совпадать с количеством столбцов в списке выбора курсора.  
  
## <a name="remarks"></a>Комментарии  
 Если параметр `SCROLL` не указан в инструкции `DECLARE CURSOR` в стиле ISO, `NEXT` является единственным поддерживаемым параметром `FETCH`. Если `SCROLL` указан в стиле ISO `DECLARE CURSOR`, поддерживаются все параметры `FETCH`.  
  
 При использовании расширений курсора [!INCLUDE[tsql](../../includes/tsql-md.md)] DECLARE применимы следующие правила.  
  
-   Если указан параметр `FORWARD_ONLY` или `FAST_FORWARD`, `NEXT` является единственным поддерживаемым параметром `FETCH`.  
  
-   Если `DYNAMIC`, `FORWARD_ONLY` или `FAST_FORWARD` не указаны и указан один из параметров `KEYSET`, `STATIC` и `SCROLL`, поддерживаются все параметры `FETCH`.  
  
-   Курсоры `DYNAMIC SCROLL` поддерживают все параметры `FETCH`, кроме `ABSOLUTE`.  
  
 Функция `@@FETCH_STATUS` возвращает состояние последней инструкции `FETCH`. Те же данные записываются в столбец fetch_status в курсоре, возвращаемом процедурой sp_describe_cursor. Эти сведения о состоянии должны использоваться для определения действительности данных, возвращаемых инструкцией `FETCH` перед попыткой выполнения любой операции с этими данными. Дополнительные сведения см. в статье [@@FETCH_STATUS (Transact-SQL)](../../t-sql/functions/fetch-status-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Разрешения `FETCH` по умолчанию предоставляются всем допустимым пользователям.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-fetch-in-a-simple-cursor"></a>A. Использование инструкции FETCH в простом курсоре  
 В следующем примере объявляется простой курсор для строк в таблице `Person.Person` с фамилией, начинающийся с `B`, и используется `FETCH NEXT` для пошагового выполнения строк. Инструкции `FETCH` возвращают значение для столбца, указанного в инструкции `DECLARE CURSOR` в качестве однострочного результирующего набора.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch.  
FETCH NEXT FROM contact_cursor;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="b-using-fetch-to-store-values-in-variables"></a>Б. Использование инструкции FETCH для сохранения значений в переменных  
 Следующий пример аналогичен примеру A, за исключением того, что выход инструкций `FETCH` сохраняется в локальных переменных, а не возвращается непосредственно клиенту. Инструкция `PRINT` объединяет переменные в одну строку и возвращает их клиенту.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Declare the variables to store the values returned by FETCH.  
DECLARE @LastName VARCHAR(50), @FirstName VARCHAR(50);  
  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch and store the values in variables.  
-- Note: The variables are in the same order as the columns  
-- in the SELECT statement.   
  
FETCH NEXT FROM contact_cursor  
INTO @LastName, @FirstName;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
  
   -- Concatenate and display the current values in the variables.  
   PRINT 'Contact Name: ' + @FirstName + ' ' +  @LastName  
  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor  
   INTO @LastName, @FirstName;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="c-declaring-a-scroll-cursor-and-using-the-other-fetch-options"></a>В. Объявление курсора SCROLL и использование других параметров инструкции FETCH  
 В следующем примере создается курсор `SCROLL`, с помощью которого можно получить полные возможности прокрутки с помощью параметров `LAST`, `PRIOR`, `RELATIVE` и `ABSOLUTE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Execute the SELECT statement alone to show the   
-- full result set that is used by the cursor.  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
-- Declare the cursor.  
DECLARE contact_cursor SCROLL CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Fetch the last row in the cursor.  
FETCH LAST FROM contact_cursor;  
  
-- Fetch the row immediately prior to the current row in the cursor.  
FETCH PRIOR FROM contact_cursor;  
  
-- Fetch the second row in the cursor.  
FETCH ABSOLUTE 2 FROM contact_cursor;  
  
-- Fetch the row that is three rows after the current row.  
FETCH RELATIVE 3 FROM contact_cursor;  
  
-- Fetch the row that is two rows prior to the current row.  
FETCH RELATIVE -2 FROM contact_cursor;  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [DEALLOCATE (Transact-SQL)](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [OPEN (Transact-SQL)](../../t-sql/language-elements/open-transact-sql.md)  
  
  
