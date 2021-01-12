---
description: ALTER SERVICE (Transact-SQL)
title: ALTER SERVICE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SERVICE
- ALTER_SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying services
- contracts [Service Broker], modifying
- ALTER SERVICE statement
- services [Service Broker], modifying
ms.assetid: 2b4608f7-bb2e-4246-aa29-b52c55995b3a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 746e1aa7215acd584e6512c3e39fb79014b7821b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100270"
---
# <a name="alter-service-transact-sql"></a>ALTER SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Изменяет существующую службу.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql 
ALTER SERVICE service_name   
   [ ON QUEUE [ schema_name . ]queue_name ]   
   [ ( < opt_arg > [ , ...n ] ) ]  
[ ; ]  
  
<opt_arg> ::=  
   ADD CONTRACT contract_name | DROP CONTRACT contract_name  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *service_name*  
 Имя службы, которую необходимо изменить. Не могут быть указаны имена сервера, базы данных и схемы.  
  
 ON QUEUE [ _schema_name_**.** ] *queue_name*  
 Указывает новую очередь для этой службы. [!INCLUDE[ssSB](../../includes/sssb-md.md)] перемещает все сообщения этой службы из текущей очереди в новую очередь.  
  
 ADD CONTRACT *contract_name*  
 Указывает контракт, добавляемый к набору контрактов, предоставляемому этой службой.  
  
 DROP CONTRACT *contract_name*  
 Указывает контракт, удаляемый из набора контрактов, предоставляемых этой службе. [!INCLUDE[ssSB](../../includes/sssb-md.md)] отправляет сообщение об ошибке всем существующим диалогам с этой службой, использующим этот контракт.  
  
## <a name="remarks"></a>Remarks  
 Когда инструкция ALTER SERVICE удаляет контракт из службы, служба перестает быть возможной целью диалогов, использующих этот контракт. Поэтому компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] не допускает начало новых диалогов с этой службой по данному контракту. На уже существующие диалоги, использующие этот контракт, изменение не влияет.  
  
 Чтобы изменить свойство AUTHORIZATION для службы, используйте инструкцию ALTER AUTHORIZATION.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешения на изменение службы имеют владелец службы, члены предопределенных ролей базы данных **db_ddladmin** и **db_owner** и члены предопределенной роли сервера **sysadmin**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-changing-the-queue-for-a-service"></a>A. Изменение очереди для службы  
 В результате выполнения следующего примера служба `//Adventure-Works.com/Expenses` будет использовать очередь `NewQueue`.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE NewQueue ;  
```  
  
### <a name="b-adding-a-new-contract-to-the-service"></a>Б. Добавление нового контракта к службе  
 В результате выполнения следующего примера службе `//Adventure-Works.com/Expenses` будет разрешено устанавливать диалоги по контракту `//Adventure-Works.com/Expenses`.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="c-adding-a-new-contract-to-the-service-dropping-existing-contract"></a>В. Добавление нового контракта к службе, удаление существующего контракта  
 В результате выполнения следующего примера службе `//Adventure-Works.com/Expenses` будут разрешено устанавливать диалоги по контракту `//Adventure-Works.com/Expenses/ExpenseProcessing` и запрещено по контракту `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseProcessing],   
     DROP CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE SERVICE (Transact-SQL)](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP SERVICE (Transact-SQL)](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
