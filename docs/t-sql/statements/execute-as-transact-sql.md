---
description: EXECUTE AS (Transact-SQL)
title: EXECUTE AS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EXECUTE AS
- EXECUTE_AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- WITH NO REVERT clause
- sessions [SQL Server], execution context
- EXECUTE AS
- execution context [SQL Server]
- switching execution context
ms.assetid: 613b8271-7f7d-4378-b7a2-5a7698551dbd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: fa823898f479867657fb106ab0d929aa861b7da5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081312"
---
# <a name="execute-as-transact-sql"></a>EXECUTE AS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Задает контекст выполнения для сеанса.  
  
 По умолчанию сеанс запускается при входе пользователя в систему и завершается при выходе пользователя из системы. Все операции во время сеанса подлежат проверке на наличие у пользователя соответствующих разрешений. При запуске инструкции **EXECUTE AS** контекст выполнения сеанса переключается на заданное имя входа или имя пользователя. После переключений контекста имя входа и пользовательские токены безопасности для этой учетной записи проходят проверку на соответствие разрешениям (а не пользователь, вызвавший инструкцию **EXECUTE AS**). В сущности, либо входная или пользовательская учетная запись олицетворяется на время сеанса или на время выполнения модуля, либо явно обращается переключение контекста.  
  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
{ EXEC | EXECUTE } AS <context_specification>  
[;]  
  
<context_specification>::=  
{ LOGIN | USER } = 'name'  
    [ WITH { NO REVERT | COOKIE INTO @varbinary_variable } ]   
| CALLER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 Имя_для_входа  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает, что контекст выполнения олицетворения — это имя входа. Область олицетворения — это уровень сервера.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных, Базе данных SQL или в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
 Пользователь  
 Определяет контекст для олицетворения пользователя в текущей базе данных. Область олицетворения ограничена текущей базой данных. При переключении контекста на пользователя базы данных разрешения уровня сервера этого пользователя не наследуются.  
  
> [!IMPORTANT]  
>  Пока переключение контекста на пользователя базы данных активно, любая попытка подключиться к ресурсам вне базы данных будет приводить к завершению инструкции с ошибками. Это касается инструкций USE *database*, распределенных запросов, а также запросов, которые ссылаются на другую базу данных, использующую трех- и четырехчастные идентификаторы.  
  
 *name* — допустимое имя пользователя или имя входа. Аргумент *name* должен принадлежать предопределенной роли сервера **sysadmin** либо быть субъектом в базе данных [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) или [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) соответственно.  
  
 *name* можно задавать как локальную переменную.  
  
 Аргумент *name* должен быть одноэлементным и не может быть группой, ролью, сертификатом, ключом или встроенной учетной записью, например NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService или NT AUTHORITY\LocalSystem.  
  
 Дополнительные сведения см. в разделе [Указание имени пользователя или имени входа](#_user) далее.  
  
 NO REVERT  
 Указывает, что переключение контекста нельзя вернуть к предыдущему контексту. Параметр **NO REVERT** можно использовать только на нерегламентированном уровне.  
  
 Дополнительные сведения о переключении к предыдущему контексту см. в разделе [REVERT &(Transact-SQL)](../../t-sql/statements/revert-transact-sql.md).  
  
 COOKIE INTO @*varbinary_variable*  
 Указывает, что контекст выполнения можно вернуть к предыдущему контексту, если при вызове инструкция REVERT WITH COOKIE содержит правильное значение @*varbinary_variable*. [!INCLUDE[ssDE](../../includes/ssde-md.md)] передает файл cookie для @*varbinary_variable*. Параметр **COOKIE INTO** можно использовать только на нерегламентированном уровне.  
  
 @*varbinary_variable* — **это varbinary(8000)** .  
  
> [!NOTE]  
>  Параметр **OUTPUT** файла cookie в настоящее время описан в документации как **varbinary(8000)** , что верно определяет его максимальную длину. Однако текущая реализация возвращает параметр **varbinary(100)** . Для продолжения правильной работы в случае увеличения размера файлов cookie в последующих версиях в приложении должен быть зарезервирован тип **varbinary(8000)** .  
  
 CALLER  
 При использовании внутри модуля указывает, что инструкции модуля выполняются в контексте вызывающей стороны.
При использовании вне модуля инструкция не действует.
 > [!NOTE]  
>  Этот параметр не доступен в среде [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
## <a name="remarks"></a>Remarks  
 Изменение в контексте выполнения продолжает действовать до тех пор, пока не произойдет одно из следующих событий.  
  
-   Будет запущена еще одна инструкция EXECUTE AS.  
  
-   Будет запущена инструкция REVERT.  
  
-   Сеанс удаляется.  
  
-   Работа хранимой процедуры или триггера, где выполнялась команда, завершается.  
  
Набор контекстов выполнения может быть создан при помощи нескольких вызовов инструкции EXECUTE AS на нескольких участниках. При вызове инструкция REVERT производит переключение контекста на имя входа или на пользователя на более высоком уровне стека контекстов. Это показано в разделе [Пример А](#_exampleA).  
  
##  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Указание имени пользователя или имени входа  
 Имя пользователя или имя для входа, указанное в инструкции EXECUTE AS \<context_specification>, должно существовать как субъект в базе данных **sys.database_principals** или **sys.server_principals** соответственно, в противном случае инструкция EXECUTE AS завершится неудачно. Кроме того, этому участнику должны быть предоставлены разрешения IMPERSONATE. Если вызывающая сторона не является владельцем базы данных или членом предопределенной роли сервера **sysadmin**, то субъект должен существовать даже тогда, когда пользователь обращается к базе данных или к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в качестве члена группы Windows. Для примера рассмотрим следующие условия. 
  
-   Группа **CompanyDomain\SQLUsers** имеет доступ к базе данных **Sales**.  
  
-   **CompanyDomain\SqlUser1** является членом **SQLUsers** и поэтому имеет открытый доступ к базе данных **Sales**.  
  
 Хотя группа **CompanyDomain\SqlUser1** имеет доступ к базе данных посредством членства в группе **SQLUsers**, инструкция `EXECUTE AS USER = 'CompanyDomain\SqlUser1'` завершается с ошибкой, потому что `CompanyDomain\SqlUser1` не существует в базе данных в качестве субъекта.  
  
Если пользователь утратил связь с учетной записью (соответствующее имя входа больше не существует) и не был создан с параметром **WITHOUT LOGIN**, попытка этого пользователя выполнить команду **EXECUTE AS** завершится неудачно.  
  
## <a name="best-practice"></a>Рекомендации  
 Указывайте имя входа или пользователя, который имеет наименьшие права доступа, необходимые для выполнения операций в сеансе. Например, не указывайте имя входа с разрешениями уровня сервера, если требуются разрешения только уровня базы данных, а также не указывайте учетную запись владельца базы данных, если только эти разрешения не являются обязательными.  
  
> [!CAUTION]  
>  Инструкция EXECUTE AS может выполняться успешно, пока компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] может разрешить имя. Если пользователь в домене существует, Windows может разрешить доступ в компоненте [!INCLUDE[ssDE](../../includes/ssde-md.md)] пользователю, даже если у пользователя Windows отсутствует доступ к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Данное обстоятельство может привести к ситуации, когда пользователь, не имеющий доступа к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], сможет войти в систему, хотя олицетворенное имя входа будет иметь только те разрешения, которые предоставлены пользователю public или guest.  
  
## <a name="using-with-no-revert"></a>Применение WITH NO REVERT  
 Если инструкция EXECUTE AS содержит необязательное предложение WITH NO REVERT, то контекст выполнения для сеанса нельзя сбросить с помощью инструкции REVERT или путем выполнения другой инструкции EXECUTE AS. Контекст, заданный инструкцией, остается до удаления сеанса.  
  
 Если указано предложение WITH NO REVERT COOKIE = @*varbinary_variable*, то [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] передает значение файла cookie в @*varbinary_variable*. Контекст выполнения, устанавливаемый данной инструкцией, можно возвратить только к предыдущему контексту, если вызов инструкции REVERT WITH COOKIE = @*varbinary_variable* содержит то же значение *\@varbinary_variable*.  
  
 Этот параметр может пригодиться в среде с организацией пула соединений. Организация пула соединений — это поддержка группы подключений к базе данных для повторного использования приложениями или сервером приложений. Поскольку значение, передаваемое в *\@varbinary_variable*, известно только инициатору инструкции EXECUTE AS, то инициатор может гарантировать, что установленный им контекст выполнения больше никто не сможет изменить.  
  
## <a name="determining-the-original-login"></a>Определение первоначального имени входа  
 Функция [ORIGINAL_LOGIN](../../t-sql/functions/original-login-transact-sql.md) возвращает имя входа, которое подключилось к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Можно использовать эту функцию для возврата идентификатора исходного имени входа в сеансах, содержащих множество явных и неявных переключений контекста.  
  
## <a name="permissions"></a>Разрешения  
 Чтобы указать параметр **EXECUTE AS** при входе, у вызывающего пользователя должно быть разрешение **IMPERSONATE** для заданного имени входа и не должно быть запрещено разрешение **IMPERSONATE ANY LOGIN**. Чтобы указать в предложении **EXECUTE AS** пользователя базы данных, вызывающая сторона должна иметь разрешения **IMPERSONATE** на указанное имя входа. Если указан параметр **EXECUTE AS CALLER**, то разрешения **IMPERSONATE** не являются обязательными.  
  
## <a name="examples"></a>Примеры  
  
###  <a name="a-using-execute-as-and-revert-to-switch-context"></a><a name="_exampleA"></a> A. Использование предложений EXECUTE AS и REVERT для переключения контекста  
 В приведенном примере создается стек контекстов выполнения с использованием нескольких участников. Затем инструкция `REVERT` используется для сброса контекста выполнения к предыдущему участнику. Инструкция `REVERT` выполняется множество раз, передвигаясь вверх по стеку до тех пор, пока контекст выполнения не будет установлен на первоначального участника.  
  

```sql
USE AdventureWorks2012;  
GO  
--Create two temporary principals  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
--Give IMPERSONATE permissions on user2 to user1  
--so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
--Verify the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
--Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1 and login2.  
--The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
--Remove temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>Б. Использование предложения WITH COOKIE  
 В следующем примере устанавливается контекст выполнения сеанса для определенного пользователя и указывается предложение WITH NO REVERT COOKIE = @*varbinary_variable*. Инструкция `REVERT` обязана указать значение, передаваемое переменной `@cookie` в `EXECUTE AS` инструкции, для успешного возвращения контекста обратно вызывающему. Чтобы запустить этот образец, должно существовать имя входа `login1` и пользователь `user1`, созданные в примере А.  
  

```sql 
DECLARE @cookie VARBINARY(8000);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie in a safe location in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(8000);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [REVERT (Transact-SQL)](../../t-sql/statements/revert-transact-sql.md)   
 [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)  
  
  

