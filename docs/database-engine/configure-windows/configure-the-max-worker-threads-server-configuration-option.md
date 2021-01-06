---
title: Настройка параметра конфигурации сервера max worker threads | Документы Майкрософт
description: Узнайте, как использовать параметр max worker threads для настройки количества рабочих потоков, доступных SQL Server для обработки определенных запросов.
ms.custom: contperf-fy20q4
ms.date: 04/14/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- worker threads [SQL Server]
- max worker threads option
ms.assetid: abeadfa4-a14d-469a-bacf-75812e48fac1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89da087dd829642653fee6ae8648af3c1c00c2a0
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878900"
---
# <a name="configure-the-max-worker-threads-server-configuration-option"></a>Настройка параметра конфигурации сервера max worker threads
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В этом разделе описываются способы настройки параметра конфигурации сервера **max worker threads** в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Параметр **max worker threads** задает количество рабочих потоков, доступных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для обработки запросов входа, выхода и аналогичных запросов приложений.


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует собственные службы потоков операционных систем, чтобы обеспечить соблюдение следующих условий.

- Один или несколько потоков одновременно поддерживают все сети, поддерживаемые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Один поток обрабатывает контрольные точки базы данных.

- Пул потоков обрабатывает запросы от всех пользователей.

Значение по умолчанию для параметра **max worker threads** — 0. Это позволяет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически настраивать количество рабочих потоков при запуске. Настройка по умолчанию является оптимальной для большинства систем. Но иногда, в зависимости от конфигурации системы, установка параметра **max worker threads** в другое определенное значение может улучшить производительность.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Если реальное количество потоков превышает число, заданное параметром **max worker threads**, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует пул рабочих потоков, так что следующий доступный рабочий поток сможет обработать запрос. Рабочий поток назначается только активным запросам и освобождается после обслуживания запроса. Это происходит в случае, даже если сеанс пользователя или подключение, в которых был создан запрос, остаются открытыми. 

-   Параметр конфигурации сервера **max worker threads** не ограничивает все потоки, которые могут быть порождены в ядре. Системные потоки, требуемые для LazyWriter, контрольной точки, модуля записи журнала, Service Broker, диспетчера блокировок или других задач, порождаются независимо от этого ограничения. Группы доступности используют некоторые рабочие потоки в рамках ограничения **max worker thread limit**, но также используют системные потоки (см. раздел [Использование потока группами доступности](../availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md#ThreadUsage)). Если число настроенных потоков превышается, то следующий запрос будет содержать сведения о системных задачах, породивших дополнительные потоки.  
  
 ```sql  
 SELECT  s.session_id, r.command, r.status,  
    r.wait_type, r.scheduler_id, w.worker_address,  
    w.is_preemptive, w.state, t.task_state,  
    t.session_id, t.exec_context_id, t.request_id  
 FROM sys.dm_exec_sessions AS s  
 INNER JOIN sys.dm_exec_requests AS r  
    ON s.session_id = r.session_id  
 INNER JOIN sys.dm_os_tasks AS t  
    ON r.task_address = t.task_address  
 INNER JOIN sys.dm_os_workers AS w  
    ON t.worker_address = w.worker_address  
 WHERE s.is_user_process = 0;  
 ```  
  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Это расширенный параметр, и изменять его следует только опытным администраторам баз данных или сертифицированным по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] специалистам. Если вы считаете, что есть проблема с производительностью, вероятно, причина не в доступности рабочих потоков. Скорее всего, причина связана с действиями, которые занимают рабочие потоки и не освобождают их. В число примеров входят длительные запросы или узкие места в системе (операции ввода-вывода, блокировка, ожидания кратковременной блокировки, ожидания сетевых операций). Рекомендуется найти причину проблемы производительности, прежде чем изменять параметр max worker threads. Дополнительные сведения об оценке производительности см. в статье [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md).
  
-   Пул потоков помогает оптимизировать производительность при подключении к серверу большого числа клиентов. Обычно для каждого запроса в операционной системе создается отдельный поток. Однако в случае сотен соединений с сервером, использование одного потока на каждый запрос приводит к потреблению большого числа системных ресурсов. Параметр **max worker threads** позволяет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создавать пул рабочих потоков, чтобы обслужить большое число запросов, что улучшает производительность.  
  
-   В следующей таблице показано автоматически настроенное число рабочих потоков (если задано значение, равное 0) для различных сочетаний ЦП, архитектуры компьютера и версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием формулы: **_Максимальное число рабочих потоков по умолчанию_ + ((* логические ЦП* – 4) * *рабочие потоки на ЦП*)**.  
  
    |Число процессоров|32-разрядный компьютер (до [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])|64-разрядный компьютер (до [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1)|64-разрядный компьютер (начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])|   
    |------------|------------|------------|------------|  
    |\< = 4|256|512|512|   
    |8|288|576|576|   
    |16|352|704|704|   
    |32|480|960|960|   
    |64|736|1472|1472|   
    |128|1248|2496|4480|   
    |256|2272|4544|8576|   
    
    До [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 *Число рабочих потоков на ЦП* зависит только от архитектуры (32- или 64-разрядная):
    
    |Число процессоров|32-разрядный компьютер <sup>1</sup>|64-разрядный компьютер|   
    |------------|------------|------------|   
    |\< = 4|256|512|   
    |\> 4|256 + ((число логических ЦП – 4) * 8)|512 <sup>2</sup> + ((число логических ЦП – 4) * 16)|   
    
    Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], *Число рабочих потоков на ЦП* зависит от архитектуры и количества процессоров (от 4 до 64 или больше 64):
    
    |Число процессоров|32-разрядный компьютер <sup>1</sup>|64-разрядный компьютер|   
    |------------|------------|------------|   
    |\< = 4|256|512|   
    |\> 4 и \< = 64|256 + ((число логических ЦП – 4) * 8)|512 <sup>2</sup> + ((число логических ЦП – 4) * 16)|   
    |\> 64|256 + ((число логических ЦП – 4) * 32)|512 <sup>2</sup> + ((число логических ЦП – 4) * 32)|   
  
    <sup>1</sup> Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] больше нельзя устанавливать в 32-разрядной операционной системе. Значения для 32-разрядного компьютера приведены, чтобы помочь клиентам, работающим с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более ранних версий. Для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], работающего на 32-разрядном компьютере, рекомендуется ограничить число рабочих потоков до 1024.
    
    <sup>2</sup> Начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], *Максимальное число рабочих потоков по умолчанию* делится на 2 для компьютеров с объемом памяти менее 2 ГБ.
  
    > [!TIP]  
    > Рекомендации по использованию процессоров в количестве, превышающем 64, см. в разделе [Рекомендации по использованию SQL Server на компьютерах, которые имеют более 64 процессоров](../../relational-databases/thread-and-task-architecture-guide.md#best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus).  
  
-   Если все рабочие потоки заняты выполнением длительных запросов, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может не отвечать на другие запросы, пока один из потоков не завершит работу и не станет доступным. Хотя это и не ошибка, такое поведение иногда нежелательно. Если процесс не отвечает и новые запросы не могут быть обработаны, подключитесь к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] через выделенное административное соединение (DAC) и уничтожьте процесс. Во избежание этого увеличьте максимальное число потоков управления.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Разрешения на выполнение хранимой процедуры **sp_configure** без параметров или только с первым параметром по умолчанию предоставляются всем пользователям. Для выполнения процедуры **sp_configure** с обоими параметрами для изменения параметра конфигурации или запуска инструкции `RECONFIGURE` необходимо иметь разрешение `ALTER SETTINGS` на уровне сервера. Разрешение `ALTER SETTINGS` неявным образом предоставлено предопределенным ролям сервера **sysadmin** и **serveradmin**.  
  
##  <a name="using-ssmanstudiofull"></a><a name="SSMSProcedure"></a> Использование [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Настройка параметра max worker threads  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши сервер и выберите пункт **Свойства**.  
  
2.  Щелкните узел **Процессоры** .  
  
3.  В поле **Максимальное число рабочих потоков** введите или выберите значение от 128 до 32 767.  
  
> [!TIP]
> Используйте параметр **max worker threads** для установки количества рабочих потоков, доступных процессам [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Значение по умолчанию параметра **max worker threads** является оптимальным для большинства систем. Но в зависимости от конфигурации системы установка параметра **max worker threads** в меньшее значение может улучшить производительность.
> Дополнительные сведения см. в разделе [Рекомендации](#Recommendations) на этой странице.
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Настройка параметра max worker threads  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере описывается использование процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) для задания значения параметра `max worker threads` равным `900`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'max worker threads', 900 ;  
GO  
RECONFIGURE;  
GO  
```  
  
##  <a name="follow-up-after-you-configure-the-max-worker-threads-option"></a><a name="FollowUp"></a> Дальнейшие действия. После настройки параметра "Максимальное количество рабочих потоков"  
 Изменения вступят в силу сразу после выполнения [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md) без необходимости перезапуска [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="see-also"></a>См. также:  
 [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Диагностическое соединение для администраторов баз данных](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)
