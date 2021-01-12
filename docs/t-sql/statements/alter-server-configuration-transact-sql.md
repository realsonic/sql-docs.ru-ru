---
description: ALTER SERVER CONFIGURATION (Transact-SQL)
title: ALTER SERVER CONFIGURATION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SERVER CONFIGURATION
- ALTER_SERVER_CONFIGURATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER CONFIGURATION, ALTER
- process affinity, setting
- ALTER SERVER CONFIGURATION statement
- setting process affinity
ms.assetid: f3059e42-5f6f-4a64-903c-86dca212a4b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5fc18fa93e47267d299c3b568011de5645f943da
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092039"
---
# <a name="alter-server-configuration-transact-sql"></a>ALTER SERVER CONFIGURATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Изменяет глобальные параметры конфигурации текущего сервера в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  

```syntaxsql
ALTER SERVER CONFIGURATION  
SET <optionspec>   
[;]  
  
<optionspec> ::=  
{  
     <process_affinity>  
   | <diagnostic_log>  
   | <failover_cluster_property>  
   | <hadr_cluster_context>  
   | <buffer_pool_extension>  
   | <soft_numa>  
   | <memory_optimized>
}  
  
<process_affinity> ::=   
   PROCESS AFFINITY   
   {  
     CPU = { AUTO | <CPU_range_spec> }   
   | NUMANODE = <NUMA_node_range_spec>   
   }  
   <CPU_range_spec> ::=   
      { CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]   
  
   <NUMA_node_range_spec> ::=   
      { NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID } [ ,...n ]  
  
<diagnostic_log> ::=   
   DIAGNOSTICS LOG   
   {   
     ON    
   | OFF    
   | PATH = { 'os_file_path' | DEFAULT }    
   | MAX_SIZE = { 'log_max_size' MB | DEFAULT }    
   | MAX_FILES = { 'max_file_count' | DEFAULT }    
   }  
  
<failover_cluster_property> ::=   
   FAILOVER CLUSTER PROPERTY <resource_property>  
   <resource_property> ::=  
      {  
        VerboseLogging = { 'logging_detail' | DEFAULT }    
      | SqlDumperDumpFlags = { 'dump_file_type' | DEFAULT }  
      | SqlDumperDumpPath = { 'os_file_path'| DEFAULT }  
      | SqlDumperDumpTimeOut = { 'dump_time-out' | DEFAULT }  
      | FailureConditionLevel = { 'failure_condition_level' | DEFAULT }  
      | HealthCheckTimeout = { 'health_check_time-out' | DEFAULT }  
      }  
  
<hadr_cluster_context> ::=  
   HADR CLUSTER CONTEXT = { 'remote_windows_cluster' | LOCAL }  
  
<buffer_pool_extension>::=  
    BUFFER POOL EXTENSION   
    { ON ( FILENAME = 'os_file_path_and_name' , SIZE = <size_spec> )   
    | OFF }  
  
    <size_spec> ::=  
        { size [ KB | MB | GB ] }  
  
<soft_numa> ::=  
    SOFTNUMA  
    { ON | OFF }  

<memory-optimized> ::=   
   MEMORY_OPTIMIZED   
   {   
     ON 
   | OFF
   | [ TEMPDB_METADATA = { ON [(RESOURCE_POOL='resource_pool_name')] | OFF }
   | [ HYBRID_BUFFER_POOL = { ON | OFF }
   }  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
**\<process_affinity> ::=**  
  
PROCESS AFFINITY  
Включает связывание потоков оборудования с процессорами.  
  
CPU = { AUTO | \<CPU_range_spec> }  
Распределяет рабочие потоки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на каждый ЦП в заданном диапазоне. Для процессоров вне заданного диапазона не назначены потоки.  
  
AUTO  
Указывает, что для потока не назначен ЦП. Разрешено свободное перемещение потоков операционной системой между процессорами в зависимости от рабочей нагрузки сервера. Это рекомендуемое значение по умолчанию.  
  
\<CPU_range_spec> ::=  
Указывает ЦП или диапазон процессоров, которым будут назначаться потоки.  
  
{ CPU_ID | CPU_ID  TO  CPU_ID } [ ,...n ]  
Список из одного или нескольких процессоров. Идентификаторы ЦП начинаются с 0 и имеют **целочисленное значение**.  
  
NUMANODE = \<NUMA_node_range_spec>  
Назначает потоки всем процессорам, принадлежащим заданному узлу NUMA или диапазону узлов.  
  
\<NUMA_node_range_spec> ::=  
Указывает номер узла NUMA или диапазон узлов NUMA.  
  
{ NUMA_node_ID | NUMA_node_ID  TO NUMA_node_ID } [ ,...n ]  
Это список из одного или нескольких узлов NUMA. Идентификаторы NUMA начинаются с 0 и имеют **целочисленное значение**.  
  
**\<diagnostic_log> ::=**  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  

  
DIAGNOSTICS LOG  
Запускает или останавливает запись в журнал диагностических данных, полученных с помощью хранимой процедуры sp_server_diagnostics. Этот аргумент также задает параметры конфигурации журналов SQLDIAG, такие как количество переключений файлов журнала, размер файлов журнала и расположение файлов. Дополнительные сведения см. в статье [Просмотр и чтение журнала диагностики экземпляра отказоустойчивого кластера](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md).  
  
ON  
Запускает запись диагностических данных [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] в расположении, указанном в параметре файла PATH. Это аргумент по умолчанию.  
  
OFF  
Прекращает запись в журнал диагностических данных.  
  
PATH = { 'os_file_path' | DEFAULT }  
Путь, определяющий расположение журналов диагностики. Расположение по умолчанию — \<\MSSQL\Log> в папке установки экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
MAX_SIZE = { 'log_max_size' MB | DEFAULT }  
Максимальный размер каждого из журналов диагностики в мегабайтах. Значение по умолчанию равно 100 МБ.  
  
MAX_FILES = { 'max_file_count' | DEFAULT }  
Максимальное число файлов журналов диагностики, которые могут храниться на компьютере, прежде чем имеющиеся файлы будут очищены и использованы для новых журналов диагностики.  
  
**\<failover_cluster_property> ::=**  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
FAILOVER CLUSTER PROPERTY  
Изменяет свойства закрытого ресурса отказоустойчивого кластера SQL Server.  
  
VERBOSE LOGGING = { 'logging_detail' | DEFAULT }  
Задает уровень ведения журнала для отказоустойчивого кластера SQL Server. Параметр можно включить для записи дополнительных сведений в журналы ошибок в целях устранения неполадок.  
  
-   0 — ведение журнала отключено (по умолчанию)  
  
-   1 — только ошибки  
  
-   2 — ошибки и предупреждения  
  
В сценариях отработки отказа ресурсов библиотека DLL ресурсов SQL Server может получить файл дампа, прежде чем произойдет отработка отказа. Это относится к технологиям FCI и групп доступности. Когда библиотека DLL ресурсов SQL Server определяет, что ресурс SQL Server завершился сбоем, она использует служебную программу Sqldumper.exe для получения файла дампа процесса SQL Server. Чтобы убедиться, что служебная программа Sqldumper.exe успешно создает файл дампа при отработке отказа ресурса, необходимо задать следующие три свойства в качестве необходимых компонентов: SqlDumperDumpTimeOut, SqlDumperDumpPath, SqlDumperDumpFlags.

SQLDUMPEREDUMPFLAGS  
Определяет тип файлов дампа, создаваемых служебной программой SQLDumper в SQL Server. Значение по умолчанию — 0. Для этого параметра используются десятичные, а не шестнадцатеричные значения. Для мини-дампа используйте 288, для мини-дампа с косвенным использованием памяти — 296, для фильтрованного дампа используйте 33024. Для получения дополнительных сведений см. [статью базы знаний о служебной программе Dumper сервера SQL Server](https://go.microsoft.com/fwlink/?LinkId=206173).  
  
SQLDUMPERDUMPPATH = { 'os_file_path' | DEFAULT }  
Место, где служебная программа SQLDumper сохраняет файлы дампов. Для получения дополнительных сведений см. [статью базы знаний о служебной программе Dumper сервера SQL Server](https://go.microsoft.com/fwlink/?LinkId=206173).  
  
SQLDUMPERDUMPTIMEOUT = { 'dump_time-out' | DEFAULT }  
Максимальное время создания дампа программой SQLDumper в случае сбоя SQL Server (в миллисекундах). Значение по умолчанию равно 0, то есть время создания дампа неограниченно. Для получения дополнительных сведений см. [статью базы знаний о служебной программе Dumper сервера SQL Server](https://go.microsoft.com/fwlink/?LinkId=206173).  
  
 FAILURECONDITIONLEVEL = { 'failure_condition_level' | DEFAULT }  
 Условия, при которых должно произойти переключение при сбое или перезапуск экземпляра отказоустойчивого кластера SQL Server. Значение по умолчанию, равное 3, означает, что ресурс SQL Server будет переключаться на резервный ресурс или перезапускаться в случае критической ошибки сервера. Дополнительные сведения об этом и других уровнях условий ошибки см. в разделе [Настройка параметров свойства FailureConditionLevel](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).  
  
HEALTHCHECKTIMEOUT = { 'health_check_time-out' | DEFAULT }  
Время, в течение которого библиотека ресурсов компонента SQL Server Database Engine будет ждать сведений о состоянии сервера, прежде чем сервер переводится в категорию неотвечающих. Время ожидания указывается в миллисекундах. Значение по умолчанию равно 60 000 миллисекунд (60 секунд).  
  
**\<hadr_cluster_context> ::=**  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
HADR CLUSTER CONTEXT **=** { **'** _remote\_windows\_cluster_ **'** | LOCAL }  
Переключает контекст кластера HADR экземпляра сервера на указанный отказоустойчивый кластер Windows Server (WSFC). *Контекст кластера HADR* определяет кластер WSFC, который управляет метаданными для реплик доступности, размещенных в экземпляре сервера. Используйте параметр SET HADR CLUSTER CONTEXT только во время миграции с кластера [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] на экземпляр [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)] или более новой версии в новом кластере WSFC.  
  
Переключать контекст кластера HADR можно только с локального WSFC на удаленный. Затем вы можете переключиться обратно с удаленного WSFC на локальный WSFC. Контекст кластера HADR можно переключить на удаленный кластер, только если на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не размещено ни одной реплики доступности.  
  
Удаленный контекст кластера HADR можно переключить обратно на локальный кластер в любое время. Однако контекст нельзя переключать повторно, пока на экземпляре сервера содержатся реплики доступности. 
  
Для определения целевого кластера укажите одно из следующих значений:  
  
*кластер_windows*  
Сетевое имя кластера WSFC. Вы можете указать короткое имя или полное имя домена. Для поиска целевого IP-адреса короткого имени ALTER SERVER CONFIGURATION использует разрешение DNS. В некоторых ситуациях краткое имя может вызвать затруднения, и DNS может вернуть неправильный IP-адрес. Рекомендуется указывать полное имя домена.  
  
> [!NOTE] 
> Миграция между кластерами с помощью этого параметра больше не поддерживается. Для переноса между кластерами, используйте распределенную группу доступности или другой способ, например доставку журналов. 
  
LOCAL  
Локальный кластер WSFC.  
  
Дополнительные сведения см. в разделе [Смена контекста кластера HADR экземпляра сервера (SQL Server)](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md).  
  
**\<buffer_pool_extension>::=**  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
ON  
Обеспечивает возможность расширения буферного пула. Этот параметр расширяет размер буферного пула за счет использования энергонезависимого хранилища. Энергонезависимое хранилище, такое как твердотельные накопители (SSD), сохраняет чистые страницы данных в пуле. Дополнительные сведения об этой возможности см. в статье [Buffer Pool Extension](../../database-engine/configure-windows/buffer-pool-extension.md). Расширение буферного пула поддерживается не во всех выпусках SQL Server. Дополнительные сведения см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
FILENAME = 'os_file_path_and_name'  
Определяет путь к каталогу и имя файла кэша расширения буферного пула. Файл должен иметь расширение BPE. Отключите BUFFER POOL EXTENSION, прежде чем изменить FILENAME.  
  
SIZE = *size* [ **KB** | MB | GB ]  
Определяет размер кэша. Указание размера по умолчанию — KB. Минимальный размер — значение параметра Max Server Memory. Максимальный размер в 32 раза больше значения параметра Max Server Memory. Дополнительные сведения о параметре Max Server Memory см. в разделе [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Отключите BUFFER POOL EXTENSION, прежде чем изменить размер файла. Для указания размера меньше текущего нужно перезапустить экземпляр SQL Server для освобождения памяти. В противном случае заданный размер должен совпадать с текущим размером или превышать его.  
  
OFF  
Отключает параметр расширения буферного пула. Перед изменением любых связанных параметров, например размера или имени файла, отключите параметр расширения буферного пула. Если этот параметр отключен, все связанные данные конфигурации удаляются из реестра.  
  
> [!WARNING]  
>  Отключение расширения буферного пула может отрицательно сказаться на производительности сервера, поскольку размер буферного пула значительно сократится.  
  
**\<soft_numa>**  

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]).  
  
ON  
Включает автоматическое секционирование для разбиения крупных аппаратных узлов NUMA на более мелкие узлы NUMA. При изменении текущего значения потребуется перезапустить ядро СУБД.  
  
OFF  
Отключает автоматическое секционирование для разбиения крупных аппаратных узлов NUMA на более мелкие узлы NUMA. При изменении текущего значения потребуется перезапустить ядро СУБД.  

> [!WARNING]
> Существуют известные проблемы с работой инструкции ALTER SERVER CONFIGURATION с параметром SOFT NUMA и агентом SQL Server.  Ниже приведена рекомендуемая последовательность операций.  
> 1) Остановите экземпляр агента SQL Server.  
> 2) Выполнение инструкции ALTER SERVER CONFIGURATION с параметром SOFT NUMA.  
> 3) Повторно запустите экземпляр SQL Server.  
> 4) Запустите экземпляр агента SQL Server.  
  
**Дополнительные сведения:** Если вы выполняете инструкцию ALTER SERVER CONFIGURATION с командой SET SOFTNUMA до перезапуска службы SQL Server, то при остановке службы агента SQL Server будет выполнена команда T-SQL RECONFIGURE, которая вернет для параметра SOFTNUMA значение, заданное до выполнения ALTER SERVER CONFIGURATION. 

**\<memory_optimized> ::=**

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]).

ON <br>
Включает все функции уровня экземпляра, которые являются частью семейства функций [выполняющейся в памяти базы данных](../../relational-databases/in-memory-database.md). Сейчас к ним относятся [оптимизированные для памяти метаданные tempdb](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) и [гибридный буферный пул](../../database-engine/configure-windows/hybrid-buffer-pool.md). Для вступления в силу требуется перезагрузка.

OFF <br>
Отключает все функции уровня экземпляра, которые являются частью семейства функций выполняющейся в памяти базы данных. Для вступления в силу требуется перезагрузка.

TEMPDB_METADATA = ON | OFF <br>
Включает или отключает только оптимизированные для памяти метаданные tempdb. Для вступления в силу требуется перезагрузка. 

RESOURCE_POOL='имя_пула_ресурсов' <br>
В сочетании с TEMPDB_METADATA = ON задает пользовательский пул ресурсов, который нужно использовать для tempdb. Если значение не указано, используется пул по умолчанию. Этот пул должен уже существовать. Если пул недоступен при перезапуске службы, tempdb будет использовать пул по умолчанию.


HYBRID_BUFFER_POOL = ON | OFF <br>
Включает или отключает гибридный буферный пул на уровне экземпляра. Для вступления в силу требуется перезагрузка.


## <a name="general-remarks"></a>Общие замечания  
Для этой инструкции не требуется перезапуск [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если явно не указано обратное. Если это экземпляр отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], перезапуск ресурса кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не требуется.  
  
## <a name="limitations-and-restrictions"></a>Ограничения  
Эта инструкция не поддерживает триггеры DDL.  
  
## <a name="permissions"></a>Разрешения  
Требования:
- Разрешения `ALTER SETTINGS` для параметра сопоставления процессов.
- Разрешения `ALTER SETTINGS` и `VIEW SERVER STATE` для параметров журнала диагностики и свойств отказоустойчивого кластера.
- Разрешение `CONTROL SERVER` для параметра контекста кластера HADR.  
- Разрешение `ALTER SERVER STATE` для параметра расширения буферного пула.  
  
Библиотека DLL ресурсов компонента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] запускается от имени учетной записи Local System. В результате в учетной записи Local System должен быть доступ на чтение и запись к пути, указанному в параметре журнала диагностики.  
  
## <a name="examples"></a>Примеры  
  
|Категория|Используемые элементы синтаксиса|  
|--------------|------------------------------|  
|[Установка соответствия процессов](#Affinity)|CPU • NUMANODE • AUTO|  
|[Настройка параметров журнала диагностики](#Diagnostic)|ON • OFF • PATH • MAX_SIZE|  
|[Установка свойств отказоустойчивого кластера](#Failover)|HealthCheckTimeout|  
|[Изменение контекста кластера для реплики доступности](#ChangeClusterContextExample)|**'** *windows_cluster* **'**|  
|[Установка расширения буферного пула](#BufferPoolExtension)|РАСШИРЕНИЕ БУФЕРНОГО ПУЛА| 
|[Настройка параметров выполняющейся в памяти базы данных](#MemoryOptimized)|MEMORY_OPTIMIZED|

  
###  <a name="setting-process-affinity"></a><a name="Affinity"></a> Установка соответствия процессов  
В примерах этого раздела показано соответствие процессов центральным процессорам (ЦП) и узлам NUMA. Сервер в этих примерах состоит из 256 процессоров, организованных в четыре группы по 16 узлов NUMA в каждой. Потоки не назначаются какому-либо узлу NUMA или ЦП.  
  
-   Группа 0: узлы NUMA от 0 до 3, процессоры от 0 до 63  
-   Группа 1: узлы NUMA от 4 до 7, процессоры от 64 до 127  
-   Группа 2: узлы NUMA от 8 до 12, процессоры от 128 до 191  
-   Группа 3: узлы NUMA от 13 до 16, процессоры от 192 до 255  
  
#### <a name="a-setting-affinity-to-all-cpus-in-groups-0-and-2"></a>A. Задание привязки для всех процессоров в группах 0 и 2  
В следующем примере задается соответствие для всех процессоров в группах 0 и 2.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 63, 128 TO 191;  
```  
  
#### <a name="b-setting-affinity-to-all-cpus-in-numa-nodes-0-and-7"></a>Б. Задание привязки для всех процессоров в узлах NUMA 0 и 7  
В следующем примере задается привязка процессоров только к узлам `0` и `7`.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY NUMANODE=0, 7;  
```  
  
#### <a name="c-setting-affinity-to-cpus-60-through-200"></a>В. Задание привязки к процессорам с номерами от 60 до 200  
В следующем примере задается соответствие для процессоров от 60 до 200.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=60 TO 200;  
```  
  
#### <a name="d-setting-affinity-to-cpu-0-on-a-system-that-has-two-cpus"></a>Г. Задание привязки к процессору 0 в системе с двумя процессорами  
В следующем примере демонстрируется задание соответствия для `CPU=0` на компьютере с двумя процессорами. Перед выполнением следующей инструкции использовалась внутренняя битовая маска соответствия 00.  
  
```sql  
ALTER SERVER CONFIGURATION SET PROCESS AFFINITY CPU=0;  
```  
  
#### <a name="e-setting-affinity-to-auto"></a>Д. Задание привязки AUTO  
В этом примере параметр соответствия устанавливается на `AUTO`.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET PROCESS AFFINITY CPU=AUTO;  
```  
  
###  <a name="setting-diagnostic-log-options"></a><a name="Diagnostic"></a> Setting diagnostic log options  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
В примерах этого раздела показана установка значений параметра журнала диагностики.  
  
#### <a name="a-starting-diagnostic-logging"></a>A. Запуск регистрации диагностических данных в журнале  
В следующем примере запускается запись в журнал диагностических данных.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
#### <a name="b-stopping-diagnostic-logging"></a>Б. Останов регистрации диагностических данных в журнале  
В следующем примере запись в журнал диагностических данных прекращается.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
#### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>В. Задание расположения журналов диагностических данных  
В следующем примере для журналов диагностических данных задается расположение по указанному пути к файлам.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
#### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>Г. Задание максимального размера каждого из журналов диагностики  
В следующем примере задан максимальный размер каждого из журналов диагностики, равный 10 мегабайтам.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
###  <a name="setting-failover-cluster-properties"></a><a name="Failover"></a> Установка свойств отказоустойчивого кластера  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
В следующем примере показана установка свойств ресурса отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="a-specifying-the-value-for-the-healthchecktimeout-property"></a>A. Указание значения свойства HealthCheckTimeout  
В следующем примере устанавливается параметр `HealthCheckTimeout`, равный 15 000 миллисекунд (15 секунд).  
  
```sql  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
###  <a name="b-changing-the-cluster-context-of-an-availability-replica"></a><a name="ChangeClusterContextExample"></a> Б. Изменение контекста кластера для реплики доступности  
В следующем примере выполняется смена контекста экземпляра кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]HADR. Для задания целевого кластера WSFC `clus01` в примере указывается полное имя объекта кластера — `clus01.xyz.com`.  
  
```sql  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
### <a name="setting-buffer-pool-extension-options"></a>Задание параметров расширения буферного пула  
  
####  <a name="a-setting-the-buffer-pool-extension-option"></a><a name="BufferPoolExtension"></a> A. Установка параметра расширения буферного пула  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
В следующем примере выполняется включение параметра расширения буферного пула и задается имя и размер файла.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 50 GB);  
```  
  
#### <a name="b-modifying-buffer-pool-extension-parameters"></a>Б. Изменение параметров расширения буферного пула  
В следующем примере изменяется размер файла расширения буферного пула. Для изменения любых параметров необходимо отключить параметр расширения буферного пула.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION OFF;  
GO  
EXEC sp_configure 'max server memory (MB)', 12000;  
GO  
RECONFIGURE;  
GO  
ALTER SERVER CONFIGURATION  
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 60 GB);  
GO   
```  

### <a name="setting-in-memory-database-options"></a><a name="MemoryOptimized"></a> Настройка параметров выполняющейся в памяти базы данных

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]).

#### <a name="a-enable-all-in-memory-database-features-with-default-options"></a>A. Включение всех функций выполняющейся в памяти базы данных с параметрами по умолчанию

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED ON;
GO
```

#### <a name="b-enable-memory-optimized-tempdb-metadata-using-the-default-resource-pool"></a>Б. Включение оптимизированных для памяти метаданных tempdb с использованием пула ресурсов по умолчанию

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
GO
```

#### <a name="c-enable-memory-optimized-tempdb-metadata-with-a-user-defined-resource-pool"></a>В. Включение оптимизированных для памяти метаданных tempdb с пользовательским пулом ресурсов

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
GO
```

#### <a name="d-enable-hybrid-buffer-pool"></a>Г. Включение гибридного буферного пула

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
GO
```

## <a name="see-also"></a>См. также:  
[Soft-NUMA (SQL Server)](../../database-engine/configure-windows/soft-numa-sql-server.md)   
[Смена контекста кластера HADR экземпляра сервера (SQL Server)](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)   
[sys.dm_os_schedulers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)   
[sys.dm_os_memory_nodes (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md)   
[sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)   
[Расширение буферного пула](../../database-engine/configure-windows/buffer-pool-extension.md)  
  
