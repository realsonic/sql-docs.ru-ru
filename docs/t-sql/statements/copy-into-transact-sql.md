---
title: COPY INTO (Transact-SQL)
titleSuffix: (Azure Synapse Analytics) - SQL Server
description: Использование инструкции COPY в Azure Synapse Analytics для загрузки данных из внешних учетных записей хранения.
ms.date: 09/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COPY_TSQL
- COPY INTO
- COPY
- LOAD
dev_langs:
- TSQL
author: kevinvngo
ms.author: kevin
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 43a5cc755eb07fe80a0d33d6b5b892e2a65ab21e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638830"
---
# <a name="copy-transact-sql"></a>COPY (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

В этой статье объясняется, как использовать инструкцию COPY в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] для загрузки данных из внешних учетных записей хранения. Инструкция COPY обеспечивает наибольшую гибкость приема данных с высокой пропускной способностью в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. COPY предоставляет следующие возможности:

- выполнять загрузку пользователям с более низкими привилегиями, без ограничения разрешений на управление хранилищем данных;
- выполнять одну инструкцию T-SQL без необходимости создавать дополнительные объекты базы данных;
- правильно анализировать и загружать CSV-файлы, где **разделители** (строк, полей, записей) **опускаются** **в столбцах с разделителями строк;**
- указывать более детализированную модель разрешений без предоставления ключей учетной записи хранения с помощью подписанных URL-адресов (SAS);
- использовать другую учетную запись хранения для расположения ERRORFILE (REJECTED_ROW_LOCATION);
- Настроить значения по умолчанию для каждого целевого столбца и указать поля исходных данных для загрузки в определенные целевые столбцы.
- Указать пользовательский символ конца строки для CSV-файлов.
- Использовать форматы даты SQL Server для CSV-файлов.
- Указать подстановочные знаки и несколько файлов в пути места хранения.

Подробные примеры и краткие руководства по использованию инструкции COPY см. в следующей документации.

- [Краткое руководство. Массовая загрузка данных с помощью инструкции COPY](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql)
- [Краткое руководство. Примеры использования инструкции COPY и поддерживаемых ею методов проверки подлинности](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples)
- [Краткое руководство. Создание инструкции COPY с помощью расширенного пользовательского интерфейса Synapse Studio](/azure/synapse-analytics/quickstart-load-studio-sql-pool)

## <a name="syntax"></a>Синтаксис  

```syntaxsql
COPY INTO [schema.]table_name
[(Column_list)] 
FROM '<external_location>' [,...n]
WITH  
 ( 
 [FILE_TYPE = {'CSV' | 'PARQUET' | 'ORC'} ]
 [,FILE_FORMAT = EXTERNAL FILE FORMAT OBJECT ]  
 [,CREDENTIAL = (AZURE CREDENTIAL) ]
 [,ERRORFILE = '[http(s)://storageaccount/container]/errorfile_directory[/]]' 
 [,ERRORFILE_CREDENTIAL = (AZURE CREDENTIAL) ]
 [,MAXERRORS = max_errors ] 
 [,COMPRESSION = { 'Gzip' | 'DefaultCodec'| 'Snappy'}] 
 [,FIELDQUOTE = 'string_delimiter'] 
 [,FIELDTERMINATOR =  'field_terminator']  
 [,ROWTERMINATOR = 'row_terminator']
 [,FIRSTROW = first_row]
 [,DATEFORMAT = 'date_format'] 
 [,ENCODING = {'UTF8'|'UTF16'}] 
 [,IDENTITY_INSERT = {'ON' | 'OFF'}]
)
```

## <a name="arguments"></a>Аргументы  

*schema_name*  
Этот аргумент необязателен, если схемой по умолчанию для пользователя, выполняющего операцию, является схема указанной таблицы. Если аргумент *schema* не указан и схема по умолчанию для пользователя, выполняющего операцию COPY, отличается от схемы таблицы, операция COPY отменяется и возвращается сообщение об ошибке.  

*table_name*  
Имя таблицы, в которую копируются данные. Целевой таблицей может быть временная или постоянная таблица, которая уже должна существовать в базе данных. 

*(column_list)*  
Необязательный список из одного или нескольких столбцов, используемых для сопоставления полей исходных данных со столбцами в целевой таблице для загрузки данных. Список *column_list* должен быть заключен в круглые скобки, а его элементы должны разделяться запятыми. Список столбцов указывается в следующем формате:

[(Column_name [Default_value] [Field_number] [,...n])]

- *Column_name* — имя столбца в целевой таблице.
- *Default_value* — значение по умолчанию, которое заменит любое значение NULL во входном файле. Значение по умолчанию применяется ко всем форматам файлов. Если столбец не указан в списке столбцов или существует пустое поле входного файла, COPY попытается загрузить значение NULL из входного файла.
- *Field_number* — номер поля входного файла, который будет сопоставлен с именем целевого столбца.
- Индексирование полей начинается с 1.

Если список столбцов не указан, COPY будет сопоставлять столбцы на основе исходного и целевого порядковых номеров: поле ввода 1 — с целевым столбцом 1, поле 2 — со столбцом 2 и т. д.

*Внешние расположения*</br>
Места размещения файлов, содержащих данные. Сейчас поддерживаются Azure Data Lake Storage (ADLS) 2-го поколения и хранилище BLOB-объектов Azure:

- *Внешнее расположение* для хранилища BLOB-объектов: https://<account>. blob.core.windows.net/<container>/<path>
- *Внешнее расположение* для ADLS 2-го поколения: https://<account>. dfs.core.windows.net/<container>/<path>

> [!NOTE]  
> Конечная точка .blob доступна для ADLS 2-го поколения и в настоящее время обеспечивает наилучшую производительность. Используйте конечную точку .blob, если для способа проверки подлинности не требуется .dfs.

- *Account* — имя учетной записи хранения.

- *Container* — имя контейнера BLOB-объектов.

- *Path* — путь к папке или файлу данных. Расположение начинается с контейнера. Если папка указана, COPY извлечет все файлы из папки и всех ее вложенных папок. COPY игнорирует скрытые папки и не возвращает файлы, которые начинаются с символа подчеркивания (_) или точки (.), если это явно не указано в пути. Так происходит даже при указании пути с подстановочным знаком.

Подстановочные знаки можно указывать в пути, где

- при сопоставлении имени пути с подстановочными знаками учитывается регистр;
- подстановочный знак может быть экранирован с помощью символа обратной косой черты (\\);
- расширение подстановочных знаков применяется рекурсивно. Например, все CSV-файлы в каталоге Customer1 (включая его подкаталоги) будут загружены, как показано в следующем примере: "Account/Container/Customer1/*.csv"

> [!NOTE]  
> Для обеспечения оптимальной производительности не рекомендуется указывать подстановочные знаки, которые увеличат количество файлов. Если возможно, не указывайте подстановочные знаки, а задайте несколько местоположений файлов.

Их можно указать только из одной учетной записи хранения и контейнера в виде списка с разделителями-запятыми, например:

- "https://<account>.blob.core.windows.net/<container>/<path>", "https://<account>.blob.core.windows.net/<container>/<path>"…

*FILE_TYPE = { "CSV" | "PARQUET" | "ORC" }*</br>
*FILE_TYPE* задает формат внешних данных.

- CSV: указывает файл данных с разделителями-запятыми, соответствующий стандарту [RFC 4180](https://tools.ietf.org/html/rfc4180).
- PARQUET: задает формат Parquet.
- ORC: Задает формат ORC.

>[!NOTE]  
>Тип файла "текст с разделителями" в Polybase заменен форматом CSV-файла, в котором разделитель-запятую по умолчанию можно настроить с помощью параметра FIELDTERMINATOR. 

*FILE_FORMAT = external_file_format_name*</br>
*FILE_FORMAT* применяется только к файлам Parquet и ORC и задает имя объекта формата внешнего файла, который хранит тип файла и метод сжатия внешних данных. Чтобы создать формат внешнего файла, используйте [CREATE EXTERNAL FILE FORMAT](create-external-file-format-transact-sql.md).

*CREDENTIAL (IDENTITY = ", SECRET = ")*</br>
*CREDENTIAL* задает механизм проверки подлинности для доступа к внешней учетной записи хранения. Ниже приведены методы проверки подлинности.

|                          |                CSV                |                      Parquet                       |                        ORC                         |
| :----------------------: | :-------------------------------: | :------------------------------------------------: | :------------------------------------------------: |
|  **Хранилище BLOB-объектов Azure**  | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD |                      SAS/KEY                       |                      SAS/KEY                       |
| **Azure Data Lake 2-го поколения** | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD |

1: Для этого способа проверки подлинности требуется конечная точка .blob ( **.blob**.core.windows.net) во внешнем пути к папке.

2: Для этого способа проверки подлинности требуется конечная точка .dfs ( **.dfs**.core.windows.net) во внешнем пути к папке.


> [!NOTE]  
>
> - При проверке подлинности в AAD или общедоступной учетной записи хранения указывать CREDENTIAL не требуется. 
> - Если ваша учетная запись хранения связана с виртуальной сетью, необходимо пройти проверку подлинности с использованием MSI (управляемое удостоверение).

- Проверка подлинности на основе подписанных URL-адресов (SAS)
  
  - *IDENTITY: константа со значением подписанного URL-адреса*
  - *SECRET:*  [*Подписанный URL-адрес*](/azure/storage/common/storage-sas-overview) *обеспечивает делегированный доступ к ресурсам в вашей учетной записи хранения.*
  -  Минимальные требуемые разрешения: READ и LIST
  
- Проверка подлинности с помощью [*субъектов-служб*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)

  - *IDENTITY: <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET: ключ субъекта-службы приложения AAD*
  -  Минимальные требуемые роли RBAC: участник для данных BLOB-объектов хранилища, владелец данных BLOB-объектов хранилища или модуль чтения данных BLOB-объектов хранилища

- Проверка подлинности с помощью ключа учетной записи хранения
  
  - *IDENTITY: константа со значением ключа учетной записи хранения*
  - *SECRET: ключ учетной записи хранения*
  
- Проверка подлинности с помощью [ управляемого удостоверения](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (конечные точки службы виртуальной сети)
  
  - *IDENTITY: константа со значением управляемого удостоверения*
  - Минимальные требуемые роли RBAC: участник для данных BLOB-объектов хранилища или владелец данных BLOB-объектов хранилища для зарегистрированного сервера базы данных SQL AAD
  
- Проверка подлинности пользователя AAD
  
  - *Параметр CREDENTIAL не требуется*
  - Минимальные требуемые роли RBAC: участник для данных BLOB-объектов хранилища или владелец данных BLOB-объектов хранилища для пользователя AAD

*ERRORFILE = Directory Location*</br>
*ERRORFILE* применяется только к CSV-файлам. Указывает каталог в инструкции COPY, в который должны записываться отклоненные строки и соответствующий файл ошибок. Можно указать полный путь из учетной записи хранения или путь относительно контейнера. Если указанный путь не существует, он будет создан от вашего имени. Создается дочерний каталог с именем "_rejectedrows". Благодаря символу "_ " каталог исключается из других процессов обработки данных, если он явно не указан в параметре LOCATION. 

В этом каталоге создается папка, имя которой соответствует времени отправки загруженных данных в формате "ГодМесяцДень-ЧасМинутаСекунда" (например, 20180330-173205). В эту папку записываются два типа файлов: файл причины (ошибка) и файл данных (строка), к каждому из которых предварительно добавляется queryID, distributionID и GUID файла. Так как данные и причина хранятся в отдельных файлах, эти файлы имеют соответствующие префиксы.

Если ERRORFILE указывает полный путь к определенной учетной записи хранения, для подключения к этому хранилищу будет использоваться ERRORFILE_CREDENTIAL. В противном случае будет применяться значение, указанное для CREDENTIAL.

*ERRORFILE_CREDENTIAL (IDENTITY = ", SECRET = ")*</br>
*ERRORFILE_CREDENTIAL* применяется только к CSV-файлам. Поддерживаемые источники данных и методы проверки подлинности:

- Хранилище BLOB-объектов Azure — SAS/SERVICE PRINCIPAL/KEY/AAD
- Azure Data Lake 2-го поколения — SAS/MSI/SERVICE PRINCIPAL/KEY/AAD
  
- Проверка подлинности на основе подписанных URL-адресов (SAS)
  - *IDENTITY: константа со значением подписанного URL-адреса*
  - *SECRET:*  [*Подписанный URL-адрес*](/azure/storage/common/storage-sas-overview) *обеспечивает делегированный доступ к ресурсам в вашей учетной записи хранения.*
  - Минимальные требуемые разрешения: READ, LIST, WRITE, CREATE, DELETE
  
- Проверка подлинности с помощью [*субъектов-служб*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)
  - *IDENTITY: <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET: ключ субъекта-службы приложения AAD*
  - Минимальные требуемые роли RBAC: участник для данных BLOB-объектов хранилища или владелец данных BLOB-объектов хранилища
  
> [!NOTE]  
> Используйте конечную точку токена OAuth 2.0 **V1**.

- Проверка подлинности с помощью ключа учетной записи хранения
  - *IDENTITY: константа со значением ключа учетной записи хранения*
  - *SECRET: ключ учетной записи хранения*
  
- Проверка подлинности с помощью [ управляемого удостоверения](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (конечные точки службы виртуальной сети)
  - *IDENTITY: константа со значением управляемого удостоверения*
  - Минимальные требуемые роли RBAC: участник для данных BLOB-объектов хранилища или владелец данных BLOB-объектов хранилища для зарегистрированного сервера базы данных SQL AAD
  
- Проверка подлинности пользователя AAD
  - *Параметр CREDENTIAL не требуется*
  - Минимальные требуемые роли RBAC: участник для данных BLOB-объектов хранилища или владелец данных BLOB-объектов хранилища для пользователя AAD

> [!NOTE]  
> Если вы используете ту же учетную запись хранения для ERRORFILE и указываете путь ERRORFILE относительно корня контейнера, указывать ERROR_CREDENTIAL не нужно.

*MAXERRORS = max_errors*</br>
*MAXERRORS* указывает максимальное число отклоненных строк, допустимых в загрузке, прежде чем операция COPY будет отменена. Каждая строка, импорт которой не может быть выполнен операцией COPY, пропускается и считается за одну ошибку. Если аргумент max_errors не указан, значение по умолчанию равно 0.

*COMPRESSION = { 'DefaultCodec '\| ’Snappy’ \| ‘GZIP’ \| ‘NONE’}*</br>
*COMPRESSION* является необязательным и задает метод сжатия данных для внешних данных.

- CSV-файлы поддерживают GZIP.
- Parquet поддерживает GZIP и Snappy.
- ORC поддерживает DefaultCodec и Snappy.
- Zlib является методом сжатия по умолчанию для ORC.

Если этот параметр не указан, команда COPY автоматически определит тип сжатия на основе расширения файла:

- .gz — **GZIP**
- .snappy — **Snappy**
- .deflate — **DefaultCodec** (только для Parquet и ORC)

 *FIELDQUOTE = "field_quote"*</br>
*FIELDQUOTE* применяется к CSV-файлам и задает отдельный символ, который будет использоваться в качестве символа кавычки (строкового разделителя) в CSV-файле. Если этот символ не задан, в качестве символа кавычки будет использоваться символ (") согласно стандарту RFC 4180. Кодировка UTF-8 для FIELDQUOTE не поддерживает символы расширенного набора ASCII и многобайтовые символы.

> [!NOTE]  
> Символы FIELDQUOTE экранируются в строковых столбцах, где есть двойной символ FIELDQUOTE (разделитель). 

*FIELDTERMINATOR = "field_terminator"*</br>
*FIELDTERMINATOR* применяется только к CSV-файлам. Указывает признак конца поля, который будет использоваться в CSV-файле. Признак конца поля можно указать в шестнадцатеричном представлении. Признак конца поля может состоять из нескольких символов. Признак конца поля по умолчанию — (,). Кодировка UTF-8 для FIELDTERMINATOR не поддерживает символы расширенного набора ASCII и многобайтовые символы.

ROW TERMINATOR = "row_terminator"</br>
*ROW TERMINATOR* применяется только к CSV-файлам. Указывает признак конца строки, который будет использоваться в CSV-файле. Признак конца строки можно указать в шестнадцатеричном представлении. Признак конца строки может состоять из нескольких символов. Признак конца строки по умолчанию — \r\n. 

Команда COPY добавляет префикс в виде символа \r при указании \n (новой строки), в результате чего получается \r\n. Чтобы указать только символ \n, используйте шестнадцатеричное представление (0x0A). При указании многосимвольных признаков конца строки в шестнадцатеричном формате не указывайте 0x между каждым символом.

Кодировка UTF-8 для ROW TERMINATOR не поддерживает символы расширенного набора ASCII и многобайтовые символы.

*FIRSTROW  = First_row_int*</br>
*FIRSTROW* применяется к CSV-файлам и указывает номер строки, которая считывается первой во всех файлах для команды COPY. Значения начинаются с 1. 1 — значение по умолчанию. Если задано значение 2, первая строка в каждом файле (строка заголовка) при загрузке данных пропускается. Строки пропускаются по признакам конца строк.

*DATEFORMAT = { ‘mdy’ \| ‘dmy’ \| ‘ymd’ \| ‘ydm’ \| ‘myd’ \| ‘dym’ }*</br>
DATEFORMAT применяется только к CSV-файлам и задает формат даты для сопоставления с форматами дат SQL Server. Обзор всех типов данных и функций даты и времени в языке Transact-SQL см. в статье [Типы данных и функции даты и времени (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md). Параметр DATEFORMAT в команде COPY имеет более высокий приоритет, чем [параметр DATEFORMAT, настроенный на уровне сеанса](set-dateformat-transact-sql.md).

*ENCODING = "UTF8" | "UTF16"*</br>
*ENCODING* применяется только к CSV-файлам. По умолчанию используется UTF8. Задает стандарт кодирования данных для файлов, загруженных командой COPY. 

*IDENTITY_INSERT = "ON" | "OFF"*</br>
IDENTITY_INSERT указывает, будет ли значение или значения идентификаторов в файле импортированных данных использоваться для столбца идентификаторов. Если параметру IDENTITY_INSERT задано значение OFF (по умолчанию), значения идентификаторов для этого столбца проверяются, но не импортируются. Azure Synapse Analytics автоматически присвоит уникальные значения на основе начального значения и значений приращения, указанных при создании таблицы. Обратите внимание на следующее поведение команды COPY.

- Если параметру IDENTITY_INSERT задано значение OFF и в таблице есть столбец идентификаторов,
  - необходимо указать список столбцов, который не сопоставляет поле ввода со столбцом идентификаторов.
- Если параметру IDENTITY_INSERT задано значение ON и в таблице есть столбец идентификаторов,
  - передаваемый список столбцов должен сопоставлять поле ввода со столбцом идентификаторов.
- Для IDENTITY COLUMN в списке столбцов не поддерживается значение по умолчанию.
- IDENTITY_INSERT задается только для одной таблицы за раз.

### <a name="permissions"></a>Разрешения  

Пользователь, выполняющий команду Copy, должен иметь следующие разрешения: 

- [ADMINISTER DATABASE BULK OPERATIONS](grant-database-permissions-transact-sql.md#remarks)
- [INSERT ](grant-database-permissions-transact-sql.md#remarks)

Требует разрешений INSERT и ADMINISTER BULK OPERATIONS. В [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] требуются разрешения INSERT и ADMINISTER DATABASE BULK OPERATIONS.

## <a name="examples"></a>Примеры  

### <a name="a-load-from-a-public-storage-account"></a>A. Загрузка из общедоступной учетной записи хранения

В следующем примере показана простейшая форма команды COPY, которая загружает данные из общедоступной учетной записи хранения. В этом примере значения по умолчанию для инструкции COPY соответствуют формату CSV-файла элемента строки.

```sql
COPY INTO dbo.[lineitem] FROM 'https://unsecureaccount.blob.core.windows.net/customerdatasets/folder1/lineitem.csv'
```

В команде COPY используются следующие значения по умолчанию:

- DATEFORMAT = Session DATEFORMAT 

- MAXERRORS = 0

- COMPRESSION — по умолчанию без сжатия

- FIELDQUOTE = "" 

- FIELDTERMINATOR = "," 

- ROWTERMINATOR = "\n"

> [!IMPORTANT]
> COPY внутренне обрабатывает "\n" как "\r\n". Дополнительные сведения см. в разделе ROWTERMINATOR.

- FIRSTROW = 1

- ENCODING = "UTF8"

- FILE_TYPE = "CSV"

- IDENTITY_INSERT = "OFF"

### <a name="b-load-authenticating-via-share-access-signature-sas"></a>Б. Загрузка с проверкой подлинности с помощью подписи общего доступа (SAS)

В следующем примере производится загрузка файлов, в которых в качестве признака конца строки используется символ перевода строки, как в файлах UNIX. В этом примере также используется ключ SAS для проверки подлинности хранилища BLOB-объектов Azure.

```sql
COPY INTO test_1
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='?sv=2018-03-28&ss=bfqt&srt=sco&sp=rl&st=2016-10-17T20%3A14%3A55Z&se=2021-10-18T20%3A19%3A00Z&sig=IEoOdmeYnE9%2FKiJDSHFSYsz4AkNa%2F%2BTx61FuQ%2FfKHefqoBE%3D'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=';',
    ROWTERMINATOR='0X0A',
    ENCODING = 'UTF8',
    DATEFORMAT = 'ymd',
    MAXERRORS = 10,
    ERRORFILE = '/errorsfolder',--path starting from the storage container
    IDENTITY_INSERT = 'ON'
)
```

### <a name="c-load-with-a-column-list-with-default-values-authenticating-via-storage-account-key"></a>В. Загрузка списка столбцов со значениями по умолчанию с проверкой подлинности с помощью ключа учетной записи хранения

В этом примере производится загрузка файлов с указанием списка столбцов со значениями по умолчанию. 

```sql
--Note when specifying the column list, input field numbers start from 1
COPY INTO test_1 (Col_one default 'myStringDefault' 1, Col_two default 1 3)
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='<Your_Account_Key>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='x6RWv4It5F2msnjelv3H4DA80n0PQW0daPdw43jM0nyetx4c6CpDkdj3986DX5AHFMIf/YN4y6kkCnU8lb+Wx0Pj+6MDw=='),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=',',
    ROWTERMINATOR='0x0A',
    ENCODING = 'UTF8',
    FIRSTROW = 2
)
```

### <a name="d-load-parquet-or-orc-using-existing-file-format-object"></a>Г. Загрузка Parquet или ORC с помощью существующего объекта формата файла

 В этом примере для загрузки всех файлов Parquet в папке используется подстановочный знак. 

```sql
COPY INTO test_parquet
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.parquet'
WITH (
    FILE_FORMAT = myFileFormat,
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>')
)
```

### <a name="e-load-specifying-wild-cards-and-multiple-files"></a>Д. Загрузка с указанием подстановочных знаков и нескольких файлов

```sql
COPY INTO t1
FROM 
'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt', 
    'https://myaccount.blob.core.windows.net/myblobcontainer/folder1'
WITH ( 
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= '<client_id>@<OAuth_2.0_Token_EndPoint>',SECRET='<key>'),
    FIELDTERMINATOR = '|'
)
```

### <a name="f-load-using-msi-credentials"></a>Е. Загрузка с использованием учетных данных MSI

```sql
COPY INTO dbo.myCOPYDemoTable
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL = (IDENTITY = 'Managed Identity'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=','
)
```

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ

### <a name="what-is-the-performance-of-the-copy-command-compared-to-polybase"></a>Какова производительность команды COPY по сравнению с PolyBase?
Производительность команды COPY зависит от рабочей нагрузки. Для обеспечения максимальной производительности при загрузке CSV-файла рекомендуется разделять входные данные на несколько файлов.

### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-csv-files"></a>Каковы рекомендации по разделению файлов при использовании команды COPY для загрузки CSV-файлов?
Рекомендации по количеству файлов приведены в таблице ниже. После достижения рекомендуемого количества файлов производительность будет повышаться при увеличении размера файлов. Простая процедура разделения файлов описана в [этой документации](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/how-to-maximize-copy-load-throughput-with-file-splits/ba-p/1314474). 

| **DWU** | **Количество файлов** |
| :-----: | :--------: |
|   100   |     60     |
|   200   |     60     |
|   300   |     60     |
|   400   |     60     |
|   500   |     60     |
|  1000  |    120     |
|  1500  |    180     |
|  2 000  |    240     |
|  2500  |    300     |
|  3000  |    360     |
|  5 000  |    600     |
|  6000  |    720     |
|  7500  |    900     |
| 10 000  |    1200    |
| 15 000  |    1800    |
| 30 000  |    3600    |


### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-parquet-or-orc-files"></a>Каковы рекомендации по разделению файлов при использовании команды COPY для загрузки файлов Parquet или ORC?
Разделять файлы Parquet и ORC не нужно, так как команда COPY разделяет их автоматически. Для оптимальной производительности файлы Parquet и ORC в учетной записи хранения Azure должны иметь размер не менее 256 МБ. 

### <a name="are-there-any-limitations-on-the-number-or-size-of-files"></a>Есть ли ограничения на число или размер файлов?
Ограничение на количество или размер файлов отсутствует, но для лучшей производительности мы рекомендуем использовать файлы размером не менее 4 МБ.

### <a name="are-there-any-known-issues-with-the-copy-statement"></a>Существуют ли известные проблемы с инструкцией COPY?
Если у вас есть рабочая область Synapse, созданная до 12.07.2020, при проверке подлинности с помощью управляемого удостоверения может появиться такое сообщение об ошибке:

*com.microsoft.sqlserver.jdbc.SQLServerException: Managed Service Identity has not been enabled on this server. Please enable Managed Service Identity and try again.*  (Управляемое удостоверение службы отключено на этом сервере. Включите управляемое удостоверение службы и повторите попытку.)

Чтобы обойти эту ошибку, повторно зарегистрируйте управляемое удостоверение рабочей области, выполнив указанные ниже действия.

1. Перейдите к рабочей области Synapse на портале Azure.
2. Перейдите к колонке "Управляемые удостоверения". 
3. Если параметр "Разрешить конвейеры" уже выбран, необходимо снять этот флажок и сохранить изменения.
4. Проверьте параметр "Разрешить конвейеры" и сохраните изменения.


## <a name="see-also"></a>См. также раздел  

 [Обзор загрузки с помощью [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/design-elt-data-loading)
