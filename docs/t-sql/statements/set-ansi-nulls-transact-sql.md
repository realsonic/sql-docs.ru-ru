---
description: SET ANSI_NULLS (Transact-SQL)
title: SET ANSI_NULLS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/24/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_ANSI_NULLS_TSQL
- ANSI_NULLS
- SET ANSI_NULLS
- ANSI_NULLS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULLS statement
- not equal to operator (<>)
- ANSI_NULLS option
- equals operator (=)
- null values [SQL Server], comparison operators
- comparison operators [SQL Server], null values
ms.assetid: aae263ef-a3c7-4dae-80c2-cc901e48c755
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current || azuresqldb-current'
ms.openlocfilehash: c2d152ce66276575de5fb299cda2e54ab0c37a43
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102352"
---
# <a name="set-ansi_nulls-transact-sql"></a>SET ANSI_NULLS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Задает совместимое со стандартом ISO поведение операторов сравнения "равно" (=) и "не равно" (<>) при их использовании со значениями NULL в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Синтаксис

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Синтаксис для [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] и [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_NULLS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Синтаксис для [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] и [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_NULLS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
Если параметру ANSI_NULLS присвоено значение ON, инструкция SELECT, использующая предложение WHERE *имя_столбца* = **NULL**, не вернет ни одной строки, даже если в столбце *имя_столбца* есть значения NULL. Инструкция SELECT, использующая предложение WHERE *column_name* <> **NULL**, не вернет ни одной строки, даже если в столбце *column_name* есть значения, отличные от NULL.  
  
Если параметр ANSI_NULLS имеет значение OFF, операторы "равно" (=) и "не равно" (<>) не следуют стандарту ISO. Инструкция SELECT, использующая предложение WHERE *column_name* = **NULL**, вернет строки, имеющие значения NULL, в столбце *column_name*. Инструкция SELECT, использующая предложение WHERE *column_name* <> **NULL**, вернет строки, имеющие значения, отличные от NULL, в столбце. Также любая инструкция SELECT, использующая предложение WHERE *column_name* <> *XYZ_value*, возвращает все строки со значениями, не равными *XYZ_value* и не равными NULL.  
  
Если параметр ANSI_NULLS равен ON, все сравнения со значением NULL возвращают значение UNKNOWN. Когда SET ANSI_NULLS равняется OFF, сравнение любых значений с NULL вернет TRUE только в том случае, если сравниваемое значение тоже NULL. Если параметр SET ANSI_NULLS не указан, применяется значение параметра ANSI_NULLS текущей базы данных. Дополнительные сведения о параметре базы данных ANSI_NULLS см. в разделе [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md).  

В следующей таблице показано влияние значения параметра ANSI_NULLS на результаты нескольких логических выражений с использованием значений NULL и значений, отличных от NULL.  
  
|Логическое выражение|SET ANSI_NULLS ON|SET ANSI_NULLS OFF|  
|---------------|---------------|------------|  
|NULL = NULL|UNKNOWN|TRUE|  
|1 = NULL|UNKNOWN|FALSE|  
|NULL <> NULL|UNKNOWN|FALSE|  
|1 <> NULL|UNKNOWN|TRUE|  
|NULL > NULL|UNKNOWN|UNKNOWN|  
|1 > NULL|UNKNOWN|UNKNOWN|  
|NULL IS NULL|TRUE|TRUE|  
|1 IS NULL|FALSE|FALSE|  
|NULL IS NOT NULL|FALSE|FALSE|  
|1 IS NOT NULL|TRUE|TRUE|  

Директива SET ANSI_NULLS ON влияет только на сравнения, в которых в качестве одного из операндов используется NULL в виде переменной или литеральной константы. Если оба операнда представляют собой столбцы или составные выражения, эта настройка не влияет на результат сравнения.  
  
Чтобы скрипт работал в соответствии с первоначальным замыслом, вне зависимости от параметра базы данных ANSI NULLS или настроек SET ANSI_NULLS, в сравнениях, которые могут содержать значения NULL, следует использовать выражения IS NULL и IS NOT NULL.  
  
Параметр ANSI_NULLS должен иметь значение ON для выполнения распределенных запросов.  
  
Также параметр ANSI_NULLS должен иметь значение ON при создании или изменении индексов вычисляемых столбцов или индексированных представлений. Если SET ANSI_NULLS равно OFF, то при работе с таблицами, содержащими индексы вычисляемых столбцов, а также при работе с индексированными представлениями инструкции CREATE, UPDATE, INSERT и DELETE завершатся неудачно. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сообщение об ошибке с перечислением всех недопустимых аргументов инструкции SET. Также при вызове инструкции SELECT в случае, если значение SET ANSI_NULLS равно OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не обрабатывает значения индексов вычисляемых столбцов или представлений и произведет выборку, словно этих индексов не существовало.  
  
> [!NOTE]  
> ANSI_NULLS является одним из семи параметров директивы SET, которые должны быть установлены определенным образом при работе с вычисляемыми столбцами или индексированными представлениями. Параметрам `ANSI_PADDING`, `ANSI_WARNINGS`, `ARITHABORT`, `QUOTED_IDENTIFIER` и `CONCAT_NULL_YIELDS_NULL` также должно быть присвоено значение ON, а параметру `NUMERIC_ROUNDABORT` — значение OFF.  
  
 При соединении с драйвером ODBC для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или поставщика OLE DB для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметру ANSI_NULLS автоматически задается значение ON. Этот параметр может быть настроен в источниках данных ODBC, в атрибутах соединения ODBC или свойствах соединения OLE DB, установленных в приложении перед подключением к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. По умолчанию значение SET ANSI_NULLS равно OFF.  
  
Если параметр ANSI_DEFAULTS установлен в значение ON, параметр ANSI_NULLS также включается.  
  
Значение ANSI_NULLS определяется во время выполнения, а не во время синтаксического анализа.  
  
Чтобы просмотреть текущее значение для этого параметра, выполните следующий запрос:
  
```sql  
DECLARE @ANSI_NULLS VARCHAR(3) = 'OFF';  
IF ( (32 & @@OPTIONS) = 32 ) SET @ANSI_NULLS = 'ON';  
SELECT @ANSI_NULLS AS ANSI_NULLS;   
```  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере операторы сравнения Equals (`=`) Not Equal To (`<>`) используются для сравнения со значениями в таблице, которые равны или не равны `NULL`. Этот пример также демонстрирует, что использование конструкции `IS NULL` не зависит от значения параметра `SET ANSI_NULLS`.  
  
```sql  
-- Create table t1 and insert values.  
CREATE TABLE dbo.t1 (a INT NULL);  
INSERT INTO dbo.t1 values (NULL),(0),(1);  
GO  
  
-- Print message and perform SELECT statements.  
PRINT 'Testing default setting';  
DECLARE @varname int;   
SET @varname = NULL;  
  
SELECT a  
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO 
```

Теперь установите параметр ANSI_NULLS в значение ON и выполните тестирование.

```sql
PRINT 'Testing ANSI_NULLS ON';  
SET ANSI_NULLS ON;  
GO  
DECLARE @varname int;  
SET @varname = NULL  
  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
```

Теперь установите параметр ANSI_NULLS в значение OFF и выполните тестирование.  

```sql
PRINT 'Testing ANSI_NULLS OFF';  
SET ANSI_NULLS OFF;  
GO  
DECLARE @varname int;  
SET @varname = NULL;  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- Drop table t1.  
DROP TABLE dbo.t1;  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY (Transact-SQL)](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [= (равно) (Transact-SQL)](../../t-sql/language-elements/equals-transact-sql.md)   
 [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md)   
 [<> (не равно) (Transact-SQL)](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)   
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
