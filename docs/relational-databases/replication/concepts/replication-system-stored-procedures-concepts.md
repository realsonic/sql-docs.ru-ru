---
description: Replication System Stored Procedures Concepts
title: Основные понятия системных хранимых процедур репликации | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server replication], programming
- programming [SQL Server replication], system stored procedures
- programming interfaces [SQL Server replication]
- system stored procedures [SQL Server replication]
- replication [SQL Server], how-to topics
ms.assetid: 816d2bda-ed72-43ec-aa4d-7ee3dc25fd8a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eb082b29411b136edca7ed14f06effa354a5f0e6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643108"
---
# <a name="replication-system-stored-procedures-concepts"></a>Replication System Stored Procedures Concepts
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] программный доступ ко всем настраиваемым пользователями функциональным возможностям в топологии репликации предоставляется системными хранимыми процедурами. Безусловно, хранимые процедуры могут выполняться отдельно с использованием среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или программы командной строки sqlcmd, но может оказаться более удобным написание файлов скриптов [!INCLUDE[tsql](../../../includes/tsql-md.md)], предназначенных для выполнения задач репликации в логической последовательности.  
  
 Задачи репликации со сценарной поддержкой предоставляют следующие преимущества:  
  
-   Сохраняется постоянная копия шагов, выполненных при развертывании топологии репликации.  
  
-   Используется единственный скрипт для настройки нескольких подписчиков.  
  
-   Обучение новых администраторов базы данных ускоряется, поскольку им предоставляется возможность оценивать, изучать, изменять существующий код или диагностировать нарушения в его работе.  
  
    > [!IMPORTANT]  
    >  Скрипты могут стать источниками уязвимости системы безопасности, могут вызывать системные функции без уведомления пользователя и его вмешательства и могут содержать учетные данные безопасности в обычном тексте. Просмотрите скрипты с точки зрения наличия связанных с ними проблем безопасности, прежде чем их использовать.  
  
## <a name="creating-replication-scripts"></a>Создание скриптов репликации  
 С точки зрения репликации скрипт представляет собой ряд, состоящий из одной или нескольких инструкций [!INCLUDE[tsql](../../../includes/tsql-md.md)], в котором каждая инструкция выполняет хранимую процедуру репликации. Скрипты — это текстовые файлы, часто имеющие такое расширение файла, как SQL, которые могут быть вызваны на выполнение с помощью программы sqlcmd. После вызова файла скрипта эта программа выполняет инструкции SQL, хранящиеся в файле. Аналогичным образом скрипт может храниться как объект запроса в проекте среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 Для создания скриптов репликации могут применяться следующие способы.  
  
-   Создание скрипта вручную.  
  
-   Использование средств создания скриптов, предусмотренных в мастерах репликации.  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Дополнительные сведения см. в разделе [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
-   использование объектов RMO для формирования программным путем скрипта создания объекта RMO.  
  
 При создании скриптов репликации вручную необходимо учитывать следующие соображения.  
  
-   Скрипты языка [!INCLUDE[tsql](../../../includes/tsql-md.md)] содержат один или несколько пакетов. Команда GO означает конец пакета. Если скрипт языка [!INCLUDE[tsql](../../../includes/tsql-md.md)] не содержит команд GO, то он выполняется как единый пакет.  
  
-   Если в одном пакете должно быть выполнено несколько хранимых процедур репликации, то после первой процедуры все последующие процедуры в пакете должны быть указаны с предшествующим ключевым словом EXECUTE.  
  
-   Все хранимые процедуры в пакете должны быть откомпилированы до выполнения пакета. Но после компиляции пакета и создания плана выполнения ошибки времени выполнения могут происходить или не происходить.  
  
-   Если скрипты создаются для настройки конфигурации репликации, то должна использоваться проверка подлинности Windows для исключения необходимости хранить учетные данные безопасности в файле скрипта. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
## <a name="sample-replication-script"></a>Образец скрипта репликации  
 Следующий скрипт может быть выполнен для установки на сервере средств публикации и распределения.  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
 Затем этот скрипт может быть сохранен локально в качестве `instdistpub.sql`, чтобы его можно было выполнить один или несколько раз, если это потребуется.  
  
 Предыдущий скрипт включает переменные скрипта **sqlcmd**, которые используются во многих примерах кода репликации в электронной документации по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Переменные скрипта определяются с использованием синтаксиса `$(MyVariable)`. Значения переменных могут быть переданы в скрипт в командной строке или в среде [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Дополнительные сведения см. в следующем подразделе данного раздела, «Выполнение скриптов репликации».  
  
## <a name="executing-replication-scripts"></a>Выполнение скриптов репликации  
 Для выполнения скрипта репликации после его создания может быть использован один из следующих способов.  
  
### <a name="creating-a-sql-query-file-in-sql-server-management-studio"></a>Создание файла SQL-запроса в среде SQL Server Management Studio  
 Файл скрипта репликации [!INCLUDE[tsql](../../../includes/tsql-md.md)] может быть создан в проекте среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] как файл SQL-запроса. После записи скрипта может быть создано соединение с базой данных для этого файла запроса и скрипт вызван на выполнение. Дополнительные сведения о создании скриптов [!INCLUDE[tsql](../../../includes/tsql-md.md)] с помощью [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] см. в статье о [редакторах запросов и текста (SQL Server Management Studio)](../../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
 Чтобы использовать скрипт с переменными скрипта, необходимо запустить среду [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] в режиме **sqlcmd**. В режиме **sqlcmd** редактор запросов распознает дополнительный синтаксис, характерный для **sqlcmd**, например обозначение `:setvar` для значений переменных. Дополнительные сведения о режиме **sqlcmd** см. в разделе об [изменении скриптов SQLCMD при помощи редактора запросов](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md). В следующем скрипте `:setvar` используется для предоставления значения переменной `$(DistPubServer)`.  
  
```  
:setvar DistPubServer N'MyPublisherAndDistributor';  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
--  
-- Additional code goes here  
--  
```  
  
### <a name="using-the-sqlcmd-utility-from-the-command-line"></a>Использование программы sqlcmd в командной строке  
 Следующий пример показывает, как выполнить файл скрипта `instdistpub.sql` из командной строки с применением [программы sqlcmd](../../../tools/sqlcmd-utility.md):  
  
```  
sqlcmd.exe -E -S sqlserverinstance -i C:\instdistpub.sql -o C:\output.log -v DistPubServer="N'MyDistributorAndPublisher'"  
```  
  
 В этом примере параметр `-E` указывает, что используется проверка подлинности Windows при соединении с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Если используется проверка подлинности Windows, то отсутствует необходимость хранить имя пользователя и пароль в файле скрипта. Имя и путь к файлу скрипта задаются параметром `-i`, а имя выходного файла — параметром `-o` (если используется этот параметр, то вывод программы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] записывается в указанный файл, а не на консоль). Программа `sqlcmd` позволяет передавать переменные скрипта в скрипт [!INCLUDE[tsql](../../../includes/tsql-md.md)] во время выполнения с помощью параметра `-v`. В этом примере программа `sqlcmd` заменяет каждое вхождение переменной `$(DistPubServer)` в скрипте значением `N'MyDistributorAndPublisher'` перед выполнением.  
  
> [!NOTE]  
>  Параметр `-X` позволяет запретить использование переменных сценария.  
  
### <a name="automating-tasks-in-a-batch-file"></a>Автоматизация задач с применением пакетного файла  
 Применение пакетного файла позволяет автоматизировать задачи администрирования репликации, задачи синхронизации репликации и другие задачи с помощью одного и того же пакетного файла. Следующий пакетный файл с помощью программы **sqlcmd** удаляет и заново создает базу данных подписки, а затем добавляет подписку слиянием по запросу. Затем в этом файле предусмотрен вызов агента слияния для синхронизации новой подписки:  
  
```  
REM ----------------------Script to synchronize merge subscription ----------------------  
REM -- Creates subscription database and   
REM -- synchronizes the subscription to MergeSalesPerson.  
REM -- Current computer acts as both Publisher and Subscriber.  
REM -------------------------------------------------------------------------------------  
  
SET Publisher=%computername%  
SET Subscriber=%computername%  
SET PubDb=AdventureWorks  
SET SubDb=AdventureWorksReplica  
SET PubName=AdvWorksSalesOrdersMerge  
  
REM -- Drop and recreate the subscription database at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE master IF EXISTS (SELECT * FROM sysdatabases WHERE name='%SubDb%' ) DROP DATABASE %SubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE master CREATE DATABASE %SubDb%"  
  
REM -- Add a pull subscription at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb% EXEC sp_addmergepullsubscription @publisher = %Publisher%, @publication = %PubName%, @publisher_db = %PubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb%  EXEC sp_addmergepullsubscription_agent @publisher = %Publisher%, @publisher_db = %PubDb%, @publication = %PubName%, @subscriber = %Subscriber%, @subscriber_db = %SubDb%, @distributor = %Publisher%"  
  
REM -- This batch file starts the merge agent at the Subscriber to   
REM -- synchronize a pull subscription to a merge publication.  
REM -- The following must be supplied on one line.  
"\Program Files\Microsoft SQL Server\130\COM\REPLMERG.EXE"  -Publisher  %Publisher% -Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB  %PubDb% -SubscriberDB %SubDb% -Publication %PubName% -PublisherSecurityMode 1 -OutputVerboseLevel 1  -Output  -SubscriberSecurityMode 1  -SubscriptionType 1 -DistributorSecurityMode 1 -Validate 3  
  
```  
  
## <a name="scripting-common-replication-tasks"></a>Сценарная поддержка общих задач репликации  
 Ниже перечислены некоторые из наиболее распространенных задач репликации, которые могут быть реализованы в виде сценариев с использованием системных хранимых процедур:  
  
-   Настройка публикации и распределения  
  
-   Изменение свойств издателя и распространителя  
  
-   Отключение публикации и распространения  
  
-   Создание публикаций и определение статей  
  
-   Удаление публикаций и статей  
  
-   Создание подписки по запросу  
  
-   Изменение подписки по запросу  
  
-   Удаление подписки по запросу  
  
-   Создание принудительной подписки  
  
-   Изменение принудительной подписки  
  
-   Удаление принудительной подписки  
  
-   Синхронизация подписки по запросу  
  
## <a name="see-also"></a>См. также  
 [Основные понятия для программирования репликации](../../../relational-databases/replication/concepts/replication-programming-concepts.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Создание скриптов репликации](../../../relational-databases/replication/scripting-replication.md)  
  
