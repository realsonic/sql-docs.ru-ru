---
description: DROP ROLE (Transact-SQL)
title: DROP ROLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP ROLE
- DROP_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting roles
- database roles [SQL Server], removing
- removing roles
- DROP ROLE statement
- roles [SQL Server], removing
- dropping roles
ms.assetid: 1f6f13ae-56a2-4ef1-93f5-8e6151b83e1d
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fae996889f288c0ba4f01efeadcbbc4da138ea46
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172576"
---
# <a name="drop-role-transact-sql"></a>DROP ROLE (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Удаляет роль из базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server  
  
DROP ROLE [ IF EXISTS ] role_name  
```  
  

```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

DROP ROLE role_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Условное удаление роли только в том случае, если она уже существует.  
  
 *role_name*  
 Определяет роль, которую следует удалить из базы данных.  
  
## <a name="remarks"></a>Примечания  
 Роли, владеющие объектами защиты, не могут быть удалены из базы данных. Чтобы удалить из базы данных роль, владеющую объектами защиты, необходимо сначала передать эти объекты другому владельцу или удалить их из базы данных. Роли, владеющие объектами защиты, не могут быть удалены из базы данных. Чтобы удалить роль, имеющую члены, необходимо сначала удалить эти члены из данной роли.  
  
 Чтобы удалить члены из роли базы данных, используйте инструкцию [ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md).  
  
 Удаление предопределенной роли базы данных не может быть осуществлено с помощью инструкции DROP ROLE.  
  
 Сведения о членстве в роли можно просмотреть в представлении каталога sys.database_role_members.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
 Чтобы удалить роль сервера, используйте инструкцию [DROP SERVER ROLE (Transact-SQL)](../../t-sql/statements/drop-server-role-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение **ALTER ANY ROLE** на базу данных, разрешение **CONTROL** на роль или членство в роли **db_securityadmin**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере роль базы данных `purchasing` удаляется из базы данных `AdventureWorks2012`.  
  
```sql  
DROP ROLE purchasing;  
GO  
```  
  
  
## <a name="see-also"></a>См. также  
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Хранимая процедура sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Функции безопасности &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  


