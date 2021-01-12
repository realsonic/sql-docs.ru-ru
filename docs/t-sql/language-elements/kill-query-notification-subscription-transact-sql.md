---
title: KILL QUERY NOTIFICATION SUBSCRIPTION
description: Удаляет подписки на уведомления о запросах из экземпляра. С помощью этой инструкции можно удалить определенную подписку или все подписки.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL QUERY NOTIFICATION SUBSCRIPTION
- KILL_QUERY_NOTIFICATION_SUBSCRIPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- KILL QUERY NOTIFICATION SUBSCRIPTION statement
- removing subscriptions
- subscriptions [SQL Server query notifications], stopping
- query notifications [SQL Server], subscriptions
ms.assetid: 8aeadf51-286c-4748-bef2-d25858b250bf
author: cawrites
ms.author: chadam
ms.openlocfilehash: f9ba5c75e44fcedd0f99e051b94812dbd3d94e5e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98084892"
---
# <a name="kill-query-notification-subscription-transact-sql"></a>KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет подписки на уведомления о запросах из экземпляра. С помощью этой инструкции можно удалить определенную подписку или все подписки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
KILL QUERY NOTIFICATION SUBSCRIPTION   
   { ALL | subscription_id }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 ALL  
 Удаляет все подписки экземпляра.  
  
 *subscription_id*  
 Удаляет подписку с идентификатором *subscription_id*.  
  
## <a name="remarks"></a>Remarks  
 Инструкция KILL QUERY NOTIFICATION SUBSCRIPTION удаляет подписки на уведомления о запросах без выдачи сообщения-уведомления.  
  
 Аргумент *subscription_id* представляет собой идентификатор подписки, отображаемый в динамическом административном представлении [sys.dm_qn_subscriptions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/query-notifications-sys-dm-qn-subscriptions.md).  
  
 Если указанного идентификатора подписки не существует, инструкция возвращает ошибку.  
  
## <a name="permissions"></a>Разрешения  
 Разрешение на выполнение этой инструкции ограничено членами предопределенной роли сервера **sysadmin**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-removing-all-query-notification-subscriptions-in-the-instance"></a>A. Удаление всех подписок на уведомления о запросах из экземпляра  
 В следующем с экземпляра удаляются все подписки на уведомления о запросах.  
  
```sql  
KILL QUERY NOTIFICATION SUBSCRIPTION ALL ;  
```  
  
### <a name="b-removing-a-single-query-notification-subscription"></a>Б. Удаление отдельной подписки на уведомления о запросах  
 В следующем примере удаляется подписка на уведомления о запросах с идентификатором `73`.  
  
```sql  
KILL QUERY NOTIFICATION SUBSCRIPTION 73 ;  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_qn_subscriptions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/query-notifications-sys-dm-qn-subscriptions.md)  
  
  
