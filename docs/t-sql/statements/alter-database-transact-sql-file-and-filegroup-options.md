---
title: 'ALTER DATABASE: файлы и файловые группы'
description: Обновление файлов и файловых групп базы данных с помощью Transact-SQL.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ADD FILE
- ADD_FILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting files
- removing files
- deleting filegroups
- file modifications [SQL Server]
- databases [SQL Server], size
- relocating files
- databases [SQL Server], modifying
- file additions [SQL Server], ALTER DATABASE
- file moving [SQL Server]
- default filegroups
- ALTER DATABASE statement, files and filegroups
- initializing files [SQL Server]
- database files [SQL Server]
- moving files
- filegroups [SQL Server], deleting
- adding filegroups
- adding files
- database filegroups [SQL Server]
- adding log files
- modifying files
- filegroups [SQL Server], adding
- file initialization [SQL Server]
- files [SQL Server], deleting
- files [SQL Server], adding
- databases [SQL Server], moving
ms.assetid: 1f635762-f7aa-4241-9b7a-b51b22292b07
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b32b17c2909b22068daf4090f0cac7bb6575706a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094664"
---
# <a name="alter-database-transact-sql-file-and-filegroup-options"></a>Параметры инструкции ALTER DATABASE для файлов и файловых групп (Transact-SQL)

Изменяет файлы и файловые группы, связанные с базой данных. Добавляет или удаляет файлы и файловые группы из базы данных и изменяет атрибуты базы данных или ее файлов и файловых групп. См. дополнительные сведения о [других параметрах ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

Дополнительные сведения о соглашениях о синтаксисе см. в статье [Соглашения о синтаксисе в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-database-transact-sql-file-and-filegroup-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="syntax"></a>Синтаксис

```syntaxsql
ALTER DATABASE database_name
{
    <add_or_modify_files>
  | <add_or_modify_filegroups>
}

<add_or_modify_files>::=
{
    ADD FILE <filespec> [ ,...n ]
        [ TO FILEGROUP { filegroup_name } ]
  | ADD LOG FILE <filespec> [ ,...n ]
  | REMOVE FILE logical_file_name
  | MODIFY FILE <filespec>
}

<filespec>::=
(
    NAME = logical_file_name
    [ , NEWNAME = new_logical_name ]
    [ , FILENAME = {'os_file_name' | 'filestream_path' | 'memory_optimized_data_path' } ]
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB| % ] ]
    [ , OFFLINE ]
)

<add_or_modify_filegroups>::=
{
    | ADD FILEGROUP filegroup_name
        [ CONTAINS FILESTREAM | CONTAINS MEMORY_OPTIMIZED_DATA ]
    | REMOVE FILEGROUP filegroup_name
    | MODIFY FILEGROUP filegroup_name
        { <filegroup_updatability_option>
        | DEFAULT
        | NAME = new_filegroup_name
        | { AUTOGROW_SINGLE_FILE | AUTOGROW_ALL_FILES }
        }
}
<filegroup_updatability_option>::=
{
    { READONLY | READWRITE }
    | { READ_ONLY | READ_WRITE }
}
```

## <a name="arguments"></a>Аргументы

**\<add_or_modify_files>::=**

Указывает файл, который будет добавлен, удален или изменен.

*database_name* — имя изменяемой базы данных.

ADD FILE — добавляет файл к базе данных.

TO FILEGROUP { *filegroup_name* } — задает файловую группу, к которой необходимо добавить указанный файл. Чтобы отобразить текущую файловую группу и узнать, какая файловая группа в данный момент установлена по умолчанию, используйте представление каталога [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md).

ADD LOG FILE — добавляет файл журнала в указанную базу данных.

REMOVE FILE *logical_file_name* — удаляет логическое описание файла из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и физический файл. Файл не может быть удален, если он не пуст.

*logical_file_name* — логическое имя, используемое в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при обращении к файлу.

> [!WARNING]
> Удаление файла базы данных, имеющего связанные с ним резервные копии `FILE_SNAPSHOT`, выполнится успешно, однако связанные моментальные снимки не будут удалены во избежание объявления недействительными резервных копий, ссылающихся на файл базы данных. Файл усекается, но физически не удаляется, чтобы сохранить резервные копии FILE_SNAPSHOT без изменений. Дополнительные сведения см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше).

MODIFY FILE — указывает файл, который должен быть изменен. Одновременно может быть изменено только одно свойство \<filespec>. Предложение NAME всегда должно быть указано в \<filespec>, чтобы определить, какой файл будет изменен. Если указано предложение SIZE, новый размер файла должен быть больше, чем текущий.

Чтобы изменить логическое имя файла данных или файла журнала, укажите логическое имя файла, который будет переименован, в предложении `NAME`, а новое логическое имя для файла — в предложении `NEWNAME`. Пример:

```sql
MODIFY FILE ( NAME = logical_file_name, NEWNAME = new_logical_name )
```

Чтобы переместить файл данных или файл журнала в новое расположение, укажите текущее логическое имя файла в предложении `NAME` и укажите новый путь и имя файла в операционной системе в предложении `FILENAME`. Пример:

```sql
MODIFY FILE ( NAME = logical_file_name, FILENAME = ' new_path/os_file_name ' )
```

При перемещении полнотекстового каталога укажите только новый путь в предложении FILENAME. Не указывайте имя файла в операционной системе.

Дополнительные сведения см. в статье [Перемещение файлов базы данных](../../relational-databases/databases/move-database-files.md).

Для файловой группы FILESTREAM значение NAME можно изменять в режиме в сети. Значение FILENAME можно изменять в режиме в сети, но внесенное изменение вступает в силу лишь после того, как будет выполнено физическое перемещение контейнера, а также остановка и последующий перезапуск сервера.

Можно задать значение параметра файла FILESTREAM, равное OFFLINE. Если файл FILESTREAM определен как вне сети, его родительская файловая группа отмечается внутри как вне сети, поэтому любая попытка доступа к данным FILESTREAM в пределах этой файловой группы окончится неудачей.

> [!NOTE]
> Параметры \<add_or_modify_files> недоступны в автономной базе данных.

**\<filespec>::=**

Управляет свойствами файла.

NAME *logical_file_name* — задает логическое имя файла.

*logical_file_name* — логическое имя, используемое в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при ссылке на файл.

NEWNAME *new_logical_file_name* — задает новое логическое имя файла.

*new_logical_file_name* — имя, которым будет заменено текущее логическое имя файла. Имя должно быть уникальным в базе данных и соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Имя может быть символьной константой или константой Юникода, обычным идентификатором или идентификатором с разделителем.

FILENAME { **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** | **'** _memory\_optimized\_data\_path_ **'** } — задает имя (физического) файла в операционной системе.

'*os_file_name*' — для стандартной файловой группы (ROWS) этот параметр представляет собой путь и имя файла, которые использовались операционной системой при создании файла. Файл должен постоянно храниться на сервере, на котором установлен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Указанный путь должен существовать до выполнения инструкции ALTER DATABASE.

> [!NOTE]
> Параметры `SIZE`, `MAXSIZE` и `FILEGROWTH` невозможно задать, если указан UNC-путь к файлу.
>
> Системные базы данных не могут размещаться в общих каталогах UNC.

Файлы данных не должны располагаться в сжатой файловой системе, кроме случаев, когда файлы являются вторичными файлами только для чтения или база данных находится в режиме только для чтения. Файлы журналов ни в коем случае не должны размещаться в сжатых файловых системах.

Если файл находится в необработанной секции, аргумент *os_file_name* должен указывать только букву диска существующей необработанной секции. В каждый необработанный раздел может быть помещен только один файл.

**'** *filestream_path* **'**  — для файловой группы FILESTREAM параметр FILENAME указывает путь, где будут храниться данные FILESTREAM. Должен существовать путь вплоть до последнего каталога, но последний каталог существовать не должен. Например, если указан путь `C:\MyFiles\MyFilestreamData`, то каталог `C:\MyFiles` должен существовать до выполнения инструкции ALTER DATABASE, но папка `MyFilestreamData` не должна существовать.

> [!NOTE]
> Свойства SIZE и FILEGROWTH к файловой группе FILESTREAM неприменимы.

**'** *memory_optimized_data_path* **'**  — для файловой группы, оптимизированной для памяти, FILENAME указывает путь, по которому будут храниться данные, оптимизированные для памяти. Должен существовать путь вплоть до последнего каталога, но последний каталог существовать не должен. Например, если указан путь `C:\MyFiles\MyData`, то каталог `C:\MyFiles` должен существовать до выполнения инструкции ALTER DATABASE, но папка `MyData` не должна существовать.

Файловую группу и файл (`<filespec>`) необходимо создавать в одной инструкции.

> [!NOTE]
> Свойства SIZE и FILEGROWTH не относятся к файловой группе MEMORY_OPTIMIZED_DATA.

Дополнительные сведения об оптимизированных для памяти файловых группах см. в статье [Оптимизированная для памяти файловая группа](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md).

SIZE *size* — указывает размер файла. Параметр SIZE не применяется к файловым группам FILESTREAM.

*size* — размер файла.

При использовании в инструкции ADD FILE аргумент *size* является начальным размером файла. При использовании в инструкции MODIFY FILE аргумент *size* является новым размером файла и должен превышать текущий размер файла.

Если для первичного файла не задан аргумент *size*, компонент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]использует размер первичного файла, указанный в базе данных **model**. Когда указан вторичный файл данных или журнала, но для этого файла не указан аргумент *size*, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] задает размер файла равным 1 МБ.

Суффиксы KB, MB, GB и TB могут использоваться для указания килобайтов, мегабайтов, гигабайтов или терабайтов. По умолчанию — MБ. Укажите целое число без десятичного разделителя. Для указания долей мегабайта преобразуйте значение в килобайты, умножив число на 1024. Например, укажите «1536 KB» вместо «1,5 MB» (1,5 x 1024 = 1536).

> [!NOTE]
> `SIZE` невозможно задать:
>
> - если указан UNC-путь к файлу;
> - для файловых групп `FILESTREAM` и `MEMORY_OPTIMIZED_DATA`.

MAXSIZE { *max_size*| UNLIMITED } — задает максимальное значение, до которого может увеличиваться размер файла.

*max_size* — максимальный размер файла. Суффиксы KB, MB, GB и TB могут использоваться для указания килобайтов, мегабайтов, гигабайтов или терабайтов. По умолчанию — MБ. Укажите целое число без десятичного разделителя. Если аргумент *max_size* не указан, то размер файла может увеличиваться до тех пор, пока диск не будет заполнен.

UNLIMITED — указывает, что размер файла может увеличиваться вплоть до заполнения диска. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] файл журнала, для которого задано неограниченное увеличение размера, имеет максимальный размер 2 ТБ, а файл данных — 16 ТБ. Ограничения размера отсутствуют, если этот параметр указан для контейнера FILESTREAM. Размер продолжает увеличиваться до полного заполнения диска.

> [!NOTE]
> `MAXSIZE` невозможно задать, если указан UNC-путь к файлу.

FILEGROWTH *growth_increment* — задает шаг автоматического приращения при увеличении размера файла. Значение параметра FILEGROWTH для файла не может превосходить значение параметра MAXSIZE. Параметр FILEGROWTH не применяется к файловым группам FILESTREAM.

*growth_increment* — объем пространства, добавляемого к файлу каждый раз, когда требуется увеличить пространство.

Значение может быть указано в килобайтах, мегабайтах, гигабайтах, терабайтах или процентах (%). Если указано число без суффикса MB, KB или %, то по умолчанию используется MB. Если размер указан в процентах (%), то шаг роста — это заданная часть в процентах от размера файла во время этого файла. Указанный размер округляется до ближайших 64 КБ.

Значение 0 указывает, что автоматическое приращение выключено и дополнительное пространство для файла не разрешено.

Если параметр FILEGROWTH не задан, доступны следующие значения по умолчанию.

|Версия|Значения по умолчанию|
|-------------|--------------------|
|Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|Данные — 64 МБ. Файлы журналов — 64 МБ.|
|Начиная с [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Данные — 1 МБ. Файлы журналов — 10 %.|
|До [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Данные — 10 %. Файлы журналов — 10 %.|

> [!NOTE]
> `FILEGROWTH` невозможно задать:
>
> - если указан UNC-путь к файлу;
> - для файловых групп `FILESTREAM` и `MEMORY_OPTIMIZED_DATA`.

OFFLINE — переводит файл в режим "вне сети" и делает все объекты в файловой группе недоступными.

> [!CAUTION]
> Используйте этот параметр только в том случае, когда файл поврежден и может быть восстановлен. Файл, переведенный в режим OFFLINE, может быть заново включен в режиме в сети только при восстановлении из резервной копии. Дополнительные сведения о восстановлении отдельного файла см. в статье [Инструкции RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md).
>
> Параметры \<filespec> недоступны в автономной базе данных.

**\<add_or_modify_filegroups>::=**

Добавить, изменить или удалить файловую группу из базы данных.

ADD FILEGROUP *filegroup_name* — добавляет в базу данных файловую группу.

CONTAINS FILESTREAM — указывает, что файловая группа хранит большие двоичные объекты (BLOB-объекты) FILESTREAM в файловой системе.

CONTAINS MEMORY_OPTIMIZED_DATA

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше)

Указывает, что файловая группа хранит оптимизируемые для памяти данные в файловой системе. Дополнительные сведения см. в статье [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Для каждой базы данных можно использовать только одну файловую группу `MEMORY_OPTIMIZED_DATA`. Для создания таблицы, оптимизируемой для памяти, файловая группа не может быть пустой. Должно быть не менее одного файла. *filegroup_name* содержит путь. Должен существовать путь вплоть до последнего каталога, но последний каталог существовать не должен.

REMOVE FILEGROUP *filegroup_name* — удаляет файловую группу из базы данных. Файловая группа не может быть удалена, пока она не пустая. Вначале удалите из файловой группы все файлы. Дополнительные сведения см. выше в разделе "REMOVE FILE *logical_file_name*".

> [!NOTE]
> Если сборщик мусора FILESTREAM не удалил все файлы из контейнера FILESTREAM, операция `ALTER DATABASE REMOVE FILE` по удалению контейнера FILESTREAM завершится неудачей и возвратит ошибку. См. раздел [Удаление контейнера FILESTREAM](#removing-a-filestream-container) далее в этой статье.

MODIFY FILEGROUP *filegroup_name* { \<filegroup_updatability_option> | DEFAULT | NAME **=** _new\_filegroup\_name_ } — изменяет файловую группу, меняя ее состояние на READ_ONLY или READ_WRITE, делая ее файловой группой по умолчанию для базы данных или изменяя имя файловой группы.

\<filegroup_updatability_option> — устанавливает свойство "только для чтения" или "чтение и запись" для файловой группы.

DEFAULT — изменяет стандартную файловую группу базы данных на *filegroup_name*. Только одна файловая группа в базе данных может быть файловой группой по умолчанию. Дополнительные сведения см. в статье [Файлы и группы файлов базы данных](../../relational-databases/databases/database-files-and-filegroups.md).

NAME = *new_filegroup_name* — изменяет имя файловой группы на *new_filegroup_name*.

AUTOGROW_SINGLE_FILE — **применимо к**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше)

Если файл в файловой группе удовлетворяет требованиям порога автоматического увеличения, увеличивается только этот файл. Это значение по умолчанию.

AUTOGROW_ALL_FILES

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше)

Если файл в файловой группе удовлетворяет требованиям порога автоматического увеличения, все файлы в файловой группе увеличиваются.

> [!NOTE]
> Это значение по умолчанию для tempdb.

**\<filegroup_updatability_option>::=**

Устанавливает свойство «только для чтения» или «чтение и запись» для файловой группы.

READ_ONLY | READONLY — определяет, что файловая группа доступна только для чтения. Изменение ее объектов запрещено. Первичную файловую группу перевести в состояние только для чтения нельзя. Чтобы изменить это состояние, необходимо обладать монопольным доступом к базе данных. Дополнительные сведения см. в описании предложения SINGLE_USER.

Поскольку база данных находится в состоянии только для чтения, невозможно производить изменения данных:

- при запуске системы будет пропущено автоматическое восстановление;
- сжатие базы данных невозможно;
- в базах данных, находящихся в состоянии только для чтения, невозможны блокировки. Это может привести к более быстрому выполнению запросов.

> [!NOTE]
> Ключевое слово `READONLY` будет удалено в будущей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте применять `READONLY` в новых разработках и запланируйте внесение изменений в приложения, использующие `READONLY` в настоящее время. Используйте вместо этого `READ_ONLY`.

READ_WRITE | READWRITE — определяет, что файловая группа доступна для чтения и записи. Разрешено изменять объекты в файловой группе. Чтобы изменить это состояние, необходимо обладать монопольным доступом к базе данных. Дополнительные сведения см. в описании предложения SINGLE_USER.

> [!NOTE]
> Ключевое слово `READWRITE` будет удалено в будущей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте применять `READWRITE` в новых разработках и запланируйте изменить приложения, использующие в настоящее время `READWRITE`, на использование `READ_WRITE`.
> [!TIP]
> Состояние этих параметров может быть определено с помощью проверки значения столбца **is_read_only** в представлении каталога **sys.databases** или свойства **Updateability** функции `DATABASEPROPERTYEX`.

## <a name="remarks"></a>Remarks

Чтобы уменьшить размер базы данных, используйте предложение [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Добавить или удалить файл во время выполнения инструкции `BACKUP` невозможно.

Для каждой базы данных может указываться не более 32 767 файлов и 32 767 файловых групп.

Начиная с [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и в более поздних версиях состояние файла базы данных (например, в сети или вне сети) поддерживается независимо от состояния базы данных. Дополнительные сведения см. в разделе [Состояния файлов](../../relational-databases/databases/file-states.md).

- Состояние файлов в пределах файловой группы определяет доступность файловой группы в целом. Чтобы файловая группа была доступна, необходимо, чтобы все файлы в файловой группе находились в режиме в сети.
- Если файловая группа вне сети, то любая попытка обращения к файловой группе с помощью инструкции SQL закончится ошибкой. При создании планов запросов для инструкций `SELECT` оптимизатор запросов избегает некластеризованных индексов и индексированных представлений, которые находятся в файловых группах вне сети. Это позволяет успешно выполнить эти инструкции. Однако если файловая группа, находящаяся в режиме вне сети, содержит кучу или кластеризованный индекс целевой таблицы, инструкции `SELECT` не будут выполнены. Кроме того, любая инструкция `INSERT`, `UPDATE` или `DELETE`, изменяющая таблицу с любым индексом в файловой группе, находящихся в режиме вне сети, также не будет выполнена.

Параметры SIZE, MAXSIZE и FILEGROWTH недоступны, если путь к файлу указан в формате UNC.

Параметры SIZE и FILEGROWTH невозможно задать для файловых групп, оптимизированных для памяти.

Ключевое слово `READONLY` будет удалено в будущей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте использовать `READONLY` в новых разработках и запланируйте изменение приложений, которые используют READONLY в настоящее время. Используйте вместо этого `READ_ONLY`.

Ключевое слово `READWRITE` будет удалено в будущей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте применять `READWRITE` в новых разработках и запланируйте изменить приложения, использующие в настоящее время `READWRITE`, на использование `READ_WRITE`.

## <a name="moving-files"></a>Перемещение файлов

Системные или определенные пользователем данные и файлы журналов можно перемещать, указывая новое местоположение в параметре FILENAME. Это может быть полезным в следующих случаях:

- Восстановление после сбоя. Например, база данных находится в подозрительном режиме или завершила работу по причине сбоя оборудования;
- Плановое перемещение.
- Перемещение для запланированного обслуживания дисков.

Дополнительные сведения см. в статье [Перемещение файлов базы данных](../../relational-databases/databases/move-database-files.md).

## <a name="initializing-files"></a>Инициализация файлов

По умолчанию файлы данных и журналов инициализируются, заполняясь нулями, при выполнении одной из следующих операций:

- создавать базу данных;
- добавление файлов к существующей базе данных;
- увеличение размера существующего файла;
- Восстановление базы данных или файловой группы.

Файлы данных могут быть инициализированы мгновенно. Это разрешено для быстрого выполнения этих файловых операций. Дополнительные сведения см. в разделе [Инициализация файлов базы данных](../../relational-databases/databases/database-instant-file-initialization.md).

## <a name="removing-a-filestream-container"></a><a name="removing-a-filestream-container"></a> Удаление контейнера FILESTREAM

Даже если контейнер FILESTREAM был очищен с использованием операции "DBCC SHRINKFILE", базе данных по-прежнему могут быть необходимы ссылки на удаленные файлы по различным причинам, связанным с обслуживанием системы. Хранимая процедура [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md) запустит сборщик мусора FILESTREAM, чтобы удалить эти файлы, когда это будет безопасно. Если сборщик мусора FILESTREAM не удалил все файлы из контейнера FILESTREAM, операция ALTER DATABASE REMOVE FILE по удалению контейнера FILESTREAM завершится неудачей и вернет ошибку. Для удаления контейнера FILESTREAM рекомендуется следующий процесс.

1. Выполните инструкцию [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) с параметром EMPTYFILE, чтобы переместить активное содержимое этого контейнера в другие контейнеры.
2. Убедитесь, что в модели восстановления FULL или BULK_LOGGED было выполнено резервное копирование журналов.
3. Убедитесь, что было запущено задание чтения журнала репликации, если это необходимо.
4. Выполните хранимую процедуру [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md), чтобы сборщик мусора удалил все файлы, которые больше не нужны в этом контейнере.
5. Выполните инструкцию ALTER DATABASE с параметром REMOVE FILE, чтобы удалить этот контейнер.
6. Повторите шаги с 2 по 4, чтобы завершить сборку мусора.
7. Используйте инструкцию ALTER Database...REMOVE FILE, чтобы удалить этот контейнер.

## <a name="examples"></a>Примеры

### <a name="a-adding-a-file-to-a-database"></a>A. Добавление файла к базе данных

В следующем примере к базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] добавляется файл данных размером 5 МБ.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = Test1dat2,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat2.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="b-adding-a-filegroup-with-two-files-to-a-database"></a>Б. Добавление файловой группы с двумя файлами к базе данных

В следующем примере в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] создается файловая группа `Test1FG1` и добавляется два файла по 5 МБ в эту файловую группу.

```sql
USE master
GO
ALTER DATABASE AdventureWorks2012
ADD FILEGROUP Test1FG1;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = test1dat3,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat3.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),  
(  
    NAME = test1dat4,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat4.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
)  
TO FILEGROUP Test1FG1;
GO
```

### <a name="c-adding-two-log-files-to-a-database"></a>В. Добавление двух файлов журнала к базе данных

В следующем примере к базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] добавляется два файла журнала размером 5 МБ каждый.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD LOG FILE
(
    NAME = test1log2,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\test2log.ldf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),
(
    NAME = test1log3,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\DATA\test3log.ldf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="d-removing-a-file-from-a-database"></a>Г. Удаление файла из базы данных

В следующем примере удаляется один из файлов, добавленных в примере Б.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
REMOVE FILE test1dat4;
GO
```

### <a name="e-modifying-a-file"></a>Д. Изменение файла

В следующем примере увеличивается размер одного из файлов, добавленных в примере Б. Инструкция ALTER DATABASE с командой MODIFY FILE может только увеличить размер файла, поэтому, если нужно уменьшить размер файла, следует использовать DBCC SHRINKFILE.

```sql
USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

В этом примере показано уменьшение размера файла данных до 100 МБ и определение размера при этом объеме.

```sql
USE AdventureWorks2012;
GO

DBCC SHRINKFILE (AdventureWorks2012_data, 100);
GO

USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

### <a name="f-moving-a-file-to-a-new-location"></a>Е. Перемещение файла в новое расположение

В следующем примере файл `Test1dat2`, созданный в примере A, перемещается в новый каталог.

> [!NOTE]
> Перед выполнением этого примера необходимо физически переместить файл в новый каталог. После выполнения остановите и запустите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или переведите базу данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] в состояние OFFLINE, а затем назад в ONLINE, чтобы осуществить изменения.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILE
(
    NAME = Test1dat2,
    FILENAME = N'c:\t1dat2.ndf'
);
GO
```

### <a name="g-moving-tempdb-to-a-new-location"></a>Ж. Перемещение базы данных tempdb в новое расположение

В следующем примере база данных `tempdb` перемещается из ее текущего расположения на диске в другое расположение. Так как база данных `tempdb` повторно создается при каждом запуске службы MSSQLSERVER, нет необходимости физически переносить файлы данных и журнала. Эти файлы создаются при запуске службы на шаге 3. Пока служба не будет запущена повторно, база данных `tempdb` продолжает функционировать на прежнем месте.

1. Определите логические имена файлов базы данных `tempdb` и их текущее расположение на диске.

    ```sql
    SELECT name, physical_name
    FROM sys.master_files
    WHERE database_id = DB_ID('tempdb');
    GO
    ```

2. Измените местоположение каждого файла с помощью `ALTER DATABASE`.

    ```sql
    USE master;
    GO
    ALTER DATABASE tempdb
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');
    GO
    ALTER DATABASE tempdb
    MODIFY FILE (NAME = templog, FILENAME = 'E:\SQLData\templog.ldf');
    GO
    ```

3. Остановите и перезапустите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

4. Проверьте изменение файла.

    ```sql
    SELECT name, physical_name
    FROM sys.master_files
    WHERE database_id = DB_ID('tempdb');
    ```

5. Удалите файлы tempdb.mdf и templog.ldf из их исходного расположения.

### <a name="h-making-a-filegroup-the-default"></a>З. Назначение файловой группы по умолчанию

В следующем примере файловая группа `Test1FG1`, созданная в примере Б, назначается файловой группой по умолчанию. Затем файловая группа по умолчанию будет переназначена на файловую группу `PRIMARY`. Обратите внимание, что слово `PRIMARY` должно быть заключено в скобки или в кавычки.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP Test1FG1 DEFAULT;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP [PRIMARY] DEFAULT;
GO
```

### <a name="i-adding-a-filegroup-using-alter-database"></a>И. Добавление файловой группы с помощью инструкции ALTER DATABASE

В следующем примере в базу данных `FILEGROUP` добавляется файловая группа `FILESTREAM`, содержащая предложение `FileStreamPhotoDB`.

```sql
--Create and add a FILEGROUP that CONTAINS the FILESTREAM clause.
ALTER DATABASE FileStreamPhotoDB
ADD FILEGROUP TodaysPhotoShoot
CONTAINS FILESTREAM;
GO

--Add a file for storing database photos to FILEGROUP
ALTER DATABASE FileStreamPhotoDB
ADD FILE
(
  NAME= 'PhotoShoot1',
  FILENAME = 'C:\Users\Administrator\Pictures\TodaysPhotoShoot.ndf'
)
TO FILEGROUP TodaysPhotoShoot;
GO
```

В следующем примере в базу данных `FILEGROUP` добавляется файловая группа `MEMORY_OPTIMIZED_DATA`, содержащая предложение `xtp_db`. В файловой группе хранятся данные, оптимизированные для памяти.

```sql
--Create and add a FILEGROUP that CONTAINS the MEMORY_OPTIMIZED_DATA clause.
ALTER DATABASE xtp_db
ADD FILEGROUP xtp_fg
CONTAINS MEMORY_OPTIMIZED_DATA;
GO

--Add a file for storing memory optimized data to FILEGROUP
ALTER DATABASE xtp_db
ADD FILE
(
  NAME='xtp_mod',
  FILENAME='d:\data\xtp_mod'
)
TO FILEGROUP xtp_fg;
GO
```

### <a name="j-change-filegroup-so-that-when-a-file-in-the-filegroup-meets-the-autogrow-threshold-all-files-in-the-filegroup-grow"></a>К. Изменение файловой группы таким образом, что если файл в файловой группе удовлетворяет требованиям порога автоматического увеличения, увеличиваются все файлы в файловой группе

 В следующем примере создаются необходимые инструкции `ALTER DATABASE` для изменения файловых групп для чтения и записи с помощью параметра `AUTOGROW_ALL_FILES`.

```sql
--Generate ALTER DATABASE ... MODIFY FILEGROUP statements
--so that all read-write filegroups grow at the same time.
SET NOCOUNT ON;

DROP TABLE IF EXISTS #tmpdbs
CREATE TABLE #tmpdbs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, isdone BIT);

DROP TABLE IF EXISTS #tmpfgs
CREATE TABLE #tmpfgs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, fgname sysname, isdone BIT);

INSERT INTO #tmpdbs ([dbid], [dbname], [isdone])
SELECT database_id, name, 0 FROM master.sys.databases (NOLOCK) WHERE is_read_only = 0 AND state = 0;

DECLARE @dbid INT, @query VARCHAR(1000), @dbname sysname, @fgname sysname

WHILE (SELECT COUNT(id) FROM #tmpdbs WHERE isdone = 0) > 0
BEGIN
  SELECT TOP 1 @dbname = [dbname], @dbid = [dbid] FROM #tmpdbs WHERE isdone = 0

  SET @query = 'SELECT ' + CAST(@dbid AS NVARCHAR) + ', ''' + @dbname + ''', [name], 0 FROM [' + @dbname + '].sys.filegroups WHERE [type] = ''FG'' AND is_read_only = 0;'
  INSERT INTO #tmpfgs
  EXEC (@query)

  UPDATE #tmpdbs
  SET isdone = 1
  WHERE [dbid] = @dbid
END;

IF (SELECT COUNT(ID) FROM #tmpfgs) > 0
BEGIN
  WHILE (SELECT COUNT(id) FROM #tmpfgs WHERE isdone = 0) > 0
  BEGIN
    SELECT TOP 1 @dbname = [dbname], @dbid = [dbid], @fgname = fgname FROM #tmpfgs WHERE isdone = 0

    SET @query = 'ALTER DATABASE [' + @dbname + '] MODIFY FILEGROUP [' + @fgname + '] AUTOGROW_ALL_FILES;'

    PRINT @query

    UPDATE #tmpfgs
    SET isdone = 1
    WHERE [dbid] = @dbid AND fgname = @fgname
  END
END;
GO
```

## <a name="see-also"></a>См. также:

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Большие двоичные объекты](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)
- [DBCC SHRINKFIL](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)
- [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md)
- [Инициализация файлов базы данных](../../relational-databases/databases/database-instant-file-initialization.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-file-and-filegroup-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Управляемый экземпляр SQL \*_**<br />&nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Управляемый экземпляр SQL Azure

Используйте эту инструкцию с базой данных в Управляемом экземпляре SQL Azure.

## <a name="syntax-for-azure-sql-managed-instance"></a>Синтаксис для Управляемого экземпляра SQL Azure

```syntaxsql
ALTER DATABASE database_name
{
    <add_or_modify_files>
  | <add_or_modify_filegroups>
}
[;]

<add_or_modify_files>::=
{
    ADD FILE <filespec> [ ,...n ]
        [ TO FILEGROUP { filegroup_name } ]
  | REMOVE FILE logical_file_name
  | MODIFY FILE <filespec>
}
  
<filespec>::=
(
    NAME = logical_file_name
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB| % ] ]
)

<add_or_modify_filegroups>::=
{
    | ADD FILEGROUP filegroup_name
    | REMOVE FILEGROUP filegroup_name
    | MODIFY FILEGROUP filegroup_name
        { <filegroup_updatability_option>
        | DEFAULT
        | NAME = new_filegroup_name
        | { AUTOGROW_SINGLE_FILE | AUTOGROW_ALL_FILES }
        }
}  
<filegroup_updatability_option>::=
{
    { READONLY | READWRITE }
    | { READ_ONLY | READ_WRITE }
}
```

## <a name="arguments"></a>Аргументы

**\<add_or_modify_files>::=**

Указывает файл, который будет добавлен, удален или изменен.

*database_name* — имя изменяемой базы данных.

ADD FILE — добавляет файл к базе данных.

TO FILEGROUP { *filegroup_name* } — задает файловую группу, к которой необходимо добавить указанный файл. Чтобы отобразить текущую файловую группу и узнать, какая файловая группа в данный момент установлена по умолчанию, используйте представление каталога [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md).

REMOVE FILE *logical_file_name* — удаляет логическое описание файла из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и физический файл. Файл не может быть удален, если он не пуст.

*logical_file_name* — логическое имя, используемое в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при обращении к файлу.

MODIFY FILE — указывает файл, который должен быть изменен. Одновременно может быть изменено только одно свойство \<filespec>. Предложение NAME всегда должно быть указано в \<filespec>, чтобы определить, какой файл будет изменен. Если указано предложение SIZE, новый размер файла должен быть больше, чем текущий.

**\<filespec>::=**

Управляет свойствами файла.

NAME *logical_file_name* — задает логическое имя файла.

*logical_file_name* — логическое имя, используемое в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при ссылке на файл.

NEWNAME *new_logical_file_name* — задает новое логическое имя файла.

*new_logical_file_name* — имя, которым будет заменено текущее логическое имя файла. Имя должно быть уникальным в базе данных и соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md). Имя может быть символьной константой или константой Юникода, обычным идентификатором или идентификатором с разделителем.

SIZE *size* — указывает размер файла.

*size* — размер файла.

При использовании в инструкции ADD FILE аргумент *size* является начальным размером файла. При использовании в инструкции MODIFY FILE аргумент *size* является новым размером файла и должен превышать текущий размер файла.

Если для первичного файла не задан аргумент *size*, компонент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]использует размер первичного файла, указанный в базе данных **model**. Когда указан вторичный файл данных или журнала, но для этого файла не указан аргумент *size*, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] задает размер файла равным 1 МБ.

Суффиксы KB, MB, GB и TB могут использоваться для указания килобайтов, мегабайтов, гигабайтов или терабайтов. По умолчанию — MБ. Укажите целое число без десятичного разделителя. Для указания долей мегабайта преобразуйте значение в килобайты, умножив число на 1024. Например, укажите «1536 KB» вместо «1,5 MB» (1,5 x 1024 = 1536).

MAXSIZE { *max_size*| UNLIMITED } — задает максимальное значение, до которого может увеличиваться размер файла.

*max_size* — максимальный размер файла. Суффиксы KB, MB, GB и TB могут использоваться для указания килобайтов, мегабайтов, гигабайтов или терабайтов. По умолчанию — MБ. Укажите целое число без десятичного разделителя. Если аргумент *max_size* не указан, то размер файла может увеличиваться до тех пор, пока диск не будет заполнен.

UNLIMITED — указывает, что размер файла может увеличиваться вплоть до заполнения диска. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] файл журнала, для которого задано неограниченное увеличение размера, имеет максимальный размер 2 ТБ, а файл данных — 16 ТБ.

FILEGROWTH *growth_increment* — задает шаг автоматического приращения при увеличении размера файла. Значение параметра FILEGROWTH для файла не может превосходить значение параметра MAXSIZE.

*growth_increment* — объем пространства, добавляемого к файлу каждый раз, когда требуется увеличить пространство.

Значение может быть указано в килобайтах, мегабайтах, гигабайтах, терабайтах или процентах (%). Если указано число без суффикса MB, KB или %, то по умолчанию используется MB. Если размер указан в процентах (%), то шаг роста — это заданная часть в процентах от размера файла во время этого файла. Указанный размер округляется до ближайших 64 КБ.

Значение 0 указывает, что автоматическое приращение выключено и дополнительное пространство для файла не разрешено.

Если параметр FILEGROWTH не задан, доступны следующие значения по умолчанию.

- Данные: 64 МБ.
- Файлы журналов: 64 МБ.

**\<add_or_modify_filegroups>::=**

Добавить, изменить или удалить файловую группу из базы данных.

ADD FILEGROUP *filegroup_name* — добавляет в базу данных файловую группу.

В следующем примере создается файловая группа, которая добавляется в базу данных с именем sql_db_mi, и в эту файловую группу добавляется файл.

```sql
ALTER DATABASE sql_db_mi ADD FILEGROUP sql_db_mi_fg;
GO
ALTER DATABASE sql_db_mi ADD FILE (NAME='sql_db_mi_mod') TO FILEGROUP sql_db_mi_fg;
```

REMOVE FILEGROUP *filegroup_name* — удаляет файловую группу из базы данных. Файловая группа не может быть удалена, пока она не пустая. Вначале удалите из файловой группы все файлы. Дополнительные сведения см. выше в разделе "REMOVE FILE *logical_file_name*".

MODIFY FILEGROUP _filegroup\_name_ { \<filegroup_updatability_option> | DEFAULT | NAME **=** _new\_filegroup\_name_ } — изменяет файловую группу, меняя ее состояние на READ_ONLY или READ_WRITE, делая ее файловой группой по умолчанию для базы данных или изменяя имя файловой группы.

\<filegroup_updatability_option> — устанавливает свойство "только для чтения" или "чтение и запись" для файловой группы.

DEFAULT — изменяет стандартную файловую группу базы данных на *filegroup_name*. Только одна файловая группа в базе данных может быть файловой группой по умолчанию. Дополнительные сведения см. в статье [Файлы и группы файлов базы данных](../../relational-databases/databases/database-files-and-filegroups.md).

NAME = *new_filegroup_name* — изменяет имя файловой группы на *new_filegroup_name*.

AUTOGROW_SINGLE_FILE

Если файл в файловой группе удовлетворяет требованиям порога автоматического увеличения, увеличивается только этот файл. Это значение по умолчанию.

AUTOGROW_ALL_FILES

Если файл в файловой группе удовлетворяет требованиям порога автоматического увеличения, все файлы в файловой группе увеличиваются.

**\<filegroup_updatability_option>::=**

Устанавливает свойство «только для чтения» или «чтение и запись» для файловой группы.

READ_ONLY | READONLY — определяет, что файловая группа доступна только для чтения. Изменение ее объектов запрещено. Первичную файловую группу перевести в состояние только для чтения нельзя. Чтобы изменить это состояние, необходимо обладать монопольным доступом к базе данных. Дополнительные сведения см. в описании предложения SINGLE_USER.

Поскольку база данных находится в состоянии только для чтения, невозможно производить изменения данных:

- при запуске системы будет пропущено автоматическое восстановление;
- сжатие базы данных невозможно;
- в базах данных, находящихся в состоянии только для чтения, невозможны блокировки. Это может привести к более быстрому выполнению запросов.

> [!NOTE]
> Ключевое слово READONLY будет удалено в будущей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте использования ключевого слова READONLY в новых разработках и запланируйте изменение приложений, которые сейчас его используют. Вместо него используйте READ_ONLY.

READ_WRITE | READWRITE — определяет, что файловая группа доступна для чтения и записи. Разрешено изменять объекты в файловой группе. Чтобы изменить это состояние, необходимо обладать монопольным доступом к базе данных. Дополнительные сведения см. в описании предложения SINGLE_USER.

> [!NOTE]
> Ключевое слово `READWRITE` будет удалено в будущей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте применять `READWRITE` в новых разработках и запланируйте изменить приложения, использующие в настоящее время `READWRITE`, на использование `READ_WRITE`.

Состояние этих параметров может быть определено с помощью проверки значения столбца **is_read_only** в представлении каталога **sys.databases** или свойства **Updateability** функции `DATABASEPROPERTYEX`.

## <a name="remarks"></a>Remarks

Чтобы уменьшить размер базы данных, используйте предложение [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Добавить или удалить файл во время выполнения инструкции `BACKUP` невозможно.

Для каждой базы данных может указываться не более 32 767 файлов и 32 767 файловых групп.

## <a name="examples"></a>Примеры

### <a name="a-adding-a-file-to-a-database"></a>A. Добавление файла к базе данных

В следующем примере к базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] добавляется файл данных размером 5 МБ.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
  NAME = Test1dat2,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="b-adding-a-filegroup-with-two-files-to-a-database"></a>Б. Добавление файловой группы с двумя файлами к базе данных

В следующем примере в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] создается файловая группа `Test1FG1` и добавляется два файла по 5 МБ в эту файловую группу.

```sql
USE master
GO
ALTER DATABASE AdventureWorks2012
ADD FILEGROUP Test1FG1;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = test1dat3,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),
(
    NAME = test1dat4,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
)  
TO FILEGROUP Test1FG1;
GO
```

### <a name="c-removing-a-file-from-a-database"></a>В. Удаление файла из базы данных

В следующем примере удаляется один из файлов, добавленных в примере Б.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
REMOVE FILE test1dat4;
GO
```

### <a name="d-modifying-a-file"></a>Г. Изменение файла

В следующем примере увеличивается размер одного из файлов, добавленных в примере Б. Инструкция ALTER DATABASE с командой MODIFY FILE может только увеличить размер файла, поэтому, если нужно уменьшить размер файла, следует использовать DBCC SHRINKFILE.

```sql
USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

В этом примере показано уменьшение размера файла данных до 100 МБ и определение размера при этом объеме.

```sql
USE AdventureWorks2012;
GO

DBCC SHRINKFILE (AdventureWorks2012_data, 100);
GO

USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

### <a name="e-making-a-filegroup-the-default"></a>Д. Назначение файловой группы по умолчанию

В следующем примере файловая группа `Test1FG1`, созданная в примере Б, назначается файловой группой по умолчанию. Затем файловая группа по умолчанию будет переназначена на файловую группу `PRIMARY`. Обратите внимание, что слово `PRIMARY` должно быть заключено в скобки или в кавычки.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP Test1FG1 DEFAULT;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP [PRIMARY] DEFAULT;
GO
```

### <a name="f-adding-a-filegroup-using-alter-database"></a>Е. Добавление файловой группы с помощью инструкции ALTER DATABASE

В следующем примере к базе данных `MyDB` добавляется `FILEGROUP`.

```sql
--Create and add a FILEGROUP
ALTER DATABASE MyDB
ADD FILEGROUP NewFG;
GO

--Add a file to FILEGROUP
ALTER DATABASE MyDB
ADD FILE
(
    NAME= 'MyFile',
)
TO FILEGROUP NewFG;
GO
```

### <a name="g-change-filegroup-so-that-when-a-file-in-the-filegroup-meets-the-autogrow-threshold-all-files-in-the-filegroup-grow"></a>Ж. Изменение файловой группы таким образом, что если файл в файловой группе удовлетворяет требованиям порога автоматического увеличения, увеличиваются все файлы в файловой группе

В следующем примере создаются необходимые инструкции `ALTER DATABASE` для изменения файловых групп для чтения и записи с помощью параметра `AUTOGROW_ALL_FILES`.

```sql
--Generate ALTER DATABASE ... MODIFY FILEGROUP statements
--so that all read-write filegroups grow at the same time.
SET NOCOUNT ON;

DROP TABLE IF EXISTS #tmpdbs
CREATE TABLE #tmpdbs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, isdone BIT);

DROP TABLE IF EXISTS #tmpfgs
CREATE TABLE #tmpfgs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, fgname sysname, isdone BIT);

INSERT INTO #tmpdbs ([dbid], [dbname], [isdone])
SELECT database_id, name, 0 FROM master.sys.databases (NOLOCK) WHERE is_read_only = 0 AND state = 0;

DECLARE @dbid INT, @query VARCHAR(1000), @dbname sysname, @fgname sysname

WHILE (SELECT COUNT(id) FROM #tmpdbs WHERE isdone = 0) > 0
BEGIN
    SELECT TOP 1 @dbname = [dbname], @dbid = [dbid] FROM #tmpdbs WHERE isdone = 0

    SET @query = 'SELECT ' + CAST(@dbid AS NVARCHAR) + ', ''' + @dbname + ''', [name], 0 FROM [' + @dbname + '].sys.filegroups WHERE [type] = ''FG'' AND is_read_only = 0;'
    INSERT INTO #tmpfgs
    EXEC (@query)

    UPDATE #tmpdbs
    SET isdone = 1
    WHERE [dbid] = @dbid
END;

IF (SELECT COUNT(ID) FROM #tmpfgs) > 0
BEGIN
    WHILE (SELECT COUNT(id) FROM #tmpfgs WHERE isdone = 0) > 0
    BEGIN
        SELECT TOP 1 @dbname = [dbname], @dbid = [dbid], @fgname = fgname FROM #tmpfgs WHERE isdone = 0

        SET @query = 'ALTER DATABASE [' + @dbname + '] MODIFY FILEGROUP [' + @fgname + '] AUTOGROW_ALL_FILES;'

        PRINT @query

        UPDATE #tmpfgs
        SET isdone = 1
        WHERE [dbid] = @dbid AND fgname = @fgname
    END
END;
GO
```

## <a name="see-also"></a>См. также:

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)
- [Оптимизированная для памяти файловая группа](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)

::: moniker-end
