---
title: Распространенные проблемы при настройке зеркального отображения базы данных
description: Сведения об устранении неполадок при установке сеанса зеркального отображения базы данных.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- endpoints [SQL Server], database mirroring
- database mirroring [SQL Server], troubleshooting
- troubleshooting [SQL Server], database mirroring
ms.assetid: 87d3801b-dc52-419e-9316-8b1f1490946c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2b50f06d3830a8f723d9d6ecc570125ecafb136a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641546"
---
# <a name="troubleshoot-database-mirroring-configuration-sql-server"></a>Диагностика конфигурации зеркального отображения базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе приводятся сведения об устранении неполадок при установке сеанса зеркального отображения базы данных.  
  
> [!NOTE]  
>  Убедитесь в том, что выполняются все [предварительные условия для зеркального отображения базы данных](../../database-engine/database-mirroring/prerequisites-restrictions-and-recommendations-for-database-mirroring.md).  
  
|Проблема|Сводка|  
|-----------|-------------|  
|Сообщение об ошибке 1418|Данное сообщение [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] указывает на то, что сетевой адрес сервера недоступен или не существует, и предполагает выполнение проверки имени сетевого адреса и повторное выполнение команды. |  
|[Измерение счетов](#Accounts)|Обсуждаются требования к правильной настройке учетных записей, под которыми работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Конечные точки](#Endpoints)|Обсуждаются требования к правильной настройке конечной точки зеркального отображения базы данных для каждого экземпляра сервера.|  
|[SystemAddress](#SystemAddress)|Обобщаются альтернативы указанию системного имени экземпляра сервера в конфигурации зеркального отображения базы данных.|  
|[Сетевой доступ](#NetworkAccess)|Документирует требования, согласно которым каждому экземпляру сервера разрешается доступ к портам других экземпляров сервера по протоколу TCP.|  
|[Подготовка зеркальной базы данных](#MirrorDbPrep)|Обобщаются требования к подготовке зеркальной базы данных для включения зеркального отображения.|  
|[Ошибка операции по созданию файла](#FailedCreateFileOp)|Описывается обработка сбоев при выполнении операции создания файла.|  
|[Запуск зеркального отображения (язык Transact-SQL)](#StartDbm)|Описывается, в каком порядке должны выполняться инструкции ALTER DATABASE *имя_базы_данных* SET PARTNER **='** _сервер_участник_ **'** .|  
|[Межбазовые транзакции](#CrossDbTxns)|Автоматический переход на другой ресурс может привести к автоматическому и, возможно, неверному разрешению проблемных транзакций. По этой причине зеркальное отображение базы данных не поддерживает транзакции между базами данных.|  
  
##  <a name="accounts"></a><a name="Accounts"></a> Измерение счетов  
 Учетные записи, под которыми работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , должны быть правильно настроены.  
  
1.  Имеют ли учетные записи нужные разрешения?  
  
    1.  Если учетные записи выполняются в одном домене, шанс неправильной настройки уменьшается.  
  
    2.  Если учетные записи работают в разных доменах или не являются учетными записями домена, то в базе данных **master** на другом компьютере необходимо создать имя входа для учетной записи и предоставить ему разрешение CONNECT на конечную точку. Дополнительные сведения см. в статье [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md). Эти требования распространяются на учетную запись сетевой службы.  
  
2.  Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется как служба под локальной системной учетной записью, то для проверки подлинности необходимо использование сертификатов. Дополнительные сведения см. в разделе [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Конечные точки  
 Конечные точки должны быть правильно настроены.  
  
1.  Убедитесь, что в каждом экземпляре сервера (основного, зеркального и следящего, если он есть) есть конечная точка зеркального отображения базы данных. Дополнительные сведения см. в разделе [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md), а также в зависимости от режима проверки подлинности в разделе [Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) или [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Убедитесь, что номера портов правильны.  
  
     Определить порт, в настоящее время связанный с конечной точкой зеркального отображения базы данных экземпляра сервера, можно через представления каталога [sys.database_mirroring_endpoints](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) и [sys.tcp_endpoints](../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) .  
  
3.  Если при зеркальном отображении базы данных возникают труднообъяснимые неполадки, рекомендуется на каждом экземпляре сервера проверить, правильный ли порт он прослушивает.   
  
4.  Убедитесь, что конечные точки запущены (STATE=STARTED). На каждом экземпляре сервера выполните следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Дополнительные сведения о столбце **state_desc** см. в разделе [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Чтобы запустить конечную точку, выполните следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Дополнительные сведения см. в статье [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Убедитесь, что роль выбрана правильно. На каждом из экземпляров сервера выполните следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
    ```  
    SELECT role FROM sys.database_mirroring_endpoints;  
    GO  
    ```  
  
     Дополнительные сведения см. в статье [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
6.  Применение имени входа для учетной записи службы от другого экземпляра сервера требует наличия разрешения CONNECT. Убедитесь, что имени входа на другом сервере предоставлено разрешение CONNECT. Чтобы узнать, кто имеет разрешение CONNECT для конечной точки, выполните следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] на каждом экземпляре сервера:  
  
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
  
##  <a name="system-address"></a><a name="SystemAddress"></a> Системный адрес  
 В качестве системного имени экземпляра сервера в конфигурации зеркального отображения базы данных можно использовать любое имя, которое однозначно идентифицирует систему. Адрес сервера может представлять собой системное имя (если системы находятся в одном), полное доменное имя или IP-адрес (желательно статический). Полное доменное имя будет работать гарантированно. Дополнительные сведения см. в разделе [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Каждому экземпляру сервера требуется доступ к портам других экземпляров сервера по протоколу TCP. Это особенно важно, если экземпляры сервера находятся в разных доменах, не имеющих доверительных отношений друг с другом (домены без доверия). Это во многом ограничивает связь между экземплярами сервера.  
  
##  <a name="mirror-database-preparation"></a><a name="MirrorDbPrep"></a> Mirror Database Preparation  
 При первом запуске зеркального отображения или повторном запуске после удаления зеркального отображения убедитесь, что зеркальная база данных подготовлена.  
  
 При создании зеркальной базы данных на зеркальном сервере убедитесь, что резервная копия основной базы данных восстановлена. Для этого укажите имя базы данных в предложении WITH NORECOVERY. Кроме того, все резервные копии журналов, созданные после резервного копирования этой базы данных, также должны быть созданы с использованием предложения WITH NORECOVERY.  
  
 Кроме того, рекомендуется, чтобы путь к файлу зеркальной базы данных (включая имя диска) по возможности совпадал с путем к основной базе данных. Если пути к файлам должны различаться, например если основная база данных расположена на диске «F:», а в зеркальной системе нет диска «F:», необходимо включить в инструкцию RESTORE параметр MOVE.  
  
> [!IMPORTANT]  
>  Если при создании зеркальной базы данных переносятся файлы базы данных, то последующее добавление файлов к базе данных не всегда возможно без прекращения зеркального отображения.  
  
 Если зеркальное отображение базы данных остановлено, то перед его повторным запуском к зеркальной базе данных необходимо применить все последующие резервные копии журналов, полученные с основной базы данных.  
  
 Дополнительные сведения см. в статье [Prepare a Mirror Database for Mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
##  <a name="failed-create-file-operation"></a><a name="FailedCreateFileOp"></a> Failed Create-File Operation  
 Чтобы добавление файла не повлияло на сеанс зеркального отображения, путь к файлам должен существовать на обоих серверах. Поэтому перемещение файлов базы данных во время создания зеркального отображения может привести к его ошибке или остановке при выполнении операции добавления файла.  
  
 Чтобы решить эту проблему, выполните следующие действия.  
  
1.  Владелец базы данных должен удалить сеанс зеркального отображения и восстановить полную резервную копию файловой группы, содержащей добавленный файл.  
  
2.  Затем необходимо создать резервную копию журналов, содержащего операцию добавления файла, на основном сервере и вручную восстановить резервную копию на зеркальной базе данных с помощью параметров WITH NORECOVERY и WITH MOVE. Эти действия приведут к созданию указанного пути к файлу на зеркальном сервере и восстановлению нового файла по этому пути.  
  
3.  Чтобы подготовить базу данных для нового сеанса зеркального отображения, владельцу также необходимо восстановить с параметром WITH NO RECOVERY все необработанные резервные копии журналов с основного сервера.  
  
 Дополнительные сведения см. в разделе [Удаление зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md), [Подготовка зеркальной базы данных к зеркальному отображению (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md), [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md), [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md) или [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="starting-mirroring-by-using-transact-sql"></a><a name="StartDbm"></a> Запуск зеркального отображения (язык Transact-SQL)  
 Порядок выполнения инструкций ALTER DATABASE *имя_базы_данных* SET PARTNER **='** _сервер_участник_ **'** очень важен.  
  
1.  Первая инструкция должна выполняться на зеркальном сервере. В этот момент зеркальный сервер не пытается соединиться с каким бы то ни было другим экземпляром сервера. Вместо этого он предписывает своей базе данных ждать, пока основной сервер не свяжется с зеркальным сервером.  
  
2.  Вторая инструкция ALTER DATABASE должна выполняться на основном сервере. Она предписывает основному серверу подключиться к зеркальному. После создания этого соединения зеркальный сервер пытается подключиться к основному серверу через другое соединение.  
  
 Дополнительные сведения см. в разделе [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md).  
  
> [!NOTE]  
>  Сведения о запуске зеркального отображения в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] см. в разделе [Создание сеанса зеркального отображения базы данных с использованием проверки подлинности Windows (среда SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="cross-database-transactions"></a><a name="CrossDbTxns"></a> Межбазовые транзакции  
 Если база данных зеркально отображается в режиме высокого уровня безопасности с автоматической отработкой отказа, то она может привести к неверному разрешению проблемных транзакций. Если автоматическая отработка отказа на зеркальную базу данных происходит во время фиксации межбазовой транзакции, то между этими базами данных может возникнуть логическая несогласованность.  
  
 Межбазовые транзакции, на которые может повлиять автоматическая отработка отказа, могут относиться к следующим типам:  
  
-   транзакции, обновляющие несколько баз данных в одном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];  
  
-   Транзакции, использующие координатор распределенных транзакций [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC).  
  
 Дополнительные сведения см. в статье [Транзакции между базами данных и распределенные транзакции для групп доступности AlwaysOn и зеркального отображения базы данных (SQL Server)](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
## <a name="see-also"></a>См. также:  
 [Настройка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Безопасность транспорта для зеркального отображения баз данных и групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)  
  
  


