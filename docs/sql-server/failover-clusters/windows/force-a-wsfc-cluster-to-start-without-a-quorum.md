---
title: Принудительный запуск кластера WSFC без кворума
description: Принудительный запуск узла отказоустойчивого кластера Windows Server без кворума, который может потребоваться для восстановления данных и повторного установления высокой доступности.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: 4a121375-7424-4444-b876-baefa8fe9015
author: cawrites
ms.author: chadam
ms.openlocfilehash: 908aa73d20ce939696124fee2f92eb87c9b8bec8
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642769"
---
# <a name="force-a-wsfc-cluster-to-start-without-a-quorum"></a>Принудительный запуск кластера WSFC без кворума
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описан порядок принудительного запуска узла отказоустойчивого кластера Windows Server (WSFC) без кворума.  Это может потребоваться в случае аварийного восстановления, в сценариях с несколькими подсетями и восстановлением данных и высокой доступности экземпляров отказоустойчивого кластера [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Перед началом работы:**  [Рекомендации](#Recommendations), [Безопасность](#Security)  
  
-   **Принудительный запуск кластера без кворума с использованием:**  [диспетчер отказоустойчивости кластеров](#FailoverClusterManagerProcedure), [Powershell](#PowerShellProcedure), [Net.exe](#CommandPromptProcedure)  
  
-   **Дальнейшие действия.**  [Дальнейшие действия: после принудительного запуска кластера без кворума](#FollowUp)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> Перед началом работы  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
 Процедуры в этом разделе (кроме указанных явно случаев) должны успешно действовать при выполнении на любом узле отказоустойчивого кластера WSFC.  Однако можно получить лучшие результаты и избежать проблем с сетью при выполнении этих действий с узла, который планируется запускать принудительно без кворума.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
 Пользователь должен входить в учетную запись домена, которая является членом локальной группы администраторов, на каждом узле кластера WSFC.  
  
##  <a name="using-failover-cluster-manager"></a><a name="FailoverClusterManagerProcedure"></a> Использование диспетчера отказоустойчивого кластера.  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Принудительный запуск кластера без кворума  
  
1.  Откройте диспетчер отказоустойчивого кластера и подключитесь к требуемому узлу кластера в режиме «в сети».  
  
2.  На панели **Действия** выберите **Принудительный запуск кластера**, а затем — **Да, запустить кластер принудительно**.  
  
3.  На левой панели в дереве **Диспетчер отказоустойчивого кластера** щелкните имя кластера.  
  
4.  В сводной панели подтвердите, что текущим значением параметра **Конфигурация кворума** является:  **Внимание: Кластер работает в состоянии ForceQuorum**.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование Powershell  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Принудительный запуск кластера без кворума  
  
1.  Запустите повышенный режим Windows PowerShell с помощью команды **Запуск от имени администратора**.  
  
2.  Импортируйте модуль `FailoverClusters` для включения командлетов кластера.  
  
3.  С помощью `Stop-ClusterNode` обеспечьте остановку службы кластеров.  
  
4.  Принудительный запуск службы кластеров с помощью `Start-ClusterNode` с `-FixQuorum` .  
  
5.  С помощью `Get-ClusterNode` с `-Propery NodeWieght = 1` установите значение, которое гарантирует для узла право голоса в кворуме.  
  
6.  Выведите свойства узла кластера в удобном для чтения формате.  
  
### <a name="example-powershell"></a>Пример (Powershell)  
 В следующем примере происходит принудительный запуск службы кластеров узла OnSrv02 AlwaysOn без кворума: задается значение `NodeWeight = 1`, затем перечисляется состояние узла кластера с вновь запущенного узла.  
  
```powershell  
Import-Module FailoverClusters  
  
$node = "Always OnSrv02"  
Stop-ClusterNode -Name $node  
Start-ClusterNode -Name $node -FixQuorum  
  
(Get-ClusterNode $node).NodeWeight = 1  
  
$nodes = Get-ClusterNode -Cluster $node  
$nodes | Format-Table -property NodeName, State, NodeWeight  
  
```  
  
##  <a name="using-netexe"></a><a name="CommandPromptProcedure"></a> Использование Net.exe  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Принудительный запуск кластера без кворума  
  
1.  С помощью удаленного рабочего стола подключитесь к нужному узлу кластера в режиме «в сети».  
  
2.  Запустите повышенный режим командной строки с помощью команды **Запуск от имени администратора**.  
  
3.  С помощью **net.exe** остановите локальную службу кластеров.  
  
4.  С помощью **net.exe** с `/forcequorum` принудительно запустите локальную службу кластеров.  
  
### <a name="example-netexe"></a>Пример (Net.exe)  
 В следующем примере происходит принудительный запуск службы кластеров узла без кворума: задается значение `NodeWeight = 1`, затем перечисляется состояние узла кластера с вновь запущенного узла.  
  
```ms-dos  
net.exe stop clussvc  
net.exe start clussvc /forcequorum  
```  
  
##  <a name="follow-up-after-forcing-cluster-to-start-without-a-quorum"></a><a name="FollowUp"></a> Дальнейшие действия. После принудительного запуска кластера без кворума  
  
-   Необходимо повторно оценить и настроить значения параметров NodeWeight для правильного построения нового кворума, прежде чем переключать другие узлы обратно в режим «в сети». В противном случае кластер может снова вернуться в режим «вне сети».  
  
     Дополнительные сведения см. в статье [Режимы кворума и конфигурация голосования WSFC (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
-   Процедуры в этом разделе представляют только один шаг в возвращении отказоустойчивого кластера Windows обратно в режим «в сети» в случае незапланированного сбоя кворума.  Кроме того, могут потребоваться дополнительные действия, позволяющие предотвратить помехи со стороны других узлов отказоустойчивого кластера WSFC в настройке нового кворума.  
  
-   Другие функции [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , например [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], зеркальное отображение базы данных и доставка журналов, могут также требовать последующих действий по восстановлению данных и полному восстановлению высокой доступности.  
  
     **Дополнительные сведения см. в следующих разделах:**  
  
     [Выполнение принудительного перехода на другой ресурс вручную для группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
     [Принудительный запуск службы в сеансе зеркального отображения базы данных (Transact-SQL)](../../../database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
  
     [Переход на вторичный сервер доставки журналов (SQL Server)](../../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   [Просмотр событий и журналов для отказоустойчивого кластера](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Командлет Get-ClusterLog отказоустойчивого кластера](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>См. также:  
 [Аварийное восстановление WSFC через принудительный кворум (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Настройка параметров NodeWeight кворума кластера](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)   
 [Командлеты отказоустойчивого кластера в Windows PowerShell по выполняемым задачам](https://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
