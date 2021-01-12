---
description: ALTER FUNCTION (Transact-SQL)
title: ALTER FUNCTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_FUNCTION_TSQL
- ALTER FUNCTION
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER FUNCTION statement
- modifying functions
- functions [SQL Server], modifying
ms.assetid: 89f066ee-05ac-4439-ab04-d8c3d5911179
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1785a65f08952660bd3a8e3cad72355d8db5b373
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081994"
---
# <a name="alter-function-transact-sql"></a>ALTER FUNCTION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  Изменяет существующую функцию [!INCLUDE[tsql](../../includes/tsql-md.md)] или CLR, которая была предварительно создана при помощи инструкции CREATE FUNCTION, без изменения разрешений и без влияния на какие-либо зависимые функции, хранимые процедуры или триггеры.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax    
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type   
    [ = default ] }   
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
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
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
-- Transact-SQL Multistatement Table-valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
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
      ( column_name [ ASC | DESC ] [ ,...n ] )  
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
-- CLR Scalar and Table-Valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
)  
RETURNS { return_data_type | TABLE <clr_table_type_definition> }  
    [ WITH <clr_function_option> [ ,...n ] ]  
    [ AS ] EXTERNAL NAME <method_specifier>  
[ ; ]  
```
  
```syntaxsql
-- CLR Function Clauses
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
-- Syntax for In-Memory OLTP: Natively compiled, scalar user-defined function  
ALTER FUNCTION [ schema_name. ] function_name    
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type   
    [ NULL | NOT NULL ] [ = default ] }   
    [ ,...n ]   
  ]   
)   
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]   
    [ AS ]   
    BEGIN ATOMIC WITH (set_option [ ,... n ])  
        function_body   
        RETURN scalar_expression  
    END  
    
<function_option>::=   
{ |  NATIVE_COMPILATION   
  |  SCHEMABINDING   
  | [ EXECUTE_AS_Clause ]   
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]   
}  
```   
  
## <a name="arguments"></a>Аргументы  
 *schema_name*  
 Имя схемы, к которой принадлежит определяемая пользователем функция.  
  
 *function_name*  
 Определенная пользователем функция, которая будет изменяться.  
  
> [!NOTE]  
>  Даже при отсутствии аргументов скобки после имени функции обязательны.  
  
 **@** _parameter_name_  
 Аргумент пользовательской функции. Может быть объявлен один или несколько аргументов.  
  
 Для функций допускается не более 2 100 параметров. При выполнении функции значение каждого из объявленных параметров должно быть указано пользователем, если для них не определены значения по умолчанию.  
  
 Определяет имя параметра, используя знак **@** как первый символ. Имя параметра должно соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Параметры являются локальными в пределах функции, в разных функциях могут быть использованы одинаковые имена параметров. Аргументы могут использоваться только вместо констант. Они не могут использоваться вместо имен таблиц, имен столбцов или имен других объектов базы данных.  
  
> [!NOTE]  
>  Значение ANSI_WARNINGS игнорируется при передаче аргументов хранимой процедуре или пользовательской функции, а также при объявлении и настройке переменных в инструкции пакетных заданий. Например, если объявить переменную как **char(3)** , а затем присвоить ей значение длиннее трех символов, данные будут усечены до размера переменной, а инструкция INSERT или UPDATE завершится без ошибок.  
  
 [ *type_schema_name.* ] *parameter_data_type*  
 Тип данных аргумента, а также схема, к которой он принадлежит. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типа данных **timestamp**. Для функций CLR допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типов данных **text**, **ntext**, **image** и **timestamp**. Нескалярные типы **cursor** и **table** не могут быть указаны в качестве типов данных параметров ни для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], ни для функций CLR.  
  
 Если *type_schema_name* не указан, [!INCLUDE[ssDEversion2005](../../includes/ssdeversion2005-md.md)] выполняет поиск *parameter_data_type* в следующем порядке:  
  
-   Схема, содержащая имена системных типов данных SQL Server.  
  
-   в установленной по умолчанию для текущего пользователя схеме в текущей базе данных;  
  
-   Схема **dbo** в текущей базе данных.  
  
 [ **=** _default_ ]  
 Значение по умолчанию для аргумента. Если определено значение *default*, то функция выполняется даже в том случае, если для данного параметра значение не указано.  
  
> [!NOTE]  
>  Для функций CLR также могут указываться значения параметров по умолчанию, кроме типов данных **varchar(max)** и **varbinary(max)** .  
  
 Если аргумент функции имеет определенное по умолчанию значение, то ключевое слово DEFAULT должно быть задано при вызове функции для получения значения по умолчанию. Применение ключевого слова DEFAULT следует отличать от использования аргументов со значениями по умолчанию в хранимых процедурах, когда не указанный аргумент неявно принимает значение по умолчанию.  
  
 *return_data_type*  
 Возвращаемое значение скалярной пользовательской функции. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типа данных **timestamp**. Для функций CLR допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типов данных **text**, **ntext**, **image** и **timestamp**. Нескалярные типы **cursor** и **table** не могут быть указаны в качестве возвращаемых типов данных ни для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], ни для функций CLR.  
  
 *function_body*  
 Указывает серию инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], совместная работа которых не вызывает побочных эффектов, например изменения содержимого таблиц, и формирует значение функции. *function_body* используется только в скалярных функциях и функциях с табличным значением из нескольких инструкций.  
  
 Для скалярных функций *function_body* представляет собой ряд инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], которые в совокупности вычисляют скалярное выражение.  
  
 Для функций с табличным значением из нескольких инструкций *function_body* является рядом инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], заполняющих возвращаемую переменную TABLE.  
  
 *scalar_expression*  
 Указывает на то, что скалярная функция возвращает скалярное значение.  
  
 TABLE  
 Указывает, что возвращаемым значением функции с табличным значением, является таблица. Функциям с табличным значением могут передаваться только константы и **@** _local\_variables_.  
  
 Во встроенных функциях с табличным значением возвращаемое значение TABLE определяется при использовании единственной инструкции SELECT. Встроенные функции не имеют соответствующих возвращаемых переменных.  
  
 В функциях с табличным значением из нескольких инструкций переменной **@** _return\_variable_ является переменная TABLE, используемая для сохранения данных и накопления строк, которые будут возвращены в качестве значения функции. Аргумент **@** _return\_variable_ может быть указан только для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], но не для функций CLR.  
  
 *select-stmt*  
 Единственная инструкция SELECT, которая определяет возвращаемое значение встроенной функции, возвращающей табличное значение.  
  
 EXTERNAL NAME \<method_specifier>*assembly_name.class_name*.*method_name*  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает метод сборки, привязываемый к функции. Имя *assembly_name* должно соответствовать существующей сборке в текущей базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], для которой включена видимость. Аргумент *class_name* должен быть допустимым идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и существовать как класс в сборке. Если класс имеет квалифицированное имя пространства имен, которое использует точку ( **.** ) для разделения частей пространства имен, имя класса разделено скобками ( **[]** ) или кавычками ( **""** ). Имя *method_name* должно быть допустимым идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и существовать как статистический метод в указанном классе.  
  
> [!NOTE]  
>  По умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не производит выполнение кода CLR. Можно создавать, изменять и удалять объекты базы данных со ссылками на модули среды CLR, но [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выполняет их до тех пор, пока не будет включен параметр [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Для включения параметра используйте хранимую процедуру [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 _\<_table\_type\_definition_\>_ **(** { \<column_definition\> \<column\_constraint\> | \<computed\_column\_definition\> } [ \<table\_constraint\> ] [ **,** ...*n* ] **)**  
 Определяет тип данных таблицы для функции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Объявление таблицы включает определения столбцов, а также ограничений для столбцов и таблиц.  
  
\< clr_table_type_definition \> **(** { *column_name**data_type* } [ **,** ...*n* ] **)** **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([предварительная версия в некоторых регионах](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)).  
  
 Определяет табличные типы данных для функции CLR. Объявление таблицы включает только имена столбцов и типы данных.  
  
 NULL|NOT NULL  
 Поддерживается только для скомпилированных в собственном коде скалярных определяемых пользователем функций. Дополнительные сведения см. в разделе [Скалярные определяемые пользователем функции для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 NATIVE_COMPILATION  
 Указывает, скомпилирована ли в собственном коде определяемая пользователем функция. Этот аргумент требуется только для скомпилированных в собственном коде скалярных определяемых пользователем функций.  
  
 Аргумент NATIVE_COMPILATION является обязательным при применении ALTER к функции и может использоваться, только если функция была создана с помощью аргумента NATIVE_COMPILATION.  
  
 BEGIN ATOMIC WITH  
 Поддерживается только для скомпилированных в собственном коде скалярных определяемых пользователем функций и является обязательным. Дополнительные сведения см. в статье [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  
  
 SCHEMABINDING  
 Аргумент SCHEMABINDING требуется для скомпилированных в собственном коде скалярных определяемых пользователем функций.  
  
 **\<function_option>::= и \<clr_function_option>::=**  
  
 Указывает, что функция будет иметь один или несколько следующих аргументов:  
  
 ENCRYPTION  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает на то, что компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] шифрует столбцы представления каталога, содержащие текст инструкции ALTER FUNCTION. Использование параметра ENCRYPTION препятствует публикации данной функции при помощи репликации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Параметр ENCRYPTION для функций CLR указывать нельзя.  
  
 SCHEMABINDING  
 Указывает, что функция привязана к объектам базы данных, которые содержат ссылки на нее. Если аргумент SCHEMABINDING указан, нельзя изменить базовые объекты таким способом, который может повлиять на определение функции. Сначала нужно изменить или удалить само определение функции, чтобы удалить зависимости от объекта, который требуется изменить.  
  
 Привязка функции к ссылающимся на нее объектам удаляется в следующих случаях:  
  
-   При удалении функции.  
  
-   При изменении функции инструкцией ALTER, если не указан параметр SCHEMABINDING.  
  
Список условий, которые необходимо учитывать перед привязкой функции к схеме, см. в разделе [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md).  
  
 RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT  
 Указывает атрибут **OnNULLCall** скалярной функции. Если данный аргумент не указан, по умолчанию предполагается CALLED ON NULL INPUT. Это означает, что текст функции выполняется даже в том случае, если в качестве аргумента передано значение NULL.  
  
 Если атрибут RETURNS NULL ON NULL INPUT указан для функции CLR, это означает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может вернуть NULL, не вызывая при этом тело функции в том случае, если в качестве какого-либо из аргументов указано значение NULL. Если определенный в \<method_specifier> метод уже обладает пользовательским атрибутом, указывающим на RETURNS NULL ON NULL INPUT, то инструкция ALTER FUNCTION указывает на CALLED ON NULL INPUT, инструкция ALTER FUNCTION обладает правом старшинства. Атрибут **OnNULLCall** не может быть указан для функций CLR с табличным значением.  
  
 Предложение EXECUTE AS  
 Указывает контекст безопасности, в котором выполняется определяемая пользователем функция. Таким образом, можно отслеживать то, какая пользовательская учетная запись [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используется для проверки разрешений функции на осуществление ссылки на любые объекты базы данных.  
  
> [!NOTE]  
>  Предложение EXECUTE AS не может быть указано для встроенных пользовательских функций.  
  
 Дополнительные сведения см. в разделе [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
**\< column_definition >::=**
  
 Определяет тип данных таблицы. Декларация таблицы включает определения столбцов и ограничений. Для функций CLR можно указать только *column_name* и *data_type*.  
  
 *column_name*  
 Имя столбца в таблице. Имена столбцов должны соответствовать правилам для идентификаторов и быть уникальными в рамках таблицы. *column_name* может иметь длину от 1 до 128 символов.  
  
 *data_type*  
 Указывает тип данных столбца. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типа данных **timestamp**. Для функций CLR допустимы любые типы данных, включая определяемые пользователем типы данных CLR, кроме типов данных **text**, **ntext**, **image**, **char**, **varchar**, **varchar(max)** и **timestamp**. Нескалярный тип данных **cursor** не может указываться в качестве типа данных столбца ни для функций [!INCLUDE[tsql](../../includes/tsql-md.md)], ни для функций CLR.  
  
 DEFAULT *constant_expression*  
 Указывает значение, присваиваемое столбцу в случае отсутствия явно заданного значения при вставке. Выражение *constant_expression* является константой, значением NULL или значением системной функции. Определения DEFAULT могут применяться к любым столбцам, кроме тех, которые имеют свойство IDENTITY. Предложение DEFAULT не может быть указано для функций CLR с табличным значением.  
  
 COLLATE *collation_name*  
 Задает параметры сортировки для столбца. Если не указано, столбцу назначаются параметры сортировки, принятые в базе данных по умолчанию. Именем параметров сортировки может быть либо имя параметров сортировки Windows, либо имя параметров сортировки SQL. Список и дополнительные сведения см. в разделах [Имя параметра сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) и [Имя параметра сортировки SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md).  
  
 Предложение COLLATE может быть использовано для изменения параметров сортировки только для столбцов с типом данных **char**, **varchar**, **nchar** или **nvarchar**.  
  
 Предложение COLLATE не может быть указано для функций CLR с табличным значением.  
  
 ROWGUIDCOL  
 Указывает, что новый столбец является столбцом глобального уникального идентификатора строки. Только один столбец типа **uniqueidentifier** в таблице может быть назначен в качестве столбца ROWGUIDCOL. Свойство ROWGUIDCOL может быть присвоено только столбцу типа **uniqueidentifier**.  
  
 Свойство ROWGUIDCOL не обеспечивает уникальности значений, хранимых в столбце. Кроме того, данное свойство не производит автоматическое формирование значений для новых строк, вставляемых в таблицу. Для создания уникальных значений произвольного столбца используйте функцию NEWID в инструкции INSERT. Может быть указано значение по умолчанию. При этом функция NEWID не может быть указана в качестве значения по умолчанию.  
  
 IDENTITY  
 Указывает, что новый столбец является столбцом идентификаторов. При добавлении в таблицу новой строки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] формирует для столбца уникальное, последовательное значение. Столбцы идентификаторов обычно используются с ограничением PRIMARY KEY для поддержания уникальности идентификаторов строк в таблице. Свойство IDENTITY может назначаться для столбцов типа **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** или **numeric(p,0)** . Для каждой таблицы можно создать только один столбец идентификаторов. Ограниченные значения по умолчанию и ограничения DEFAULT не могут использоваться в столбце идентификаторов. Необходимо указывать либо оба аргумента (и *seed*, и *increment*), либо не указывать ни одного из них. Если ничего не указано, применяется значение по умолчанию (1,1).  
  
 Предложение IDENTITY не может быть указано для функций CLR с табличным значением.  
  
 *seed*  
 Целочисленное значение, присваиваемое первой строке таблицы.  
  
 *increment*  
 Целочисленное значение, добавляемое к значению *seed* для каждой последующей строки таблицы.  
  
**\< column_constraint >::= и \< table_constraint>::=**
  
 Определяет ограничение для указанного столбца или таблицы. Для функций CLR единственное допустимое ограничение — NULL. Именованные ограничения недопустимы.  
  
 NULL | NOT NULL  
 Определяет, допустимы ли для столбца значения NULL. Параметр NULL не является ограничением в строгом смысле слова, но может быть указан так же, как и NOT NULL. Ограничение NOT NULL не может быть указано для функций CLR с табличным значением.  
  
 PRIMARY KEY  
 Ограничение, обеспечивающее целостность сущностей для указанного столбца через уникальный индекс. В возвращающих табличное значение пользовательских функциях ограничение PRIMARY KEY может быть создано только для одного столбца таблицы. Ограничение PRIMARY KEY не может быть указано для функций CLR с табличным значением.  
  
 UNIQUE  
 Ограничение, которое обеспечивает целостность сущностей для указанного столбца или столбцов с помощью уникального индекса. В таблице может быть несколько ограничений UNIQUE. Ограничение UNIQUE не может быть указано для функций CLR с табличным значением.  
  
 CLUSTERED | NONCLUSTERED  
 Указывает, что для ограничения PRIMARY KEY или UNIQUE создается кластеризованный или некластеризованный индекс. Ограничения PRIMARY KEY используют параметр CLUSTERED, а ограничения UNIQUE используют параметр NONCLUSTERED.  
  
 Параметр CLUSTERED может быть указан только для одного ограничения. Если параметр CLUSTERED указан для ограничения UNIQUE и указано ограничение PRIMARY KEY, то PRIMARY KEY использует NONCLUSTERED.  
  
 Параметры СLUSTERED и NONСLUSTERED не могут быть указаны для функций CLR с табличным значением.  
  
 CHECK  
 Ограничение, обеспечивающее целостность домена путем ограничения возможных значений, которые могут быть введены в столбец или столбцы. Ограничения CHECK не могут быть указаны для функций CLR с табличным значением.  
  
 *logical_expression*  
 Логическое выражение, возвращающее значения TRUE или FALSE.  
  
 **\<computed_column_definition>::=**  
  
 Указывает вычисляемый столбец. Дополнительные сведения о вычисляемых столбцах см. в разделе [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
 *column_name*  
 Имя вычисляемого столбца.  
  
 *computed_column_expression*  
 Выражение, определяющее значение вычисляемого столбца.  
  
 **\<index_option>::=**  
  
 Указывает параметры индекса для индекса PRIMARY KEY или UNIQUE. Дополнительные сведения о параметрах индекса см. в разделе [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
 PAD_INDEX = { ON | OFF }  
 Определяет разреженность индекса. Значение по умолчанию — OFF.  
  
 FILLFACTOR = *fillfactor*  
 Определяет величину в процентах, указывающую, насколько компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] должен заполнять конечный уровень каждой страницы индекса во время создания или изменения индекса. Значение *fillfactor* должно быть целым числом от 1 до 100. Значение по умолчанию равно 0.  
  
 IGNORE_DUP_KEY = { ON | OFF }  
 Определяет ответ на ошибку, случающуюся, когда операция вставки пытается вставить в уникальный индекс повторяющиеся значения ключа. Параметр IGNORE_DUP_KEY применяется только к операциям вставки, производимым после создания или перестроения индекса. Значение по умолчанию — OFF.  
  
 STATISTICS_NORECOMPUTE = { ON | OFF }  
 Указывает, выполнялся ли перерасчет статистики распределения. Значение по умолчанию — OFF.  
  
 ALLOW_ROW_LOCKS = { ON | OFF }  
 Указывает, разрешена ли блокировка строк. Значение по умолчанию — ON.  
  
 ALLOW_PAGE_LOCKS = { ON | OFF }  
 Указывает, разрешена ли блокировка страниц. Значение по умолчанию — ON.  
  
## <a name="remarks"></a>Remarks  
 ALTER FUNCTION не может использоваться для преобразования скалярной функции в функцию с табличным значением или наоборот. Также ALTER FUNCTION не может использоваться для преобразования одной встроенной функции в функцию из нескольких инструкций или наоборот. ALTER FUNCTION не может использоваться для преобразования функции [!INCLUDE[tsql](../../includes/tsql-md.md)] в функцию среды CLR или наоборот.  
  
 Следующие инструкции компонента Service Broker не могут быть включены в определение [!INCLUDE[tsql](../../includes/tsql-md.md)] пользовательской функции:  
  
-   BEGIN DIALOG CONVERSATION  
-   END CONVERSATION  
-   GET CONVERSATION GROUP  
-   MOVE CONVERSATION  
-   RECEIVE  
-   SEND  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения ALTER для функции или для схемы. Если в функции указан определяемый пользователем тип, требуется разрешение EXECUTE на этот тип.  
  
## <a name="see-also"></a>См. также:  
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [DROP FUNCTION (Transact-SQL)](../../t-sql/statements/drop-function-transact-sql.md)   
 [Внесение изменений в схемы баз данных публикации](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
