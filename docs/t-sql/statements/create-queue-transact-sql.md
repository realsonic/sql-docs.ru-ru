---
description: CREATE QUEUE (Transact-SQL)
title: CREATE QUEUE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- QUEUE_TSQL
- CREATE QUEUE
- QUEUE
- CREATE_QUEUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE QUEUE statement
- internal activation [Service Broker]
- stored procedure activation [Service Broker]
- message retention [Service Broker]
- unavailable queues [Service Broker]
- activation stored procedures [Service Broker]
- queues [Service Broker], creating
ms.assetid: fce80faf-2bdc-475d-8ca1-31438ed41fb0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ce0306d204d4ce29a76e2041bebe67971e24c3a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092024"
---
# <a name="create-queue-transact-sql"></a>CREATE QUEUE (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Создает в базе данных новую очередь. Очереди служат для хранения сообщений. Когда сообщение достигает службы, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] помещает его в очередь, связанную со службой.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
CREATE QUEUE <object>
   [ WITH
     [ STATUS = { ON | OFF } [ , ] ]
     [ RETENTION = { ON | OFF } [ , ] ]
     [ ACTIVATION (
         [ STATUS = { ON | OFF } , ]
           PROCEDURE_NAME = <procedure> ,
           MAX_QUEUE_READERS = max_readers ,
           EXECUTE AS { SELF | 'user_name' | OWNER }
            ) [ , ] ]
     [ POISON_MESSAGE_HANDLING (
         [ STATUS = { ON | OFF } ] ) ]
    ]
     [ ON { filegroup | [ DEFAULT ] } ]
[ ; ]

<object> ::=
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }

<procedure> ::=
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*database_name* (объект) — имя базы данных, в которой должна быть создана новая очередь. Параметр *database_name* должен указывать имя существующей базы данных. Если *database_name* не указан, в текущей базе данных создается очередь.

*schema_name* (объект) — имя схемы, которой принадлежит новая очередь. Значения по умолчанию для схемы по умолчанию текущего пользователя, выполняющего инструкцию. Если инструкция CREATE QUEUE выполняется элементом предопределенной роли сервера sysadmin или элементом предопределенных ролей базы данных db_dbowner или db_ddladmin в базе данных, указанной аргументом *database_name*, то *schema_name* может определять схему, не связанную с именем входа текущего соединения. Иначе указанная схема *schema_name* должна являться схемой по умолчанию для пользователя, выполняющего инструкцию.

*queue_name* — имя создаваемой очереди. Это имя должно соответствовать правилам для идентификаторов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

STATUS (Queue) — указывает, доступна очередь (ON) или нет (OFF). Когда очередь недоступна, нельзя ни добавлять в нее сообщения, ни удалять их из нее. Можно создать очередь в состоянии недоступности, чтобы сообщения не поступали в очередь до тех пор, пока очередь не будет сделана доступной с помощью инструкции ALTER QUEUE. Если это предложение опущено, значение по умолчанию равно ON, и очередь доступна.

RETENTION — указывает параметр хранения для очереди. Если указано RETENTION = ON, то все сообщения, посылаемые или отправляемые во время диалогов, которые используют данную очередь, хранятся в очереди до окончания этих диалогов. Это позволяет хранить сообщения для аудита или выполнять компенсирующие транзакции в случае ошибки. Если это предложение не указано, параметр хранения по умолчанию установлен в OFF.

> [!NOTE]
> Установка RETENTION = ON может уменьшить производительность. Ее следует использовать только в том случае, если это требуется для приложения.

ACTIVATION — указывает сведения о хранимых процедурах, которые нужно активировать, чтобы начать обработку сообщений в этой очереди.

STATUS (Активация) — указывает, запускает ли компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] хранимую процедуру. Если параметр STATUS = ON, то очередь запускает хранимую процедуру, указанную параметром PROCEDURE_NAME, если количество выполняемых в настоящий момент хранимых процедур меньше, чем значение MAX_QUEUE_READERS, и если сообщения прибывают в очередь быстрее, чем хранимые процедуры получают сообщения. Если параметр STATUS = OFF, то очередь не активирует хранимую процедуру. Если это предложение не указано, значение по умолчанию равно ON.

PROCEDURE_NAME = \<procedure> — указывает имя хранимой процедуры, которая должна быть активирована для обработки сообщений в данной очереди. Это значение должно являться идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

*database_name* (процедура) — имя базы данных, которая содержит хранимую процедуру.

*schema_name* (процедура) — имя схемы, которая содержит хранимую процедуру.

*procedure_name* — имя хранимой процедуры.

MAX_QUEUE_READERS =*max_readers* — определяет максимальное количество экземпляров хранимой процедуры активации, запускаемых очередью одновременно. Значение аргумента *max_readers* должно быть числом от **0** до **32 767**.

EXECUTE AS — указывает учетную запись пользователя базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], от имени которого выполняется хранимая процедура активации. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен быть в состоянии проверить разрешения для пользователя, когда очередь запускает хранимую процедуру. Для пользователя домена сервер должен быть подключен к домену в момент активации процедуры, иначе произойдет ошибка активации. Для пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сервер всегда в состоянии проверить разрешения.

SELF указывает, что хранимая процедура выполняется как текущий пользователь. (участника базы данных, выполняющего эту инструкцию CREATE QUEUE).

*user_name* — имя пользователя, от имени которого выполняется хранимая процедура. Параметр *user_name* должен являться именем допустимого пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], указанным в виде идентификатора [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Текущий пользователь должен иметь разрешение IMPERSONATE на указанного аргументом *user_name* пользователя.

OWNER — указывает, что хранимая процедура выполняется в контексте владельца очереди.

POISON_MESSAGE_HANDLING — определяет, включена ли обработка сообщений о сбое для очереди. Значение по умолчанию — ON.

Очередь, в которой параметру обработки сообщений о сбое задано значение OFF, не будет отключена после пяти последовательных откатов транзакций. Это позволяет приложению определить пользовательскую систему обработки опасных сообщений.

ON *filegroup |* [**DEFAULT**] — указывает файловую группу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на основании которой должна создаваться эта очередь. Можно использовать параметр *filegroup* для идентификации файловой группы или идентификатор DEFAULT, чтобы использовать файловую группу по умолчанию для базы данных компонента Service Broker. В контексте данного предложения слово DEFAULT не является ключевым словом и должно быть отделено как идентификатор. Если файловая группа не задана, то очередь использует файловую группу по умолчанию для базы данных.

## <a name="remarks"></a>Remarks

Очередь может быть использована как целевой объект инструкции SELECT. Однако содержимое очереди может быть изменено только с помощью инструкций, которые выполняются в таких диалогах компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)], как SEND, RECEIVE и END CONVERSATION. Очередь не может быть целевым объектом инструкций INSERT, UPDATE, DELETE и TRUNCATE.

Очередь не может быть временным объектом. Поэтому имена очередей, начинающиеся с символа **#** , недопустимы.

Создание очереди в неактивном состоянии позволяет службе получить ее структуру, прежде чем сообщения начнут поступать в очередь.

Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] не останавливает хранимые процедуры активации, если в очереди отсутствуют сообщения. Хранимые процедуры активации должны завершить работу, если в течение короткого промежутка времени в очереди отсутствуют доступные сообщения.

Разрешения для хранимых процедур активации проверяются во время их активации компонентом [!INCLUDE[ssSB](../../includes/sssb-md.md)], а не при создании очереди. Инструкция CREATE QUEUE не проверяет, имеет ли пользователь, указанный в предложении EXECUTE AS, разрешения на выполнение хранимой процедуры, указанной в предложении PROCEDURE NAME.

Если очередь недоступна, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] сохраняет сообщения для служб, пользующихся данной очередью, в очереди передачи для базы данных. Представление каталога sys.transmission_queue содержит представление очереди передачи.

Очередь относится к объектам схемы. Очереди появляются в представлении каталога sys.objects.

Следующая таблица содержит столбцы в очереди.

|Имя столбца|Тип данных|Описание|
|-----------------|---------------|-----------------|
|status|**tinyint**|Состояние сообщения. Инструкция RECEIVE возвращает сообщения со значением состояния, равным **1**. Если хранение сообщений включено, то значение состояния устанавливается в 0. Если хранение сообщений выключено, то сообщение удаляется из очереди. Сообщения в очереди могут иметь одно из следующих состояний:<br /><br /> **0**=Сохраненное полученное сообщение<br /><br /> **1**=Готовность к получению<br /><br /> **2**=Еще не завершено<br /><br /> **3**=Сохраненное отправленное сообщение|
|priority|**tinyint**|Уровень приоритета, назначенный для этого сообщения.|
|queuing_order|**bigint**|Порядковый номер сообщения в очереди.|
|conversation_group_id|**uniqueidentifier**|Идентификатор группы сообщений, которой принадлежит данное сообщение.|
|conversation_handle|**uniqueidentifier**|Дескриптор диалога, частью которого является данное сообщение.|
|message_sequence_number|**bigint**|Порядковый номер сообщения в диалоге.|
|service_name|**nvarchar(512)**|Имя службы, к которой относится диалог.|
|service_id|**int**|Идентификатор объекта службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], к которой относится диалог.|
|service_contract_name|**nvarchar(256)**|Имя контракта, которому следует диалог.|
|service_contract_id|**int**|Идентификатор объекта контракта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которому следует диалог.|
|message_type_name|**nvarchar(256)**|Имя типа сообщения, который описывает сообщение.|
|message_type_id|**int**|Идентификатор объекта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для типа сообщения, описывающий сообщение.|
|validation|**nchar(2)**|Проверка, используемая для сообщения:<br /><br /> E = пустая<br /><br /> N = нет<br /><br /> X = XML|
|message_body|**varbinary(max)**|Содержимое сообщения.|
|message_id|**uniqueidentifier**|Уникальный идентификатор для сообщения.|

## <a name="permissions"></a>Разрешения

Разрешение на создание службы имеют члены предопределенных ролей базы данных `db_ddladmin` и `db_owner` и предопределенной роли сервера `sysadmin`.

Разрешение `REFERENCES` на очередь по умолчанию имеет владелец очереди, члены предопределенных ролей `db_ddladmin` или `db_owner` базы данных, а также члены предопределенной роли сервера `sysadmin`.

Разрешение `RECEIVE` на очередь по умолчанию имеет владелец очереди, члены предопределенной роли `db_owner` базы данных, а также члены предопределенной роли сервера `sysadmin`.

## <a name="examples"></a>Примеры

### <a name="a-creating-a-queue-with-no-parameters"></a>A. Создание очереди без параметров

В следующем примере создается очередь, готовая к приему сообщений. Для очереди не указана хранимая процедура активации.

```sql
CREATE QUEUE ExpenseQueue ;
```

### <a name="b-creating-an-unavailable-queue"></a>Б. Создание недоступной очереди

В следующем примере создается очередь, недоступная для приема сообщений. Для очереди не указана хранимая процедура активации.

```sql
CREATE QUEUE ExpenseQueue WITH STATUS=OFF ;
```

### <a name="c-creating-a-queue-and-specify-internal-activation-information"></a>В. Создание очереди с указанием внутренних сведений об активации

В следующем примере создается очередь, готовая к приему сообщений. При поступлении сообщения в очередь запускается хранимая процедура `expense_procedure`. Хранимая процедура выполняется в контексте пользователя `ExpenseUser`. Очередь запускает не более `5` экземпляров хранимой процедуры.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS=ON,
    ACTIVATION (
        PROCEDURE_NAME = expense_procedure
        , MAX_QUEUE_READERS = 5
        , EXECUTE AS 'ExpenseUser' ) ;
```

### <a name="d-creating-a-queue-on-a-specific-filegroup"></a>Г. Создание очереди для указанной файловой группы

В следующем примере создается очередь на основании файловой группы `ExpenseWorkFileGroup`.

```sql
CREATE QUEUE ExpenseQueue
    ON ExpenseWorkFileGroup ;
```

### <a name="e-creating-a-queue-with-multiple-parameters"></a>Д. Создание очереди с несколькими параметрами

В следующем примере создается очередь на основании файловой группы `DEFAULT`. Очередь недоступна для приема сообщений. Сообщения хранятся в очереди до завершения диалога, которому они принадлежат. При переключении очереди в состояние готовности к приему сообщений с помощью инструкции ALTER QUEUE в очереди активируется хранимая процедура `2008R2.dbo.expense_procedure` для обработки сообщений. Хранимая процедура выполняется в контексте пользователя, выполнившего инструкцию `CREATE QUEUE`. Очередь запускает не более `10` экземпляров хранимой процедуры.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS = OFF
      , RETENTION = ON
      , ACTIVATION (
          PROCEDURE_NAME = AdventureWorks2012.dbo.expense_procedure
          , MAX_QUEUE_READERS = 10
          , EXECUTE AS SELF )
    ON [DEFAULT];
```

## <a name="see-also"></a>См. также:

- [ALTER QUEUE (Transact-SQL)](../../t-sql/statements/alter-queue-transact-sql.md)
- [CREATE SERVICE (Transact-SQL)](../../t-sql/statements/create-service-transact-sql.md)
- [DROP QUEUE (Transact-SQL)](../../t-sql/statements/drop-queue-transact-sql.md)
- [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
