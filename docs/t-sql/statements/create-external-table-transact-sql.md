---
description: CREATE EXTERNAL TABLE (Transact-SQL)
title: CREATE EXTERNAL TABLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_EXTERNAL_TABLE
- CREATE EXTERNAL TABLE
- PolyBase, T-SQL
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, table create
- PolyBase, external table
ms.assetid: 6a6fd8fe-73f5-4639-9908-2279031abdec
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2595db017ce9572f6ceb7a4c7eca16f9db9e5abc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095802"
---
# <a name="create-external-table-transact-sql"></a>CREATE EXTERNAL TABLE (Transact-SQL)

Создание внешней таблицы

Эта статья приводит синтаксис, аргументы, комментарии, разрешения и примеры для любых выбранных продуктов SQL.

Дополнительные сведения о соглашениях о синтаксисе см. в статье [Соглашения о синтаксисе в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [База данных SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Общие сведения. SQL Server

Эта команда создает внешнюю таблицу PolyBase для доступа к данным, хранящимся в кластере Hadoop или хранилище BLOB-объектов Azure. Внешняя таблица PolyBase ссылается на данные, хранящиеся в кластере Hadoop или хранилище BLOB-объектов Azure.

**Область применения**: SQL Server 2016 или более поздней версии

Используйте внешнюю таблицу с внешним источником данных для запросов PolyBase. Внешние источники данных используются для обеспечения взаимодействия и поддерживают следующие основные варианты использования.

- Виртуализация и загрузка данных с помощью [PolyBase](../../relational-databases/polybase/polybase-guide.md)
- Операции массовой загрузки в SQL Server или базе данных SQL с помощью `BULK INSERT` или `OPENROWSET`

См. также [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) и [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Create a new external table
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )
    WITH (
        LOCATION = 'folder_or_filepath',
        DATA_SOURCE = external_data_source_name,
        FILE_FORMAT = external_file_format_name
        [ , <reject_options> [ ,...n ] ]
    )
[;]

<reject_options> ::=
{
    | REJECT_TYPE = value | percentage
    | REJECT_VALUE = reject_value
    | REJECT_SAMPLE_VALUE = reject_sample_value
}

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
```

## <a name="arguments"></a>Аргументы

*{им_базы.имя_схемы.имя_таблицы | имя_схемы.имя_таблицы | имя_таблицы}*  Одно-, двух- или трехсоставное имя для создаваемой таблицы. Если речь идет о внешней таблице, в SQL хранятся только метаданные таблицы, а также базовая статистика о файле или папке, на которые ссылается Hadoop и хранилище больших двоичных объектов Azure. Никакие данные не перемещаются и не хранятся в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!IMPORTANT]
> Для обеспечения наилучшей производительности, если драйвер внешнего источника данных поддерживает трехкомпонентное имя, настоятельно рекомендуется указать такое имя.  

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE поддерживает возможность настроить имя столбца, тип данных, допустимость значений NULL и параметры сортировки. Параметр DEFAULT CONSTRAINT нельзя использовать с внешними таблицами.

Определения столбцов, включая типы данных и количество столбцов, должны соответствовать данным во внешних файлах. В случае несоответствия при запросе данных строки файла будут отклонены.

LOCATION = '*путь к файлу или папке*'. Указывает путь к папке или файлу и имя файла для фактических данных в хранилище больших двоичных объектов Azure или Hadoop. Расположение начинается с корневой папки. Корневая папка — это расположение данных, указанное во внешнем источнике данных.

В SQL Server инструкция CREATE EXTERNAL TABLE создает путь к папке и саму папку, если она еще не существует. Затем вы можете использовать инструкцию INSERT INTO, чтобы экспортировать данные из локальной таблицы SQL Server во внешний источник данных. Дополнительные сведения: [Запросы PolyBase](../../relational-databases/polybase/polybase-queries.md).

Если вы укажете LOCATION в качестве папки, запрос PolyBase, который выбирает из внешней таблицы, извлечет файлы из этой папки и всех ее вложенных папок. Как и Hadoop, PolyBase не возвращает скрытые папки. Кроме того, не возвращаются файлы, имя которых начинается с подчеркивания (_) или точки (.).

В этом примере, если указано LOCATION='/webdata/', запрос PolyBase вернет строки из mydata.txt и mydata2.txt. Он не вернет файл mydata3.txt, который вложен в скрытую папку. Он также не вернет файл _hidden.txt, так как тот является скрытым.

![Рекурсивные данные для внешних таблиц](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Рекурсивные данные для внешних таблиц")

Чтобы изменить значение по умолчанию и только для чтения в корневой папке, установите для атрибута \<polybase.recursive.traversal> значение false в файле конфигурации core-site.xml. Этот файл находится в папке `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Например, `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *имя внешнего источника данных*. Задает имя внешнего источника данных, содержащего расположение внешних данных. Этим расположением является файловая система Hadoop (HDFS), контейнер больших двоичных объектов службы хранилища Azure или Azure Data Lake Store. Для создания внешнего источника данных используйте инструкцию [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *имя формата внешнего файла*. Задает имя объекта формата внешнего файла, который хранит тип файла и метод сжатия внешних данных. Чтобы создать формат внешнего файла, используйте [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Параметры отклонения. Можно указать параметры отклонения, определяющие, как PolyBase будет обрабатывать *грязные* записи, извлеченные из внешнего источника данных. Запись считается "грязной", если фактический тип данных или количество столбцов не совпадают с определениями столбцов во внешней таблице.

Если вы не указываете или не меняете значения отклонения, PolyBase использует значения по умолчанию. Сведения о параметрах отклонения сохраняются как дополнительные метаданные при создании внешней таблицы с помощью инструкции CREATE EXTERNAL TABLE. При последующем выполнении инструкции SELECT или SELECT INTO SELECT для выбора данных из внешней таблицы PolyBase будет использовать параметры отклонения для определения числа или процента строк, которые можно отклонить, прежде чем запрос завершится ошибкой. Запрос будет возвращать (частичные) результаты, пока не будет превышено пороговое значение отклонения. Затем он выдаст соответствующее сообщение об ошибке.

REJECT_TYPE = **value** | percentage. Уточняет, указан параметр REJECT_VALUE как литеральное значение или процент.

Значение value — REJECT_VALUE указано в виде литерала, а не в процентах. Запрос PolyBase завершится ошибкой, если число отклоненных строк превышает *reject_value*.

Например, если задано REJECT_VALUE = 5 и REJECT_TYPE = value, запрос PolyBase SELECT завершится ошибкой после отклонения пяти строк.

Значение percentage — REJECT_VALUE указывается в процентах, а не в виде литерала. Запрос PolyBase завершится ошибкой, когда *процент* отклоненных строк превысит *reject_value*. Процент отклоненных строк вычисляется с интервалами.

REJECT_VALUE = *значение отклонения*. Задает значение или процент строк, которые можно отклонить, прежде чем запрос завершится ошибкой.

Если задано REJECT_TYPE = value, значение *reject_value* должно быть целым числом от 0 до 2 147 483 647.

Если задано REJECT_TYPE = percentage, значение *reject_value* должно быть числом с плавающей точкой от 0 до 100.

REJECT_SAMPLE_VALUE = *значение отклоняемого образца*. Этот атрибут является обязательным, если задано REJECT_TYPE = percentage. Он определяет количество строк, которое запрос попытается извлечь, прежде чем PolyBase пересчитает процент отклоненных строк.

Параметр *reject_sample_value* должен быть целым числом от 0 до 2 147 483 647.

Например, если задано REJECT_SAMPLE_VALUE = 1000, PolyBase рассчитает процент отклоненных строк после попытки импортировать 1000 строк из внешнего файла данных. Если процент отклоненных строк меньше значения *reject_value*, PolyBase попытается извлечь следующие 1000 строк. Он продолжает повторно вычислять процент отклоненных строк после каждой попытки извлечения 1000 строк.

> [!NOTE]
> Поскольку PolyBase вычисляет процент отклоненных строк с интервалами, фактический процент отклоненных строк может превысить *reject_value*.

Пример

В этом примере показано, как три параметра REJECT взаимодействуют друг с другом. Например, если задано REJECT_TYPE = percentage, REJECT_VALUE = 30 и REJECT_SAMPLE_VALUE = 100, произойдет следующее:

- PolyBase попытается извлечь первые 100 строк; из них 25 отклонено, а 75 — извлечено успешно.
- Процент строк с ошибками равен 25 %, что меньше значения отклонения — 30 %. Поэтому PolyBase продолжит извлечение данных из внешнего источника.
- PolyBase пытается загрузить следующие 100 строк. На этот раз 25 строк извлечено успешно и 75 — отклонено.
- Процент отклоненных строк пересчитывается и составляет 50 %. Процент отклоненных строк превысил значение отклонения 30 %.
- Запрос PolyBase завершается ошибкой, поскольку после попытки извлечение первых 200 строк 50 % из них было отклонено. Обратите внимание, что до того, как запрос PolyBase выявил превышение порога отклонения, были возвращены соответствующие строки.

SCHEMA_NAME. Предложение SCHEMA_NAME позволяет сопоставить определение внешней таблицы с таблицей в другой схеме в удаленной базе данных. Это предложение используется для устранения неоднозначности между схемами, которые существуют одновременно в локальных и удаленных базах данных.

OBJECT_NAME. Предложение OBJECT_NAME позволяет сопоставить определение внешней таблицы с таблицей с другим именем в удаленной базе данных. Это предложение используется для устранения неоднозначности между именами объектов, которые существуют одновременно в локальных и удаленных базах данных.

DISTRIBUTION. Необязательный параметр. Он необходим только для баз данных типа SHARD_MAP_MANAGER. Этот аргумент указывает, следует считать таблицу сегментированной или реплицированной. Если задан аргумент **SHARDED** (*имя столбца*), данные из разных таблиц не перекрываются. **REPLICATED** указывает, что таблицы содержат одинаковые данные в каждом сегменте. **ROUND_ROBIN** указывает, что для распределения данных используется метод конкретного приложения.

## <a name="permissions"></a>Разрешения

Требуются следующие разрешения:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Имя входа, которое создает внешний источник данных, должно иметь разрешение на чтение и запись во внешний источник данных, находящийся в хранилище больших двоичных объектов Azure или Hadoop.

> [!IMPORTANT]
> Разрешение ALTER ANY EXTERNAL DATA SOURCE предоставляет любому субъекту возможность создания и изменения объекта внешнего источника данных и, таким образом, также предоставляет возможность доступа ко всем учетным данным уровня базы данных в базе данных. Это разрешение следует рассматривать как высоко привилегированное, поэтому его следует предоставлять только доверенным субъектам в системе.

## <a name="error-handling"></a>Обработка ошибок

При выполнении инструкции CREATE EXTERNAL TABLE PolyBase пытается подключиться к внешнему источнику данных. Если попытка соединения завершается ошибкой, инструкция также завершается ошибкой и внешняя таблица не создается. Это может занять около минуты, поскольку PolyBase повторяет попытки соединения, прежде чем запрос будет завершен ошибкой.

## <a name="general-remarks"></a>Общие замечания

Если используются нерегламентированные запросы, например SELECT FROM EXTERNAL TABLE, PolyBase сохраняет строки, полученные из внешнего источника данных, во временной таблице. После выполнения запроса PolyBase удаляет временную таблицу. В таблицах SQL не сохраняются постоянные данные.

Если выполняется импорт, например SELECT INTO FROM EXTERNAL TABLE, PolyBase сохраняет строки, полученные из внешнего источника данных, в виде постоянных данных в таблице SQL. Новая таблица создается при выполнении запросов, когда PolyBase извлекает внешние данные.

PolyBase может передать некоторые вычисления запросов в Hadoop для повышения производительности запросов. Это действие называется включением предиката. Для его выполнения задайте параметр расположения диспетчера ресурсов Hadoop в [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Вы можете создать несколько внешних таблиц, которые ссылаются на один или разные внешние источники данных.

## <a name="limitations-and-restrictions"></a>Ограничения

Поскольку данные внешней таблицы не находятся под прямым контролем SQL Server, они могут быть в любое время изменены или удалены внешним процессом. По этой причине результаты запроса к внешней таблице необязательно будут детерминированными. Один и то же запрос может возвращать разные результаты при каждом обращении к внешней таблице. Аналогичным образом, запрос может завершиться ошибкой, если внешние данные удалены или перемещены.

Вы можете создать несколько внешних таблиц, которые ссылаются на разные внешние источники данных. Если же вы одновременно выполняете запросы к различным источникам данных Hadoop, каждый источник Hadoop должен иметь одинаковый параметр конфигурации сервера hadoop connectivity. Например, невозможно одновременно выполнить запрос к кластеру Cloudera Hadoop и кластеру Hortonworks Hadoop, поскольку они используют разные параметры конфигурации. Сведения о параметрах конфигурации и поддерживаемых сочетаниях см. в разделе [Конфигурация подключения к PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Только эти инструкции DDL допускаются для внешних таблиц:

- CREATE TABLE и DROP TABLE
- CREATE STATISTICS и DROP STATISTICS
- CREATE VIEW и DROP VIEW

Неподдерживаемые конструкции и операции:

- Ограничение DEFAULT для столбцов внешней таблицы
- Операции DML обновления, вставки и удаления

### <a name="query-limitations"></a>Ограничения запросов

PolyBase может обработать не более 33 тысяч файлов на папку при выполнении параллельных запросов PolyBase со степенью 32. Это максимальное число включает файлы и вложенные папки в каждой папке HDFS. Если степень параллелизма меньше 32, пользователь может выполнять запросы PolyBase к папкам в HDFS, если в них содержится более 33 тысяч файлов. Рекомендуется указывать короткие пути к внешним файлам и следить за тем, чтобы в каждой папке HDFS было не более 30 тысяч файлов. При ссылке на слишком большое число файлов может возникнуть исключение, связанное с нехваткой памяти на виртуальной машине Java.

### <a name="table-width-limitations"></a>Ограничения по ширине таблицы

В PolyBase в SQL Server 2016 ширина строки должна составлять не более 32 КБ в связи с максимальным размером допустимой строки в определении таблицы. Если схема суммы столбцов превышает 32 КБ, PolyBase не сможет запросить данные.

### <a name="data-type-limitations"></a>Ограничения типов данных

Во внешних таблицах PolyBase нельзя использовать следующие типы данных:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Все определяемые пользователем типы данных

## <a name="locking"></a>Блокировка

Общая блокировка на объект SCHEMARESOLUTION.

## <a name="security"></a>Безопасность

Файлы данных для внешней таблицы хранятся в хранилище больших двоичных объектов Azure или Hadoop. Эти файлы данных создаются и управляются вашими собственными процессами. В ваши обязанности входит управление безопасностью внешних данных.

## <a name="examples"></a>Примеры

### <a name="a-create-an-external-table-with-data-in-text-delimited-format"></a>A. Создайте внешнюю таблицу с данными с текстовыми разделителями.

В этом примере показаны все действия по созданию внешней таблицы с данными, отформатированными в виде файлов с текстовыми разделителями. В нем определяется внешний источник данных *mydatasource* и формат внешнего файла *myfileformat*. Затем эти объекты уровня базы данных указываются в инструкции CREATE EXTERNAL TABLE. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) и [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (FIELD_TERMINATOR ='|')
);

CREATE EXTERNAL TABLE ClickStream (
    url varchar(50),
    event_date date,
    user_IP varchar(50)
)
WITH (
        LOCATION='/webdata/employee.tbl',
        DATA_SOURCE = mydatasource,
        FILE_FORMAT = myfileformat
    )
;
```

### <a name="b-create-an-external-table-with-data-in-rcfile-format"></a>Б. Создайте внешнюю таблицу с данными в формате RCFile.

В этом примере показаны все действия по созданию внешней таблицы с данными, отформатированными в виде файлов RCFile. В нем определяется внешний источник данных *mydatasource_rc* и формат внешнего файла *myfileformat_rc*. Затем эти объекты уровня базы данных указываются в инструкции CREATE EXTERNAL TABLE. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) и [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_rc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_rc
WITH (
    FORMAT_TYPE = RCFILE,
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'
)
;

CREATE EXTERNAL TABLE ClickStream_rc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/employee_rc.tbl',
        DATA_SOURCE = mydatasource_rc,
        FILE_FORMAT = myfileformat_rc
    )
;
```

### <a name="c-create-an-external-table-with-data-in-orc-format"></a>В. Создайте внешнюю таблицу с данными в формате ORC.

В этом примере показаны все действия по созданию внешней таблицы с данными, отформатированными в виде файлов ORC. В нем определяется внешний источник данных mydatasource_orc и формат внешнего файла myfileforma_orc. Затем эти объекты уровня базы данных указываются в инструкции CREATE EXTERNAL TABLE. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) и [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_orc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_orc
WITH (
    FORMAT = ORC,
    COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
;

CREATE EXTERNAL TABLE ClickStream_orc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/',
        DATA_SOURCE = mydatasource_orc,
        FILE_FORMAT = myfileformat_orc
    )
;
```

### <a name="d-querying-hadoop-data"></a>Г. Запрос данных Hadoop

Clickstream — это внешняя таблица, соединенная с текстовым файлом с разделителями employee.tbl в кластере Hadoop. Следующий запрос выглядит так же, как запрос к стандартной таблице. Однако этот запрос извлекает данные из Hadoop, а затем вычисляет результаты.

```sql
SELECT TOP 10 (url) FROM ClickStream WHERE user_ip = 'xxx.xxx.xxx.xxx'
;
```

### <a name="e-join-hadoop-data-with-sql-data"></a>Д. Объединение данных Hadoop с данными SQL

Этот запрос выглядит так же, как стандартный запрос JOIN для двух таблиц SQL. Разница в том, что PolyBase получает данные таблицы Clickstream из Hadoop, а затем объединяет их с таблицей UrlDescription. Одна таблица является внешней, а другая — стандартной таблицей SQL.

```sql
SELECT url.description
FROM ClickStream cs
JOIN UrlDescription url ON cs.url = url.name
WHERE cs.url = 'msdn.microsoft.com'
;
```

### <a name="f-import-data-from-hadoop-into-a-sql-table"></a>Е. Импорт данных из Hadoop в таблицу SQL

В этом примере создается новая таблица SQL — ms_user, в которой постоянно хранятся результаты объединения стандартной таблицы SQL *user* и внешней таблицы *ClickStream*.

```sql
SELECT DISTINCT user.FirstName, user.LastName
INTO ms_user
FROM user INNER JOIN (
    SELECT * FROM ClickStream WHERE cs.url = 'www.microsoft.com'
    ) AS ms
ON user.user_ip = ms.user_ip
;
```

### <a name="g-create-an-external-table-for-a-sharded-data-source"></a>Ж. Создание внешней таблицы для сегментированного источника данных

В этом примере удаленное динамическое административное представление сопоставляется с внешней таблицей с помощью предложений SCHEMA_NAME и OBJECT_NAME.

```sql
CREATE EXTERNAL TABLE [dbo].[all_dm_exec_requests]([session_id] smallint NOT NULL,
  [request_id] int NOT NULL,
  [start_time] datetime NOT NULL,
  [status] nvarchar(30) NOT NULL,
  [command] nvarchar(32) NOT NULL,
  [sql_handle] varbinary(64),
  [statement_start_offset] int,
  [statement_end_offset] int,
  [cpu_time] int NOT NULL)
WITH
(
  DATA_SOURCE = MyExtSrc,
  SCHEMA_NAME = 'sys',
  OBJECT_NAME = 'dm_exec_requests',
  DISTRIBUTION=ROUND_ROBIN
);
```

### <a name="h-create-an-external-table-for-sql-server"></a>З. Создание внешней таблицы для SQL Server

```sql
     -- Create a Master Key
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';
    GO
     /*  specify credentials to external data source
     *  IDENTITY: user name for external source.
     *  SECRET: password for external source.
     */
     CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
     WITH IDENTITY = 'username', Secret = 'password';
    GO

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
    WITH (
    LOCATION = 'sqlserver://SqlServer',
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = SQLServerCredentials
    );
    GO

    CREATE SCHEMA sqlserver;
    GO

     /* LOCATION: sql server table/view in 'database_name.schema_name.object_name' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE sqlserver.customer(
     C_CUSTKEY INT NOT NULL,
     C_NAME VARCHAR(25) NOT NULL,
     C_ADDRESS VARCHAR(40) NOT NULL,
     C_NATIONKEY INT NOT NULL,
     C_PHONE CHAR(15) NOT NULL,
     C_ACCTBAL DECIMAL(15,2) NOT NULL,
     C_MKTSEGMENT CHAR(10) NOT NULL,
     C_COMMENT VARCHAR(117) NOT NULL
      )
      WITH (
      LOCATION='tpch_10.dbo.customer',
      DATA_SOURCE=SqlServerInstance
     );
```

### <a name="i-create-an-external-table-for-oracle"></a>И. Создание внешней таблицы для Oracle

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

   /*
   * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
   * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
   * CONNECTION_OPTIONS: Specify driver location
   * CREDENTIAL: the database scoped credential, created above.
   */
   CREATE EXTERNAL DATA SOURCE external_data_source_name
   WITH (
     LOCATION = 'oracle://<server address>[:<port>]',
     -- PUSHDOWN = ON | OFF,
     CREDENTIAL = credential_name)

   /*
   * LOCATION: Oracle table/view in '.<schema_name>.<object_name>' format
   * DATA_SOURCE: the external data source, created above.
   */
   CREATE EXTERNAL TABLE customers(
   [O_ORDERKEY] DECIMAL(38) NOT NULL,
   [O_CUSTKEY] DECIMAL(38) NOT NULL,
   [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
   [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
   [O_ORDERDATE] DATETIME2(0) NOT NULL,
   [O_ORDERPRIORITY] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_CLERK] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_SHIPPRIORITY] DECIMAL(38) NOT NULL,
   [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
   )
   WITH (
    LOCATION='.mySchema.customer',
    DATA_SOURCE= external_data_source_name
   );
```

### <a name="j-create-an-external-table-for-teradata"></a>К. Создание внешней таблицы для Teradata

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = teradata://<server address>[:<port>],
   -- PUSHDOWN = ON | OFF,
    CREDENTIAL =credential_name
    );


     /* LOCATION: Teradata table/view in '<database_name>.<object_name>' format
      * DATA_SOURCE: the external data source, created above.
      */
     CREATE EXTERNAL TABLE customer(
      L_ORDERKEY INT NOT NULL,
      L_PARTKEY INT NOT NULL,
     L_SUPPKEY INT NOT NULL,
     L_LINENUMBER INT NOT NULL,
     L_QUANTITY DECIMAL(15,2) NOT NULL,
     L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL,
     L_DISCOUNT DECIMAL(15,2) NOT NULL,
     L_TAX DECIMAL(15,2) NOT NULL,
     L_RETURNFLAG CHAR NOT NULL,
     L_LINESTATUS CHAR NOT NULL,
     L_SHIPDATE DATE NOT NULL,
     L_COMMITDATE DATE NOT NULL,
     L_RECEIPTDATE DATE NOT NULL,
     L_SHIPINSTRUCT CHAR(25) NOT NULL,
     L_SHIPMODE CHAR(10) NOT NULL,
     L_COMMENT VARCHAR(44) NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

### <a name="k-create-an-external-table-for-mongodb"></a>Л. Создание внешней таблицы для MongoDB

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

     /* LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = mongodb://<server>[:<port>],
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = credential_name
    );

     /* LOCATION: MongoDB table/view in '<database_name>.<schema_name>.<object_name>' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE customers(
     [O_ORDERKEY] DECIMAL(38) NOT NULL,
     [O_CUSTKEY] DECIMAL(38) NOT NULL,
     [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
     [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
     [O_ORDERDATE] DATETIME2(0) NOT NULL,
     [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

## <a name="see-also"></a>См. также:

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* База данных SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>Общие сведения. База данных SQL Azure

В Базе данных SQL Azure создает внешнюю таблицу для [эластичных запросов (предварительная версия)](/azure/sql-database/sql-database-elastic-query-overview/).


См. также [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Create a table for use with elastic query  
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH ( <sharded_external_table_options> )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<sharded_external_table_options> ::=  
        DATA_SOURCE = external_data_source_name,
        SCHEMA_NAME = N'nonescaped_schema_name',  
        OBJECT_NAME = N'nonescaped_object_name',  
        [DISTRIBUTION  = SHARDED(sharding_column_name) | REPLICATED | ROUND_ROBIN]]  
    )  
[;]  
```

## <a name="arguments"></a>Аргументы

*{им_базы.имя_схемы.имя_таблицы | имя_схемы.имя_таблицы | имя_таблицы}*  Одно-, двух- или трехсоставное имя для создаваемой таблицы. Если речь идет о внешней таблице, в SQL хранятся только метаданные таблицы, а также базовая статистика о файле или папке, на которые ссылается база данных SQL Azure. Никакие данные не перемещаются и не хранятся в базе данных SQL Azure.

> [!IMPORTANT]
> Для обеспечения наилучшей производительности, если драйвер внешнего источника данных поддерживает трехкомпонентное имя, настоятельно рекомендуется указать такое имя.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE поддерживает возможность настроить имя столбца, тип данных, допустимость значений NULL и параметры сортировки. Параметр DEFAULT CONSTRAINT нельзя использовать с внешними таблицами.

> [!NOTE]
> Типы данных `Text`, `nText` и `XML` не поддерживаются для столбцов во внешних таблицах базы данных SQL Microsoft Azure.

Определения столбцов, включая типы данных и количество столбцов, должны соответствовать данным во внешних файлах. В случае несоответствия при запросе данных строки файла будут отклонены.

Параметры сегментированных внешних таблиц

Указывает внешний источник данных (не источник данных SQL Server) и метод распределения для [эластичного запроса](/azure/azure-sql/database/elastic-query-overview).

DATA_SOURCE Предложение DATA_SOURCE определяет внешний источник данных (карту сегментов) для внешней таблицы. Пример см. в разделе [Создание внешних таблиц](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

SCHEMA_NAME и OBJECT_NAME Предложения SCHEMA_NAME и OBJECT_NAME сопоставляют определение внешней таблицы с таблицей в другой схеме. Если эти предложения отсутствуют, то предполагается, что удаленный объект является таблицей "dbo", имя которой совпадает с именем внешней таблицы. Это особенно необходимо, если имя удаленной таблицы уже занято в базе данных, где создается внешняя таблица. Например, вы хотите определить внешнюю таблицу для получения общего представления из представлений каталогов или динамических административных представлений, которые находятся на вашем развернутом уровне данных. Так как представления каталогов и динамические административные представления уже существуют локально, их имена нельзя использовать для определения внешней таблицы. Вместо этого вы можете задать другое имя и в предложениях SCHEMA_NAME и OBJECT_NAME использовать имя представления каталога или динамического административного представления. Пример см. в разделе [Создание внешних таблиц](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

DISTRIBUTION Предложение DISTRIBUTION определяет распределение данных, используемое для этой таблицы. Обработчик запросов использует сведения, указанные в предложении DISTRIBUTION, для создания наиболее эффективных планов запросов.

- SHARDED означает, что данные секционируются по базе данных горизонтально. Ключ секционирования для распределения данных указывается в параметре <sharding_column_name>.
- REPLICATED означает, что в каждой базе данных имеются идентичные копии таблицы. Вы должны самостоятельно позаботиться о соответствии реплик во всех базах данных.
- ROUND_ROBIN означает, что таблица секционируется горизонтально с применением метода распределения в зависимости от приложений.

## <a name="permissions"></a>Разрешения

Пользователи, имеющие доступ к внешней таблице, автоматически получают доступ к базовой удаленной таблице с учетными данными, указанными в определении внешнего источника данных. Старайтесь избегать нежелательного повышения прав с использованием учетных данных для внешнего источника данных. Методы GRANT или REVOKE применяются к внешней таблице так же, как и к обычной. Определив внешний источник данных и внешние таблицы, вы можете использовать все возможности T-SQL для создания запросов к внешним таблицам.

## <a name="error-handling"></a>Обработка ошибок

Если при выполнении инструкции CREATE EXTERNAL TABLE попытка соединения завершается ошибкой, инструкция также завершается ошибкой и внешняя таблица не создается. Это может занять около минуты, поскольку база данных SQL повторяет попытки соединения, прежде чем запрос будет завершен ошибкой.

## <a name="general-remarks"></a>Общие замечания

Если используются нерегламентированные запросы, например SELECT FROM EXTERNAL TABLE, база данных SQL сохраняет строки, полученные из внешнего источника данных, во временной таблице. После выполнения запроса база данных SQL удаляет временную таблицу. В таблицах SQL не сохраняются постоянные данные.

Если выполняется импорт, например SELECT INTO FROM EXTERNAL TABLE, база данных SQL сохраняет строки, полученные из внешнего источника данных, в виде постоянных данных в таблице SQL. Новая таблица создается при выполнении запросов, когда база данных SQL извлекает внешние данные.

Вы можете создать несколько внешних таблиц, которые ссылаются на один или разные внешние источники данных.

## <a name="limitations-and-restrictions"></a>Ограничения

При доступе к данным через внешнюю таблицу семантика изоляции в SQL Server не соблюдается. Это означает, что при запросах к внешней таблице блокировка или изоляция моментальных снимков не применяется, поэтому возвращаемые данные могут меняться при изменении данных во внешнем источнике данных.  Один и то же запрос может возвращать разные результаты при каждом обращении к внешней таблице. Аналогичным образом, запрос может завершиться ошибкой, если внешние данные удалены или перемещены.

Вы можете создать несколько внешних таблиц, которые ссылаются на разные внешние источники данных.

Только эти инструкции DDL допускаются для внешних таблиц:

- CREATE TABLE и DROP TABLE
- CREATE VIEW и DROP VIEW

Неподдерживаемые конструкции и операции:

- Ограничение DEFAULT для столбцов внешней таблицы
- Операции DML обновления, вставки и удаления

Во внешний источник данных могут быть переданы только литеральные предикаты, определенные в запросе. В отличие от этого при доступе к связанным серверам могут использоваться предикаты, определенные во время выполнения запроса, то есть используемые в сочетании с вложенным циклом в плане запроса. Это часто приводит к тому, что создается локальная копия всей внешней таблицы, после чего к ней производится присоединение.

```sql
  \\ Assuming External.Orders is an external table and Customer is a local table.
  \\ This query  will copy the whole of the external locally as the predicate needed
  \\ to filter isn't known at compile time. Its only known during execution of the query
  
  SELECT Orders.OrderId, Orders.OrderTotal
    FROM External.Orders
   WHERE CustomerId in (SELECT TOP 1 CustomerId
                          FROM Customer
                          WHERE CustomerName = 'MyCompany')
```

Использование внешних таблиц не позволяет реализовывать параллелизм в плане запроса.

Внешние таблицы реализуются как удаленный запрос, поэтому предполагаемое количество возвращаемых строк обычно равно 1000. Существуют и другие правила на основе типа предиката, используемые для фильтрации внешней таблицы. Они представляют собой оценки на основе правил, а не на основе фактических данных во внешней таблице. Оптимизатор не обращается к удаленному источнику данных для получения более точной оценки.

### <a name="data-type-limitations"></a>Ограничения типов данных

Во внешних таблицах PolyBase нельзя использовать следующие типы данных:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Все определяемые пользователем типы данных

## <a name="locking"></a>Блокировка

Общая блокировка на объект SCHEMARESOLUTION.

## <a name="examples"></a>Примеры

### <a name="a-create-external-table-for-azure-sql-database"></a>A. Создание внешней таблицы для базы данных SQL Azure

```sql
CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
( [CustomerID] [int] NOT NULL,
  [CustomerName] [varchar](50) NOT NULL,
  [Company] [varchar](50) NOT NULL)
WITH
( DATA_SOURCE = MyElasticDBQueryDataSrc)
```

## <a name="see-also"></a>См. также:

- [Обзор эластичных запросов к базе данных SQL Azure](/azure/sql-database/sql-database-elastic-query-overview)
- [Отчеты по масштабируемым облачным базам данных (предварительная версия)](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning)
- [Приступая к работе с межбазовыми запросами (вертикальное секционирование)](/azure/sql-database/sql-database-elastic-query-getting-started-vertical)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Общие сведения. Azure Synapse Analytics

Внешняя таблица используется в следующих целях:

- Запрос к Hadoop или хранилищу больших двоичных объектов Azure с помощью инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Импорт и хранение данных из Hadoop или хранилища больших двоичных объектов Azure.
- Импорт и хранение данных из хранилища Azure Data Lake Store.

См. также [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) и [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).  

## <a name="syntax"></a>Синтаксис

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
    | REJECTED_ROW_LOCATION = '/REJECT_Directory'
}  
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    ( <column_definition> [ ,...n ] )   
    WITH ( 
        LOCATION = 'folder_or_filepath',   
        DATA_SOURCE = external_data_source_name,   
        FILE_FORMAT = external_file_format_name   
    )   
[;]   
<column_definition> ::= 
column_name <data_type> 
    [ COLLATE collation_name ] 
```
---

## <a name="arguments"></a>Аргументы

*{им_базы.имя_схемы.имя_таблицы | имя_схемы.имя_таблицы | имя_таблицы}*  Одно-, двух- или трехсоставное имя для создаваемой таблицы. Если речь идет о внешней таблице, хранятся только метаданные таблицы, а также базовая статистика о файле или папке, на которые ссылается Azure Data Lake, Hadoop и хранилище больших двоичных объектов Azure. При создании внешних таблиц фактические данные не перемещаются и не сохраняются.

> [!IMPORTANT]
> Для обеспечения наилучшей производительности, если драйвер внешнего источника данных поддерживает трехкомпонентное имя, настоятельно рекомендуется указать такое имя.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE поддерживает возможность настроить имя столбца, тип данных, допустимость значений NULL и параметры сортировки. Параметр DEFAULT CONSTRAINT нельзя использовать с внешними таблицами.

> [!NOTE]
> Типы данных `Text`, `nText` и `XML` не поддерживаются для столбцов во внешних таблицах хранилища данных SQL Microsoft Azure.

Определения столбцов, включая типы данных и количество столбцов, должны соответствовать данным во внешних файлах. В случае несоответствия при запросе данных строки файла будут отклонены.

LOCATION = '*путь к файлу или папке*'. Указывает путь к папке или файлу и имя файла для фактических данных в Azure Data Lake, хранилище больших двоичных объектов Azure или Hadoop. Расположение начинается с корневой папки. Корневая папка — это расположение данных, указанное во внешнем источнике данных. Инструкция [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) создает путь к папке и саму папку, если она еще не существует. `CREATE EXTERNAL TABLE` не создает путь к папке.

Если вы укажете LOCATION в качестве папки, запрос PolyBase, который выбирает из внешней таблицы, извлечет файлы из этой папки и всех ее вложенных папок. Как и Hadoop, PolyBase не возвращает скрытые папки. Кроме того, не возвращаются файлы, имя которых начинается с подчеркивания (_) или точки (.).

В этом примере, если указано LOCATION='/webdata/', запрос PolyBase вернет строки из mydata.txt и mydata2.txt. Он не вернет файл mydata3.txt, который вложен в скрытую папку. Он также не вернет файл _hidden.txt, так как тот является скрытым.

![Рекурсивные данные для внешних таблиц](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Рекурсивные данные для внешних таблиц")

Чтобы изменить значение по умолчанию и только для чтения в корневой папке, установите для атрибута \<polybase.recursive.traversal> значение false в файле конфигурации core-site.xml. Этот файл находится в папке `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Например, `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *имя внешнего источника данных*. Задает имя внешнего источника данных, содержащего расположение внешних данных. Это расположение находится в Azure Data Lake. Для создания внешнего источника данных используйте инструкцию [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *имя формата внешнего файла*. Задает имя объекта формата внешнего файла, который хранит тип файла и метод сжатия внешних данных. Чтобы создать формат внешнего файла, используйте [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Параметры отклонения. Можно указать параметры отклонения, определяющие, как PolyBase будет обрабатывать *грязные* записи, извлеченные из внешнего источника данных. Запись считается "грязной", если фактический тип данных или количество столбцов не совпадают с определениями столбцов во внешней таблице.

Если вы не указываете или не меняете значения отклонения, PolyBase использует значения по умолчанию. Сведения о параметрах отклонения сохраняются как дополнительные метаданные при создании внешней таблицы с помощью инструкции CREATE EXTERNAL TABLE. При последующем выполнении инструкции SELECT или SELECT INTO SELECT для выбора данных из внешней таблицы PolyBase будет использовать параметры отклонения для определения числа или процента строк, которые можно отклонить, прежде чем запрос завершится ошибкой. Запрос будет возвращать (частичные) результаты, пока не будет превышено пороговое значение отклонения. Затем он выдаст соответствующее сообщение об ошибке.

REJECT_TYPE = **value** | percentage. Уточняет, указан параметр REJECT_VALUE как литеральное значение или процент.

Значение value — REJECT_VALUE указано в виде литерала, а не в процентах. Запрос PolyBase завершится ошибкой, если число отклоненных строк превышает *reject_value*.

Например, если задано REJECT_VALUE = 5 и REJECT_TYPE = value, запрос PolyBase SELECT завершится ошибкой после отклонения пяти строк.

Значение percentage — REJECT_VALUE указывается в процентах, а не в виде литерала. Запрос PolyBase завершится ошибкой, когда *процент* отклоненных строк превысит *reject_value*. Процент отклоненных строк вычисляется с интервалами.

REJECT_VALUE = *значение отклонения*. Задает значение или процент строк, которые можно отклонить, прежде чем запрос завершится ошибкой.

Если задано REJECT_TYPE = value, значение *reject_value* должно быть целым числом от 0 до 2 147 483 647.

Если задано REJECT_TYPE = percentage, значение *reject_value* должно быть числом с плавающей точкой от 0 до 100.

REJECT_SAMPLE_VALUE = *значение отклоняемого образца*. Этот атрибут является обязательным, если задано REJECT_TYPE = percentage. Он определяет количество строк, которое запрос попытается извлечь, прежде чем PolyBase пересчитает процент отклоненных строк.

Параметр *reject_sample_value* должен быть целым числом от 0 до 2 147 483 647.

Например, если задано REJECT_SAMPLE_VALUE = 1000, PolyBase рассчитает процент отклоненных строк после попытки импортировать 1000 строк из внешнего файла данных. Если процент отклоненных строк меньше значения *reject_value*, PolyBase попытается извлечь следующие 1000 строк. Он продолжает повторно вычислять процент отклоненных строк после каждой попытки извлечения 1000 строк.

> [!NOTE]
> Поскольку PolyBase вычисляет процент отклоненных строк с интервалами, фактический процент отклоненных строк может превысить *reject_value*.

Пример

В этом примере показано, как три параметра REJECT взаимодействуют друг с другом. Например, если задано REJECT_TYPE = percentage, REJECT_VALUE = 30 и REJECT_SAMPLE_VALUE = 100, произойдет следующее:

- PolyBase попытается извлечь первые 100 строк; из них 25 отклонено, а 75 — извлечено успешно.
- Процент строк с ошибками равен 25 %, что меньше значения отклонения — 30 %. Поэтому PolyBase продолжит извлечение данных из внешнего источника.
- PolyBase пытается загрузить следующие 100 строк. На этот раз 25 строк извлечено успешно и 75 — отклонено.
- Процент отклоненных строк пересчитывается и составляет 50 %. Процент отклоненных строк превысил значение отклонения 30 %.
- Запрос PolyBase завершается ошибкой, поскольку после попытки извлечение первых 200 строк 50 % из них было отклонено. Обратите внимание, что до того, как запрос PolyBase выявил превышение порога отклонения, были возвращены соответствующие строки.

REJECTED_ROW_LOCATION = *расположение каталога*

Указывает каталог во внешнем источнике данных, в который должны записываться строки и соответствующий файл ошибок.
Если указанный файл не существует, PolyBase создаст его от вашего имени. Создается дочерний каталог с именем "\_rejectedrows". Благодаря символу "\_" каталог исключается из других процессов обработки данных, если он явно не указан в параметре LOCATION. В этом каталоге создается папка, имя которой соответствует времени отправки загруженных данных в формате "ГодМесяцДень-ЧасМинутаСекунда" (например, 20180330-173205). В эту папку записываются файлы двух типов: файлы причин и файлы данных.

Как файлы причин, так и файлы данных имеют идентификаторы queryID, связанные с инструкцией CTAS. Так как данные и причины хранятся в отдельных файлах, эти файлы имеют соответствующие суффиксы.

## <a name="permissions"></a>Разрешения

Требуются следующие разрешения:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**

> [!NOTE]
> Разрешения CONTROL DATABASE необходимы для создания только MASTER KEY, DATABASE SCOPED CREDENTIAL и EXTERNAL DATA SOURCE.

Имя входа, которое создает внешний источник данных, должно иметь разрешение на чтение и запись во внешний источник данных, находящийся в хранилище больших двоичных объектов Azure или Hadoop.

> [!IMPORTANT]
> Разрешение ALTER ANY EXTERNAL DATA SOURCE предоставляет любому субъекту возможность создания и изменения объекта внешнего источника данных и, таким образом, также предоставляет возможность доступа ко всем учетным данным уровня базы данных в базе данных. Это разрешение следует рассматривать как высоко привилегированное, поэтому его следует предоставлять только доверенным субъектам в системе.

## <a name="error-handling"></a>Обработка ошибок

При выполнении инструкции CREATE EXTERNAL TABLE PolyBase пытается подключиться к внешнему источнику данных. Если попытка соединения завершается ошибкой, инструкция также завершается ошибкой и внешняя таблица не создается. Это может занять около минуты, поскольку PolyBase повторяет попытки соединения, прежде чем запрос будет завершен ошибкой.

## <a name="general-remarks"></a>Общие замечания

Если используются нерегламентированные запросы, например SELECT FROM EXTERNAL TABLE, PolyBase сохраняет строки, полученные из внешнего источника данных, во временной таблице. После выполнения запроса PolyBase удаляет временную таблицу. В таблицах SQL не сохраняются постоянные данные.

Если выполняется импорт, например SELECT INTO FROM EXTERNAL TABLE, PolyBase сохраняет строки, полученные из внешнего источника данных, в виде постоянных данных в таблице SQL. Новая таблица создается при выполнении запросов, когда PolyBase извлекает внешние данные.

PolyBase может передать некоторые вычисления запросов в Hadoop для повышения производительности запросов. Это действие называется включением предиката. Для его выполнения задайте параметр расположения диспетчера ресурсов Hadoop в [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Вы можете создать несколько внешних таблиц, которые ссылаются на один или разные внешние источники данных.

## <a name="limitations-and-restrictions"></a>Ограничения

Поскольку данные внешней таблицы не находятся под прямым контролем Azure Synapse, они могут быть в любое время изменены или удалены внешним процессом. По этой причине результаты запроса к внешней таблице необязательно будут детерминированными. Один и то же запрос может возвращать разные результаты при каждом обращении к внешней таблице. Аналогичным образом, запрос может завершиться ошибкой, если внешние данные удалены или перемещены.

Вы можете создать несколько внешних таблиц, которые ссылаются на разные внешние источники данных.

Только эти инструкции DDL допускаются для внешних таблиц:

- CREATE TABLE и DROP TABLE
- CREATE STATISTICS и DROP STATISTICS
- CREATE VIEW и DROP VIEW

Неподдерживаемые конструкции и операции:

- Ограничение DEFAULT для столбцов внешней таблицы
- Операции DML обновления, вставки и удаления

### <a name="query-limitations"></a>Ограничения запросов

Рекомендуется, чтобы в одной папке было не больше 30 тысяч файлов. При ссылке на слишком большое число файлов может возникнуть исключение, связанное с нехваткой памяти на виртуальной машине Java, либо снизиться производительность.

### <a name="table-width-limitations"></a>Ограничения по ширине таблицы

В PolyBase в хранилище данных Azure ширина строки должна составлять не более 1 МБ в связи с максимальным размером допустимой строки в определении таблицы. Если схема суммы столбцов превышает 1 МБ, PolyBase не сможет запросить данные.

### <a name="data-type-limitations"></a>Ограничения типов данных

Во внешних таблицах PolyBase нельзя использовать следующие типы данных:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Все определяемые пользователем типы данных

## <a name="locking"></a>Блокировка

Общая блокировка на объект SCHEMARESOLUTION.

## <a name="examples"></a>Примеры

### <a name="a-importing-data-from-adls-gen-2-into-azure-ssdw"></a>A. Импорт данных из ADLS 2-го поколения в Azure [!INCLUDE[ssDW](../../includes/ssdw-md.md)]. 

Примеры для ADLS 1-го поколения см. в статье [Создание внешнего источника данных](create-external-data-source-transact-sql.md).

```sql

-- These values come from your Azure Active Directory Application used to authenticate to ADLS Gen 2. 
CREATE DATABASE SCOPED CREDENTIAL ADLUser
WITH IDENTITY = '<clientID>@\<OAuth2.0TokenEndPoint>',
SECRET = '<KEY>' ;

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (TYPE = HADOOP,
      LOCATION = 'abfss://data@pbasetr.azuredatalakestore.net'
)

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(
    FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
       , STRING_DELIMITER = ''
      , DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'
      , USE_TYPE_DEFAULT = FALSE
      )
)

CREATE EXTERNAL TABLE [dbo].[DimProductexternal]
( [ProductKey] [int] NOT NULL,
  [ProductLabel] nvarchar NULL,
  [ProductName] nvarchar NULL )
WITH
(
    LOCATION='/DimProduct/' ,
    DATA_SOURCE = AzureDataLakeStore ,
    FILE_FORMAT = TextFileFormat ,
    REJECT_TYPE = VALUE ,
    REJECT_VALUE = 0
) ;

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey] ) )
AS SELECT * FROM
[dbo].[DimProduct_external] ;
```

## <a name="see-also"></a>См. также:

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT (Azure Synapse Analytics)](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Общие сведения. Система платформы аналитики

Внешняя таблица используется в следующих целях:

- Запрос к Hadoop или хранилищу больших двоичных объектов Azure с помощью инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Импорт и хранение данных из Hadoop или хранилища больших двоичных объектов Azure в вашей Системе платформы аналитики.

См. также [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) и [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Синтаксис

```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]

<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
  
}  
```

## <a name="arguments"></a>Аргументы

*{им_базы.имя_схемы.имя_таблицы | имя_схемы.имя_таблицы | имя_таблицы}*  Одно-, двух- или трехсоставное имя для создаваемой таблицы. Если речь идет о внешней таблице, в Системе платформы аналитики хранятся только метаданные таблицы, а также базовая статистика о файле или папке, на которые ссылается Hadoop и хранилище больших двоичных объектов Azure. Никакие данные не перемещаются и не хранятся в Системе платформы аналитики.

> [!IMPORTANT]
> Для обеспечения наилучшей производительности, если драйвер внешнего источника данных поддерживает трехкомпонентное имя, настоятельно рекомендуется указать такое имя.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE поддерживает возможность настроить имя столбца, тип данных, допустимость значений NULL и параметры сортировки. Параметр DEFAULT CONSTRAINT нельзя использовать с внешними таблицами.

Определения столбцов, включая типы данных и количество столбцов, должны соответствовать данным во внешних файлах. В случае несоответствия при запросе данных строки файла будут отклонены.

LOCATION = '*путь к файлу или папке*'. Указывает путь к папке или файлу и имя файла для фактических данных в хранилище больших двоичных объектов Azure или Hadoop. Расположение начинается с корневой папки. Корневая папка — это расположение данных, указанное во внешнем источнике данных.

В Системе платформы аналитики инструкция [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) создает путь к папке и саму папку, если она еще не существует. `CREATE EXTERNAL TABLE` не создает путь к папке.

Если вы укажете LOCATION в качестве папки, запрос PolyBase, который выбирает из внешней таблицы, извлечет файлы из этой папки и всех ее вложенных папок. Как и Hadoop, PolyBase не возвращает скрытые папки. Кроме того, не возвращаются файлы, имя которых начинается с подчеркивания (_) или точки (.).

В этом примере, если указано LOCATION='/webdata/', запрос PolyBase вернет строки из mydata.txt и mydata2.txt. Он не вернет файл mydata3.txt, который вложен в скрытую папку. Он также не вернет файл _hidden.txt, так как тот является скрытым.

![Рекурсивные данные для внешних таблиц](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Рекурсивные данные для внешних таблиц")

Чтобы изменить значение по умолчанию и только для чтения в корневой папке, установите для атрибута \<polybase.recursive.traversal> значение false в файле конфигурации core-site.xml. Этот файл находится в папке `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Например, `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *имя внешнего источника данных*. Задает имя внешнего источника данных, содержащего расположение внешних данных. Это расположение находится не в хранилище больших двоичных объектов Azure или Hadoop. Для создания внешнего источника данных используйте инструкцию [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *имя формата внешнего файла*. Задает имя объекта формата внешнего файла, который хранит тип файла и метод сжатия внешних данных. Чтобы создать формат внешнего файла, используйте [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Параметры отклонения. Можно указать параметры отклонения, определяющие, как PolyBase будет обрабатывать *грязные* записи, извлеченные из внешнего источника данных. Запись считается "грязной", если фактический тип данных или количество столбцов не совпадают с определениями столбцов во внешней таблице.

Если вы не указываете или не меняете значения отклонения, PolyBase использует значения по умолчанию. Сведения о параметрах отклонения сохраняются как дополнительные метаданные при создании внешней таблицы с помощью инструкции CREATE EXTERNAL TABLE. При последующем выполнении инструкции SELECT или SELECT INTO SELECT для выбора данных из внешней таблицы PolyBase будет использовать параметры отклонения для определения числа или процента строк, которые можно отклонить, прежде чем запрос завершится ошибкой. Запрос будет возвращать (частичные) результаты, пока не будет превышено пороговое значение отклонения. Затем он выдаст соответствующее сообщение об ошибке.

REJECT_TYPE = **value** | percentage. Уточняет, указан параметр REJECT_VALUE как литеральное значение или процент.

Значение value — REJECT_VALUE указано в виде литерала, а не в процентах. Запрос PolyBase завершится ошибкой, если число отклоненных строк превышает *reject_value*.

Например, если задано REJECT_VALUE = 5 и REJECT_TYPE = value, запрос PolyBase SELECT завершится ошибкой после отклонения пяти строк.

Значение percentage — REJECT_VALUE указывается в процентах, а не в виде литерала. Запрос PolyBase завершится ошибкой, когда *процент* отклоненных строк превысит *reject_value*. Процент отклоненных строк вычисляется с интервалами.

REJECT_VALUE = *значение отклонения*. Задает значение или процент строк, которые можно отклонить, прежде чем запрос завершится ошибкой.

Если задано REJECT_TYPE = value, значение *reject_value* должно быть целым числом от 0 до 2 147 483 647.

Если задано REJECT_TYPE = percentage, значение *reject_value* должно быть числом с плавающей точкой от 0 до 100.

REJECT_SAMPLE_VALUE = *значение отклоняемого образца*. Этот атрибут является обязательным, если задано REJECT_TYPE = percentage. Он определяет количество строк, которое запрос попытается извлечь, прежде чем PolyBase пересчитает процент отклоненных строк.

Параметр *reject_sample_value* должен быть целым числом от 0 до 2 147 483 647.

Например, если задано REJECT_SAMPLE_VALUE = 1000, PolyBase рассчитает процент отклоненных строк после попытки импортировать 1000 строк из внешнего файла данных. Если процент отклоненных строк меньше значения *reject_value*, PolyBase попытается извлечь следующие 1000 строк. Он продолжает повторно вычислять процент отклоненных строк после каждой попытки извлечения 1000 строк.

> [!NOTE]
> Поскольку PolyBase вычисляет процент отклоненных строк с интервалами, фактический процент отклоненных строк может превысить *reject_value*.

Пример

В этом примере показано, как три параметра REJECT взаимодействуют друг с другом. Например, если задано REJECT_TYPE = percentage, REJECT_VALUE = 30 и REJECT_SAMPLE_VALUE = 100, произойдет следующее:

- PolyBase попытается извлечь первые 100 строк; из них 25 отклонено, а 75 — извлечено успешно.
- Процент строк с ошибками равен 25 %, что меньше значения отклонения — 30 %. Поэтому PolyBase продолжит извлечение данных из внешнего источника.
- PolyBase пытается загрузить следующие 100 строк. На этот раз 25 строк извлечено успешно и 75 — отклонено.
- Процент отклоненных строк пересчитывается и составляет 50 %. Процент отклоненных строк превысил значение отклонения 30 %.
- Запрос PolyBase завершается ошибкой, поскольку после попытки извлечение первых 200 строк 50 % из них было отклонено. Обратите внимание, что до того, как запрос PolyBase выявил превышение порога отклонения, были возвращены соответствующие строки.

## <a name="permissions"></a>Разрешения

Требуются следующие разрешения:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Имя входа, которое создает внешний источник данных, должно иметь разрешение на чтение и запись во внешний источник данных, находящийся в хранилище больших двоичных объектов Azure или Hadoop.

> [!IMPORTANT]
> Разрешение ALTER ANY EXTERNAL DATA SOURCE предоставляет любому субъекту возможность создания и изменения объекта внешнего источника данных и, таким образом, также предоставляет возможность доступа ко всем учетным данным уровня базы данных в базе данных. Это разрешение следует рассматривать как высоко привилегированное, поэтому его следует предоставлять только доверенным субъектам в системе.

## <a name="error-handling"></a>Обработка ошибок

При выполнении инструкции CREATE EXTERNAL TABLE PolyBase пытается подключиться к внешнему источнику данных. Если попытка соединения завершается ошибкой, инструкция также завершается ошибкой и внешняя таблица не создается. Это может занять около минуты, поскольку PolyBase повторяет попытки соединения, прежде чем запрос будет завершен ошибкой.

## <a name="general-remarks"></a>Общие замечания

Если используются нерегламентированные запросы, например SELECT FROM EXTERNAL TABLE, PolyBase сохраняет строки, полученные из внешнего источника данных, во временной таблице. После выполнения запроса PolyBase удаляет временную таблицу. В таблицах SQL не сохраняются постоянные данные.

Если выполняется импорт, например SELECT INTO FROM EXTERNAL TABLE, PolyBase сохраняет строки, полученные из внешнего источника данных, в виде постоянных данных в таблице SQL. Новая таблица создается при выполнении запросов, когда PolyBase извлекает внешние данные.

PolyBase может передать некоторые вычисления запросов в Hadoop для повышения производительности запросов. Это действие называется включением предиката. Для его выполнения задайте параметр расположения диспетчера ресурсов Hadoop в [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Вы можете создать несколько внешних таблиц, которые ссылаются на один или разные внешние источники данных.

## <a name="limitations-and-restrictions"></a>Ограничения

Поскольку данные внешней таблицы не находятся под прямым контролем устройства, они могут быть в любое время изменены или удалены внешним процессом. По этой причине результаты запроса к внешней таблице необязательно будут детерминированными. Один и то же запрос может возвращать разные результаты при каждом обращении к внешней таблице. Аналогичным образом, запрос может завершиться ошибкой, если внешние данные удалены или перемещены.

Вы можете создать несколько внешних таблиц, которые ссылаются на разные внешние источники данных. Если же вы одновременно выполняете запросы к различным источникам данных Hadoop, каждый источник Hadoop должен иметь одинаковый параметр конфигурации сервера hadoop connectivity. Например, невозможно одновременно выполнить запрос к кластеру Cloudera Hadoop и кластеру Hortonworks Hadoop, поскольку они используют разные параметры конфигурации. Сведения о параметрах конфигурации и поддерживаемых сочетаниях см. в разделе [Конфигурация подключения к PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Только эти инструкции DDL допускаются для внешних таблиц:

- CREATE TABLE и DROP TABLE
- CREATE STATISTICS и DROP STATISTICS
- CREATE VIEW и DROP VIEW

Неподдерживаемые конструкции и операции:

- Ограничение DEFAULT для столбцов внешней таблицы
- Операции DML обновления, вставки и удаления

### <a name="query-limitations"></a>Ограничения запросов

PolyBase может обработать не более 33 тысяч файлов на папку при выполнении параллельных запросов PolyBase со степенью 32. Это максимальное число включает файлы и вложенные папки в каждой папке HDFS. Если степень параллелизма меньше 32, пользователь может выполнять запросы PolyBase к папкам в HDFS, если в них содержится более 33 тысяч файлов. Рекомендуется указывать короткие пути к внешним файлам и следить за тем, чтобы в каждой папке HDFS было не более 30 тысяч файлов. При ссылке на слишком большое число файлов может возникнуть исключение, связанное с нехваткой памяти на виртуальной машине Java.

### <a name="table-width-limitations"></a>Ограничения по ширине таблицы

В PolyBase в SQL Server 2016 ширина строки должна составлять не более 32 КБ в связи с максимальным размером допустимой строки в определении таблицы. Если схема суммы столбцов превышает 32 КБ, PolyBase не сможет запросить данные.

В Azure Synapse Analytics это ограничение было увеличено до 1 МБ.

### <a name="data-type-limitations"></a>Ограничения типов данных

Во внешних таблицах PolyBase нельзя использовать следующие типы данных:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Все определяемые пользователем типы данных

## <a name="locking"></a>Блокировка

Общая блокировка на объект SCHEMARESOLUTION.

## <a name="security"></a>Безопасность

Файлы данных для внешней таблицы хранятся в хранилище больших двоичных объектов Azure или Hadoop. Эти файлы данных создаются и управляются вашими собственными процессами. В ваши обязанности входит управление безопасностью внешних данных.

## <a name="examples"></a>Примеры

### <a name="a-join-hdfs-data-with-analytics-platform-system-data"></a>A. Соединение данных HDFS с данными Системы платформы аналитики

```sql
SELECT cs.user_ip FROM ClickStream cs
JOIN User u ON cs.user_ip = u.user_ip
WHERE cs.url = 'www.microsoft.com'
;
```

### <a name="b-import-row-data-from-hdfs-into-a-distributed-analytics-platform-system-table"></a>Б. Импорт данных строк из HDFS в распределенную таблицу Системы платформы аналитики

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = HASH (url) )
AS SELECT url, event_date, user_ip FROM ClickStream
;
```

### <a name="c-import-row-data-from-hdfs-into-a-replicated-analytics-platform-system-table"></a>В. Импорт данных строк из HDFS в реплицированную таблицу Системы платформы аналитики

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = REPLICATE )
AS SELECT url, event_date, user_ip
FROM ClickStream
;
```

## <a name="see-also"></a>См. также:

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT (Azure Synapse Analytics)](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
