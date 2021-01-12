---
description: END CONVERSATION (Transact-SQL)
title: END CONVERSATION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- END DIALOG
- END CONVERSATION
- END_DIALOG_TSQL
- END_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [Service Broker], conversations
- dialogs [Service Broker], ending
- closing conversations
- END CONVERSATION statement
- conversations [Service Broker], ending
- ending conversations [SQL Server]
ms.assetid: 4415a126-cd22-4a5e-b84a-d8c68515c83b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 16dd2a5201d001a54103408386036cee39040523
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093456"
---
# <a name="end-conversation-transact-sql"></a>END CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Завершает одну из сторон существующего диалога.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
END CONVERSATION conversation_handle  
   [   [ WITH ERROR = failure_code DESCRIPTION = 'failure_text' ]  
     | [ WITH CLEANUP ]  
    ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *conversation_handle*  
 Дескриптор завершаемого диалога.  
  
 WITH ERROR =*код_ошибки*  
 Код ошибки. Параметр *код_ошибки* имеет тип **int**. Код ошибки — это определяемое пользователем число, которое включается в сообщение об ошибке, отсылаемое другой стороне диалога. Код ошибки должен быть больше 0.  
  
 DESCRIPTION =*описание_ошибки*  
 Сообщение об ошибке. Параметр *описание_ошибки* имеет тип **nvarchar(3000)**. Текст ошибки — это определяемый пользователем текст, который включается в сообщение об ошибке, отсылаемое другой стороне диалога.  
  
 WITH CLEANUP  
 Удаляет все сообщения и записи представлений каталога для одной стороны диалога, которая не может завершиться нормально. Другая сторона диалога не уведомляется об очистке. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] удаляет конечную точку диалога, все сообщения диалога в очереди передачи и все сообщения диалога в очереди обслуживания. Администраторы могут использовать этот параметр для удаления диалогов, которые не могут быть нормально завершены. Например, если удаленная служба безвозвратно удалена, администратор может использовать параметр WITH CLEANUP для удаления диалогов, работающих с этой службой. Не следует использовать параметр WITH CLEANUP в коде приложения [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Если инструкция END CONVERSATION WITH CLEANUP будет запущена до того, как получающая конечная точка подтвердит получение сообщения, то отправляющая конечная точка пошлет сообщение повторно. Потенциально это может вызвать повторный запуск диалога.  
  
## <a name="remarks"></a>Комментарии  
 При завершении диалога блокируется группа сообщений, к которой относится указанный аргумент *дескриптор_диалога*. При завершении диалога компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] удаляет из очереди обслуживания все сообщения, относящиеся к этому диалогу.  
  
 После того как диалог завершен, приложение больше не может отправлять и получать сообщения. Оба участника диалога должны вызвать инструкцию END CONVERSATION для его завершения. Если компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] не получил от второго участника ни сообщения об окончании диалога, ни сообщения об ошибке, то компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] уведомляет его о том, что диалог завершен. В этом случае, несмотря на то, что дескриптор диалога уже недействителен, конечная точка диалога остается активной до тех пор, пока экземпляр, на котором находится служба, не подтвердит сообщение.  
  
 Если компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] еще не обработал сообщение об окончании диалога или ошибке, то компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] уведомляет удаленного участника, что диалог завершен. Какие именно сообщения компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] отправляет удаленной службе, зависит от указанных параметров:  
  
-   Если диалог завершается без ошибок и диалог с удаленной службой еще активен, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] отправляет ей сообщение типа `https://schemas.microsoft.com/SQL/ServiceBroker/EndDialog`. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] добавляет это сообщение в очередь передачи в порядке создания диалогов. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] отправляет все сообщения для этого диалога, уже находящиеся в очереди передачи, прежде чем отправить это сообщение.  
  
-   Если диалог завершается с ошибкой и диалог с удаленной службой еще активен, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] отправляет ей сообщение типа `https://schemas.microsoft.com/SQL/ServiceBroker/Error`. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] удаляет все сообщения для этого диалога, уже находящиеся в очереди передачи.  
  
-   Предложение WITH CLEANUP позволяет администратору базы данных удалять диалоги, которые не могут быть завершены нормальным способом. Этот параметр удаляет все сообщения и элементы представления каталога для данного диалога. Обратите внимание, что в этом случае удаленная сторона диалога не получает никакого сигнала о том, что диалог завершен, и не сможет получить сообщения, которые были отправлены приложением, но еще не были переданы по сети. Старайтесь не пользоваться этим параметром, если диалог может быть завершен нормально.  
  
 После завершения диалога, инструкция SEND языка [!INCLUDE[tsql](../../includes/tsql-md.md)], в которой указан дескриптор диалога, вызовет ошибку [!INCLUDE[tsql](../../includes/tsql-md.md)]. Если сообщения для данного диалога поступают с противоположной стороны, компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] отбрасывает их.  
  
 Если на момент завершения диалога у удаленной службы остались неотправленные сообщения, они будут удалены. Это не рассматривается как ошибка, и удаленная служба не получит уведомления о том, что сообщения удалены.  
  
 Код ошибки, указанный в предложении WITH ERROR, должен быть положительным числом. Отрицательные числа зарезервированы для сообщений об ошибках компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 Инструкция END CONVERSATION не может применяться в определяемой пользователем функции.  
  
## <a name="permissions"></a>Разрешения  
 Для завершения активного диалога текущий пользователь должен быть его владельцем, членом предопределенной роли сервера sysadmin или предопределенной роли базы данных db_owner.  
  
 Член предопределенной роли сервера sysadmin или предопределенной роли базы данных db_owner может указывать предложение WITH CLEANUP для удаления метаданных диалога, который уже завершен.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-ending-a-conversation"></a>A. Завершение диалога  
 Следующий пример завершает диалог, определяемый дескриптором `@dialog_handle`.  
  
```sql 
END CONVERSATION @dialog_handle ;  
```  
  
### <a name="b-ending-a-conversation-with-an-error"></a>Б. Завершение диалога с ошибкой  
 В следующем примере диалог, определяемый дескриптором `@dialog_handle`, завершается с ошибкой, если обрабатываемая инструкция сообщила об ошибке. Обратите внимание, что этот упрощенный подход к обработке ошибок может не подойти для некоторых приложений.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER,  
        @ErrorSave INT,  
        @ErrorDesc NVARCHAR(100) ;  
BEGIN TRANSACTION ;  
  
<receive and process message>  
  
SET @ErrorSave = @@ERROR ;  
  
IF (@ErrorSave <> 0)  
  BEGIN  
      ROLLBACK TRANSACTION ;  
      SET @ErrorDesc = N'An error has occurred.' ;  
      END CONVERSATION @dialog_handle   
      WITH ERROR = @ErrorSave DESCRIPTION = @ErrorDesc ;  
  END  
ELSE  
  
COMMIT TRANSACTION ;  
```  
  
### <a name="c-cleaning-up-a-conversation-that-cannot-complete-normally"></a>В. Очистка диалога, который не может быть нормально завершен  
 Следующий пример завершает диалог, определяемый дескриптором `@dialog_handle`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] немедленно удаляет все сообщения из очереди обслуживания и очереди передачи, не уведомляя об этом удаленную службу. Поскольку при завершении диалога с очисткой удаленная служба не уведомляется, этот способ следует использовать только в тех случаях, когда удаленная служба недоступна и не может получить сообщение **EndDialog** или **Error**.  
  
```sql  
END CONVERSATION @dialog_handle WITH CLEANUP ;  
```  
  
## <a name="see-also"></a>См. также  
 [BEGIN CONVERSATION TIMER (Transact-SQL)](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
