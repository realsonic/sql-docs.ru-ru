---
description: + (Объединение строк) (Transact-SQL)
title: + (объединение строк) (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- concatenation
- +
- string
dev_langs:
- TSQL
helpviewer_keywords:
- concatenation [SQL Server]
- string concatenation operators
- + (string concatenation)
ms.assetid: 35cb3d7a-48f5-4b13-926c-a9d369e20ed7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 93226b1e477c0ba0c1e5d72380c1c65cccf278eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100991"
---
# <a name="-string-concatenation-transact-sql"></a>+ (объединение строк) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Оператор в строковом выражении, объединяющий две или более символьных или двоичных строки, два или более столбцов или несколько строк и имен столбцов в одно выражение (строковый оператор).  Например, `SELECT 'book'+'case';` возвращает `bookcase`.
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
expression + expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Любое действительное [выражение](../../t-sql/language-elements/expressions-transact-sql.md) любого типа данных в категории символьных и двоичных данных, за исключением типов данных **image**, **ntext** и **text**. Оба выражения должны иметь одинаковый тип данных, или одно из выражений должно допускать неявное преобразование к типу данных другого выражения.  
  
 При сцеплении двоичных строк с любыми символами между двоичными строками необходимо использовать явное преобразование в символьные данные. В следующем примере показано, когда `CONVERT` или `CAST` должны использоваться с двоичной конкатенацией и когда `CONVERT` или `CAST` не нужно использовать.  
  
```sql
DECLARE @mybin1 VARBINARY(5), @mybin2 VARBINARY(5)  
SET @mybin1 = 0xFF  
SET @mybin2 = 0xA5  
-- No CONVERT or CAST function is required because this example   
-- concatenates two binary strings.  
SELECT @mybin1 + @mybin2  
-- A CONVERT or CAST function is required because this example  
-- concatenates two binary strings plus a space.  
SELECT CONVERT(VARCHAR(5), @mybin1) + ' '   
   + CONVERT(VARCHAR(5), @mybin2)  
-- Here is the same conversion using CAST.  
SELECT CAST(@mybin1 AS VARCHAR(5)) + ' '   
   + CAST(@mybin2 AS VARCHAR(5))  
```  
  
## <a name="result-types"></a>Типы результата  
 Возвращает тип данных аргумента с самым высоким приоритетом. Дополнительные сведения см. в разделе [Приоритет типов данных (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Комментарии  
 При работе с пустыми строками нулевой длины оператор + (объединение строк) ведет себя иначе, чем при работе со значениями NULL или с неизвестными значениями. Символьная строка символа нулевой длины может быть указана в виде двух одинарных кавычек без каких-либо символов между ними. Двоичная строка нулевой длины может быть указана как 0x без указания каких-либо байтовых значений в шестнадцатеричной константе. При сцеплении строки нулевой длины всегда сцепляются две указанные строки. При работе со строками со значением NULL результат объединения зависит от настроек сеанса. При присоединении нулевого значения к известному значению результатом будет неизвестное значение, объединение строк с нулевым значением также дает нулевое значение, как и в арифметических действиях с нулевыми значениями. Однако можно изменить данное поведение, поменяв значение `CONCAT_NULL_YIELDS_NULL` для текущего сеанса. Дополнительные сведения см. в разделе [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  
  
 Если результат объединения строк превышает предел в 8 000 байт, то он усекается. Однако усечения не произойдет, если хотя бы одна из сцепляемых строк принадлежит к типу больших значений.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-string-concatenation"></a>A. Использование объединения строк  
 В следующем примере создается единственный столбец с заголовком `Name` из нескольких символьных столбцов, где за фамилией лица следуют запятая, один пробел и имя того же лица. Результирующий набор сортируется в алфавитном порядке по возрастанию, сначала по фамилии, а затем по имени.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ', ' + FirstName) AS Name  
FROM Person.Person  
ORDER BY LastName ASC, FirstName ASC;  
```  
  
### <a name="b-combining-numeric-and-date-data-types"></a>Б. Объединение числовых типов данных и дат  
 В приведенном ниже примере функция `CONVERT` используется для объединения типов данных **numeric** и **date**.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT 'The order is due on ' + CONVERT(VARCHAR(12), DueDate, 101)  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderID = 50001;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ------------------------------------------------  
 The order is due on 04/23/2007  
 (1 row(s) affected)
 ```  
  
### <a name="c-using-multiple-string-concatenation"></a>В. Использование объединения нескольких строк  
 Следующий пример сцепляет несколько строк в одну длинную строку для отображения фамилии и первой буквы инициалов вице-президентов в [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]. После фамилии ставится запятая, а после первой буквы инициалов — точка.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ',' + SPACE(1) + SUBSTRING(FirstName, 1, 1) + '.') AS Name, e.JobTitle  
FROM Person.Person AS p  
    JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle LIKE 'Vice%'  
ORDER BY LastName ASC;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Name               Title  
 -------------      ---------------`  
 Duffy, T.          Vice President of Engineering  
 Hamilton, J.       Vice President of Production  
 Welcker, B.        Vice President of Sales  

 (3 row(s) affected)
 ```  
 
### <a name="d-using-large-strings-in-concatenation"></a>Г. Использование больших строк при объединении
В приведенном ниже примере выполняется объединение нескольких строк в одну длинную строку, а затем предпринимается попытка вычислить длину итоговой строки. Итоговая длина результирующего набора равна 16 000, так как вычисление выражения начинается слева, то есть @x + @z + @y => (@x + @z) + @y. В этом случае результат (@x + @z) усекается до 8000 байт, а затем в результирующий набор добавляется значение @y, из-за чего длина итоговой строки становится равна 16 000. Так как @y — это строка типа с большим значением, усечения не происходит.

```sql
DECLARE @x VARCHAR(8000) = REPLICATE('x', 8000)
DECLARE @y VARCHAR(max) = REPLICATE('y', 8000)
DECLARE @z VARCHAR(8000) = REPLICATE('z',8000)
SET @y = @x + @z + @y
-- The result of following select is 16000
SELECT LEN(@y) AS y
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 y        
 -------  
 16000  
  
 (1 row(s) affected)
 ```  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-multiple-string-concatenation"></a>Д. Использование объединения нескольких строк  
 В приведенном ниже примере несколько строк сцепляются в одну длинную строку для отображения фамилий и инициалов имен вице-президентов из образца базы данных. После фамилии ставится запятая, а после первой буквы инициалов — точка.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ', ' + SUBSTRING(FirstName, 1, 1) + '.') AS Name, Title  
FROM DimEmployee  
WHERE Title LIKE '%Vice Pres%'  
ORDER BY LastName ASC;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Name               Title                                           
-------------      ---------------  
Duffy, T.          Vice President of Engineering  
Hamilton, J.       Vice President of Production  
Welcker, B.        Vice President of Sales  
```  
  
## <a name="see-also"></a>См. также  
 [+= (присваивание объединения строк) (Transact-SQL)](../../t-sql/language-elements/string-concatenation-equal-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
  
  



