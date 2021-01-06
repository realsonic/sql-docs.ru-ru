---
title: WSFC служба кластеров работает в режиме вне сети | Документы Майкрософт
description: Политика "Состояние кластера WSFC" проверяет состояние отказоустойчивого кластера Windows Server. Эта политика переходит в неисправное состояние, когда кластер отключается от сети или находится в состоянии "принудительный кворум".
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp1WSFCquorum.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: d502548d-ece6-4a42-9ded-2157d33e3d21
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7231b013f4fabb5e91cf02d6885620cdc876a3a6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643700"
---
# <a name="wsfc-cluster-service-is-offline"></a>WSFC служба кластеров работает в режиме вне сети

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
## <a name="introduction"></a>Введение  
  
|||  
|-|-|  
|**Имя политики**|Состояние кластера WSFC|  
|**Проблема**|Служба кластеров WSFC находится вне сети.|  
|**Категория**|**Критическая**|  
|**Аспект**|Экземпляр SQL Server|  
  
## <a name="description"></a>Описание  
 Эта политика проверяет состояние отказоустойчивого кластера Windows Server (WSFC). Политика находится в неисправном состоянии и выдает предупреждение, если кластер WSFC находится вне сети или в состоянии «принудительный кворум». Все группы доступности, размещенные на этом кластере, находятся в режиме «вне сети», либо требуется процедура аварийного восстановления.  
  
 Эта политика находится в исправном состоянии, если состояние кластера — «нормальный кворум».  
  
> [!NOTE]  
>  Для этого выпуска [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], сведения о возможных причинах проблем и решениях приведены в разделе [Служба кластера WSFC не в сети](https://go.microsoft.com/fwlink/p/?LinkId=220849) в TechNet Wiki.  
  
## <a name="possible-causes"></a>Возможные причины  
 Эта неполадка может возникать из-за неисправности службы кластера либо при потере кворума в кластере.  
  
## <a name="possible-solution"></a>Возможное решение  
 Используйте администратор кластеров для запуска процедур принудительного кворума или аварийного восстановления. Если проблему не удается устранить при помощи процедур принудительного кворума или аварийного восстановления, обратитесь к администратору кластера за помощью в устранении этой неполадки. Дополнительные сведения см. в разделе [Принудительный запуск кластера WSFC без кворума](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md) электронной документации по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
