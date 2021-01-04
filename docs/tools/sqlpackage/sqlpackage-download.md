---
title: Скачивание и установка sqlpackage
description: Скачивание и установка sqlpackage для Windows, macOS или Linux
ms.custom: tools|sos
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 10/02/2020
ms.openlocfilehash: c86dde50390bdf14719da96f7fc3d0cc35a9c440
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577948"
---
# <a name="download-and-install-sqlpackage"></a>Скачивание и установка sqlpackage

Программа sqlpackage выполняется в Windows, macOS и Linux.

Скачайте и установите последний выпуск платформы .NET Framework, а также предварительные версии для macOS и Linux:

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|[Windows](#get-sqlpackage-for-windows)|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2143544)|18 сентября 2020 г.| 18.6 | 15.0.4897.1 |
|[macOS .NET Core](#get-sqlpackage-net-core-for-macos) |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2143659)|18 сентября 2020 г.| 18.6| 15.0.4897.1 |
|[Linux .NET Core](#get-sqlpackage-net-core-for-linux) |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2143497)|18 сентября 2020 г.| 18.6| 15.0.4897.1 |
|[Windows .NET Core](#get-sqlpackage-net-core-for-windows) |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2143496)|18 сентября 2020 г.| 18.6| 15.0.4897.1 |

Подробнее см. в [заметках о выпуске](release-notes-sqlpackage.md). Чтобы скачать дополнительные языки, воспользуйтесь разделом [Доступные языки](#available-languages).

## <a name="dacfx"></a>DacFx
SqlPackage — это интерфейс командной строки для платформы DacFx, который предоставляет некоторые общедоступные API DacFx. DacServices ([Microsoft.SqlServer.Dac](/dotnet/api/microsoft.sqlserver.dac.dacservices)) — это связанный механизм для интеграции развертывания базы данных в конвейер приложений.  API-интерфейс DacServices доступен в пакете NuGet [Microsoft.SqlServer.DACFx](https://www.NuGet.org/packages/Microsoft.SqlServer.DACFx).  Текущая версия DacFx — 150.4897.1.

Установка пакета NuGet в интерфейсе командной строки .NET выполняется с помощью следующей команды:

```cmd
dotnet add package Microsoft.SqlServer.DACFx
```

>[!NOTE]
> Были опубликованы дополнительные пакеты NuGet с именем DacFx: "Microsoft.SqlServer.DacFx.x64" и "Microsoft.SqlServer.DacFx.x86". В пакете "Microsoft.SqlServer.DACFx" реализована поддержка обеих платформ. Новые ссылки необходимо настраивать на этот пакет, а не на варианты x64 или x86.

## <a name="get-sqlpackage-for-windows"></a>Получение sqlpackage для Windows

Этот выпуск sqlpackage включает стандартные средства установщика Windows и ZIP-файл: 

1. Скачайте и запустите [установщик DacFramework.msi для Windows](https://go.microsoft.com/fwlink/?linkid=2143544).
2. Откройте новое окно командной строки и запустите файл sqlpackage.exe.
    - Программа sqlpackage устанавливается в папку ```C:\Program Files\Microsoft SQL Server\150\DAC\bin```.

## <a name="get-sqlpackage-net-core-for-windows"></a>Получение .NET Core sqlpackage для Windows

1. Скачайте [sqlpackage для Windows](https://go.microsoft.com/fwlink/?linkid=2143496).
2. Чтобы извлечь файл, щелкните пакет правой кнопкой мыши в Проводнике Windows и выберите действие "Извлечь все...", а затем укажите целевой каталог.
3. Откройте окно терминала и перейдите в ту папку, куда только что извлекли содержимое sqlpackage.

   ```cmd
   > sqlpackage
   ```

## <a name="get-sqlpackage-net-core-for-macos"></a>Получение .NET Core sqlpackage для macOS

1. Скачайте [sqlpackage для macOS](https://go.microsoft.com/fwlink/?linkid=2143659).
2. Чтобы извлечь файл и запустить sqlpackage, откройте новое окно терминала и введите следующие команды:

   ```bash
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-osx-<version string>.zip -d ~/sqlpackage 
   $ echo 'export PATH="$PATH:~/sqlpackage"' >> ~/.bash_profile
   $ source ~/.bash_profile
   $ sqlpackage
   ```

   > [!NOTE]
   > Возможно, для запуска sqlpackage в macOS потребуется изменить параметры безопасности. Используйте следующие команды для взаимодействия с Gatekeeper из командной строки.

   **До выполнения sqlpackage:**
   ```bash
   $ sudo spctl --master-disable
   ```

   **После выполнения sqlpackage:**
   ```bash
   $ sudo spctl --master-enable
   ```

## <a name="get-sqlpackage-net-core-for-linux"></a>Получение .NET Core sqlpackage для Linux

1. Скачайте [sqlpackage для Linux](https://go.microsoft.com/fwlink/?linkid=2143497) с помощью одного из установщиков или архива tar.gz:
2. Чтобы извлечь файл и запустить sqlpackage, откройте новое окно терминала и введите следующие команды:

   ```bash
   $ cd ~
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-linux-<version string>.zip -d ~/sqlpackage 
   $ echo "export PATH=\"\$PATH:$HOME/sqlpackage\"" >> ~/.bashrc
   $ chmod a+x ~/sqlpackage/sqlpackage
   $ source ~/.bashrc
   $ sqlpackage
   ```

   > [!NOTE]
   > В Debian, Redhat и Ubuntu, возможно, будут отсутствовать некоторые зависимости. Чтобы установить эти зависимости с учетом вашей версии Linux, используйте следующие команды:

   **Debian:**

   ```bash
   $ sudo apt-get install libunwind8
   ```

   **Redhat:**

   ```bash
   $ yum install libunwind
   $ yum install libicu
   ```

   **Ubuntu:**

   ```bash
   $ sudo apt-get install libunwind8

   # install the libicu library based on the Ubuntu version
   $ sudo apt-get install libicu52      # for 14.x
   $ sudo apt-get install libicu55      # for 16.x
   $ sudo apt-get install libicu57      # for 17.x
   $ sudo apt-get install libicu60      # for 18.x
   ```

## <a name="uninstall-sqlpackage"></a>Удаление sqlpackage

Если вы установили sqlpackage с помощью установщика Windows, удаление выполняется так же, как и для любого приложения Windows.

Если вы установили sqlpackage с помощью ZIP-файла или другого архива, удалите файлы.

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

sqlpackage на основе .NET Core 3.1 выполняется в Windows, macOS и Linux.  К sqlpackage применяются [требования к ОС для .NET Core 3.1](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md).

### <a name="windows-x64"></a>Windows (x64)

- Windows 10 (1607+)
- Windows 8.1
- Windows 7 с пакетом обновления 1 (SP1)
- Windows Server Core
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019

### <a name="macos"></a>MacOS

- macOS 10.15 "Catalina"
- macOS 10.14 "Mojave"
- macOS 10.13 "High Sierra"

### <a name="linux-x64"></a>Linux (x64)

- Red Hat Enterprise Linux 7+
- SUSE Linux Enterprise Server v12 SP2+
- Ubuntu 16.04+

## <a name="available-languages"></a>Доступные языки

Этот выпуск sqlpackage можно установить для следующих языков:

sqlpackage для Windows:  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40a)

.NET Core sqlpackage для Windows:  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40a)

.NET Core sqlpackage для macOS:  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40a)

.NET Core sqlpackage для Linux:  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40a)


## <a name="next-steps"></a>Next Steps

- См. подробнее о [sqlpackage](sqlpackage.md)

[Заявление Майкрософт о конфиденциальности](https://go.microsoft.com/fwlink/?LinkId=521839)