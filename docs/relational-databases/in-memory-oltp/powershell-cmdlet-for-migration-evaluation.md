---
title: Командлет PowerShell для оценки миграции | Документация Майкрософт
description: Узнайте о командлете Save-SqlMigrationReport, который оценивает пригодность объектов в базе данных SQL Server к миграции в In-Memory OLTP.
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 117250d3-9982-47fe-94fd-6f29f6159940
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 78b4d84a98d7ea2c753a673a222a1a19e0b7e5e2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641298"
---
# <a name="powershell-cmdlet-for-migration-evaluation"></a>Командлет PowerShell для оценки миграции

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Командлет `Save-SqlMigrationReport` — это средство, которое оценивает пригодность нескольких объектов в базе данных SQL Server к миграции.

В настоящее время доступна оценка пригодности к миграции с помощью этого командлета только для выполняющейся в памяти OLTP. Командлет можно запускать в среде Windows PowerShell с повышенными привилегиями и sqlps.

В качестве альтернативы непосредственному запуску этого командлета PowerShell можно выполнить командлет неявным образом с помощью SQL Server Management Studio (SSMS). В **обозревателе объектов** SSMS можно щелкнуть правой кнопкой мыши таблицу и выбрать **помощник по оптимизации памяти**.

## <a name="syntax"></a>Синтаксис

```
Save-SqlMigrationReport
    -FolderPath <output_path>
    [ -MigrationType <migration_scenario_type> ]
    [
        [ -Server <server_name> -Database <database_name>
            [ -Schema <schema_name> ] [ -Object <object_name> ]
        ]
       |
        [ -InputObject <smo_object> ]
    ]
;
```

## <a name="parameters"></a>Параметры

В таблице ниже описаны параметры.

Следует обратить особое внимание на ряд синтаксических аспектов. Если указан параметр `-InputObject`, нельзя указать следующие параметры:

- `-Server`
- `-Database`
- `-Schema`
- `-Object`

И наоборот, если _не_ указан параметр `-InputObject`, необходимо указать параметры `-Server` и `-Database`. Если указан параметр `-Server`, можно ограничить область, указав параметр `-Object` или `-Schema`, либо и тот и другой.

| Имя параметра | Описание |
| :------------- | :---------- |
| База данных | Имя целевой базы данных SQL Server. Обязателен, если является обязательным `-Server`.<br/><br/> Необязателен в sqlps. |
| FolderPath | Папка, в которую командлету следует поместить созданные отчеты.<br/><br/> Обязательный элемент. |
| InputObject | Целевой объект SMO для командлета.<br/><br/> Обязателен в среде Windows PowerShell, если не указан параметр `-Server`.<br/><br/> Необязателен в sqlps. |
| MigrationType | Тип сценария миграции для командлета. В настоящее время единственным является значение по умолчанию — **OLTP**.<br/><br/> Необязательный параметр. |
| Объект | Имя объекта, по которому требуется отчет. Это может быть таблица или хранимая процедура. |
| Пароль | Обязателен, если является обязательным `-Username`. |
| схема | Имя схемы, владеющей объектом, по которому будет создан отчет.<br/><br/> Необязательный параметр. |
| Сервер | Имя целевого экземпляра SQL Server. Обязателен в среде Windows PowerShell, если не указан параметр `-InputObject`.<br/><br/> Необязателен в sqlps. |
| Имя пользователя | Требуется при подключении через проверку подлинности SQL Server, в отличие от проверки подлинности Windows. В противном случае не указывается. |
| &nbsp; | &nbsp; |

## <a name="prerequisites"></a>Предварительные требования

Перед запуском этого командлета необходимо сначала установить модуль с именем **SqlServer**:

- `Install-Module -Name SqlServer`

> [!NOTE]
> Старый модуль `SQLPS` больше не обслуживается. Используйте новый модуль `SqlServer`.

Дополнительные сведения см. в разделе [Install SQL Server PowerShell module](../../powershell/download-sql-server-ps-module.md).

## <a name="example-cmdlet-line"></a>Пример строки командлета

Ниже приводится фактическая строка командлета, которая использовалась для создания отчета, показанного далее в этой статье.

```powershell
Save-SqlMigrationReport `
  -FolderPath 'C:\Test\PowerShell-ps1\Save-SqlMigrationReport\' `
  -Server 'MyUserName123456.database.windows.net' `
  -Database 'MyDatabaseName_31' `
  -Schema 'dbo' `
  -Object 'Table2' `
  -Username 'MyUserName' `
  -Password 'MyPassword' `
  -MigrationType 'OLTP' `
;
```

## <a name="example-output-report"></a>Пример итогового отчета

В папке, указанной для параметра `-FolderPath`, при выполнении этого командлета создаются два следующих пути к папкам. Оба пути начинаются со _значения\_имени_ сервера:

- MyDatabaseName_31\Tables\
- MyDatabaseName_31\Stored Procedures\

Каждый файл отчета по объекту хранится в соответствующей папке.

Имена файлов отчетов имеют расширение **.html**. Например, имя фактически созданного файла — **MigrationAdvisorChecklistReport_Table2_20190728.html**.

HTML-код в основном является таблицей из двух столбцов со следующими заголовками:

- Описание
- Результат проверки

Далее приведен фактический пример HTML-отчета для одной таблицы.

```html
<?xml version="1.0" encoding="utf-8"?>
<html>
  <head>
    <title>Memory optimization checklist for [MyDatabaseName_31].[Table2]</title>
  </head>
  <body>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 14pt;">
      <b>Memory optimization checklist for [MyDatabaseName_31].[Table2]</b>
    </p>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 10pt;">
      <b>Report Date/Time:</b>7/28/2019 2:25 PM<br /></p>
    <table border="1" cellpadding="5" cellspacing="0" STYLE="font-family: Verdana, Arial, sans-serif; font-size: 9pt;">
      <tr style="background-color:Silver">
        <th colspan="2" align="center">Description</th>
        <th align="center">Validation Result</th>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported data types are defined on this table. </td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No sparse columns are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No identity columns with unsupported seed and increment are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No foreign key relationships are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported constraints are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No unsupported indexes are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported triggers are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">Post migration row size does not exceed the row size limit of memory-optimized tables.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">Table is not partitioned or replicated.</td>
        <td>Succeeded</td>
      </tr>
    </table>
  </body>
</html>
```

Ниже показано, как примерно выглядит эта таблица.

| Описание | Результат проверки |
| :---------- | :---------------- |
| Для этой таблицы не определено ни одного неподдерживаемого типа данных. | Выполнено |
| Для этой таблицы не определено ни одного разреженного столбца. | Выполнено |
| Для этой таблицы не заданы столбцы идентификаторов с начальными значениями и значениями приращения, которые не поддерживаются. | Выполнено |
| Для этой таблицы не определено ни одной связи по внешнему ключу. | Выполнено |
| Для этой таблицы не определено ни одного неподдерживаемого ограничения. | Выполнено |
| Для этой таблицы не определено ни одного неподдерживаемого индекса. | Выполнено |
| Для этой таблицы не определены неподдерживаемые триггеры. | Выполнено |
| Размер строк, которые сформировались после переноса, не превышает ограничение размера строк для оптимизированных для памяти таблиц. | Выполнено |
| Таблица не секционирована и не реплицирована. | Выполнено |
| &nbsp; | &nbsp; |

## <a name="related-links"></a>Связанные ссылки

- Справочная документация: [Save-SqlMigrationReport](/powershell/module/sqlserver/save-sqlmigrationreport)
