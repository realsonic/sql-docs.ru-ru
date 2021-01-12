---
description: CREATE EXTERNAL LIBRARY (Transact-SQL) — SQL Server
title: CREATE EXTERNAL LIBRARY (Transact-SQL) — SQL Server | Документация Майкрософт
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL LIBRARY
- CREATE_EXTERNAL_LIBRARY_TSQL
- EXTERNAL LIBRARY
- EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9839a6fa8bd678afdce426f9dcd5657500c5a6cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100797"
---
# <a name="create-external-library-transact-sql"></a>CREATE EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Отправляет файлы пакетов R, Python или Java в базу данных из указанного байтового потока или пути к файлу. Эта инструкция служит универсальным механизмом для администратора базы данных, с помощью которого он может отправлять артефакты, необходимые для любой новой внешней языковой среды выполнения и платформы операционной системы, поддерживаемой [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. 

> [!NOTE]
> В SQL Server 2017 поддерживаются язык R и платформа Windows. R, Python и внешние языки на платформах Windows и Linux поддерживаются в SQL Server 2019 и более поздних версиях.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Отправляет файлы пакетов R или Python в базу данных из указанного байтового потока или пути к файлу. Эта инструкция служит универсальным механизмом, с помощью которого администратор базы данных может отправлять необходимые артефакты. 

> [!NOTE]
> В управляемом экземпляре SQL Azure для установки библиотеки можно использовать **sqlmlutils**. Дополнительные сведения о см. в статьях [Установка пакетов Python с помощью sqlmlutils](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true) и [Установка новых пакетов R с помощью sqlmlutils](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true).
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>Синтаксис для SQL Server 2019

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> }  
    [, PLATFORM = <platform> ])  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'  
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<platform> :: = 
{
      WINDOWS
    | LINUX
}

<language> :: = 
{
      'R'
    | 'Python'
    | <external_language>
}

```
::: moniker-end
::: moniker range=">=sql-server-2017 <=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>Синтаксис для SQL Server 2017

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = 'R' )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> }  
    [, PLATFORM = WINDOWS ])  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}
```
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
## <a name="syntax-for-azure-sql-managed-instance"></a>Синтаксис для Управляемого экземпляра SQL Azure

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = <library_bits>)  
}  

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<language> :: = 
{
      'R'
    | 'Python'
}
```
::: moniker-end

### <a name="arguments"></a>Аргументы

**library_name**

Библиотеки, загруженные в экземпляр, могут быть открытыми или закрытыми. Если библиотека создается членом `dbo`, она является открытой и может использоваться всеми пользователями. В противном случае библиотека является закрытой и доступна только этому пользователю.

Имена библиотек должны быть уникальными в контексте определенного пользователя или владельца. Например, два пользователя **RUser1** и **RUser2** могут загрузить библиотеку R `ggplot2` индивидуально и по отдельности. Но если пользователь **RUser1** хочет отправить новую версию `ggplot2`, второму экземпляру нужно присвоить другое имя либо он должен заменить существующую библиотеку.

Имена библиотек не могут присваиваться произвольным образом. Имя библиотеки должно быть таким же, как имя, необходимое для загрузки библиотеки из внешнего скрипта.

**owner_name**

Указывает имя пользователя или роли, которой принадлежит внешняя библиотека. Если атрибут не указан, владельцем становится текущий пользователь.

Библиотеки, принадлежащие владельцу базы данных, считаются глобальными по отношению к базе данных и среде выполнения. Иными словами, владельцы базы данных могут создавать библиотеки, которые содержат общий набор библиотек или пакетов, совместно используемых несколькими пользователями. Если внешнюю библиотеку создает другой пользователь, кроме пользователя `dbo`, к этой внешней библиотеке будет иметь доступ только этот пользователь.

Когда пользователь **RUser1** выполняет внешний сценарий, значение `libPath` может содержать несколько путей. Первый путь всегда является путем к общей библиотеке, созданной владельцем базы данных. Во второй части `libPath` указывается путь, содержащий пакеты, загруженные отдельно пользователем **RUser1**.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

Указывает содержимое пакета для конкретной платформы. Поддерживается только один файл артефакта на платформу.

Файл можно указать в виде локального или сетевого пути.

При попытке доступа к файлу, указанному в **<client_library_specifier>** , SQL Server олицетворяет разрешения контекста безопасности текущего имени входа Windows. Если **<client_library_specifier>** задает расположение в сети (UNC-путь), олицетворение текущего имени входа не распространяется на это расположение в сети из-за ограничений передачи прав. В этом случае доступ осуществляется при помощи контекста безопасности учетной записи службы SQL Server. Дополнительные сведения см. в статье [Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

При необходимости можно указать платформу операционной системы для файла. Для каждой платформы операционной системы для конкретного языка или среды выполнения разрешен только один артефакт файла или содержимое.
::: moniker-end

**library_bits**

Задает содержимое пакета как шестнадцатеричный литерал, аналогично сборкам. 

Этот параметр полезен, если необходимо создать библиотеку или изменить существующую библиотеку (и у вас есть необходимые разрешения), но файловая система на сервере ограничена, и не удается скопировать файлы библиотеки в место, доступное для сервера.

::: moniker range="=sql-server-2017"
**PLATFORM = WINDOWS**

Указывает платформу для содержимого библиотеки. Является значением по умолчанию для платформы узла, на которой выполняется SQL Server. Поэтому пользователю не нужно указывать это значение. Оно необходимо в случае, когда поддерживается несколько платформ или пользователь хочет указать другую платформу.
В SQL Server 2017 поддерживается только платформа Windows.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**PLATFORM**

Указывает платформу для содержимого библиотеки. Является значением по умолчанию для платформы узла, на которой выполняется SQL Server. Поэтому пользователю не нужно указывать это значение. Оно необходимо в случае, когда поддерживается несколько платформ или пользователь хочет указать другую платформу.
В SQL Server 2019 поддерживаются платформы Windows и Linux.
::: moniker-end

::: moniker range="=sql-server-2017"
**LANGUAGE = 'R'**

Задает язык пакета. Язык R поддерживается в SQL Server 2017.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

Задает язык пакета. Значение может быть `R` или `Python` в Управляемом экземпляре SQL Azure.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

Задает язык пакета. Значением может быть `R`, `Python` или название внешнего языка (см. раздел [CREATE EXTERNAL LANGUAGE](create-external-language-transact-sql.md)).
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks

::: moniker range=">=sql-server-2017 <=sql-server-2017"
При использовании файла в языке R пакеты должны быть подготовлены в виде сжатых архивных файлов с расширением ZIP для Windows. 
В SQL Server 2017 поддерживается только платформа Windows.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
При использовании файла в языке R пакеты должны быть подготовлены в виде сжатых архивных файлов с расширением ZIP.  

Для языка Python необходимо подготовить пакет в WHL- или ZIP-файле в виде файла с ZIP-архивом. Если пакет уже является ZIP-файлом, он должен быть включен в новый ZIP-файл. Отправка пакета в качестве WHL- или ZIP-файла напрямую в настоящее время не поддерживается.
::: moniker-end

Инструкция `CREATE EXTERNAL LIBRARY` загружает биты библиотеки в базу данных. Библиотека устанавливается, когда пользователь запускает внешний скрипт с помощью [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) и вызывает пакет или библиотеку.

Библиотеки, загруженные в экземпляр, могут быть открытыми или закрытыми. Если библиотека создается членом `dbo`, она является открытой и может использоваться всеми пользователями. В противном случае библиотека является закрытой и доступна только этому пользователю.

Набор пакетов, называемых *системными пакетами*, устанавливается в экземпляре SQL предварительно. Пользователь не может добавлять, обновлять и удалять системные пакеты.

## <a name="permissions"></a>Разрешения

Требуется разрешение `CREATE EXTERNAL LIBRARY`. По умолчанию любой пользователь с учетной записью **dbo**, являющийся членом роли **db_owner**, имеет разрешения на создание внешней библиотеки. Другим пользователям необходимо явным образом предоставить разрешение с помощью инструкции [GRANT](./grant-database-permissions-transact-sql.md), указав CREATE EXTERNAL LIBRARY в качестве привилегии.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В SQL Server 2019 в дополнение к разрешению "CREATE EXTERNAL LIBRARY" пользователю также нужно разрешение references для внешнего языка, чтобы создать для этого языка внешние библиотеки.

```sql
GRANT REFERENCES ON EXTERNAL LANGUAGE::Java to user
GRANT CREATE EXTERNAL LIBRARY to user
```

::: moniker-end

Для изменения любой библиотеки требуется отдельное разрешение `ALTER ANY EXTERNAL LIBRARY`.

Чтобы создать внешнюю библиотеку, используя путь к файлу, пользователь должен иметь проверенное на подлинность имя входа Windows или быть членом предопределенной роли сервера sysadmin.

## <a name="examples"></a>Примеры

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="add-an-external-library-to-a-database"></a>Добавление внешней библиотеки в базу данных  

В следующем примере внешняя библиотека `customPackage` добавляется в базу данных.

```sql
CREATE EXTERNAL LIBRARY customPackage
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip') WITH (LANGUAGE = 'R');
```

После успешной загрузки библиотеки в экземпляр пользователь выполняет процедуру `sp_execute_external_script`, чтобы установить библиотеку.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В языке Python в SQL Server 2019 пример также выполняется при замене `'R'` на `'Python'`.
::: moniker-end

### <a name="installing-packages-with-dependencies"></a>Установка пакетов с зависимостями

Если вы хотите установить пакет с зависимостями, нужно обязательно проанализировать зависимости первого и второго уровней и убедиться, что все необходимые пакеты доступны, _до_ установки целевого пакета.

Предположим, вы хотите установить новый пакет `packageA`:

+ `packageA` имеет зависимость от `packageB`
+ `packageB` имеет зависимость от `packageC`

Для успешной установки `packageA` необходимо создать библиотеки для `packageB` и `packageC` одновременно с добавлением `packageA` в SQL Server. Не забудьте проверить версии необходимых пакетов.

На практике зависимости популярных пакетов обычно гораздо сложнее, чем в этом простом примере. Например, **ggplot2** может требовать более 30 пакетов, а эти пакеты могут требовать дополнительные пакеты, которые недоступны на сервере. Отсутствие пакета или наличие пакета неправильной версии могут привести к сбою установки.

Так как может быть трудно определить все зависимости при просмотре манифеста пакета, рекомендуем использовать такой пакет, как [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html), для поиска всех пакетов, необходимых для успешного завершения установки.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"

+ Загрузите целевой пакет и его зависимости. Все файлы должны находиться в папке, доступной на сервере.

    ```sql
    CREATE EXTERNAL LIBRARY packageA 
    FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageA.zip') 
    WITH (LANGUAGE = 'R'); 
    GO

    CREATE EXTERNAL LIBRARY packageB FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageB.zip') 
    WITH (LANGUAGE = 'R');
    GO

    CREATE EXTERNAL LIBRARY packageC FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageC.zip') 
    WITH (LANGUAGE = 'R');
    GO
    ```

+ Сначала установите необходимые пакеты.

    Если необходимый пакет уже отправлен экземпляру, не нужно добавлять его снова. Просто не забудьте проверить версию существующего пакета. 
    
    Необходимые пакеты `packageC` и `packageB` установлены в правильном порядке, когда `sp_execute_external_script` впервые запускается для установки пакета `packageA`.

    Но если доступны не все необходимые пакеты, установка целевого пакета `packageA` завершается ошибкой.

    ```sql
    EXEC sp_execute_external_script 
    @language =N'R', 
    @script=N'
    # load the desired package packageA
    library(packageA)
    '
    ```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В языке Python в SQL Server 2019 пример также выполняется при замене `'R'` на `'Python'`.
::: moniker-end

### <a name="create-a-library-from-a-byte-stream"></a>Создание библиотеки из потока байтов

Если у вас нет возможности сохранить файлы пакета на сервере, вы можете передать содержимое пакетов в переменной. В следующем примере библиотека создается путем передачи битов в виде шестнадцатеричного литерала.

```SQL
CREATE EXTERNAL LIBRARY customLibrary FROM (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В языке Python в SQL Server 2019 пример также выполняется при замене **'R'** на **'Python'** .
::: moniker-end

> [!NOTE]
> В этом примере кода показан только синтаксис. Двоичное значение в `CONTENT =` было усечено для удобства чтения и не создает рабочую библиотеку. Фактическое содержимое двоичной переменной будет гораздо длиннее.

### <a name="change-an-existing-package-library"></a>Изменение существующей библиотеки пакета

Можно использовать инструкцию DDL `ALTER EXTERNAL LIBRARY` для добавления нового содержимого библиотеки или изменения существующего содержимого библиотеки. Для изменения существующей библиотеки требуется разрешение `ALTER ANY EXTERNAL LIBRARY`.

Дополнительные сведения см. в разделе [ALTER EXTERNAL LIBRARY](alter-external-library-transact-sql.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="add-a-java-jar-file-to-a-database"></a>Добавьте JAR-файл Java к базе данных  

В следующем примере внешний JAR-файл `customJar` добавляется в базу данных.

```sql
CREATE EXTERNAL LIBRARY customJar
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\customJar.jar') 
WITH (LANGUAGE = 'Java');
```

После успешной загрузки библиотеки в экземпляр пользователь выполняет процедуру `sp_execute_external_script`, чтобы установить библиотеку.

```sql
EXEC sp_execute_external_script
    @language = N'Java'
    , @script = N'customJar.MyCLass.myMethod'
    , @input_data_1 = N'SELECT * FROM dbo.MyTable'
WITH RESULT SETS ((column1 int))
```

### <a name="add-an-external-package-for-both-windows-and-linux"></a>Добавление внешнего пакета одновременно для Windows и Linux

Вы можете указать два параметра `<file_spec>`: один для Windows, а другой для Linux.

```sql
CREATE EXTERNAL LIBRARY lazyeval 
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.zip', PLATFORM = WINDOWS),
(CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.tar.gz', PLATFORM = LINUX)
WITH (LANGUAGE = 'R')
```

При установке пакета с помощью процедуры `sp_execute_external_script` используется содержимое библиотеки для той платформы, на которой выполняется экземпляр SQL Server.

```sql
EXECUTE sp_execute_external_script 
    @LANGUAGE = N'R',
    @SCRIPT = N'
library(packageA)
```
::: moniker-end

## <a name="see-also"></a>См. также раздел

[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)