---
title: Настройка маршрутизации только для чтения в группе доступности
description: Вы можете автоматически направлять весь трафик только для чтения к вторичной реплике с помощью маршрутизации только для чтения для группы доступности Always On — с помощью Transact-SQL (T-SQL) или PowerShell.
ms.custom: seodec18
ms.date: 02/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 7bd89ddd-0403-4930-a5eb-3c78718533d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: f7e96df4eba36bbcb3da18a1423b5162aef557a7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170806"
---
# <a name="configure-read-only-routing-for-an-always-on-availability-group"></a>Настройка маршрутизации только для чтения в группе доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Чтобы настроить группу доступности AlwaysOn для поддержки маршрутизации только для чтения в [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], можно использовать процедуру [!INCLUDE[tsql](../../../includes/tsql-md.md)] или PowerShell. *Маршрутизация только для чтения* означает способность [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] направлять уточняющие запросы на соединение только для чтения к имеющейся [доступной для чтения вторичной реплике](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) AlwaysOn (то есть реплике, настроенной для разрешения рабочих нагрузок только для чтения при выполнении вторичной роли). Для поддержки маршрутизации только для чтения группа доступности должна иметь [прослушиватель группы доступности](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md). Клиент, запрашивающий данные в режиме только чтения, должен направлять свои запросы к данному прослушивателю, а строки подключения клиента должны определять намерение приложения как «только для чтения». Это означает, что они должны быть *запросами на соединение с правами чтения*.  

Маршрутизация только для чтения доступна в [!INCLUDE[sssql15](../../../includes/sssql16-md.md)] и более поздних версиях.

> [!NOTE]  
>  Дополнительные сведения о настройке доступной для чтения вторичной реплики см. в разделе [Настройка доступа только для чтения в реплике доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Группа доступности должна обладать прослушивателем группы доступности. Дополнительные сведения см. в разделе [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
-   Одна или несколько реплик доступности должны быть настроены для принятия запросов только для чтения во вторичной роли (то есть являться [предназначенными для чтения вторичными репликами](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)). Дополнительные сведения см. в разделе [Настройка доступа только для чтения в реплике доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  
  
-   Необходимо подключиться к экземпляру сервера, на котором размещена текущая первичная реплика.  

-   Если вы используете вход SQL, убедитесь, что учетная запись настроена корректно. Подробные сведения см. в статье [Управление именами входа для заданий, использующих базы данных в группе доступности Always On](logins-and-jobs-for-availability-group-databases.md).
  
##  <a name="what-replica-properties-do-you-need-to-configure-to-support-read-only-routing"></a><a name="RORReplicaProperties"></a> Какие свойства реплики необходимо настроить для поддержки маршрутизации только для чтения?  
  
-   Для каждой доступной для чтения вторичной реплики, которая поддерживает маршрутизацию только для чтения, необходимо указать *URL-адрес маршрутизации только для чтения*. Этот URL-адрес задействуется, только если локальная реплика выполняется под вторичной ролью. URL-адрес маршрутизации только для чтения должен быть указан для каждой реплики отдельно (если для реплики требуется подобная маршрутизация). Все URL-адреса маршрутизации только для чтения используются для направления запросов на соединение с намерением чтения к определенной доступной для чтения вторичной реплике. Как правило, каждой доступной для чтения вторичной реплике назначается URL-адрес маршрутизации только для чтения.  
  
     Сведения о вычислении URL-адреса маршрутизации только для чтения для реплики доступности см. в разделе [Вычисление значения read_only_routing_url для AlwaysOn](https://web.archive.org/web/20170512023255/https://blogs.msdn.microsoft.com/mattn/2012/04/25/calculating-read_only_routing_url-for-alwayson/)
  
-   Для каждой реплики доступности, которая должна поддерживать маршрутизацию только для чтения и при этом является первичной, необходимо задать *список маршрутизации только для чтения*. Определенный список маршрутизации только для чтения вступает в силу, только если локальная реплика выполняется под первичной ролью. Такой список должен указываться для тех конкретных реплик, для которых он требуется. Как правило, каждый список маршрутизации только для чтения будет содержать все URL-адреса маршрутизации только для чтения, причем URL-адрес локальной реплики будет идти в конце списка.  
  
    > [!NOTE]  
    >  Запросы на подключение для чтения направляются в первую имеющуюся запись из списка маршрутизации только для чтения текущей первичной реплики. Однако для реплик только для чтения поддерживается балансировка нагрузки. Дополнительные сведения см. в разделе [Настройка балансировки нагрузки между репликами только для чтения](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
> [!NOTE]  
>  Сведения о прослушивателях групп доступности и дополнительные сведения о маршрутизации только для чтения см. в разделе [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
  
|Задача|Разрешения|  
|----------|-----------------|  
|Настройка реплик при создании группы доступности|Требуется членство в фиксированной роли сервера **sysadmin** и одно из разрешений: CREATE AVAILABILITY GROUP, ALTER ANY AVAILABILITY GROUP или CONTROL SERVER.|  
|Изменение реплики доступности|Необходимо разрешение ALTER AVAILABILITY GROUP для группы доступности, разрешение CONTROL AVAILABILITY GROUP, разрешение ALTER ANY AVAILABILITY GROUP или разрешение CONTROL SERVER.|  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
### <a name="configure-a-read-only-routing-list"></a>Настройка списка маршрутизации только для чтения  
 Выполните действия ниже, чтобы настроить маршрутизацию только для чтения с помощью Transact-SQL. Пример кода см. в подразделе [Пример (Transact-SQL)](#TsqlExample)далее в этом разделе.  
  
1.  Подключитесь к экземпляру сервера, на котором находится первичная реплика.  
  
2.  Если вы указываете реплику для новой группы доступности, воспользуйтесь инструкцией [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Если вы добавляете или изменяете реплику для существующей группы доступности, воспользуйтесь инструкцией [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Чтобы настроить маршрутизацию только для чтения для вторичной роли, укажите в предложении ADD REPLICA или MODIFY REPLICA WITH параметр SECONDARY_ROLE следующим образом:  
  
         SECONDARY_ROLE **(** READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **')**  
  
         Существуют следующие параметры URL-адреса маршрутизации только для чтения.  
  
         *system-address*  
         Это строка, такая как адрес системы, полное доменное имя или IP-адрес, однозначно идентифицирующий целевую компьютерную систему.  
  
         *port*  
         Номер порта, который используется компонентом ядра СУБД экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
         Например:   `SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433')`  
  
         В предложении MODIFY REPLICA параметр ALLOW_CONNECTIONS не является обязательным, если реплика уже настроена для соединений только для чтения.  
  
         Дополнительные сведения см. в разделе [Вычисление значения read_only_routing_url для AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Чтобы настроить маршрутизацию только для чтения для первичной роли, в предложении ADD REPLICA или MODIFY REPLICA WITH укажите параметр PRIMARY_ROLE следующим образом:  
  
         PRIMARY_ROLE **(** READ_ONLY_ROUTING_LIST **=('** _server_ **'** [ **,** ...*n* ] **))**  
  
         Здесь *server* идентифицирует экземпляр сервера, на котором размещена вторичная реплика только для чтения в группе доступности.  
  
         Например:   `PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('Server1','Server2'))`  
  
        > [!NOTE]  
        >  Необходимо настроить URL-адрес маршрутизации только для чтения перед настройкой списка маршрутизации только для чтения.  
  
###  <a name="configure-load-balancing-across-read-only-replicas"></a><a name="loadbalancing"></a> Настройка балансировки нагрузки между репликами только для чтения  
 Начиная с версии [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)]балансировку нагрузки можно настроить в наборе реплик только для чтения. Раньше при маршрутизации только для чтения трафик всегда направлялся к первой доступной реплике только для чтения в списке маршрутизации. Чтобы воспользоваться этой функцией, используйте один уровень вложенных скобок вокруг экземпляров сервера **READ_ONLY_ROUTING_LIST** в команде **CREATE AVAILABILITY GROUP** или **ALTER AVAILABILITY GROUP** .  
  
 Например, в приведенном ниже списке маршрутизации запрос на подключение для чтения равномерно распределяется между двумя репликами только для чтения: `Server1` и `Server2`. Вложенные скобки вокруг этих серверов определяют набор с балансировкой нагрузки. Если в этом наборе ни одна из реплик недоступна, будет происходить дальнейшее последовательное подключение к другим репликам в списке маршрутизации только для чтения: `Server3` и `Server4`.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), 'Server3', 'Server4')  
```  
  
 Обратите внимание, что каждая запись в списке маршрутизации может быть набором реплик только для чтения с балансировкой нагрузки. Это продемонстрировано в следующем примере.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), ('Server3', 'Server4', 'Server5'), 'Server6')  
```  
  
 Поддерживается только один уровень вложенных скобок.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 В следующем примере изменяются две реплики доступности существующей группы доступности `AG1` для поддержки маршрутизации только для чтения в том случае, если одна из этих реплик в настоящий момент обладает первичной ролью. Чтобы определить экземпляры сервера, на которых размещена реплика доступности, в этом примере указаны имена экземпляров —`COMPUTER01` и `COMPUTER02`.  
  
```  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
GO  
  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование PowerShell  
  
### <a name="configure-a-read-only-routing-list"></a>Настройка списка маршрутизации только для чтения  
 Выполните действия ниже, чтобы настроить маршрутизацию только для чтения с помощью PowerShell. Пример кода см. в подразделе [Пример (PowerShell)](#PSExample)далее в этом разделе.  
  
1.  Установите значение по умолчанию (**cd**) равным экземпляру сервера, на котором размещена первичная реплика.  
  
2.  При добавлении реплики доступности в группу доступности воспользуйтесь командлетом **New-SqlAvailabilityReplica** . При изменении существующей реплики доступности воспользуйтесь командлетом **Set-SqlAvailabilityReplica** . Соответствующие параметры:  
  
    -   Чтобы настроить маршрутизацию только для чтения для вторичной роли, укажите параметр **ReadonlyRoutingConnectionUrl"** _url_ **"** .  
  
         Здесь *url* — это полное доменное имя и порт, которые используются для маршрутизации к реплике соединений только для чтения. Например:  `-ReadonlyRoutingConnectionUrl "TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024"`  
  
         Дополнительные сведения см. в разделе [Вычисление значения read_only_routing_url для AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Чтобы настроить доступ соединения для первичной роли, укажите **ReadonlyRoutingList"** _server_ **"** [ **,** ...*n* ], где *server* обозначает экземпляр сервера, на котором размещена вторичная реплика только для чтения в группе доступности. Например:  `-ReadOnlyRoutingList "SecondaryServer","PrimaryServer"`  
  
        > [!NOTE]  
        >  Необходимо настроить URL-адрес маршрутизации только для чтения для реплики перед тем, как перейти к настройке ее списка маршрутизации.  
  
    > [!NOTE]  
    >  Чтобы просмотреть синтаксис командлета, воспользуйтесь командлетом **Get-Help** в среде PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
### <a name="set-up-and-use-the-sql-server-powershell-provider"></a>Настройка и использование поставщика SQL Server PowerShell  
  
-   [Поставщик SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Получение справок по SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Пример (PowerShell)  
 В следующем примере выполняется настройка первичной реплики и одной вторичной реплики в группе доступности с использованием маршрутизации только для чтения. С начала примера каждой реплике присваивается URL-адрес для маршрутизации только для чтения. Затем для первичной реплики задается список маршрутизации только для чтения. Соединения со свойством «ReadOnly» в строке подключения будут перенаправляться на вторичную реплику. Если такая вторичная реплика недоступна для чтения (в соответствии со значением параметра **ConnectionModeInSecondaryRole** ), подключение направляется обратно в первичную реплику.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
$secondaryReplica = Get-Item "AvailabilityReplicas\SecondaryServer"  
  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://PrimaryServer.domain.com:1433" -InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://SecondaryServer.domain.com:1433" -InputObject $secondaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingList "SecondaryServer","PrimaryServer" -InputObject $primaryReplica  
```  
  
##  <a name="follow-up-after-configuring-read-only-routing"></a><a name="FollowUp"></a> Дальнейшие действия. После настройки маршрутизации только для чтения  
 Как только текущая первичная реплика и предназначенные для чтения вторичные реплики будут настроены для поддержки маршрутизации только для чтения в обеих ролях, предназначенные для чтения вторичные реплики смогут принимать запросы соединения с намерением чтения от клиентов, которые подключаются через прослушиватель группы доступности.  
  
> [!TIP]  
>  При использовании [bcp Utility](../../../tools/bcp-utility.md) или [sqlcmd Utility](../../../tools/sqlcmd-utility.md)можно указать доступ только для чтения к любой вторичной реплике, которой разрешен доступ только для чтения. Для этого нужно указать параметр **-K ReadOnly** .  
  
###  <a name="requirements-and-recommendations-for-client-connection-strings"></a><a name="ConnStringReqsRecs"></a> Требования и рекомендации для строк подключения клиента  
 В случае если клиентское приложение использует маршрутизацию только для чтения, его строка подключения должна удовлетворять следующим требованиям.  
  
-   Используйте протокол TCP.  
  
-   Задайте атрибут/свойство намерения приложения как «только для чтения».  
  
-   Создайте ссылку на прослушиватель группы доступности, настроенный для поддержки маршрутизации только для чтения.  
  
-   Сошлитесь на базу данных в этой группе доступности.  
  
 Кроме того, рекомендуется, чтобы строки подключения использовали отработку отказа резервных подсетей при сбое, что поддерживает параллельный клиентский поток для каждой реплики в каждой подсети. Это позволяет свести к минимуму повторное подключение клиента после отработки отказа.  
  
 Синтаксис строки подключения зависит от поставщика SQL Server, который использует приложение. Следующий пример строки подключения для поставщика данных .NET Framework 4.0.2 для SQL Server демонстрирует фрагменты строки подключения, которые необходимы и рекомендуются для работы с маршрутизацией только для чтения.  
  
```  
Server=tcp:MyAgListener,1433;Database=Db1;IntegratedSecurity=SSPI;ApplicationIntent=ReadOnly;MultiSubnetFailover=True  
```  
  
 Дополнительные сведения о намерении приложения и маршрутизации только для чтения см. в разделе [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
### <a name="if-read-only-routing-is-not-working-correctly"></a>Маршрутизация только для чтения работает неправильно  
 Дополнительные сведения об устранении неполадок с конфигурацией маршрутизации только для чтения см. в разделе [Маршрутизация только для чтения работает неправильно](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md#ROR).  
  
##  <a name="next-steps"></a><a name="RelatedTasks"></a> Следующие шаги 
**Просмотр конфигурации маршрутизации только для чтения**  
  
-   [sys.availability_read_only_routing_lists (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md) (столбец **read_only_routing_url**)  
  
**Настройка доступа соединения клиентов**  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Настройка доступа только для чтения в реплике доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
**Использование строк подключения в приложениях**  
  
-   [Поддержка высокого уровня доступности и аварийного восстановления собственного клиента SQL Server](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
  
-   [Использование ключевых слов строки подключения с собственным клиентом SQL Server](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
**Блоги**  
  
-    [Вычисление значения read_only_routing_url для AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)  
  
-    [Блоги команды разработчиков SQL Server Always On: официальный блог по SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
-    [Блоги инженеров CSS SQL Server](/archive/blogs/psssql/)  
  
**Технические документы**  
  
-    [Технические документы Майкрософт Microsoft по SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
-    [Технические документы группы консультантов по SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  

**Дополнительное содержимое**

- [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

- [Активные вторичные реплики: вторичные реплики для чтения (группы доступности Always On)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   

- [Сведения о доступе клиентского подключения к репликам доступности (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 
- [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)