---
description: EVENTDATA (Transact-SQL)
title: EVENTDATA (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EVENTDATA
- fn_event_data
- EVENTDATA_TSQL
- fn_event_data_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server instance event data [SQL Server]
- event notifications [SQL Server], event status
- events [SQL Server], status information
- EVENTDATA function
- status information [SQL Server], events
- DDL triggers, returning event data
ms.assetid: 03a80e63-6f37-4b49-bf13-dc35cfe46c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: ab94e98f7a7a3dadacca109e64fa8c5cc2f5e1b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093604"
---
# <a name="eventdata-transact-sql"></a>EVENTDATA (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Эта функция возвращает сведения о событиях сервера или базы данных. Когда создается уведомление о событии и результаты возвращаются на соответствующий компонент Service Broker, вызывается `EVENTDATA`. Также использование `EVENTDATA` поддерживается в триггерах DDL и триггерах входа.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
EVENTDATA( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks  
Функция `EVENTDATA` возвращает данные только в том случае, если она вызвана непосредственно из триггера DDL или триггера входа. `EVENTDATA` всегда возвращает значение NULL, если ее вызывают другие подпрограммы, даже те, которые вызваны с помощью триггера DDL или триггера входа.
  
Данные, возвращаемые функцией `EVENTDATA`, становятся недопустимыми после транзакции, которая:

+ вызывает `EVENTDATA` явным образом;
+ вызывает `EVENTDATA` явным образом;
+ зафиксирована;
+ отменена.  
  
> [!CAUTION]  
>  `EVENTDATA` возвращает данные в формате XML, которые передаются клиенту в кодировке Юникод с 2-байтовым представлением символов. `EVENTDATA` возвращает XML-код, который может представлять такие кодовые точки в Юникод:  
>   
>  `0x0009`  
>   
>  `0x000A`  
>   
>  `0x000D`  
>   
>  `>= 0x0020 && <= 0xD7FF`  
>   
>  `>= 0xE000 && <= 0xFFFD`  
>   
>  В XML невозможно выразить и использовать некоторые символы, встречающиеся в идентификаторах и данных [!INCLUDE[tsql](../../includes/tsql-md.md)]. Символы или данные с элементами кода, не указанными в приведенном выше списке, сопоставляются с вопросительным знаком (?).  
  
Пароли не отображаются при выполнении инструкций `CREATE LOGIN` и `ALTER LOGIN`. Это сделано для безопасности входа.  
  
## <a name="schemas-returned"></a>Возвращаемые схемы  
Функция EVENTDATA возвращает значение с типом данных **xml**. По умолчанию определение схемы для всех событий устанавливается в каталог [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Tools\Binn\schemas\sqlserver\2006\11\events\events.xsd.  
  
Кроме того, схема событий опубликована на веб-странице [Microsoft SQL Server XML Schemas](https://go.microsoft.com/fwlink/?LinkID=31850) (Схемы XML Microsoft SQL Server).  
  
Чтобы извлечь схему для какого-то конкретного события, нужно выполнить поиск в схеме для составного типа `EVENT_INSTANCE_<event_type>`. Например, чтобы извлечь схему для события `DROP_TABLE`, выполните поиск в схеме по `EVENT_INSTANCE_DROP_TABLE`.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-querying-event-data-in-a-ddl-trigger"></a>A. Запрос данных о событии в триггере DDL  
В этом примере создается триггер DDL, который блокирует создание таблиц в базе данных. Примените запрос XQuery к XML-данным, сформированным функцией `EVENTDATA`, чтобы получить инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)], которая вызвала срабатывание триггера. Дополнительные сведения см. в статье [Справочник по языку XQuery (SQL Server)](../../xquery/xquery-language-reference-sql-server.md).  
  
> [!NOTE]  
>  При использовании представления **В виде сетки** в запросе [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] к элементу `<TSQLCommand>`, символы разрыва строк в тексте команды не отображаются. Используйте вместо этого представление **В виде текста**.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER safety   
ON DATABASE   
FOR CREATE_TABLE   
AS   
    PRINT 'CREATE TABLE Issued.'  
    SELECT EVENTDATA().value  
        ('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
   RAISERROR ('New tables cannot be created in this database.', 16, 1)   
   ROLLBACK  
;  
GO  
--Test the trigger.  
CREATE TABLE NewTable (Column1 INT);  
GO  
--Drop the trigger.  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
> [!NOTE]  
>  Чтобы получить данные о событии используйте метод XQuery **value()**, а не **query()**. Метод **query()** возвращает XML-данные, содержащие символы возврата каретки и переноса строки (CR/LF), экранированные амперсандом, а метод **value()** не отображает эти символы.  
  
### <a name="b-creating-a-log-table-with-event-data-in-a-ddl-trigger"></a>Б. Создание таблицы журнала с данными о событии в триггере DDL  
В этом примере для хранения сведений обо всех событиях уровня базы данных создается таблица, которая заполняется триггером DDL. Примените запрос XQuery к XML-данным, сформированным функцией `EVENTDATA`, чтобы получить тип события и инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
```sql 
USE AdventureWorks2012;  
GO  
CREATE TABLE ddl_log (PostTime DATETIME, DB_User NVARCHAR(100), Event NVARCHAR(100), TSQL NVARCHAR(2000));  
GO  
CREATE TRIGGER log   
ON DATABASE   
FOR DDL_DATABASE_LEVEL_EVENTS   
AS  
DECLARE @data XML  
SET @data = EVENTDATA()  
INSERT ddl_log   
   (PostTime, DB_User, Event, TSQL)   
   VALUES   
   (GETDATE(),   
   CONVERT(NVARCHAR(100), CURRENT_USER),   
   @data.value('(/EVENT_INSTANCE/EventType)[1]', 'NVARCHAR(100)'),   
   @data.value('(/EVENT_INSTANCE/TSQLCommand)[1]', 'NVARCHAR(2000)') ) ;  
GO  
--Test the trigger.  
CREATE TABLE TestTable (a INT);  
DROP TABLE TestTable ;  
GO  
SELECT * FROM ddl_log ;  
GO  
--Drop the trigger.  
DROP TRIGGER log  
ON DATABASE;  
GO  
--Drop table ddl_log.  
DROP TABLE ddl_log;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [Использование функции EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md)   
 [Триггеры DDL](../../relational-databases/triggers/ddl-triggers.md)   
 [Уведомления о событиях](../../relational-databases/service-broker/event-notifications.md)   
 [Триггеры входа](../../relational-databases/triggers/logon-triggers.md)  
  
  
