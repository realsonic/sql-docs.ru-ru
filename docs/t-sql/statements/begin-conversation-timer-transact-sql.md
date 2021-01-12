---
description: BEGIN CONVERSATION TIMER (Transact-SQL)
title: BEGIN CONVERSATION TIMER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONVERSATION
- BEGIN_CONVERSATION_TSQL
- TIMER_TSQL
- TIMER
- CONVERSATION TIMER
- CONVERSATION_TIMER_TSQL
- BEGIN CONVERSATION TIMER
- BEGIN_CONVERSATION_TIMER_TSQL
- CONVERSATION_TSQL
- BEGIN CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CONVERSATION TIMER statement
- DialogTimer message
- dialogs [Service Broker], beginning
- timeouts [Service Broker]
- timer messages [Service Broker]
- conversations [Service Broker], timers
- starting timers [Service Broker]
- https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer message
ms.assetid: 98e49b3f-a38f-4180-8171-fa9cb30db4cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6c30351f5fe448b967d874f3ef0a9eb5dc94faa5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098479"
---
# <a name="begin-conversation-timer-transact-sql"></a>BEGIN CONVERSATION TIMER (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Запускает таймер. По истечении времени ожидания [!INCLUDE[ssSB](../../includes/sssb-md.md)] помещает сообщение типа `https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer` в локальную очередь для диалога.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
BEGIN CONVERSATION TIMER ( conversation_handle )  
   TIMEOUT = timeout   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 BEGIN CONVERSATION TIMER **(** _conversation\_handle_ **)**  
 Указывает диалог, длительность которого нужно проконтролировать. *conversation_handle* должен иметь тип **uniqueidentifier**.  
  
 TIMEOUT  
 Указывает время ожидания (в секундах) перед добавлением сообщения в очередь.  
  
## <a name="remarks"></a>Комментарии  
 Таймер диалога предоставляет приложению способ получения сообщения в диалоге по истечении заданного времени. Вызов BEGIN CONVERSATION TIMER в диалоге до истечения времени ожидания устанавливает новое значение времени ожидания. В отличие от времени жизни диалога у каждой стороны диалога имеется свой таймер диалога. Сообщение **DialogTimer** появляется в локальной очереди, не оказывая влияния на удаленную сторону диалога. Поэтому приложение может использовать сообщение таймера для любых нужд.  
  
 Например, можно использовать таймер диалога для предотвращения слишком долгого ожидания приложением запоздалого отклика. Если завершение диалога ожидается в течение 30 секунд, то можно установить таймер для этого диалога на 60 секунд (30 секунд плюс 30 секунд допустимой задержки). Если диалог все еще открыт по истечении 60 секунд, приложение получит в очереди этого диалога сообщение об истечении времени ожидания.  
  
 В качестве альтернативы приложение может использовать таймер диалога для запроса активации в определенный момент. Например, можно создать службу, которая каждые пять минут сообщает число активных подключений. или службу, которая каждый вечер сообщает число открытых заказов на покупку. Служба устанавливает таймер диалога так, чтобы его время ожидания истекло в нужный момент; когда время ожидания истечет, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] отправляет сообщение **DialogTimer**. Сообщение **DialogTimer** приводит к тому, что компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] запускает хранимую процедуру активации для очереди. Хранимая процедура отправляет сообщение удаленной службе и перезапускает таймер диалога.  
  
 Инструкция BEGIN CONVERSATION TIMER не может использоваться в пользовательских функциях.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешением на установку таймера диалога обладают пользователи, у которых есть разрешение SEND для службы в диалоге, члены предопределенной роли сервера **sysadmin** и члены предопределенной роли базы данных **db_owner**.  
  
## <a name="examples"></a>Примеры  
 Этот пример устанавливает двухминутное время ожидания для диалога с дескриптором `@dialog_handle`.  
  
```sql 
-- @dialog_handle is of type uniqueidentifier and  
-- contains a valid conversation handle.  
  
BEGIN CONVERSATION TIMER (@dialog_handle)  
TIMEOUT = 120 ;  
```  
  
## <a name="see-also"></a>См. также  
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)  
  
  
