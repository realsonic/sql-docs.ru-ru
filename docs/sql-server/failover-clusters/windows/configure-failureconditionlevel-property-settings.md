---
title: Настройка параметров свойства FailureConditionLevel
description: Свойство FailureConditionLevel служит для задания для экземпляра отказоустойчивого кластера FCI в группе AlwaysOn условий отработки отказа или перезапуска.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 513dd179-9a46-46da-9fdd-7632cf6d0816
author: cawrites
ms.author: chadam
ms.openlocfilehash: ecb034767917eac38fa2a161fb749ca8d438b995
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642819"
---
# <a name="configure-failureconditionlevel-property-settings"></a>Настройка параметров свойства FailureConditionLevel
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Свойство FailureConditionLevel служит для задания для экземпляра отказоустойчивого кластера FCI в группе AlwaysOn условий отработки отказа или перезапуска. Изменения значения этого свойства вступают в силу немедленно, без перезапуска службы отказоустойчивого кластера Windows Server или ресурса отказоустойчивого кластера SQL Server.  
  
-   **Перед началом работы**  [Параметры свойства FailureConditionLevel](#Restrictions), [Безопасность](#Security)  
  
-   **Настройка параметров свойства FailureConditionLevel с помощью** [PowerShell](#PowerShellProcedure), [диспетчера отказоустойчивого кластера](#WSFC), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="failureconditionlevel-property-settings"></a><a name="Restrictions"></a> параметры свойства FailureConditionLevel  
 Условия сбоя устанавливаются по прогрессирующей шкале. Каждый уровень, с 1 по 5, включает все условия предыдущих уровней, а также собственные дополнительные условия. Это означает, что с каждым уровнем вероятность отработки отказа или перезапуска повышается.  Дополнительные сведения см. в подразделе «Определение сбоев» раздела [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md) .  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требует разрешения ALTER SETTINGS и VIEW SERVER STATE.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование PowerShell  
  
##### <a name="to-configure-failureconditionlevel-settings"></a>Настройка параметров свойства FailureConditionLevel  
  
1.  Запустите повышенный режим Windows PowerShell с помощью команды **Запуск от имени администратора**.  
  
2.  Импортируйте модуль **FailoverClusters** для включения командлетов кластера.  
  
3.  С помощью командлета **Get-ClusterResource** найдите ресурс [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , а затем с помощью командлета **Set-ClusterParameter** задайте свойство **FailureConditionLevel** для экземпляра отказоустойчивого кластера.  
  
> [!TIP]  
>  Каждый раз при открытии нового окна PowerShell потребуется импортировать модуль **FailoverClusters** .  
  
### <a name="example-powershell"></a>Пример (PowerShell)  
 В следующем примере параметр FailureConditionLevel экземпляра в ресурсе [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] «`SQL Server (INST1)`» изменяется на обработку отказа или перезапуск при критических ошибках сервера.  
  
```powershell  
Import-Module FailoverClusters  
  
$fci = "SQL Server (INST1)"  
Get-ClusterResource $fci | Set-ClusterParameter FailureConditionLevel 3  
  
```  
  
### <a name="related-content-powershell"></a>См. также (PowerShell)  
  
-   [Кластеризация и высокая доступность](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) (блог группы отказоустойчивой кластеризации и балансировки сетевой нагрузки)  
  
-   [Приступая к работе с Windows PowerShell в отказоустойчивом кластере](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Команды ресурса кластера и соответствующие командлеты Windows PowerShell](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> Использование оснастки «Диспетчер отказоустойчивости кластеров»  
 **Настройка параметров свойств FailureConditionLevel.**  
  
1.  Откройте оснастку «Диспетчер отказоустойчивости кластеров»  
  
2.  Разверните узел **Службы и приложения** и выберите требуемый кластер FCI.  
  
3.  Щелкните правой кнопкой мыши **Ресурс SQL Server** в разделе **Другие ресурсы** и выберите пункт **Свойства** . Откроется диалоговое окно **Свойства** ресурсов SQL Server.  
  
4.  Перейдите на вкладку **Свойства** , введите необходимое значение свойства **FaliureConditionLevel** и нажмите кнопку **ОК** , чтобы применить изменение.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Настройка параметров свойств FailureConditionLevel.**  
  
 С помощью инструкции [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] можно задать значение свойства FailureConditionLevel.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 Следующий пример назначает свойству FailureConditionLevel значение 0, которое указывает, что при любых условиях сбоя отработка отказа или перезапуск не будет выполняться автоматически.  
  
```  
ALTER SERVER CONFIGURATION SET FAILOVER CLUSTER PROPERTY FailureConditionLevel = 0;  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)   
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
