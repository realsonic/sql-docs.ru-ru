---
description: CHAR (Transact-SQL)
title: CHAR (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- char_TSQL
- char
dev_langs:
- TSQL
helpviewer_keywords:
- converting int ASCII code to character
- control characters
- tab
- ASCII conversions
- CHAR function
- carriage return
- inserting control characters
- characters [SQL Server], control
- line feed
- printing ASCII values
ms.assetid: 955afe94-539c-465d-af22-16ec45da432a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4c0281782b3a21c809ee725d4dd99d80fbad20ec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097298"
---
# <a name="char-transact-sql"></a>CHAR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция преобразует код ASCII **int** в значение символа.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CHAR ( integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*integer_expression*  
Целое число от 0 до 255. `CHAR` возвращает значение `NULL` для целочисленных выражений за пределами этого диапазона, или когда целое число выражает только первый байт двухбайтового символа.

> [!NOTE]
> Некоторые неевропейские кодировки, такие как [Shift Japanese Industrial Standards (Shift JIS)](https://www.wikipedia.org/wiki/Shift_JIS), содержат символы, которые могут быть представлены в однобайтовой схеме кодирования, но требуют многобайтового кодирования. Дополнительные сведения о кодировках см. в статье [Однобайтовые и многобайтовые кодировки](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets). 
  
## <a name="return-types"></a>Типы возвращаемых данных
**char(1)**
  
## <a name="remarks"></a>Комментарии  
Функция `CHAR` может использоваться для вставки управляющих символов в символьные строки. В этой таблице показаны некоторые часто используемые управляющие символы.
  
|Управляющий символ|Значение|  
|---|---|
|Вкладка|**char(9)**|  
|Перевод строки|**char(10)**|  
|Возврат каретки|**char(13)**|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-ascii-and-char-to-print-ascii-values-from-a-string"></a>A. Использование ASCII и CHAR для отображения значений ASCII из строки  
В этом примере отображается значение ASCII и символ для каждого символа в строке `New Moon`.
  
```sql
SET TEXTSIZE 0;  
-- Create variables for the character string and for the current   
-- position in the string.  
DECLARE @position INT, @string CHAR(8);  
-- Initialize the current position and the string variables.  
SET @position = 1;  
SET @string = 'New Moon';  
WHILE @position <= DATALENGTH(@string)  
   BEGIN  
   SELECT ASCII(SUBSTRING(@string, @position, 1)),   
      CHAR(ASCII(SUBSTRING(@string, @position, 1)))  
   SET @position = @position + 1  
   END;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- -
78          N  
----------- -  
101         e  
----------- -  
119         w  
----------- -  
32  
----------- -  
77          M  
----------- -  
111         o  
----------- -  
111         o  
----------- - 
110         n  
```
  
### <a name="b-using-char-to-insert-a-control-character"></a>Б. Использование функции CHAR для вставки управляющего символа  
В этом примере используется выражение `CHAR(13)`, чтобы отображать имя и адрес электронной почты сотрудника в отдельных строках, когда результаты запроса возвращаются в виде текста. В этом примере используется база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT p.FirstName + ' ' + p.LastName, + CHAR(13)  + pe.EmailAddress   
FROM Person.Person p 
INNER JOIN Person.EmailAddress pe ON p.BusinessEntityID = pe.BusinessEntityID  
  AND p.BusinessEntityID = 1;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Ken Sanchez
ken0@adventure-works.com
  
(1 row(s) affected)
```
  
### <a name="c-using-ascii-and-char-to-print-ascii-values-from-a-string"></a>В. Использование ASCII и CHAR для отображения значений ASCII из строки  
В этом примере используется набор символов ASCII. В нем возвращается значение символа для шести разных числовых значений символов ASCII.
  
```sql
SELECT CHAR(65) AS [65], CHAR(66) AS [66],   
CHAR(97) AS [97], CHAR(98) AS [98],   
CHAR(49) AS [49], CHAR(50) AS [50];  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
65   66   97   98   49   50  
---- ---- ---- ---- ---- ----  
A    B    a    b    1    2  
```
  
### <a name="d-using-char-to-insert-a-control-character"></a>Г. Использование функции CHAR для вставки управляющего символа  
В этом примере используется `CHAR(13)` для получения сведений из файла sys.databases в отдельных строках, когда результаты запроса возвращаются в виде текста.
  
```sql
SELECT name, 'was created on ', create_date, CHAR(13), name, 'is currently ', state_desc   
FROM sys.databases;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
name                                      create_date               name                                  state_desc  
--------------------------------------------------------------------------------------------------------------------  
master                    was created on  2003-04-08 09:13:36.390   master                  is currently  ONLINE 
tempdb                    was created on  2014-01-10 17:24:24.023   tempdb                  is currently  ONLINE   
AdventureWorksPDW2012     was created on  2014-05-07 09:05:07.083   AdventureWorksPDW2012   is currently  ONLINE 
```

### <a name="e-using-char-to-return-single-byte-characters"></a>Д. Использование функции CHAR для возврата однобайтовых символов  
В этом примере используется целое и шестнадцатеричное значения в допустимом диапазоне кодировки ASCII. Функция CHAR может возвратить однобайтовый японский символ.
  
```sql
SELECT CHAR(188) AS single_byte_representing_complete_character, 
  CHAR(0xBC) AS single_byte_representing_complete_character;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
single_byte_representing_complete_character single_byte_representing_complete_character
------------------------------------------- -------------------------------------------
ｼ                                           ｼ                                         
```

### <a name="f-using-char-to-return-multibyte-characters"></a>Е. Использование функции CHAR для возврата многобайтовых символов  
В этом примере используется целое и шестнадцатеричное значения в допустимом диапазоне кодировки ASCII. Функция CHAR возвращает значение NULL, так как параметр представляет только первый байт многобайтового символа.
  
```sql
SELECT CHAR(129) AS first_byte_of_double_byte_character, 
  CHAR(0x81) AS first_byte_of_double_byte_character;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
first_byte_of_double_byte_character first_byte_of_double_byte_character
----------------------------------- -----------------------------------
NULL                                NULL                                         
```
  
## <a name="see-also"></a>См. также
 [ASCII (Transact-SQL)](../../t-sql/functions/ascii-transact-sql.md)  
 [NCHAR (Transact-SQL)](../../t-sql/functions/nchar-transact-sql.md)  
 [UNICODE (Transact-SQL)](../../t-sql/functions/unicode-transact-sql.md)  
 [+ (объединение строк) (Transact-SQL)](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)
  
  

