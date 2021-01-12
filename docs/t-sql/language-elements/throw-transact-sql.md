---
description: THROW (Transact-SQL)
title: THROW (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- THROW_TSQL
- THROW
dev_langs:
- TSQL
helpviewer_keywords:
- THROW statement
ms.assetid: 43661b89-8f13-4480-ad53-70306cbb14c5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a1ad3ea0844911d5059cc2445c2978c3866f2916
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100305"
---
# <a name="throw-transact-sql"></a>THROW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Вызывает исключение и передает выполнение блоку CATCH конструкции TRY...CATCH в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
THROW [ { error_number | @local_variable },  
        { message | @local_variable },  
        { state | @local_variable } ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *error_number*  
 Константа или переменная, представляющая исключение. Аргумент *error_number* имеет тип **int**, должен иметь значение не меньше 50 000 и не больше 2 147 483 647.  
  
 *message*  
 Строка или переменная, описывающая исключение. Аргумент *message* имеет тип **nvarchar(2048)** .  
  
 *state*  
 Константа или переменная со значением в диапазоне от 0 до 255, указывающие состояние, которое должно быть связано с сообщением. Аргумент *state* имеет тип **tinyint**.  
  
## <a name="remarks"></a>Remarks  
 В инструкции, выполняемой до инструкции THROW, должен использоваться признак конца инструкции — точка с запятой (;).  
  
 Если конструкция TRY...CATCH недоступна, то пакет инструкций завершается. Задаются номер строки и процедура, где вызывается исключение. Серьезности задается значение 16.  
  
 Если инструкция THROW указана без параметров, то она должна находиться внутри блока CATCH. Результатом этого будет вызов возникшего исключения. Любая ошибка, возникающая в инструкции THROW, приводит к завершению пакета инструкций.  
  
 % является зарезервированным символом в тексте сообщения инструкции THROW, и его необходимо экранировать. Дважды укажите знак %, чтобы получить % в тексте сообщения, например: "Увеличение превышает 15 %% исходного значения."  
  
## <a name="differences-between-raiserror-and-throw"></a>Различия между инструкциями RAISERROR и THROW  
 В следующей таблице приведены некоторые различия между инструкциями RAISERROR и THROW.  
  
|RAISERROR, инструкция|Инструкция THROW|  
|-------------------------|---------------------|  
|Если инструкции RAISERROR передается параметр *msg_id*, то идентификатор должен быть задан в sys.messages.|Параметр *error_number* не требуется определять в sys.messages.|  
|Параметр *msg_str* может содержать стили форматирования **printf**.|Параметр *message* не принимает форматирование стиля **printf**.|  
|Параметр *severity* указывает серьезность исключения.|Параметр *severity* отсутствует. Если для создания исключения используется THROW, уровень серьезности всегда равен 16. Однако при использовании THROW для повторного создания существующего исключения для уровня серьезности устанавливается уровень серьезности этого исключения.|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-throw-to-raise-an-exception"></a>A. Использование инструкции THROW для вызова исключения  
 В следующем примере показано использование инструкции `THROW` для вызова исключения.  
  
```sql  
THROW 51000, 'The record does not exist.', 1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 51000, Level 16, State 1, Line 1  
  
 The record does not exist.
 ```  
  
### <a name="b-using-throw-to-raise-an-exception-again"></a>Б. Использование инструкции THROW для повторного вызова исключения  
 В следующем примере показано использование инструкции `THROW` для повторного вызова последнего исключения.  
  
```sql  
USE tempdb;  
GO  
CREATE TABLE dbo.TestRethrow  
(    ID INT PRIMARY KEY  
);  
BEGIN TRY  
    INSERT dbo.TestRethrow(ID) VALUES(1);  
--  Force error 2627, Violation of PRIMARY KEY constraint to be raised.  
    INSERT dbo.TestRethrow(ID) VALUES(1);  
END TRY  
BEGIN CATCH  
  
    PRINT 'In catch block.';  
    THROW;  
END CATCH;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 In catch block. 
 Msg 2627, Level 14, State 1, Line 1  
 Violation of PRIMARY KEY constraint 'PK__TestReth__3214EC272E3BD7D3'. Cannot insert duplicate key in object 'dbo.TestRethrow'.  
 The statement has been terminated.
 ```  
  
### <a name="c-using-formatmessage-with-throw"></a>В. Использование инструкции FORMATMESSAGE с ключевым словом THROW  
 В следующем примере показано использование функции `FORMATMESSAGE` с ключевым словом `THROW` для вызова настроенных сообщений об ошибке. В данном примере сначала создается определяемое пользователем сообщение об ошибке с помощью `sp_addmessage`. Так как инструкция THROW не позволяет задать параметры подстановки для параметра *message* так, как это делает инструкция RAISERROR, для передачи трех значений параметров, ожидаемых при сообщении об ошибке 60000, используется функция FORMATMESSAGE.  
  
```sql  
EXEC sys.sp_addmessage  
     @msgnum   = 60000  
    ,@severity = 16  
    ,@msgtext  = N'This is a test message with one numeric parameter (%d), one string parameter (%s), and another string parameter (%s).'  
    ,@lang = 'us_english';   
GO  
  
DECLARE @msg NVARCHAR(2048) = FORMATMESSAGE(60000, 500, N'First string', N'second string');   
  
THROW 60000, @msg, 1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 60000, Level 16, State 1, Line 2  
 This is a test message with one numeric parameter (500), one string parameter (First string), and another string parameter (second string).
 ```  
  
## <a name="see-also"></a>См. также:  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)   
 [Степени серьезности ошибок ядра СУБД](../../relational-databases/errors-events/database-engine-error-severities.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)   
 [GOTO &#40;Transact-SQL&#41;](../../t-sql/language-elements/goto-transact-sql.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md)   
 [SET XACT_ABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-xact-abort-transact-sql.md)  
  
  

