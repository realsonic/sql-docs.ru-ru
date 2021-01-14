---
title: Новые возможности служб Reporting Services | Документация Майкрософт
description: Сведения о новых возможностях различных версий SQL Server Reporting Services, включая изменения в основных функциональных областях.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 12/05/2019
ms.openlocfilehash: eaef4f651b65b2097aa4cfe0f41c97442469f193
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171446"
---
# <a name="whats-new-in-sql-server-reporting-services-ssrs"></a>Новые возможности служб SQL Server Reporting Services (SSRS)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)]

Ознакомьтесь со сведениями о новых возможностях в различных версиях SQL Server [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Эта статья охватывает основные функциональные аспекты и обновляется по мере выпуска новых элементов.

Сведения о сервере отчетов Power BI см. в статье [Что такое сервер отчетов Power BI?](https://docs.microsoft.com/power-bi/report-server/get-started)

::: moniker range=">=sql-server-ver15"

## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

**Скачать** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

Службы [SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122) можно скачать из Центра загрузки Майкрософт.

### <a name="azure-sql-managed-instance-support"></a>Поддержка Управляемого экземпляра SQL Azure

Теперь вы можете разместить каталог базы данных, используемый для служб SQL Server Reporting Services (SSRS), в Управляемом экземпляре SQL Azure, размещенном либо на виртуальной машине, либо в вашем центре обработки данных. Поддержка предоставляется только для использования учетных данных базы данных для подключения к Управляемому экземпляру SQL.

### <a name="power-bi-premium-dataset-support"></a>Поддержка набора данных Power BI Premium

Вы можете подключиться к наборам данных Power BI с помощью построителя отчетов (Майкрософт) или средств SQL Server Data Tools (SSDT). Затем эти отчеты можно опубликовать в SSRS 2019, установив подключение к SQL Server Analysis Services. Чтобы включить сценарий, пользователи должны использовать сохраненные имя пользователя и пароль Windows.

### <a name="alttext-alternative-text-support-for-report-elements"></a>Поддержка AltText (альтернативный текст) для элементов отчета

При создании отчетов вы можете использовать всплывающие подсказки, чтобы указать текст для каждого элемента отчета. Технология чтения с экрана правильно определяет эти подсказки.

### <a name="azure-active-directory-application-proxy-support"></a>Поддержка Azure Active Directory Application Proxy

Благодаря Azure Active Directory Application Proxy вам больше не нужно управлять собственной прокси-службой веб-приложения, чтобы обеспечить безопасный доступ через мобильные или веб-приложения.

### <a name="custom-headers"></a>Настраиваемые заголовки

Устанавливает значения заголовков для всех URL-адресов, соответствующих указанному шаблону регулярных выражений. Пользователи могут обновить значение пользовательского заголовка с допустимым XML, чтобы установить значения заголовка для выбранных URL-адресов запроса. Администраторы могут добавлять любое количество заголовков в XML. Дополнительные сведения см. в [этом разделе](tools/server-properties-advanced-page-reporting-services.md#customheaders) статьи **Server Properties Advanced Page - Power BI Report Server & Reporting Services** (Расширенная страница свойств сервера — Сервер отчетов Power BI и службы Reporting Services).

### <a name="transparent-database-encryption"></a>Прозрачное шифрование данных

SQL Server 2019 теперь поддерживает прозрачное шифрование данных для базы данных каталога SSRS в выпусках "Корпоративный" и "Стандартный". 

### <a name="microsoft-report-builder-update"></a>Обновление построителя отчетов (Майкрософт)

Недавно выпущенная версия построителя отчетов полностью совместима с версиями служб Reporting Services 2016, 2017 и 2019 годов. Она также совместима со всеми выпущенными и поддерживаемыми версиями сервера отчетов Power BI.

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="sql-server-2017-reporting-services"></a>Службы SQL Server 2017 Reporting Services

**Скачать** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

Чтобы скачать службы SQL Server 2017 Reporting Services, перейдите в **[Центр загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=55252)** .

### <a name="comments-on-reports"></a>Комментарии к отчетам

В отчетах теперь доступны комментарии, с помощью которых можно оставлять сообщения и взаимодействовать с другими пользователями. Для комментариев также доступны вложения.

![Комментарии на сервере отчетов](media/what-s-new-in-sql-server-reporting-services-ssrs/report-server-comments.png)

Дополнительные сведения см. в разделе [Добавление комментариев к отчету на сервере отчетов](https://powerbi.microsoft.com/documentation/reportserver-add-comments/).

### <a name="dax-queries-in-reporting-tools"></a>Запросы DAX в средствах создания отчетов

В последних выпусках построителя отчетов и SQL Server Data Tools вы можете создавать собственные запросы DAX для табличных моделей данных SQL Server Analysis Services. Вы можете перетаскивать поля в конструкторах запросов. См. [блог по Reporting Services](https://blogs.msdn.microsoft.com/sqlrsteamblog/2017/03/09/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools/).

### <a name="rest-api-support"></a>Поддержка REST API

В целях разработки и настройки современных приложений службы SQL Server Reporting Services теперь поддерживают интерфейс API RESTful, полностью совместимый с OpenAPI. Полная спецификация и документация по API теперь находится на сайте [SwaggerHub](https://app.swaggerhub.com/apis/microsoft-rs/SSRS/2.0).

### <a name="query-designer-support-for-dax-now-in-report-builder-and-sql-server-data-tools"></a>Поддержка конструктора запросов для DAX в построителе отчетов и SQL Server Data Tools

В построителе отчетов и SQL Server Data Tools вы можете создавать собственные запросы DAX для поддерживаемых табличных моделей данных SQL Server Analysis Services. Для перетаскивания нужных полей можно использовать конструктор запросов в обоих средствах. После этого будет создан запрос DAX.

Подробнее см. в [блоге по службам Reporting Services](https://blogs.msdn.microsoft.com/sqlrsteamblog/2017/03/09/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools/).

* Скачайте [построитель отчетов SQL Server](https://go.microsoft.com/fwlink/?LinkId=734968).
* Скачайте [SQL Server Data Tools — релиз-кандидат](https://docs.microsoft.com/sql/ssdt/sql-server-data-tools-ssdt-release-candidate).

> [!NOTE]
> Конструктор запросов для DAX можно использовать только с табличными источниками данных SSAS, встроенными в SQL Server 2016 и более поздних версий.
::: moniker-end

## <a name="ssrs-2016"></a>SSRS 2016

### <a name="reporting-services-ssrswebportal-non-markdown"></a>Службы Reporting Services: [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]  

Доступен новый [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]. Обновленный веб-портал включает следующие данные:
- Ключевые показатели эффективности
- Мобильные отчеты
- Отчеты с разбиением на страницы
- файлы Excel
- файлы Power BI Desktop.

[!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] заменяет диспетчер отчетов, который использовался в предыдущих версиях.

Для создания мобильных отчетов вам потребуется [!INCLUDE[SS_MobileReptPub_Short](../includes/ss-mobilereptpub-short.md)].

Дополнительные сведения о [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]см. в статье [Веб-портал (основной режим служб SSRS)](../reporting-services/web-portal-ssrs-native-mode.md).  

![Снимок экрана, показывающий портал SQL Server Reporting Services.](../reporting-services/media/ssrsportal.png "ssRSPortal")  

#### <a name="custom-branding-for-the-ssrswebportal-non-markdown"></a>Пользовательская фирменная символика на веб-портале [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

С помощью пакета настройки фирменной символики можно настроить [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] , задав логотип и цвета организации.  

Дополнительные сведения об индивидуальной фирменной символике см. в разделе [Фирменная символика на веб-портале](https://msdn.microsoft.com/6dac97f7-02a6-4711-81a3-e850a6b40bf1).

#### <a name="key-performance-indicators-kpi-in-the-ssrswebportal-non-markdown"></a>Ключевые показатели эффективности (KPI) на веб-портале [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

[!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] позволяет создать ключевые показатели эффективности (KPI), соответствующие текущей папке. При создании KPI вы можете выбрать поля наборов данных и суммировать их значения. Кроме того, вы можете выбрать содержимое для предоставления дополнительной детализации.

![Снимок экрана, показывающий ключевые показатели эффективности на портале SQL Server Reporting Services.](../reporting-services/media/ssrs-webportal-kpi.png)

Дополнительные сведения см. в разделе [Работа с ключевыми показателями эффективности на веб-портале](https://msdn.microsoft.com/a28cf500-6d47-4268-a248-04837e7a09eb).

### <a name="mobile-reports"></a>Мобильные отчеты

Мобильные отчеты в службах Reporting Services представляют собой специализированные отчеты, оптимизированные для широкого ряда форм-факторов и обеспечивающие оптимальную работу с данными на мобильных устройствах. Мобильные отчеты содержат различные визуализации — от диаграмм времени, категорий и сравнения до древовидных и пользовательских карт. Подключите мобильные отчеты к различным источникам данных, включая локальные многомерные и табличные данные служб SQL Server Analysis Services. Поля для мобильных отчетов можно поместить в рабочую область конструктора с настраиваемыми строками и столбцами сетки. Гибкие элементы мобильных отчетов автоматически масштабируются в зависимости от размера экрана. Мобильные отчеты можно сохранять на сервере службы отчетов, а затем просматривать в браузере или мобильном приложении Power BI. Поддерживаются такие устройства:

- iPad
- iPhone
- телефоны под управлением Android;
- любые устройства с Windows 10.

#### <a name="mobile-report-publisher"></a>Издатель мобильных отчетов  

[!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long.md)] позволяет создавать и публиковать мобильные отчеты SQL Server на [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] служб [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)].  

![SS_MRP_LayoutTabSmall](../reporting-services/media/ss-mrp-layouttabsm.png "SS_MRP_LayoutTabSmall")  

Дополнительные сведения см. в разделе [Создание мобильных отчетов с помощью издателя мобильных отчетов SQL Server](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md).  

#### <a name="sql-server-mobile-reports-hosted-in-reporting-services-available-in-power-bi-mobile-app"></a>Мобильные отчеты SQL Server, размещаемые в службах Reporting Services, доступны в приложении Power BI Mobile  

Приложение Power BI Mobile для iOS на iPad и iPhone сейчас может отображать мобильные отчеты SQL Server, которые находятся на вашем локальном сервере отчетов.  

![SS_MRP_iPad_HomeSm](../reporting-services/media/ss-mrp-ipad-homesm.png "SS_MRP_iPad_HomeSm")  

По умолчанию без внесения изменений в конфигурацию установить соединение будет невозможно. Дополнительные сведения о том, как разрешить приложению Power BI Mobile устанавливать соединение с сервером отчетов, см. в разделе [Enable a report server for Power BI Mobile access](../reporting-services/report-server/enable-a-report-server-for-power-bi-mobile-access.md).

### <a name="support-of-sharepoint-mode-and-sharepoint-2016"></a>Поддержка режима SharePoint и SharePoint 2016  

Службы [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] поддерживают интеграцию с SharePoint 2013 и SharePoint 2016.

Дополнительные сведения см. в разделе:  

- [Поддерживаемые сочетания SharePoint, компонентов служб Reporting Services и надстроек (SQL Server 2016)](../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md)  

- [Где найти надстройку службы Reporting Services для продуктов SharePoint](../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  

- [Установка режима интеграции с SharePoint для служб Reporting Services](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  

### <a name="microsoft-net-framework-4-support"></a>Поддержка платформы Microsoft .NET Framework 4  

[!INCLUDE[ssRSCurrent-md](../includes/ssrscurrent-md.md)] поддерживают текущие версии Microsoft .NET Framework 4, включая 4.0 и 4.5.1. Если ни одна из версий .Net Framework 4.x не установлена, программа [!INCLUDE[ssNoVersion-md](../includes/ssnoversion-md.md)] установит .NET 4.0 на этапе установки компонентов.  

### <a name="report-improvements"></a>Усовершенствования в отчетах

**Механизм визуализации HTML 5**: новый механизм визуализации HTML5, ориентированный на современные "полнофункциональные" веб-стандарты и современные браузеры.  Новый механизм визуализации больше не зависит от режима Quirks, используемого в некоторых старых браузерах.

Дополнительные сведения о поддержке браузеров см. в разделе [Поддержка браузера для служб Reporting Services и Power View](../reporting-services/browser-support-for-reporting-services-and-power-view.md).  

**Современные отчеты с разбиением на страницы**: новые, современные стили для диаграмм, датчиков, карт и других средств визуального представления данных позволяют создавать современные удобные отчеты с разбивкой на страницы.

**Диаграммы типа "дерево" и "солнечные лучи"** : улучшайте отчеты с помощью диаграмм "дерево" ![ssrs_treemap_icon](../reporting-services/media/ssrs-treemap-icon.png "ssrs_treemap_icon") и "солнечные лучи" ![ssrs_sunburst_icon](../reporting-services/media/ssrs-sunburst-icon.png "ssrs_sunburst_icon") для удобной визуализации иерархических данных. Дополнительные сведения см. в разделе [Tree Map and Sunburst Charts in Reporting Services](../reporting-services/report-design/tree-map-and-sunburst-charts-in-reporting-services.md).  

**Внедрение отчетов**: теперь с помощью iframe, а также параметров URL-адреса мобильные отчеты и отчеты с разбиением по страницам можно внедрять в другие веб-страницы и приложения.  

**Закрепление элементов отчета на панели мониторинга Power BI** : при просмотре отчета в [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]можно выбрать элементы отчета и закрепить их на панели мониторинга [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .   Закреплять можно такие элементы, как диаграммы, панели датчиков, карты и изображения. Вы можете:

1. Выберите группу, содержащую панель мониторинга, на которой нужно закрепить элемент.
2. Выберите панель мониторинга, на которой требуется закрепить элемент.
3. Выберите частоту обновления плитки на этой панели мониторинга.

![Примечание.](https://docs.microsoft.com/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "Примечание") Обновлением управляют подписки служб [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], а после закрепления элемента можно изменить подписку и настроить другое расписание обновления.

![Снимок экрана: диалоговое окно «Закрепить в Power BI панели мониторинга».](../reporting-services/media/ssrs-pin-to-powerbi.png) 

Дополнительные сведения см. в разделах [Интеграция Сервера отчетов Power BI (диспетчер конфигурации)](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md) и [Закрепление элементов служб Reporting Services на информационных панелях Power BI](../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md).  

**Визуализация в PowerPoint и экспорт**: новый модуль подготовки отчетов [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] имеет формат Microsoft PowerPoint (PPTX). Экспортировать отчеты в формате PPTX можно из обычных приложений, построителя отчетов, конструктора отчетов (в SSDT) и [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. Например, на следующем рисунке показано меню экспорта из [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. 

![Снимок экрана, показывающий раскрывающийся список «Экспорт» с вызванным параметром PowerPoint.](../reporting-services/media/ssrs-export-powerpoint.png) 

Кроме того, формат PPTX можно выбрать для вывода подписок и использовать доступ по URL-адресу сервера отчетов для отрисовки и экспорта отчета. Например, следующая URL-команда в браузере экспортирует отчет из именованного экземпляра сервера отчетов.  

```https
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  

Дополнительные сведения см. в разделе [Export a Report Using URL Access](../reporting-services/export-a-report-using-url-access.md).

**PDF заменяет ActiveX для удаленной печати**: теперь на панели инструментов в средстве просмотра отчетов для печати используется функция PDF, а не элементы ActiveX. Большинство современных браузеров, включая Microsoft Edge, поддерживают новое средство просмотра отчетов. Никаких дополнительных элементов управления ActiveX загружать не нужно! В зависимости от используемого браузера, а также установленных приложений и служб для просмотра PDF, [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] либо будет открывать диалоговое окно для печати отчета, либо предлагать скачать PDF-файл. Имея права администратора, вы можете отключить функцию печати на стороне клиента в [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].

Дополнительные сведения см. в разделе [Включение и отключение печати на стороне клиента для служб Reporting Services](../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md).

![Снимок экрана с диалоговым окном «Печать».](../reporting-services/media/ssrs-pdf-printing.png)

### <a name="subscription-improvements"></a>Усовершенствования подписки  

|Компонент|Поддерживаемый режим сервера|  
|-------------|---------------------------|  
|**Включение и отключение подписок**. Новые параметры пользовательского интерфейса для быстрого отключения и включения подписок. Отключенные подписки сохраняют другие свойства конфигурации, такие как расписание, и их можно легко включить.<br /><br /> ![Снимок экрана, показывающий параметры включения, отключения и удаления.](../reporting-services/media/ssrs-enable-disable-subscriptions.png)<br /><br /> Дополнительные сведения см. в разделе [Disable or Pause Report and Subscription Processing](../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md).|Основной режим|  
|**Описание подписки**. При создании новой подписки в свойства подписки теперь можно включить описание отчета. Описание добавляется на странице сводки о подписке.|SharePoint и собственный режим|  
|**Изменение владельца подписки**. Улучшенный пользовательский интерфейс для быстрого изменения владельца подписки. Предыдущие версии [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] позволяют администраторам изменять владельцев подписки с помощью скрипта. Начиная с версии [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] , владельца подписки можно изменять с помощью пользовательского интерфейса или скрипта. Изменение владельца подписки — это стандартная задача администрирования, выполняемая в случае увольнения пользователей или изменения их роли в организации.|SharePoint и собственный режим|  
|**Общие учетные данные для подписок на общую папку**. Благодаря подпискам на общую папку [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] сейчас существует два рабочих процесса.<br /><br /> Новая возможность в этой версии: [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] позволяет администратору настроить одну учетную запись для общей папки, которую можно использовать для нескольких подписок. Настройка учетной записи для общей папки выполняется в диспетчере конфигурации основного режима, параметры которого считывает [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]: **Укажите учетную запись общей папки**. На странице конфигурации подписки пользователям нужно выбрать **Использовать учетную запись общей папки**.<br /><br /> Вы можете настроить отдельные подписки с разными учетными данными для целевой общей папки.<br /><br /> Кроме того, два этих подхода можно использовать совместно. Например, когда какие-то подписки на общую папку используют централизованную учетную запись общей папки, а другие подписки используют разные учетные данные.|Основной режим|

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)

Новая версия SSDT включает в себя шаблоны проектов для [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)]: мастер проектов сервера отчетов и проект сервера отчетов. Сведения о загрузке SSDT см. в разделе [SQL Server Data Tools для Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=827542).  

### <a name="report-builder-improvements"></a>Усовершенствования построителя отчетов

**Новый пользовательский интерфейс построителя отчетов**: пользовательский интерфейс в [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] теперь имеет современный вид и содержит упрощенные элементы интерфейса.  

|Создать|Previous|  
|-|-|  
|![ssrs_rbfacelift_new](../reporting-services/media/ssrs-rbfacelift-new.png "ssrs_rbfacelift_new")|![ssrs_rbfacelift_old](../reporting-services/media/ssrs-rbfacelift-old.png "ssrs_rbfacelift_old")|  

**Панель пользовательских параметров**: сейчас можно настраивать панель параметров. С помощью области конструктора в построителе отчетов параметр можно перетащить в конкретный столбец и строку на панели параметров. Для изменения макета панели столбцы можно добавлять и удалять. Дополнительные сведения см. в разделах [Настройка области параметров в отчете (построитель отчетов)](../reporting-services/report-design/customize-the-parameters-pane-in-a-report-report-builder.md).  

![Список параметров в области данных отчета и на панели параметров](../reporting-services/media/ssrs-customizeparameter-parameterlist-reportdatapane.png "Список параметров в области данных отчета и на панели параметров")  

**Поддержка высокого разрешения**: [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] поддерживает масштабирование с высоким разрешением (DPI; точек на дюйм) и соответствующие устройства.  Дополнительные сведения о высоком разрешении см. в следующих разделах:  

- [Windows 8.1 DPI Scaling Enhancements](https://blogs.windows.com/windowsexperience/2013/07/15/windows-8-1-dpi-scaling-enhancements/)  

- [Высокое разрешение и Windows 8.1](https://technet.microsoft.com/library/dn528848.aspx)  

## <a name="next-steps"></a>Дальнейшие действия

[Новые возможности в службах Analysis Services](https://msdn.microsoft.com/aa69c299-b8f4-4969-86d8-b3292fe13f08)  
[Обратная совместимость](reporting-services-backward-compatibility.md)  
[Возможности служб Reporting Services, поддерживаемые различными выпусками SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)  
[Обновление и перенос служб Reporting Services](../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
[службы Reporting Services](../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
