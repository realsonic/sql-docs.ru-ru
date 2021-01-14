---
title: SQL Server Management Studio (SSMS)
description: Узнайте о пакете SQL Server Management Studio (SSMS) и его возможностях, в том числе об управлении решениями Analysis Services.
ms.prod: sql
ms.technology: ssms
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: ''
f1_keywords:
- sql13.ssms.viewhelp.f1
helpviewer_keywords:
- SQL Server Management Studio
- SQL Server Management Studio for Integration Services
- SQL Server Management Studio for Reporting Services
- SQL Server Management Studio for Analysis Services
ms.custom: seo-lt-2019
ms.date: 09/11/2019
ms.openlocfilehash: 1961d2f831a8db86c782a5e2330d74f4346d6d19
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096111"
---
# <a name="what-is-sql-server-management-studio-ssms"></a>Что такое SQL Server Management Studio (SSMS)?

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) — это интегрированная среда для управления любой инфраструктурой SQL. Используйте SSMS для доступа, настройки, администрирования и разработки всех компонентов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], Базы данных SQL Azure и Azure Synapse Analytics, а также управления ими. Среда SSMS предоставляет единую полнофункциональную служебную программу, которая сочетает в себе обширную группу графических инструментов с рядом отличных редакторов сценариев для доступа к службе [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] для разработчиков и администраторов баз данных всех профессиональных уровней.

- [**Скачивание SQL Server Management Studio (SSMS)**](download-sql-server-management-studio-ssms.md)
- [**Скачивание SQL Server Developer**](https://my.visualstudio.com/Downloads?q=SQL%20Server%20Developer)
- [**Скачивание Visual Studio**](https://www.visualstudio.com/downloads/)

![Снимок экрана SQL Server Management Studio.](media/sql-server-management-studio-ssms/ssms.png)

## <a name="sql-server-management-studio-components"></a>Компоненты среды SQL Server Management Studio  
  
|Description|Компонент|  
|---------------|---------|  
|**Обозреватель объектов** используется для просмотра всех объектов и управления ими в одном экземпляре [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)](или более).|[Обозреватель объектов](../ssms/object/object-explorer.md)|  
|**Обозреватель шаблонов** используется для создания файлов со стандартным текстом, которые можно использовать для ускорения разработки запросов и скриптов, и управления ими.|[Обозреватель шаблонов](../ssms/template/template-explorer.md)|  
|Устаревший **обозреватель решений** используется для создания проектов, применяемых для управления такими элементами администрирования, как скрипты и запросы.|[Обозреватель решений](../ssms/solution/solution-explorer.md)|  
|Использование инструментов для визуального проектирования, включенных в [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].|[Visual Database Tools](../ssms/visual-db-tools/visual-database-tools.md)|  
|Использование редакторов языков [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] для интерактивного написания и отладки запросов и скриптов.|[Редакторы запросов и текста](./f1-help/database-engine-query-editor-sql-server-management-studio.md)

## <a name="sql-server-management-studio-for-business-intelligence"></a>SQL Server Management Studio для решения задач бизнес-аналитики

Среда [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] предназначена для доступа к службам [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] и [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], а также для их настройки, администрирования и управления ими. Хотя все три технологии бизнес-аналитики полагаются на среду [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], административные задачи, связанные с каждой из этих технологий, несколько отличаются.

> [!NOTE]
> Чтобы создать и изменить решения [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]и [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , воспользуйтесь [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)], а не [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)] представляет собой среду разработки, основанную на [!INCLUDE[msCoName](../includes/msconame_md.md)][!INCLUDE[vsprvs](../includes/vsprvs-md.md)].

### <a name="managing-analysis-services-solutions-using-sql-server-management-studio"></a>Управление решениями Analysis Services в среде SQL Server Management Studio

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] позволяет управлять объектами служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , например выполнять их резервное копирование и обработку.

[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] позволяет создавать проекты скриптов служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , в которых выполняются разработка и сохранение скриптов с использованием многомерных выражений (MDX), расширений интеллектуального анализа данных (DMX) и XML для аналитики (XMLA). Проекты скриптов служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] используются для выполнения задач управления или повторного создания баз данных, кубов и других объектов в экземплярах служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] . Например, можно разработать скрипт XMLA в проекте скрипта служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , который создает объекты непосредственно в существующем экземпляре служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] . Проекты скриптов служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] могут быть сохранены в составе решения и интегрироваться с контролем исходного кода.
  
Дополнительные сведения об использовании среды [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]см. в разделе [Разработка и реализация с помощью среды SQL Server Management Studio](/analysis-services/instances/analysis-services-scripts-project-in-sql-server-management-studio).
  
### <a name="managing-integration-services-solutions-using-sql-server-management-studio"></a>Управление решениями Integration Services в среде SQL Server Management Studio

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] позволяет использовать службу [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] для управления пакетами и наблюдения за выполняющимися пакетами. В среде [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] можно организовать пакеты в папки, выполнять, импортировать и экспортировать пакеты, переносить пакеты служб DTS и обновлять пакеты служб [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .

### <a name="managing-reporting-services-projects-using-sql-server-management-studio"></a>Управление проектами служб Reporting Service в среде SQL Server Management Studio

Среда SQL Server Management Studio позволяет включать компоненты служб Reporting Services, администрировать серверы и базы данных, управлять ролями и заданиями.

Она реализует функции управления общими расписаниями (в папке «Общие расписания») и базами данных сервера отчетов (ReportServer, ReportServerTempdb). Можно также создать роль RSExecRole в системной базе данных Master, когда база данных сервера отчетов перемещается в новое или другое ядро СУБД SQL Server ([!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde_md.md)]). Дополнительные сведения об этих задачах см. в следующих статьях:  

- [Службы Reporting Services в SSMS](../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md)
- [Администрирование базы данных сервера отчетов](../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)
- [Создание роли RSExecRole](../reporting-services/security/create-the-rsexecrole.md)

Позволяет включать и настраивать различные функции, задавать для сервера значения по умолчанию, управлять ролями и заданиями. Дополнительные сведения об этих задачах см. в следующих статьях:

- [Определение свойств сервера отчетов](../reporting-services/tools/set-report-server-properties-management-studio.md)
- [Создание, удаление и изменение ролей](../reporting-services/security/role-definitions-create-delete-or-modify.md)
- [Разрешение и запрет печати на стороне клиента для служб Reporting Services](../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md)

## <a name="non-english-language-versions-of-sql-server-management-studio-ssms"></a>Локализованные версии SQL Server Management Studio (SSMS)

Блокировка разных языков установки снята. Например, немецкий SSMS можно установить на французскую версию Windows. Если язык операционной системы не соответствует языку SSMS, пользователю следует изменить язык в разделе "Сервис" — "Параметры" — "Международные настройки". В противном случае SSMS будет отображать интерфейс пользователя на английском языке.

Дополнительные сведения о различных настройках языка и региональных параметров с предыдущими версиями см. в разделе [Установка локализованных версий SSMS](install-other-languages.md).

## <a name="support-policy-for-ssms"></a>Политика поддержки для SSMS

- Начиная с SSMS 17.0, команда разработчиков средств SQL внедрила [политику современного жизненного цикла Майкрософт](https://support.microsoft.com/help/30881/modern-lifecycle-policy).
- Ознакомьтесь с исходным [объявлением о выходе политики современного жизненного цикла](https://support.microsoft.com/help/447912/announcing-microsoft-modern-lifecycle-policy). Дополнительные сведения см. в статье [Политика современного жизненного цикла: вопросы и ответы](https://support.microsoft.com/help/30882/modern-lifecycle-policy-faq).
- Сведения о сборе диагностических данных и использовании функций см. в статье [Приложение к конфиденциальности в SQL Server](../sql-server/sql-server-privacy.md).

## <a name="cross-platform-tool"></a>Кроссплатформенный инструмент

[!INCLUDE[ssms-azure-data-studio-mention](../includes/ssms-azure-data-studio-mention.md)]

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>Дальнейшие действия

- [Установка локализованных версий SSMS](install-other-languages.md)
- [Подключение к экземпляру SQL Server и его запрос](./quickstarts/connect-query-sql-server.md)
- [Написание инструкций Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]