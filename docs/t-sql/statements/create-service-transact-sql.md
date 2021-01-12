---
description: CREATE SERVICE (Transact-SQL)
title: CREATE SERVICE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SERVICE_TSQL
- SERVICE
- CREATE SERVICE
- SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- services [Service Broker], creating
- CREATE SERVICE statement
- contracts [Service Broker], service creation
ms.assetid: fb804fa2-48eb-4878-a12f-4e0d5f4bc9e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f4c3d54c74d21994974ea1cdf47dbea07d8171ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097013"
---
# <a name="create-service-transact-sql"></a>CREATE SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Создает новую службу. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] — это имя конкретной задачи или набора бизнес-задач. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] использует имя службы для маршрутизации сообщений, доставки сообщений в нужную очередь в базе данных и принудительного соблюдения контракта для диалога.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE SERVICE service_name  
   [ AUTHORIZATION owner_name ]  
   ON QUEUE [ schema_name. ]queue_name  
   [ ( contract_name | [DEFAULT][ ,...n ] ) ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *service_name*  
 Имя создаваемой службы. Новая служба создается в текущей базе данных. Владельцем службы назначается участник, указанный в предложении AUTHORIZATION. Не могут быть указаны имена сервера, базы данных и схемы. Аргумент *service_name* должен быть допустимым аргументом **sysname**.  
  
> [!NOTE]  
> Не создавайте службу, которая использует ключевое слово ANY для *service_name*. При указании ключевого слова `ANY` для имени службы в инструкции `CREATE BROKER PRIORITY` приоритет применяется ко всем службам. Его применение не ограничивается службой с именем ANY.  
  
 AUTHORIZATION *owner_name*  
 Определяет в качестве владельца службы указанного пользователя или роль базы данных. Если текущим пользователем является **dbo** или **sa**, то аргумент *owner_name* может быть именем любого допустимого пользователя или роли. В противном случае аргумент *owner_name* должен быть именем текущего пользователя, именем пользователя, для которого у текущего пользователя есть разрешение IMPERSONATE, или именем роли, которой принадлежит текущий пользователь.  
  
 ON QUEUE [ _schema_name_**.** ] *queue_name*  
 Указывает очередь, в которую поступают сообщения для службы. Очередь должна существовать в той же самой базе данных, что и служба. Если имя схемы *schema_name* не указано, используется схема по умолчанию пользователя, выполняющего инструкцию.  
  
 *contract_name*  
 Указывает контракт, для которого данная служба может быть целью. Служебные программы инициируют диалог с данной службой с помощью указанных контрактов. Если контракты не указаны, инициировать диалог может только служба.  
  
 **[** DEFAULT **]**  
 Указывает, что служба может быть целью для диалогов, которые следуют контракту DEFAULT. В контексте данного предложения слово DEFAULT не является ключевым словом и должно быть отделено как идентификатор. Контракт DEFAULT разрешает обеим сторонам диалога отправлять сообщения с типом сообщения DEFAULT. Тип сообщений DEFAULT не использует проверку.  
  
## <a name="remarks"></a>Комментарии  
 Служба раскрывает функциональность, обеспечиваемую контрактами, с которыми она связана, так что они могут быть использованы другими службами. Инструкция `CREATE SERVICE` указывает контракты, для которых эта служба является целью. Службы могут быть целью только для диалогов, которые используют контракты, указанные службой. Служба, для которой не указаны контракты, не раскрывает функциональность другим службам.  
  
 Диалоги, инициированные такой службой, могут использовать любой контракт. Если служба только инициирует диалоги, ее можно создавать без определения контрактов.  
  
 Когда компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] принимает новый диалог от удаленной службы, имя целевой службы определяет очередь, в которую брокер размещает сообщения диалога.  
  
## <a name="permissions"></a>Разрешения  
 Разрешение на создание значений по умолчанию для службы имеют члены предопределенных ролей базы данных `db_ddladmin` и `db_owner` или предопределенной роли сервера `sysadmin`. Пользователь, выполняющий инструкцию `CREATE SERVICE`, должен иметь разрешение `REFERENCES` на очередь и все указанные контракты.  
  
 По умолчанию разрешением `REFERENCES` на значения по умолчанию для службы обладает владелец службы, члены предопределенной роли базы данных `db_ddladmin` или `db_owner` и члены предопределенной роли сервера `sysadmin`. По умолчанию разрешением `SEND` на значения по умолчанию для службы обладает владелец службы, члены предопределенной роли базы данных `db_owner` и члены предопределенной роли сервера `sysadmin`.  
  
 Служба не может быть временным объектом. Имена служб, начинающиеся с **#** , допустимы, но являются постоянными объектами.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-service-with-one-contract"></a>A. Создание службы с одним контрактом  
 В следующем примере создается служба `//Adventure-Works.com/Expenses` в очереди `ExpenseQueue` в схеме `dbo`. Диалоги, которые задают данную службу, должны следовать контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE [dbo].[ExpenseQueue]  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="b-creating-a-service-with-multiple-contracts"></a>Б. Создание службы с несколькими контрактами  
 В следующем примере создается служба `//Adventure-Works.com/Expenses` в очереди `ExpenseQueue`. Диалоги, которые задают данную службу, должны следовать контракту `//Adventure-Works.com/Expenses/ExpenseSubmission` или `//Adventure-Works.com/Expenses/ExpenseProcessing`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission],  
     [//Adventure-Works.com/Expenses/ExpenseProcessing]) ;  
```  
  
### <a name="c-creating-a-service-with-no-contracts"></a>В. Создание службы без контрактов  
 В следующем примере создается очередь `//Adventure-Works.com/Expenses on the ExpenseQueue` службы. Данная служба не имеет сведений о контракте. Поэтому она может быть только инициатором диалога.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue ;  
```  
  
## <a name="see-also"></a>См. также  
 [ALTER SERVICE (Transact-SQL)](../../t-sql/statements/alter-service-transact-sql.md)   
 [DROP SERVICE (Transact-SQL)](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
