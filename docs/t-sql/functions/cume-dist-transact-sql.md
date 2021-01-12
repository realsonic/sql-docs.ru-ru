---
description: CUME_DIST (Transact-SQL)
title: CUME_DIST (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CUME_DIST
- CUME_DIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CUME_DIST function
- analytic functions, CUME_DIST
ms.assetid: 491b07f3-9ffd-4cdd-93e5-5abb636fc5ef
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4252c237e234fdc380ca0f4b18cfcf55f6bf9ae4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093743"
---
# <a name="cume_dist-transact-sql"></a>CUME_DIST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] эта функция вычисляет интегральное распределение значений в группе значений. Другими словами, `CUME_DIST` вычисляет относительное положение указанного значения в группе значений. Исходя из восходящего порядка сортировки, `CUME_DIST` значения для строки _r_ — это число строк со значениями, меньшими или равными значению _r_, деленное на число строк, полученных в секции или результирующем наборе запроса. Функция `CUME_DIST` подобна функции `PERCENT_RANK`.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CUME_DIST( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_)  

Предложение _partition\_by\_clause_ делит результирующий набор, полученный с помощью предложения FROM, на секции, к которым применяется функция. Если аргумент _partition\_by\_clause_ не указан, функция `CUME_DIST` обрабатывает все строки результирующего набора запроса как одну группу. Аргумент _order\_by\_clause_ определяет логический порядок, в котором выполняется операция. Для функции `CUME_DIST` аргумент _order\_by\_clause_ является обязательным. `CUME_DIST` не принимает \<rows or range clause> синтаксиса OVER. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).
  
## <a name="return-types"></a>Типы возвращаемых данных
**float(53)**
  
## <a name="remarks"></a>Remarks  
`CUME_DIST` возвращает диапазон значений, которые больше 0 и меньше или равны 1. Для равных значений всегда вычисляется одно и то же значение накопительного распределения. `CUME_DIST` включает значения NULL по умолчанию и рассматривает их как наименьшие из возможных значений.
  
Функция `CUME_DIST` не детерминирована. Дополнительные сведения см. в разделе [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Примеры  
В этом примере с помощью функции `CUME_DIST` выполняется вычисление процентиля заработной платы для каждого сотрудника указанного отдела. Значение, возвращаемое функцией `CUME_DIST`, представляет процент сотрудников, заработная плата которых меньше или равна заработной плате текущего сотрудника этого отдела. Функция `PERCENT_RANK` вычисляет процент заработной платы сотрудника в рамках отдела. Для секционирования строк результирующего набора по отделам в примере указывается значение _partition\_by\_clause_. Предложение ORDER BY в предложении OVER логически упорядочивает строки в каждой секции. Предложение ORDER BY в инструкции SELECT определяет порядок отображения результирующего набора.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также раздел
[PERCENT_RANK (Transact-SQL)](../../t-sql/functions/percent-rank-transact-sql.md)
  
  
