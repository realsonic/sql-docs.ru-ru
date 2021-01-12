---
description: SELECT @local_variable (Transact-SQL)
title: SELECT @local_variable (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- variable_TSQL
- '@loca_variable'
- '@local'
- variable
- '@loca_variable_TSQL'
- '@local_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- variables [SQL Server], assigning
- SELECT statement [SQL Server], @local_variable
- '@local_variable'
- local variables [SQL Server]
ms.assetid: 8e1a9387-2c5d-4e51-a1fd-a2a95f026d6f
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current ||>= sql-server-2016 ||= azure-sqldw-latest||>= sql-server-linux-2017
ms.openlocfilehash: 3b1545d50733a5139c0a4d6ef22f22b719691fc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100344"
---
# <a name="select-local_variable-transact-sql"></a>SELECT @local_variable (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Присваивает локальную переменную значению выражения.  
  
 Для присваивания переменных рекомендуется использовать инструкцию [SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md) вместо SELECT @*local_variable*.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SELECT { @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression } 
    [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

@*local_variable*  
 Это объявленная переменная, которой должно быть присвоено значение.  
  
{= \| += \| -= \| \*= \| /= \| %= \| &= \| ^= \| \|= }  
Присвоить значение справа переменной слева.  
  
Составной оператор присваивания:  

| оператор | action |  
| -------- | ------ |  
| = | Присваивает следующее за ним выражение переменной. |  
| += | Сложение и присваивание |  
| -= | Вычитание и присваивание |  
| \*= | Умножение и присваивание |  
| /= | Деление и присваивание |  
| %= | Остаток от деления и присваивание |  
| &= | Выполнение побитовой операции AND и присваивание |  
| ^= | Выполнение побитовой операции XOR и присваивание |  
| \|= | Выполнение побитовой операции OR и присваивание |  

*expression*  
Любое допустимое выражение [expression](../../t-sql/language-elements/expressions-transact-sql.md). В их число также входит скалярный вложенный запрос.  

## <a name="remarks"></a>Remarks

SELECT @*local_variable* обычно используется для возвращения одиночного значения в переменную. Однако, если аргумент *expression* является именем столбца, может вернуться несколько значений. Если инструкция SELECT возвращает более одного значения, переменной присваивается последнее возвращенное значение.  

Если инструкция SELECT не возвращает ни одной строки, переменная сохраняет свое текущее значение. Если аргумент *expression* является скалярным вложенным запросом, который не возвращает значений, переменная принимает значение NULL.  

Одна инструкция SELECT может инициализировать несколько локальных переменных.  

> [!NOTE]
> Инструкция SELECT, содержащая назначение переменной, не может быть использована для выполнения операций по получению типичного результирующего набора.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-use-select-local_variable-to-return-a-single-value"></a>A. Используйте инструкцию SELECT @local_variable для возвращения одиночного значения  
 В следующем примере переменной `@var1` присвоено значение `Generic Name`. Запрос к таблице `Store` не возвращает строк, потому что в ней отсутствует значение, указанное для `CustomerID`. Переменная сохраняет значение `Generic Name`.  
  
```sql  
-- Uses AdventureWorks    
  
DECLARE @var1 VARCHAR(30);         
SELECT @var1 = 'Generic Name';         
SELECT @var1 = Name         
FROM Sales.Store         
WHERE CustomerID = 1000 ;        
SELECT @var1 AS 'Company Name';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Company Name  
 ------------------------------  
 Generic Name  
 ```  
  
### <a name="b-use-select-local_variable-to-return-null"></a>Б. Используйте инструкцию SELECT @local_variable для возвращения значения NULL  
 В следующем примере вложенный запрос используется для присвоения значения `@var1`. Так как значение, заданное для `CustomerID`, не существует, вложенный запрос не возвращает значение и переменная принимает значение `NULL`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @var1 VARCHAR(30)   
SELECT @var1 = 'Generic Name'   
SELECT @var1 = (SELECT Name   
FROM Sales.Store   
WHERE CustomerID = 1000)   
SELECT @var1 AS 'Company Name' ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Company Name  
----------------------------  
NULL  
```  
  
## <a name="see-also"></a>См. также:  
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Составные операторы (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)  
