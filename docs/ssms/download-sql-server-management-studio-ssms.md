---
title: Скачивание SQL Server Management Studio (SSMS)
description: Скачайте последнюю версию SQL Server Management Studio (SSMS).
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- установить SSMS, скачать SSMS, последняя версия SSMS
- SQL Server Management Studio
- ssms.exe
- sql man studio
- sql management studio
- установка sql management studio
- скачать sql management studio
- ms sql management studio
- установить sql management studio
- скачать ssms
- sql server ssms
- ssms express
ms.assetid: adafeeef-4255-4924-8042-02f503d599ca
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/17/2020
ms.openlocfilehash: a4798fbc01e015b85e31d9768fd8135af6d202f4
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596867"
---
# <a name="download-sql-server-management-studio-ssms"></a>Скачивание SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

SQL Server Management Studio (SSMS) — это интегрированная среда для управления любой инфраструктурой SQL, от SQL Server до баз данных SQL Azure. SSMS предоставляет средства для настройки, наблюдения и администрирования экземпляров SQL Server и баз данных. С помощью SSMS можно развертывать, отслеживать и обновлять компоненты уровня данных, используемые вашими приложениями, а также создавать запросы и скрипты.

Используйте SSMS для создания запросов к базам данных и хранилищам данных, их проектирования и управления ими, где бы они ни находились: на локальном компьютере или в облаке.

## <a name="download-ssms"></a>Скачать SSMS

:::image type="icon" source="media/download-icon.png" border="false"::: **[Скачать SQL Server Management Studio (SSMS)](https://aka.ms/ssmsfullsetup)**

SSMS 18.8 — это последняя общедоступная (GA) версия SSMS. Если у вас установлена предыдущая общедоступная версия SSMS 18, при установке SSMS 18.8 она будет обновлена до версии 18.8.

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

- Номер выпуска: 18.8
- Номер сборки: 15.0.18369.0
- Дата выпуска: 17 декабря 2020 г.

Если у вас есть замечания и предложения или вы хотите сообщить о проблемах, обратитесь к команде разработчиков SSMS через [пользовательские отзывы на SQL Server](https://aka.ms/sqlfeedback).

При установке SSMS 18.x не обновляются и не заменяются версии SSMS 17.x или более ранние. Среда SSMS 18.x устанавливается параллельно с предыдущими версиями, поэтому обе версии остаются доступными для использования. Но если у вас установлена *предварительная версия* SSMS 18.x, перед установкой SSMS 18.8 ее необходимо удалить. Чтобы узнать, установлена ли у вас предварительная версия, перейдите в окно **Справка > О программе**.

Если на компьютере есть несколько параллельных установок SSMS, всегда проверяйте, правильную ли версию вы запускаете. Последняя версия называется **Microsoft SQL Server Management Studio 18**.

> [!Note]
> Если вы открываете локализованную версию этой страницы и хотите просмотреть актуальные материалы, посетите эту страницу на [версии сайта на языке US-English](). С версии сайта US-English вы можете скачать SSMS на других [языках из числа доступных](#available-languages).

## <a name="available-languages"></a>Доступные языки

Этот выпуск SSMS можно установить на следующих языках.

SQL Server Management Studio 18.8:  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40a)

> [!NOTE]
> Модуль SQL Server PowerShell устанавливается отдельно из коллекции PowerShell. Дополнительные сведения см. в статье [Загрузка модуля PowerShell (SQL Server)](../powershell/download-sql-server-ps-module.md).

## <a name="whats-new"></a>Новые возможности

Дополнительные сведения о новых возможностях в этом выпуске см. в [заметках о выпуске SSMS](release-notes-ssms.md).

В этом выпуске есть несколько [известных проблем](release-notes-ssms.md#known-issues-188).

## <a name="previous-versions"></a>Предыдущие версии

Эта статья применима только к последней версии SSMS. Предыдущие версии SSMS можно скачать на [этой странице](../ssms/release-notes-ssms.md#previous-ssms-releases).

[!INCLUDE[ssms-connect-azure-ad](../includes/ssms-connect-azure-ad.md)]

## <a name="unattended-install"></a>Автоматическая установка

Кроме того, SSMS можно установить с помощью сценария командной строки.

Если вы хотите установить SSMS в фоновом режиме без запросов графического пользовательского интерфейса, выполните следующие действия.

1. Откройте командную строку с повышенными привилегиями.

2. Введите в командной строке следующую команду.

    ```console
    start "" /w <path where SSMS-ENU.exe file is located> /Quiet SSMSInstallRoot=<path where you want to install SSMS>
    ```

    Пример

    ```console
    start "" /w %systemdrive%\SSMSfrom\SSMS-Setup-ENU.exe /Quiet SSMSInstallRoot=%systemdrive%\SSMSto
    ```

    Можно также передать */Passive* вместо */Quiet*, чтобы увидеть пользовательский интерфейс программы установки.

3. Если все работает правильно, в этом примере можно будет увидеть, что среда SSMS установлена по пути %systemdrive%\SSMSto\Common7\IDE\Ssms.exe. Если что-то пошло не так, можно проверить возвращенный код ошибки и взглянуть на файл журнала в %TEMP%\SSMSSetup.

## <a name="installation-with-azure-data-studio"></a>Установка с Azure Data Studio

- Начиная с SSMS 18.7, вместе с SSMS устанавливается системная версия Azure Data Studio по умолчанию.  Если на рабочей станции уже установлена аналогичная или более поздняя системная версия Azure Data Studio (стабильная или для предварительной оценки), то при сравнении с включенной в состав SSMS версией Azure Data Studio установка Azure Data Studio в рамках SSMS пропускается. Сведения о версии Azure Data Studio можно найти в заметках о выпуске.
- Установщику системной версии Azure Data Studio требуются те же права безопасности, что и установщику SSMS.
- Установка Azure Data Studio выполняется с параметрами установки Azure Data Studio по умолчанию. Это необходимо для создания папки меню "Пуск" и добавления Azure Data Studio в путь к ней. Ярлык на рабочем столе не создается. Azure Data Studio не регистрируется как редактор по умолчанию для каких-либо типов файлов.
- Локализация Azure Data Studio осуществляется с помощью расширений языкового пакета. Чтобы локализовать Azure Data Studio, скачайте соответствующий языковой пакет из [marketplace-магазина расширений](../azure-data-studio/extensions/add-extensions.md).
- Сейчас установку Azure Data Studio можно пропустить, запустив установщик SSMS с флагом командной строки `DoNotInstallAzureDataStudio=1`.

## <a name="uninstall"></a>Удаление

Набор SSMS имеет общие компоненты с другими решениями, которые останутся установленными после его удаления.

Сюда входят следующие компоненты:

- Azure Data Studio
- Microsoft .NET Framework 4.7.2
- Драйвер Microsoft OLE DB для SQL Server
- Microsoft ODBC Driver for SQL Server версии 17
- Распространяемый компонент Microsoft Visual C++ 2013 (x86)
- Распространяемый компонент Microsoft Visual C++ 2017 (x86)
- Распространяемый компонент Microsoft Visual C++ 2017 (x64)
- Microsoft Visual Studio Tools for Applications 2017

Эти компоненты не удаляются, так как могут являться общими с другими продуктами. Их удаление может нарушить работу этих продуктов.

## <a name="supported-sql-offerings"></a>Поддерживаемые предложения SQL

- Эта версия SSMS работает со всеми [поддерживаемыми версиями SQL Server 2008–[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]](https://support.microsoft.com/lifecycle?C2=1044) и предоставляет превосходную поддержку новейших облачных функций Базы данных SQL Azure и Azure Synapse Analytics.
- Кроме того, SSMS 18.x можно установить одновременно с SSMS 17.x, SSMS 16.x или SQL Server 2014 и более ранними версиями.
- Службы SQL Server Integration Services (SSIS) — среда SSMS версии 17.x и более поздней не поддерживает подключение к устаревшим службам SQL Server Integration Services. Для подключения к более ранней версии служб Integration Services используйте соответствующую версию SSMS. Например, используйте SSMS 16.x для подключения к службам SQL Server 2016 Integration Services. Версии SSMS 17.x и SSMS 16.x можно установить параллельно на одном компьютере. Начиная с выпуска SQL Server 2012 база данных каталога SSIS (SSISDB) является рекомендуемым средством для хранения, выполнения и мониторинга пакетов служб Integration Services, а также управления ими. Дополнительные сведения см. в разделе [Каталог служб SSIS](../integration-services/catalog/ssis-catalog.md).

## <a name="ssms-system-requirements"></a>Требования к системе для SSMS

При использовании последнего пакета обновления текущий выпуск SSMS поддерживает следующие 64-разрядные платформы:

Поддерживаемые операционные системы

- Windows 10 (64-разрядная) как минимум версии 1607 (10.0.14393)
- Windows 8.1 (64-разрядная)
- Windows Server 2019 (64-разрядная версия)
- Windows Server 2016 (64-разрядная версия)
- Windows Server 2012 R2 (64-разрядная версия)
- Windows Server 2012 (64-разрядная версия)
- Windows Server 2008 R2 (64-разрядная версия)

Поддерживаемое оборудование:

- Процессор x86 (Intel, AMD) с частотой как минимум 1,8 ГГц. Рекомендуется использовать как минимум двухъядерный процессор.
- 2 ГБ ОЗУ; рекомендуется 4 ГБ ОЗУ (минимум 2,5 ГБ при выполнении на виртуальной машине)
- Доступное место на жестком диске объемом от 2 до 10 ГБ

> [!NOTE]
> Решение SSMS доступно только как 32-разрядное приложение для Windows. Если вам нужно средство для операционных систем, отличных от Windows, рекомендуем использовать Azure Data Studio. Azure Data Studio — это кроссплатформенное средство для macOS, Linux и Windows. Дополнительные сведения см. в разделе [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

## <a name="next-steps"></a>Дальнейшие действия

- [Средства SQL](../tools/overview-sql-tools.md)
- [Документация по SQL Server Management Studio](sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
- [Скачать SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Последние обновления](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)
- [Руководство по архитектуре данных Azure](/azure/architecture/data-guide/)
- [Блог по SQL Server](https://cloudblogs.microsoft.com/sqlserver/)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]