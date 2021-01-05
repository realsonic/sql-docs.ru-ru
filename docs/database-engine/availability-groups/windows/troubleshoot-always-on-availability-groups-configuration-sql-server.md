---
title: Распространенные проблемы и решения для групп доступности
description: Устраняйте типичные проблемы, возникающие при настройке экземпляров сервера для групп доступности Always On в SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], deploying
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], configuring
ms.assetid: 8c222f98-7392-4faf-b7ad-5fb60ffa237e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0fbdfdced5e0563e86775e75aaf187d6f26a0a38
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641528"
---
# <a name="troubleshoot-always-on-availability-groups-configuration-sql-server"></a>Поиск и устранение неисправностей конфигурации групп доступности AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Этот раздел содержит сведения об устранении типичных проблем, возникающих при настройке экземпляров сервера для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Примеры типичных проблем настройки: [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] отключен, учетные записи настроены неправильно, конечная точка зеркального отображения баз данных не существует, конечная точка недоступна (ошибка SQL Server 1418), отсутствует сетевой доступ, команда присоединения базы данных завершается с ошибкой (ошибка SQL Server 35250).  
  
> [!NOTE]  
>  Проверьте, выполняются ли предварительные требования [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Дополнительные сведения см. в разделе [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 **В этом разделе:**  
  
|Section|Описание|  
|-------------|-----------------|  
|[Функция групп доступности AlwaysOn не включена](#IsHadrEnabled)|Если экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не включен для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], то экземпляр не поддерживает создание групп доступности и на нем не могут размещаться реплики доступности.|  
|[Измерение счетов](#Accounts)|Обсуждаются требования к правильной настройке учетных записей, под которыми работает [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[Конечные точки](#Endpoints)|Описывается диагностика проблем конечной точки зеркального отображения баз данных для экземпляра сервера.|  
|[Имя системы](#SystemName)|Обобщаются альтернативы указанию системного имени экземпляра сервера в URL-адресе конечной точки.|  
|[Сетевой доступ](#NetworkAccess)|Документирует требование к каждому экземпляру сервера, на котором размещается реплика доступности, чтобы каждый такой экземпляр имел доступ к порту каждого другого экземпляра сервера по протоколу TCP.|  
|[Доступ к конечной точке (ошибка SQL Server 1418)](#Msg1418)|Содержит сведения об этом сообщении об ошибке [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[Ошибка присоединения базы данных (ошибка SQL Server 35250)](#JoinDbFails)|Обсуждаются возможные причины и способы устранения проблемы с присоединением баз данных-получателей к группе доступности, поскольку соединение с первичной репликой неактивно.|  
|[Маршрутизация только для чтения работает неправильно](#ROR)||  
|[Связанные задачи](#RelatedTasks)|Содержит список разбитых по задачам разделов в электронной документации по [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] , особенно важных для устранения неполадок с конфигурацией группы.|  
|[См. также](#RelatedContent)|Содержит список важных ресурсов, не входящих в состав электронной документации по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
  
##  <a name="always-on-availability-groups-is-not-enabled"></a><a name="IsHadrEnabled"></a> Функция групп доступности AlwaysOn не включена  
 Функция [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] должна быть включена на каждом из экземпляров [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Дополнительные сведения см. в разделе [Включение и отключение групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
##  <a name="accounts"></a><a name="Accounts"></a> Измерение счетов  
 Учетные записи, под которыми работает [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , должны быть правильно настроены.  
  
1.  Имеют ли учетные записи нужные разрешения?  
  
    1.  Если участники запущены под одной и той же учетной записью домена, то правильные имена входа существуют в обеих базах данных **master** . Это рекомендовано и упрощает настройку безопасности базы данных.  
  
    2.  Если два экземпляра сервера выполняются под разными учетными записями, то для каждой учетной записи должно быть создано имя входа в базе данных **master** на удаленном экземпляре сервера и этому имени входа необходимо присвоить разрешения CONNECT для подключения к конечной точке зеркального отображения базы данных на этом экземпляре сервера. Дополнительные сведения см. в статье [Создание учетных записей для зеркального отображения баз данных или групп доступности AlwaysOn (SQL Server)](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
2.  Если [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] выполняется как встроенная учетная запись, например как учетная запись локальной системы, локальной службы, сетевой службы или как недоменная учетная запись, для проверки подлинности конечных точек следует использовать сертификаты. Если учетные записи служб используют учетные записи доменов в одном домене, вы можете предоставить доступ CONNECT для каждой учетной записи службы на всех расположениях реплики либо воспользоваться сертификатами. Дополнительные сведения см. в разделе [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Конечные точки  
 Конечные точки должны быть правильно настроены.  
  
1.  Убедитесь, что каждый экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на котором планируется размещать реплику доступности (каждое *расположение реплики*), имеет конечную точку зеркального отображения баз данных. Чтобы определить, существует ли конечная точка зеркального отображения баз данных на данном экземпляре сервера, воспользуйтесь представлением каталога [sys.database_mirroring_endpoints](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md). Дополнительные сведения см. в статье [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) или [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  Убедитесь, что номера портов правильны.  
  
     Чтобы определить, какой порт в текущий момент связан с конечной точкой зеркального отображения базы данных экземпляра сервера, воспользуйтесь следующей инструкцией [!INCLUDE[tsql](../../../includes/tsql-md.md)] :  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints;  
    GO  
    ```  
  
3.  Если при настройке [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] возникают труднообъяснимые неполадки, рекомендуется на каждом экземпляре сервера проверить, правильный ли порт он прослушивает.  
  
4.  Убедитесь, что конечные точки запущены (STATE=STARTED). На каждом экземпляре сервера выполните следующую инструкцию [!INCLUDE[tsql](../../../includes/tsql-md.md)]:  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Дополнительные сведения о столбце **state_desc** см. в разделе [sys.database_mirroring_endpoints (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Чтобы запустить конечную точку, выполните следующую инструкцию [!INCLUDE[tsql](../../../includes/tsql-md.md)]:  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Дополнительные сведения см. в статье [ALTER ENDPOINT (Transact-SQL)](../../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Убедитесь, что имени входа на другом сервере предоставлено разрешение CONNECT. Чтобы узнать, кто имеет разрешение CONNECT для конечной точки, выполните следующую инструкцию [!INCLUDE[tsql](../../../includes/tsql-md.md)] на каждом экземпляре сервера:  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-name"></a><a name="SystemName"></a> System Name  
 В качестве системного имени экземпляра сервера в URL-адресе конечной точки можно использовать любое имя, которое однозначно идентифицирует систему. Адрес сервера может представлять собой системное имя (если системы находятся в одном), полное доменное имя или IP-адрес (желательно статический). Полное доменное имя будет работать гарантированно. Дополнительные сведения см. в разделе [Указание URL-адреса конечной точки при добавлении или изменении реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Каждый экземпляр сервера, на котором размещается реплика доступности, должен иметь доступ к порту каждого другого экземпляра сервера по протоколу TCP. Это особенно важно, если экземпляры сервера находятся в разных доменах, не имеющих доверительных отношений друг с другом (домены без доверия).  
  
##  <a name="endpoint-access-sql-server-error-1418"></a><a name="Msg1418"></a> Доступ к конечной точке (ошибка SQL Server 1418)  
 Это сообщение [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] уведомляет, что сетевой адрес сервера, указанный в URL-адресе конечной точки сервера, недоступен или не существует, и предлагает выполнить проверку имени сетевого адреса и повторно выполнить команду.  
  
##  <a name="join-database-fails-sql-server-error-35250"></a><a name="JoinDbFails"></a> Ошибка присоединения базы данных (ошибка SQL Server 35250)  
 В этом разделе обсуждаются возможные причины и способы устранения проблемы с присоединением баз данных-получателей к группе доступности, вызванные тем, что соединение с первичной репликой неактивно.  
  
 **Решение.**  
  
1.  Проверьте настройку брандмауэра: разрешена ли связь для портов конечных точек между экземплярами серверов, на которых размещаются первичная и вторичная реплика (по умолчанию порт 5022).  
  
2.  Проверьте, обладает ли учетная запись сетевой службы разрешением на подключение к конечной точке.  
  
##  <a name="read-only-routing-is-not-working-correctly"></a><a name="ROR"></a> Маршрутизация только для чтения работает неправильно  
 Проверьте следующие параметры значений конфигурации и исправьте их при необходимости.  
  
|Если...|Действие|Комментарии|Ссылка|  
|---------|------------|--------------|----------|  
|Текущая первичная реплика|Убедитесь, что прослушиватель группы доступности находится в режиме «в сети».|**Чтобы убедиться, что прослушиватель имеет состояние «в сети», выполните следующие действия.**<br /><br /> `SELECT * FROM sys.dm_tcp_listener_states;`<br /><br /> **Перезапуск прослушивателя с состоянием «вне сети»**<br /><br /> `ALTER AVAILABILITY GROUP myAG RESTART LISTENER 'myAG_Listener';`|[sys.dm_tcp_listener_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Текущая первичная реплика|Убедитесь, что параметр READ_ONLY_ROUTING_LIST содержит только экземпляры сервера, где размещена вторичная реплика.|**Определение доступных для чтения вторичных реплик:** sys.availability_replicas (столбец **secondary_role_allow_connections_desc** )<br /><br /> **Просмотр списка маршрутизации только для чтения:** sys.availability_read_only_routing_lists<br /><br /> **Изменение списка маршрутизации только для чтения.** ALTER AVAILABILITY GROUP|[sys.availability_replicas (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [sys.availability_read_only_routing_lists (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Каждая реплика в списке read_only_routing_list|Убедитесь, что брандмауэр Windows не блокирует порт READ_ONLY_ROUTING_URL.|-|[Настройка брандмауэра Windows для доступа к компоненту Database Engine](../../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Каждая реплика в списке read_only_routing_list|В диспетчере конфигурации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] убедитесь в следующем.<br /><br /> Удаленное соединение с SQL Server включено.<br /><br /> TCP/IP включен.<br /><br /> IP-адреса настроены правильно.|-|[Просмотр или изменение свойств сервера (SQL Server)](../../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)<br /><br /> [Настройка сервера для прослушивания указанного TCP-порта (диспетчер конфигурации SQL Server)](../../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)|  
|Каждая реплика в списке read_only_routing_list|Убедитесь, что параметр READ_ONLY_ROUTING_URL (TCP <strong>://</strong>*system-address*<strong>:</strong>*port*) содержит правильное полное доменное имя (FQDN) и номер порта.|-|[Вычисление значения read_only_routing_url для AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)<br /><br /> [sys.availability_replicas (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Система клиента|Убедитесь, что драйвер клиента поддерживает маршрутизацию только для чтения.|-|[Подключение клиента AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Укажите URL-адрес конечной точки при добавлении или изменении реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Подготовка базы данных-получателя для присоединения к группе доступности вручную (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Устранение неполадок с операцией добавления файла, давшей сбой (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
-   [Управление именами входа и заданиями для баз данных группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
-   [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   [Просмотр событий и журналов для отказоустойчивого кластера](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Командлет Get-ClusterLog отказоустойчивого кластера](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
-   [Блог команды разработчиков SQL Server Always On: официальный блог по SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>См. также:  
 [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Конфигурация клиентской сети](../../../database-engine/configure-windows/client-network-configuration.md)   
 [Предварительные требования, ограничения и рекомендации для групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
