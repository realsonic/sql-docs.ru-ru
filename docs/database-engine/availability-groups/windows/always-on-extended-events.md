---
title: Настройка расширенных событий для групп доступности
description: SQL Server определяет расширенные события, характерные для групп доступности Always On. Вы можете отслеживать расширенные события для диагностирования первопричин при устранении неполадок для группы доступности.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 5950f98a-3950-473d-95fd-cde3557b8fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 7ba062db14f9cb3d2ba262350c1ff5b93a079884
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091824"
---
# <a name="configure-extended-events-for-always-on-availability-groups"></a>Настройка расширенных событий для групп доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  SQL Server определяет расширенные события, характерные для групп доступности Always On. Вы можете отслеживать расширенные события для диагностирования первопричин при устранении неполадок для группы доступности. Просмотреть расширенные события группы доступности можно с помощью следующего запроса:  
  
```sql  
SELECT * FROM sys.dm_xe_objects WHERE name LIKE '%hadr%'  
```  
   
##  <a name="alwayson_health-session"></a><a name="BKMK_alwayson_health"></a> Сеанс Alwayson_health  
 Сеанс расширенных событий alwayson_health создается автоматически при создании группы доступности и фиксирует подмножество связанных с ней событий. Этот сеанс предварительно настроен в качестве удобного средства, которое поможет вам быстрее приступить к устранению неполадок для группы доступности. Мастер создания группы доступности автоматически запускает сеанс на всех участвующих репликах доступности, настроенных в мастере.  
  
> [!IMPORTANT]  
>  Если вы не создали группу доступности с помощью **мастера создания группы доступности**, сеанс alwayson_health может не запуститься автоматически. Если сеанс не запущен, фиксация данных о событиях в случае непредвиденной проблемы невозможна. Нужно вручную запустить сеанс и с помощью свойств настроить его для автоматического запуска.  
  
 Чтобы просмотреть определение сеанса alwayson_health, сделайте следующее:  
  
1.  В **обозревателе объектов** разверните узлы **Управление**, **Расширенные события** и **Сеансы**.  
  
2.  Щелкните правой кнопкой элемент **Alwayson_health**, выберите команду **Создать скрипт для сеанса**, укажите **CREATE в** и выберите вариант **В новом окне редактора запросов**.  

Сведения о некоторых событиях, связанных с alwayson_health, см. в [справочнике по расширенным событиям](always-on-extended-events.md#BKMK_Reference).  


##  <a name="extended-events-for-debugging"></a><a name="BKMK_Debugging"></a> Расширенные события для отладки  
 Кроме расширенных событий, охватываемых сеансом Alwayson_health, SQL Server определяет обширный набор событий отладки для групп доступности. Чтобы полноценно использовать эти дополнительные расширенные события в сеансе, выполните описанные ниже процедуры:  
  
1.  В **обозревателе объектов** разверните узлы **Управление**, **Расширенные события** и **Сеансы**.  
  
2.  Щелкните правой кнопкой мыши элемент **Сеансы** и выберите **Создать сеанс**. Либо щелкните правой кнопкой элемент **Alwayson_health** и выберите **Свойства**.  
  
3.  На панели **Выбор страницы** щелкните **События**.  
  
4.  В столбце **Категория** библиотеки событий выберите **alwayson** и очистите остальные категории.  
  
5.  В столбце **Канал** выберите **Отладка**. Все еще не выбранные события, связанные с группой доступности, отображаются в библиотеке событий.  
  
6.  Выделите событие в библиотеке событий, а затем нажмите кнопку **>** , чтобы выбрать его для данного сеанса.  
  
7.  Закончив работу с сеансом, нажмите кнопку **ОК**, чтобы закрыть его. Убедитесь, что сеанс запущен, чтобы он фиксировал выбранные вами события.  
  
##  <a name="always-on-availability-groups-extended-events-reference"></a><a name="BKMK_Reference"></a> Справочник по расширенным событиям для групп доступности AlwaysOn  
 Этот раздел описывает некоторые из расширенных событий, которые используются для отслеживания групп доступности.  
  
 [availability_replica_state_change](#BKMK_availability_replica_state_change)  
  
 [availability_group_lease_expired](#BKMK_availability_group_lease_expired)  
  
 [availability_replica_automatic_failover_validation](#BKMK_availability_replica_automatic_failover_validation)  
  
 [error_reported (несколько номеров ошибки): для проблем с транспортом или подключением](#BKMK_error_reported)  
  
 [data_movement_suspend_resume](#BKMK_data_movement_suspend_resume)  
  
 [alwayson_ddl_executed](#BKMK_alwayson_ddl_executed)  
  
 [availability_replica_manager_state](#BKMK_availability_replica_manager_state)  
  
 [error_reported (1480): изменилась роль реплики базы данных](#BKMK_error_reported_1480)  
  
###  <a name="availability_replica_state_change"></a><a name="BKMK_availability_replica_state_change"></a> availability_replica_state_change  
 Возникает при изменении состояния реплики доступности. Создание группы доступности или присоединение к реплике доступности может вызывать это событие. Это удобно при диагностике сбоя автоматического перехода на другой ресурс. Его также можно использовать для трассировки шагов перехода на другой ресурс.  
  
#### <a name="event-information"></a>Сведения о событии  
  
|Столбец|Описание|  
|------------|-----------------|  
|Имя|availability_replica_state_change|  
|Категория|always on|  
|Channel|Операционный|  
  
#### <a name="event-fields"></a>Поля событий  
  
|Имя|Type_name|Описание|  
|----------|----------------|-----------------|  
|availability_group_id|guid|Идентификатор группы доступности.|  
|availability_group_name|unicode_string|Имя группы доступности.|  
|availability_replica_id|guid|Идентификатор реплики доступности.|  
|previous_state|availability_replica_state|Роль реплики перед изменением.<br /><br /> **Возможны следующие значения:**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
|current_state|availability_replica_state|Роль реплики после изменения.<br /><br /> **Возможны следующие значения:**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_replica_state_change  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_group_lease_expired"></a><a name="BKMK_availability_group_lease_expired"></a> availability_group_lease_expired  
 Возникает, когда для кластера и группы доступности имеется проблема подключения, а также истек срок аренды. Это событие указывает, что нарушено подключение между группой доступности и базовым кластером WSFC. При возникновении проблемы подключения на первичной реплике это событие может вызвать автоматический переход на другой ресурс или перевести группу доступности в автономный режим.  
  
#### <a name="event-information"></a>Сведения о событии  
  
|Столбец|Описание|  
|------------|-----------------|  
|Имя|availability_group_lease_expired|  
|Категория|always on|  
|Channel|Операционный|  
  
#### <a name="event-fields"></a>Поля событий  
  
|Имя|Type_name|Описание|  
|----------|----------------|-----------------|  
|availability_group_id|guid|Идентификатор группы доступности.|  
|availability_group_name|unicode_string|Имя группы доступности.|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_group_lease_expired  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_automatic_failover_validation"></a><a name="BKMK_availability_replica_automatic_failover_validation"></a> availability_replica_automatic_failover_validation  
 Возникает, когда автоматический переход на другой ресурс проверяет готовность реплики доступности в качестве первичной реплики, и показывает, готова ли целевая реплика доступности стать новой первичной репликой. Например, проверка перехода на другой ресурс возвращает значение false, когда синхронизированы или присоединены не все базы данных. Это событие призвано предоставлять точку отказа при переходах на другой ресурс. Эта информация представляет интерес для администратора базы данных, особенно при автоматических переходах на другой ресурс, так как они не требуют вмешательства. Администратор базы данных может просмотреть событие, чтобы узнать причину сбоя автоматического перехода на другой ресурс.  
  
#### <a name="event-information"></a>Сведения о событии  
  
|Имя|Описание|  
|----------|-----------------|  
|availability_replica_automatic _failover_validation||  
|Категория|always on|  
|Channel|Аналитический|  
  
#### <a name="event-fields"></a>Поля событий  
  
|Имя|Type_name|Описание|  
|----------|----------------|-----------------|  
|availability_group_id|guid|Идентификатор группы доступности.|  
|availability_group_name|unicode_string|Имя группы доступности.|  
|availability_replica_id|guid|Идентификатор реплики доступности.|  
|forced_quorum|validation_result_type|Если это значение равно TRUE, то автоматический переход на другой ресурс на этой реплике доступности объявляется недействительным.<br /><br /> TRUE<br /><br /> FALSE|  
|joined_and_synchronized|validation_result_type|Если это значение равно FALSE, то автоматический переход на другой ресурс на этой реплике доступности объявляется недействительным.<br /><br /> TRUE<br /><br /> FALSE|  
|previous_primary_or_automatic_failover_target|validation_result_type|Если это значение равно FALSE, то автоматический переход на другой ресурс на этой реплике доступности объявляется недействительным.<br /><br /> TRUE<br /><br /> FALSE|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_automatic_failover_validation (  
WHERE (  
[forced_quorum]=(TRUE) OR [joined_and_synchronized]=(FALSE) OR [previous_primary_or_automatic_failover_target]=(TRUE)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
  
```  
  
###  <a name="error_reported-multiple-error-numbers-for-transport-or-connection-issues"></a><a name="BKMK_error_reported"></a> error_reported (несколько номеров ошибки): для проблем с транспортом или подключением  
 Каждое отфильтрованное событие свидетельствует о проблеме подключения в конечной точки зеркального отображения базы данных или транспорта, от которой зависит группа доступности.  
  
|Столбец|Описание|  
|------------|-----------------|  
|Имя|error_reported<br /><br /> Числа для фильтрации: 35201, 35202, 35206, 35204, 35207, 9642, 9666, 9691, 9692, 9693, 28034, 28036, 28080, 28091, 33309|  
|Категория|ошибки|  
|Channel|Административный|  
  
#### <a name="error-numbers-to-filter"></a>Номера ошибок для фильтрации  
  
|Номер ошибки|Описание|  
|------------------|-----------------|  
|35201|Истекло время ожидания при попытке установить соединение с репликой доступности "%ls".|  
|35202|Соединение для группы доступности "%ls" от реплики доступности "%ls" с идентификатором [%ls] до реплики доступности "%ls" с идентификатором [%ls] было успешно установлено.  Это информационное сообщение. Вмешательство пользователя не требуется.|  
|35206|Истекло время ожидания для ранее установленного соединения с репликой доступности "%ls".|  
|35204|Подключение между экземпляром "%ls" и "%ls" отключено из-за завершения работы конечной точки.|  
|Время ожидания + подключение|  
|35207|Попытка соединения с идентификатором группы доступности "%ls" из реплики с идентификатором "%ls" к реплике с идентификатором "%ls" завершилась с ошибкой %d,  уровень серьезности %d, состояние %d. (Этот вариант может плохо подходить для использования администратором базы данных. В этом случае нужно проверить и удалить его позднее.)|  
|9642|Произошла ошибка конечной точки транспорта компонента Service Broker или зеркального отображения базы данных. Ошибка: %i, состояние: %i. (Роль ближней конечной точки: %S_MSG, адрес дальней конечной точки: "%.*hs".) Ошибка: %i, состояние: %i. (Роль ближней конечной точки: %S_MSG, адрес дальней конечной точки: "%.\*hs".)|  
|9666|Конечная точка %S_MSG находится в отключенном или остановленном состоянии.|  
|9691|Конечная точка %S_MSG прекратила прослушивание соединений.|  
|9692|Конечная точка %S_MSG не может прослушивать порт %d, поскольку он используется другим процессом.|  
|9693|Конечная точка %S_MSG не может прослушивать соединения из-за следующей ошибки: "%.*ls".|  
|28034|Ошибка подтверждения соединения. Имя входа "%.*ls" не имеет разрешения CONNECT на конечную точку. Состояние %d.|  
|28036|Ошибка подтверждения соединения. Сертификат, используемый данной конечной точкой, не обнаружен: %S_MSG. Используйте DBCC CHECKDB в базе данных master для проверки целостности метаданных конечных точек. Состояние %d.|  
|28080|Ошибка подтверждения соединения. Конечная точка  %S_MSG не настроена. Состояние %d.|  
|28091|Запуск конечной точки для %S_MSG без проверки подлинности не поддерживается.|  
|33309|Не удалось запустить конечную точку кластера, так как конфигурация по умолчанию конечной точки %S_MSG еще не загружена.|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--Connectivity Error Messages  
[error_number]=(35201)   
OR [error_number]=(35202)   
OR [error_number]=(35204)   
OR [error_number]=(35206)   
OR [error_number]=(35207)   
OR [error_number]=(9642)   
--OR [error_number]=(9666)   
OR [error_number]=(9691)   
OR [error_number]=(9692)   
OR [error_number]=(9693)   
OR [error_number]=(28034)   
OR [error_number]=(28036)   
OR [error_number]=(28080)   
OR [error_number]=(28091)   
OR [error_number]=(33309)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="data_movement_suspend_resume"></a><a name="BKMK_data_movement_suspend_resume"></a> data_movement_suspend_resume  
 Возникает, когда приостановлено или возобновлено перемещение базы данных для реплики базы данных.  
  
#### <a name="event-information"></a>Сведения о событии  
  
|Столбец|Описание|  
|------------|-----------------|  
|Имя|data_movement_suspend_resume|  
|Категория|Всегда включена|  
|Channel|Операционный|  
  
#### <a name="event-fields"></a>Поля событий  
  
|Имя|Type_name|Описание|  
|-|-|-|  
|availability_group_id|guid|Идентификатор группы доступности.|  
|availability_group_name|unicode_string|Имя группы доступности (при его наличии).|  
|availability_replica_id|guid|Идентификатор реплики доступности.|  
|database_replica_id|guid|Идентификатор базы данных доступности.|  
|database_replica_name|unicode_string|Имя базы данных доступности.|  
|database_id|uint32|Идентификатор базы данных доступности.|  
|suspend_status|suspend_status_type|Значения состояния приостановки.<br /><br /> SUSPEND_NULL<br /><br /> RESUMED<br /><br /> SUSPENDED<br /><br /> SUSPENDED_INVALID|  
|suspend_source|suspend_source_type|Источник действия приостановки или возобновления.|  
|suspend_reason|unicode_string|Причина приостановки, зарегистрированная диспетчером реплик базы данных.|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT data_movement_suspend_resume (  
WHERE (  
[suspend_status]=(SUSPENDED)or [suspend_status]=(SUSPENDED_INVALID) or   
[suspend_status]=(SUSPEND_NULL)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="alwayson_ddl_executed"></a><a name="BKMK_alwayson_ddl_executed"></a> alwayson_ddl_executed  
 Возникает, когда выполняется инструкция языка DDL для группы доступности, включая CREATE, ALTER или DROP. Основной задачей этого события является указание проблемы с действием пользователя на реплике доступности или указание начальной точки оперативного действия, следом за которым возникла проблема среды выполнения, например переход на другой ресурс вручную, принудительный переход на другой ресурс, приостановка или возобновление перемещения данных.  
  
#### <a name="event-information"></a>Сведения о событии  
  
|Столбец|Описание|  
|------------|-----------------|  
|Имя|alwayson_ddl_execution|  
|Категория|always on|  
|Channel|Аналитический|  
  
#### <a name="event-fields"></a>Поля событий  
  
|Имя|Type_name|Описание|  
|----------|----------------|-----------------|  
|availability_group_id|Guid|Идентификатор группы доступности.|  
|availability_group_name|unicode_string|Имя группы доступности.|  
|ddl_action|alwayson_ddl_action|Указывает тип действия DDL: CREATE, ALTER или DROP.|  
|ddl_phase|ddl_opcode|Указывает фазу операции DDL: BEGIN, COMMIT или ROLLBACK.|  
|.|unicode_string|Текст выполненной инструкции.|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT alwayson_ddl_executed  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_manager_state"></a><a name="BKMK_availability_replica_manager_state"></a> availability_replica_manager_state  
 Возникает при изменении состояния диспетчера реплики доступности. Это событие указывает пульс для диспетчера реплики доступности. Когда диспетчер реплики доступности не находится в работоспособном состоянии, все реплики доступности в соответствующем экземпляре SQL Server будут отключены.  
  
#### <a name="event-information"></a>Сведения о событии  
  
|Столбец|Описание|  
|------------|-----------------|  
|Имя|availability_replica_manager_state_change|  
|Категория|always on|  
|Channel|Операционный|  
  
#### <a name="event-fields"></a>Поля событий  
  
|Имя|Type_name|Описание|  
|----------|----------------|-----------------|  
|current_state|manager_state|Текущее состояние диспетчера реплики доступности.<br /><br /> Справка в Интернете<br /><br /> Вне сети<br /><br /> WaitingForClusterCommunication|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса Alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_manager_state (  
WHERE ([current_state]=(OFFLINE))  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="error_reported-1480-database-replica-role-change"></a><a name="BKMK_error_reported_1480"></a> error_reported (1480): изменилась роль реплики базы данных  
 Это отфильтрованное событие error_reported возникает асинхронно после изменения роли реплики доступности. Оно указывает, какой базе данных доступности не удается изменить свою ожидаемую роль во время перехода на другой ресурс.  
  
#### <a name="event-information"></a>Сведения о событии  
  
|Столбец|Описание|  
|------------|-----------------|  
|Имя|error_reported<br /><br /> Номер ошибки 1480: база данных "DATABASE_NAME" REPLICATION_TYPE_MSG меняет роль с "OLD_ROLE" на "NEW_ROLE" по причине REASON_MSG|  
|Категория|ошибки|  
|Channel|Административный|  
  
#### <a name="alwayson_health-session-definition"></a>Определение сеанса alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--database replica role change message  
OR [error_number] = (1480)  
  
--database replica runtime error messages  
OR [error_number]=(823)   
OR [error_number]=(824)   
OR [error_number]=(829)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
## <a name="next-steps"></a>Дальнейшие действия  
 [Просмотр данных о сеансе событий](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110))