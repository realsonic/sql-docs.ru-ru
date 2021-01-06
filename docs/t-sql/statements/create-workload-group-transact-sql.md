---
description: CREATE WORKLOAD GROUP (Transact-SQL)
title: CREATE WORKLOAD GROUP (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/27/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 1ad781bd554d7bcf506371ab71e3441e38f91f8c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643984"
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server и Управляемый экземпляр SQL

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Управляемый экземпляр SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server и Управляемый экземпляр SQL

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Создает группу рабочей нагрузки. Группы рабочей нагрузки являются контейнерами для набора запросов и служат основой для настройки управления рабочими нагрузками в системе. Группы рабочей нагрузки позволяют резервировать ресурсы для изоляции рабочей нагрузки, сохранять ресурсы, определять ресурсы для каждого запроса и соблюдать правила выполнения. После выполнения инструкции вступают в действие параметры.

:::image type="icon" source="../../database-engine/configure-windows/media/topic-link.gif"::: [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

```syntaxsql
CREATE WORKLOAD GROUP group_name
 WITH
 (   MIN_PERCENTAGE_RESOURCE = value 
   , CAP_PERCENTAGE_RESOURCE = value 
   , REQUEST_MIN_RESOURCE_GRANT_PERCENT = value
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH } ]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]

```

*group_name*</br>
Указывает имя, по которому идентифицируется группа рабочей нагрузки. *group_name* имеет тип sysname. Оно может иметь длину до 128 символов и должно быть уникальным в пределах экземпляра.

*MIN_PERCENTAGE_RESOURCE* = value</br>
Указывает гарантированное минимальное выделение ресурсов для этой группы рабочей нагрузки, не используемое совместно с другими группами рабочей нагрузки. *value* является целым числом в диапазоне от 0 до 100. Сумма значений min_percentage_resource во всех группах рабочей нагрузки не может превышать 100. Значение min_percentage_resource не может превышать значение cap_percentage_resource. Для каждого уровня обслуживания предусмотрены минимальные действующие значения. Дополнительные сведения см. в разделе о [действующих значениях](#effective-values).

*CAP_PERCENTAGE_RESOURCE* = value</br>
Указывает максимальное использование ресурсов для всех запросов в группе рабочей нагрузки. Диапазон допустимых целых значений для value — от 1 до 100. Значение cap_percentage_resource не может превышать значение min_percentage_resource. Действующее значение для cap_percentage_resource можно уменьшить, если min_percentage_resource настраивается со значением больше нуля в других группах рабочей нагрузки.

*REQUEST_MIN_RESOURCE_GRANT_PERCENT* = value</br>
Задает минимальный объем ресурсов, выделяемых для каждого запроса. *value* является обязательным параметром с диапазоном десятичных значений от 0,75 до 100,00. Значение request_min_resource_grant_percent должно делиться на 0,25, быть кратно min_percentage_resource и быть меньше cap_percentage_resource. Для каждого уровня обслуживания предусмотрены минимальные действующие значения. Дополнительные сведения см. в разделе о [действующих значениях](#effective-values).

Пример:

```sql
CREATE WORKLOAD GROUP wgSample 
WITH
  ( MIN_PERCENTAGE_RESOURCE = 26                -- integer value
    , REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3.25 -- factor of 26 (guaranteed a minimum of 8 concurrency)
    , CAP_PERCENTAGE_RESOURCE = 100 )
```

Рассмотрим значения, используемые для классов ресурсов в качестве рекомендуемых для request_min_resource_grant_percent.  В приведенной ниже таблице содержатся сведения о выделении ресурсов для 2-го поколения.

|Класс ресурсов|Процент ресурсов|
|---|---|
|Smallrc|3 %|
|Mediumrc|10 %|
|Largerc|22 %|
|Xlargerc|70 %|
|||

*REQUEST_MAX_RESOURCE_GRANT_PERCENT* = value</br>         
Задает максимальный объем ресурсов, выделяемых для каждого запроса. *value* — это необязательный десятичный параметр со значением по умолчанию, равным значению request_min_resource_grant_percent. Значение *value* должно быть больше или равно значению request_min_resource_grant_percent. Если значение request_max_resource_grant_percent больше значения request_min_resource_grant_percent и доступны системные ресурсы, для запроса выделяются дополнительные ресурсы.

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }</br>        
Указывает важность запроса по умолчанию в группе рабочей нагрузки. Важность представлена одним из следующих значений, причем значением по умолчанию является NORMAL.

- LOW
- BELOW_NORMAL
- NORMAL (по умолчанию)
- ABOVE_NORMAL
- HIGH.

Важность, заданная в группе рабочей нагрузки, является важностью по умолчанию для всех запросов в группе рабочей нагрузки. Пользователь также может задать важность на уровне классификатора, который может переопределить параметр важности группы рабочей нагрузки. Это позволяет различать уровень важности запросов в группе рабочей нагрузки для быстрого получения доступа к незарезервированным ресурсам. Если сумма min_percentage_resource в группах рабочей нагрузки меньше 100, значит имеются незарезервированные ресурсы, назначаемые на основе уровня важности.

*QUERY_EXECUTION_TIMEOUT_SEC* = value</br>     
Указывает максимальное время в секундах, в течение которого может выполняться запрос до его отмены. *value* должно быть 0 или положительным целым числом. Значение value по умолчанию равно 0, что означает неограниченное время запроса. QUERY_EXECUTION_TIMEOUT_SEC начинает отсчет, когда запрос переходит в состояние выполнения, а не ставится в очередь.

## <a name="remarks"></a>Remarks

Группы рабочей нагрузки, соответствующие классам ресурсов, создаются автоматически для обеспечения обратной совместимости. Эти группы рабочей нагрузки, определенные системой, нельзя удалить. Можно создать восемь дополнительных групп рабочей нагрузки, определяемых пользователем.

Если группа рабочей нагрузки создается со значением `min_percentage_resource` больше нуля, инструкция `CREATE WORKLOAD GROUP` будет находиться в очереди до тех пор, пока не появится достаточно ресурсов для создания группы рабочей нагрузки.

## <a name="effective-values"></a>Действующие значения

Параметры `min_percentage_resource`, `cap_percentage_resource`, `request_min_resource_grant_percent` и `request_max_resource_grant_percent` имеют действующие значения, которые корректируются в контексте текущего уровня обслуживания и конфигурации других групп рабочей нагрузки.

Параметр `request_min_resource_grant_percent` имеет действующее значение, так как в зависимости от уровня обслуживания каждый запрос требует наличия минимальных ресурсов.  Например, на самом низком уровне обслуживания DW100c требуется минимум 25 % ресурсов на запрос.  Если в группе рабочей нагрузки для `request_min_resource_grant_percent` и `request_max_resource_grant_percent` настроено значение 3 %, действующие значения для обоих параметров корректируются на значение 25 % при запуске экземпляра.  Если экземпляр масштабируется до DW1000c, настроенные и действующие значения обоих параметров будут иметь значение 3 %, так как это минимальное поддерживаемое значение на таком уровне обслуживания.  Если экземпляр масштабируется выше DW1000c, для настроенных и действующих значений обоих параметров сохранится значение 3 %.  Дополнительные сведения о действующих значениях на разных уровнях обслуживания см. в таблице ниже.

|Уровень обслуживания|Наименьшее действительное значение для REQUEST_MIN_RESOURCE_GRANT_PERCENT|Максимальное число одновременных запросов|
|---|---|---|
|DW100c|25 %|4|
|DW200c|12,5 %|8|
|DW300c|8 %|12|
|DW400c|6,25 %|16|
|DW500c|5 %|20|
|DW1000c|3 %|32|
|DW1500c|3 %|32|
|DW2000c|2 %|48|
|DW2500c|2 %|48|
|DW3000c|1,5 %|64|
|DW5000c|1,5 %|64|
|DW6000c|0,75 %|128|
|DW7500c|0,75 %|128|
|DW10000c|0,75 %|128|
|DW15000c|0,75 %|128|
|DW30000c|0,75 %|128|
||||

Значение параметра `min_percentage_resource` должно быть больше действующего значения `request_min_resource_grant_percent` или равно ему. Группа рабочей нагрузки с настроенным значением `min_percentage_resource`, которое меньше действующего значения `min_percentage_resource`, имеет значение, скорректированное во время выполнения до нуля. В этом случае ресурсы, настроенные для `min_percentage_resource`, будут общими для всех групп рабочей нагрузки. Например, группа рабочей нагрузки `wgAdHoc` со значением `min_percentage_resource`, равным 10 %, которая выполняется на уровне обслуживания DW1000c, будет иметь действующее значение `min_percentage_resource`, равное 10 % (3 % — это минимальное поддерживаемое значение на уровне DW1000c). `wgAdhoc` на уровне обслуживания DW100c получит действующее значение min_percentage_resource, равное 0 %. Значение 10 %, настроенное для `wgAdhoc`, будет общим для всех групп рабочей нагрузки.

Параметр `cap_percentage_resource` также имеет действующее значение. Если группа рабочей нагрузки `wgAdhoc` настроена со значением `cap_percentage_resource`, равным 100 %, а другая группа рабочей нагрузки, `wgDashboards`, создается с равным 25 % значением `min_percentage_resource`, действующее значение `cap_percentage_resource` для `wgAdhoc` составит 75 %.

Самый простой способ понять значения времени выполнения для групп рабочей нагрузки — выполнить запрос к системному представлению [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md).

## <a name="permissions"></a>Разрешения

Требуется разрешение `CONTROL DATABASE`

## <a name="see-also"></a>См. также раздел

- [DROP WORKLOAD GROUP (Transact-SQL)](drop-workload-group-transact-sql.md)
- [ALTER WORKLOAD GROUP (Transact-SQL)](alter-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Краткое руководство. Настройка изоляции рабочих нагрузок с помощью T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
