---
title: BULK INSERT (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для инструкции BULK INSERT.
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BULK_TSQL
- BULK_INSERT
- BULK_INSERT_TSQL
- BULK INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- tables [SQL Server], importing data into
- inserting files
- views [SQL Server], importing data into
- BULK INSERT statement
- views [SQL Server], exporting data from
- importing data, bulk import
- bulk importing [SQL Server], BULK INSERT statement
- file importing [SQL Server]
ms.assetid: be3984e1-5ab3-4226-a539-a9f58e1e01e2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 12c1273473fd91c0ea5222add0288fac1b06f37b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170596"
---
# <a name="bulk-insert-transact-sql"></a>BULK INSERT (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Выполняет импорт файла данных в таблицу или представление базы данных в формате, указанном пользователем, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
BULK INSERT
   { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }
      FROM 'data_file'
     [ WITH
    (
   [ [ , ] BATCHSIZE = batch_size ]
   [ [ , ] CHECK_CONSTRAINTS ]
   [ [ , ] CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ [ , ] DATAFILETYPE =
      { 'char' | 'native'| 'widechar' | 'widenative' } ]
   [ [ , ] DATA_SOURCE = 'data_source_name' ]
   [ [ , ] ERRORFILE = 'file_name' ]
   [ [ , ] ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] FIRSTROW = first_row ]
   [ [ , ] FIRE_TRIGGERS ]
   [ [ , ] FORMATFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] KEEPIDENTITY ]
   [ [ , ] KEEPNULLS ]
   [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]
   [ [ , ] LASTROW = last_row ]
   [ [ , ] MAXERRORS = max_errors ]
   [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]
   [ [ , ] ROWS_PER_BATCH = rows_per_batch ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
   [ [ , ] TABLOCK ]

   -- input file format options
   [ [ , ] FORMAT = 'CSV' ]
   [ [ , ] FIELDQUOTE = 'quote_characters']
   [ [ , ] FORMATFILE = 'format_file_path' ]
   [ [ , ] FIELDTERMINATOR = 'field_terminator' ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
    )]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*database_name*. Имя базы данных, в которой находится указанная таблица или представление. Если не указано, предполагается текущая база данных.

*schema_name*. Имя схемы таблицы или представления. Указание аргумента *schema_name* необязательно, если схемой по умолчанию для пользователя, выполняющего операцию массового импорта, является схема указанной таблицы или представления. Если аргумент *schema* не указан и схема по умолчанию для пользователя, выполняющего операцию массового импорта, отличается от схемы таблицы или представления, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сообщение об ошибке и операция массового импорта не выполняется.

*table_name*. Имя таблицы или представления, куда производится массовый импорт данных. Могут указываться только те представления, в которых все столбцы относятся к одной и той же базовой таблице. Дополнительные сведения об ограничениях, накладываемых на загрузку данных в представления, см. в разделе [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md).

**'** _data_file_ **'** . Полный путь и имя файла данных, который содержит импортируемые в указанную таблицу или представление данные. Инструкция BULK INSERT может импортировать данные с диска (сетевого, гибкого, жесткого и т. д.) или из хранилища больших двоичных объектов Azure.

Аргумент *data_file* должен указывать действительный путь с того сервера, на котором запущен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если аргумент *data_file* является удаленным файлом, указывайте имя в формате UNC. Имя в формате UNC имеет форму \\\\*Имя_системы*\\*Имя_общего_ресурса*\\*Путь*\\*Имя_файла*. Пример:

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.dat';
```

**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 и базе данных SQL Azure.
Начиная с [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1, аргумент data_file может находиться в хранилище больших двоичных объектов Azure. В этом случае необходимо указать параметр **data_source_name**. Пример см. в разделе [Импорт данных из файла в хранилище BLOB-объектов Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

**'** _data_source_name_ **'** 
**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 и базе данных SQL Azure.
Именованный внешний источник данных, указывающий на расположение импортируемого файла в хранилище больших двоичных объектов Azure. Внешний источник данных должен быть создан с помощью параметра `TYPE = BLOB_STORAGE`, который доступен в [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md). Пример см. в разделе [Импорт данных из файла в хранилище BLOB-объектов Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

BATCHSIZE **=** _batch_size_. Задает количество строк в одном пакете. Каждый пакет копируется на сервер за одну транзакцию. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] фиксирует или откатывает транзакцию для каждого из пакетов. По умолчанию, все данные, содержащиеся в файле, передаются одним пакетом. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.

CHECK_CONSTRAINTS. Указывает, что при выполнении операции массового импорта будет выполняться проверка всех ограничений целевой таблицы или представления. Без параметра CHECK_CONSTRAINTS все ограничения CHECK и FOREIGN KEY пропускаются, и после завершения операции ограничение таблицы помечается как ненадежное.

> [!NOTE]
> Ограничения UNIQUE и PRIMARY KEY проверяются в любом случае. При импортировании в символьный столбец, имеющий ограничение NOT NULL, команда BULK INSERT вставляет пустую строку, если в текстовом файле нет значения.

Рано или поздно необходимо проверять всю таблицу на соответствие ограничениям. Если таблица перед началом операции массового импорта была не пуста, затраты на повторную проверку ограничений могут превысить затраты на применение ограничений CHECK к добавочным данным.

Отключение проверки ограничений (настройка по умолчанию) может потребоваться в тех ситуациях, когда входные данные содержат строки, нарушающие эти ограничения. Можно выполнить импорт данных при отключенной проверке ограничений CHECK, а затем при помощи инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] удалить недопустимые данные.

> [!NOTE]
> Параметр MAXERRORS не влияет на проверку ограничений.

CODEPAGE **=** { **'** ACP **'** \| **'** OEM **'** \| **'** RAW **'** \| **'** _code_page_ **'** } Задает кодовую страницу данных в файле данных. Аргумент CODEPAGE имеет смысл только в том случае, если данные содержат столбцы типа **char**, **varchar** или **text** с символьными значениями, коды которых больше **127** или меньше **32**. Пример см. в разделе [Указание кодовой страницы](#d-specifying-a-code-page).

> [!IMPORTANT]
> Параметр CODEPAGE для [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] не поддерживается в Linux. Параметр CODEPAGE для [!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)] допускает только значение **RAW**.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] рекомендует указывать имя параметров сортировки для каждого столбца в [файле форматирования](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

|Значение аргумента CODEPAGE|Описание|
|--------------------|-----------------|
|ACP|Столбцы типа **char**, **varchar** или **text** преобразуются из кодовой страницы [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (ISO 1252) в кодовую страницу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|OEM (по умолчанию)|Столбцы типа **char**, **varchar** или **text** преобразуются из кодовой страницы OEM в кодовую страницу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|RAW|Преобразование кодовой страницы не производится. Это самый быстрый режим.|
|*code_page*|Номер кодовой страницы, например 850.<br /><br /> **&#42;&#42; Важно! &#42;&#42;** Версии до [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] не поддерживают кодовую страницу 65001 (кодировка UTF-8).|
| &nbsp; | &nbsp; |

DATAFILETYPE **=** { **'char'** \| **'native'** \| **'widechar'** \| **'widenative'** } Указывает, что инструкция BULK INSERT выполняет операцию импорта с использованием указанного типа файла данных.

&nbsp;

|Значение DATAFILETYPE|Представление данных|
|------------------------|------------------------------|
|**char** (по умолчанию)|В символьном формате.<br /><br /> Дополнительные сведения см. в разделе [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).|
|**native**|В собственных типах базы данных. Создайте файл данных собственных типов путем массового импорта данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью программы **bcp**.<br /><br /> Значение собственного типа обеспечивает более высокую производительность по сравнению со значением типа char. Собственный формат данных рекомендуется использовать во время массовой передачи данных между несколькими экземплярами SQL Server при помощи файла данных, не содержащего символы в расширенной кодировке или символы в двухбайтовой кодировке (DBCS).<br /><br /> Дополнительные сведения см. в разделе [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).|
|**widechar**|Знаки Юникода.<br /><br /> Дополнительные сведения см. в разделе [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).|
|**widenative**|В собственных типах базы данных, за исключением столбцов типа **char**, **varchar** и **text**, в которых данные хранятся в Юникоде. Создайте файл данных типа **widenative** путем массового импорта данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью программы **bcp**.<br /><br /> Значение **widenative** обеспечивает более высокую производительность по сравнению с **widechar**. Если файл данных содержит расширенные символы [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)], укажите значение **widenative**.<br /><br /> Дополнительные сведения см. в разделе [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).|
| &nbsp; | &nbsp; |

ERRORFILE **='** _file_name_ **'** . Указывает файл, используемый для сбора строк, которые имеют ошибки форматирования и не могут быть преобразованы в набор строк OLE DB. Эти строки без изменений копируются из файла данных в файл ошибок.

Файл ошибок создается на стадии выполнения команды. Если он уже существует, возникает ошибка. Дополнительно создается управляющий файл с расширением ERROR.txt. в котором содержатся ссылки на каждую из строк в файле ошибок и диагностические сведения. После исправления ошибок эти данные могут быть повторно загружены.
**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], аргумент `error_file_path`может находиться в хранилище больших двоичных объектов Azure.

'errorfile_data_source_name' **Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Именованный внешний источник данных, указывающий на расположение файла ошибки в хранилище больших двоичных объектов Azure, в котором будут содержаться ошибки, обнаруженные во время импорта. Внешний источник данных должен быть создан с помощью параметра `TYPE = BLOB_STORAGE`, который доступен в [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FIRSTROW **=** _first_row_. Указывает номер первой строки для загрузки. Значение по умолчанию — первая строка указанного файла данных. Значения аргумента FIRSTROW начинаются с 1.

> [!NOTE]
> Атрибут FIRSTROW не предназначен для пропуска заголовков столбцов. Пропуск заголовков не поддерживается инструкцией BULK INSERT. При пропуске строк компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] выполняет поиск только в признаках конца поля и не проверяет данные в полях пропущенных строк.

FIRE_TRIGGERS. Указывает, что при массовом импорте будут выполняться триггеры типа INSERT, определенные для целевой таблицы. Если для операции INSERT определены триггеры в целевой таблице, они будут срабатывать для каждого загруженного пакета.

Если параметр FIRE_TRIGGERS не указан, триггеры Insert не выполняются.

FORMATFILE_DATA_SOURCE **=** 'data_source_name' **Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 1.1.
Именованный внешний источник данных, указывающий на расположение файла форматирования в хранилище больших двоичных объектов Azure, который будет определять схему импортированных данных. Внешний источник данных должен быть создан с помощью параметра `TYPE = BLOB_STORAGE`, который доступен в [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

KEEPIDENTITY. Указывает, что значение или значения идентификаторов в файле импортированных данных будут использоваться для столбца идентификаторов. Если параметр KEEPIDENTITY не указан, значения идентификаторов для этого столбца проверяются, но не импортируются, а [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически назначает уникальные значения на основе начального значения и приращения, указанных при создании таблицы. Если файл данных не содержит значений для столбца идентификаторов, укажите в файле форматирования, что столбец идентификаторов в таблице или представлении при импорте данных следует пропустить. В этом случае [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически назначит уникальные значения для этого столбца. Дополнительные сведения см. в разделе [DBCC CHECKIDENT (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).

Дополнительные сведения о хранении значений идентификаторов см. в статье [Сохранение значений идентификаторов при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).

KEEPNULLS. Указывает, что пустым столбцам при массовом импорте должны присваиваться значения NULL, а не значения по умолчанию, назначенные для этих столбцов. Дополнительные сведения см. в разделе [Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).

KILOBYTES_PER_BATCH **=** _kilobytes_per_batch_. Определяет приблизительное число килобайт данных в пакете как *kilobytes_per_batch*. По умолчанию, значение KILOBYTES_PER_BATCH неизвестно. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.

LASTROW **=** _last_row_. Указывает номер последней строки для загрузки. Значение по умолчанию 0, что обозначает последнюю строку в указанном файле данных.

MAXERRORS **=** _max_errors_. Указывает максимальное число синтаксических ошибок, допустимых для файла данных, прежде чем операция массового импорта будет отменена. Каждая строка, импорт которой при массовом импорте не может быть выполнен, пропускается и считается за одну ошибку. Если аргумент *max_errors* не указан, значение по умолчанию равно 10.

> [!NOTE]
> Параметр MAX_ERRORS не применяет проверки ограничения или преобразование типов данных **money** и **bigint**.

ORDER ( { *column* [ ASC | DESC ] } [ **,** ... *n* ] ). Задает способ сортировки данных в файле данных. Производительность массового импорта увеличивается, если импортируемые данные упорядочены согласно кластеризованному индексу таблицы (при наличии). Если файл данных упорядочен в другом порядке, то есть в порядке отличном от порядка ключа кластеризованного индекса или если в таблице отсутствует кластеризованный индекс, то предложение ORDER не обрабатывается. В целевой таблице должны быть указаны имена столбцов. По умолчанию, операция массовой вставки считает, что файл данных не отсортирован. Для оптимизированного массового импорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , помимо прочего, проверяет сортировку импортируемых данных.

*n*. Заполнитель, означающий, что может быть указано несколько столбцов.

ROWS_PER_BATCH **=** _rows_per_batch_. Указывает примерное количество строк данных в файле данных.

По умолчанию все данные в файле отправляются на сервер за одну транзакцию, а число строк в пакете оптимизатору запросов неизвестно. Если указать аргумент ROWS_PER_BATCH (со значением > 0), сервер будет использовать это значение для оптимизации операции массового импорта. Значение, указанное в ROWS_PER_BATCH, должно приблизительно совпадать с фактическим числом строк. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.

TABLOCK. Указывает необходимость запроса блокировки уровня таблицы на время выполнения массового импорта. Если таблица не имеет индексов и указано ключевое слово TABLOCK, загрузка в таблицу может производиться параллельно несколькими клиентами. По умолчанию работа блокировки определяется параметром таблицы **table lock on bulk load**. Блокировка на время выполнения массового импорта значительно повышает производительность, позволяя снизить состязание блокировок таблицы. Дополнительные сведения о вопросах производительности см. в подразделе «Замечания» далее в этом разделе.

Для индекса columnstore поведение блокировки отличается, так как он внутри делится на несколько наборов строк. Каждый поток загружает данные отдельно в каждый набор строк, выполняя X-блокировку в наборе строк, что позволяет загружать данные параллельно с сеансами загрузки данных. При использовании параметра TABLOCK поток применит X-блокировку к таблице (в отличие от блокировки BU для традиционных наборов строк), что сделает невозможной одновременную загрузку данных для других параллельных потоков.

### <a name="input-file-format-options"></a>Параметры формата входного файла

FORMAT **=** 'CSV' **Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Указывает файл данных с разделителями-запятыми, соответствующий стандарту [RFC 4180](https://tools.ietf.org/html/rfc4180).

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.csv'
WITH ( FORMAT='CSV');
```

FIELDQUOTE **=** 'field_quote' **Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Определяет символ, который будет использоваться в качестве символа кавычки в CSV-файле. Если этот символ не задан, в качестве символа кавычки будет использоваться символ (") согласно стандарту [RFC 4180](https://tools.ietf.org/html/rfc4180).

FORMATFILE **=** '_format_file_path_'. Указывает полный путь к файлу форматирования. Этот файл форматирования содержит описание файла данных — сведения, полученные путем применения программы **bcp** к той же таблице или представлению. И предназначен для случаев, когда:

- файл данных содержит больше или меньше столбцов, чем таблица или представление;
- столбцы расположены в другом порядке;
- отличаются разделители столбцов;
- имеются какие-либо другие изменения в формате данных. Файлы форматирования обычно создаются с помощью программы **bcp** и затем при необходимости изменяются в текстовом редакторе. Дополнительные сведения см в разделах [Служебная программа bcp](../../tools/bcp-utility.md) и [Создание файла форматирования](../../relational-databases/import-export/create-a-format-file-sql-server.md).

**Применимо к:** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 и базе данных SQL Azure.
Начиная с [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP-версии 1.1, аргумент format_file_path может находиться в хранилище больших двоичных объектов Azure.

FIELDTERMINATOR **='** _field_terminator_ **'** . Указывает признак конца поля, используемый для файлов данных типа **char** и **widechar**. По умолчанию, признаком конца поля является символ табуляции (\t). Дополнительные сведения см. в разделе [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

ROWTERMINATOR **='** _row_terminator_ **'** . Указывает признак конца строки, используемый для файлов данных типа **char** и **widechar**. По умолчанию признаком конца строки является символ **\r\n** (символ перевода строки). Дополнительные сведения см. в разделе [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

## <a name="compatibility"></a>Совместимость

Инструкция BULK INSERT осуществляет более строгую проверку загружаемых из файла данных, что может вызвать прекращение работы существующих скриптов, которые ранее работали с неправильными данными. В частности, теперь BULK INSERT проверяет следующее:

- собственные представления типов данных **float** или **real** являются допустимыми;
- Данные в Юникоде имеют четную длину.

## <a name="data-types"></a>Типы данных

### <a name="string-to-decimal-data-type-conversions"></a>Преобразование символьного типа данных в десятичный

Преобразования символьного типа данных в десятичный, которые используются в инструкции BULK INSERT, следуют тем же правилам, что и функция [!INCLUDE[tsql](../../includes/tsql-md.md)] [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md), которая отклоняет числовые значения в экспоненциальном представлении. Такие строки инструкция BULK INSERT трактует как недопустимые и создает отчет ошибки преобразования.

Чтобы решить эту проблему, применяется файл форматирования, позволяющий выполнить массовый импорт данных типа **float** в экспоненциальном представлении в десятичный столбец. В файле форматирования необходимо явно описать столбец с типом данных **real** или **float**. Дополнительные сведения об этих типах данных см. в разделе [Типы данных float и real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md).

> [!NOTE]
> Файлы форматирования представляют данные **real** в виде типа данных **SQLFLT4**, а данные **float** — в виде типа данных **SQLFLT8**. Дополнительные сведения о файлах формата, отличного от XML, см. в разделе [Указание типа файлового хранилища с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).

#### <a name="example-of-importing-a-numeric-value-that-uses-scientific-notation"></a>Пример импорта числового значения в экспоненциальном представлении

В этом примере используется следующая таблица:

```sql
CREATE TABLE t_float(c1 FLOAT, c2 DECIMAL (5,4));
```

 Пользователю необходимо выполнить массовый импорт данных в таблицу `t_float`. Файл данных C:\t_float-c.dat содержит данные в экспоненциальном представлении **float**, например:

```input
8.0000000000000002E-28.0000000000000002E-2
```

Однако инструкция BULK INSERT не сможет выполнить импорт этих данных непосредственно в таблицу `t_float`, так как второй столбец `c2` имеет тип данных `decimal`. Поэтому необходим файл форматирования. В нем данные типа **float** в экспоненциальном представлении должны быть сопоставлены десятичному формату столбца `c2`.

Следующий файл форматирования использует тип данных `SQLFLT8` для сопоставления второго поля данных со вторым столбцом:

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="30"/>
<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30"/> </RECORD> <ROW>
<COLUMN SOURCE="1" NAME="c1" xsi:type="SQLFLT8"/>
<COLUMN SOURCE="2" NAME="c2" xsi:type="SQLFLT8"/> </ROW> </BCPFORMAT>
```

Чтобы задействовать этот файл форматирования (файл `C:\t_floatformat-c-xml.xml`) при импорте тестовых данных в тестовую таблицу, необходимо выполнить следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:

```sql
BULK INSERT bulktest..t_float
FROM 'C:\t_float-c.dat' WITH (FORMATFILE='C:\t_floatformat-c-xml.xml');
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="data-types-for-bulk-exporting-or-importing-sqlxml-documents"></a>Типы данных для массового экспорта или импорта документов SQLXML

Для массового экспорта или импорта данных SQLXML используется один из следующих типов данных в файле форматирования.

|Тип данных|Действие|
|---------------|------------|
|SQLCHAR или SQLVARCHAR|Данные отправляются в кодовой странице клиента или кодовой странице, определенной параметрами сортировки. Тот же эффект достигается указанием параметра DATAFILETYPE **='char'** без указания файла форматирования.|
|SQLNCHAR или SQLNVARCHAR|Данные отправляются в Юникоде. Тот же эффект достигается указанием параметра DATAFILETYPE **= 'widechar'** без указания файла форматирования.|
|SQLBINARY или SQLVARBIN|Данные отправляются без преобразования.|
| &nbsp; | &nbsp; |

## <a name="general-remarks"></a>Общие замечания

Сравнение инструкции BULK INSERT, инструкции INSERT ... SELECT \* FROM OPENROWSET(BULK...) и команды **bcp** см. в разделе [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).

Дополнительные сведения о подготовке данных к массовому импорту см. в разделе [Подготовка данных к массовому экспорту или импорту (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

Инструкция BULK INSERT может выполняться внутри пользовательской транзакции для импорта данных в таблицу или представление. При необходимости использовать несколько соответствий для массового импорта данных, в транзакции можно указать предложение BATCHSIZE в инструкции BULK INSERT. При откате транзакции, состоящей из нескольких пакетов, каждый из пакетов, который в процессе транзакции был отправлен в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], откатывается.

## <a name="interoperability"></a>Совместимость

### <a name="importing-data-from-a-csv-file"></a>Импорт данных из CSV-файла

Начиная с [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 инструкция BULK INSERT поддерживает формат CSV, как и база данных SQL Azure.
До [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 CSV-файлы с разделителями-запятыми не поддерживаются в операциях массового импорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Однако в некоторых случаях CSV-файл можно использовать в качестве файла данных для массового импорта данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения о требованиях к импорту данных из CSV-файла см. в разделе [Подготовка данных к массовому экспорту или импорту (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

## <a name="logging-behavior"></a>Режим ведения журнала

 Сведения о том, когда в журнале транзакций регистрируются операции вставки строк, выполняемые при массовом импорте в SQL Server, см. в статье [Предварительные условия для минимального протоколирования массового импорта данных](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md). Минимальный уровень ведения журнала не поддерживается в базе данных SQL Azure.

## <a name="restrictions"></a><a name="Limitations"></a> Ограничения

При использовании файла форматирования с BULK INSERT можно указать не более 1024 полей. Это значение совпадает с максимальным числом столбцов в таблице. При использовании файла форматирования для инструкции BULK INSERT с файлом данных, который содержит больше 1024 поля, формируется ошибка 4822. Программа [bcp](../../tools/bcp-utility.md) не имеет этого ограничения, поэтому для файлов данных, которые содержат больше 1024 поля, используйте инструкцию BULK INSERT без файла форматирования или команду **bcp**.

## <a name="performance-considerations"></a>Вопросы производительности

Если число страниц, которые должны быть записаны на диск в едином пакете, превышает внутренний порог, может быть произведен полный просмотр буферного пула для определения страниц, подлежащих записи на диск при фиксации пакета. Такой полный просмотр может повредить производительности массового импорта. Превышение внутреннего порога может возникнуть, если большой буферный пул работает с медленной подсистемой ввода-вывода. Избежать переполнения буфера на больших компьютерах можно либо отказавшись от использования указания TABLOCK (что удалит оптимизацию массовых операций), либо задав меньший размер пакета (что сохранит оптимизацию массовых операций).

В связи с различиями в компьютерах рекомендуется испытать свою рабочую нагрузку с различными размерами пакетов, чтобы выявить оптимальный вариант.

При использовании базы данных SQL Azure рекомендуется временно увеличить уровень производительности базы данных или экземпляра до импорта, если импортируется большой объем данных.

## <a name="security"></a>Безопасность

### <a name="security-account-delegation-impersonation"></a>Делегирование учетных записей безопасности (Олицетворение)

Если пользователь использует имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , то применяется профиль безопасности учетной записи процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . За пределами компонента Database Engine невозможно выполнить проверку подлинности имени входа, проходящего проверку подлинности SQL Server. Поэтому, если имя входа, использующее проверку подлинности SQL Server, инициирует команду BULK INSERT, подключение к данным устанавливается с помощью контекста безопасности учетной записи процесса SQL Server (учетной записи, которая используется службой SQL Server Database Engine). Для того чтобы прочитать исходные данные, учетной записи, которая используется службой SQL Server Database Engine, необходимо предоставить доступ к этим исходным данным. Если же пользователь [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] входит в систему с проверкой подлинности Windows, то ему доступны только те файлы, к которым имеет доступ учетная запись пользователя, независимо от профиля безопасности процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Если с помощью программы **sqlcmd** или **osql** инструкция BULK INSERT выполняется на одном компьютере, вставка данных происходит в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на другом компьютере, а аргумент *data_file* указывается на третьем компьютере с помощью UNC-пути, то может возникнуть ошибка 4861.

Чтобы решить эту проблему, воспользуйтесь проверкой подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и укажите имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которое использует профиль безопасности учетной записи процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], либо настройте Windows для делегирования учетных записей безопасности. Дополнительные сведения о том, как сделать учетную запись пользователя доступной для делегирования, см. в справке по Windows.

Дополнительные сведения о вопросах безопасности при использовании BULK INSERT см. в статье [Массовый импорт данных при помощи инструкции BULK INSERT или OPENROWSET(BULK...) (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

При импорте данных, которые не являются общедоступными (анонимный доступ), из хранилища BLOB-объектов Azure создайте [DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md) на основе ключа SAS, зашифрованного с помощью [MASTER KEY](create-master-key-transact-sql.md), а затем создайте [внешний источник базы данных](../../t-sql/statements/create-external-data-source-transact-sql.md) для использования в команде BULK INSERT. Пример см. в разделе [Импорт данных из файла в хранилище BLOB-объектов Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

### <a name="permissions"></a>Разрешения

Требует разрешений INSERT и ADMINISTER BULK OPERATIONS. В базе данных SQL Azure требуются разрешения INSERT и ADMINISTER DATABASE BULK OPERATIONS. Разрешения ADMINISTER BULK OPERATIONS или роль bulkadmin не поддерживаются для SQL Server на Linux. Операции массовой вставки для SQL Server на Linux можно выполнять только с помощью `sysadmin`. 

Кроме того, необходимо разрешение ALTER TABLE, если выполняется одно из следующих условий.

- Существуют ограничения, и параметр CHECK_CONSTRAINTS не указан.

   > [!NOTE]
   > Ограничения отключены по умолчанию. Чтобы проверить ограничения явно, укажите параметр CHECK_CONSTRAINTS.

- Триггеры существуют, и параметр FIRE_TRIGGER не указан.

   > [!NOTE]
   > По умолчанию, триггеры не срабатывают. Чтобы явно включить триггеры, укажите параметр FIRE_TRIGGER.

- Для импорта значений идентификаторов из файла данных указан параметр KEEPIDENTITY.

## <a name="examples"></a>Примеры

### <a name="a-using-pipes-to-import-data-from-a-file"></a>A. Применение символа вертикальной черты для импорта данных из файла

В следующем примере выполняется импорт подробных сведений о заказах из указанного файла данных в таблицу `AdventureWorks2012.Sales.SalesOrderDetail`, используя символ вертикальной черты (`|`) в качестве признака конца столбца и `|\n` в качестве признака конца строки.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
      (  
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR =' |\n'
      );
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="b-using-the-fire_triggers-argument"></a>Б. Применение аргумента FIRE_TRIGGERS

В следующем примере указывается аргумент `FIRE_TRIGGERS`.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
     (
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR = ':\n'
         , FIRE_TRIGGERS
      );
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="c-using-line-feed-as-a-row-terminator"></a>В. Применение символа перевода строки в качестве признака конца строки

 В следующем примере производится импорт файла, в котором в качестве признака конца строки используется символ перевода строки, как в файлах UNIX.

```sql
DECLARE @bulk_cmd VARCHAR(1000);
SET @bulk_cmd = 'BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
FROM ''<drive>:\<path>\<filename>''
WITH (ROWTERMINATOR = '''+CHAR(10)+''')';
EXEC(@bulk_cmd);
```

> [!NOTE]
> В соответствии с тем, как в Microsoft Windows обрабатываются текстовые файлы, **(\n** автоматически заменяется на **\r\n)** .

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="d-specifying-a-code-page"></a>Г. Указание кодовой страницы

В следующем примере показано указание кодовой страницы.

```sql
BULK INSERT MyTable
FROM 'D:\data.csv'
WITH
( CODEPAGE = '65001'
   , DATAFILETYPE = 'char'
   , FIELDTERMINATOR = ','
);
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="e-importing-data-from-a-csv-file"></a>Д. Импорт данных из CSV-файла

В следующем примере показано, как указать CSV-файл без заголовка (первая строка), используя `;` в качестве признака конца поля и `0x0a` в качестве признака конца строки.

```sql
BULK INSERT Sales.Invoices
FROM '\\share\invoices\inv-2016-07-25.csv'
WITH (FORMAT = 'CSV'
      , FIRSTROW=2
      , FIELDQUOTE = '\'
      , FIELDTERMINATOR = ';'
      , ROWTERMINATOR = '0x0a');
```

> [!IMPORTANT]
> База данных SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="f-importing-data-from-a-file-in-azure-blob-storage"></a>Е. Импорт данных из файла в хранилище BLOB-объектов Azure

В приведенном ниже примере показано, как загрузить данные из CSV-файла в расположение хранилища BLOB-объектов Azure, для которого был создан ключ SAS. Расположение хранилища BLOB-объектов Azure настроено как внешний источник данных. Для этого требуются учетные данные для базы с подписанным URL-адресом, зашифрованным с помощью главного ключа в пользовательской базе данных.

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
          LOCATION = 'https://****************.blob.core.windows.net/invoices'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (DATA_SOURCE = 'MyAzureBlobStorage');
```

> [!IMPORTANT]
> SQL Azure поддерживает только чтение из хранилища BLOB-объектов Azure.

### <a name="g-importing-data-from-a-file-in-azure-blob-storage-and-specifying-an-error-file"></a>Ж. Импорт данных из файла в хранилище BLOB-объектов Azure и определение файла с ошибкой

В следующем примере показано, как загрузить данные из CSV-файла в расположение хранилища BLOB-объектов Azure, которое была настроено в качестве внешнего источника данных, и определить файл с ошибкой. Для этого требуются учетные данные для базы с подписанным URL-адресом. Обратите внимание: при выполнении базы данных SQL Azure параметр ERRORFILE должен использоваться вместе с ERRORFILE_DATA_SOURCE. В противном случае импорт может завершиться с ошибкой разрешения. Файл, указанный в ERRORFILE, не должен существовать в контейнере.

```sql
BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (
         DATA_SOURCE = 'MyAzureInvoices'
         , FORMAT = 'CSV'
         , ERRORFILE = 'MyErrorFile'
         , ERRORFILE_DATA_SOURCE = 'MyAzureInvoices');
```

Полные примеры использования функции `BULK INSERT`, включая настройку учетных данных и внешнего источника данных, см. в статье [Примеры массового доступа к данным в хранилище BLOB-объектов Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="additional-examples"></a>Дополнительные примеры

 Другие примеры использования `BULK INSERT`содержатся в следующих разделах:

- [Примеры массового импорта и экспорта XML-документов (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Сохранение значений идентификаторов при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)
- [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)
- [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
- [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)
- [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>См. также:

- [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [Программа bcp](../../tools/bcp-utility.md)
- [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
- [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)
- [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)
- [Подготовка данных к массовому экспорту или импорту (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)