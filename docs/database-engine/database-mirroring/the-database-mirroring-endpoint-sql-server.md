---
title: Конечная точка зеркального отображения базы данных (SQL Server) | Документы Майкрософт
description: Сведения о выделенной конечной точке зеркального отображения базы данных, которая необходима каждому экземпляру сервера для участия в группах доступности AlwaysOn или зеркальном отображении базы данных.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], AlwaysOn Availability Groups
- endpoints [SQL Server], database mirroring
- Availability Groups [SQL Server], endpoint
ms.assetid: 39332dc5-678e-4650-9217-6aa3cdc41635
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 81d32a7a21c8e152944b8e24002320e865372681
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641963"
---
# <a name="the-database-mirroring-endpoint-sql-server"></a>Конечная точка зеркального отображения базы данных (SQL Server)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Для участия в [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] или зеркальном отображении базы данных экземпляр сервера должен иметь собственную выделенную *конечную точку зеркального отображения базы данных*. Это специальная конечная точка, которая используется исключительно для приема соединений от других экземпляров сервера. В каждом экземпляре сервера для каждого соединения [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] или зеркального отображения базы данных с любым другим экземпляром сервера требуется одна конечная точка зеркального отображения базы данных.  
  
 Для передачи и получения сообщений между экземплярами серверов, участвующими в сеансах зеркального отображения базы данных или размещающих реплики доступности, в конечных точках зеркального отображения базы данных используется протокол TCP. Конечная точка зеркального отображения базы данных прослушивает порт TCP с уникальным номером.  
  
> [!NOTE]  
>  В клиентских соединениях с основным сервером или первичной репликой не используйте конечную точку зеркального отображения.  
  
> [!NOTE]  
>  В будущей версии Microsoft SQL Server возможность зеркального копирования базы данных будет удалена. Избегайте использования этой возможности в новых разработках и запланируйте перевод приложений, в которых она применяется, на использование [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] .  
  
  
##  <a name="server-network-address"></a><a name="ServerNetworkAddress"></a> Сетевой адрес сервера  
 Сетевой адрес экземпляра сервера (его *сетевой адрес сервера* или *URL-адрес конечной точки*) содержит номер порта его конечной точки, а также системное и доменное имя его главного компьютера. Номер порта уникально определяет конкретный экземпляр сервера.  
  
 На следующем рисунке показано, как реализуется уникальная идентификация расположенных на одном сервере двух экземпляров сервера. Сетевые адреса сервера обоих экземпляров сервера содержат одно и то же системное имя, `MYSYSTEM`, и доменное имя, `Adventure-Works.MyDomain.com`. Чтобы система могла направлять соединения тому или иному экземпляру сервера, сетевой адрес сервера включает в себя номер порта, ассоциированного с конечной точкой зеркального отображения конкретного экземпляра сервера.  
  
 ![Сетевые адреса сервера экземпляра по умолчанию](../../database-engine/availability-groups/windows/media/dbm-2-instances-ports-1-system.gif "Сетевые адреса сервера экземпляра по умолчанию")  
  
 По умолчанию, экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не содержит конечной точки зеркального отображения базы данных. Их нужно создавать вручную в рамках подготовки сеанса зеркального отображения базы данных. Системный администратор должен создавать отдельную конечную точку на каждом экземпляре сервера, который должен принимать участие в зеркальном отображении базы данных. Обратите внимание, что если конечная точка зеркального отображения базы данных требуется для нескольких экземпляров сервера на данном компьютере, следует указать разные номера портов для каждой конечной точки.  
  
> [!IMPORTANT]  
>  Если компьютер, на котором запущен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , оснащен брандмауэром, конфигурация брандмауэра должна разрешать установку как входящих, так и исходящих соединений для порта, указанного в конечной точке.  
  
 Для зеркального отображения базы данных и [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]проверка подлинности и шифрование настраиваются в конечной точке. Дополнительные сведения см. в статье [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
> [!IMPORTANT]  
>  Не изменяйте конфигурацию используемой конечной точки зеркального отображения базы данных. Экземпляры серверов используют конечные точки друг друга, чтобы узнать состояние других систем. Если конфигурация конечной точки изменена, может быть выполнен ее перезапуск, который может быть причиной возникновения ошибки на других экземплярах сервера. Это особенно важно в режиме автоматического перехода на другой ресурс, когда изменение конфигурации конечной точки участника может привести к переходу на другой ресурс.  
  
  
##  <a name="determining-the-authentication-type-for-a-database-mirroring-endpoint"></a><a name="EndpointAuthenticationTypes"></a> Определение типа проверки подлинности для конечной точки зеркального отображения базы данных  
 Важно понимать, что учетные записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на экземплярах сервера определяют, какой тип проверки подлинности вы можете использовать для конечных точек зеркального отображения баз данных.  
  
-   Если все экземпляры сервера выполняются с учетной записью службы домена, для конечных точек зеркального отображения базы данных вы можете использовать проверку подлинности Windows. Если все экземпляры сервера запущены под одной и той же учетной записью домена, правильные имена входа автоматически существуют в обеих базах данных **master** . Это упрощает настройку безопасности базы данных доступности и рекомендуется.  
  
     Если какие-либо экземпляры сервера, на которых размещаются реплики доступности для группы доступности, выполняются с разными учетными записями, имя входа для каждой учетной записи необходимо создать в базе данных **master** другого экземпляра сервера. Затем этому имени пользователя нужно предоставить разрешения CONNECT для подключения к конечной точке зеркального отображения базы данных этого экземпляра сервера. Дополнительные сведения см. в статье [Создание учетных записей для зеркального отображения баз данных или групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
     Если для экземпляров сервера используется проверка подлинности Windows, вы можете создать конечные точки зеркального отображения базы данных с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)], PowerShell или мастера создания группы доступности.  
  
    > [!NOTE]  
    >  Если экземпляр сервера, который выбран для размещения реплики доступности, еще не содержит конечной точки зеркального отображения базы данных, мастер создания группы доступности может автоматически создать конечную точку зеркального отображения базы данных, в которой будет использоваться проверка подлинности Windows. Дополнительные сведения см. в статье [Использование мастера добавления базы данных в группу доступности (среда SQL Server Management Studio)](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
-   Если какой-либо экземпляр сервера выполняется с встроенной учетной записью, например учетной записью локальной системы, локальной службы, сетевой службы или не входящей в домен учетной записью, то для проверки подлинности конечных точек следует использовать сертификаты. Если для конечных точек зеркального отображения баз данных используются сертификаты, системный администратор должен настроить каждый экземпляр сервера для использования сертификатов на входящих и исходящих соединениях.  
  
     Автоматического способа настройки защиты зеркального отображения баз данных с помощью сертификатов не существует. Потребуется использовать инструкцию CREATE ENDPOINT языка [!INCLUDE[tsql](../../includes/tsql-md.md)] или командлет PowerShell **New-SqlHadrEndpoint** . Дополнительные сведения см. в разделе [Создание внешнего источника данных (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md). Сведения о включении проверки подлинности сертификата на экземпляре сервера содержатся в статье [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Настройка конечной точки зеркального отображения базы данных**  
  
-   [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Укажите URL-адрес конечной точки при добавлении или изменении реплики доступности (SQL Server)](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Использование мастера добавления базы данных в группу доступности (среда SQL Server Management Studio)](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
 **Просмотр сведений о конечной точке зеркального отображения базы данных**  
  
-   [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
  
## <a name="see-also"></a>См. также:  
 [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Диагностика конфигурации зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [sys.dm_hadr_availability_replica_states (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)   
 [sys.dm_db_mirroring_connections (Transact-SQL)](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)  
  
  
