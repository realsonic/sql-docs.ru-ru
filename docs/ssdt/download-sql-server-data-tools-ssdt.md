---
title: Скачать SQL Server Data Tools (SSDT)
description: Подробнее о средствах SQL Server Data Tools (SSDT). Узнайте, как установить этот набор средств разработки для баз данных с Visual Studio 2019 и Visual Studio 2017.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
keywords: установка SSDT, скачивание SSDT, последняя версия SSDT
ms.assetid: b0fc4987-d260-4d0a-9dd1-98099835b361
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 02/20/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 8913e553460c9fae3d008783d571aca81b5c21a4
ms.sourcegitcommit: 44eebb659f9b226c08aea6c31a909b22ad4fec60
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860574"
---
# <a name="download-sql-server-data-tools-ssdt-for-visual-studio"></a>Скачать SQL Server Data Tools (SSDT) для Visual Studio

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

**SQL Server Data Tools (SSDT)**  — это современное средство разработки, позволяющее создавать реляционные базы данных SQL Server, базы данных SQL в Azure, модели данных Analysis Services (AS), пакеты Integration Services (IS) и отчеты Reporting Services (RS). С помощью SSDT вы можете проектировать и развертывать любые типы содержимого SQL Server так же просто, как разрабатывать приложения в Visual Studio.

## <a name="ssdt-for-visual-studio-2019"></a>SSDT для Visual Studio 2019

### <a name="changes-in-ssdt-for-visual-studio-2019"></a>Изменения в SSDT для Visual Studio 2019

Основные функции SSDT для создания проектов баз данных остались неотъемлемой частью Visual Studio.

С выходом Visual Studio 2019 требуемые функции для поддержки Analysis Services, Integration Services и Reporting Services перемещены в соответствующие расширения Visual Studio (VSIX) и доступны только с помощью этих расширений.

> [!NOTE]
> Автономного установщика SSDT для Visual Studio 2019 не существует.

### <a name="install-ssdt-with-visual-studio-2019"></a>Установка SSDT с Visual Studio 2019

Если среда [Visual Studio 2019](/visualstudio/install/install-visual-studio?preserve-view=true&view=vs-2019) уже установлена, вы можете изменить список рабочих нагрузок, включив в него SSDT. Если вы еще не установили Visual Studio 2019, вы можете скачать и использовать [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/).

Чтобы изменить установленные рабочие нагрузки Visual Studio для включения SSDT, используйте установщик Visual Studio Installer.

1. Запустите установщик Visual Studio Installer. В меню "Пуск" Windows можно выполнить поиск по слову "installer".

   ![Установщик Visual Studio Installer в меню "Пуск" Windows для 2019](../ssdt/media/visual-studio-installer.png)

2. В установщике выберите выпуск Visual Studio, в который необходимо добавить SSDT, а затем выберите **Изменить**.

3. Выберите **SQL Server Data Tools** в разделе **Хранение и обработка данных** в списке рабочих нагрузок.

   ![Рабочая нагрузка "Хранение и обработка данных" 2019](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2019.png)

Для проектов Analysis Services, Integration Services или Reporting Services установите соответствующие [расширения](/visualstudio/ide/finding-and-using-visual-studio-extensions) в Visual Studio в разделе **Расширения** > **Управление расширениями** или с помощью [Marketplace](https://marketplace.visualstudio.com/search?term=services&target=VS&category=All%20categories&vsVersion=&sortBy=Relevance).

* [Службы Analysis Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftAnalysisServicesModelingProjects)
* [Службы интеграции](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [службы Reporting Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftReportProjectsforVisualStudio)

## <a name="ssdt-for-visual-studio-2017"></a>SSDT для Visual Studio 2017

### <a name="changes-in-ssdt-for-visual-studio-2017"></a>Изменения в SSDT для Visual Studio 2017

Начиная с Visual Studio 2017, функции создания проектов баз данных входят в пакет установки Visual Studio. Для использования основных возможностей SSDT устанавливать автономный установщик SSDT не нужно.

Однако для создания проектов Analysis Services, Integration Services и Reporting Services по-прежнему требуется автономный установщик SSDT.

### <a name="install-ssdt-with-visual-studio-2017"></a>Установка SSDT с Visual Studio 2017

Чтобы установить SSDT во время [установки Visual Studio](/visualstudio/install/install-visual-studio), выберите рабочую нагрузку **Хранение и обработка данных**, а затем выберите **SQL Server Data Tools**.

Если Visual Studio уже установлена, используйте установщик Visual Studio Installer, чтобы изменить установленные рабочие нагрузки для включения SSDT.

1. Запустите установщик Visual Studio Installer. В меню "Пуск" Windows можно выполнить поиск по слову "установщик".

   ![Установщик Visual Studio Installer в меню "Пуск" Windows для 2017](../ssdt/media/visual-studio-installer.png)

2. В установщике выберите выпуск Visual Studio, в который необходимо добавить SSDT, а затем выберите **Изменить**.

3. Выберите **SQL Server Data Tools** в разделе **Хранение и обработка данных** в списке рабочих нагрузок.

   ![Рабочая нагрузка "Хранение и обработка данных" 2017](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2017.png)

### <a name="install-analysis-services-integration-services-and-reporting-services-tools"></a>Установка средств Analysis Services, Integration Services и Reporting Services

Чтобы обеспечить поддержку для проектов Analysis Services, Integration Services и Reporting Services, запустите [автономный установщик SSDT](#ssdt-for-vs-2017-standalone-installer).

Установщик выводит список доступных экземпляров Visual Studio, в которые будут добавлены средства SSDT. Если среда Visual Studio не установлена, то при выборе параметра **Установить новый экземпляр SQL Server Data Tools** будут установлены средства SSDT с минимальной требуемой версией Visual Studio. Однако для оптимальной работы рекомендуется использовать SSDT с [последней версией Visual Studio](https://www.visualstudio.com/downloads).

![Выбор AS, IS, RS](../ssdt/media/download-sql-server-data-tools-ssdt/select-services.png)

## <a name="ssdt-for-vs-2017-standalone-installer"></a>SSDT для VS 2017 (автономный установщик)

:::image type="icon" source="media/download.png" border="false"::: **[Скачать SSDT для Visual Studio 2017 (15.9.7)](https://go.microsoft.com/fwlink/?linkid=2151759)**

> [!IMPORTANT]
> * Перед установкой SSDT для Visual Studio 2017 (15.9.7) удалите расширения *Проекты служб Analysis Services* и *Проекты Reporting Services*, если они установлены, а затем закройте все экземпляры Visual Studio. 
> * Источник Power Query для SQL Server 2017 удален из списка стандартных компонентов. Теперь мы объявили источник Power Query для SQL Server 2017 и 2019 как отдельный компонент, который можно скачать [здесь](https://www.microsoft.com/download/details.aspx?id=100619).
> * Чтобы спроектировать пакеты с помощью соединителей Oracle и Teradata, а также задать в качестве целевой версию SQL Server, выпущенную до SQL 2019, помимо [соединителя Microsoft Oracle для SQL 2019](https://www.microsoft.com/download/details.aspx?id=58228) и [соединителя Microsoft Teradata для SQL 2019](https://www.microsoft.com/download/details.aspx?id=100599) необходимо установить соответствующую версию соединителя Майкрософт для Oracle и Teradata от Attunity.
>    * [Соединитель версии 5.0 для Oracle и Teradata от Attunity, предназначенный для SQL Server 2017](https://www.microsoft.com/download/details.aspx?id=55179)
>    * [Соединитель версии 4.0 для Oracle и Teradata от Attunity, предназначенный для SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=52950)
>    * [Соединитель версии 3.0 для Oracle и Teradata от Attunity, предназначенный для SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=44582)
>    * [Соединитель версии 2.0 для Oracle и Teradata от Attunity, предназначенный для SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29283)

### <a name="release-notes"></a>Заметки о выпуске

Полный список изменений см. в [заметках о выпуске для SQL Server Data Tools (SSDT)](release-notes-ssdt.md).

### <a name="system-requirements"></a>Требования к системе

SSDT для Visual Studio 2017 имеет те же [требования к системе](/visualstudio/productinfo/vs2017-system-requirements-vs), что и Visual Studio.

### <a name="available-languages---ssdt-for-vs-2017"></a>Доступные языки — SSDT для VS 2017

Этот выпуск **SSDT для Visual Studio 2017** можно установить на следующих языках:

* [Китайский (упрощенный)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x804)
* [Китайский (традиционный)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x404)
* [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x409)
* [Французский](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40c)
* [Немецкий](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x407)
* [Итальянский](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x410)
* [Японский](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x411)
* [Корейский](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x412)
* [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x416)
* [Русский](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x419)
* [Испанский](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40a)

### <a name="considerations-and-limitations"></a>Рекомендации и ограничения

* Visual Studio Community нельзя установить в автономном режиме.

* Чтобы обновить SSDT, необходимо выполнить те же действия, что и при установке SSDT. Например, если вы добавили SSDT с помощью расширений VSIX, обновление SSDT необходимо выполнять так же с помощью расширений VSIX. Если вы устанавливали SSDT отдельно, необходимо выполнить обновление таким же образом.

## <a name="offline-install"></a>Автономная установка

Если ваш компьютер не подключен к Интернету, следуйте инструкциям из этого раздела, чтобы установить SSDT. Дополнительные сведения см. в разделе [Создание сетевого подключения в Visual Studio 2017](/visualstudio/install/create-a-network-installation-of-visual-studio).

**Подключившись к Интернету**, первым делом выполните следующие действия:

1. [Скачайте автономный установщик SSDT](#ssdt-for-vs-2017-standalone-installer).

2. [Скачайте vs_sql.exe](https://aka.ms/vs/15/release/vs_sql.exe).

3. Не отключаясь от Интернета, выполните одну из следующих команд, чтобы скачать все файлы, необходимые для автономной установки. Обязательно используйте параметр `--layout`, чтобы скачать сами файлы для автономной установки. Замените `<filepath>` на фактический путь макетов, чтобы сохранить файлы.
   1. Для установки определенного языка укажите языковой стандарт: `vs_sql.exe --layout c:\<filepath> --lang en-us` (размер версии с одним языком составляет примерно 1 ГБ).
   1. Для установки всех языков пропустите аргумент `--lang`: `vs_sql.exe --layout c:\<filepath>` (размер версии со всеми языками составляет порядка 3,9 ГБ).

Дальнейшие действия можно выполнить в **автономном режиме**.

1. Запустите `vs_setup.exe --NoWeb`, чтобы установить оболочку VS2017 Shell и SQL Server Data Project.

2. В папке макетов запустите `SSDT-Setup-ENU.exe /install` и выберите SSIS/SSRS/SSAS.
   а. Для автоматической установки выполните команду `SSDT-Setup-ENU.exe /INSTALLALL[:vsinstances] /passive`.

Для отображения доступных параметров запустите `SSDT-Setup-ENU.exe /help`

> [!NOTE]
> При использовании полной версии Visual Studio 2017 создайте автономную папку только для SSDT и запустите из нее `SSDT-Setup-ENU.exe` (не добавляйте SSDT в другой автономный макет Visual Studio 2017). При добавлении макета SSDT в существующий автономный макет Visual Studio необходимые компоненты среды выполнения (.exe) не создаются.

## <a name="supported-sql-versions"></a>Поддерживаемые версии SQL

|Шаблоны проектов|Поддерживаемые платформы SQL|
|-------------------|--------------------|
|реляционные базы данных| SQL Server 2005\* — SQL Server 2017<br> (используйте SSDT 17.x или SSDT for Visual Studio 2017 для подключения к [SQL Server на Linux](../linux/sql-server-linux-overview.md))<br /><br />База данных SQL Azure<br /><br />Azure Synapse Analytics (поддерживает только запросы, проекты базы данных пока не поддерживаются)<br /><br /> \* Версия SQL Server 2005 не поддерживается,<br /><br /> перейдите на поддерживаемую версию SQL.|
|Модели служб Analysis Services<br /><br />Reporting Services, отчеты служб | SQL Server 2008 — SQL Server 2017|
|пакеты служб Integration Services| SQL Server 2012 — SQL Server 2019 |

## <a name="dacfx"></a>DacFx

Средства SSDT для Visual Studio 2015 и 2017 используют DacFx 17.4.1: [Скачать Data-Tier Application Framework (DacFx) 17.4.1](https://www.microsoft.com/download/details.aspx?id=56508).

## <a name="previous-versions"></a>Предыдущие версии

Чтобы скачать и установить SSDT для Visual Studio 2015 или более старую версию SSDT, см. [Предыдущие выпуски SQL Server Data Tools (SSDT и SSDT-BI)](previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md).

## <a name="see-also"></a>См. также:

* [Форум MSDN по SSDT](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt) 

* [Блог группы разработчиков SSDT](/archive/blogs/ssdt/)

* [Справочник по API DACFx](/previous-versions/sql/sql-server-2014/dn645454(v=sql.120))

* [Скачивание SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)

## <a name="next-steps"></a>Дальнейшие действия

После установки SSDT ознакомьтесь со следующими руководствами, чтобы узнать, как создать базы данных, пакеты, модели данных и отчеты в SSDT.

* [Разработка базы данных вне сети с учетом проекта](project-oriented-offline-database-development.md)

* [Учебник по службам SSIS. Создание простого ETL-пакета](../integration-services/ssis-how-to-create-an-etl-package.md)

* [Руководства по службам Analysis Services](/analysis-services/analysis-services-tutorials-ssas)

* [Создание простого табличного отчета (учебник по службам SSRS)](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
