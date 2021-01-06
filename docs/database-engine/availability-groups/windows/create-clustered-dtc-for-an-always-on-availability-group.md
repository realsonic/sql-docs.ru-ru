---
title: Создание кластеризованного ресурса DTC для группы доступности
description: В этом разделе рассматривается полная конфигурация кластеризованного ресурса DTC для группы доступности AlwaysOn SQL Server.
ms.custom: seodec18
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 0e332aa4-2c48-4bc4-a404-b65735a02cea
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c976a81277f18f89486343af32c96509f8383fda
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643281"
---
# <a name="create-clustered-dtc-resource-for-an-always-on-availability-group"></a>Создание кластеризованного ресурса DTC для группы доступности Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

В этом разделе рассматривается полная конфигурация кластеризованного ресурса DTC для группы доступности AlwaysOn SQL Server. Выполнение полной конфигурации может занять до одного часа. 

В рамках этого пошагового руководства создается кластеризованный ресурс DTC и группы доступности SQL Server в соответствии с требованиями из раздела [Кластеризация DTC для групп доступности SQL Server](../../../database-engine/availability-groups/windows/cluster-dtc-for-sql-server-2016-availability-groups.md).

В руководстве используются скрипты PowerShell и Transact-SQL (T-SQL).  Для работы многих скриптов T-SQL требуется включить **Режим SQLCMD** .  Дополнительные сведения о **режиме SQLCMD** см. в разделе [Включение режима скриптов SQLCMD в редакторе запросов](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md).  Требуется импортировать модуль PowerShell **FailoverClusters** .  Дополнительные сведения об импорте модуля PowerShell см. в разделе [Импорт модуля PowerShell](/powershell/scripting/developer/module/importing-a-powershell-module).  Это пошаговое руководство основано на следующих допущениях.
- Выполнены все требования из раздела [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
- Используется домен `contoso.lab`.
- Пользователь имеет разрешение на создание объектов-компьютеров в том подразделении, где будет создан ресурс сетевого имени DTC.
- Пользователь является пользователем домена с правами администратора на всех узлах в кластере.
- Создана общая папка `sqlbackups` для резервных копий.
- Экземпляры SQL Server по умолчанию называются `SQLNODE1` и `SQLNODE2`.
- Одна и та же учетная запись службы используется для всех экземпляров SQL Server.
- Пользователь является членом предопределенной роли sysadmin SQL Server на всех экземплярах SQL Server.
- Результат по умолчанию для транзакций, которые DTC не удается разрешить, будет иметь значение `presume commit`.
- Зеркальная конечная точка будет использовать порт `5022`.
- Другие группы доступности или кластеризованные ресурсы DTC отсутствуют.
- Сведения о кластере (существующий):
  - Имя: `Cluster`
  - Сетевое имя: `Cluster Network 1`
  - Узлы: `SQLNODE1, SQLNODE2`
  - Общее хранилище: `Cluster Disk 3` (во владении `SQLNODE1`)
- Сведения о кластере (создаваемый):
  - Ресурс сетевого имени: `DTCnet1`
  - Ресурс сетевого имени DTC: `DTC1`
  - Ресурс физического диска DTC: `DTCDisk1`
  - Ресурс подсети и IP-адреса DTC: `192.168.2.54`, `255.255.255.0`
  - Ресурс IP-адреса DTC: `DTCIP1`

## <a name="1-check-operating-system"></a>1. Проверка операционной системы
Для поддерживаемых распределенных транзакций [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] должен выполняться в Windows Server 2016 или Windows Server 2012 R2.  Для Windows Server 2012 R2 необходимо установить обновление KB3090973, доступное по адресу [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  Этот скрипт проверяет версию операционной системы и потребность в установке исправления 3090973.  Выполните приведенный ниже скрипт PowerShell в `SQLNODE1`.

```powershell  
# A few OS checks

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {

    # At least 2012 R2
    $os = (Get-WmiObject -class Win32_OperatingSystem -ComputerName $node).caption;
    IF($os -like "*2012 R2*" -or $os -like "*2016*")
    {
        Write-Host "$os is supported on $node.";
    }
    ELSE
    {
        Write-Host "STOP!  $os is not supported on $node.";
    }
    
    # KB 3090973
    IF($os -like "*2012 R2*")
    {
        $kb = Get-Hotfix -ComputerName $node | Where {$_.HotFixID -eq 'KB3090973'};
        IF($kb)
        {
            Write-Host "KB3090973 is installed on $node."
        }
        ELSE
        {
            Write-Host "HotFixID KB3090973 must be applied on $node.  See https://support.microsoft.com/kb/3090973 for additional information and to download the hotfix.";
        }
    }
    ELSE
    {
        Write-Host "KB3090973 is not applicable to $os on $node."
    }
}
```  
## <a name="2---configure-firewall-rules"></a>2.   Настройка правил брандмауэра
Этот скрипт настроит брандмауэр так, чтобы разрешить трафик DTC в каждом SQL Server, где размещается реплика группы доступности, а также на любом другом сервере, участвующем в распределенной транзакции.  Кроме того, скрипт настроит брандмауэр для разрешения подключений к конечной точке зеркального отображения базы данных.  Выполните приведенный ниже скрипт PowerShell в `SQLNODE1`.

```powershell  
# Configure Firewall

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {
    Get-NetFirewallRule -CimSession $node -DisplayGroup 'Distributed Transaction Coordinator' -Enabled False -ErrorAction SilentlyContinue | Enable-NetFirewallRule;
    New-NetFirewallRule -CimSession $node -DisplayName 'SQL Server Mirroring' -Description 'Port 5022 for SQL Server Mirroring' -Action Allow -Direction Inbound -Protocol TCP -LocalPort 5022 -RemotePort Any -LocalAddress Any -RemoteAddress Any;
    };
```  
## <a name="3--configure-in-doubt-xact-resolution"></a>3.  Настройка **in-doubt xact resolution** 
Этот скрипт настроит параметр конфигурации сервера **in-doubt xact resolution**, чтобы предположить фиксацию для сомнительных транзакций.  Запустите следующий скрипт T-SQL в SQL Server Management Studio (SSMS) для `SQLNODE1` в **режиме SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Configure in-doubt xact resolution on all SQL Server instances to presume commit
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------
```

## <a name="4-create-test-databases"></a>4. Создание баз данных проверки
Скрипт создаст базу данных с именем `AG1` на `SQLNODE1` и базу данных с именем `dtcDemoAG1` на `SQLNODE2`.  Запустите следующий скрипт T-SQL в SSMS для `SQLNODE1` в **режиме SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- On SQLNODE1 
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'AG1')
BEGIN
    DROP DATABASE AG1;
END
GO

CREATE DATABASE AG1;
ALTER DATABASE AG1 SET RECOVERY FULL WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::AG1 to sa;
GO

USE AG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );

INSERT Names
VALUES ('AG1', GETDATE());
GO


-- Against SQNODE2
:connect SQLNODE2
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'dtcDemoAG1')
BEGIN
    DROP DATABASE dtcDemoAG1;
END
GO

CREATE DATABASE dtcDemoAG1;
ALTER DATABASE dtcDemoAG1 SET RECOVERY SIMPLE WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::dtcDemoAG1 to sa;
GO

USE dtcDemoAG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );
GO    
----------------------------------------------------------------
```
## <a name="5---create-endpoints"></a>5.   Создание конечных точек
Этот скрипт создаст конечную точку с именем `AG1_endpoint`, которая прослушивает TCP-порт `5022`.  Запустите следующий скрипт T-SQL в SSMS для `SQLNODE1` в **режиме SQLCMD**.

```sql  
/**********************************************
Execute on SQLNODE1 in SQLCMD mode
**********************************************/

-- Create endpoint on server instance that hosts the primary replica:
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------
```

## <a name="6---prepare-databases-for-availability-group"></a>6.   Подготовка баз данных для групп доступности
Скрипт выполнит резервное копирование `AG1` на `SQLNODE1` и восстановить ее в `SQLNODE2`.  Запустите следующий скрипт T-SQL в SSMS для `SQLNODE1` в **режиме SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Backup database
BACKUP DATABASE AG1 
TO DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH FORMAT, STATS = 10;

-- Backup transaction log
BACKUP LOG AG1
TO DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH FORMAT, STATS = 10;
GO


-- Restore database and logs on secondary WITH NORECOVERY
:connect SQLNODE2
USE [master]
RESTORE DATABASE AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH NORECOVERY, STATS = 10;
GO
```

## <a name="7---create-availability-group"></a>7.   Создание группы доступности
[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] необходимо создать с помощью команды **CREATE AVAILABILITY GROUP** и предложения **WITH DTC_SUPPORT = PER_DB**.  Сейчас невозможно изменить существующую группу доступности.  Мастер создания групп доступности не позволяет включить поддержку DTC для новой группы доступности.  Следующий скрипт создаст группу доступности и присоединит базу данных-получатель.  Запустите следующий скрипт T-SQL в SSMS для `SQLNODE1` в **режиме SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

--  Create Availability Group on SQLNODE1 
USE master;
CREATE AVAILABILITY GROUP DTCAG1
WITH (DTC_SUPPORT = PER_DB) 
FOR DATABASE AG1
REPLICA ON 
  'SQLNODE1' WITH
     (
     ENDPOINT_URL = 'TCP://SQLNODE1.contoso.lab:5022', 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ),
  'SQLNODE2' WITH 
     (
     ENDPOINT_URL = 'TCP://SQLNODE2.contoso.lab:5022',
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ); 
GO


-- SQLNODE2
-- Join secondary replica to the Availability Group 
:connect SQLNODE2
ALTER AVAILABILITY GROUP DTCag1 JOIN;

-- Join database to the Availability Group
ALTER DATABASE AG1 SET HADR AVAILABILITY GROUP = DTCAG1;
GO
```

> [!IMPORTANT]
> Вы не можете включить DTC для существующей [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] примет следующий синтаксис для существующей группы доступности:  
> 
> USE master;    
> ALTER AVAILABILITY GROUP \<availability_group\>  
> SET (DTC_Support = Per_DB)  
> 
> Тем не менее реальное изменение конфигурации не производится.  Можно подтвердить конфигурацию **dtc_support** следующим запросом T-SQL:  
> 
> SELECT name, dtc_support FROM sys.availability_groups  
> 
> Единственный способ включить поддержку DTC для группы доступности заключается в создании группы доступности с помощью Transact-SQL.
 
## <a name="8--prepare-cluster-resources"></a><a name="ClusterDTC"></a>8.  Подготовка ресурсов кластера

Этот скрипт подготовит зависимые ресурсы DTC: диск и IP-адрес.  Общее хранилище будет добавлено в кластер Windows.  Будут созданы сетевые ресурсы, после чего будет создан DTC, предоставляемый группе доступности в виде ресурса.  Выполните приведенный ниже скрипт PowerShell в `SQLNODE1`. Еще раз благодарим [Аллана Херта](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/) за этот скрипт!

```powershell  
# Create a clustered Microsoft Distributed Transaction Coordinator properly in the resource group with SQL Server

\<#----------------------------------- Begin User Input -----------------------------------#>
$AGgrp = "DTCag1";                          # Name of the WSFC resource group that will contain the DTC resource

$WSFC = (Get-Cluster).Name;                 # Windows Failover Cluster name
$DTCnetwk = "Cluster Network 1"             # WSFC Network to use for the DTC IP address

$ClusterAvailableDisk = "Cluster Disk 3";   # Designated disk that can support failover clustering and is visible to all nodes, but not yet part of the set of clustered disks
$DTCdisk = "DTCDisk1";                      # Name of the disk to be used with DTC

$DTCipresnm = "DTCIP1";                     # WSFC Friendly Name of the DTC's IP resource 
$DTCipaddr = "192.168.2.54";                # IP address of the DTC resource 
$DTCsubnet = "255.255.255.0";               # Subnet for the DTC IP address 
$DTCnetnm = "DTCNet1";                      # WSFC Friendly Name of the Network Name resource
$DTCresnm = "DTC1";                         # Name of the WSFC DTC Network Name resource; Name must be unique in AD
\<#------------------------------------ End User Input ------------------------------------#>


# Make a new disk available for use in a failover cluster.
Get-ClusterAvailableDisk | Where {$_.Name -eq $ClusterAvailableDisk} | Add-ClusterDisk;

# Rename disk
$resource = Get-ClusterResource $ClusterAvailableDisk; $resource.Name = $DTCdisk;

# Create the IP resource
Add-ClusterResource -Name $DTCipresnm -ResourceType "IP Address" -Group $AGgrp;

# Set the network to use, IP address, and subnet
# All three have to be configured at the same time
$DTCIPres = Get-ClusterResource $DTCipresnm;
$ntwk = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Network,$DTCnetwk;
$ipaddr = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Address,$DTCipaddr;
$subnet = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,SubnetMask,$dtcsubnet;

$setdtcipparams = $ntwk,$ipaddr,$subnet;
$setdtcipparams | Set-ClusterParameter;

# Create the Network Name resource
Add-ClusterResource $DTCnetnm -ResourceType "Network Name" -Group $AGgrp;

# Set the value for the Network Name resource
Get-ClusterResource $DTCnetnm | Set-ClusterParameter DnsName $DTCresnm;

# Add the IP address as a depenency of the Network Name resource
Add-ClusterResourceDependency $DTCnetnm $DTCipresnm;

# Create the Distributed Transaction Coordinator resource
Add-ClusterResource $DTCresnm -ResourceType "Distributed Transaction Coordinator" -Group $AGgrp;

# Add the Network Name as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCnetnm;

# Move the disk into the resource group with SQL Server
Move-ClusterResource -Name $DTCdisk -Group $AGgrp;

# Add the disk as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCdisk;

# Bring the IP resource online
Start-ClusterResource $DTCipresnm;

# Bring the Network Name resource online
Start-ClusterResource $DTCnetnm;

# Bring the DTC resource online
Start-ClusterResource $DTCresnm;
```  

## <a name="9---enable-network-dtc"></a>9.   Включение сетевого DTC 

Следующий скрипт включит сетевой доступ DTC для кластеризованной службы DTC, чтобы удаленные компьютеры могли прикрепляться к распределенным транзакциям по сети.  Выполните приведенный ниже скрипт PowerShell в `SQLNODE1`.

```powershell  
# Enable Network DTC access for the clustered DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

# Enter Name of DTC resource

$DtcName = "DTC1";
\<# ------- End of User Input ------- #>

[bool]$restart = 0;
$node = (Get-ClusterResource -Name $DtcName).OwnerNode.Name;
$DtcSettings = Get-DtcNetworkSetting -DtcName $DtcName;

IF ($DtcSettings.InboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -InboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($DtcSettings.OutboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -OutboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($restart -eq 1)
{
    Stop-Dtc -CimSession $node -DtcName $DTCname -Confirm:$false;
    Start-Dtc -CimSession $node -DtcName $DTCname;
}
```  

## <a name="10--disable-and-stop-the-local-dtc-service-on-each-node"></a>10.  Отключение и остановка локальной службы DTC на каждом узле

Чтобы распределенные транзакции гарантированно использовали кластеризованный ресурс DTC, отключите локальную службу DTC на обоих узлах.  Следующий скрипт отключит и остановит локальную службу DTC на каждом узле.  Выполните приведенный ниже скрипт PowerShell в `SQLNODE1`.
```powershell  
# Disable local DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$DTCname = 'Local';
$nodes = (Get-ClusterNode).Name;

 foreach ($node in $nodes) {

    $service = Get-WmiObject -class Win32_Service -computername $node -Filter "Name='MSDTC'";
    IF ($service.StartMode -ne 'Disabled')
    {
        $service.ChangeStartMode('Disabled');
    }
    
    IF ($service.State -ne 'Stopped')
    {
        $service.StopService();
    }
}
```  

## <a name="11--cycle-the-ssnoversion-service-for-each-instance"></a>11.  Остановка и запуск службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для каждого экземпляра

После полной настройки кластеризованной службы DTC необходимо остановить и перезапустить каждый экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в группе доступности, чтобы убедиться, что [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] зарегистрирован для использования этой службы DTC.

В первый раз, когда службе [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] потребуется распределенная транзакция, она будет зарегистрирована в службе DTC. Служба SQL Server будет продолжать использовать эту службу DTC до ее перезапуска. Если доступна кластеризованная служба DTC, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] будет зарегистрирован в этой службе. Если кластеризованная служба DTC недоступна, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] будет зарегистрирован в локальной службе DTC. Чтобы проверить регистрацию [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в кластеризованной службе DTC, остановите и перезапустите каждый экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 

Выполните действия, содержащиеся в приведенном ниже скрипте T-SQL:
```sql  
/*
Gracefully cycle the SQL Server service and failover the Availability Group
    a.  On SQLNODE2, cycle the SQL Server service from SQL Server Configuration Manger

    b.  On SQLNODE2 failover the Availability Group to SQLNODE2
        Execute T-SQL script, below, on SQLNODE2 (Use Results to Text)

    c.  On SQLNODE1, cycle the SQL Server service from SQL Server Configuration Manger

    d.  On SQLNODE1 failover the Availability Group to SQLNODE1 once the databases are back in sync.
        Execute T-SQL script, below, on SQLNODE1 (Use Results to Text)
*/

SET NOCOUNT ON;

-- Ensure replica is secondary
IF (
SELECT rs.is_primary_replica 
    FROM sys.availability_groups ag
    JOIN sys.dm_hadr_database_replica_states rs
    ON  ag.group_id = rs.group_id
    WHERE ag.name = N'DTCag1'
    AND rs.is_local = 1) = 0
BEGIN
    -- Wait for SYNCHRONIZED state
    DECLARE @ctr tinyint = 0;
    declare @msg varchar(128);
    WHILE (SELECT synchronization_state 
        FROM sys.availability_groups ag
        JOIN sys.dm_hadr_database_replica_states rs
        ON  ag.group_id = rs.group_id
        WHERE ag.name = N'DTCag1'
        AND rs.is_primary_replica = 0
        AND rs.is_local = 1) <> 2
    BEGIN
        WAITFOR DELAY '00:00:01'
        SET @ctr += 1
        SET @msg = 'Waiting for databases to become synchronized. Duration in seconds: ' + cast(@ctr AS varchar(3))
        RAISERROR (@msg, 0, 1) WITH NOWAIT
    END

    ALTER AVAILABILITY GROUP DTCAG1 FAILOVER;
    SELECT 'Failover complete' AS [Sucess]
END
ELSE BEGIN
    SELECT 'This instance is the primary replica.  Connect to the secondary replica and try again.' AS [Error]
END

```

## <a name="12--test-configuration"></a>12.  Проверка конфигурации

Для создания распределенной транзакции в этом тесте используется связанный сервер с `SQLNODE1` на `SQLNODE2`.  Убедитесь, первичная реплика группы доступности находится на `SQLNODE1`. Чтобы проверить конфигурацию, вы выполните следующие действия.

- Создание связанных серверов
- Выполнение распределенной транзакции

### <a name="create-linked-servers"></a>Создание связанных серверов  
Следующий скрипт создает два связанных сервера на `SQLNODE1`.  Запустите следующий скрипт T-SQL в SSMS для `SQLNODE1`.

```sql  
-- SQLNODE1
IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE1')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE1';   
END

IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE2')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE2';   
END
 ```

### <a name="execute-a-distributed-transaction"></a>Выполнение распределенной транзакции
Сначала этот скрипт возвращает текущую статистику по транзакции DTC.  Затем он выполняет распределенную транзакцию с использованием баз данных на `SQLNODE1` и `SQLNODE2`.  Потом скрипт снова возвращает статистику по транзакции DTC, где должен отображаться увеличенный счетчик.  Выполните физическое подключение к `SQLNODE1` и запустите следующий скрипт T-SQL в SSMS для `SQLNODE1` в **режиме SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
    Must be physically connected to SQLNODE1
*******************************************************************/

USE AG1;
SET NOCOUNT ON;

-- Get Baseline
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;

SET XACT_ABORT ON
BEGIN DISTRIBUTED TRANSACTION
    INSERT INTO SQLNODE1.[AG1].[dbo].[Names] VALUES ('TestValue1', GETDATE());
    INSERT INTO SQLNODE2.[dtcDemoAG1].[dbo].[Names] VALUES ('TestValue2', GETDATE());
COMMIT TRAN
GO

-- Review DTC Transaction Statistics
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;
```

> [!IMPORTANT]
> Требуется выполнить инструкцию `USE AG1` , чтобы убедиться, что контекст базы данных имеет значение `AG1`.  В противном случае появится следующее сообщение об ошибке: "Контекст транзакции используется другим сеансом".