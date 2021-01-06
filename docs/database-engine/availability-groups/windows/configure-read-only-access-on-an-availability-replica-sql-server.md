---
title: Настройка доступа только для чтения к вторичной реплике группы доступности
description: 'Настройте вторичную реплику в группе доступности Always On так, чтобы разрешить доступ только для чтения. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 22387419-22c4-43fa-851c-5fecec4b049b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9c2b804263539bb195ab8270a57dd8258811d2d4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644372"
---
# <a name="configure-read-only-access-to-a-secondary-replica-of-an-always-on-availability-group"></a>Настройка доступа только для чтения к вторичной реплике в группе доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  По умолчанию и доступ для чтения и записи, и доступ только для чтения разрешены в первичной реплике, а подключения к вторичным репликам группы доступности AlwaysOn запрещены. В этом разделе описывается настройка доступа к соединениям реплики доступности в группе доступности AlwaysOn в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]или PowerShell.  
  
 Сведения о последствиях включения доступа только для чтения во вторичной реплике и обзор доступа к соединениям см. в статьях [Сведения о доступе клиентского подключения к репликам доступности (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md) и [Активные вторичные реплики: вторичные реплики для чтения (группы доступности Always On)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Требования и ограничения  
  
-   Если нужно настроить разный доступ к подключениям, необходимо подключиться к экземпляру сервера, на котором размещается первичная реплика.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
  
|Задача|Разрешения|  
|----------|-----------------|  
|Настройка реплик при создании группы доступности|Требуется членство в фиксированной роли сервера **sysadmin** и одно из разрешений: CREATE AVAILABILITY GROUP, ALTER ANY AVAILABILITY GROUP или CONTROL SERVER.|  
|Изменение реплики доступности|Необходимо разрешение ALTER AVAILABILITY GROUP для группы доступности, разрешение CONTROL AVAILABILITY GROUP, разрешение ALTER ANY AVAILABILITY GROUP или разрешение CONTROL SERVER.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 **Настройка доступа к реплике доступности**  
  
1.  В обозревателе объектов подключитесь к экземпляру сервера, на котором размещена первичная реплика, и разверните дерево сервера.  
  
2.  Разверните узел **Высокий уровень доступности AlwaysOn** и узел **Группы доступности** .  
  
3.  Щелкните группу доступности, реплику которой нужно изменить.  
  
4.  Щелкните правой кнопкой мыши реплику доступности и выберите пункт **Свойства**.  
  
5.  В диалоговом окне **Свойства реплики доступности** можно изменить доступ к соединению для первичной и вторичной роли следующим образом:  
  
    -   Для вторичной роли выберите новое значение в раскрывающемся списке **Доступная для чтения вторичная** следующим образом.  
  
         **Нет**  
         Для баз данных-получателей этой реплики соединения пользователя не разрешаются. Для них не разрешен доступ для чтения. Это параметр по умолчанию.  
  
         **Назначение — только чтение**  
         Для баз данных-получателей этой реплики разрешены лишь подключения только для чтения. Для всех баз данных-получателей разрешен доступ для чтения.  
  
         **Да**  
         Для баз данных-получателей этой реплики разрешены все соединения, но только с доступом для чтения. Для всех баз данных-получателей разрешен доступ для чтения.  
  
    -   Для первичной роли выберите новое значение в раскрывающемся списке **Соединения в первичной роли** следующим образом:  
  
         **разрешить все соединения.**  
         Разрешаются все соединения с базами данных в первичной реплике. Это параметр по умолчанию.  
  
         **разрешить соединения с доступом на чтение и запись;**  
         Если свойство «Назначение приложения» имеет значение **ReadWrite** или не задано, то соединение разрешено. Соединения, у которых свойство соединения «Назначение приложения» равно **ReadOnly** , не разрешены. Таким образом, клиент не сможет по ошибке подключить рабочую нагрузку с намерением чтения к первичной реплике. Дополнительные сведения о свойстве соединения «Назначение приложения» см. в разделе [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Настройка доступа к реплике доступности**  
  
> [!NOTE]  
>  Пример этой процедуры см. в подразделе [Примеры (Transact-SQL)](#TsqlExample)далее в этом разделе.  
  
1.  Подключитесь к экземпляру сервера, на котором находится первичная реплика.  
  
2.  Если вы указываете реплику для новой группы доступности, воспользуйтесь инструкцией [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Если вы добавляете или изменяете реплику существующей группы доступности, воспользуйтесь инструкцией [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Чтобы настроить доступ к соединению для вторичной роли, укажите в предложении ADD REPLICA или MODIFY REPLICA WITH параметр SECONDARY_ROLE следующим образом:  
  
         SECONDARY_ROLE **(** ALLOW_CONNECTIONS **=** { NO | READ_ONLY | ALL } **)**  
  
         где  
  
         NO  
         Прямые подключения для баз данных-получателей этой реплики не разрешаются. Для них не разрешен доступ для чтения. Это параметр по умолчанию.  
  
         READ_ONLY  
         Для баз данных-получателей этой реплики разрешены лишь подключения только для чтения. Для всех баз данных-получателей разрешен доступ для чтения.  
  
         ALL  
         Для баз данных-получателей этой реплики разрешены все соединения, но только с доступом для чтения. Для всех баз данных-получателей разрешен доступ для чтения.  
  
3.  Чтобы настроить доступ к соединению для первичной роли, укажите в предложении ADD REPLICA или MODIFY REPLICA WITH параметр PRIMARY_ROLE следующим образом:  
  
     PRIMARY_ROLE **(** ALLOW_CONNECTIONS **=** { READ_WRITE | ALL } **)**  
  
     где  
  
     READ_WRITE  
     Соединения, у которых свойство "Назначение приложения" равно **ReadOnly** , не разрешены.  Если свойство «Назначение приложения» имеет значение **ReadWrite** или не задано, то соединение разрешено. Дополнительные сведения о свойстве соединения «Назначение приложения» см. в разделе [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
     ALL  
     Разрешаются все соединения с базами данных в первичной реплике. Это параметр по умолчанию.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 В следующем примере вторичная реплика добавляется в группу доступности с именем *AG2*. Для размещения новой реплики доступности указывается отдельный экземпляр сервера *COMPUTER03\HADR_INSTANCE*. В этой реплике разрешены только соединения для чтения и записи для первичной роли, а для вторичной роли разрешены соединения с намерением чтения.  
  
```  
ALTER AVAILABILITY GROUP AG2   
   ADD REPLICA ON   
      'COMPUTER03\HADR_INSTANCE' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03:7022',  
         PRIMARY_ROLE ( ALLOW_CONNECTIONS = READ_WRITE ),  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY )  
         );   
GO  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Использование PowerShell  
 **Настройка доступа к реплике доступности**  
  
> [!NOTE]  
>  Пример кода см. в подразделе [Пример (PowerShell)](#PSExample)далее в этом разделе.  
  
1.  Перейдите в каталог (**cd**) экземпляра сервера, в котором находится первичная реплика.  
  
2.  При добавлении реплики доступности в группу доступности воспользуйтесь командлетом **New-SqlAvailabilityReplica** . При изменении существующей реплики доступности воспользуйтесь командлетом **Set-SqlAvailabilityReplica** . Соответствующие параметры:  
  
    -   Чтобы настроить доступ к соединению для вторичной роли, укажите параметр **ConnectionModeInSecondaryRole**_secondary_role_keyword_ , где *secondary_role_keyword* равно одному из следующих значений:  
  
         **AllowNoConnections**  
         Не допускаются прямые соединения с базами данных во вторичной реплике, кроме того, к базам данных также нельзя получить доступ только для чтения. Это параметр по умолчанию.  
  
         **AllowReadIntentConnectionsOnly**  
         Разрешаются только соединения с базами данных во вторичной реплике, у которых свойство "Назначение приложения" равно **ReadOnly**. Дополнительные сведения об этом свойстве см. в разделе [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
         **AllowAllConnections**  
         К базам данных во вторичной реплике разрешаются все соединения на доступ только для чтения.  
  
    -   Чтобы настроить доступ к соединению для первичной роли, укажите параметр **ConnectionModeInPrimaryRole**_primary_role_keyword_, где *primary_role_keyword* равно одному из следующих значений:  
  
         **AllowReadWriteConnections**  
         Соединения, у которых свойство «Назначение приложения» равно ReadOnly, разрешены. Если свойство «Назначение приложения» имеет значение ReadWrite либо оно не задано, то соединение разрешено. Дополнительные сведения о свойстве соединения «Назначение приложения» см. в разделе [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
         **AllowAllConnections**  
         Разрешаются все соединения с базами данных в первичной реплике. Это параметр по умолчанию.  
  
    > [!NOTE]  
    >  Чтобы просмотреть синтаксис командлета, воспользуйтесь командлетом **Get-Help** в среде PowerShell [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] . Дополнительные сведения см. в разделе [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Настройка и использование поставщика SQL Server PowerShell**  
  
-   [Поставщик SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Пример (PowerShell)  
 В следующем примере параметры **ConnectionModeInSecondaryRole** и **ConnectionModeInPrimaryRole** устанавливаются в значение **AllowAllConnections**.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
Set-SqlAvailabilityReplica -ConnectionModeInSecondaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ConnectionModeInPrimaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
  
```  
  
##  <a name="follow-up-after-configuring-read-only-access-for-an-availability-replica"></a><a name="FollowUp"></a> Дальнейшие действия. После настройки доступа только для чтения для реплики доступности  
 **Доступ только для чтения к к доступным для чтения вторичным репликам.**  
  
-   При использовании [bcp Utility](../../../tools/bcp-utility.md) или [sqlcmd Utility](../../../tools/sqlcmd-utility.md)можно указать доступ только для чтения к любой вторичной реплике, которой разрешен доступ только для чтения. Для этого нужно указать параметр **-K ReadOnly** .  
  
-   Обеспечение возможности подключения клиентских приложений к доступным для чтения вторичным репликам.  
  
|Предварительные требования|Ссылка|  
|------------------|----------|  
|Убедитесь, что группа доступности имеет прослушиватель.|[Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|Настройте маршрутизацию только для чтения в группе доступности.|[Настройка маршрутизации только для чтения в группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)|  
  
 **Факторы, которые могут повлиять на триггеры и задания после отработки отказа.**  
  
 Если имеются триггеры и задания, которые не могут выполняться в недоступной или доступной для чтения базы данных-получателе, то в скриптах триггеров и заданий следует проверять, какой базой данных является искомая реплика, базой данных-источником или базой данных-получателем, доступной для чтения. Для получения этих сведений используйте функцию [DATABASEPROPERTYEX](../../../t-sql/functions/databasepropertyex-transact-sql.md), возвращающую свойство **Updateability** базы данных. Чтобы определить базу данных, доступную только для чтения, задайте в качестве значения READ_ONLY, как в примере ниже:  
  
```  
DATABASEPROPERTYEX([db name],'UpdateAbility') = N'READ_ONLY'  
```  
  
 Чтобы определить базу данных для чтения и записи, укажите в качестве значения READ_WRITE.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Настройка маршрутизации только для чтения в группе доступности (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Создание или настройка прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
  
-   [Always On: ценностное предложение читаемой вторичной реплики](/archive/blogs/sqlserverstorageengine/alwayson-value-proposition-of-readable-secondary)  
  
-   [Always On: почему есть два варианта включения вторичной реплики для читаемой рабочей нагрузки?](/archive/blogs/sqlserverstorageengine/alwayson-why-there-are-two-options-to-enable-a-secondary-replica-for-read-workload)  
  
-   [Always On: Настройка доступной для чтения вторичной реплики](/archive/blogs/sqlserverstorageengine/alwayson-setting-up-readable-seconary-replica)  
  
-   [Always On: я только что включил читаемую вторичную реплику, но мой запрос был заблокирован. В чем дело?](/archive/blogs/sqlserverstorageengine/alwayson-i-just-enabled-readable-secondary-but-my-query-is-blocked)  
  
-   [Always On: обеспечение доступности последних статистических данных о читаемой вторичной реплике, базе данных только для чтения и моментальном снимке базы данных](/archive/blogs/sqlserverstorageengine/alwayson-making-latest-statistics-available-on-readable-secondary-read-only-database-and-database-snapshot)  
  
-   [Always On: проблемы со статистическими данными о базе данных только для чтения, моментальном снимке базы данных и вторичной реплике](/archive/blogs/sqlserverstorageengine/alwayson-challenges-with-statistics-on-readonly-database-database-snapshot-and-secondary-replica)  
  
-   [Always On: влияние на основную рабочую нагрузку при запуске рабочей нагрузки составления отчетов на вторичной реплике](/archive/blogs/sqlserverstorageengine/alwayson-impact-on-the-primary-workload-when-you-run-reporting-workload-on-the-secondary-replica)  
  
-   [Always On: влияние сопоставления рабочей нагрузки составления отчетов на вторичной реплике на изоляцию моментальных снимков](/archive/blogs/sqlserverstorageengine/alwayson-impact-of-mapping-reporting-workload-on-readable-secondary-to-snapshot-isolation)  
  
-   [Always On: минимизация блокировок потока REDO при запуске рабочей нагрузки составления отчетов на вторичной реплике](/archive/blogs/sqlserverstorageengine/alwayson-minimizing-blocking-of-redo-thread-when-running-reporting-workload-on-secondary-replica)  
  
-   [Always On: читаемая вторичная реплика и задержка данных](/archive/blogs/sqlserverstorageengine/alwayson-readable-secondary-and-data-latency)  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Активные вторичные реплики: вторичные реплики для чтения (группы доступности Always On)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Сведения о доступе клиентского подключения к репликам доступности (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
