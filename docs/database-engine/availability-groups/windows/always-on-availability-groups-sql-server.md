---
title: 'Группы доступности: решение для обеспечения высокой доступности и аварийного восстановления'
description: Группы доступности Always On — это решение SQL Server для высокой доступности и аварийного восстановления, являющееся более полнофункциональной альтернативой зеркальному отображению баз данных на уровне предприятия. Изучите основные сведения и функциональные возможности этой функции.
ms.custom: seodec18
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- secondary replicas, see Availability Groups [SQL Server]
- availability [SQL Server]
- AlwaysOn [SQL Server], see Availability Groups [SQL Server]
- Availability Groups [SQL Server]
ms.assetid: aa427606-8422-4656-b205-c9e665ddc8c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7f9b06bd6805e75c053037b18fc6870ef1469500
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642228"
---
# <a name="always-on-availability-groups-a-high-availability-and-disaster-recovery-solution"></a>Группы доступности Always On: решение для обеспечения высокой доступности и аварийного восстановления
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Функция [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] — это решение высокой доступности и аварийного восстановления, являющееся альтернативой зеркальному отображению баз данных на уровне предприятия. Поддержка [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], добавленная с версии [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , позволяет максимально увеличить доступность набора пользовательских баз данных для предприятия. *Группа доступности* поддерживает среду отработки отказа для дискретного набора пользовательских баз данных, известных как *базы данных доступности*, которые совместно выполняют переход на другой ресурс. Группа доступности поддерживает набор первичных баз данных чтения и записи и от одного до четырех наборов соответствующих вторичных баз данных. Кроме того, базы данных-получатели можно сделать доступными только для чтения или для некоторых операций резервного копирования.  
  
 Группа доступности выполняет переход на другой ресурс на уровне реплики доступности. Переход на другой ресурс не вызывается проблемами баз данных, например обозначением базы данных как подозрительной в связи с потерей файла данных, удалением базы данных или повреждением журнала транзакций.  
 
 >[!NOTE]
 >Полное официальное название этой функции обеспечения доступности — группы доступности AlwaysOn. В качестве сокращения используется вариант AG, но не AOAG или AAG. 
  
##  <a name="benefits"></a><a name="Benefits"></a> Преимущества  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] предоставляет широкий набор параметров, позволяющих повысить уровень доступности баз данных и улучшить использование ресурсов. Ключевыми компонентами являются:  
  
-   Поддержка до девяти реплик доступности. *Реплика доступности* является выделенным экземпляром группы доступности, который размещается на конкретном экземпляре SQL Server и поддерживает локальную копию каждой базы данных доступности, которая принадлежит группе доступности. Каждая группа доступности поддерживает одну первичную реплику и до восьми вторичных реплик. Дополнительные сведения см. в статье [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Каждая реплика доступности должна размещаться на отдельном узле одного отказоустойчивого кластера Windows Server (WSFC). Дополнительные сведения о предварительных требованиях, ограничениях и рекомендациях для групп доступности см. в статье [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Поддерживает альтернативные режимы доступности следующим образом:  
  
    -   *Режим асинхронной фиксации*. Этот режим доступности представляет собой решение аварийного восстановления, которое хорошо работает тогда, когда реплики доступности распределены на различных расстояниях.  
  
    -   *Режим синхронной фиксации*. Этот режим доступности отдает предпочтение высокому уровню доступности и защите данных перед производительностью за счет повышения задержки транзакций. Отдельно взятая группа доступности может поддерживать до трех реплик доступности с синхронной фиксацией, в том числе текущую первичную реплику.  
  
     Дополнительные сведения см. в разделе [Режимы доступности (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). 

     В [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] максимальное количество синхронных реплик увеличено до пяти, по сравнению с тремя в [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)]. Вы можете настроить эту группу из пяти реплик для автоматического перехода на другой ресурс в пределах группы. Предоставляется одна первичная реплика и четыре синхронные вторичные реплики.
  
-   Поддерживает различные формы отработки отказа другой группы доступности: автоматический переход на другой ресурс, запланированный переход на другой ресурс вручную (обычно называемый "переходом на другой ресурс вручную") и принудительный переход на другой ресурс вручную (который обычно называется "принудительной отработкой отказа"). Дополнительные сведения см. далее в подразделе [Отработка отказа и режимы отработки отказа (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Позволяет настроить данную реплику доступности для поддержки одной или обеих возможностей активных вторичных реплик.  
  
    -   Доступ с подключением только для чтения, который позволяет использовать подключения только для чтения для доступа и чтения баз данных во время работы в качестве вторичной реплики. Дополнительные сведения см. в разделе [Активные вторичные реплики: доступные только для чтения вторичные реплики (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
    -   Выполнение операций резервного копирования для своих баз данных во время работы в качестве вторичной реплики. Дополнительные сведения см. в статье [Активные вторичные реплики, резервное копирование во вторичных репликах (группы доступности Always On)](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
     Использование возможностей активных вторичных реплик позволяет улучшить эффективность использования информационных технологий и снизить стоимость за счет более рационального использования ресурсов вторичного аппаратного обеспечения. Кроме того, перевод приложений с намерением чтения и заданий резервного копирования на вторичные реплики позволяет повысить производительность работы основной реплики.  
  
-   Поддерживает прослушиватель группы доступности для каждой группы доступности. *Прослушиватель группы доступности* — это сервер, к которому могут подключаться клиенты, чтобы получить доступ к базе данных из первичной или вторичной реплики группы доступности AlwaysOn. Прослушиватели группы доступности направляют входящие соединения на первичную реплику или на доступную только для чтения вторичную реплику. Прослушиватель обеспечивает быструю отработку отказа приложений после отработки отказа группы доступности. Дополнительные сведения см. в разделе [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
-   Поддерживает гибкую политику отработки отказа для обеспечения большего контроля над отработкой отказа группы доступности. Дополнительные сведения см. далее в подразделе [Отработка отказа и режимы отработки отказа (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Поддерживает автоматическое восстановление страниц для защиты от повреждения. Дополнительные сведения см. в статье [Автоматическое восстановление страниц (группы доступности: зеркальное отображение баз данных)](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md).  
  
-   Поддерживает шифрование и сжатие, обеспечивающие безопасный, высокопроизводительный транспорт.  
  
-   Предоставляет интегрированный набор средств для упрощения развертывания и управления группами доступности, включая  
  
    -   [!INCLUDE[tsql](../../../includes/tsql-md.md)] для создания групп доступности и управления ими. Дополнительные сведения см. в статье [Общие сведения об инструкциях Transact-SQL для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
    -   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] , как показано ниже:  
  
        -   [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] создает и настраивает группы доступности. В некоторых средах этот мастер также может автоматически подготавливать базы данных-получатели и запускать синхронизацию данных для каждой из них. Дополнительные сведения см. в статье [Использование диалогового окна "Создание группы доступности" (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md).  
  
        -   [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] добавляет одну или несколько баз данных-источников к существующей группе доступности. В некоторых средах этот мастер также может автоматически подготавливать базы данных-получатели и запускать синхронизацию данных для каждой из них. Дополнительные сведения см. в разделе [Использование мастера добавления базы данных в группу доступности (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
        -   [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] добавляет одну или несколько баз данных-получателей к существующей группе доступности. В некоторых средах этот мастер также может автоматически подготавливать базы данных-получатели и запускать синхронизацию данных для каждой из них. Дополнительные сведения см. в разделе [Использование мастера добавления реплики в группу доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md).  
  
        -   [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)] запускает переход на другой ресурс вручную для группы доступности. В зависимости от конфигурации и состояния вторичной реплики, указанной в качестве целевой реплики отработки отказа, мастер может выполнить запланированный или принудительный переход на другой ресурс вручную. Дополнительные сведения см. в подразделе [Использование мастера отработки отказа группы доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md).  
  
    -   [!INCLUDE[ssAoDash](../../../includes/ssaodash-md.md)] отслеживает группы доступности AlwaysOn, реплики доступности и базы данных доступности и оценивает результаты политик AlwaysOn. Дополнительные сведения см. в статье [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md).  
  
    -   В области сведений обозревателя объектов отображаются основные сведения о существующих группах доступности. Дополнительные сведения см. в разделе [Использование раздела "Подробности обозревателя объектов" для мониторинга групп доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Командлеты PowerShell Дополнительные сведения см. в статье [Обзор командлетов PowerShell для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md).  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Термины и определения  
 **группа доступности**  
 Контейнер для набора баз данных, *базы данных доступности*, которые совместно отрабатывают отказ.  
  
 **база данных доступности**  
 База данных, принадлежащая к группе доступности. Для каждой базы данных доступности группа доступности поддерживает одну копию для чтения и записи ( *первичная база данных*) и до восьми копий только для чтения (*вторичные базы данных*).  
  
 **база данных-источник**  
 Копия базы данных доступности для чтения и записи.  
  
 **база данных-получатель**  
 Копия базы данных доступности только для чтения.  
  
 **реплика доступности**  
 Экземпляр группы доступности, который размещается на определенном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и поддерживает локальную копию каждой базы данных доступности, входящей в группу доступности. Существует два типа реплик доступности: одна *первичная реплика* и до восьми *вторичных реплик*.  
  
 **первичная реплика**  
 Реплика доступности делает базы данных-источники доступными для соединений чтения и записи с клиентов, а также отправляет записи журнала транзакций для каждой базы данных-источника каждой вторичной реплике.  
  
 **вторичная реплика**  
 Реплика доступности, которая поддерживает вторичную копию каждой базы данных доступности и служит потенциальным назначением отработки отказа для группы доступности. При необходимости вторичная реплика может поддерживать доступ только для чтения к базам данных-получателям и создание резервных копий баз данных-получателей.  
  
 **прослушиватель группы доступности**  
 Имя сервера, к которому могут подключаться клиенты, чтобы получить доступ к базе данных из первичной или вторичной реплики группы доступности AlwaysOn. Прослушиватели группы доступности направляют входящие соединения на первичную реплику или на доступную только для чтения вторичную реплику.  
  
> [!NOTE]  
>  Дополнительные сведения см. в статье [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="interoperability-and-coexistence-with-other-database-engine-features"></a><a name="Interoperability"></a> Возможности взаимодействия и совместной работы с другими функциями компонента Database Engine  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] можно использовать вместе со следующими функциями и компонентами службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   [Об отслеживании измененных данных (SQL Server)](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
-   [Об отслеживании изменений (SQL Server)](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
  
-   [Автономные базы данных](../../../relational-databases/databases/contained-databases.md)  
  
-   [Шифрование базы данных](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
-   [Моментальные снимки базы данных](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)  
  
-   [FILESTREAM](../../../relational-databases/blob/filestream-sql-server.md)  
  
-   [FileTable](../../../relational-databases/blob/filetables-sql-server.md)  
  
-   [Доставка журналов](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
-   [Удаленное хранилище больших двоичных объектов](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
-   [Репликация](../../../relational-databases/replication/sql-server-replication.md)  
  
-   [Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
-   [Агент SQL Server](../../../ssms/agent/sql-server-agent.md)  
  
-   [службы Reporting Services](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)  
  
> [!WARNING]  
>  Дополнительные сведения об ограничениях на использование других компонентов с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в статье [Группы доступности AlwaysOn: взаимодействие (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Начало работы с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   **Блоги**  
  
     [Блоги команды разработчиков SQL Server AlwaysOn: официальный блог по SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Блоги инженеров CSS SQL Server](/archive/blogs/psssql/)  
  
-   **Видеоролики**  
  
     [Microsoft SQL Server с рабочим названием Denali AlwaysOn, часть 1. Вводные сведения о решении следующего поколения по обеспечению высокого уровня доступности](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server с рабочим названием Denali AlwaysOn, часть 2. Создание критически важного решения по обеспечению высокого уровня доступности с использованием AlwaysOn](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Технические документы**  
  
     [Руководство по решениям режима AlwaysOn в Microsoft SQL Server для обеспечения высокой доступности и аварийного восстановления](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Технические документы Майкрософт Microsoft по SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Технические документы группы консультантов по SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Настройка экземпляра сервера для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Администрирование группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Отслеживание групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Общие сведения об инструкциях Transact-SQL для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [Обзор командлетов PowerShell для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
