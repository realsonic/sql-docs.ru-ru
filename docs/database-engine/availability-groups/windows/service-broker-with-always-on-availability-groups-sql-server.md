---
title: Компонент Service Broker с группами доступности
description: Содержит сведения о настройке Service Broker с группами доступности Always On SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: 881c20e5-1c99-44eb-b393-09fc5ea0f122
author: cawrites
ms.author: chadam
ms.openlocfilehash: 521d66002a1ad3d2ead2a906dc40a87d22ae220a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642112"
---
# <a name="service-broker-with-always-on-availability-groups-sql-server"></a>Компонент Service Broker с группами доступности AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  В этом разделе содержатся сведения о настройке компонента Service Broker для работы с [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
  
##  <a name="requirements-for-a-service-in-an-availability-group-to-receive-remote-messages"></a><a name="ReceiveRemoteMessages"></a> Требования к службе в группе доступности для получения удаленных сообщений  
  
1.  **Убедитесь, что группа доступности имеет прослушиватель.**  
  
     Дополнительные сведения см. в разделе [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
2.  **Убедитесь, что конечная точка компонента Service Broker существует и правильно настроена.**  
  
     В каждом экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , где размещается реплика доступности для группы доступности, настройте конечную точку компонента Service Broker следующим образом.  
  
    -   Установите параметр LISTENER_IP в значение ALL. Этот параметр разрешает соединения по любому допустимому IP-адресу, привязанному к прослушивателю группы доступности.  
  
    -   Установите в параметре PORT компонента Service Broker одинаковый номер порта во всех экземплярах сервера.  
  
        > [!TIP]  
        >  Чтобы просмотреть номер порта конечной точки компонента Service Broker в заданном экземпляре сервера, запросите столбец **port** представления каталога [sys.tcp_endpoints](../../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) , где **type_desc** = 'SERVICE_BROKER'.  
  
     В следующем примере создается конечная точка компонента Service Broker с проверкой подлинности Windows, которая использует порт компонента Service Broker по умолчанию (4022) и прослушивает все допустимые IP-адреса.  
  
    ```  
    CREATE ENDPOINT [SSBEndpoint]  
        STATE = STARTED  
        AS TCP  (LISTENER_PORT = 4022, LISTENER_IP = ALL )  
        FOR SERVICE_BROKER (AUTHENTICATION = WINDOWS)  
    ```  
  
     Дополнительные сведения см. в разделе [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md).  

    > [!NOTE]  
    Компонент SQL Server Service Broker не поддерживает несколько подсетей. Задайте для параметра `RegisterAllProvidersIP` значение 0 и убедитесь, что кластер имеет необходимое разрешение в DNS для использования статических IP-адресов. Дополнительные сведения см. в разделе [Настройка прослушивателя группы доступности](create-or-configure-an-availability-group-listener-sql-server.md). При попытке использовать отключенный IP-адрес Service Broker может отложить сообщение с состоянием CONVERSING.

3.  **Предоставьте разрешение CONNECT на конечную точку.**  
  
     Предоставьте разрешение CONNECT на конечную точку компонента Service Broker роли PUBLIC или некоторому имени входа.  
  
     В следующем примере разрешение на подключение к конечной точке компонента Service Broker с именем `broker_endpoint` предоставляется роли PUBLIC.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::[broker_endpoint] TO [PUBLIC]  
    ```  
  
     Дополнительные сведения см. в статье [GRANT (Transact-SQL)](../../../t-sql/statements/grant-transact-sql.md).  
  
4.  **Убедитесь, что база данных msdb содержит маршрут AutoCreatedLocal или маршрут к некоторой службе.**  
  
    > [!NOTE]  
    >  По умолчанию все пользовательские базы данных, включая **msdb**, содержат маршрут **AutoCreatedLocal**. Он соответствует имени любой службы и любому экземпляру компонента Service Broker и указывает, что сообщение должно быть доставлено внутри текущего экземпляра. Маршрут **AutoCreatedLocal** имеет более низкий приоритет, чем маршруты, в которых явно указывается служба, обменивающаяся данными с удаленным экземпляром.  
  
     Сведения о создании маршрутов см. в статьях [Примеры маршрутизации для компонента Service Broker](https://msdn.microsoft.com/library/ms166090\(SQL.105\).aspx) (версия [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] электронной документации) и [CREATE ROUTE (Transact-SQL)](../../../t-sql/statements/create-route-transact-sql.md).  
  
##  <a name="requirements-for-sending-messages-to-a-remote-service-in-an-availability-group"></a><a name="SendRemoteMessages"></a> Требования к отправке сообщений удаленной службе в группе доступности  
  
1.  **Создайте маршрут к целевой службе.**  
  
     Настройте маршрут следующим образом.  
  
    -   Задайте в параметре ADDRESS IP-адрес прослушивателя группы доступности, в которой размещается база данных службы.  
  
    -   Задайте в параметре PORT порт, указанный в конечной точке компонента Service Broker в каждом из удаленных экземпляров SQL Server.  
  
     В следующем примере создается маршрут с именем `RouteToTargetService` для службы `ISBNLookupRequestService` . Маршрут ведет к прослушивателю группы доступности `MyAgListener`, который использует порт 4022.  
  
    ```  
    CREATE ROUTE [RouteToTargetService] WITH   
    SERVICE_NAME = 'ISBNLookupRequestService',   
    ADDRESS = 'TCP://MyAgListener:4022';  
  
    ```  
  
     Дополнительные сведения см. в статье [CREATE ROUTE (Transact-SQL)](../../../t-sql/statements/create-route-transact-sql.md).  
  
2.  **Убедитесь, что база данных msdb содержит маршрут AutoCreatedLocal или маршрут к некоторой службе.** (Дополнительные сведения см. в подразделе [Требования к службе в группе доступности для получения удаленных сообщений](#ReceiveRemoteMessages)выше.)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
-   [CREATE ROUTE (Transact-SQL)](../../../t-sql/statements/create-route-transact-sql.md)  
  
-   [GRANT (Transact-SQL)](../../../t-sql/statements/grant-transact-sql.md)  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
-   [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Настройка учетных записей входа для зеркального отображения баз данных или групп доступности AlwaysOn (SQL Server)](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [SQL Server Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
