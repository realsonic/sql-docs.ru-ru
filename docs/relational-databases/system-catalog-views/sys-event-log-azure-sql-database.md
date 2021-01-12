---
description: sys.event_log (база данных SQL Azure)
title: sys.event_log (база данных SQL Azure) | Документация Майкрософт
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- event_log
- sys.event_log_TSQL
- event_log_TSQL
- sys.event_log
dev_langs:
- TSQL
helpviewer_keywords:
- event_log
- sys.event_log
ms.assetid: ad5496b5-e5c7-4a18-b5a0-3f985d7c4758
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: c3a358e23d9428440166dd98a0f29ca8e4ea11c5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093105"
---
# <a name="sysevent_log-azure-sql-database"></a>sys.event_log (база данных SQL Azure)

[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Возвращает успешные [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] подключения к базе данных, сбои подключений и взаимоблокировки. С помощью этих данных можно отслеживать и устранять неполадки операций с базой данных, используя [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
> [!CAUTION]  
> Для установок с большим количеством баз данных или большим числом имен входа действие в sys.event_log может вызвать ограничения производительности, высокой загрузки ЦП и, возможно, привести к ошибкам входа. Запросы sys.event_log могут повлиять на проблему. Корпорация Майкрософт работает над устранением этой проблемы. Тем временем, чтобы снизить влияние этой проблемы, Ограничьте запросы sys.event_log. Пользователи подключаемого модуля NewRelic SQL Server должны посетить [база данных SQL Microsoft Azure настройки подключаемого модуля & настройке производительности](https://discuss.newrelic.com/t/microsoft-azure-sql-database-plugin-tuning-performance-tweaks/30729) для получения дополнительных сведений о конфигурации.  
  
 Представление `sys.event_log` содержит следующие столбцы.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Имя базы данных. Если подключение завершилось ошибкой и пользователь не указал имя базы данных, то этот столбец остается пустым.|  
|**start_time**|**datetime2**|Дата и время начала интервала статистической обработки в формате UTC. Для статистических событий время всегда кратно 5 минутам. Пример:<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|Дата и время окончания интервала статистической обработки в формате UTC. Для агрегированных событий **End_time** всегда находится в течение 5 минут позже, чем соответствующее **start_time** в той же строке. Для событий, которые не являются статистическими, **start_time** и **end_time** равны фактической дате и времени события в формате UTC.|  
|**event_category**|**nvarchar (64)**|Высокоуровневый компонент, вызвавший данное событие.<br /><br /> Список возможных значений см. в разделе [типы событий](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_type**|**nvarchar (64)**|Тип события.<br /><br /> Список возможных значений см. в разделе [типы событий](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_subtype**|**int**|Подтип произошедшего события.<br /><br /> Список возможных значений см. в разделе [типы событий](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_subtype_desc**|**nvarchar (64)**|Описание подтипа события.<br /><br /> Список возможных значений см. в разделе [типы событий](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**severity**|**int**|Степень серьезности ошибки. Возможны следующие значения:<br /><br /> 0 = информационные<br />1 = предупреждение<br />2 = ошибка|  
|**event_count**|**int**|Количество раз, которое произошло это событие для указанной базы данных в течение указанного интервала времени (**start_time** и **end_time**).|  
|**description**|**nvarchar(max)**|Подробное описание события.<br /><br /> Список возможных значений см. в разделе [типы событий](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**additional_data**|**XML**|*Примечание. это значение всегда равно NULL для базы данных SQL Azure версии 12. Дополнительные сведения о получении событий взаимоблокировок для версии 12 см. в разделе " [примеры](#Deadlock) ".*<br /><br /> Для событий **взаимоблокировок** этот столбец содержит граф взаимоблокировок. Этот столбец содержит значение NULL для других типов событий. |  
  
##  <a name="event-types"></a><a name="EventTypes"></a> Типы событий

 События, регистрируемые каждой строкой в этом представлении, идентифицируются по категории (**event_category**), типу события (**event_type**) и подтипу (**event_subtype**). В следующей таблице перечислены типы событий, собираемых в этом представлении.  
  
 Для событий в категории **Подключение** сводные данные доступны в представлении sys.database_connection_stats.  
  
> [!NOTE]  
> Это представление включает не все возможные события базы данных [!INCLUDE[ssSDS](../../includes/sssds-md.md)], которые могут возникнуть, а только события перечисленные ниже. Дополнительные категории, типы событий и подтипы могут быть добавлены в будущих версиях [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**description**|  
|-------------------------|---------------------|------------------------|------------------------------|------------------|---------------------|  
|**установлен**|**connection_successful**|0|**connection_successful**|0|Успешное подключение к базе данных.|  
|**установлен**|**connection_failed**|0|**invalid_login_name**|2|Имя входа не является допустимым именем входа в данной версии SQL Server.|  
|**установлен**|**connection_failed**|1|**windows_auth_not_supported**|2|Имена входа Windows не поддерживаются в данной версии SQL Server.|  
|**установлен**|**connection_failed**|2|**attach_db_not_supported**|2|Пользователь запросил присоединение файла базы данных, который не поддерживается.|  
|**установлен**|**connection_failed**|3|**change_password_not_supported**|2|Пользователь запросил изменение пароля пользователя, входящего в систему, который не поддерживается.|  
|**установлен**|**connection_failed**|4|**login_failed_for_user**|2|Ошибка входа пользователя.|  
|**установлен**|**connection_failed**|5|**login_disabled**|2|Имя входа отключено.|  
|**установлен**|**connection_failed**|6|**failed_to_open_db**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Не удалось открыть базу данных. Может быть вызвано тем, что база данных не существует или проверка подлинности для открытия базы данных не пройдена.|  
|**установлен**|**connection_failed**|7|**blocked_by_firewall**|2|Клиенту с IP-адресом доступ к серверу не разрешен.|  
|**установлен**|**connection_failed**|8|**client_close**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> При установке соединения могло быть превышено время ожидания клиента. Попробуйте увеличить время ожидания соединения.|  
|**установлен**|**connection_failed**|9|**перенастройки**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> При подключении возникла ошибка, так как в это время выполнялась настройка базы данных.|  
|**установлен**|**connection_terminated**|0|**idle_connection_timeout**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Соединение бездействует дольше заданного системой порогового периода.|  
|**установлен**|**connection_terminated**|1|**перенастройки**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Сеанс прерван из-за изменения конфигурации базы данных.|  
|**установлен**|**регулирования**|*\<reason code>*|**reason_code**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Запрос регулируется.  Код причины регулирования: *\<reason code>* . Дополнительные сведения см. в разделе [регулирование подсистемы](/previous-versions/azure/dn338079(v=azure.100)).|  
|**установлен**|**throttling_long_transaction**|40549|**long_transaction**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Сеанс завершен по причине долго выполняющейся транзакции. Рекомендуется сократить транзакцию. Дополнительные сведения см. в разделе [ограничения ресурсов](/previous-versions/azure/dn338081(v=azure.100)).|  
|**установлен**|**throttling_long_transaction**|40550|**excessive_lock_usage**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Сеанс был завершен в связи с тем, что он получил слишком много блокировок. Рекомендуется сократить число строк, считываемых или изменяемых в одной транзакции. Дополнительные сведения см. в разделе [ограничения ресурсов](/previous-versions/azure/dn338081(v=azure.100)).|  
|**установлен**|**throttling_long_transaction**|40551|**excessive_tempdb_usage**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Сеанс был завершен в связи с чрезмерным использованием базы данных TEMPDB. Попробуйте изменить запрос, чтобы сократить объем использования временных таблиц. Дополнительные сведения см. в разделе [ограничения ресурсов](/previous-versions/azure/dn338081(v=azure.100)).|  
|**установлен**|**throttling_long_transaction**|40552|**excessive_log_space_usage**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Сеанс был завершен в связи с чрезмерным использованием объема журнала транзакций. Рекомендуется сократить число строк, изменяемых в одной транзакции. Дополнительные сведения см. в разделе [ограничения ресурсов](/previous-versions/azure/dn338081(v=azure.100)).|  
|**установлен**|**throttling_long_transaction**|40553|**excessive_memory_usage**|2|*Примечание. применяется только к версии 11 базы данных SQL Azure.*<br /><br /> Сеанс был завершен в связи с чрезмерным использованием памяти. Рекомендуется изменить запрос, сократив число обрабатываемых строк. Дополнительные сведения см. в разделе [ограничения ресурсов](/previous-versions/azure/dn338081(v=azure.100)).|  
|**ядре**|**блокировку**|0|**блокировку**|2|Возникла взаимоблокировка.|  
  
## <a name="permissions"></a>Разрешения

 Пользователи с разрешением на доступ к базе данных **master** имеют доступ только для чтения к этому представлению.  
  
## <a name="remarks"></a>Комментарии  
  
### <a name="event-aggregation"></a>Статистическая обработка событий

 Сведения о событиях для этого представления собираются и обрабатываются каждые 5 минут. Столбец **event_count** представляет количество определенных **event_type** и **event_subtype** для определенной базы данных в течение заданного интервала времени.  
  
> [!NOTE]  
> Некоторые события, например взаимоблокировки, не обрабатываются статистически. Для этих событий **event_count** будет иметь значение 1 и **start_time** и **end_time** будет равно фактической дате и времени в формате UTC, когда произошло событие.  
  
 Например, если пользователю не удается подключиться к базе данных Database1 из-за недопустимого имени входа 7 раз с 11:00 до 11:05 5 февраля 2012 г. (UTC), эти сведения доступны в одной строке в следующем представлении:  
  
|**database_name**|**start_time**|**end_time**|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**event_count**|**description**|**additional_data**|  
|------------------------|---------------------|-------------------|-------------------------|---------------------|------------------------|------------------------------|------------------|----------------------|---------------------|--------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`connectivity`|`connection_failed`|`4`|`login_failed_for_user`|`2`|`7`|`Login failed for user.`|`NULL`|  
  
### <a name="interval-start_time-and-end_time"></a>start_time и end_time интервала  
 Событие включается в интервал агрегирования, когда событие происходит *в* или _после_**start_time** и _до_**end_time** для этого интервала. Например, событие, которое происходит точно в `2012-10-30 19:25:00.0000000`, будет включено только во второй интервал, показанный ниже.  
  
```
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```

### <a name="data-updates"></a>Обновление данных

 Данные в этом представлении с течением времени накапливаются. Как правило, данные накапливаются в течение часа с начала интервала статистической обработки, но для отображения всех данных в представлении может потребоваться до 24 часов. В течение этого времени сведения в одной строке могут периодически обновляться.  
  
### <a name="data-retention"></a>Хранение данных

 Данные в этом представлении могут храниться не более 30 дней, или, возможно, меньше, в зависимости от количества баз данных и числа уникальных событий, создаваемых каждой базой данных. Для сохранения этих данных в течение более длительного периода скопируйте их в отдельную базу данных. После создания первоначальной копии представления строки могут быть обновлены по мере накопления данных. Чтобы копия данных была актуальной, периодически выполняйте просмотр таблицы для определения увеличения числа событий существующих строк и для определения новых строк (вы можете определить уникальные строки с помощью времени начала и окончания интервала), а затем обновить свою копию данных с применением этих изменений.  
  
### <a name="errors-not-included"></a>Исключенные ошибки

 Это представление может содержать не все сведения о подключениях и ошибках:  
  
- Это представление не включает все [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ошибки базы данных, которые могут возникнуть, только те, которые указаны в [типах событий](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) в этом разделе.  
- Если в центре обработки данных возникает сбой компьютера [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , в таблице событий может отсутствовать небольшой объем данных.  
- Если IP-адрес заблокирован через DoSGuard, события подключения с этого IP-адреса не могут собираться и не будут отображаться в этом представлении.  
  
## <a name="examples"></a>Примеры  
  
### <a name="simple-examples"></a>Простые примеры

 Следующий запрос возвращает все события, произошедшие между 12:00 25 сентября до 12:00 28 сентября 2011 г. (UTC). По умолчанию результаты запроса сортируются по **start_time** (по возрастанию).  

```sql
SELECT * FROM sys.event_log
WHERE start_time >= '2011-09-25 12:00:00'
    AND end_time <= '2011-09-28 12:00:00';  
```

Следующий запрос возвращает все события взаимоблокировки для базы данных Database1 (применяется только к версии 11 базы данных SQL Azure).  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'deadlock'
    AND database_name = 'Database1';  
```

<a name="Deadlock"></a> Следующий запрос возвращает все события взаимоблокировки для базы данных Database1 (применяется только к базе данных SQL Azure версии 12).  

```sql
WITH CTE AS (  
       SELECT CAST(event_data AS XML)  AS [target_data_XML]  
   FROM sys.fn_xe_telemetry_blob_target_read_file('dl', null, null, null)  
)  
SELECT target_data_XML.value('(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
target_data_XML.query('/event/data[@name=''xml_report'']/value/deadlock') AS deadlock_xml,  
target_data_XML.query('/event/data[@name=''database_name'']/value').value('(/value)[1]', 'nvarchar(100)') AS db_name  
FROM CTE  
```

Следующий запрос возвращает события жесткого регулирования для рабочих потоков SQL, возникших с 10:00 до 11:00 на 25 сентября 2011 г. (UTC).  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'throttling'
    AND event_subtype = 4194307
    AND start_time >= '2011-09-25 10:00:00'
    AND end_time <= '2011-09-25 11:00:00';  
```

### <a name="db-scoped-extended-event"></a>DB-Scoped расширенное событие

 Используйте следующий пример кода для настройки сеанса расширенного события с областью действия базы данных (XEvent):  

```sql
IF EXISTS  
    (SELECT * from sys.database_event_sessions  
        WHERE name = 'azure_monitor_deadlock_session')  
BEGIN  
    ALTER EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE  
        DROP TARGET package0.ring_buffer;  
  
    DROP EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE;  
END  
  
CREATE EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    ADD EVENT sqlserver.database_xml_deadlock_report  
    ADD TARGET package0.ring_buffer  
    (  
        SET max_memory = 2048, max_events_limit = 10  
    )  
    WITH (STARTUP_STATE = ON,  
          EVENT_RETENTION_MODE = ALLOW_SINGLE_EVENT_LOSS);  
  
ALTER EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    STATE = START;  
```

### <a name="check-for-deadlock"></a>Проверить взаимоблокировку

Используйте следующий запрос, чтобы проверить, существует ли взаимоблокировка.  

```sql
WITH CTE AS (  
    SELECT CAST(xet.target_data AS XML)  AS [target_data_XML]  
        FROM            sys.dm_xe_database_session_targets AS xet  
             INNER JOIN sys.dm_xe_database_sessions        AS xe  
                 ON (xe.address = xet.event_session_address)  
        WHERE xe.name = 'azure_monitor_deadlock_session'  
)  
, CTE2 AS (  
    SELECT  
            T2.EventData.query('.').value(  
                '(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
            T2.EventData.query('.').query(  
                '(/event/data/value/deadlock)[1]')     AS deadlock_xml  
        FROM CTE  
            CROSS Apply [target_data_XML].nodes(  
                '/RingBufferTarget/event') AS T2(EventData)  
)  
SELECT * FROM CTE2;  
```

## <a name="see-also"></a>См. также:

 [Расширенные события в Базе данных SQL Azure](/azure/azure-sql/database/xevent-db-diff-from-svr)  
