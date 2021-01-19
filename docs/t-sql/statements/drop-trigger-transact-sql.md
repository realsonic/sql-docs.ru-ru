---
description: DROP TRIGGER (Transact-SQL)
title: DROP TRIGGER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP TRIGGER
- DROP_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- renaming triggers
- triggers [SQL Server], removing
- DDL triggers, removing
- DROP TRIGGER statement
- deleting triggers
- dropping triggers
- removing triggers
- DML triggers, removing
ms.assetid: 092d0d71-9f1e-4e38-a1c4-2487adfa5b4e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: de35e51632ef898e89e8ab6d036d58243ba03f4f
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172086"
---
# <a name="drop-trigger-transact-sql"></a>DROP TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Удаляет один или более триггеров DML или DDL из текущей базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
DROP TRIGGER [ IF EXISTS ] [schema_name.]trigger_name [ ,...n ] [ ; ]  
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE or UPDATE statement (DDL Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON { DATABASE | ALL SERVER }   
[ ; ]  
  
-- Trigger on a LOGON event (Logon Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON ALL SERVER  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658), [!INCLUDE[sssds](../../includes/sssds-md.md)]).  
  
 Условное удаление триггера только в том случае, если он уже существует.  
  
 *schema_name*  
 Имя схемы, которой принадлежит триггер DML. Действие триггеров DML ограничивается областью схемы таблицы или представления, для которых они созданы. Аргумент *schema_name* не может указываться для триггеров DDL или триггеров входа.  
  
 *trigger_name*  
 Имя удаляемого триггера. Чтобы просмотреть список только что созданных триггеров, см. [sys.server_assembly_modules](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) или [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md).  
  
 DATABASE  
 Обозначает область действия триггера DDL в текущей базе данных. Если при создании или изменении триггера был указан аргумент DATABASE, то он должен указываться и далее.  
  
 ALL SERVER  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Обозначает область действия триггера DDL на текущем сервере. Если при создании или изменении триггера был указан аргумент ALL SERVER, то он должен указываться и далее. Параметр ALL SERVER также применяется к триггерам входа.  
  
> [!NOTE]  
>  Этот параметр недоступен в автономной базе данных.  
  
## <a name="remarks"></a>Remarks  
 Триггер DML может быть удален напрямую или в результате удаления таблицы триггера. При удалении таблицы удаляются все связанные с ней триггеры.  
  
 При удалении триггера соответствующие данные в представлениях каталогов **sys.objects**, **sys.triggers** и **sys.sql_modules** также удаляются.  
  
 С помощью инструкции DROP TRIGGER сразу несколько триггеров DDL можно удалить только в том случае, если при их создании были использованы одинаковые предложения ON.  
  
 Чтобы переименовать триггер, используйте инструкции DROP TRIGGER и CREATE TRIGGER. Чтобы изменить определение триггера, используйте инструкцию ALTER TRIGGER.  
  
 Дополнительные сведения об определении зависимостей для конкретных триггеров см. в разделах [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md), [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) и [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).  
  
 Дополнительные сведения о просмотре текста триггеров см. в разделах [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md) и [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
 Дополнительные сведения о просмотре списка существующих триггеров см. в разделах [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) и [sys.server_triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Чтобы удалить триггер DML, необходимо разрешение ALTER для таблицы или представления, в которых определен данный триггер.  
  
 Чтобы удалить триггер входа или триггер DDL, определенный в области сервера (ON ALL SERVER), для этого сервера требуется разрешение CONTROL SERVER. Чтобы удалить триггер DDL, определенный в области базы данных (ON DATABASE), необходимо разрешение ALTER ANY DATABASE DDL TRIGGER для текущей базы данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-dropping-a-dml-trigger"></a>A. Удаление триггера DML  
 В следующем примере показано удаление триггера `employee_insupd` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. (Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] можно использовать синтаксис DROP TRIGGER IF EXISTS.)  
  
```sql  
IF OBJECT_ID ('employee_insupd', 'TR') IS NOT NULL  
   DROP TRIGGER employee_insupd;  
```  
  
### <a name="b-dropping-a-ddl-trigger"></a>Б. Удаление триггера DDL  
 В ходе выполнения следующего примера происходит удаление триггера DDL `safety`.  
  
> [!IMPORTANT]  
>  Функция OBJECT_ID не может быть использована для выяснения факта существования в базе данных триггеров DDL, так как они не относятся к области схемы и данные о них не заносятся в каталог **sys.objects**. Запросы на объекты, не относящиеся к области схемы, должны выполняться при помощи соответствующих представлений каталогов. Для триггеров DDL используйте **sys.triggers**.  
  
```sql  
DROP TRIGGER safety  
ON DATABASE;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [ENABLE TRIGGER (Transact-SQL)](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Получение сведений о триггерах DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  
  
  
