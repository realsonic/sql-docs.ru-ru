---
description: CREATE TABLE (Azure Synapse Analytics)
title: CREATE TABLE (Azure Synapse Analytics) | Документация Майкрософт
ms.custom: ''
ms.date: 07/03/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ea21c73c-40e8-4c54-83d4-46ca36b2cf73
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: bf8ef986da54559c5928677643f6bd99c63c2266
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643020"
---
# <a name="create-table-azure-synapse-analytics"></a>CREATE TABLE (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Создает новую таблицу в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

Сведения о таблицах и об использовании таблиц см. в разделе [Таблицы в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-overview/).

> [!NOTE]
>  Обсуждения [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] в этой статье применимы как к [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], так и к [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], если не указано иное.

 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

<a name="Syntax"></a>

## <a name="syntax"></a>Синтаксис
  
```syntaxsql
-- Create a new table.
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( 
      { column_name <data_type>  [ <column_options> ] } [ ,...n ]
    )  
    [ WITH ( <table_option> [ ,...n ] ) ]  
[;]  

<column_options> ::=
    [ COLLATE Windows_collation_name ]  
    [ NULL | NOT NULL ] -- default is NULL  
    [ <column_constraint> ]

<column_constraint>::=
    {
        DEFAULT DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED  NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }

<table_option> ::=
    {
       CLUSTERED COLUMNSTORE INDEX --default for Azure Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column [,...n])  
      | HEAP --default for Parallel Data Warehouse
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) -- default is ASC
    }  
    {
        DISTRIBUTION = HASH ( distribution_column_name )
      | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics
      | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse
    }
    | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] -- default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) )

<data type> ::=
      datetimeoffset [ ( n ) ]  
    | datetime2 [ ( n ) ]  
    | datetime  
    | smalldatetime  
    | date  
    | time [ ( n ) ]  
    | float [ ( n ) ]  
    | real [ ( n ) ]  
    | decimal [ ( precision [ , scale ] ) ]   
    | numeric [ ( precision [ , scale ] ) ]   
    | money  
    | smallmoney  
    | bigint  
    | int   
    | smallint  
    | tinyint  
    | bit  
    | nvarchar [ ( n | max ) ]  -- max applies only to Azure Synapse Analytics 
    | nchar [ ( n ) ]  
    | varchar [ ( n | max )  ] -- max applies only to Azure Synapse Analytics  
    | char [ ( n ) ]  
    | varbinary [ ( n | max ) ] -- max applies only to Azure Synapse Analytics  
    | binary [ ( n ) ]  
    | uniqueidentifier  
```  

<a name="Arguments"></a>
## <a name="arguments"></a>Аргументы

 *database_name*  
 Имя базы данных, которая будет содержать новую таблицу. Значение по умолчанию — текущая база данных.  
  
 *schema_name*  
 Схема таблицы. *Схема* является необязательной. Если схема не указана, используется схема по умолчанию.  
  
 *table_name*  
 Имя новой таблицы. Чтобы создать локальную временную таблицу, укажите # перед именем таблицы.  Пояснения и рекомендации для временных таблиц см. в разделе [Временные таблицы в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-temporary/). 

 *column_name*  
 Имя столбца таблицы.

### <a name="column-options"></a><a name="ColumnOptions"></a> Параметры столбца

 `COLLATE` *параметры_сортировки_Windows*  
 Задает параметры сортировки для выражения. Параметры сортировки должны входить в число параметров сортировки Windows, поддерживаемых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Список параметров сортировки Windows, поддерживаемых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в разделе [Имя параметров сортировки Windows (Transact-SQL)](windows-collation-name-transact-sql.md)/).  
  
 `NULL` | `NOT NULL`  
 Указывает, допустимы ли для столбца значения `NULL`. Значение по умолчанию — `NULL`.  
  
 [ `CONSTRAINT` *имя_ограничения* ] `DEFAULT` *выражение_ограничения*  
 Указывает значение столбца по умолчанию.  
  
 | Аргумент | Объяснение |
 | -------- | ----------- |
 | *constraint_name* | Необязательное имя ограничения. Имя ограничения уникально в пределах базы данных. Имя можно использовать повторно в других базах данных. |
 | *constant_expression* | Значение по умолчанию для столбца. Выражение должно быть литералом или константой. Например, могут использоваться следующие константные выражения: `'CA'`, `4`. Следующие константные выражения не могут использоваться: `2+3`, `CURRENT_TIMESTAMP`. |
  
### <a name="table-structure-options"></a><a name="TableOptions"></a> Параметры структуры таблицы

Рекомендации по выбору типа таблицы см. в разделе [Индексирование таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index/).
  
 `CLUSTERED COLUMNSTORE INDEX` 
 
Сохраняет таблицу как кластеризованный индекс columnstore. Кластеризованный индекс columnstore применяется ко всем данным таблицы. Это поведение является стандартным для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
 
 `HEAP` Сохраняет таблицу в виде кучи. Это поведение является стандартным для [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 `CLUSTERED INDEX` ( *index_column_name* [ ,...*n* ] )  
 Сохраняет таблицу в виде кластеризованного индекса с одним или несколькими ключевыми столбцами. Данные сохраняются по записям. Используйте *имя_индексного_столбца* для указания имен одного или нескольких ключевых столбцов в индексе.  Дополнительные сведения см. в подразделе "Таблицы rowstore" в разделе "Общие замечания".
 
 `LOCATION = USER_DB` Этот параметр не рекомендуется использовать. Он является допустимым с точки зрения синтаксиса, но больше не требуется и не влияет на поведение.   
  
### <a name="table-distribution-options"></a><a name="TableDistributionOptions"></a> Параметры распределения таблицы

Сведения о выборе наилучшего метода распределения и об использовании таблиц распределения см. в разделе [Распределение таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-distribute/).

`DISTRIBUTION = HASH` ( *имя_столбца_распределения* ) Назначает каждую строку одному распределению путем хэширования значения, которое хранится в столбце распределения с указанным *именем_столбца_распределения*. Алгоритм является детерминированным, то есть одному и тому же значению всегда соответствует одно и то же распределение.  Столбец распределения должен быть определен как NOT NULL, так как все записи, имеющие значение NULL, назначены одному и тому же распределению.

`DISTRIBUTION = ROUND_ROBIN` Равномерно распределяет строки между всеми распределениями циклическим способом. Это поведение является стандартным для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].

`DISTRIBUTION = REPLICATE` Сохраняет по одной копии таблицы на каждом вычислительном узле. Для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] таблица хранится в базе данных распространителя на каждом вычислительном узле. Для [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] таблица хранится в файловой группе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которая охватывает вычислительный узел. Это поведение является стандартным для [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
### <a name="table-partition-options"></a><a name="TablePartitionOptions"></a> Параметры секционирования таблицы
Рекомендации по использованию секций таблицы см. в разделе [Секционирование таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/).

 `PARTITION` ( *имя_столбца_секции* `RANGE` [ `LEFT` | `RIGHT` ] `FOR VALUES` ( [ *граничное_значение* [,...*n*] ] ))   
Создает одну или несколько секций таблицы. Эти секции представляют собой горизонтальные срезы таблицы, которые позволяют применять операции к подмножествам записей независимо от того, хранится ли таблица в виде кучи, кластеризованного индекса или кластерного индекса columnstore. В отличие от столбца распределения секции таблицы не определяют распределений, в которых хранятся записи. Вместо этого секции таблицы определяют группирование и хранение строк в каждом распределении.  

| Аргумент | Объяснение |
| -------- | ----------- |
|*имя_столбца_секции*| Указывает столбец, который [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] будет использовать для секционирования строк. Столбец может иметь любой тип данных. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] сортирует значения столбцов секционирования по возрастанию. Порядок от низкого к высокому следует от `LEFT` до `RIGHT` в спецификации `RANGE`. |  
| `RANGE LEFT` | Указывает граничное значение, принадлежащее секции слева (меньшие значения). Значение по умолчанию — LEFT. |
| `RANGE RIGHT` | Указывает граничное значение, принадлежащее секции справа (большие значения). | 
| `FOR VALUES` ( *boundary_value* [,...*n*] ) | Указывает граничные значения для секции. *граничное_значение* является константным выражением. Оно не может быть NULL. Оно должно иметь тип данных *столбца_секции* или неявно преобразовываться в этот тип. Это значение не может быть усечено во время неявного преобразования, так чтобы размер и масштаб значения не соответствовали типу данных *столбца_секции*<br></br><br></br>Если вы указали предложение `PARTITION`, но не указали граничное значение, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] создаст секционированную таблицу с одной секцией. Если это допустимо, вы можете разделить таблицу на две секции в дальнейшем.<br></br><br></br>При указании одного граничного значения в результирующей таблице будет две секции; одна для значений меньше граничного значения и вторая для значения больше граничного значения. При перемещении секции в несекционированную таблицу эта таблица получит данные, но в метаданных этой таблицы будут отсутствовать границы секций.| 

 См. раздел [Создание секционированной таблицы](#PartitionedTable) в разделе "Примеры".

### <a name="ordered-clustered-columnstore-index-option"></a>Вариант упорядоченного кластеризованного индекса columnstore 

Кластеризованный индекс columnstore включен по умолчанию для создания таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  Перед сжатием данных в сегментах columnstore данные в индексе не сортируются.  При создании индекса с ORDER данные сортируются до добавления в сегменты индекса, и производительность запросов можно улучшить. Дополнительные сведения см. в разделе [Настройка производительности с упорядоченным кластеризованным индексом columnstore](/azure/sql-data-warehouse/performance-tuning-ordered-cci?view=azure-sqldw-latest&preserve-view=true).  

Упорядоченный кластеризованный индекс columnstore можно создавать для столбцов любых типов данных, поддерживаемых в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], за исключением строковых столбцов.  

Пользователи могут запрашивать столбец **column_store_order_ordinal** в **sys.index_columns** для получения списка столбцов, по которым упорядочена таблица, и последовательности упорядочения.  

Дополнительные сведения см. в разделе [Настройка производительности с упорядоченным кластеризованным индексом columnstore](/azure/sql-data-warehouse/performance-tuning-ordered-cci).   

### <a name="data-type"></a><a name="DataTypes"></a> Тип данных

[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] поддерживает наиболее часто используемые типы данных. Ниже приведен список поддерживаемых типов данных, сведения о них и размер при хранении в байтах. Чтобы лучше разобраться в типах данных и способах их использования, обратитесь к разделу [Типы данных таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] ](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-data-types).

Таблица преобразования типов данных приведена в разделе "Неявные преобразования" статьи [CAST и CONVERT (Transact-SQL)](../functions/cast-and-convert-transact-sql.md).

>[!NOTE]
>См. подробнее о [типах данных и функциях даты и времени (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md).

`datetimeoffset` [ ( *n* ) ]  
 Значение по умолчанию для *n* равно 7.  
  
 `datetime2` [ ( *n* ) ]  
То же, что `datetime`, за исключением того, что можно указать количество долей секунды. Значение по умолчанию для *n* равно `7`.  
  
|Значение *n*|Точность|Масштабирование|  
|--:|--:|-:|  
|`0`|19|0|  
|`1`|21|1|  
|`2`|22|2|  
|`3`|23|3|  
|`4`|24|4|  
|`5`|25|5|  
|`6`|26|6|  
|`7`|27|7|  
  
 `datetime`  
 Сохраняет дату и время дня длиной от 19 до 23 символов по Григорианскому календарю. Дата может содержать год, месяц и день. Время содержит часы, минуты и секунды. Также можно включить отображение трех цифр для долей секунды. Размер при хранении составляет 8 байт.  
  
 `smalldatetime`  
 Сохраняет дату и время. Размер при хранении составляет 4 байта.  
  
 `date`  
 Сохраняет дату длиной до 10 символов по григорианскому календарю. Размер при хранении составляет 3 байта. Дата сохраняется в виде целого числа.  
  
 `time` [ ( *n* ) ]  
 Значение по умолчанию для *n* равно `7`.  
  
 `float` [ ( *n* ) ]  
 Типы приблизительных числовых данных, используемые для числовых данных с плавающей запятой. Данные с плавающей запятой являются приблизительными, поэтому не все значения из диапазона могут быть отображены точно. *n* указывает количество битов, используемых для хранения мантиссы `float` в экспоненциальном представлении. *n* определяет точность и размер хранения данных. Если указан параметр *n*, он должен находиться в диапазоне от `1` до `53`. Значение *n* по умолчанию — `53`.  
  
| Значение *n* | Точность | Объем памяти |  
| --------: | --------: | -----------: |  
| 1–24   | 7 цифр  | 4 байта      |  
| 25–53  | 15 знаков | 8 байт      |  
  
 В приложении [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] параметр *n* может принимать одно из двух возможных значений. Если `1`<= *n* <= `24`, *n* принимает значение `24`. Если `25` <= *n* <= `53`, *n* принимает значение `53`.  
  
 Тип данных [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] `float` соответствует стандарту ISO для всех значений *n* в диапазоне от `1` до `53`. Синонимом типа double precision является тип `float(53)`.  
  
 `real` [ ( *n* ) ]  
 Определение типа real соответствует определение типа float. Синонимом по стандарту ISO для типа `real` является `float(24)`.  
  
 `decimal` [ ( *точность* [ *, масштаб* ] ) ] | `numeric` [ ( *точность* [ *, масштаб* ] ) ]  
 Хранит числа с фиксированной точностью и масштабом.  
  
 *precision*  
 Максимальное количество десятичных разрядов числа (как слева, так и справа от десятичной запятой). Точность должна находиться в диапазоне от `1` до `38`. Точность по умолчанию — `18`.  
  
 *масштаб*  
 Максимальное количество десятичных разрядов числа справа от десятичной запятой. *Масштаб* должен иметь значение от `0` до *точности*. *Масштаб* можно указать только в том случае, если указана *точность*. Масштаб по умолчанию — `0`, поэтому `0` <= *масштаб* <= *точность*. Максимальный размер хранилища зависит от точности.  
  
| Точность | Байты хранилища  |  
| ---------: |-------------: |  
|  1–9       |             5 |  
| 10–19      |             9 |  
| 20–28      |            13 |  
| 29–38      |            17 |  
  
 `money` | `smallmoney`  
 Типы данных, представляющие денежные значения.  
  
| Тип данных | Байты хранилища |  
| --------- | ------------: |  
| `money`|8|  
| `smallmoney` |4|  
  
 `bigint` \| `int` \| `smallint` \| `tinyint`  
 Типы точных числовых данных, использующие целые значения. Размер данных хранения приведен в следующей таблице.  
  
| Тип данных | Байты хранилища |  
| --------- | ------------: |  
| `bigint`|8|  
| `int` |4|  
| `smallint` |2|  
| `tinyint` |1|  
  
 `bit`  
 Целочисленный тип данных, который может принимать значения `1`, `0` или NULL. Компонент [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] оптимизирует хранение столбцов типа bit. Если в таблице имеется 8 или менее столбцов типа bit, они хранятся как 1 байт. Если имеется от 9 до 16 столбцов типа bit, они хранятся как 2 байта и т.д.  
  
 `nvarchar` [ ( *n* | `max` ) ]  -- `max` применяется только к [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Символьные данные Юникода с переменной длиной. *n* может иметь значение от 1 до 4000. Значение `max` указывает, что максимальный размер при хранении составляет 2^31-1 байт (2 ГБ). Размер для хранения в байтах равен удвоенному числу введенных символов + 2 байта. Введенные данные могут иметь длину в ноль символов.  
  
 `nchar` [ ( *n* ) ]  
 Символьные данные фиксированной длины в формате Юникод. Длина составляет *n* символов. *n* должно иметь значение от `1` до `4000`. Размер хранилища — дважды *n* байт.  
  
 `varchar` [ ( *n*  | `max` ) ]  -- `max` применяется только к [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].   
 Символьные данные переменной длины не в формате Юникод. Длина составляет *n* байт. *n* может иметь значение от `1` до `8000`. Значение `max` указывает, что максимальный размер при хранении составляет 2^31-1 байт (2 ГБ). Размер при хранении — это фактический размер введенных данных плюс 2 байта.  
  
 `char` [ ( *n* ) ]  
 Символьные данные фиксированной длины не в формате Юникод. Длина составляет *n* байт. *n* может иметь значение от `1` до `8000`. Размер при хранении составляет *n* байт. Значение *n* по умолчанию — `1`.  
  
 `varbinary` [ ( *n*  | `max` ) ]  -- `max` применяется только к [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Двоичные данные с переменной длиной. *n* может иметь значение от `1` до `8000`. Значение `max` указывает, что максимальный размер при хранении составляет 2^31-1 байт (2 ГБ). Размер хранения равен фактической длине данных плюс два байта. Значение по умолчанию для *n* равно 7.  
  
 `binary` [ ( *n* ) ]  
 Двоичные данные фиксированной длины *n* байт. *n* может иметь значение от `1` до `8000`. Размер при хранении составляет *n* байт. Значение по умолчанию для *n* равно `7`.  
  
 `uniqueidentifier`  
 16-байтовый идентификатор GUID.  
   
<a name="Permissions"></a>  
## <a name="permissions"></a>Разрешения  
 Для создания таблицы требуется разрешение в предопределенной роли базы данных `db_ddladmin` или:
 - разрешение `CREATE TABLE` в базе данных
 - разрешение `ALTER SCHEMA` для схемы, которая будет содержать таблицу. 

Для создания секционированной таблицы требуется разрешение в предопределенной роли базы данных `db_ddladmin` или

- разрешение `ALTER ANY DATASPACE`
  
 Имя входа, от имени которого создается локальная временная таблица, получает разрешения `CONTROL`, `INSERT`, `SELECT` и `UPDATE` для этой таблицы.  
 
<a name="GeneralRemarks"></a>  
## <a name="general-remarks"></a>Общие замечания  
 
Минимальные и максимальные ограничения см. в разделе [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]Ограничения емкости ](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-service-capacity-limits/). 
 
### <a name="determining-the-number-of-table-partitions"></a>Определение числа секций таблицы
Каждая пользовательская таблица делится на несколько таблиц меньшего размера, которые хранятся в различных расположениях, называемых распределениями. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] использует 60 распределений. В [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] количество распределений зависит от количества вычислительных узлов.
 
Каждое распределение содержит все секции таблицы. Например, для 60 распределений, четырех секций таблицы и одной пустой секции мы получим 300 секций (5 x 60= 300). Если в таблице есть кластеризованные индексы columnstore, то в каждой секции будет один индекс columnstore, т. е. в общей сложности в таблице будет 300 индексов columnstore.

Рекомендуется использовать меньшее число секций таблицы, чтобы в каждом индексе columnstore было достаточное количество строк для использования всех преимуществ индексов columnstore. Дополнительные сведения см. в статье [Секционирование таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] ](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/)и в разделе об [индексировании таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/).  

### <a name="rowstore-table-heap-or-clustered-index"></a>Таблица rowstore (куча или кластеризованный индекс)

Таблица rowstore — это таблица, которая хранится в порядке по строкам. Это куча или кластеризованный индекс. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] создает все таблицы rowstore с включенным сжатием страниц. Это поведение не настраивается пользователем.

### <a name="columnstore-table-columnstore-index"></a>Таблица columnstore (индекс columnstore)

Таблица columnstore — это таблица, которая хранится в порядке по столбцам. Индекс columnstore — это технология, которая управляет данными, хранящимися в таблице columnstore.  Кластеризованный индекс columnstore не влияет на способ распределения данных, а влияет на способ хранения данных в пределах каждого распределения.

Чтобы преобразовать таблицу rowstore в таблицу columnstore, удалите все существующие индексы таблицы и создайте кластеризованный индекс columnstore. Пример см. в разделе [CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md).

Дополнительные сведения вы найдете в следующих статьях:
- [Сводка функций индексов columnstore по версиям](/sql/relational-databases/indexes/columnstore-indexes-what-s-new)
- [Индексирование таблиц в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)
- [Руководство по индексам columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md) 

<a name="LimitationsRestrictions"></a>  
## <a name="limitations-and-restrictions"></a>Ограничения  
 Вы не можете определить ограничение DEFAULT для столбца распределения.  
  
### <a name="partitions"></a>Секции
При использовании секций в столбце секции не могут использоваться параметры сортировки только для Юникода. Например следующая инструкция завершится с ошибкой.  
  
 ```sql
CREATE TABLE t1 ( c1 varchar(20) COLLATE Divehi_90_CI_AS_KS_WS) WITH (PARTITION (c1 RANGE FOR VALUES (N'')))
```  
 
 Если *граничное_значение* представляет собой литеральное значение, которое должно быть неявно преобразовано в тип данных *столбца_секции*, возникнет несоответствие. Литеральное значение отображается с помощью системных представлений [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], но преобразованное значение используется для операций [!INCLUDE[tsql](../../includes/tsql-md.md)]. 

### <a name="temporary-tables"></a>Временные таблицы

 Глобальные временные таблицы, которые начинаются с ##, не поддерживаются.  
  
 На локальные временные таблицы распространяются следующие ограничения:  
  
-   Они видимы только в текущем сеансе. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] удаляет их автоматически в конце сеанса. Для явного удаления этих таблиц используйте инструкцию DROP TABLE.   
-   Их нельзя переименовать. 
-   Они не могут иметь секции или представления.  
-   Изменить их разрешения невозможно. Инструкции `GRANT`, `DENY` и `REVOKE` не могут использоваться с локальными временными таблицами.   
-   Консольные команды базы данных для временных таблиц блокируются.   
-   Если в пакете используется несколько локальных временных таблиц, они должны иметь уникальные имена. Если в нескольких сеансах запускается один и тот же пакет и создается одна и та же локальная временная таблица, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] добавляет числовой суффикс к имени локальной временной таблицы, чтобы сохранить уникальные имена локальных временных таблиц.  
    
<a name="LockingBehavior"></a>   
## <a name="locking-behavior"></a>Режим блокировки  
 Применяет монопольную блокировку к таблице. Применяет совмещаемую блокировку к объектам DATABASE, SCHEMA и SCHEMARESOLUTION.  
 

<a name="ExamplesColumn"></a>   
## <a name="examples-for-columns"></a>Примеры для столбцов

### <a name="a-specify-a-column-collation"></a><a name="ColumnCollation"></a> A. Указание параметров сортировки столбца 
 В следующем примере создается таблица `MyTable` с двумя различными параметрами сортировки столбцов. По умолчанию столбец `mycolumn1` имеет параметры сортировки по умолчанию Latin1_General_100_CI_AS_KS_WS. Столбец `mycolumn2` имеет параметры сортировки Frisian_100_CS_AS.  
  
```sql
CREATE TABLE MyTable   
  (  
    mycolumnnn1 nvarchar,  
    mycolumn2 nvarchar COLLATE Frisian_100_CS_AS )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```  
  
### <a name="b-specify-a-default-constraint-for-a-column"></a><a name="DefaultConstraint"></a> Б. Указания ограничения DEFAULT для столбца

 В следующем примере показан синтаксис для указания значения по умолчанию для столбца. Столбец colA имеет ограничение по умолчанию constraint_colA и значение по умолчанию 0.  
  
```sql
CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```

<a name="ExamplesTemporaryTables"></a> 
## <a name="examples-for-temporary-tables"></a>Примеры для временных таблиц

### <a name="c-create-a-local-temporary-table"></a><a name="TemporaryTable"></a> В. Создание локальной временной таблицы  
 В следующем примере создается локальная временная таблица с именем #myTable. Имя таблицы состоит из трех частей и начинается с #.
  
```sql
CREATE TABLE AdventureWorks.dbo.#myTable
  (  
   id int NOT NULL,  
   lastName varchar(20),  
   zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```

<a name="ExTableStructure"></a>  
## <a name="examples-for-table-structure"></a>Примеры структуры таблиц

### <a name="d-create-a-table-with-a-clustered-columnstore-index"></a><a name="ClusteredColumnstoreIndex"></a> Г. Создание таблицы с кластеризованным индексом columnstore  
 В следующем примере создается таблица распределения с кластеризованным индексом columnstore. Каждое распределение будет храниться в виде кластеризованного индекса columnstore.  
  
 Кластеризованный индекс columnstore не влияет на способ распределения данных; данные всегда распределены по строкам. Кластеризованный индекс влияет на способ хранения данных в пределах каждого распределения.  
  
```sql
  CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH   
  (   
    DISTRIBUTION = HASH ( colB ),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  

### <a name="e-create-an-ordered-clustered-columnstore-index"></a><a name="OrderedClusteredColumnstoreIndex"></a> Д. Создание упорядоченного кластеризованного индекса columnstore

В следующем примере показано, как создать упорядоченный кластеризованный индекс columnstore. Индекс упорядочен по SHIPDATE.

```sql
CREATE TABLE Lineitem  
WITH (DISTRIBUTION = ROUND_ROBIN, CLUSTERED COLUMNSTORE INDEX ORDER(SHIPDATE))  
AS  
SELECT * FROM ext_Lineitem
```

<a name="ExTableDistribution"></a> 
## <a name="examples-for-table-distribution"></a>Примеры распределения таблиц

### <a name="f-create-a-round_robin-table"></a><a name="RoundRobin"></a> Е. Создание таблицы ROUND_ROBIN  
 В следующем примере создается таблица ROUND_ROBIN с тремя столбцами и без секций. Данные распространяются между всеми распределениями. Создается таблица с кластеризованным индексом columnstore, который обладает лучшей производительностью и характеристиками сжатия данных по сравнению с кучей или кластеризованным индексом rowstore.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );  
```  
  
### <a name="g-create-a-hash-distributed-table"></a><a name="HashDistributed"></a> G. Создание таблицы с распределением хэша

 В следующем примере создается точно такая же таблица, как в предыдущем примере. Тем не менее для этой таблицы строки распределены (по столбцу `id`) вместо случайного распределения, как в таблице ROUND_ROBIN. Создается таблица с кластеризованным индексом columnstore, который обладает лучшей производительностью и характеристиками сжатия данных по сравнению с кучей или кластеризованным индексом rowstore.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),   
    CLUSTERED COLUMNSTORE INDEX  
  );  
```  
  
### <a name="h-create-a-replicated-table"></a><a name="Replicated"></a> H. Создание реплицированной таблицы  
 В следующем примере создается реплицированная таблица, как и в предыдущем примере. Реплицированные таблицы копируются в полном объеме на каждый вычислительный узел. Благодаря наличию копии на каждом вычислительном узле уменьшается объем перемещаемых данных для запросов. В этом примере таблица создается с использованием кластеризованного индекса, который обеспечивает лучшее сжатие данных, чем куча. Куча может не содержать достаточно записей для достижения хорошего сжатия с использованием кластеризованного индекса columnstore.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = REPLICATE,
    CLUSTERED INDEX (lastName)  
  );  
```  

<a name="ExTablePartitions"></a> 
## <a name="examples-for-table-partitions"></a>Примеры секций таблиц

###  <a name="i-create-a-partitioned-table"></a><a name="PartitionedTable"></a> I. Создание секционированной таблицы

 В следующем примере создается такая же таблица, как в примере A, с добавлением секционирования RANGE LEFT для столбца `id`. В нем указаны четыре граничных значения секций, таким образом, общее количество секций равно пяти.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
  (
  
    PARTITION ( id RANGE LEFT FOR VALUES (10, 20, 30, 40 )),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  
  
 В этом примере данные будут отсортированы в следующих секциях:  
  
- Секция 1: столбцы до 10-го включительно
- Секция 2: столбцы с 11-го по 20-й
- Секция 3: столбцы с 21-го по 30-й
- Секция 4: столбцы с 31-го по 40-й
- Секция 5: столбцы с 41-го и далее  
  
 Если эта же таблица была секционирована с использованием RANGE RIGHT вместо RANGE LEFT (по умолчанию), данные будут отсортированы в следующих секциях:  
  
- Секция 1: столбцы до 10-го  
- Секция 2: столбцы с 10-го по 19-й
- Секция 3: столбцы с 20-го по 29-й
- Секция 4: столбцы с 30-го по 39-й
- Секция 5: столбцы с 40-го и далее  
  
### <a name="j-create-a-partitioned-table-with-one-partition"></a><a name="OnePartition"></a> J. Создание секционированной таблицы с одной секцией

 В следующем примере создается секционированная таблица с одной секцией. В нем не указаны граничные значения, поэтому создается одна секция.  
  
```sql
CREATE TABLE myTable (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
    (
      PARTITION ( id RANGE LEFT FOR VALUES ( )),  
      CLUSTERED COLUMNSTORE INDEX  
    )  
;  
```  
  
### <a name="k-create-a-table-with-date-partitioning"></a><a name="DatePartition"></a> K. Создание таблицы с секционированием даты

 В следующем примере создается новая таблица с именем `myTable` с секционированием по столбцу `date`. При использовании RANGE RIGHT и дат в качестве граничных значений в каждой секции будут находиться данные для одного месяца.  
  
```sql
CREATE TABLE myTable (  
    l_orderkey      bigint,
    l_partkey       bigint,
    l_suppkey       bigint,
    l_linenumber    bigint,
    l_quantity      decimal(15,2),  
    l_extendedprice decimal(15,2),  
    l_discount      decimal(15,2),  
    l_tax           decimal(15,2),  
    l_returnflag    char(1),  
    l_linestatus    char(1),  
    l_shipdate      date,  
    l_commitdate    date,  
    l_receiptdate   date,  
    l_shipinstruct  char(25),  
    l_shipmode      char(10),  
    l_comment       varchar(44))  
WITH
  (
    DISTRIBUTION = HASH (l_orderkey),  
    CLUSTERED COLUMNSTORE INDEX,  
    PARTITION ( l_shipdate  RANGE RIGHT FOR VALUES
      (  
        '1992-01-01','1992-02-01','1992-03-01','1992-04-01','1992-05-01',
        '1992-06-01','1992-07-01','1992-08-01','1992-09-01','1992-10-01',
        '1992-11-01','1992-12-01','1993-01-01','1993-02-01','1993-03-01',
        '1993-04-01','1993-05-01','1993-06-01','1993-07-01','1993-08-01',
        '1993-09-01','1993-10-01','1993-11-01','1993-12-01','1994-01-01',
        '1994-02-01','1994-03-01','1994-04-01','1994-05-01','1994-06-01',
        '1994-07-01','1994-08-01','1994-09-01','1994-10-01','1994-11-01',
        '1994-12-01'  
      ))
  );  
```  
  
<a name="SeeAlso"></a>
## <a name="see-also"></a>См. также раздел
 
[CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
[DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)   
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
[sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) 
