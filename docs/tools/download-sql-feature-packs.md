---
title: Скачивание расширенных компонентов и средств SQL Server
description: Краткое описание нескольких загружаемых и автономных пакетов, которые корпорация Майкрософт предоставляет для повышения ценности SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
keywords: Пакет дополнительных компонентов
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 12/15/2019
ms.openlocfilehash: e99362bd36d99aeeb2b44d3947454f198d29be7c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642455"
---
# <a name="download-sql-server-extended-features-and-tools"></a>Скачивание расширенных компонентов и средств SQL Server

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Краткое описание нескольких загружаемых и автономных пакетов, которые корпорация Майкрософт предоставляет для повышения ценности SQL Server.

## <a name="analysis-services"></a>Службы Analysis Services

| Компонент | Description |
|----|-----|
| [Клиентские библиотеки служб Analysis Services](https://go.microsoft.com/fwlink/?LinkID=853574) |Клиентские библиотеки служб Microsoft Analysis Services включают программные интерфейсы (API) для проверки подлинности и обмена запросами и ответами с Microsoft SQL Server Analysis Services 2005 или более поздней версии, Microsoft Azure Analysis Services и Microsoft Power BI.<br><br> Клиентские библиотеки Microsoft Analysis Services включают следующие пакеты установки. </br> ADOMD.NET для Microsoft Analysis Services </br> Поставщик OLE DB служб Microsoft Analysis Services (MSOLAP) </br> Объекты AMO (Майкрософт) |
| [NuGetAnalysisSrvs](https://www.nuget.org/profiles/NuGetAnalysisSrvs) | NuGet для Analysis Services |
|||

## <a name="azure"></a>Azure

| Компонент | Description |
|----|-----|
| [Резервное копирование SQL Server в Windows Azure Tool](https://go.microsoft.com/fwlink/?LinkID=391033) | Средство Microsoft SQL Server Backup to Windows Azure Tool позволяет выполнять резервное копирование в хранилище больших двоичных объектов Windows Azure и сжимает резервные копии SQL Server, хранящиеся локально или в облаке. |
|||

## <a name="command-line-programming-and-t-sql"></a>Командная строка, программирование и T-SQL

| Компонент | Description |
|----|-----|
| [Служебные программы командной строки для SQL Server](https://www.microsoft.com/download/details.aspx?id=52680) | Программа SQLCMD позволяет пользователям подключаться к экземплярам SQL Server, отправлять пакеты Transact-SQL, а также выводить информацию о наборах строк с этих экземпляров. |
| [Драйверы для PHP для SQL Server](https://aka.ms/downloadmsphpsql) | Драйверы Microsoft для PHP для SQL Server — это расширения для PHP, позволяющие считывать и записывать данные SQL Server из скриптов PHP. |
| [Драйвер JDBC для SQL Server](https://aka.ms/downloadmssqljdbc) | Драйвер Microsoft JDBC Driver для SQL Server предоставляет доступ к SQL Server из любого приложения Java, сервера приложений Java или приложения с поддержкой Java.|
| [Платформа приложения уровня данных SQL Server](https://www.microsoft.com/download/details.aspx?id=56508) | Платформа приложения уровня данных (DAC) SQL Server — это компонент на базе платформы .NET Framework, предоставляющий службы жизненного цикла приложений для разработки баз данных и управления ими. |
| [Семантическая статистика языка SQL Server](../relational-databases/search/install-and-configure-semantic-search.md) | База данных семантической статистики языка — это необходимый компонент для функции статистического семантического поиска в Microsoft SQL Server. |
| [Общие управляющие объекты SQL Server](../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) | Управляющие объекты SQL Server (SMO) представляют собой объектную модель платформы .NET Framework, позволяющую разработчикам программного обеспечения создавать клиентские приложения для управления объектами и службами SQL Server, а также для их администрирования. |
| [Системные типы CLR](https://go.microsoft.com/fwlink/?linkid=2108808) | Пакет SQL Server System CLR Types содержит компоненты, реализующие в SQL Server новые типы идентификаторов: geometry, geography и hierarchy. **Примечание. Для использования этого компонента также требуется [установщик Windows 4.5](https://go.microsoft.com/fwlink/?LinkId=123373)** . |
| [Расширения Windows PowerShell для Microsoft SQL Server](../database-engine/install-windows/install-sql-server-powershell.md) | В состав Расширений PowerShell для SQL Server входят поставщик и набор командлетов, которые позволяют администраторам и разработчикам создавать скрипты PowerShell для управления экземплярами SQL Server. |
|||

## <a name="database-engine"></a>Ядро СУБД

| Компонент | Description |
|----|-----|
| [Служебные программы командной строки для SQL Server](https://www.microsoft.com/download/details.aspx?id=52680) | Программа SQLCMD позволяет пользователям подключаться к экземплярам SQL Server, отправлять пакеты Transact-SQL, а также выводить информацию о наборах строк с этих экземпляров. |
| [Удаленное хранилище больших двоичных объектов](https://go.microsoft.com/fwlink/?linkid=2109005) | Удаленное хранилище больших двоичных объектов SQL Server — это способ хранения больших двоичных объектов, представляющих неструктурированные данные, в ассоциативном хранилище данных с адресацией по содержимому. Компонент состоит из клиентской библиотеки DLL, которая связана с клиентским приложением и набором хранимых процедур для установки на SQL Server. |
| [Помощник по обновлению SQL Server](../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md) | Помощник по обновлению анализирует экземпляры SQL Server при подготовке к обновлению до SQL Server. |
|||

## <a name="integration-services"></a>Службы Integration Services

| Компонент | Description |
|----|-----|
| [Пакет дополнительных компонентов Integration Services для Azure](../integration-services/azure-feature-pack-for-integration-services-ssis.md) | Пакет дополнительных компонентов Microsoft Integration Services для Azure включает средства, позволяющие использовать IS для подключения к Azure Stack. |
| [Пакет дополнительных компонентов служб Integration Services для SQL Server](https://www.microsoft.com/download/details.aspx?id=100303). | Эти автономные пакеты дополнительно повышают ценность последних версий служб SQL Server Integration Services. |
|||

## <a name="kerberos"></a>Kerberos

| Компонент | Description |
|----|-----|
| [Диспетчер конфигурации Kerberos для Microsoft SQL Server](https://www.microsoft.com/download/details.aspx?id=39046) | Проверка подлинности Kerberos ― это метод обеспечения высокой степени безопасности для проверки подлинности сущности клиента и сервера (субъектов безопасности) в сети. |
|||

## <a name="master-data-services"></a>Службы Master Data Services

| Компонент | Description |
|----|-----|
| [Надстройка Master Data Service для Microsoft Excel](https://go.microsoft.com/fwlink/?LinkID=390972) | Надстройка служб Master Data Services (MDS) для Microsoft Excel представляет собой средство управления с широкими возможностями, позволяющее легко и эффективно управлять основными данными. |
|||

## <a name="providers-and-drivers"></a>Поставщики и драйверы

| Компонент | Description |
|----|-----|
| [Драйверы ODBC для Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkID=852531) | Драйверы Microsoft ODBC Driver для SQL Server поддерживают возможность подключения из Windows и Unix к Microsoft SQL Server и Базе данных SQL Microsoft Azure. |
| [Поставщик OLE DB для DB2 в Microsoft SQL Server](/host-integration-server/db2oledbv/installing-data-provider-version-6-0) | Поставщик Microsoft OLE DB для DB2 v5.0 поддерживает набор технологий и средств для интеграции важных данных, хранящихся в базах данных IBM DB2, с новыми решениями. Разработчики и администраторы SQL Server могут использовать этот поставщик данных со службами Integration Services, Analysis Services, репликации, Reporting Services и обработчиком распределенных запросов. См. сведения об установке поставщика данных в электронной документации по продукту (доступно в Интернете для чтения и скачивания). |
|||

## <a name="reporting-services"></a>Службы Reporting Services

| Компонент | Description |
|----|-----|
| [построитель отчетов](https://www.microsoft.com/download/details.aspx?id=53613) | Построитель отчетов — это удобная среда разработки отчетов для ИТ-специалистов и опытных пользователей. Он поддерживает все возможности создания отчетов служб SQL Server Reporting Services. |
| [Надстройка Reporting Services для Microsoft SharePoint](../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)| Надстройка служб Reporting Services для технологий Microsoft SharePoint позволяет интегрировать возможности служб Reporting Services с инструментами SharePoint для совместной работы. |
| [Элемент управления Report Viewer для приложений ASP.NET Web Forms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/) | Этот элемент управления Report Viewer позволяет внедрять отчеты SQL Server Reporting Services с разбиением на страницы в приложения ASP.NET Web Forms. |
| [Элемент управления Report Viewer для приложений Windows Forms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/) | Этот элемент управления Report Viewer позволяет внедрять отчеты SQL Server Reporting Services с разбиением на страницы в приложения Windows Forms. |
|||

## <a name="sql-server-data-tools"></a>SQL Server Data Tools

| Компонент | Description |
|----|-----|
| [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)| SQL Server Data Tools — это современное средство разработки, позволяющее создавать реляционные базы данных SQL Server, базы данных SQL Azure, модели данных Analysis Services (AS), пакеты Integration Services (IS) и отчеты Reporting Services (RS). С помощью SSDT вы можете проектировать и развертывать любые типы содержимого SQL Server так же просто, как разрабатывать приложения в Visual Studio.|
|||

## <a name="see-also"></a>См. также раздел

- [Документация по SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [Дополнительные обновления и пакеты обновления](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)
- [Скачать SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
