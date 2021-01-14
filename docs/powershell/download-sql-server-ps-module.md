---
title: Загрузка модуля PowerShell (SQL Server)
description: Узнайте, как установить модуль SqlServer PowerShell, который предоставляет командлеты, поддерживающие последние функции SQL, а также содержит обновленные версии командлетов в модуле SQLPS.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 960f146d7cd6943d27a0ecce2c1edaedc41de472
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102904"
---
# <a name="install-the-sql-server-powershell-module"></a>Установка модуля SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта статья содержит инструкции по установке модуля **SqlServer** PowerShell.

## <a name="powershell-modules-for-sql-server"></a>Модули PowerShell для SQL Server

Существует два модуля PowerShell SQL Server.

- **SqlServer**. Модуль SqlServer содержит новые командлеты для поддержки новейших функций SQL. Этот модуль также содержит обновленные версии командлетов в **SQLPS**. Чтобы скачать модуль SqlServer, перейдите к странице [модуля SqlServer в коллекции PowerShell](https://www.powershellgallery.com/packages/Sqlserver).

- **SQLPS**. SQLPS — это модуль, используемый [агентом SQL](sql-server-powershell.md#sql-server-agent) для запуска заданий агента в шагах задания агента с помощью подсистемы PowerShell.

> [!NOTE]
> Версии модуля **SqlServer** PowerShell в коллекции PowerShell поддерживают управление версиями и требуют PowerShell 5.0 или более поздней версии.

Разделы справки можно найти здесь:

- Командлеты [SqlServer](/powershell/module/sqlserver).
- Командлеты [SQLPS](/powershell/module/sqlps).

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

[SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) не устанавливает ни один из модулей PowerShell. Для работы PowerShell с SSMS установите модуль **SqlServer** из [коллекции PowerShell](https://www.powershellgallery.com/packages/Sqlserver).

> [!NOTE]
> В SSMS версии 16.x входит более ранняя версия модуля **SqlServer**.

## <a name="azure-data-studio"></a>Azure Data Studio

[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) не устанавливает ни один из модулей PowerShell. Для работы PowerShell с Azure Data Studio установите модуль **SqlServer** из [коллекции PowerShell](https://www.powershellgallery.com/packages/Sqlserver).

Можно использовать [расширение PowerShell](../azure-data-studio/extensions/powershell-extension.md), которое обеспечивает полноценную поддержку редактора PowerShell в Azure Data Studio.

## <a name="installing-or-updating-the-sqlserver-module"></a>Установка или обновление модуля SqlServer

Чтобы установить модуль **SqlServer** из коллекции PowerShell, запустите сеанс [PowerShell](/powershell/scripting/overview) с правами администратора. Вы также можете запустить Azure Data Studio от имени администратора и выполнить эти команды в сеансе PowerShell во встроенном терминале.

Для выполнения с повышенными правами можно также использовать *Install-Module SQLServer-Scope CurrentUser*. Этот командлет полезен для пользователей, не являющихся администраторами в своей среде. Однако, поскольку область ограничена текущим пользователем, другие пользователи на том же компьютере не могут использовать этот модуль.

### <a name="install-the-sqlserver-module"></a>Установка модуля SqlServer

Чтобы установить модуль SqlServer для всех пользователей, выполните следующую команду в сеансе PowerShell:

```powershell
Install-Module -Name SqlServer
```

### <a name="to-view-the-versions-of-the-sqlserver-module-installed"></a>Просмотр установленных версий модуля SqlServer

Выполните следующую команду, чтобы просмотреть установленные версии модуля SqlServer.

```powershell
Get-Module SqlServer -ListAvailable
```

### <a name="install-for-the-current-user-rather-than-as-an-administrator"></a>Установка для текущего пользователя (не администратора)

Если не удается запустить сеанс PowerShell от имени администратора, можно выполнить установку для текущего пользователя, используя следующую команду:

```powershell
Install-Module -Name SqlServer -Scope CurrentUser
```

### <a name="to-overwrite-a-previous-version-of-the-sqlserver-module"></a>Перезапись предыдущей версии модуля SqlServer

Вы можете перезаписать предыдущую версию с помощью команды `Install-Module`.

```powershell
Install-Module -Name SqlServer -AllowClobber
```

> [!Note]
> PowerShell всегда использует последний установленный модуль.

### <a name="update-the-installed-version-of-the-sqlserver-module"></a>Обновление установленной версии модуля SqlServer

Если доступны обновленные версии модуля **SqlServer**, можно установить более новую версию с помощью следующей команды:

```powershell
Install-Module -Name SqlServer -AllowClobber
```

Вы можете использовать команду `Update-Module`, чтобы установить последнюю версию модуля SQLServer PowerShell, но это не приведет к удалению предыдущих версий. Эта команда устанавливает более новую версию параллельно, чтобы вы могли экспериментировать с последней версией, но по-прежнему иметь предыдущие модули.

Однако если вы не хотите сохранять предыдущие версии модулей, можно удалить их с помощью команды `Uninstall-Module`.

Если установлено более одной версии, можно вывести их список с помощью следующей команды:

```powershell
Get-Module SqlServer -ListAvailable
```

Чтобы удалить предыдущие версии, выполните следующую команду:

```powershell
Uninstall-module -Name SQLServer -RequiredVersion "<version number>" -AllowClobber
```

### <a name="troubleshooting"></a>Устранение неполадок

Если при установке возникли проблемы, см. [документацию по Install-Module](https://www.powershellgallery.com/packages/PowerShellGet/2.2.1) и [справочник по Install-Module](/powershell/module/powershellget/Install-Module).

## <a name="using-a-specific-version-of-the-sqlserver-module"></a>Использование конкретной версии модуля SqlServer

Чтобы использовать конкретную версию модуля, импортируйте ее с определенным номером версии, как показано в следующей команде:

```powershell
Import-Module SqlServer -Version 21.1.18218
```

## <a name="pre-release-versions-of-the-sqlserver-module"></a>Предварительные версии модуля SqlServer

Предварительные (или preview) версии модуля SqlServer могут быть доступны в коллекции PowerShell.

> [!IMPORTANT]
> Их можно найти и установить с помощью обновленных командлетов *Find-Module* и *Install-Module*, которые входят в модуль [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet), передав параметр *-AllowPrerelease*. Чтобы использовать эти командлеты, установите модуль PowerShellGet, а затем откройте новый сеанс.

### <a name="to-discover-pre-release-versions-of-the-sqlserver-module"></a>Обнаружение предварительных версий модуля SqlServer

Чтобы найти предварительные версии модуля SqlServer, выполните следующую команду:

```powershell
Find-Module SqlServer -AllowPrerelease
```

### <a name="to-install-a-specific-pre-release-version-of-the-sqlserver-module"></a>Установка конкретной предварительной версии модуля SqlServer

Чтобы установить конкретную предварительную версию модуля, укажите при установке конкретный номер версии.

Используйте для этого следующую команду:

```powershell
Install-Module SqlServer -RequiredVersion 21.1.18218-preview -AllowPrerelease
```

## <a name="sql-server-powershell-on-linux"></a>SQL Server PowerShell в Linux

Сведения об установке SQL Server PowerShell в Linux см. в [этой статье](../linux/sql-server-linux-manage-powershell-core.md).

## <a name="other-modules"></a>Прочие модули

- [AZ. SQL](https://www.powershellgallery.com/packages/Az.Sql/) — командлеты служб SQL для Azure Resource Manager в Windows PowerShell и PowerShell Core.

- [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc/) — модуль с ресурсами DSC для развертывания и настройки Microsoft SQL Server.

## <a name="cmdlet-reference"></a>Справка по командлетам

- [Командлеты SqlServer](/powershell/module/sqlserver)
- [Командлеты SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Дальнейшие действия

- [SQL Server PowerShell](sql-server-powershell.md)
- [Командлеты SQL Server PowerShell](/powershell/module/sqlserver)
- [Использование PowerShell с Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)