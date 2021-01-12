---
description: DROP ROUTE (Transact-SQL)
title: DROP ROUTE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP ROUTE
- DROP_ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dropping routes
- DROP ROUTE statement
- deleting routes
- routes [Service Broker], removing
- removing routes
ms.assetid: d8fab0bc-d54a-46ca-9437-552db7477d40
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0a699415666b17a53ebbedf596a16ebfe8fae4e7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081193"
---
# <a name="drop-route-transact-sql"></a>DROP ROUTE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет маршрут путем удаления сведений для маршрута из таблицы маршрутизации текущей базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DROP ROUTE route_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *route_name*  
 Имя удаляемого маршрута. Не могут быть указаны имена сервера, базы данных и схемы.  
  
## <a name="remarks"></a>Комментарии  
 Таблица маршрутизации, где хранится таблица метаданных, данные из которой могут быть получены с помощью представления каталога **sys.routes**. Таблица маршрутизации может быть обновлена только с помощью инструкций CREATE ROUTE, ALTER ROUTE и DROP ROUTE.  
  
 Можно удалить маршрут независимо от того, используют ли его какие-либо диалоги. Однако если не существует другого маршрута к удаленной службе, сообщения для этих диалогов останутся в очереди передачи до тех пор, пока не будет создан маршрут к удаленной службе или время ожидания диалога истечет.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешением на удаление значений маршрута по умолчанию обладает владелец маршрута, члены предопределенной роли базы данных ddl_admin или db_owner и члены предопределенной роли сервера sysadmin.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано удаление маршрута `ExpenseRoute`.  
  
```sql  
DROP ROUTE ExpenseRoute ;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER ROUTE (Transact-SQL)](../../t-sql/statements/alter-route-transact-sql.md)   
 [CREATE ROUTE (Transact-SQL)](../../t-sql/statements/create-route-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.routes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-routes-transact-sql.md)  
  
  
