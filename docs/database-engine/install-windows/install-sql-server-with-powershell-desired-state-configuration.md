---
title: 'Установка: PowerShell Desired State Configuration'
description: Установите SQL Server с помощью PowerShell DSC и узнайте о начальной установке автономного экземпляра SQL Server 2017 в Windows Server 2016.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.devlang: PowerShell
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
author: randomnote1
ms.author: dareist
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d0c3828e26e5bde24d678cfac8959264adafcb13
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091598"
---
# <a name="install-sql-server-with-powershell-desired-state-configuration"></a>Установка SQL Server с помощью PowerShell Desired State Configuration

Часто ли вам приходится буквально "на автомате" пролистывать интерфейс установки SQL Server, нажимая одни и те же кнопки и вводя давно знакомую информацию? А потом, после установки, вы вдруг вспоминаете: "Надо же было еще указать группу DBA в роли **sysadmin**!" Затем приходится выполнять следующие действия:
* перезапуск в однопользовательском режиме;
* добавление пользователей или групп;
* возврат SQL Server обратно в многопользовательский режим;
* тестирование. 

Но еще хуже то, что вы больше не уверены в установке: "Вдруг что-то еще забылось?" Вы можете задаться именно таким вопросом.

На помощь приходит [PowerShell Desired State Configuration (DSC)](/powershell/scripting/dsc/overview/overview). DSC позволяет создать один шаблон конфигурации и использовать его на сотнях тысяч серверов. В зависимости от сборки может потребоваться настроить несколько параметров программы установки. Но это несущественная проблема, так как все стандартные параметры можно оставить на месте. Это исключает возможность того, что вы забудете ввести важный параметр.

Эта статья рассказывает о начальной установке автономного экземпляра SQL Server 2017 на Windows Server 2016 с использованием DSC-ресурса **SqlServerDsc**. Базовые знания по DSC полезны, так как мы не станем детально рассматривать, как работает DSC.

Для этого пошагового руководства потребуются следующие элементы.

- Машина под управлением Windows Server 2016.
- Установочный носитель SQL Server 2017.
- DSC-ресурс **SqlServerDsc**.

## <a name="prerequisites"></a>Предварительные требования

В большинстве случаев для обработки предварительных требований будет использоваться служба DSC. Однако в целях демонстрации мы делаем это вручную.

## <a name="install-the-sqlserverdsc-dsc-resource"></a>Установка DSC-ресурса SqlServerDsc

DSC-ресурс [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc) можно скачать в [коллекции PowerShell](https://www.powershellgallery.com/) с помощью командлета [Install-Module](/powershell/module/powershellget/Install-Module). 

> [!NOTE]
> Для установки модуля служба PowerShell должна быть запущена **от имени администратора**.

```PowerShell
Install-Module -Name SqlServerDsc
```

### <a name="get-the-sql-server-2017-installation-media"></a>Получение установочного носителя SQL Server 2017
Скачайте установочный носитель SQL Server 2017 на сервер. Мы скачали SQL Server 2017 Enterprise в подписке Visual Studio и скопировали файл ISO сюда: `C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso`.

Теперь извлеките ISO в каталог.

```PowerShell
New-Item -Path C:\SQL2017 -ItemType Directory
$mountResult = Mount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso' -PassThru
$volumeInfo = $mountResult | Get-Volume
$driveInfo = Get-PSDrive -Name $volumeInfo.DriveLetter
Copy-Item -Path ( Join-Path -Path $driveInfo.Root -ChildPath '*' ) -Destination C:\SQL2017\ -Recurse
Dismount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso'
```

## <a name="create-the-configuration"></a>Создание конфигурации

### <a name="configuration"></a>Конфигурация

Создайте функцию конфигурации, которая будет вызываться для создания документов в формате [MOF](/windows/desktop/WmiSdk/managed-object-format--mof-).

```PowerShell
Configuration SQLInstall
{...}
```

### <a name="modules"></a>Модули

Импортируйте модули в текущий сеанс. Эти модули указывают документу конфигурации правила сборки MOF-документов. Они также сообщают подсистеме DSC, как применять MOF-документы на сервере:

```PowerShell
Import-DscResource -ModuleName SqlServerDsc
```

### <a name="resources"></a>Ресурсы

#### <a name="net-framework"></a>.NET Framework

SQL Server основан на .NET Framework. Поэтому нам нужно убедиться, что эта платформа установлена, прежде чем установить SQL Server. Для установки компонента **Windows "Net-Framework-45-Core"** используется ресурс **WindowsFeature**.

```PowerShell
WindowsFeature 'NetFramework45'
{
     Name = 'Net-Framework-45-Core'
     Ensure = 'Present'
}
```

#### <a name="sqlsetup"></a>SqlSetup

Ресурс **SqlSetup** сообщает DSC параметры для установки SQL Server. Для базовой установки необходимы следующие параметры.

- **InstanceName**. Имя экземпляра. Для экземпляра по умолчанию используйте **MSSQLSERVER**.
- **Features**. Компоненты для установки. В этом примере мы устанавливаем только компонент **SQLEngine**.
- **SourcePath**. Путь к установочному носителю SQL. В этом примере установочный носитель SQL сохранен в папке `C:\SQL2017`. Чтобы занимаемое место на сервере было минимальным, можно использовать сетевую папку.
- **SQLSysAdminAccounts**. Пользователи или группы, которые будут участниками роли **sysadmin**. В этом примере мы предоставляем доступ **sysadmin** локальной группе Administrators. 

> [!NOTE]
> Мы не рекомендуем эту конфигурацию в среде с высоким уровнем безопасности.

Полный список и описание параметров, доступных в **SqlSetup**, см. в [GitHub-репозитории SqlServerDsc](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlsetup).

Ресурс **SqlSetup** лишь устанавливает SQL и **не сохраняет** применяемые параметры. Например, предположим, что параметр **SQLSysAdminAccounts** задается во время установки. Администратор может добавить или удалить учетные записи для входа в систему в роли **sysadmin**. При этом ресурс **SqlSetup** не будет затронут. Если вы хотите, чтобы DSC принудительно применила членство в роли **sysadmin**, используйте ресурс [SqlServerRole](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlserverrole).

#### <a name="finish-configuration"></a>Завершение конфигурации

```PowerShell
Configuration SQLInstall
{
     Import-DscResource -ModuleName SqlServerDsc

     node localhost
     {
          WindowsFeature 'NetFramework45'
          {
               Name   = 'NET-Framework-45-Core'
               Ensure = 'Present'
          }

          SqlSetup 'InstallDefaultInstance'
          {
               InstanceName        = 'MSSQLSERVER'
               Features            = 'SQLENGINE'
               SourcePath          = 'C:\SQL2017'
               SQLSysAdminAccounts = @('Administrators')
               DependsOn           = '[WindowsFeature]NetFramework45'
          }
     }
}
```

## <a name="build-and-deploy"></a>Сборка и развертывание

### <a name="compile-the-configuration"></a>Компиляция конфигурации

Укажите через точку скрипт конфигурации:

```PowerShell
. .\SQLInstallConfiguration.ps1
```

Выполните функцию конфигурации:

```PowerShell
SQLInstall
```

Каталог **SQLInstall** создается в рабочем каталоге. Он содержит файл с именем **localhost.mof**. Содержимое MOF-файла — скомпилированная конфигурация DSC.

### <a name="deploy-the-configuration"></a>Развертывание конфигурации

Чтобы запустить развертывание SQL Server с DCS, вызовите командлет **Start-DscConfiguration**. Командлету необходимо указать следующие параметры:

- **Path**. Путь к папке с документами MOF для развертывания Например, `C:\SQLInstall`.
- **Wait**. Ожидание завершения задания конфигурации.
- **Force**. Переопределение существующих конфигураций DSC.
- **Verbose**. Отображение подробных выходных данных. Это помогает устранить неполадки при отправке конфигурации в первый раз.

```PowerShell
Start-DscConfiguration -Path C:\SQLInstall -Wait -Force -Verbose
```

По мере того, как применяется конфигурация, в подробных выходных данных будет показано, что происходит. Если никаких ошибок (красный текст) не возникнет, то на экране появится сообщение **Операция "Invoke CimMethod" завершена.** Это значит, что SQl Server установлен.

## <a name="validate-installation"></a>Проверка установки

### <a name="dsc"></a>DSC

Командлеты [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/test-dscconfiguration) позволяют определить, соответствует ли текущее состояние сервера желаемому. В нашем случае это установка SQL Server. Результатом выполнения **Test-DscConfiguration** должно быть значение **True** (Истина).

```PowerShell
PS C:\> Test-DscConfiguration
True
```

### <a name="services"></a>Службы

Список служб теперь должен возвращать службы SQL Server:

```PowerShell
PS C:\> Get-Service -Name *SQL*
Status  Name           DisplayName
------  ----           -----------
Running MSSQLSERVER    SQL Server (MSSQLSERVER)
Stopped SQLBrowser     SQL Server Browser
Running SQLSERVERAGENT SQL Server Agent (MSSQLSERVER)
Running SQLTELEMETRY   SQL Server CEIP service (MSSQLSERVER)
Running SQLWriter      SQL Server VSS Writer
```

### <a name="sql-server"></a>SQL Server

```PowerShell
PS C:\> & sqlcmd -S $env:COMPUTERNAME
1> SELECT @@SERVERNAME
2> GO
1> quit
```

## <a name="see-also"></a>См. также раздел

[Обзор Windows PowerShell Desired State Configuration](/powershell/scripting/dsc/overview/overview)

[Установка SQL Server из командной строки](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

[Установка SQL Server с помощью файла конфигурации](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)
