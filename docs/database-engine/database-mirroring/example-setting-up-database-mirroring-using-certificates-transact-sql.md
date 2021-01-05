---
title: Пример Зеркальное отображение базы данных с сертификатами (T-SQL)
description: В этой статье приведен пример настройки зеркального отображения базы данных SQL Server с использованием проверки подлинности на основе сертификатов с помощью Transact-SQL в SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- certificates [SQL Server], database mirroring
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: df489ecd-deee-465c-a26a-6d1bef6d7b66
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a6b1093f9c648d8753b68b9c9905dfe072ce4210
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639971"
---
# <a name="example-setting-up-database-mirroring-using-certificates-transact-sql"></a>Пример Настройка зеркального отображения с помощью сертификатов (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом примере описаны все действия, выполняемые при создании сеанса зеркального отображения базы данных с использованием проверки подлинности на основе сертификатов. Примеры в этом подразделе используют язык [!INCLUDE[tsql](../../includes/tsql-md.md)]. За исключением случаев, когда сеть гарантированно защищена, рекомендуется для соединений зеркального отображения базы данных применять шифрование.  
  
 При копировании сертификата на другую систему используйте безопасный метод копирования. Отнеситесь с особым вниманием к хранению сертификатов в безопасном месте.  
  
##  <a name="example"></a><a name="ExampleH2"></a> Пример  
 В следующем примере показано, что необходимо сделать на одном участнике, который находится на узле HOST_A. В этом примере два участника являются экземплярами сервера по умолчанию в трех компьютерных системах. Два экземпляра сервера запущены в ненадежных доменах Windows, поэтому необходима проверка подлинности на основе сертификата.  
  
 Начальная основная роль принимается узлом HOST_A, а зеркальная роль — узлом HOST_B.  
  
 Настройка зеркального отображения базы данных с помощью сертификатов состоит из четырех основных этапов, три из которых — 1, 2 и 4 — показаны в этом примере. Ниже приведены эти этапы.  
  
1.  [Настройка исходящих соединений](#ConfiguringOutboundConnections)  
  
     В этом примере приводится пошаговое описание следующих процессов.  
  
    1.  Настройка узла Host_A для исходящих соединений.  
  
    2.  Настройка узла Host_B для исходящих соединений.  
  
     Сведения об этом этапе настройки зеркального отображения базы данных см. в разделе [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  [Настройка входящих соединений](#ConfigureInboundConnections)  
  
     В этом примере приводится пошаговое описание следующих процессов.  
  
    1.  Настройка узла Host_A для входящих соединений.  
  
    2.  Настройка узла Host_B для входящих соединений.  
  
     Сведения об этом этапе настройки зеркального отображения базы данных см. в разделе [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
3.  Создание зеркальной базы данных  
  
     Дополнительные сведения о создании зеркальной базы данных см. в разделе [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
4.  [Настройка участников зеркального отображения](#ConfigureMirroringPartners)  
  
###  <a name="configuring-outbound-connections"></a><a name="ConfiguringOutboundConnections"></a> Настройка исходящих соединений  
 **Настройка узла Host_A для исходящих соединений**  
  
1.  При необходимости создайте в базе данных master главный ключ базы данных.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
2.  Сделайте сертификат для данного экземпляра сервера.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate';  
    GO  
    ```  
  
3.  Создайте конечную точку для экземпляра сервера с использованием его сертификата.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_A_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Создайте резервную копию сертификата HOST_A и скопируйте ее на другую систему HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
5.  С помощью безопасного метода скопируйте файл «C:\HOST_A_cert.cer» на узел HOST_B.  
  
 **Настройка узла Host_B для исходящих соединений**  
  
1.  При необходимости создайте в базе данных master главный ключ базы данных.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
    GO  
    ```  
  
2.  Создайте сертификат для экземпляра сервера HOST_B.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert   
       WITH SUBJECT = 'HOST_B certificate for database mirroring';  
    GO  
    ```  
  
3.  Создайте конечную точку зеркального отображения для экземпляра сервера на узле HOST_B.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_B_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Создайте резервную копию сертификата HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
    GO   
    ```  
  
5.  С помощью безопасного метода скопируйте файл «C:\HOST_B_cert.cer» на узел HOST_A.  
  
 Дополнительные сведения см. в разделе [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 [&#91;В начало примера&#93;](#ExampleH2)  
  
###  <a name="configuring-inbound-connections"></a><a name="ConfigureInboundConnections"></a> Настройка входящих соединений  
 **Настройка узла Host_A для входящих соединений**  
  
1.  Создайте имя входа на узле HOST_A для узла HOST_B.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
2.  Создайте пользователя для этого имени входа.  
  
    ```  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
3.  Свяжите сертификат с пользователем.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
4.  Предоставьте данной учетной записи разрешение CONNECT на эту удаленную конечную точку зеркального отображения.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
 **Настройка узла Host_B для входящих соединений**  
  
1.  Создайте имя входа на узле HOST_B для узла HOST_A.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_A_login WITH PASSWORD = '=Sample#2_Strong_Password2';  
    GO  
    ```  
  
2.  Создайте пользователя для этого имени входа.  
  
    ```  
    CREATE USER HOST_A_user FOR LOGIN HOST_A_login;  
    GO  
    ```  
  
3.  Свяжите сертификат с пользователем.  
  
    ```  
    CREATE CERTIFICATE HOST_A_cert  
       AUTHORIZATION HOST_A_user  
       FROM FILE = 'C:\HOST_A_cert.cer'  
    GO  
    ```  
  
4.  Предоставьте данной учетной записи разрешение CONNECT на эту удаленную конечную точку зеркального отображения.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_A_login];  
    GO  
    ```  
  
> [!IMPORTANT]  
>  При необходимости запуска в режиме высокого уровня защиты с автоматической отработкой отказа нужно повторить эти шаги установки, чтобы настроить следящий сервер для исходящих и входящих соединений. При настройке входящих соединений с задействованным следящим сервером необходимо настроить имена входа и пользователей для следящего сервера на обоих участниках и для обоих участников на следящем сервере.  
  
 Дополнительные сведения см. в разделе [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 [&#91;В начало примера&#93;](#ExampleH2)  
  
### <a name="creating-the-mirror-database"></a>Создание зеркальной базы данных  
 Дополнительные сведения о создании зеркальной базы данных см. в разделе [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
###  <a name="configuring-the-mirroring-partners"></a><a name="ConfigureMirroringPartners"></a> Настройка участников зеркального отображения  
  
1.  В экземпляре зеркального сервера, расположенного на узле HOST_B, установите в качестве участника экземпляр сервера, расположенного на узле HOST_A (сделав его начальным экземпляром основного сервера). Замените допустимый сетевой адрес на `TCP://HOST_A.Mydomain.Corp.Adventure-Works``.com:7024`. Дополнительные сведения см. в разделе [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
    ```  
    --At HOST_B, set server instance on HOST_A as partner (principal server):  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_A.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
2.  В экземпляре основного сервера, расположенного на узле HOST_A, установите в качестве участника экземпляр сервера, расположенного на узле HOST_B (сделав его начальным экземпляром зеркального сервера). Замените допустимый сетевой адрес на `TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024`.  
  
    ```  
    --At HOST_A, set server instance on HOST_B as partner (mirror server).  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
3.  В этом примере предполагается, что сеанс будет выполнен в режиме высокой производительности. Чтобы настроить этот сеанс для использования режима высокой производительности, в экземпляре основного сервера (на узле HOST_A) установите безопасность транзакций в положение OFF.  
  
    ```  
    --Change to high-performance mode by turning off transacton safety.  
    ALTER DATABASE AdventureWorks   
        SET PARTNER SAFETY OFF  
    GO  
    ```  
  
    > [!NOTE]  
    >  При необходимости запуска в режиме высокого уровня защиты с автоматической отработкой отказа оставьте безопасность транзакций в положении FULL (установка по умолчанию) и добавьте следящий сервер как можно быстрее после выполнения второй инструкции SET PARTNER **'** _сервер_партнер_ **'** . Обратите внимание, что следящий сервер вначале нужно настроить для исходящих и входящих соединений.  
  
 [&#91;В начало примера&#93;](#ExampleH2)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Управление именами входа и заданиями после переключения ролей (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
-   [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md) (SQL Server)  
  
-   [Диагностика конфигурации зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [Центр безопасности для ядра СУБД SQL Server и Базы данных Azure SQL](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
