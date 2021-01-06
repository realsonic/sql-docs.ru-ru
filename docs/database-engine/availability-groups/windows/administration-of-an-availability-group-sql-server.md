---
title: Администрирование группы доступности (индекс содержимого)
description: 'Справочный индекс статей по основам администрирования группы доступности Always On: изменение свойств, добавление или удаление реплик, добавление или удаление баз данных, отработка отказа, настройка прослушивателя и т. д.'
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], managing
ms.assetid: 0b7542fa-235e-413d-81bf-3eff9ee07480
author: cawrites
ms.author: chadam
ms.openlocfilehash: 414b407cc2adccf20f2f9d7dc0e01bfb171899f7
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643341"
---
# <a name="administration-of-an-availability-group"></a>Администрирование группы доступности
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
 Управление существующей группой доступности AlwaysOn в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] включает в себя одну или несколько следующих задач:  
  
-   Изменение свойств существующей реплики доступности, например для изменения клиентского доступа к соединению (для настройки вторичных реплик, доступных для чтения), изменение режима отработки отказа, режима доступности или задание времени ожидания сеанса.    
-   Добавление или удаление вторичных реплик.    
-   Добавление или удаление базы данных.    
-   Приостановка или возобновление работы базы данных.   
-   Выполнение запланированной отработки отказа вручную ( *отработка отказа вручную*) или принудительной отработки отказа вручную ( *принудительная отработка отказа*).    
-   Создание или настройка прослушивателя группы доступности.    
-   Управление [доступными для чтения вторичными репликами](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) для данной группы доступности. Необходимо настроить доступ для чтения для одной или нескольких реплик, выполняемых под вторичной ролью, и настроить маршрутизацию только для чтения.    
-   Управление [резервными копиями вторичных реплик](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) определенной группы доступности. Необходимо настроить место выполнения задач по созданию резервных копий и написать скрипты для этих задач. необходимо создать скрипты заданий резервного копирования для каждой базы данных в группе доступности на каждом экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на котором размещается реплика доступности.    
-   Удаление группы доступности.    
-   Миграция между кластерами групп доступности AlwaysOn для обновления ОС  
  
## <a name="configure-an-existing-availability-group"></a>Настройка существующей группы доступности
  
-   [Добавление вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Удаление вторичной реплики из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Добавление базы данных в группу доступности (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)    
-   [Удаление базы данных-получателя из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Удаление базы данных-источника из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Настройка гибкой политики отработки отказа для обеспечения контроля над автоматическим переходом на другой ресурс (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
 ## <a name="manage-an-availability-group"></a>Управление группой доступности  
  
-   [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Выполнение запланированного перехода на другой ресурс вручную для группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Выполнение принудительного перехода на другой ресурс вручную для группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Удаление группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
 ## <a name="manage-an-availability-replica"></a>Управление репликой доступности  
  
-   [Добавление вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Присоединение вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Удаление вторичной реплики из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Смена режима доступности для реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Смена режима отработки отказа для реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Настройка резервного копирования в репликах доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Настройка доступа только для чтения в реплике доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Настройка маршрутизации только для чтения в группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Изменение периода ожидания сеанса для реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="manage-an-availability-database"></a>Управление базой данных доступности  
  
-   [Добавление базы данных в группу доступности (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
-   [Присоединение базы данных-получателя к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Удаление базы данных-источника из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Удаление базы данных-получателя из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Приостановка базы данных доступности (SQL Server)](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
-   [Возобновление базы данных доступности (SQL Server)](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
## <a name="monitor-an-availability-group"></a>Наблюдение за группой доступности
  
-   [Отслеживание групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
 ## <a name="support-migrating-availability-groups-to-a-new-wsfc-cluster-cross-cluster-migration"></a>Поддержка миграции групп доступности на новый кластер WSFC (миграция с одного кластера на другой)
  
-   [Смена контекста кластера HADR экземпляра сервера (SQL Server)](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
-   [Перевод группы доступности в режим "вне сети" (SQL Server)](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   **Блоги**  
  
     [Блоги команды разработчиков SQL Server Always On: официальный блог команды разработчиков SQL Server Always On](/archive/blogs/sqlalwayson/)    
[Блоги инженеров CSS SQL Server](/archive/blogs/psssql/)  
  
-   **Видеоролики**  
  
     [Microsoft SQL Server с рабочим названием Denali Always On, часть 1: представляем решение высокого уровня доступности следующего поколения](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)    
     [Microsoft SQL Server с рабочим названием Denali Always On, часть 2: создание критически важного решения по обеспечению высокого уровня доступности с использованием Always On](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Технические документы**  
  
     [Технические документы Майкрософт по SQL Server 2012](https://msdn.microsoft.com/library/hh403491.aspx)    
     [Технические документы группы консультантов по SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>См. также:  
 [Группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Настройка экземпляра сервера для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Активные вторичные реплики: вторичные реплики для чтения (группы доступности Always On)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Активные вторичные реплики: резервное копирование во вторичных репликах (группы доступности Always On)](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)   
 [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Политики AlwaysOn на случай проблем в работе с группами доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Отслеживание групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Группы доступности AlwaysOn: взаимодействие (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Общие сведения об инструкциях Transact-SQL для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [Обзор командлетов PowerShell для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
