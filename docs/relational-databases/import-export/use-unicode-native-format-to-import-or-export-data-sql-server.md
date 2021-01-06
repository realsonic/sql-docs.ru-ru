---
title: Использование собственного формата Юникода для импорта или экспорта данных (SQL Server) | Документация Майкрософт
description: Использование собственного формата Юникода для массовой передачи данных между экземплярами SQL Server, позволяющее исключить преобразование типов данных в символьный формат и из него.
ms.custom: ''
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- Unicode [SQL Server], bulk importing and exporting
- data formats [SQL Server], Unicode native
ms.assetid: a6213308-f3d5-406e-9029-19d8bb3367f3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 977e7c4222a200d697304be0be774214fbc4376a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643472"
---
# <a name="use-unicode-native-format-to-import-or-export-data-sql-server"></a>Использование собственного формата Юникода для импорта или экспорта данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Собственный формат Юникода может быть полезен при копировании данных из одной установки [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в другую. Использование собственного формата для несимвольных данных позволяет сэкономить время благодаря исключению ненужных преобразований типов данных в символьный формат и обратно. Использование символьного формата Юникода для всех символьных данных предотвращает потерю дополнительных символов в ходе массовой передачи данных между серверами, использующими различные кодовые страницы. Файл данных в собственном формате Юникода может быть считан с помощью любого метода массового импорта.  
  
 Собственный формат Юникода рекомендуется использовать для массовой передачи данных между несколькими экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью файла данных, который содержит дополнительный набор символов или символы в кодировке DBCS. В отношении несимвольных данных собственный формат Юникода использует собственные (для базы данных) типы данных. Для символьных данных, таких как [char](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [текст](../../t-sql/data-types/ntext-text-and-image-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)и [ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md), собственный формат Юникод использует символьный формат Юникод.  
  
 Данные типа [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) , которые хранятся в виде инструкции SQLVARIANT в файле собственного формата Юникод, функционируют точно так же, как в файле данных собственного формата, за исключением того, что значения типа [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) и [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) преобразуются в [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) и [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), что требует вдвое больше места для хранения столбцов, подлежащих преобразованию. В процессе массового импорта в столбец таблицы исходные метаданные сохраняются, а значения преобразуются обратно в исходные типы данных [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) и [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) .  
 
 |В этом разделе.|
|---|
|[Командные параметры для собственного формата Юникода](#command_options)|
|[Пример условий теста](#etc)<br />&emsp;&#9679;&emsp;[Образец таблицы](#sample_table)<br />&emsp;&#9679;&emsp;[Образец файла форматирования в формате, отличном от XML](#nonxml_format_file)|
|[Примеры](#examples)<br />&emsp;&#9679;&emsp;[Использование bcp и собственного формата Юникода для экспорта данных](#bcp_widenative_export)<br />&emsp;&#9679;&emsp;[Использование bcp и собственного формата Юникода для импорта данных без файла форматирования](#bcp_widenative_import)<br />&emsp;&#9679;&emsp;[Использование bcp и собственного формата Юникода для импорта данных при помощи файла форматирования, не являющегося XML](#bcp_widenative_import_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и собственного формата Юникода без файла форматирования](#bulk_widenative)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и собственного формата Юникода с файлом форматирования, не являющимся XML](#bulk_widenative_fmt)<br />&emsp;&#9679;&emsp;[Использование OPENROWSET и собственного формата Юникода с файлом форматирования, не являющимся XML](#openrowset_widenative_fmt)|
|[Связанные задачи](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
  
## <a name="command-options-for-unicode-native-format"></a>Командные параметры для собственного формата Юникода<a name="command_options"></a>  
Импортировать в таблицу данные в собственном формате Юникода можно при помощи программы [bcp](../../tools/bcp-utility.md), инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) или [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Для команды [bcp](../../tools/bcp-utility.md) или инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) формат данных можно указать в инструкции.  Для инструкции [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) нужно указать формат данных в файле форматирования.  
  
Собственный формат Юникода поддерживается следующими параметрами командной строки:  
  
|Get-Help|Параметр|Описание|  
|-------------|------------|-----------------|  
|bcp|**-N**|Заставляет программу **bcp** использовать собственный формат Юникод, при котором для всех несимвольных данных используются собственные (базы данных) типы данных, а для всех символьных данных (**char**, **nchar**, **varchar**, **nvarchar**, **text** и **ntext**) используется символьный формат Юникод.|  
|BULK INSERT|DATAFILETYPE **="widenative"**|Собственный формат Юникода используется при массовом импорте данных.|  
|OPENROWSET|Недоступно|Требуется использовать файл форматирования.|
    
> [!NOTE]
>  Также в файле форматирования можно указать форматирование для каждого поля. Дополнительные сведения см в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  
## <a name="example-test-conditions"></a>Пример условий теста<a name="etc"></a>  
Примеры в этой статье основаны на таблице и файле форматирования, которые определены ниже.

### <a name="sample-table"></a>**Образец таблицы**<a name="sample_table"></a>
Приведенный ниже скрипт создает тестовую базу данных, таблицу с именем `myWidenative` и заполняет таблицу начальными значениями.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidenative ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidenative
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="sample-non-xml-format-file"></a>**Образец файла форматирования в формате, отличном от XML**<a name="nonxml_format_file"></a>
SQL Server поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования.  Файл форматирования не в формате XML поддерживается более ранними версиями SQL Server.  Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myWidenative.fmt`в формате, отличном от XML, на основе схемы `myWidenative`.  Чтобы создать файл форматирования с помощью служебной программы [bcp](../../tools/bcp-utility.md) , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format также требует наличия параметра **-f** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, а **T** используется для указания доверенного подключения, в рамках которого применяется встроенная система безопасности.  В командной строке введите следующие команды:

```
bcp TestDatabase.dbo.myWidenative format nul -f D:\BCP\myWidenative.fmt -T -N

REM Review file
Notepad D:\BCP\myWidenative.fmt
```

> [!IMPORTANT]
> Убедитесь, что файл форматирования не в формате XML заканчивается символом перевода строки или возврата каретки.  В противном случае, скорее всего, появится следующее сообщение об ошибке:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Примеры<a name="examples"></a>
В приведенных ниже примерах используется база данных и файлы форматирования, созданные ранее.

### <a name="using-bcp-and-unicode-native-format-to-export-data"></a>**Использование bcp и собственного формата Юникода для экспорта данных**<a name="bcp_widenative_export"></a>
Параметр **-N** и команда **OUT** .  Примечание. Файл данных, созданный в этом примере, будет использоваться во всех последующих примерах.  В командной строке введите следующие команды:
```
bcp TestDatabase.dbo.myWidenative OUT D:\BCP\myWidenative.bcp -T -N

REM Review results
NOTEPAD D:\BCP\myWidenative.bcp
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-without-a-format-file"></a>**Использование bcp и собственного формата Юникода для импорта данных без файла форматирования**<a name="bcp_widenative_import"></a>
Параметр **-N** и команда **IN** .  В командной строке введите следующие команды:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -T -N

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-with-a-non-xml-format-file"></a>**Использование bcp и собственного формата Юникода для импорта данных при помощи файла форматирования, не являющегося XML**<a name="bcp_widenative_import_fmt"></a>
Параметры **-N** и **-f** switches и **IN** commи.  В командной строке введите следующие команды:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -f D:\BCP\myWidenative.fmt -T -N

REM Review results is SSMS
```

### <a name="using-bulk-insert-and-unicode-native-format-without-a-format-file"></a>**Использование инструкции BULK INSERT и собственного формата Юникода без файла форматирования**<a name="bulk_widenative"></a>
Аргумент **DATAFILETYPE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
    FROM 'D:\BCP\myWidenative.bcp'
    WITH (
        DATAFILETYPE = 'widenative'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-bulk-insert-and-unicode-native-format-with-a-non-xml-format-file"></a>**Использование инструкции BULK INSERT и собственного формата Юникода с файлом форматирования, не являющимся XML**<a name="bulk_widenative_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
   FROM 'D:\BCP\myWidenative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidenative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-openrowset-and-unicode-native-format-with-a-non-xml-format-file"></a>**Использование OPENROWSET и собственного формата Юникода с файлом форматирования, не являющимся XML**<a name="openrowset_widenative_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative;  -- for testing
INSERT INTO TestDatabase.dbo.myWidenative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidenative.bcp', 
        FORMATFILE = 'D:\BCP\myWidenative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

## <a name="related-tasks"></a>Связанные задачи<a name="RelatedTasks"></a>
Использование форматов данных для массового импорта или экспорта  
-   [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
