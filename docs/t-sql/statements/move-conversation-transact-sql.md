---
description: MOVE CONVERSATION (Transact-SQL)
title: MOVE CONVERSATION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- MOVE_CONVERSATION_TSQL
- MOVE CONVERSATION
- MOVE_TSQL
- MOVE
dev_langs:
- TSQL
helpviewer_keywords:
- moving conversations
- MOVE CONVERSATION statement
- relocating conversations
- conversations [Service Broker], groups
- conversations [Service Broker], moving
ms.assetid: 1da4d2c9-e767-434e-b49b-615711a7f626
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a789534b4ff40a7d9f6f360c941bd920ce23c44e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095684"
---
# <a name="move-conversation-transact-sql"></a>MOVE CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Перемещает диалог в другую группу сообщений.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
MOVE CONVERSATION conversation_handle  
   TO conversation_group_id  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *conversation_handle*  
 Переменная или константа, содержащая дескриптор перемещаемого диалога. *conversation_handle* должен иметь тип **uniqueidentifier**.  
  
 TO *conversation_group_id*  
 Переменная или константа, содержащая идентификатор группы сообщений, в которую перемещается диалог. *conversation_group_id* должен иметь тип **uniqueidentifier**.  
  
## <a name="remarks"></a>Комментарии  
 Инструкция MOVE CONVERSATION перемещает диалог, указанный аргументом *conversation_handle*, в группу сообщений, определяемую аргументом *conversation_group_id*. Диалоги могут перенаправляться только между группами диалогов, связанными с одной и той же очередью.  
  
> [!IMPORTANT]  
>  Если инструкция MOVE CONVERSATION не является первой инструкцией в пакете или хранимой процедуре, предыдущая инструкция должна заканчиваться точкой с запятой (**;**) — разделителем инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Инструкция MOVE CONVERSATION блокирует группу сообщений, связанную с диалогом *conversation_handle*, а также группу, заданную аргументом *conversation_group_id*, до тех пор, пока транзакция, содержащая инструкцию, не будет зафиксирована или откачена.  
  
 Инструкция MOVE CONVERSATION недопустима в пользовательских функциях.  
  
## <a name="permissions"></a>Разрешения  
 Для перемещения диалога, текущий пользователь должен быть владельцем диалога и группы сообщений, либо членом предопределенной роли сервера sysadmin или предопределенной роли db_owner базы данных.  
  
## <a name="examples"></a>Примеры  
 В следующем примере диалог перемещается в другую группу сообщений.  
  
```sql  
DECLARE @conversation_handle UNIQUEIDENTIFIER,  
        @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_handle =  
    <retrieve conversation handle from database> ;  
SET @conversation_group_id =  
    <retrieve conversation group ID from database> ;  
  
MOVE CONVERSATION @conversation_handle TO @conversation_group_id ;  
```  
  
## <a name="see-also"></a>См. также  
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [GET CONVERSATION GROUP (Transact-SQL)](../../t-sql/statements/get-conversation-group-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [sys.conversation_groups (Transact-SQ)](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)   
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
