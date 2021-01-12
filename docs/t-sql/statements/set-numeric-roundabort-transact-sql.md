---
description: SET NUMERIC_ROUNDABORT (Transact-SQL)
title: SET NUMERIC_ROUNDABORT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NUMERIC_ROUNDABORT
- SET_NUMERIC_ROUNDABORT_TSQL
- SET NUMERIC_ROUNDABORT
- NUMERIC_ROUNDABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- rounding expressions
- precision [SQL Server], rounded expressions
- expressions [SQL Server], rounding
- NUMERIC_ROUNDABORT
- SET NUMERIC_ROUNDABORT statement
ms.assetid: d20e74f1-b8da-466c-b180-9d8a8b851a77
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6cac574403a39550d7f838544a9d88b42f8f9fdf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100757"
---
# <a name="set-numeric_roundabort-transact-sql"></a>SET NUMERIC_ROUNDABORT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Указывает уровень детализации отчетов об ошибках, которые формируются при потере точности во время округления.  
  
![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Синтаксис

```syntaxsql

SET NUMERIC_ROUNDABORT { ON | OFF }
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
Если параметр SET NUMERIC_ROUNDABORT имеет значение ON, после потери точности в выражении формируется ошибка. Если задано значение OFF, потеря точности не приводит к сообщениям об ошибках. Результат округляется с точностью столбца или переменной, в которых сохраняется результат.  
  
Потеря точности происходит, когда выполняется попытка сохранения значения с фиксированной точностью в столбце или переменной с меньшей точностью.  
  
Если параметру SET NUMERIC_ROUNDABORT присвоено значение ON, параметр SET ARITHABORT определяет серьезность формируемой ошибки. В следующей таблице показано влияние этих двух параметров на сообщения об ошибках при потере точности.  
  
|Параметр|SET NUMERIC_ROUNDABORT ON|SET NUMERIC_ROUNDABORT OFF|
|-------------|--------------------------------|---------------------------------|
|SET ARITHABORT ON|Формируется ошибка; результирующий набор не возвращается.|Ошибок и предупреждений нет; результат округлен.|  
|SET ARITHABORT OFF|Создается предупреждение; выражение возвращает NULL.|Ошибок и предупреждений нет; результат округлен.|  

Значение параметра SET NUMERIC_ROUNDABORT задается на этапе выполнения или запуска, но не на этапе синтаксического анализа.

При создании или изменении индексов вычисляемых столбцов или индексированных представлений параметр SET NUMERIC_ROUNDABORT должен принимать значение OFF. Если значение параметра SET NUMERIC_ROUNDABORT будет равно ON, применение следующих инструкций к таблицам с индексами на вычисляемых столбцах или вычисляемым представлениям завершится ошибкой.

- CREATE 
- UPDATE 
- INSERT 
- DELETE 

Дополнительные сведения о настройке параметров SET с индексированными представлениями и индексами на вычисляемых столбцах см. в подразделе [Анализ использования инструкций SET](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).
  
Чтобы просмотреть текущее значение для этого параметра, выполните следующий запрос:
  
```sql
DECLARE @NUMERIC_ROUNDABORT VARCHAR(3) = 'OFF';  
IF ( (8192 & @@OPTIONS) = 8192 ) SET @NUMERIC_ROUNDABORT = 'ON';  
SELECT @NUMERIC_ROUNDABORT AS NUMERIC_ROUNDABORT;  
  
```  
  
## <a name="permissions"></a>Разрешения  
Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
Следующий пример показывает два значения, которые являются точными до четырех знаков после запятой. Они складываются и сохраняются в переменную, которая является точной до двух десятичных разрядов. Выражения демонстрируют влияние различных значений параметров `SET NUMERIC_ROUNDABORT` и `SET ARITHABORT`.  
  
```sql
-- SET NOCOUNT to ON,   
-- SET NUMERIC_ROUNDABORT to ON, and SET ARITHABORT to ON.  
SET NOCOUNT ON;  
PRINT 'SET NUMERIC_ROUNDABORT ON';  
PRINT 'SET ARITHABORT ON';  
SET NUMERIC_ROUNDABORT ON;  
SET ARITHABORT ON;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to ON and SET ARITHABORT to OFF.  
PRINT 'SET NUMERIC_ROUNDABORT ON';  
PRINT 'SET ARITHABORT OFF';  
SET NUMERIC_ROUNDABORT ON;  
SET ARITHABORT OFF;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to OFF and SET ARITHABORT to ON.  
PRINT 'SET NUMERIC_ROUNDABORT OFF';  
PRINT 'SET ARITHABORT ON';  
SET NUMERIC_ROUNDABORT OFF;  
SET ARITHABORT ON;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to OFF and SET ARITHABORT to OFF.  
PRINT 'SET NUMERIC_ROUNDABORT OFF';  
PRINT 'SET ARITHABORT OFF';  
SET NUMERIC_ROUNDABORT OFF;  
SET ARITHABORT OFF;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
[Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
[SET ARITHABORT (Transact-SQL)](../../t-sql/statements/set-arithabort-transact-sql.md)  
  
  
