---
description: OPENROWSET (Transact-SQL)
title: OPENROWSET (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPENROWSET_TSQL
- OPENROWSET
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENROWSET function
- remote data access [SQL Server], OPENROWSET
- ad hoc distributed queries
- OPENROWSET function, Transact-SQL
- OPENROWSET statement
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: f47eda43-33aa-454d-840a-bb15a031ca17
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 43f3bbae327731f6522b71c0fdaafbc9441fe9ca
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169753"
---
# <a name="openrowset-transact-sql"></a>OPENROWSET (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Содержит все необходимые сведения о соединении, которые требуются для доступа к удаленным данным источника данных OLE DB. Это альтернативный метод для доступа к таблицам на связанном сервере и является однократным нерегламентированным методом соединения и удаленного доступа к данным с помощью OLE DB. Вместо этого для более частых ссылок на источники данных OLE DB используйте связанные серверы. Дополнительные сведения см. в разделе [Связанные серверы (компонент Database Engine)](../../relational-databases/linked-servers/linked-servers-database-engine.md). Из предложения FROM запроса можно ссылаться на функцию `OPENROWSET` как на имя таблицы. Функция `OPENROWSET` также может использоваться как целевая таблица в инструкции `INSERT`, `UPDATE` или `DELETE`. Это зависит от возможностей поставщика OLE DB. Несмотря на то, что запрос может возвратить несколько результирующих наборов, функция `OPENROWSET` возвращает только первый из них.

Функция `OPENROWSET` также поддерживает массовые операции с помощью встроенного поставщика BULK, позволяющего считывать данные из файла и возвращать их в виде набора строк.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
OPENROWSET
( { 'provider_name' 
    , { 'datasource' ; 'user_id' ; 'password' | 'provider_string' }
    , {   <table_or_view> | 'query' }
   | BULK 'data_file' ,
       { FORMATFILE = 'format_file_path' [ <bulk_options> ]
       | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB }
} )

<table_or_view> ::= [ catalog. ] [ schema. ] object

<bulk_options> ::=

   [ , DATASOURCE = 'data_source_name' ]

   [ , ERRORFILE = 'file_name' ]
   [ , ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ , MAXERRORS = maximum_errors ]

   [ , FIRSTROW = first_row ]
   [ , LASTROW = last_row ]
   [ , ROWS_PER_BATCH = rows_per_batch ]
   [ , ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) [ UNIQUE ] ]
  
   -- bulk_options related to input file format
   [ , CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ , FORMAT = 'CSV' ]
   [ , FIELDQUOTE = 'quote_characters']
   [ , FORMATFILE = 'format_file_path' ]
   [ , FORMATFILE_DATA_SOURCE = 'data_source_name' ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

### <a name="provider_name"></a>'*provider_name*'
Символьная строка, которая представляет понятное имя (или PROGID) поставщика OLE DB, указанное в реестре. Аргумент *provider_name* не имеет значения по умолчанию. Примеры имен поставщиков: `Microsoft.Jet.OLEDB.4.0`, `SQLNCLI` или `MSDASQL`.

### <a name="datasource"></a>'*datasource*'
Строковая константа, соответствующая конкретному источнику данных OLE DB. Аргумент *datasource* — это свойство DBPROP_INIT_DATASOURCE, которое должно быть передано в интерфейс IDBProperties поставщика для инициализации поставщика. Обычно эта строка содержит имя файла базы данных, имя сервера баз данных или имя, с помощью которого поставщик находит базу или базы данных.
Источником данных может быть путь к файлу `C:\SAMPLES\Northwind.mdb'` для поставщика `Microsoft.Jet.OLEDB.4.0` или строка подключения `Server=Seattle1;Trusted_Connection=yes;` для поставщика `SQLNCLI`.

### <a name="user_id"></a>'*user_id*'
Строковая константа, представляющая имя пользователя, передаваемое указанному поставщику OLE DB. Аргумент *user_id* указывает контекст безопасности для подключения и передается как свойство DBPROP_AUTH_USERID для инициализации поставщика. Аргумент *user_id* не может быть именем входа Microsoft Windows.

### <a name="password"></a>'*password*'
Строковая константа, представляющая пароль пользователя, передаваемый поставщику OLE DB. Аргумент *password* передается как свойство DBPROP_AUTH_PASSWORD при инициализации поставщика. Аргумент *password* не может быть паролем Microsoft Windows.

```sql
SELECT a.*
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
                   'C:\SAMPLES\Northwind.mdb';
                   'admin';
                   'password',
                   Customers) AS a;
```

### <a name="provider_string"></a>'*provider_string*'
Строковая константа для конкретного поставщика, которая передается как свойство DBPROP_INIT_PROVIDERSTRING для инициализации поставщика OLE DB. Аргумент *provider_string* обычно инкапсулирует все необходимые сведения о подключении для инициализации поставщика. Список ключевых слов, распознаваемых поставщиком OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в разделе [Свойства инициализации и авторизации](../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md).

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                            Department) AS d;
```

### <a name="table_or_view"></a><table_or_view>
Удаленная таблица или представление, содержащие данные, которые должны быть прочитаны `OPENROWSET`. Это может быть объект с именем из трех частей со следующими компонентами:
- *каталог* (необязательно) — имя каталога или базы данных, в которых хранится указанный объект.
- *схема* (необязательно) — имя схемы или владелец указанного объекта.
- *объект* — имя объекта, уникальным образом идентифицирующее объект, с которым производится взаимодействие.

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                 AdventureWorks2012.HumanResources.Department) AS d;
```

### <a name="query"></a>'*query*'
Строковая константа, посылаемая поставщику и исполняемая им. Локальный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не обрабатывает этот запрос, но обрабатывает результаты запроса, возвращаемые поставщиком, это так называемый транзитный запрос. Передаваемые запросы полезны при использовании поставщиков, которые не предоставляют свои табличные данные через таблицы имен, а только через командный язык. Передаваемые запросы поддерживаются на удаленном сервере настолько, насколько поставщик запросов поддерживает объект OLE DB Command и его обязательные интерфейсы. Дополнительные сведения см. в статье [SQL Server Native Client (OLE DB)](../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md).

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT TOP 10 GroupName, Name
     FROM AdventureWorks2012.HumanResources.Department') AS a;
```

### <a name="bulk"></a>BULK
Использует поставщик больших наборов строк для функции OPENROWSET, чтобы читать данные из файла. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] функция OPENROWSET может считывать данные из файла без их загрузки в целевую таблицу. Это позволяет использовать функцию OPENROWSET совместно с обычной инструкцией SELECT.

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

Аргументы параметра BULK позволяют полностью управлять началом и концом считывания данных, отладку ошибок и способ обработки полученных данных. Например, можно указать, что файл с данными будет считан как набор строк типа **varbinary**, **varchar** или **nvarchar** из одной строки и одного столбца. Поведение по умолчанию описано в следующем далее описании аргументов.

 Дополнительные сведения об использовании параметра BULK см. в подразделе «Примечания» далее в этом разделе. Дополнительные сведения о разрешениях, необходимых параметру BULK, см. в подразделе «Разрешения» далее в этом разделе.

> [!NOTE]
> Функция OPENROWSET (BULK ...) не оптимизирует ведение журнала при использовании ее для импорта данных с моделью полного восстановления.

Сведения о подготовке данных к массовому импорту см. в разделе [Подготовка данных к массовому экспорту или импорту (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

#### <a name="bulk-data_file"></a>BULK '*data_file*'
Полный путь к файлу данных, данные из которого копируются в целевую таблицу.

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 аргумент data_file может находиться в хранилище BLOB-объектов Azure. Примеры см. в статье [Примеры массового доступа к данным в хранилище BLOB-объектов Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

#### <a name="bulk-error-handling-options"></a>BULK: параметры обработки ошибок

##### <a name="errorfile"></a>ERRORFILE
`ERRORFILE` ='*file_name*. Указывает файл, используемый для сбора строк, которые имеют ошибки форматирования и не могут быть преобразованы в набор строк OLE DB. Эти строки без изменений копируются из файла данных в файл ошибок.

Файл ошибок создается в начале выполнения команды. Если он уже существует, возникнет ошибка. Дополнительно создается управляющий файл с расширением ERROR.txt. Этот файл ссылается на каждую строку в файле ошибок и позволяет провести их диагностику. После исправления ошибок данные могут быть загружены.
**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], аргумент `error_file_path`может находиться в хранилище больших двоичных объектов Azure.

##### <a name="errorfile_data_source_name"></a>ERRORFILE_DATA_SOURCE_NAME
**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Именованный внешний источник данных, указывающий на расположение файла ошибки в хранилище больших двоичных объектов Azure, в котором будут содержаться ошибки, обнаруженные во время импорта. Внешний источник данных должен быть создан с помощью параметра `TYPE = BLOB_STORAGE`, который доступен в [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

##### <a name="maxerrors"></a>MAXERRORS
`MAXERRORS` =*maximum_errors*. Указывает максимальное количество синтаксических ошибок или ошибок форматирования строк, отмеченное в файле форматирования, которое может произойти до того, как функция OPENROWSET сформирует исключение. Пока значение MAXERRORS не достигнуто, функция OPENROWSET не учитывает все ошибочные строки, не загружая их, и считает каждую ошибочную строку за одну ошибку.

Значение по умолчанию для *maximum_errors* — 10.

> [!NOTE]
> `MAX_ERRORS` не применяется к ограничениям CHECK или преобразованиям типов **money** и **bigint**.

#### <a name="bulk-data-processing-options"></a>BULK: параметры обработки данных

##### <a name="firstrow"></a>FIRSTROW
`FIRSTROW` =*first_row*. Указывает номер первой строки для загрузки. Значение по умолчанию — 1. Значение по умолчанию — первая строка указанного файла данных. Номера строк определяются с помощью подсчета признаков конца строки. Значения аргумента FIRSTROW начинаются с 1.

##### <a name="lastrow"></a>LASTROW
`LASTROW` =*last_row*. Указывает номер последней строки для загрузки. Значение по умолчанию равно 0. Оно указывает на последнюю строку в используемом файле данных.

##### <a name="rows_per_batch"></a>ROWS_PER_BATCH
`ROWS_PER_BATCH` =*rows_per_batch*. Указывает примерное количество строк данных в файле данных. Значение должно быть того же порядка, что и реальное количество строк.

`OPENROWSET` всегда импортирует файл данных в одном пакете. Однако при задании аргумента *rows_per_batch* со значением > 0 обработчик запросов использует значение *rows_per_batch* в качестве указания для назначения ресурсов в плане запроса.

По умолчанию значение аргумента ROWS_PER_BATCH неизвестно. Указание аргумента ROWS_PER_BATCH = 0 равносильно опусканию аргумента ROWS_PER_BATCH.

##### <a name="order"></a>ORDER
`ORDER` ( { *column* [ ASC | DESC ] } [ ,... *n* ] [ UNIQUE ] ). Необязательное указание, определяющее способ сортировки данных в файле данных. По умолчанию массовая операция считает, что файл данных не упорядочен. Производительность можно повысить, если оптимизатор запросов сможет использовать заданный порядок для создания более эффективного плана запроса. Ниже приведены примеры, в которых указано, когда полезно назначить сортировку.

- Вставка строк в таблицу с кластеризованным индексом, в которой данные набора строк сортируются по ключу кластеризованного индекса.
- Соединение набора строк с другой таблицей с совпадающими столбцами сортировки и соединения.
- Статистическая обработка данных набора строк по столбцам сортировки.
- Использование набора строк как исходной таблицы в предложении FROM запроса с совпадающими столбцами сортировки и соединения.

##### <a name="unique"></a>UNIQUE
`UNIQUE` указывает, что в файле данных нет повторяющихся записей.

Если собственные строки файла данных не отсортированы в соответствии с заданным порядком или если задано указание UNIQUE и присутствуют повторяющиеся ключи, то будет возвращена ошибка.

При использовании ORDER обязательны псевдонимы столбцов. Список псевдонимов столбцов должен ссылаться на производную таблицу, к которой обращается предложение BULK. Имена столбцов, указанных в предложении ORDER, ссылаются на список псевдонимов столбцов. Нельзя указывать столбцы типов больших значений (**varchar(max)** , **nvarchar(max)** , **varbinary(max)** и **xml**) и типов больших объектов (**text**, **ntext** и **image**).

##### <a name="single_blob"></a>SINGLE_BLOB
Возвращает содержимое файла *data_file* в виде набора строк с одной строкой и одним столбцом типа **varbinary(max)** .

> [!IMPORTANT]
> XML-данные рекомендуется импортировать с помощью параметра SINGLE_BLOB, а не SINGLE_CLOB или SINGLE_NCLOB, потому что только параметр SINGLE_BLOB поддерживает все возможные преобразования кодировок в Windows.

##### <a name="single_clob"></a>SINGLE_CLOB
Считывает файл *data_file* как ASCII-файл, возвращая содержимое в виде набора строк с одной строкой и одним столбцом типа **varchar(max)** и используя параметры сортировки текущей базы данных.

##### <a name="single_nclob"></a>SINGLE_NCLOB
Считывает файл *data_file* в Юникоде, возвращая содержимое в виде набора строк с одной строкой и одним столбцом типа **nvarchar(max)** и используя параметры сортировки текущей базы данных.

```sql
SELECT *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_NCLOB) AS Document;
```

#### <a name="bulk-input-file-format-options"></a>BULK: параметры формата входного файла

##### <a name="codepage"></a>CODEPAGE
`CODEPAGE` = { 'ACP' \| 'OEM' \| 'RAW' \| '*code_page*' }. Задает кодовую страницу данных в файле данных. Аргумент CODEPAGE имеет смысл только в том случае, если данные содержат столбцы типа **char**, **varchar** или **text** с символами, коды которых больше 127 или меньше 32.

> [!IMPORTANT]
> `CODEPAGE` не поддерживается в Linux.

> [!NOTE]
> Рекомендуется указывать имя параметра сортировки для каждого столбца в файле форматирования, кроме случаев, когда параметр 65001 должен иметь приоритет над спецификацией параметров сортировки или кодовой страницы.

|Значение аргумента CODEPAGE|Описание|
|--------------------|-----------------|
|ACP|Столбцы типа **char**, **varchar** или **text** преобразуются из кодовой страницы ANSI или [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (ISO 1252) в кодовую страницу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|OEM (по умолчанию)|Столбцы типа **char**, **varchar** или **text** преобразуются из системной кодовой страницы OEM в кодовую страницу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|RAW|Преобразование из одной кодовой страницы в другую не выполняется. Это наиболее быстрый параметр.|
|*code_page*|Показывает исходную кодовую страницу, в которой представлены символы в файле данных, например 850.<br /><br /> **Важно!** Версии до [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] не поддерживают кодовую страницу 65001 (кодировка UTF-8).|

##### <a name="format"></a>FORMAT
`FORMAT` **=** 'CSV' **Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Указывает файл данных с разделителями-запятыми, соответствующий стандарту [RFC 4180](https://tools.ietf.org/html/rfc4180).

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

##### <a name="formatfile"></a>FORMATFILE
`FORMATFILE` ='*format_file_path*. Указывает полный путь к файлу форматирования. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает два типа файлов форматирования: XML и отличный от XML.

Файл форматирования необходим для определения типов столбцов в результирующем наборе. Единственное исключение — случай, когда указаны аргументы SINGLE_CLOB, SINGLE_BLOB или SINGLE_NCLOB, при которых файл форматирования не обязателен.

Сведения о файлах форматирования см. в разделе [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Начиная с [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1, аргумент format_file_path может находиться в хранилище больших двоичных объектов Azure. Примеры см. в статье [Примеры массового доступа к данным в хранилище BLOB-объектов Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

##### <a name="fieldquote"></a>FIELDQUOTE
`FIELDQUOTE` **=** 'field_quote' **Область применения:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Определяет символ, который будет использоваться в качестве символа кавычки в CSV-файле. Если этот символ не задан, в качестве символа кавычки будет использоваться символ (") согласно стандарту [RFC 4180](https://tools.ietf.org/html/rfc4180).

## <a name="remarks"></a>Remarks

Функцию `OPENROWSET` можно использовать для доступа к удаленным данным из источников OLE DB только в том случае, если параметр реестра **DisallowAdhocAccess** явно содержит значение 0 для указанного поставщика, а также если включен расширенный параметр конфигурации Ad Hoc Distributed Queries. Если эти параметры не установлены, поведение по умолчанию запрещает нерегламентированный доступ.

При удаленном доступе к источнику данных OLE DB автоматическое делегирование идентификатора имени входа доверительных соединений с сервера, к которому подключен клиент, на запрашиваемый сервер не выполняется. Делегирование проверки подлинности должно быть настроено.

Имена каталога или схемы необходимы, если поставщик OLE DB поддерживает несколько каталогов и схем для указанного источника данных. Значения аргументов _catalog_ и _schema_ можно не указывать, если поставщик OLE DB их не поддерживает. Если поставщик поддерживает только имена схем, необходимо указать двухкомпонентное имя в формате _схема_ **.** _объект_. Если поставщик поддерживает только имена каталогов, необходимо указать трехкомпонентное имя в формате _каталог_ **.** _схема_ **.** _объект_. Для передаваемых запросов, использующих поставщик OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], необходимо указать трехкомпонентное имя. Дополнительные сведения см. в статье [Синтаксические обозначения в Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

В качестве аргументов в функции `OPENROWSET` нельзя использовать переменные.

Любой вызов функции `OPENDATASOURCE`, `OPENQUERY` или `OPENROWSET` в предложении `FROM` вычисляется отдельно и независимо от любого вызова этих функций, используемого как назначение при обновлении, даже если в двух таких вызовах будут заданы идентичные аргументы. В частности, условия фильтра или соединения, применяемые к результатам одного из таких вызовов, никак не влияют на результаты другого.

## <a name="using-openrowset-with-the-bulk-option"></a>Применение инструкции OPENROWSET с параметром BULK

Следующие усовершенствования [!INCLUDE[tsql](../../includes/tsql-md.md)] поддерживают функцию OPENROWSET(BULK…).

- Предложение FROM, используемое в инструкции `SELECT`, может вызывать `OPENROWSET(BULK...)` вместо имени таблицы с полной функциональностью инструкции `SELECT`.

   Функции `OPENROWSET` с параметром `BULK` требуется корреляционное имя, также известное как переменная диапазона или псевдоним в предложении `FROM`. Могут быть указаны псевдонимы столбцов. Если список псевдонимов столбцов не указан, файл форматирования должен содержать имена столбцов. Указание псевдонимов столбцов переопределяет имена столбцов в файле форматирования, такие как:

  - `FROM OPENROWSET(BULK...) AS table_alias`
  - `FROM OPENROWSET(BULK...) AS table_alias(column_alias,...n)`

  > [!IMPORTANT]
  > Сбой при добавлении `AS <table_alias>` приведет к ошибке: Сообщение 491, уровень 16, состояние 1, строка 20. Корреляционное имя должно быть указано для группового набора строк в предложении FROM.

- Инструкция `SELECT...FROM OPENROWSET(BULK...)` запрашивает данные в файле напрямую, не импортируя их в таблицу. Кроме того, инструкции `SELECT...FROM OPENROWSET(BULK...)` могут перечислять псевдонимы массовых столбцов, используя файл форматирования для указания имен столбцов и типов данных.
- Использование `OPENROWSET(BULK...)` в качестве исходной таблицы в инструкции `INSERT` или `MERGE` обеспечивает массовый импорт данных из файла в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделе [Массовый импорт данных при помощи инструкции BULK INSERT или OPENROWSET(BULK...) (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).
- При использовании параметра `OPENROWSET BULK` с инструкцией `INSERT` предложение BULK поддерживает табличные указания. Кроме обычных табличных указаний, таких как `TABLOCK`, предложение `BULK` принимает следующие специальные табличные указания: `IGNORE_CONSTRAINTS` (пропускает только ограничения `CHECK` и `FOREIGN KEY`), `IGNORE_TRIGGERS`, `KEEPDEFAULTS` и `KEEPIDENTITY`. Дополнительные сведения см. в разделе [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md).

  Сведения об использовании инструкций `INSERT...SELECT * FROM OPENROWSET(BULK...)` см. в статье [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md). Сведения о том, когда в журнале транзакций регистрируются операции вставки строк, выполняемые при массовом импорте, см. в разделе [Предварительные условия для минимального протоколирования массового импорта данных](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).

> [!NOTE]
> При использовании функции `OPENROWSET` важно понимать, как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обрабатывает олицетворение. Сведения о вопросах безопасности см. в статье [Массовый импорт данных при помощи инструкции BULK INSERT или OPENROWSET(BULK...) (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

### <a name="bulk-importing-sqlchar-sqlnchar-or-sqlbinary-data"></a>Массовый импорт данных SQLCHAR, SQLNCHAR или SQLBINARY

Функция OPENROWSET(BULK...) предполагает, что максимальная длина данных SQLCHAR, SQLNCHAR или SQLBINARY не превышает 8 000 байт (если не указано иное). Если импортируемые данные находятся в поле данных LOB, которое содержит любые объекты **varchar(max)** , **nvarchar(max)** или **varbinary(max)** , превышающие 8000 байт, необходимо использовать XML-файл форматирования, определяющий максимальную длину для поля данных. Чтобы указать максимальную длину, измените файл форматирования и объявите атрибут MAX_LENGTH.

> [!NOTE]
> Автоматически сформированный файл форматирования не задает длину или максимальную длину для поля LOB. Однако можно изменить файл форматирования и указать длину или максимальную длину вручную.

### <a name="bulk-exporting-or-importing-sqlxml-documents"></a>Массовый экспорт или импорт документов SQLXML

Чтобы выполнить массовый экспорт или импорт SQLXML-данных используйте один из следующих типов данных в файле форматирования:

|Тип данных|Действие|
|---------------|------------|
|SQLCHAR или SQLVARYCHAR|Данные отправляются в кодовой странице клиента или кодовой странице, определенной параметрами сортировки.|
|SQLNCHAR или SQLNVARCHAR|Данные отправляются в Юникоде.|
|SQLBINARY или SQLVARYBIN|Данные отправляются без преобразования.|

## <a name="permissions"></a>Разрешения

Разрешения функции `OPENROWSET` определяются разрешениями имени пользователя, передаваемого поставщику OLE DB. Для использования параметра `BULK` требуется разрешение `ADMINISTER BULK OPERATIONS` или `ADMINISTER DATABASE BULK OPERATIONS`.

## <a name="examples"></a>Примеры

### <a name="a-using-openrowset-with-select-and-the-sql-server-native-client-ole-db-provider"></a>A. Использование функции OPENROWSET с инструкцией SELECT и поставщиком OLE DB для собственного клиента SQL Server

В приведенном ниже примере для доступа к таблице `HumanResources.Department` в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] на удаленном сервере `Seattle1` используется поставщик OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. (При использовании SQLNCLI [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет использовать последнюю версию поставщика OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].) Инструкция `SELECT` используется для определения возвращаемого набора строк. Строка поставщика содержит ключевые слова `Server` и `Trusted_Connection`. Эти ключевые слова распознаются поставщиком OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT GroupName, Name, DepartmentID
      FROM AdventureWorks2012.HumanResources.Department
      ORDER BY GroupName, Name') AS a;
```

### <a name="b-using-the-microsoft-ole-db-provider-for-jet"></a>Б. Использование поставщика Microsoft OLE DB для Jet

В приведенном ниже примере для доступа к таблице `Customers` в базе данных `Northwind`[!INCLUDE[msCoName](../../includes/msconame-md.md)] Access используется поставщик OLE DB для Jet ([!INCLUDE[msCoName](../../includes/msconame-md.md)]).

> [!NOTE]
> В этом примере предполагается, что Access установлен. Для запуска данного примера необходимо установить базу данных Northwind.

```sql
SELECT CustomerID, CompanyName
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
      'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';
      'admin';'',Customers);
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="c-using-openrowset-and-another-table-in-an-inner-join"></a>В. Использование функции OPENROWSET и другой таблицы в предложении INNER JOIN

В приведенном ниже примере производится выборка всех данных из таблицы `Customers` в локальном экземпляре базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `Northwind` и из таблицы `Orders` в базе данных Access `Northwind`, хранящейся на том же компьютере.

> [!NOTE]
> В этом примере предполагается, что Access установлен. Для запуска данного примера необходимо установить базу данных Northwind.

```sql
USE Northwind;
GO
SELECT c.*, o.*
FROM Northwind.dbo.Customers AS c
   INNER JOIN OPENROWSET('Microsoft.Jet.OLEDB.4.0',
   'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';'admin';'', Orders)
   AS o
   ON c.CustomerID = o.CustomerID ;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="d-using-openrowset-to-bulk-insert-file-data-into-a-varbinarymax-column"></a>Г. Использование функции OPENROWSET для массовой вставки данных из файла в столбец типа varbinary(max)

 В следующем примере создается небольшая таблица для демонстрационных целей и вставляются данные из файла с именем `Text1.txt`, расположенного в корневом каталоге диска `C:`, в столбец `varbinary(max)`.

```sql
CREATE TABLE myTable(FileName NVARCHAR(60),
  FileType NVARCHAR(60), Document VARBINARY(max));
GO

INSERT INTO myTable(FileName, FileType, Document)
   SELECT
      'Text1.txt' AS FileName
      , '.txt' AS FileType
      , *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_BLOB) AS Document;
GO
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="e-using-the-openrowset-bulk-provider-with-a-format-file-to-retrieve-rows-from-a-text-file"></a>Д. Использование поставщика OPENROWSET BULK с файлом форматирования для получения строк из текстового файла

В следующем примере используется файл форматирования для получения строк, разделенных символами табуляции, из файла `values.txt`, который содержит следующие данные:

```
1     Data Item 1
2     Data Item 2
3     Data Item 3
```

Файл форматирования `values.fmt` описывает столбцы в файле `values.txt`:

```
9.0
2  
1  SQLCHAR  0  10 "\t"        1  ID                      SQL_Latin1_General_Cp437_BIN
2  SQLCHAR  0  40 "\r\n"      2  Description        SQL_Latin1_General_Cp437_BIN
```

Это запрос, который возвращает данные:

```sql
SELECT a.* FROM OPENROWSET( BULK 'c:\test\values.txt',
   FORMATFILE = 'c:\test\values.fmt') AS a;
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="f-specifying-a-format-file-and-code-page"></a>Е. Указание файла форматирования и кодовой страницы

В приведенном ниже примере показано, как одновременно использовать параметры файла форматирования и кодовой страницы.

```sql
INSERT INTO MyTable SELECT a.* FROM
OPENROWSET (BULK N'D:\data.csv', FORMATFILE =
    'D:\format_no_collation.txt', CODEPAGE = '65001') AS a;
```

### <a name="g-accessing-data-from-a-csv-file-with-a-format-file"></a>Ж. Доступ к данным из CSV-файла с помощью файла форматирования

**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="h-accessing-data-from-a-csv-file-without-a-format-file"></a>З. Доступ к данным из CSV-файла без использования файла форматирования

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\Program Files\Microsoft SQL Server\MSSQL14.CTP1_1\MSSQL\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

```sql
SELECT *
FROM OPENROWSET
   (  'MSDASQL'
     ,'Driver={Microsoft Access Text Driver (*.txt, *.csv)}'
     ,'select * from E:\Tlog\TerritoryData.csv')
;
```

> [!IMPORTANT]
>
> - Драйвер ODBC должен быть 64-разрядным. Откройте вкладку **Драйверы** приложения [Источники данных OBDC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md) в Windows, чтобы проверить это. Есть 32-разрядный `Microsoft Text Driver (*.txt, *.csv)`, который не будет работать с 64-разрядной версией sqlservr.exe.
> - База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="i-accessing-data-from-a-file-stored-on-azure-blob-storage"></a>И. Доступ к данным из файла, который находится в хранилище BLOB-объектов Azure

**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
В приведенном ниже примере используется внешний источник данных, указывающий на контейнер в учетной записи хранения Azure и учетные данные уровня базы данных, созданные для подписанного URL-адреса.

```sql
SELECT * FROM OPENROWSET(
   BULK 'inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoices',
   SINGLE_CLOB) AS DataFile;
```

Полные примеры использования функции `OPENROWSET`, включая настройку учетных данных и внешнего источника данных, см. в статье [Примеры массового доступа к данным в хранилище BLOB-объектов Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage"></a>К. Импорт данных в таблицу из файла, который находится в хранилище BLOB-объектов Azure

В приведенном ниже примере показано, как с помощью команды OPENROWSET загрузить данные из CSV-файла в расположение хранилища BLOB-объектов Azure, для которого был создан ключ SAS. Расположение хранилища BLOB-объектов Azure настроено как внешний источник данных. Для этого требуются учетные данные для базы с подписанным URL-адресом, зашифрованным с помощью главного ключа в пользовательской базе данных.

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/curriculum'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

INSERT INTO achievements with (TABLOCK) (id, description)
SELECT * FROM OPENROWSET(
   BULK  'csv/achievements.csv',
   DATA_SOURCE = 'MyAzureBlobStorage',
   FORMAT ='CSV',
   FORMATFILE='csv/achievements-c.xml',
   FORMATFILE_DATA_SOURCE = 'MyAzureBlobStorage'
    ) AS DataFile;
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает чтение из Хранилища BLOB-объектов Azure только с помощью маркера SAS.

### <a name="additional-examples"></a>Дополнительные примеры

Дополнительные примеры использования инструкции `INSERT...SELECT * FROM OPENROWSET(BULK...)` см. в следующих статьях:

- [Примеры массового импорта и экспорта XML-документов (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Сохранение значений идентификаторов при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>См. также:

- [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)
- [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)
- [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)
- [OPENDATASOURCE (Transact-SQL)](../../t-sql/functions/opendatasource-transact-sql.md)
- [OPENQUERY (Transact-SQL)](../../t-sql/functions/openquery-transact-sql.md)
- [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)
- [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)
- [sp_serveroption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)
- [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)
- [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)
