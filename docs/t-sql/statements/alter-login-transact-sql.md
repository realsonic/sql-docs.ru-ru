---
description: ALTER LOGIN (Transact-SQL)
title: ALTER LOGIN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_LOGIN_TSQL
- ALTER LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER LOGIN statement
- change password
- mapping logins [SQL Server]
- logins [SQL Server], modifying
- passwords [SQL Server], modifying
- names [SQL Server], logins
- modifying login accounts
ms.assetid: e247b84e-c99e-4af8-8b50-57586e1cb1c5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 62fcae210d2fdcef552f7a3f8ae0053a16e8c5c6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642230"
---
# <a name="alter-login-transact-sql"></a>ALTER LOGIN (Transact-SQL)

Изменяет свойства учетной записи входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [База данных SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE

<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK
  
<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

## <a name="arguments"></a>Аргументы

*login_name* — указывает имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которое необходимо изменить. Имена входа домена необходимо заключать в квадратные скобки в формате [домен\пользователь].

ENABLE | DISABLE — включает или отключает это имя входа. Отключение входа не влияет на поведение имен входа, которые уже подключены. (Чтобы завершить существующие подключения, используйте инструкцию `KILL`.) Отключенные имена входа сохраняют свои разрешения и все еще могут быть олицетворены.

PASSWORD **='** _password_ **'**  — применятся только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает пароль для имени входа, которое необходимо изменить. В паролях учитывается регистр символов.

PASSWORD **=** _hashed\_password_ — применяется только к ключевому слову HASHED. Указывает хэшированное значение пароля для создаваемого имени входа.

> [!IMPORTANT]
> Если имя входа (или пользователь автономной базы данных) используется для подключения и выполняется проверка подлинности, данные идентификаторов имени входа при подключении помещаются в кэш. Для имени входа, использующегося при проверке подлинности Windows, сюда относятся данные о членстве в группах Windows. Идентификатор имени входа остается зарегистрированным на протяжении периода, в который поддерживается соединение. Для принудительного изменения идентификатора, например сброса пароля или изменения членства в группе Windows, имя входа необходимо использовать для выхода из центра проверки подлинности (Windows или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]), а затем повторно выполнить вход. Член предопределенной роли сервера **sysadmin** или любого имени входа с разрешением **ALTER ANY CONNECTION** может использовать команду **KILL** и принудительно разорвать подключение для выполнения повторного подключения с использованием имени входа. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] может повторно использовать сведения о соединении при открытии нескольких соединений в окнах обозревателя объектов и редактора запросов. Закройте все соединения для принудительного повторного подключения.

HASHED — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что пароль, введенный после аргумента PASSWORD, уже хэширован. Если этот параметр не выбран, то пароль хэшируется перед сохранением в базе данных. Этот параметр следует использовать только для синхронизации имен входа между двумя серверами. Не используйте параметр HASHED для плановой смены паролей.

OLD_PASSWORD **='** _oldpassword_ **'**  — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пароль имени входа, которому будет присвоен новый пароль. В паролях учитывается регистр символов.

MUST_CHANGE — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если данный параметр включен, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребует ввести новый пароль при первом использовании измененного имени входа.

DEFAULT_DATABASE **=** _database_ — указывает базу данных по умолчанию, связываемую с именем входа.

DEFAULT_LANGUAGE **=** _language_ — указывает язык по умолчанию, назначаемый имени входа. Языком по умолчанию для всех имен входа базы данных SQL является английский, и его нельзя изменить. Языком по умолчанию для имени входа `sa` в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Linux является английский, но его можно изменить.

NAME = *login_name* — указывает новое имя для имени входа, которое необходимо переименовать. Если оно является именем входа Windows, то идентификатор безопасности участника Windows, соответствующий новому имени, должен совпадать с идентификатором безопасности, относящимся к имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Новое имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может содержать обратную косую черту (\\).

CHECK_EXPIRATION = { ON | **OFF** } — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, должна ли политика истечения срока действия паролей принудительно применяться к этому имени входа. Значение по умолчанию — OFF.

CHECK_POLICY **=** { **ON** | OFF } — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что политики паролей Windows компьютера, на котором работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , должны принудительно применяться к этому имени входа. Значение по умолчанию — ON.

CREDENTIAL = *credential_name* — имя учетных данных для сопоставления с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Учетные данные уже должны существовать на сервере. Дополнительные сведения см. в статье [Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md). Учетные данные не могут быть сопоставлены с именем входа sa.

NO CREDENTIAL — удаляет все существующие сопоставления имени входа с учетными данными сервера. Дополнительные сведения см. в статье [Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

UNLOCK — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что заблокированное имя входа должно быть разблокировано.

ADD CREDENTIAL — добавляет к имени входа учетные данные поставщика расширенного управления ключами. Дополнительные сведения см. в статье [Расширенное управление ключами (Extensible Key Management)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

DROP CREDENTIAL — удаляет из имени входа учетные данные поставщика расширенного управления ключами. Дополнительные сведения см. в статье [Расширенное управление ключами (Extensible Key Management)] (../.. /relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Remarks

Если параметр CHECK_POLICY имеет значение ON, аргумент HASHED использовать нельзя.

При изменении значения CHECK_POLICY на ON происходит следующее.

- Журнал паролей инициализируется значением хэша текущего пароля.

  При изменении CHECK_POLICY на OFF происходит следующее.

- Параметр CHECK_EXPIRATION также получает значение OFF.
- Журнал паролей очищается.
- Значение *lockout_time* сбрасывается.

Если задан параметр MUST_CHANGE, то параметры CHECK_EXPIRATION и CHECK_POLICY должны иметь значение ON. В противном случае выполнение инструкции приведет к ошибке.

Если значение параметра CHECK_POLICY установлено равным OFF, то параметр CHECK_EXPIRATION не может иметь значения ON. Выполнение инструкции ALTER LOGIN с таким сочетанием параметров завершится ошибкой.

Вы не можете использовать ALTER_LOGIN с аргументом DISABLE для запрета доступа к группе Windows. Например, инструкция ALTER LOGIN [*domain\group*] DISABLE вернет следующее сообщение об ошибке:

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

Это сделано намеренно.
  
В [!INCLUDE[ssSDS](../../includes/sssds-md.md)] данные имени входа необходимы для проверки подлинности подключения, и правила брандмауэра на уровне сервера временно кэшируются в каждой базе данных. Этот кэш периодически обновляется. Чтобы принудительно обновить кэш проверки подлинности и убедиться в том, что база данных имеет последнюю версию таблицы имен входа, выполните инструкцию [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Разрешения

Необходимо разрешение ALTER ANY LOGIN.

Если используется параметр CREDENTIAL, то также требуется разрешение ALTER ANY CREDENTIAL.

Если изменяемое имя входа является членом предопределенной роли сервера **sysadmin** или имеет разрешение CONTROL SERVER, ему также требуется разрешение CONTROL SERVER для внесения следующих изменений.

- Сброс пароля без указания старого.
- Включение параметров MUST_CHANGE, CHECK_POLICY или CHECK_EXPIRATION.
- Изменение имени входа.
- Включение или отключение имени входа.
- Сопоставление имени входа с другими учетными данными.

Участник может изменить пароль, язык по умолчанию и базу данных по умолчанию для своего имени входа.

## <a name="examples"></a>Примеры

### <a name="a-enabling-a-disabled-login"></a>A. Включение отключенного имени входа

Следующий пример включает имя входа `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>Б. Изменение пароля для имени входа

В следующем примере пароль для имени входа `Mary5` изменяется на надежный пароль.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-password-of-a-login-when-logged-in-as-the-login"></a>В. Изменение пароля для того имени, с которым выполнен вход в систему

Если вам нужно изменить пароль для того имени входа, с которым вы вошли в систему, но у вас нет разрешения `ALTER ANY LOGIN`, необходимо указать параметр `OLD_PASSWORD`.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>' OLD_PASSWORD = '<oldWeakPasswordHere>';
```

### <a name="d-changing-the-name-of-a-login"></a>Г. Изменение имени входа

Следующий пример изменяет имя входа `Mary5` на `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="e-mapping-a-login-to-a-credential"></a>Д. Сопоставление имени входа с учетными данными

Следующий пример сопоставляет имя входа `John2` с учетными данными `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="f-mapping-a-login-to-an-extensible-key-management-credential"></a>Е. Сопоставление имени входа с учетными данными расширенного управления ключами

В следующем примере имя входа `Mary5` сопоставляется с учетными данными `EKMProvider1`.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>Е. Разблокирование имени входа

Чтобы разблокировать имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните следующую инструкцию, заменив \*\*\*\* нужным паролем учетной записи.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Чтобы разблокировать учетную запись без изменения пароля, отключите и снова включите политику проверки.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>Ж. Изменение пароля для имени входа с помощью параметра HASHED

В следующем примере пароль имени входа `TestUser` изменяется на заранее хэшированное значение.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>См. также:

- [Учетные данные](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* База данных SQL \*_**
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>База данных SQL

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for Azure SQL Database

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>Аргументы

*login_name* — указывает имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которое необходимо изменить. Имена входа домена необходимо заключать в квадратные скобки в формате [домен\пользователь].

ENABLE | DISABLE — включает или отключает это имя входа. Отключение входа не влияет на поведение имен входа, которые уже подключены. (Чтобы завершить существующие подключения, используйте инструкцию `KILL`.) Отключенные имена входа сохраняют свои разрешения и все еще могут быть олицетворены.

PASSWORD **='** _password_ **'**  — применятся только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает пароль для имени входа, которое необходимо изменить. В паролях учитывается регистр символов.

Для поддержания подключений к базе данных SQL в активном состоянии требуется повторная авторизация (выполняемая ядром СУБД) по крайней мере каждые 10 часов. Ядро СУБД пытается выполнить повторную авторизацию с использованием первоначального пароля. Пользователю не нужно вводить никаких данных. В целях повышения производительности при сбросе пароля в базе данных SQL повторная проверка подлинности подключения не проводится, даже если подключение сбрасывается из-за создания пула подключений. В локальном развертывании SQL Server поведение иное. Если пароль изменился с момента первоначальной авторизации подключения, подключение должно быть завершено и должно быть установлено новое подключение с использованием нового пароля. Пользователь с разрешением KILL DATABASE CONNECTION может явным образом завершить подключение к базе данных SQL с помощью команды KILL. Дополнительные сведения см. в статье [KILL (Transact-SQL)](../../t-sql/language-elements/kill-transact-sql.md).

> [!IMPORTANT]
> Если имя входа (или пользователь автономной базы данных) используется для подключения и выполняется проверка подлинности, данные идентификаторов имени входа при подключении помещаются в кэш. Для имени входа, использующегося при проверке подлинности Windows, сюда относятся данные о членстве в группах Windows. Идентификатор имени входа остается зарегистрированным на протяжении периода, в который поддерживается соединение. Для принудительного изменения идентификатора, например сброса пароля или изменения членства в группе Windows, имя входа необходимо использовать для выхода из центра проверки подлинности (Windows или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]), а затем повторно выполнить вход. Член предопределенной роли сервера **sysadmin** или любого имени входа с разрешением **ALTER ANY CONNECTION** может использовать команду **KILL** и принудительно разорвать подключение для выполнения повторного подключения с использованием имени входа. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] может повторно использовать сведения о соединении при открытии нескольких соединений в окнах обозревателя объектов и редактора запросов. Закройте все соединения для принудительного повторного подключения.

OLD_PASSWORD **='** _oldpassword_ **'**  — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пароль имени входа, которому будет присвоен новый пароль. В паролях учитывается регистр символов.

NAME = *login_name* — указывает новое имя для имени входа, которое необходимо переименовать. Если оно является именем входа Windows, то идентификатор безопасности участника Windows, соответствующий новому имени, должен совпадать с идентификатором безопасности, относящимся к имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Новое имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может содержать обратную косую черту (\\).

## <a name="remarks"></a>Remarks

В [!INCLUDE[ssSDS](../../includes/sssds-md.md)] данные имени входа необходимы для проверки подлинности подключения, и правила брандмауэра на уровне сервера временно кэшируются в каждой базе данных. Этот кэш периодически обновляется. Чтобы принудительно обновить кэш проверки подлинности и убедиться в том, что база данных имеет последнюю версию таблицы имен входа, выполните инструкцию [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Разрешения

Необходимо разрешение ALTER ANY LOGIN.

Если изменяемое имя входа является членом предопределенной роли сервера **sysadmin** или имеет разрешение CONTROL SERVER, ему также требуется разрешение CONTROL SERVER для внесения следующих изменений.

- Сброс пароля без указания старого.
- Изменение имени входа.
- Включение или отключение имени входа.
- Сопоставление имени входа с другими учетными данными.

Субъект может изменить пароль для своего имени входа.

## <a name="examples"></a>Примеры

Эти примеры также включают примеры использования других продуктов SQL. См. выше информацию о том, какие аргументы поддерживаются.

### <a name="a-enabling-a-disabled-login"></a>A. Включение отключенного имени входа

Следующий пример включает имя входа `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>Б. Изменение пароля для имени входа

В следующем примере пароль для имени входа `Mary5` изменяется на надежный пароль.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>В. Изменение имени входа

Следующий пример изменяет имя входа `Mary5` на `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>Г. Сопоставление имени входа с учетными данными

Следующий пример сопоставляет имя входа `John2` с учетными данными `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>Д. Сопоставление имени входа с учетными данными расширенного управления ключами

В следующем примере имя входа `Mary5` сопоставляется с учетными данными `EKMProvider1`.


**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>Е. Разблокирование имени входа

Чтобы разблокировать имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните следующую инструкцию, заменив \*\*\*\* нужным паролем учетной записи.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Чтобы разблокировать учетную запись без изменения пароля, отключите и снова включите политику проверки.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>Ж. Изменение пароля для имени входа с помощью параметра HASHED

В следующем примере пароль имени входа `TestUser` изменяется на заранее хэшированное значение.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>См. также:

- [Учетные данные](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Управляемый экземпляр SQL \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Управляемый экземпляр SQL Azure

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server and Azure SQL Managed Instance

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK

<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

> [!NOTE]
> Изменились функции, доступные после создания администратору Azure AD для управляемого экземпляра. Дополнительные сведения см. в разделе [Новые функции администратора Azure AD для MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

```syntaxsql
-- Syntax for Azure SQL Managed Instance using Azure AD logins

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
     DEFAULT_DATABASE = database
   | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>Аргументы

### <a name="arguments-applicable-to-sql-and-azure-ad-logins"></a>Аргументы, применимые к именам входа SQL и Azure AD

*login_name* — указывает имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которое необходимо изменить. Имена для входа Azure AD должны быть указаны как user@domain. Например, john.smith@contoso.com, либо же как группа или приложение Azure AD. Для имен входа Azure AD *login_name* должно соответствовать существующему имени входа Azure AD, созданному в базе данных master.

ENABLE | DISABLE — включает или отключает это имя входа. Отключение входа не влияет на поведение имен входа, которые уже подключены. (Чтобы завершить существующие подключения, используйте инструкцию `KILL`.) Отключенные имена входа сохраняют свои разрешения и все еще могут быть олицетворены.

DEFAULT_DATABASE **=** _database_ — указывает базу данных по умолчанию, связываемую с именем входа.

DEFAULT_LANGUAGE **=** _language_ — указывает язык по умолчанию, назначаемый имени входа. Языком по умолчанию для всех имен входа базы данных SQL является английский, и его нельзя изменить. Языком по умолчанию для имени входа `sa` в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Linux является английский, но его можно изменить.

### <a name="arguments-applicable-only-to-sql-logins"></a>Аргументы, применимые только к именам входа SQL

PASSWORD **='** _password_ **'**  — применятся только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает пароль для имени входа, которое необходимо изменить. В паролях учитывается регистр символов. Пароли также не применяются при использовании с внешними именами входа, такими как имена входа Azure AD.

Для поддержания подключений к базе данных SQL в активном состоянии требуется повторная авторизация (выполняемая ядром СУБД) по крайней мере каждые 10 часов. Ядро СУБД пытается выполнить повторную авторизацию с использованием первоначального пароля. Пользователю не нужно вводить никаких данных. В целях повышения производительности при сбросе пароля в базе данных SQL повторная проверка подлинности подключения не проводится, даже если подключение сбрасывается из-за создания пула подключений. В локальном развертывании SQL Server поведение иное. Если пароль изменился с момента первоначальной авторизации подключения, подключение должно быть завершено и должно быть установлено новое подключение с использованием нового пароля. Пользователь с разрешением KILL DATABASE CONNECTION может явным образом завершить подключение к базе данных SQL с помощью команды KILL. Дополнительные сведения см. в статье [KILL (Transact-SQL)](../../t-sql/language-elements/kill-transact-sql.md).

PASSWORD **=** _hashed\_password_ — применяется только к ключевому слову HASHED. Указывает хэшированное значение пароля для создаваемого имени входа.

HASHED — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что пароль, введенный после аргумента PASSWORD, уже хэширован. Если этот параметр не выбран, то пароль хэшируется перед сохранением в базе данных. Этот параметр следует использовать только для синхронизации имен входа между двумя серверами. Не используйте параметр HASHED для плановой смены паролей.

OLD_PASSWORD **='** _oldpassword_ **'**  — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пароль имени входа, которому будет присвоен новый пароль. В паролях учитывается регистр символов.

MUST_CHANGE<br>
Применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если данный параметр включен, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребует ввести новый пароль при первом использовании измененного имени входа.

NAME = *login_name* — указывает новое имя для имени входа, которое необходимо переименовать. Если оно является именем входа Windows, то идентификатор безопасности участника Windows, соответствующий новому имени, должен совпадать с идентификатором безопасности, относящимся к имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Новое имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может содержать обратную косую черту (\\).

CHECK_EXPIRATION = { ON | **OFF** } — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, должна ли политика истечения срока действия паролей принудительно применяться к этому имени входа. Значение по умолчанию — OFF.

CHECK_POLICY **=** { **ON** | OFF } — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что политики паролей Windows компьютера, на котором работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , должны принудительно применяться к этому имени входа. Значение по умолчанию — ON.

CREDENTIAL = *credential_name* — имя учетных данных для сопоставления с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Учетные данные уже должны существовать на сервере. Дополнительные сведения см. в статье [Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md). Учетные данные не могут быть сопоставлены с именем входа sa.

NO CREDENTIAL — удаляет все существующие сопоставления имени входа с учетными данными сервера. Дополнительные сведения см. в статье [Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

UNLOCK — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что заблокированное имя входа должно быть разблокировано.

ADD CREDENTIAL — добавляет к имени входа учетные данные поставщика расширенного управления ключами. Дополнительные сведения см. в статье [Расширенное управление ключами (Extensible Key Management)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

DROP CREDENTIAL — удаляет из имени входа учетные данные поставщика расширенного управления ключами. Дополнительные сведения см. в статье [Расширенное управление ключами (Extensible Key Management)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Remarks

Если параметр CHECK_POLICY имеет значение ON, аргумент HASHED использовать нельзя.

При изменении значения CHECK_POLICY на ON происходит следующее.

- Журнал паролей инициализируется значением хэша текущего пароля.

  При изменении CHECK_POLICY на OFF происходит следующее.

- Параметр CHECK_EXPIRATION также получает значение OFF.
- Журнал паролей очищается.
- Значение *lockout_time* сбрасывается.

Если задан параметр MUST_CHANGE, то параметры CHECK_EXPIRATION и CHECK_POLICY должны иметь значение ON. В противном случае выполнение инструкции приведет к ошибке.

Если значение параметра CHECK_POLICY установлено равным OFF, то параметр CHECK_EXPIRATION не может иметь значения ON. Выполнение инструкции ALTER LOGIN с таким сочетанием параметров завершится ошибкой.

Нельзя использовать параметр ALTER_LOGIN с аргументом DISABLE для запрещения доступа группе Windows. Это сделано намеренно. Например, инструкция ALTER_LOGIN [*domain\group*] DISABLE вернет следующее сообщение об ошибке:

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

В [!INCLUDE[ssSDS](../../includes/sssds-md.md)] данные имени входа необходимы для проверки подлинности подключения, и правила брандмауэра на уровне сервера временно кэшируются в каждой базе данных. Этот кэш периодически обновляется. Чтобы принудительно обновить кэш проверки подлинности и убедиться в том, что база данных имеет последнюю версию таблицы имен входа, выполните инструкцию [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Разрешения

Необходимо разрешение ALTER ANY LOGIN.

Если используется параметр CREDENTIAL, то также требуется разрешение ALTER ANY CREDENTIAL.

Если изменяемое имя входа является членом предопределенной роли сервера **sysadmin** или имеет разрешение CONTROL SERVER, ему также требуется разрешение CONTROL SERVER для внесения следующих изменений.

- Сброс пароля без указания старого.
- Включение параметров MUST_CHANGE, CHECK_POLICY или CHECK_EXPIRATION.
- Изменение имени входа.
- Включение или отключение имени входа.
- Сопоставление имени входа с другими учетными данными.

Участник может изменить пароль, язык по умолчанию и базу данных по умолчанию для своего имени входа.

Только субъект SQL с привилегиями `sysadmin` может выполнить команду ALTER LOGIN для имени входа Azure AD.

## <a name="examples"></a>Примеры

Эти примеры также включают примеры использования других продуктов SQL. См. выше информацию о том, какие аргументы поддерживаются.

### <a name="a-enabling-a-disabled-login"></a>A. Включение отключенного имени входа

Следующий пример включает имя входа `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>Б. Изменение пароля для имени входа

В следующем примере пароль для имени входа `Mary5` изменяется на надежный пароль.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>В. Изменение имени входа

Следующий пример изменяет имя входа `Mary5` на `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>Г. Сопоставление имени входа с учетными данными

Следующий пример сопоставляет имя входа `John2` с учетными данными `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>Д. Сопоставление имени входа с учетными данными расширенного управления ключами

В следующем примере имя входа `Mary5` сопоставляется с учетными данными `EKMProvider1`.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий, Управляемый экземпляр SQL Azure.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>Е. Разблокирование имени входа

Чтобы разблокировать имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните следующую инструкцию, заменив \*\*\*\* нужным паролем учетной записи.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Чтобы разблокировать учетную запись без изменения пароля, отключите и снова включите политику проверки.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>Ж. Изменение пароля для имени входа с помощью параметра HASHED

В следующем примере пароль имени входа `TestUser` изменяется на заранее хэшированное значение.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий, Управляемый экземпляр SQL Azure.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

### <a name="h-disabling-the-login-of-an-azure-ad-user"></a>З. Отключение входа пользователя Azure AD

Следующий пример отключает имя входа пользователя Azure AD, joe@contoso.com.

```sql
ALTER LOGIN [joe@contoso.com] DISABLE
```

## <a name="see-also"></a>См. также:

- [Учетные данные](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for Azure Synapse

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>Аргументы

*login_name* — указывает имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которое необходимо изменить. Имена входа домена необходимо заключать в квадратные скобки в формате [домен\пользователь].

ENABLE | DISABLE — включает или отключает это имя входа. Отключение входа не влияет на поведение имен входа, которые уже подключены. (Чтобы завершить существующие подключения, используйте инструкцию `KILL`.) Отключенные имена входа сохраняют свои разрешения и все еще могут быть олицетворены.

PASSWORD **='** _password_ **'**  — применятся только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает пароль для имени входа, которое необходимо изменить. В паролях учитывается регистр символов.

Для поддержания подключений к базе данных SQL в активном состоянии требуется повторная авторизация (выполняемая ядром СУБД) по крайней мере каждые 10 часов. Ядро СУБД пытается выполнить повторную авторизацию с использованием первоначального пароля. Пользователю не нужно вводить никаких данных. В целях повышения производительности при сбросе пароля в базе данных SQL повторная проверка подлинности подключения не проводится, даже если подключение сбрасывается из-за создания пула подключений. В локальном развертывании SQL Server поведение иное. Если пароль изменился с момента первоначальной авторизации подключения, подключение должно быть завершено и должно быть установлено новое подключение с использованием нового пароля. Пользователь с разрешением KILL DATABASE CONNECTION может явным образом завершить подключение к базе данных SQL с помощью команды KILL. Дополнительные сведения см. в статье [KILL (Transact-SQL)](../../t-sql/language-elements/kill-transact-sql.md).

> [!IMPORTANT]
> Если имя входа (или пользователь автономной базы данных) используется для подключения и выполняется проверка подлинности, данные идентификаторов имени входа при подключении помещаются в кэш. Для имени входа, использующегося при проверке подлинности Windows, сюда относятся данные о членстве в группах Windows. Идентификатор имени входа остается зарегистрированным на протяжении периода, в который поддерживается соединение. Для принудительного изменения идентификатора, например сброса пароля или изменения членства в группе Windows, имя входа необходимо использовать для выхода из центра проверки подлинности (Windows или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]), а затем повторно выполнить вход. Член предопределенной роли сервера **sysadmin** или любого имени входа с разрешением **ALTER ANY CONNECTION** может использовать команду **KILL** и принудительно разорвать подключение для выполнения повторного подключения с использованием имени входа. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] может повторно использовать сведения о соединении при открытии нескольких соединений в окнах обозревателя объектов и редактора запросов. Закройте все соединения для принудительного повторного подключения.

OLD_PASSWORD **='** _oldpassword_ **'**  — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пароль имени входа, которому будет присвоен новый пароль. В паролях учитывается регистр символов.

NAME = *login_name* — указывает новое имя для имени входа, которое необходимо переименовать. Если оно является именем входа Windows, то идентификатор безопасности участника Windows, соответствующий новому имени, должен совпадать с идентификатором безопасности, относящимся к имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Новое имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может содержать обратную косую черту (\\).

## <a name="remarks"></a>Remarks

В [!INCLUDE[ssSDS](../../includes/sssds-md.md)] данные имени входа необходимы для проверки подлинности подключения, и правила брандмауэра на уровне сервера временно кэшируются в каждой базе данных. Этот кэш периодически обновляется. Чтобы принудительно обновить кэш проверки подлинности и убедиться в том, что база данных содержит последнюю версию таблицы имен входа, выполните инструкцию [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Разрешения

Необходимо разрешение ALTER ANY LOGIN.

Если изменяемое имя входа является членом предопределенной роли сервера **sysadmin** или имеет разрешение CONTROL SERVER, ему также требуется разрешение CONTROL SERVER для внесения следующих изменений.

- Сброс пароля без указания старого.
- Изменение имени входа.
- Включение или отключение имени входа.
- Сопоставление имени входа с другими учетными данными.

Субъект может изменить пароль для своего имени входа.

## <a name="examples"></a>Примеры

Эти примеры также включают примеры использования других продуктов SQL. См. выше информацию о том, какие аргументы поддерживаются.

### <a name="a-enabling-a-disabled-login"></a>A. Включение отключенного имени входа

Следующий пример включает имя входа `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>Б. Изменение пароля для имени входа

В следующем примере пароль для имени входа `Mary5` изменяется на надежный пароль.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>В. Изменение имени входа

Следующий пример изменяет имя входа `Mary5` на `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>Г. Сопоставление имени входа с учетными данными

Следующий пример сопоставляет имя входа `John2` с учетными данными `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>Д. Сопоставление имени входа с учетными данными расширенного управления ключами

В следующем примере имя входа `Mary5` сопоставляется с учетными данными `EKMProvider1`.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>Е. Разблокирование имени входа

Чтобы разблокировать имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните следующую инструкцию, заменив \*\*\*\* нужным паролем учетной записи.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Чтобы разблокировать учетную запись без изменения пароля, отключите и снова включите политику проверки.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>Ж. Изменение пароля для имени входа с помощью параметра HASHED

В следующем примере пароль имени входа `TestUser` изменяется на заранее хэшированное значение.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>См. также:

- [Учетные данные](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Система платформы аналитики

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for Analytics Platform System

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    }

<status_option> ::=ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
      | <password_option> [<password_option> ]
    ]
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }

<password_option> ::=
    MUST_CHANGE | UNLOCK
```

## <a name="arguments"></a>Аргументы

*login_name* — указывает имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которое необходимо изменить. Имена входа домена необходимо заключать в квадратные скобки в формате [домен\пользователь].

ENABLE | DISABLE — включает или отключает это имя входа. Отключение входа не влияет на поведение имен входа, которые уже подключены. (Чтобы завершить существующие подключения, используйте инструкцию `KILL`.) Отключенные имена входа сохраняют свои разрешения и все еще могут быть олицетворены.

PASSWORD **='** _password_ **'**  — применятся только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает пароль для имени входа, которое необходимо изменить. В паролях учитывается регистр символов.

> [!IMPORTANT]
> Если имя входа (или пользователь автономной базы данных) используется для подключения и выполняется проверка подлинности, данные идентификаторов имени входа при подключении помещаются в кэш. Для имени входа, использующегося при проверке подлинности Windows, сюда относятся данные о членстве в группах Windows. Идентификатор имени входа остается зарегистрированным на протяжении периода, в который поддерживается соединение. Для принудительного изменения идентификатора, например сброса пароля или изменения членства в группе Windows, имя входа необходимо использовать для выхода из центра проверки подлинности (Windows или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]), а затем повторно выполнить вход. Член предопределенной роли сервера **sysadmin** или любого имени входа с разрешением **ALTER ANY CONNECTION** может использовать команду **KILL** и принудительно разорвать подключение для выполнения повторного подключения с использованием имени входа. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] может повторно использовать сведения о соединении при открытии нескольких соединений в окнах обозревателя объектов и редактора запросов. Закройте все соединения для принудительного повторного подключения.

OLD_PASSWORD **='** _oldpassword_ **'**  — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пароль имени входа, которому будет присвоен новый пароль. В паролях учитывается регистр символов.

MUST_CHANGE — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если данный параметр включен, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребует ввести новый пароль при первом использовании измененного имени входа.

NAME = *login_name* — указывает новое имя для имени входа, которое необходимо переименовать. Если оно является именем входа Windows, то идентификатор безопасности участника Windows, соответствующий новому имени, должен совпадать с идентификатором безопасности, относящимся к имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Новое имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может содержать обратную косую черту (\\).

CHECK_EXPIRATION = { ON | **OFF** } — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, должна ли политика истечения срока действия паролей принудительно применяться к этому имени входа. Значение по умолчанию — OFF.

CHECK_POLICY **=** { **ON** | OFF } — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что политики паролей Windows компьютера, на котором работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , должны принудительно применяться к этому имени входа. Значение по умолчанию — ON.

UNLOCK — применяется только к именам входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указывает, что заблокированное имя входа должно быть разблокировано.

## <a name="remarks"></a>Remarks

Если параметр CHECK_POLICY имеет значение ON, аргумент HASHED использовать нельзя.

При изменении значения CHECK_POLICY на ON происходит следующее.

- Журнал паролей инициализируется значением хэша текущего пароля.

  При изменении CHECK_POLICY на OFF происходит следующее.

- Параметр CHECK_EXPIRATION также получает значение OFF.
- Журнал паролей очищается.
- Значение *lockout_time* сбрасывается.

Если задан параметр MUST_CHANGE, то параметры CHECK_EXPIRATION и CHECK_POLICY должны иметь значение ON. В противном случае выполнение инструкции приведет к ошибке.

Если значение параметра CHECK_POLICY установлено равным OFF, то параметр CHECK_EXPIRATION не может иметь значения ON. Выполнение инструкции ALTER LOGIN с таким сочетанием параметров завершится ошибкой.

Нельзя использовать параметр ALTER_LOGIN с аргументом DISABLE для запрещения доступа группе Windows. Это сделано намеренно. Например, инструкция ALTER_LOGIN [*domain\group*] DISABLE вернет следующее сообщение об ошибке:

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

В [!INCLUDE[ssSDS](../../includes/sssds-md.md)] данные имени входа необходимы для проверки подлинности подключения, и правила брандмауэра на уровне сервера временно кэшируются в каждой базе данных. Этот кэш периодически обновляется. Чтобы принудительно обновить кэш проверки подлинности и убедиться в том, что база данных имеет последнюю версию таблицы имен входа, выполните инструкцию [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Разрешения

Необходимо разрешение ALTER ANY LOGIN.

Если используется параметр CREDENTIAL, то также требуется разрешение ALTER ANY CREDENTIAL.

Если изменяемое имя входа является членом предопределенной роли сервера **sysadmin** или имеет разрешение CONTROL SERVER, ему также требуется разрешение CONTROL SERVER для внесения следующих изменений.

- Сброс пароля без указания старого.
- Включение параметров MUST_CHANGE, CHECK_POLICY или CHECK_EXPIRATION.
- Изменение имени входа.
- Включение или отключение имени входа.
- Сопоставление имени входа с другими учетными данными.

Участник может изменить пароль, язык по умолчанию и базу данных по умолчанию для своего имени входа.

## <a name="examples"></a>Примеры

Эти примеры также включают примеры использования других продуктов SQL. См. выше информацию о том, какие аргументы поддерживаются.

### <a name="a-enabling-a-disabled-login"></a>A. Включение отключенного имени входа

Следующий пример включает имя входа `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>Б. Изменение пароля для имени входа

В следующем примере пароль для имени входа `Mary5` изменяется на надежный пароль.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>В. Изменение имени входа

Следующий пример изменяет имя входа `Mary5` на `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>Г. Сопоставление имени входа с учетными данными

Следующий пример сопоставляет имя входа `John2` с учетными данными `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>Д. Сопоставление имени входа с учетными данными расширенного управления ключами

В следующем примере имя входа `Mary5` сопоставляется с учетными данными `EKMProvider1`.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>Е. Разблокирование имени входа

Чтобы разблокировать имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните следующую инструкцию, заменив \*\*\*\* нужным паролем учетной записи.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Чтобы разблокировать учетную запись без изменения пароля, отключите и снова включите политику проверки.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>Ж. Изменение пароля для имени входа с помощью параметра HASHED

В следующем примере пароль имени входа `TestUser` изменяется на заранее хэшированное значение.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>См. также:

- [Учетные данные](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
