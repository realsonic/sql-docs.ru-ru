---
description: CREATE RESOURCE POOL (Transact-SQL)
title: CREATE RESOURCE POOL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE RESOURCE POOL
- RESOURCE POOL
- CREATE_RESOURCE_POOL_TSQL
- RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE RESOURCE POOL
ms.assetid: 82712505-c6f9-4a65-a469-f029b5a2d6cd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fdb6f04f524dc9c15061c633caca78e394460d48
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092007"
---
# <a name="create-resource-pool-transact-sql"></a>CREATE RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Создает пул ресурсов регулятора ресурсов в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Пул ресурсов представляет подмножество физических ресурсов (память, процессоры и ввод-вывод) экземпляра компонента Database Engine. Регулятор ресурсов позволяет администратору базы данных распределять ресурсы сервера по пулам ресурсов, используя до 64 пулов. Регулятор ресурсов доступен не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
```syntaxsql
CREATE RESOURCE POOL pool_name  
[ WITH  
    (  
        [ MIN_CPU_PERCENT = value ]  
        [ [ , ] MAX_CPU_PERCENT = value ]   
        [ [ , ] CAP_CPU_PERCENT = value ]   
        [ [ , ] AFFINITY {SCHEDULER =  
                  AUTO 
                | ( <scheduler_range_spec> )   
                | NUMANODE = ( <NUMA_node_range_spec> )
                } ]   
        [ [ , ] MIN_MEMORY_PERCENT = value ]  
        [ [ , ] MAX_MEMORY_PERCENT = value ]  
        [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
        [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
    )   
]  
[;]  
  
<scheduler_range_spec> ::=  
{ SCHED_ID | SCHED_ID TO SCHED_ID }[,...n]  
  
<NUMA_node_range_spec> ::=  
{ NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID }[,...n]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*pool_name*  
Определяемое пользователем имя для пула ресурсов. Аргумент *pool_name* является алфавитно-цифровым и может содержать до 128 символов. Данный аргумент должен быть уникальным в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
MIN_CPU_PERCENT =*value*  
Указывает гарантированную среднюю пропускную способность ЦП для всех запросов в пуле ресурсов при возникновении состязания использования ЦП. *value* имеет тип integer и значение по умолчанию 0. Диапазон допустимых значений для *value* — от 0 до 100.  
  
MAX_CPU_PERCENT =*value*  
Указывает максимальную среднюю пропускную способность ЦП для всех запросов в пуле ресурсов при возникновении состязания использования ЦП. *value* имеет тип integer и значение по умолчанию 100. Диапазон допустимых значений для *value* — от 1 до 100.  
  
CAP_CPU_PERCENT = *значение*   
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
Задает жесткое ограничение пропускной способности ЦП, которая предоставляется всем запросам в пуле ресурсов. Ограничивает максимальный уровень пропускной способности ЦП заданным значением. *value* имеет тип integer и значение по умолчанию 100. Диапазон допустимых значений для *value* — от 1 до 100.  
  
AFFINITY {SCHEDULER = AUTO | ( \<scheduler_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}      
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
Подключает пул ресурсов к заданным планировщикам. Значение по умолчанию — AUTO.  
  
AFFINITY SCHEDULER = **(** \<scheduler_range_spec> **)** . Сопоставляет пул ресурсов с расписаниями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], обозначенными заданными идентификаторами. Эти идентификаторы сопоставляются со значениями в столбце scheduler_id представления [sys.dm_os_schedulers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 
  
При использовании AFFINITY NUMANODE = **(** \<NUMA_node_range_spec> **)** пул ресурсов приводится в соответствие с планировщиками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые сопоставляются с физическими ЦП, соответствующими данному узлу NUMA или диапазону узлов. Вы можете использовать следующий запрос [!INCLUDE[tsql](../../includes/tsql-md.md)] для обнаружения сопоставления между конфигурацией физического узла NUMA и идентификаторами планировщиков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc   
    ON osn.node_id = sc.parent_node_id   
    AND sc.scheduler_id < 1048576;  
```  
  
MIN_MEMORY_PERCENT = *значение*    
Указывает минимальный объем памяти, резервируемый для данного пула ресурсов, который не подлежит использованию совместно с другими пулами ресурсов. Аргумент *value* является целым числом, значение по умолчанию — 0. Разрешенный диапазон значений *value* составляет от 0 до 100.  
  
MAX_MEMORY_PERCENT = *значение*    
Указывает общий объем памяти сервера, который может использоваться для запросов в данном пуле ресурсов. *value* имеет тип integer и значение по умолчанию 100. Диапазон допустимых значений для *value* — от 1 до 100.  
  
MIN_IOPS_PER_VOLUME = *значение*    
**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
Указывает минимальный объем операций ввода-вывода в секунду (IOPS) на дисковый том, который следует резервировать для пула ресурсов. Диапазон допустимых значений для *value* — от 0 до 2^31-1 (2 147 483 647). Укажите значение 0, чтобы не указывать минимальный порог для пула. Значение по умолчанию равно 0.  
  
MAX_IOPS_PER_VOLUME = *значение*    
**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
Указывает максимальный объем операций ввода-вывода в секунду (IOPS) на дисковый том, при котором поддерживается пул ресурсов. Диапазон допустимых значений для *value* — от 0 до 2^31-1 (2 147 483 647). Укажите значение 0, чтобы задать неограниченный порог для пула. Значение по умолчанию равно 0.  
  
Если для параметра `MAX_IOPS_PER_VOLUME` пула установлено значение 0, пул не регулируется и может занять все операции ввода-вывода в секунду в системе, даже если для остальных пулов задан параметр MIN_IOPS_PER_VOLUME. На этот случай рекомендуется устанавливать достаточно высокое значение `MAX_IOPS_PER_VOLUME` для этого пула (например, максимальное значение 2^31-1), если требуется, чтобы пул регулировался на ввод-вывод.  
  
## <a name="remarks"></a>Remarks  
Параметры `MIN_IOPS_PER_VOLUME` и `MAX_IOPS_PER_VOLUME` позволяют задать минимальное и максимальное число операций чтения и записи в секунду. Эти операции чтения или записи могут быть любого размера и не показывают минимальную или максимальную пропускную способность.  
  
Значения параметров `MAX_CPU_PERCENT` и `MAX_MEMORY_PERCENT` должны быть больше или равны значениям параметров `MIN_CPU_PERCENT` и `MIN_MEMORY_PERCENT` соответственно.  
  
Параметр `CAP_CPU_PERCENT` отличается от параметра `MAX_CPU_PERCENT` тем, что рабочие нагрузки, связанные с этим пулом, могут использовать ресурсы ЦП в объеме, который превышает значение параметра `MAX_CPU_PERCENT` (если они доступны), но без превышения значения параметра `CAP_CPU_PERCENT`.  
  
Общий процент загрузки ЦП для каждого соответствующего компонента (планировщики или узлы NUMA) не должен превышать 100 %.  
  
## <a name="permissions"></a>Разрешения  
Требуется разрешение `CONTROL SERVER`.  
  
## <a name="examples"></a>Примеры  
### <a name="1-shows-how-to-create-a-resource-pool"></a>1. Демонстрация создания пула ресурсов

В этом примере создается пул ресурсов с именем bigPool. Для этого пула используются параметры по умолчанию регулятора ресурсов.  
  
```sql  
CREATE RESOURCE POOL bigPool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="2-set-the-cap_cpu_percent-to-a-hard-cap-and-set-affinity-scheduler"></a>2. Установка жесткого ограничения для параметра CAP_CPU_PERCENT и установка значений для параметра AFFINITY SCHEDULER

Задайте для параметра CAP_CPU_PERCENT жесткое ограничение 30 %, а для параметра AFFINITY SCHEDULER — диапазон 0–63, 128–191. 
  
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
     MIN_CPU_PERCENT = 10,  
     MAX_CPU_PERCENT = 20,  
     CAP_CPU_PERCENT = 30,  
     AFFINITY SCHEDULER = (0 TO 63, 128 TO 191),  
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
      );  
```  
  
### <a name="3-set-min_iops_per_volume-and-max_iops_per_volume"></a>3. Установка значений параметров Set MIN_IOPS_PER_VOLUME и MAX_IOPS_PER_VOLUME   

Задайте для параметра MIN_IOPS_PER_VOLUME значение 20, а для параметра MAX_IOPS_PER_VOLUME — 100. Эти значения управляют физическими операциями чтения и записи при вводе-выводе, доступными для пула ресурсов.  
  
**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
    MIN_IOPS_PER_VOLUME = 20,  
    MAX_IOPS_PER_VOLUME = 100  
      );  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-resource-pool-transact-sql.md)     
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)     
 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)     
 [ALTER WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md)     
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)     
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)     
 [Пул ресурсов регулятора ресурсов](../../relational-databases/resource-governor/resource-governor-resource-pool.md)     
 [Создание пула ресурсов](../../relational-databases/resource-governor/create-a-resource-pool.md)    
  
