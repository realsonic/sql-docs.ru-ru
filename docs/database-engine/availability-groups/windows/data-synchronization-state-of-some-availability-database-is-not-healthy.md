---
title: Состояние синхронизации данных некоторых баз данных доступности не является исправным
description: Определите возможные причины, по которым состояние синхронизации данных у некоторых баз данных в группе доступности Always On — "не работоспособны".
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.drp3datasynchealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 89f95d15-33c6-4768-bccd-9dbf8c4f49a9
author: cawrites
ms.author: chadam
ms.openlocfilehash: ab55e83942a2591a335759ae3df82b42d3d9db9b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643255"
---
# <a name="data-synchronization-state-of-some-availability-database-is-not-healthy"></a>Состояние синхронизации данных некоторых баз данных доступности не является исправным
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Введение  
  
|||  
|-|-|  
|**Имя политики**|Состояние синхронизации данных реплики доступности|  
|**Проблема**|Состояние синхронизации данных некоторых баз данных доступности не является исправным.|  
|**Категория**|**Предупреждение**|  
|**Аспект**|Реплика доступности|  
  
## <a name="description"></a>Description  
 Эта политика проверяет состояние синхронизации базы данных доступности (также называемой «реплика базы данных»). Политика находится в неисправном состоянии, когда синхронизация данных приобретает состояние NOT SYNCRONIZING, либо не является SYNCHRONIZED при синхронной фиксации реплики баз данных.  
  
> [!NOTE]  
>  Для этого выпуска [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]сведения о возможных причинах проблем и решениях приведены в разделе [Состояние синхронизации данных некоторых баз данных доступности не является исправным](https://go.microsoft.com/fwlink/p/?LinkId=220863) в TechNet Wiki.  
  
## <a name="possible-causes"></a>Возможные причины  
 Одна или несколько баз данных доступности в этой реплике не находятся в исправном состоянии синхронизации данных. Если это реплика доступности с асинхронной фиксацией, все базы данных доступности должны находиться в состоянии SYNCHRONIZING. Если эта реплика доступности настроена для синхронной фиксации, все базы данных доступности должны быть в состоянии SYNCHRONIZED. Возможны следующие причины этой проблемы.  
  
-   Возможно, эта реплика доступности отключена.  
  
-   Перемещение данных может быть временно приостановлено.  
  
-   База данных может быть недоступна.  
  
-   Возможны некоторые задержки из-за проблем с сетью или нагрузки на основную или дополнительную реплику.  
  
## <a name="possible-solution"></a>Возможное решение  
 Устраните все неполадки, связанные с подключением или приостановкой перемещения данных. Проверьте события, связанные с этой неполадкой при помощи среды SQL Server Management Studio, и найдите ошибку базы данных. Выполните действия по устранению неполадок, предписанные для этой ошибки.  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
