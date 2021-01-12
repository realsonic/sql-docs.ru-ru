---
description: CREATE EXTERNAL FILE FORMAT (Transact-SQL)
title: CREATE EXTERNAL FILE FORMAT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/08/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL FILE FORMAT
- CREATE_EXTERNAL_FILE_FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, file format
- PolyBase, external file format
ms.assetid: abd5ec8c-1a0e-4d38-a374-8ce3401bc60c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3e72de795b4572c010a315eb7fe02e8f10e731cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100839"
---
# <a name="create-external-file-format-transact-sql"></a>CREATE EXTERNAL FILE FORMAT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

Создает объект формата внешнего файла, определяя внешние данные, которые сохранены в Hadoop, Хранилище BLOB-объектов Azure или Azure Data Lake Store, либо данные для входных и выходных потоков, связанных с внешними потоками. Создание формата внешнего файла — обязательное условие для создания внешней таблицы. Создавая формат внешнего файла, вы указываете фактическую структуру данных, на которые ссылается внешняя таблица.  
  
Поддерживаются следующие форматы файлов.
  
- Текст с разделителями  
  
- Hive RCFile — не применяется к Azure Synapse Analytics.
  
- Hive ORC
  
- Parquet

- JSON — применяется только к SQL Azure для пограничных вычислений.


Инструкции по созданию внешней таблицы см. в разделе [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md).
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис
  
### <a name="delimited-text"></a>[Текст с разделителями](#tab/delimited)
```syntaxsql
-- Create an external file format for DELIMITED (CSV/TSV) files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
        FORMAT_TYPE = DELIMITEDTEXT  
    [ , FORMAT_OPTIONS ( <format_options> [ ,...n  ] ) ]  
    [ , DATA_COMPRESSION = {  
           'org.apache.hadoop.io.compress.GzipCodec'  
         | 'org.apache.hadoop.io.compress.DefaultCodec'  
        }  
     ]);

<format_options> ::=  
{  
    FIELD_TERMINATOR = field_terminator  
    | STRING_DELIMITER = string_delimiter 
    | First_Row = integer -- ONLY AVAILABLE FOR AZURE SYNAPSE ANALYTICS
    | DATE_FORMAT = datetime_format  
    | USE_TYPE_DEFAULT = { TRUE | FALSE } 
    | Encoding = {'UTF8' | 'UTF16'} 
}
```
### <a name="rc"></a>[RC](#tab/rc)
```syntaxsql
--Create an external file format for RC files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = {  
        'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'  
      | 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'  
    }  
    [ , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec' ]);
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="orc"></a>[ORC](#tab/orc)
```syntaxsql  
--Create an external file format for ORC file.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (
         FORMAT_TYPE = ORC  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.DefaultCodec'      }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="parquet"></a>[Parquet](#tab/parquet)
```syntaxsql
--Create an external file format for PARQUET files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
         FORMAT_TYPE = PARQUET  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      }  
    ]);    
```
### <a name="json"></a>[JSON](#tab/json)
```syntaxsql
-- Create an external file format for JSON files.
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = JSON  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      
      | 'org.apache.hadoop.io.compress.DefaultCodec'  }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

---
  
## <a name="arguments"></a>Аргументы  
*file_format_name*  
Задает имя формата внешнего файла.
 
### <a name="format_type"></a>FORMAT_TYPE 
`FORMAT_TYPE = [ PARQUET | ORC | RCFILE | DELIMITEDTEXT]`

Задает формат внешних данных.
  
- PARQUET Задает формат Parquet.
  
- ORC  
  Задает формат ORC. Для использования этого параметра во внешнем кластере Hadoop требуется Hive версии 0.11 или выше. В Hadoop формат файла ORC обеспечивает более качественное сжатие и более высокую производительность, чем формат файла RCFILE.

- RCFILE (в сочетании с SERDE_METHOD = *SERDE_method*) Задает формат файла RcFile. Этот параметр требует задания метода сериализации и десериализации куч (SerDe). То же требование действует при использовании Hive/HiveQL в Hadoop для отправки запросов файлам RC. Обратите внимание, что метод SerDe учитывает регистр.

  Примеры задания RCFile с двумя методами SerDe, которые поддерживаются PolyBase.

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'

- DELIMITEDTEXT Задает формат текста с разделителями столбцов (которые также называются признаками конца поля).
   
- JSON — определяет формат JSON. Применяется только к SQL Azure для пограничных вычислений. 

### <a name="data_compression"></a>DATA\_COMPRESSION
 `DATA_COMPRESSION = *data_compression_method*`  
 Задает метод сжатия данных для внешних данных. Если параметр DATA_COMPRESSION не задан, по умолчанию используются несжатые данные.
Для правильной работы сжатые Gzip файлы должны иметь файловое расширение .gz.
 
 #### <a name="delimited-text"></a>[Текст с разделителями](#tab/delimited)
 Тип формата DELIMITEDTEXT поддерживает следующие методы сжатия:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'

#### <a name="rc"></a>[RC](#tab/rc)
 Тип формата RCFILE поддерживает следующий метод сжатия:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
  
#### <a name="orc"></a>[ORC](#tab/orc)
 Тип файлового формата ORC поддерживает следующие методы сжатия:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="parquet"></a>[Parquet](#tab/parquet)
 Тип файлового формата PARQUET поддерживает следующие методы сжатия:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="json"></a>[JSON](#tab/json)
 Тип формата файлов JSON поддерживает следующие методы сжатия.
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
---  
### <a name="delimited-text-format-options"></a>Варианты форматов текста с разделителями

Варианты форматов, описанные в этом разделе, являются необязательными и применяются только к текстовым файлам с разделителями.

#### <a name="field_terminator"></a>FIELD_TERMINATOR
`FIELD_TERMINATOR = *field_terminator*`  
Применяется только для текстовых файлов с разделителями. Признак конца поля задает один или несколько символов, отмечающих окончание каждого поля (столбца) в файле с разделителями текста. По умолчанию используется вертикальная черта (|). Для гарантированной поддержки рекомендуется использовать один или несколько символов ascii.
  
  
Примеры:  
  
-   FIELD_TERMINATOR = '|'  
  
-   FIELD_TERMINATOR = ' '  
  
-   FIELD_TERMINATOR = \t  
  
-   FIELD_TERMINATOR = '~|~'  
  
#### <a name="string_delimiter"></a>STRING_DELIMITER
`STRING_DELIMITER = *string_delimiter*`

Задает признак конца поля для данных типа "строка" в файле с разделителями текста. Разделитель строк имеет длину один или несколько символов и заключен в одинарные кавычки. По умолчанию используется пустая строка "". Для гарантированной поддержки рекомендуется использовать один или несколько символов ascii.
 
  
 Примеры:  

-   STRING_DELIMITER = '"'

-   STRING_DELIMITER = '0x22' — шестнадцатеричная двойная кавычка
  
-   STRING_DELIMITER = '*'  
  
-   STRING_DELIMITER = ,  
  
-   STRING_DELIMITER = '0x7E0x7E' — две тильды (например, ~~)
 
#### <a name="first_row"></a>FIRST_ROW
 `FIRST_ROW = *First_row_int*`  
Задает номер строки, которая читается первой во всех файлах во время загрузки PolyBase. Этот параметр может принимать значения 1–15. Если задано значение 2, первая строка в каждом файле (строка заголовка) при загрузке данных пропускается. Строки пропускаются по признакам конца строк (/r/n, /r, /n). Если для экспорта используется этот вариант, строки добавляются в данные, чтобы гарантировать возможность прочтения файла без потери данных. Если задано значение больше 2, первой экспортируется строка с названиями столбцов внешней таблицы.

#### <a name="date_format"></a>DATE\_FORMAT
 `DATE_FORMAT = *datetime_format*`  
Задает пользовательский формат для всех данных о дате и времени, которые могут отображаться в текстовом файле с разделителями. Если исходный файл использует стандартные форматы даты и времени, этот параметр не является обязательным. Можно использовать не более одного пользовательского формата даты и времени на файл. Невозможно указать для файла более одного пользовательского формата даты и времени. Однако можно использовать несколько форматов даты и времени, если каждый из них является форматом по умолчанию для соответствующего типа данных в определении внешней таблицы.

> [!IMPORTANT]
> PolyBase использует пользовательский формат даты только для импорта данных. Для записи данных во внешний файл пользовательский формат не используется.

 Если параметр DATE_FORMAT не задан или является пустой строкой, PolyBase использует следующие форматы по умолчанию:
  
-   DateTime: 'yyyy-MM-dd HH:mm:ss'  
  
-   SmallDateTime: 'yyyy-MM-dd HH:mm'  
  
-   Date: 'yyyy-MM-dd'  
  
-   DateTime2: 'yyyy-MM-dd HH:mm:ss'  
  
-   DateTimeOffset: 'yyyy-MM-dd HH:mm:ss'  
  
-   Time: 'HH:mm:ss'  

> [!IMPORTANT]
> При использовании пользовательского значения `DATE_FORMAT` все форматы типов по умолчанию будут переопределены. Это означает, что вам потребуется использовать одинаковые форматы даты в ячейках с датой и временем, датой и временем в ваших файлах. При переопределении `DATE_FORMAT` вы не сможете использовать значения даты и времени в разных форматах.

**Примеры форматов даты** приводятся в следующей таблице:
  
Примечания об этой таблице:  
  
-   Год, месяц и день могут иметь различные форматы и порядок. В таблице показан только формат **ymd**. Месяц может обозначаться одной или двумя цифрами или тремя символами. День может обозначаться одной или двумя цифрами. Год может обозначаться двумя или четырьмя цифрами.
  
-   Миллисекунды (fffffff) не требуются.
  
-   Указание времени суток (tt) не является обязательным. По умолчанию используется время AM (до полудня).
  
|Тип данных|Пример|Описание|  
|---------------|-------------|-----------------|  
|Дата и время|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'|Кроме года, месяца и дня этот формат данных включает 00-24 часа, 00-59 минут, 00-59 секунд и трехзначное обозначение миллисекунд.|  
|Дата и время|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffftt'|Кроме года, месяца и дня этот формат данных включает 00-12 часов, 00-59 минут, 00-59 секунд и трехзначное обозначение миллисекунд и указание времени суток: AM, am, PM или pm. |  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd HH:mm'|Кроме года, месяца и дня этот формат данных включает 00-23 часа, 00-59 минут.|  
|SmallDateTime|DATE_FORMAT =  'yyyy-MM-dd hh:mmtt'|Кроме года, месяца и дня этот формат данных включает 00-11 часов, 00-59 минут и указание времени суток: AM, am, PM или pm. Секунды не указаны.|  
|Дата|DATE_FORMAT =  'yyyy-MM-dd'|Год, месяц и день. Элемент времени не включен.|  
|Дата|DATE_FORMAT = 'yyyy-MMM-dd'|Год, месяц и день. Если используется трехзначное обозначение месяца, входящее значение — это одна из строк янв, фев, мар, апр, май, июн, июл, авг, сен, окт, ноя или дек.|  
|datetime2|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff'|Кроме года, месяца и дня этот формат данных включает 00-23 часа, 00-59 минут, 00-59 секунд и семизначное обозначение миллисекунд.|  
|datetime2|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt'|Кроме года, месяца и дня этот формат данных включает 00-11 часов, 00-59 минут, 00-59 секунд и семизначное обозначение миллисекунд и указание времени суток: AM, am, PM или pm.|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fffffff zzz'|Кроме года, месяца и дня этот формат данных включает 00-23 часа, 00-59 минут, 00-59 секунд, семизначное обозначение миллисекунд и смещение часового пояса, которое указывается во входящем файле в виде `{+&#124;-}HH:ss`. Например, так как время в Лос-Анджелесе без перехода на летнее время на 8 часов отстает от времени UTC, значение -08:00 во входящем файле задает часовой пояс для Лос-Анджелеса.|  
|DateTimeOffset|DATE_FORMAT = 'yyyy-MM-dd hh:mm:ss.ffffffftt zzz'|Кроме года, месяца и дня этот формат данных включает 00-11 часов, 00-59 минут, 00-59 секунд и семизначное обозначение миллисекунд, указание времени суток (AM, am, PM или pm) и смещение часового пояса. См. в описании в предыдущей строке.|  
|Time|DATE_FORMAT = 'HH:mm:ss'|Нет значения даты, только 00-23 часа 00-59 минут и 00-59 секунд.|  
  
 ##### <a name="supported-date-and-time-formats"></a>Поддерживаемые форматы даты и времени
 
 Формат внешнего файла может описывать большое число форматов даты и времени:
  
|DATETIME|smalldatetime|Дата|datetime2|datetimeoffset|  
|--------------|-------------------|----------|---------------|--------------------|  
|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fff]|[M[M]]M-[d]d-[yy]yy HH:mm[:00]|[M[M]]M-[d]d-[yy]yy|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff]|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm[:00][tt]||[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fff]|[M[M]]M-[yy]yy-[d]d HH:mm[:00]|[M[M]]M-[yy]yy-[d]d|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff]|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm[:00][tt]||[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fff]|[yy]yy-[M[M]]M-[d]d HH:mm[:00]|[yy]yy-[M[M]]M-[d]d|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm[:00][tt]||[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fff]|[yy]yy-[d]d-[M[M]]M HH:mm[:00]|[yy]yy-[d]d-[M[M]]M|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm[:00][tt]||[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fff]|[d]d-[M[M]]M-[yy]yy HH:mm[:00]|[d]d-[M[M]]M-[yy]yy|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff]|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm[:00][tt]||[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fff]|[d]d-[yy]yy-[M[M]]M HH:mm[:00]|[d]d-[yy]yy-[M[M]]M|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm[:00][tt]||[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
  
 Сведения.  
  
-   Для разделения значений месяца, дня и года можно использовать символы "-", "/" или ".". Для простоты в таблице используется только разделитель "-".
  
-   Чтобы указать месяц в виде текста, используйте три и более символов. Обозначения месяца из одного или двух символов интерпретируются как число.
  
-   Для разделения значений времени используйте символ ":".
  
-   Буквы в квадратных скобках необязательны.
  
-   Буквы tt обозначают время суток [AM|PM|am|pm]. По умолчанию используется AM. Если задано значение tt, значение часа (hh) должно находиться в диапазоне от 0 до 12.
  
-   Буквы zzz обозначают смещение часового пояса для текущего часового пояса системы в формате {+ |-} HH:ss].
 
#### <a name="use_type_default"></a>USE_TYPE_DEFAULT 
 `USE_TYPE_DEFAULT = { TRUE | FALSE }`
 
 Указывает способ обработки отсутствующих значений в текстовых файлах с разделителями, когда PolyBase извлекает данные из текстового файла.
  
 TRUE  
 При извлечении данных из текстового файла сохраните каждое отсутствующее значение, воспользовавшись значением по умолчанию для типа данных в соответствующем столбце в определении внешней таблицы. Например, замените отсутствующее значение следующим:  
  
-   0, если столбец определен как числовой. Столбцы decimal не поддерживаются и приводят к ошибке.
  
-   Пустая строка "", если столбец является строковым.
  
-   1900-01-01, если столбец является столбцом дат.
  
 **FALSE**  
 Сохраните все отсутствующие значения как NULL. Любые значения NULL, сохраненные с использованием слова NULL в текстовом файле с разделителями, импортируются в качестве строки NULL.
 
#### <a name="encoding"></a>ENCODING
   `Encoding = {'UTF8' | 'UTF16'}`
   
 В [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] (APS CU7.4) PolyBase может выполнять чтение текстовых файлов с разделителями в кодировке UTF8 и UTF16-LE. В SQL Server PolyBase не поддерживает чтение файлов в кодировке UTF16.

## <a name="permissions"></a>Разрешения  
 Требуется разрешение ALTER ANY EXTERNAL FILE FORMAT.
  
## <a name="general-remarks"></a>Общие замечания
 Формат внешнего файла действует на уровне баз данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Он действует на уровне сервера в [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 Все варианты форматов являются необязательными и применяются только к текстовым файлам с разделителями.  
  
 Если данные хранятся в одном из сжатых форматов, PolyBase сначала распаковывает данные и только потом возвращает записи данных.
  
## <a name="limitations-and-restrictions"></a>Ограничения
  
 Разделитель строк в текстовых файлах с разделителями должен поддерживаться объектом LineRecordReader в Hadoop. То есть необходимо использовать следующие разделители: \r, \n или \r\n. Эти разделители не настраиваются пользователем.
  
 Сочетания поддерживаемых методов SerDe с RCFiles, а также поддерживаемые методы сжатия данных перечислены ранее в этой статье. Поддерживаются не все комбинации.
  
 Максимальное число одновременно выполняющихся процессов PolyBase — 32. При выполнении 32 параллельных запросов каждый запрос может прочитать не более 33 000 файлов из внешнего файлового расположения. Корневая папка и каждая вложенная папка тоже считаются файлами. Если степень параллелизма меньше 32, внешнее файловое расположение может содержать более 33 000 файлов.
  
 Из-за ограничения на количество файлов во внешней таблице рекомендуется хранить не более 30 000 файлов в корневой и вложенных папках внешнего файлового расположения. Кроме того, рекомендуется не создавать много вложенных папок в корневом каталоге. При ссылке на слишком большое число файлов может возникнуть исключение, связанное с нехваткой памяти на виртуальной машине Java.
  
  При экспорте данных в Hadoop или хранилище BLOB-объектов Azure с помощью PolyBase передаются только данные без имен столбцов (метаданных), как определено в команде CREATE EXTERNAL TABLE.

## <a name="locking"></a>Блокировка  
 Принимает совмещаемую блокировку на объекте EXTERNAL FILE FORMAT.
  
## <a name="performance"></a>Производительность
 При использовании сжатых файлов всегда приходится идти на компромисс: перенести меньше данных между внешним источником данных и SQL Server, но увеличить использование ресурсов ЦП для сжатия и распаковывания данных.
  
 Сжатые текстовые файлы Gzip разделить невозможно. Чтобы повысить производительность сжатых текстовых файлов Gzip, рекомендуется создать несколько хранимых в одном каталоге файлов во внешнем источнике данных. Такая файловая структура позволяет PolyBase читать и распаковывать данные быстрее, используя несколько процессов чтения и распаковывания. Оптимальное количество сжатых файлов — максимальное число процессов чтения данных на вычислительном узле. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] максимальное число процессов чтения данных на узел составляет 8 для каждого узла. В [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 2-го поколения это число составляет 20. В [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] максимальное число процессов чтения данных на узел варьируется в зависимости от SLO. Дополнительные сведения см. в разделе [Шаблоны и стратегии загрузки [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-create-a-delimitedtext-external-file-format"></a>A. Создание формата внешнего файла DELIMITEDTEXT  
 В этом примере создается формат внешнего файла *textdelimited1* для текстового файла с разделителями. Параметры, заданные для FORMAT\_OPTIONS, указывают, что поля в файле необходимо разделять вертикальной чертой (|). Текстовый файл также сжимается с помощью кодека Gzip. Если параметр DATA\_COMPRESSION не задан, сжатие текстового файла не выполняется.
  
 Для текстового файла с разделителями в качестве метода сжатия данных может использоваться либо кодек по умолчанию (org.apache.hadoop.io.compress.DefaultCodec), либо кодек Gzip (org.apache.hadoop.io.compress.GzipCodec).
  
```sql  
CREATE EXTERNAL FILE FORMAT textdelimited1  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,  
    FORMAT_OPTIONS (  
        FIELD_TERMINATOR = '|',  
        DATE_FORMAT = 'MM/dd/yyyy' ),  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
);  
```  
  
### <a name="b-create-an-rcfile-external-file-format"></a>Б. Создание формата внешнего файла RCFile  
 В этом примере создается формат внешнего файла для RCFile, использующего метод сериализации/десериализации org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe. Кроме того, указано, что в качестве метода сжатия данных необходимо использовать кодек по умолчанию. Если параметр DATA_COMPRESSION не задан, по умолчанию сжатие не выполняется.
  
```sql  
CREATE EXTERNAL FILE FORMAT rcfile1  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe',  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'  
);  
```  
  
### <a name="c-create-an-orc-external-file-format"></a>В. Создание формата внешнего файла ORC  
 В этом примере создается формат внешнего файла для файла ORC, который сжимает данные с помощью метода сжатия данных org.apache.io.compress.SnappyCodec. Если параметр DATA_COMPRESSION не задан, по умолчанию сжатие не выполняется.
  
```sql 
CREATE EXTERNAL FILE FORMAT orcfile1  
WITH (  
    FORMAT_TYPE = ORC,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
  
### <a name="d-create-a-parquet-external-file-format"></a>Г. Создание формата внешнего файла PARQUET  
 В этом примере создается формат внешнего файла для файла Parquet, который сжимает данные с помощью метода сжатия данных org.apache.io.compress.SnappyCodec. Если параметр DATA_COMPRESSION не задан, по умолчанию сжатие не выполняется.  
  
```sql  
CREATE EXTERNAL FILE FORMAT parquetfile1  
WITH (  
    FORMAT_TYPE = PARQUET,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
### <a name="e-create-a-delimited-text-file-skipping-header-row-azure-synapse-analytics-only"></a>Д. Создание текстового файла с разделителями с пропуском строки заголовка (только для Azure Synapse Analytics)
 В этом примере создается формат внешнего файла для файла CSV с одной строкой заголовка. 
  
```sql  
CREATE EXTERNAL FILE FORMAT skipHeader_CSV
WITH (FORMAT_TYPE = DELIMITEDTEXT,
      FORMAT_OPTIONS(
          FIELD_TERMINATOR = ',',
          STRING_DELIMITER = '"',
          FIRST_ROW = 2, 
          USE_TYPE_DEFAULT = True)
)
```   
### <a name="f-create-a-json-external-file-format"></a>Е. Создание формата внешнего файла JSON  
 В этом примере создается формат внешнего файла для файла JSON, который сжимает данные с помощью метода сжатия данных org.apache.io.compress.SnappyCodec. Если параметр DATA_COMPRESSION не задан, по умолчанию сжатие не выполняется. Этот пример применяется к SQL Azure для пограничных вычислений и в настоящее время не поддерживается для других продуктов SQL. 
  
```sql  
CREATE EXTERNAL FILE FORMAT jsonFileFormat  
WITH (  
    FORMAT_TYPE = JSON,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  

## <a name="see-also"></a>См. также:
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
 [sys.external_file_formats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)  
