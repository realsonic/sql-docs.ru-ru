---
title: Архитектура Soft-NUMA (SQL Server) | Документы Майкрософт
description: Узнайте о программной архитектуре NUMA в SQL Server 2014 SP2 и более новых версиях. Узнайте, как использовать автоматический программный NUMA и как вручную настроить SQL Server для использования программной архитектуры NUMA.
ms.custom: ''
ms.date: 02/13/2018
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- NUMA
- soft-NUMA
helpviewer_keywords:
- NUMA
- non-uniform memory access
- soft-NUMA
ms.assetid: 1af22188-e08b-4c80-a27e-4ae6ed9ff969
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df2d323f8978ea5ce9cdaf23c2acf177517ff1ff
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170936"
---
# <a name="soft-numa-sql-server"></a>Архитектура Soft-NUMA (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Современные процессоры имеют несколько ядер на одном сокете. Каждый сокет обычно представлен одним узлом NUMA. Ядро базы данных SQL Server секционирует разные внутренние структуры и потоки служб в узлы NUMA.  Если используются процессоры с 10 и более ядрами на сокет, распределение нагрузки между аппаратными узлами NUMA с помощью программной архитектуры NUMA зачастую позволяет повысить масштабируемость и производительность системы. До [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) для использования программной архитектуры NUMA (Soft-NUMA) нужно было редактировать реестр, чтобы добавить маску сходства для настройки узла. Такая настройка выполнялась на уровне узла, а не для экземпляра. Начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] архитектура Soft-NUMA настраивается автоматически на уровне экземпляра базы данных при запуске службы [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
> [!NOTE]  
> Архитектура Soft-NUMA не поддерживает процессоры с "горячей" заменой.  
  
## <a name="automatic-soft-numa"></a>Автоматическое создание архитектуры Soft-NUMA  
По умолчанию в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] автоматически создает узлы архитектуры Soft-NUMA, если во время запуска обнаруживает более восьми физических ядер на один сокет или узел NUMA. Процессорные ядра с технологией Hyper-Threading не различаются при подсчете физических ядер на узле.  Если обнаружено больше восьми физических ядер на один сокет, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] создает узлы архитектуры Soft-NUMA. В идеале узлы содержат по восемь ядер, но поддерживают и другое количество: от пяти до девяти логических ядер на один узел. Размер аппаратного узла может быть ограничен маской сходства ЦП. Количество узлов NUMA не может превышать максимальное количество поддерживаемых узлов NUMA.  
  
Использование архитектуры Soft-NUMA можно отключать и включать с помощью инструкции [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md) с аргументом `SET SOFTNUMA`. Чтобы изменение этого параметра вступило в силу, потребуется перезапустить ядро базы данных.  
  
На рисунке ниже показан пример сведений об архитектуре Soft-NUMA, которые вы увидите в журнале ошибок SQL Server, если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обнаружит аппаратные узлы NUMA с более чем восемью физическими ядрами на каждый узел или сокет.  


```
2016-11-14 13:39:43.17 Server      SQL Server detected 2 sockets with 12 cores per socket and 24 logical processors per socket, 48 total logical processors; using 48 logical processors based on SQL Server licensing. This is an informational message; no user action is required.     
2016-11-14 13:39:43.35 Server      Automatic soft-NUMA was enabled because SQL Server has detected hardware NUMA nodes with greater than 8 physical cores.     
2016-11-14 13:39:43.63 Server      Node configuration: node 0: CPU mask: 0x0000000000555555:0 Active CPU mask: 0x0000000000555555:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.    
2016-11-14 13:39:43.63 Server      Node configuration: node 1: CPU mask: 0x0000000000aaaaaa:0 Active CPU mask: 0x0000000000aaaaaa:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.    
2016-11-14 13:39:43.63 Server      Node configuration: node 2: CPU mask: 0x0000555555000000:0 Active CPU mask: 0x0000555555000000:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.     
2016-11-14 13:39:43.63 Server      Node configuration: node 3: CPU mask: 0x0000aaaaaa000000:0 Active CPU mask: 0x0000aaaaaa000000:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.   
```   

> [!NOTE]
> Начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 используйте флаг трассировки 8079, чтобы разрешить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использовать автоматическую программную архитектуру NUMA. Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] эта реакция управляется подсистемой, и флаг трассировки 8079 не оказывает влияния. Дополнительные сведения см. в разделе [DBCC TRACEON — флаги трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="manual-soft-numa"></a>Создание архитектуры Soft-NUMA вручную  
Чтобы вручную настроить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для использования архитектуры Soft-NUMA, отключите автоматическую настройку архитектуры Soft-NUMA и добавьте в реестре маску сходства для настройки узла. Маска архитектуры Soft-NUMA в этом случае указывается как запись реестра с двоичным типом данных, типом данных DWORD (шестнадцатеричным или десятичным) или QWORD (шестнадцатеричным или десятичным). Чтобы настроить большее количество процессоров (больше чем первые 32), используйте значения реестра QWORD или BINARY. (Значения QWORD нельзя использовать до [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]). Отредактировав реестр, перезапустите [!INCLUDE[ssDE](../../includes/ssde-md.md)], чтобы конфигурация архитектуры Soft-NUMA вступила в силу.  
  
> [!TIP]
> Нумерация процессоров начинается с 0.  

> [!WARNING]
> [!INCLUDE[ssNoteRegistry](../../includes/ssnoteregistry-md.md)]  
  
Рассмотрим пример компьютера с восемью ЦП, который не имеет оборудования NUMA. Настраиваются три узла программной архитектуры NUMA.   
Экземпляр А компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] настраивается для использования процессоров в количестве от 0 до 3. Второй экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] установлен и настроен для использования процессоров с 4 до 7. Визуально пример может быть представлен следующим образом.  
  
 `CPUs          0  1  2  3  4  5  6  7`  
  
 `Soft-NUMA   <-N0--><-N1-><----N2---->`  
  
 `SQL Server  <instance A ><instance B>`  
  
 Экземпляр А, испытывающий значительную нагрузку ввода-вывода, теперь имеет два потока ввода-вывода и один поток модуля отложенной записи. Экземпляр В, выполняющий операции с интенсивным использованием процессора, имеет только один поток ввода-вывода и один поток модуля отложенной записи. Экземплярам может быть выделено различное количество памяти, но в отличие от оборудования NUMA, они оба получают память из одного блока памяти операционной системы, и здесь нет соответствия памяти и процессора.  
  
 Поток модуля отложенной записи связан с представлением физических узлов памяти NUMA в SQLOS. Поэтому любое число физических узлов NUMA, представленное оборудованием, будет равно числу создаваемых потоков модуля отложенной записи. Дополнительные сведения см. в разделе [Как это работает: программная архитектура NUMA, поток завершения ввода-вывода, рабочие потоки модуля отложенной записи и узлы памяти](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-soft-numa-i-o-completion-thread-lazy-writer-workers/ba-p/316044).  
  
> [!NOTE]
> Разделы реестра **Soft-NUMA** не копируются при обновлении экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="set-the-cpu-affinity-mask"></a>Установка маски схожести ЦП  
 Выполните следующую инструкцию в экземпляре А, чтобы настроить его для использования процессоров 0, 1, 2 и 3 путем задания маски схожести ЦП.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 3;  
```  
  
Выполните следующую инструкцию в экземпляре В, чтобы настроить его для использования процессоров 4, 5, 6 и 7 путем задания маски схожести ЦП.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=4 TO 7;  
```  
  
### <a name="map-soft-numa-nodes-to-cpus"></a>Установка соответствия программной архитектуры NUMA нескольким процессорам  
 С помощью программы "Редактор реестра" (regedit.exe) добавьте приведенные ниже разделы реестра, чтобы установить соответствие между узлом 0 программной архитектуры NUMA и ЦП 0 и 1, узлом 1 программной архитектуры NUMA и ЦП 2 и 3, а также узлом 2 и ЦП 4, 5, 6 и 7.  
  
> [!TIP]
> Чтобы указать процессоры с 60 по 63, используйте значение QWORD F000000000000000 или значение BINARY 1111000000000000000000000000000000000000000000000000000000000000.  
  
 В приведенном ниже примере предположим, что имеется сервер DL580 G9 с 18 ядрами на сокет (в четырех сокетах) и каждый сокет находится в собственной K-группе. Создаваемая конфигурация программной архитектуры NUMA может быть следующей: шесть ядер на узел, три узла на группу, четыре группы.  
  
|Пример для сервера [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с несколькими K-группами|Тип|Имя значения|Данные|  
|-----------------------------------------------------------------------------------------------------------------|----------|----------------|----------------|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node0|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node0|DWORD|Группа|0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node1|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node1|DWORD|Группа|0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node2|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node2|DWORD|Группа|0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node3|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node3|DWORD|Группа|1|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node4|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node4|DWORD|Группа|1|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node5|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node5|DWORD|Группа|1|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node6|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node6|DWORD|Группа|2|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node7|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node7|DWORD|Группа|2|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node8|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node8|DWORD|Группа|2|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node9|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node9|DWORD|Группа|3|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node10|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node10|DWORD|Группа|3|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node11|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node11|DWORD|Группа|3|  
  
## <a name="metadata"></a>Метаданные  
 Для просмотра текущего состояния и конфигурации архитектуры Soft-NUMA можно использовать указанные ниже динамические административные представления.  
  
-   [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md): отображает текущее значение (0 или 1) параметра SOFTNUMA.  
  
-   [sys.dm_os_sys_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md): в столбцах *softnuma_configuration* и *softnuma_configuration_desc* показаны текущие значения конфигурации.  
  
> [!NOTE]
> Можно просмотреть текущее значение для автоматического создания программной архитектуры NUMA с помощью инструкции [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md), но изменить это значение с помощью **sp_configure** невозможно. Необходимо использовать инструкцию [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md) с аргументом `SET SOFTNUMA`.  
  
## <a name="see-also"></a>См. также:  
[Сопоставление портов TCP/IP с узлами NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/map-tcp-ip-ports-to-numa-nodes-sql-server.md)    
[Параметр конфигурации сервера «affinity mask»](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)    
[ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md)     
[sys.dm_os_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-nodes-transact-sql.md)   
  
