---
description: EXP (Transact-SQL)
title: EXP (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EXP_TSQL
- EXP
dev_langs:
- TSQL
helpviewer_keywords:
- exponential functions
- EXP function
ms.assetid: 5a9b8c52-6fb6-4e33-8b02-a878785b2f51
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b3c9429ab47de08584fbc995fa8dfa9c8182ee40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101230"
---
# <a name="exp-transact-sql"></a>EXP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает значение экспоненты заданного выражения типа **float**.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
EXP ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *float_expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md) типа **float** или типа, который может быть неявно преобразован в тип **float**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **float**  
  
## <a name="remarks"></a>Комментарии  
 Константа **e** (2,718281...) является основанием натурального логарифма.  
  
 Экспонента числа является константой **e**, возведенной в степень числа. Например, EXP(1,0) = e^1,0 = 2,71828182845905, а EXP(10) = e^10 = 22026,4657948067.  
  
 Экспонента, взятая от натурального логарифма числа, равна самому этому числу: EXP (LOG (*n*)) = *n*. Натуральный логарифм, взятый от экспоненты числа, равен самому этому числу: LOG (EXP (*n*)) = *n*.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-finding-the-exponent-of-a-number"></a>A. Вычисление экспонента числа  
 В ходе выполнения следующего примера объявляется переменная и возвращается ее экспонента (`10`) с текстовым описанием.  
  
```sql  
DECLARE @var FLOAT  
SET @var = 10  
SELECT 'The EXP of the variable is: ' + CONVERT(VARCHAR, EXP(@var))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------------------------------------------------  
The EXP of the variable is: 22026.5  
(1 row(s) affected)  
```  
  
### <a name="b-finding-exponentials-and-natural-logarithms"></a>Б. Нахождение экспонентов и натуральных логарифмов  
 Представленный ниже пример возвращает значение экспоненты, взятой от натурального логарифма `20`, а также значение натурального логарифма, взятого от экспоненты `20`. Так как указанные функции являются обратными друг для друга, то в обоих случаях возвращается значение `20`.  
  
```sql  
SELECT EXP(LOG(20)), LOG(EXP(20))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------- ----------------------  
20                     20  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-finding-the-exponent-of-a-number"></a>В. Вычисление экспонента числа  
 В приведенном ниже примере возвращается значение экспоненты указанного значения (`10`).  
  
```sql  
SELECT EXP(10);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------  
22026.4657948067  
```  
  
### <a name="d-finding-exponential-values-and-natural-logarithms"></a>Г. Нахождение значений экспоненты и натуральных логарифмов  
 Представленный ниже пример возвращает значение экспоненты, взятой от натурального логарифма `20`, а также значение натурального логарифма, взятого от экспоненты `20`. Так как указанные функции являются обратными друг для друга, то в обоих случаях возвращается значение `20`.  
  
```sql  
SELECT EXP( LOG(20)), LOG( EXP(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------- -----------------  
20                  20  
```  
  
## <a name="see-also"></a>См. также  
 [Математические функции (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [LOG (Transact-SQL)](../../t-sql/functions/log-transact-sql.md)   
 [LOG10 (Transact-SQL)](../../t-sql/functions/log10-transact-sql.md)  
  
  

