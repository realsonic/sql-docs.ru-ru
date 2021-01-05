---
title: Настройка HealthCheckTimeout для группы доступности
description: Настройка параметра HealthCheckTimeout для группы доступности AlwaysOn, указывающего время, которое библиотека DLL ресурсов SQL Server ожидает, прежде чем сообщать об отсутствии ответа.
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 3bbeb979-e6fc-4184-ad6e-cca62108de74
author: cawrites
ms.author: chadam
ms.openlocfilehash: f1b7fb37f0f72475c86bbaa256eeb3634f9ccb4b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642739"
---
# <a name="configure-healthchecktimeout-property-settings"></a>Настройка параметров свойства HealthCheckTimeout
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Параметр HealthCheckTimeout используется для задания временного интервала (в миллисекундах) ожидания библиотекой ресурсов SQL Server данных, возвращаемых хранимой процедурой [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) , до появления сообщения о том, что экземпляр отказоустойчивого кластера AlwaysOn SQL Server (FCI) не отвечает. Изменения, внесенные в параметры времени ожидания, вступают в силу немедленно и не требуют перезапуска ресурса SQL Server.  
  
-   **Перед началом работы**  [Ограничения](#Limits), [Безопасность](#Security)  
  
-   **Настройка параметра HealthCheckTimeout с помощью:**  [PowerShell](#PowerShellProcedure), [диспетчера отказоустойчивости кластеров](#WSFC), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Limits"></a> Ограничения  
 Значение по умолчанию для этого свойства составляет 30 000 миллисекунд (30 секунд). Минимальное значение равно 15 000 миллисекундам (15 секундам).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требует разрешения ALTER SETTINGS и VIEW SERVER STATE.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование PowerShell  
  
##### <a name="to-configure-healthchecktimeout-settings"></a>Настройка параметров HealthCheckTimeout  
  
1.  Запустите повышенный режим Windows PowerShell с помощью команды **Запуск от имени администратора**.  
  
2.  Импортируйте модуль **FailoverClusters** для включения командлетов кластера.  
  
3.  C помощью командлета **Get-ClusterResource** найдите ресурс [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , а затем используйте командлет **Set-ClusterParameter** , чтобы настроить свойство **HealthCheckTimeout** для экземпляра отказоустойчивого кластера.  
  
> [!TIP]  
>  Каждый раз при открытии нового окна PowerShell потребуется импортировать модуль **FailoverClusters** .  
  
### <a name="example-powershell"></a>Пример (PowerShell)  
 В следующем примере демонстрируется изменение параметра HealthCheckTimeout ресурса [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] «`SQL Server (INST1)`» на 60 000 миллисекунд.  
  
```powershell  
Import-Module FailoverClusters  
  
$fci = "SQL Server (INST1)"  
Get-ClusterResource $fci | Set-ClusterParameter HealthCheckTimeout 60000  
  
```  
  
### <a name="related-content-powershell"></a>См. также (PowerShell)  
  
-   [Кластеризация и высокая доступность](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) (блог группы отказоустойчивой кластеризации и балансировки сетевой нагрузки)  
  
-   [Приступая к работе с Windows PowerShell в отказоустойчивом кластере](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Команды ресурса кластера и соответствующие командлеты Windows PowerShell](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> Использование оснастки «Диспетчер отказоустойчивости кластеров»  
 **Для настройки параметров HealthCheckTimeout**  
  
1.  Откройте оснастку «Диспетчер отказоустойчивости кластеров»  
  
2.  Раскройте узел **Службы и приложения** и выберите требуемый кластер FCI.  
  
3.  Щелкните правой кнопкой мыши **Ресурс SQL Server** в разделе **Другие ресурсы** и выберите из контекстного меню пункт **Свойства** . Откроется диалоговое окно **Свойства** ресурсов SQL Server.  
  
4.  Перейдите на вкладку **Свойства** , введите желаемое значение свойства **HealthCheckTimeout** и нажмите кнопку **ОК** , чтобы применить изменение.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 С помощью инструкции [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] вы можете задать значение свойства HealthCheckTimeOut.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 В следующем примере для параметра HealthCheckTimeout устанавливается значение, равное 15 000 миллисекунд (15 секунд).  
  
```  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
## <a name="see-also"></a>См. также:  
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
