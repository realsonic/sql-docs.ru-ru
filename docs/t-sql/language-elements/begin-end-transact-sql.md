---
description: BEGIN...END (Transact-SQL)
title: BEGIN...END (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BEGIN
- BEGIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- enclosing statements [SQL Server]
- BEGIN statement
- control-of-flow language [SQL Server], BEGIN...END statement
- BEGIN...END keyword
- grouping statements, BEGIN...END statement
- executing Transact-SQL statements together [SQL Server]
- statements [SQL Server], grouping
ms.assetid: fc2c7f76-f1f9-4f91-beef-bc8ef0da2feb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1dc4990bc19a23dc0759adf64bfa3c22cae324d4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100444"
---
# <a name="beginend-transact-sql"></a>BEGIN...END (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Включает в себя последовательность инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], позволяя выполнять группу инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ключевые слова BEGIN и END относятся к языку потока управления.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
BEGIN  
    { sql_statement | statement_block }   
END  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 { *sql_statement* | *statement_block* }  
 Любая допустимая инструкция или группа инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], определенная с помощью блока инструкций.  
  
## <a name="remarks"></a>Комментарии  
 Блоки BEGIN...END могут быть вложенными.  
  
 Хотя все инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] допустимы в пределах блока BEGIN...END, некоторые инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] не следует группировать в пределах одного пакета (блока инструкций).  
  
## <a name="examples"></a>Примеры  
 В следующем примере ключевые слова `BEGIN` и `END` определяют ряд инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], которые будут выполняться вместе. Если не включить блок `BEGIN...END`, будут выполнены оба оператора `ROLLBACK TRANSACTION` и возвращены оба сообщения `PRINT`.  
  
```sql
USE AdventureWorks2012
GO  
BEGIN TRANSACTION
GO  
IF @@TRANCOUNT = 0  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM Person.Person WHERE LastName = 'Adams'
    ROLLBACK TRANSACTION
    PRINT N'Rolling back the transaction two times would cause an error.'
END
ROLLBACK TRANSACTION
PRINT N'Rolled back the transaction.'
GO  
/*  
Rolled back the transaction.  
*/  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В приведенном ниже примере ключевые слова `BEGIN` и `END` определяют ряд инструкций языка [!INCLUDE[DWsql](../../includes/dwsql-md.md)], которые выполняются вместе. Если не включить блок `BEGIN...END`, в приведенном ниже примере образуется непрерывный цикл.  
  
```sql
-- Uses AdventureWorks  

DECLARE @Iteration Integer = 0  
WHILE @Iteration <10  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM dbo.DimCustomer WHERE LastName = 'Adams'
    SET @Iteration += 1  
END
```  
  
## <a name="see-also"></a>См. также  
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [END (BEGIN...END) (Transact-SQL)](../../t-sql/language-elements/end-begin-end-transact-sql.md)
