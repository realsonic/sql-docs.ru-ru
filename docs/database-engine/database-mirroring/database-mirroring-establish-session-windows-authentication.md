---
title: Настройка зеркального отображения базы данных
description: Действия по настройке связи зеркального отображения базы данных между основным и зеркальным экземплярами с использованием проверки подлинности Windows.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 143c68a5-589f-4e7f-be59-02707e1a430a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7584e4611d957f48efcb73d12b5965f0a4727b45
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642040"
---
# <a name="configure-database-mirroring"></a>Настройка зеркального отображения базы данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 После того как зеркальная база данных готова (см. раздел [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)), можно установить сеанс зеркального отображения базы данных. Экземпляры участника, зеркала и следящего сервера должны быть отдельными экземплярами сервера, расположенными на отдельных системных узлах.  
  
> [!IMPORTANT]
>  Настройку зеркального отображения базы данных рекомендуется выполнять в часы с наименьшей загрузкой, поскольку этот процесс может повлиять на производительность.  
> 
> [!NOTE]
>  Указанный экземпляр сервера может быть задействован в нескольких одновременных сеансах зеркального отображения базы данных с одними и теми же или разными участниками. В некоторых сеансах указанный экземпляр сервера может быть участником, в других — следящим сервером. На экземпляре зеркального сервера должен работать тот же выпуск [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , что и на экземпляре основного сервера. Зеркальное отображение баз данных поддерживается не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Кроме того, настоятельно рекомендуется размещать серверы в системах со сравнимой производительностью, которые могут справляться с одинаковой рабочей нагрузкой.  
  
### <a name="to-establish-a-database-mirroring-session"></a>Установка сеанса зеркального отображения базы данных  
  
1.  Создайте зеркальную базу данных. Дополнительные сведения см. в статье [Prepare a Mirror Database for Mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
2.  Настройка безопасности на каждом из экземпляров сервера.  
  
     В сеансе зеркального отображения базы данных каждому экземпляру сервера требуется конечная точка зеркального отображения базы данных. Если конечная точка отсутствует, ее необходимо создать.  
  
    > [!NOTE]  
    >  Метод проверки подлинности, применяемый экземпляром сервера при зеркальном отображении базы данных, является свойством его конечной точки зеркального отображения базы данных. Для зеркального отображения базы данных доступны два типа защиты транспорта: проверка подлинности Windows или проверка подлинности на основе сертификата. Дополнительные сведения см. в статье [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     На каждом сервере-партнере убедитесь, что конечная точка существует для зеркального отображения базы данных. Независимо от поддерживаемого количества сеансов зеркального отображения экземпляр сервера может иметь только одну конечную точку зеркального отображения базы данных. Если этот экземпляр сервера будет использоваться исключительно для участников сеансов зеркального отображения базы данных, можно присвоить роль участника конечной точке (ROLE **=** PARTNER). Если этот сервер будет использоваться также в качестве следящего сервера в других сеансах зеркального отображения базы данных, присвойте роли конечной точки значение ALL.  
  
     Для выполнения инструкции SET PARTNER требуется, чтобы параметры STATE конечных точек обоих участников имели значение STARTED.  
  
     Чтобы узнать, имеет ли экземпляр сервера конечную точку зеркального отображения базы данных, а также узнать ее роль и состояние на этом экземпляре, используйте следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Не изменяйте конфигурацию используемой конечной точки зеркального отображения базы данных. Если конечная точка зеркального отображения базы данных существует и уже используется, рекомендуется использовать эту конечную точку для каждого сеанса экземпляра сервера. Удаление используемой конечной точки приведет к перезапуску конечной точки, завершению всех соединений всех существующих сеансов, что может привести к ошибке для других экземпляров сервера. Это является особо важным в режиме повышенной безопасности с автоматической отработкой отказа. В данном режиме изменение конфигурации конечной точки участника может привести к выполнению отработки отказа. Если для сеанса указан следящий сервер, удаление конечных точек зеркального отображения базы данных может привести к тому, что основной сервер потеряет кворум, поэтому база данных будет переведена в режим «вне сети» и ее пользователи будут отключены. Дополнительные сведения см. в статье [Кворум. Как следящий сервер влияет на доступность базы данных &#40;зеркальное отображение базы данных&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Если каждый участник нуждается в конечной точке, см. раздел [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
3.  Если экземпляры участников запущены под другими учетными записями пользователей домена, для каждой требуется имя входа в базе данных **master** . Если имя входа отсутствует, его необходимо создать. Дополнительные сведения см. в разделе [Allow Network Access to a Database Mirroring Endpoint Using Windows Authentication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
4.  Для установка основного сервера в качестве участника на зеркальной базе данных, подключитесь к зеркальному серверу и выполните следующую инструкцию:  
  
     ALTER DATABASE *\<database_name\>* SET PARTNER **=** _\<server\_network\_address\>_  
  
     где _\<database\_name\>_  — имя отображаемой базы данных (это имя одинаково на обоих участниках), а _\<server\_network\_address\>_  — сетевой адрес основного сервера.  
  
     Чтобы указать сетевой адрес сервера, используйте следующий синтаксис:  
  
     TCP <b>\://</b> _\<system-address\>_ <b>\:</b> _\<port\>_  
  
     где _\<system-address>_  — строка, однозначно идентифицирующая целевой компьютер, а _\<port>_  — номер порта, используемого конечной точкой зеркального отображения экземпляра сервера-участника. Дополнительные сведения см. в разделе [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Например, на экземпляре зеркального сервера следующая инструкция ALTER DATABASE устанавливает участника в качестве исходного экземпляра основного сервера: Имя базы данных — **AdventureWorks**, системный адрес — DBSERVER1 (имя системы участника), а 7022 — порт, используемый конечной точкой зеркального отображения базы данных участника:  
  
    ```  
    ALTER DATABASE AdventureWorks   
       SET PARTNER = 'TCP://DBSERVER1:7022'  
    ```  
  
     Эта инструкция подготавливает зеркальный сервер для формирования сеанса, когда с ним соединяется основной сервер.  
  
5.  Для установки зеркального сервера в качестве участника на основной базе данных, подключитесь к основному серверу и выполните следующую инструкцию:  
  
     ALTER DATABASE _\<database\_name\>_ SET PARTNER **=** _\<server\_network\_address\>_  
  
     Дополнительные сведения см. в шаге 4.  
  
     Например, в экземпляре основного сервера следующая инструкция ALTER DATABASE устанавливает участника в качестве исходного экземпляра зеркального сервера: Имя базы данных — **AdventureWorks**, системный адрес — DBSERVER2 (имя системы участника), а 7025 — порт, используемый конечной точкой зеркального отображения базы данных участника:  
  
    ```  
    ALTER DATABASE AdventureWorks SET PARTNER = 'TCP://DBSERVER2:7022'  
    ```  
  
     Ввод этой инструкции на основном сервере начинает сеанс зеркального отображения базы данных.  
  
6.  По умолчанию сеанс установлен в состояние полной безопасности транзакций (параметр SAFETY установлен в FULL), что способствует запуску сеанса в синхронном, высокого уровня защиты режиме без автоматической отработки отказа. Можно перенастроить сеанс для выполнения либо в режиме высокого уровня защиты с автоматической отработкой отказа, либо в асинхронном режиме высокого уровня производительности, как описано ниже.  
  
    -   **Режим высокого уровня защиты с автоматической отработкой отказа**  
  
         Чтобы сеанс высокого уровня защиты поддерживал автоматическую отработку отказа, добавьте экземпляр следящего сервера. Дополнительные сведения см. в разделе [Добавление следящего сервера для зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
    -   **Высокопроизводительный режим**  
  
         С другой стороны, если автоматическая отработка отказа нежелательна или важней производительность, а не высокий уровень доступности, можно выключить безопасность транзакций. Дополнительные сведения см. в разделе [Изменение безопасности транзакций в сеансах зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  В высокопроизводительном режиме параметр WITNESS должен быть установлен в OFF. Дополнительные сведения см. в статье [Кворум. Как следящий сервер влияет на доступность базы данных &#40;зеркальное отображение базы данных&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
## <a name="example"></a>Пример  
  
> [!NOTE]  
>  Следующий пример устанавливает сеанс зеркального отображения базы данных между участниками для существующей зеркальной базы данных. Дополнительные сведения о создании зеркальной базы данных см. в разделе [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Пример показывает основные шаги для создания сеанса зеркального отображения базы данных без следящего сервера. Эти два участника являются экземплярами сервера по умолчанию на двух компьютерных системах (PARTNERHOST1 и PARTNERHOST5). Оба экземпляра-участника выполняются под одной и той же учетной записью пользователя домена Windows (MYDOMAIN\dbousername).  
  
1.  На экземпляре основного сервера (экземпляр по умолчанию на PARTNERHOST1) создается конечная точка, которая поддерживает все роли, используя порт 7022:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
    > [!NOTE]  
    >  Пример настройки имени входа см. в разделе [Разрешение сетевого доступа к конечной точке зеркального отображения базы данных с использованием проверки подлинности Windows (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
2.  На экземпляре зеркального сервера (экземпляр по умолчанию на PARTNERHOST5) создается конечная точка, которая поддерживает все роли, используя порт 7022:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
3.  На экземпляре основного сервера (на PARTNERHOST1) создается резервная копия базы данных:  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdvWorks_dbmirror.bak'   
        WITH FORMAT  
    GO  
    ```  
  
4.  На экземпляре зеркального сервера (на компьютере `PARTNERHOST5`) восстанавливается база данных:  
  
    ```  
    RESTORE DATABASE AdventureWorks   
        FROM DISK = 'Z:\AdvWorks_dbmirror.bak'   
        WITH NORECOVERY  
    GO  
    ```  
  
5.  После того, как полная резервная копия базы данных создана, обязательно создается резервная копия журнала для основной базы данных. В следующем примере с помощью инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] создается резервная копия журналов для того же файла, который использовался в предыдущей резервной копии базы данных:  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Перед тем как приступать к зеркальному отображению, необходимо применить требуемую резервную копию журналов (и все последующие резервные копии журналов).  
  
     В следующем примере с помощью инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] восстанавливается первый журнал из файла «C:\AdventureWorks.bak»:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\ AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  На экземпляре зеркального сервера установите экземпляр сервера на PARTNERHOST1 в качестве участника (делая его исходным основным сервером):  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1:7022'  
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  По умолчанию сеанс зеркального отображения базы данных выполняется в синхронном режиме, который зависит от наличия полной безопасности транзакций (параметр SAFETY установлен в FULL). Чтобы сеанс выполнялся в асинхронном, высокопроизводительном режиме, установите параметр SAFETY в OFF. Дополнительные сведения см. в статье [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
8.  На экземпляре основного сервера установите экземпляр сервера на компьютере `PARTNERHOST5` в качестве участника (сделав его исходным зеркальным сервером):  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5:7022'  
    GO  
    ```  
  
9. Если необходимо использовать режим высокого уровня безопасности с автоматической отработкой отказа, дополнительно установите экземпляр следящего сервера. Дополнительные сведения см. в разделе [Добавление следящего сервера для зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
> [!NOTE]  
>  Подробный пример настройки зеркального отображения базы данных, в котором показана настройка защиты, подготовка зеркальной базы данных, настройка партнеров и добавление следящего сервера, см. в разделе [Настройка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>См. также:  
 [Настройка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [Разрешение сетевого доступа к конечной точке зеркального отображения базы данных с использованием проверки подлинности Windows (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Зеркальное отображение баз данных и доставка журналов (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)   
 [Зеркальное отображение базы данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Зеркальное отображение и репликация баз данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [Настройка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Режимы работы зеркального отображения базы данных](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  

