---
description: ERROR_STATE (Transact-SQL)
title: ERROR_STATE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ERROR_STATE_TSQL
- ERROR_STATE
dev_langs:
- TSQL
helpviewer_keywords:
- messages [SQL Server], state
- ERROR_STATE function
- errors [SQL Server], state
- TRY...CATCH [SQL Server]
- CATCH block
- states [SQL Server], error numbers
ms.assetid: 6059af00-83fe-409f-ab7c-daad111bc671
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0add98a5f8e05a9d0deddaf4bfa2428ba6b0ff08
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087956"
---
# <a name="error_state-transact-sql"></a>ERROR_STATE (Transact-SQL)

[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Возвращает номер состояния для ошибки, вызвавшей запуск блока CATCH в конструкции TRY…CATCH.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
ERROR_STATE ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 **int**  
  
## <a name="return-value"></a>Возвращаемое значение  
 При вызове в блоке CATCH возвращает номер состояния сообщения об ошибке, вызвавшей запуск блока CATCH.  
  
 Возвращает значение NULL в случае вызова вне блока CATCH.  
  
## <a name="remarks"></a>Комментарии  
 Некоторые сообщения об ошибках могут возникнуть в нескольких точках кода компонента [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Например, в нескольких различных ситуациях может возникнуть ошибка «1105». Каждому определенному условию, вызывающему ошибку, назначен уникальный код состояния.  
  
 При просмотре баз данных известных проблем, например базы знаний [!INCLUDE[msCoName](../../includes/msconame-md.md)], можно по номеру состояния выяснить, соответствует ли запись о проблеме той ошибке, которая возникла. Например, если в статье базы знаний рассматривается сообщение об ошибке 1105 с состоянием 2, а полученное сообщение об ошибке 1105 имеет состояние 3, то в этом случае ошибка произошла по иной причине, чем ошибка, описанная в статье.  
  
 Кроме того, сотрудник службы поддержки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может использовать код состояния ошибки для поиска в исходном коде места, где произошла ошибка, это поможет ему в диагностировании неполадки.  
  
 Функцию ERROR_STATE можно вызывать в любом месте области видимости блока CATCH.  
  
 Функция ERROR_STATE возвращает состояние ошибки вне зависимости от числа ее запусков или места запуска в области видимости блока CATCH. Это отличается от поведения таких функций, как @@ERROR, которые возвращают номер ошибки в инструкции лишь сразу после ее возникновения или в первой инструкции блока CATCH.  
  
 Во вложенных блоках CATCH функция ERROR_STATE возвращает состояние ошибки, относящееся к области видимости блока CATCH, в которой она вызвана. Например, блок CATCH внешней конструкции TRY...CATCH может содержать вложенную конструкцию TRY...CATCH. Во вложенном блоке CATCH функция ERROR_STATE возвращает состояние ошибки, вызвавшей вложенный блок CATCH. Если функция ERROR_STATE запускается во внешнем блоке CATCH, то она возвращает состояние ошибки, вызвавшей внешний блок CATCH.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-error_state-in-a-catch-block"></a>A. Использование функции ERROR_STATE в блоке CATCH  
 В следующем примере приведена инструкция `SELECT`, вызывающая ошибку деления на ноль. Возвращается состояние ошибки.  
  
```sql  
BEGIN TRY  
    -- Generate a divide by zero error  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_STATE() AS ErrorState;  
END CATCH;  
GO  
```  
  
### <a name="b-using-error_state-in-a-catch-block-with-other-error-handling-tools"></a>Б. Использование функции ERROR_STATE в блоке CATCH вместе с другими средствами обработки ошибок  
 В следующем примере приведена инструкция `SELECT`, вызывающая ошибку деления на ноль. Вместе с состоянием ошибки возвращаются сведения, относящиеся к этой ошибке.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-error_state-in-a-catch-block-with-other-error-handling-tools"></a>В. Использование функции ERROR_STATE в блоке CATCH вместе с другими средствами обработки ошибок  
 В следующем примере приведена инструкция `SELECT`, вызывающая ошибку деления на ноль. Вместе с состоянием ошибки возвращаются сведения, относящиеся к этой ошибке.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)    
 [Справочник по ошибкам и событиям (компонент Database Engine)](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

