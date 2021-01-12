---
description: SET ANSI_NULL_DFLT_ON (Transact-SQL)
title: SET ANSI_NULL_DFLT_ON (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ANSI_NULL_DFLT_ON
- ANSI_NULL_DFLT_ON_TSQL
- SET ANSI_NULL_DFLT_ON
- SET_ANSI_NULL_DFLT_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULL_DFLT_ON statement
- ANSI_NULL_DFLT_ON option
- default nullability
- null values [SQL Server], overriding
- overriding default nullability
ms.assetid: 8c925924-a466-4c8b-aeb2-7e0d341f32db
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39854417675cc4a7ee767c27918ae16c070c9ef5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100786"
---
# <a name="set-ansi_null_dflt_on-transact-sql"></a>SET ANSI_NULL_DFLT_ON (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Изменяет поведение сеанса для переопределения установленной по умолчанию допустимости значений NULL для новых столбцов, если значение параметра **ANSI null default** для базы данных — **false**. Дополнительные сведения об установке значения параметра **ANSI null default** см. в разделе [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database

SET ANSI_NULL_DFLT_ON {ON | OFF}
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ANSI_NULL_DFLT_ON ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
 Данный параметр определяет допустимость значений NULL в новых столбцах только в случае, если допустимость значений NULL не указана в инструкциях CREATE TABLE и ALTER TABLE. Если значение SET ANSI_NULL_DFLT_ON равно ON, для новых, создаваемых с помощью инструкций ALTER TABLE и CREATE TABLE столбцов будут разрешены значения NULL, если для такого столбца явно не задана допустимость значений NULL. Параметр SET ANSI_NULL_DFLT_ON не влияет на столбцы, создаваемые с явным значением NULL или NOT NULL.  
  
 Оба параметра SET ANSI_NULL_DFLT_OFF и SET ANSI_NULL_DFLT_ON не могут быть установлены в ON одновременно. Если один параметр установлен в ON, то другой установлен в OFF. Поэтому можно установить в ON либо ANSI_NULL_DFLT_OFF, либо ANSI_NULL_DFLT_ON, или же установить оба параметра в OFF. Если один из параметров установлен в ON, применяется соответствующая установка (SET ANSI_NULL_DFLT_OFF или SET ANSI_NULL_DFLT_ON). Если оба параметра имеют значение OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует значение столбца **is_ansi_null_default_on** в представлении каталога [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Для более надежного выполнения скриптов на языке [!INCLUDE[tsql](../../includes/tsql-md.md)], используемых в базах данных с различными настройками допустимости значений NULL, лучше указывать свойства NULL или NOT NULL с помощью инструкций CREATE TABLE и ALTER TABLE.  
  
 При соединении с драйвером ODBC для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или поставщика OLE DB для Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметр ANSI_NULL_DFLT_ON автоматически устанавливается в ON. Значение SET ANSI_NULL_DFLT_ON для соединений из приложений DB-Library по умолчанию равно OFF.  
  
 Если значение SET ANSI_DEFAULTS равно ON, включается SET ANSI_NULL_DFLT_ON.  
  
 Параметр SET ANSI_NULL_DFLT_ON устанавливается во время выполнения или запуска, но не во время синтаксического анализа.  
  
 Настройка SET ANSI_NULL_DFLT_ON не используется при создании таблиц с помощью инструкции SELECT INTO.  
  
 Чтобы просмотреть текущее значение для этого параметра, выполните следующий запрос.  
  
```sql  
DECLARE @ANSI_NULL_DFLT_ON VARCHAR(3) = 'OFF';  
IF ( (1024 & @@OPTIONS) = 1024 ) SET @ANSI_NULL_DFLT_ON = 'ON';  
SELECT @ANSI_NULL_DFLT_ON AS ANSI_NULL_DFLT_ON;  
  
```  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показаны результаты применения `SET ANSI_NULL_DFLT_ON` с обоими значениями для параметра базы данных **ANSI null default**.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON  
-- has an effect when the 'ANSI null default' for the database is false.  
-- Set the 'ANSI null default' database option to false by executing  
-- ALTER DATABASE.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT OFF;  
GO  
-- Create table t1.  
CREATE TABLE t1 (a TINYINT) ;  
GO   
-- NULL INSERT should fail.  
INSERT INTO t1 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t2.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t2 (a TINYINT);  
GO   
-- NULL insert should succeed.  
INSERT INTO t2 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t3.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t3 (a TINYINT);  
GO  
-- NULL insert should fail.  
INSERT INTO t3 (a) VALUES (NULL);  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON   
-- has no effect when the 'ANSI null default' for the database is true.  
-- Set the 'ANSI null default' database option to true.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON  
GO  
  
-- Create table t4.  
CREATE TABLE t4 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t4 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t5.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t5 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t5 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t6.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t6 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t6 (a) VALUES (NULL);  
GO  
  
-- Set the 'ANSI null default' database option to false.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON;  
GO  
  
-- Drop tables t1 through t6.  
DROP TABLE t1,t2,t3,t4,t5,t6;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [SET ANSI_NULL_DFLT_OFF (Transact-SQL)](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)  
  
  
