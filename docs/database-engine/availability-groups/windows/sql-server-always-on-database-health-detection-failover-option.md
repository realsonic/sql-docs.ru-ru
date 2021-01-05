---
title: Определение уровня работоспособности баз данных
description: Сведения о функции определения работоспособности уровня базы данных, доступной для группы доступности Always On SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- AlwaysOn
- DB_FAILOVER
- Always On
- High Availability
- SQL Server
ms.assetid: d74afd28-25c3-48a1-bc3f-e353bee615c2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1d0abb2a078c9dd2e7f540ee05b70b60dd756a34
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639199"
---
# <a name="availability-group-database-level-health-detection-failover-option"></a>Параметр определения уровня работоспособности базы данных группы доступности
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Начиная с SQL Server 2016, при настройке группы доступности AlwaysOn стал доступен параметр определения уровня работоспособности базы данных (DB_FAILOVER). Функция определения уровня работоспособности баз данных замечает, когда база данных выходит из сетевого режима, в случае если возникают какие-либо неполадки, и инициирует автоматический переход группы доступности на другой ресурс.

Этот параметр включен для группы доступности в целом, поэтому определение уровня работоспособности базы данных отслеживает каждую базу данных в группе доступности. Он не может быть включен выборочно для конкретных баз данных в группе доступности.

## <a name="benefits-of-database-level-health-detection-option"></a>Преимущества параметра определения уровня работоспособности базы данных
---
Параметр определения уровня работоспособности базы данных группы доступности является хорошим рекомендуемым вариантом для обеспечения высокого уровня доступности баз данных. Рекомендуется настроить его для всех групп доступности. Если высокая доступность приложения зависит от нескольких баз данных, объедините их в группу доступности с включенным параметром определения работоспособности базы данных.

Например, если параметр определения уровня работоспособности базы данных включен и SQL Server не удалось выполнить запись в файл журнала транзакций для одной из баз данных, состояние этой базы данных изменится и будет указывать на сбой. Вскоре произойдет отработка отказа группы доступности, после чего приложение сможет повторно подключиться и продолжить работу с минимальным нарушением после перехода баз данных в сетевой режим.

### <a name="enabling-database-level-health-detection"></a>Включение определения уровня работоспособности базы данных

Несмотря на то, что это обычно рекомендуется сделать, параметр определения работоспособности базы данных **отключен по умолчанию** с целью обеспечения обратной совместимости с параметрами по умолчанию в предыдущих версиях.

Существует несколько простых способов включения параметра определения уровня работоспособности базы данных.

1. В среде SQL Server Management Studio подключитесь к ядру СУБД. В окне "Обозреватель объектов" щелкните правой кнопкой мыши узел "Высокий уровень доступности AlwaysOn" и запустите **мастер создания группы доступности**. На странице "Укажите имя" установите флажок **Определение уровня работоспособности баз данных**. Затем выполните действия на оставшихся страницах мастера.

   ![Флажок включения определения работоспособности базы данных для групп AlwaysOn](../../../database-engine/availability-groups/windows/media/always-on-enable-database-health-checkbox.png)

2. Просмотрите **Свойства** существующей группы доступности в среде SQL Server Management Studio. Подключитесь к серверу SQL Server. В окне "Обозреватель объектов" разверните узел "Высокий уровень доступности AlwaysOn". Разверните узел "Группы доступности". Щелкните правой кнопкой группу доступности и выберите пункт "Свойства". Выберите параметр **Определение уровня работоспособности баз данных**, а затем нажмите кнопку "ОК" или зафиксируйте изменение.

   ![Свойства группы доступности AlwaysOn — определение уровня работоспособности баз данных](../../../database-engine/availability-groups/windows/media/always-on-ag-properties-database-level-health-detection.png)


3. Синтаксис Transact-SQL для **CREATE AVAILABILITY GROUP**. Параметр DB_FAILOVER принимает значения ON или OFF.

   ```sql
   CREATE AVAILABILITY GROUP [Contoso-ag]
   WITH (DB_FAILOVER=ON)
   FOR DATABASE [AutoHa-Sample]
   REPLICA ON
       N'SQLSERVER-0' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-0.DOMAIN.COM:5022',
         FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
       N'SQLSERVER-1' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-1.DOMAIN.COM:5022',
        FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
    ```

4. Синтаксис Transact-SQL для **ALTER AVAILABILITY GROUP**. Параметр DB_FAILOVER принимает значения ON или OFF.

   ```sql
   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = ON);

   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = OFF);
   ```

### <a name="caveats"></a>Предупреждения

Важно отметить, что в настоящее время параметр определения уровня работоспособности баз данных не запускает SQL Server для наблюдения за бесперебойной работой диска и SQL Server не отслеживает доступность файла базы данных напрямую. Отказ или недоступность диска необязательно станет причиной автоматического перехода группы доступности на другой ресурс.

Например, если во время простоя базы данных и при отсутствии активных транзакций и физических операций записи некоторые файлы базы данных станут недоступны, SQL Server может не выполнить операции чтения или записи в файлы и не изменить состояние для этой базы данных немедленно, поэтому отработка отказа запущена не будет. Позже, когда появится контрольная точка базы данных или произойдет физическая операция чтения или записи для выполнении запроса, SQL Server может обнаружить проблему с файлом и среагировать путем изменения состояния базы данных. Впоследствии группа доступности с включенным параметром определения уровня работоспособности будет переведена на другой ресурс из-за изменения работоспособности базы данных.

Еще один пример: когда ядру СУБД SQL Server требуется считать страницу данных для выполнения запроса, а страница данных кэшируется в память буферного пула, то для выполнения запроса может не потребоваться чтение с диска с физическим доступом. Таким образом, отсутствующий или недоступный файл данных не может немедленно запустить автоматический переход на другой ресурс даже в том случае, если включен параметр определения работоспособности базы данных, так как состояние базы данных не имеет значения "Немедленно".


## <a name="database-failover-is-separate-from-flexible-failover-policy"></a>Отработка отказа базы данных отличается от политики гибкой отработки отказа.
Определение уровня работоспособности базы данных реализует политику гибкой отработки отказа, которая настраивает пороговые значения работоспособности процесса SQL Server для политики отработки отказа. Определение уровня работоспособности базы данных настраивается с помощью параметра DB_FAILOVER, тогда как для настройки определения работоспособности процесса SQL Server используется отдельный параметр группы доступности FAILURE_CONDITION_LEVEL. Два параметра являются независимыми.

## <a name="managing-and-monitoring-database-level-health-detection"></a>Мониторинг определения уровня работоспособности базы данных и управление им

### <a name="dynamic-management-views"></a>Динамические административные представления

В системном DMV sys.availability_groups отображается столбец db_failover, который указывает, отключен (0) или включен (1) параметр определения уровня работоспособности базы данных.

```sql
select name, db_failover from sys.availability_groups
```


Пример выходных данных DMV:

|name  |  db_failover|
|---------|---------|
| Contoso-ag | 1  |

### <a name="errorlog"></a>ErrorLog
В журнале ошибок SQL Server (или тексте из sp_readerrorlog) будет отображаться сообщение об ошибке 41653 в случае отработки отказа группы доступности в результате проверок определения уровня работоспособности базы данных.

Например, в этом фрагменте журнала ошибок показано, что произошел сбой записи в журнал транзакций из-за проблем с диском и впоследствии была завершена работа базы данных с именем AutoHa Sample, в результате чего определение уровня работоспособности базы данных запустило отработку отказа группы доступности.

>2016-04-25 12:20:21.08 spid1s      Error: 17053, Severity: 16, состояние: 1.
>
>2016-04-25 12:20:21.08 spid1s      SQLServerLogMgr::LogWriter: Operating system error 21(The device is not ready.) encountered.
>2016-04-25 12:20:21.08 spid1s      Ошибка записи во время сброса журнала.
>
>2016-04-25 12:20:21.08 spid79      Error: 9001, Severity: 21, State: 4.
>
>2016-04-25 12:20:21.08 spid79      Журнал для базы данных "AutoHa-Sample" недоступен. Проверьте журнал событий на наличие сообщений о связанных ошибках. Устраните все ошибки и заново запустите базу данных.
>
>**2016-04-25 12:20:21.15 spid79      Error: 41653, Severity: 21, State: 1.**
>
>**2016-04-25 12:20:21.15 spid79      Database 'AutoHa-Sample' encountered an error (error type: 2 'DB_SHUTDOWN') causing failure of the availability group 'Contoso-ag'.  Дополнительные сведения об обнаруженных ошибках см. в журнале ошибок SQL Server.  Если эта проблема сохраняется, обратитесь к системному администратору.**
>
>2016-04-25 12:20:21.17 spid79      State information for database 'AutoHa-Sample' - Hardened Lsn: '(34:664:1)'    Commit LSN: '(34:656:1)'    Commit Time: 'Apr 25 2016 12:19PM'
>
>2016-04-25 12:20:21.19 spid15s     Подключение групп доступности AlwaysOn к базе данных-получателю завершено для базы данных-источника "AutoHa-Sample" в реплике доступности "SQLServer-0" с ИД реплики: {c4ad5ea4-8a99-41fa-893e-189154c24b49}. Это информационное сообщение. Вмешательство пользователя не требуется.
>
>2016-04-25 12:20:21.21 spid75      Always On: The local replica of availability group 'Contoso-ag' is preparing to transition to the resolving role in response to a request from the Windows Server Failover Clustering (WSFC) cluster. Это информационное сообщение. Вмешательство пользователя не требуется.
>
>2016-04-25 12:20:21.21 spid75      Состояние локальной реплики доступности в группе доступности "ag" было изменено с "PRIMARY_NORMAL" на "RESOLVING_NORMAL".  Состояние изменено, так как группа доступности переходит в режим "вне сети".  Реплика переходит в автономный режим, поскольку связанная группа доступности была удалена или пользователь перевел связанную группу доступности в режим "вне сети" на консоли управления сервером отказоустойчивой кластеризации Windows (WSFC), или группа доступности переходит на другой экземпляр SQL Server.  Дополнительные сведения см. в журнале ошибок SQL Server, консоли управления отказоустойчивой кластеризации Windows Server (WSFC) или журнале WSFC.

### <a name="extended-event-sqlserveravailability_replica_database_fault_reporting"></a>Расширенное событие sqlserver.availability_replica_database_fault_reporting

В SQL Server 2016 определено новое расширенное событие, запускаемое определением уровня работоспособности базы данных.  Имя события — **sqlserver.availability_replica_database_fault_reporting**.

Это событие XEvent запускается только в первичной реплике. Оно запускается при обнаружении проблемы с уровнем работоспособности базы данных, размещенной в группе доступности.

Ниже приведен пример создания сеанса XEvent, который записывает это событие. Так как путь не указан, выходной файл XEvent должен находиться в пути к журналу ошибок SQL Server по умолчанию. В первичной реплике группы доступности выполните следующий скрипт:

Пример скрипта сеанса расширенных событий

```sql
CREATE EVENT SESSION [AlwaysOn_dbfault] ON SERVER
ADD EVENT sqlserver.availability_replica_database_fault_reporting
ADD TARGET package0.event_file(SET filename=N'dbfault.xel',max_file_size=(5),max_rollover_files=(4))
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,
    MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)
GO
ALTER EVENT SESSION AlwaysOn_dbfault ON SERVER STATE=START
GO
```

#### <a name="extended-event-output"></a>Выходные данные расширенного события
С помощью среды SQL Server Management Studio подключитесь к основному серверу SQL Server, разверните узел "Управление", а затем узел "Расширенные события". Найдите сеанс (имя AlwaysOn_dbfault в примере выше) и разверните его для просмотра выходных файлов. Щелкните выходной файл, после чего файл события откроется в новой вкладке.

Описание полей:

|Столбец данных | Описание|
|---------|---------|
|availability_group_id |Идентификатор группы доступности.|
|availability_group_name |Имя группы доступности.|
|availability_replica_id |Идентификатор реплики доступности.|
|availability_replica_name |Имя реплики доступности.|
|database_name |Имя базы данных, сообщившей о сбое.|
|database_replica_id |Идентификатор реплики базы данных доступности.|
|failover_ready_replicas |Количество синхронизируемых вторичных реплик файлов для автоматического перехода в случае сбоя.|
|fault_type  | Идентификатор сообщенного сбоя. Возможные значения:  <br/> 0 — отсутствует <br/>1 — неизвестно<br/>2 — завершение работы|
|is_critical | Это значение всегда должно возвращать true для XEvent, начиная с SQL Server 2016.|


В выходных данных этого примера fault_type показывает, что в группе доступности "Contoso-ag" в реплике "SQLSERVER-1" произошло критическое событие из-за имени базы данных "AutoHa-Sample2" с типом сбоя "2 — завершение работы".

|Поле  | Значение|
|---------|---------|
|availability_group_id | 24E6FE58-5EE8-4C4E-9746-491CFBB208C1|
|availability_group_name | Contoso-ag|
|availability_replica_id | 3EAE74D1-A22F-4D9F-8E9A-DEFF99B1F4D1|
|availability_replica_name | SQLSERVER-1|
|database_name | AutoHa-Sample2|
|database_replica_id | 39971379-8161-4607-82E7-098590E5AE00|
|failover_ready_replicas | 1|
|fault_type | 2|
|is_critical | True|


### <a name="related-references"></a>Связанные справочники

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)

* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)

* [Гибкая политика отработки отказа для автоматического перехода на другой ресурс группы доступности (SQL Server)](./configure-flexible-automatic-failover-policy.md)

* [Enhance AlwaysOn Failover Policy to Test SQL Server Database Data and Log Drives](/archive/blogs/alwaysonpro/enhance-alwayson-failover-policy-to-test-sql-server-database-data-and-log-drives) (Улучшения политики перехода на другой ресурс AlwaysOn для тестирования данных базы данных и дисков журнала SQL Server)

* [Расширенные события](../../../relational-databases/extended-events/extended-events.md)