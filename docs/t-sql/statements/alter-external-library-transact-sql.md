---
description: ALTER EXTERNAL LIBRARY (Transact-SQL)
title: ALTER EXTERNAL LIBRARY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- ALTER EXTERNAL LIBRARY
- ALTER_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 65792225a4fe990720d4df1199dfe0e3683e0b63
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082650"
---
# <a name="alter-external-library-transact-sql"></a>ALTER EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Изменяет содержимое существующей внешней библиотеки пакетов.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> В SQL Server 2017 поддерживаются язык R и платформа Windows. R, Python и внешние языки на платформах Windows и Linux поддерживаются в SQL Server 2019 и более поздних версиях.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> В Управляемом экземпляре SQL Azure библиотеку можно изменить, удалив ее, а затем установив измененную версию с помощью пакета **sqlmlutils**. Дополнительные сведения о **sqlmlutils** см. в статьях [Установка пакетов Python с помощью sqlmlutils](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true) и [Установка новых пакетов R с помощью sqlmlutils](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true).
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>Синтаксис для SQL Server 2019

```syntaxsql
ALTER EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ]
SET <file_spec>
WITH ( LANGUAGE = <language> )
[ ; ]

<file_spec> ::=
{
    (CONTENT = { <client_library_specifier> | <library_bits> | NONE}
    [, PLATFORM = <platform> )
}

<client_library_specifier> :: =
{
      '[\\computer_name\]share_name\[path\]manifest_file_name'
    | '[local_path\]manifest_file_name'
    | '<relative_path_in_external_data_source>'
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
::: moniker range="=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>Синтаксис для SQL Server 2017

```syntaxsql
ALTER EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ]
SET <file_spec>
WITH ( LANGUAGE = 'R' )
[ ; ]

<file_spec> ::=
{
    (CONTENT = { <client_library_specifier> | <library_bits> | NONE}
    [, PLATFORM = WINDOWS )
}

<client_library_specifier> :: =
{
      '[\\computer_name\]share_name\[path\]manifest_file_name'
    | '[local_path\]manifest_file_name'
    | '<relative_path_in_external_data_source>'
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

Указывает имя существующей библиотеки пакетов. Область действия библиотек ограничивается пользователем. Имена библиотек должны быть уникальными в контексте определенного пользователя или владельца.

Имя библиотеки не может назначаться произвольным образом. То есть необходимо использовать имя, которое ожидает вызывающая среда выполнения при загрузке пакета.

**owner_name**

Указывает имя пользователя или роли, которой принадлежит внешняя библиотека.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

Указывает содержимое пакета для конкретной платформы. Поддерживается только один файл артефакта на платформу.

Файл можно указать в виде локального или сетевого пути. Если указан параметр источника данных, имя файла может быть относительным путем по отношению к контейнеру, на который ссылается `EXTERNAL DATA SOURCE`.

При необходимости можно указать платформу операционной системы для файла. Для каждой платформы операционной системы для конкретного языка или среды выполнения разрешен только один артефакт файла или содержимое.
::: moniker-end

**library_bits**

Задает содержимое пакета как шестнадцатеричный литерал, аналогично сборкам.

Этот параметр можно использовать, если у вас есть необходимое разрешение на изменение библиотеки, но доступ к файлам на сервере ограничен и не удается сохранить содержимое в пути, к которому у сервера есть доступ.

Вместо этого можно передать содержимое пакета в качестве переменной в двоичном формате.

::: moniker range=">=sql-server-2017 <=sql-server-2017"
**platform = WINDOWS**

Указывает платформу для содержимого библиотеки. Это значение является обязательным при изменении существующей библиотеки для добавления другой платформы.
В SQL Server 2017 поддерживается только платформа Windows.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**platform**

Указывает платформу для содержимого библиотеки. Это значение является обязательным при изменении существующей библиотеки для добавления другой платформы. 
В SQL Server 2019 поддерживаются платформы Windows и Linux.
::: moniker-end

::: moniker range=">=sql-server-2017 <=sql-server-2017"
**LANGUAGE = 'R'**

Задает язык пакета. Язык R поддерживается в SQL Server 2017.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

Задает язык пакета. Значение может быть **R** или **Python** в Управляемом экземпляре SQL Azure.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

Задает язык пакета. Значением может быть **R**, **Python** или название внешнего языка (см. раздел [CREATE EXTERNAL LANGUAGE](create-external-language-transact-sql.md)).
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks

::: moniker range=">=sql-server-2017 <=sql-server-2017"
В языке R пакеты должны быть подготовлены в виде сжатых архивных файлов с расширением ZIP для Windows. В SQL Server 2017 поддерживается только платформа Windows.  
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
При использовании файла в языке R пакеты должны быть подготовлены в виде сжатых архивных файлов с расширением ZIP. 

Для языка Python необходимо подготовить пакет в WHL- или ZIP-файле в виде файла с ZIP-архивом. Если пакет уже является ZIP-файлом, он должен быть включен в новый ZIP-файл. Отправка пакета в качестве WHL- или ZIP-файла напрямую в настоящее время не поддерживается.
::: moniker-end

Инструкция `ALTER EXTERNAL LIBRARY` только загружает биты библиотеки в базу данных. Измененная библиотека устанавливается при выполнении кода в [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), который вызывает библиотеку.

Набор пакетов, называемых *системными пакетами*, устанавливается в экземпляре SQL предварительно. Пользователь не может добавлять, обновлять и удалять системные пакеты.

## <a name="permissions"></a>Разрешения

По умолчанию разрешение на запуск ALTER EXTERNAL LIBRARY имеет пользователь **dbo** или любой член роли **db_owner**. Кроме того, внешнюю библиотеку может изменять пользователь, который ее создал.

## <a name="examples"></a>Примеры

В следующем примере изменяется внешняя библиотека `customPackage`.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="replace-the-contents-of-a-library-using-a-file"></a>Замена содержимого библиотеки с помощью файла

В следующем примере изменяется внешняя библиотека `customPackage` с помощью сжатого ZIP-файла, содержащего обновленные биты.

```sql
ALTER EXTERNAL LIBRARY customPackage 
SET 
  (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip')
WITH (LANGUAGE = 'R');
```

Чтобы установить обновленную библиотеку, выполните хранимую процедуру `sp_execute_external_script`.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
;
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В языке Python пример также выполняется при замене `'R'` на `'Python'`.
::: moniker-end

### <a name="alter-an-existing-library-using-a-byte-stream"></a>Изменение существующей библиотеки с помощью байтового потока

В следующем примере существующая библиотека изменяется путем передачи новых битов в виде шестнадцатеричного литерала.

```SQL
ALTER EXTERNAL LIBRARY customLibrary 
SET (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
В языке Python пример также выполняется при замене `'R'` на `'Python'`.
::: moniker-end

> [!NOTE]
> В этом примере кода показан только синтаксис. Двоичное значение в `CONTENT =` было усечено для удобства чтения и не создает рабочую библиотеку. Фактическое содержимое двоичной переменной будет гораздо длиннее.

## <a name="see-also"></a>См. также раздел

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)