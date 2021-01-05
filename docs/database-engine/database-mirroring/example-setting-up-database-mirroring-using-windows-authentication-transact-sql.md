---
title: Настройка зеркального отображения базы данных с проверкой подлинности Windows (T-SQL)
description: В этой статье содержится пример создания сеанса зеркального отображения базы данных со следящим сервером с использованием проверки подлинности Windows с Transact-SQL в SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: 35800769-aede-4aac-b077-0e0e487e302f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 128fb94e9e99315a0a8f9d24067fc863467fb349
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641746"
---
# <a name="example-configure-database-mirroring-using-windows-authentication-transact-sql"></a>Пример Настройка зеркального отображения с использованием проверки подлинности Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом примере показаны все этапы создания сеанса зеркального отображения базы данных со следящим сервером, использующим проверку подлинности Windows. Примеры в этом подразделе используют язык [!INCLUDE[tsql](../../includes/tsql-md.md)]. Обратите внимание, что в качестве альтернативы использованию этапов [!INCLUDE[tsql](../../includes/tsql-md.md)] для установки зеркального отображения баз данных можно воспользоваться мастером конфигурации безопасности зеркального отображения баз данных. Дополнительные сведения см. в подразделе [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
## <a name="prerequisite"></a>Предварительные требования  
 В примере используется образец базы данных **AdventureWorks** , которая по умолчанию использует простую модель восстановления. Для зеркального отображения этой базы данных нужно переключить ее на модель полного восстановления. Чтобы сделать это средствами языка [!INCLUDE[tsql](../../includes/tsql-md.md)], выполните следующую инструкцию ALTER DATABASE.  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks   
SET RECOVERY FULL;  
GO  
```  
  
 Дополнительные сведения об изменении модели восстановления в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] см. в разделе [Просмотр или изменение модели восстановления базы данных (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
### <a name="permissions"></a>Разрешения  
 Требуется разрешение ALTER для базы данных и разрешение CREATE ENDPOINT либо членство в предопределенной роли сервера **sysadmin** .  
  
## <a name="example"></a>Пример  
 В этом примере два участника и следящий сервер являются экземплярами серверов по умолчанию на трех компьютерах. На трех экземплярах сервера работает один и тот же домен Windows, но в данном примере для каждого экземпляра следящего сервера предусмотрены разные учетные записи пользователя (применяемые в качестве учетной записи запуска для службы).  
  
 В следующей таблице представлены все значения, использованные в этом примере.  
  
|Начальная роль зеркального отображения|Система размещения|Учетная запись пользователя домена|  
|----------------------------|-----------------|-------------------------|  
|Основной|PARTNERHOST1|*\<Mydomain>\\<dbousername\>*|  
|Зеркальное отображение|PARTNERHOST5|*\<Mydomain>\\<dbousername\>*|  
|Свидетель|WITNESSHOST4|*\<Somedomain>\\<witnessuser\>*|  
  
1.  Создайте конечную точку на экземпляре основного сервера (экземпляр по умолчанию для PARTNERHOST1).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=PARTNER)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    -- Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
2.  Создайте конечную точку на экземпляре зеркального сервера (экземпляр по умолчанию для PARTNERHOST5).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
3.  Создайте конечную точку на экземпляре следящего сервера (экземпляр по умолчанию для WITNESSHOST4).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    --Create a login for the partner server instances,  
    --which are both running as Mydomain\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [Mydomain\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
4.  Создайте зеркальную базу данных. Дополнительные сведения см. в статье [Prepare a Mirror Database for Mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
5.  На экземпляре зеркального сервера PARTNERHOST5 установите экземпляр сервера PARTNERHOST1 в качестве участника (для этого его нужно сделать начальным экземпляром основного сервера).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1.COM:7022'  
    GO  
    ```  
  
6.  На экземпляре основного сервера PARTNERHOST1 установите экземпляр сервера PARTNERHOST5 в качестве участника (для этого нужно сделать его начальным экземпляром зеркального сервера).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5.COM:7022'  
    GO  
    ```  
  
7.  На основном сервере задайте следящий сервер (находящийся на экземпляре WITNESSHOST4).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4.COM:7022'  
    GO  
    ```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Запуск мастера настройки безопасности зеркального отображения баз данных (среда SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Настройка зеркальной базы данных на использование свойства TRUSTWORTHY (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
-   [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Пример. Настройка зеркального отображения с помощью сертификатов (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Центр безопасности для ядра СУБД SQL Server и Базы данных Azure SQL](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
