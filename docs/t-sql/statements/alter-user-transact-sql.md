---
description: ALTER USER (Transact-SQL)
title: ALTER USER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_USER_TSQL
- ALTER USER
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database users
- ALTER USER statement
- renaming databases
- schemas [SQL Server], default
- database user names [SQL Server]
- names [SQL Server], database users
- default schemas
- modifying default schemas
ms.assetid: 344fc6ce-a008-47c8-a02e-47fae66cc590
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff06180d1abb67239407d91c564558e4d45c2550
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644424"
---
# <a name="alter-user-transact-sql"></a>ALTER USER (Transact-SQL)

Переименовывает пользователя базы данных или изменяет его схему, используемую по умолчанию.

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [База данных SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server

ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
```

## <a name="arguments"></a>Аргументы

 *userName* указывает имя, по которому пользователь идентифицируется в этой базе данных.

 LOGIN **=** _имя_для_входа_ сопоставляет пользователя с другим именем входа, заменив идентификатор безопасности пользователя для соответствия идентификатору безопасности имени входа.

 NAME **=** _новое_имя_пользователя_ задает новое имя для этого пользователя. Аргумент *newUserName* еще не должен существовать в текущей базе данных.

 DEFAULT_SCHEMA **=** { *имя_схемы* | NULL } задает первую схему для поиска сервером, когда он разрешает имена объектов для этого пользователя. При установке схемы по умолчанию в значение NULL схема по умолчанию удаляется из группы Windows. Параметр NULL нельзя использовать с пользователем Windows.

 PASSWORD **=** '*пароль*'  **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Указывает пароль для пользователя, которого необходимо изменить. В паролях учитывается регистр символов.

> [!NOTE]
> Данный параметр доступен только для содержащихся пользователей. Дополнительные сведения см. в разделах [Автономные базы данных](../../relational-databases/databases/contained-databases.md) и [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'старый_пароль'_ **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Текущий пароль пользователя, который будет заменен на "*password*". В паролях учитывается регистр символов. Параметр *OLD_PASSWORD* является обязательным для изменения пароля. Изменить пароль без этого параметра можно только при наличии разрешения **ALTER ANY USER**. Необходимость указать *OLD_PASSWORD* не позволит пользователям с разрешением **IMPERSONATION** изменить пароль.

> [!NOTE]
> Данный параметр доступен только для содержащихся пользователей.

 DEFAULT_LANGUAGE **=** _{ NONE \| \<lcid> \| \<language name> \| \<language alias> }_ **Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.

 Указывает язык по умолчанию, присвоенный пользователю. Если этот параметр установлен в значение NONE, то в качестве языка по умолчанию выбирается текущий язык по умолчанию для базы данных. При последующей смене языка по умолчанию для базы данных язык по умолчанию для пользователя не меняется. Аргумент *DEFAULT_LANGUAGE* может быть идентификатором локали (lcid), названием языка или псевдонимом языка.

> [!NOTE]
> Этот параметр можно задать только в автономной базе данных и только для содержащихся пользователей.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Отключает проверки шифрованных метаданных на сервере в операциях массового копирования. Это позволяет пользователю массово копировать зашифрованные данные между таблицами или базами данных без расшифровки данных. Значение по умолчанию — OFF.

> [!WARNING]
> Неправильное использование этого параметра может привести к повреждению данных. Дополнительные сведения см. в разделе [Перенос конфиденциальных данных с помощью функции Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Remarks

 Схемой по умолчанию будет первая схема, которую найдет сервер, после того как получит имена объектов для данного пользователя базы данных. Если не указано иное, схемой по умолчанию будет владелец объектов, создаваемых этим пользователем базы данных.

 Если пользователь имеет схему по умолчанию, то будет использоваться эта схема. Если у пользователя нет схемы по умолчанию, но он является членом группы, которая имеет схему по умолчанию, используется схема по умолчанию группы. Если пользователь не имеет схемы по умолчанию и является членом нескольких групп, схемой по умолчанию для этого пользователя будет схема по умолчанию группы Windows с минимальным значением principal_id и явно заданной схемой по умолчанию. Если для пользователя нельзя определить схему по умолчанию, будет использоваться схема **dbo**.

 В качестве значения DEFAULT_SCHEMA можно задать схему, которая в настоящий момент не существует в базе данных. Поэтому значение DEFAULT_SCHEMA можно определить для пользователя еще до создания схемы.

 Значение DEFAULT_SCHEMA не может быть указано для пользователя, сопоставленного с сертификатом или асимметричным ключом.

> [!IMPORTANT]
> Значение параметра DEFAULT_SCHEMA не учитывается, если пользователь является членом предопределенной роли сервера **sysadmin**. Для всех членов роли сервера **sysadmin** по умолчанию установлена схема `dbo`.

 Имя пользователя, сопоставленное с именем входа или группой Windows, можно изменить только в том случае, если значение идентификатора безопасности имени нового пользователя совпадает с уже записанным в базе данных. Такая проверка предотвращает имитацию подключения к базе данных с помощью имен входа Windows.

 Предложение WITH LOGIN позволяет сопоставить пользователя с другим именем входа. Пользователи, не имеющие имени входа, а также пользователи, сопоставленные с сертификатом или с асимметричным ключом, не могут быть повторно сопоставлены с помощью этого предложения. Возможно повторное сопоставление только пользователей (или групп) SQL и Windows. Предложение WITH LOGIN не удастся использовать для изменения типа пользователя, например для перехода от учетной записи Windows на имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Имя пользователя будет автоматически изменено на имя входа, если удовлетворяются следующие условия.

- Пользователь является пользователем Windows.

- Имя является именем Windows (содержит обратную косую черту).

- Новое имя не было указано.

- Текущее имя отличается от имени входа.

 В противном случае пользователь не будет переименован, если только вызывающий не вызовет дополнительно предложение NAME.

Имя пользователя, сопоставленное с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], сертификатом или асимметричным ключом, не может содержать обратную косую черту (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Безопасность

> [!NOTE]
> Пользователь с разрешением **ALTER ANY USER** может изменить схему по умолчанию для любого пользователя. Пользователь с измененной схемой может производить выборку данных неизвестным образом из неправильной таблицы или выполнять код из неправильной схемы.

### <a name="permissions"></a>Разрешения

 Чтобы изменить имя пользователя, необходимо разрешение **ALTER ANY USER** для базы данных.

 Чтобы изменить целевое имя входа пользователя, необходимо разрешение **CONTROL** в базе данных.

 Чтобы изменить имя пользователя с разрешением **CONTROL** в базе данных, требуется разрешение **CONTROL** в базе данных.

 Чтобы изменить схему или язык по умолчанию, требуется разрешение **ALTER** на пользователя. Пользователь может изменить свою собственную схему или язык по умолчанию.

## <a name="examples"></a>Примеры

Все примеры выполняются в базе данных пользователя.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Изменение имени пользователя базы данных

 В следующем примере показано изменение имени пользователя базы данных с `Mary5` на `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>Б. Изменение схемы пользователя, используемой по умолчанию

 В данном примере показано изменение схемы пользователя, используемой по умолчанию, с `Mary51` на `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>В. Изменение нескольких параметров одновременно

 В следующем примере несколько параметров пользователя автономной базы данных изменяются одной инструкцией.

**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

## <a name="see-also"></a>См. также раздел

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [Автономные базы данных](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* База данных SQL \*_**
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>База данных SQL

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for Azure SQL Database

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = schemaName
| LOGIN = loginName
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
[;]

-- Azure SQL Database Update Syntax
ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- SQL Database syntax when connected to a federation member
ALTER USER userName
 WITH <set_item> [ ,... n ]
[;]

<set_item> ::=
 NAME = newUserName
```

## <a name="arguments"></a>Аргументы

 *userName* указывает имя, по которому пользователь идентифицируется в этой базе данных.

 LOGIN **=** _имя_для_входа_ сопоставляет пользователя с другим именем входа, заменив идентификатор безопасности пользователя для соответствия идентификатору безопасности имени входа.

 Если ALTER USER является единственной инструкцией в пакете SQL, предложение WITH LOGIN поддерживается в Базе данных SQL Azure. Если инструкция ALTER USER не единственная в пакете SQL или выполняется в динамическом коде SQL, предложение WITH LOGIN не поддерживается.

 NAME **=** _новое_имя_пользователя_ задает новое имя для этого пользователя. Аргумент *newUserName* еще не должен существовать в текущей базе данных.

 DEFAULT_SCHEMA **=** { *имя_схемы* | NULL } задает первую схему для поиска сервером, когда он разрешает имена объектов для этого пользователя. При установке схемы по умолчанию в значение NULL схема по умолчанию удаляется из группы Windows. Параметр NULL нельзя использовать с пользователем Windows.

 PASSWORD **=** '*пароль*'  **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Указывает пароль для пользователя, которого необходимо изменить. В паролях учитывается регистр символов.

> [!NOTE]
> Данный параметр доступен только для содержащихся пользователей. Дополнительные сведения см. в разделах [Автономные базы данных](../../relational-databases/databases/contained-databases.md) и [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'старый_пароль'_ **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Текущий пароль пользователя, который будет заменен на "*password*". В паролях учитывается регистр символов. Параметр *OLD_PASSWORD* является обязательным для изменения пароля. Изменить пароль без этого параметра можно только при наличии разрешения **ALTER ANY USER**. Необходимость указать *OLD_PASSWORD* не позволит пользователям с разрешением **IMPERSONATION** изменить пароль.

> [!NOTE]
> Данный параметр доступен только для содержащихся пользователей.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Отключает проверки шифрованных метаданных на сервере в операциях массового копирования. Это позволяет пользователю массово копировать зашифрованные данные между таблицами или базами данных без расшифровки данных. Значение по умолчанию — OFF.

> [!WARNING]
> Неправильное использование этого параметра может привести к повреждению данных. Дополнительные сведения см. в разделе [Перенос конфиденциальных данных с помощью функции Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Remarks

 Схемой по умолчанию будет первая схема, которую найдет сервер, после того как получит имена объектов для данного пользователя базы данных. Если не указано иное, схемой по умолчанию будет владелец объектов, создаваемых этим пользователем базы данных.

 Если пользователь имеет схему по умолчанию, будет использоваться эта схема. Если у пользователя нет схемы по умолчанию, но он является членом группы, которая имеет схему по умолчанию, используется схема по умолчанию группы. Если пользователь не имеет схемы по умолчанию и является членом нескольких групп, схемой по умолчанию для этого пользователя будет схема по умолчанию группы Windows с минимальным значением principal_id и явно заданной схемой по умолчанию. Если для пользователя нельзя определить схему по умолчанию, будет использоваться схема **dbo**.

 В качестве значения DEFAULT_SCHEMA можно задать схему, которая в настоящий момент не существует в базе данных. Поэтому значение DEFAULT_SCHEMA можно определить для пользователя еще до создания схемы.

 Значение DEFAULT_SCHEMA не может быть указано для пользователя, сопоставленного с сертификатом или асимметричным ключом.

> [!IMPORTANT]
> Значение параметра DEFAULT_SCHEMA не учитывается, если пользователь является членом предопределенной роли сервера **sysadmin**. Для всех членов роли сервера **sysadmin** по умолчанию установлена схема `dbo`.

 Имя пользователя, сопоставленное с именем входа или группой Windows, можно изменить только в том случае, если значение идентификатора безопасности имени нового пользователя совпадает с уже записанным в базе данных. Такая проверка предотвращает имитацию подключения к базе данных с помощью имен входа Windows.

 Предложение WITH LOGIN позволяет сопоставить пользователя с другим именем входа. Пользователи, не имеющие имени входа, а также пользователи, сопоставленные с сертификатом или с асимметричным ключом, не могут быть повторно сопоставлены с помощью этого предложения. Возможно повторное сопоставление только пользователей (или групп) SQL и Windows. Предложение WITH LOGIN не удастся использовать для изменения типа пользователя, например для перехода от учетной записи Windows на имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Имя пользователя будет автоматически изменено на имя входа, если удовлетворяются следующие условия.

- Пользователь является пользователем Windows.

- Имя является именем Windows (содержит обратную косую черту).

- Новое имя не было указано.

- Текущее имя отличается от имени входа.

 В противном случае пользователь не будет переименован, если только вызывающий не вызовет дополнительно предложение NAME.

Имя пользователя, сопоставленное с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], сертификатом или асимметричным ключом, не может содержать обратную косую черту (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Безопасность

> [!NOTE]
> Пользователь с разрешением **ALTER ANY USER** может изменить схему по умолчанию для любого пользователя. Пользователь с измененной схемой может производить выборку данных неизвестным образом из неправильной таблицы или выполнять код из неправильной схемы.

### <a name="permissions"></a>Разрешения

 Чтобы изменить имя пользователя, необходимо разрешение **ALTER ANY USER** для базы данных.

 Чтобы изменить целевое имя входа пользователя, необходимо разрешение **CONTROL** в базе данных.

 Чтобы изменить имя пользователя с разрешением **CONTROL** в базе данных, требуется разрешение **CONTROL** в базе данных.

 Чтобы изменить схему или язык по умолчанию, требуется разрешение **ALTER** на пользователя. Пользователь может изменить свою собственную схему или язык по умолчанию.

## <a name="examples"></a>Примеры

Все примеры выполняются в базе данных пользователя.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Изменение имени пользователя базы данных

 В следующем примере показано изменение имени пользователя базы данных с `Mary5` на `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>Б. Изменение схемы пользователя, используемой по умолчанию

 В данном примере показано изменение схемы пользователя, используемой по умолчанию, с `Mary51` на `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>В. Изменение нескольких параметров одновременно

 В следующем примере несколько параметров пользователя автономной базы данных изменяются одной инструкцией.

```sql
ALTER USER Philip
WITHNAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per';
GO
```

## <a name="see-also"></a>См. также раздел

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [Автономные базы данных](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Управляемый экземпляр SQL \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Управляемый экземпляр SQL Azure

## <a name="syntax"></a>Синтаксис

> [!IMPORTANT]
> Для Управляемого экземпляра SQL Azure поддерживаются только следующие параметры при применении к пользователям с именами входа Azure AD: `DEFAULT_SCHEMA = { schemaName | NULL }` и `DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }`
> </br> </br> Для сопоставления пользователей в базе данных, которая была перенесена в Управляемый экземпляр SQL Azure, было добавлено новое расширение синтаксиса. Синтаксис ALTER USER позволяет сопоставлять пользователей базы данных в объединенном и синхронизированном домене с Azure AD с именами входа Azure AD.

```syntaxsql
-- Syntax for SQL Managed Instance
ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- Users or groups that are migrated as federated and synchronized with Azure AD have the following syntax:

/** Applies to Windows users that were migrated and have the following user names:
- Windows user <domain\user>
- Windows group <domain\MyWindowsGroup>
- Windows alias <MyWindowsAlias>
**/

ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
 NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
```

## <a name="arguments"></a>Аргументы

 *userName* указывает имя, по которому пользователь идентифицируется в этой базе данных.

 LOGIN **=** _имя_для_входа_ сопоставляет пользователя с другим именем входа, заменив идентификатор безопасности пользователя для соответствия идентификатору безопасности имени входа.

 Если ALTER USER является единственной инструкцией в пакете SQL, предложение WITH LOGIN поддерживается в Базе данных SQL Azure. Если инструкция ALTER USER не единственная в пакете SQL или выполняется в динамическом коде SQL, предложение WITH LOGIN не поддерживается.

 NAME **=** _новое_имя_пользователя_ задает новое имя для этого пользователя. Аргумент *newUserName* еще не должен существовать в текущей базе данных.

 DEFAULT_SCHEMA **=** { *имя_схемы* | NULL } задает первую схему для поиска сервером, когда он разрешает имена объектов для этого пользователя. При установке схемы по умолчанию в значение NULL схема по умолчанию удаляется из группы Windows. Параметр NULL нельзя использовать с пользователем Windows.

 PASSWORD **=** '*password*'

 Указывает пароль для пользователя, которого необходимо изменить. В паролях учитывается регистр символов.

> [!NOTE]
> Данный параметр доступен только для содержащихся пользователей. Дополнительные сведения см. в разделах [Автономные базы данных](../../relational-databases/databases/contained-databases.md) и [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'старый_пароль'_

 Текущий пароль пользователя, который будет заменен на "*password*". В паролях учитывается регистр символов. Параметр *OLD_PASSWORD* является обязательным для изменения пароля. Изменить пароль без этого параметра можно только при наличии разрешения **ALTER ANY USER**. Необходимость указать *OLD_PASSWORD* не позволит пользователям с разрешением **IMPERSONATION** изменить пароль.

> [!NOTE]
> Данный параметр доступен только для содержащихся пользователей.

 DEFAULT_LANGUAGE **=** _{ NONE | \<lcid> | \<language name> | \<language alias> }_

 Указывает язык по умолчанию, присвоенный пользователю. Если этот параметр установлен в значение NONE, то в качестве языка по умолчанию выбирается текущий язык по умолчанию для базы данных. При последующей смене языка по умолчанию для базы данных язык по умолчанию для пользователя не меняется. Аргумент *DEFAULT_LANGUAGE* может быть идентификатором локали (lcid), названием языка или псевдонимом языка.

> [!NOTE]
> Этот параметр можно задать только в автономной базе данных и только для содержащихся пользователей.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]

 Отключает проверки шифрованных метаданных на сервере в операциях массового копирования. Это позволяет пользователю массово копировать зашифрованные данные между таблицами или базами данных без расшифровки данных. Значение по умолчанию — OFF.

> [!WARNING]
> Неправильное использование этого параметра может привести к повреждению данных. Дополнительные сведения см. в разделе [Перенос конфиденциальных данных с помощью функции Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Remarks

 Схемой по умолчанию будет первая схема, которую найдет сервер, после того как получит имена объектов для данного пользователя базы данных. Если не указано иное, схемой по умолчанию будет владелец объектов, создаваемых этим пользователем базы данных.

 Если пользователь имеет схему по умолчанию, будет использоваться эта схема. Если у пользователя нет схемы по умолчанию, но он является членом группы, которая имеет схему по умолчанию, используется схема по умолчанию группы. Если пользователь не имеет схемы по умолчанию и является членом нескольких групп, схемой по умолчанию для этого пользователя будет схема по умолчанию группы Windows с минимальным значением principal_id и явно заданной схемой по умолчанию. Если для пользователя нельзя определить схему по умолчанию, будет использоваться схема **dbo**.

 В качестве значения DEFAULT_SCHEMA можно задать схему, которая в настоящий момент не существует в базе данных. Поэтому значение DEFAULT_SCHEMA можно определить для пользователя еще до создания схемы.

 Значение DEFAULT_SCHEMA не может быть указано для пользователя, сопоставленного с сертификатом или асимметричным ключом.

> [!IMPORTANT]
> Значение параметра DEFAULT_SCHEMA не учитывается, если пользователь является членом предопределенной роли сервера **sysadmin**. Для всех членов роли сервера **sysadmin** по умолчанию установлена схема `dbo`.

 Имя пользователя, сопоставленное с именем входа или группой Windows, можно изменить только в том случае, если значение идентификатора безопасности имени нового пользователя совпадает с уже записанным в базе данных. Такая проверка предотвращает имитацию подключения к базе данных с помощью имен входа Windows.

 Предложение WITH LOGIN позволяет сопоставить пользователя с другим именем входа. Пользователи, не имеющие имени входа, а также пользователи, сопоставленные с сертификатом или с асимметричным ключом, не могут быть повторно сопоставлены с помощью этого предложения. Возможно повторное сопоставление только пользователей (или групп) SQL и Windows. Предложение WITH LOGIN не удастся использовать для изменения типа пользователя, например для перехода от учетной записи Windows на имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Единственным исключением является изменение пользователя Windows на пользователя Azure AD.

> [!NOTE]
> Указанные ниже правила не применяются к пользователям Windows в Управляемом экземпляре SQL Azure, так как мы не поддерживаем создание имен входа Windows в Управляемом экземпляре SQL Azure. Параметр WITH LOGIN можно использовать только при наличии имен входа Azure AD.

 Имя пользователя будет автоматически изменено на имя входа, если удовлетворяются следующие условия.

- Пользователь является пользователем Windows.

- Имя является именем Windows (содержит обратную косую черту).

- Новое имя не было указано.

- Текущее имя отличается от имени входа.

 В противном случае пользователь не будет переименован, если только вызывающий не вызовет дополнительно предложение NAME.

Имя пользователя, сопоставленное с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], сертификатом или асимметричным ключом, не может содержать обратную косую черту (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

### <a name="remarks-for-windows-users-in-sql-on-premises-migrated-to-azure-sql-managed-instance"></a>Примечания для пользователей Windows в локальной среде SQL, перенесенных в Управляемый экземпляр SQL Azure

Эти замечания относятся к проверке подлинности в качестве пользователей Windows, которые были объединены и синхронизированы с Azure AD.

> [!NOTE]
> Изменились функции, доступные администратору Azure AD для Управляемого экземпляра SQL Azure после создания. Дополнительные сведения см. в разделе [Новые функции администратора Azure AD для MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

- Проверка пользователей или групп Windows, сопоставленных с Azure AD, выполняется по умолчанию с помощью API Graph во всех версиях синтаксиса ALTER USER, используемого для миграции.
- Локальные пользователи, которым были назначены псевдонимы (другое имя, помимо имени исходной учетной записи Windows), будут иметь имя с псевдонимом.
- При проверке подлинности Azure AD параметр LOGIN применяется только к Управляемому экземпляру SQL Azure и не может использоваться с базой данных SQL.
- Чтобы просмотреть имена входа для субъектов Azure AD, используйте следующую команду: `select * from sys.server_principals`.
- Проверьте, что указанный тип имени входа имеет значение `E` или `X`.
- Параметр PASSWORD нельзя использовать для пользователей Azure AD.
- Во всех случаях миграции роли и разрешения пользователей или групп Windows будут автоматически переданы новым пользователям или группам Azure AD.
- Новое расширение синтаксиса, **FROM EXTERNAL PROVIDER**, доступно для изменения пользователей и групп Windows из локальной среды SQL на пользователей и группы Azure AD. Домен Windows должен быть объединен с Azure AD, а при использовании этого расширения все члены домена Windows должны существовать в Azure AD. Синтаксис **FROM EXTERNAL PROVIDER** применяется к Управляемому экземпляру SQL Azure и должен использоваться в случае, если пользователи Windows не имеют имен входа в исходном экземпляре SQL и их следует сопоставлять с автономными пользователями базы данных Azure AD.
- В этом случае допустимое userName может быть следующим:
- Пользователь Widows (_домен\пользователь_).
- Группа Windows (_MyWidnowsGroups_).
- Псевдоним Windows (_MyWindowsAlias_).
- Результат команды ALTER заменяет старое userName соответствующим именем, которое найдено в Azure AD на основе исходного идентификатора безопасности старого userName. Измененное имя заменяется и сохраняется в метаданных базы данных:
- (_домен\пользователь_) будет заменено на Azure AD user@domain.com.
- (_домен\\MyWidnowsGroup_) будет заменено на группу Azure AD.
- (_MyWindowsAlias_) останется без изменений, но идентификатор безопасности этого пользователя будет проверен в Azure AD.

> [!NOTE]
> Если идентификатор безопасности исходного пользователя, преобразованный в objectID, не удается найти в Azure AD, команда ALTER USER завершится ошибкой.

- Чтобы просмотреть измененных пользователей, используйте следующую команду: `select * from sys.database_principals`
- Проверьте указанный тип пользователя `E` или `X`.
- Если для миграции пользователей Windows в пользователи Azure AD используется синтаксис NAME, действуют следующие ограничения.
- Необходимо указать допустимый LOGIN.
- Параметр NAME будет проверен Azure AD и может быть только следующим:
- Имя LOGIN.
- Псевдоним — имя не может существовать в Azure AD.
- Во всех остальных случаях синтаксис завершится ошибкой.

## <a name="security"></a>Безопасность

> [!NOTE]
> Пользователь с разрешением **ALTER ANY USER** может изменить схему по умолчанию для любого пользователя. Пользователь с измененной схемой может производить выборку данных неизвестным образом из неправильной таблицы или выполнять код из неправильной схемы.

### <a name="permissions"></a>Разрешения

 Чтобы изменить имя пользователя, необходимо разрешение **ALTER ANY USER** для базы данных.

 Чтобы изменить целевое имя входа пользователя, необходимо разрешение **CONTROL** в базе данных.

 Чтобы изменить имя пользователя с разрешением **CONTROL** в базе данных, требуется разрешение **CONTROL** в базе данных.

 Чтобы изменить схему или язык по умолчанию, требуется разрешение **ALTER** на пользователя. Пользователь может изменить свою собственную схему или язык по умолчанию.

## <a name="examples"></a>Примеры

Все примеры выполняются в базе данных пользователя.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Изменение имени пользователя базы данных

 В следующем примере показано изменение имени пользователя базы данных с `Mary5` на `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>Б. Изменение схемы пользователя, используемой по умолчанию

 В данном примере показано изменение схемы пользователя, используемой по умолчанию, с `Mary51` на `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>В. Изменение нескольких параметров одновременно

 В следующем примере несколько параметров пользователя автономной базы данных изменяются одной инструкцией.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

### <a name="d-map-the-user-in-the-database-to-an-azure-ad-login-after-migration"></a>Г. После миграции сопоставьте пользователя в базе данных с именем входа Azure AD

В следующем примере показано повторное сопоставление пользователя `westus/joe` с пользователем Azure AD, `joe@westus.com`. Этот пример предназначен для имен входа, уже существующих в управляемом экземпляре. Это необходимо сделать после завершения миграции базы данных на Управляемый экземпляр SQL Azure и в том случае, когда есть необходимость использовать имя входа Azure AD для проверки подлинности.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com
```

### <a name="e-map-an-old-windows-user-in-the-database-without-a-login-in-azure-sql-managed-instance-to-an-azure-ad-user"></a>Д. Сопоставление старого пользователя Windows в базе данных без имени входа в Управляемом экземпляре SQL Azure с пользователем Azure AD.

В следующем примере показано повторное сопоставление пользователя, `westus/joe` без имени входа, с пользователем Azure AD, `joe@westus.com`. Федеративный пользователь должен существовать в Azure AD.

```sql
ALTER USER [westus/joe] FROM EXTERNAL PROVIDER
```

### <a name="f-map-the-user-alias-to-an-existing-azure-ad-login"></a>Е. Сопоставление псевдонима пользователя с существующим именем входа Azure AD.

В следующем примере показано повторное сопоставление имени пользователя, `westus\joe` с `joe_alias`. В этом случае соответствующая учетная запись Azure AD — это `joe@westus.com`.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com, name= joe_alias
```

### <a name="g-map-a-windows-group-that-was-migrated-in-azure-sql-managed-instance-to-an-azure-ad-group"></a>Ж. Сопоставление группы Windows, перенесенной в Управляемый экземпляр SQL Azure, с группой Azure AD

В следующем примере выполняется повторное сопоставление старой локальной группы `westus\mygroup` с группой Azure AD `mygroup` в управляемом экземпляре. Эта группа должна существовать в Azure AD.

```sql
ALTER USER [westus\mygroup] WITH LOGIN = mygroup
```

## <a name="see-also"></a>См. также раздел

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [Автономные базы данных](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)
- [Руководство. Миграция локальных пользователей и групп Windows в SQL Server в Управляемый экземпляр SQL с помощью синтаксиса T-SQL DDL](/azure/sql-database/tutorial-managed-instance-azure-active-directory-migration)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for Azure Synapse

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Аргументы

 *userName* указывает имя, по которому пользователь идентифицируется в этой базе данных.

 LOGIN **=** _имя_для_входа_ сопоставляет пользователя с другим именем входа, заменив идентификатор безопасности пользователя для соответствия идентификатору безопасности имени входа.

 Если ALTER USER является единственной инструкцией в пакете SQL, предложение WITH LOGIN поддерживается в Базе данных SQL Azure. Если инструкция ALTER USER не единственная в пакете SQL или выполняется в динамическом коде SQL, предложение WITH LOGIN не поддерживается.

 NAME **=** _новое_имя_пользователя_ задает новое имя для этого пользователя. Аргумент *newUserName* еще не должен существовать в текущей базе данных.

 DEFAULT_SCHEMA **=** { *имя_схемы* | NULL } задает первую схему для поиска сервером, когда он разрешает имена объектов для этого пользователя. При установке схемы по умолчанию в значение NULL схема по умолчанию удаляется из группы Windows. Параметр NULL нельзя использовать с пользователем Windows.

## <a name="remarks"></a>Remarks

 Схемой по умолчанию будет первая схема, которую найдет сервер, после того как получит имена объектов для данного пользователя базы данных. Если не указано иное, схемой по умолчанию будет владелец объектов, создаваемых этим пользователем базы данных.

 Если пользователь имеет схему по умолчанию, будет использоваться эта схема. Если у пользователя нет схемы по умолчанию, но он является членом группы, которая имеет схему по умолчанию, используется схема по умолчанию группы. Если пользователь не имеет схемы по умолчанию и является членом нескольких групп, схемой по умолчанию для этого пользователя будет схема по умолчанию группы Windows с минимальным значением principal_id и явно заданной схемой по умолчанию. Если для пользователя нельзя определить схему по умолчанию, будет использоваться схема **dbo**.

 В качестве значения DEFAULT_SCHEMA можно задать схему, которая в настоящий момент не существует в базе данных. Поэтому значение DEFAULT_SCHEMA можно определить для пользователя еще до создания схемы.

 Значение DEFAULT_SCHEMA не может быть указано для пользователя, сопоставленного с сертификатом или асимметричным ключом.

> [!IMPORTANT]
> Значение параметра DEFAULT_SCHEMA не учитывается, если пользователь является членом предопределенной роли сервера **sysadmin**. Для всех членов роли сервера **sysadmin** по умолчанию установлена схема `dbo`.

 Предложение WITH LOGIN позволяет сопоставить пользователя с другим именем входа. Пользователи, не имеющие имени входа, а также пользователи, сопоставленные с сертификатом или с асимметричным ключом, не могут быть повторно сопоставлены с помощью этого предложения. Возможно повторное сопоставление только пользователей (или групп) SQL и Windows. Предложение WITH LOGIN не удастся использовать для изменения типа пользователя, например для перехода от учетной записи Windows на имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Имя пользователя будет автоматически изменено на имя входа, если удовлетворяются следующие условия.

- Новое имя не было указано.

- Текущее имя отличается от имени входа.

 В противном случае пользователь не будет переименован, если только вызывающий не вызовет дополнительно предложение NAME.

Имя пользователя, сопоставленное с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], сертификатом или асимметричным ключом, не может содержать обратную косую черту (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Безопасность

> [!NOTE]
> Пользователь с разрешением **ALTER ANY USER** может изменить схему по умолчанию для любого пользователя. Пользователь с измененной схемой может производить выборку данных неизвестным образом из неправильной таблицы или выполнять код из неправильной схемы.

### <a name="permissions"></a>Разрешения

 Чтобы изменить имя пользователя, необходимо разрешение **ALTER ANY USER** для базы данных.

 Чтобы изменить целевое имя входа пользователя, необходимо разрешение **CONTROL** в базе данных.

 Чтобы изменить имя пользователя с разрешением **CONTROL** в базе данных, требуется разрешение **CONTROL** в базе данных.

 Чтобы изменить схему или язык по умолчанию, требуется разрешение **ALTER** на пользователя. Пользователь может изменить свою собственную схему или язык по умолчанию.

## <a name="examples"></a>Примеры

Все примеры выполняются в базе данных пользователя.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Изменение имени пользователя базы данных

 В следующем примере показано изменение имени пользователя базы данных с `Mary5` на `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>Б. Изменение схемы пользователя, используемой по умолчанию

В данном примере показано изменение схемы пользователя, используемой по умолчанию, с `Mary51` на `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>См. также раздел

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [Автономные базы данных](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
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

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Аргументы

 *userName* указывает имя, по которому пользователь идентифицируется в этой базе данных.

 LOGIN **=** _имя_для_входа_ сопоставляет пользователя с другим именем входа, заменив идентификатор безопасности пользователя для соответствия идентификатору безопасности имени входа.

 Если ALTER USER является единственной инструкцией в пакете SQL, предложение WITH LOGIN поддерживается в Базе данных SQL Azure. Если инструкция ALTER USER не единственная в пакете SQL или выполняется в динамическом коде SQL, предложение WITH LOGIN не поддерживается.

 NAME **=** _новое_имя_пользователя_ задает новое имя для этого пользователя. Аргумент *newUserName* еще не должен существовать в текущей базе данных.

 DEFAULT_SCHEMA **=** { *имя_схемы* | NULL } задает первую схему для поиска сервером, когда он разрешает имена объектов для этого пользователя. При установке схемы по умолчанию в значение NULL схема по умолчанию удаляется из группы Windows. Параметр NULL нельзя использовать с пользователем Windows.

## <a name="remarks"></a>Remarks

 Схемой по умолчанию будет первая схема, которую найдет сервер, после того как получит имена объектов для данного пользователя базы данных. Если не указано иное, схемой по умолчанию будет владелец объектов, создаваемых этим пользователем базы данных.

 Если пользователь имеет схему по умолчанию, будет использоваться эта схема. Если у пользователя нет схемы по умолчанию, но он является членом группы, которая имеет схему по умолчанию, используется схема по умолчанию группы. Если пользователь не имеет схемы по умолчанию и является членом нескольких групп, схемой по умолчанию для этого пользователя будет схема по умолчанию группы Windows с минимальным значением principal_id и явно заданной схемой по умолчанию. Если для пользователя нельзя определить схему по умолчанию, будет использоваться схема **dbo**.

 В качестве значения DEFAULT_SCHEMA можно задать схему, которая в настоящий момент не существует в базе данных. Поэтому значение DEFAULT_SCHEMA можно определить для пользователя еще до создания схемы.

 Значение DEFAULT_SCHEMA не может быть указано для пользователя, сопоставленного с сертификатом или асимметричным ключом.

> [!IMPORTANT]
> Значение параметра DEFAULT_SCHEMA не учитывается, если пользователь является членом предопределенной роли сервера **sysadmin**. Для всех членов роли сервера **sysadmin** по умолчанию установлена схема `dbo`.

 Предложение WITH LOGIN позволяет сопоставить пользователя с другим именем входа. Пользователи, не имеющие имени входа, а также пользователи, сопоставленные с сертификатом или с асимметричным ключом, не могут быть повторно сопоставлены с помощью этого предложения. Возможно повторное сопоставление только пользователей (или групп) SQL и Windows. Предложение WITH LOGIN не удастся использовать для изменения типа пользователя, например для перехода от учетной записи Windows на имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Имя пользователя будет автоматически изменено на имя входа, если удовлетворяются следующие условия.

- Новое имя не было указано.

- Текущее имя отличается от имени входа.

 В противном случае пользователь не будет переименован, если только вызывающий не вызовет дополнительно предложение NAME.

Имя пользователя, сопоставленное с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], сертификатом или асимметричным ключом, не может содержать обратную косую черту (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Безопасность

> [!NOTE]
> Пользователь с разрешением **ALTER ANY USER** может изменить схему по умолчанию для любого пользователя. Пользователь с измененной схемой может производить выборку данных неизвестным образом из неправильной таблицы или выполнять код из неправильной схемы.

### <a name="permissions"></a>Разрешения

 Чтобы изменить имя пользователя, необходимо разрешение **ALTER ANY USER** для базы данных.

 Чтобы изменить целевое имя входа пользователя, необходимо разрешение **CONTROL** в базе данных.

 Чтобы изменить имя пользователя с разрешением **CONTROL** в базе данных, требуется разрешение **CONTROL** в базе данных.

 Чтобы изменить схему или язык по умолчанию, требуется разрешение **ALTER** на пользователя. Пользователь может изменить свою собственную схему или язык по умолчанию.

## <a name="examples"></a>Примеры

Все примеры выполняются в базе данных пользователя.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Изменение имени пользователя базы данных

 В следующем примере показано изменение имени пользователя базы данных с `Mary5` на `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>Б. Изменение схемы пользователя, используемой по умолчанию
 В данном примере показано изменение схемы пользователя, используемой по умолчанию, с `Mary51` на `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>См. также раздел

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [Автономные базы данных](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
