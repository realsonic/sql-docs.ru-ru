---
description: LIKE (Transact-SQL)
title: LIKE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ESCAPE
- LIKE
- ESCAPE_TSQL
- LIKE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ESCAPE keyword
- '% (wildcard - character(s) to match)'
- ASCII pattern matching
- pattern searching [SQL Server]
- wildcard characters [SQL Server]
- literals [SQL Server], using wildcards
- character strings [SQL Server], LIKE
- exact matches [SQL Server]
- Boolean functions
- LIKE comparisons
- matching patterns [SQL Server]
- NOT LIKE keyword
ms.assetid: 581fb289-29f9-412b-869c-18d33a9e93d5
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd3f8c22b0087342b5d6b72ac492a3d542537c97
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98084559"
---
# <a name="like-transact-sql"></a>LIKE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Определяет, совпадает ли указанная символьная строка с заданным шаблоном. Шаблон может включать обычные символы и символы-шаблоны. Во время сравнения с шаблоном необходимо, чтобы его обычные символы в точности совпадали с символами, указанными в строке. Символы-шаблоны могут совпадать с произвольными элементами символьной строки. Использование символов-шаблонов в отличие от использования операторов сравнения строки (= и !=) делает оператор LIKE более гибким. Если тип данных одного из аргументов не является символьной строкой, компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], если это возможно, преобразует его в тип данных символьной строки.  
  
 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
match_expression [ NOT ] LIKE pattern [ ESCAPE escape_character ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
match_expression [ NOT ] LIKE pattern  
```  
>[!NOTE]
> ESCAPE и STRING_ESCAPE сейчас не поддерживаются в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *match_expression*  
 Любое допустимое выражение [expression](../../t-sql/language-elements/expressions-transact-sql.md) символьного типа данных.  
  
 *pattern*  
 Конкретная строка символов для поиска в *match_expression* может содержать следующие допустимые символы-шаблоны. Длина значения *pattern* не может превышать 8000 байт.  
  
|Символ-шаблон|Описание|Пример|  
|------------------------|-----------------|-------------|  
|%|Любая строка, содержащая ноль или более символов.|Инструкция WHERE Название LIKE '%компьютер%' выполняет поиск и выдает все названия книг, содержащие слово «компьютер».|  
|_ (подчеркивание)|Любой одиночный символ.|Инструкция WHERE фамилия_автора LIKE '_етров' выполняет поиск и выдает все имена, состоящие из шести букв и заканчивающиеся сочетанием «етров» (Петров, Ветров и т.п.).|  
|[ ]|Любой одиночный символ, содержащийся в диапазоне ([a-f]) или наборе ([abcdef]).|Инструкция WHERE Фамилия_автора LIKE '[Л-С]омов' выполняет поиск и выдает все фамилии авторов, заканчивающиеся на «омов» и начинающиеся на любую букву в промежутке от «Л» до «С», например Ломов, Ромов, Сомов и т.п. При выполнении операции поиска в диапазоне символы, включенные в диапазон, могут изменяться в зависимости от правил сортировки параметров сортировки.|  
|[^]|Любой одиночный символ, не содержащийся в  диапазоне ([^a-f]) или наборе ([^abcdef]).|Инструкция WHERE Фамилия_автора LIKE 'ив[^а]%' выполняет поиск и выдает все фамилии, начинающиеся на "ив", в которых третья буква отличается от "а".|  
  
 *escape_character*  
 Символ, помещаемый перед символом-шаблоном для того, чтобы символ-шаблон рассматривался как обычный символ, а не как шаблон. Аргумент *escape_character* является символьным выражением, не имеющим значения по умолчанию и возвращающим результат в виде одного символа.  
  
## <a name="result-types"></a>Типы результата  
 **Boolean**  
  
## <a name="result-value"></a>Значение результата  
 Оператор LIKE возвращает значение TRUE, если аргумент *match_expression* совпадает с указанным аргументом *pattern*.  
  
## <a name="remarks"></a>Комментарии  
 При использовании оператора LIKE для сравнения строк во внимание принимаются все символы строки-шаблона. К значимым символам также относятся начальные и конечные пробелы. Если операция сравнения в запросе должна вернуть все строки, содержащие строки LIKE 'абв ' (с символом пробела на конце), то строка, содержащая "абв" (без пробела), не будет возвращена. Однако завершающие пробелы в выражении, с которым сравнивается шаблон, не учитываются. Если операция сравнения в запросе должна вернуть все строки, содержащие строки LIKE 'абв' (без знака пробела на конце), то будут возвращены все строки, содержащие «абв», как с завершающими пробелами, так и без них.  
  
 При сравнении строк с помощью оператора LIKE с использованием шаблона, содержащего тип данных **char** и **varchar**, могут возникнуть проблемы из-за методов хранения каждого типа данных. В ходе выполнения следующего примера локальная переменная **char** передается хранимой процедуре, а затем с помощью сравнения с шаблоном выполняется поиск всех сотрудников, чьи фамилии начинаются с указанной последовательности букв.  
  
```sql
-- Uses AdventureWorks  
  
CREATE PROCEDURE FindEmployee @EmpLName CHAR(20)  
AS  
SELECT @EmpLName = RTRIM(@EmpLName) + '%';  
SELECT p.FirstName, p.LastName, a.City  
FROM Person.Person p JOIN Person.Address a ON p.BusinessEntityID = a.AddressID  
WHERE p.LastName LIKE @EmpLName;  
GO  
EXEC FindEmployee @EmpLName = 'Barb';  
GO  
```  
  
 Выполнение процедуры `FindEmployee` не дает результатов, так как переменная типа **char** (`@EmpLName`) всегда имеет длину в 20 символов, до которой дополняется завершающими знаками пробела. Переменные, содержащиеся в столбце `LastName`, имеют тип **varchar**. Поэтому завершающие пробелы в них не дописываются. Данная процедура завершается неудачей, так как завершающие пробелы учитываются.  
  
 Процедура из следующего примера выполняется успешно, так как завершающие пробелы к переменной типа **varchar** не добавляются.  
  
```sql
-- Uses AdventureWorks  
  
CREATE PROCEDURE FindEmployee @EmpLName VARCHAR(20)  
AS  
SELECT @EmpLName = RTRIM(@EmpLName) + '%';  
SELECT p.FirstName, p.LastName, a.City  
FROM Person.Person p JOIN Person.Address a ON p.BusinessEntityID = a.AddressID  
WHERE p.LastName LIKE @EmpLName;  
GO  
EXEC FindEmployee @EmpLName = 'Barb';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
FirstName      LastName            City
----------     -------------------- --------------- 
Angela         Barbariol            Snohomish
David          Barber               Snohomish
(2 row(s) affected)  
```

## <a name="pattern-matching-by-using-like"></a>Совпадение с шаблоном с использованием оператора LIKE  
 Оператор LIKE поддерживает шаблоны в ASCII и Юникоде. Если все аргументы (*match_expression*, *pattern* и *escape_character*, если он указан) имеют символьный тип ASCII, то применяется шаблон ASCII. В случае, когда какой-либо из аргументов имеет тип данных Юникод, выполняется преобразование всех аргументов в Юникод и применяется шаблон Юникод. Если вы используете оператор LIKE с типом данных Юникода (**nchar** или **nvarchar**), завершающие пробелы учитываются в отличие от других типов данных (не Юникода). Работа оператора LIKE с данными в Юникоде совместима со стандартом ISO. Принцип работы оператора LIKE с данными ASCII совместим с более ранними версиями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Приведенные ниже примеры поясняют различия между результатами сравнения данных с шаблонами оператора LIKE, представленными в Юникоде и ASCII.  
  
```sql  
-- ASCII pattern matching with char column  
CREATE TABLE t (col1 CHAR(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE col1 LIKE '% King';   -- returns 1 row  
  
-- Unicode pattern matching with nchar column  
CREATE TABLE t (col1 NCHAR(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE col1 LIKE '% King';   -- no rows returned  
  
-- Unicode pattern matching with nchar column and RTRIM  
CREATE TABLE t (col1 NCHAR(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE RTRIM(col1) LIKE '% King';   -- returns 1 row  
```
  
> [!NOTE]  
>  Операции сравнения с помощью оператора LIKE зависят от параметров сортировки. Дополнительные сведения см. в разделе [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md).  
  
## <a name="using-the--wildcard-character"></a>Использование символа-шаблона «%»  
 Если в операторе LIKE указать символ '5%', то компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] будет искать число «5», за которым следует любая строка с числом символов от нуля и больше.  
  
 Например, при выполнении следующего примера отображаются все динамические административные представления базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], так как все они начинаются символами `dm`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT Name  
FROM sys.system_views  
WHERE Name LIKE 'dm%';  
GO  
```  
  
 Чтобы отобразить все объекты, не являющиеся динамическими административными представлениями, используется синтаксис `NOT LIKE 'dm%'`. Например, если всего имеется 32 объекта и оператор LIKE выдает 13 наименований, совпадающих с шаблоном, то оператор NOT LIKE возвращает 19 объектов, не соответствующих указанному в операторе LIKE шаблону.  
  
 По такому шаблону, как `LIKE '[^d][^m]%'`, не всегда будут возвращаться одни и те же имена. Вместо 19 имен можно найти только 14, так как имена, которые начинаются с буквы `d` или у которых второй буквой является `m`, будут исключены из результата, как и имена динамических административных представлений. Причиной такой реакции на событие является поэтапный поиск отрицательных символов-шаблонов: за один шаг обрабатывается один символ-шаблон. Процесс поиска совпадений прекращается при возникновении сбоя на любой стадии выполнения.  
  
## <a name="using-wildcard-characters-as-literals"></a>Использование символов-шаблонов в качестве литералов  
 Символы-шаблоны могут быть использованы в качестве литералов. Чтобы использовать символ-шаблон в качестве литерала, его необходимо заключать в скобки. В следующей таблице представлены несколько примеров применения ключевого слова LIKE вместе с символами-шаблонами [ ].  
  
|Символ|Значение|  
|------------|-------------|  
|LIKE '5[%]'|5 %|  
|LIKE '[_]n'|_n|  
|LIKE '[a-cdf]'|a, b, c, d или f|  
|LIKE '[-acdf]'|-, a, b, c, d или f|  
|LIKE '[ [ ]'|[|  
|LIKE ']'|]|  
|LIKE 'abc[_]d%'|abc_d и abc_de|  
|LIKE 'abc[def]'|abcd, abce и abcf|  
  
## <a name="pattern-matching-with-the-escape-clause"></a>Совпадение с шаблоном с помощью предложения ESCAPE  
 Можно искать символьные строки, в состав которых входит один или более специальных символов-шаблонов. Например, таблица discounts базы данных customers может содержать значения скидок, включающих знак процента (%). Чтобы выполнить поиск знака процента в качестве символа-шаблона, необходимо ввести ключевое слово ESCAPE и escape-символ. Например, образец базы данных содержит столбец с именем comment, в котором хранится значение «30%». Чтобы найти строки, содержащие последовательность символов «30%» в столбце comment, необходимо указать предложение WHERE, например `WHERE comment LIKE '%30!%%' ESCAPE '!'`. Если предложение ESCAPE и escape-символ не указаны, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] вернет все записи, содержащие последовательность символов "30!".  
  
 Если в шаблоне LIKE после escape-символа нет никакого символа, то шаблон является недопустимым и оператор LIKE возвращает значение FALSE. Если символ после escape-символа не является символом-шаблоном, то escape-символ игнорируется, а следующий символ рассматривается как обычный символ в шаблоне. К этим символам-шаблонам относятся: подчеркивание (_), процент (%) и левая квадратная скобка ([), в том случае, если они заключены в квадратные скобки. Escape-символы могут использоваться в квадратных скобках ([ ]), включая: знак вставки (^), дефис (-) и правую квадратную скобку (]).  
  
 Символ 0x0000 (**char(0)**) не определен в параметрах сортировки Windows, и его нельзя включать в LIKE.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-like-with-the--wildcard-character"></a>A. Применение оператора LIKE с символом-шаблоном %  
 В следующем примере в таблице `415` выполняется поиск всех телефонных номеров с кодом города `PersonPhone`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, ph.PhoneNumber  
FROM Person.PersonPhone AS ph  
INNER JOIN Person.Person AS p  
ON ph.BusinessEntityID = p.BusinessEntityID  
WHERE ph.PhoneNumber LIKE '415%'  
ORDER by p.LastName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
 FirstName             LastName             Phone
 -----------------     -------------------  ------------
 Ruben                 Alonso               415-555-124  
 Shelby                Cook                 415-555-0121  
 Karen                 Hu                   415-555-0114  
 John                  Long                 415-555-0147  
 David                 Long                 415-555-0123  
 Gilbert               Ma                   415-555-0138  
 Meredith              Moreno               415-555-0131  
 Alexandra             Nelson               415-555-0174  
 Taylor                Patterson            415-555-0170  
 Gabrielle              Russell             415-555-0197  
 Dalton                 Simmons             415-555-0115  
 (11 row(s) affected)  
```

### <a name="b-using-not-like-with-the--wildcard-character"></a>Б. Применение оператора NOT LIKE с символом-шаблоном %  
 В следующем примере в таблице `PersonPhone` выполняется поиск всех телефонных номеров с региональным кодом, отличным от `415`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, ph.PhoneNumber  
FROM Person.PersonPhone AS ph  
INNER JOIN Person.Person AS p  
ON ph.BusinessEntityID = p.BusinessEntityID  
WHERE ph.PhoneNumber NOT LIKE '415%' AND p.FirstName = 'Gail'  
ORDER BY p.LastName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
FirstName              LastName            Phone
---------------------- -------------------- -------------------
Gail                  Alexander            1 (11) 500 555-0120  
Gail                  Butler               1 (11) 500 555-0191  
Gail                  Erickson             834-555-0132  
Gail                  Erickson             849-555-0139  
Gail                  Griffin              450-555-0171  
Gail                  Moore                155-555-0169  
Gail                  Russell              334-555-0170  
Gail                  Westover             305-555-0100  
(8 row(s) affected)  
```

### <a name="c-using-the-escape-clause"></a>В. Применение предложения ESCAPE  
 В следующем примере предложение `ESCAPE` и escape-символ используются для поиска символьной строки `10-15%` в столбце `c1` таблицы `mytbl2`.  
  
```sql
USE tempdb;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'mytbl2')  
   DROP TABLE mytbl2;  
GO  
USE tempdb;  
GO  
CREATE TABLE mytbl2  
(  
 c1 sysname  
);  
GO  
INSERT mytbl2 VALUES ('Discount is 10-15% off'), ('Discount is .10-.15 off');  
GO  
SELECT c1   
FROM mytbl2  
WHERE c1 LIKE '%10-15!% off%' ESCAPE '!';  
GO  
```  
  
### <a name="d-using-the---wildcard-characters"></a>Г. Использование символов-шаблонов [ ]  
 В следующем примере выполняется поиск в таблице `Person` сотрудников с именем `Cheryl` или`Sheryl`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, FirstName, LastName   
FROM Person.Person   
WHERE FirstName LIKE '[CS]heryl';  
GO  
```  
  
 В следующем примере выполняется поиск строк в таблице `Person` для сотрудников с фамилией `Zheng` или `Zhang`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName  
FROM Person.Person  
WHERE LastName LIKE 'Zh[ae]ng'  
ORDER BY LastName ASC, FirstName ASC;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-like-with-the--wildcard-character"></a>Д. Применение оператора LIKE с символом-шаблоном %  
 В следующем примере в таблице `DimEmployee` выполняется поиск всех сотрудников, телефонные номера которых начинаются с `612`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone LIKE '612%'  
ORDER by LastName;  
```  
  
### <a name="f-using-not-like-with-the--wildcard-character"></a>Е. Применение оператора NOT LIKE с символом-шаблоном %  
 В следующем примере в таблице `DimEmployee` выполняется поиск всех телефонных номеров, которые не начинаются с `612`.  .  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone NOT LIKE '612%'  
ORDER by LastName;  
```  
  
### <a name="g-using-like-with-the-_-wildcard-character"></a>Ж. Применение оператора LIKE с символом-шаблоном _  
 В следующем примере в таблице `DimEmployee` выполняется поиск всех телефонных номеров, начинающихся с `2` и заканчивающихся на `6`. Подстановочный знак "%" добавлен в конце шаблона поиска, что соответствует любым следующим символам в значениях столбца с телефонными номерами.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone LIKE '6_2%'  
ORDER by LastName;   
```  
  
## <a name="see-also"></a>См. также  
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
