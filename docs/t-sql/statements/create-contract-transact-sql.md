---
description: Инструкция CREATE CONTRACT (Transact-SQL)
title: CREATE CONTRACT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONTRACT_TSQL
- CREATE_CONTRACT_TSQL
- CREATE CONTRACT
- CONTRACT
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE CONTRACT statement
- contracts [Service Broker], creating
- message types [Service Broker], contracts
ms.assetid: 494cbfa6-8e93-4161-a64d-90d681915211
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3541197e5c74ebf5c5bc2ad38c43770028fbd43b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099540"
---
# <a name="create-contract-transact-sql"></a>Инструкция CREATE CONTRACT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создание нового контракта. Контракт определяет типы сообщений, используемые в диалогах компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)], а также определяет, какой из участников диалога может посылать сообщения этого типа. Каждый диалог соответствует контракту. Инициирующая служба определяет контракт для диалога перед его началом. Целевая служба определяет контракты, диалоги для которых она принимает.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
CREATE CONTRACT contract_name  
   [ AUTHORIZATION owner_name ]  
      (  {   { message_type_name | [ DEFAULT ] }  
          SENT BY { INITIATOR | TARGET | ANY }   
       } [ ,...n] )   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *contract_name*  
 Имя создаваемого контракта. Новый контракт создается в текущей базе данных и передается во владение участнику, указанному в предложении AUTHORIZATION.  Не могут быть указаны имена сервера, базы данных и схемы. Имя *contract_name* может содержать не более 128 символов.  
  
> [!NOTE]  
>  Не создавайте контракт, использующий ключевое слово ANY для аргумента *contract_name*. При указании ключевого слова ANY для имени контракта в инструкции CREATE BROKER PRIORITY приоритет применяется ко всем контрактам. Его применение не ограничивается контрактом с именем ANY.  
  
 AUTHORIZATION *owner_name*  
 Устанавливает в качестве владельца контракта определенного пользователя или роль базы данных. Если текущим пользователем является **dbo** или **sa**, то аргумент *owner_name* может быть именем любого допустимого пользователя или роли. В противном случае аргумент *owner_name* должен быть именем текущего пользователя, именем пользователя, на олицетворение которого у текущего пользователя есть разрешения, или именем роли, к которой относится текущий пользователь. Если это предложение опущено, контракт принадлежит текущему пользователю.  
  
 *message_type_name*  
 Имя типа сообщений, включаемого в качестве части контракта.  
  
 SENT BY  
 Указывает, какая из конечных точек может послать сообщение указанного типа сообщений. Контракты сохраняют сообщения, которые могут быть использованы службами для создания определенных диалогов. У каждого диалога есть две конечные точки: конечная точка *инициатора*, то есть служба, которая начала диалог, и конечная точка *цели*, то есть служба, с которой связывается инициатор.  
  
 INITIATOR  
 Указывает на то, что только инициатор диалога может посылать сообщения определенного типа. Служба, которая начинает диалог, называется *инициатором* сеанса связи.  
  
 TARGET  
 Указывает на то, что только цель диалога может посылать сообщения определенного типа. Служба, которая принимает диалог, инициированный другой службой, называется *целью* диалога.  
  
 ANY  
 Указывает на то, что сообщения этого типа могут посылаться как инициатором, так и целью.  
  
 [ DEFAULT ]  
 Указывает на то, что данный контракт поддерживает сообщения с установленным по умолчанию типом сообщений. По умолчанию во всех базах данных содержится тип сообщений с названием DEFAULT. Этот тип данных использует проверку типа NONE.  В контексте данного предложения слово DEFAULT не является ключевым словом и должно быть отделено как идентификатор. Microsoft SQL Server также предоставляет контракт DEFAULT, указывающий тип сообщения DEFAULT.   
  
## <a name="remarks"></a>Комментарии  
 Порядок типов сообщений в контракте не важен. После того, как цель получает первое сообщение, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] позволяет каждому участнику диалога в любое время посылать любые сообщения, разрешенные для этого участника. Например, если инициатор диалога может посылать сообщения типа **//Adventure-Works.com/Expenses/SubmitExpense**, [!INCLUDE[ssSB](../../includes/sssb-md.md)] позволяет инициатору посылать произвольное количество сообщений **SubmitExpense** в течение диалога.  
  
 Типы и направление сообщений в контракте не могут быть изменены. Чтобы изменить параметр AUTHORIZATION для контракта, следует воспользоваться инструкцией ALTER AUTHORIZATION.  
  
 Необходимо, чтобы контракт позволял инициатору посылать сообщения. Инструкция CREATE CONTRACT завершается сбоем, если в контракте не содержится ни одного сообщения типа SENT BY ANY или SENT BY INITIATOR.  
  
 Независимо от контракта служба всегда может принимать сообщения типов `https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer`, `https://schemas.microsoft.com/SQL/ServiceBroker/Error` и `https://schemas.microsoft.com/SQL/ServiceBroker/EndDialog`. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] использует эти типы сообщений в системных сообщениях для приложения.  
  
 Контракт не может быть временным объектом. Контракты могут иметь имена, начинающиеся с символа #, но являются постоянными объектами.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию создавать контракты могут члены предопределенных ролей базы данных **db_ddladmin** и **db_owner** и предопределенной роли сервера **sysadmin**.  
  
 По умолчанию разрешением REFERENCES для контракта обладают владелец контракта, члены предопределенных ролей базы данных **db_ddladmin** и **db_owner** и члены предопределенной роли сервера **sysadmin**.  
  
 Пользователь, выполняющий инструкцию CREATE CONTRACT, должен обладать разрешением REFERENCES на все указанные типы сообщений.  
  
## <a name="examples"></a>Примеры  
 **A. Создание контракта**  
  
 В следующем примере создается контракт компенсации расходов, основанный на трех типах сообщений.  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]           
    VALIDATION = WELL_FORMED_XML ;           
  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/ExpenseApprovedOrDenied]           
    VALIDATION = WELL_FORMED_XML ;           
  
CREATE MESSAGE TYPE           
    [//Adventure-Works.com/Expenses/ExpenseReimbursed]           
    VALIDATION= WELL_FORMED_XML ;           
  
CREATE CONTRACT            
    [//Adventure-Works.com/Expenses/ExpenseSubmission]           
    ( [//Adventure-Works.com/Expenses/SubmitExpense]           
          SENT BY INITIATOR,           
      [//Adventure-Works.com/Expenses/ExpenseApprovedOrDenied]           
          SENT BY TARGET,           
      [//Adventure-Works.com/Expenses/ExpenseReimbursed]           
          SENT BY TARGET           
    ) ;  
```  
  
## <a name="see-also"></a>См. также  
 [DROP CONTRACT (Transact-SQL)](../../t-sql/statements/drop-contract-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
