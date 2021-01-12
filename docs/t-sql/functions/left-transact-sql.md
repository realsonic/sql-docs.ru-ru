---
description: LEFT (Transact-SQL)
title: LEFT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LEFT
- LEFT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- character strings [SQL Server], LEFT
- characters [SQL Server], leftmost
- LEFT function
- leftmost character of expression
ms.assetid: 44a8c71b-63d8-458b-8b5d-99d570067c3c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 532555bf5908a693ffdabbf1337eacf485bd3836
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101152"
---
# <a name="left-transact-sql"></a>LEFT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает указанное число символов символьного выражения слева.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
LEFT ( character_expression , integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *character_expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md) символьных или двоичных данных. *character_expression* может быть константой, переменной или столбцом. *character_expression* может иметь любой тип данных, который может быть неявно преобразован в **varchar** или **nvarchar**, кроме **text** или **ntext**. В противном случае используйте функцию [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) для явного преобразования типа аргумента *character_expression*.  
 
> [!NOTE]  
> Если *string_expression* имеет тип **binary** или **varbinary**, LEFT выполняет неявное преобразование в тип **varchar** и, следовательно, двоичные входные данные не сохраняются.  
  
 *integer_expression*  
 Положительное целое число, указывающее количество символов выражения *character_expression*, которое будет возвращено. Если аргумент *integer_expression* отрицателен, возвращается ошибка. Если аргумент *integer_expression* имеет тип **bigint** и содержит большое значение, аргумент *character_expression* должен иметь длинный тип данных, например **varchar(max)** .  
  
 Параметр *integer_expression* обрабатывает суррогатный символ UTF-16 как один символ.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Возвращает значение типа **varchar**, если *character_expression* имеет символьный тип данных, отличный от Юникода.  
  
 Возвращает значение типа **nvarchar**, если *character_expression* имеет символьный тип данных Юникода.  
  
## <a name="remarks"></a>Remarks  
 При использовании параметров сортировки SC в параметре *integer_expression* суррогатная пара UTF-16 рассматривается как один символ. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-left-with-a-column"></a>A. Применение функции LEFT со столбцом  
 В следующем примере возвращается по пять первых символов от каждого из названий продуктов в таблице `Product` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT LEFT(Name, 5)   
FROM Production.Product  
ORDER BY ProductID;  
GO  
```  
  
### <a name="b-using-left-with-a-character-string"></a>Б. Применение функции LEFT с символьной строкой  
 Следующий пример показывает, как функция `LEFT` используется для получения двух первых символов из символьной строки `abcdefg`.  
  
```sql  
SELECT LEFT('abcdefg',2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab   
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-left-with-a-column"></a>В. Применение функции LEFT со столбцом  
 В следующем примере возвращаются пять первых символов от каждого из названий продуктов.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT(EnglishProductName, 5)   
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="d-using-left-with-a-character-string"></a>Г. Применение функции LEFT с символьной строкой  
 Следующий пример показывает, как функция `LEFT` используется для получения двух первых символов из символьной строки `abcdefg`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT('abcdefg',2) FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab  
```  
  
## <a name="see-also"></a>См. также:  
 [LTRIM (Transact-SQL)](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT (Transact-SQL)](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM (Transact-SQL)](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT (Transact-SQL)](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING (Transact-SQL)](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM (Transact-SQL)](../../t-sql/functions/trim-transact-sql.md)  
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

