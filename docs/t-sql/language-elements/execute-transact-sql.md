---
description: EXECUTE (Transact-SQL)
title: EXECUTE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EXEC
- EXECUTE_TSQL
- EXECUTE
- EXEC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote stored procedures [SQL Server]
- command strings [SQL Server]
- extended stored procedures [SQL Server], executing
- stored procedures [SQL Server], executing
- Transact-SQL statements, executing
- strings [SQL Server], executing
- statements [SQL Server], executing
- context switching [SQL Server], execution context
- user-defined functions [SQL Server], executing
- character strings [SQL Server], executing
- switching execution context
- EXECUTE statement
ms.assetid: bc806b71-cc55-470a-913e-c5f761d5c4b7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3ed27623969b2390f1d5b5b36b51a4c2eb5129e7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100431"
---
# <a name="execute-transact-sql"></a>EXECUTE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Выполняет строку команды или символьную строку в пакете [!INCLUDE[tsql](../../includes/tsql-md.md)] либо один из следующих модулей: системная хранимая процедура, определяемая пользователем хранимая процедура, хранимая процедура CLR, скалярная, определяемая пользователем функция или расширенная хранимая процедура. Инструкция EXECUTE может использоваться для отправки транзитных команд на связанные серверы. или явно указывать контекст, в котором выполняется команда. Метаданные для результирующего набора могут быть определены с помощью параметров WITH RESULT SETS.
  
> [!IMPORTANT]  
>  Прежде чем передавать инструкции EXECUTE строку символов, выполните ее проверку. Ни в коем случае не запускайте на выполнение команду, которая сформирована на основе данных, введенных пользователем, и не проверена.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  

::: moniker range=">=sql-server-ver15"
В следующем блоке кода показан синтаксис в SQL Server 2019. Также можно посмотреть раздел [Синтаксис в SQL Server 2017 и более ранних](execute-transact-sql.md?view=sql-server-2017&preserve-view=true). 

```syntaxsql
-- Syntax for SQL Server 2019
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
    [ AT DATA_SOURCE data_source_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

::: moniker range=">=sql-server-2016"

В следующем блоке кода показан синтаксис в SQL Server 2017 и более ранних версиях. Также можно посмотреть раздел [Синтаксис в SQL Server 2019](execute-transact-sql.md?view=sql-server-ver15&preserve-view=true).


```syntaxsql
-- Syntax for SQL Server 2017 and earleir  
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

  
```syntaxsql
-- In-Memory OLTP   

Execute a natively compiled, scalar user-defined function  
[ { EXEC | EXECUTE } ]   
    {   
      [ @return_status = ]   
      { module_name | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable   
                           | [ DEFAULT ]   
                           }  
        ]   
      [ ,...n ]   
      [ WITH <execute_option> [ ,...n ] ]   
    }  
<execute_option>::=  
{  
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database   
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name  | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH RECOMPILE ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS {  USER } = ' name ' ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML  
  
```

  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Execute a stored procedure  
[ { EXEC | EXECUTE } ]  
    procedure_name   
        [ { value | @variable [ OUT | OUTPUT ] } ] [ ,...n ] }  
[;]  
  
-- Execute a SQL string  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'tsql_string' } [ +...n ] )  
[;]  
```  


  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 @*return_status*  
 Необязательная целочисленная переменная, в которой сохраняется состояние возврата из модуля. Этот аргумент должен быть объявлен в пакете, хранимой процедуре или функции, прежде чем его можно будет указать в инструкции EXECUTE.  
  
 Если используется для вызова скалярной, определяемой пользователем функции, переменная @*return_status* может иметь любой скалярный тип данных.  
  
 *module_name*  
 Полное или неполное имя вызываемой хранимой процедуры или скалярной пользовательской функции. Имена модулей должны соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). В именах расширенных хранимых процедур учитывается регистр, вне зависимости от параметров сортировки сервера.  
  
 Допускается выполнение модуля, созданного в другой базе данных, если пользователь, выполняющий этот модуль, является его владельцем или имеет соответствующие разрешения на его выполнение в этой базе данных. Модуль может быть выполнен на другом сервере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если пользователь, запускающий модуль на выполнение, имеет соответствующие разрешения на этом сервере (удаленный доступ) и на выполнение модуля в этой базе данных. Если указано имя сервера, а имя базы данных не указано, компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ищет модуль в базе данных пользователя по умолчанию.  
  
 *number*  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
 Необязательный целочисленный аргумент, используемый для группирования одноименных процедур. Этот аргумент не предназначен для расширенных хранимых процедур.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Дополнительные сведения о группах процедур см. в статье [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 @*module_name_var*  
 Имя локально определенной переменной, которая содержит имя модуля.  
  
 Это может быть переменная, содержащая имя скомпилированной в собственном коде скалярной определяемой пользователем функции.  
  
 @*parameter*  
 Параметр для *module_name*, как определено в модуле. Имена аргументов должны предваряться символом (@). Если используется в формате @*parameter_name*=*value*, то имена параметров и констант могут указываться не в том порядке, в котором они определены в модуле. Однако если какой-либо из параметров указан в формате @*parameter_name*=*value*, то все последующие параметры должны быть указаны в том же формате.  
  
 По умолчанию параметры могут допускать значения NULL.  
  
 *value*  
 Значение параметра, передаваемое модулю или транзитной команде. Если имена параметров не указаны, значения параметров должны указываться в том же порядке, в каком они определены в модуле.  
  
 При выполнении транзитных команд для связанных серверов порядок значений параметров зависит от поставщика OLE DB связанного сервера. Большинство поставщиков OLE DB привязывают значения к аргументам слева направо.  
  
 Если значение параметра является именем объекта, символьной строкой или предваряется именем базы данных или схемы, это значение целиком должно быть заключено в одинарные кавычки. Если значение параметра является ключевым словом, оно должно быть заключено в двойные кавычки.  
  
Если вы передаете одно слово, которое не начинается с `@` и не заключено в кавычки (например, если вы забудете `@` в имени параметра), слово рассматривается как строка типа nvarchar, несмотря на отсутствие кавычек.

 Если в модуле определено значение по умолчанию, пользователь может вызвать модуль без указания этого параметра.  
  
 Значение по умолчанию может быть равно NULL. Как правило, действие, которое должно быть выполнено в этом случае, указывается в определении модуля.  
  
 @*variable*  
 Переменная, в которой сохраняется или возвращается аргумент.  
  
 OUTPUT  
 Указывает, что модуль или командная строка возвращает параметр. Совпадающий параметр модуля или командной строки также должен быть создан с ключевым словом OUTPUT. Это ключевое слово следует указывать для переменной курсора, если она передается в качестве аргумента.  
  
 Если *value* определен как OUTPUT модуля, выполняемого для связанного сервера, то любые изменения в соответствующем параметре @*parameter*, произведенные поставщиком OLE DB, по окончании выполнения модуля будут скопированы обратно в переменную.  
  
 Если используются параметры OUTPUT и предполагается использовать возвращаемые значения в других инструкциях вызываемого пакета или модуля, значение параметра должно передаваться в виде переменной, то есть @*parameter* = @*variable*. Выполнить модуль, указав OUTPUT для параметра, который не определен в модуле как параметр OUTPUT, нельзя. Константы в качестве аргументов OUTPUT в модуль не передаются, а для возврата аргумента необходимо указывать имя переменной. Перед выполнением процедуры для переменной должен быть объявлен тип данных и присвоено значение.  
  
 Если EXECUTE выполняет удаленную хранимую процедуру или транзитную команду к связанному серверу, то параметры OUTPUT не могут иметь типы данных больших объектов (LOB).  
  
 Возвращаемые аргументы могут иметь любой тип, кроме типов данных LOB.  
  
 DEFAULT  
 Определяет значение параметра по умолчанию, как определено в модуле. Если в модуле для параметра не определено значения по умолчанию, а при вызове для этого параметра ни значение, ни ключевое слово DEFAULT не указаны, выдается ошибка.  
  
 @*string_variable*  
 Имя локальной переменной. @*string_variable* может иметь любой тип данных: **char**, **varchar**, **nchar** или **nvarchar**. В том числе типы данных **(max)** .  
  
 [N] '*tsql_string*'  
 Строковая константа. *tsql_string* может иметь любой тип данных **nvarchar** или **varchar**. Если указано "N", строка интерпретируется как тип данных **nvarchar**.  
  
 AS \<context_specification>  
 Определяет контекст, в котором выполняется инструкция.  
  
 Имя_для_входа  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
 Указывает, что воплощаемым контекстом является имя входа, область олицетворения — сервер.  
  
 Пользователь  
 Определяет контекст для олицетворения пользователя в текущей базе данных. Область олицетворения ограничена текущей базой данных. При переключении контекста на пользователя базы данных разрешения уровня сервера этого пользователя не наследуются.  
  
> [!IMPORTANT]  
>  Пока активно переключение контекста на пользователя базы данных, любая попытка доступа к ресурсам за ее пределами вызовет ошибку выполнения инструкции. Это относится к инструкциям USE *database*, распределенным запросам, а также запросам, содержащим ссылки на другие базы данных по идентификаторам, состоящим из трех или четырех элементов.  
  
 '*name*'  
 Допустимое имя пользователя или имя входа. Аргумент *name* должен принадлежать предопределенной роли сервера sysadmin либо быть участником в базе данных [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) или [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) соответственно.  
  
 В качестве аргумента *name* не может быть указана встроенная учетная запись (например NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService или NT AUTHORITY\LocalSystem).  
  
 Дополнительные сведения см. в разделе [Указание имени пользователя или имени входа](#_user) далее.  
  
 [N] '*command_string*'  
 Строковая константа, содержащая транзитную команду, передаваемую связанному серверу. Если указано "N", строка интерпретируется как тип данных **nvarchar**.  
  
 [?]  
 Обозначает параметры, для которых задаются значения в списке \<arg-list> передаваемых команд, используемых в инструкции EXEC('…', \<arg-list>) AT \<linkedsrv>.  
  
 AT *linked_server_name*  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
 Указывает, что *command_string* выполняется для *linked_server_name*, а результаты, при их наличии, возвращаются клиенту. Значение *linked_server_name* должно указывать на существующее определение связанного сервера на локальном сервере. Определение связанного сервера производится при помощи хранимой процедуры [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
 WITH \<execute_option>  
 Возможные параметры выполнения. Параметры RESULT SETS нельзя указывать в инструкции INSERT...EXEC.  
 
AT DATA_SOURCE data_source_name **Применимо к**: [!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] и более поздних версий.
  
 Указывает, что *command_string* выполняется для *data_source_name*, а результаты, при их наличии, возвращаются клиенту. *data_source_name* должно ссылаться на существующее определение EXTERNAL DATA SOURCE (внешнего источника данных) в базе данных. Поддерживаются только источники данных, указывающие на SQL Server. Кроме того, для SQL Server поддерживаются источники данных кластера больших данных, которые указывают на пул вычислений, пул данных или пул носителей. Источники данных определяются с помощью [CREATE EXTERNAL DATA SOURCE](../statements/create-external-data-source-transact-sql.md) (создание внешнего источника данных).  
  
 WITH \<execute_option>  
 Возможные параметры выполнения. Параметры RESULT SETS нельзя указывать в инструкции INSERT...EXEC.  
  
|Термин|Определение|  
|----------|----------------|  
|RECOMPILE|Инициирует перекомпиляцию нового плана, его использование и удаление после выполнения модуля. Если для модуля имеется существующий план запроса, то он остается в кэше.<br /><br /> Следует указывать этот параметр в тех случаях, когда передаются нетипичные аргументы или если данные существенно изменились. Он не предназначен для расширенных хранимых процедур. Рекомендуется реже пользоваться этим параметром, поскольку он очень ресурсоемок.<br /><br /> **Примечание.** Использовать параметр WITH RECOMPILE при вызове хранимой процедуры, для которой применяется синтаксис OPENDATASOURCE, невозможно. Параметр WITH RECOMPILE не учитывается при указании четырехкомпонентного имени объекта.<br /><br /> **Примечание.** Скомпилированные в собственном коде скалярные определяемые пользователем функции не поддерживают RECOMPILE. Если потребуется выполнить повторную компиляцию, используйте процедуру [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md).|  
|**RESULT SETS UNDEFINED**|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Этот параметр не дает гарантии, какие результаты, если они есть, будут возвращены. Определение также не предоставляется. Инструкция выполняется без ошибок, независимо от того, возвращаются ли какие-либо результаты. RESULT SETS UNDEFINED — действие по умолчанию, если не указан result_sets_option.<br /><br /> Для интерпретируемых скалярных определяемых пользователем функций и скомпилированных в собственном коде скалярных определяемых пользователем функций этот параметр не работает, так как функции никогда не возвращают результирующий набор.|  
|RESULT SETS NONE|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Гарантирует, что выполняемая инструкция не вернет никаких результатов. Если возвращены какие-либо результаты, то пакет отменяется.<br /><br /> Для интерпретируемых скалярных определяемых пользователем функций и скомпилированных в собственном коде скалярных определяемых пользователем функций этот параметр не работает, так как функции никогда не возвращают результирующий набор.|  
|*\<result_sets_definition>*|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Обеспечивает гарантию, что результат будет возвращен в виде, определенном в result_sets_definition. Для выражений, которые возвращают множество результирующих наборов, обеспечьте множество разделов *result_sets_definition*. Заключите каждый раздел *result_sets_definition* в скобки, разделяя их запятыми. Дополнительные сведения см. ниже в разделе \<result_sets_definition>.<br /><br /> Этот параметр всегда приводит к ошибке для скомпилированных в собственном коде скалярных определяемых пользователем функций, поскольку функции никогда не возвращают результирующий набор.|
  
\<result_sets_definition>
**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Описывает результирующие наборы, возвращенные выполненными инструкциями. Предложения result_sets_definition имеют следующий смысл  
  
|Термин|Определение|  
|----------|----------------|  
|{<br /><br /> column_name<br /><br /> Тип данных<br /><br /> [ COLLATE collation_name]<br /><br /> [NULL &#124; NOT NULL]<br /><br /> }|См. таблицу ниже.|  
|db_name|Имя базы данных, содержащей таблицу, представление или возвращающую табличное значение функцию.|  
|schema_name|Имя схемы, являющейся владельцем таблицы, представления или возвращающей табличное значение функции.|  
|table_name &#124; view_name &#124; table_valued_function_name|Указывает, что будут возвращены столбцы, указанные в таблице, представлении или возвращающей табличное значение функции. Табличные переменные, временные таблицы и синонимы не поддерживаются синтаксисом объектов AS.|  
|AS TYPE [schema_name.]table_type_name|Указывает, что будут возвращены столбцы, указанные в типе таблицы.|  
|AS FOR XML|Указывает, что результаты XML, полученные от инструкции или хранимой процедуры, которая вызывается с помощью инструкции EXECUTE, преобразуются в формат, как если бы они были сформированы инструкцией SELECT ... Инструкция FOR XML ... Все форматирование директив типов удаляется из исходной инструкции, а результаты возвращаются, как если бы директива типа не была указана. AS FOR XML не преобразует в XML отличные от XML табличные результаты, полученные от выполненной инструкции или хранимой процедуры.|  
  
|Термин|Определение|  
|----------|----------------|  
|column_name|Имена всех столбцов. Если число столбцов отличается от результирующего набора, возникнет ошибка и пакет будет отменен. Если имя столбца отличается от результирующего набора, то возвращаемое имя столбца будет установлено в имя из определения.|  
|Тип данных|Типы данных для каждого из столбцов. Если типы данных различаются, то выполняется неявное преобразование к определенному типу данных. Если преобразование выполнить не удалось, то пакет отменяется|  
|COLLATE collation_name|Параметры сортировки для каждого из столбцов. При несоответствии параметров сортировки предпринимается попытка неявного его приведения. Если это сделать не удалось, пакет отменяется.|  
|NULL &#124; NOT NULL|Допустимость значения NULL для каждого из столбцов. Если определено NOT NULL, а возвращенные данные содержат значения NULL, то возникает ошибка и пакет отменяется. Если ничего не указано, то значение по умолчанию соответствует параметрам ANSI_NULL_DFLT_ON и ANSI_NULL_DFLT_OFF.|  
  
 Фактический результирующий набор, возвращаемый во время выполнения, может отличаться от результата, определенного в предложении WITH RESULT SETS по одному из следующих признаков: числом результирующих наборов, числом столбцов, именами столбцов, допустимостью значений NULL и типами данных. Если отличается число результирующих наборов, возникнет ошибка, и пакет будет отменен.  
  
## <a name="remarks"></a>Remarks  
 Параметры могут передаваться через *value* или с помощью @*parameter_name*=*value.* . Параметр не является частью транзакции, поэтому если он изменился в транзакции, которая впоследствии подверглась откату, то прежнее значение параметра не восстанавливается. Возвращаемым вызывающему значением всегда является то значение, которое существует на момент выхода из модуля.  
  
 Если модуль вызывает другой модуль, выполняет управляемый код модуля среды CLR, определяемого пользователем типа или статистического выражения, возникает вложенность. Уровень вложенности увеличивается каждый раз, когда вызванный модуль или управляемый код начинает выполнение, и уменьшается при завершении его выполнения. Превышение максимальной вложенности (32 уровня) приводит к ошибке выполнения всей цепочки вызовов. Текущий уровень вложенности хранится в системной функции @@NESTLEVEL.  
  
 Поскольку удаленные хранимые процедуры и расширенные хранимые процедуры не входят в область транзакции (это не относится к транзакциям, начатым инструкцией BEGIN DISTRIBUTED TRANSACTION или при указании различных параметров конфигурации), осуществить откат команд, выполняемых через вызовы к ним, невозможно. Дополнительные сведения см. в разделах [Системные хранимые процедуры(Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) и [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
 Если выполняется процедура, которая передает переменную типа cursor с размещенным в ней курсором, то возникает ошибка.  
  
 Не надо указывать ключевое слово EXECUTE при выполнении модулей, если эта инструкция стоит первой в пакете.  
  
 Дополнительные сведения, относящиеся к хранимым процедурам CLR, см. в разделе «Хранимые процедуры CLR».  
  
## <a name="using-execute-with-stored-procedures"></a>Выполнение хранимых процедур через EXECUTE  
 Не надо указывать ключевое слово EXECUTE при выполнении хранимых процедур, если эта инструкция стоит первой в пакете.  
  
 Имена системных хранимых процедур [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] начинаются с символов «sp_». Физически они хранятся в [базе данных Resource](../../relational-databases/databases/resource-database.md), но логически относятся к схеме sys любой системной или определяемой пользователем базе данных. При выполнении системной расширенной хранимой процедуры (в пакете или в модуле, например в пользовательской хранимой процедуре или функции) рекомендуется предварять ее имя указанием схемы sys.  
  
 Имена системных расширенных хранимых процедур [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] начинаются с символов «xp_» и содержатся в схеме dbo базы данных master. При выполнении системной расширенной хранимой процедуры (в пакете или в модуле, например в пользовательской хранимой процедуре или функции) рекомендуется предварять ее имя указанием master.dbo.  
  
 При выполнении пользовательской хранимой процедуры (в пакете или в модуле, например в пользовательской хранимой процедуре или функции) рекомендуется предварять ее имя указанием схемы. Не рекомендуется давать пользовательским хранимым процедурам те же имена, что и системным. Дополнительные сведения о выполнении хранимых процедур см. в разделе [Выполнение хранимых процедур](../../relational-databases/stored-procedures/execute-a-stored-procedure.md).  
  
## <a name="using-execute-with-a-character-string"></a>Указание в EXECUTE символьных строк  
 В предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] длина символьной строки была ограничена 8 000 байт. Это требовало динамического объединения длинных строк во время выполнения. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно указать типы данных **varchar(max)** и **nvarchar(max)** , которые позволяют символьным строкам содержать до 2 ГБ данных.  
  
 Изменения в контексте базы данных действуют только до окончания инструкции EXECUTE. Например, после запуска инструкции `EXEC` контекстом базы данных становится master.  
  
```sql  
USE master; EXEC ('USE AdventureWorks2012; SELECT BusinessEntityID, JobTitle FROM HumanResources.Employee;');  
```  
  
## <a name="context-switching"></a>Переключение контекста  
 Предложение `AS { LOGIN | USER } = ' name '` переключает контекст выполнения динамической инструкции. Если переключение контекста указано в виде `EXECUTE ('string') AS <context_specification>`, его длительность ограничена областью действия запроса, в котором он выполняется.  
  
###  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Указание имени пользователя или имени входа  
 Имя пользователя или имя входа, указанное в предложении `AS { LOGIN | USER } = ' name '`, должно присутствовать в качестве участника в представлении sys.database_principals или sys.server_principals соответственно, в противном случае инструкция завершится ошибкой. Кроме того, этому участнику должны быть предоставлены разрешения IMPERSONATE. Если процедура вызывается не владельцем базы данных и не членом предопределенной роли сервера sysadmin, указанный участник должен существовать даже в том случае, если пользователь производит доступ к базе данных или экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в качестве члена группы Windows. Для примера рассмотрим следующие условия.  
  
-   Группа CompanyDomain\SQLUsers имеет доступ к базе данных Sales.  
  
-   Пользователь CompanyDomain\SqlUser1 является членом группы SQLUsers и, таким образом, неявно имеет доступ к базе данных Sales.  
  
 Несмотря на то, что пользователь CompanyDomain\SqlUser1 имеет доступ к базе данных как член группы SQLUsers, инструкция `EXECUTE @string_variable AS USER = 'CompanyDomain\SqlUser1'` завершится ошибкой, так как `CompanyDomain\SqlUser1` не существует в базе данных в качестве участника.  
  
### <a name="best-practices"></a>Рекомендации  
 Указывайте имя входа или пользователя, имеющего минимальные права на операции, выполняемые в инструкции или модуле. Например: не следует указывать имя входа, которое обладает разрешениями уровня сервера, если необходимы только разрешения уровня базы данных. Учетную запись владельца базы данных следует указывать только тогда, когда разрешения, которыми он обладает, действительно необходимы.  
  
## <a name="permissions"></a>Разрешения  
 На выполнение инструкции EXECUTE разрешения не требуются. Однако необходимы разрешения на защищаемые объекты, на которые ссылается командная строка в инструкции EXECUTE. Например, если строка содержит инструкцию INSERT, вызывающий инструкцию EXECUTE пользователь должен иметь разрешение INSERT на целевую таблицу. Разрешения проверяются в месте нахождения инструкции EXECUTE, даже если она содержится внутри модуля.  
  
 Разрешение EXECUTE на модуль по умолчанию имеет владелец модуля, который может передать его другим пользователям. При запуске модуля, выполняющего командную строку, разрешения проверяются в контексте того пользователя, который выполняет модуль, а не того, который его создал. Но в случае, если владельцем вызывающего и вызываемого модуля является один и тот же пользователь, проверка разрешений EXECUTE для второго модуля не выполняется.  
  
 Если модуль производит доступ к другому объекту базы данных, то выполнение завершится успешно при наличии разрешения EXECUTE на модуль и при выполнении одного из следующих условий.  
  
-   Модуль помечен как EXECUTE AS USER или SELF, и владелец модуля обладает соответствующими разрешениями на данный объект. Дополнительные сведения об олицетворении в модуле см. в разделе [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
-   Модуль помечен как EXECUTE AS CALLER, и есть соответствующие разрешения на данный объект.  
  
-   Модуль помечен как EXECUTE AS *user_name*, а *user_name* имеет соответствующие разрешения на объект.  
  
### <a name="context-switching-permissions"></a>Разрешения для переключения контекста  
 Чтобы указать в предложении EXECUTE AS имя входа, вызывающая сторона должна иметь разрешения IMPERSONATE на указанное имя входа. Чтобы указать в предложении EXECUTE AS пользователя базы данных, вызывающая сторона должна иметь разрешения IMPERSONATE на указанное имя входа. Если контекст выполнения не указан или указано EXECUTE AS CALLER, никакие разрешения IMPERSONATE не требуются.  
  
## <a name="examples-sql-server"></a>Примеры: SQL Server
  
### <a name="a-using-execute-to-pass-a-single-parameter"></a>A. Вызов EXECUTE с передачей единственного аргумента  
 Хранимая процедура `uspGetEmployeeManagers` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] ожидает один параметр (`@EmployeeID`). В следующем примере производится выполнение хранимой процедуры `uspGetEmployeeManagers` с `Employee ID 6` в качестве значения параметра.  
  
```sql    
EXEC dbo.uspGetEmployeeManagers 6;  
GO  
```  
  
 При выполнении переменная может быть явно поименована.  
  
```sql    
EXEC dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
 Если приведенная инструкция является первой в пакете или скрипте **osql** или **sqlcmd**, то указывать EXEC не требуется.  
  
```sql    
dbo.uspGetEmployeeManagers 6;  
GO  
--Or  
dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
### <a name="b-using-multiple-parameters"></a>Б. Передача нескольких аргументов  
 В следующем примере выполняется хранимая процедура `spGetWhereUsedProductID` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Передаются два параметра: код продукта `819` и дата проверки `@CheckDate,` со значением типа `datetime`.  
  
```sql    
DECLARE @CheckDate DATETIME;  
SET @CheckDate = GETDATE();  
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;  
GO  
```  
  
### <a name="c-using-execute-tsql_string-with-a-variable"></a>В. Использование EXECUTE tsql_string с переменной  
 Следующий пример показывает, как инструкция `EXECUTE` обрабатывает динамически построенные строки, содержащие переменные. В примере производится создание курсора `tables_cursor`, в который помещается список всех пользовательских таблиц в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], а затем на основе этого списка перестраиваются индексы всех таблиц.  
  
```sql    
DECLARE tables_cursor CURSOR  
   FOR  
   SELECT s.name, t.name   
   FROM sys.objects AS t  
   JOIN sys.schemas AS s ON s.schema_id = t.schema_id  
   WHERE t.type = 'U';  
OPEN tables_cursor;  
DECLARE @schemaname sysname;  
DECLARE @tablename sysname;  
FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN;  
   EXECUTE ('ALTER INDEX ALL ON ' + @schemaname + '.' + @tablename + ' REBUILD;');  
   FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
END;  
PRINT 'The indexes on all tables have been rebuilt.';  
CLOSE tables_cursor;  
DEALLOCATE tables_cursor;  
GO  
  
```  
  
### <a name="d-using-execute-with-a-remote-stored-procedure"></a>Г. Использование EXECUTE с удаленной хранимой процедурой  
 В следующем примере производится выполнение хранимой процедуры `uspGetEmployeeManagers` на удаленном сервере `SQLSERVER1` и сохранение возвращенного состояния выполнения в переменной `@retstat`.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
```sql    
DECLARE @retstat INT;  
EXECUTE @retstat = SQLSERVER1.AdventureWorks2012.dbo.uspGetEmployeeManagers @BusinessEntityID = 6;  
```  
  
### <a name="e-using-execute-with-a-stored-procedure-variable"></a>Д. Использование в инструкции EXECUTE переменной хранимой процедуры  
 В следующем примере создается переменная, которая содержит имя хранимой процедуры.  
  
```sql  
DECLARE @proc_name VARCHAR(30);  
SET @proc_name = 'sys.sp_who';  
EXEC @proc_name;  
  
```  
  
### <a name="f-using-execute-with-default"></a>Е. Указание в инструкции EXECUTE ключевого слова DEFAULT  
 В следующем примере производится создание хранимой процедуры со значениями по умолчанию для первого и третьего аргументов. При запуске эти значения вставляются в первый и третий аргументы, если они не переданы при вызове процедуры. Обратите внимание, что ключевое слово `DEFAULT` может использоваться по-разному.  
  
```sql    
IF OBJECT_ID(N'dbo.ProcTestDefaults', N'P')IS NOT NULL  
   DROP PROCEDURE dbo.ProcTestDefaults;  
GO  
-- Create the stored procedure.  
CREATE PROCEDURE dbo.ProcTestDefaults (  
@p1 SMALLINT = 42,   
@p2 CHAR(1),   
@p3 VARCHAR(8) = 'CAR')  
AS   
   SET NOCOUNT ON;  
   SELECT @p1, @p2, @p3  
;  
GO  
```  
  
 Хранимая процедура `Proc_Test_Defaults`может быть выполнена во множестве разных сочетаний.  
  
```sql    
-- Specifying a value only for one parameter (@p2).  
EXECUTE dbo.ProcTestDefaults @p2 = 'A';  
-- Specifying a value for the first two parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'B';  
-- Specifying a value for all three parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'C', 'House';  
-- Using the DEFAULT keyword for the first parameter.  
EXECUTE dbo.ProcTestDefaults @p1 = DEFAULT, @p2 = 'D';  
-- Specifying the parameters in an order different from the order defined in the procedure.  
EXECUTE dbo.ProcTestDefaults DEFAULT, @p3 = 'Local', @p2 = 'E';  
-- Using the DEFAULT keyword for the first and third parameters.  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'H', DEFAULT;  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'I', @p3 = DEFAULT;  
  
```  
  
### <a name="g-using-execute-with-at-linked_server_name"></a>Ж. Указание AT имя_связанного_сервера в инструкции EXECUTE  
 В следующем примере командная строка передается удаленному серверу. Создается связанный сервер `SeattleSales`, который указывает на другой экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а затем на нем выполняется инструкция DDL (`CREATE TABLE`).  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
```sql    
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
EXECUTE ( 'CREATE TABLE AdventureWorks2012.dbo.SalesTbl   
(SalesID int, SalesName varchar(10)) ; ' ) AT SeattleSales;  
GO  
```  
  
### <a name="h-using-execute-with-recompile"></a>З. Использование инструкции EXECUTE с аргументом WITH RECOMPILE  
 В следующем примере производится выполнение хранимой процедуры `Proc_Test_Defaults` с компиляцией нового плана запроса, который после выполнения модуля удаляется.  
  
```sql    
EXECUTE dbo.Proc_Test_Defaults @p2 = 'A' WITH RECOMPILE;  
GO  
```  
  
### <a name="i-using-execute-with-a-user-defined-function"></a>И. Выполнение определяемой пользователем функции с помощью инструкции EXECUTE  
 В следующем примере выполняется скалярная, определяемая пользователем функция `ufnGetSalesOrderStatusText` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Возвращенное значение сохраняется в переменной `@returnstatus`. Функции передается один входной аргумент `@Status`, который имеет тип данных **tinyint**.  
  
```sql    
DECLARE @returnstatus NVARCHAR(15);  
SET @returnstatus = NULL;  
EXEC @returnstatus = dbo.ufnGetSalesOrderStatusText @Status = 2;  
PRINT @returnstatus;  
GO  
```  
  
### <a name="j-using-execute-to-query-an-oracle-database-on-a-linked-server"></a>К. Применение инструкции EXECUTE для запроса к базе данных Oracle на связанном сервере  
 Следующий пример демонстрирует выполнение нескольких инструкций `SELECT` на удаленном сервере Oracle. Пример начинается с добавления сервера Oracle в качестве связанного и создания имени входа на этом сервере.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver    
        @server='ORACLE',  
        @srvproduct='Oracle',  
        @provider='OraOLEDB.Oracle',   
        @datasrc='ORACLE10';  
  
EXEC sp_addlinkedsrvlogin   
    @rmtsrvname='ORACLE',  
    @useself='false',   
    @locallogin=null,   
    @rmtuser='scott',   
    @rmtpassword='tiger';  
  
EXEC sp_serveroption 'ORACLE', 'rpc out', true;  
GO  
  
-- Execute several statements on the linked Oracle server.  
EXEC ( 'SELECT * FROM scott.emp') AT ORACLE;  
GO  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', 7902) AT ORACLE;  
GO  
DECLARE @v INT;   
SET @v = 7902;  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', @v) AT ORACLE;  
GO   
```  
  
### <a name="k-using-execute-as-user-to-switch-context-to-another-user"></a>Л. Переключение контекста на другого пользователя с помощью инструкции EXECUTE AS USER  
 В следующем примере выполняется командная строка [!INCLUDE[tsql](../../includes/tsql-md.md)], которая создает таблицу и указывает предложение `AS USER` для переключения контекста выполнения инструкции с вызывающего на пользователя `User1`. При запуске инструкции компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] проверит разрешения пользователя `User1`. Пользователь `User1` должен присутствовать в базе данных как пользователь и должен иметь разрешения на создание таблиц в схеме `Sales`; в противном случае инструкция завершается ошибкой.  
  
```sql    
EXECUTE ('CREATE TABLE Sales.SalesTable (SalesID INT, SalesName VARCHAR(10));')  
AS USER = 'User1';  
GO  
```  
  
### <a name="l-using-a-parameter-with-execute-and-at-linked_server_name"></a>М. Использование параметра с командами AT имя_связанного_сервера и EXECUTE  
 В следующем примере командная строка передается удаленному серверу со знаком вопроса (`?`) в качестве заполнителя для параметра. Пример создает связанный сервер `SeattleSales`, который указывает на другой экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а затем выполняется инструкция `SELECT` по отношению к этому связанному серверу. Инструкция `SELECT` использует знак вопроса в качестве заполнителя для параметра `ProductID` (`952`), предоставляемого после инструкции.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
-- Execute the SELECT statement.  
EXECUTE ('SELECT ProductID, Name   
    FROM AdventureWorks2012.Production.Product  
    WHERE ProductID = ? ', 952) AT SeattleSales;  
GO  
```  
  
### <a name="m-using-execute-to-redefine-a-single-result-set"></a>Н. Использование инструкции EXECUTE для переопределения одного результирующего набора  
 В некоторых из предыдущих примеров выполнялась инструкция `EXEC dbo.uspGetEmployeeManagers 6;`, которая возвращала 7 столбцов. Следующий пример показывает использование синтаксиса `WITH RESULT SET` для изменения имени и типов данных возвращаемого результирующего набора.  
  
**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
EXEC uspGetEmployeeManagers 16  
WITH RESULT SETS  
(   
   ([Reporting Level] INT NOT NULL,  
    [ID of Employee] INT NOT NULL,  
    [Employee First Name] NVARCHAR(50) NOT NULL,  
    [Employee Last Name] NVARCHAR(50) NOT NULL,  
    [Employee ID of Manager] NVARCHAR(max) NOT NULL,  
    [Manager First Name] NVARCHAR(50) NOT NULL,  
    [Manager Last Name] NVARCHAR(50) NOT NULL )  
);  
  
```  
  
### <a name="n-using-execute-to-redefine-a-two-result-sets"></a>О. Использование инструкции EXECUTE для переопределения двух результирующих наборов  
 При выполнении инструкции, возвращающей более одного результирующего набора, необходимо определить каждый из ожидаемых результирующих наборов. В следующем примере в [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] создается процедура, которая возвращает два результирующих набора. Затем процедура выполняется с предложением **WITH RESULT SETS** и указывается два определения результирующих наборов.  
  
**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
--Create the procedure  
CREATE PROC Production.ProductList @ProdName NVARCHAR(50)  
AS  
-- First result set  
SELECT ProductID, Name, ListPrice  
    FROM Production.Product  
    WHERE Name LIKE @ProdName;  
-- Second result set   
SELECT Name, COUNT(S.ProductID) AS NumberOfOrders  
    FROM Production.Product AS P  
    JOIN Sales.SalesOrderDetail AS S  
        ON P.ProductID  = S.ProductID   
    WHERE Name LIKE @ProdName  
    GROUP BY Name;  
GO  
  
-- Execute the procedure   
EXEC Production.ProductList '%tire%'  
WITH RESULT SETS   
(  
    (ProductID INT,   -- first result set definition starts here  
    Name NAME,  
    ListPrice MONEY)  
    ,                 -- comma separates result set definitions  
    (Name NAME,       -- second result set definition starts here  
    NumberOfOrders INT)  
);  
  
```  
  ### <a name="o-using-execute-with-at-data_source-data_source_name-to-query-a-remote-sql-server"></a>П. Использование инструкции EXECUTE с data_source_name AT DATA_SOURCE для запроса удаленного SQL Server 
  
 В следующем примере командная строка передается во внешний источник данных, указывающий на экземпляр SQL Server. 
  
**Область применения**: [!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] и более поздних версий
  
```sql    
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE my_sql_server;  
GO  
```  
  
### <a name="p-using-execute-with-at-data_source-data_source_name-to-query-compute-pool-in-sql-server-big-data-cluster"></a>Т. Использование инструкции EXECUTE с data_source_name AT DATA_SOURCE для запроса вычислительного пула в кластере больших данных SQL Server 

 В следующем примере командная строка передается во внешний источник данных, указывающий на вычислительный пул в кластере больших данных SQL Server. В примере создается источник данных `SqlComputePool` для вычислительного пула в кластере больших данных SQL Server и выполняется инструкция `SELECT` к источнику данных. 
  
**Область применения**: [!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] и более поздних версий
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlComputePool 
WITH (LOCATION = 'sqlcomputepool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlComputePool;  
GO  
```  

### <a name="q-using-execute-with-at-data_source-data_source_name-to-query-data-pool-in-sql-server-big-data-cluster"></a>У. Использование инструкции EXECUTE с data_source_name AT DATA_SOURCE для запроса пула данных в кластере больших данных SQL Server 
 В следующем примере командная строка передается во внешний источник данных, указывающий на вычислительный пул в кластере больших данных SQL Server. В примере создается источник данных `SqlDataPool` для пула данных в кластере больших данных SQL Server и выполняется инструкция `SELECT` к источнику данных. 
  
**Область применения**: [!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] и более поздних версий
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlDataPool 
WITH (LOCATION = 'sqldatapool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlDataPool;  
GO  
```

### <a name="r-using-execute-with-at-data_source-data_source_name-to-query-storage-pool-in-sql-server-big-data-cluster"></a>Ф. Использование инструкции EXECUTE с data_source_name AT DATA_SOURCE для запроса пула носителей в кластере больших данных SQL Server 

 В следующем примере командная строка передается во внешний источник данных, указывающий на вычислительный пул в кластере больших данных SQL Server. В примере создается источник данных `SqlStoragePool` для пула данных в кластере больших данных SQL Server и выполняется инструкция `SELECT` к источнику данных. 
  
**Область применения**: [!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] и более поздних версий
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlStoragePool
WITH (LOCATION = 'sqlhdfs://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlStoragePool;  
GO  
```

  
## <a name="examples-azure-synapse-analytics"></a>Примеры: Azure Synapse Analytics 
  
### <a name="a-basic-procedure-execution"></a>A. Выполнение основной процедуры  
 Выполнение хранимой процедуры:  
  
```sql  
EXEC proc1;  
```  
  
 Вызов хранимой процедуры с именем, определенным во время выполнения:  
  
```sql    
EXEC ('EXEC ' + @var);  
```  
  
 Вызов хранимой процедуры из хранимой процедуры:  
  
```sql   
CREATE sp_first AS EXEC sp_second; EXEC sp_third;  
```  
  
### <a name="b-executing-strings"></a>Б. Выполнение строк  
 Выполнение строки SQL:  
  
```sql   
EXEC ('SELECT * FROM sys.types');  
```  
  
 Выполнение вложенной строки:  
  
```sql  
EXEC ('EXEC (''SELECT * FROM sys.types'')');  
```  
  
 Выполнение строковой переменной:  
  
```sql  
DECLARE @stringVar NVARCHAR(100);  
SET @stringVar = N'SELECT name FROM' + ' sys.sql_logins';  
EXEC (@stringVar);  
```  
  
### <a name="c-procedures-with-parameters"></a>В. Процедуры с параметрами  

 В следующем примере создается процедура с параметрами и демонстрируется три способа выполнения процедуры.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROC ProcWithParameters  
    @name NVARCHAR(50),  
@color NVARCHAR(15)  
AS   
SELECT ProductKey, EnglishProductName, Color FROM [dbo].[DimProduct]  
WHERE EnglishProductName LIKE @name  
AND Color = @color;  
GO  
  
-- Executing using positional parameters  
EXEC ProcWithParameters N'%arm%', N'Black';  
-- Executing using named parameters in order  
EXEC ProcWithParameters @name = N'%arm%', @color = N'Black';  
-- Executing using named parameters out of order  
EXEC ProcWithParameters @color = N'Black', @name = N'%arm%';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [@@NESTLEVEL (Transact-SQL)](../../t-sql/functions/nestlevel-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [Программа osql](../../tools/osql-utility.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVERT (Transact-SQL)](../../t-sql/statements/revert-transact-sql.md)   
 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sqlcmd Utility](../../tools/sqlcmd-utility.md)   
 [SUSER_NAME (Transact-SQL)](../../t-sql/functions/suser-name-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [USER_NAME (Transact-SQL)](../../t-sql/functions/user-name-transact-sql.md)   
 [OPENDATASOURCE (Transact-SQL)](../../t-sql/functions/opendatasource-transact-sql.md)   
 [Скалярные пользовательские функции для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)  
  
  
