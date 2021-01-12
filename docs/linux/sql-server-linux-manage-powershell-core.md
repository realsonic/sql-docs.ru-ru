---
title: Управление SQL Server на Linux с помощью PowerShell Core
description: Узнайте больше о SQL Server PowerShell, изучив несколько примеров по использованию SQL Server PowerShell с PowerShell Core (PS Core) в macOS и Linux.
ms.date: 04/22/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: vanto
ms.openlocfilehash: a65ad9ad35e4feb2a55ffbdc259601a7b16106f9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094559"
---
# <a name="manage-sql-server-on-linux-with-powershell-core"></a>Управление SQL Server на Linux с помощью PowerShell Core

В этой статье рассматривается модуль [SQL Server PowerShell](../powershell/sql-server-powershell.md) и приводится несколько примеров того, как можно использовать его для работы с PowerShell Core в macOS и Linux. PowerShell Core — это проект с открытым кодом на сайте [GitHub](https://github.com/powershell/powershell).

## <a name="cross-platform-editor-options"></a>Параметры кроссплатформенного редактора

Все инструкции, приведенные ниже для PowerShell Core, будут работать и в обычном терминале. Их также можно выполнять из терминала в VS Code или Azure Data Studio.  Как VS Code, так и Azure Data Studio доступны в macOS и Linux.  Дополнительные сведения об Azure Data Studio см. в [этом кратком руководстве](../azure-data-studio/quickstart-sql-server.md).  Кроме того, можно использовать [расширение PowerShell](../azure-data-studio/extensions/powershell-extension.md).

## <a name="installing-powershell-core"></a>Установка PowerShell Core

Дополнительные сведения об установке PowerShell Core на различных поддерживаемых и экспериментальных платформах см. в следующих статьях:

- [Установка PowerShell Core в Windows](/powershell/scripting/install/installing-powershell-core-on-windows)
- [Установка PowerShell Core в Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Установка PowerShell Core в macOS](/powershell/scripting/install/installing-powershell-core-on-macos)
- [Установка PowerShell Core в ARM](/powershell/scripting/install/powershell-core-on-arm)

## <a name="install-the-sqlserver-module"></a>Установка модуля SqlServer

Модуль `SqlServer` доступен в [коллекции PowerShell](https://www.powershellgallery.com/packages/SqlServer/). При работе с SQL Server следует всегда использовать последнюю версию модуля SqlServer PowerShell.

Чтобы установить модуль SqlServer, откройте сеанс PowerShell Core и выполните следующий код:

```powerhsell
Install-Module -Name SqlServer
```

Дополнительные сведения об установке модуля SqlServer из коллекции PowerShell см. на [этой странице](../powershell/download-sql-server-ps-module.md).

## <a name="using-the-sqlserver-module"></a>Использование модуля SqlServer

Для начала запустим PowerShell Core.  Если вы используете macOS или Linux, откройте *сеанс терминала* на своем компьютере и введите **pwsh**, чтобы запустить новый сеанс PowerShell Core.  В Windows нажмите клавиши <kbd>Win</kbd>+<kbd>R</kbd> и введите `pwsh`, чтобы запустить новый сеанс PowerShell Core.

```
pwsh
```

В SQL Server представлен модуль PowerShell под названием **SqlServer**. Вы можете использовать модуль **SqlServer** для импорта компонентов SQL Server (поставщик и командлеты SQL Server) в скрипт или среду PowerShell.

Скопируйте следующую команду и вставьте ее в командную строку PowerShell, чтобы импортировать модуль **SqlServer** в текущий сеанс PowerShell:

```powershell
Import-Module SqlServer
```

Введите следующую команду в командной строке PowerShell, чтобы убедиться в том, что модуль **SqlServer** был импортирован правильно:

```powershell
Get-Module -Name SqlServer
```

В PowerShell должны отображаться данные примерно следующего вида:

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     21.1.18102 SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>Подключение к SQL Server и получение сведений о сервере

В приведенных ниже инструкциях мы используем PowerShell Core для подключения к экземпляру SQL Server на Linux и просмотра некоторых свойств сервера.

Скопируйте следующие команды и вставьте их в командную строку PowerShell. При запуске этих команд в PowerShell будут выполнены следующие действия:
- отображение диалогового окна с запросом на ввод имени узла и IP-адреса экземпляра;
- открытие диалогового окна *Запрос учетных данных PowerShell*, в котором необходимо ввести соответствующие сведения; для подключения к экземпляру SQL Server на Linux вы можете использовать свои *имя пользователя SQL* и *пароль SQL*;
- использование командлета **Get-SqlInstance** для подключения к **серверу** и просмотра некоторых свойств.

При необходимости можно заменить переменную `$serverInstance` IP-адресом или именем узла вашего экземпляра SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and return a few properties
Get-SqlInstance -ServerInstance $serverInstance -Credential $credential
# done
```

В PowerShell должны отображаться данные примерно следующего вида:

```
Instance Name                   Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------                   -------    ------------ -----------  ------------ ----------------
your_server_instance            14.0.3048  RTM          CU13         Linux        Ubuntu
```

> [!NOTE]
> Если эти значения не отображаются, скорее всего, подключение к целевому экземпляру SQL Server установить не удалось. Убедитесь в том, что эти же данные можно использовать для подключения из SQL Server Management Studio. Затем ознакомьтесь с [рекомендациями по устранению неполадок с подключением](sql-server-linux-troubleshooting-guide.md#connection).

## <a name="using-the-sql-server-powershell-provider"></a>Использование поставщика SQL Server для PowerShell

В качестве альтернативы для подключения к экземпляру SQL Server можно использовать [поставщик SQL Server PowerShell](../powershell/sql-server-powershell-provider.md).  Он позволяет работать с экземпляром SQL Server из командной строки так же, как с древовидной структурой в обозревателе объектов.  По умолчанию этот поставщик представлен в виде диска PSDrive с именем `SQLSERVER:\`, который может использоваться для подключения к экземплярам SQL Server, доступным вашей учетной записи домена, и работы с ними.  Дополнительные сведения о настройке проверки подлинности Active Directory для SQL Server на Linux см. в разделе [Шаги настройки](./sql-server-linux-active-directory-auth-overview.md#configuration-steps).

Также вы можете использовать проверку подлинности SQL в поставщике SQL Server PowerShell. Для этого с помощью командлета `New-PSDrive` создайте новый диск PSDrive и укажите учетные данные для подключения.

В приведенном ниже примере демонстрируется создание диска PSDrive с использованием проверки подлинности SQL.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

New-PSDrive -Name SQLonDocker -PSProvider SqlServer -Root 'SQLSERVER:\SQL\localhost,10002\Default\' -Credential $credential
```

Чтобы убедиться в том, что диск был создан, выполните командлет `Get-PSDrive`.

```powershell
Get-PSDrive
```

После создания нового диска PSDrive вы можете начать работу с ним.

```powershell
dir SQLonDocker:\Databases
```

Выходные данные могут выглядеть следующим образом.  Обратите внимание, что эти данные аналогичны тем, которые SSMS отображает в узле "Базы данных".  В нем представлены пользовательские, а не системные базы данных.

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.76 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.57 MB Simple       140 sa
```

Чтобы просмотреть все базы данных в вашем экземпляре, можно использовать командлет `Get-SqlDatabase`.

## <a name="get-databases"></a>Получение баз данных

Один из важных командлетов, которые следует знать, — `Get-SqlDatabase`.  Для многих операций с базами данных или содержащимися в них объектами можно использовать командлет `Get-SqlDatabase`.  Если заданы значения параметров `-ServerInstance` и `-Database`, будут извлечены только эти объекты базы данных.  Тем не менее, если вы зададите только параметр `-ServerInstance`, будет возвращен полный список баз данных в этом экземпляре.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

# Connect to the Instance and retrieve all databases
Get-SqlDatabase -ServerInstance ServerB -Credential $credential
```

Вот пример данных, которые возвращаются командой Get-SqlDatabase, приведенной выше:

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.88 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.63 MB Simple       140 sa
master               Normal        6.00 MB  600.00 KB Simple       140 sa
model                Normal       16.00 MB    5.70 MB Full         140 sa
msdb                 Normal       15.50 MB    1.14 MB Simple       140 sa
tempdb               Normal       16.00 MB    5.49 MB Simple       140 sa

```

## <a name="examine-sql-server-error-logs"></a>Проверка журналов ошибок SQL Server

Далее описывается использование PowerShell Core для проверки журналов ошибок подключения в вашем экземпляре SQL Server на Linux.

Скопируйте следующие команды и вставьте их в командную строку PowerShell. Это может занять несколько минут. Данные команды выполняют следующие действия:
- отображение диалогового окна с запросом на ввод имени узла и IP-адреса экземпляра;
- открытие диалогового окна *Запрос учетных данных PowerShell*, в котором необходимо ввести соответствующие сведения; для подключения к экземпляру SQL Server на Linux вы можете использовать свои *имя пользователя SQL* и *пароль SQL*;
- Использование командлета **Get-SqlErrorLog** для подключения к экземпляру SQL Server на Linux и извлечения журналов ошибок, обнаруженных со **вчерашнего дня**

При необходимости можно заменить переменную `$serverInstance` IP-адресом или именем узла вашего экземпляра SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday
# done
```

## <a name="explore-cmdlets-currently-available-in-ps-core"></a>Командлеты, доступные в настоящее время в PowerShell Core
Хотя модуль SqlServer сейчас содержит 109 командлетов Windows PowerShell, только 62 из них доступны в PowerShell Core. Ниже приведен их полный список.  Подробную документацию по всем командлетам модуля SqlServer см. в [справочнике по командлетам SqlServer](/powershell/module/sqlserver/).

Приведенная ниже команда выводит все командлеты, доступные в используемой версии PowerShell.

```powershell
Get-Command -Module SqlServer -CommandType Cmdlet |
Sort-Object -Property Noun |
SELECT Name
```

- ConvertFrom-EncodedSqlName
- ConvertTo-EncodedSqlName
- Get-SqlAgent
- Get-SqlAgentJob
- Get-SqlAgentJobHistory
- Get-SqlAgentJobSchedule
- Get-SqlAgentJobStep
- Get-SqlAgentSchedule
- Invoke-SqlAssessment
- Get-SqlAssessmentItem
- Remove-SqlAvailabilityDatabase
- Resume-SqlAvailabilityDatabase
- Add-SqlAvailabilityDatabase
- Suspend-SqlAvailabilityDatabase
- New-SqlAvailabilityGroup
- Set-SqlAvailabilityGroup
- Remove-SqlAvailabilityGroup
- Switch-SqlAvailabilityGroup
- Join-SqlAvailabilityGroup
- Revoke-SqlAvailabilityGroupCreateAnyDatabase
- Grant-SqlAvailabilityGroupCreateAnyDatabase
- New-SqlAvailabilityGroupListener
- Set-SqlAvailabilityGroupListener
- Add-SqlAvailabilityGroupListenerStaticIp
- Set-SqlAvailabilityReplica
- Remove-SqlAvailabilityReplica
- New-SqlAvailabilityReplica
- Set-SqlAvailabilityReplicaRoleToSecondary
- New-SqlBackupEncryptionOption
- Get-SqlBackupHistory
- Invoke-Sqlcmd
- New-SqlCngColumnMasterKeySettings
- Remove-SqlColumnEncryptionKey
- Get-SqlColumnEncryptionKey
- Remove-SqlColumnEncryptionKeyValue
- Add-SqlColumnEncryptionKeyValue
- Get-SqlColumnMasterKey
- Remove-SqlColumnMasterKey
- New-SqlColumnMasterKey
- Get-SqlCredential
- Set-SqlCredential
- New-SqlCredential
- Remove-SqlCredential
- New-SqlCspColumnMasterKeySettings
- Get-SqlDatabase
- Restore-SqlDatabase
- Backup-SqlDatabase
- Set-SqlErrorLog
- Get-SqlErrorLog
- New-SqlHADREndpoint
- Set-SqlHADREndpoint
- Get-SqlInstance
- Add-SqlLogin
- Remove-SqlLogin
- Get-SqlLogin
- Set-SqlSmartAdmin
- Get-SqlSmartAdmin
- Read-SqlTableData
- Write-SqlTableData
- Read-SqlViewData
- Read-SqlXEvent
- Convert-UrnToPath

## <a name="see-also"></a>См. также раздел
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)