---
description: CREATE EVENT SESSION (Transact-SQL)
title: CREATE EVENT SESSION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE EVENT SESSION
- SESSION
- EVENT SESSION
- SESSION_TSQL
- EVENT_SESSION_TSQL
- CREATE_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- CREATE EVENT SESSION statement
ms.assetid: 67683027-2b0f-47aa-b223-604731af8b4d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 006ef49a390045d5c6d7d8c8c174da90e37f4f91
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100867"
---
# <a name="create-event-session-transact-sql"></a>CREATE EVENT SESSION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Создает сеанс расширенных событий, который идентифицирует источник событий, цели и параметры сеанса событий.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Синтаксис

```syntaxsql
CREATE EVENT SESSION event_session_name
ON { SERVER | DATABASE }
{  
    <event_definition> [ ,...n]
    [ <event_target_definition> [ ,...n] ]
    [ WITH ( <event_session_options> [ ,...n] ) ]
}
;

<event_definition>::=
{
    ADD EVENT [event_module_guid].event_package_name.event_name
         [ ( {
                 [ SET { event_customizable_attribute = <value> [ ,...n] } ]
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n] } ) ]
                 [ WHERE <predicate_expression> ]
        } ) ]
}

<predicate_expression> ::=
{
    [ NOT ] <predicate_factor> | {( <predicate_expression> ) }
    [ { AND | OR } [ NOT ] { <predicate_factor> | ( <predicate_expression> ) } ]
    [ ,...n ]
}  
  
<predicate_factor>::=
{
    <predicate_leaf> | ( <predicate_expression> )
}

<predicate_leaf>::=
{
      <predicate_source_declaration> { = | < > | ! = | > | > = | < | < = } <value>
    | [event_module_guid].event_package_name.predicate_compare_name ( <predicate_source_declaration>, <value> )
}

<predicate_source_declaration>::=
{
    event_field_name | ( [event_module_guid].event_package_name.predicate_source_name )
}

<value>::=
{
    number | 'string'
}

<event_target_definition>::=
{
    ADD TARGET [event_module_guid].event_package_name.target_name
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]
}

<event_session_options>::=
{  
    [    MAX_MEMORY = size [ KB | MB ] ]
    [ [,] EVENT_RETENTION_MODE = { ALLOW_SINGLE_EVENT_LOSS | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } ]
    [ [,] MAX_DISPATCH_LATENCY = { seconds SECONDS | INFINITE } ]
    [ [,] MAX_EVENT_SIZE = size [ KB | MB ] ]
    [ [,] MEMORY_PARTITION_MODE = { NONE | PER_NODE | PER_CPU } ]
    [ [,] TRACK_CAUSALITY = { ON | OFF } ]
    [ [,] STARTUP_STATE = { ON | OFF } ]
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*event_session_name* — это определяемое пользователем имя для сеанса событий. Аргумент *event_session_name* является алфавитно-цифровым и может содержать до 128 символов. Данный аргумент должен быть уникальным в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).

ADD EVENT [ *event_module_guid* ].*event_package_name*.*event_name* — это событие, связываемое с сеансом событий, где:

- *event_module_guid* — идентификатор GUID для модуля, содержащего событие;
- *event_package_name* — пакет, который содержит объект действия;
- *event_name* — объект события.

События отображаются в представлении sys.dm_xe_objects со значением object_type, равным "event".

SET { *event_customizable_attribute*= \<value> [ ,...*n*] } позволяет установить настраиваемые атрибуты для события. Настраиваемые атрибуты отображаются в представлении sys.dm_xe_object_columns со значением column_type, равным "customizable", и object_name = *event_name*.

ACTION ( { [*event_module_guid*].*event_package_name*.*action_name* [ **,** ...*n*] }) — это действие, связанное с сеансом событий, где:

- *event_module_guid* — идентификатор GUID для модуля, содержащего событие;
- *event_package_name* — пакет, который содержит объект действия;
- *action_name* — объект действия.

Действия отображаются в представлении sys.dm_xe_objects со значением object_type, равным "action".

WHERE \<predicate_expression> Задает выражение предиката, используемое, чтобы определить необходимость обработки события. Если \<predicate_expression> имеет значение true, то обработка события продолжается действиями и целями сеанса. Если \<predicate_expression> имеет значение false, то событие удаляется сеансом прежде, чем оно будет обработано действиями и целями для сеанса. Выражения предиката ограничены 3000 символами, что является пределом для строковых аргументов.

*event_field_name* — имя поля события, которое идентифицирует источник предиката.

[*event_module_guid*].*event_package_name*.*predicate_source_name* — имя глобального источника предиката, где:

- *event_module_guid* — идентификатор GUID для модуля, содержащего событие;
- *event_package_name* — пакет, который содержит объект предиката;
- *predicate_source_name* определен в представлении sys.dm_xe_objects как object_type со значением "pred_source".

[*event_module_guid*].*event_package_name*.*predicate_compare_name* — имя объекта предиката, связываемого с событием, где:

- *event_module_guid* — идентификатор GUID для модуля, содержащего событие;
- *event_package_name* — пакет, который содержит объект предиката;
- *predicate_compare_name* — глобальный источник, определенный в представлении sys.dm_xe_objects со значением object_type, равным "pred_compare".

*number* — любой числовой тип, включая **decimal**. Ограничения: недостаток доступной физической памяти или слишком большое число, которое невозможно представить 64-разрядным целым.

*string* — строка Юникод или ANSI, требуемая для предикатного сравнения. Для функций предикатного сравнения не выполняется неявное преобразование строкового типа. Передача неверного типа приводит к ошибке.

ADD TARGET [*event_module_guid*].*event_package_name*.*target_name* — целевой объект, связываемый с сеансом событий, где:

- *event_module_guid* — идентификатор GUID для модуля, содержащего событие;
- *event_package_name* — пакет, который содержит объект действия;
- *target_name* представляет собой целевой объект. Целевые объекты отображаются в представлении sys.dm_xe_objects со значением object_type, равным «target».

SET { *target_parameter_name*= \<value> [, ...*n*] } задает параметр целевого объекта. Параметры цели отображаются в представлении sys.dm_xe_object_columns со значением column_type, равным "customizable", и object_name = *target_name*.

> [!IMPORTANT]
> Если используется цель "Кольцевой буфер", рекомендуется установить параметр цели max_memory в значение 2048 килобайт (КБ), чтобы избежать возможного усечения выходных XML-данных. Дополнительные сведения об использовании разных типов целевых объектов см. в статье [Цели расширенных событий SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).

WITH ( \<event_session_options> [ ,...*n*] ) Задает параметры, используемые с сеансом событий.

MAX_MEMORY =*size* [ KB | **MB** ] задает максимальный объем памяти, выделенной в сеансе для буферов событий. Значение по умолчанию — 4 МБ. *size* — целое значение, которое может быть представлено значением в килобайтах (КБ) или мегабайтах (МБ). Максимальный объем не может превышать 2 ГБ (2048 МБ). Но не рекомендуется использовать значения размера памяти в гигабайтном диапазоне.

EVENT_RETENTION_MODE = { **ALLOW_SINGLE_EVENT_LOSS** | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } задает режим хранения событий, используемый для обработки потери события.

**ALLOW_SINGLE_EVENT_LOSS**: возможна потеря события в сеансе. Если все буферы событий полны, то удаляется только одно событие. Потеря одного события при заполнении буферов событий обеспечивает приемлемые характеристики производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], одновременно уменьшая до минимума потери данных в обработанном потоке событий.

ALLOW_MULTIPLE_EVENT_LOSS: из сеанса могут быть потеряны полные буферы событий, содержащие несколько событий. Число потерянных событий зависит от размера памяти, выделенной для сеанса, способа секционирования памяти и размера событий в буфере. Этот параметр уменьшает влияние быстрого заполнения буферов событий на производительность сервера, но возможна потеря большого числа событий в сеансе.

NO_EVENT_LOSS: потеря событий не разрешена. Этот параметр обеспечивает сохранение всех произошедших событий. При использовании этого параметра все задачи, которые инициируют события, должны ждать освобождения пространства в буфере событий. Это может привести к заметному снижению производительности во время активного сеанса событий. Соединения пользователя могут простаивать при ожидании событий, данные которых должны быть записаны на диск из буфера.

MAX_DISPATCH_LATENCY = { *seconds* SECONDS | **INFINITE** } задает промежуток времени, в течение которого события будут находиться в буферной памяти перед отправкой в целевые объекты сеанса событий. По умолчанию это значение равно 30 секундам.

*seconds* SECONDS — время ожидания в секундах перед началом выгрузки содержимого буферов в целевые объекты. *seconds* является целым числом. Минимальное значение задержки составляет 1 секунду. Чтобы задать неограниченную задержку (INFINITE), можно использовать значение 0.

**INFINITE**: запись на диск буферов в целевых объектах только при заполнении буферов или закрытии сеанса событий.

> [!NOTE]
> MAX_DISPATCH_LATENCY = 0 SECONDS эквивалентно MAX_DISPATCH_LATENCY = INFINITE.

MAX_EVENT_SIZE =*size* [ KB | **MB** ] задает максимальный допустимый размер для событий. MAX_EVENT_SIZE следует устанавливать только для того, чтобы разрешить одиночные события размером более MAX_MEMORY; задание значения, меньшего MAX_MEMORY, приведет к ошибке. *size* — целое значение, которое может быть представлено значением в килобайтах (КБ) или мегабайтах (МБ). Если значение *size* указано в килобайтах, то минимально допустимое значение — 64 KБ. Если задано значение MAX_EVENT_SIZE, то в дополнение к MAX_MEMORY создаются два буфера размером *size*. Это значит, что общий объем памяти, используемой для буферизации событий, составляет MAX_MEMORY + 2 * MAX_EVENT_SIZE.

MEMORY_PARTITION_MODE = { **NONE** | PER_NODE | PER_CPU } задает расположение, в котором создаются буферы событий.

**NONE**: в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создается один набор буферов.

PER_NODE: набор буферов создается в каждом узле NUMA.

PER_CPU: набор буферов создается для каждого ЦП.

TRACK_CAUSALITY = { ON | **OFF** } указывает, будут ли отслеживаться причинно-следственные связи. Если отслеживание включено, то причинность позволяет коррелировать связанные события в различных серверных соединениях.

STARTUP_STATE = { ON | **OFF** } указывает, необходимо ли запускать этот сеанс событий автоматически при запуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Если `STARTUP_STATE = ON`, то сеанс событий будет запущен, только если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] остановлен, а затем перезапущен.

ON: сеанс событий запускается при начальном запуске.

**OFF**: сеанс событий не запускается при начальном запуске.

## <a name="remarks"></a>Remarks

Приоритеты выполнения логических операторов распределяются следующим образом: `NOT` (наивысший приоритет), `AND`, `OR` (низший приоритет).

## <a name="permissions"></a>Разрешения

В SQL Server требуется разрешение `ALTER ANY EVENT SESSION`.
В Базе данных SQL требуется соответствующее разрешение `ALTER ANY DATABASE EVENT SESSION`.

## <a name="examples"></a>Примеры

### <a name="sql-server-example"></a>Пример для SQL Server

В следующем примере демонстрируется, как создать сеанс событий с именем `test_session`. В этом примере добавляются два события, а также используется цель средства отслеживания событий для Windows.

```sql
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='test_session')
    DROP EVENT session test_session ON SERVER;
GO
CREATE EVENT SESSION test_session
ON SERVER
    ADD EVENT sqlos.async_io_requested,
    ADD EVENT sqlserver.lock_acquired
    ADD TARGET package0.etw_classic_sync_target
        (SET default_etw_session_logfile_path = N'C:\demo\traces\sqletw.etl' )
    WITH (MAX_MEMORY=4MB, MAX_EVENT_SIZE=4MB);
GO
```
### <a name="sql-database-example"></a>Пример для Базы данных SQL

Пример для Базы данных SQL Azure см. в разделе с [примером кода Transact-SQL](/azure/sql-database/sql-database-xevent-code-event-file#transact-sql-code)

### <a name="code-examples-can-differ-for-azure-sql-database"></a>Примеры кода для базы данных SQL Azure могут отличаться

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>См. также:

- [ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)
- [DROP EVENT SESSION (Transact-SQL)](../../t-sql/statements/drop-event-session-transact-sql.md)
- [sys.server_event_sessions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)
- [sys.dm_xe_objects (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)
- [sys.dm_xe_object_columns (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)