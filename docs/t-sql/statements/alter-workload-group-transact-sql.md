---
description: ALTER WORKLOAD GROUP (Transact-SQL)
title: ALTER WORKLOAD GROUP (Transact-SQL)
ms.custom: ''
ms.date: 05/05/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_WORKLOAD_GROUP_TSQL
- ALTER WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER WORKLOAD GROUP statement
ms.assetid: 957addce-feb0-4e54-893e-5faca3cd184c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 2c9c90cb1107d33f6ecbb42e0974371555d9b9aa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098424"
---
# <a name="alter-workload-group-transact-sql"></a>ALTER WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server и Управляемый экземпляр SQL

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Управляемый экземпляр SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server и Управляемый экземпляр SQL

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Изменяет существующую группу рабочей нагрузки.

См. ниже раздел "Поведение `ALTER WORKLOAD GROUP`", чтобы узнать, как `ALTER WORKLOAD GROUP` работает в системе с выполняющимися и поставленными в очередь запросами. 

Те же ограничения, что действуют для [CREATE WORKLOAD GROUP](create-workload-group-transact-sql.md), применяются и к `ALTER WORKLOAD GROUP`.  Перед изменением параметров выполните запрос к [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md), чтобы убедиться, что значения находятся в допустимых диапазонах.

## <a name="syntax"></a>Синтаксис

```syntaxsql
ALTER WORKLOAD GROUP group_name
 WITH
 ([       MIN_PERCENTAGE_RESOURCE = value ]
  [ [ , ] CAP_PERCENTAGE_RESOURCE = value ]
  [ [ , ] REQUEST_MIN_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ] 
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]
  ```

## <a name="arguments"></a>Аргументы

group_name  
Это имя существующей пользовательской группы рабочей нагрузки, которая будет изменена.  Изменять значение group_name недопустимо. 

MIN_PERCENTAGE_RESOURCE = value  
Значение value должно представлять собой целое число в диапазоне от 0 до 100.  При изменении min_percentage_resource сумма значений min_percentage_resource во всех группах рабочей нагрузки не может превышать 100.  Для изменения min_percentage_resource требуется завершить выполнение всех текущих запросов в группе рабочей нагрузки перед выполнением команды.  Дополнительные сведения см. в разделе "Поведение ALTER WORKLOAD GROUP" этого документа.

CAP_PERCENTAGE_RESOURCE = value  
Значение value должно представлять собой целое число в диапазоне от 1 до 100.  Значение cap_percentage_resource не может превышать значение min_percentage_resource.  Для изменения cap_percentage_resource требуется завершить выполнение всех текущих запросов в группе рабочей нагрузки перед выполнением команды.  Дополнительные сведения см. в разделе "Поведение ALTER WORKLOAD GROUP" этого документа. 

REQUEST_MIN_RESOURCE_GRANT_PERCENT = value  
Значение value должно представлять собой десятичное число в диапазоне от 0,75 до 100,00.  Значение request_min_resource_grant_percent должно быть кратно min_percentage_resource и меньше, чем cap_percentage_resource. 
  
REQUEST_MAX_RESOURCE_GRANT_PERCENT = value  
Значение value должно представлять собой десятичное число, превышающее значение request_min_resource_grant_percent.

IMPORTANCE = { LOW \|  BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
Изменяет уровень важности запроса по умолчанию в группе рабочей нагрузки.

QUERY_EXECUTION_TIMEOUT_SEC = value  
Изменяет максимальное время в секундах, в течение которого может выполняться запрос, прежде чем он будет отменен. Значение value должно быть положительным целым числом или нулем. Значение value по умолчанию равно 0, что означает неограниченное время.   

## <a name="permissions"></a>Разрешения

Требуется разрешение CONTROL DATABASE

## <a name="example"></a>Пример

В приведенном ниже примере выполняется проверка значений в представлении каталога для wgDataLoads и изменение этих значений.

```sql
SELECT *
FROM sys.workload_management_workload_groups  
WHERE [name] = 'wgDataLoads'

ALTER WORKLOAD GROUP wgDataLoads WITH
( MIN_PERCENTAGE_RESOURCE            = 40
 ,CAP_PERCENTAGE_RESOURCE            = 80
 ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 10 )
 ```

## <a name="alter-workload-group-behavior"></a>Поведение ALTER WORKLOAD GROUP

В любой момент времени в системе имеется три типа запросов:
- Запросы, которые еще не классифицированы.
- Классифицированные запросы, которые ожидают блокировку объектов или системные ресурсы.
- Классифицированные запросы, которые выполняются.

Время, когда параметры вступят в силу, будет отличаться в зависимости от свойств изменяемой группы рабочей нагрузки.

**Importance или query_execution_timeout**. Для importance и query_execution_timeout неклассифицированные запросы получают новые значения конфигурации.  Ожидающие и текущие запросы будут выполняться с прежней конфигурацией.  Запрос `ALTER WORKLOAD GROUP` выполняется сразу вне зависимости от текущих запросов в группе рабочей нагрузки.

**Request_min_resource_grant_percent или request_max_resource_grant_percent**. Для request_min_resource_grant_percent и request_max_resource_grant_percent выполняющиеся запросы используют прежнюю конфигурацию.  Ожидающие и неклассифицированные запросы получают новые значения конфигурации.  Запрос `ALTER WORKLOAD GROUP` выполняется сразу вне зависимости от текущих запросов в группе рабочей нагрузки.

**Min_percentage_resource или cap_percentage_resource**. Для min_percentage_resource и cap_percentage_resource выполняющиеся запросы используют прежнюю конфигурацию.  Ожидающие и неклассифицированные запросы получают новые значения конфигурации. 

Для изменения min_percentage_resource и cap_percentage_resource требуется очистка выполняющихся запросов в изменяемой группе рабочей нагрузки.  При уменьшении min_percentage_resource освобожденные ресурсы возвращаются в общий пул, что позволяет запросам из других групп рабочей нагрузки использовать эти ресурсы.  И наоборот, при увеличении min_percentage_resource потребуется ожидание, пока выполнятся запросы, использующие только необходимые ресурсы из общего пула.  Операция изменения группы рабочей нагрузки будет иметь приоритетный доступ к общим ресурсам по отношению к другим запросам, ожидающим выполнения в общем пуле.  Если сумма min_percentage_resource превышает 100 %, запрос ALTER WORKLOAD GROUP немедленно завершится сбоем. 

**Действие блокировки**. Для изменения группы рабочей нагрузки необходима глобальная блокировка всех таких групп.  Запрос на изменение группы рабочей нагрузки помещается в очередь вслед за уже отправленными запросами на создание или удаление таких групп.  Если сразу отправляется целый пакет инструкций ALTER, то эти инструкции обрабатываются в порядке их отправки.  

## <a name="see-also"></a>См. также раздел

- [CREATE WORKLOAD GROUP (Transact-SQL)](create-workload-group-transact-sql.md)
- [DROP WORKLOAD GROUP (Transact-SQL)](drop-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Краткое руководство. Настройка изоляции рабочих нагрузок с помощью T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
