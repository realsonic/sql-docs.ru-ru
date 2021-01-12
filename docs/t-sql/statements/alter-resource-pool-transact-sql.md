---
description: Инструкция ALTER RESOURCE POOL (Transact-SQL)
title: ALTER RESOURCE POOL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_RESOURCE_POOL_TSQL
- ALTER RESOURCE POOL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE POOL
ms.assetid: 9c1c4cfb-0e3b-4f01-bf57-3fce94c7d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 993ffa1892818d34415c77e0d5acd7969b8696fe
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092097"
---
# <a name="alter-resource-pool-transact-sql"></a>Инструкция ALTER RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет существующую конфигурацию пула ресурсов регулятора ресурсов в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER RESOURCE POOL { pool_name | "default" }  
[WITH  
    ( [ MIN_CPU_PERCENT = value ]  
    [ [ , ] MAX_CPU_PERCENT = value ]   
    [ [ , ] CAP_CPU_PERCENT = value ]   
    [ [ , ] AFFINITY {
                        SCHEDULER = AUTO 
                      | ( <scheduler_range_spec> ) 
                      | NUMANODE = ( <NUMA_node_range_spec> )
                      }]   
    [ [ , ] MIN_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]   
    [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
    [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
)]   
[;]  
  
<scheduler_range_spec> ::=  
{SCHED_ID | SCHED_ID TO SCHED_ID}[,...n]  
  
<NUMA_node_range_spec> ::=  
{NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID}[,...n]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 { *pool_name* |  **"default"** }  
 Имя существующего определяемого пользователем пула ресурсов или пула ресурсов по умолчанию, создаваемого при установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Если слово "default" используется с инструкцией ALTER RESOURCE POOL, оно должно быть заключено в кавычки ("") или квадратные скобки ([]) во избежание конфликта с системным зарезервированным словом DEFAULT. Дополнительные сведения см. в разделе [Идентификаторы баз данных](../../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
>  В стандартных группах рабочей нагрузки и пулах ресурсов используются имена со строчными буквами, такие как «default». Это необходимо учитывать при работе с серверами, где параметры сортировки учитывают регистр символов. Серверы, параметры сортировки которых не учитывают регистр (например, SQL_Latin1_General_CP1_CI_AS), будут рассматривать строки «default» и «Default» как одинаковые.  
  
 MIN_CPU_PERCENT =*value*  
 Указывает гарантированную среднюю пропускную способность ЦП для всех запросов в пуле ресурсов при возникновении состязания использования ЦП. *value* имеет тип integer и значение по умолчанию 0. Диапазон допустимых значений для *value* — от 0 до 100.  
  
 MAX_CPU_PERCENT =*value*  
 Указывает максимальную среднюю пропускную способность ЦП для всех запросов в пуле ресурсов при возникновении состязания за ресурсы ЦП. *value* имеет тип integer и значение по умолчанию 100. Диапазон допустимых значений для *value* — от 1 до 100.  
  
 CAP_CPU_PERCENT = *значение*  
 **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 Указывает целевую максимальную емкость ЦП для запросов в пуле ресурсов. *value* имеет тип integer и значение по умолчанию 100. Диапазон допустимых значений для *value* — от 1 до 100.  
  
> [!NOTE]  
>  Из-за статистической природы системы управления ЦП могут появиться случайные пики, превышающие значение, указанное в CAP_CPU_PERCENT.  
  
 AFFINITY {SCHEDULER = AUTO | (Scheduler_range_spec) | NUMANODE = (NUMA_node_range_spec)}  
 **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 Подключает пул ресурсов к заданным планировщикам. Значение по умолчанию — AUTO.  
  
 AFFINITY SCHEDULER = (Scheduler_range_spec) сопоставляет пул ресурсов с расписаниями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], обозначенными заданными идентификаторами. Эти идентификаторы сопоставляются со значениями в столбце scheduler_id представления [sys.dm_os_schedulers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).  
  
 При использовании AFFINITY NAMANODE = (NUMA_node_range_spec) пул ресурсов приводится в соответствие с планировщиками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые сопоставляются с физическими процессорами, соответствующими данному узлу NUMA или диапазону узлов. Вы можете использовать следующий запрос Transact-SQL для обнаружения сопоставления между конфигурацией физического узла NUMA и идентификаторами планировщиков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc 
   ON osn.node_id = sc.parent_node_id 
      AND sc.scheduler_id < 1048576;  
```  
  
 MIN_MEMORY_PERCENT = *значение*  
 Указывает минимальный объем памяти, резервируемый для данного пула ресурсов, который не подлежит использованию совместно с другими пулами ресурсов. *value* имеет тип integer и значение по умолчанию 0. Диапазон допустимых значений для *value* — от 0 до 100.  
  
 MAX_MEMORY_PERCENT =*value*  
 Указывает общий объем памяти сервера, который может использоваться для запросов в данном пуле ресурсов. *value* имеет тип integer и значение по умолчанию 100. Диапазон допустимых значений для *value* — от 1 до 100.  
  
 MIN_IOPS_PER_VOLUME = *значение*  
 **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
 Указывает минимальный объем операций ввода-вывода в секунду (IOPS) на дисковый том, который следует резервировать для пула ресурсов. Диапазон допустимых значений для *value* — от 0 до 2^31-1 (2 147 483 647). Укажите значение 0, чтобы не указывать минимальный порог для пула.  
  
 MAX_IOPS_PER_VOLUME = *значение*  
 **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
 Указывает максимальный объем операций ввода-вывода в секунду (IOPS) на дисковый том, при котором поддерживается пул ресурсов. Диапазон допустимых значений для *value* — от 0 до 2^31-1 (2 147 483 647). Укажите значение 0, чтобы задать неограниченный порог для пула. Значение по умолчанию равно 0.  
  
 Если значение параметра MAX_IOPS_PER_VOLUME для пула установлено в 0, пул не регулируется вообще и может занять все IOPS в системе, даже если для остальных пулов задан параметр MIN_IOPS_PER_VOLUME. На этот случай рекомендуется устанавливать достаточно высокое значение MAX_IOPS_PER_VOLUME для этого пула (например, максимальное значение 2^31-1), если требуется, чтобы пул регулировался на ввод-вывод.  
  
## <a name="remarks"></a>Remarks  
 Значения MAX_CPU_PERCENT и MAX_MEMORY_PERCENT должны быть больше либо равны значениям MIN_CPU_PERCENT и MIN_MEMORY_PERCENT соответственно.  
  
 MAX_CPU_PERCENT может использовать емкость ЦП, превышающую значение MAX_CPU_PERCENT, если она доступна. Хотя периодически могут возникать пиковые значения, превышающие CAP_CPU_PERCENT, рабочие нагрузки не должны превышать значение CAP_CPU_PERCENT в течение продолжительного периода, даже если емкость ЦП доступна.  
  
 Общий процент ЦП для каждого соответствующего компонента (планировщики или узлы NUMA) не должен превышать 100 %.  
  
 При выполнении инструкций DDL рекомендуется иметь представление о состояниях регулятора ресурсов. Дополнительные сведения см. в разделе [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) (Регулятор ресурсов).  
  
 При изменении параметра, влияющего на план, новый параметр вступит в силу в ранее кэшированных планах только после выполнения инструкции DBCC FREEPROCCACHE (*pool_name*), где *pool_name* — это имя пула ресурсов Resource Governor.  
  
-   Если вы указываете для AFFINITY одного планировщика вместо нескольких, необязательно выполнять инструкцию DBCC FREEPROCCACHE, поскольку параллельные планы могут выполняться в последовательном режиме. Однако это будет не так эффективно, как компилирование последовательного плана.  
  
-   Если вы указываете для AFFINITY нескольких планировщиков вместо одного, необязательно выполнять инструкцию DBCC FREEPROCCACHE. Однако последовательные планы не могут выполняться параллельно, поэтому очистка соответствующего кэша позволит новым планам потенциально компилироваться с помощью параллелизма.  
  
> [!CAUTION]  
>  Удаление кэшированных планов из пула ресурсов, который связан с несколькими группами рабочей нагрузки, повлияет на все группы рабочей нагрузки в определяемом пользователем пуле ресурсов *pool_name*.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение CONTROL SERVER.  
  
## <a name="examples"></a>Примеры  
 В следующем примере сохраняются все параметры по умолчанию для пула ресурсов `default`, за исключением `MAX_CPU_PERCENT`, значение которого изменяется на `25`.  
  
```sql  
ALTER RESOURCE POOL "default"  
WITH  
     ( MAX_CPU_PERCENT = 25);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 В следующем примере `CAP_CPU_PERCENT` задает жесткое ограничение 80 %, а `AFFINITY SCHEDULER` получает отдельное значение 8 и диапазон от 12 до 16.  
  
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
```sql  
ALTER RESOURCE POOL Pool25  
WITH(   
     MIN_CPU_PERCENT = 5,  
     MAX_CPU_PERCENT = 10,       
     CAP_CPU_PERCENT = 80,  
     AFFINITY SCHEDULER = (8, 12 TO 16),   
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
);  
  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md)   
 [CREATE RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
