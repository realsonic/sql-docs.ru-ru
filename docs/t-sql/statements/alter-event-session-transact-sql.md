---
description: ALTER EVENT SESSION (Transact-SQL)
title: ALTER EVENT SESSION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER EVENT SESSION
- ALTER_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- extended events [SQL Server], Transact-SQL
- ALTER EVENT SESSION statement
ms.assetid: da006ac9-f914-4995-a2fb-25b5d971cd90
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bd70edba2747e4f68046dcb98e4c886442097c0b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98083017"
---
# <a name="alter-event-session-transact-sql"></a>ALTER EVENT SESSION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Запускает или останавливает сеанс событий или изменяет конфигурацию сеанса.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER EVENT SESSION event_session_name  
ON SERVER  
{  
    [ [ {  <add_drop_event> [ ,...n] }     
       | { <add_drop_event_target> [ ,...n ] } ]   
    [ WITH ( <event_session_options> [ ,...n ] ) ]  
    ]  
    | [ STATE = { START | STOP } ]  
}  
  
<add_drop_event>::=  
{  
    [ ADD EVENT <event_specifier>   
         [ ( {   
                 [ SET { event_customizable_attribute = <value> [ ,...n ] } ]  
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n ] } ) ]  
                 [ WHERE <predicate_expression> ]  
        } ) ]  
   ]   
   | DROP EVENT <event_specifier> }  
  
<event_specifier> ::=  
{  
[event_module_guid].event_package_name.event_name  
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
  
<add_drop_event_target>::=  
{  
    ADD TARGET <event_target_specifier>  
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]  
    | DROP TARGET <event_target_specifier>  
}  
  
<event_target_specifier>::=  
{  
    [event_module_guid].event_package_name.target_name  
}  
  
<event_session_options>::=  
{  
    [    MAX_MEMORY = size [ KB | MB] ]  
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
  
|Термин|Определение|  
|-|-|
|*event_session_name*|Имя существующего сеанса событий.|  
|STATE = START &#124; STOP|Запускает или останавливает сеанс событий. Это аргумент действителен, только если к объекту сеанса событий применяется ALTER EVENT SESSION.|  
|ADD EVENT \<event_specifier>|Связывает событие, определенное аргументом \<event_specifier>, с сеансом событий.|
|[*event_module_guid*] *.event_package_name.event_name*|Имя события в пакете событий, где:<br /><br /> -   *event_module_guid* — идентификатор GUID для модуля, содержащего событие;<br />-   *event_package_name* — пакет, который содержит объект действия;<br />-   *event_name* — объект события.<br /><br /> События отображаются в представлении sys.dm_xe_objects со значением object_type, равным "event".|  
|SET { *event_customizable_attribute*= \<value> [ ,...*n*] }|Указывает настраиваемые атрибуты для события. Настраиваемые атрибуты отображаются в представлении sys.dm_xe_object_columns со значением column_type, равным "customizable", и object_name = *event_name*.|  
|ACTION ( { [*event_module_guid*] *.event_package_name.action_name* [ **,** ...*n*] } )|Действие, связанное с сеансом событий, где:<br /><br /> -   *event_module_guid* — идентификатор GUID для модуля, содержащего событие;<br />-   *event_package_name* — пакет, который содержит объект действия;<br />-   *action_name* — объект действия.<br /><br /> Действия отображаются в представлении sys.dm_xe_objects со значением object_type, равным "action".|  
|WHERE \<predicate_expression>|Задает выражение предиката, используемое, чтобы определить необходимость обработки события. Если \<predicate_expression> имеет значение true, то обработка события продолжается действиями и целями сеанса. Если \<predicate_expression> имеет значение false, то событие удаляется сеансом прежде, чем оно будет обработано действиями и целями для сеанса. Выражения предиката ограничены 3000 символами, что является пределом для строковых аргументов.|
|*event_field_name*|Имя поля события, которое идентифицирует источник предиката.|  
|[event_module_guid].event_package_name.predicate_source_name|Имя глобального источника предиката, где:<br /><br /> -   *event_module_guid* — идентификатор GUID для модуля, содержащего событие;<br />-   *event_package_name* — пакет, который содержит объект предиката;<br />-   *predicate_source_name* определен в представлении sys.dm_xe_objects как object_type со значением "pred_source".|  
|[*event_module_guid*].*event_package_name*.*predicate_compare_name*|Имя объекта предиката, связываемого с событием, где:<br /><br /> -   *event_module_guid* — идентификатор GUID для модуля, содержащего событие;<br />-   *event_package_name* — пакет, который содержит объект предиката;<br />-   *predicate_compare_name* — глобальный источник, определенный в представлении sys.dm_xe_objects со значением object_type, равным "pred_compare".|  
|DROP EVENT \<event_specifier>|Удаляет событие, обозначенное *\<event_specifier>* . \<event_specifier> должен быть допустимым в рамках сеанса события.|  
|ADD TARGET \<event_target_specifier>|Связывает целевой объект, определенный аргументом \<event_target_specifier>, с сеансом событий.|
|[*event_module_guid*].*event_package_name*.*target_name*|Имя целевого объекта в сеансе событий, где:<br /><br /> -   *event_module_guid* — идентификатор GUID для модуля, содержащего событие;<br />-   *event_package_name* — пакет, который содержит объект действия;<br />-   *target_name* представляет собой действие. Действия отображаются в представлении sys.dm_xe_objects как object_type со значением "target".|  
|SET { *target_parameter_name*= \<value> [, ...*n*] }|Задает параметр целевого объекта. Параметры цели отображаются в представлении sys.dm_xe_object_columns со значением column_type, равным "customizable", и object_name = *target_name*.<br /><br /> **ПРИМЕЧАНИЕ.** Если используется цель "Кольцевой буфер", рекомендуется установить параметр цели max_memory в значение 2048 килобайт (КБ), чтобы избежать возможного усечения выходных XML-данных. Дополнительные сведения об использовании разных типов целевых объектов см. в статье [Цели расширенных событий SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).|  
|DROP TARGET \<event_target_specifier>|Удаляет целевой объект, обозначенный \<event_target_specifier>. \<event_target_specifier> должен быть допустимым в рамках сеанса события.|  
|EVENT_RETENTION_MODE = { **ALLOW_SINGLE_EVENT_LOSS** &#124; ALLOW_MULTIPLE_EVENT_LOSS &#124; NO_EVENT_LOSS }|Задает режим хранения событий, используемый для обработки потери события.<br /><br /> **ALLOW_SINGLE_EVENT_LOSS**<br /> Возможна потеря события в сеансе. Если все буферы событий полны, то удаляется только одно событие. Потеря одного события при заполнении буферов событий обеспечивает приемлемые характеристики производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], одновременно уменьшая до минимума потери данных в обработанном потоке событий.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS<br /> Из сеанса могут быть потеряны полные буферы событий, содержащие несколько событий. Число потерянных событий зависит от размера памяти, выделенной для сеанса, способа секционирования памяти и размера событий в буфере. Этот параметр уменьшает влияние быстрого заполнения буферов событий на производительность сервера, но возможна потеря большого числа событий в сеансе.<br /><br /> NO_EVENT_LOSS<br /> Потеря событий не разрешена. Этот параметр обеспечивает сохранение всех произошедших событий. При использовании этого параметра все задачи, которые инициируют события, должны ждать освобождения пространства в буфере событий. Это может привести к заметному снижению производительности во время активного сеанса событий. Соединения пользователя могут простаивать при ожидании событий, данные которых должны быть записаны на диск из буфера.|  
|MAX_DISPATCH_LATENCY = { *seconds* SECONDS &#124; **INFINITE** }|Задает промежуток времени, в течение которого события находятся в буферной памяти перед отправкой в цели сеанса событий. Минимальное значение задержки составляет 1 секунду. Чтобы задать неограниченную задержку (INFINITE), можно использовать значение 0. По умолчанию это значение равно 30 секундам.<br /><br /> *seconds* SECONDS<br /> Время ожидания (в секундах) перед началом выгрузки содержимого буферов в цели. *seconds* является целым числом.<br /><br /> **INFINITE**<br /> Запись на диск буферов в цели только при заполнении буферов или закрытии сеанса событий.<br /><br /> **ПРИМЕЧАНИЕ.** MAX_DISPATCH_LATENCY = 0 SECONDS эквивалентно MAX_DISPATCH_LATENCY = INFINITE.|  
|MAX_EVENT_SIZE =*size* [ KB &#124; **MB** ]|Задает максимальный допустимый размер для событий. MAX_EVENT_SIZE следует устанавливать только для того, чтобы разрешить одиночные события размером более MAX_MEMORY; задание значения, меньшего MAX_MEMORY, приведет к ошибке. *size* — целое значение, которое может быть представлено значением в килобайтах (КБ) или мегабайтах (МБ). Если значение *size* указано в килобайтах, то минимально допустимое значение — 64 KБ. Если задано значение MAX_EVENT_SIZE, то в дополнение к MAX_MEMORY создаются два буфера размером *size*. Это значит, что общий объем памяти, используемой для буферизации событий, составляет MAX_MEMORY + 2 * MAX_EVENT_SIZE.|  
|MEMORY_PARTITION_MODE = { **NONE** &#124; PER_NODE &#124; PER_CPU }|Задает место, в котором создаются буферы событий.<br /><br /> **NONE**<br /> В экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создается один набор буферов.<br /><br /> PER NODE — набор буферов создается в каждом узле NUMA.<br /><br /> PER CPU — набор буферов создается для каждого ЦП.|  
|TRACK_CAUSALITY = { ON &#124; **OFF** }|Указывает, будут ли отслеживаться причинно-следственные связи. Если отслеживание включено, то причинность позволяет коррелировать связанные события в различных серверных соединениях.|  
|STARTUP_STATE = { ON &#124; **OFF** }|Указывает, необходимо ли запустить данный сеанс событий автоматически при запуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если STARTUP_STATE = ON, то сеанс событий будет запущен, только если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] остановлен, а затем перезапущен.<br /><br /> ON — сеанс событий запускается при начальном запуске.<br /><br /> **OFF** — сеанс событий не запускается при начальном запуске.|  
  
## <a name="remarks"></a>Remarks  
 Аргументы `ADD` и `DROP` нельзя использовать в одной инструкции.  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение `ALTER ANY EVENT SESSION`.  
  
## <a name="examples"></a>Примеры  
 Следующий пример запускает сеанс событий, получает динамическую статистику сеанса, а затем добавляет два события в существующий сеанс.  
  
```sql  
-- Start the event session  
ALTER EVENT SESSION test_session ON SERVER  
STATE = start;  
GO  

-- Obtain live session statistics   
SELECT * FROM sys.dm_xe_sessions;  
SELECT * FROM sys.dm_xe_session_events;  
GO  
  
-- Add new events to the session  
ALTER EVENT SESSION test_session ON SERVER  
ADD EVENT sqlserver.database_transaction_begin,  
ADD EVENT sqlserver.database_transaction_end;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)   
 [DROP EVENT SESSION (Transact-SQL)](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [Цели расширенных событий SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))   
 [sys.server_event_sessions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)   
 [sys.dm_xe_objects (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)   
 [sys.dm_xe_object_columns (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)  
  
