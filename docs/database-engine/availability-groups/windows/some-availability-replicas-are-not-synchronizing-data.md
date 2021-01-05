---
title: Реплики доступности, не выполняющие синхронизацию данных
description: Возможные причины и способы устранения, когда одна или несколько реплик доступности в группе доступности Always On не синхронизируют данные с первичной репликой.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp4synchronizing.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 3db6a569-e942-4321-a0dd-c4ab002087c8
author: cawrites
ms.author: chadam
ms.openlocfilehash: d1b824e4b88ff5d8b0e31abcfcb6dca0acae815a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642098"
---
# <a name="some-availability-replicas-are-not-synchronizing-data"></a>Некоторые реплики доступности не выполняют синхронизацию данных
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Введение  
  
|||  
|-|-|  
|**Имя политики**|Состояние синхронизации данных реплик доступности|  
|**Проблема**|Некоторые реплики доступности не выполняют синхронизацию данных.|  
|**Категория**|**Предупреждение**|  
|**Аспект**|группа доступности|  
  
## <a name="description"></a>Description  
 Эта политика сворачивает состояние синхронизации данных всех реплик доступности в группе доступности и проверяет рабочее состояние синхронизации у всех реплик доступности. Политика находится в нерабочем состоянии, если у какой-либо реплики доступности при синхронизации данных имеется состояние NOT SYNCRONIZING.  
  
 Политика находится в рабочем состоянии, если ни у одной из реплик доступности при синхронизации данных не имеется состояние NOT SYNCRONIZING.  
  
> [!NOTE]  
>  Для этого выпуска [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]сведения о возможных причинах проблем и решениях доступны в разделе [Некоторые реплики доступности не выполняют синхронизацию данных](https://go.microsoft.com/fwlink/p/?LinkId=220852) в TechNet Wiki.  
  
## <a name="possible-causes"></a>Возможные причины  
 В этой группе доступности по крайней мере у одной вторичной реплики имеется состояние синхронизации NOT SYNCHRONIZING и она не получает данных от первичной реплики.  
  
## <a name="possible-solution"></a>Возможное решение  
 Используйте состояние политики реплик доступности для поиска реплики доступности с состоянием NOT SYNCHROINIZING и устраните проблему в реплике доступности.  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
