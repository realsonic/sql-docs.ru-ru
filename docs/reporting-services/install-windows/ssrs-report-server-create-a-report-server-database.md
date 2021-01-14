---
title: Создание базы данных сервера отчетов (диспетчер конфигурации) | Документация Майкрософт
description: В собственном режиме SQL Server Reporting Services использует две реляционные базы данных SQL Server для хранения метаданных и объектов сервера отчетов. Одна база данных используется как основное хранилище, а вторая — для хранения временных данных.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 9/2/2020
ms.openlocfilehash: 1b7eb1e521e1c987cb956d054c4c17beba819462
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172366"
---
# <a name="create-a-report-server-database-report-server-configuration-manager"></a>Создание базы данных сервера отчетов, диспетчер конфигурации сервера отчетов  

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

В собственном режиме SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] использует две реляционные базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для хранения метаданных и объектов сервера отчетов. Одна база данных используется как основное хранилище, а вторая — для хранения временных данных. 

Эти базы данных создаются одновременно и связываются по именам. В экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию базы данных именуются **reportserver** и **reportservertempdb**. В совокупности эти две базы данных называются **базой данных сервера отчетов** или **каталогом сервера отчетов**.

::: moniker range="=sql-server-2016"

В режиме [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]интеграции с SharePoint **SQL Server** добавляет третью базу данных для метаданных предупреждений об изменении данных. Эти три базы данных создаются отдельно для каждого приложения службы SSRS. Имена баз данных по умолчанию содержат идентификатор GUID, который соответствует приложению службы. 

Далее приводятся примеры имен этих трех баз данных, используемых в режиме интеграции с SharePoint.

- ReportingService_90a9f37075544f22953c4a62e4a9f370  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370TempDB  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370_Alerting  

::: moniker-end
  
> [!IMPORTANT]  
> Не создавайте приложения, которые выполняют запросы к базе данных сервера отчетов. Схема базы данных сервера отчетов не предназначена для общего доступа. Структура таблиц в разных версиях может различаться. При создании приложений, которым необходим доступ к базе данных сервера отчетов, всегда используйте API-интерфейсы SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
>
> Исключением из этого правила являются представления журнала выполнения. Дополнительные сведения см. в разделе [Журнал выполнения сервера отчетов и представление ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
## <a name="ways-to-create-the-report-server-database"></a>Способы создания базы данных сервера отчетов

 ### <a name="native-mode"></a>Основной режим
 Создать базу данных сервера отчетов, работающего в собственном режиме, вы можете одним из следующих способов.  
  
- **Автоматически**. Если вы выбрали установку с конфигурацией по умолчанию, используйте мастер установки SQL Server. В мастере установки SQL Server этот вариант назван **Установка и настройка** и размещен на странице **Report Server Installation Options** (Параметры установки сервера отчетов). Если вы выберете вариант **Только установка**, то для создания базы данных необходимо воспользоваться диспетчером конфигурации сервера отчетов SQL Server.  
  
- **Вручную**. Используйте диспетчер конфигурации SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Базу данных сервера отчетов следует создавать вручную, если вы используете для хранения базы данных удаленный [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Дополнительные сведения см. в разделе [Создание базы данных сервера отчетов, работающего в собственном режиме](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

::: moniker range="=sql-server-2016"
  
### <a name="sharepoint-mode"></a>Режим интеграции с SharePoint 
На странице **Report Server Installation Options** (Параметры установки сервера отчетов) есть только один вариант для режима интеграции с SharePoint: **Install Only** (Только установка). Это действие устанавливает все файлы SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и общую службу SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. После этого следует создать по крайней мере одно приложение службы SSRS одним из следующих способов.  
  
- Откройте центр администрирования на сервере SharePoint и создайте приложение службы SSRS. Для получения дополнительных сведений см. раздел [о создании приложения службы](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication) в статье **Установка первого сервера отчетов в режиме интеграции с SharePoint**.  
  
- Создайте приложение службы и базы данных сервера отчетов с помощью командлетов PowerShell для SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Дополнительные сведения см. в примере создания приложений службы из статьи [Командлеты PowerShell для режима интеграции служб Reporting Services с SharePoint](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  

::: moniker-end
  
## <a name="database-server-version-requirements"></a>Требования к версии сервера баз данных

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используется для размещения баз данных сервера отчетов. Экземпляр компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] может быть локальным или удаленным. Далее перечислены поддерживаемые версии компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], на которых можно размещать базы данных сервера отчетов.  
::: moniker range=">=sql-server-ver15"

- Управляемый экземпляр SQL Azure

- SQL Server 2019

::: moniker-end
::: moniker range=">=sql-server-2017"

- SQL Server 2017  
::: moniker-end

- [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

Если вы создаете базу данных сервера отчетов на удаленном компьютере, настройте подключение через учетную запись пользователя домена или учетную запись службы с сетевым доступом. Если вы используете удаленный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], тщательно выберите правильные учетные данные для подключения сервера отчетов к этому экземпляру. Дополнительные сведения см. в разделе [Настройка соединения с базой данных сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
> [!IMPORTANT]  
> Сервер отчетов и экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на котором хранится база данных сервера отчетов, могут находиться в разных доменах. При развертывании в Интернете сервер, как правило, защищают с помощью брандмауэра. 
>
> При настройке доступа в Интернет на сервере отчетов используйте учетные данные [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для подключения к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], размещенному за брандмауэром. Защитите подключение с помощью протокола IPSEC.  
  
## <a name="edition-requirements-for-a-database-server"></a>Требования к выпуску для сервера баз данных 

 При создании базы данных сервера отчетов не все выпуски [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно использовать для ее размещения. Дополнительные сведения см. в разделе [о требованиях к выпуску серверной базы данных сервера отчетов](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md#edition-requirements-for-the-report-server-database) статьи [Возможности служб Reporting Services, поддерживаемые различными выпусками SQL Server](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).  

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о [диспетчере конфигурации сервера отчетов](reporting-services-configuration-manager-native-mode.md).  

Остались вопросы? Посетите [форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)