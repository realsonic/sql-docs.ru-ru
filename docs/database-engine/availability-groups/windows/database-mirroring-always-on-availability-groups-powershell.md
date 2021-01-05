---
title: 'PowerShell: Конечная точка зеркального отображения базы данных для групп доступности'
description: В статье описано создание конечной точки зеркального отображения базы данных для групп доступности Always On при помощи PowerShell
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], endpoint
ms.assetid: 6197bbe7-67d4-446d-ba5f-cabfa5df77f1
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9b1be7e6cbfead5bf0fa14bf60f294ad852d9ad
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643243"
---
# <a name="create-a-database-mirroring-endpoint-for-an-availability-group-using-powershell"></a>Создание конечной точки зеркального отображения базы данных для групп доступности при помощи PowerShell
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  В этом разделе описывается создание конечной точки зеркального отображения базы данных для использования [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью PowerShell.  
  

  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется разрешение CREATE ENDPOINT или членство в предопределенной роли сервера sysadmin. Дополнительные сведения см. в разделе [GRANT, предоставление разрешений на конечную точку (Transact-SQL)](../../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  

> [!IMPORTANT]  
>  Алгоритм RC4 устарел. [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого рекомендуется использовать алгоритм AES.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование PowerShell  
 **Создание конечной точки зеркального отображения базы данных**  
  
1.  Перейдите в каталог (**cd**) экземпляра сервера, для которого требуется создать конечную точку зеркального отображения.  
  
2.  Создайте конечную точку с помощью командлета **New-SqlHadrEndpoint** , а затем с помощью командлета **Set-SqlHadrEndpoint** запустите ее.  
  
###  <a name="example-powershell"></a><a name="PShellExample"></a> Пример (PowerShell)  
 Следующие команды PowerShell создают конечную точку зеркального отображения базы данных на экземпляре SQL Server (*Machine*\\*Instance*). Эта конечная точка использует порт 5022.  
  
> [!IMPORTANT]  
>  Этот пример работает только на экземпляре сервера, на котором в данный момент отсутствует конечная точка зеркального отображения базы данных.  
  
```  
# Create the endpoint.  
$endpoint = New-SqlHadrEndpoint MyMirroringEndpoint -Port 5022 -Path SQLSERVER:\SQL\Machine\Instance  
  
# Start the endpoint  
Set-SqlHadrEndpoint -InputObject $endpoint -State "Started"  
  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Настройка конечной точки зеркального отображения базы данных**  
  
-   [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Укажите URL-адрес конечной точки при добавлении или изменении реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Просмотр сведений о конечной точке зеркального отображения базы данных**  
  
-   [sys.database_mirroring_endpoints (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [Создание группы доступности (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)   
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
