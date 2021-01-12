---
description: RECEIVE (Transact-SQL)
title: RECEIVE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RECEIVE_TSQL
- RECEIVE
dev_langs:
- TSQL
helpviewer_keywords:
- queues [Service Broker], message retrieval
- messages [Service Broker], retrieving
- RECEIVE statement
- receiving messages
- retrieving messages
ms.assetid: 878c6c14-37ab-4b87-9854-7f8f42bac7dd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 840f023e1a906350f34d0709459842a3fe3c796a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081071"
---
# <a name="receive-transact-sql"></a>RECEIVE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Извлекает из очереди одно или несколько сообщений. В зависимости от настройки хранения для очереди удаляет сообщение из очереди или обновляет состояние сообщения в очереди.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
[ WAITFOR ( ]  
    RECEIVE [ TOP ( n ) ]   
        <column_specifier> [ ,...n ]  
        FROM <queue>  
        [ INTO table_variable ]  
        [ WHERE {  conversation_handle = conversation_handle  
                 | conversation_group_id = conversation_group_id } ]  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<column_specifier> ::=  
{    *   
  |  { column_name | [ ] expression } [ [ AS ] column_alias ]  
  |  column_alias = expression   
}     [ ,...n ]   
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 WAITFOR  
 Указывает, что инструкция RECEIVE ожидает прибытия сообщения в очередь, если в данный момент сообщений нет.  
  
 TOP( *n* )  
 Указывает максимальное количество сообщений, которые должны быть возвращены. Если это предложение не указано, возвращаются все сообщения, удовлетворяющие критерию инструкции.  
  
 \*  
 Указывает, что результирующий набор содержит все столбцы в очереди.  
  
 *column_name*  
 Имя столбца, который должен быть включен в результирующий набор.  
  
 *expression*  
 Имя столбца, константа, функция или любое сочетание имен столбцов, констант и функций, соединенных оператором.  
  
 *column_alias*  
 Альтернативное имя, заменяющее имя столбца в результирующем наборе.  
  
 FROM  
 Указывает очередь, содержащую получаемые сообщения.  
  
 *database_name*  
 Имя базы данных, содержащей очередь, из которой нужно получать сообщения. Если аргумент *database_name* не указан, по умолчанию используется текущая база данных.  
  
 *schema_name*  
 Имя схемы, владеющей очередью, из которой нужно получать сообщения. Если аргумент *schema_name* не указан, по умолчанию используется схема по умолчанию текущего пользователя.  
  
 *queue_name*  
 Имя очереди, из которой нужно получать сообщения.  
  
 INTO *table_variable*  
 Указывает табличную переменную, в которую инструкция RECEIVE записывает сообщения. У табличной переменной должно быть столько же столбцов, сколько и в сообщениях. Тип данных каждого столбца в табличной переменной должен поддерживать неявное преобразование к типу данных соответствующего столбца в сообщениях. Если ключевое слово INTO не указано, сообщения возвращаются в виде результирующего набора.  
  
 WHERE  
 Указывает диалог или группу сообщений для приема сообщений. Если этот аргумент опущен, возвращаются сообщения из следующей доступной группы сообщений.  
  
 conversation_handle = *conversation_handle*  
 Указывает диалог для принятых сообщений. Указанный *дескриптор диалога* должен иметь тип **uniqueidentifer** либо тип, который можно преобразовать в **uniqueidentifier**.  
  
 conversation_group_id = *conversation_group_id*  
 Указывает группу сообщений для принятых сообщений. Указанный *идентификатор группы диалога* должен иметь тип **uniqueidentifier** либо тип, который можно преобразовать в **uniqueidentifier**.  
  
 TIMEOUT *timeout*  
 Указывает количество времени, в миллисекундах, в течение которого инструкция должна ожидать сообщение. Это предложение может быть использовано только вместе с предложением WAITFOR. Если данное предложение не указано или время ожидания равно –**1**, время ожидания будет неограниченно. По истечении времени ожидания инструкция RECEIVE возвращает пустой результирующий набор.  
  
## <a name="remarks"></a>Комментарии  
  
> [!IMPORTANT]  
>  Если инструкция RECEIVE не является первой в пакете или хранимой процедуре, то предшествующая инструкция должна заканчиваться точкой с запятой (;).  
  
 Инструкция RECEIVE считывает сообщения из очереди и возвращает результирующий набор. Результирующий набор может быть пустым или содержать несколько строк, каждая из которых содержит одно сообщение. Если предложение INTO не используется, а аргумент *column_specifier* не присваивает значения локальным переменным, инструкция возвращает результирующий набор вызывающей программе.  
  
 Сообщения, возвращаемые инструкцией RECEIVE, могут быть различных типов. В приложениях можно использовать столбец **message_type_name** для перенаправления каждого сообщения коду, обрабатывающему соответствующий тип сообщения. Существует два класса типов сообщений.  
  
-   Типы сообщений, определяемые приложениями, которые создаются с помощью инструкции CREATE MESSAGE TYPE. Набор определяемых приложениями типов сообщений, разрешенных в диалоге, определен контрактом компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)], указанным для диалога.  
  
-   Системные сообщения компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)], возвращающие состояние или сведения об ошибке.  
  
 Инструкция RECEIVE удаляет полученные сообщения из очереди, если только очередь не указывает сохранение сообщений. Если параметр RETENTION для очереди установлен в ON, инструкция RECEIVE устанавливает для столбца **status** значение **0** и оставляет сообщение в очереди. При откате транзакции, содержащей инструкцию RECEIVE, производится также откат всех изменений в очереди в пределах транзакции; сообщения при этом возвращаются в очередь.  
  
 Все сообщения, возвращаемые инструкцией RECEIVE, принадлежат к одной и той же группе сообщений. Инструкция RECEIVE блокирует группу сообщений для возвращенных сообщений до тех пор, пока не завершится транзакция, содержащая инструкцию. Инструкция RECEIVE возвращает сообщения со значением **status**, равным **1**. Результирующий набор, возвращенный инструкцией RECEIVE, неявно упорядочен.  
  
-   Если сообщения от нескольких диалогов отвечают условиям предложения WHERE, инструкция RECEIVE возвращает все сообщения от одного диалога раньше сообщений для любого другого диалога. Диалоги обрабатываются в порядке убывания уровня приоритета.  
  
-   Внутри одного диалога инструкция RECEIVE возвращает сообщения в порядке возрастания **message_sequence_number**.  
  
 Предложение WHERE инструкции RECEIVE может содержать только одно условие поиска, использующее аргумент **conversation_handle** или аргумент **conversation_group_id**. Условие поиска не может содержать какие-либо другие столбцы в очереди. Аргумент **conversation_handle** или **conversation_group_id** не может быть выражением. Набор возвращаемых сообщений зависит от условий, указанных в предложении WHERE.  
  
-   Если указан аргумент **conversation_handle**, инструкция RECEIVE возвращает все сообщения из указанных диалогов, доступные в очереди.  
  
-   Если указан аргумент **conversation_group_id**, инструкция RECEIVE возвращает все сообщения, доступные в очереди, от любых диалогов, являющихся членами указанной группы диалогов.  
  
-   При отсутствии предложения WHERE инструкция RECEIVE определяет, какая группа сообщений:  
  
    -   имеет одно или более сообщений в очереди;  
  
    -   не была заблокирована другой инструкцией RECEIVE;  
  
    -   имеет самый высокий приоритет среди всех групп сообщений, отвечающих этим критериям.  
  
     После этого инструкция RECEIVE возвращает все доступные в очереди сообщения от любого диалога, являющегося членом выбранной группы сообщений.  
  
 Если дескриптор диалога или идентификатор группы сообщений, указываемые в предложении WHERE, не существуют или не связаны с указанной очередью, инструкция RECEIVE возвращает ошибку.  
  
 Если состояние очереди, задаваемой в инструкции RECEIVE, установлено в OFF, инструкция регистрирует это как ошибку языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Если указано предложение WAITFOR, инструкция ожидает в течение заданного времени ожидания или до тех пор, пока не будет предоставлен результирующий набор. Если в момент, когда инструкция находится в состоянии ожидания, очередь удаляется или состояние очереди устанавливается в OFF, инструкция немедленно возвращает ошибку. Если инструкция RECEIVE указывает группу сообщений или дескриптор диалога, а служба для этого диалога удалена или перемещена в другую очередь, инструкция RECEIVE сообщает об ошибке языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Недопустимо использовать инструкцию RECEIVE в пользовательской функции.  
  
 Инструкция RECEIVE не может ограничить приоритет. Если одна инструкция RECEIVE блокирует группу сообщений и возвращает множество сообщений из диалогов с низким приоритетом, в той группе становится невозможно получить сообщения от диалогов с высоким приоритетом. Для предотвращения такой ситуации при получении сообщений от диалогов с низким приоритетом необходимо использовать предложение TOP, чтобы ограничить количество сообщений, получаемое каждой инструкцией RECEIVE.  
  
## <a name="queue-columns"></a>Столбцы очереди  
 В следующей таблице указаны столбцы в очереди.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**status**|**tinyint**|Состояние сообщения. Состояние сообщений, возвращаемых командой RECEIVE, всегда **0**. Сообщения в очереди могут иметь одно из следующих значений:<br /><br /> **0**= готово **1**= получено сообщение **2**= еще не завершено **3**= отправленное сообщение сохранено.|  
|**priority**|**tinyint**|Уровень приоритета диалога, распространяющийся на сообщение.|  
|**queuing_order**|**bigint**|Порядковый номер сообщения в очереди.|  
|**conversation_group_id**|**uniqueidentifier**|Идентификатор группы сообщений, которой принадлежит данное сообщение.|  
|**conversation_handle**|**uniqueidentifier**|Дескриптор диалога, частью которого является данное сообщение.|  
|**message_sequence_number**|**bigint**|Порядковый номер сообщения в диалоге.|  
|**service_name**|**nvarchar(512)**|Имя службы, к которой относится диалог.|  
|**service_id**|**int**|Идентификатор объекта службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], к которой относится диалог.|  
|**service_contract_name**|**nvarchar(256)**|Имя контракта, которому следует диалог.|  
|**service_contract_id**|**int**|Идентификатор объекта контракта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которому следует диалог.|  
|**message_type_name**|**nvarchar(256)**|Имя типа сообщения, который описывает формат сообщения. Сообщения могут иметь тип, определенный приложением, или быть системными сообщениями компонента Service Broker.|  
|**message_type_id**|**int**|Идентификатор объекта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для типа сообщения, описывающий сообщение.|  
|**validation**|**nchar(2)**|Проверка, используемая для сообщения:<br /><br /> **E**=Пусто **N**=Нет **X**=XML|  
|**message_body**|**varbinary(MAX)**|Содержимое сообщения.|  
  
## <a name="permissions"></a>Разрешения  
 Для получения сообщения пользователь должен иметь разрешение RECEIVE на очередь.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-receiving-all-columns-for-all-messages-in-a-conversation-group"></a>A. Получение всех столбцов для всех сообщений в группе сообщений  
 На следующем примере показано, как получаются все доступные сообщения для следующей доступной группы сообщений из очереди `ExpenseQueue`. Инструкция возвращает сообщения в виде результирующего набора.  
  
```sql  
RECEIVE * FROM ExpenseQueue ;  
```  
  
### <a name="b-receiving-specified-columns-for-all-messages-in-a-conversation-group"></a>Б. Получение указанных столбцов для всех сообщений в группе сообщений  
 На следующем примере показано, как получаются все доступные сообщения для следующей доступной группы сообщений из очереди `ExpenseQueue`. Инструкция возвращает сообщения в виде результирующего набора, содержащего столбцы `conversation_handle`, `message_type_name` и `message_body`.  
  
```sql  
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue ;  
```  
  
### <a name="c-receiving-the-first-available-message-in-the-queue"></a>В. Получение первого доступного сообщения в очереди  
 На следующем примере показано, как в качестве результирующего набора возвращается первое доступное сообщение из очереди `ExpenseQueue`.  
  
```sql  
RECEIVE TOP (1) * FROM ExpenseQueue ;  
```  
  
### <a name="d-receiving-all-messages-for-a-specified-conversation"></a>Г. Получение всех сообщений для указанного диалога  
 На следующем примере показано, как в качестве результирующего набора возвращаются все доступные сообщения для указанного диалога из очереди `ExpenseQueue`.  
  
```sql  
DECLARE @conversation_handle UNIQUEIDENTIFIER ;  
  
SET @conversation_handle = <retrieve conversation from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_handle = @conversation_handle ;  
```  
  
### <a name="e-receiving-messages-for-a-specified-conversation-group"></a>Д. Получение сообщений для заданной группы сообщений  
 На следующем примере показано, как в качестве результирующего набора возвращаются все доступные сообщения для указанной группы сообщений из очереди `ExpenseQueue`.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id =   
    <retrieve conversation group ID from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="f-receiving-into-a-table-variable"></a>Е. Получение сообщений в табличную переменную  
 На следующем примере показано, как в табличную переменную принимаются все доступные сообщения для указанной группы диалога из очереди `ExpenseQueue`.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
DECLARE @procTable TABLE(  
     service_instance_id UNIQUEIDENTIFIER,  
     handle UNIQUEIDENTIFIER,  
     message_sequence_number BIGINT,  
     service_name NVARCHAR(512),  
     service_contract_name NVARCHAR(256),  
     message_type_name NVARCHAR(256),  
     validation NCHAR,  
     message_body VARBINARY(MAX)) ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database> ;  
  
RECEIVE TOP (1)  
    conversation_group_id,  
    conversation_handle,  
    message_sequence_number,  
    service_name,  
    service_contract_name,  
    message_type_name,  
    validation,  
    message_body  
FROM ExpenseQueue  
INTO @procTable  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="g-receiving-messages-and-waiting-indefinitely"></a>Ж. Получение сообщений без ограничения времени ожидания  
 На следующем примере показано, как получаются все доступные сообщения для следующей доступной группы сообщений в очереди `ExpenseQueue`. Инструкция ожидает до тех пор, пока по крайней мере одно сообщение не станет доступным, после чего возвращает результирующий набор, содержащий все столбцы сообщения.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue) ;  
```  
  
### <a name="h-receiving-messages-and-waiting-for-a-specified-interval"></a>З. Получение сообщений с ожиданием в течение указанного интервала времени  
 На следующем примере показано, как получаются все доступные сообщения для следующей доступной группы сообщений в очереди `ExpenseQueue`. Инструкция ожидает в течение 60 секунд или до тех пор, пока по крайней мере одно сообщение не станет доступным (в зависимости от того, что произойдет раньше). Инструкция возвращает результирующий набор, который содержит все столбцы сообщения, если доступно хотя бы одно сообщение. В противном случае инструкция возвращает пустой результирующий набор.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="i-receiving-messages-modifying-the-type-of-a-column"></a>И. Получение сообщений с изменением типа столбца  
 На следующем примере показано, как получаются все доступные сообщения для следующей доступной группы сообщений в очереди `ExpenseQueue`. Если тип сообщения указывает на то, что сообщение содержит документ XML, инструкция преобразует тело сообщения в XML.  
  
```sql  
WAITFOR (  
    RECEIVE message_type_name,  
        CASE  
            WHEN validation = 'X' THEN CAST(message_body as XML)  
            ELSE NULL  
         END AS message_body   
         FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="j-receiving-a-message-extracting-data-from-the-message-body-retrieving-conversation-state"></a>К. Получение сообщений, извлечение данных из тела сообщения, извлечение состояния диалога  
 На следующем примере показано, как получается следующее доступное сообщение в очереди `ExpenseQueue` для следующей доступной группы сообщений. Если сообщение имеет тип `//Adventure-Works.com/Expenses/SubmitExpense`, инструкция извлекает из тела сообщения идентификатор служащего и список элементов. Инструкция извлекает также состояние для диалога из таблицы `ConversationState`.  
  
```sql  
WAITFOR(  
    RECEIVE   
    TOP(1)  
      message_type_name,  
      COALESCE(  
           (SELECT TOP(1) ConversationState  
            FROM CurrentConversations AS cc  
            WHERE cc.ConversationHandle = conversation_handle),  
           'NEW')  
      AS ConversationState,  
      COALESCE(  
          (SELECT TOP(1) ErrorCount  
           FROM CurrentConversations AS cc  
           WHERE cc.ConversationHandle = conversation_handle),   
           0)  
      AS ConversationErrors,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).value(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"  
                   (/rpt:ExpenseReport/rpt:EmployeeID)[1]', 'nvarchar(20)')  
         ELSE NULL  
      END AS EmployeeID,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).query(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"   
                     /rpt:ExpenseReport/rpt:ItemDetail')  
          ELSE NULL  
      END AS ItemList  
    FROM ExpenseQueue   
), TIMEOUT 60000 ;  
```  
  
## <a name="see-also"></a>См. также  
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [BEGIN CONVERSATION TIMER (Transact-SQL)](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [CREATE CONTRACT (Transact-SQL)](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/create-message-type-transact-sql.md)   
 [SEND (Transact-SQL)](../../t-sql/statements/send-transact-sql.md)   
 [CREATE QUEUE (Transact-SQL)](../../t-sql/statements/create-queue-transact-sql.md)   
 [ALTER QUEUE (Transact-SQL)](../../t-sql/statements/alter-queue-transact-sql.md)   
 [DROP QUEUE (Transact-SQL)](../../t-sql/statements/drop-queue-transact-sql.md)  
  
  
