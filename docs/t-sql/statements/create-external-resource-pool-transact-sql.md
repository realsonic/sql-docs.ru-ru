---
description: CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
title: CREATE EXTERNAL RESOURCE POOL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL RESOURCE POOL
- CREATE EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE POOL
- EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE
- EXTERNAL_RESOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL RESOURCE POOL statement
ms.assetid: 8cc798ad-c395-461c-b7ff-8c561c098808
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 7f57b0e829c240813d6f1a453d764e64bce6b279
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171606"
---
# <a name="create-external-resource-pool-transact-sql"></a>CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Создает внешний пул для определения ресурсов для внешних процессов. Пул ресурсов представляет подмножество физических ресурсов (память и ЦП) экземпляра ядра СУБД. Resource Governor может распределять ресурсы сервера по пулам ресурсов, используя до 64 пулов.

::: moniker range="=sql-server-2016"
Для [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] в [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] внешний пул управляет `rterm.exe`, `BxlServer.exe` и другими сформированными процессами.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Для [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] внешний пул управляет `rterm.exe`, `python.exe`, `BxlServer.exe` и другими сформированными процессами.
::: moniker-end
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
 

## <a name="syntax"></a>Синтаксис  
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"

```syntaxsql
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_PROCESSES = value ]   
    )   
]  
[ ; ]  
  
<CPU_range_spec> ::=    
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]  
```  
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```syntaxsql
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] AFFINITY CPU =    
            {  
                AUTO   
              | ( <cpu_range_spec> )   
              | NUMANODE = ( <NUMA_node_id> )   
            } ]   
    [ [ , ] MAX_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_PROCESSES = value ]   
    )   
]  
[ ; ]  
  
<CPU_range_spec> ::=    
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]  
```  
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*pool_name*  
Определяемое пользователем имя внешнего пула ресурсов. *pool_name* является буквенно-цифровым и может содержать до 128 символов. Этот аргумент должен быть уникальным внутри экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и должен соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT =*value*  
Максимальная средняя пропускная способность ЦП для всех запросов, которую может получить внешний пул ресурсов при возникновении состязания за ресурсы ЦП. *value* — целое число. Диапазон допустимых значений для *value* — от 1 до 100.


MAX_MEMORY_PERCENT =*value*  
Указывает общий объем памяти сервера, который может использоваться для запросов в данном внешнем пуле ресурсов. *value* — целое число. Диапазон допустимых значений для *value* — от 1 до 100.

MAX_PROCESSES =*value*  
Максимально допустимое количество процессов для внешнего пула ресурсов. Укажите 0, чтобы задать неограниченный порог для пула, который впоследствии ограничивается только ресурсами компьютера.
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT =*value*  
Максимальная средняя пропускная способность ЦП для всех запросов, которую может получить внешний пул ресурсов при возникновении состязания за ресурсы ЦП. *value* — целое число. Диапазон допустимых значений для *value* — от 1 до 100.

AFFINITY {CPU = AUTO | ( <CPU_range_spec>) | NUMANODE = (\<NUMA_node_range_spec>)} подключает внешний пул ресурсов к конкретным ЦП.

AFFINITY CPU = **(** <CPU_range_spec> **)** сопоставляет внешний пул ресурсов с ЦП [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], заданными с помощью CPU_ID.

При использовании AFFINITY NUMANODE = **(\<NUMA_node_range_spec> **)** внешний пул ресурсов сопоставляется с физическими ЦП [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], соответствующими данными узлу NUMA или диапазону узлов. 

MAX_MEMORY_PERCENT =*value*  
Указывает общий объем памяти сервера, который может использоваться для запросов в данном внешнем пуле ресурсов. *value* — целое число. Диапазон допустимых значений для *value* — от 1 до 100.

MAX_PROCESSES =*value*  
Максимально допустимое количество процессов для внешнего пула ресурсов. Укажите 0, чтобы задать неограниченный порог для пула, который впоследствии ограничивается только ресурсами компьютера.
::: moniker-end

## <a name="remarks"></a>Remarks

[!INCLUDE[ssDE](../../includes/ssde-md.md)] реализует пул ресурсов при выполнении инструкции [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md).

Общие сведения о пулах ресурсов см. в разделах [Пул ресурсов Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md), [sys.resource_governor_external_resource_pools (Transact-SQL)](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) и [sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md).

Сведения об управлении внешними пулами ресурсов для машинного обучения см. в статье [Resource governance for machine learning in SQL Server](../../machine-learning/administration/resource-governor.md) (Управление ресурсами для машинного обучения в SQL Server). 

## <a name="permissions"></a>Разрешения

Требуется разрешение `CONTROL SERVER`.

## <a name="examples"></a>Примеры

Для внешнего пула использование ЦП ограничено 75 процентами. Максимальный объем памяти — 30 процентов доступной памяти на компьютере.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>См. также раздел

+ [Параметр конфигурации сервера «external scripts enabled»](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [CREATE RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [Пул ресурсов регулятора ресурсов](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)
+ [sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)
+ [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)
