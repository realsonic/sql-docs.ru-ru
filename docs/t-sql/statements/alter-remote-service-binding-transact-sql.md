---
description: ALTER REMOTE SERVICE BINDING (Transact-SQL)
title: ALTER REMOTE SERVICE BINDING (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER REMOTE SERVICE BINDING
- ALTER_REMOTE_SERVICE_BINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote service bindings [Service Broker], modifying
- ALTER REMOTE SERVICE BINDING statement
- modifying remote service bindings
ms.assetid: ee620b4a-9375-4eaa-a016-69916c9e1e68
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c671b412d5d9ce34c34fdc49e16215dafc8ec83e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092131"
---
# <a name="alter-remote-service-binding-transact-sql"></a>ALTER REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет пользователя, связанного с привязкой удаленной службы, или изменяет настройку привязки, допускающую анонимную проверку подлинности.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER REMOTE SERVICE BINDING binding_name   
   WITH [ USER = <user_name> ] [ , ANONYMOUS = { ON | OFF } ]   
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *binding_name*  
 Имя привязки удаленной службы, которую требуется изменить. Не могут быть указаны имена сервера, базы данных и схемы.  
  
 WITH USER = \<*user_name>*  
 Указывает пользователя базы данных, у которого имеется сертификат для этой привязки, связанный с удаленной службой. Открытый ключ из этого сертификата используется для кодирования и проверки подлинности сообщений, обмен которыми производится удаленной службой.  
  
 ANONYMOUS  
 Указывает, используется ли анонимная проверка подлинности при связи с удаленной службой. При настройке ANONYMOUS = ON используется анонимная проверка подлинности и учетные данные локального пользователя не передаются удаленной службе. При настройке ANONYMOUS = OFF передаются учетные данные пользователя. Если это выражение не задано, по умолчанию параметр принимает значение OFF.  
  
## <a name="remarks"></a>Remarks  
 Открытый ключ в сертификате, связанный с пользователем *user_name*, используется для проверки подлинности сообщений, отправляемых в удаленную службу, и для шифрования сеансового ключа, который затем используется для шифрования сеанса связи. Сертификат для пользователя *user_name* должен соответствовать сертификату для имени входа в базе данных, в которой находится удаленная служба.  
  
## <a name="permissions"></a>Разрешения  
 Разрешение на изменение привязки удаленной службы имеется по умолчанию у владельца привязки удаленной службы, членов предопределенной роли базы данных **db_owner**, а также членов предопределенной роли сервера **sysadmin**.  
  
 У пользователя, вызывающего инструкцию ALTER REMOTE SERVICE BINDING, должно быть разрешение на олицетворение пользователя, указанного в выражении.  
  
 Чтобы изменить параметр AUTHORIZATION для привязки удаленной службы, используйте инструкцию ALTER AUTHORIZATION.  
  
## <a name="examples"></a>Примеры  
 В следующем примере привязка `APBinding` удаленной службы изменяется таким образом, чтобы шифровать сообщения с помощью сертификатов учетной записи `SecurityAccount`.  
  
```sql  
ALTER REMOTE SERVICE BINDING APBinding  
    WITH USER = SecurityAccount ;  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
