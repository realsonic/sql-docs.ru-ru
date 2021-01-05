---
title: Службы Reporting Services с группами доступности
description: Узнайте о настройке служб Reporting Services для работы с группами доступности Always On в SQL Server. Поддерживаемые функции для разных вариантов использования будут различными.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: edeb5c75-fb13-467e-873a-ab3aad88ab72
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: 306a200cadc535ea9da3d5d21dcd74fb15cf278c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641531"
---
# <a name="reporting-services-with-always-on-availability-groups-sql-server"></a>Службы Reporting Services с группами доступности AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  В этом разделе содержатся сведения о настройке компонента [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] для работы с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] (группами доступности) в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Существует три варианта использования служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] и [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] : базы данных для источников данных отчетов, базы данных сервера отчетов и конструирование отчетов. Поддерживаемые функции и необходимая конфигурация для разных вариантов использования будут различными.  
  
 Основное преимущество применения [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] с источниками данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] заключается в возможности использования доступных для чтения вторичных реплик в качестве источников данных для отчетов, при этом вторичные реплики продолжают обеспечивать отработку отказа для базы данных-источника.  
  
 Общие сведения о [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в разделе [Вопросы и ответы по группам доступности AlwaysOn для SQL Server 2012 (../../../sql-server/index.yml)](../../../sql-server/index.yml).  

##  <a name="requirements-for-using-reporting-services-and-always-on-availability-groups"></a><a name="bkmk_requirements"></a> Требования, которые необходимо выполнить для использования служб Reporting Services и групп доступности AlwaysOn  
 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] и Сервер отчетов Microsoft Power BI использует .NET Framework 4.0 и поддерживает свойства строки соединения [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] при работе с источниками данных.  
  
 Чтобы использовать [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в связке с  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2014 или более ранней версии, следует загрузить и установить исправление для .Net 3.5 SP1. Это исправление добавляет в клиент SQL Server поддержку компонентов групп доступности, а также поддержку свойств строки подключения **ApplicationIntent** и **MultiSubnetFailover**. Если не установить это исправление на все компьютеры, на которых размещен сервер отчетов, то пользователи, пытающиеся просмотреть отчеты, будут видеть сообщение об ошибке примерно следующего содержания, которое также будет записываться в журнал трассировки сервера отчетов.  
  
> **Сообщение об ошибке**: "Ключевое слово "applicationintent" не поддерживается"  
  
 Это сообщение выдается в том случае, когда в строке подключения служб [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] указано одно из свойств [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , которое сервер не распознает. Указанное сообщение об ошибке отображается при нажатии кнопки "Проверка подключения" в пользовательском интерфейсе служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], а также при просмотре отчета, если на сервере отчета включено отслеживание удаленных ошибок.  
  
 Дополнительные сведения о необходимом исправлении см. в разделе [Исправление КБ 2654347A добавляет поддержку функций AlwaysOn из SQL Server 2012 в платформу .NET Framework 3.5 с пакетом обновления 1 (SP1)](https://go.microsoft.com/fwlink/?LinkId=242896).  
  
 Дополнительные сведения о требованиях [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в статье [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] файлы конфигурации, например **RSreportserver.config**, не поддерживаются как часть функциональности [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Если изменения в файл конфигурации на одном из серверов отчетов вносятся вручную, то необходимо будет вручную обновить реплики.  
  
##  <a name="report-data-sources-and-availability-groups"></a><a name="bkmk_reportdatasources"></a> Источники данных отчетов и группы доступности  
 Источники данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] на основе [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] будут работать по-разному в зависимости от того, каким образом администратор настроил среду групп доступности.  
  
 Для использования [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в источниках данных отчетов необходимо настроить строку соединения с источником данных отчета, где должно быть указано *DNS-имя прослушивателя* группы доступности. Поддерживаются следующие источники данных.  
  
-   Источник данных ODBC, использующий SQL Native Client.  
  
-   Клиент SQL Server, если на сервере отчетов применено исправление .Net.  
  
 Строка соединения также может содержать новые свойства соединения AlwaysOn, которые настраивают запросы отчетов на использование вторичной реплики для доступных только для чтения отчетов. Использование вторичной реплики для запросов отчетов позволяет снизить нагрузку на доступную для чтения и записи первичную реплику. На следующем рисунке приведен пример конфигурации группы доступности из трех реплик, где строки подключения источников данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] имеют параметр ApplicationIntent=ReadOnly. В этом примере запросы отчетов отправляются вторичной, а не первичной реплике.  
  
 
  
 Ниже приведен пример строки подключения, где [AvailabilityGroupListenerName] ― это **DNS-имя прослушивателя** , заданное при создании реплик.  
  
 `Data Source=[AvailabilityGroupListenerName];Initial Catalog = AdventureWorks2016; ApplicationIntent=ReadOnly`  
  
 При нажатии кнопки **Проверка соединения** в пользовательском интерфейсе служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] будет проверена возможность установления соединения, при этом конфигурация группы доступности проверяться не будет. Если параметр ApplicationIntent включить в строку подключения к серверу, который не входит в группу доступности, то этот дополнительный параметр не будет использован, а при нажатии кнопки **Проверка соединения** будет просто проверена возможность установления соединения с указанным сервером.  
  
 Место изменения строки подключения будет зависеть от способа создания и публикации отчетов.  
  
-   **Собственный режим.** Для общих источников данных и отчетов, которые уже опубликованы на сервере отчетов, работающем в собственном режиме, используйте [!INCLUDE[ssRSWebPortal-Non-Markdown](../../../includes/ssrswebportal-non-markdown-md.md)].  
  
-   **Режим интеграции с SharePoint.** Для отчетов, которые уже опубликованы на сервере SharePoint, пользуйтесь страницами конфигурации SharePoint в библиотеках документов.  
  
-   **Конструирование отчетов:** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] или [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] при создании новых отчетов. Дополнительные сведения приведены в разделе "Конструирование отчетов" этой главы.  
  
 **Дополнительные ресурсы**  
  
-   [Управление источниками данных отчета](../../../reporting-services/report-data/manage-report-data-sources.md)  
  
-   Дополнительные сведения об имеющихся свойствах строки подключения см. в разделе [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
-   Дополнительные сведения о прослушивателях групп доступности см. в статье [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Рекомендации** Обычно при получении вторичными репликами изменений данных от первичной реплики происходит задержка. На задержку обновления между первичной и вторичными репликами влияют следующие факторы.  
  
-   Число вторичных реплик. При добавлении в конфигурацию новых вторичных реплик задержка увеличивается.  
  
-   Географическое местоположение и расстояние между первичной и вторичными репликами. Например, задержка будет больше, если вторичные реплики находятся в другом центре обработки данных, чем в ситуации, когда они расположены в том же здании, что и первичная реплика.  
  
-   Настройка режима доступности для каждой реплики. Режим доступности определяет, ожидает ли первичная реплика перед фиксацией транзакций для базы данных, чтобы вторичная реплика сохранила записи журнала транзакций на диск. Дополнительные сведения см. в разделе "Режимы доступности" статьи [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 При использовании доступной только для чтения вторичной реплики в качестве источника данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] важно обеспечить соответствие задержки обновления данных потребностям пользователей отчетов.  
  
##  <a name="report-design-and-availability-groups"></a><a name="bkmk_reportdesign"></a> Конструирование отчетов и группы доступности  
 При конструировании отчетов в [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] или проекта отчета в среде [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]пользователь может настроить строку подключения к источнику данных отчета с помощью новых свойств соединения, предусмотренных в [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Поддержка новых свойств соединения зависит от того, где пользователь просматривает отчеты.  
  
-   **Локальный предварительный просмотр:** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] и [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] используют .Net Framework 4.0 и поддерживают свойства строки подключения [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
-   **Удаленный или серверный режим просмотра**. Если после публикации отчета на сервере отчетов или использования просмотра в [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] на экране появляется сообщение об ошибке примерно следующего содержания, то это указывает на то, что просматривается отчет на сервере отчетов, где не установлено исправление .Net Framework 3.5 с пакетом обновления 1 (SP1) для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
> **Сообщение об ошибке**: "Ключевое слово "applicationintent" не поддерживается"  
  
##  <a name="report-server-databases-and-availability-groups"></a><a name="bkmk_reportserverdatabases"></a> Базы данных сервера отчетов и группы доступности  
 Службы Reporting Services и Сервер отчетов Power BI имеют ограниченную поддержку использования [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] с базами данных сервера отчетов. В группе доступности базы данных сервера отчетов можно настроить так, чтобы они были частью реплики. Однако при отработке отказа службы [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] не будут автоматически использовать другую реплику баз данных сервера отчетов. Использование MultiSubnetFailover не поддерживается для баз данных сервера отчетов.  
  
 Чтобы выполнить отработку отказа или восстановление, необходимо произвести определенные действия вручную или воспользоваться пользовательскими скриптами автоматизации. ПЕРЕД выполнением этих действий некоторые функции сервера отчетов могут работать неправильно после отработки отказа [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
> [!NOTE]  
>  При планировании отработки отказа и аварийного восстановления для баз данных сервера отчетов рекомендуется всегда создавать резервную копию ключа шифрования сервера отчетов.  
  
###  <a name="differences-between-sharepoint-native-mode"></a><a name="bkmk_differences_in_server_mode"></a> Различия с собственным режимом SharePoint  
 В этом разделе перечислены различия между способами взаимодействия сервера отчетов с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]в режиме интеграции с SharePoint и в собственном режиме.  
  
 Сервер отчетов, работающий в режиме интеграции с SharePoint, создает **3** базы данных для каждого создаваемого вами приложения служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] : Соединение с базой данных сервера отчетов, работающей в режиме интеграции с SharePoint, настраивается в центре администрирования SharePoint при создании нового приложения службы SharePoint. Имена баз данных по умолчанию включают идентификатор GUID, связанный с приложением службы. Ниже приведены примеры имен баз данных, для сервера отчетов в режиме интеграции с SharePoint:  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6TempDB  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6_Alerting  
  
 Сервера отчетов, работающие в собственном режиме, используют **2** базы данных. Ниже приведены примеры имен баз данных, для сервера отчетов в собственном режиме работы:  
  
-   ReportServer  
  
-   ReportServerTempDB  
  
 В этом режиме базы данных Alerting и связанные компоненты не поддерживаются и не используются. Серверы отчетов, работающие в собственном режиме, настраиваются в диспетчере конфигурации служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . В режиме интеграции с SharePoint в качестве имени базы данных приложения службы следует указать имя точки доступа клиента, которую вы создали при конфигурации SharePoint. Дополнительные сведения о настройке SharePoint с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]см. в разделе [Настройка и управление группами доступности SQL Server для SharePoint Server (/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14))](/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14)).  
  
> [!NOTE]
>  Серверы отчетов, работающие в режиме интеграции с SharePoint, используют процесс синхронизации между базами данных приложения служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] и базами данных содержимого SharePoint. Важно поддерживать работу баз данных сервера отчетов и баз данных содержимого вместе. Следует рассмотреть возможность включения этих баз данных в одну группу доступности, чтобы отработка отказа и восстановление для них выполнялось одновременно. Рассмотрим следующий сценарий.  
> 
>  -   Выполняется восстановление или отработка отказа копии базы данных содержимого, которая не получила такие же обновления данных, какие получила база данных сервера отчетов.  
> -   Процесс синхронизации служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] обнаружит различия между списками элементов из базы данных содержимого и из баз данных сервера отчетов.  
> -   Процесс синхронизации удалит или обновит элементы в базе данных содержимого.  
  
###  <a name="prepare-report-server-databases-for-availability-groups"></a><a name="bkmk_prepare_databases"></a> Подготовка баз данных сервера отчетов для групп доступности  
 Ниже приведены основные шаги по подготовке и добавлению баз данных сервера отчетов в [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
-   Создайте группу доступности и задайте *DNS-имя прослушивателя*.  
  
-   **Первичная реплика**. Включите базы данных сервера отчетов в одну группу доступности и создайте первичную реплику, в которой будут представлены все базы данных сервера отчетов.  
  
-   **Вторичные реплики**. Создайте одну или несколько вторичных реплик. Стандартным вариантом копирования баз данных из первичной реплики во вторичные реплики является восстановление баз данных на вторичной реплике с помощью инструкции RESTORE WITH NORECOVERY. Дополнительные сведения о создании вторичных реплик и проверке работоспособности синхронизации данных см. в статье [Запуск перемещения данных для базы данных-получателя AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
-   **Учетные данные сервера отчетов**. Во вторичных репликах, созданных из первичной, необходимо создать соответствующие учетные данные сервера отчетов. Действия, которые для этого нужно предпринять, зависят от типа проверки подлинности, используемой в среде служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]: учетная запись службы Window [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], учетная запись пользователя Windows или проверка подлинности SQL Server. Дополнительные сведения см. в разделе [Настройка соединения с базой данных сервера отчетов (диспетчер конфигурации SSRS)](../../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Обновите подключение к базе данных, указав в нем DNS-имя прослушивателя. В отношении серверов отчетов, работающих в собственном режиме, измените параметр **Имя базы данных сервера отчетов** в диспетчере конфигурации служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . В режиме интеграции с SharePoint измените **Имя сервера базы данных** для приложений служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
###  <a name="steps-to-complete-disaster-recovery-of-report-server-databases"></a><a name="bkmk_steps_to_complete_failover"></a> Действия по выполнению аварийного восстановления баз данных сервера отчетов  
 После отработки отказа на вторичную реплику, выполненной [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , необходимо выполнить следующие действия.  
  
1.  Остановите экземпляр службы агента SQL Server, который использовался СУБД базы данных-источника, где размещены базы данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
2.  Запустите службу агента SQL Server на компьютере, который стал новой первичной репликой.  
  
3.  Остановите службу сервера отчетов.  
  
     Если сервер отчетов работает в собственном режиме, то остановите сервер Windows сервера отчетов с помощью диспетчера конфигурации служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
     Если же сервер отчетов настроен в режиме интеграции с SharePoint, то остановите общую службу [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] в центре администрирования SharePoint.  
  
4.  Запустите службу сервера отчетов или службу [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] SharePoint.  
  
5.  Проверьте возможность выполнения отчетов в новой первичной реплике.  
  
###  <a name="report-server-behavior-when-a-failover-occurs"></a><a name="bkmk_failover_behavior"></a> Работа сервера отчетов при выполнении отработки отказа  
 Когда выполняется отработка отказа для баз данных сервера отчетов и была обновлена среда сервера отчетов, в которой теперь будет новая первичная реплика, возникают некоторые проблемы, которые являются следствием отработки отказа или восстановления. Эти проблемы могут по-разному сказаться на работе службы [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] в момент отработки отказа — это зависит от нагрузки и от времени, которое потребуется [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] для перехода на вторичную реплику, а администратору сервера отчетов — на обновление среды отчетов для работы с новой первичной репликой.  
  
-   Фоновая обработка может выполняться несколько раз из-за логики повтора и неспособности сервера отчетов пометить запланированную работу как выполненную во время отработки отказа.  
  
-   Фоновая обработка, которая обычно запускается во время отработки отказа, не запускается, поскольку агент SQL Server не способен записывать данные в базе данных сервера отчетов, и эти данные не будут синхронизированы с новой первичной репликой.  
  
-   После завершения отработки отказа базы данных и перезапуска службы сервера отчетов задания агента SQL Server будут автоматически созданы повторно. Пока задания агента SQL не будут воссозданы, любая фоновая обработка, связанная с заданиями агента SQL Server, выполняться не будет. Сюда входят подписки расписания и моментальные снимки служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Поддержка высокого уровня доступности и аварийного восстановления собственного клиента SQL Server](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Начало работы с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)   
 [Использование ключевых слов строки подключения с собственным клиентом SQL Server](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)   
 [Поддержка высокого уровня доступности и аварийного восстановления собственного клиента SQL Server](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Сведения о доступе клиентского подключения к репликам доступности (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
