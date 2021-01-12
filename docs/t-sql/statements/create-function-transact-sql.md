---
description: CREATE FUNCTION (Transact-SQL)
title: CREATE FUNCTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FUNCTION
- CREATE FUNCTION
- CREATE_FUNCTION_TSQL
- FUNCTION_TSQL
- TVF
- MSTVF
dev_langs:
- TSQL
helpviewer_keywords:
- invoking functions
- extended stored procedures [SQL Server], functions
- table-valued parameters
- user-defined functions [SQL Server], creating
- CLR functions [SQL Server]
- CREATE FUNCTION statement
- nondeterministic functions
- user-defined data types
- functions [SQL Server], creating
- inline table-valued functions [SQL Server]
- multi-statement table-valued functions [SQL Server]
- table-valued functions [SQL Server], CREATE FUNCTION
- parameters [SQL Server], functions
- nesting user-defined functions
- deterministic functions
- scalar-valued functions
- scalar UDF
- MSTVF
- TVF
- functions [SQL Server], invoking
ms.assetid: 864b393f-225f-4895-8c8d-4db59ea60032
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cb4c79d1f4c8c7b8103ff9d34b8f32fd497b8c6a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095815"
---
# <a name="create-function-transact-sql"></a>CREATE FUNCTION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Создает определяемую пользователем функцию в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Определяемая пользователем функция представляет собой подпрограмму [!INCLUDE[tsql](../../includes/tsql-md.md)] или среды CLR, которая принимает параметры, выполняет действия, такие как сложные вычисления, а затем возвращает результат этих действий в виде значения. Возвращаемое значение может быть скалярным значением или таблицей. При помощи этой инструкции можно создать подпрограмму, которую можно повторно использовать следующими способами.

- В инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)], например SELECT.

- В приложениях, вызывающих функцию.

- В определении другой пользовательской функции.

- Для параметризации представления или улучшения функциональности индексированного представления.

- Для определения столбца таблицы.

- Для определения ограничения CHECK на столбец.

- Для замены хранимой процедуры.

- Использование встроенной функции в качестве предиката фильтра для политики безопасности

> [!NOTE]
>
> - В этом разделе рассматривается интеграция среды CLR .NET Framework с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Интеграция со средой CLR не применяется к [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
> - [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] — см. статью [CREATE FUNCTION (Azure Synapse Analytics)](../../t-sql/statements/create-function-sql-data-warehouse.md).

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Transact-SQL Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
 [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN scalar_expression
    END
[ ; ]
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

```syntaxsql
-- Transact-SQL Multi-Statement Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [READONLY] }
    [ ,...n ]
  ]
)
RETURNS @return_variable TABLE <table_type_definition>
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN
    END
[ ; ]

```

```syntaxsql
-- Transact-SQL Function Clauses
<function_option>::=
{
    [ ENCRYPTION ]
  | [ SCHEMABINDING ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
  | [ INLINE = { ON | OFF }]
}

<table_type_definition>:: =
( { <column_definition> <column_constraint>
  | <computed_column_definition> }
    [ <table_constraint> ] [ ,...n ]
)
<column_definition>::=
{
    { column_name data_type }
    [ [ DEFAULT constant_expression ]
      [ COLLATE collation_name ] | [ ROWGUIDCOL ]
    ]
    | [ IDENTITY [ (seed , increment ) ] ]
    [ <column_constraint> [ ...n ] ]
}

<column_constraint>::=
{
    [ NULL | NOT NULL ]
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      [ WITH FILLFACTOR = fillfactor
        | WITH ( < index_option > [ , ...n ] )
      [ ON { filegroup | "default" } ]
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<computed_column_definition>::=
column_name AS computed_column_expression

<table_constraint>::=
{
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      ( column_name [ ASC | DESC ] [ ,...n ]
        [ WITH FILLFACTOR = fillfactor
        | WITH ( <index_option> [ , ...n ] )
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<index_option>::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS ={ ON | OFF }
}
```

```syntaxsql
-- CLR Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS { return_data_type }
    [ WITH <clr_function_option> [ ,...n ] ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS TABLE <clr_table_type_definition>
    [ WITH <clr_function_option> [ ,...n ] ]
    [ ORDER ( <order_clause> ) ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Function Clauses
<order_clause> ::=
{
   <column_name_in_clr_table_type_definition>
   [ ASC | DESC ]
} [ ,...n]

<method_specifier>::=
    assembly_name.class_name.method_name

<clr_function_option>::=
}
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
}

<clr_table_type_definition>::=
( { column_name data_type } [ ,...n ] )
```

```syntaxsql
-- In-Memory OLTP: Syntax for natively compiled, scalar user-defined function
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
    [ NULL | NOT NULL ] [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
     WITH <function_option> [ ,...n ]
    [ AS ]
    BEGIN ATOMIC WITH (set_option [ ,... n ])
        function_body
        RETURN scalar_expression
    END

<function_option>::=
{
  |  NATIVE_COMPILATION
  |  SCHEMABINDING
  | [ EXECUTE_AS_Clause ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
}

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*OR ALTER*
**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 1 (SP1) и выше), а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Условно изменяет функцию только в том случае, если она уже существует.

> [!NOTE]
> Необязательный синтаксис [OR ALTER] для среды CLR доступен, начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 1 (SP1) с накопительным пакетом обновления 1 (CU1).

*schema_name*. Имя схемы, к которой принадлежит определяемая пользователем функция.

*function_name*. Имя определяемой пользователем функции. Имена функций должны удовлетворять правилам построения [идентификаторов](../../relational-databases/databases/database-identifiers.md) и быть уникальными в пределах базы данных и схемы.

> [!NOTE]
> Даже при отсутствии аргументов скобки после имени функции обязательны.

@*parameter_name*. Параметр пользовательской функции. Может быть объявлен один или несколько аргументов.

Для функций допускается не более 2 100 параметров. При выполнении функции значение каждого из объявленных параметров должно быть указано пользователем, если для них не определены значения по умолчанию.

Определяет имя параметра, используя знак @ как первый символ. Имя параметра должно соответствовать правилам для идентификаторов. Параметры являются локальными в пределах функции, в разных функциях могут быть использованы одинаковые имена параметров. Аргументы могут использоваться только вместо констант. Они не могут использоваться вместо имен таблиц, имен столбцов или имен других объектов базы данных.

> [!NOTE]
> Параметры ANSI_WARNINGS не годятся для передачи в хранимые процедуры, пользовательские функции и при объявлении и установке переменных в пакетных инструкциях. Например, если объявить переменную как **char(3)** , а затем присвоить ей значение длиннее трех символов, данные будут усечены до размера переменной, а инструкция `INSERT` или `UPDATE` завершится без ошибок.

[ *type_schema_name*. ] *parameter_data_type*. Тип данных параметра, а также схема, к которой он принадлежит. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допустимы любые типы данных, включая определяемые пользователем типы данных CLR и определяемые пользователем табличные типы, за исключением типа данных **timestamp**. Для функций CLR допустимы все типы данных, включая определяемые пользователем типы данных CLR, за исключением типов данных **text**, **ntext**, **image**, определяемых пользователем табличных типов и типов данных **timestamp**. Нескалярные типы **cursor** и **table** не могут быть указаны в качестве типов данных параметров ни для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], ни для функций CLR.

Если *type_schema_name* не указан, [!INCLUDE[ssDE](../../includes/ssde-md.md)] выполняет поиск *scalar_parameter_data_type* в следующем порядке:

- в схеме, содержащей имена системных типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];
- в установленной по умолчанию для текущего пользователя схеме в текущей базе данных;
- Схема **dbo** в текущей базе данных.

[ =*default* ] Значение по умолчанию для параметра. Если определено значение *default*, то функция выполняется даже в том случае, если для данного параметра значение не указано.

> [!NOTE]
> Для функций CLR также могут указываться значения параметров по умолчанию, кроме типов данных **varchar(max)** и **varbinary(max)** .

Если параметр функции имеет значение по умолчанию, то для него должно быть указано ключевое слово DEFAULT для получения функцией значения по умолчанию. Применение ключевого слова DEFAULT следует отличать от использования аргументов со значениями по умолчанию в хранимых процедурах, когда не указанный аргумент неявно принимает значение по умолчанию. Однако ключевое слово DEFAULT не требуется при вызове скалярной функции с помощью инструкции EXECUTE.

READONLY. Указывает, что параметр не может быть обновлен или изменен при определении функции. Параметр READONLY необходим для определяемых пользователем параметров табличного типа (TVP) и не может использоваться для других типов параметров.

*return_data_type*. Возвращаемое значение скалярной пользовательской функции. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типа данных **timestamp**. Для функций CLR допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типов данных **text**, **ntext**, **image** и **timestamp**. Нескалярные типы **cursor** и **table** не могут быть указаны в качестве возвращаемых типов данных ни для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], ни для функций CLR.

*function_body*. Указывает серию инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], совместная работа которых не вызывает побочных эффектов, например изменения содержимого таблиц, и формирует значение функции. *function_body* используется только в скалярных функциях и функциях с табличным значением из нескольких инструкций (MSTVF).

Для скалярных функций *function_body* представляет собой ряд инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], которые в совокупности вычисляют скалярное выражение.

Для функций с табличным значением из нескольких инструкций *function_body* является рядом инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], заполняющих возвращаемую переменную TABLE.

*scalar_expression*. Указывает скалярное значение, возвращаемое скалярной функцией.

TABLE. Указывает, что возвращаемым значением функции с табличным значением (TVF) является таблица. Функциям с табличным значением могут передаваться только константы и @*local_variables*.

Во встроенных функциях с табличным значением возвращаемое значение TABLE определяется при использовании единственной инструкции SELECT. Встроенные функции не имеют соответствующих возвращаемых переменных.

<a name="mstvf"></a> В функциях с табличным значением из нескольких инструкций переменной \@*return_variable* является переменная TABLE, используемая для сохранения данных и накопления строк, которые будут возвращены в качестве значения функции. Аргумент \@*return_variable* может быть указан только для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], но не для функций CLR.

*select_stmt*. Единственная инструкция SELECT, которая определяет возвращаемое значение встроенной функции, возвращающей табличное значение (TVF).

ORDER (\<order_clause>). Указывает порядок, в котором возвращаются результаты из функции с табличным значением. Дополнительные сведения см. в разделе [Использование порядка сортировки в функциях с табличным значением CLR](#using-sort-order-in-clr-table-valued-functions) далее в этом разделе.

EXTERNAL NAME \<method_specifier> *имя_сборки*.*имя_класса*.*имя_метода*
**Относится к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с пакетом обновления 1 (SP1) и выше)

Указывает сборку и метод, на которые должно ссылаться имя создаваемой функции.

- *assembly_name* — должно соответствовать значению в столбце `name` инструкции `SELECT * FROM sys.assemblies;`.

  Это имя, которое использовалось в инструкции `CREATE ASSEMBLY`.

- *class_name* — должно соответствовать значению в столбце `assembly_name` инструкции `SELECT * FROM sys.assembly_modules;`.

  Часто это значение содержит точку или пунктир. В таких случаях согласно требованиям синтаксиса Transact-SQL значение должно заключаться в квадратные скобки [] или в двойные кавычки "".

- *method_name* — должно соответствовать значению в столбце `method_name` инструкции `SELECT * FROM sys.assembly_modules;`.

  Метод должен быть статическим.

В типичном примере для библиотеки MyFood.DLL, в которой все типы находятся в пространстве имен MyFood, значение `EXTERNAL NAME` может быть следующим: `MyFood.[MyFood.MyClass].MyStaticMethod`

> [!NOTE]
>
> - По умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не производит выполнение кода CLR. Можно создавать, изменять и удалять объекты базы данных со ссылками на модули среды CLR, но [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выполняет их до тех пор, пока не будет включен параметр [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Для его включения воспользуйтесь хранимой процедурой [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).
> - Этот параметр недоступен в автономной базе данных.

*\<*table_type_definition*>* ( { \<column_definition> \<column_constraint>| \<computed_column_definition> } [ \<table_constraint> ] [ ,...*n* ] ). Определяет тип табличных данных для функции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Объявление таблицы включает определения столбцов, а также ограничений для столбцов и таблиц. Таблица всегда помещается в первичную файловую группу.

*\< clr_table_type_definition >* ( { *имя_столбца**тип_данных* } [ ,...*n* ] ) **Относится к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с пакетом обновления 1 (SP1) и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ([предварительная версия в некоторых регионах](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)).

Определяет табличные типы данных для функции CLR. Объявление таблицы включает только имена столбцов и типы данных. Таблица всегда помещается в первичную файловую группу.

NULL|NOT NULL. Поддерживается только для скомпилированных в собственном коде скалярных определяемых пользователем функций. Дополнительные сведения см. в разделе [Скалярные определяемые пользователем функции для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).

NATIVE_COMPILATION. Указывает, скомпилирована ли в собственном коде определяемая пользователем функция. Этот аргумент требуется только для скомпилированных в собственном коде скалярных определяемых пользователем функций.

BEGIN ATOMIC WITH. Поддерживается только для скомпилированных в собственном коде скалярных определяемых пользователем функций и является обязательным. Дополнительные сведения см. в статье [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).

SCHEMABINDING. Аргумент SCHEMABINDING требуется для скомпилированных в собственном коде скалярных определяемых пользователем функций.

EXECUTE AS. Аргумент EXECUTE AS требуется для скомпилированных в собственном коде скалярных определяемых пользователем функций.

**\<function_option>::= и \<clr_function_option>::=**

Указывает, что функция будет иметь один или несколько из следующих параметров.

ENCRYPTION **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с пакетом обновления 1 (SP1) и выше)

Указывает, что компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] преобразует исходный текст инструкции CREATE FUNCTION в скрытый формат. Выходные данные запутывания не видны непосредственно ни в одном представлении каталога. Пользователи, не имеющие доступа к системным таблицам или файлам баз данных, не смогут получить скрытый текст. Однако этот текст будет доступен привилегированным пользователям, которые либо смогут обращаться к системным таблицам через [порт DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md), либо будут иметь непосредственный доступ к файлам баз данных. Кроме того, пользователи, имеющие право на подключение отладчика к серверному процессу, могут получить исходный текст процедуры из памяти во время выполнения. Дополнительные сведения о доступе к метаданным системы см. в статье [Настройка видимости метаданных](../../relational-databases/security/metadata-visibility-configuration.md).

Использование этого параметра препятствует публикации данной функции как части репликации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот параметр для функций CLR указывать нельзя.

SCHEMABINDING. Указывает, что функция привязана к объектам базы данных, которые содержат ссылки на нее. Если аргумент SCHEMABINDING указан, нельзя изменить базовые объекты таким способом, который может повлиять на определение функции. Сначала нужно изменить или удалить само определение функции, чтобы удалить зависимости от объекта, который требуется изменить.

Привязка функции к ссылающимся на нее объектам удаляется в следующих случаях:

- При удалении функции.
- При изменении функции инструкцией `ALTER`, если не указан параметр `SCHEMABINDING`.

Функция может быть привязана к схеме только в том случае, если выполняются следующие условия.

- Функция является функцией [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Пользовательские функции и представления, на которые ссылается данная функция, также привязаны к схеме.
- Объекты, на которые ссылается функция, указываются двухкомпонентными именами.
- Функция и объекты, на которые она ссылается, относятся к одной и той же базе данных.
- Пользователь, выполняющий инструкцию `CREATE FUNCTION`, имеет разрешение `REFERENCES` на объекты базы данных, на которые ссылается функция.

RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT** Указывает атрибут **OnNULLCall** для скалярной функции. Если данный аргумент не указан, по умолчанию предполагается CALLED ON NULL INPUT. Это означает, что текст функции выполняется даже в том случае, если в качестве аргумента передано значение NULL.

Если атрибут RETURNS NULL ON NULL INPUT указан для функции CLR, это означает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может вернуть NULL, не вызывая при этом тело функции в том случае, если в качестве какого-либо из аргументов указано значение NULL. Если метод функции CLR, указанный в \<method_specifier>, уже имеет пользовательский атрибут, указывающий RETURNS NULL ON NULL INPUT, но инструкция CREATE FUNCTION указывает CALLED ON NULL INPUT, то приоритет имеет инструкция CREATE FUNCTION. Атрибут **OnNULLCall** не может быть указан для функций CLR с табличным значением.

Предложение EXECUTE AS. Указывает контекст безопасности, в котором выполняется определяемая пользователем функция. Иными словами, есть возможность управлять тем, какую учетную запись пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует при определении разрешений на объекты базы данных, на которые ссылается функция.

> [!NOTE]
> `EXECUTE AS` недопустимо указывать для встроенных функций с табличным значением.

Дополнительные сведения см. в разделе [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md).

INLINE = { ON | OFF } **ОБЛАСТЬ ПРИМЕНЕНИЯ**: SQL Server 2019 и более поздних версий.

Указывает, должна ли эта скалярная пользовательская функция быть встроенной. Это предложение применяется только к скалярным пользовательским функциям. Предложение `INLINE` не является обязательным. Если предложение `INLINE` не указано, оно автоматически принимает значение ON или OFF в зависимости от возможности встраивания пользовательской функции. Если `INLINE=ON` указано, но функция не может быть встраиваемой, выдается ошибка. Дополнительные сведения: [Встраивание скалярной функции, определяемой пользователем](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).

**\< column_definition >::=**

Определяет тип данных таблицы. Декларация таблицы включает определения столбцов и ограничений. Для функций CLR можно указать только *column_name* и *data_type*.

*column_name*. Имя столбца в таблице. Имена столбцов должны соответствовать правилам для идентификаторов и быть уникальными в рамках таблицы. *column_name* может иметь длину от 1 до 128 символов.

*data_type*. Указывает тип данных в столбце. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типа данных **timestamp**. Для функций CLR допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типов данных **text**, **ntext**, **image**, **char**, **varchar**, **varchar(max)** и **timestamp**. Нескалярный тип данных **cursor** не может указываться в качестве типа данных столбца ни для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], ни для функций CLR.

DEFAULT *constant_expression*. Указывает значение, присваиваемое столбцу, если значение явно не задано при вставке. Выражение *constant_expression* является константой, значением NULL или значением системной функции. Определения DEFAULT могут применяться к любым столбцам, кроме тех, которые имеют свойство IDENTITY. Предложение DEFAULT не может быть указано для функций CLR с табличным значением.

COLLATE *collation_name* — задает параметры сортировки для столбца. Если не указано, столбцу назначаются параметры сортировки, принятые в базе данных по умолчанию. Именем параметров сортировки может быть либо имя параметров сортировки Windows, либо имя параметров сортировки SQL. Список и дополнительные сведения о параметрах сортировки см. в разделах [Имя параметра сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) и [Имя параметра сортировки SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Предложение COLLATE может быть использовано для изменения параметров сортировки только для столбцов с типом данных **char**, **varchar**, **nchar** или **nvarchar**.

> [!NOTE]
> `COLLATE` недопустимо указывать для функций с табличным значением в среде выполнения.

ROWGUIDCOL. Показывает, что новый столбец является строковым столбцом идентификаторов GUID. Только один столбец типа **uniqueidentifier** в таблице может быть назначен в качестве столбца ROWGUIDCOL. Свойство ROWGUIDCOL может быть присвоено только столбцу типа **uniqueidentifier**.

Свойство ROWGUIDCOL не обеспечивает уникальности значений, хранимых в столбце. Кроме того, данное свойство не производит автоматическое формирование значений для новых строк, вставляемых в таблицу. Для создания уникальных значений произвольного столбца используйте функцию NEWID в инструкции INSERT. Может быть указано значение по умолчанию. При этом функция NEWID не может быть указана в качестве значения по умолчанию.

IDENTITY — указывает, что новый столбец является столбцом идентификаторов. При добавлении в таблицу новой строки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] формирует для столбца уникальное, последовательное значение. Столбцы идентификаторов обычно используются с ограничением PRIMARY KEY для поддержания уникальности идентификаторов строк в таблице. Свойство IDENTITY может назначаться для столбцов типа **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** или **numeric(p,0)** . Для каждой таблицы можно создать только один столбец идентификаторов. Ограниченные значения по умолчанию и ограничения DEFAULT не могут использоваться в столбце идентификаторов. Необходимо указывать либо оба аргумента (и *seed*, и *increment*), либо не указывать ни одного из них. Если ничего не указано, применяется значение по умолчанию (1,1).

Предложение IDENTITY не может быть указано для функций CLR с табличным значением.

*seed*. Целочисленное значение, присваиваемое первой строке таблицы.

*increment*. Целочисленное значение, добавляемое к значению *seed* для каждой последующей строки таблицы.

 **\< column_constraint >::= и \< table_constraint>::=**

Определяет ограничение для указанного столбца или таблицы. Для функций CLR единственное допустимое ограничение — NULL. Именованные ограничения недопустимы.

NULL | NOT NULL. Определяет, допустимы ли для столбца значения NULL. Параметр NULL не является ограничением в строгом смысле слова, но может быть указан так же, как и NOT NULL. Ограничение NOT NULL не может быть указано для функций CLR с табличным значением.

PRIMARY KEY. Ограничение, обеспечивающее целостность сущностей для указанного столбца через уникальный индекс. В возвращающих табличное значение пользовательских функциях ограничение PRIMARY KEY может быть создано только для одного столбца таблицы. Ограничение PRIMARY KEY не может быть указано для функций CLR с табличным значением.

UNIQUE — ограничение, которое обеспечивает целостность сущностей для указанного столбца или столбцов с помощью уникального индекса. В таблице может быть несколько ограничений UNIQUE. Ограничение UNIQUE не может быть указано для функций CLR с табличным значением.

CLUSTERED | NONCLUSTERED — указывает, что для ограничения PRIMARY KEY или UNIQUE создается кластеризованный или некластеризованный индекс. Ограничения PRIMARY KEY используют параметр CLUSTERED, а ограничения UNIQUE используют параметр NONCLUSTERED.

Параметр CLUSTERED может быть указан только для одного ограничения. Если параметр CLUSTERED указан для ограничения UNIQUE и указано ограничение PRIMARY KEY, то PRIMARY KEY использует NONCLUSTERED.

Параметры СLUSTERED и NONСLUSTERED не могут быть указаны для функций CLR с табличным значением.

CHECK — ограничение, обеспечивающее целостность домена путем ограничения возможных значений, которые можно ввести в столбец или столбцы. Ограничения CHECK не могут быть указаны для функций CLR с табличным значением.

*logical_expression* — логическое выражение, возвращающее значения TRUE или FALSE.

**\<computed_column_definition>::=**

Указывает вычисляемый столбец. Дополнительные сведения о вычисляемых столбцах см. в разделе [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).

*column_name*. Имя вычисляемого столбца.

*computed_column_expression*. Выражение, определяющее значение вычисляемого столбца.

**\<index_option>::=**

Указывает параметры индекса для индекса PRIMARY KEY или UNIQUE. Дополнительные сведения о параметрах индекса см. в разделе [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** } Задает заполнение индекса. Значение по умолчанию — OFF.

FILLFACTOR = *fillfactor*. Определяет величину в процентах, показывающую насколько компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен заполнять конечный уровень каждой страницы индекса во время его создания и изменения. Значение *fillfactor* должно быть целым числом от 1 до 100. Значение по умолчанию равно 0.

IGNORE_DUP_KEY = { ON | **OFF** } — определяет реакцию на ошибку, возникающую при попытке вставки повторяющихся ключевых значений в уникальный индекс. Параметр IGNORE_DUP_KEY применяется только к операциям вставки, производимым после создания или перестроения индекса. Значение по умолчанию — OFF.

STATISTICS_NORECOMPUTE = { ON | **OFF** } Указывает, выполнялся ли перерасчет статистики распределения. Значение по умолчанию — OFF.

ALLOW_ROW_LOCKS = { **ON** | OFF } Указывает, разрешена ли блокировка строк. Значение по умолчанию — ON.

ALLOW_PAGE_LOCKS = { **ON** | OFF } Указывает, разрешена ли блокировка страниц. Значение по умолчанию — ON.

## <a name="best-practices"></a>Рекомендации

Если определяемая пользователем функция создана без применения предложения `SCHEMABINDING`, то изменения базовых объектов могут повлиять на определение функции и привести к непредвиденным результатам при вызове функции. Рекомендуется реализовать один из следующих методов, чтобы обеспечить, что функция не устареет из-за изменения ее базовых объектов.

- Укажите при создании функции предложение `WITH SCHEMABINDING`. Это обеспечит невозможность изменения объектов, на которые ссылается определение функции, если при этом не изменяется сама функция.
- Выполняйте хранимую процедуру [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) после изменения любого объекта, указанного в определении функции.

> [!IMPORTANT]
> Дополнительные сведения и информацию о производительности в связи со встроенными функциями с табличным значением (встроенные TVF) или функциями с табличным значением из нескольких инструкций (MSTVF) см. в разделе [Создание определяемых пользователем функций (ядро СУБД)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

## <a name="data-types"></a>Типы данных

Если в функции CLR указаны параметры, они должны иметь тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], как было ранее определено для *scalar_parameter_data_type*. Дополнительные сведения о сравнении системных типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с типами данных интеграции со средой CLR и типами данных среды CLR платформы [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] см. в разделе [Сопоставление данных параметров CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

Чтобы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] мог ссылаться на правильный метод при перегрузке в классе, метод, указанный в \<method_specifier>, должен иметь следующие характеристики.

- Принимать то же число параметров, которое указано в [ ,...*n* ].
- Принимать все параметры по значению, а не по ссылке.
- Принимать типы параметров, совместимые с теми, что указаны в функции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Если в качестве возвращаемого значения функции CLR указан табличный тип (RETURNS TABLE), то возвращаемые данные метода в \<method_specifier> должны иметь тип **IEnumerator** или **Enumerable** и подразумевается, что этот интерфейс реализуется автором функции. В отличие от функций [!INCLUDE[tsql](../../includes/tsql-md.md)] функции CLR не могут содержать ограничения PRIMARY KEY, UNIQUE и CHECK в \<table_type_definition>. Типы данных столбцов, указанные в \<table_type_definition>, должны совпадать с типами данных соответствующих столбцов результирующего набора, возвращаемого методом в \<method_specifier> во время выполнения. Проверка типов на этапе создания функции не производится.

Дополнительные сведения о программировании функций CLR см. в разделе [Определяемые пользователем функции среды CLR](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md).

## <a name="general-remarks"></a>Общие замечания

Скалярная функция может быть указана в любом месте вместо скалярного выражения, в том числе в вычисляемых столбцах и определениях ограничений CHECK. Кроме того, скалярная функция может быть выполнена инструкцией [EXECUTE](../../t-sql/language-elements/execute-transact-sql.md). Скалярные функции должны вызываться с помощью как минимум двухкомпонентного имени ( *<schema>.<function>* ). Дополнительные сведения о многокомпонентных именах см. в разделе [Соглашения о синтаксисе в Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md). Функция, возвращающая табличное значение, может быть вызвана в любом месте, где допускаются табличные выражения, — в предложении `FROM`ROM инструкций `SELECT`, `INSERT`, `UPDATE` или `DELETE`. Дополнительные сведения см. в разделе [Выполнение определяемых пользователем функций](../../relational-databases/user-defined-functions/execute-user-defined-functions.md).

## <a name="interoperability"></a>Совместимость

В функциях допустимы следующие инструкции.

- Инструкции присваивания.
- Инструкции управления потоком, за исключением инструкций `TRY...CATCH`.
- Инструкции `DECLARE`, объявляющие локальные переменные и локальные курсоры.
- Инструкции `SELECT`, которые содержат списки выбора с выражениями, присваивающими значения локальным переменным.
- Операции над локальными курсорами, которые объявляются, открываются, закрываются и освобождаются в теле функции. Допустимы только те инструкции `FETCH`, которые предложением `INTO` присваивают значения локальным переменным. Инструкции `FETCH`, возвращающие данные клиенту, недопустимы.
- Инструкции `INSERT`, `UPDATE` и `DELETE`, которые изменяют локальные табличные переменные.
- Инструкции `EXECUTE`, вызывающие расширенные хранимые процедуры.

Дополнительные сведения см. в разделе [Создание определяемых пользователем функций (ядро СУБД)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="computed-column-interoperability"></a>Взаимодействие с вычисляемыми столбцами

Функции имеют перечисленные ниже свойства. Значения этих свойств определяют, может ли данная функция быть указана в вычисляемых столбцах, которые могут быть материализованными или индексированными.

|Свойство|Описание|Примечания|
|--------------|-----------------|-----------|
|**IsDeterministic**|Функция детерминированная или недетерминированная.|Для детерминированных функций разрешается доступ к локальным данным. Например, функция, которая при вызове с одними и теми же параметрами и в одном том же состоянии базы данных всегда возвращает один и тот же результат, называется детерминированной.|
|**IsPrecise**|Функция точная или неточная.|Неточные функции содержат такие операции, как операции с плавающей запятой.|
|**IsSystemVerified**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может проверять свойства точности и детерминированности функций.||
|**SystemDataAccess**|Функции, производящие доступ к системным данным (системным каталогам или виртуальным системным таблицам) в локальном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].||
|**UserDataAccess**|Функция производит доступ к данным пользователя в локальном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Сюда входят определяемые пользователем и временные таблицы, но не табличные переменные.|

Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] свойства точности и детерминизма [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определяет автоматически. Свойства доступа к данным и детерминированности функций CLR могут быть указаны пользователем. Дополнительные сведения см. в разделе [Общие сведения о пользовательских атрибутах интеграции со средой CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md).

Для отображения текущих значений этих свойств используйте функцию [OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md).

> [!IMPORTANT]
> Функции необходимо создавать с детерминированным `SCHEMABINDING`.

Вычисляемый столбец, который обращается к определяемой пользователем функции, может быть включен в индекс, если функция имеет следующие значения свойств:

- **IsDeterministic** = true
- **IsSystemVerified** = true (если вычисляемый столбец не является сохраняемым)
- **UserDataAccess** = false
- **SystemDataAccess** = false

Дополнительные сведения см. в разделе [Индексы вычисляемых столбцов](../../relational-databases/indexes/indexes-on-computed-columns.md).

### <a name="calling-extended-stored-procedures-from-functions"></a>Вызов расширенной хранимой процедуры из функций

Расширенные хранимые процедуры, если они вызываются из тела функции, не могут возвращать клиенту результирующие наборы. Все API ODS, которые возвращают результирующие наборы клиенту, вернут FAIL. Расширенная хранимая процедура может подключаться к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], но она не должна пытаться присоединиться к той же транзакции, что и функция, из которой вызвана расширенная хранимая процедура.

Как и при вызове из пакета или хранимой процедуры, расширенная хранимая процедура будет выполняться в контексте учетной записи системы безопасности Windows, от имени которой выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Владелец хранимой процедуры должен это понимать, когда он предоставляет пользователям разрешение EXECUTE на нее.

## <a name="limitations-and-restrictions"></a>Ограничения

Определяемые пользователем функции не могут выполнять действия, изменяющие состояние базы данных.

Определяемые пользователем функции не могут содержать предложение `OUTPUT INTO`, целью которого является таблица.

Следующие инструкции компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] не могут быть включены в определение определяемой пользователем функции [!INCLUDE[tsql](../../includes/tsql-md.md)]:

- `BEGIN DIALOG CONVERSATION`
- `END CONVERSATION`
- `GET CONVERSATION GROUP`
- `MOVE CONVERSATION`
- `RECEIVE`
- `SEND`

Определяемые пользователем функции могут быть вложенными, то есть из одной функции может быть вызвана другая. Уровень вложенности увеличивается на единицу каждый раз, когда начинается выполнение вызванной функции и уменьшается на единицу, когда ее выполнение завершается. Вложенность определяемых пользователем функций не может превышать 32 уровней. Превышение максимального уровня вложенности приводит к ошибке выполнения для всей цепочки вызываемых функций. Каждый вызов управляемого кода из определяемой пользователем функции [!INCLUDE[tsql](../../includes/tsql-md.md)] считается за один уровень вложенности из 32 возможных. Методы, вызываемые из управляемого кода, под это ограничение не подпадают.

### <a name="using-sort-order-in-clr-table-valued-functions"></a>Использование порядка сортировки в функциях CLR с табличным значением

При использовании предложения `ORDER` в функциях CLR с табличным значением придерживайтесь следующих рекомендаций.

- Необходимо гарантировать, чтобы результаты всегда были упорядочены в указанном порядке. Если результаты находятся не в указанном порядке, при выполнении запроса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сформирует сообщение об ошибке.
- Если указано предложение `ORDER`, выходные данные функции с табличным значением должны быть отсортированы в соответствии с параметрами сортировки столбца (явными или неявными). Например, если для столбца используются параметры сортировки для китайского языка (указанные в определении функции с табличным значением или полученные из параметров сортировки базы данных), то возвращаемые результаты должны быть отсортированы в соответствии с правилами сортировки, принятыми в китайском языке.
- При использовании предложение `ORDER` всегда проверяется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при возврате результатов, независимо от его использования обработчиком запросов для выполнения оптимизации. Рекомендуется использовать предложение `ORDER` только при уверенности в его пользе для обработчика запросов.
- Обработчик запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически использует преимущества предложения `ORDER` в следующих случаях.

  - Запросы Insert, в которых предложение `ORDER` совместимо с индексом.
  - Предложения `ORDER BY`, совместимые с предложением `ORDER`.
  - Статистические выражения, где `GROUP BY` совместим с предложением `ORDER`.
  - Статистические выражения с ключевым словом `DISTINCT`, в которых уникальные столбцы совместимы с предложением `ORDER`.

Предложение `ORDER` не гарантирует упорядочивания результатов при выполнении запроса SELECT, если `ORDER BY` не указано в самом запросе. Сведения о запросе столбцов, включенных в порядок сортировки для функций с табличным значением, см. в разделе [sys.function_order_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-function-order-columns-transact-sql.md).

## <a name="metadata"></a>Метаданные

В следующей таблице приводятся системные представления каталога, возвращающие метаданные об определяемых пользователем функциях.

|Системное представление|Описание|
|-----------------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|См. пример Д, приведенный ниже в разделе с примерами.|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|Выводит сведения об определяемых пользователем функциях CLR.|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|Выводит сведения о параметрах, определенных в определяемых пользователем функциях.|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|Отображает базовые объекты, на которые ссылается функция.|

## <a name="permissions"></a>Разрешения

Требуется разрешение `CREATE FUNCTION` на базу данных и разрешение `ALTER` для схемы, в которой создается функция. Если в функции указан определяемый пользователем тип, требуется разрешение `EXECUTE` на этот тип.

## <a name="examples"></a>Примеры

> [!NOTE]
> Дополнительные примеры и рекомендации по производительности для определяемых пользователем функций см. в разделе [Создание определяемых пользователем функций (ядро СУБД)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="a-using-a-scalar-valued-user-defined-function-that-calculates-the-iso-week"></a>A. Применение скалярной определяемой пользователем функции, вычисляющей неделю по ISO

В следующем примере показано создание определяемой пользовательской функции `ISOweek`, которая получает в качестве аргумента дату и вычисляет номер недели по ISO. Для правильной работы этой функции перед ее вызовом должна быть выполнена инструкция `SET DATEFIRST 1`.

Следующий пример также показывает использование предложения [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) для указания контекста безопасности, в котором может быть выполнена хранимая процедура. В этом примере параметр `CALLER` указывает, что процедура будет выполнена в контексте пользователя, который ее вызывает. Также могут быть указаны параметры `SELF`, `OWNER` и *user_name*.

Показан вызов функции. Обратите внимание, что `DATEFIRST` устанавливается в значение `1`.

```sql
CREATE FUNCTION dbo.ISOweek (@DATE datetime)
RETURNS int
WITH EXECUTE AS CALLER
AS
BEGIN
    DECLARE @ISOweek int;
    SET @ISOweek= DATEPART(wk,@DATE)+1
        -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104');
--Special cases: Jan 1-3 may belong to the previous year
    IF (@ISOweek=0)
        SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1
            AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1;
--Special case: Dec 29-31 may belong to the next year
    IF ((DATEPART(mm,@DATE)=12) AND
        ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28))
    SET @ISOweek=1;
    RETURN(@ISOweek);
END;
GO
SET DATEFIRST 1;
SELECT dbo.ISOweek(CONVERT(DATETIME,'12/26/2004',101)) AS 'ISO Week';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ISO Week
----------------
52
```

### <a name="b-creating-an-inline-table-valued-function"></a>Б. Создание встроенной функции с табличным значением

Результатом следующего примера является встроенная функция, возвращающая табличное значение в базу данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Для каждого из товаров, проданных в магазине, она возвращает три столбца: `ProductID`, `Name` и статистику с начала года по магазину — `YTD Total`.

```sql
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)
RETURNS TABLE
AS
RETURN
(
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'
    FROM Production.Product AS P
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID
    WHERE C.StoreID = @storeid
    GROUP BY P.ProductID, P.Name
);
GO
```

При вызове этой функции выполняется следующий запрос.

```sql
SELECT * FROM Sales.ufn_SalesByStore (602);
```

### <a name="c-creating-a-multi-statement-table-valued-function"></a>В. Создание функции с табличным значением из нескольких инструкций

В следующем примере создается функция с табличным значением `fn_FindReports(InEmpID)` в базе данных AdventureWorks2012. Если ей передать допустимый идентификатор сотрудника, она вернет таблицу, в которой содержатся все сотрудники, которые прямо или опосредованно перед ним отчитываются. В функции для построения иерархического списка сотрудников используется рекурсивное обобщенное табличное выражение (CTE). Дополнительные сведения о рекурсивных обобщенных табличных выражениях см. в разделе [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md).

```sql
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)
RETURNS @retFindReports TABLE
(
    EmployeeID int primary key NOT NULL,
    FirstName nvarchar(255) NOT NULL,
    LastName nvarchar(255) NOT NULL,
    JobTitle nvarchar(50) NOT NULL,
    RecursionLevel int NOT NULL
)
--Returns a result set that lists all the employees who report to the
--specific employee directly or indirectly.*/
AS
BEGIN
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns
    AS (
        -- Get the initial list of Employees for Manager n
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0
        FROM HumanResources.Employee e
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        WHERE e.BusinessEntityID = @InEmpID
        UNION ALL
        -- Join recursive member to anchor
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1
        FROM HumanResources.Employee e
            INNER JOIN EMP_cte
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        )
-- copy the required columns to the result of the function
    INSERT @retFindReports
    SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
    FROM EMP_cte
    RETURN
END;
GO
-- Example invocation
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
FROM dbo.ufn_FindReports(1);

GO
```

### <a name="d-creating-a-clr-function"></a>Г. Создание функции CLR

В следующем примере создается функция CLR `len_s`. Перед ее созданием сборка `SurrogateStringFunction.dll` регистрируется в локальной базе данных.

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с пакетом обновления 1 (SP1) и выше)

```sql
DECLARE @SamplesPath nvarchar(1024);
-- You may have to modify the value of this variable if you have
-- installed the sample in a location other than the default location.
SELECT @SamplesPath = REPLACE
    (  physical_name
     , 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf'
     , 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\'
    )
  FROM master.sys.database_files
  WHERE name = 'master';

CREATE ASSEMBLY [SurrogateStringFunction]
FROM @SamplesPath + 'StringManipulate\CS\StringManipulate\bin\debug\SurrogateStringFunction.dll'
WITH PERMISSION_SET = EXTERNAL_ACCESS;
GO

CREATE FUNCTION [dbo].[len_s] (@str nvarchar(4000))
RETURNS bigint
AS EXTERNAL NAME [SurrogateStringFunction].[Microsoft.Samples.SqlServer.SurrogateStringFunction].[LenS];
GO
```

Пример создания функции CLR с табличным значением см. в разделе [Функции среды CLR с табличным значением](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-table-valued-functions.md).

### <a name="e-displaying-the-definition-of-user-defined-functions"></a>Д. Отображение определения определяемых пользователем функций

```sql
SELECT definition, type
FROM sys.sql_modules AS m
JOIN sys.objects AS o ON m.object_id = o.object_id
    AND type IN ('FN', 'IF', 'TF');
GO
```

Определения зашифрованных функций, созданных с помощью параметра `ENCRYPTION`, в sys.sql_modules не отображаются, но все остальные сведения о них доступны.

## <a name="see-also"></a>См. также:

- [Создание определяемых пользователем функций (компонент Database Engine)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)
- [ALTER FUNCTION (Transact-SQL)](../../t-sql/statements/alter-function-transact-sql.md)
- [DROP FUNCTION (Transact-SQL)](../../t-sql/statements/drop-function-transact-sql.md)
- [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md)
- [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)
- [Определяемые пользователем функции среды CLR](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)