---
description: sys.dm_os_sys_info (Transact-SQL)
title: sys.dm_os_sys_info (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_sys_info_TSQL
- dm_os_sys_info
- dm_os_sys_info_TSQL
- sys.dm_os_sys_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_info dynamic management view
- time [SQL Server], instance started
- starting time
ms.assetid: 20f6bc9c-839a-4fa4-b3f3-a6c47d1b69af
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eede187e90a838ae0e0a2c0051a5b4d496c7b4b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096544"
---
# <a name="sysdm_os_sys_info-transact-sql"></a>sys.dm_os_sys_info (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает разнородный набор полезных сведений о компьютере, а также о ресурсах, доступных для служб [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] и используемых ими.  
  
> **Примечание.** Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_sys_info**.  
  
|Имя столбца|Тип данных|Примечания и описание для конкретной версии |  
|-----------------|---------------|-----------------|  
|**cpu_ticks**|**bigint**|Задает текущий счетчик тактов времени ЦП. Метки времени ЦП поступают от счетчика процессора RDTSC. Это монотонно возрастающее число. Не допускает значения NULL.|  
|**ms_ticks**|**bigint**|Указывает число миллисекунд, прошедших со времени запуска компьютера. Не допускает значения NULL.|  
|**cpu_count**|**int**|Указывает количество логических процессоров в системе. Не допускает значения NULL.|  
|**hyperthread_ratio**|**int**|Указывает количество логических или физических ядер, соответствующих одному физическому пакету процессора. Не допускает значения NULL.|  
|**physical_memory_in_bytes**|**bigint**|**Применимо к:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с помощью [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Указывает общий объем физической памяти компьютера. Не допускает значения NULL.|  
|**physical_memory_kb**|**bigint**|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше.<br /><br /> Указывает общий объем физической памяти компьютера. Не допускает значения NULL.|  
|**virtual_memory_in_bytes**|**bigint**|**Применимо к:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с помощью [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Объем виртуальной памяти, доступной процессу в пользовательском режиме. Это значение можно использовать для определения того, был ли SQL Server запущен с параметром 3-GB.|  
|**virtual_memory_kb**|**bigint**|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше.<br /><br /> Указывает общий объем виртуального адресного пространства, доступного процессу в пользовательском режиме. Не допускает значения NULL.|  
|**bpool_committed**|**int**|**Применимо к:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с помощью [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Представляет фиксированную физическую память в килобайтах (КБ) в диспетчере памяти. Не включает зарезервированную память в диспетчере памяти. Не допускает значения NULL.|  
|**committed_kb**|**int**|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше.<br /><br /> Представляет фиксированную физическую память в килобайтах (КБ) в диспетчере памяти. Не включает зарезервированную память в диспетчере памяти. Не допускает значения NULL.|  
|**bpool_commit_target**|**int**|**Применимо к:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с помощью [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Представляет объем памяти, в килобайтах (КБ), доступный диспетчеру памяти SQL Server.|  
|**committed_target_kb**|**int**|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше.<br /><br /> Представляет объем памяти, в килобайтах (КБ), доступный диспетчеру памяти SQL Server. Целевой объем вычисляется с помощью разнообразных входных данных, в том числе:<br /><br /> — Текущее состояние системы, включая ее загрузку;<br /><br /> — память, запрашиваемая текущими процессами<br /><br /> — объем памяти, установленной на компьютере;<br /><br /> параметры конфигурации<br /><br /> Если **committed_target_kb** больше **committed_kb**, диспетчер памяти попытается получить дополнительную память. Если **committed_target_kb** меньше **committed_kb**, диспетчер памяти попытается уменьшить зафиксированный объем памяти. **Committed_target_kb** всегда содержит украденную и зарезервированную память. Не допускает значения NULL.|  
|**bpool_visible**|**int**|**Применимо к:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] с помощью [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Количество буферов по 8 KБ в буферном пуле, которые непосредственно доступны в виртуальном адресном пространстве процесса. Если расширения AWE не используются, то при получении буферным пулом целевого объема памяти (bpool_committed = bpool_commit_target) значение bpool_visible становится равным значению bpool_committed. Если расширения AWE используются на 32-разрядной версии SQL Server, bpool_visible представляет размер отображаемой сопоставленной памяти AWE, которая используется для доступа к физической памяти, выделенной под буферный пул. Размер отображаемой сопоставленной памяти привязан к адресному пространству процесса, поэтому видимый объем памяти будет меньше, чем фиксированный объем, и в дальнейшем может быть уменьшен внутренними компонентами, которые потребляют память для целей, не связанных со страницами базы данных. Если значение bpool_visible слишком маленькое, есть вероятность получения ошибок нехватки памяти.|  
|**visible_target_kb**|**int**|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше.<br /><br /> Совпадает с **committed_target_kb**. Не допускает значения NULL.|  
|**stack_size_in_bytes**|**int**|Указывает размер стека вызова для каждого потока, созданного [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Не допускает значения NULL.|  
|**os_quantum**|**bigint**|Представляет такт времени для задач без вытеснения, выраженный в миллисекундах. Такт (в секундах) = **os_quantum** /тактовая частота ЦП. Не допускает значения NULL.|  
|**os_error_mode**|**int**|Задает режим ошибки для процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Не допускает значения NULL.|  
|**os_priority_class**|**int**|Указывает класс приоритета для процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Допускает значение NULL.<br /><br /> 32 = нормальный (журнал ошибок будет выдавать сообщение, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] начинает выполняться с обычной базой приоритетов (= 7)).<br /><br /> 128 = высокий (журнал ошибок будет выдавать сообщение, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется с высокой базой приоритетов  (=13)).<br /><br /> Дополнительные сведения см. в статье [Настройка параметра конфигурации сервера priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).|  
|**max_workers_count**|**int**|Представляет максимальное число исполнителей, которые могут быть созданы. Не допускает значения NULL.|  
|**scheduler_count**|**int**|Представляет число пользовательских планировщиков, настроенных при выполнении процесса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Не допускает значения NULL.|  
|**scheduler_total_count**|**int**|Представляет общее число планировщиков в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Не допускает значения NULL.|  
|**deadlock_monitor_serial_number**|**int**|Указывает идентификатор текущей последовательности монитора взаимоблокировок. Не допускает значения NULL.|  
|**sqlserver_start_time_ms_ticks**|**bigint**|Представляет **ms_tick** номер при [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] последнем запуске. Сравнивается с текущим столбцом ms_ticks. Не допускает значения NULL.|  
|**sqlserver_start_time**|**datetime**|Указывает дату и время [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] последнего запуска локальной системы. Не допускает значения NULL.|  
|**affinity_type**|**int**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Задает текущий используемый тип привязки процесса к процессорам. Не допускает значения NULL. Дополнительные сведения см. в разделе [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).<br /><br /> 1 = MANUAL<br /><br /> 2 = AUTO|  
|**affinity_type_desc**|**varchar(60)**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Описывает столбец **affinity_type** . Не допускает значения NULL.<br /><br /> MANUAL = сходство было задано хотя бы для одного ЦП.<br /><br /> AUTO = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может свободно перемещать потоки между процессорами.|  
|**process_kernel_time_ms**|**bigint**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Общее время в миллисекундах, затраченное всеми потоками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в режиме ядра. Это значение может быть больше, чем время одного процессора, поскольку оно включает в себя время всех процессоров сервера. Не допускает значения NULL.|  
|**process_user_time_ms**|**bigint**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Общее время в миллисекундах, затраченное всеми потоками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в пользовательском режиме. Это значение может быть больше, чем время одного процессора, поскольку оно включает в себя время всех процессоров сервера. Не допускает значения NULL.|  
|**time_source**|**int**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Указывает API-интерфейс, который службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используют для извлечения реального времени. Не допускает значения NULL.<br /><br /> 0 = QUERY_PERFORMANCE_COUNTER<br /><br /> 1 = MULTIMEDIA_TIMER|  
|**time_source_desc**|**nvarchar(60)**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Описывает столбец **time_source** . Не допускает значения NULL.<br /><br /> QUERY_PERFORMANCE_COUNTER = API [QueryPerformanceCounter](/windows/win32/api/profileapi/nf-profileapi-queryperformancecounter) извлекает время стены.<br /><br /> MULTIMEDIA_TIMER = API [таймера мультимедиа](/previous-versions//ms713418(v=vs.85)) , который получает время стены.|  
|**virtual_machine_type**|**int**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Указывает, выполняется ли [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в виртуальной среде.  Не допускает значения NULL.<br /><br /> 0 = нет<br /><br /> 1 = HYPERVISOR<br /><br /> 2 = OTHER|  
|**virtual_machine_type_desc**|**nvarchar(60)**|**Применимо к**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] и выше.<br /><br /> Описывает столбец **virtual_machine_type** . Не допускает значения NULL.<br /><br /> NONE = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не работает внутри виртуальной машины.<br /><br /> ГИПЕРВИЗОР = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] работает внутри виртуальной машины, размещенной под управлением низкоуровневой оболочки (ОС узла, использующей аппаратную виртуализацию).<br /><br /> OTHER = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] работает внутри виртуальной машины, размещенной в ОС, которая не использует помощник по оборудованию, например Microsoft Virtual PC.|  
|**softnuma_configuration**|**int**|**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше.<br /><br /> Задает способ настройки узлов NUMA. Не допускает значения NULL.<br /><br /> 0 = ВЫКЛЮЧЕНо указывает оборудование по умолчанию<br /><br /> 1 = Автоматизированная программная архитектура NUMA<br /><br /> 2 = ручная программная NUMA с помощью реестра|  
|**softnuma_configuration_desc**|**nvarchar(60)**|**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и выше.<br /><br /> OFF = функция Soft-NUMA ОТКЛЮЧЕНа<br /><br /> ON = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически определяет размеры узлов NUMA для программной архитектуры NUMA<br /><br /> ВРУЧНУЮ = программная архитектура NUMA настроена вручную|
|**process_physical_affinity**|**nvarchar (3072)** |**Применимо к:** Начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] .<br /><br />Информация, которую еще нужно получить. |
|**sql_memory_model**|**int**|**Применимо к:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 и более поздние версии.<br /><br />Указывает модель памяти, используемую [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для выделения памяти. Не допускает значения NULL.<br /><br />1 = Стандартная модель памяти<br />2 = Блокировка страниц в памяти<br /> 3 = большие страницы в памяти;|
|**sql_memory_model_desc**|**nvarchar(120)**|**Применимо к:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 и более поздние версии.<br /><br />Указывает модель памяти, используемую [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для выделения памяти. Не допускает значения NULL.<br /><br />  =  Стандартные [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует обычную модель памяти для выделения памяти. Это модель памяти SQL по умолчанию, когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] учетная запись службы не имеет прав блокировки страниц в памяти во время запуска.<br />  =  LOCK_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует блокировку страниц в памяти для выделения памяти. Это диспетчер памяти SQL по умолчанию, когда SQL Server учетной записи службы с правами на блокировку страниц в памяти во время запуска SQL Server.<br />   =  LARGE_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует большие страницы в памяти для выделения памяти. SQL Server использует механизм распределения больших страниц для выделения памяти только в выпуске Enterprise Edition, когда SQL Server учетная запись службы обладает правом на блокировку страниц в памяти во время запуска сервера и если включен флаг трассировки 834.|
|**pdw_node_id**|**int**|**Применимо к:** [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
|**socket_count** |**int** | **Применимо к:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 2 (SP2) и выше.<br /><br />Указывает число разъемов процессора, доступных в системе. |  
|**cores_per_socket** |**int** | **Применимо к:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 2 (SP2) и выше.<br /><br />Указывает число процессоров на каждый сокет, доступный в системе. |  
|**numa_node_count** |**int** | **Применимо к:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 2 (SP2) и выше.<br /><br />Указывает количество узлов NUMA, доступных в системе. В этом столбце содержатся физические узлы NUMA, а также мягкие узлы NUMA. |  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
