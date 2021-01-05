---
title: Просмотр свойств прослушивателя группы доступности
description: Инструкции по просмотру свойств прослушивателя группы доступности Always On с помощью SQL Server Management Studio, Transact-SQL или PowerShell в SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/11/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygrouplistenerproperties.general.f1
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
ms.assetid: aca0d016-3228-40b8-bdc3-285ed6d9b280
author: cawrites
ms.author: chadam
ms.openlocfilehash: b3b77706425eae89bd74b4803859e4a5c029ad17
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641724"
---
# <a name="view-availability-group-listener-properties-sql-server"></a>Просмотр свойств прослушивателя группы доступности (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается просмотр свойств *прослушивателя группы доступности* AlwaysOn при помощи среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 **Просмотр свойств прослушивателя**  
  
1.  В обозревателе объектов подключитесь к экземпляру сервера, на котором размещена любая реплика группы доступности, свойства прослушивателя которой необходимо просмотреть. Щелкните имя сервера, чтобы развернуть дерево сервера.  
  
2.  Разверните узел **Высокий уровень доступности AlwaysOn** и узел **Группы доступности** .  
  
3.  Разверните узел группы доступности и разверните узел **Прослушиватели группы доступности** .  
  
4.  Щелкните правой кнопкой мыши прослушиватель, свойства которого необходимо просмотреть, и выберите пункт **Свойства** .  
  
5.  Откроется диалоговое окно **Свойства прослушивателя группы доступности** . Дополнительные сведения см. в подразделе [Свойства прослушивателя группы доступности (диалоговое окно)](#AgListenerPropertiesDialog)далее в этом разделе.  
  
###  <a name="availability-group-listener-properties-dialog-box"></a><a name="AgListenerPropertiesDialog"></a> Свойства прослушивателя группы доступности (диалоговое окно)  
 **DNS-имя прослушивателя**  
 Сетевое имя прослушивателя группы доступности.  
  
 **порт**.  
 TPC-порт, используемый этим прослушивателем.  
  
> [!NOTE]  
>  Если вы подключены к основной реплике, можно использовать это поле для изменения номера порта прослушивателя. Для этого необходимо разрешение ALTER AVAILABILITY GROUP для группы доступности, разрешение CONTROL AVAILABILITY GROUP, разрешение ALTER ANY AVAILABILITY GROUP или разрешение CONTROL SERVER.  
  
 **Сетевой режим**  
 Указывает TCP-протокол, используемый прослушивателем, один из:  
  
 **DHCP**  
 Прослушиватель использует динамический IP-адрес, назначенный сервером, на котором запущен протокол DHCP.  
  
 **Статический IP-адрес**  
 Прослушиватель использует один или несколько статических IP-адресов. Для доступа к разным подсетям прослушивателю группы доступности необходимо использовать статический IP-адрес.  
  
 В сетке отображается каждая из подсетей, в которых работает прослушиватель и IP-адрес, связанный с данной подсетью.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Просмотр свойств прослушивателя**  
  
 Для мониторинга прослушивателей группы доступности используйте следующие представления.  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Возвращает строку для каждого совместимого виртуального IP-адреса, который в настоящее время включен для прослушивателя группы доступности.  
  
 **Имена столбцов:** listener_id, IP-адрес, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Для любой выбранной группы доступности возвращает либо ноль строк, указывая, что с группой доступности не связано ни одного сетевого имени, либо отдельную строку для каждой конфигурации прослушивателя группы доступности в кластере WSFC.  
  
 **Имена столбцов:** group_id, listener_id, dns_name, порт, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Возвращает строку, содержащую сведения о динамическом состоянии для каждого прослушивателя TCP.  
  
 **Имена столбцов:** listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
> [!NOTE]  
>  Дополнительные сведения об использовании [!INCLUDE[tsql](../../../includes/tsql-md.md)] для отслеживания среды [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в разделе [Отслеживание групп доступности (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Удаление прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Отслеживание групп доступности (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
