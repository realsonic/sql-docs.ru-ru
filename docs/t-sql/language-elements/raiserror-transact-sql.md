---
description: RAISERROR (Transact-SQL)
title: RAISERROR (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RAISERROR
- RAISERROR_TSQL
- RAISEERROR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- errors [SQL Server], RAISERROR statement
- user-defined error messages [SQL Server]
- system flags
- generating errors [SQL Server]
- TRY block [SQL Server]
- recording errors
- ad hoc messages
- RAISERROR statement
- CATCH block
- messages [SQL Server], RAISERROR statement
ms.assetid: 483588bd-021b-4eae-b4ee-216268003e79
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6025177359ba8da3d2d330c88ec08a162f12b2d2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102477"
---
# <a name="raiserror-transact-sql"></a>RAISERROR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> Инструкция **RAISERROR** не учитывает **SET XACT_ABORT**. В новых приложениях следует использовать инструкцию **THROW** вместо **RAISERROR**.

  Создает сообщение об ошибке и запускает обработку ошибок для сеанса. Инструкция RAISERROR может либо ссылаться на определенное пользователем сообщение, находящееся в представлении каталога sys.messages, либо динамически создавать сообщение. Это сообщение возвращается как сообщение об ошибке сервера вызывающему приложению или соответствующему блоку CATCH конструкции TRY…CATCH. В новых же приложениях следует использовать инструкцию [THROW](../../t-sql/language-elements/throw-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
RAISERROR ( { msg_id | msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
RAISERROR ( { msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *msg_id*  
 Номер сообщения об ошибке, определенного пользователем, которое сохранено в представлении каталога sys.messages при помощи процедуры sp_addmessage. Номера пользовательских сообщений об ошибках должны быть больше 50 000. Если аргумент *msg_id* не определен, то инструкция RAISERROR создает сообщение об ошибке с номером ошибки 50000.  
  
 *msg_str*  
 Определенное пользователем сообщение с форматом, аналогичным формату функции **printf** из стандартной библиотеки языка С. Это сообщение об ошибке не должно содержать более 2 047 символов. Если сообщение содержит 2 048 и более символов, то отображаются только первые 2 044, а за ними появляется знак многоточия, показывающий, что сообщение было усечено. Обратите внимание, что параметры подстановки содержат больше символов, чем видно на выходе из-за внутренней структуры хранения. Например, если параметр подстановки *%d* имеет значение 2, он выводит один символ в строку сообщения, хотя при хранении занимает три дополнительных символа. Из-за этой особенности хранения количество доступных символов для выходного сообщения уменьшается.  
  
 Если аргумент *msg_str* определен, инструкция RAISERROR создает сообщение об ошибке с номером ошибки 50000.  
  
 Аргумент *msg_str* является символьной строкой, которая может содержать спецификации преобразования. Каждая спецификация преобразования определяет, каким образом значение из списка аргументов будет отформатировано и помещено в поле в местоположении спецификации преобразования в строке *msg_str*. Спецификации преобразования имеют следующий формат:  
  
 % [[*flag*] [*width*] [. *precision*] [{h | l}]] *type*  
  
 В строке *msg_str* могут использоваться следующие параметры:  
  
 *flag*  
  
 Код, определяющий промежутки и выравнивание подставляемого значения.  
  
|Код|Префикс или выравнивание|Описание|  
|----------|-----------------------------|-----------------|  
|- (знак «минус»)|Выравнивать слева|Выравнивает значение аргумента по левой границе поля заданной ширины.|  
|+ (знак «плюс»)|Префикс знака|Добавляет перед значением аргумента знак «плюс» (+) или «минус» (-), если значение принадлежит к типу со знаком.|  
|0 (ноль)|Дополнение нулями|Добавляет к выходному значению спереди нули до тех пор, пока не будет достигнута минимальная ширина. При одновременном указании 0 и знака «минус» (-) флаг 0 не учитывается.|  
|# (число)|Префикс 0x для шестнадцатеричного типа x или X|При использовании формата o, x или X флаг знака числа (#) предшествует любому ненулевому значению 0, 0x или 0X соответственно. Если флаг знака числа (#) стоит перед d, i или u, он пропускается.|  
|' ' (blank)|Заполнение пробелами|Добавляет к выходным значениям пробелы, если значение со знаком и положительно. Этот параметр не учитывается, если включается вместе с флагом знака «плюс» (+).|  
  
 *width*  
  
 Целое число, определяющее минимальную ширину поля, в которое помещается значение аргумента. Если длина значения аргумента равна значению параметра *width* или превышает его, то значение записывается без заполнения. Если значение короче, чем значение параметра *width*, оно дополняется до длины, определенной в параметре *width*.  
  
 Символ «звездочка» (*) означает, что ширина определяется соответствующим аргументом в списке аргументов, значение которого должно быть целым числом.  
  
 *precision*  
  
 Максимальное число символов, берущееся из значения аргумента для значения строки. Например, если в строке содержится пять символов, а точность равна 3, то для значения строки используются первые три символа.  
  
 Для целых значений аргумент *precision* определяет минимальное количество отображаемых цифр.  
  
 Символ «звездочка» (*) означает, что точность определяется соответствующим аргументом в списке аргументов, значение которого должно быть целым числом.  
  
 {h | l} *type*  
  
 Используется с типами символов d, i, o, s, x, X или u, создает значения типа данных **shortint** (h) или **longint** (l).  
  
|Спецификация типа|Представляет|  
|------------------------|----------------|  
|d или i|Целое число со знаком|  
|o|Восьмеричное число без знака|  
|s|Строка|  
|u|Целое число без знака|  
|x или X|Шестнадцатеричное число без знака|  
  
> [!NOTE]  
>  Эти спецификации типа основаны на изначально определенных доя функции **printf** в стандартной библиотеке C. Спецификации типов, используемые в строках сообщений инструкции RAISERROR, сопоставляются с типами данных языка [!INCLUDE[tsql](../../includes/tsql-md.md)], а спецификации, используемые в функции **printf**, сопоставляются с типами данных языка C. Спецификации типов, используемые в функции **printf**, не поддерживаются инструкцией RAISERROR, если в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] нет типов данных, схожих с соответствующими типами данных языка C. Например, спецификация *%p* для указателей не поддерживается инструкцией RAISERROR, так как в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] нет типа данных для указателей.  
  
> [!NOTE]  
>  Для преобразования какого-либо значения в тип данных [!INCLUDE[tsql](../../includes/tsql-md.md)] **bigint** нужно указать спецификацию **%I64d**.  
  
 *\@local_variable*  
 Переменная любого допустимого типа данных для символов, содержащая строку того же формата, что и строка *msg_str*. Аргумент *\@local_variable* должен иметь тип **char** или **varchar** либо должен неявно преобразовываться в эти типы данных.  
  
 *severity*  
 Определенная пользователем степень серьезности, связанный с этим сообщением. Если при помощи аргумента *msg_id* вызываются пользовательские сообщения, созданные процедурой sp_addmessage, уровень серьезности, указанный в инструкции RAISERROR, заменяет уровень серьезности, указанный в процедуре sp_addmessage.  
  
 Степень серьезности от 0 до 18 может указать любой пользователь. Уровни серьезности от 19 до 25 могут быть указаны только членами предопределенной роли сервера sysadmin и пользователями с разрешениями ALTER TRACE. Для степеней серьезности от 19 до 25 требуется параметр WITH LOG. Степени серьезности меньше 0 интерпретируются как 0. Степени серьезности больше 25 интерпретируются как 25.  
  
> [!CAUTION]  
>  Уровни серьезности от 20 до 25 считаются неустранимыми. Если обнаруживается неустранимый уровень серьезности, то после получения сообщения соединение с клиентом обрывается и регистрируется сообщение об ошибке в журналах приложений и ошибок.  
  
 Можно указать -1, чтобы получить степень серьезности, связанную с ошибкой, как показано в следующем примере.  
  
```sql  
RAISERROR (15600,-1,-1, 'mysp_CreateCustomer');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 15600, Level 15, State 1, Line 1   
 An invalid parameter or option was specified for procedure 'mysp_CreateCustomer'.
 ```  
  
 *state*  
 Целое число от 0 до 255. Для отрицательных значений по умолчанию используется 1. Значения больше 255 использовать не следует. 
  
 Если одна и та же пользовательская ошибка возникает в нескольких местах, то при помощи уникального номера состояния для каждого местоположения можно определить, в каком месте кода появилась ошибка.  
  
 *argument*  
 Параметры, использующиеся при подстановке для переменных, определенных в *msg_str*, или для сообщений, соответствующих аргументу *msg_id*. Число параметров подстановки может быть от 0 и более, при этом общее количество параметров подстановки не может превышать 20. Каждый параметр подстановки может быть локальной переменной или любым из этих типов данных: **tinyint**, **smallint**, **int**, **char**, **varchar**, **nchar**, **nvarchar**, **binary** или **varbinary**. Другие типы данных не поддерживаются.  
  
 *Параметр*  
 Настраиваемый параметр для ошибки может принимать одно из значений, находящихся в следующей таблице.  
  
|Значение|Описание|  
|-----------|-----------------|  
|LOG|Записывает сообщения об ошибках в журнал ошибок и журнал приложения экземпляра компонента [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Сообщения об ошибках в журнале ошибок ограничены размером в 440 байт. Только члены предопределенной роли сервера sysadmin или пользователи с разрешениями ALTER TRACE могут указывать ключевое слово WITH LOG.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|NOWAIT|Немедленно посылает сообщения клиенту.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|SETERROR|Устанавливает значения параметров @@ERROR и ERROR_NUMBER равными *msg_id* или 50000, независимо от уровня серьезности.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
  
## <a name="remarks"></a>Remarks  
 Ошибки, созданные инструкцией RAISERROR, аналогичны ошибкам, созданным кодом компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Значения, указанные в инструкции RAISERROR, выводятся системными функциями ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY, ERROR_STATE и @@ERROR. Если инструкция RAISERROR с уровнем серьезности 11 или выше выполняется в блоке TRY, управление передается соответствующему блоку CATCH. Ошибка возвращается вызывающему объекту, если инструкция RAISERROR вызывается:  
  
-   за пределами области любого блока TRY;  
  
-   с уровнем серьезности, равным 10 и менее в блоке TRY;  
  
-   с уровнем серьезности, равным 20 и выше, что приводит к обрыву подключения к базе данных.  
  
 В блоках CATCH инструкция RAISERROR может использоваться для передачи сообщения об ошибке, вызывающего блок CATCH, во время получения исходных сведений об ошибке при помощи таких системных функций, как ERROR_NUMBER и ERROR_MESSAGE. По умолчанию для сообщений с серьезностью от 1 до 10 параметр @@ERROR устанавливается в значение 0.  
  
 Если при помощи аргумента *msg_id* задано пользовательское сообщение, доступное в представлении каталога sys.messages, инструкция RAISERROR обрабатывает сообщение из текстового столбца при помощи тех же правил, которые применялись к тексту пользовательского сообщения, заданного аргументом *msg_str*. Текст пользовательского сообщения может содержать спецификации преобразования, а инструкция RAISERROR сопоставит значения аргументов данным спецификациям преобразования. Используйте процедуру sp_addmessage для добавления пользовательских сообщений об ошибках и процедуру sp_dropmessage для удаления пользовательских сообщений об ошибках.  
  
 Инструкция RAISERROR может использоваться в качестве альтернативы инструкции PRINT для возвращения сообщений вызывающим приложениям. Инструкция RAISERROR поддерживает функцию подстановки символов, сходную с функцией **printf** стандартной библиотеки языка С, которая отличается от инструкции PRINT [!INCLUDE[tsql](../../includes/tsql-md.md)]. Инструкция PRINT не зависит от блоков TRY, в то время как инструкция RAISERROR, выполняемая с уровнем серьезности от 11 до 19 в блоке TRY, передает управление процессом соответствующему блоку CATCH. Задайте уровень серьезности, равный 10 или меньше, чтобы инструкция RAISERROR возвращала сообщения из блока TRY без вызова блока CATCH.  
  
 Обычно последовательные аргументы заменяют последовательные спецификации преобразования; первый аргумент заменяет первую спецификацию преобразования, второй аргумент заменяет вторую спецификацию преобразования и так далее. Например, в следующей инструкции `RAISERROR` первый аргумент `N'number'` подставляется на место первой спецификации преобразования `%s`, а второй аргумент `5` — на место второй спецификации преобразования `%d.`.  
  
```sql  
RAISERROR (N'This is message %s %d.', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'number', -- First argument.  
           5); -- Second argument.  
-- The message text returned is: This is message number 5.  
GO  
```  
  
 Если в качестве ширины или точности спецификации преобразования задан символ «звездочка» (*), то используемое для ширины или для точности значение определяется как значение аргумента целого типа. В этом случае одна спецификация преобразования может использоваться до трех аргументов — для значений ширины, точности и подстановки.  
  
 Например, каждая из следующих инструкций `RAISERROR` возвращает одну и ту же строку. Одна определяет значения ширины и точности в списке аргументов, другая определяет их в спецификации преобразования.  
  
```sql  
RAISERROR (N'<\<%*.*s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           7, -- First argument used for width.  
           3, -- Second argument used for precision.  
           N'abcde'); -- Third argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
RAISERROR (N'<\<%7.3s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-error-information-from-a-catch-block"></a>A. Возвращение сведений об ошибке из блока CATCH  
 Следующий пример кода показывает, как можно использовать инструкцию `RAISERROR` внутри блока `TRY`, чтобы передать управление блоку `CATCH`. Также в этом примере показано, каким образом для возвращения сведений об ошибке используется инструкция `RAISERROR`, которая вызывает блок `CATCH`.  
  
> [!NOTE]  
>  Инструкция RAISERROR может формировать только ошибки с состоянием от 1 до 127 включительно. Так как компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] может вызывать ошибки с состоянием 0, рекомендуется проверять состояние ошибки, возвращаемое функцией ERROR_STATE, перед передачей его по значению в виде параметра состояния для инструкции RAISERROR.  
  
```sql  
BEGIN TRY  
    -- RAISERROR with severity 11-19 will cause execution to   
    -- jump to the CATCH block.  
    RAISERROR ('Error raised in TRY block.', -- Message text.  
               16, -- Severity.  
               1 -- State.  
               );  
END TRY  
BEGIN CATCH  
    DECLARE @ErrorMessage NVARCHAR(4000);  
    DECLARE @ErrorSeverity INT;  
    DECLARE @ErrorState INT;  
  
    SELECT   
        @ErrorMessage = ERROR_MESSAGE(),  
        @ErrorSeverity = ERROR_SEVERITY(),  
        @ErrorState = ERROR_STATE();  
  
    -- Use RAISERROR inside the CATCH block to return error  
    -- information about the original error that caused  
    -- execution to jump to the CATCH block.  
    RAISERROR (@ErrorMessage, -- Message text.  
               @ErrorSeverity, -- Severity.  
               @ErrorState -- State.  
               );  
END CATCH;  
```  
  
### <a name="b-creating-an-ad-hoc-message-in-sysmessages"></a>Б. Создание нерегламентированного сообщения в представлении каталога sys.messages  
 В следующем примере показано, как инициировать сообщение, хранящееся в представлении каталога sys.messages. Это сообщение было добавлено в представление каталога sys.messages при помощи системной хранимой процедуры `sp_addmessage` как сообщение с номером `50005`.  
  
```sql  
sp_addmessage @msgnum = 50005,  
              @severity = 10,  
              @msgtext = N'<\<%7.3s>>';  
GO  
RAISERROR (50005, -- Message id.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
sp_dropmessage @msgnum = 50005;  
GO  
```  
  
### <a name="c-using-a-local-variable-to-supply-the-message-text"></a>В. Использование локальной переменной для предоставления текста сообщения  
 В следующем примере кода показано, как использовать локальную переменную для предоставления текста сообщения для инструкции `RAISERROR`.  
  
```sql  
DECLARE @StringVariable NVARCHAR(50);  
SET @StringVariable = N'<\<%7.3s>>';  
  
RAISERROR (@StringVariable, -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [PRINT (Transact-SQL)](../../t-sql/language-elements/print-transact-sql.md)   
 [sp_addmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [xp_logevent (Transact-SQL)](../../relational-databases/system-stored-procedures/xp-logevent-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL)](../../t-sql/functions/error-state-transact-sql.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)  
  
  

