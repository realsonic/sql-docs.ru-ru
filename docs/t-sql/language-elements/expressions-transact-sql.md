---
description: Выражения (Transact-SQL)
title: Выражения (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- Boolean expressions
- expressions [SQL Server], about expressions
- combining expressions
- Transact-SQL expressions
- expressions [SQL Server], combining
- simple expressions [SQL Server]
- complex expressions [SQL Server]
ms.assetid: ee53c5c8-e36c-40f9-8cd1-d933791b98fa
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6db31166cf0931e7ace6a6f1366fed9776509568
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100384"
---
# <a name="expressions-transact-sql"></a>Выражения (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Сочетание символов и операторов, используемое компонентом [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] для вычисления одиночного значения данных. Отдельные константы, переменные, столбцы и скалярные функции являются примерами простых выражений. Для соединения двух и более простых выражений в одно сложное можно использовать операторы.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
{ constant | scalar_function | [ table_name. ] column | variable   
    | ( expression ) | ( scalar_subquery )   
    | { unary_operator } expression   
    | expression { binary_operator } expression   
    | ranking_windowed_function | aggregate_windowed_function  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Expression in a SELECT statement  
<expression> ::=   
{  
    constant   
    | scalar_function   
    | column  
    | variable  
    | ( expression  )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE Windows_collation_name ]  
  
-- Scalar Expression in a DECLARE, SET, IF...ELSE, or WHILE statement  
<scalar_expression> ::=  
{  
    constant   
    | scalar_function   
    | variable  
    | ( expression  )  
    | (scalar_subquery )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE { Windows_collation_name ]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
  
|Термин|Определение|  
|----------|----------------|  
|*constant*|Символ, представляющий одно конкретное значение данных. Дополнительные сведения см. в статье [Константы (Transact-SQL)](../../t-sql/data-types/constants-transact-sql.md).|  
|*scalar_function*|Единица синтаксиса [!INCLUDE[tsql](../../includes/tsql-md.md)], который предоставляет определенную службу и возвращает одиночное значение. *scalar_function* может быть встроенной скалярной функцией, такой как SUM, GETDATE или CAST, либо определяемыми пользователем скалярными функциями.|  
|[ _table_name_ **.** ]|Имя или псевдоним таблицы.|  
|*column*|Имя столбца. Только имя столбца используется в выражении.|  
|*variable*|Имя переменной или параметр. Дополнительные сведения см. в статье [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md).|  
| **(** _expression_  **)**|Любое допустимое выражение из определенных в этом разделе. Скобки являются операторами группировки, гарантирующими, что все операторы выражения внутри скобок будут выполнены, прежде чем результирующее выражение будет объединено с другим.|  
|**(** _scalar_subquery_ **)**|Вложенный запрос, возвращающий одиночное значение. Пример:<br /><br /> `SELECT MAX(UnitPrice)`<br /><br /> `FROM Products`|  
|{ *unary_operator* }|Унарные операторы можно применять только к выражениям, выполняемым с любыми типами данных из категории числовых типов данных. Оператор, имеющий только один числовой операнд:<br /><br /> + обозначает положительное число.<br /><br /> - обозначает отрицательное число.<br /><br /> ~ обозначает оператор дополнения.|  
|{ *binary_operator* }|Этот оператор указывает, как объединяются два выражения для получения единого результата. Аргумент *binary_operator* может быть арифметическим, логическим, битовым или унарным оператором, а также оператором присвоения (=), сравнения или объединения (+). Дополнительные сведения об операторах  см. в разделе Операторы (Transact-SQL).|  
|*ranking_windowed_function*|Любая ранжирующая функция языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Дополнительные сведения см. в разделе [Ранжирующие функции (Transact-SQL)](../../t-sql/functions/ranking-functions-transact-sql.md).|  
|*aggregate_windowed_function*|Любая агрегатная функция языка [!INCLUDE[tsql](../../includes/tsql-md.md)], содержащая предложение OVER. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).|  
  
## <a name="expression-results"></a>Результаты выражения  
 Для простых выражений, состоящих из одной константы, переменной, скалярной функции или имени столбца, в качестве типа данных, параметров сортировки, числа разрядов, точности и значения выражения используются тип данных, параметры сортировки, число разрядов, точность и значение элемента, на который они ссылаются.  
  
 При объединении двух выражений с помощью операторов сравнения или логических операторов результат будет иметь логический тип данных и может принимать одно из следующих значений: TRUE, FALSE или UNKNOWN. Дополнительные сведения о логических типах данных см. в разделе [Операторы сравнения (Transact-SQL)](../../t-sql/language-elements/comparison-operators-transact-sql.md).  
  
 При объединении двух выражений с помощью арифметических, побитовых или строковых операторов тип данных результата определяется используемым оператором.  
  
 Сложные выражения, составленные из нескольких символов и операторов, вычисляются в одиночные результаты. Тип данных, параметры сортировки, точность и значение результирующего выражения определяются объединением составляющих выражений (по два за один раз) до тех пор, пока не будет получен конечный результат. Последовательность, в которой эти выражения объединяются, зависит от приоритета операторов в выражении.  
  
## <a name="remarks"></a>Комментарии  
 Два выражения можно объединить каким-либо оператором, если оба они относятся к типам данных, поддерживаемым оператором, и выполняется хотя бы одно из следующих условий.  
  
-   Выражения относятся к одному типу данных.  
  
-   Тип данных с более низким приоритетом может быть неявно преобразован в тип данных с более высоким приоритетом.  
  
 Если выражения не удовлетворяют этим условиям, функция CAST или CONVERT явным образом преобразует тип данных с более низким приоритетом или в тип данных с более высоким приоритетом, или в промежуточный тип данных, который затем может быть неявно преобразован в тип данных с более высоким приоритетом.  
  
 Если неявное или явное преобразование не поддерживается, эти два выражения объединить невозможно.  
  
 Параметры сортировки любого выражения, результатом которого является символьная строка, определяются правилами очередности параметров сортировки. Дополнительные сведения см. в статье [Очередность параметров сортировки (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
 В языке программирования, таком как C или [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], вычисление выражения всегда приводит к получению единственного результата. Выражения в списке [!INCLUDE[tsql](../../includes/tsql-md.md)] подчиняются правилу: выражение вычисляется отдельно для каждой строки в результирующем наборе. У отдельных выражений могут быть различные значения в каждой строке результирующего набора, но у каждой строки имеется только одно значение для выражения. Например, в следующей инструкции `SELECT` выражениями являются как ссылка на `ProductID`, так и значение `1+2` в списке выбора:  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, 1+2  
FROM Production.Product;  
GO  
```  
  
 Выражение `1+2` дает результат `3` в каждой строке результирующего набора. Несмотря на то, что выражение `ProductID` формирует уникальное значение для каждой строки в результирующем наборе, в каждой строке содержится только одно значение для `ProductID`.  
 
- [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] выделяет фиксированный максимальный объем памяти для каждого потока, чтобы ни один поток не мог использовать всю память целиком.  Часть этой памяти используется для хранения выражений запросов.  Если запрос содержит слишком много выражений и требуемая память превышает внутреннее ограничение, подсистема не будет выполнять его.  Чтобы избежать этой проблемы, пользователь может разделить запрос на несколько запросов с меньшим количеством выражений в каждом. Например, имеется запрос с длинным списком выражений в предложении WHERE: 

```sql
DELETE FROM dbo.MyTable 
WHERE
(c1 = '0000001' AND c2 = 'A000001') or
(c1 = '0000002' AND c2 = 'A000002') or
(c1 = '0000003' AND c2 = 'A000003') or
...

```
Измените этот запрос следующим образом:

```sql
DELETE FROM dbo.MyTable WHERE (c1 = '0000001' AND c2 = 'A000001');
DELETE FROM dbo.MyTable WHERE (c1 = '0000002' AND c2 = 'A000002');
DELETE FROM dbo.MyTable WHERE (c1 = '0000003' AND c2 = 'A000003');
...
```

## <a name="see-also"></a>См. также  
 [AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [COALESCE (Transact-SQL)](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Приоритет типов данных (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [NULLIF (Transact-SQL)](../../t-sql/language-elements/nullif-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  
