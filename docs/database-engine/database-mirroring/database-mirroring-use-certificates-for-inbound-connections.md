---
title: Использование сертификатов для входящего соединения
description: Этапы настройки экземпляров сервера для использования сертификатов проверки подлинности входящих соединений при зеркальном отображении базы данных.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- inbound connections
- database mirroring [SQL Server], security
ms.assetid: 5d48bb98-61f0-4b99-8f1a-b53f831d63d0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2eb5bd6f4b8b234d8b48b32645ecd18b7734a776
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643390"
---
# <a name="database-mirroring---use-certificates-for-inbound-connections"></a>Использование сертификатов для входящих соединений при зеркальном отображении базы данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описаны этапы настройки экземпляров сервера для использования сертификатов проверки подлинности входящих соединений при зеркальном отображении базы данных. Перед настройкой входящих соединений необходимо настроить исходящие соединения на каждом экземпляре сервера. Дополнительные сведения см. в разделе [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 Процесс настройки входящих соединений состоит из следующих основных шагов:  
  
1.  Создайте имя входа для другой системы.  
  
2.  Создайте пользователя для этого имени входа.  
  
3.  Получите сертификат для конечной точки зеркального отображения другого экземпляра сервера.  
  
4.  Свяжите сертификат с пользователем, созданным на шаге 2.  
  
5.  Предоставьте этому имени входа разрешение CONNECT на эту конечную точку зеркального отображения.  
  
 Если имеется следящий сервер, для него также необходимо настроить входящие соединения. Для этого нужно настроить имена входа, пользователей и сертификаты для следящего сервера на обоих участниках, и наоборот.  
  
 Ниже эти шаги описаны подробно. Для каждого шага приведен пример настройки экземпляра сервера в системе с именем HOST_A. В соответствующем разделе «Пример» показаны те же шаги для другого экземпляра сервера в системе с именем HOST_B.  
  
### <a name="to-configure-server-instances-for-inbound-mirroring-connections-on-host_a"></a>Настройка экземпляров сервера на входящие соединения зеркального отображения (на узле HOST_A)  
  
1.  Создайте имя входа для другой системы.  
  
     В следующем примере в базе данных **master** экземпляра сервера на узле HOST_А создается имя входа для системы HOST_B; созданное имя входа называется `HOST_B_login`. Подставьте в пример свой собственный пароль.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login   
       WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
     Дополнительные сведения см. в разделе [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md).  
  
     Чтобы просмотреть имена входа для данного экземпляра сервера, необходимо выполнить следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT * FROM sys.server_principals  
    ```  
  
     Дополнительные сведения см. в разделе [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
2.  Создайте пользователя для этого имени входа.  
  
     В следующем примере для имени входа, созданного на предыдущем шаге, создается пользователь `HOST_B_user`.  
  
    ```  
    USE master;  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
     Дополнительные сведения см. в разделе [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md).  
  
     Чтобы просмотреть пользователей данного экземпляра сервера, необходимо выполнить следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT * FROM sys.sysusers;  
    ```  
  
     Дополнительные сведения см. в разделе [sys.sysusers (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md).  
  
3.  Получите сертификат для конечной точки зеркального отображения другого экземпляра сервера.  
  
     Необходимо получить копию сертификата для конечной точки зеркального отображения удаленного экземпляра сервера, если это еще не было сделано во время настройки исходящих соединений. Для этого создайте резервную копию сертификата в соответствующем экземпляре сервера, как описано в разделе [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md). При копировании сертификата на другую систему используйте безопасный метод копирования. Отнеситесь с особым вниманием к хранению сертификатов в безопасном месте.  
  
     Дополнительные сведения см. в разделе [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
4.  Свяжите сертификат с пользователем, созданным на шаге 2.  
  
     В следующем примере сертификат узла HOST_B связывается с соответствующим пользователем на узле HOST_А.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
     Дополнительные сведения см. в разделе [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Чтобы просмотреть сертификаты данного экземпляра сервера, необходимо выполнить следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT * FROM sys.certificates  
    ```  
  
     Дополнительные сведения см. в разделе [sys.certificates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
5.  Предоставьте данной учетной записи разрешение CONNECT на эту удаленную конечную точку зеркального отображения.  
  
     Например, чтобы предоставить разрешение HOST_A удаленному экземпляру сервера на HOST_B для подключения к его локальной учетной записи, то есть подключиться к `HOST_B_login`, необходимо выполнить следующие инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
    ```  
    USE master;  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
     Дополнительные сведения см. в разделе [GRANT, предоставление разрешений на конечную точку (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
 Настройка проверки подлинности сертификата, выданного узлу HOST_B для входа в систему HOST_A, завершена.  
  
 Необходимо выполнить аналогичные шаги для настройки входящего соединения от узла HOST_A к узлу HOST_B. Они перечислены ниже в разделе «Примеры» и проиллюстрированы в части входящих соединений, описанных в примере.  
  
## <a name="example"></a>Пример  
 В следующем примере показано, как настроить узел HOST_B для входящих соединений.  
  
> [!NOTE]  
>  В этом примере используется файл сертификата, содержащий сертификата HOST_A, который создан фрагментом кода из раздела [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
```  
USE master;  
--On HOST_B, create a login for HOST_A.  
CREATE LOGIN HOST_A_login WITH PASSWORD = 'AStrongPassword!@#';  
GO  
--Create a user, HOST_A_user, for that login.  
CREATE USER HOST_A_user FOR LOGIN HOST_A_login  
GO  
--Obtain HOST_A certificate. (See the note   
--   preceding this example.)  
--Asscociate this certificate with the user, HOST_A_user.  
CREATE CERTIFICATE HOST_A_cert  
   AUTHORIZATION HOST_A_user  
   FROM FILE = 'C:\HOST_A_cert.cer';  
GO  
--Grant CONNECT permission for the server instance on HOST_A.  
GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO HOST_A_login  
GO  
```  
  
 Если предполагается работа в режиме высокого уровня безопасности с автоматическим переходом на другой ресурс, необходимо повторить те же самые этапы для настройки следящего сервера на входящие и исходящие соединения.  
  
 Дополнительные сведения о создании зеркальной базы данных, содержащей пример Transact-SQL, см. в разделе [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Пример запроса Transact-SQL, устанавливающий сеанс с режимом высокой производительности, см. в разделе [Пример. Настройка зеркального отображения базы данных с помощью сертификатов (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>Безопасность .NET Framework  
 При копировании сертификата на другую систему используйте безопасный метод копирования. Отнеситесь с особым вниманием к хранению сертификатов в безопасном месте.  
  
## <a name="see-also"></a>См. также:  
 [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [GRANT, предоставление разрешений конечной точке (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [Настройка зашифрованной зеркальной базы данных](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Диагностика конфигурации зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
