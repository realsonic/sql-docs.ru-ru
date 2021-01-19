---
description: CREATE USER (Transact-SQL)
title: CREATE USER (Transact-SQL)
ms.custom: ''
ms.date: 11/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WITHOUT_LOGIN_TSQL
- CREATE_USER_TSQL
- SQL13.SWB.DATABASEUSER.OWNEDSCHEMAS.F1
- WITHOUT LOGIN
- CREATE USER
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- adding users
- WITHOUT LOGIN [SQL Server]
- CREATE USER statement
- database user additions [SQL Server]
- USER WITHOUT LOGIN [SQL Server]
- users [SQL Server], adding
- users [SQL Server]
ms.assetid: 01de7476-4b25-4d58-85b7-1118fe64aa80
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 446e9fa7dc252da75bc1c14ecf3fbe0fdf92b3e8
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170326"
---
# <a name="create-user-transact-sql"></a>CREATE USER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Добавляет нового пользователя в текущую базу данных. Ниже перечислены 12 типов пользователей с примером базового синтаксиса.  
  
**Пользователи с именем входа в базе данных master**. Это самый распространенный тип пользователей.  
  
-   Пользователь с именем входа, задаваемым по учетной записи Windows Active Directory. `CREATE USER [Contoso\Fritz];`     
-   Пользователь с именем входа, задаваемым по группе Windows. `CREATE USER [Contoso\Sales];`   
-   Пользователь с именем входа для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `CREATE USER Mary;`  
  
**Пользователи, проходящие проверку подлинности в базе данных**. Рекомендуется для повышения переносимости базы данных.  
 Всегда разрешается в [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Разрешается только в автономной базе данных в [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].  
  
-   Пользователь, соответствующий пользователю Windows без имени входа. `CREATE USER [Contoso\Fritz];`    
-   Пользователь, соответствующий группе Windows без имени входа. `CREATE USER [Contoso\Sales];`  
-   Пользователь в [!INCLUDE[ssSDS](../../includes/sssds-md.md)] или [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] на основе пользователя Azure Active Directory. `CREATE USER [Fritz@contoso.com] FROM EXTERNAL PROVIDER;`     

-   Пользователь автономной базы данных с паролем. (Недоступно в [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].) `CREATE USER Mary WITH PASSWORD = '********';`   
  
**Пользователи, соответствующие субъектам Windows, которые подключаются с помощью имени входа группы Windows**  
  
-   Пользователь, соответствующий пользователю Windows, который не имеет имени входа, но может подключаться к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] за счет членства в роли Windows. `CREATE USER [Contoso\Fritz];`  
  
-   Пользователь, соответствующий группе Windows, которая не имеет имени входа, но может подключаться к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] за счет членства в другой роли Windows. `CREATE USER [Contoso\Fritz];`  
  
**Пользователи, которые не могут проходить проверку подлинности**. Такие пользователи не могут входить в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
-   Пользователь без имени входа. Не может выполнить вход, но ему можно предоставлять разрешения. `CREATE USER CustomApp WITHOUT LOGIN;`    
-   Пользователь, связанный с сертификатом. Не может выполнить вход, но может предоставлять разрешения и подписывать модули. `CREATE USER TestProcess FOR CERTIFICATE CarnationProduction50;`  
-   Пользователь, связанный с асимметричным ключом. Не может выполнить вход, но может предоставлять разрешения и подписывать модули. `CREATE User TestProcess FROM ASYMMETRIC KEY PacificSales09;`   
 
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure SQL Managed Instance
  
-- Syntax Users based on logins in master  
CREATE USER user_name   
    [   
        { FOR | FROM } LOGIN login_name   
    ]  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that authenticate at the database  
CREATE USER   
    {  
      windows_principal [ WITH <options_list> [ ,... ] ]  
  
    | user_name WITH PASSWORD = 'password' [ , <options_list> [ ,... ]   
    | Azure_Active_Directory_principal FROM EXTERNAL PROVIDER   
    }  
  
 [ ; ]  
  
-- Users based on Windows principals that connect through Windows group logins  
CREATE USER   
    {   
          windows_principal [ { FOR | FROM } LOGIN windows_principal ]  
        | user_name { FOR | FROM } LOGIN windows_principal  
}  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that cannot authenticate   
CREATE USER user_name   
    {  
         WITHOUT LOGIN [ WITH <limited_options_list> [ ,... ] ]  
       | { FOR | FROM } CERTIFICATE cert_name   
       | { FOR | FROM } ASYMMETRIC KEY asym_key_name   
    }  
 [ ; ]  
  
<options_list> ::=  
      DEFAULT_SCHEMA = schema_name  
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }  
    | SID = sid   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name ]   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
-- SQL Database syntax when connected to a federation member  
CREATE USER user_name  
[;]

-- Syntax for users based on Azure AD logins for Azure SQL Managed Instance
CREATE USER user_name   
    [   { FOR | FROM } LOGIN login_name  ]  
    | FROM EXTERNAL PROVIDER
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  

<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name 
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ] 
```

> [!NOTE]
> Изменились функции, доступные администратору Azure AD для Управляемого экземпляра SQL Azure после создания. Дополнительные сведения см. в разделе [Новые функции администратора Azure AD для MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

```syntaxsql
-- Syntax for Azure Synapse Analytics  
  
CREATE USER user_name   
    [ { { FOR | FROM } { LOGIN login_name }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]

CREATE USER Azure_Active_Directory_principal FROM EXTERNAL PROVIDER  
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]
``` 
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CREATE USER user_name   
    [ { { FOR | FROM }  
      {   
        LOGIN login_name   
      }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *user_name*  
 Указывает имя, по которому пользователь идентифицируется в этой базе данных. *user_name* — это **sysname**. Он может иметь длину до 128 символов. Когда создается пользователь, соответствующий участнику Windows, именем пользователя становится имя участника Windows, если не указано другое имя.  
  
 LOGIN *login_name*  
 Указывает имя входа, для которого создается пользователь базы данных. *login_name* должен быть допустимым именем входа на сервере. Может быть именем входа, соответствующим участнику Windows (пользователю или группе) или именем входа для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Когда это имя входа SQL Server входит в базу данных, оно получает имя и идентификатор создаваемого пользователя базы данных. При создании имени входа, сопоставленного с субъектом Windows, используйте формат **[** _\<domainName\>_ **\\** _\<loginName\>_ **]** . Примеры см. в разделе [Сводка синтаксиса](#SyntaxSummary).  
  
 Если CREATE USER является единственной инструкцией в пакете SQL, предложение WITH LOGIN поддерживается в Базе данных SQL Azure. Если инструкция CREATE USER не единственная в пакете SQL или выполняется в динамическом коде SQL, предложение WITH LOGIN не поддерживается.  
  
 WITH DEFAULT_SCHEMA = *schema_name*  
 Указывает первую схему, которую найдет сервер, после того, как он получит имена объектов для пользователя данной базы данных.  
  
 '*windows_principal*'  
 Указывает участника Windows, для которого создается пользователь базы данных. *windows_principal* может быть пользователем Windows или группой Windows. Пользователь будет создаваться даже в случае, если для *windows_principal* отсутствует имя входа. Если при подключении к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для *windows_principal* отсутствует имя входа,то субъект Windows должен пройти проверку подлинности в компоненте [!INCLUDE[ssDE](../../includes/ssde-md.md)] за счет членства в группе Windows, имеющей имя входа, либо в строке подключения в качестве исходного каталога должна указываться автономная база данных. При создании пользователя, сопоставленного с субъектом Windows, используйте формат **[** _\<domainName\>_ **\\** _\<loginName\>_ **]** . Примеры см. в разделе [Сводка синтаксиса](#SyntaxSummary). Имена пользователей, основанные на пользователях Active Directory, могут иметь не более 21 символа в длину.
  
 '*Azure_Active_Directory_principal*'  
 **Применимо к**: [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].  
  
 Указывает субъект Azure Active Directory, для которого создается пользователь базы данных. *Azure_Active_Directory_principal* может быть пользователем Azure Active Directory, группой Azure Active Directory или приложением Azure Active Directory. (Пользователи Azure Active Directory не могут иметь имена входа для проверки подлинности Windows [!INCLUDE[ssSDS](../../includes/sssds-md.md)]; только пользователи базы данных.) В строке подключения необходимо указать автономную базу данных в качестве исходного каталога.

 Для субъектов Azure AD действуют такие требования синтаксиса CREATE USER:

- имя участника-пользователя (UserPrincipalName) объекта Azure AD для пользователей Azure AD;

  - `CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;`  
  - `CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;`

- отображаемое имя (DisplayName) объекта Azure AD для групп и приложений Azure AD. Для группы безопасности *Nurses* будет отображаться следующее:  
  
  - `CREATE USER [Nurses] FROM EXTERNAL PROVIDER;`  
  
 См. дополнительные сведения об [использовании аутентификации Azure Active Directory для аутентификации с помощью SQL](/azure/azure-sql/database/authentication-aad-overview).  
  
WITH PASSWORD = '*password*'  
 **Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Может использоваться только в автономной базе данных. Задает пароль для создаваемого пользователя. Начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] сохраненные сведения о пароле вычисляются с помощью SHA-512 соленого пароля.  
  
WITHOUT LOGIN  
 Указывает, что пользователь не должен сопоставляться с существующим именем входа.  
  
CERTIFICATE *cert_name*  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Указывает сертификат, для которого создается пользователь базы данных.  
  
ASYMMETRIC KEY *asym_key_name*  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Указывает асимметричный ключ, для которого создается пользователь базы данных.  
  
DEFAULT_LANGUAGE = *{ NONE \| \<lcid> \| \<language name> \| \<language salias> }*  
 **Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Задает язык по умолчанию для нового пользователя. Если для пользователя задается язык по умолчанию, а затем язык базы данных по умолчанию изменяется, то язык по умолчанию для пользователя сохраняет указанное значение. Если язык по умолчанию не указывается, то языком по умолчанию для пользователя становится язык по умолчанию для базы данных. Если язык по умолчанию для пользователя не указывается, а язык по умолчанию для базы данных изменяется после создания пользователя, то язык по умолчанию для пользователя меняется на новый язык по умолчанию для базы данных.  
  
> [!IMPORTANT]  
>  Аргумент *DEFAULT_LANGUAGE* используется только для пользователя автономной базы данных.  
  
SID = *sid*  
 **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 Применимо только для пользователей с паролями (проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) в автономной базе данных. Указывает идентификатор SID нового пользователя базы данных. Если этот параметр не выбран, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] назначает идентификатор SID автоматически. Используйте параметр идентификатора SID для создания пользователей в нескольких базах данных с одинаковыми идентификаторами SID. Это удобно при создании пользователей в нескольких базах данных для подготовки обработки отказа AlwaysOn. Чтобы определить идентификатор SID пользователя, выполните запрос sys.database_principals.  
  
ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  
 **Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Отключает проверки шифрованных метаданных на сервере в операциях массового копирования. Это позволяет пользователю массово копировать зашифрованные данные между таблицами или базами данных без расшифровки данных. Значение по умолчанию — OFF.  
  
> [!WARNING]  
>  Неправильное использование этого параметра может привести к повреждению данных. Дополнительные сведения см. в разделе [Перенос конфиденциальных данных с помощью функции Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
## <a name="remarks"></a>Remarks  
 Если предложение FOR LOGIN не указано, новый пользователь базы данных будет сопоставлен с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], имеющим такое же имя.  
  
 Схемой по умолчанию будет первая схема, которую найдет сервер, после того как получит имена объектов для данного пользователя базы данных. Если не указано иное, схемой по умолчанию будет владелец объектов, создаваемых этим пользователем базы данных.  
  
 Если пользователь имеет схему по умолчанию, будет использоваться эта схема. Если у пользователя нет схемы по умолчанию, но он является членом группы, которая имеет схему по умолчанию, используется схема по умолчанию группы. Если пользователь не имеет схемы по умолчанию и является членом нескольких групп, схемой по умолчанию для этого пользователя будет схема по умолчанию группы Windows с минимальным значением principal_id и явно заданной схемой по умолчанию. (Невозможно явно выбрать одну из доступных схем по умолчанию как предпочтительную.) Если для пользователя нельзя определить схему по умолчанию, будет использоваться схема **dbo**.  
  
 Значение DEFAULT_SCHEMA может быть установлено до создания схемы, на которую оно указывает.  
  
 Значение DEFAULT_SCHEMA не может указываться при создании пользователя, сопоставленного с сертификатом или асимметричным ключом.  
  
 Значение параметра DEFAULT_SCHEMA не учитывается, если пользователь является членом предопределенной роли сервера sysadmin. Для всех членов предопределенной роли сервера sysadmin по умолчанию установлена схема `dbo`.  
  
 Предложение WITHOUT LOGIN создает пользователя, который не сопоставляется с именем входа SQL Server. Такой пользователь может подключиться к базе данных как guest. Этому пользователю без имени входа можно назначать разрешения, и когда контекст безопасности меняется на пользователя без имени входа, то исходные пользователи получают его разрешения. См. пример [Г. Создание и использование пользователя без имени входа](#withoutLogin).  
  
 Символ обратной косой черты ( **\\** ) может содержаться только в именах пользователей, сопоставленных с субъектами Windows.
  
 С помощью инструкции CREATE USER нельзя создать пользователя guest, потому что пользователь guest уже существует в каждой базе данных. Активировать пользователя guest можно, предоставив ему разрешение CONNECT, как показано далее:  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
 Данные о пользователях базы данных отображаются в представлении каталога [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).

Для создания имен входа Azure AD на уровне сервера в Управляемом экземпляре SQL доступно новое расширение синтаксиса **FROM EXTERNAL PROVIDER**. Имена входа Azure AD позволяют сопоставлять субъекты Azure AD на уровне базы данных с именами входа Azure AD на уровне сервера. Чтобы создать пользователя Azure AD по имени входа Azure AD, используйте следующий синтаксис:

`CREATE USER [AAD_principal] FROM LOGIN [Azure AD login]`

При создании пользователя в Управляемом экземпляре SQL значение login_name должно соответствовать имеющемуся имени входа Azure AD. В противном случае в результате использования предложения **FROM EXTERNAL PROVIDER** будет создан только пользователь Azure AD без имени входа в базе данных master. Например, следующая команда создает автономного пользователя:

`CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER`
  
##  <a name="syntax-summary"></a><a name="SyntaxSummary"></a> Сводка синтаксиса  
 **Пользователи, соответствующие именам входа в базе данных master**  
  
 В следующем списке показан возможный синтаксис для пользователей, связанных с именами входа. Параметры схемы по умолчанию не указываются.  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FOR LOGIN SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FROM LOGIN SQLAUTHLOGIN`  
  
**Пользователи, проходящие проверку подлинности в базе данных**  
  
 В следующем списке показан возможный синтаксис для пользователей, которые могут использоваться только в автономной базе данных. Созданные пользователи не будут связаны с именами входа в базе данных **master**. Параметры схемы и языковые параметры, задаваемые по умолчанию, не указываются.  
  
> [!IMPORTANT]  
>  Этот синтаксис предоставляет пользователям доступ к базе данных и новый доступ к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER Barry WITH PASSWORD = 'sdjklalie8rew8337!$d'`  
  
**Пользователи на основе субъектов Windows без подключения к базе данных master**  
  
 В следующем списке показан возможный синтаксис для пользователей, имеющих доступ к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] за счет членства в группе Windows, но не имеющих имени входа в базе данных **master**. Такой синтаксис можно использовать во всех типах базы данных. Параметры схемы и языковые параметры, задаваемые по умолчанию, не указываются.  
  
 Этот синтаксис аналогичен пользователям, соответствующим именам входа в базе данных master, однако пользователи данной категории не имеют имени входа в базе данных master. Пользователь должен получать доступ к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] с помощью имени входа группы Windows.  
  
 Этот синтаксис аналогичен пользователям автономной базы данных, соответствующим участникам Windows, однако пользователи данной категории не получают новый доступ к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
  
**Пользователи, которые не могут проходить проверку подлинности**  
  
 В следующем списке показан возможный синтаксис для пользователей, которые не могут выполнять вход в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   `CREATE USER RIGHTSHOLDER WITHOUT LOGIN`  
-   `CREATE USER CERTUSER FOR CERTIFICATE SpecialCert`  
-   `CREATE USER CERTUSER FROM CERTIFICATE SpecialCert`  
-   `CREATE USER KEYUSER FOR ASYMMETRIC KEY SecureKey`  
-   `CREATE USER KEYUSER FROM ASYMMETRIC KEY SecureKey`  
  
## <a name="security"></a>Безопасность  
 При создании пользователя предоставляется доступ к базе данных, однако доступ к объектам в базе данных не предоставляется автоматически. После создания пользователи обычно добавляются в роли базы данных, которые имеют разрешение на доступ к объектам базы данных, либо разрешения на объект предоставляются непосредственно пользователю. Сведения о проектировании системы разрешений см. в статье [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
### <a name="special-considerations-for-contained-databases"></a>Замечания, относящиеся к автономным базам данных  
 Если при подключении к автономной базе данных пользователь не имеет имени входа в базе данных **master**, то строка подключения должна содержать имя автономной базы данных в качестве исходного каталога. Параметр исходного каталога всегда обязателен для пользователя автономной базы данных с паролем.  
  
 Создание пользователей в автономной базе данных позволяет отделить базу данных от экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], что позволяет легко переместить ее в другой экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделах [Автономные базы данных](../../relational-databases/databases/contained-databases.md) и [Пользователи автономной базы данных — создание переносимой базы данных](../../relational-databases/security/contained-database-users-making-your-database-portable.md). Сведения об изменении пользователя базы данных с пользователя, соответствующего имени входа для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на пользователя автономной базы данных с паролем см. в разделе [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).  
  
 В автономной базе данных пользователям не обязательно иметь имена входа в базе данных **master**. Администраторы [!INCLUDE[ssDE](../../includes/ssde-md.md)] должны учитывать, что доступ к автономной базе данных можно предоставлять на уровне базы данных, а не на уровне компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Дополнительные сведения см. в разделе [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
 При использовании пользователей автономной базы данных [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] настройте доступ с помощью правила брандмауэра уровня базы данных вместо правила брандмауэра уровня сервера. Дополнительные сведения см. в разделе [sp_set_database_firewall_rule (база данных SQL Azure)](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md).
 
Для пользователей автономной базы данных [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] и [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] SSMS может поддерживать многофакторную проверку подлинности. Дополнительные сведения: [Поддержка в SSMS многофакторной проверки подлинности Azure AD для базы данных SQL и [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] ](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/).  
  
### <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER ANY USER для базы данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-database-user-based-on-a-sql-server-login"></a>A. Создание пользователя базы данных, соответствующего имени входа SQL Server  
 В следующем примере сначала создается имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`AbolrousHazem`, а затем создается соответствующий пользователь `AbolrousHazem` в базе данных `AdventureWorks2012`.  
  
```sql  
CREATE LOGIN AbolrousHazem   
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
```   
Изменение на пользовательскую базу данных. Например, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используйте инструкцию `USE AdventureWorks2012`. В [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] необходимо создать новое подключение к пользовательской базе данных.

```sql   
CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
GO   
```  
  
### <a name="b-creating-a-database-user-with-a-default-schema"></a>Б. Создание пользователя базы данных со схемой по умолчанию  
 В следующем примере вначале создается имя входа `WanidaBenshoof` с паролем на сервер, а затем в базе данных создается соответствующий пользователь `Wanida` со схемой по умолчанию `Marketing`.  
  
```sql  
CREATE LOGIN WanidaBenshoof   
    WITH PASSWORD = '8fdKJl3$nlNv3049jsKK';  
USE AdventureWorks2012;  
CREATE USER Wanida FOR LOGIN WanidaBenshoof   
    WITH DEFAULT_SCHEMA = Marketing;  
GO  
```  
  
### <a name="c-creating-a-database-user-from-a-certificate"></a>В. Создание пользователя базы данных из сертификата  
 В следующем примере в базе данных создается пользователь `JinghaoLiu` из сертификата `CarnationProduction50`.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
```sql  
USE AdventureWorks2012;  
CREATE CERTIFICATE CarnationProduction50  
    WITH SUBJECT = 'Carnation Production Facility Supervisors',  
    EXPIRY_DATE = '11/11/2011';  
GO  
CREATE USER JinghaoLiu FOR CERTIFICATE CarnationProduction50;  
GO   
```  
  
###  <a name="d-creating-and-using-a-user-without-a-login"></a><a name="withoutLogin"></a> Г. Создание и использование пользователя без имени входа  
 В следующем примере создается пользователь базы данных `CustomApp`, который не сопоставляется с именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Затем пример предоставляет пользователю `adventure-works\tengiz0` разрешение на олицетворение `CustomApp` пользователя.  
  
```sql  
USE AdventureWorks2012 ;  
CREATE USER CustomApp WITHOUT LOGIN ;  
GRANT IMPERSONATE ON USER::CustomApp TO [adventure-works\tengiz0] ;  
GO   
```  
  
 Для использования учетных данных `CustomApp` , пользователь `adventure-works\tengiz0` выполняет следующее выражение.  
  
```sql  
EXECUTE AS USER = 'CustomApp' ;  
GO  
```  
  
 Для возврата к учетным данным `adventure-works\tengiz0` , пользователь выполняет следующее выражение.  
  
```sql  
REVERT ;  
GO  
```  
  
### <a name="e-creating-a-contained-database-user-with-password"></a>Д. Создание пользователя автономной базы данных с паролем  
 В следующем примере создается пользователь автономной базы данных с паролем. Этот пример можно выполнить только в автономной базе данных.  
  
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий. Этот пример работает в [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], если удаляется DEFAULT_LANGUAGE.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER Carlo  
WITH PASSWORD='RN92piTCh%$!~3K9844 Bl*'  
    , DEFAULT_LANGUAGE=[Brazilian]  
    , DEFAULT_SCHEMA=[dbo]  
GO   
```  
  
### <a name="f-creating-a-contained-database-user-for-a-domain-login"></a>Е. Создание пользователя автономной базы данных для имени входа домена  
 В следующем примере создается пользователь автономной базы данных для имени входа Fritz в домене Contoso. Этот пример можно выполнить только в автономной базе данных.  
  
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER [Contoso\Fritz] ;  
GO   
```  
  
### <a name="g-creating-a-contained-database-user-with-a-specific-sid"></a>Ж. Создание пользователя автономной базы данных с конкретным идентификатором SID  
 В следующем примере создается пользователь автономной базы данных с проверкой подлинности SQL Server, имя пользователя — CarmenW. Этот пример можно выполнить только в автономной базе данных.  
  
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER CarmenW WITH PASSWORD = 'a8ea v*(Rd##+'  
, SID = 0x01050000000000090300000063FF0451A9E7664BA705B10E37DDC4B7;
```  
  
### <a name="h-creating-a-user-to-copy-encrypted-data"></a>З. Создание пользователя для копирования зашифрованных данных  
 В следующем примере создается пользователь, который может копировать данные, защищенные компонентом Always Encrypted, из одного набора таблиц, содержащего зашифрованные столбцы, в другой набор таблиц с зашифрованными столбцами (в той же или другой базе данных).  Дополнительные сведения см. в разделе [Перенос конфиденциальных данных с помощью функции Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql  
CREATE USER [Chin]   
WITH   
      DEFAULT_SCHEMA = dbo  
    , ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = ON ;  
```

### <a name="i-create-an-azure-ad-user-from-an-azure-ad-login-in-sql-managed-instance"></a>И. Создание пользователя Azure AD по имени входа Azure AD в Управляемом экземпляре SQL

 Чтобы создать пользователя Azure AD по имени входа Azure AD, используйте приведенный ниже синтаксис.

 Войдите в управляемый экземпляр, используя имя входа Azure AD с ролью `sysadmin`. Приведенная ниже инструкция создает пользователя Azure AD bob@contoso.com по имени входа bob@contoso.com. Это имя входа было создано в примере [CREATE LOGIN](./create-login-transact-sql.md#examples).

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [bob@contoso.com];
GO
```

> [!IMPORTANT]
> При создании **пользователя** по имени входа Azure AD указываемое значение *user_name* должно совпадать со значением *login_name***имени входа**.

Создание пользователя Azure AD в качестве группы на основе имени входа Azure AD, являющегося группой, не поддерживается.

```sql
CREATE USER [AAD group] FROM LOGIN [AAD group];
GO
```

Вы можете создать пользователя Azure AD по имени входа Azure AD, являющемуся группой.

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [AAD group];
GO
```

### <a name="j-create-an-azure-ad-user-without-an-aad-login-for-the-database"></a>К. Создание пользователя Azure AD без имени входа AAD для базы данных

Чтобы создать пользователя Azure AD bob@contoso.com (автономного) в базе данных Управляемого экземпляра SQL, используйте следующий синтаксис:

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
GO
```

## <a name="next-steps"></a>Дальнейшие действия  
После создания пользователя вы можете добавить пользователя к роли базы данных с помощью инструкции [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md).  
Используйте [GRANT](../../t-sql/statements/grant-object-permissions-transact-sql.md) для предоставления роли разрешений на объект, чтобы она имела доступ к таблицам. Общие сведения о модели безопасности SQL Server см. в разделе [Разрешения](../../relational-databases/security/permissions-database-engine.md).   
  
## <a name="see-also"></a>См. также:  
 [Создание пользователя базы данных](../../relational-databases/security/authentication-access/create-a-database-user.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [ALTER USER (Transact-SQL)](../../t-sql/statements/alter-user-transact-sql.md)   
 [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)   
 [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Автономные базы данных](../../relational-databases/databases/contained-databases.md)   
 [Подключение к базе данных SQL с использованием аутентификации Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)   
 [Приступая к работе с разрешениями Database Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)