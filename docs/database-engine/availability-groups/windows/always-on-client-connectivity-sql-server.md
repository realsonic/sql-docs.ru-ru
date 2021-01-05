---
title: Поддержка возможности подключения драйвера и клиента для групп доступности
description: 'В этом разделе обсуждаются особенности обеспечения подключений клиентов к группам доступности AlwaysOn, в том числе предварительные условия, ограничения и рекомендации по конфигурации и настройке клиентов. '
ms.custom: seodec18
ms.date: 07/28/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], prerequisites and restrictions
- Availability Groups [SQL Server], client connectivity
ms.assetid: b456448d-1757-48c8-8bbb-2d1c2d6d61e9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 20a1d0074330552e4a87defb65e9e6e1082df40c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642254"
---
# <a name="driver-and-client-connectivity-support-for-availability-groups"></a>Поддержка возможности подключения драйвера и клиента для групп доступности
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  В этом разделе обсуждаются особенности обеспечения подключений клиентов к группам доступности AlwaysOn, в том числе предварительные условия, ограничения и рекомендации по конфигурации и настройке клиентов.  
  
 
##  <a name="client-connectivity-support"></a><a name="ClientConnSupport"></a> Поддержка возможности подключения клиента  
 В приведенном далее разделе представлены сведения о поддержке [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] для обмена данными с клиентом.  
  
 **Поддержка драйверов**  
  
 В следующей таблице приведены сведения о поддержке драйверов для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
|Драйвер|Отработка отказа с использованием нескольких подсетей|Назначение приложения|Маршрутизация только для чтения|Переход на другой ресурс с использованием нескольких подсетей: переход на другой ресурс для конечной точки одной более быстрой подсети|Переход на другой ресурс с использованием нескольких подсетей: разрешение именованного экземпляра для кластеризованных экземпляров SQL|  
|------------|----------------------------|------------------------|------------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------------------|  
|SQL Native Client 11.0 ODBC|Да|Да|Да|Да|Да|  
|SQL Native Client 11.0 OLEDB|нет|Да|Да|нет|нет|  
|ADO.NET с платформой .NET Framework 4.0 с улучшением подключения*|Да|Да|Да|Да|Да|  
|ADO.NET с платформой .NET Framework 3.5 с пакетом обновления 1 (SP1) с улучшением подключения**|Да|Да|Да|Да|Да|  
|[Microsoft ODBC Driver 13.1+ для SQL Server](../../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)|Да|Да|Да|Да|Да|
|[Microsoft JDBC Driver 4.0+ для SQL Server](../../../connect/jdbc/microsoft-jdbc-driver-for-sql-server.md)|Да|Да|Да|Да|Да| 
|[Драйвер Microsoft OLE DB для SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md)|Да|Да|Да|Да|Да| 
  
 * Скачайте исправление подключения для ADO.NET с платформой .NET Framework 4.0: [https://support.microsoft.com/kb/2600211](https://support.microsoft.com/kb/2600211).  
  
 ** Скачайте исправление подключения для ADO.NET с платформой .NET Framework 3.5 с пакетом обновления 1 (SP1): [https://support.microsoft.com/kb/2654347](https://support.microsoft.com/kb/2654347).  
 
 * Скачайте новый драйвер Microsoft OLE DB для SQL Server: [https://aka.ms/downloadmsoledbsql](../../../connect/oledb/download-oledb-driver-for-sql-server.md).  

> [!IMPORTANT]  
>  Для подключения к прослушивателю группы доступности клиент должен использовать строку подключения TCP.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Отказоустойчивая кластеризация и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Сведения о доступе клиентского подключения к репликам доступности (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Руководство по решениям режима AlwaysOn в Microsoft SQL Server для обеспечения высокой доступности и аварийного восстановления](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))   
 [Блог команды разработчиков SQL Server Always On: официальный блог команды разработчиков SQL Server Always On](/archive/blogs/sqlalwayson/)   
 [На компьютерах под управлением Windows Server 2003, Windows Vista, Windows Server 2008, Windows 7 или Windows Server 2008 R2 происходит долговременная задержка при повторном подключении с помощью протокола IPSec](https://support.microsoft.com/kb/980915)   
 [Службе кластера необходимо около 30 секунд для отработки отказа IP-адресов IPv6 в Windows Server 2008 R2](https://support.microsoft.com/kb/2578113)   
 [Медленная отработка отказа при отсутствии маршрутизатора между кластером и сервером приложений](https://support.microsoft.com/kb/2582281)  
  
