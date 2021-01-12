---
title: CHARINDEX (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для функции CHARINDEX.
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHARINDEX
- CHARINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], pattern searching
- CHARINDEX function
- pattern searching [SQL Server]
- starting point of expression in character string
ms.assetid: 78c10341-8373-4b30-b404-3db20e1a3ac4
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1914ae25b1b8f86e66c1f85b4789e7ded66c0f02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093776"
---
# <a name="charindex-transact-sql"></a>CHARINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция выполняет поиск одного символьного выражения внутри второго символьного выражения, возвращая начальную позицию первого выражения, если найдено.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*expressionToFind*  
Символьное [выражение](../../t-sql/language-elements/expressions-transact-sql.md), содержащее последовательность для поиска. *expressionToFind* имеет ограничение 8000 символов.
  
*expressionToSearch*  
Символьное выражение, в котором производится поиск.
  
*start_location*  
Выражение типа **integer** или **bigint**, с которого начинается поиск. Если аргумент *start_location* не указан, имеет отрицательное значение или равен нулю (0), то поиск начинается с начала выражения *expressionToSearch*.
  
## <a name="return-types"></a>Типы возвращаемых данных
**bigint**, если *expressionToSearch* имеет тип данных **nvarchar(max)** , **varbinary(max)** или **varchar(max)** ; в противном случае **int**.
  
## <a name="remarks"></a>Remarks  
Если выражение *expressionToFind* или  *expressionToSearch* имеет тип данных Юникода (**nchar** или **nvarchar**), а второе выражение — нет, функция CHARINDEX преобразует такое другое выражение в тип данных Юникода. Функция CHARINDEX не поддерживает типы данных **image**, **ntext** и **text**.
  
Если выражение *expressionToFind* или *expressionToSearch* имеет значение NULL, то CHARINDEX возвращает значение NULL.
  
Если функции CHARINDEXне удается найти *expressionToFind* в *expressionToSearch*, она возвращает 0.
  
Функция CHARINDEX выполняет сравнения на основе параметров сортировки входных данных. Для выполнения сравнения в указанных параметрах сортировки используйте функцию COLLATE, чтобы явно указать параметры сортировки для входных данных.
  
Начальная возвращенная позиция начинается с 1, а не с 0.
  
Символ 0x0000 (**char(0)** ) не определен в параметрах сортировки Windows, и его нельзя включать в CHARINDEX.
  
## <a name="supplementary-characters-surrogate-pairs"></a>Дополнительные символы (суррогатные пары)  
Если используются параметры сортировки SC, и в *start_location*, и в возвращаемом значении суррогатные пары учитываются как один символ, а не как два. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-the-starting-position-of-an-expression"></a>A. Возвращение начальной позиции выражения  
В этом примере выполняется поиск `bicycle` в переменной строкового значения `@document`.
  
```sql
DECLARE @document VARCHAR(64);  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bicycle', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
48            
```  
  
### <a name="b-searching-from-a-specific-position"></a>Б. Поиск с конкретной позиции  
В этом примере используется необязательный параметр *start_location* для запуска поиска `vital` в пятом столбце переменной строкового значения `@document`.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('vital', @document, 5);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
16            
  
(1 row(s) affected)  
```  
  
### <a name="c-searching-for-a-nonexistent-expression"></a>В. Поиск несуществующего выражения  
В этом примере показан результирующий набор, который выводится если функции CHARINDEX не удается найти *expressionToFind* в *expressionToSearch*.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bike', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
  
(1 row(s) affected)
```
  
### <a name="d-performing-a-case-sensitive-search"></a>Г. Выполнение поиска с учетом регистра  
В этом примере выполняется поиск строки `'TEST'` в строке `'This is a Test``'` с учетом регистра.
  
```sql
USE tempdb;  
GO  
--perform a case sensitive search  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
```  
  
В этом примере выполняется поиск строки `'Test'` в `'This is a Test'` с учетом регистра.
  
```sql
  
USE tempdb;  
GO  
SELECT CHARINDEX ( 'Test',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
### <a name="e-performing-a-case-insensitive-search"></a>Д. Выполнение поиска без учета регистра  
В этом примере выполняется поиск строки `'TEST'` в `'This is a Test'` без учета регистра.
  
```sql
USE tempdb;  
GO  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CI_AS);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-searching-from-the-start-of-a-string-expression"></a>Е. Поиск с начала строкового выражения  
В этом примере возвращается первая позиция строки `is` в строке `This is a string`, начиная с позиции 1 (первого символа) в строке `This is a string`.
  
```sql
SELECT CHARINDEX('is', 'This is a string');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
3
```  
  
### <a name="g-searching-from-a-position-other-than-the-first-position"></a>Ж. Поиск с позиции, отличной от первой  
В этом примере возвращается первая позиция строки `is` в строке `This is a string`, начиная с позиции 4 (четвертого символа).
  
```sql
SELECT CHARINDEX('is', 'This is a string', 4);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
 6
 ```  
  
### <a name="h-results-when-the-string-is-not-found"></a>З. Результаты в случае, если строка не найдена  
В этом примере демонстрируется возвращаемое значение в случае, если функции CHARINDEX не удается найти строку *string_pattern* в строке.
  
```sql
SELECT TOP(1) CHARINDEX('at', 'This is a string') FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
0
```  
  
## <a name="see-also"></a>См. также раздел
 [LEN (Transact-SQL)](../../t-sql/functions/len-transact-sql.md)  
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)  
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
 [+ (объединение строк) (Transact-SQL)](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  


