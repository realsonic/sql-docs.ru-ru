---
description: SET @local_variable (Transact-SQL)
title: SET @local_variable (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- SET @local_variable
- variables [SQL Server], assigning
- SET statement, @local_variable
- local variables [SQL Server]
ms.assetid: d410e06e-061b-4c25-9973-b2dc9b60bd85
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: caf0fcb2a9b00c8f391280395629348f055d4039
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100965"
---
# <a name="set-local_variable-transact-sql"></a>SET @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Устанавливает указанную локальную переменную, предварительно созданную с помощью инструкции DECLARE @*local_variable*, в указанное значение.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
Синтаксис для SQL Server и Базы данных SQL Azure:

```syntaxsql
SET   
{ @local_variable  
    [ . { property_name | field_name } ] = { expression | udt_name { . | :: } method_name }  
}  
|  
{ @SQLCLR_local_variable.mutator_method  
}  
|  
{ @local_variable  
    {+= | -= | *= | /= | %= | &= | ^= | |= } expression  
}  
|   
  { @cursor_variable =   
    { @cursor_variable | cursor_name   
    | { CURSOR [ FORWARD_ONLY | SCROLL ]   
        [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
        [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
        [ TYPE_WARNING ]   
    FOR select_statement   
        [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]   
      }   
    }  
}   
```  
Синтаксис для Azure Synapse Analytics и Parallel Data Warehouse:  
```syntaxsql
SET @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
**@** _local_variable_  
Имя переменной любого типа, за исключением **cursor**, **text**, **ntext**, **image** или **table**. Имена переменных должны начинаться с одного символа ( **@** ). Имена переменных должны соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
*property_name*  
Свойство определяемого пользователем типа.  
  
*field_name*  
Общее поле определяемого пользователем типа.  
  
*udt_name*  
Имя определяемого пользователем типа среды CLR.  
  
`{ . | :: }`  
Указывает метод пользовательского типа среды CLR. Для метода экземпляра (не статического) используйте точку (**.**). Для статического метода используйте два двоеточия (**::**). Для обращения к методу, свойству или полю определяемого пользователем типа среды CLR необходимо разрешение EXECUTE для этого типа.  
  
_method_name_ **(** _argument_ [ **,** ... *n* ] **)**  
Метод определяемого пользователем типа, который принимает один или несколько аргументов для изменения состояния экземпляра типа. Статические методы должны быть общими.  
  
**@** _SQLCLR_local_variable_  
Переменная, тип которой находится в сборке. Дополнительные сведения см. в статье [Основные понятия о программировании интеграции со средой CLR](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md).  
  
*mutator_method*  
Метод из сборки, который может менять состояние объекта. К этому методу применяется свойство SQLMethodAttribute.IsMutator.  
  
`{ += | -= | *= | /= | %= | &= | ^= | |= }`  
Составной оператор присваивания:  
  
 +=              Сложение и присваивание  
  
 -=              Вычитание и присваивание  
  
 *=              Умножение и присваивание  
  
 /=              Деление и присваивание  
  
 %=              Остаток от деления и присваивание  
  
 &=             Выполнение побитовой операции AND и присваивание  
  
 ^=                Выполнение побитовой операции XOR и присваивание  
  
 |=              Выполнение побитовой операции OR и присваивание  
  
*expression*  
Любое допустимое [выражение](../../t-sql/language-elements/expressions-transact-sql.md).  
  
*cursor_variable*  
Имя переменной курсора. Если переменная целевого курсора ранее ссылалась на другой курсор, эта ссылка будет удалена.  
  
*cursor_name*  
Имя курсора, объявленного при помощи инструкции DECLARE CURSOR.  
  
CURSOR  
Указывает, что инструкция SET содержит декларацию курсора.  
  
SCROLL  
Указывает, что курсор поддерживает все параметры выборки: FIRST, LAST, NEXT, PRIOR, RELATIVE и ABSOLUTE. Аргумент SCROLL нельзя указать вместе с аргументом FAST_FORWARD.  
  
FORWARD_ONLY  
Указывает, что курсор поддерживает только параметр FETCH NEXT. Курсор извлекается в одном направлении с первой до последней строки. Если вы задали аргумент FORWARD_ONLY без ключевых слов STATIC, KEYSET или DYNAMIC, создается курсор типа DYNAMIC. Если вы не указали ни аргумент FORWARD_ONLY, ни аргумент SCROLL, по умолчанию используется аргумент FORWARD_ONLY, если нет ключевых слов STATIC, KEYSET или DYNAMIC. Для курсоров STATIC, KEYSET и DYNAMIC аргумент SCROLL добавляется по умолчанию.  
  
STATIC  
Определяет курсор, который создает временную копию данных для использования курсором. Все запросы к курсору получают ответ от этой временной таблицы в базе данных tempdb. Поэтому изменения, внесенные в базовые таблицы после открытия курсора, не влияют на данные, возвращенные операциями получения, которые выполняются для курсора. Кроме того, этот курсор не поддерживает изменения.  
  
KEYSET  
Указывает, что членство или порядок строк в курсоре неизменны при его открытии. Набор ключей, который уникально определяет строки, встраивается в таблицу keyset в базе данных tempdb. Изменения на неключевые значения в базовых таблицах, либо созданных владельцем курсора, либо зафиксированных другими пользователями, видны при прокрутке владельцем курсора содержимого курсора. Вставки, выполненные другими пользователями, не будут видны. Кроме того, невозможно выполнить вставку через серверный курсор [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Если какая-то строка удаляется, при попытке выбрать эту строку функция @@FETCH_STATUS возвращает значение –2. Обновление значений ключа из-за границ курсора аналогично удалению старой строки с последующей вставкой новой строки. Строка с новыми значениями невидима, а при попы получить строку со старыми значениями для @@FETCH_STATUS возвращается значение –2. Новые значения видимы сразу, если обновление выполнено через курсор с помощью предложения WHERE CURRENT OF.  
  
DYNAMIC  
Определяет курсор, который отражает все изменения данных в строках в его результирующем наборе по мере того, как владелец курсора осуществляет прокрутку по курсору. Значения данных, порядок и членство строк в каждой выборке могут меняться. Динамические курсоры не поддерживают параметры абсолютного и относительного получения.  
  
FAST_FORWARD  
Задает курсор типа FORWARD_ONLY, READ_ONLY с включенной оптимизацией. Нельзя задать аргумент FAST_FORWARD, если задан аргумент SCROLL.  
  
READ_ONLY  
Предотвращает внесение изменений через этот курсор. В предложении WHERE CURRENT OF нельзя помещать ссылку на курсор в инструкцию UPDATE или DELETE. Этот параметр имеет преимущество над установленной по умолчанию возможностью обновления курсора.  
  
SCROLL LOCKS  
Указывает, что позиционированные обновления или удаления, осуществляемые с помощью курсора, гарантированно будут выполнены успешно. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] блокирует строки по мере их считывания в курсор, чтобы обеспечить доступность этих строк для последующих изменений. Нельзя задать аргумент SCROLL_LOCKS, если задан аргумент FAST_FORWARD.  
  
OPTIMISTIC  
Указывает, что позиционированные обновления или удаления, осуществляемые с помощью курсора, не будут выполнены, если с момента считывания в курсор строка была обновлена. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не блокирует строки по мере их считывания в курсор. Вместо этого используются сравнения значений столбца timestamp или значений контрольных сумм, если в таблице нет столбца timestamp, чтобы определить факт изменения строки после ее считывания в курсор. Если строка была изменена, то попытки позиционированного обновления или удаления будут безрезультатными. Нельзя задать аргумент OPTIMISTIC, если задан аргумент FAST_FORWARD.  
  
TYPE_WARNING  
Указывает, что клиенту будет отправлено предупреждение, если курсор неявно будет преобразован из одного запрашиваемого типа в другой.  
  
FOR *select_statement*  
Стандартная инструкция SELECT, которая определяет результирующий набор курсора. Ключевые слова FOR BROWSE и INTO недопустимы в аргументе *select_statement*, входящем в объявление курсора.  
  
Если вы используете ключевые слова DISTINCT, UNION, GROUP BY или HAVING либо в аргумент *select_list* включено статистическое выражение, будет создан курсор STATIC.  
  
Если каждая из базовых таблиц не имеет уникального индекса и курсора ISO SCROLL или запрашивается курсор KEYSET [!INCLUDE[tsql](../../includes/tsql-md.md)], он автоматически будет курсором STATIC.  
  
Если аргумент *select_statement* содержит предложение ORDER BY, в котором столбцы не являются уникальными идентификаторами строк, курсор DYNAMIC будет преобразован в курсор KEYSET или в курсор STATIC, если курсор KEYSET нельзя открыть. То же самое происходит с курсором, определенным при помощи синтаксиса ISO, но без ключевого слова STATIC.  
  
READ ONLY  
Предотвращает внесение изменений через этот курсор. В предложении WHERE CURRENT OF нельзя помещать ссылку на курсор в инструкцию UPDATE или DELETE. Этот параметр имеет преимущество над установленной по умолчанию возможностью обновления курсора. Это ключевое слово отличается от READ_ONLY тем, что между READ и ONLY вместо подчеркивания употребляется пробел.  
  
`UPDATE [OF column_name[ ,... n ] ]`  
Определяет обновляемые столбцы в курсоре. Если OF *column_name* [**,**...*n*] определено, только перечисленные столбцы позволяют вносить изменения. Если список не предоставлен, можно обновить все столбцы, если только курсор не был определен как READ_ONLY.  
  
## <a name="remarks"></a>Комментарии  
После объявления переменной она получает значение NULL. Используйте инструкцию SET, чтобы установить значение, отличное от NULL, для объявленной переменной. Инструкция SET, которая назначает значение переменной, возвращает одно значение. Если инициализируется несколько переменных, следует использовать отдельные инструкции SET для каждой локальной переменной.  
  
Вы можете использовать переменные только в выражениях, но не вместо имен объектов или ключевых слов. Для создания динамических инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] используйте инструкцию EXECUTE.  
  
Синтаксические правила для SET **@** _cursor_variable_ не включают ключевые слова LOCAL и GLOBAL. Если вы используете синтаксис SET **@** _cursor_variable_ = CURSOR..., курсор создается как GLOBAL или LOCAL в зависимости от заданного по умолчанию параметра локального курсора базы данных.  
  
Переменные курсора всегда локальные, даже если ссылаются на глобальный курсор. Если переменная курсора ссылается на глобальный курсор, курсор имеет ссылки как на глобальный, так и на локальный курсоры. Дополнительные сведения см. в примере В.  
  
Дополнительные сведения см. в статье [DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md).  
  
Вы можете использовать составной оператор присваивания во всех случаях, когда выполняется присваивание с выражением справа от оператора, включающим переменные, а также инструкцию SET в инструкциях UPDATE, SELECT и RECEIVE.  
  
Не используйте переменную в инструкции SELECT для сцепления значений (то есть для вычисления статистических значений). Запрос может вернуть непредвиденные результаты. Это связано с тем, что все выражения в списке SELECT (включая назначения) могут быть выполнены несколько раз для каждой строки вывода. Дополнительные сведения см. в [этой статье базы знаний](https://support.microsoft.com/kb/287515).  
  
## <a name="permissions"></a>Разрешения  
Требуется членство в роли public. Все пользователи могут использовать SET **@** _local_variable_.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-printing-the-value-of-a-variable-initialized-by-using-set"></a>A. Отображение значения переменной, инициализированной при помощи инструкции SET  
Следующий пример создает переменную `@myvar`, записывает строковое значение в переменную и печатает значение переменной `@myvar`.  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT @myvar;  
GO  
```  
  
### <a name="b-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>Б. Использование локальной переменной, значение которой назначено при помощи инструкции SET в инструкции SELECT  
В следующем примере создается локальная переменная с именем `@state`. Она используется в инструкции `SELECT` для поиска имен и фамилий всех работников, которые живут в штате `Oregon`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @state CHAR(25);  
SET @state = N'Oregon';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name, City  
FROM HumanResources.vEmployee  
WHERE StateProvinceName = @state;  
```  
  
### <a name="c-using-a-compound-assignment-for-a-local-variable"></a>В. Использование составного оператора присваивания для локальной переменной  
Следующие два примера позволяют получить один и тот же результат. Они создают локальную переменную с именем `@NewBalance`, умножают ее на 10 и отображают новое значение локальной переменной в инструкции `SELECT`. Во втором примере используется составной оператор присваивания.  
  
```sql  
/* Example one */  
DECLARE  @NewBalance  INT ;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT  @NewBalance;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT @NewBalance;  
```  
  
### <a name="d-using-set-with-a-global-cursor"></a>Г. Использование инструкции SET с глобальным курсором  
Следующий пример создает локальную переменную и устанавливает переменную курсора в значение имени глобального курсора.  
  
```sql  
DECLARE my_cursor CURSOR GLOBAL   
FOR SELECT * FROM Purchasing.ShipMethod  
DECLARE @my_variable CURSOR ;  
SET @my_variable = my_cursor ;   
--There is a GLOBAL cursor declared(my_cursor) and a LOCAL variable  
--(@my_variable) set to the my_cursor cursor.  
DEALLOCATE my_cursor;   
--There is now only a LOCAL variable reference  
--(@my_variable) to the my_cursor cursor.  
```  
  
### <a name="e-defining-a-cursor-by-using-set"></a>Д. Определение курсора при помощи инструкции SET  
Следующий пример использует инструкцию `SET` для определения курсора.  
  
```sql  
DECLARE @CursorVar CURSOR;  
  
SET @CursorVar = CURSOR SCROLL DYNAMIC  
FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN @CursorVar;  
  
FETCH NEXT FROM @CursorVar;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM @CursorVar  
END;  
  
CLOSE @CursorVar;  
DEALLOCATE @CursorVar;  
```  
  
### <a name="f-assigning-a-value-from-a-query"></a>Е. Присваивание значения из запроса  
Следующий пример использует запрос для присваивания значения переменной.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM Sales.Customer);  
SELECT @rows;  
```  
  
### <a name="g-assigning-a-value-to-a-user-defined-type-variable-by-modifying-a-property-of-the-type"></a>Ж. Присваивание значения переменной определяемого пользователем типа путем изменения свойства типа  
Следующий пример устанавливает значение для определяемого пользователем типа `Point` путем изменения значения свойства типа `X`.  
  
```sql  
DECLARE @p Point;  
SET @p.X = @p.X + 1.1;  
SELECT @p;  
GO  
```  
  
### <a name="h-assigning-a-value-to-a-user-defined-type-variable-by-invoking-a-method-of-the-type"></a>З. Присваивание значения переменной определяемого пользователем типа путем вызова метода типа  
Следующий пример устанавливает значение определяемого пользователем типа **point** путем вызова метода `SetXY` типа.  
  
```sql  
DECLARE @p Point;  
SET @p=point.SetXY(23.5, 23.5);  
```  
  
### <a name="i-creating-a-variable-for-a-clr-type-and-calling-a-mutator-method"></a>И. Создание переменной для типа CLR и вызов метода мутатора  
В следующем примере создается переменная для типа `Point`, а затем выполняется метод мутатора в `Point`.  
  
```sql  
CREATE ASSEMBLY mytest FROM 'c:\test.dll' WITH PERMISSION_SET = SAFE  
CREATE TYPE Point EXTERNAL NAME mytest.Point  
GO  
DECLARE @p Point = CONVERT(Point, '')  
SET @p.SetXY(22, 23);  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="j-printing-the-value-of-a-variable-initialized-by-using-set"></a>К. Отображение значения переменной, инициализированной при помощи инструкции SET  
Следующий пример создает переменную `@myvar`, записывает строковое значение в переменную и печатает значение переменной `@myvar`.  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT TOP 1 @myvar FROM sys.databases;
```  
  
### <a name="k-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>Л. Использование локальной переменной, значение которой назначено при помощи инструкции SET в инструкции SELECT  
Следующий пример создает локальную переменную с именем `@dept` и использует ее в инструкции `SELECT` для нахождения имен и фамилий всех работников, которые живут в штате `Marketing`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @dept CHAR(25);  
SET @dept = N'Marketing';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name  
FROM DimEmployee   
WHERE DepartmentName = @dept;  
```  
  
### <a name="l-using-a-compound-assignment-for-a-local-variable"></a>М. Использование составного оператора присваивания для локальной переменной  
Следующие два примера позволяют получить один и тот же результат. Они создают локальную переменную с именем `@NewBalance`, умножают ее на `10` и отображают новое значение локальной переменной в инструкции `SELECT`. Во втором примере используется составной оператор присваивания.  
  
```sql  
/* Example one */  
DECLARE  @NewBalance INT;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
```  
  
### <a name="m-assigning-a-value-from-a-query"></a>Н. Присваивание значения из запроса  
Следующий пример использует запрос для присваивания значения переменной.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM dbo.DimCustomer);  
SELECT TOP 1 @rows FROM sys.tables;  
```  
  
## <a name="see-also"></a>См. также  
[Составные операторы (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
[EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

