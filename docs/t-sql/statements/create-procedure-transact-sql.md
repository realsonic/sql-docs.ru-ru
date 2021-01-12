---
description: CREATE PROCEDURE (Transact-SQL)
title: CREATE PROCEDURE (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- PROC
- PROCEDURE
- CREATE PROCEDURE
- CREATE_PROC_TSQL
- PROCEDURE_TSQL
- CREATE PROC
- PROC_TSQL
- CREATE_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- parameters [SQL Server], stored procedures
- table-valued parameters
- SET statement, stored procedures
- stored procedures [SQL Server], creating
- wildcard parameters [SQL Server]
- maximum size of stored procedures
- WITH RECOMPILE clause
- common language runtime [SQL Server], stored procedures
- CREATE PROCEDURE statement
- local temporary procedures [SQL Server]
- WITH ENCRYPTION clause
- output parameters [SQL Server]
- nesting stored procedures
- user-defined stored procedures [SQL Server]
- system stored procedures [SQL Server], creating
- deferred name resolution, stored procedures
- referenced tables [SQL Server]
- global temporary procedures [SQL Server]
- cursor data type
- temporary stored procedures [SQL Server]
- size [SQL Server], stored procedures
- automatic stored procedure execution
- creating stored procedures
ms.assetid: afe3d86d-c9ab-44e4-b74d-4e3dbd9cc58c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8ebd5ecac550537f13d20e25a19222477ccebb6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092126"
---
# <a name="create-procedure-transact-sql"></a>CREATE PROCEDURE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Создает хранимую процедуру [!INCLUDE[tsql](../../includes/tsql-md.md)] или хранимую процедуру общеязыковой среды выполнения (CLR) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Хранимые процедуры похожи на процедуры из других языков программирования в том, что они могут:

- принимать входные параметры и возвращать вызывающей процедуре или пакету ряд значений в виде выходных параметров;
- содержать программные инструкции, которые выполняют операции в базе данных, в том числе вызывающие другие процедуры;
- возвращать значение состояния вызывающей процедуре или пакету, таким образом передавая сведения об успешном или неуспешном завершении (и причины последнего).

Используйте эту инструкцию для создания постоянной процедуры в текущей базе данных или временной процедуры в базе данных **tempdb**.

> [!NOTE]
> В этом разделе рассматривается интеграция среды CLR .NET Framework с SQL Server. Интеграция со средой CLR не применяется к [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Azure.

Перейдите к разделу [Простые примеры](#Simple), чтобы пропустить сведения о синтаксисе и просмотреть краткий пример базовой хранимой процедуры.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Transact-SQL Syntax for Stored Procedures in SQL Server and Azure SQL Database

CREATE [ OR ALTER ] { PROC | PROCEDURE }
    [schema_name.] procedure_name [ ; number ]
    [ { @parameter [ type_schema_name. ] data_type }
        [ VARYING ] [ = default ] [ OUT | OUTPUT | [READONLY]
    ] [ ,...n ]
[ WITH <procedure_option> [ ,...n ] ]
[ FOR REPLICATION ]
AS { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }
[;]

<procedure_option> ::=
    [ ENCRYPTION ]
    [ RECOMPILE ]
    [ EXECUTE AS Clause ]
```

```syntaxsql
-- Transact-SQL Syntax for CLR Stored Procedures

CREATE [ OR ALTER ] { PROC | PROCEDURE }
    [schema_name.] procedure_name [ ; number ]
    [ { @parameter [ type_schema_name. ] data_type }
        [ = default ] [ OUT | OUTPUT ] [READONLY]
    ] [ ,...n ]
[ WITH EXECUTE AS Clause ]
AS { EXTERNAL NAME assembly_name.class_name.method_name }
[;]
```

```syntaxsql
-- Transact-SQL Syntax for Natively Compiled Stored Procedures

CREATE [ OR ALTER ] { PROC | PROCEDURE } [schema_name.] procedure_name
    [ { @parameter data_type } [ NULL | NOT NULL ] [ = default ]
        [ OUT | OUTPUT ] [READONLY]
    ] [ ,... n ]
  WITH NATIVE_COMPILATION, SCHEMABINDING [ , EXECUTE AS clause ]
AS
{
  BEGIN ATOMIC WITH (set_option [ ,... n ] )
sql_statement [;] [ ... n ]
 [ END ]
}
 [;]

<set_option> ::=
    LANGUAGE = [ N ] 'language'
  | TRANSACTION ISOLATION LEVEL = { SNAPSHOT | REPEATABLE READ | SERIALIZABLE }
  | [ DATEFIRST = number ]
  | [ DATEFORMAT = format ]
  | [ DELAYED_DURABILITY = { OFF | ON } ]
```

```syntaxsql
-- Transact-SQL Syntax for Stored Procedures in Azure Synapse Analytics
-- and Parallel Data Warehouse

-- Create a stored procedure
CREATE { PROC | PROCEDURE } [ schema_name.] procedure_name
    [ { @parameterdata_type } [ OUT | OUTPUT ] ] [ ,...n ]
AS { [ BEGIN ] sql_statement [;][ ,...n ] [ END ] }
[;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

OR ALTER

**Область применения**: Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 1 (SP1)).

Изменяет процедуру, если она уже существует.

*schema_name* — имя схемы, к которой относится процедура. Процедуры привязаны к схеме. Если имя схемы не указано при создании процедуры, то автоматически назначается схема по умолчанию для пользователя, который создает процедуру.

*procedure_name* — имя процедуры. Имена процедур должны соответствовать требованиям, предъявляемым к [идентификаторам](../../relational-databases/databases/database-identifiers.md), и должны быть уникальными в схеме.

При задании имен для процедур не следует пользоваться префиксом **sp_** . Этим префиксом в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обозначаются системные процедуры. Использование этого префикса может нарушить работу кода приложения, если обнаружится системная процедура с таким же именем.

Локальную или глобальную временную процедуру можно создать, указав один символ номера (#) перед *procedure_name* ( *#procedure_name*) для локальных временных процедур и два символа номера для глобальных временных процедур ( *##procedure_name*). Локальная временная процедура видима только соединению, которое создало процедуру, и удаляется, когда это соединение закрывается. Глобальная временная процедура доступна для всех соединений и удаляется при завершении последнего сеанса, в котором она использовалась. Для процедур CLR нельзя задавать временные имена.

Полное имя процедуры или глобальной временной процедуры не может содержать более 128 символов (с учетом символов ##). Полное имя локальной временной процедуры с учетом символа # не может содержать более 116 символов.

**;** *число*

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Необязательный целочисленный аргумент, используемый для группирования одноименных процедур. Все сгруппированные процедуры можно удалить, выполнив одну инструкцию DROP PROCEDURE.

> [!NOTE]
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]

В нумерованных процедурах нельзя использовать определяемые пользователем типы данных **xml** и CLR, и их нельзя использовать в структуре плана.

**@** *parameter* — параметр, объявленный в процедуре. Укажите имя параметра, начинающееся со знака **@** . Имя параметра должно соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Параметры являются локальными в пределах процедуры; в разных процедурах могут быть использованы одинаковые имена параметров.

Можно объявить от 1 до 2100 параметров. При выполнении процедуры значение каждого из объявленных параметров должно быть указано пользователем, если для параметра не определено значение по умолчанию или значение не задано равным другому параметру. Если процедура содержит [возвращающие табличное значение параметры](../../relational-databases/tables/use-table-valued-parameters-database-engine.md), а в вызове отсутствует параметр, передается пустая таблица. Параметры могут использоваться только в качестве выражений-констант; они не могут использоваться вместо имен таблиц, столбцов или других объектов базы данных. Дополнительные сведения см. в разделе [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md).

Параметры не могут быть объявлены, если указан параметр FOR REPLICATION.

[ _type\_schema\_name_ **.** ] *data_type* — тип данных параметра и схема, к которой относится этот тип.

**Рекомендации по процедурам [!INCLUDE[tsql](../../includes/tsql-md.md)]**

- Все типы данных [!INCLUDE[tsql](../../includes/tsql-md.md)] можно использовать в качестве параметров.
- Для создания возвращающих табличное значение параметров можно использовать определяемый пользователем табличный тип. Возвращающие табличное значение параметры могут быть только ВХОДНЫМИ и должны сопровождаться ключевым словом READONLY. Дополнительные сведения см. в статье [Использование возвращающих табличные значения параметров (ядро СУБД)](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).
- Типы данных **cursor** могут быть только ВЫХОДНЫМИ параметрами и должны сопровождаться ключевым словом VARYING.

**Рекомендации по процедурам CLR**

- Все собственные типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], имеющие эквиваленты в управляемом коде, можно использовать в качестве параметров. Дополнительные сведения о соответствии между типами среды CLR и системными типами данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Сопоставление данных параметров CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md). Дополнительные сведения о системных типах данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и их синтаксисе см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).

- Возвращающие табличное значение типы данных и типы данных **cursor** не могут служить параметрами.
- Если тип параметра является определяемым пользователем типом данных CLR, то необходимо иметь связанное с этим типом разрешение EXECUTE.

VARYING — указывает результирующий набор, поддерживаемый в качестве выходного параметра. Этот параметр динамически формируется процедурой, и его содержимое может различаться. Область применения — только параметры **cursor**. Этот параметр недопустим для процедур CLR.

*default* — значение параметра по умолчанию. Если для некоторого параметра определено значение по умолчанию, то процедуру можно выполнить без указания значения этого параметра. Значение по умолчанию должно быть константой или может быть равно NULL. Значение константы может иметь вид шаблона, что позволяет использовать ключевое слово LIKE при передаче параметра в процедуру.

Значения по умолчанию записываются в столбец **sys.parameters.default** только для процедур CLR. В случае параметров процедуры [!INCLUDE[tsql](../../includes/tsql-md.md)] этот столбец содержит значения NULL.

OUT | OUTPUT — показывает, что параметр является выходным. Используйте выходные параметры для возврата значений коду, вызвавшему процедуру. Параметры **text**, **ntext** и **image** не могут быть выходными, если процедура не является процедурой CLR. Выходным параметром с ключевым словом OUTPUT может быть заполнитель курсора, если процедура не является процедурой CLR. Возвращающий табличное значение тип данных не может быть указан в качестве выходного параметра процедуры.

READONLY — указывает, что параметр не может быть обновлен или изменен в тексте процедуры. Если тип параметра является возвращающим табличное значение типом, то должно быть указано ключевое слово READONLY.

RECOMPILE — указывает, что [!INCLUDE[ssDE](../../includes/ssde-md.md)] не кэширует план запроса для этой процедуры, что вызывает ее компиляцию при каждом выполнении. Дополнительные сведения о причинах принудительной повторной компиляции см. в разделе [Перекомпиляция хранимой процедуры](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md). Этот параметр нельзя использовать, если указано предложение FOR REPLICATION, а также для процедур CLR.

Чтобы [!INCLUDE[ssDE](../../includes/ssde-md.md)] удалил планы отдельных запросов в процедуре, следует использовать указание запроса RECOMPILE в определении запроса. Дополнительные сведения см. в разделе [Указания запросов (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).

ENCRYPTION

**Область применения**: SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Показывает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет запутывание исходного текста инструкции CREATE PROCEDURE. Результат запутывания не виден непосредственно ни в одном представлении каталога [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Пользователи, не имеющие доступа к системным таблицам или файлам баз данных, не смогут получить запутанный текст, однако этот текст доступен привилегированным пользователям, которые либо смогут обращаться к системным таблицам через [порт DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md), либо будут иметь непосредственный доступ к файлам баз данных. Кроме того, пользователи, имеющие право на подключение отладчика к серверному процессу, могут получить расшифрованный текст процедуры из памяти во время выполнения. Дополнительные сведения о доступе к метаданным системы см. в статье [Настройка видимости метаданных](../../relational-databases/security/metadata-visibility-configuration.md).

Этот параметр недопустим для процедур CLR.

Процедуры, созданные с этим аргументом, не могут быть опубликованы как часть репликации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

EXECUTE AS *clause* — определяет контекст безопасности, в котором должна быть выполнена процедура.

Для скомпилированных в собственном коде хранимых процедур, начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], отсутствуют ограничения на предложение EXECUTE AS. В [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] предложения SELF, OWNER и *user_name* поддерживаются с помощью хранимых процедур, скомпилированных в собственном коде.

Дополнительные сведения см. в разделе [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md).

FOR REPLICATION

**Область применения**: SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает, что процедура создается для репликации. Следовательно, ее нельзя выполнять на подписчике. Процедура, созданная с параметром FOR REPLICATION, используется в качестве фильтра и выполняется только в процессе репликации. Параметры не могут быть объявлены, если указан параметр FOR REPLICATION. Параметр FOR REPLICATION нельзя указывать для процедур CLR. Параметр RECOMPILE не учитывается для процедур, созданных с параметром FOR REPLICATION.

Процедура `FOR REPLICATION` имеет тип объекта **RF** в представлениях **sys.objects** и **sys.procedures**.

{ [ BEGIN ] *sql_statement* [;] [ ...*n* ] [ END ] } — одна или несколько инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], составляющих текст процедуры. Инструкции можно заключить в необязательные ключевые слова BEGIN и END. Дополнительные сведения см. далее в разделах "Рекомендации", "Общие замечания" и "Ограничения".

EXTERNAL NAME _assembly\_name_ **.** _class\_name_ **.** _method\_name_

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

Указывает метод сборки [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] для процедуры CLR, на которую создается ссылка. Аргумент *class_name* должен быть допустимым идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и существовать как класс в сборке. Если класс имеет квалифицированное имя пространства имен, которое использует точку ( **.** ) для разделения частей пространства имен, имя класса разделено скобками ( **[]** ) или кавычками ( **""** ). Указанный метод класса должен быть статическим.

По умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не производит выполнение кода CLR. Можно создавать, изменять и удалять объекты базы данных со ссылками на модули среды CLR, но [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выполняет их до тех пор, пока не будет включен параметр [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Для включения параметра используйте хранимую процедуру [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

> [!NOTE]
> Процедуры CLR не поддерживаются в автономной базе данных.

ATOMIC WITH

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает атомарное выполнение хранимой процедуры. Изменения принимаются, либо все изменения откатываются с исключением. Блок ATOMIC WITH требуется для скомпилированных в собственном коде хранимых процедур.

Если процедура возвращает результат (явно с помощью инструкции RETURN либо неявно путем завершения выполнения), выполненная работа фиксируется процедурой. Если процедура выдает ошибку, выполняется откат работы, выполненной процедурой.

Параметр XACT_ABORT по умолчанию имеет значение ON внутри блока ATOMIC и не может быть изменен. Параметр XACT_ABORT указывает, выполняет ли [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматический откат текущей транзакции, если инструкция языка [!INCLUDE[tsql](../../includes/tsql-md.md)] вызывает ошибку выполнения.

 Следующие параметры SET всегда имеют значение ON внутри блока ATOMIC; параметры нельзя изменить.

- CONCAT_NULL_YIELDS_NULL
- QUOTED_IDENTIFIER, ARITHABORT
- NOCOUNT
- ANSI_NULLS
- ANSI_WARNINGS

Параметры SET нельзя изменить внутри блоков ATOMIC. Параметры SET пользовательского сеанса не используются в области скомпилированных в собственном коде хранимых процедур. Эти параметры фиксируются во время компиляции.

Операции BEGIN, ROLLBACK и COMMIT нельзя использовать внутри блока ATOMIC.

Для каждой скомпилированной в собственном коде хранимой процедуры существует один блок ATOMIC. Блок входит во внешнюю область процедуры. Блоки не могут быть вложенными. Дополнительные сведения о скомпилированных в собственном коде хранимых процедурах см. в разделе [Хранимые процедуры, скомпилированные в собственном коде](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).

**NULL** | NOT NULL — определяет, допустимы ли для параметра значения NULL. Значение по умолчанию — NULL.

NATIVE_COMPILATION

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает, что процедура компилируется в собственном режиме. NATIVE_COMPILATION, SCHEMABINDING и EXECUTE AS можно указывать в любом порядке. Дополнительные сведения см. в статье [Хранимые процедуры, скомпилированные в собственном коде](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).

SCHEMABINDING

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Гарантирует, что таблицы, на которые ссылается процедура, нельзя удалить или изменить. SCHEMABINDING требуется для хранимых процедур, скомпилированных в собственном коде. Дополнительные сведения см. в статье [Хранимые процедуры, скомпилированные в собственном коде](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md). Ограничения SCHEMABINDING такие же, как и для определяемых пользователем функций. Дополнительные сведения см. в подразделе SCHEMABINDING раздела [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md).

LANGUAGE = [N] 'language'

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Эквивалентно параметру сеанса [SET LANGUAGE (Transact-SQL)](../../t-sql/statements/set-language-transact-sql.md). LANGUAGE = [N] 'language' (обязательный параметр).

TRANSACTION ISOLATION LEVEL

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Требуется для хранимых процедур, скомпилированных в собственном коде. Указывает уровень изоляции транзакции для хранимой процедуры. Существуют следующие варианты выбора.

Дополнительные сведения об этих параметрах см. в разделе [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).

REPEATABLE READ — указывает, что инструкции не могут считывать данные, которые были изменены другими транзакциями, но еще не были зафиксированы. Если другая транзакция изменяет данные, считанные текущей транзакцией, текущая транзакция завершится с ошибкой.

SERIALIZABLE — указывает следующее:

- Инструкции не могут считывать данные, которые были изменены другими транзакциями, но еще не были зафиксированы.
- Если другая транзакция изменяет данные, считанные текущей транзакцией, текущая транзакция завершается ошибкой.
- Если другая транзакция вставляет новые строки со значениями ключа, которые входят в диапазон ключей, считываемых любой инструкцией текущей транзакции, текущая транзакция завершается ошибкой.

SNAPSHOT — указывает, что данные, считанные любой инструкцией в транзакции, согласованы с версией данных, которые существовали в начале транзакции.

DATEFIRST = *number*

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает первый день недели в виде числа от 1 до 7. DATEFIRST — необязательный параметр. Если он не указан, то значение выводится из указанного языка.

Дополнительные сведения см. в разделе [SET DATEFIRST (Transact-SQL)](../../t-sql/statements/set-datefirst-transact-sql.md).

DATEFORMAT = *format*

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Указывает порядок частей даты (месяца, дня и года) для интерпретации символьных строк date, smalldatetime, datetime, datetime2 и datetimeoffset. DATEFORMAT — необязательный параметр. Если он не указан, то значение выводится из указанного языка.

Дополнительные сведения см. в разделе [SET DATEFORMAT (Transact-SQL)](../../t-sql/statements/set-dateformat-transact-sql.md).

DELAYED_DURABILITY = { OFF | ON }

**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Фиксации транзакции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут быть полностью устойчивыми, использовать настройки по умолчанию или быть отложенными устойчивыми.

Дополнительные сведения см. в разделе [Управление устойчивостью транзакций](../../relational-databases/logs/control-transaction-durability.md).

## <a name="simple-examples"></a><a name="Simple"></a> Простые примеры

Чтобы помочь вам приступить к работе, рассмотрим два простых примера. `SELECT DB_NAME() AS ThisDB;` возвращает имя текущей базы данных.
Можно создать оболочку этой инструкции в хранимой процедуре, например:

```sql
CREATE PROC What_DB_is_this
AS
SELECT DB_NAME() AS ThisDB;
```

Вызов хранимой процедуры с помощью инструкции: `EXEC What_DB_is_this;`

Немного более сложным вариантом является предоставление входного параметра для повышения уровня гибкости процедуры. Пример:

```sql
CREATE PROC What_DB_is_that @ID INT
AS
SELECT DB_NAME(@ID) AS ThatDB;
```

Укажите идентификационный номер базы данных при вызове процедуры. Например, `EXEC What_DB_is_that 2;` возвращает `tempdb`.

Дополнительные примеры см.в подразделе [Примеры](#Examples) в конце этого раздела.

## <a name="best-practices"></a>Рекомендации

Это неполный список рекомендаций, однако данные советы помогут повысить производительность процедур.

- Начинайте текст процедуры с инструкции SET NOCOUNT ON (она должна следовать сразу за ключевым словом AS). В этом случае отключаются сообщения, отправляемые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] клиенту после выполнения любых инструкций SELECT, INSERT, UPDATE, MERGE и DELETE. При этом для ясности создаются минимальные выходные данные. Но на доступном сегодня оборудовании не наблюдается ощутимого повышения производительности. Дополнительные сведения см. в разделе [SET NOCOUNT (Transact-SQL)](../../t-sql/statements/set-nocount-transact-sql.md).
- При создании или упоминании объектов базы данных в процедуре используйте имена схем. Отсутствие необходимости поиска в нескольких схемах экономит время обработки, затрачиваемое компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] на разрешение имен объектов. Кроме того, предотвращаются проблемы с разрешениями и доступом, вызываемые назначением схемы по умолчанию для пользователя, когда объекты создаются без указания схемы.
- Не используйте функции-оболочки для столбцов, указанных в предложениях WHERE и JOIN. В таком случае столбцы становятся недетерминированными, и обработчик запросов не может использовать индексы.
- Не используйте скалярные функции в инструкциях SELECT, возвращающих множество строк данных. Поскольку скалярная функция должна применяться к каждой строке, инструкция будет выполняться как обработка на уровне строк, что приводит к снижению производительности.
- Не используйте `SELECT *`. Вместо этого указывайте имена нужных столбцов. Это предотвращает некоторые ошибки компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], которые останавливают выполнение процедуры. Например, инструкция `SELECT *`, возвращающая данные из таблицы с 12 столбцами, а затем вставляющая эти данные во временную таблицу с 12 столбцами, выполняется успешно, пока не изменится число или порядок столбцов в любой из этих таблиц.
- Не выполняйте обработку или передачу слишком большого объема данных. Как можно раньше ограничивайте область результатов в коде процедуры, чтобы все последующие операции, выполняемые процедурой, работали с минимально возможным набором данных. Отправляйте в клиентское приложение только необходимые данные. Это более эффективно, чем передача дополнительных данных по сети, при которой клиентскому приложению приходится обрабатывать необоснованно большие результирующие наборы.
- Используйте явные транзакции, указывая ключевые слова BEGIN/COMMIT TRANSACTION, и по возможности сокращайте транзакции. Длинные транзакции увеличивают время блокировки записей и повышают шанс возникновения взаимоблокировок.
- Используйте функцию [!INCLUDE[tsql](../../includes/tsql-md.md)] TRY…CATCH для обработки ошибок в пределах процедуры. В конструкцию TRY…CATCH можно инкапсулировать весь блок инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)]. Это не только снижает расход ресурсов, но также повышает точность отчетов об ошибках и значительно сокращает труд программиста.
- Используйте ключевое слово DEFAULT для всех столбцов таблицы, на которые ссылаются инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TABLE и ALTER TABLE в тексте процедуры. Это исключает передачу значений NULL в столбцы, которые не допускают значений NULL.
- Используйте ключевые слова NULL и NOT NULL для каждого столбца во временной таблице. Если атрибуты NULL или NOT NULL не указаны в инструкции CREATE TABLE или ALTER TABLE, то способ назначения этих атрибутов столбцам компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)] определяется параметрами ANSI_DFLT_ON и ANSI_DFLT_OFF. Если в контексте соединения выполняется процедура, где значения этих параметров отличаются от значений в соединении, где была создана процедура, то столбцы таблицы, созданной для второго соединения, могут отличаться по признаку допустимости значений NULL и работать иначе. Если атрибут NULL или NOT NULL явно задан для каждого столбца, то временные таблицы создаются с одним и тем же признаком допустимости значений NULL во всех соединениях, где выполняется процедура.
- Используйте инструкции изменения, которые преобразуют значения NULL и исключают из запросов строки, содержащие значения NULL. Учтите, что в [!INCLUDE[tsql](../../includes/tsql-md.md)] значение NULL не означает пустое значение или отсутствие значения. Это заполнитель для неизвестного значения, который может вызвать непредвиденные результаты, особенно в случае запроса результирующих наборов или использования функций AGGREGATE.
- Используйте оператор UNION ALL вместо операторов UNION и OR, если нет необходимости получить уникальные значения. Для оператора UNION ALL требуется меньше затрат на обработку, поскольку из результирующего набора не исключаются повторы.

## <a name="general-remarks"></a>Общие замечания

Стандартный максимальный размер процедуры не установлен.

Переменные, указанные в процедуре, могут определяться пользователем или быть системными, такими как @@SPID.

При выполнении процедуры в первый раз она компилируется, при этом определяется оптимальный план получения данных. При последующих вызовах процедуры можно снова использовать уже созданный план, если он еще находится в кэше планов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].

Процедуры могут выполняться автоматически при запуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Они должны быть созданы системным администратором в базе данных **master** и выполняться в контексте предопределенной роли сервера **sysadmin** в фоновом процессе. Они не могут иметь ни входных, ни выходных параметров. Дополнительные сведения см. в разделе [Выполнение хранимых процедур](../../relational-databases/stored-procedures/execute-a-stored-procedure.md).

Процедуры называются вложенными, если одна процедура вызывает другую или выполняет управляемый код по ссылке на подпрограмму, тип или статистическое выражение среды CLR. Процедуры и ссылки на управляемый код могут быть вложены не более чем на 32 уровня. Уровень вложенности увеличивается на единицу, когда вызванная процедура или управляемый код начинает выполняться, и уменьшается на единицу, когда их выполнение заканчивается. Методы, вызываемые из управляемого кода, не учитываются в этом ограничении. Однако когда хранимая процедура CLR выполняет операции доступа к данным через управляемый поставщик SQL Server, при переходе из управляемого кода в SQL добавляется дополнительный уровень вложенности.

Если уровень вложенности превышает максимальное значение, вся цепочка вызовов заканчивается ошибкой. Получить уровень вложенности текущей выполняемой хранимой процедуры можно через функцию @@NESTLEVEL.

## <a name="interoperability"></a>Совместимость

При создании или изменении процедуры [!INCLUDE[ssDE](../../includes/ssde-md.md)] компонент [!INCLUDE[tsql](../../includes/tsql-md.md)] сохраняет значения SET QUOTED_IDENTIFIER и SET ANSI_NULLS. Эти первоначальные значения используются при выполнении процедуры. Таким образом, пока процедура выполняется, любые значения SET QUOTED_IDENTIFIER и SET ANSI_NULLS, задаваемые во время клиентского сеанса, не учитываются.

Другие параметры SET, такие как SET ARITHABORT, SET ANSI_WARNINGS или SET ANSI_PADDINGS, при создании или изменении процедуры не сохраняются. Если логика процедуры зависит от конкретного значения параметра, включите в начало процедуры инструкцию SET, чтобы гарантировать нужное значение. Если инструкция SET выполняется из процедуры, то значение действует только до завершения процедуры. После этого оно принимает прежнее значение, которое имело место при вызове процедуры. Это позволяет клиентам задавать нужные им параметры без влияния на логику процедуры.

Внутри процедуры может быть указана любая инструкция SET, за исключением SET SHOWPLAN_TEXT и SET SHOWPLAN_ALL. Эти инструкции могут встречаться только в пакете. Выбранный параметр SET остается в силе до завершения процедуры, после чего восстанавливает прежнее значение.

> [!NOTE]
> Значение SET ANSI_WARNINGS не учитывается при передаче параметров процедуре или определяемой пользователем функции, а также при объявлении и задании переменных в инструкции пакета. Например, если объявить переменную как **char**(3), а затем присвоить ей значение длиннее трех символов, данные будут усечены до размера переменной, а инструкция INSERT или UPDATE завершится без ошибок.

## <a name="limitations-and-restrictions"></a>Ограничения

Инструкцию CREATE PROCEDURE нельзя объединять с другими инструкциями [!INCLUDE[tsql](../../includes/tsql-md.md)] в одном пакете.

Следующие инструкции нельзя использовать нигде в тексте хранимой процедуры.

| CREATE | SET | USE |
|--------|-----|-----|
| CREATE AGGREGATE | SET SHOWPLAN_TEXT | USE *database_name*|
| CREATE DEFAULT | SET SHOWPLAN_XML
| CREATE RULE | SET PARSEONLY |
| CREATE SCHEMA | SET SHOWPLAN_ALL |
| CREATE или ALTER TRIGGER |
| CREATE или ALTER FUNCTION |
| CREATE или ALTER PROCEDURE |
| CREATE или ALTER VIEW |

 Процедура может ссылаться на таблицы, которые еще не существуют. Во время создания хранимой процедуры выполняется только проверка синтаксиса. Процедура не компилируется до первого выполнения. Ссылки на все упоминаемые в процедуре объекты разрешаются только во время компиляции. Таким образом, ничто не мешает создать синтаксически правильную процедуру, ссылающуюся на несуществующие таблицы, однако если эти таблицы отсутствуют во время выполнения хранимой процедуры, она завершится с ошибкой.

 Имя функции нельзя указать в качестве значения по умолчанию для параметра или в качестве значения, передаваемого для параметра во время выполнения процедуры. Однако функцию можно передать как переменную, как показано в следующем примере.

```sql
-- Passing the function value as a variable.
DECLARE @CheckDate DATETIME = GETDATE();
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;
GO
```

Если процедура вносит изменения на удаленном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], то откат этих изменений будет невозможен. Удаленные процедуры не участвуют в транзакциях.

Чтобы компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] правильно выбрал перегруженную в .NET Framework версию метода, в предложении EXTERNAL NAME необходимо указывать метод следующим образом.

- Он должен быть объявлен как статический метод.
- Он должен принимать то же количество параметров, что и процедура.
- Типы параметров метода должны быть совместимы с типами данных соответствующих параметров процедуры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о соответствии между типами данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] см. в разделе [Сопоставление данных параметров CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

## <a name="metadata"></a>Метаданные

В следующей таблице перечислены представления каталога и динамические административные представления, которые могут использоваться для получения сведений о хранимых процедурах.

|Представление|Описание|
|----------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Возвращает определение процедуры [!INCLUDE[tsql](../../includes/tsql-md.md)]. Текст процедуры, созданной с параметром ENCRYPTION, нельзя увидеть при помощи представления каталога **sys.sql_modules**.|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|Возвращает сведения о процедуре CLR.|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|Возвращает сведения о параметрах, которые определены в процедуре|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)|Возвращает объекты, на которые ссылается процедура.|

Для оценки размера скомпилированной процедуры следует использовать следующие счетчики системного монитора.

|Имя объекта системного монитора|Имя счетчика системного монитора|
|-------------------------------------|--------------------------------------|
|SQLServer: объект Plan Cache|Коэффициент попадания в кэш|
||Страницы кэша|
||Счетчик объектов в кэше*|

*Эти счетчики доступны для разных категорий объектов кэша, включая нерегламентированные запросы [!INCLUDE[tsql](../../includes/tsql-md.md)], подготовленные запросы [!INCLUDE[tsql](../../includes/tsql-md.md)], процедуры, триггеры и т. д. Дополнительные сведения см. в статье [SQL Server, объект Plan Cache](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md).

## <a name="security"></a>Безопасность

### <a name="permissions"></a>Разрешения

Требуется разрешение **CREATE PROCEDURE** на базу данных и разрешение **ALTER** на схему, в которой создается процедура, либо членство в предопределенной роли базы данных **db_ddladmin**.

Для хранимых процедур CLR пользователь должен владеть сборкой, на которую ссылается предложение EXTERNAL NAME, или иметь разрешение **REFERENCES** на эту сборку.

## <a name="create-procedure-and-memory-optimized-tables"></a><a name="mot"></a> CREATE PROCEDURE и таблицы, оптимизированные для памяти

Доступ к оптимизированным для памяти таблицам можно осуществлять из традиционных и скомпилированных в собственном коде хранимых процедур. В большинстве случаев использование собственных процедур является более эффективным способом. Дополнительные сведения см. в статье [Хранимые процедуры, скомпилированные в собственном коде](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).

В следующем примере показано создание скомпилированной в собственном коде хранимой процедуре, имеющей доступ к таблице, оптимизированной для памяти `dbo.Departments`.

```sql
CREATE PROCEDURE dbo.usp_add_kitchen @dept_id INT, @kitchen_count INT NOT NULL
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION
AS
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

UPDATE dbo.Departments
SET kitchen_count = ISNULL(kitchen_count, 0) + @kitchen_count
WHERE ID = @dept_id
END;
GO
```

 Процедуру, созданную без параметра NATIVE_COMPILATION, нельзя преобразовать в хранимую процедуру, скомпилированную в собственном коде.

 Описание возможностей программирования в откомпилированных в собственном коде хранимых процедурах, поддерживаемой контактной зоны запроса и операторов см. в разделе [Поддерживаемые функции для модулей, скомпилированных в собственном коде T-SQL](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).

## <a name="examples"></a><a name="Examples"></a> Примеры

|Категория|Используемые элементы синтаксиса|
|--------------|------------------------------|
|[Базовый синтаксис](#BasicSyntax)|CREATE PROCEDURE|
|[Передача параметров](#Parameters)|@parameter <br> &nbsp;&nbsp;• = default <br> &nbsp;&nbsp; • OUTPUT <br> &nbsp;&nbsp; • тип возвращающего табличное значение параметра <br> &nbsp;&nbsp; • CURSOR VARYING|
|[Изменение данных с помощью хранимой процедуры](#Modify)|UPDATE|
|[Обработка ошибок](#Error)|TRY...CATCH|
|[Запутывание определений процедур](#Encrypt)|WITH ENCRYPTION|
|[Принудительная перекомпиляция хранимой процедуры](#Recompile)|WITH RECOMPILE|
|[Задание контекста безопасности](#Security)|EXECUTE AS|

### <a name="basic-syntax"></a><a name="BasicSyntax"></a> Базовый синтаксис

В примерах из этого раздела показаны основные возможности инструкции CREATE PROCEDURE с применением минимально необходимого синтаксиса.

#### <a name="a-creating-a-simple-transact-sql-procedure"></a>A. Создание простой процедуры Transact-SQL

В следующем примере создается хранимая процедура, возвращающая из представления всех сотрудников (с указанием имени и фамилии), их должности и названия отделов в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Эта процедура не использует параметры. Далее в примере рассматриваются три метода выполнения процедуры.

```sql
CREATE PROCEDURE HumanResources.uspGetAllEmployees
AS
    SET NOCOUNT ON;
    SELECT LastName, FirstName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment;
GO

SELECT * FROM HumanResources.vEmployeeDepartment;
```

Процедуру `uspGetEmployees` можно выполнять следующими способами.

```sql
EXECUTE HumanResources.uspGetAllEmployees;
GO
-- Or
EXEC HumanResources.uspGetAllEmployees;
GO
-- Or, if this procedure is the first statement within a batch:
HumanResources.uspGetAllEmployees;
```

#### <a name="b-returning-more-than-one-result-set"></a>Б. Возвращение более чем одного результирующего набора

Следующая процедура возвращает два результирующих набора.

```sql
CREATE PROCEDURE dbo.uspMultipleResults
AS
SELECT TOP(10) BusinessEntityID, Lastname, FirstName FROM Person.Person;
SELECT TOP(10) CustomerID, AccountNumber FROM Sales.Customer;
GO
```

#### <a name="c-creating-a-clr-stored-procedure"></a>В. Создание хранимой процедуры CLR

В следующем примере создается процедура `GetPhotoFromDB`, ссылающаяся на метод `GetPhotoFromDB` класса `LargeObjectBinary` из сборки `HandlingLOBUsingCLR`. Перед созданием процедуры сборка `HandlingLOBUsingCLR` регистрируется в локальной базе данных.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] (если используется сборка, созданная с помощью *assembly_bits*).

```sql
CREATE ASSEMBLY HandlingLOBUsingCLR
FROM '\\MachineName\HandlingLOBUsingCLR\bin\Debug\HandlingLOBUsingCLR.dll';
GO
CREATE PROCEDURE dbo.GetPhotoFromDB
(
    @ProductPhotoID INT
    , @CurrentDirectory NVARCHAR(1024)
    , @FileName NVARCHAR(1024)
)
AS EXTERNAL NAME HandlingLOBUsingCLR.LargeObjectBinary.GetPhotoFromDB;
GO
```

### <a name="passing-parameters"></a><a name="Parameters"></a> Передача параметров

Примеры в этом разделе показывают, как использовать входные и выходные параметры для передачи значений в хранимую процедуру и возврата значений из нее.

#### <a name="d-creating-a-procedure-with-input-parameters"></a>Г. Создание простой процедуры со входными параметрами

В следующем примере показано создание хранимой процедуры, которая возвращает сведения об определенном сотруднике по переданным значениям фамилии и имени сотрудника. Эта процедура принимает только полные соответствия передаваемым параметрам.

```sql
IF OBJECT_ID ( 'HumanResources.uspGetEmployees', 'P' ) IS NOT NULL
    DROP PROCEDURE HumanResources.uspGetEmployees;
GO
CREATE PROCEDURE HumanResources.uspGetEmployees
    @LastName NVARCHAR(50),
    @FirstName NVARCHAR(50)
AS

    SET NOCOUNT ON;
    SELECT FirstName, LastName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment
    WHERE FirstName = @FirstName AND LastName = @LastName;
GO

```

Процедуру `uspGetEmployees` можно выполнять следующими способами.

```sql
EXECUTE HumanResources.uspGetEmployees N'Ackerman', N'Pilar';
-- Or
EXEC HumanResources.uspGetEmployees @LastName = N'Ackerman', @FirstName = N'Pilar';
GO
-- Or
EXECUTE HumanResources.uspGetEmployees @FirstName = N'Pilar', @LastName = N'Ackerman';
GO
-- Or, if this procedure is the first statement within a batch:
HumanResources.uspGetEmployees N'Ackerman', N'Pilar';

```

#### <a name="e-using-a-procedure-with-wildcard-parameters"></a>Д. Использование процедуры с параметрами-шаблонами

В следующем примере показано создание хранимой процедуры, которая возвращает сведения о сотрудниках по переданным полным или частичным значениям имени и фамилии. Эта процедура ищет соответствие полученным параметрам, а если параметры не предоставлены, то используется шаблон, заданный по умолчанию (фамилии, начинающиеся с буквы `D`).

```sql
IF OBJECT_ID ( 'HumanResources.uspGetEmployees2', 'P' ) IS NOT NULL
    DROP PROCEDURE HumanResources.uspGetEmployees2;
GO
CREATE PROCEDURE HumanResources.uspGetEmployees2
    @LastName NVARCHAR(50) = N'D%',
    @FirstName NVARCHAR(50) = N'%'
AS
    SET NOCOUNT ON;
    SELECT FirstName, LastName, JobTitle, Department
    FROM HumanResources.vEmployeeDepartment
    WHERE FirstName LIKE @FirstName AND LastName LIKE @LastName;
```

Процедуру `uspGetEmployees2` можно выполнять во многих сочетаниях. Здесь показаны лишь некоторые из возможных сочетаний.

```sql
EXECUTE HumanResources.uspGetEmployees2;
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'Wi%';
-- Or
EXECUTE HumanResources.uspGetEmployees2 @FirstName = N'%';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'[CK]ars[OE]n';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'Hesse', N'Stefen';
-- Or
EXECUTE HumanResources.uspGetEmployees2 N'H%', N'S%';
```

#### <a name="f-using-output-parameters"></a>Е. Использование выходных параметров (OUTPUT)

В следующем примере создается процедура `uspGetList`. Эта процедура возвращает список товаров, цена на которые не превышает указанный предел. Данный пример поясняет использование нескольких инструкций `SELECT` и нескольких параметров `OUTPUT`. Параметры OUTPUT предоставляют внешней процедуре, пакету или нескольким инструкциям [!INCLUDE[tsql](../../includes/tsql-md.md)] доступ к значениям, заданным во время выполнения процедуры.

```sql
IF OBJECT_ID ( 'Production.uspGetList', 'P' ) IS NOT NULL
    DROP PROCEDURE Production.uspGetList;
GO  
CREATE PROCEDURE Production.uspGetList @Product VARCHAR(40)
    , @MaxPrice MONEY
    , @ComparePrice MONEY OUTPUT
    , @ListPrice MONEY OUT
AS  
    SET NOCOUNT ON;
    SELECT p.[Name] AS Product, p.ListPrice AS 'List Price'
    FROM Production.Product AS p
    JOIN Production.ProductSubcategory AS s
      ON p.ProductSubcategoryID = s.ProductSubcategoryID
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice;
-- Populate the output variable @ListPprice.
SET @ListPrice = (SELECT MAX(p.ListPrice)
    FROM Production.Product AS p
    JOIN Production.ProductSubcategory AS s
      ON p.ProductSubcategoryID = s.ProductSubcategoryID
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice);
-- Populate the output variable @compareprice.
SET @ComparePrice = @MaxPrice;
GO
```

Процедура `uspGetList` возвращает из базы данных [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] список товаров (велосипедов) стоимостью менее `$700`. Параметры `OUTPUT``@Cost` и `@ComparePrices` используются с языком управления выполнением для вывода информации в окне **Сообщения**.

> [!NOTE]
> Переменная OUTPUT должна быть определена при создании процедуры и при использовании переменной. Имена параметра и переменной могут быть разными, однако типы данных и порядок расположения параметров должны совпадать, если только не используется `@ListPrice` = *variable*.

```sql
DECLARE @ComparePrice MONEY, @Cost MONEY;
EXECUTE Production.uspGetList '%Bikes%', 700,
    @ComparePrice OUT,
    @Cost OUTPUT
IF @Cost <= @ComparePrice
BEGIN
    PRINT 'These products can be purchased for less than
    $'+RTRIM(CAST(@ComparePrice AS VARCHAR(20)))+'.'
END
ELSE
    PRINT 'The prices for all products in this category exceed
    $'+ RTRIM(CAST(@ComparePrice AS VARCHAR(20)))+'.';
```

Частичный результирующий набор:

```
Product                     List Price
--------------------------  ----------
Road-750 Black, 58          539.99
Mountain-500 Silver, 40     564.99
Mountain-500 Silver, 42     564.99
...
Road-750 Black, 48          539.99
Road-750 Black, 52          539.99

(14 row(s) affected)

These items can be purchased for less than $700.00.
```

#### <a name="g-using-a-table-valued-parameter"></a>Ж. Использование возвращающих табличные значения параметров

В следующем примере показано использование возвращающего табличное значение параметра для вставки в таблицу нескольких строк. В примере создается тип параметра, объявляется табличная переменная для ссылки, заполняется список параметров, а затем значения передаются в хранимую процедуру. Хранимая процедура использует эти значения для вставки в таблицу нескольких строк.

```sql
/* Create a table type. */
CREATE TYPE LocationTableType AS TABLE
( LocationName VARCHAR(50)
, CostRate INT );
GO

/* Create a procedure to receive data for the table-valued parameter. */
CREATE PROCEDURE usp_InsertProductionLocation
    @TVP LocationTableType READONLY
    AS
    SET NOCOUNT ON
    INSERT INTO [AdventureWorks2012].[Production].[Location]
       ([Name]
       , [CostRate]
       , [Availability]
       , [ModifiedDate])
    SELECT *, 0, GETDATE()
    FROM @TVP;
GO

/* Declare a variable that references the type. */
DECLARE @LocationTVP
AS LocationTableType;

/* Add data to the table variable. */
INSERT INTO @LocationTVP (LocationName, CostRate)
    SELECT [Name], 0.00
    FROM
    [AdventureWorks2012].[Person].[StateProvince];

/* Pass the table variable data to a stored procedure. */
EXEC usp_InsertProductionLocation @LocationTVP;
GO
```

##### <a name="h-using-an-output-cursor-parameter"></a>З. Использование параметра-курсора OUTPUT

В следующем примере используется параметр курсора OUTPUT для возврата курсора, локального относительно процедуры, в вызывающий пакет, процедуру или триггер.

Сначала следует создать процедуру, объявляющую и открывающую курсор для таблицы `Currency`:

```sql
CREATE PROCEDURE dbo.uspCurrencyCursor
    @CurrencyCursor CURSOR VARYING OUTPUT
AS
    SET NOCOUNT ON;
    SET @CurrencyCursor = CURSOR
    FORWARD_ONLY STATIC FOR
      SELECT CurrencyCode, Name
      FROM Sales.Currency;
    OPEN @CurrencyCursor;
GO
```

Затем выполним пакет, в котором объявляется локальная переменная-курсор, выполняется процедура, назначающая курсор локальной переменной, и извлекаются строки из курсора.

```sql
DECLARE @MyCursor CURSOR;
EXEC dbo.uspCurrencyCursor @CurrencyCursor = @MyCursor OUTPUT;
WHILE (@@FETCH_STATUS = 0)
BEGIN;
    FETCH NEXT FROM @MyCursor;
END;
CLOSE @MyCursor;
DEALLOCATE @MyCursor;
GO
```

### <a name="modifying-data-by-using-a-stored-procedure"></a><a name="Modify"></a> Изменение данных с помощью хранимой процедуры

Примеры в этом разделе показывают, как производится вставка или изменение данных в таблицах и представлениях путем включения инструкции языка DML в определение процедуры.

#### <a name="i-using-update-in-a-stored-procedure"></a>И. Использование UPDATE в хранимой процедуре

В следующем примере инструкция UPDATE используется в хранимой процедуре. Процедура принимает один входной параметр `@NewHours` и один выходной параметр `@RowCount`. Значение параметра `@NewHours` используется в инструкции UPDATE для обновления столбца `VacationHours` в таблице `HumanResources.Employee`. Выходной параметр `@RowCount` используется для возврата значения числа задействованных строк в локальную переменную. Выражение CASE используется в предложении SET для условного определения значения, которое задано для столбца `VacationHours`. Если для сотрудника применяется почасовая ставка оплаты (`SalariedFlag` = 0), то в столбце `VacationHours` устанавливается текущее количество часов плюс значение, заданное в `@NewHours`. В противном случае в столбце `VacationHours` указывается значение, заданное в `@NewHours`.

```sql
CREATE PROCEDURE HumanResources.Update_VacationHours
@NewHours SMALLINT, @Rowcount INT OUTPUT
AS
SET NOCOUNT ON;
UPDATE HumanResources.Employee
SET VacationHours =
    ( CASE
        WHEN SalariedFlag = 0 THEN VacationHours + @NewHours
        ELSE @NewHours
        END
    )
WHERE CurrentFlag = 1;
SET @Rowcount = @@rowcount;

GO
DECLARE @Rowcount INT
EXEC HumanResources.Update_VacationHours 40, @Rowcount OUTPUT
PRINT @Rowcount;
```

### <a name="error-handling"></a><a name="Error"></a> Обработка ошибок

Примеры в этом разделе показывают, как производится обработка ошибок, которые могут возникнуть при выполнении хранимых процедур.

#### <a name="j-using-trycatch"></a>К. Использование конструкции TRY...CATCH

В следующем примере конструкция TRY…CATCH используется для возврата сведений об ошибках во время выполнения хранимой процедуры.

```sql
CREATE PROCEDURE Production.uspDeleteWorkOrder ( @WorkOrderID INT )
AS
SET NOCOUNT ON;
BEGIN TRY
  BEGIN TRANSACTION
  -- Delete rows from the child table, WorkOrderRouting, for the specified work order.
    DELETE FROM Production.WorkOrderRouting
    WHERE WorkOrderID = @WorkOrderID;
  -- Delete the rows from the parent table, WorkOrder, for the specified work order.
    DELETE FROM Production.WorkOrder
    WHERE WorkOrderID = @WorkOrderID;
  COMMIT
END TRY

BEGIN CATCH
  -- Determine if an error occurred.
  IF @@TRANCOUNT > 0
    ROLLBACK

  -- Return the error information.
  DECLARE @ErrorMessage NVARCHAR(4000), @ErrorSeverity INT;
  SELECT @ErrorMessage = ERROR_MESSAGE(),@ErrorSeverity = ERROR_SEVERITY();
  RAISERROR(@ErrorMessage, @ErrorSeverity, 1);
END CATCH;

GO
EXEC Production.uspDeleteWorkOrder 13;

/* Intentionally generate an error by reversing the order in which rows
   are deleted from the parent and child tables. This change does not
   cause an error when the procedure definition is altered, but produces
   an error when the procedure is executed.
*/
ALTER PROCEDURE Production.uspDeleteWorkOrder ( @WorkOrderID INT )
AS

BEGIN TRY
  BEGIN TRANSACTION
  -- Delete the rows from the parent table, WorkOrder, for the specified work order.
    DELETE FROM Production.WorkOrder
    WHERE WorkOrderID = @WorkOrderID;

  -- Delete rows from the child table, WorkOrderRouting, for the specified work order.
    DELETE FROM Production.WorkOrderRouting
    WHERE WorkOrderID = @WorkOrderID;
  COMMIT TRANSACTION
END TRY

BEGIN CATCH
  -- Determine if an error occurred.
  IF @@TRANCOUNT > 0
    ROLLBACK TRANSACTION

  -- Return the error information.
  DECLARE @ErrorMessage NVARCHAR(4000), @ErrorSeverity INT;
  SELECT @ErrorMessage = ERROR_MESSAGE(),@ErrorSeverity = ERROR_SEVERITY();
  RAISERROR(@ErrorMessage, @ErrorSeverity, 1);
END CATCH;
GO
-- Execute the altered procedure.
EXEC Production.uspDeleteWorkOrder 15;

DROP PROCEDURE Production.uspDeleteWorkOrder;
```

### <a name="obfuscating-the-procedure-definition"></a><a name="Encrypt"></a> Запутывание определений процедур

Примеры в этом разделе показывают, как применить запутывание к определению хранимой процедуры.

#### <a name="k-using-the-with-encryption-option"></a>Л. Использование параметра WITH ENCRYPTION

В следующем примере создается процедура `HumanResources.uspEncryptThis`.

**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, база данных SQL.

```sql
CREATE PROCEDURE HumanResources.uspEncryptThis
WITH ENCRYPTION
AS
    SET NOCOUNT ON;
    SELECT BusinessEntityID, JobTitle, NationalIDNumber,
        VacationHours, SickLeaveHours
    FROM HumanResources.Employee;
GO
```

Параметр `WITH ENCRYPTION` запутывает определение процедуры при запросе системного каталога или использовании функций метаданных, как показано в следующих примерах.

Запустите `sp_helptext`.

```sql
EXEC sp_helptext 'HumanResources.uspEncryptThis';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

`The text for object 'HumanResources.uspEncryptThis' is encrypted.`

Напрямую выполните запрос к представлению каталога `sys.sql_modules`:

```sql
SELECT definition FROM sys.sql_modules
WHERE object_id = OBJECT_ID('HumanResources.uspEncryptThis');
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
definition
--------------------------------
NULL
```

### <a name="forcing-the-procedure-to-recompile"></a><a name="Recompile"></a> Принудительная перекомпиляция хранимой процедуры

В примерах этого раздела показано использование предложения WITH RECOMPILE для принудительной перекомпиляции процедуры при каждом ее выполнении.

#### <a name="l-using-the-with-recompile-option"></a>М. Использование параметра WITH RECOMPILE

Предложение `WITH RECOMPILE` полезно, если передаваемые в процедуру параметры являются нетипичными или если новый план выполнения процедуры не следует кэшировать или хранить в памяти.

```sql
IF OBJECT_ID ( 'dbo.uspProductByVendor', 'P' ) IS NOT NULL
    DROP PROCEDURE dbo.uspProductByVendor;
GO
CREATE PROCEDURE dbo.uspProductByVendor @Name VARCHAR(30) = '%'
WITH RECOMPILE
AS
    SET NOCOUNT ON;
    SELECT v.Name AS 'Vendor name', p.Name AS 'Product name'
    FROM Purchasing.Vendor AS v
    JOIN Purchasing.ProductVendor AS pv
      ON v.BusinessEntityID = pv.BusinessEntityID
    JOIN Production.Product AS p
      ON pv.ProductID = p.ProductID
    WHERE v.Name LIKE @Name;
```

### <a name="setting-the-security-context"></a><a name="Security"></a> Задание контекста безопасности

В примерах этого раздела предложение EXECUTE AS используется для задания контекста безопасности, в котором выполняется хранимая процедура.

#### <a name="m-using-the-execute-as-clause"></a>Н. Использование предложения EXECUTE AS

Следующий пример показывает использование предложения [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) для указания контекста безопасности, в котором может выполняться процедура. В данном случае параметр `CALLER` указывает, что процедура может выполняться в контексте вызывающего ее пользователя.

```sql
CREATE PROCEDURE Purchasing.uspVendorAllInfo
WITH EXECUTE AS CALLER
AS
    SET NOCOUNT ON;
    SELECT v.Name AS Vendor, p.Name AS 'Product name',
      v.CreditRating AS 'Rating',
      v.ActiveFlag AS Availability
    FROM Purchasing.Vendor v
    INNER JOIN Purchasing.ProductVendor pv
      ON v.BusinessEntityID = pv.BusinessEntityID
    INNER JOIN Production.Product p
      ON pv.ProductID = p.ProductID
    ORDER BY v.Name ASC;
GO
```

#### <a name="n-creating-custom-permission-sets"></a>О. Создание пользовательских наборов разрешений

В следующем примере предложение EXECUTE AS используется для создания пользовательских разрешений для операции базы данных. Некоторые операции (например, TRUNCATE TABLE) не имеют предоставляемых разрешений. Включив инструкцию TRUNCATE TABLE в хранимую процедуру и указав, что эта процедура должна выполняться от имени пользователя, у которого есть разрешения на изменение таблицы, можно предоставить разрешение на усечение таблицы пользователю с разрешением EXECUTE на эту процедуру.

```sql
CREATE PROCEDURE dbo.TruncateMyTable
WITH EXECUTE AS SELF
AS TRUNCATE TABLE MyDB..MyTable;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="o-create-a-stored-procedure-that-runs-a-select-statement"></a>П. Создание хранимой процедуры, которая выполняет инструкцию SELECT

В этом примере показан основной синтаксис для создания и выполнения процедуры. При выполнении пакета CREATE PROCEDURE должна быть первой инструкцией. Например, чтобы создать следующую хранимую процедуру в [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)], сначала следует задать контекст базы данных, а затем выполнить инструкцию CREATE PROCEDURE.

```sql
-- Uses AdventureWorksDW database

--Run CREATE PROCEDURE as the first statement in a batch.
CREATE PROCEDURE Get10TopResellers
AS
BEGIN
    SELECT TOP (10) r.ResellerName, r.AnnualSales
    FROM DimReseller AS r
    ORDER BY AnnualSales DESC, ResellerName ASC;
END
;
GO

--Show 10 Top Resellers
EXEC Get10TopResellers;
```

## <a name="see-also"></a>См. также:

- [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)
- [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)
- [Курсоры](../../relational-databases/cursors.md)
- [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
- [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [DROP PROCEDURE (Transact-SQL)](../../t-sql/statements/drop-procedure-transact-sql.md)
- [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)
- [EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-transact-sql.md)
- [Хранимые процедуры (компонент Database Engine)](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)
- [sp_procoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md)
- [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)
- [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)
- [sys.procedures (Transact-SQL)](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)
- [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)
- [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [sys.numbered_procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md)
- [sys.numbered_procedure_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-numbered-procedure-parameters-transact-sql.md)
- [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)
- [Создание хранимой процедуры](../../relational-databases/stored-procedures/create-a-stored-procedure.md)
- [Использование параметров, возвращающих табличные значения (ядро СУБД)](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)
- [Функция динамического управления sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
