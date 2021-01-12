---
description: SET QUOTED_IDENTIFIER (Transact-SQL)
title: SET QUOTED_IDENTIFIER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- QUOTED_IDENTIFIER_TSQL
- SET_QUOTED_IDENTIFIER_TSQL
- SET QUOTED_IDENTIFIER
- QUOTED_IDENTIFIER
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- identifiers [SQL Server], delimited
- QUOTED_IDENTIFIER option
- quoted identifiers
- ISO delimited identifiers rules
- SET QUOTED_IDENTIFIER statement
ms.assetid: 10f66b71-9241-4a3a-9292-455ae7252565
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 232c287564593eee0deab4ae37dc13c36d2d689b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100775"
---
# <a name="set-quoted_identifier-transact-sql"></a>SET QUOTED_IDENTIFIER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Заставляет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следовать правилам ISO относительно разделения кавычками идентификаторов и строк-литералов. Идентификаторы, заключенные в двойные кавычки, могут быть либо зарезервированными ключевыми словами [!INCLUDE[tsql](../../includes/tsql-md.md)], либо могут содержать символы, которые обычно запрещены правилами синтаксиса для идентификаторов [!INCLUDE[tsql](../../includes/tsql-md.md)].

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and serverless SQL pool in Azure Synapse Analytics

SET QUOTED_IDENTIFIER { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET QUOTED_IDENTIFIER ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
Когда `SET QUOTED_IDENTIFIER` включен (ON) (по умолчанию), идентификаторы можно отделять двойными кавычками (" "), а литералы должны быть отделены одинарными кавычками (' '). Все строки, находящиеся в двойных кавычках, интерпретируются как идентификаторы объектов. Такие образом, заключенные в кавычки идентификаторы не должны удовлетворять правилам языка [!INCLUDE[tsql](../../includes/tsql-md.md)] для идентификаторов. Они могут быть зарезервированными ключевыми словами, а также могут содержать символы, обычно запрещенные для идентификаторов языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Выражения со строками-литералами нельзя заключать в двойные кавычки; для этих целей необходимо использовать одинарные кавычки. Если одинарная кавычка (') является частью строки литерала, то она может быть представлена двумя одинарными кавычками (''). Если в именах объектов базы данных используются зарезервированные ключевые слова, то параметру `SET QUOTED_IDENTIFIER` должно быть присвоено значение ON.

Если параметру `SET QUOTED_IDENTIFIER` присвоено значение OFF, то идентификаторы невозможно заключать в кавычки и следует соблюдать все правила [!INCLUDE[tsql](../../includes/tsql-md.md)] для идентификаторов. Дополнительные сведения см. в разделе [Идентификаторы баз данных](../../relational-databases/databases/database-identifiers.md). Литералы могут разделяться как одинарными, так и двойными кавычками. Если строки-литералы разделяются двойными кавычками, то в строке могут содержаться внедренные одинарные кавычки, такие как апострофы.

> [!NOTE]
> QUOTED_IDENTIFIER не влияет на идентификаторы с разделителем, заключенные в квадратные скобки ([ ]).

Параметр `SET QUOTED_IDENTIFIER` должен иметь значение ON при создании или изменении индексов по вычисляемым столбцам или индексированным представлениям. Если для параметра `SET QUOTED_IDENTIFIER` установлено значение OFF, то выполнение инструкций CREATE, UPDATE, INSERT и DELETE для таблиц с индексами, основанными на вычисляемых столбцах или индексированных представлениях, будет завершаться ошибкой. Дополнительные сведения о настройке параметров SET с индексированными представлениями и индексами в вычисляемых столбцах см. в разделе [Анализ использования инструкций SET](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).

Параметр `SET QUOTED_IDENTIFIER` должен иметь значение ON при создании отфильтрованного индекса.

Параметр `SET QUOTED_IDENTIFIER` должен иметь значение ON при вызове методов типа данных XML.

При соединении с драйвером Native Client OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и поставщиком Native Client OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметр QUOTED_IDENTIFIER автоматически устанавливается в значение ON. Это может быть настроено в источниках данных ODBC, в атрибутах соединения ODBC или свойствах соединения OLE DB. По умолчанию параметр SET QUOTED_IDENTIFIER имеет значение OFF для соединений из приложений DB-Library.

После создания таблицы параметр QUOTED IDENTIFIER всегда записывается в метаданные таблицы со значением ON, даже если он был установлен в OFF при создании таблицы.

Когда создается хранимая процедура, параметры SET QUOTED_IDENTIFIER и SET ANSI_NULLS фиксируются и используются для последующих вызовов этой хранимой процедуры.

При выполнении операций внутри хранимой процедуры значение SET QUOTED_IDENTIFIER не меняется.

Если для `SET ANSI_DEFAULTS` задано значение ON, для QUOTED_IDENTIFIER также задано значение ON.

Также параметр `SET QUOTED_IDENTIFIER` связан с параметром QUOTED_IDENTIFIER инструкции [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

`SET QUOTED_IDENTIFIER` применяется во время синтаксического анализа [!INCLUDE[tsql](../../includes/tsql-md.md)] и влияет только на анализ, а не на выполнение или оптимизацию запроса.

При анализе неструктурированных пакетов верхнего уровня используется текущий параметр сеанса для QUOTED_IDENTIFIER. При анализе пакета все вхождения `SET QUOTED_IDENTIFIER` приведут к изменению поведения анализа с этой точки и сохранят этот параметр для сеанса. Поэтому после анализа и выполнения пакета параметр QUOTED_IDENTIFER сеанса будет задан в соответствии с последним вхождением `SET QUOTED_IDENTIFIER` в пакете.

Статический [!INCLUDE[tsql](../../includes/tsql-md.md)] в хранимой процедуре анализируется с использованием параметра QUOTED_IDENTIFIER, действующего для пакета, создавшего или изменившего хранимую процедуру. Параметр `SET QUOTED_IDENTIFIER` не работает, когда появляется в тексте хранимой процедуры в виде статического [!INCLUDE[tsql](../../includes/tsql-md.md)].

Анализ вложенного пакета с процедурой `sp_executesql` или `exec()` начинается с использованием параметра QUOTED_IDENTIFIER сеанса. Если вложенный пакет находится внутри хранимой процедуры, анализ начинается с использованием параметра QUOTED_IDENTIFIER хранимой процедуры. При анализе пакета все вхождения `SET QUOTED_IDENTIFIER` приведут к изменению поведения анализа с этой точки, но параметр QUOTED_IDENTIFIER сеанса останется без изменений.

Чтобы просмотреть текущее значение для этого параметра, выполните следующий запрос:

```sql
DECLARE @QUOTED_IDENTIFIER VARCHAR(3) = 'OFF';
IF ( (256 & @@OPTIONS) = 256 ) 
SET @QUOTED_IDENTIFIER = 'ON';

SELECT @QUOTED_IDENTIFIER AS QUOTED_IDENTIFIER;
```

## <a name="permissions"></a>Разрешения
Необходимо членство в роли `PUBLIC`.

## <a name="examples"></a>Примеры

### <a name="a-using-the-quoted-identifier-setting-and-reserved-word-object-names"></a>A. Использование режима заключенных в кавычки идентификаторов и имен объектов, состоящих из зарезервированных слов

В следующем примере показано, что параметр `SET QUOTED_IDENTIFIER` должен иметь значение `ON`, а ключевые слова в именах таблиц должны быть заключены в двойные кавычки, чтобы создать и использовать объекты, содержащие в именах зарезервированные ключевые слова.

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- Create statement fails.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Create statement succeeds.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SELECT "identity","order"
FROM "select"
ORDER BY "order";
GO

DROP TABLE "SELECT";
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

### <a name="b-using-the-quoted-identifier-setting-with-single-and-double-quotation-marks"></a>Б. Использование настроек заключенного в кавычки идентификатора с двойными и одинарными кавычками

 В этом примере показано, как используются двойные и одинарные кавычки в строковых выражениях, если параметру `SET QUOTED_IDENTIFIER` присвоено значение `ON` и значение `OFF`.

```sql
SET QUOTED_IDENTIFIER OFF;
GO

USE AdventureWorks2012;
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES
    WHERE TABLE_NAME = 'Test')
    DROP TABLE dbo.Test;
GO
USE AdventureWorks2012;
CREATE TABLE dbo.Test (ID INT, String VARCHAR(30)) ;
GO

-- Literal strings can be in single or double quotation marks.
INSERT INTO dbo.Test VALUES (1, "'Text in single quotes'");
INSERT INTO dbo.Test VALUES (2, '''Text in single quotes''');
INSERT INTO dbo.Test VALUES (3, 'Text with 2 '''' single quotes');
INSERT INTO dbo.Test VALUES (4, '"Text in double quotes"');
INSERT INTO dbo.Test VALUES (5, """Text in double quotes""");
INSERT INTO dbo.Test VALUES (6, "Text with 2 """" double quotes");
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Strings inside double quotation marks are now treated
-- as object names, so they cannot be used for literals.
INSERT INTO dbo."Test" VALUES (7, 'Text with a single '' quote');
GO

-- Object identifiers do not have to be in double quotation marks
-- if they are not reserved keywords.
SELECT ID, String
FROM dbo.Test;
GO

DROP TABLE dbo.Test;
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
 ID          String
 ----------- ------------------------------
 1           'Text in single quotes'
 2           'Text in single quotes'
 3           Text with 2 '' single quotes
 4           "Text in double quotes"
 5           "Text in double quotes"
 6           Text with 2 "" double quotes
 7           Text with a single ' quote
 ```

## <a name="see-also"></a>См. также:
[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)    
[CREATE DEFAULT](../../t-sql/statements/create-default-transact-sql.md)    
[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)    
[CREATE RULE](../../t-sql/statements/create-rule-transact-sql.md)    
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md)    
[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)    
[Data Types](../../t-sql/data-types/data-types-transact-sql.md)    
[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)    
[SELECT](../../t-sql/queries/select-transact-sql.md)    
[Инструкции SET](../../t-sql/statements/set-statements-transact-sql.md)    
[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)    
[sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)    
[Идентификаторы баз данных](../../relational-databases/databases/database-identifiers.md)
