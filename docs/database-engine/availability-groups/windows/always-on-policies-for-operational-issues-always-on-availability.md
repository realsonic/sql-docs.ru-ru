---
title: Управление на основе политик. Группы доступности
description: Модель работоспособности групп доступности Always On вычисляет набор стандартных (PBM) политик управления на основе политик. Их можно использовать для просмотра исправности группы доступности, а также реплик и баз данных в SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], policies
ms.assetid: afa5289c-641a-4c03-8749-44862384ec5f
author: cawrites
ms.author: chadam
ms.openlocfilehash: ac6918ca2bc7a89adbad0fe531e1bf94948a9090
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641449"
---
# <a name="policy-based-management-for-operational-issues-with-always-on-availability-groups"></a>Управление проблемами в работе на основе политик с использованием групп доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Модель работоспособности групп доступности Always On вычисляет набор стандартных (PBM) политик управления на основе политик. Их можно использовать для просмотра исправности группы доступности, а также реплик доступности и баз данных в SQL Server.  
  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Термины и определения  
 Стандартные политики AlwaysOn  
 Набор встроенных политик, который позволяет администратору базы данных проверить группу доступности, а также соответствие ее реплик доступности и баз данных состояниям, определенным политиками AlwaysOn.  
  
 [Группы доступности AlwaysOn](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
 Решение высокой доступности и аварийного восстановления, являющееся альтернативой зеркальному отображению баз данных на уровне предприятия.  
  
 группа доступности  
 Контейнер для дискретного набора пользовательских баз данных, известного как *базы данных доступности*, которые совместно отрабатывают отказ.  
  
 реплика доступности  
 Создание экземпляра группы доступности, которая размещена на определенном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и поддерживает локальную копию каждой базы данных доступности, входящей в группу доступности. Существует два типа реплик доступности: одна *первичная* и от одной до четырех *вторичных реплик*. Экземпляры сервера, на которых размещаются реплики доступности для данной группы доступности, должны размещаться на разных узлах одной кластеризации WSFC.  
  
 база данных доступности  
 База данных, принадлежащая к группе доступности. Для каждой базы данных доступности группа доступности поддерживает одну копию для записи и чтения ( *база данных-источник*) и от одной до четырех копий только для чтения (*базы данных-получатели*).  
  
 Панель мониторинга AlwaysOn  
 Панель мониторинга [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] дает обзорное представление исправности группы доступности. Дополнительные сведения см. в подразделе [Панель мониторинга AlwaysOn](#Dashboard)далее в этом разделе.  
  
##  <a name="predefined-policies-and-issues"></a><a name="Always OnPBM"></a> Стандартные политики и проблемы  
 В следующей таблице приведены итоговые сведения о стандартных политиках.  
  
|Имя политики|Проблема|Категория **&#42;**|Аспект|  
|-----------------|-----------|--------------------|-----------|  
|Состояние кластера WSFC|[WSFC служба кластеров работает в режиме вне сети](../../../database-engine/availability-groups/windows/wsfc-cluster-service-is-offline.md).|Critical|Экземпляр SQL Server|  
|Режим «в сети» группы доступности|[Группа доступности в режиме вне сети](../../../database-engine/availability-groups/windows/availability-group-is-offline.md).|Critical|группа доступности|  
|Готовность группы доступности к автоматическому переходу на другой ресурс при сбое|[Группа доступности не готова к автоматическому переходу на другой ресурс](../../../database-engine/availability-groups/windows/availability-group-is-not-ready-for-automatic-failover.md).|Critical|группа доступности|  
|Состояние синхронизации данных реплик доступности|[Некоторые реплики доступности не выполняют синхронизацию данных](../../../database-engine/availability-groups/windows/some-availability-replicas-are-not-synchronizing-data.md).|Предупреждение|группа доступности|  
|Состояние синхронизации данных синхронных реплик|[Некоторые синхронные реплики не синхронизированы](../../../database-engine/availability-groups/windows/some-synchronous-replicas-are-not-synchronized.md).|Предупреждение|группа доступности|  
|Состояние роли реплик доступности|[Некоторые реплики доступности не имеют исправной роли](../../../database-engine/availability-groups/windows/some-availability-replicas-do-not-have-a-healthy-role.md).|Предупреждение|группа доступности|  
|Состояние соединения с репликами доступности|[Некоторые реплики доступности отключены](../../../database-engine/availability-groups/windows/some-availability-replicas-are-disconnected.md).|Предупреждение|группа доступности|  
|Состояние роли реплики доступности|[Доступность репликации не имеет исправной роли](../../../database-engine/availability-groups/windows/availability-replica-does-not-have-a-healthy-role.md).|Critical|Реплика доступности|  
|Состояние соединения с репликами доступности|[Реплика доступности отключена](../../../database-engine/availability-groups/windows/availability-replica-is-disconnected.md).|Critical|Реплика доступности|  
|Состояние присоединения реплики доступности|[Реплика доступности не присоединена](../../../database-engine/availability-groups/windows/availability-replica-is-not-joined.md).|Предупреждение|Реплика доступности|  
|Состояние синхронизации данных реплики доступности|[Состояние синхронизации данных некоторых баз данных доступности не является исправным](../../../database-engine/availability-groups/windows/data-synchronization-state-of-some-availability-database-is-not-healthy.md).|Предупреждение|Реплика доступности|  
|Состояние приостановки базы данных доступности|[База данных доступности приостановлена](../../../database-engine/availability-groups/windows/availability-database-is-suspended.md).|Предупреждение|База данных доступности|  
|Состояние присоединения базы данных доступности|[База данных-получатель не присоединена](../../../database-engine/availability-groups/windows/secondary-database-is-not-joined.md).|Предупреждение|База данных доступности|  
|Состояние синхронизации базы данных доступности|[Состояние синхронизации данных баз данных доступности не является исправным](../../../database-engine/availability-groups/windows/data-synchronization-state-of-availability-database-is-not-healthy.md).|Предупреждение|База данных доступности|  
  
> [!IMPORTANT]
>  **&#42;** При работе с политиками AlwaysOn в качестве идентификаторов используются имена категорий. При изменении имени категории AlwaysOn ее функция оценки работоспособности будет нарушена. Поэтому не следует изменять имена категорий AlwaysOn.  
  
##  <a name="always-on-dashboard"></a><a name="Dashboard"></a> Панель мониторинга AlwaysOn  
 Панель мониторинга AlwaysOn дает обзорное представление исправности группы доступности. На панели мониторинга AlwaysOn имеются следующие функции:  
  
-   Позволяет легко увидеть сведения о данной группе доступности, а также ее реплик доступности и баз данных.  
  
-   Показывает визуальные признаки ключевых состояний, чтобы помочь администраторам баз данных принимать быстрые рабочие решения.  
  
-   Предоставляет точки запуска для сценариев устранения неполадок.  
  
-   Для данной рабочей проблемы выдает в диалоговом окне **Результат оценки политики** сведения о нарушениях определенной политики исправности AlwaysOn и ссылки на справку для исправления.  
  
-   Предоставляет средство просмотра расширенных событий исправности с целью показа предшествующих событий для специфических проблем AlwaysOn.  
  
-   Если автоматическое переключение группы доступности может помочь устранению, предоставляет точку запуска для ссылок[мастера перехода на другой ресурс для группы доступности](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md). Этот мастер помогает администратору базы данных выполнить переход на другой ресурс вручную.  
  
##  <a name="extending-the-always-on-health-model"></a><a name="ExtendHealthModel"></a> Расширение модели исправности AlwaysOn  
 Расширение модели исправности [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] — это просто создание собственных пользовательских политик и их размещение в категориях по типам наблюдаемых объектов.  После изменения некоторых параметров панель мониторинга AlwaysOn автоматически вычисляет собственные, определяемые пользователем политики, а также стандартные политики AlwaysOn.  
  
 Определяемая пользователем политика может использовать доступные аспекты управления на основе политик, включая те из них, которые применяются в стандартных политиках AlwaysOn (см. подраздел [Стандартные политики и проблемы](#Always OnPBM)выше в этом разделе). Аспект сервера содержит следующие свойства для наблюдения за исправностью [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]: (**IsHadrEnabled** и **HadrManagerStatus**). Аспект сервера содержит также следующие свойства политики для наблюдения за конфигурацией кластера WSFC. **ClusterQuorumType** и **ClusterQuorumState**.  
  
 Дополнительные сведения см. в записи [Модель исправности AlwaysOn, часть 2. Расширение модели исправности](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model) (в блоге SQL Server AlwaysOn Team; на английском языке).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Использование политик AlwaysOn для просмотра состояния группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
-   [Аварийное восстановление WSFC через принудительный кворум (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
  
-   [Принудительный запуск кластера WSFC без кворума](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
-   [Выполнение принудительного перехода на другой ресурс вручную для группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Устранение неполадок с операцией добавления файла, давшей сбой (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   [Модель исправности AlwaysOn, часть 1. Архитектура модели исправности](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)  
  
-   [Модель исправности AlwaysOn, часть 2. Расширение модели исправности](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)  
  
-   [Руководство по решениям режима AlwaysOn в Microsoft SQL Server для обеспечения высокой доступности и аварийного восстановления](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>См. также:  
 [Группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Администрирование группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Отслеживание групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
