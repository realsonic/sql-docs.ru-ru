---
description: AND (Transact-SQL)
title: AND (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- AND_TSQL
- AND
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], TRUE
- "TRUE"
- AND, about AND operators
- AND
- combining expressions
ms.assetid: b61d7f8d-5a51-49b7-91dd-f6190a5a0fb9
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 412344918629de3fa9b3ea1dbf90f41f4e343bdb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101064"
---
# <a name="and-transact-sql"></a>AND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Объединяет два логических выражения и возвращает значение **TRUE**, если оба выражения имеют значение **TRUE**. Если в инструкции используется более одного логического оператора, то операторы **AND** вычисляются первыми. Можно изменить порядок вычисления с помощью скобок.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
boolean_expression AND boolean_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *boolean_expression*  
 Любое допустимое [выражение](../../t-sql/language-elements/expressions-transact-sql.md), результатом которого являются логические значения **TRUE**, **FALSE** или **UNKNOWN**.  
  
## <a name="result-types"></a>Типы результата  
 **Boolean**  
  
## <a name="result-value"></a>Значение результата  
 Возвращает значение TRUE, если оба выражения — TRUE.  
  
## <a name="remarks"></a>Комментарии  
 В следующей диаграмме показаны результаты сравнения значений TRUE и FALSE с использованием оператора AND.  
  
||TRUE|FALSE|UNKNOWN|  
|------|----------|-----------|-------------|  
|**TRUE**|TRUE|FALSE|UNKNOWN|  
|**FALSE**|FALSE|FALSE|FALSE|  
|**UNKNOWN**|UNKNOWN|FALSE|UNKNOWN|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-the-and-operator"></a>A. Использование оператора AND  
 В следующем примере выбираются данные о сотрудниках, занимающих должность `Marketing Assistant`, начисленная продолжительность отпуска для которых составляет более `41` часов.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT  BusinessEntityID, LoginID, JobTitle, VacationHours   
FROM HumanResources.Employee  
WHERE JobTitle = 'Marketing Assistant'  
AND VacationHours > 41 ;  
```  
  
### <a name="b-using-the-and-operator-in-an-if-statement"></a>Б. Использование оператора AND в инструкции IF  
 В следующих примерах демонстрируется использование оператора AND в инструкции IF. В первой инструкции условия `1 = 1` и `2 = 2` имеют значение true, отсюда итоговый результат true. Во втором примере аргумент `2 = 17` имеет значение false, поэтому результатом будет false.  
  
```sql  
IF 1 = 1 AND 2 = 2  
BEGIN  
   PRINT 'First Example is TRUE'  
END  
ELSE PRINT 'First Example is FALSE' ;  
GO  
  
IF 1 = 1 AND 2 = 17  
BEGIN  
   PRINT 'Second Example is TRUE'  
END  
ELSE PRINT 'Second Example is FALSE' ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  
