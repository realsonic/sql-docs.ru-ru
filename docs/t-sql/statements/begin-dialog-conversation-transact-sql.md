---
description: BEGIN DIALOG CONVERSATION (Transact-SQL)
title: BEGIN DIALOG CONVERSATION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DIALOG CONVERSATION
- DIALOG
- BEGIN_DIALOG_TSQL
- BEGIN_DIALOG_CONVERSATION_TSQL
- DIALOG_CONVERSATION_TSQL
- DIALOG_TSQL
- BEGIN DIALOG
- BEGIN DIALOG CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker]
- beginning dialogs
- dialogs [Service Broker], beginning
- BEGIN DIALOG CONVERSATION statement
- cryptography [SQL Server], conversations
- opening conversations
- encryption [SQL Server], conversations
- starting conversations
ms.assetid: 8e814f9d-77c1-4906-b8e4-668a86fc94ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f1c32ad7e6a970b9ebb50dc29e7dda767f5e4f40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098522"
---
# <a name="begin-dialog-conversation-transact-sql"></a>BEGIN DIALOG CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Создает диалог между двумя службами. Диалог — это сеанс связи, который обеспечивает доставку сообщений строго по порядку между двумя службами.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
BEGIN DIALOG [ CONVERSATION ] @dialog_handle  
   FROM SERVICE initiator_service_name  
   TO SERVICE 'target_service_name'  
       [ , { 'service_broker_guid' | 'CURRENT DATABASE' }]   
   [ ON CONTRACT contract_name ]  
   [ WITH  
   [  { RELATED_CONVERSATION = related_conversation_handle   
      | RELATED_CONVERSATION_GROUP = related_conversation_group_id } ]   
   [ [ , ] LIFETIME = dialog_lifetime ]   
   [ [ , ] ENCRYPTION = { ON | OFF }  ] ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 **@** _dialog_handle_  
 Переменная, используемая для хранения дескриптора диалога, сформированного системой для нового диалога, который был возвращен инструкцией BEGIN DIALOG CONVERSATION. Переменная должна иметь тип **uniqueidentifier**.  
  
 FROM SERVICE *initiator_service_name*  
 Указывает службу стороны, вызывающей диалог. Указанное имя должно быть именем службы в текущей базе данных. Очередь, указанная для вызывающей службы, получает сообщения, возвращенные целевой службой, и сообщения, созданные компонентом Service Broker для данного диалога.  
  
 TO SERVICE **'** _target_service_name_ **'**  
 Указывает целевую службу, с которой необходимо начать диалог. *target_service_name* имеет тип **nvarchar(256)**. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] производит побайтовое сравнение при поиске соответствия строке *target_service_name*. Другими словами, сравнение чувствительно к регистру и не использует текущие параметры сортировки.  
  
 *service_broker_guid*  
 Указывает базу данных, в которой расположена целевая служба. Если несколько баз данных содержат экземпляр целевой службы, можно передавать данные определенной базе данных, задав аргумент *service_broker_guid*.  
  
 *service_broker_guid* имеет тип **nvarchar(128)**. Чтобы найти *service_broker_guid* для базы данных, выполните следующий запрос в базе данных:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID() ;  
```  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
 **'** CURRENT DATABASE **'**  
 Указывает на то, что диалог использует аргумент *service_broker_guid* для текущей базы данных.  
  
 ON CONTRACT *contract_name*  
 Указывает контракт, которого придерживается данный диалог. Контракт должен существовать в текущей базе данных. Если целевая служба не принимает новые диалоги по указанному контракту, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] возвращает сообщение об ошибке диалога. Если это предложение опущено, диалог придерживается контракта с именем **DEFAULT**.  
  
 RELATED_CONVERSATION **=** _related_conversation_handle_  
 Указывает существующую группу сообщений, к которой добавляется новый диалог. Если присутствует данное предложение, новый диалог принадлежит к той же группе сообщений, что и диалог, указанный аргументом *related_conversation_handle*. Аргумент *related_conversation_handle* должен иметь тип, способный неявно преобразовываться в тип **uniqueidentifier**. Инструкция завершается неудачно, если аргумент *related_conversation_handle* не ссылается на существующий диалог.  
  
 RELATED_CONVERSATION_GROUP **=** _related_conversation_group_id_  
 Указывает существующую группу сообщений, к которой добавляется новый диалог. Если присутствует данное предложение, новый диалог будет добавлен к группе сообщений, указанной аргументом *related_conversation_group_id*. Аргумент *related_conversation_group_id* должен иметь тип, способный неявно преобразовываться в тип **uniqueidentifier**. Если аргумент *related_conversation_group_id* не ссылается на существующую группу сообщений, компонент Service Broker создает новую группу сообщений с заданным аргументом *related_conversation_group_id* и связывает новый диалог с полученной группой сообщений.  
  
 LIFETIME **=** _dialog_lifetime_  
 Указывает максимальный период времени, в течение которого диалог будет оставаться открытым. Для успешного завершения диалога обе конечные точки должны явно завершить диалог до истечения его времени жизни. Значение аргумента *dialog_lifetime* должно быть выражено в секундах. Этот аргумент имеет тип **int**. Если предложение LIFETIME не указано, время жизни диалога принимает максимальное значение типа данных **int**.  
  
 ENCRYPTION  
 Указывает, должны ли отправленные и полученные в данном диалоге сообщения шифроваться, если они отправляются за пределы экземпляра [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Диалог, который подлежит шифрованию, является *защищенным диалогом*. Если используется предложение ENCRYPTION = ON, а сертификаты, необходимые для поддержки шифрования, не настроены, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] возвращает сообщение об ошибке диалога. При использовании предложения ENCRYPTION = OFF сообщения шифруются в том случае, если привязка удаленной службы настроена на использование аргумента *target_service_name*; иначе сообщения отсылаются нешифрованными. Если данное предложение отсутствует, присваивается значение по умолчанию ON.  
  
> [!NOTE]  
>  Сообщения, которыми обмениваются службы в одном и том же экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], никогда не шифруются. Однако главный ключ базы данных и сертификаты на шифрование все еще требуются для диалогов, использующих шифрование, если службы, для которых создается диалог, находятся в разных базах данных. Благодаря чему диалоги могут быть продолжены, когда в процессе диалога одна из баз данных перемещается в другой экземпляр.  
  
## <a name="remarks"></a>Комментарии  
 Все сообщения являются частью диалога. Следовательно, перед отправкой сообщения целевой службе служба вызывающей стороны должна начать с ней диалог. Данные, указанные в инструкции BEGIN DIALOG CONVERSATION, аналогичны адресу письма. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] использует эти данные для отправки сообщений правильной службе. Указанная в предложении TO SERVICE служба — это адрес, по которому отправляются сообщения. Служба, указанная в предложении FROM SERVICE, является обратным адресом, используемым для ответных сообщений.  
  
 Целевой службе диалога не обязательно вызывать BEGIN DIALOG CONVERSATION. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] создает диалог в целевой базе данных, когда от инициатора диалога поступает первое сообщение.  
  
 В начале диалога создается конечная точка диалога в базе данных для инициирующей службы, но сетевое подключение к экземпляру, на котором размещается целевая служба, не создается. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] не устанавливает соединение с целевой службой диалога, пока не будет отправлено первое сообщение.  
  
 Если в инструкции BEGIN DIALOG CONVERSATION не указан связанный диалог или группа сообщений, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] создает новую группу сообщений для нового диалога.  
  
 Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] запрещает произвольное группирование диалогов. Для всех диалогов в группе сообщений в предложении FROM должна быть указана либо вызывающая служба, либо целевая служба диалога.  
  
 Команда BEGIN DIALOG CONVERSATION блокирует группу сообщений, содержащую возвращенный аргумент *dialog_handle*. Если команда содержит предложение RELATED_CONVERSATION_GROUP, группой сообщений для *dialog_handle* является та, которая указана в аргументе *related_conversation_group_id*. Если команда содержит предложение RELATED_CONVERSATION, группой сообщений для *dialog_handle* является та, которая указана в аргументе *related_conversation_handle*.  
  
 Инструкция BEGIN DIALOG CONVERSATION не используется в функции, определяемой пользователем.  
  
## <a name="permissions"></a>Разрешения  
 Чтобы создать диалог, текущий пользователь должен иметь разрешение RECEIVE на очередь для службы, указанной в предложении FROM команды, и разрешение REFERENCES на указанный контракт.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-beginning-a-dialog"></a>A. Создание диалога  
 В следующем примере начинается диалог, идентификатор которого сохраняется в параметре `@dialog_handle.`. Служба `//Adventure-Works.com/ExpenseClient` является инициатором диалога, а служба `//Adventure-Works.com/Expenses` — его целевой службой. Этот диалог следует контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="b-beginning-a-dialog-with-an-explicit-lifetime"></a>Б. Создание диалога с явно заданным временем жизни  
 В следующем примере создается двусторонний диалог с сохранением идентификатора диалога в параметре `@dialog_handle`. Служба `//Adventure-Works.com/ExpenseClient` является инициатором диалога, а служба `//Adventure-Works.com/Expenses` — его целевой службой. Этот диалог следует контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`. Если диалог не был закрыт при помощи команды END CONVERSATION в течение `60` секунд, компонент Service Broker завершает этот диалог с ошибкой.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH LIFETIME = 60 ;  
```  
  
### <a name="c-beginning-a-dialog-with-a-specific-broker-instance"></a>В. Создание диалога с определенным экземпляром брокера  
 В следующем примере создается двусторонний диалог с сохранением идентификатора диалога в параметре `@dialog_handle`. Служба `//Adventure-Works.com/ExpenseClient` является инициатором диалога, а служба `//Adventure-Works.com/Expenses` — его целевой службой. Этот диалог следует контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`. Компонент Service Broker перенаправляет сообщения этого диалога компоненту Service Broker с идентификатором GUID `a326e034-d4cf-4e8b-8d98-4d7e1926c904.`  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses',   
              'a326e034-d4cf-4e8b-8d98-4d7e1926c904'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="d-beginning-a-dialog-and-relating-it-to-an-existing-conversation-group"></a>Г. Создание диалога и его соотнесение с существующей группой сообщений  
 В следующем примере создается двусторонний диалог с сохранением идентификатора диалога в параметре `@dialog_handle`. Служба `//Adventure-Works.com/ExpenseClient` является инициатором диалога, а служба `//Adventure-Works.com/Expenses` — его целевой службой. Этот диалог следует контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`. Компонент Service Broker связывает этот диалог с группой сообщений, указанной параметром `@conversation_group_id`, а не создает новую группу сообщений.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION_GROUP = @conversation_group_id ;  
```  
  
### <a name="e-beginning-a-dialog-with-an-explicit-lifetime-and-relating-the-dialog-to-an-existing-conversation"></a>Д. Создание диалога с явно заданным временем жизни и соотнесение диалога с одним из существующих диалогов  
 В следующем примере создается двусторонний диалог с сохранением идентификатора диалога в параметре `@dialog_handle`. Служба `//Adventure-Works.com/ExpenseClient` является инициатором диалога, а служба `//Adventure-Works.com/Expenses` — его целевой службой. Этот диалог следует контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`. Новый диалог принадлежит к той же группе сообщений, что и `@existing_conversation_handle`. Если диалог не был закрыт при помощи команды END CONVERSATION в течение `600` секунд, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] завершает этот диалог с ошибкой.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
DECLARE @existing_conversation_handle UNIQUEIDENTIFIER  
  
SET @existing_conversation_handle = <retrieve conversation handle from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION = @existing_conversation_handle  
   LIFETIME = 600 ;  
```  
  
### <a name="f-beginning-a-dialog-with-optional-encryption"></a>Е. Создание диалога с дополнительным шифрованием  
 В следующем примере создается двусторонний диалог с сохранением идентификатора диалога в параметре `@dialog_handle`. Служба `//Adventure-Works.com/ExpenseClient` является инициатором диалога, а служба `//Adventure-Works.com/Expenses` — его целевой службой. Этот диалог следует контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`. Диалог в этом примере разрешает сообщению перемещаться в сети без шифрования в случае, если шифрование недоступно.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH ENCRYPTION = OFF ;  
```  
  
## <a name="see-also"></a>См. также  
 [BEGIN CONVERSATION TIMER (Transact-SQL)](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [MOVE CONVERSATION (Transact-SQL)](../../t-sql/statements/move-conversation-transact-sql.md)   
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
