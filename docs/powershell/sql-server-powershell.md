---
title: SQL Server PowerShell
description: Узнайте о двух модулях SQL Server PowerShell — SqlServer и SQLPS, — которые включают поставщиков и командлеты PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 89b70725-bbe7-4ffe-a27d-2a40005a97e7
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 06/11/2020
ms.openlocfilehash: fad1ac64dcc1024d5dad091670300b8d39fa0632
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092716"
---
# <a name="sql-server-powershell"></a>SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

**[Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md)**

Существует два модуля SQL Server PowerShell — **[SqlServer](/powershell/module/sqlserver)** и **[SQLPS](/powershell/module/sqlps)** .

Самым актуальным модулем PowerShell является модуль **SqlServer**.

Модуль **SQLPS** входит в состав установки SQL Server (для обеспечения обратной совместимости), но больше не обновляется.

Модуль **SqlServer** содержит обновленные версии командлетов в **SQLPS** и новые командлеты для поддержки последних функций SQL.

Предыдущие версии модуля **SqlServer** *входили* в состав среды [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md), но только с SSMS версий 16.x.

Для работы PowerShell с SSMS версии 17.0 и более поздних установите модуль **SqlServer** из [коллекции PowerShell](https://www.powershellgallery.com/packages/SqlServer).

Вы также можете использовать [PowerShell с Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md).

**Почему модуль SQLPS изменился на SqlServer?**

Для поставки обновлений SQL PowerShell было необходимо изменить удостоверение модуля SQL PowerShell, а также программу-оболочку, известную как *SQLPS.exe*. В связи с эти изменением теперь существует два модуля SQL PowerShell — модуль **SqlServer** и модуль **SQLPS**.  

**Обновите скрипты PowerShell, если вы импортируете модуль SQLPS.**

Если у вас есть скрипты PowerShell, которые выполняют команду `Import-Module -Name SQLPS`, и вы хотите использовать преимущества новых функциональных возможностей поставщика и новых командлетов, необходимо изменить их на `Import-Module -Name SqlServer`. Новый модуль устанавливается в папку `%ProgramFiles%\WindowsPowerShell\Modules\SqlServer`. Поэтому не нужно обновлять переменную $env:PSModulePath. При наличии скриптов, использующих версию модуля с именем **SqlServer**, созданную сторонними производителями или сообществом, используйте параметр Prefix, чтобы имена не конфликтовали.

Рекомендуется запустить сценарий с помощью *Import-Module SQLServer*, чтобы избежать проблем параллельной установки, если модуль SQLPS установлен на том же компьютере.

Этот раздел относится к скриптам, выполняемым из PowerShell, а не агента SQL. Новый модуль можно использовать с шагами задания агента SQL с помощью [#NOSQLPS](#sql-server-agent).

## <a name="sql-server-powershell-components"></a>Компоненты SQL Server PowerShell

Компоненты модуля **SqlServer**:

- [Поставщики PowerShell](/powershell/module/microsoft.powershell.core/about/about_providers) — предоставляет простой механизм навигации, аналогичный путям в файловой системе. Можно построить пути, аналогичные путям файловой системы, где диску соответствует управляющая объектная модель SQL Server, а узлы основаны на классах объектной модели. Затем можно использовать привычные команды, такие как **cd** и **dir** , чтобы перемещаться по путям, аналогично переходу по структуре папок в окне командной строки. Для выполнения действий на узлах пути можно использовать другие команды, например **ren** или **del**.

- Набор командлетов, которые поддерживают такие действия, как запуск скрипта **sqlcmd**, содержащего инструкции Transact-SQL или XQuery.  

- Поставщик AS и командлеты, которые ранее устанавливались отдельно.

## <a name="sql-server-versions"></a>Версии SQL Server

Командлеты SQL PowerShell можно использовать для управления экземплярами базы данных SQL Azure, Azure Synapse Analytics и во всех [поддерживаемых продуктах SQL Server](https://support.microsoft.com/lifecycle/search/1044).

## <a name="sql-server-identifiers-that-contain-characters-not-supported-in-powershell-paths"></a>Идентификаторы SQL Server, содержащие символы, не поддерживаемые в путях Windows PowerShell

Командлеты **Encode-Sqlname** и **Decode-Sqlname** помогают указать идентификаторы SQL Server, содержащие символы, не поддерживаемые в путях Windows PowerShell. Дополнительные сведения см. в статье [SQL Server Identifiers in PowerShell](sql-server-identifiers-in-powershell.md).

Используйте командлет **Convert-UrnToPath**, чтобы преобразовать уникальное имя ресурса для объекта ядра СУБД в путь для поставщика SQL Server PowerShell. Дополнительные сведения см. в статье [Convert URNs to SQL Server Provider Paths](/powershell/module/sqlserver/Convert-UrnToPath).
  
## <a name="query-expressions-and-unique-resource-names"></a>Выражения запросов и уникальные имена ресурсов  

Выражения запроса — это строки, которые используют синтаксис, напоминающий XPath для указания набора условий, перечисляющих один или несколько объектов в иерархии объектной модели. Уникальное имя ресурса (URN) — это определенный тип строки выражения запроса, который уникально определяет один объект. Дополнительные сведения см. в статье [Query Expressions and Uniform Resource Names](query-expressions-and-uniform-resource-names.md).

## <a name="sql-server-agent"></a>Агент SQL Server

Модуль, используемый агентом SQL Server, не изменился. Таким образом, задания агента SQL Server, которые содержат шаги задания типа PowerShell, используют модуль SQLPS. Дополнительные сведения: [Запуск PowerShell с помощью агента SQL Server](run-windows-powershell-steps-in-sql-server-agent.md). Однако начиная с версии SQL Server 2019 вы можете отключить SQLPS. Для этого в первой строке шага задания типа PowerShell можно добавить `#NOSQLPS`, чтобы агент SQL не запускал автоматическую загрузку модуля SQLPS. После этого задание агента SQL Server запустит установленную на компьютере версию PowerShell, и вы можете использовать любой другой модуль PowerShell.

Если вы хотите использовать модуль **SqlServer** в шаге задания агента SQL, можно поместить этот код в первые две строки скрипта.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```

## <a name="cmdlet-reference"></a>Справка по командлетам

- [Командлеты SqlServer](/powershell/module/sqlserver)
- [Командлеты SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Дальнейшие действия

- [Скачивание модуля PowerShell SQL Server](download-sql-server-ps-module.md)
- [Командлеты SQL Server PowerShell](/powershell/module/sqlserver)
- [Использование PowerShell с Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)