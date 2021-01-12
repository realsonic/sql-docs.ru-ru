---
description: SEND (Transact-SQL)
title: SEND (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SEND_ON_CONVERSATION_TSQL
- ON_CONVERSATION_TSQL
- SEND
- SEND_TSQL
- SEND ON CONVERSATION
- ON CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], message sending
- SEND statement
- messages [Service Broker], sending
- sending messages
ms.assetid: b6e66aeb-1714-4c2b-b7c2-d386d77b0d46
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 25ef934ae04ca8ba9cc9c4dee9f488341a1eb352
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091836"
---
# <a name="send-transact-sql"></a>SEND (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Отправляет сообщение с помощью одного или нескольких существующих диалогов.  
  
![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SEND  
   ON CONVERSATION [(]conversation_handle [,.. @conversation_handle_n][)]  
   [ MESSAGE TYPE message_type_name ]  
   [ ( message_body_expression ) ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
ON CONVERSATION *conversation_handle [.. @conversation_handle_n]*  
Указывает диалоги, к которым принадлежит сообщение. Аргумент *conversation_handle* должен содержать правильный идентификатор диалога. Один и тот же дескриптор диалога можно использовать только один раз.  
  
MESSAGE TYPE *message_type_name*  
Указывает тип отправляемого сообщения. Этот тип сообщений должен входить в контракты служб, используемых этими диалогами. Эти контракты должны позволять отправить сообщение данного типа с этой стороны диалога. Например, целевые службы диалогов могут отправлять только сообщения, отмеченные в контракте как SENT BY TARGET или SENT BY ANY. Если это предложение пропущено, то сообщение принадлежит к типу сообщений DEFAULT.  
  
*message_body_expression*  
Содержит выражение, представляющее тело сообщения. Выражение *message_body_expression* является необязательным. Однако если аргумент *message_body_expression* указан, то выражение должно иметь тип, преобразуемый в тип **varbinary(max)**. Выражение не может иметь значение NULL. Если это предложение не указано, то тело сообщения пустое.  
  
## <a name="remarks"></a>Комментарии  
  
> [!IMPORTANT]  
>  Если инструкция SEND не является первой в пакете или хранимой процедуре, предшествующая ей инструкция должна заканчиваться точкой с запятой (;).  
  
Инструкция SEND передает сообщение от служб на одном конце одного или нескольких диалогов компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] службам на другом конце этих диалогов. Инструкция RECEIVE используется для получения отправленного сообщения из очередей, связанных с целевыми службами.  
  
Указываемые в предложении ON CONVERSATION дескрипторы диалогов берутся из одного из трех источников.  
  
- Если отправленное сообщение не является ответом на полученное от другой службы сообщение, используйте дескриптор диалога, возвращаемый инструкцией BEGIN DIALOG, с помощью которой был создан диалог.  
  
- Если отправленное сообщение является ответом на полученное от другой службы сообщение, используйте дескриптор диалога, возвращенный инструкцией RECEIVE, которая вернула исходное сообщение.  
  
- Иногда код, содержащий инструкцию SEND, отделен от кода, содержащего инструкции BEGIN DIALOG или RECEIVE, которые предоставляют дескриптор диалога. В таких случаях дескриптор диалога должен быть одним из элементов данных в сведениях о состоянии, которые передаются в код, содержащий инструкцию SEND.  
  
Сообщения, отправленные службам в других экземплярах компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], хранятся в очереди передачи текущей базы данных, пока они не будут переданы в очереди обслуживания удаленных экземпляров. Сообщения, отправленные службам в рамках одного экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], помещаются непосредственно в связанные с этими службами очереди. Если условие препятствует помещению локального сообщения непосредственно в очередь целевой службы, оно будет сохранено в очереди передачи до появления такой возможности. Это может происходить, например, при некоторых ошибках или в случае, если очередь целевой службы неактивна. Просмотреть сообщения в очереди передачи можно с помощью системного представления **sys.transmission_queue**.  
  
SEND — это атомарная инструкция. Если с помощью инструкции не удается отправить сообщение в несколько диалогов (например, из-за того, что в одном диалоге произошла ошибка), сообщения не сохраняются в очереди передачи и не помещаются в очередь целевой службы.  
  
Компонент Service Broker оптимизирует хранение и передачу сообщений, отправляемых нескольким диалогам в одной инструкции SEND.  
  
Передача сообщений, находящихся в очередях передачи экземпляра, выполняется в последовательности, которая определяется:  
  
- уровнем приоритета связанной с ними конечной точки диалога;  
  
- последовательности отправки в диалоге при одинаковом приоритете.  
  
Уровни приоритета, указанные в приоритетах диалога, применяются к сообщениям в очереди передачи, только если параметр базы данных HONOR_BROKER_PRIORITY имеет значение ON. Если параметр HONOR_BROKER_PRIORITY имеет значение OFF, все сообщения, помещаемые в очередь передачи для этой базы данных, получают приоритет по умолчанию, равный 5. Уровни приоритета не применяются к инструкции SEND, если сообщения помещаются непосредственно в очередь обслуживания одного экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
Инструкция SEND по отдельности блокирует каждый диалог, в который отправляется сообщение, чтобы обеспечить упорядоченную доставку для каждого диалога.  
  
Инструкция SEND недопустима в определяемых пользователем функциях.  
  
## <a name="permissions"></a>Разрешения  
Для отправки сообщения у текущего пользователя должно быть разрешение RECEIVE для очереди в каждой из служб, которые отправляют сообщение.  
  
## <a name="examples"></a>Примеры  
В этом примере инициируется диалог и отправляется XML-сообщение. Чтобы отправить сообщение, в примере выполняется преобразование XML-объекта в тип **varbinary(max)**.  
  
```sql
DECLARE @dialog_handle UNIQUEIDENTIFIER,  
        @ExpenseReport XML ;  
  
SET @ExpenseReport = < construct message as appropriate for the application > ;  
  
BEGIN DIALOG @dialog_handle  
FROM SERVICE [//Adventure-Works.com/Expenses/ExpenseClient]  
TO SERVICE '//Adventure-Works.com/Expenses'  
ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseProcessing] ;  
  
SEND ON CONVERSATION @dialog_handle  
    MESSAGE TYPE [//Adventure-Works.com/Expenses/SubmitExpense]  
    (@ExpenseReport) ;  
```  
  
В этом примере инициируется три диалога и в каждый из них отправляется XML-сообщение.  
  
```sql
DECLARE @dialog_handle1 UNIQUEIDENTIFIER,  
        @dialog_handle2 UNIQUEIDENTIFIER,  
        @dialog_handle3 UNIQUEIDENTIFIER,  
        @OrderMsg XML ;  
  
SET @OrderMsg = < construct message as appropriate for the application > ;  
  
BEGIN DIALOG @dialog_handle1  
FROM SERVICE [//InitiatorDB/InitiatorService]  
TO SERVICE '//TargetDB1/TargetService'  
ON CONTRACT [//AllDBs/OrderProcessing] ;  
  
BEGIN DIALOG @dialog_handle2  
FROM SERVICE [//InitiatorDB/InitiatorService]  
TO SERVICE '//TargetDB2/TargetService'  
ON CONTRACT [//AllDBs/OrderProcessing] ;  
  
BEGIN DIALOG @dialog_handle3  
FROM SERVICE [//InitiatorDB/InitiatorService]  
TO SERVICE '//TargetDB3/TargetService'  
ON CONTRACT [//AllDBs/OrderProcessing] ;  
  
SEND ON CONVERSATION (@dialog_handle1, @dialog_handle2, @dialog_handle3)  
    MESSAGE TYPE [//AllDBs/OrderMsg]  
    (@OrderMsg) ;  
```  
  
## <a name="see-also"></a>См. также  
[BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
[END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
[RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)   
[sys.transmission_queue (Transact-SQL)](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)  
  
  
