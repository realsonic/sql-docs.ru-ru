---
title: Параметры конфигурации памяти сервера | Документация Майкрософт
ms.custom: contperf-fy20q4
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: Узнайте, как настроить объем памяти, выделяемой диспетчером SQL Server Memory Manager процессам SQL Server. Просматривайте подходы и примеры управления памятью.
helpviewer_keywords:
- Virtual Memory Manager
- max server memory option
- virtual memory [SQL Server]
- VMM
- server memory options [SQL Server]
- servers [SQL Server], memory
- buffer pools [SQL Server]
- min server memory option
- manual memory options [SQL Server]
- memory [SQL Server], servers
ms.assetid: 29ce373e-18f8-46ff-aea6-15bbb10fb9c2
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 71cab9c11aaa4f3dc569761fdfef407601162b1c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878922"
---
# <a name="server-memory-configuration-options"></a>Параметры конфигурации памяти сервера
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Измените объем памяти (в мегабайтах) для процесса SQL Server, используемого экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Существует два параметра использовании серверной памяти: **Мин. память сервера** и **Макс. память сервера**. Эти параметры меняют объем памяти, выделяемой диспетчером SQL Server Memory Manager процессам SQL Server.
  
Параметры по умолчанию и минимальное допустимое значение для этих параметров

|Параметр  |  По умолчанию | Минимально допустимое  |
|---------|---------|---------|
|**min server memory**     |    0     |    0     |
|**max server memory**     |     2 147 483 647 мегабайт (МБ)     |  128 МБ       |

По умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может динамически изменять требования к памяти в зависимости от доступных системных ресурсов. Дополнительные сведения см. в разделе [Управление динамической памятью](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management).

> [!IMPORTANT]  
> Если вы зададите слишком высокое значение **макс. памяти сервера** одному экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], возможно, придется конкурировать с другими экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], размещенными на том же узле, за память. Если же задать слишком низкое значение, может возникнуть значительный дефицит памяти или проблемы с производительностью. Если присвоить параметру **Макс. памяти сервера** минимальное значение, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может не запуститься. Если не удается запустить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] после изменения этого параметра, запустите его с использованием параметра запуска **_-f_** и верните параметр **max server memory** к предыдущему значению. Дополнительные сведения см. в разделе [Параметры запуска службы Database Engine](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может использовать память динамически; но можно установить параметры памяти вручную и ограничить объем памяти, доступный для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Перед настройкой объема памяти для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определите подходящее значение путем вычитания из общего объема физической памяти того объема, который требуется операционной системе, выделениям памяти, не управляемым параметром max_server_memory, и другим экземплярам [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (и для других нужд, если компьютер не выделен полностью под сервер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Разница — максимальный объем памяти, который можно выделить текущему экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 
## <a name="set-options-manually"></a><a name="manually"></a> Задать параметры вручную  
Можно установить для параметров сервера **Мин. памяти сервера** и **Макс. памяти сервера** значения, покрывающие весь доступный объем памяти. Этот метод полезен для системных администраторов или администраторов баз данных, когда требуется настроить экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] так, чтобы его параметры не противоречили требованиям к памяти других приложений или других экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], запущенных на этом узле.

> [!NOTE]
> Параметры **min server memory** и **max server memory** являются расширенными. При использовании системной хранимой процедуры **sp_configure** для изменения этих настроек изменить их можно, только если параметр **show advanced options** установлен в значение 1. Эти параметры вступают в силу сразу же без перезагрузки сервера.  
  
<a name="min_server_memory"></a>Параметр **min_server_memory** используется для гарантированного предоставления минимального объема памяти, доступного диспетчеру памяти [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выделяет немедленно объем памяти, указанный в параметре **min server memory** , после запуска. Тем не менее, когда это значение достигается с ростом рабочей нагрузки, экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может освободить память, выделенную буферному пулу, если не уменьшить значение параметра **min server memory** . Например, если на одном узле может находиться сразу несколько экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], задайте параметр min_server_memory вместо max_server_memory, чтобы зарезервировать память для экземпляра. Кроме того, необходимо задать значение min_server_memory в виртуализированной среде, чтобы гарантировать, что при дефиците памяти на базовом узле не будет попыток выделить больше памяти из буферного пула в гостевой виртуальной машине [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чем это необходимо для приемлемой производительности.

>[!NOTE]
>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не гарантирует, что объем памяти, заданный параметром **min server memory**, будет выделен. Если нагрузка на сервер никогда не требует выделения всего объема памяти, заданного параметром **min server memory**, сервер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет использовать меньше памяти.

<a name="max_server_memory"></a> Параметр **max_server_memory** гарантирует, что в ОС не возникнет дефицит памяти. Чтобы задать конфигурацию "Макс. памяти сервера", отследите общее использование памяти процессом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и определите требования к памяти. Более точные вычисления для одного экземпляра
- Зарезервируйте 1–4 ГБ от общего объема памяти для ОС.
- Затем вычтите эквивалент потенциального выделения памяти ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]), которое не входит в диапазон **max server memory**, который вычисляется так: **размер стека <sup>1</sup> \* вычисляемое максимальное число рабочих потоков <sup>2</sup>** . Остаток и даст значение параметра max_server_memory в случае установки одного экземпляра.

<sup>1</sup> Сведения о размерах стеков потока для различных архитектур см. в разделе [Руководство по архитектуре управления памятью](../../relational-databases/memory-management-architecture-guide.md#stacksizes).

<sup>2</sup> Сведения о вычислении рабочих потоков по умолчанию для заданного числа сходных ЦП на текущем узле см. в разделе [Настройка параметра конфигурации сервера "Максимальное число рабочих потоков"](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).

## <a name="use-ssmanstudiofull"></a>Используйте [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
Используйте два параметра памяти сервера, **Мин. памяти сервера** и **Макс. памяти сервера**, для настройки объема памяти (в мегабайтах), находящейся в управлении диспетчера памяти [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. По умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может динамически изменять требования к памяти в зависимости от доступных системных ресурсов.  
  
### <a name="procedure-for-configuring-a-fixed-amount-of-memory-not-recommended"></a>Настройка фиксированного объема памяти (не рекомендуется)  
Установка фиксированного размера памяти  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши сервер и выберите пункт **Свойства**.  
  
2.  Щелкните узел **Память** .  
  
3.  В пункте **Параметры памяти сервера** введите нужные значения в поля **Минимальный размер памяти сервера** и **Максимальный размер памяти сервера**.  
  
     Оставьте параметры по умолчанию, чтобы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] изменял требования к памяти динамически, исходя из доступности системных ресурсов. Рекомендуется задать для параметра **Макс. памяти сервера** значение, [указанное выше](#max_server_memory). 

На следующем снимке экрана показаны все три шага: 

:::image type="content" source="media/server-memory-server-configuration-options/configure-memory-in-ssms.png" alt-text="Настройка памяти в SSMS":::

  
## <a name="lock-pages-in-memory-lpim"></a>Блокировка страниц в памяти (LPIM) 
Эта политика Windows определяет, какие учетные записи могут использовать процесс для сохранения данных в физической памяти, чтобы система не отправляла страницы данных в виртуальную память на диске. Блокировка страниц в памяти может обеспечивать отклик сервера, когда содержимое памяти заносится в файл подкачки. Для параметра **Блокировка страниц в памяти** указывается значение "Включено" в экземплярах выпуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition и выше, если учетной записи с привилегией на выполнение sqlservr.exe предоставлено право пользователя Windows *Блокировка страниц в памяти* (LPIM).  
  
Чтобы отключить параметр **Блокировка страниц в памяти** для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], удалите право пользователя *Блокировка страниц в памяти* у учетной записи с привилегиями для запуска sqlservr.exe (стартовой учетной записи [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
 
Задание этого параметра не повлияет на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [динамическое управление памятью](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management), что позволит расширить или сузить ее по запросу других клерков памяти. При использовании пользовательского права *Блокировка страниц в памяти* рекомендуется задать верхний предел для параметра **Макс. памяти сервера**, как [указано выше](#max_server_memory).

> [!IMPORTANT]
> Задавать этот параметр следует, только если он необходим, то есть при наличии признаков того, что процесс sqlservr вытесняется из памяти. В этом случае в журнале ошибок появится ошибка 17890, как в следующем примере: `A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`
> Начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [флаг трассировки 845](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) не требуется для использования заблокированных страниц в выпуске Standard Edition. 
  
### <a name="to-enable-lock-pages-in-memory"></a>Включение блокировки страниц в памяти  
Включение параметра "Блокировка страниц в памяти"  
  
1.  В меню **Пуск** выберите команду **Выполнить**. В окне **Открыть** введите **gpedit.msc**.  
  
     Откроется диалоговое окно **Групповая политика** .  
  
2.  В консоли **Групповая политика** разверните узел **Конфигурация компьютера**, затем узел **Конфигурация Windows**.  
  
3.  Разверните узлы **Настройки безопасности** и **Локальные политики**.  
  
4.  Выберите папку **Назначение прав пользователя** .  
  
     Политики будут показаны на панели подробностей.  
  
5.  На этой панели дважды щелкните параметр **Блокировка страниц в памяти**.  
  
6.  В диалоговом окне **Параметр политики локальной защиты** добавьте учетную запись с правами запуска sqlservr.exe (стартовая учетная запись [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
## <a name="multiple-instances-of-ssnoversion"></a>Несколько экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 При выполнении нескольких экземпляров компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]существует три подхода к управлению памятью.  
  
-   Используйте параметр **Макс. памяти сервера**, чтобы управлять использованием памяти, как [указано выше](#max_server_memory). Установите максимальные значения для каждого экземпляра, учитывая, что их сумма не должна превышать общий объем физической памяти, установленной на компьютере. Рекомендуется выделять каждому экземпляру объем памяти, пропорциональный его ожидаемой рабочей нагрузке или размеру базы данных. Данный подход имеет то преимущество, что свободная память доступна новым процессам или экземплярам сразу же после их запуска. Недостаток состоит в том, что, когда выполняются не все экземпляры, ни один из выполняющихся экземпляров не сможет использовать память, оставшуюся свободной.  
  
-   Используйте параметр **Мин. памяти сервера**, чтобы управлять использованием памяти, как [указано выше](#min_server_memory). Установите минимальные значения для каждого экземпляра так, чтобы их сумма была на 1-2 ГБ меньше общего объема физической памяти, установленной на компьютере. Рекомендуется выделять каждому экземпляру минимальный объем памяти, пропорциональный его ожидаемой рабочей нагрузке. Данный подход имеет то преимущество, что выполняющиеся экземпляры могут использовать оставшуюся свободную память в случае, когда выполняются не все экземпляры. Данный подход также полезен, когда на компьютере выполняется другой процесс, интенсивно потребляющий память, так как при этом обеспечивается удовлетворение как минимум заданных потребностей сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в памяти. Недостаток состоит в том, что при запуске нового экземпляра (или любого другого процесса) уже выполняющимся экземплярам требуется некоторое время для освобождения памяти, особенно если для этого им необходимо записать измененные страницы обратно в базу данных.  
  
-   Отсутствие действий (не рекомендуется). Первый экземпляр, столкнувшийся с рабочей нагрузкой, попытается захватить всю память. Простаивающие экземпляры или экземпляры, запущенные позже других, могут в конечном итоге быть вынуждены работать лишь с минимальным доступным объемом памяти. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не пытается равномерно распределять возможности использования памяти между экземплярами. Тем не менее все экземпляры будут реагировать на сигналы уведомлений памяти Windows, корректируя объемы используемой ими памяти. Операционная система Windows не балансирует память между приложениями с помощью уведомлений памяти API-интерфейса. Эти уведомления лишь обеспечивают глобальную обратную связь относительно доступности памяти в системе.  
  
 Эти настройки можно изменять без перезапуска экземпляров, поэтому можно легко экспериментировать с целью нахождения наиболее подходящих настроек для данной модели использования.  
  
## <a name="provide-the-maximum-amount-of-memory"></a>Выделение максимального объема памяти

Для всех выпусков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] память можно выделять вплоть до предела виртуального адресного пространства процесса. Дополнительные сведения см. в разделе [Предельный объем памяти для выпусков Windows и Windows Server](/windows/desktop/Memory/memory-limits-for-windows-releases#physical-memory-limits-windows-server-2016).

## <a name="examples"></a>Примеры

### <a name="example-a-set-the-max-server-memory-option-to-4-gb"></a>Пример A. Задание параметра max server memory равным 4 ГБ.
 В следующем примере параметр `max server memory` устанавливается равным 4 ГБ.  Обратите внимание, что, несмотря на то что `sp_configure` указывает имя параметра как `max server memory (MB)`, в примере демонстрируется пропуск `(MB)`.

```sql
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'max server memory', 4096;
GO
RECONFIGURE;
GO
```
При этом будет выведена инструкция, похожая на следующую:

> Параметр конфигурации "max server memory" (в МБ) изменился с 2147483647 на 4096. Выполните инструкцию RECONFIGURE для установки.

### <a name="example-b-determining-current-memory-allocation"></a>Пример Б. Определение текущего распределения памяти  
 Следующий запрос возвращает информацию о текущем распределении памяти.  

```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
   large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
   locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
   virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
   virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
   virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
   page_fault_count AS sql_page_fault_count,
   memory_utilization_percentage AS sql_memory_utilization_percentage, 
   process_physical_memory_low AS sql_process_physical_memory_low, 
   process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  

### <a name="example-c-determining-value-for-max-server-memory-mb"></a>Пример В. Определение значения параметра "max server memory" (в МБ).
Следующий запрос возвращает сведения о настроенном сейчас значении и значении, которое используется в SQL Server.  Этот запрос возвратит результаты независимо от того, имеет ли параметр "show advanced options" значение true.

```sql
SELECT c.value, c.value_in_use
FROM sys.configurations c WHERE c.[name] = 'max server memory (MB)'
```
  
## <a name="next-steps"></a>Дальнейшие действия
 [Руководство по архитектуре управления памятью](../../relational-databases/memory-management-architecture-guide.md)   
 [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Параметры запуска службы Database Engine](../../database-engine/configure-windows/database-engine-service-startup-options.md)   
 [Выпуски и поддерживаемые функции SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md#Cross-BoxScaleLimits)   
 [Выпуски и поддерживаемые функции SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md#Cross-BoxScaleLimits)   
 [Выпуски и поддерживаемые функции SQL Server 2017 в Linux](../../linux/sql-server-linux-editions-and-components-2017.md#Cross-BoxScaleLimits)   
 [Предельный объем памяти для выпусков Windows и Windows Server](/windows/desktop/Memory/memory-limits-for-windows-releases)
