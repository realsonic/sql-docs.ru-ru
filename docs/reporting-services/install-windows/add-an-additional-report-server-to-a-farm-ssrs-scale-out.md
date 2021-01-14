---
description: Добавление дополнительного сервера отчетов в ферму (горизонтально масштабируемые службы SSRS)
title: Добавление дополнительного сервера отчетов в ферму (горизонтально масштабируемые службы SSRS) | Документы Майкрософт
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: c1a6b683-15cf-44ae-ac60-ceee63a60aaf
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: e0f1807d8050408ca7e6153d9f7b9189b004bc8a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101976"
---
# <a name="add-an-additional-report-server-to-a-farm-ssrs-scale-out"></a>Добавление дополнительного сервера отчетов в ферму (горизонтально масштабируемые службы SSRS)

  Добавление второго и последующих серверов отчетов в режиме интеграции с SharePoint в ферму SharePoint может улучшить производительность обработки и время ответа сервера отчетов. Если при добавлении пользователей, отчетов и приложений на сервер отчетов производительность уменьшается, добавление дополнительных серверов отчетов может улучшить ее. Второй сервер отчетов также рекомендуется добавить для повышения доступности серверов отчетов, когда обнаруживаются проблемы с оборудованием или проводится штатное обслуживание отдельных серверов в среде. Начиная с выпуска [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] действия по созданию масштабного развертывания в среде служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint соответствуют стандартам развертывания ферм SharePoint и используют возможности SharePoint по балансировке нагрузки.  
  
> [!IMPORTANT]  
>  Масштабное развертывание служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] поддерживается не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделе [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] статьи [Функции, поддерживаемые выпусками SQL Server](~/sql-server/editions-and-components-of-sql-server-2017.md#SSRS).  
  
> [!TIP]  
>  Начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] диспетчер конфигурации служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] не используется для добавления серверов и горизонтального увеличения масштаба серверов отчетов. Продукты SharePoint управляют масштабным развертыванием служб Reporting Services по мере добавления в ферму серверов SharePoint со службами [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Сведения о горизонтальном увеличении масштаба серверов отчетов в собственном режиме см. в разделе [Настройка развертывания с горизонтальным увеличением масштаба сервера отчетов в собственном режиме (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
##  <a name="load-balancing"></a><a name="bkmk_loadbalancing"></a> Балансировка нагрузки  
 Балансировка нагрузки для приложений служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] выполняется автоматически SharePoint, если в среде не работает самостоятельно разработанное или предоставленное сторонними разработчиками решение по балансировке нагрузки. По умолчанию SharePoint балансирует нагрузку так, чтобы каждое приложение служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] распределялось по всем серверам приложений, где запущена служба [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Чтобы проверить, что служба [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] установлена и запущена, щелкните ссылку **Управление службами на сервере** в центре администрирования SharePoint.  
  
##  <a name="prerequisites"></a><a name="bkmk_prerequisites"></a> Предварительные требования  
  
-   Для запуска программы установки SQL Server необходимо быть локальным администратором.  
  
-   Компьютер должен быть присоединен к домену.  
  
-   Необходимо знать имя существующего сервера базы данных, на котором размещены базы данных конфигурации и содержимого SharePoint.  
  
-   Сервер базы данных должен быть настроен для установления удаленных соединений с базой данных.  В противном случае будет невозможно подключить новый сервер к ферме, так как он не сможет соединиться с базами данных конфигурации SharePoint.  
  
-   На новом сервере должна быть установлена та же версия SharePoint, что и на текущих работающих серверах фермы. Например, если в ферме уже установлен SharePoint 2013 с пакетом обновления 1 (SP1), то на новом сервере также нужно установить пакет обновления 1 (SP1), прежде чем можно будет присоединить его к ферме.  
  
##  <a name="steps"></a><a name="bkmk_steps"></a> Шаги  
 Шаги, приведенные в этом разделе, предполагают, что установкой и настройкой сервера занимается администратор фермы SharePoint. На схеме показана типичная трехуровневая среда. Пронумерованные элементы описаны ниже.  
  
-   (1) Несколько серверов клиентского веб-интерфейса (WFE). Для серверов WFE требуется надстройка служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] для SharePoint 2016.  
  
-   (2) Один сервер приложений, на котором запущены службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и веб-сайты, например центр администрирования. Чтобы добавить второй сервер приложений на этот уровень, выполните следующие шаги.  
  
-   (3) Два сервера базы данных SQL Server.  
  
-   (4) Представляет программное или решение для оборудования по распределению сетевой нагрузки (NLB)  
  
 ![Добавление сервера приложений служб Reporting Services](../../reporting-services/install-windows/media/rs-sharepointscale.gif "Добавление сервера приложений служб Reporting Services")  
  
 Следующие шаги предполагают, что установкой и настройкой сервера занимается администратор. Сервер будет настроен в качестве нового сервера приложений на ферме и не будет использоваться в качестве веб-интерфейса (WFE).  
  
|Шаг|Описание и ссылка|  
|----------|--------------------------|  
|Добавление сервера SharePoint в ферму.|Для развертывания еще одного приложения служб Reporting Services потребуется установить SharePoint.<br/><br/>Инструкции для SharePoint 2013 см. в разделе [Добавление сервера SharePoint в ферму в SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm).<br/><br/>Инструкции для SharePoint 2016 см. в разделе [Добавление сервера SharePoint в ферму в SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm).|  
|Установите и настройте службы Reporting Services в режиме интеграции с SharePoint.|Запустите установку SQL Server. Дополнительные сведения об установке режима SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] см. в разделе [Установка первого сервера отчетов в режиме интеграции с SharePoint](install-the-first-report-server-in-sharepoint-mode.md).<br /><br /> Если сервер будет использоваться только в качестве сервера приложений и не будет служить в качестве WFE, то не нужно выбирать компонент **Надстройка служб Reporting Services для продуктов SharePoint**.<br /><br /> 1) На странице **Роль установки** выберите **Установка компонентов SQL Server**.<br /><br /> 2) На странице **Выбор компонентов** выберите компонент **Reporting Services — SharePoint**<br /><br /> 3) На странице **Конфигурация служб Reporting Services**  установите флажок **Только установка** для компонента **Службы Reporting Services в режиме интеграции с SharePoint**.|  
|Убедитесь, что службы Reporting Services работают.|1) В центре администрирования SharePoint выберите пункт **Управление серверами на ферме** в группе **Параметры системы** .<br /><br /> 2) Проверьте службу **SQL Server Reporting Services**.<br /><br />Дополнительные сведения см. в разделе [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).|  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional"></a> Дополнительная настройка  
 Отдельные серверы служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в масштабном развертывании вы можете оптимизировать для выполнения фоновой обработки только так, чтобы они не соперничали за ресурсы с интерактивным выполнением отчетов. Фоновая обработка включает в себя расписания, подписки и оповещения о данных.  
  
 Чтобы изменить поведение отдельных серверов отчетов, в файле конфигурации **RSreportServer.config** для **\<IsWebServiceEnable>** задайте значение false.  
  
 По умолчанию для серверов отчетов параметру \<IsWebServiceEnable> присвоено значение TRUE. Если параметры всех серверов настроены для значения TRUE, нагрузка от интерактивной и фоновой обработки будет сбалансирована по всем узлам фермы.  
  
 Если параметру \<IsWebServiceEnable> присвоить значение False для всех серверов отчетов, при попытке использовать функции служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] на экране появится сообщение об ошибке, подобное приведенному далее.  
  
```output
The Reporting Services Web Service is not enabled. Configure at least one instance of the Reporting Services SharePoint Service to have <IsWebServiceEnable> set to true.
```
 
 Дополнительные сведения см. в статье [Изменение файла конфигурации служб Reporting Services (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)  

## <a name="next-steps"></a>Дальнейшие действия

[Добавление сервера SharePoint в ферму в SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[Добавление сервера SharePoint в ферму в SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm)

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).