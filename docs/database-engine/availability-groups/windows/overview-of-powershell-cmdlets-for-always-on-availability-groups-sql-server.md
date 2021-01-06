---
title: Командлеты PowerShell для групп доступности
description: 'Справочник по различным командлетам PowerShell, доступным для управления группами доступности Always On. '
ms.custom: seo-lt-2019
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], PowerShell cmdlets
- Availability Groups [SQL Server], about
- PowerShell [SQL Server], cmdlets
ms.assetid: b3fef0d5-b6d7-4386-a0f0-d06c165ad4de
author: cawrites
ms.author: chadam
ms.openlocfilehash: ae6462d46760f1a5f58f01a2170470e2d4ea7fac
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643503"
---
# <a name="overview-of-powershell-cmdlets-for-always-on-availability-groups"></a>Обзор командлетов PowerShell для групп доступности AlwaysOn
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] PowerShell ― это оболочка командной строки для задач и язык скриптов, разработанный специально для администрирования систем. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] содержит набор командлетов PowerShell для [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] , которые позволяют развертывать, управлять и отслеживать группы доступности, реплики доступности и базы данных доступности.  
  
> [!NOTE]  
>  Командлет PowerShell может завершиться после успешного инициирования действия. Это не означает, что предписанное действие, например отработка отказа для группы доступности, завершено. При создании скрипта для некой последовательности действий может оказаться необходимым проверить состояние действий и подождать из завершения.  
  
> [!NOTE]  
>  Список разделов электронной документации по [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] , в которых описано использование командлетов для выполнения задач [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , приведен в подразделе "Связанные задачи" в разделе [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="configuring-a-server-instance-for-always-on-availability-groups"></a><a name="ConfiguringServerInstance"></a> Настройка экземпляра сервера для групп доступности AlwaysOn  
  
|Командлеты|Description|Поддерживается на|  
|-------------|-----------------|------------------|
|[**Disable-SqlAlwaysOn**](/powershell/module/sqlserver/disable-sqlalwayson)|Отключает компонент [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] для экземпляра сервера.|Экземпляр сервера, указанный параметром **Path**, **InputObject** или параметром **Name** . (Необходим выпуск [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с поддержкой [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].)|  
|[**Enable-SqlAlwaysOn**](/powershell/module/sqlserver/enable-sqlalwayson)|Включает [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в экземпляре [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] , который поддерживает компонент [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Дополнительные сведения о поддержке [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в разделе [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).|Любой выпуск [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с поддержкой [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].|  
|[**New-SqlHadrEndPoint**](/powershell/module/sqlserver/new-sqlhadrendpoint)|Создает новую конечную точку зеркального отображения базы данных на экземпляре сервера. Эта конечная точка необходима для перемещения данных между базой данных-источником и базой данных-получателем.|Любой экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|[**Set-SqlHadrEndpoint**](/powershell/module/sqlserver/set-sqlhadrendpoint)|Изменяет свойства существующей конечной точки зеркального отображения базы данных, например имя, состояние и свойства проверки подлинности.|Экземпляр сервера, который поддерживает [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] и в котором отсутствует конечная точка зеркального отображения базы данных|  

  
##  <a name="backing-up-and-restoring-databases-and-transaction-logs"></a><a name="BnRcmdlets"></a> Резервное копирование и восстановление баз данных и журналов транзакций  
  
|Командлеты|Description|Поддерживается на|  
|-------------|-----------------|------------------|  
|[**Backup-SqlDatabase**](/powershell/module/sqlserver/backup-sqldatabase)|Создает резервную копию данных или журнала.|Любая база данных, находящаяся в режиме «в сети» (для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]база данных на том экземпляре сервера, на котором размещена первичная реплика)|  
|[**Restore-SqlDatabase**](/powershell/module/sqlserver/restore-sqldatabase)|Восстанавливает резервную копию.|Любой экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]экземпляр сервера, на котором размещена вторичная реплика)<br /><br />

  >[!Important]
  >При подготовке базы данных-получателя необходимо использовать параметр **-NoRecovery** в каждой команде **Restore-SqlDatabase**. 
  
 Дополнительные сведения об использовании этих командлетов для подготовки базы данных-получателя см. в разделе [Подготовка базы данных-получателя для присоединения к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
##  <a name="creating-and-managing-an-availability-group"></a><a name="DeployManageAGs"></a> Creating and Managing an Availability Group  
  
|Командлеты|Description|Поддерживается на|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroup**](/powershell/module/sqlserver/new-sqlavailabilitygroup)|Создает новую группу доступности.|Экземпляр сервера, в котором будет размещена первичная реплика|  
|[**Remove-SqlAvailabilityGroup**](/powershell/module/sqlserver/remove-sqlavailabilitygroup)|Удаляет группу доступности.|Экземпляр сервера с включенным компонентом HADR|  
|[**Set-SqlAvailabilityGroup**](/powershell/module/sqlserver/set-sqlavailabilitygroup)|Устанавливает свойства группы доступности; включение и отключение режимов «в сети» и «вне сети» группы доступности|Экземпляр сервера, в котором размещена первичная реплика|  
|[**Switch-SqlAvailabilityGroup**](/powershell/module/sqlserver/switch-sqlavailabilitygroup)|Запускает одну из следующих форм отработки отказа.<br /><br /> Принудительная отработка отказа для группы доступности (возможна потеря данных).<br /><br /> Переход на другой ресурс группы доступности вручную.|Экземпляр сервера, на котором размещается целевая вторичная реплика|  
  
##  <a name="creating-and-managing-an-availability-group-listener"></a><a name="AGlisteners"></a> Creating and Managing an Availability Group Listener  
  
|Командлет|Description|Поддерживается на|  
|------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/new-sqlavailabilitygrouplistener)|Создает прослушиватель группы доступности и привязывает его к существующей группе доступности.|Экземпляр сервера, в котором размещена первичная реплика|  
|[**Set-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/set-sqlavailabilitygrouplistener)|Изменяет порт существующего прослушивателя группы доступности.|Экземпляр сервера, в котором размещена первичная реплика|  
|[**Add-SqlAvailabilityGroupListenerStaticIp**](/powershell/module/sqlserver/add-sqlavailabilitygrouplistenerstaticip)|Добавляет статический IP-адрес в конфигурацию существующего прослушивателя группы доступности. IP-адрес может быть адресом IPv4 с подсетью или адресом IPv6.|Экземпляр сервера, в котором размещена первичная реплика|  
  
##  <a name="creating-and-managing-an-availability-replica"></a><a name="DeployManageARs"></a> Creating and Managing an Availability Replica  
  
|Командлеты|Description|Поддерживается на|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityReplica**](/powershell/module/sqlserver/new-sqlavailabilityreplica)|Создает новую реплику доступности. Вы можете использовать параметр **-AsTemplate** для создания в памяти объекта реплики доступности для каждой новой реплики доступности.|Экземпляр сервера, в котором размещена первичная реплика|  
|[**Join-SqlAvailabilityGroup**](/powershell/module/sqlserver/join-sqlavailabilitygroup)|Присоединяет вторичную реплику к группе доступности.|Экземпляр сервера, в котором размещена вторичная реплика|  
|[**Remove-SqlAvailabilityReplica**](/powershell/module/sqlserver/remove-sqlavailabilityreplica)|Удаляет реплику доступности.|Экземпляр сервера, в котором размещена первичная реплика|  
|[**Set-SqlAvailabilityReplica**](/powershell/module/sqlserver/set-sqlavailabilityreplica)|Устанавливает свойства реплики доступности.|Экземпляр сервера, в котором размещена первичная реплика|  
  
##  <a name="adding-and-managing-an-availability-database"></a><a name="DeployManageDbs"></a> Adding and Managing an Availability Database  
  
|Командлеты|Description|Поддерживается на|  
|-------------|-----------------|------------------|  
|[**Add-SqlAvailabilityDatabase**](/powershell/module/sqlserver/add-sqlavailabilitydatabase)|Добавляет базу данных в группу доступности на первичной реплике.<br /><br /> Присоединяет вторичную базу данных к группе доступности на вторичной реплике.|Любой экземпляр сервера, на котором размещается реплика доступности (поведение отличается для первичных и вторичных реплик)|  
|[**Remove-SqlAvailabilityDatabase**](/powershell/module/sqlserver/remove-sqlavailabilitydatabase)|Удаляет базу данных из группы доступности на первичной реплике.<br /><br /> Удаляет локальную базу данных-получатель из локальной вторичной реплики на вторичной реплике.|Любой экземпляр сервера, на котором размещается реплика доступности (поведение отличается для первичных и вторичных реплик)|  
|[**Resume-SqlAvailabilityDatabase**](/powershell/module/sqlserver/resume-sqlavailabilitydatabase)|Возобновляет перемещение данных для приостановленной базы данных доступности.|Экземпляр сервера, на котором была приостановлена база данных.|  
|[**Suspend-SqlAvailabilityDatabase**](/powershell/module/sqlserver/suspend-sqlavailabilitydatabase)|Приостанавливает перемещение данных в любой базе данных доступности.|Любой экземпляр сервера, на котором размещена реплика доступности.|  
  
##  <a name="monitoring-availability-group-health"></a><a name="MonitorTblshtAGs"></a> Monitoring Availability Group Health  
 Следующие командлеты [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] позволяют отслеживать работоспособность группы доступности, ее реплик и баз данных.  
  
> [!IMPORTANT]  
>  Для выполнения этих командлетов необходимо иметь разрешения CONNECT, VIEW SERVER STATE и VIEW ANY DEFINITION.  
  
|Командлет|Description|Поддерживается на|  
|------------|-----------------|------------------|  
|[**Test-SqlAvailabilityGroup**](/powershell/module/sqlserver/test-sqlavailabilitygroup)|Оценивает работоспособность группы доступности при помощи оценки состояния политик управления SQL Server.|Любой экземпляр сервера, на котором размещена реплика доступности.*|  
|[**Test-SqlAvailabilityReplica**](/powershell/module/sqlserver/test-sqlavailabilityreplica)|Оценивает работоспособность реплик доступности при помощи оценки состояния политик управления SQL Server.|Любой экземпляр сервера, на котором размещена реплика доступности.*|  
|[**Test-SqlDatabaseReplicaState**](/powershell/module/sqlserver/test-sqldatabasereplicastate)|Оценивает работоспособность базы данных доступности на всех присоединенных репликах доступности при помощи оценки состояния политик управления SQL Server.|Любой экземпляр сервера, на котором размещена реплика доступности.*|  
  
 \* Чтобы просмотреть сведения обо всех репликах доступности в группе доступности, используйте экземпляр сервера, на котором размещена первичная реплика.  
  
 Дополнительные сведения см. в разделе [Использование политик AlwaysOn для определения работоспособности группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Получение справок по SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
