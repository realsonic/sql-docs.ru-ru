---
title: Средства для отслеживания групп доступности
description: 'Ссылки на различные средства для отслеживания производительности и работоспособности групп доступности Always On. '
ms.custom: seodec18
ms.date: 06/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], troubleshooting
ms.assetid: 1d5e3291-0d0a-45a1-88e5-1fc242d17210
author: cawrites
ms.author: chadam
ms.openlocfilehash: 734c89393b77fa74db57ce2896af64bbac7c1a63
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643745"
---
# <a name="tools-to-monitor-always-on-availability-groups"></a>"Средства для отслеживания групп доступности Always On"
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Для отслеживания свойств и состояния группы доступности AlwaysOn можно пользоваться указанными ниже средствами.  
  
|Инструмент|Краткое описание|Ссылки|  
|----------|-----------------------|-----------|  
|Пакет мониторинга System Center для SQL Server|Пакет мониторинга для SQL Server (SQLMP) является рекомендованным решением для мониторинга групп доступности, реплик доступности и баз данных доступности для ИТ-администраторов. Для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] особенно релевантны следующие функции мониторинга.<br /><br /> Автоматическое обнаружение групп доступности, реплик доступности и баз данных доступности среди сотен компьютеров. Это позволяет легко отслеживать всю номенклатуру [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .<br /><br /> Полнофункциональная рассылка уведомлений и бронирование в System Center Operations Manager (SCOM). Эти функции обеспечивают подробный набор знаний о том, как быстрее решить проблему.<br /><br /> В пользовательском расширении мониторинга исправности AlwaysOn используется управление на основе политик (PBM).<br /><br /> Производится свертка исправности с баз данных доступности до реплик доступности.<br /><br /> Пользовательские задачи, которые управляют [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] из консоли System Center Operations Manager.|Чтобы загрузить пакет мониторинга (SQLServerMP.msi) и *Руководство по пакету управления SQL Server для System Center Operations Manager* (SQLServerMPGuide.doc), перейдите на страницу:<br /><br /> [Пакет мониторинга System Center для SQL Server](https://www.microsoft.com/download/details.aspx?displaylang=en&id=10631)|  
|[!INCLUDE[tsql](../../../includes/tsql-md.md)]|[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] и динамические административные представления предоставляют широкий набор сведений о группах доступности и их репликах, базах данных, прослушивателях и кластерной среде WSFC.|[Отслеживание групп доступности (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|Область **Сведения обозревателя объектов** содержит базовые сведения о группах доступности, размещенных на экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , с которым установлено соединение.<br /><br /> **\*\* Совет. \*\*** Эта панель используется для выбора нескольких групп доступности, реплик или баз данных и выполнения рутинных задач администрирования с выбранными объектами, например удаления нескольких реплик доступности или баз данных из группы доступности.|[Использование сведений обозревателя объектов для мониторинга групп доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|Диалоговые окна **Свойства** позволяют просматривать свойства групп доступности, реплик или прослушивателей, а также, в некоторых случаях, позволяют изменять значения данных свойств.|-   [Просмотр свойств группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)<br />-   [Просмотр свойств реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)<br />-   [Просмотр свойств прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)|  
|Системный монитор|Объект производительности **SQLServer:Availability Replica** содержит счетчики производительности, которые сообщают сведения о репликах доступности.|[SQL Server, реплика доступности](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)|  
|Системный монитор|Объект производительности **SQLServer:Database Replica** содержит счетчики производительности, которые сообщают сведения о базах данных-получателях для определенной вторичной реплики.<br /><br /> Объект **SQLServer:Databases** в SQL Server содержит счетчики производительности, которые, среди прочего, отслеживают активность журнала транзакций. Следующие счетчики имеют особое значение для отслеживания активности журнала транзакций для баз данных доступности: **Время записи журнала на диск (мс)** , **Сбросов журнала в секунду**, **Неудачных обращений к кэшу пула журнала в секунду**, **Операций чтения диска пула журнала в секунду** и **Запросов пула журнала в секунду**.|[SQL Server, реплика базы данных](../../../relational-databases/performance-monitor/sql-server-database-replica.md) и [SQL Server, объект баз данных](../../../relational-databases/performance-monitor/sql-server-databases-object.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   **Блоги**  
  
     [Модель исправности AlwaysOn, часть 1. Архитектура модели исправности](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)  
  
     [Модель исправности AlwaysOn, часть 2. Расширение модели исправности](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)  
  
     [Мониторинг исправности AlwaysOn с использованием PowerShell, часть 1. Общий обзор командлетов](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)  
  
     [Мониторинг исправности AlwaysOn с использованием PowerShell, часть 2. Расширенное использование командлетов](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)  
  
     [Мониторинг исправности AlwaysOn с использованием PowerShell, часть 3. Простое приложение для мониторинга](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)  
  
     [Мониторинг исправности AlwaysOn с использованием PowerShell, часть 4. Интеграция с агентом SQL Server](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)  
  
     [Блоги команды разработчиков SQL Server AlwaysOn: официальный блог по SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Блоги инженеров CSS SQL Server](/archive/blogs/psssql/)  
  
-   **Технические документы**  
  
     [Технические документы Майкрософт Microsoft по SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Технические документы группы консультантов по SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога групп доступности AlwaysOn (Transact-SQL)](../../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Динамические представления управления и функции, связанные с группами доступности AlwaysOn (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Гибкая политика отработки отказа для автоматического перехода на другой ресурс группы доступности (SQL Server)](./configure-flexible-automatic-failover-policy.md)   
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Автоматическое восстановление страниц (группы доступности: зеркальное отображение баз данных)](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
