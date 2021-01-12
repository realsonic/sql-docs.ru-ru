---
description: CREATE TYPE (Transact-SQL)
title: CREATE TYPE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.swb.sysdatatype.properties.f1
- CREATE TYPE
- TYPE_TSQL
- TYPE
- CREATE_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UDTs [SQL Server], creating
- CLR user-defined types [SQL Server]
- user-defined table types [SQL Server]
- user-defined types [SQL Server], creating
- CREATE TYPE statement
- alias data types [SQL Server], creating
- data types [SQL Server], creating
ms.assetid: 2202236b-e09f-40a1-bbc7-b8cff7488905
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a2a3cafce24295670839dbce25f67301900ce57b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099514"
---
# <a name="create-type-transact-sql"></a>CREATE TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает в текущей базе данных псевдоним типа данных или определяемый пользователем тип в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Реализация псевдонима типа данных основывается на собственном системном типе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Определяемый пользователем тип реализуется с помощью класса сборки в среде [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] CLR. Чтобы привязать определяемый пользователем тип данных к его реализации, сборка среды CLR, содержащая реализацию данного типа, должна быть сначала зарегистрирована в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью инструкции [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md).  
  
 Возможность выполнения CLR-кода в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отключена по умолчанию. Можно создавать, изменять и удалять объекты базы данных, которые ссылаются на модули управляемого кода, но эти ссылки не будут действовать в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если параметр [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) не включен с помощью процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
 
> [!NOTE]  
>  В этом разделе рассматривается интеграция среды CLR .NET Framework с SQL Server. Интеграция со средой CLR не применяется к [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Azure.
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- User-defined Data Type Syntax    
CREATE TYPE [ schema_name. ] type_name  
{   
    [
      FROM base_type   
      [ ( precision [ , scale ] ) ]  
      [ NULL | NOT NULL ]
    ]
    | EXTERNAL NAME assembly_name [ .class_name ]   
    | AS TABLE ( { <column_definition> | <computed_column_definition> [ ,... n ] }
      [ <table_constraint> ] [ ,... n ]    
      [ <table_index> ] [ ,... n ] } )
 
} [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   
    [ NULL | NOT NULL ]  
    [   
        DEFAULT constant_expression ]   
      | [ IDENTITY [ ( seed ,increment ) ]   
    ]  
    [ ROWGUIDCOL ] [ <column_constraint> [ ...n ] ]   
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
                [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH ( <index_option> [ ,...n ] )   
        ]  
  | CHECK ( logical_expression )   
}   
  
<computed_column_definition> ::=  
  
column_name AS computed_column_expression   
[ PERSISTED [ NOT NULL ] ]  
[   
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [   
            WITH ( <index_option> [ ,...n ] )  
        ]  
    | CHECK ( logical_expression )   
]   
  
<table_constraint> ::=  
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
    ( column [ ASC | DESC ] [ ,...n ] )   
        [   
    WITH ( <index_option> [ ,...n ] )   
        ]  
    | CHECK ( logical_expression )   
}   
  
<index_option> ::=  
{  
    IGNORE_DUP_KEY = { ON | OFF }  
}  

< table_index > ::=  
  INDEX constraint_name  
     [ CLUSTERED | NONCLUSTERED ]   (column [ ASC | DESC ] [ ,... n ] )} }  
```  
  
```syntaxsql
-- User-defined Memory Optimized Table Types Syntax  
CREATE TYPE [schema_name. ] type_name  
AS TABLE ( { <column_definition> [ ,... n ] }  
    | [ <table_constraint> ] [ ,... n ]    
    | [ <table_index> ] [ ,... n ] } )
    [ WITH ( <table_option> [ ,... n ] ) ]  
 [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   [ NULL | NOT NULL ]    [  
      [ IDENTITY [ (1 , 1) ]  
    ]  
    [ <column_constraint> [ ... n ] ]    [ <column_index> ]  
  
<data type> ::=  
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]  
  
<column_constraint> ::=  
{ PRIMARY KEY {   NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count) 
                | NONCLUSTERED } }  
  
< table_constraint > ::=  
{ PRIMARY KEY { NONCLUSTERED HASH (column [ ,... n ] ) 
                   WITH (BUCKET_COUNT = bucket_count) 
               | NONCLUSTERED  (column [ ASC | DESC ] [ ,... n ] )  } }  
  
<column_index> ::=  
  INDEX index_name  
{ { [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count) 
     | NONCLUSTERED } }  
  
< table_index > ::=  
  INDEX constraint_name  
{ { [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count) 
 |  [NONCLUSTERED]  (column [ ASC | DESC ] [ ,... n ] )} }  
  
<table_option> ::=  
{  
    [MEMORY_OPTIMIZED = {ON | OFF}]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *schema_name*  
 Имя схемы, к которой относится псевдоним типа данных или определяемый пользователем тип данных.  
  
 *type_name*  
 Имя псевдонима типа данных или определяемого пользователем типа данных. Имена типов должны соответствовать требованиям к именам [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
 *base_type*  
 Предоставленный [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип данных, на котором основан тип данных псевдонима. Аргумент *base_type* имеет тип **sysname**, не имеет значения по умолчанию и может принимать одно из следующих значений:  
  
|||||  
|-|-|-|-|  
|**bigint**|**binary(** *n* **)**|**bit**|**char(** *n* **)**|  
|**date**|**datetime**|**datetime2**|**datetimeoffset**|  
|**decimal**|**float**|**image**|**int**|  
|**money**|**nchar(** *n* **)**|**ntext**|**numeric**|  
|**nvarchar(** *n* &#124; **max)**|**real**|**smalldatetime**|**smallint**|  
|**smallmoney**|**sql_variant**|**text**|**time**|  
|**tinyint**|**uniqueidentifier**|**varbinary(** *n* &#124; **max)**|**varchar(** *n* &#124; **max)**|  
  
 Аргумент *base_type* может быть также синонимом любого типа данных, сопоставляемого с одним из этих системных типов данных.  
  
 *precision*  
 Для типа **decimal** или **numeric** является неотрицательным целым числом, которое указывает на максимальное общее число подлежащих сохранению десятичных знаков как слева, так и справа от десятичного разделителя, отделяющего десятичную дробь от целого числа. Дополнительные сведения см. в разделе [decimal и numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 *масштаб*  
 Для типа **decimal** или **numeric** является неотрицательным целым числом, которое указывает на максимальное общее число подлежащих сохранению десятичных знаков справа от разделителя, отделяющего десятичную дробь от целого числа. Значение должно быть меньше или равно заданной степени точности. Дополнительные сведения см. в разделе [decimal и numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 **NULL** | NOT NULL  
 Указывает, может ли данный тип иметь значение NULL. Если не указано иное, по умолчанию принимается значение NULL.  
  
 *assembly_name*  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает на сборку [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которая ссылается на реализацию определяемого пользователем типа в среде CLR. Аргумент *assembly_name* должен соответствовать существующей сборке в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в текущей базе данных.  
  
> [!NOTE]  
>  Параметр EXTERNAL_NAME недоступен в автономной базе данных.  
  
 **[.** *class_name*  **]**  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Задает класс сборки, реализующей определяемый пользователем тип. Аргумент *class_name* должен быть допустимым идентификатором и существовать как класс в сборке с видимостью сборки. Аргумент *class_name* учитывает регистр символов независимо от параметров сортировки, установленных в библиотеке. Его значение должно точно соответствовать имени класса в соответствующей сборке. Именем класса может быть заключенное в квадратные скобки ( **[]** ) имя с указанием пространства имен, если в языке программирования, на котором записан класс, используется концепция пространств имен, как, например, в языке C#. Если аргумент *class_name* не задан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]считает, что его значение равно значению аргумента *type_name*.  
  
 \<column_definition>  
 Определяет столбцы для определяемого пользователем табличного типа.  
  
 \<data type>  
 Определяет тип данных в столбце для определяемого пользователем табличного типа. Дополнительные сведения о типах данных см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md). Дополнительные сведения о таблицах см. в разделе [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<column_constraint>  
 Определяет ограничения столбца для определяемого пользователем табличного типа. Из ограничений поддерживаются PRIMARY KEY, UNIQUE и CHECK. Дополнительные сведения о таблицах см. в разделе [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<computed_column_definition>  
 Определяет выражение вычисляемого столбца в качестве столбца в определяемом пользователем табличном типе. Дополнительные сведения о таблицах см. в разделе [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<table_constraint>  
 Определяет ограничение таблицы на основе определяемого пользователем табличного типа. Из ограничений поддерживаются PRIMARY KEY, UNIQUE и CHECK.  
  
 \<index_option>  
 Определяет реакцию на ошибку, возникшую из-за дублирования значений ключа при вставке нескольких строк в уникальный кластеризованный или уникальный некластеризованный индекс. Дополнительные сведения о параметрах индекса см. в разделе [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
 
  `INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] )`  
     
**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Задает создание индекса для таблицы. Это может быть кластеризованный или некластеризованный индекс. Индекс будет содержать указанные столбцы и сортировать данные по возрастанию или убыванию.
  
 INDEX  
 Индексы столбцов и таблиц необходимо указывать в составе инструкции CREATE TABLE. DROP INDEX и CREATE INDEX не поддерживаются для таблиц, оптимизированных для памяти.  
  
 MEMORY_OPTIMIZED  
 **Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Указывает, является ли тип таблицы оптимизированным для памяти. Этот параметр по умолчанию выключен. Таблица (тип) не является таблицей (типом), оптимизированной для памяти. Оптимизированные для памяти типы таблицы — это оптимизированные для памяти пользовательские таблицы, схемы которых, как и схемы других пользовательских таблиц, сохраняются на диске.  
  
 BUCKET_COUNT  
 **Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Отображает число контейнеров, которые необходимо создать в хэш-индексе. Максимальное значение для параметра BUCKET_COUNT в хэш-индексах составляет 1 073 741 824. Дополнительные сведения о числах контейнеров см. в разделе [Индексы для таблиц, оптимизированных для памяти](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md). *bucket_count* — это обязательный аргумент.  
  
 HASH  
 **Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Указывает, что был создан индекс HASH. Хэш-индексы поддерживаются только в таблицах, оптимизированных для памяти.  
  
## <a name="remarks"></a>Remarks  
 И класс сборки, ссылка на который содержится в *assembly_name*, и его методы должны удовлетворять требованиям, предъявляемым при реализации определяемых пользователем типов в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения об этих требованиях см. в статье [Определяемые пользователем типы CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  
  
 К числу дополнительных соображений относятся следующие:  
  
-   данный класс может иметь перегруженные методы, но эти методы могут вызываться только из управляемого кода, а не с помощью языка [!INCLUDE[tsql](../../includes/tsql-md.md)];  
  
-   все статические члены должны быть объявлены как **const** или **readonly**, если аргумент *assembly_name* имеет значение SAFE или EXTERNAL_ACCESS.  
  
 Внутри базы данных может существовать только один определяемый пользователем тип, зарегистрированный на основе любого указанного типа, который был загружен в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из CLR. Если определяемый пользователем тип создается на основе типа CLR, для которого в этой базе данных уже существует пользовательский тип, инструкция CREATE TYPE вызывает сбой. Это ограничение необходимо для того, чтобы избежать неоднозначности при разрешении типа SQL Type, если тип CLR может соответствовать более чем одному определяемому пользователем типу.  
  
 Если какой-либо метод-мутатор в данном типе не возвращает значения *void*, инструкция CREATE TYPE не выполняется.  
  
 Чтобы внести изменения в определяемый пользователем тип, нужно удалить этот тип с помощью инструкции DROP TYPE и затем снова создать его.  
  
 В отличие от определяемых пользователем типов, созданных с помощью хранимой процедуры **sp_addtype**, разрешение REFERENCES на типы, созданные с помощью инструкции CREATE TYPE, не предоставляется роли базы данных **public** автоматически. Это разрешение должно предоставляться отдельно.  
  
 В определяемых пользователем табличных типах определяемые пользователем структурированные типы, используемые в *column_name* \<data type>, представляют собой часть области схемы базы данных, в которой определен тип таблицы. Чтобы получить доступ к определяемым пользователем структурированным типам в другой области базы данных, используйте двухкомпонентные имена.  
  
 В определяемых пользователем табличных типах первичный ключ на вычисляемых столбцах должен иметь атрибуты PERSISTED и NOT NULL.  
  
## <a name="memory-optimized-table-types"></a>Оптимизированные для памяти типы таблиц  
 Начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], обработка данных в типе таблицы может выполняться в основной памяти, но не на диске. Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Примеры кода по созданию типов оптимизированных для памяти таблиц см. в разделе [Создание таблиц, оптимизированных для памяти, и хранимых процедур, скомпилированных в собственном коде](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения CREATE TYPE в текущей базе данных и разрешения ALTER для схемы *schema_name*. Если аргумент *schema_name* не указан, в действие вступают принимаемые по умолчанию правила разрешения имен с целью определения схемы для текущего пользователя. Если аргумент *assembly_name* указан, пользователь должен либо быть владельцем данной сборки, либо иметь разрешение REFERENCES для работы с ней.  

 Если какие-либо столбцы в инструкции CREATE TABLE определены как принадлежащие к определяемому пользователем типу данных, необходимо иметь разрешение REFERENCES на него.
 
   >[!NOTE]
  > После создания таблицы со столбцом, который использует определяемый пользователем тип, потребуется разрешение REFERENCES для работы с такими типами.
  > Если эта таблица должна создаваться в базе данных TempDB, либо разрешение REFERENCES должно предоставляться явным образом **перед** каждой процедурой создания таблицы, либо этот тип данных и разрешения REFERENCES нужно добавить в базу данных модели. В таком случае этот тип данных и разрешения будут всегда доступны в базе данных TempDB. В противном случае определяемый пользователем тип данных и разрешения нельзя будет использовать после перезапуска SQL Server. Дополнительные сведения см. в разделе [CREATE TABLE](./create-table-transact-sql.md#permissions-1).
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-an-alias-type-based-on-the-varchar-data-type"></a>A. Создание псевдонима на базе типа данных varchar  
 В следующем примере создается псевдоним на базе определенного в системе типа данных `varchar`.  
  
```sql  
CREATE TYPE SSN  
FROM varchar(11) NOT NULL ;  
```  
  
### <a name="b-creating-a-user-defined-type"></a>Б. Создание определяемого пользователем типа  
 В следующем примере создается тип `Utf8String`, который ссылается на класс `utf8string` в сборке `utf8string`. Перед тем как приступить к созданию этого типа, сборка `utf8string` регистрируется в локальной базе данных. Замените двоичную часть инструкции CREATE ASSEMBLY на допустимое описание.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
```sql  
CREATE ASSEMBLY utf8string  
AUTHORIZATION [dbi]   
FROM 0x4D... ;  
GO  
CREATE TYPE Utf8String   
EXTERNAL NAME utf8string.[Microsoft.Samples.SqlServer.utf8string] ;  
GO  
```  
  
### <a name="c-creating-a-user-defined-table-type"></a>В. Создание определяемого пользователем табличного типа  
 В следующем примере создается определяемый пользователем табличный тип, который имеет два столбца. Дополнительные сведения о создании и использовании возвращающих табличное значение параметрах см. в разделе [Использование параметров, возвращающих табличные значения (ядро СУБД)](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
```sql  
CREATE TYPE LocationTableType AS TABLE   
    ( LocationName VARCHAR(50)  
    , CostRate INT );  
GO  
```  

### <a name="d-creating-a-user-defined-table-type-with-primary-key-and-index"></a>Г. Создание определяемого пользователем табличного типа с первичным узлом и индексом
В приведенном ниже примере создается определяемый пользователем табличный тип с тремя столбцами, один из которых (`Name`) является первичным ключом, а другой (`Price`) имеет некластеризованный индекс.  Дополнительные сведения о создании и использовании возвращающих табличное значение параметрах см. в разделе [Использование параметров, возвращающих табличные значения (ядро СУБД)](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).

```sql
CREATE TYPE InventoryItem AS TABLE
(
    [Name] NVARCHAR(50) NOT NULL,
    SupplierId BIGINT NOT NULL,
    Price DECIMAL (18, 4) NULL,
    PRIMARY KEY (
        Name
    ),
    INDEX IX_InventoryItem_Price (
        Price
    )
)
GO
```
  
## <a name="see-also"></a>См. также:  
 [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP TYPE (Transact-SQL)](../../t-sql/statements/drop-type-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)    
 [Определяемые пользователем типы CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)     
 [Работа с определяемыми пользователем типами в SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)     
