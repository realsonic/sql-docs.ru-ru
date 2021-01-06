---
title: Присоединение вторичной реплики к группе доступности
description: Инструкции по присоединению вторичной реплики к группе доступности Always On с помощью Transact-SQL (T-SQL), PowerShell или SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.joinreplica.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], joining
- Availability Groups [SQL Server], configuring
ms.assetid: e5bd2489-097a-490e-8ea1-34fe48378ad1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9ada906d271130f0160e4bf8ce8f88d88b91b2e0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644222"
---
# <a name="join-a-secondary-replica-to-an-always-on-availability-group"></a>Присоединение вторичной реплики к группе доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается присоединение вторичной реплики к группе доступности AlwaysOn с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]или PowerShell в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. После добавления вторичной реплики в группу доступности AlwaysOn необходимо присоединить эту реплику к группе доступности. Операция присоединения реплики должна быть выполнена на экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на котором находится вторичная реплика.  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Первичная реплика группы доступности должна быть в сети.    
-   Пользователь должен быть подключен к экземпляру сервера, на котором находится дополнительная реплика, которая еще не была присоединена к группе доступности.    
-   Экземпляр локального сервера должен иметь возможность подключения к конечной точке зеркального отображения базы данных на экземпляре сервера, где находится первичная реплика.  
  
> [!IMPORTANT]  
>  Если какое-либо из предварительных условий не выполняется, происходит сбой операции соединения. После неудачной попытки соединения может быть необходимо подключиться к экземпляру сервера, на котором содержится первичная реплика, чтобы удалить и повторно добавить вторичную реплику, прежде чем можно будет выполнить соединение с группой доступности. Дополнительные сведения см. в разделах [Удаление вторичной реплики из группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) и [Добавление вторичной реплики к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER AVAILABILITY GROUP для группы доступности, разрешение CONTROL AVAILABILITY GROUP, разрешение ALTER ANY AVAILABILITY GROUP или разрешение CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 **Присоединение реплики доступности к группе доступности**  
  
1.  В обозревателе объектов подключитесь к экземпляру сервера, на котором размещена вторичная реплика, и щелкните имя сервера, чтобы развернуть его дерево.  
  
2.  Разверните узел **Высокий уровень доступности AlwaysOn** и узел **Группы доступности** .  
  
3.  Выберите группу доступности вторичной реплики, к которой выполнено подключение.  
  
4.  Щелкните правой кнопкой мыши вторичную реплику и выберите пункт **Включить в группу доступности**.  
  
5.  Откроется диалоговое окно **Присоединить реплику к группе доступности** .  
  
6.  Чтобы присоединить вторичную реплику к группе доступности, нажмите кнопку **ОК**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Присоединение реплики доступности к группе доступности**  
  
1.  Подключитесь к экземпляру сервера, на котором находится дополнительная реплика.  
  
2.  Инструкция [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) используется следующим образом:  
  
     ALTER AVAILABILITY GROUP *имя_группы* JOIN  
  
     где *имя_группы* — это имя группы доступности.  
  
     В следующем примере объединяются дополнительная реплика и группа доступности `MyAG`.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG JOIN;  
    ```  
  
    > [!NOTE]  
    >  Пример использования инструкции [!INCLUDE[tsql](../../../includes/tsql-md.md)] в контексте см. в статье [Создание группы доступности (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование PowerShell  
 **Присоединение реплики доступности к группе доступности**  
  
 В поставщике [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell:  
  
1.  Перейдите в каталог (**cd**) экземпляра сервера, на котором размещается вторичная реплика.  
  
2.  Присоедините вторичную реплику к группе доступности, выполнив командлет **Join-SqlAvailabilityGroup** с именем группы доступности.  
  
     Например, следующая команда присоединяет вторичную реплику, размещенную на экземпляре сервера по указанному пути, к группе доступности `MyAg`.  На этом экземпляре сервера должна быть размещена вторичная реплика этой группы доступности.  
  
    ```  
    Join-SqlAvailabilityGroup -Path SQLSERVER:\SQL\SecondaryServer\InstanceName -Name 'MyAg'  
    ```  
  
    > [!NOTE]  
    >  Чтобы просмотреть синтаксис командлета, воспользуйтесь командлетом **Get-Help** в среде PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Настройка и использование поставщика SQL Server PowerShell**  
  
-   [Поставщик SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-configure-secondary-databases"></a><a name="FollowUp"></a> Дальнейшие действия. Настройка баз данных-получателей  
 Для каждой базы данных в группе доступности необходимо настроить базу данных-получатель на экземпляре сервера, где находится дополнительная реплика. Настроить базы данных-получатели до или после присоединения дополнительной реплики к группе доступности можно следующим образом.  
  
1.  Восстановите последние базы данных и резервные копии журналов каждой базы данных-источника на экземпляр сервера, где находится вторичная реплика, используя инструкцию RESTORE WITH NORECOVERY для каждой операции восстановления. Дополнительные сведения см. в статье [Ручная подготовка базы данных-получателя для присоединения к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
2.  Присоедините каждую базу данных-получатель к группе доступности. Дополнительные сведения см. в статье [Присоединение базы данных-получателя к группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>См. также:  
 [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Поиск и устранение неисправностей конфигурации групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
