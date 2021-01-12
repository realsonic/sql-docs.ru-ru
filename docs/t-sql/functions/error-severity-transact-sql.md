---
description: ERROR_SEVERITY (Transact-SQL)
title: ERROR_SEVERITY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ERROR_SEVERITY_TSQL
- ERROR_SEVERITY
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], severity
- messages [SQL Server], severity
- TRY...CATCH [SQL Server]
- CATCH block
- ERROR_SEVERITY function
ms.assetid: 50228f2f-6949-4d2e-8e43-fad11bf973ab
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b23ff01d4ae18fef4ab6092cf19a91b461e98526
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093634"
---
# <a name="error_severity-transact-sql"></a>ERROR_SEVERITY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает значение, указывающее на уровень серьезности ошибки, которая вызвала выполнение блока CATCH конструкции TRY...CATCH.  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
ERROR_SEVERITY ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 **int**  
  
## <a name="return-value"></a>Возвращаемое значение  
При вызове в блоке CATCH, в котором произошла ошибка, функция `ERROR_SEVERITY` возвращает значение серьезности ошибки, вызвавшей выполнение блока `CATCH`.  

Функция `ERROR_SEVERITY` возвращает значение NULL в случае вызова вне блока CATCH.  
  
## <a name="remarks"></a>Комментарии  
Функцию `ERROR_SEVERITY` можно вызывать в любом месте области действия блока CATCH.  
  
Функция `ERROR_SEVERITY` возвращает значение серьезности ошибки независимо от количества ее выполнений или от места ее вызова в области действия блока `CATCH`. В этом ее отличие от таких функций, как @@ERROR, которые возвращают номер ошибки только в той инструкции, которая непосредственно следует за инструкцией, вызвавшей ошибку.  
  
Функция `ERROR_SEVERITY`, как правило, работает во вложенном блоке `CATCH`. Функция `ERROR_SEVERITY` возвращает значение серьезности ошибки, соответствующее области действия блока `CATCH`, который ссылался на данный блок `CATCH`. Например, блок `CATCH` внешней конструкции TRY...CATCH может содержать внутреннюю конструкцию `TRY...CATCH`. Во внутреннем блоке `CATCH` функция `ERROR_SEVERITY` возвращает значение серьезности ошибки, вызвавшей внутренний блок `CATCH`. Если функция `ERROR_SEVERITY` выполняется во внешнем блоке `CATCH`, она возвращает значение серьезности ошибки, вызвавшей внешний блок `CATCH`.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-error_severity-in-a-catch-block"></a>A. Использование ERROR_SEVERITY в блоке CATCH  
В приведенном ниже примере показана хранимая процедура, которая создает ошибку деления на 0. Функция `ERROR_SEVERITY` возвращает значение серьезности этой ошибки.  

```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_SEVERITY() AS ErrorSeverity;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorSeverity
-------------
16

(1 row(s) affected)

```  
  
### <a name="b-using-error_severity-in-a-catch-block-with-other-error-handling-tools"></a>Б. Использование ERROR_SEVERITY в блоке CATCH с другими инструментами обработки ошибок  
В приведенном ниже примере показана инструкция `SELECT`, вызывающая ошибку деления на ноль. Хранимая процедура возвращает сведения об этой ошибке.  

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
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure  ErrorLine   ErrorMessage
----------- ------------- ----------- --------------- ----------- ----------------------------------
8134        16            1           NULL            4           Divide by zero error encountered.

(1 row(s) affected)

```  
  
## <a name="see-also"></a>См. также:  
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_STATE (Transact-SQL)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
 [Справочник по ошибкам и событиям (компонент Database Engine)](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

