---
description: DROP USER (Transact-SQL)
title: DROP USER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_USER_TSQL
- DROP USER
dev_langs:
- TSQL
helpviewer_keywords:
- dropping users
- DROP USER statement
- deleting users
- database user removal [SQL Server]
- removing users
- users [SQL Server], removing
ms.assetid: d6e0e21a-7568-4321-b6d6-bcfba183a719
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 874285a9275484239394d4b091b72f06e234a8a2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170416"
---
# <a name="drop-user-transact-sql"></a>DROP USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет пользователя из текущей базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DROP USER [ IF EXISTS ] user_name  
```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP USER user_name  
```  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658), [!INCLUDE[sssds](../../includes/sssds-md.md)]).  
  
 Условное удаление пользователя только в том случае, если он уже существует.  
  
 *user_name*  
 Указывает имя, по которому пользователь идентифицируется в этой базе данных.  
  
## <a name="remarks"></a>Комментарии  
 Пользователи, которые владеют защищаемыми объектами, не могут быть удалены из базы данных. Перед удалением пользователя, который владеет защищаемым объектом, необходимо удалить или сменить владельца защищаемого объекта.  
  
 Пользователя «Гость» нельзя удалить, однако его можно отключить, отменив разрешение CONNECT путем выполнения инструкции REVOKE CONNECT FROM GUEST в любой базе данных, кроме master или tempdb.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER ANY USER для базы данных.  
  
## <a name="examples"></a>Примеры  
 Следующий пример удаляет пользователя базы данных `AbolrousHazem` из базы данных `AdventureWorks2012`.  
  
```sql  
DROP USER AbolrousHazem;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [ALTER USER (Transact-SQL)](../../t-sql/statements/alter-user-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
