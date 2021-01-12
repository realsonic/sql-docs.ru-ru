---
title: CREATE TRIGGER (Transact-SQL)
description: Справочник по Transact-SQL для инструкции CREATE TRIGGER, которая используется для создания триггера DML, DDL или входа.
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE TRIGGER
- TRIGGER
- CREATE_TRIGGER_TSQL
- TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- recursive DML triggers [SQL Server]
- CREATE TRIGGER statement
- multiple triggers
- deferred name resolution, DML triggers
- DML triggers, creating
- nested triggers
- server-scoped triggers
- DDL triggers, creating
- triggers [SQL Server], creating
- database-scoped triggers [SQL Server]
ms.assetid: edeced03-decd-44c3-8c74-2c02f801d3e7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: mathoma
ms.openlocfilehash: c7bdee119d34181a59d8717957c228e6d24facdc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099546"
---
# <a name="create-trigger-transact-sql"></a>CREATE TRIGGER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Создает триггер языка обработки данных, DDL или входа. Триггер — это особая разновидность хранимой процедуры, которая автоматически выполняется при возникновении события на сервере базы данных. Триггеры DML выполняются, когда пользователь пытается изменить данные с помощью событий языка обработки данных (DML). Событиями DML являются процедуры INSERT, UPDATE или DELETE, применяемые к таблице или представлению. Эти триггеры срабатывают при запуске любого допустимого события независимо от наличия и числа затронутых строк таблицы. Дополнительные сведения см. в разделе [DML Triggers](../../relational-databases/triggers/dml-triggers.md).  
  
Триггеры DDL активируются в ответ на разные события языка описания данных (DDL). Эти события прежде всего соответствуют инструкциям [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE, ALTER, DROP и некоторым системным хранимым процедурам, которые выполняют схожие с DDL операции. 

Триггеры входа могут срабатывать в ответ на событие LOGON, которое возникает при создании пользовательского сеанса. Вы можете создавать триггеры непосредственно из инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] или методов сборок, созданных в среде CLR платформы [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] и переданных в экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет создавать несколько триггеров для любой инструкции.  
  
> [!IMPORTANT]  
>  Вредоносный программный код внутри триггеров может быть запущен с расширенными правами доступа. Дополнительные сведения о том, как уменьшить эту угрозу, см. в статье [Управление безопасностью триггеров](../../relational-databases/triggers/manage-trigger-security.md).  
  
> [!NOTE]  
>  В этой статье рассматривается интеграция среды CLR .NET Framework с SQL Server. Интеграция со средой CLR не применяется к базе данных SQL Azure.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="sql-server-syntax"></a>Синтаксис SQL Server  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
[ WITH APPEND ]  
[ NOT FOR REPLICATION ]   
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME <method specifier [ ; ] > }  
  
<dml_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
```  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a 
-- table (DML Trigger on memory-optimized tables)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
AS { sql_statement  [ ; ] [ ,...n ] }  
  
<dml_trigger_option> ::=  
    [ NATIVE_COMPILATION ]  
    [ SCHEMABINDING ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE or UPDATE statement (DDL Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { ALL SERVER | DATABASE }   
[ WITH <ddl_trigger_option> [ ,...n ] ]  
{ FOR | AFTER } { event_type | event_group } [ ,...n ]  
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<ddl_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a LOGON event (Logon Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON ALL SERVER   
[ WITH <logon_trigger_option> [ ,...n ] ]  
{ FOR| AFTER } LOGON    
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<logon_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
## <a name="azure-sql-database-syntax"></a>Синтаксис базы данных SQL Azure  
  
```syntaxsql
-- Azure SQL Database Syntax   
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
 [ WITH <dml_trigger_option> [ ,...n ] ]   
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
  AS { sql_statement  [ ; ] [ ,...n ] [ ; ] > }  
  
<dml_trigger_option> ::=   
        [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Azure SQL Database Syntax  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE, or UPDATE STATISTICS statement (DDL Trigger)   
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { DATABASE }   
 [ WITH <ddl_trigger_option> [ ,...n ] ]   
{ FOR | AFTER } { event_type | event_group } [ ,...n ]   
AS { sql_statement  [ ; ] [ ,...n ]  [ ; ] }  
  
<ddl_trigger_option> ::=   
    [ EXECUTE AS Clause ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
OR ALTER  
**Область применения**: Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] с пакетом обновления 1 (SP1)). 
  
Условно изменяет триггер только в том случае, если он уже существует. 
  
*schema_name*  
Имя схемы, которой принадлежит триггер DML. Действие триггеров DML ограничивается схемой той таблицы или того представления, для которых они созданы. Аргумент *schema_name* не может указываться для триггеров DDL или триггеров входа.  
  
*trigger_name*  
Имя триггера. Аргумент *trigger_name* должен соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md) с одним дополнительным ограничением: *trigger_name* не может начинаться с символов # или ##.  
  
*table* | *view*  
Таблица или представление, в котором выполняется триггер DML. Эту таблицу или представление иногда называют таблицей триггера или представлением триггера соответственно. Указание уточненного имени таблицы или представления не является обязательным. Ссылку на представление можно использовать только в триггере INSTEAD OF. Нельзя определить триггеры DML для локальной или глобальной временных таблиц.  
  
DATABASE  
Применяет область действия триггера DDL к текущей базе данных. Если этот аргумент определен, триггер срабатывает всякий раз при возникновении в базе данных события типа *event_type* или *event_group*.  
  
ALL SERVER  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
Применяет область действия триггера DDL или триггера входа к текущему серверу. Если этот аргумент определен, триггер срабатывает всякий раз при возникновении на текущем сервере события типа *event_type* или *event_group*.  
  
WITH ENCRYPTION  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
Маскирует текст инструкции CREATE TRIGGER. Использование параметра WITH ENCRYPTION не позволяет публиковать триггер как часть репликации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Параметр WITH ENCRYPTION нельзя указать для триггеров CLR.  
  
EXECUTE AS  
Указывает контекст безопасности, в котором выполняется триггер. Позволяет управлять учетной записью пользователя, используемой экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для проверки разрешений на любые объекты базы данных, ссылаемые триггером.  
  
Этот параметр является обязательным для триггеров в таблицах, оптимизированных для памяти.  
  
Дополнительные сведения см. в разделе [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
NATIVE_COMPILATION  
Указывает, что триггер компилируется в собственном коде.  
  
Этот параметр является обязательным для триггеров в таблицах, оптимизированных для памяти.  
  
SCHEMABINDING  
Гарантирует, что используемые триггером таблицы ну будут удалены или изменены.  
  
Этот параметр является обязательным для триггеров в таблицах, оптимизированных для памяти, и не поддерживается для триггеров в обычных таблицах.  
  
FOR | AFTER  
Значение FOR или AFTER указывает, что триггер DML срабатывает только после успешного запуска всех операций в инструкции SQL, по которой срабатывает триггер. Кроме того, до запуска триггера должны успешно завершиться все каскадные действия и проверки ограничений, на которые есть ссылки.  
  
Нельзя определить триггеры AFTER для представлений.  
  
INSTEAD OF  
Указывает, что триггер DML выполняется *вместо* инструкции SQL, по которой он срабатывает, то есть переопределяет действия запускающих инструкций. Аргумент INSTEAD OF нельзя использовать для триггеров DDL или триггеров входа.  
  
Для каждой инструкции INSERT, UPDATE или DELETE в таблице или представлении можно определить не более одного триггера INSTEAD OF. Также вы можете определить представления представлений, указав для каждого их уровня собственный триггер INSTEAD OF.  
  
Триггеры INSTEAD OF нельзя определять для обновляемых представлений, которые используют параметр WITH CHECK OPTION. Такое действие вызовет ошибку, если триггер INSTEAD OF добавляется к обновляемому представлению с параметром WITH CHECK OPTION. Чтобы удалить этот параметр, выполните инструкцию ALTER VIEW перед определением триггера INSTEAD OF.  
  
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }  
Определяет инструкции изменения данных, при применении которых к таблице или представлению срабатывает триггер DML. Укажите хотя бы один вариант. В определении триггера разрешены любые сочетания вариантов в любом порядке.  
  
Для триггеров INSTEAD OF нельзя использовать параметр DELETE в таблицах со ссылочной связью, которая определяет каскадное действие ON DELETE. Аналогично параметр UPDATE недопустим в таблицах, у которых есть ссылочная связь с каскадным действием ON UPDATE.  
  
WITH APPEND  
**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].  
  
Указывает, что требуется добавить триггер существующего типа. Аргумент WITH APPEND нельзя использовать для триггеров INSTEAD OF и в тех случаях, когда явно указан триггер AFTER. Для сохранения обратной совместимости аргумент WITH APPEND следует использовать только при указании параметра FOR без INSTEAD OF или AFTER. Нельзя указать WITH APPEND, если используется EXTERNAL NAME (то есть триггер является триггером CLR).  
  
*event_type*  
Имя языкового события [!INCLUDE[tsql](../../includes/tsql-md.md)], запуск которого вызывает срабатывание триггера DDL. Список событий, которые могут быть использованы в триггерах DDL, приведен в разделе [DDL-события](../../relational-databases/triggers/ddl-events.md).  
  
*event_group*  
Имя стандартной группы языковых событий [!INCLUDE[tsql](../../includes/tsql-md.md)]. Триггер DDL срабатывает после запуска любого языкового события [!INCLUDE[tsql](../../includes/tsql-md.md)], которое относится к группе *event_group*. Список групп событий, которые могут быть использованы в триггерах DDL, приведен в разделе [Группы DDL-событий](../../relational-databases/triggers/ddl-event-groups.md).  
  
После завершения инструкции CREATE TRIGGER параметр *event_group* работает в режиме макроса, добавляя охватываемые им типы события в представление каталога sys.trigger_events.  
  
NOT FOR REPLICATION  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
Указывает, что триггер не должен выполняться, когда агент репликации изменяет настроенную для триггера таблицу.  
  
*sql_statement*  
Условия и действия триггера. Условия триггера указывают дополнительные критерии, определяющие, какие события — DML, DDL или событие входа — вызывают выполнение триггера.  
  
Действия триггера, указанные в инструкциях языка [!INCLUDE[tsql](../../includes/tsql-md.md)], вступают в силу после попытки использования операции.  
  
Триггеры могут содержать любое количество инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)] любого типа, за некоторыми исключениями. Дополнительные сведения см. в подразделе "Примечания". Триггеры предназначены для проверки или изменения данных при выполнении инструкций модификации или определения данных. Не следует возвращать из них данные пользователю. Инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] в составе триггера часто содержат выражения [языка управления потоком](~/t-sql/language-elements/control-of-flow.md).  
  
Триггеры DML используют логические (концептуальные) таблицы deleted и inserted. По своей структуре они подобны таблице, для которой определен триггер, то есть таблице, к которой применяется действие пользователя. В таблицах deleted и inserted содержатся старые или новые значения строк, которые могут быть изменены действиями пользователя. Например, для запроса всех значений таблицы `deleted` можно использовать инструкцию:  
  
```sql  
SELECT * FROM deleted;  
```  
  
Дополнительные сведения см. в разделе [Использование таблиц inserted и deleted](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md).  
  
Триггеры DDL и триггеры входа собирают сведения о запускающих событиях с помощью функции [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md). Дополнительные сведения см. в разделе [Использование функции EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет обновлять столбцы типа **text**, **ntext** или **image** с помощью триггера INSTEAD OF для таблиц или представлений.  
  
> [!IMPORTANT]
>  Типы данных **ntext**, **text** и **image** будут исключены в следующей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Следует избегать использования этих типов данных при новой разработке и запланировать изменение приложений, использующих их в настоящий момент. Вместо них следует использовать типы данных [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)и [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) . Как триггеры AFTER, так и триггеры INSTEAD OF поддерживают данные типов **varchar(MAX)** , **nvarchar(MAX)** и **varbinary(MAX)** в таблицах inserted и deleted.  
  
Для триггеров в таблицах, оптимизированных для памяти, единственной инструкцией *sql_statement*, разрешенной на верхнем уровне, является блок ATOMIC. В блоке ATOMIC допускается только T-SQL, разрешенный в процедурах, компилируемых в собственном коде.  
  
\< method_specifier > 
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.  
  
Указывает метод сборки для связывания с CLR-триггером. Этот метод не должен принимать аргументы и возвращать значения void. Аргумент *class_name* должен быть допустимым идентификатором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и существовать как класс в сборке с видимостью сборки. Если класс имеет имя, содержащее точки (.) для разделения частей пространства имен, имя класса должно быть заключено в квадратные скобки ([ ]) или двойные кавычки (" "). Класс не может быть вложенным.  
  
> [!NOTE]  
>  По умолчанию возможность [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запускать код CLR отключена. Можно создавать, изменять и удалять объекты базы данных, которые ссылаются на модули управляемого кода, но эти модули не будут выполнены в экземплярах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если вы не включили [параметр clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) с помощью процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
## <a name="remarks-for-dml-triggers"></a>Примечания о триггерах DML  
Триггеры DML часто используются для применения бизнес-правил и обеспечения целостности данных. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] декларативное ограничение ссылочной целостности обеспечивается инструкциями ALTER TABLE и CREATE TABLE. Но декларативное ограничение ссылочной целостности не обеспечивает ссылочную целостность между базами данных. Ограничение ссылочной целостности подразумевает выполнение правил связи между первичными и внешними ключами таблиц. Для обеспечения ограничений ссылочной целостности используйте в инструкциях ALTER TABLE и CREATE TABLE ограничения PRIMARY KEY и FOREIGN KEY. Если ограничения распространяются на таблицу триггера, они проверяются после выполнения триггера INSTEAD OF, но до выполнения триггера AFTER. Если будет обнаружено нарушение ограничений, для триггера INSTEAD OF выполняется откат, а триггер AFTER не срабатывает.  
  
Вы можете указать, какой триггер AFTER будет выполняться для таблицы первым, а какой последним, с помощью sp_settriggerorder. Для таблицы можно определить только один первый и один последний триггер для каждой из операций INSERT, UPDATE и DELETE. Если для таблицы определены другие триггеры AFTER, они выполняются в случайном порядке.  
  
Если инструкция ALTER TRIGGER изменяет первый или последний триггер, для него удаляется метка первого или последнего триггера и порядок сортировки нужно установить заново с помощью sp_settriggerorder.  
  
Триггер AFTER выполняется только после того, как вызывающая срабатывание триггера инструкция SQL успешно выполняется. Успешное выполнение также подразумевает завершение всех ссылочных каскадных действий и проверки ограничений, связанных с измененными или удаленными объектами. Триггер AFTER не вызывает рекурсивное срабатывание триггера INSTEAD OF для той же таблицы.  
  
Если определенный для таблицы триггер INSTEAD OF выполняет в этой таблице какую-либо инструкцию, которая обычно приводит к срабатыванию триггера INSTEAD OF, этот триггер не вызывается рекурсивно. Вместо этого инструкция обрабатывается так, как если бы у таблицы отсутствовал триггер INSTEAD OF и начинается последовательность применения ограничений и выполнения триггеров AFTER. Для примера предположим, что для таблицы определен триггер INSTEAD OF INSERT. Этот триггер выполняет инструкцию INSERT в той же таблице, и в этом случае выполненная в триггере INSTEAD OF инструкция INSERT не приводит к новому срабатыванию триггера. Выполняемая триггером команда INSERT начинает процесс применения ограничений и срабатывания всех триггеров AFTER INSERT, определенных для этой таблицы.  
  
Если определенный для представления триггер INSTEAD OF выполняет по отношению к этому представлению какую-либо инструкцию, которая обычно приводит к срабатыванию триггера INSTEAD OF, триггер рекурсивно не вызывается. Вместо этого инструкция выполняет изменение базовых таблиц, на которых основано представление. В данном случае определение представления должно удовлетворять всем ограничениям, установленным для обновляемых представлений. Определение обновляемых представлений см. в разделе [Изменение данных через представление](../../relational-databases/views/modify-data-through-a-view.md).  
  
Для примера предположим, что для представления определен триггер INSTEAD OF UPDATE. Этот триггер выполняет инструкцию UPDATE в том же представлении, и в этом случае выполненная в триггере INSTEAD OF инструкция UPDATE не приводит к новому срабатыванию триггера. Выполняемая в триггере инструкция UPDATE обрабатывает представление так, как если бы у него не было триггера INSTEAD OF. Столбцы, измененные с помощью инструкции UPDATE, должны принадлежать одной базовой таблице. Каждая модификация базовой таблицы вызывает применение последовательности ограничений и взвод триггеров AFTER, определенных для данной таблицы.  
  
### <a name="testing-for-update-or-insert-actions-to-specific-columns"></a>Проверка действий инструкций UPDATE или INSERT на указанные столбцы  
Триггер [!INCLUDE[tsql](../../includes/tsql-md.md)] можно настроить для выполнения некоторых действий при изменении определенных столбцов в инструкциях UPDATE или INSERT. Используйте для этих целей в теле триггера конструкции [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md) или [COLUMNS_UPDATED](../../t-sql/functions/columns-updated-transact-sql.md). Конструкция UPDATE() проверяет действие инструкций UPDATE или INSERT на одном столбце. COLUMNS_UPDATED проверяет выполнение операций UPDATE или INSERT над множеством столбцов. Эта функция возвращает битовый шаблон с информацией о том, какие столбцы были вставлены или обновлены.  
  
### <a name="trigger-limitations"></a>Ограничения триггеров  
Инструкция CREATE TRIGGER должна быть первой инструкцией в пакете и может применяться только к одной таблице.  
  
Триггер создается только в текущей базе данных, но может, тем не менее, содержать ссылки на объекты за пределами текущей базы данных.  
  
Если для уточнения триггера указано имя схемы, имя таблицы необходимо уточнить таким же образом.  
  
Одно и то же действие триггера может быть определено более чем для одного действия пользователя (например, INSERT и UPDATE) в одной и той же инструкции CREATE TRIGGER.  
  
Триггеры INSTEAD OF DELETE и INSTEAD OF UPDATE нельзя определить для таблицы, у которой есть внешний ключ с каскадным действием для операции DELETE или UPDATE.  
  
Внутри триггера может быть использована любая инструкция SET. Выбранный параметр SET остается в силе во время выполнения триггера, после чего настройки возвращаются в предыдущее состояние.  
  
Во время срабатывания триггера результаты возвращаются вызывающему приложению так же, как и в случае с хранимыми процедурами. Чтобы при срабатывании триггера в приложение не возвращались результаты, не включайте в триггер инструкции SELECT, которые возвращают результаты или инструкции присвоения переменных. Если триггер содержит инструкции SELECT, которые возвращают результаты пользователю, либо инструкции присвоения значения переменным, для него требуется особый подход. Возвращаемые результаты нужно будет передать в каждое приложение, которому разрешено изменять таблицу триггера. Если в триггере происходит присвоение переменной, следует использовать инструкцию SET NOCOUNT в начале триггера, чтобы предотвратить возвращение каких-либо результирующих наборов.  
  
Хотя инструкция TRUNCATE TABLE по сути аналогичная инструкции DELETE, она не активирует триггер, так как не заносит в журнал удаление отдельных строк. Но беспокоиться о случайном обходе триггера DELETE таким образом нужно только пользователям с разрешениями на выполнение инструкции TRUNCATE TABLE.  
  
Инструкция WRITETEXT (с ведением журнала и без него) не запускает триггеры.  
  
Следующие инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] не разрешены в триггерах DML:  

- ALTER DATABASE
- CREATE DATABASE
- DROP DATABASE
- RESTORE DATABASE
- RESTORE LOG
- RECONFIGURE

Кроме того, не допускается использование перечисленных ниже инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] в тексте триггера DML, если он применяется к таблице или представлению, которые являются целью действий триггера.  
  
- CREATE INDEX (в т.ч CREATE SPATIAL INDEX и CREATE XML INDEX)
- ALTER INDEX
- DROP INDEX
- DROP TABLE
- DBCC DBREINDEX
- ALTER PARTITION FUNCTION
- ALTER TABLE, если используется в следующих целях:
    - Добавление, изменение или удаление столбцов.
    - Переключение секций.
    - Добавление или удаление ограничений PRIMARY KEY и UNIQUE.

> [!NOTE]  
>  Поскольку [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает пользовательских триггеров в системных таблицах, рекомендуется не создавать пользовательские триггеры для системных таблиц. 

### <a name="optimizing-dml-triggers"></a>Оптимизация триггеров DML
Триггеры работают в транзакциях (в том числе неявных) и блокируют ресурсы на весь период, в течение которого транзакция открыта. Такая блокировка действует, пока транзакция не будет зафиксирована (COMMIT) или отклонена (ROLLBACK). Чем дольше выполняется триггер, тем выше вероятность блокирования другого процесса. Старайтесь создавать такие триггеры, которые выполняются максимально быстро. Один из способов сократить время выполнения — освободить триггер, если инструкция DML изменяет 0 строк. 

Чтобы освободить триггер для команды, которая не изменяет ни одной строки, используйте системную переменную [ROWCOUNT_BIG](../functions/rowcount-big-transact-sql.md). 

В следующем фрагменте кода T-SQL триггер освобождается для команды, которая не изменяет ни одной строки. Этот код нужно добавить в начале каждого триггера DML:

```sql
IF (ROWCOUNT_BIG() = 0)
RETURN;
```
  
  
## <a name="remarks-for-ddl-triggers"></a>Примечания о триггерах DDL  
Триггеры DDL, как и стандартные триггеры, запускают хранимые процедуры в ответ на какое-либо событие. В отличие от стандартных триггеров, они не срабатывают при выполнении инструкций UPDATE, INSERT или DELETE для таблицы или представления. Вместо этого они обычно срабатывают в ответ на инструкции языка определения данных (DDL). К ним относятся инструкции CREATE, ALTER, DROP, GRANT, DENY, REVOKE и UPDATE STATISTICS. Системные хранимые процедуры, выполняющие операции, подобные операциям DDL, также могут запускать триггеры DDL.  
  
> [!IMPORTANT]  
>  Протестируйте триггеры DDL, чтобы получить ответ на выполнение системных хранимых процедур. Например, инструкция CREATE TYPE и хранимые процедуры sp_addtype и sp_rename вызовут срабатывание триггера DDL, созданного для события CREATE_TYPE.  
  
Дополнительные сведения о триггерах DDL см. в разделе [Триггеры DDL](../../relational-databases/triggers/ddl-triggers.md).  
  
Триггеры DDL не срабатывают в ответ на события, влияющие на локальные или глобальные временные таблицы и хранимые процедуры.  
  
В отличие от триггеров DML, триггеры DDL не ограничены областью схемы. Это означает, что для запроса метаданных о триггерах DDL нельзя воспользоваться такими функциями, как OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY и OBJECTPROPERTYEX. Используйте вместо них представления каталога. Дополнительные сведения см. в статье [Получение сведений о триггерах DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md).  
  
> [!NOTE]  
>  Триггеры DDL сервера находятся в папке **Триггеры** обозревателя объектов среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Эта папка находится под папкой **Объекты сервера** . Триггеры DDL, доступные в области базы данных, находятся в папке **Триггеры базы данных**. Эта папка находится в папке **Программирование** соответствующей базы данных.  
  
## <a name="logon-triggers"></a>Триггеры входа  
Триггеры входа выполняют хранимые процедуры в ответ на событие LOGON. Это событие вызывается, когда для пользователя создается сеанс в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Триггеры входа срабатывают после проверки подлинности при входе, но перед тем, как устанавливается пользовательский сеанс. Таким образом, все созданные внутри триггера сообщения, которые обычно передаются пользователю (например, сообщения об ошибках и сообщения от инструкции PRINT), перенаправляются в журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделе [Триггеры входа](../../relational-databases/triggers/logon-triggers.md).  
  
Если проверка подлинности завершается сбоем, триггеры входа не срабатывают.  
  
Распределенные транзакции не поддерживаются в триггерах входа. Если триггер содержит распределенную транзакцию, при его срабатывании возвращается ошибка 3969.  
  
### <a name="disabling-a-logon-trigger"></a>Отключение триггера входа  
Триггер входа может эффективно запрещать подключения к службам [!INCLUDE[ssDE](../../includes/ssde-md.md)] для всех пользователей, в том числе членов предопределенной роли сервера **sysadmin** . Если триггер входа запрещает соединения, члены предопределенной роли сервера **sysadmin** могут подключаться с помощью выделенного административного соединения или путем вызова [!INCLUDE[ssDE](../../includes/ssde-md.md)] в режиме минимальной конфигурации (-f). Дополнительные сведения см. в разделе [Параметры запуска службы Database Engine](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
## <a name="general-trigger-considerations"></a>Общие соглашения о триггерах  
  
### <a name="returning-results"></a>Возвращаемые результаты  
Возможность возвращать результаты из триггеров будет исключена из следующей версии SQL Server. Триггеры, которые возвращают результирующие наборы, могут привести к непредвиденному поведению в приложениях, не предназначенных для работы с ними. Старайтесь не возвращать результирующие наборы из триггеров во всех новых проектах и постепенно исправляйте такое поведение в существующих приложениях. Чтобы триггеры не возвращали результирующие наборы, для параметра [disallow results from triggers](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) необходимо установить значение 1.  
  
Триггеры входа всегда запрещают возврат результирующих наборов, и это нельзя изменить. Если триггер входа формирует результирующий набор, его не удастся запустить и любая попытка входа, при которой срабатывает такой триггер, будет запрещена.  
  
### <a name="multiple-triggers"></a>Несколько триггеров  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет создавать несколько триггеров для каждого события DML, DDL или LOGON. Например, если CREATE TRIGGER FOR UPDATE выполняется для таблицы, которая уже имеет триггер UPDATE, будет создан дополнительный триггер для обновлений. В более ранних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] был разрешен только один триггер в каждой таблице для каждого события изменения данных INSERT, UPDATE или DELETE.  
  
### <a name="recursive-triggers"></a>Рекурсивные триггеры  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] также поддерживает рекурсивный вызов триггеров, если вы включите параметр RECURSIVE_TRIGGERS с помощью инструкции ALTER DATABASE.  
  
В рекурсивных триггерах могут возникать следующие типы рекурсии:  
  
-   Косвенная рекурсия  
  
     При косвенной рекурсии приложение обновляет таблицу T1. Это событие вызывает срабатывание триггера TR1, обновляющего таблицу T2. Затем срабатывает триггер T2, который обновляет таблицу T1.  
  
-   Прямая рекурсия  
  
     При прямой рекурсии приложение обновляет таблицу T1. Это событие вызывает срабатывание триггера TR1, обновляющего таблицу T1. Поскольку таблица T1 уже была обновлена, триггер TR1 срабатывает снова и т. д.  
  
В следующем примере используются оба типа рекурсий: прямая и косвенная. Допустим, для таблицы T1 определены два триггера: TR1 и TR2. Триггер TR1 рекурсивно обновляет таблицу T1. Инструкция UPDATE выполняет TR1 и TR2 по одному разу. Кроме того, запуск TR1 вызывает выполнение триггеров TR1 (рекурсивно) и TR2. В таблицах inserted и deleted триггера содержатся строки, которые относятся только к инструкции UPDATE, вызвавшей срабатывание триггера.  
  
> [!NOTE]  
>  Описанная ситуация имеет место только в том случае, если настройка RECURSIVE_TRIGGERS включена с помощью инструкции ALTER DATABASE. Не существует определенного порядка для выполнения нескольких триггеров, определенных для одного события. Каждый триггер должен быть самодостаточным.  
  
Отключение настройки RECURSIVE_TRIGGERS предотвращает выполнение только прямых рекурсий. Чтобы отключить косвенную рекурсию, с помощью хранимой процедуры sp_configure присвойте параметру сервера nested triggers значение 0.  
  
Если один из триггеров (независимо от уровня вложенности) выполняет инструкцию ROLLBACK TRANSACTION, никакие другие триггеры не выполняются.  
  
### <a name="nested-triggers"></a>Вложенные триггеры  
Для триггеров допускается не более 32 уровней вложенности. Если триггер изменяет таблицу, для которой определен другой триггер, активируется этот второй триггер. Он может, в свою очередь, вызвать третий триггер и так далее. Если любой из триггеров в цепочке отключает бесконечный цикл, то уровень вложенности превышает допустимый предел, и срабатывание триггера отменяется. Когда триггер [!INCLUDE[tsql](../../includes/tsql-md.md)] запускает управляемый код ссылкой на подпрограмму CLR, тип, или статистическое выражение, такая ссылка считается одним из 32 допустимых уровней вложенности. Это ограничение не распространяется на методы, вызываемые из управляемого кода.  
  
Чтобы отменить вложенные триггеры, присвойте значение 0 параметру nested triggers хранимой процедуры sp_configure. Конфигурация по умолчанию поддерживает вложенные триггеры. Если вложенные триггеры отключены, отключаются и рекурсивные триггеры, независимо от значения RECURSIVE_TRIGGERS, которое установлено с помощью инструкции ALTER DATABASE.  
  
Первый триггер AFTER, вложенный в триггер INSTEAD OF, срабатывает даже в том случае, если для сервера настроен нулевой уровень **вложенных триггеров**. Но в таком случае остальные триггеры AFTER не сработают. Проверьте все приложения на наличие вложенных триггеров, чтобы определить соблюдение бизнес-правил, прежде чем устанавливать значение 0 для параметра **nested triggers** (вложенные триггеры). Если правила не соблюдаются, внесите соответствующие изменения.  
  
### <a name="deferred-name-resolution"></a>Отложенная интерпретация имен  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет добавлять в хранимые процедуры, триггеры и пакеты [!INCLUDE[tsql](../../includes/tsql-md.md)] ссылки на таблицы, которые не существуют во время компиляции. Такая возможность называется отложенной интерпретацией имен.  
  
## <a name="permissions"></a>Разрешения  
Чтобы создать триггер DML, ему нужно разрешение ALTER для таблицы или представления, для которых создается этот триггер.  
  
Чтобы создать триггер DDL в области сервера (ON ALL SERVER) или триггера входа, требуется разрешение CONTROL SERVER для этого сервера. Чтобы создать триггер DDL в области базы данных (ON DATABASE), требуется разрешение ALTER ANY DATABASE DDL TRIGGER для текущей базы данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-a-dml-trigger-with-a-reminder-message"></a>A. Использование триггера DML с предупреждающим сообщением  
Следующий триггер DML отправляет клиенту сообщение, когда кто-то пытается добавить или изменить данные в таблице `Customer` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE TRIGGER reminder1  
ON Sales.Customer  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Customer Relations', 16, 10);  
GO  
```  
  
### <a name="b-using-a-dml-trigger-with-a-reminder-e-mail-message"></a>Б. Использование триггера DML с предупреждающим сообщением, отправляемым по электронной почте  
В следующем примере указанному пользователю (`MaryM`) по электронной почте отправляется сообщение при изменении таблицы `Customer`.  
  
```sql  
CREATE TRIGGER reminder2  
ON Sales.Customer  
AFTER INSERT, UPDATE, DELETE   
AS  
   EXEC msdb.dbo.sp_send_dbmail  
        @profile_name = 'AdventureWorks2012 Administrator',  
        @recipients = 'danw@Adventure-Works.com',  
        @body = 'Don''t forget to print a report for the sales force.',  
        @subject = 'Reminder';  
GO  
```  
  
### <a name="c-using-a-dml-after-trigger-to-enforce-a-business-rule-between-the-purchaseorderheader-and-vendor-tables"></a>В. Использование триггера DML AFTER для принудительного применения бизнес-правил между таблицами PurchaseOrderHeader и Vendor  
Так ограничения CHECK ссылаются только на столбцы, для которых определено ограничение на уровне таблицы или столбца, все межтабличные ограничения (в нашем примере это бизнес-правила) следует определять как триггеры.  
  
В следующем примере создается триггер DML в базе данных AdventureWorks 2012. Этот триггер проверяет оценку кредитоспособности для поставщика (оценка не равна 5) при попытке добавить новый заказ на покупку в таблицу `PurchaseOrderHeader`. Чтобы получить оценку кредитоспособности поставщика, требуется ссылка на таблицу `Vendor`. Если рейтинг кредитоспособности слишком низок, поступает сообщение об этом и вставка не выполняется.  
  
```sql  
-- This trigger prevents a row from being inserted in the Purchasing.PurchaseOrderHeader 
-- table when the credit rating of the specified vendor is set to 5 (below average).  
  
CREATE TRIGGER Purchasing.LowCredit ON Purchasing.PurchaseOrderHeader  
AFTER INSERT  
AS  
IF (ROWCOUNT_BIG() = 0)
RETURN;
IF EXISTS (SELECT *  
           FROM Purchasing.PurchaseOrderHeader AS p   
           JOIN inserted AS i   
           ON p.PurchaseOrderID = i.PurchaseOrderID   
           JOIN Purchasing.Vendor AS v   
           ON v.BusinessEntityID = p.VendorID  
           WHERE v.CreditRating = 5  
          )  
BEGIN  
RAISERROR ('A vendor''s credit rating is too low to accept new  
purchase orders.', 16, 1);  
ROLLBACK TRANSACTION;  
RETURN   
END;  
GO  
  
-- This statement attempts to insert a row into the PurchaseOrderHeader table  
-- for a vendor that has a below average credit rating.  
-- The AFTER INSERT trigger is fired and the INSERT transaction is rolled back.  
  
INSERT INTO Purchasing.PurchaseOrderHeader (RevisionNumber, Status, EmployeeID,  
VendorID, ShipMethodID, OrderDate, ShipDate, SubTotal, TaxAmt, Freight)  
VALUES (  
2  
,3  
,261  
,1652  
,4  
,GETDATE()  
,GETDATE()  
,44594.55  
,3567.564  
,1114.8638 );  
GO  
  
```  
  
### <a name="d-using-a-database-scoped-ddl-trigger"></a>Г. Использование триггера DDL уровня базы данных  
В следующем примере триггер DDL используется для предотвращения удаления синонимов в базе данных.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_SYNONYM  
AS   
IF (@@ROWCOUNT = 0)
RETURN;
   RAISERROR ('You must disable Trigger "safety" to remove synonyms!', 10, 1)  
   ROLLBACK  
GO  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
### <a name="e-using-a-server-scoped-ddl-trigger"></a>Д. Использование триггера DDL уровня сервера  
В следующем примере триггер DDL используется для вывода сообщения при возникновении на данном экземпляре сервера любого из событий CREATE DATABASE, а функция `EVENTDATA` используется для получения текста соответствующей инструкции на языке [!INCLUDE[tsql](../../includes/tsql-md.md)]. Примеры использования функции EVENTDATA в триггерах DDL см. в разделе [Использование функции EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
```sql  
CREATE TRIGGER ddl_trig_database   
ON ALL SERVER   
FOR CREATE_DATABASE   
AS   
    PRINT 'Database Created.'  
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
GO  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
```  
  
### <a name="f-using-a-logon-trigger"></a>Е. Использование триггера входа  
В следующем примере триггера входа выполняется запрет попытки подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в качестве члена имени входа *login_test*, если для этого имени входа уже запущено три сеанса.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
```sql  
USE master;  
GO  
CREATE LOGIN login_test WITH PASSWORD = '3KHJ6dhx(0xVYsdf' MUST_CHANGE,  
    CHECK_EXPIRATION = ON;  
GO  
GRANT VIEW SERVER STATE TO login_test;  
GO  
CREATE TRIGGER connection_limit_trigger  
ON ALL SERVER WITH EXECUTE AS 'login_test'  
FOR LOGON  
AS  
BEGIN  
IF ORIGINAL_LOGIN()= 'login_test' AND  
    (SELECT COUNT(*) FROM sys.dm_exec_sessions  
            WHERE is_user_process = 1 AND  
                original_login_name = 'login_test') > 3  
    ROLLBACK;  
END;  
  
```  
  
### <a name="g-viewing-the-events-that-cause-a-trigger-to-fire"></a>Ж. Просмотр событий, вызвавших срабатывание триггера  
В следующем примере выполняются запросы к представлениям каталога `sys.triggers` и `sys.trigger_events` с целью определения, какие события языка [!INCLUDE[tsql](../../includes/tsql-md.md)] вызывали срабатывание триггера `safety`. Триггер `safety`, созданный в примере Г, приведен выше.  
  
```sql  
SELECT TE.*  
FROM sys.trigger_events AS TE  
JOIN sys.triggers AS T ON T.object_id = TE.object_id  
WHERE T.parent_class = 0 AND T.name = 'safety';  
GO  
```  

    

## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [COLUMNS_UPDATED (Transact-SQL)](../../t-sql/functions/columns-updated-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER (Transact-SQL)](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [TRIGGER_NESTLEVEL (Transact-SQL)](../../t-sql/functions/trigger-nestlevel-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_settriggerorder (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-settriggerorder-transact-sql.md)   
 [UPDATE() (Transact-SQL)](../../t-sql/functions/update-trigger-functions-transact-sql.md)   
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
  
  


