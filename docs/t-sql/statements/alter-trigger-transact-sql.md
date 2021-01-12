---
description: ALTER TRIGGER (Transact-SQL)
title: ALTER TRIGGER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER TRIGGER
- ALTER_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DDL triggers, modifying
- triggers [SQL Server], modifying
- modifying triggers
- ALTER TRIGGER statement
- DML triggers, modifying
ms.assetid: 2a99c7c1-ac2f-4637-aa7c-3d1bf514e500
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 63cb1d939fc4c46c2382c369c66d79764f2d6211
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098469"
---
# <a name="alter-trigger-transact-sql"></a>ALTER TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Изменяет определение триггера DML, DDL или триггера входа, созданного ранее инструкцией CREATE TRIGGER. Триггеры создаются при помощи инструкции CREATE TRIGGER. Они могут быть созданы непосредственно из инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] или методов сборок, созданных в среде CLR платформы [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], и переданы на экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения о параметрах, используемых в инструкции ALTER TRIGGER, см. в разделе [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  

ALTER TRIGGER schema_name.trigger_name   
ON  ( table | view )   
[ WITH <dml_trigger_option> [ ,...n ] ]  
 ( FOR | AFTER | INSTEAD OF )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
[ NOT FOR REPLICATION ]   
AS { sql_statement [ ; ] [ ...n ] | EXTERNAL NAME <method specifier>   
[ ; ] }   
  
<dml_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ <EXECUTE AS Clause> ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table 
-- (DML Trigger on memory-optimized tables)  

ALTER TRIGGER schema_name.trigger_name   
ON  ( table  )   
[ WITH <dml_trigger_option> [ ,...n ] ]  
 ( FOR | AFTER )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
AS { sql_statement [ ; ] [ ...n ] }   
  
<dml_trigger_option> ::=  
    [ NATIVE_COMPILATION ]  
    [ SCHEMABINDING ]  
    [ <EXECUTE AS Clause> ]  
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE, 
-- or UPDATE statement (DDL Trigger)  
  
ALTER TRIGGER trigger_name   
ON { DATABASE | ALL SERVER }   
[ WITH <ddl_trigger_option> [ ,...n ] ]  
{ FOR | AFTER } { event_type [ ,...n ] | event_group }   
AS { sql_statement [ ; ] | EXTERNAL NAME <method specifier>   
[ ; ] }  
}   
  
<ddl_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ <EXECUTE AS Clause> ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
-- Trigger on a LOGON event (Logon Trigger)  

ALTER TRIGGER trigger_name   
ON ALL SERVER   
[ WITH <logon_trigger_option> [ ,...n ] ]  
{ FOR| AFTER } LOGON   
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  
  [ ; ] }  
  
<logon_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
```  
  
```syntaxsql
-- Azure SQL Database Syntax   
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)   
  
ALTER TRIGGER schema_name. trigger_name   
ON (table | view )   
 [ WITH <dml_trigger_option> [ ,...n ] ]   
 ( FOR | AFTER | INSTEAD OF )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
AS { sql_statement [ ; ] [...n ] }   
  
<dml_trigger_option> ::=   
    [ <EXECUTE AS Clause> ]   
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE, or UPDATE statement (DDL Trigger)   
  
ALTER TRIGGER trigger_name   
ON { DATABASE }   
 [ WITH <ddl_trigger_option> [ ,...n ] ]   
{ FOR | AFTER } { event_type [ ,...n ] | event_group }   
AS { sql_statement   
[ ; ] }  
}   
  
<ddl_trigger_option> ::=   
    [ <EXECUTE AS Clause> ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *schema_name*  
 Имя схемы, которой принадлежит триггер DML. Действие триггеров DML ограничивается областью схемы таблицы или представления, для которых они созданы. Аргумент *schema**_name* необязателен, только если триггер DML и соответствующая таблица или представление принадлежат схеме по умолчанию. Аргумент *schema_name* не может указываться для триггеров DDL или триггеров входа.  
  
 *trigger_name*  
 Существующий триггер, подлежащий изменению.  
  
 *table* | *view*  
 Таблица или представление, в котором выполняется триггер DML. Указывать полное имя таблицы или представления необязательно.  
  
 DATABASE  
 Применяет область действия триггера DDL к текущей базе данных. Если этот аргумент определен, триггер срабатывает всякий раз при возникновении в базе данных события типа *event_type* или *event_group*.  
  
 ALL SERVER  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Применяет область действия триггера DDL или триггера входа к текущему серверу. Если этот аргумент определен, триггер срабатывает всякий раз при возникновении на текущем сервере события типа *event_type* или *event_group*.  
  
 WITH ENCRYPTION  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Шифрует записи sys.syscomments sys.sql_modules, содержащие текст инструкции ALTER TRIGGER. Использование параметра WITH ENCRYPTION не позволяет публиковать триггер как часть репликации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Параметр WITH ENCRYPTION не может быть указан для триггеров CLR.  
  
> [!NOTE]  
>  Чтобы данный параметр остался активным, триггер, созданный с использованием инструкции WITH ENCRYPTION, должен быть определен повторно в инструкции ALTER TRIGGER.  
  
 EXECUTE AS  
 Указывает контекст безопасности, в котором выполняется триггер. Позволяет управлять учетной записью пользователя, которая используется экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для проверки разрешений на любые объекты базы данных, на которые ссылается триггер.  
  
 Дополнительные сведения см. в разделе [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
 NATIVE_COMPILATION  
 Указывает, что триггер компилируется в собственном коде.  
  
 Этот параметр является обязательным для триггеров в таблицах, оптимизированных для памяти.  
  
 SCHEMABINDING  
 Гарантирует, что таблицы, на которые ссылается триггер, нельзя удалить или изменить.  
  
 Этот параметр является обязательным для триггеров в таблицах, оптимизированных для памяти, и не поддерживается для триггеров в обычных таблицах.  
  
 AFTER  
 Указывает, что этот триггер запускается только после того, как запускающая его инструкция SQL успешно выполнена. Все ссылочные каскадные действия и проверки ограничений также должны быть успешно выполнены перед запуском триггера.  
  
 AFTER используется по умолчанию, если указано только ключевое слово FOR.  
  
 Триггеры DML AFTER могут определяться только для таблиц.  
  
 INSTEAD OF  
 Указывает, что триггер DML выполняется вместо запускающей инструкции SQL, переопределяя таким образом действия запускающих инструкций. Аргумент INSTEAD OF не может быть указан для триггеров DDL или триггеров входа.  
  
 На каждую инструкцию INSERT, UPDATE или DELETE в таблице или представлении может быть определено не более одного триггера INSTEAD OF. Однако можно определить представления на представлениях, где у каждого представления есть собственный триггер INSTEAD OF.  
  
 Использование триггеров INSTEAD OF не допускается в представлениях, созданных с использованием параметра WITH CHECK OPTION. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вызывает ошибку, если триггер INSTEAD OF добавляется к представлению с параметром WITH CHECK OPTION. Пользователь должен удалить этот параметр с помощью инструкции ALTER VIEW, прежде чем определить триггер INSTEAD OF.  
  
 { [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] } | { [INSERT ] [ , ] [ UPDATE ] }  
 Задает инструкции изменения данных при применении к таблице или представлению, активирует триггер DML. Необходимо указать как минимум одну инструкцию. В определении триггера разрешается любое сочетание этих параметров в любом порядке. Если указано больше одного параметра, параметры следует разделить запятыми.  
  
 Для триггеров INSTEAD OF параметр DELETE не разрешен в таблицах, имеющих ссылочную связь с указанием каскадного действия ON DELETE. Аналогично, параметр UPDATE не разрешен в таблицах, у которых есть ссылочная связь с указанием каскадного действия ON UPDATE. Дополнительные сведения см. в разделе [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md).  
  
 *event_type*  
 Имя языкового события [!INCLUDE[tsql](../../includes/tsql-md.md)], которое после выполнения вызывает срабатывание триггера DDL. Список событий, которые могут быть использованы в триггерах DDL, приведен в разделе [DDL-события](../../relational-databases/triggers/ddl-events.md).  
  
 *event_group*  
 Имя стандартной группы событий языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Триггер DDL срабатывает после возникновения любого события языка [!INCLUDE[tsql](../../includes/tsql-md.md)], принадлежащего к группе *event_group*. Список групп событий, которые могут быть использованы в триггерах DDL, приведен в разделе [Группы DDL-событий](../../relational-databases/triggers/ddl-event-groups.md). После завершения инструкции ALTER TRIGGER *event_group* также функционирует в качестве макроса, добавляя события соответствующих типов в представление каталога sys.trigger_events.  
  
 NOT FOR REPLICATION  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает, что триггер не должен запускаться, когда агент репликации изменяет таблицу, связанную с триггером.  
  
 *sql_statement*  
 Условия и действия триггера.  
  
 Для триггеров в таблицах, оптимизированных для памяти, единственной инструкцией *sql_statement*, разрешенной на верхнем уровне, является блок ATOMIC. В блоке ATOMIC допускается только T-SQL, разрешенный в процедурах, компилируемых в собственном коде.  
  
 EXTERNAL NAME \<method_specifier>  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает метод сборки для привязки к триггеру. Этот метод не должен принимать аргументы и возвращать значения void. Аргумент *class_name* должен быть допустимым идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и существовать как класс в сборке с видимостью сборки. Класс не может быть вложенным.  
  
## <a name="remarks"></a>Remarks  
 Дополнительные сведения об ALTER TRIGGER см. в подразделе "Замечания" раздела [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md).  
  
> [!NOTE]  
>  Параметры EXTERNAL_NAME и ON_ALL_SERVER недоступны в автономной базе данных.  
  
## <a name="dml-triggers"></a>Триггеры DML  
 Инструкция ALTER TRIGGER поддерживает обновляемые вручную представления через триггеры INSTEAD OF для таблиц и представлений. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] применяет ALTER TRIGGER аналогично для всех видов триггеров (AFTER, INSTEAD-OF).  
  
 Первые и последние триггеры AFTER, которые будут выполнены в таблице, могут быть определены с использованием процедуры sp_settriggerorder. Для таблицы могут указываться только первый и последний триггеры AFTER. Если в таблице есть другие триггеры AFTER, они будут выполняться случайным образом.  
  
 Если инструкция ALTER TRIGGER меняет первый или последний триггер, первый или последний набор атрибутов измененного триггера удаляется, а порядок сортировки должен быть установлен заново с помощью процедуры sp_settriggerorder.  
  
 Триггер AFTER выполняется только после того, как вызывающая срабатывание триггера инструкция SQL была успешно выполнена. Успешное выполнение также подразумевает завершение всех ссылочных каскадных действий и проверки ограничений, связанных с измененными или удаленными объектами. Операция триггера AFTER проверяет влияние запускающей триггер инструкции, а также всех ссылочных каскадных действий UPDATE и DELETE, которые вызваны запускающей инструкцией.  
  
 Если действие DELETE по отношению к потомку или ссылающейся таблице является результатом действия CASCADE для инструкции DELETE из родительской таблицы, а для этой дочерней таблицы определен триггер INSTEAD OF для DELETE, то триггер не учитывается и выполняется действие DELETE.  
  
## <a name="ddl-triggers"></a>Триггеры DDL  
 В отличие от триггеров DML, триггеры DDL не ограничены областью схемы. Поэтому OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY и OBJECTPROPERTY(EX) не могут использоваться при запросах к метаданным о триггерах DDL. Используйте вместо них представления каталога. Дополнительные сведения см. в статье [Получение сведений о триггерах DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md).  
  
## <a name="logon-triggers"></a>Триггеры входа  
 В [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не поддерживаются триггеры на событиях имен входа.  
  
## <a name="permissions"></a>Разрешения  
 Для изменения триггера DML требуется разрешение ALTER в таблице или представлении, в котором определен триггер.  
  
 Чтобы изменить триггер входа или триггер DDL, определенный в области сервера (ON ALL SERVER), требуется разрешение CONTROL SERVER на этом сервере. Чтобы изменить триггер DDL, определенный в области базы данных (ON DATABASE), требуется разрешение ALTER ANY DATABASE DDL TRIGGER в текущей базе данных.  
  
## <a name="examples"></a>Примеры  
 В следующем примере в базе данных AdventureWorks 2012 создается триггер DML, который выводит пользовательское сообщение клиенту, когда пользователь пытается добавить или изменить данные в таблице `SalesPersonQuotaHistory`. Затем триггер изменяется с использованием инструкции `ALTER TRIGGER`, чтобы применить триггер только к действиям `INSERT`. Этот триггер полезен, так как он напоминает пользователям, что при обновлениях и вставках строк в эту таблицу необходимо направить уведомление в отдел `Compensation`.  
  
```sql  
CREATE TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
WITH ENCRYPTION  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  

-- Now, change the trigger.  
ALTER TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
AFTER INSERT  
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER (Transact-SQL)](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_helptrigger (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [Создание хранимой процедуры](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sp_addmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [Транзакции](../../relational-databases/native-client-ole-db-transactions/transactions.md)   
 [Получение сведений о триггерах DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [Получение сведений о триггерах DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md)   
 [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)   
 [Внесение изменений в схемы баз данных публикации](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
