---
title: Удаление вторичной реплики из группы доступности
description: 'Инструкции по удалению вторичной реплики из группы доступности Always On с помощью Transact-SQL (T-SQL), PowerShell или SQL Server Management Studio. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removesecondaryar.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 35ddc8b6-3e7c-4417-9a0a-d4987a09ddf7
author: cawrites
ms.author: chadam
ms.openlocfilehash: b128836eafee3d3ae0e7416927f6a3a1993698cf
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642501"
---
# <a name="remove-a-secondary-replica-from-an-availability-group-sql-server"></a>Удаление вторичной реплики из группы доступности (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается удаление вторичной реплики из группы доступности AlwaysOn с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]или PowerShell в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
 
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Эта задача поддерживается только в первичной реплике.    
-   Из группы доступности может быть удалена только вторичная реплика.  
  
## <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Необходимо иметь подключение к экземпляру сервера, на котором размещена первичная реплика группы доступности.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER AVAILABILITY GROUP для группы доступности, разрешение CONTROL AVAILABILITY GROUP, разрешение ALTER ANY AVAILABILITY GROUP или разрешение CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 **Удаление вторичной реплики**  
  
1.  В обозревателе объектов подключитесь к экземпляру сервера, на котором размещена первичная реплика, и разверните дерево сервера.  
  
2.  Разверните узел **Высокий уровень доступности AlwaysOn** и узел **Группы доступности** .  
  
3.  Выберите группу доступности и разверните узел **Реплики доступности** .  
  
4.  Этот шаг имеет следующие различия в зависимости от того, удаляется одна или несколько реплик.  
  
    -   Чтобы удалить несколько реплик, используйте область **Подробности** обозревателя объектов, чтобы просмотреть и выбрать реплики для удаления. Дополнительные сведения см. в разделе [Использование раздела "Подробности обозревателя объектов" для мониторинга групп доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Чтобы удалить одну реплику, выберите ее на панели **Обозреватель объектов** или на панели **Подробности обозревателя объектов** .  
  
5.  Щелкните правой кнопкой мыши выбранную вторичную реплику или реплики и выберите в контекстном меню команду **Удалить из группы доступности** .  
  
6.  Чтобы удалить все перечисленные вторичные реплики, в диалоговом окне **Удаление вторичных реплик из группы доступности** нажмите кнопку **ОК**. Если все перечисленные группы доступности удалять не нужно, нажмите кнопку **Отмена**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Удаление вторичной реплики**  
  
1.  Подключитесь к экземпляру сервера, на котором находится первичная реплика.  
  
2.  Инструкция [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) используется следующим образом:  
  
     ALTER AVAILABILITY GROUP *имя_группы* REMOVE REPLICA ON "*имя_экземпляра*" [,...*n*],  
  
     где *имя_группы* — имя группы доступности, а *имя_экземпляра* — экземпляр сервера, на котором размещена вторичная реплика.  
  
     В следующем примере удаляется вторичная реплика из группы доступности *MyAG* . Целевая вторичная реплика расположена на экземпляре сервера с именем *HADR_INSTANCE* на компьютере *COMPUTER02*.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE REPLICA ON 'COMPUTER02\HADR_INSTANCE';  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование PowerShell  
 **Удаление вторичной реплики**  
  
1.  Перейдите в каталог (**cd**) экземпляра сервера, в котором находится первичная реплика.  
  
2.  Используйте командлет **Remove-SqlAvailabilityReplica** .  
  
     Например, следующая команда удаляет реплику доступности на сервере `MyReplica` из группы доступности с именем `MyAg`.  Эта команда должна выполняться на экземпляре сервера, на котором размещена первичная реплика группы доступности.  
  
    ```  
    Remove-SqlAvailabilityReplica `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Чтобы просмотреть синтаксис командлета, воспользуйтесь командлетом **Get-Help** в среде PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Настройка и использование поставщика SQL Server PowerShell**  
  
-   [Поставщик SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-replica"></a><a name="PostBestPractices"></a> Дальнейшие действия. После удаления вторичной реплики  
 Если была выбрана реплика, которая в данный момент недоступна, ее удаление произойдет в момент перехода в режим «в сети».  
  
 Удаление реплики прекращает поступление в нее данных. После того, как вторичная реплика подтверждает, что она была удалена из глобального хранилища, реплика удаляет параметры группы доступности из своих баз данных, которые остаются на экземпляре локального сервера в состоянии RECOVERING.  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Добавление вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)   
 [Удаление группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
