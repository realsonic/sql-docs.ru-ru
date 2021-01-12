---
description: '% (остаток от деления) (Transact-SQL)'
title: '% (остаток от деления) (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- modulo
- modulus
- '% (Modulo)'
- '% (Modulus)'
- MOD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- '% (modulo operator)'
- '% (modulus operator)'
- remainder of division operation
- modulo operator (%)
- modulus operator (%)
ms.assetid: f93c662e-f405-486e-bf23-a2d03907b5bd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 34b80f69ef53e5cfa3c6c52edbdf3a78c8058e3c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98083909"
---
# <a name="-modulus-transact-sql"></a>% (остаток от деления) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает остаток от деления одного числа на другое.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
dividend % divisor  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *dividend*  
 Делимое числовое выражение. Аргумент *dividend* должен быть допустимым [выражением](../../t-sql/language-elements/expressions-transact-sql.md) любого типа из категорий целочисленных или денежных типов данных или типа **numeric**.  
  
 *divisor*  
 Числовое выражение, на которое делится делимое. Аргумент *divisor* должен быть допустимым выражением любого типа из категорий целочисленных или денежных типов данных или типа **numeric**.  
  
## <a name="result-types"></a>Типы результата  
 Определяются типами данных обоих аргументов.  
  
## <a name="remarks"></a>Комментарии  
 Оператор взятия остатка от деления можно использовать в списке выбора инструкции SELECT с любым сочетанием имен столбцов, числовых констант или любым допустимым выражением из категории целочисленных или денежных типов, а также типа **numeric**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-simple-example"></a>A. Простой пример  
 В следующем примере выполняется деление числа 38 на 5. Это приводит к получению числа 7, являющегося целой частью результата, и показывает, что в качестве остатка от деления возвращается число 3.  
  
```sql  
SELECT 38 / 5 AS Integer, 38 % 5 AS Remainder;
```  
  
### <a name="b-example-using-columns-in-a-table"></a>Б. Пример с использованием столбцов в таблице  
 Следующий пример возвращает код продукта, цену единицы модуля продукта и остаток от деления цены каждого продукта, преобразовывает к целому значению, в количество заказанных продуктов.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(100)ProductID, UnitPrice, OrderQty,  
   CAST((UnitPrice) AS INT) % OrderQty AS Modulo  
FROM Sales.SalesOrderDetail;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-simple-example"></a>В. Простой пример  
 В приведенном ниже примере показаны результаты применения оператора `%` при делении 3 на 2.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(1) 3%2 FROM dimEmployee;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
1         
```  
  
## <a name="see-also"></a>См. также:  
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [%= (присваивание остатка от деления) (Transact-SQL)](../../t-sql/language-elements/modulo-equals-transact-sql.md)   
 [Составные операторы (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


