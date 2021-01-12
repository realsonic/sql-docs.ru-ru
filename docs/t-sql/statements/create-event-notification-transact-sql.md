---
description: CREATE EVENT NOTIFICATION (Transact-SQL)
title: CREATE EVENT NOTIFICATION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_EVENT_NOTIFICATION_TSQL
- NOTIFICATION_TSQL
- EVENT
- NOTIFICATION
- CREATE EVENT NOTIFICATION
- EVENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EVENT NOTIFICATION statement
- events [SQL Server], notifications
- event notifications [SQL Server], creating
ms.assetid: dbbff0e8-9e25-4f12-a1ba-e12221d16ac2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6f1e3aa71a31dfff7bd7ec83d367efdaa8a01967
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100885"
---
# <a name="create-event-notification-transact-sql"></a>CREATE EVENT NOTIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает объект, отправляющий в службу компонента Service Broker данные о событии сервера или базы данных. Уведомления о событиях создаются только при помощи инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE EVENT NOTIFICATION event_notification_name   
ON { SERVER | DATABASE | QUEUE queue_name }   
[ WITH FAN_IN ]  
FOR { event_type | event_group } [ ,...n ]  
TO SERVICE 'broker_service' , { 'broker_instance_specifier' | 'current database' }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *event_notification_name*  
 Имя уведомления о событии. Имя уведомления о событии должно соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md), и оно должно быть уникальным в той области, где они были созданы: SERVER, DATABASE или *object_name*.  
  
 SERVER  
 Применяет область уведомления о событии к текущему экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если задано это значение, уведомление срабатывает при каждом возникновении события, указанного в предложении FOR, на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 DATABASE  
 Применяет область уведомления о событии к текущей базе данных. Если задано это значение, уведомление срабатывает при каждом возникновении в текущей базе данных события, указанного в предложении FOR.  
  
 QUEUE  
 Применяет область уведомления о событии к определенной очереди в текущей базе данных. Параметр QUEUE можно указать, только если также указан параметр FOR QUEUE_ACTIVATION или FOR BROKER_QUEUE_DISABLED.  
  
 *queue_name*  
 Имя очереди, к которой применяется уведомление о событии. Аргумент *queue_name* можно указать, только если задан параметр QUEUE.  
  
 WITH FAN_IN  
 Служит для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] указанием рассылать обозначенным службам только одно сообщение на событие для всех уведомлений о событиях, которые:  
  
-   созданы в том же событии;  
  
-   созданы тем же участником (который определен таким же идентификатором безопасности);  
  
-   указывают ту же службу и *broker_instance_specifier*;  
  
-   указывают WITH FAN_IN.  
  
 Например, были созданы три уведомления о событии. Все уведомления о событии указывают параметры FOR ALTER_TABLE, WITH FAN_IN, предложение TO SERVICE и созданы тем же идентификатором безопасности. При выполнении инструкции ALTER TABLE сообщения, которые созданы этими тремя уведомлениями о событии, объединяются в одно. Поэтому целевая служба получает только одно сообщение о событии.  
  
 *event_type*  
 Имя типа события, которое вызывает уведомление о событии. Аргумент *event_type* может быть типом события DDL [!INCLUDE[tsql](../../includes/tsql-md.md)], трассировки SQL или типом события [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Список подходящих типов событий DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в разделе [DDL-события](../../relational-databases/triggers/ddl-events.md). QUEUE_ACTIVATION и BROKER_QUEUE_DISABLED являются типами событий компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Дополнительные сведения см. в разделе [Event Notifications](../../relational-databases/service-broker/event-notifications.md).  
  
 *event_group*  
 Имя предопределенной группы типов событий [!INCLUDE[tsql](../../includes/tsql-md.md)] или трассировки SQL. Возникновение любого события из группы может привести к срабатыванию уведомления о событии. Список групп DDL-событий, содержащихся в них событий [!INCLUDE[tsql](../../includes/tsql-md.md)] и область их определения см. в разделе [Группы DDL-событий](../../relational-databases/triggers/ddl-event-groups.md).  
  
 Аргумент *event_group* также выполняет функцию макроса, добавляя входящие в него типы событий к представлению каталога **sys.events** после завершения инструкции CREATE EVENT NOTIFICATION.  
  
 **'** *broker_service* **'**  
 Указывает целевую службу, получающую данные о событиях экземпляра. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] начинает один или несколько диалогов с целевой службой для уведомления о событии. Данная служба должна поддерживать тип сообщений о событиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и контракт, использованный для отправки сообщения.  
  
 Диалог остается открытым до удаления уведомления о событии. Некоторые ошибки могут привести к досрочному завершению диалогов. Явное завершение всех или некоторых диалогов может помешать конечной службе получать новые сообщения.  
  
 { **'** _broker\_instance\_specifier_ **'**  |  **'current database'** }  
 Указывает экземпляр компонента Service Broker, на котором выполняется *broker_service*. Значение для определенного компонента Service Broker может быть получено путем запроса столбца **service_broker_guid** представления каталога **sys.databases**. Чтобы указать экземпляр компонента Service Broker в текущей базе данных, используйте **'current database'**. **'current database'** — это нечувствительный к регистру строковый литерал.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
## <a name="remarks"></a>Remarks  
 Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] включает в себя тип сообщений и контракт исключительно для уведомлений о событии. Таким образом, необязательно создавать службу, которая запускает компонент Service Broker, поскольку она уже существует и указывает следующее имя контракта: `https://schemas.microsoft.com/SQL/Notifications/PostEventNotification`.  
  
 Целевая служба, принимающая уведомления о событиях, должна поддерживать такой уже существующий контракт.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSB](../../includes/sssb-md.md)] на удаленном сервере, необходимо настроить безопасность диалога. Безопасность диалога должна быть настроена вручную согласно модели полной безопасности. Дополнительные сведения см. в разделе [Настройка безопасности диалогов для уведомлений о событиях](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md).  
  
 Если происходит откат транзакции события, активирующей уведомление, то же происходит и с отправкой уведомления о событии. Уведомления о событиях не срабатывают в результате действий, определенных триггерами, когда происходит завершение или откат транзакции внутри триггера. Учитывая то, что события трассировки не связаны транзакциями, уведомления о событиях, основанные на событиях трассировки, отправляются вне зависимости от того, происходит ли откат активировавшей их транзакции.  
  
 Если диалог между сервером и конечной службой прерывается после срабатывания уведомления о событии, то появляется сообщение об ошибке, а уведомление о событии удаляется.  
  
 На транзакцию события, инициировавшую уведомление, не влияет успешность отправки уведомления о событии.  
  
 Все неудачные попытки отправки уведомления о событии заносятся в журнал.  
  
## <a name="permissions"></a>Разрешения  
 Чтобы создать уведомление о событии в области базы данных (ON DATABASE), необходимо разрешение CREATE DATABASE DDL EVENT NOTIFICATION в текущей базе данных.  
  
 Чтобы создать уведомление о событии для инструкции DDL в области сервера (ON SERVER), необходимо разрешение CREATE DDL EVENT NOTIFICATION на сервере.  
  
 Чтобы создать уведомление о событии для события трассировки, необходимо разрешение CREATE TRACE EVENT NOTIFICATION на сервере.  
  
 Чтобы создать уведомление о событии в области очереди, необходимо разрешение ALTER для запроса.  
  
## <a name="examples"></a>Примеры  
  
> [!NOTE]  
>  В примерах A и B, идентификатор GUID, указанный в предложении `TO SERVICE 'NotifyService'` («8140a771-3c4b-4479-8ac0-81008ab17984») специфичен для компьютера, на котором установлен пример. Для этого экземпляра использовался идентификатор GUID для базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
>   
>  Чтобы копировать и запустить эти примеры, необходимо заменить этот идентификатор GUID на идентификатор из вашего компьютера и экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Как объясняется выше в разделе "Аргументы", **'** _broker\_instance\_specifier_ **'** можно получить с помощью запроса к столбцу service_broker_guid представления каталога sys.databases.  
  
### <a name="a-creating-an-event-notification-that-is-server-scoped"></a>A. Создание уведомления о событии уровня сервера  
 В следующем примере создаются объекты, необходимые для установки целевой службы при помощи компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Конечная служба ссылается на тип сообщения и контракт вызывающей стороны исключительно для уведомлений о событиях. Затем уведомление о событии создается в конечной службе, которая начинает рассылку уведомлений в ответ на событие трассировки `Object_Created` на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
--Create a queue to receive messages.  
CREATE QUEUE NotifyQueue ;  
GO  

--Create a service on the queue that references  
--the event notifications contract.  
CREATE SERVICE NotifyService  
ON QUEUE NotifyQueue  
([https://schemas.microsoft.com/SQL/Notifications/PostEventNotification]);  
GO  

--Create a route on the service to define the address   
--to which Service Broker sends messages for the service.  
CREATE ROUTE NotifyRoute  
WITH SERVICE_NAME = 'NotifyService',  
ADDRESS = 'LOCAL';  
GO 

--Create the event notification.  
CREATE EVENT NOTIFICATION log_ddl1   
ON SERVER   
FOR Object_Created   
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984' ;  
```  
  
### <a name="b-creating-an-event-notification-that-is-database-scoped"></a>Б. Создание уведомления о событии уровня базы данных  
 В следующем примере создается уведомление о событии для той же службы, что и в предыдущем примере. Уведомление о событии срабатывает в ответ на событие `ALTER_TABLE` в образце базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE EVENT NOTIFICATION Notify_ALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
```  
  
### <a name="c-getting-information-about-an-event-notification-that-is-server-scoped"></a>В. Получение данных об уведомлении о событии уровня сервера  
 В следующем примере у представления каталога `sys.server_event_notifications` запрашиваются метаданные об уведомлении о событии `log_ddl1`, созданном в области сервера.  
  
```sql  
SELECT * FROM sys.server_event_notifications  
WHERE name = 'log_ddl1';  
```  
  
### <a name="d-getting-information-about-an-event-notification-that-is-database-scoped"></a>Г. Получение сведений об уведомлении о событии уровня базы данных  
 В следующем примере у представления каталога `sys.event_notifications` запрашиваются метаданные об уведомлении о событии `Notify_ALTER_T1`, созданном на уровне базы данных.  
  
```sql  
SELECT * FROM sys.event_notifications  
WHERE name = 'Notify_ALTER_T1';  
```  
  
## <a name="see-also"></a>См. также:  
 [Уведомления о событиях](../../relational-databases/service-broker/event-notifications.md)   
 [DROP EVENT NOTIFICATION (Transact-SQL)](../../t-sql/statements/drop-event-notification-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.event_notifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)   
 [sys.server_event_notifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md)   
 [sys.event (Transact-SQL)](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)   
 [sys.server_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-events-transact-sql.md)  
  
  
