---
title: Создание конечной точки зеркального отображения базы данных (Transact-SQL)
description: Использование Transact-SQL для создания конечной точки зеркального отображения базы данных с помощью проверки подлинности Windows в SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], database mirroring
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: baf1a4b1-6790-4275-b261-490bca33bdb9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4a05341397185deefcc6de5d25ded34e910d7fac
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642161"
---
# <a name="create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql"></a>Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается создание конечной точки зеркального отображения базы данных, использующей проверку подлинности Windows, в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] при помощи среды [!INCLUDE[tsql](../../includes/tsql-md.md)]. Для поддержки зеркального отображения баз данных или [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] , каждому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] требуется конечная точка зеркального отображения базы данных. На экземпляре сервера может быть только одна конечная точка зеркального отображения базы данных, которой назначен только один порт. Конечная точка зеркального отображения базы данных может использовать любой доступный порт на локальной системе, на которой эта точка создана. Все сеансы зеркального отображения базы данных на экземпляре сервера прослушивают этот порт, и все входящие соединения для зеркального отображения базы данных используют этот порт.  
  
> [!IMPORTANT]  
>  Если конечная точка зеркального отображения базы данных существует и уже используется, рекомендуется использовать именно эту конечную точку. Удаление используемой конечной точки нарушает работу существующих сеансов.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  [Безопасность](#Security)  
  
-   **Создание конечной точки зеркального отображения базы данных с помощью различных средств.**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
 Методы проверки подлинности и шифрования для каждого экземпляра сервера устанавливаются администратором системы.  
  
> [!WARNING]  
>  Алгоритм RC4 устарел. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого рекомендуется использовать алгоритм AES.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требует разрешения `sysadmin` или членства в предопределенной роли сервера `CREATE ENDPOINT`. Дополнительные сведения см. в разделе [GRANT, предоставление разрешений на конечную точку (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-create-a-database-mirroring-endpoint-that-uses-windows-authentication"></a>Создание конечной точки зеркального отображения базы данных с проверкой подлинности Windows  
  
1.  Подключитесь к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , для которого требуется создать конечную точку зеркального отображения базы данных.  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Определите, не существует ли уже конечная точка зеркального отображения, с помощью следующей инструкции:  
  
    ```sql  
    SELECT name, role_desc, state_desc FROM sys.database_mirroring_endpoints;   
    ```  
  
    > [!IMPORTANT]  
    >  Если конечная точка зеркального отображения уже существует для данного экземпляра сервера, используйте ее для всех сеансов, устанавливаемых с этим экземпляром.  
  
4.  Чтобы создать конечную точку с проверкой подлинности Windows на языке Transact-SQL, выполните инструкцию `CREATE ENDPOINT`. Эта инструкция выглядит так:  
  
     ```syntaxsql
     CREATE ENDPOINT *\<endpointName>*  
  
     STATE=STARTED  
  
     AS TCP ( LISTENER_PORT = *\<listenerPortList>* )  
  
     FOR DATABASE_MIRRORING  
  
     (  
  
     [ AUTHENTICATION = **WINDOWS** [ *\<authorizationMethod>* ]  
  
     ]  
  
     [ [**,**] ENCRYPTION = **REQUIRED**  
  
     [ ALGORITHM { *\<algorithm>* } ]  
  
     ]  
  
     [**,**] ROLE = *\<role>*  
  
     )  
     ```
     
     Где:  
  
    -   *\<endpointName>*  — уникальное имя для конечной точки зеркального отображения базы данных экземпляра сервера.  
  
    -   STARTED указывает, что конечная точка должна запуститься и начать ожидать соединения. Обычно конечные точки зеркального отображения базы данных создаются в состоянии STARTED. Можно также начать сеанс в состоянии STOPPED (по умолчанию) или DISABLED.  
  
    -   *\<listenerPortList>*  — номер одного порта (*nnnn*), по которому сервер должен прослушивать сообщения зеркального отображения базы данных. Разрешается только протокол TCP; использование другого протокола приведет к ошибке.  
  
         Номер порта на одном компьютере может использоваться только один раз. Конечная точка зеркального отображения базы данных может использовать любой доступный порт на локальной системе, на которой эта точка создана. Чтобы определить, какие порты в данный момент используются в системе конечными точками протокола TCP, выполните следующую инструкцию Transact-SQL:  
  
        ```sql  
        SELECT name, port FROM sys.tcp_endpoints;  
        ```  
  
        > [!IMPORTANT]  
        >  У каждого экземпляра сервера должен быть один и только один уникальный прослушиваемый порт.  
  
    -   Для проверки подлинности Windows параметр AUTHENTICATION является необязательным, если не требуется, чтобы в конечной точке использовались только NTLM или Kerberos для проверки подлинности соединений. *\<authorizationMethod>* указывает метод, используемый для проверки подлинности соединений как один из следующих: NTLM, KERBEROS или NEGOTIATE. Значение по умолчанию NEGOTIATE соответствует тому, что конечная точка будет использовать протокол согласования Windows для выбора NTLM либо Kerberos. Согласование разрешает соединения без проверки подлинности или с ней, в зависимости от уровня проверки подлинности противоположной конечной точки.  
  
    -   По умолчанию параметр ENCRYPTION имеет значение REQUIRED. Это означает, что все соединения с этой конечной точкой должны использовать шифрование. Однако можно отключить шифрование или сделать его необязательным. Существуют следующие варианты.  
  
        |Значение|Определение|  
        |-----------|----------------|  
        |DISABLED|Указывает на то, что данные, передаваемые через соединение, не будут зашифрованы.|  
        |SUPPORTED|Указывает на то, что данные будут зашифрованы только в случае, если у противоположной конечной точки этот аргумент принял значение SUPPORTED или REQUIRED.|  
        |REQUIRED|Указывает на то, что данные, передаваемые через соединение, шифруются.|  
  
         Если конечная точка требует шифрования, у другой конечной точки значением параметра ENCRYPTION может быть SUPPORTED или REQUIRED.  
  
    -   *\<algorithm>* предоставляет параметр определения стандартов шифрования для конечной точки. Значение *\<algorithm>* может указывать один из следующих алгоритмов или сочетание алгоритмов: RC4, AES, AES RC4 или RC4 AES.  
  
         Алгоритм AES RC4 определяет, что данная конечная точка будет согласовывать алгоритм шифрования, отдавая предпочтение алгоритму AES. Алгоритм RC4 AES определяет, что данная конечная точка будет согласовывать алгоритм шифрования, отдавая предпочтение алгоритму RC4. Если обе конечные точки указывают оба алгоритма, но в разной последовательности, то используется та, которая указана на принимающей стороне. Явно укажите тот же алгоритм, чтобы избежать ошибок соединения между разными серверами.
  
        > [!WARNING]  
        >  Алгоритм RC4 устарел. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого рекомендуется использовать алгоритм AES.  
  
    -   *\<role>* определяет роль или роли, которые может выполнять сервер. Указание параметра ROLE необходимо. Однако роль конечной точки является значимой только для зеркального отображения базы данных. Для [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]роль конечной точки игнорируется.  
  
         Чтобы сервер мог выполнять одну роль в одном сеансе зеркального отображения базы данных и другую — в другом, укажите ROLE=ALL. Чтобы ограничить роль сервера только участником или следящим, укажите `ROLE = PARTNER` или `ROLE = WITNESS`, соответственно.  
  
        > [!NOTE]  
        >  Дополнительные сведения о параметрах зеркального отображения баз данных для различных выпусков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]см. в разделе [Функции, поддерживаемые различными выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
     Полное описание синтаксиса инструкции CREATE ENDPOINT см. в разделе [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
    > [!NOTE]  
    >  Чтобы изменить существующую конечную точку, используйте [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
###  <a name="example-creating-endpoints-to-support-for-database-mirroring-transact-sql"></a><a name="TsqlExample"></a> Пример. Создание конечных точек для поддержки функции зеркального отображения базы данных (Transact-SQL)  
 В следующем примере создаются конечные точки зеркального отображения базы данных для экземпляров сервера по умолчанию на трех разных компьютерах.  
  
|Роль экземпляра сервера|Имя главного компьютера|  
|-----------------------------|---------------------------|  
|Участник (изначально в роли основного сервера)|`SQLHOST01\.`|  
|Участник (изначально в роли зеркального сервера)|`SQLHOST02\.`|  
|Свидетель|`SQLHOST03\.`|  
  
 В этом примере все три конечные точки используют порт с номером 7022, хотя можно было бы использовать номер любого доступного порта. Параметр AUTHENTICATION необязателен, так как все конечные точки используют тип по умолчанию, проверку подлинности Windows. Параметр ENCRYPTION также необязателен, так как все конечные точки намереваются согласовывать метод проверки подлинности соединения, а это поведение по умолчанию — для проверки подлинности Windows. Также все конечные точки требуют шифрование, что также является поведением по умолчанию.  
  
 Каждый сервер ограничен ролью или участника, или следящего, и конечная точка каждого сервера явно задает эту роль (ROLE=PARTNER или ROLE=WITNESS).  
  
> [!IMPORTANT]  
>  У каждого экземпляра сервера может быть только одна конечная точка. Поэтому, если нужно, чтобы экземпляр сервера был в некоторых сеансах участником, а в некоторых — следящим, укажите ROLE=ALL.  
  
```sql  
--Endpoint for initial principal server instance, which  
--is the only server instance running on SQLHOST01.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for initial mirror server instance, which  
--is the only server instance running on SQLHOST02.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for witness server instance, which  
--is the only server instance running on SQLHOST03.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=WITNESS);  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Настройка конечной точки зеркального отображения базы данных**  
  
-   [Создание конечной точки зеркального отображения базы данных для групп доступности AlwaysOn (SQL Server PowerShell)](../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Использование сертификатов для конечной точки зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Включение использования сертификатов для исходящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Включение использования сертификатов для входящих соединений в конечной точке зеркального отображения базы данных (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Укажите URL-адрес конечной точки при добавлении или изменении реплики доступности (SQL Server)](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Просмотр сведений о конечной точке зеркального отображения базы данных**  
  
-   [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [Указание сетевого адреса сервера (зеркальное отображение базы данных)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Пример. Настройка зеркального отображения базы данных с помощью проверки подлинности Windows (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)   
 [Конечная точка зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  

