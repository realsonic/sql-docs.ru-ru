---
title: Приступая к работе с разрешениями ядра СУБД | Документация Майкрософт
description: Обзор некоторых основных концепций безопасности в SQL Server и сведения о типичной реализации разрешений ядра СУБД.
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: quickstart
helpviewer_keywords:
- permissions [SQL Server], getting started
ms.assetid: 051af34e-bb5b-403e-bd33-007dc02eef7b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 41b69dfc9cc5564025fd7b28d14094389ff52450
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171696"
---
# <a name="getting-started-with-database-engine-permissions"></a>Приступая к работе с разрешениями Database Engine
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Управление разрешениями в [!INCLUDE[ssDE](../../../includes/ssde-md.md)] осуществляется на уровне сервера с помощью имен входа и ролей сервера и на уровне базы данных с помощью пользователей и ролей базы данных. Модель для [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] предоставляет ту же систему в каждой базе данных, однако разрешения на уровне сервера недоступны. В этом разделе рассматриваются некоторые основные понятия безопасности, а затем описывается типичная реализация разрешений.  
  
## <a name="security-principals"></a>Субъекты безопасности  
 Субъект безопасности — это официальное название удостоверений, которые используют [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и которым можно назначать разрешения для выполнения действий. Обычно это пользователи или группы пользователей, однако субъектами безопасности могут быть и другие сущности, олицетворяющие пользователей. Создавать субъекты безопасности и управлять ими можно с помощью списков [!INCLUDE[tsql](../../../includes/tsql-md.md)] или [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
##### <a name="logins"></a>Имена входа  
 Имена входа — это учетные записи отдельных пользователей для входа в [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] поддерживают имена входа на основе проверки подлинности Windows и на основе проверки подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения об этих двух типах имен входа см. в разделе [Choose an Authentication Mode](../../../relational-databases/security/choose-an-authentication-mode.md).  
  
##### <a name="fixed-server-roles"></a>Предопределенные роли сервера  
 В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]предопределенные роли сервера — это набор предварительно настроенных ролей, который представляет собой удобную группу разрешений на уровне сервера. Имена входа можно добавить в роли, используя инструкцию `ALTER SERVER ROLE ... ADD MEMBER` . Дополнительные сведения см. в разделе [ALTER SERVER ROLE (Transact-SQL)](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] не поддерживает предопределенные роли сервера, однако включает две роли в базе данных master (`dbmanager` и `loginmanager`), которые выполняют аналогичные функции.  
  
##### <a name="user-defined-server-roles"></a>Определяемые пользователем роли сервера  
 В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]можно создавать собственные роли сервера и назначать им разрешения на уровне сервера. Имена входа можно добавить в роли сервера, используя инструкцию `ALTER SERVER ROLE ... ADD MEMBER` . Дополнительные сведения см. в разделе [ALTER SERVER ROLE (Transact-SQL)](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] не поддерживает определяемые пользователем роли сервера.  
  
##### <a name="database-users"></a>Пользователи базы данных  
 Именам входа доступ к базе данных предоставляется путем создания пользователя базы данных в базе данных и сопоставления этого пользователя базы данных с именем входа. Как правило, имя пользователя базы данных совпадает с именем входа, хотя это и необязательно. Один пользователь базы данных сопоставляется с одним именем входа. Имя входа может быть сопоставлено только с одним пользователем в базе данных, однако может сопоставляться как пользователь базы данных в нескольких базах данных.  
  
 Кроме того, можно создать пользователей базы данных без соответствующих имен входа. Они называются *пользователями автономной базы данных*. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] рекомендуют использовать пользователей автономной базы данных, поскольку это упрощает перенос базы данных на другой сервер. Как и для имен входа, для пользователей автономной базы данных можно использовать проверку подлинности Windows или проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Пользователи автономной базы данных — создание переносимой базы данных](../../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
 Существует 12 типов пользователей с незначительными различиями в способах проверки подлинности и представляемых сущностях. Список пользователей см. в разделе [CREATE USER (Transact-SQL)](../../../t-sql/statements/create-user-transact-sql.md).  
  
##### <a name="fixed-database-roles"></a>Предопределенные роли базы данных  
 Предопределенные роли базы данных — это набор предварительно настроенных ролей, который представляет собой удобную группу разрешений на уровне базы данных. Пользователей базы данных и определяемые пользователем роли базы данных можно добавить в предопределенные роли базы данных с помощью инструкции `ALTER ROLE ... ADD MEMBER`. Дополнительные сведения см. в разделе [ALTER ROLE (Transact-SQL)](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="user-defined-database-roles"></a>Определяемые пользователем роли базы данных  
 Пользователи с разрешением `CREATE ROLE` могут создавать определяемые пользователем роли базы данных для представления групп пользователей с общими разрешениями. Обычно разрешения предоставляются или отклоняются для всей роли, что упрощает управление разрешениями и мониторинг. Пользователей базы данных можно добавлять в роли базы данных с помощью инструкции `ALTER ROLE ... ADD MEMBER` . Дополнительные сведения см. в разделе [ALTER ROLE (Transact-SQL)](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="other-principals"></a>Другие субъекты  
 В данной статье не рассматриваются дополнительные субъекты безопасности, такие как роли приложений и имена входа и пользователи, основанные на сертификатах или асимметричных ключах.  
  
 График, отображающий связи между пользователями Windows, группами Windows, именами входа и пользователями базы данных, см. в разделе [Create a Database User](../../../relational-databases/security/authentication-access/create-a-database-user.md).  
  
## <a name="typical-scenario"></a>Типичный сценарий  
 В следующем примере представлен типичный и рекомендуемый способ настройки разрешений.  
  
#### <a name="in-active-directory-or-azure-active-directory"></a>В Active Directory или Azure Active Directory  
  
1.  Создайте пользователя Windows для каждого пользователя.  
  
2.  Создайте группы Windows, представляющие рабочие единицы и рабочие функции.  
  
3.  Добавьте пользователей Windows в группы Windows.  

#### <a name="if-the-person-connecting-will-be-connecting-to-many-databases"></a>Если пользователь будет подключаться к нескольким базам данных  
  
1.  Создайте имя входа для групп Windows. (При использовании проверки подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] пропустите шаги, относящиеся к Active Directory, и создайте имена входа для проверки подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .)  
  
2.  В базе данных пользователей создайте пользователя базы данных для имени входа, представляющего группы Windows.  
  
3.  В базе данных пользователей создайте одну или несколько определяемых пользователем ролей базы данных, представляющих аналогичные функции. Например, финансовый аналитик и аналитик продаж.  
  
4.  Добавьте пользователей базы данных в одну или несколько определяемых пользователем ролей базы данных.  
  
5.  Предоставьте разрешения для определяемых пользователем ролей базы данных.  
  
#### <a name="if-the-person-connecting-will-be-connecting-to-only-one-database"></a>Если пользователь будет подключаться к только к одной базе данных  
  
1.  В базе данных пользователей создайте пользователя автономной базы данных для группы Windows. (При использовании проверки подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] пропустите шаги, относящиеся к Active Directory, и создайте проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для пользователя автономной базы данных.)  
  
1.  В базе данных пользователей создайте одну или несколько определяемых пользователем ролей базы данных, представляющих аналогичные функции. Например, финансовый аналитик и аналитик продаж.  
  
1.  Добавьте пользователей базы данных в одну или несколько определяемых пользователем ролей базы данных.  
  
1.  Предоставьте разрешения для определяемых пользователем ролей базы данных.  
  
 Как правило, результатом на этом этапе является пользователь Windows, являющийся членом группы Windows. Группа Windows имеет имя входа в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. Имя входа сопоставляется с удостоверением пользователя в базе данных пользователей. Пользователь является членом роли базы данных. Теперь необходимо добавить разрешения для роли.  
  
## <a name="assigning-permissions"></a>Назначение разрешений  
 Большинство инструкций назначения разрешений имеет следующий формат:  
  
```  
AUTHORIZATION  PERMISSION  ON  SECURABLE::NAME  TO  PRINCIPAL;  
```  
  
-   `AUTHORIZATION` должен быть равен `GRANT`, `REVOKE` или `DENY`.  
  
-   Предложение `PERMISSION` определяет, какое действие разрешено или запрещено. [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] можно указать 230 разрешений. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] предусмотрено меньше разрешений, так как некоторые действия не относятся к Azure. Разрешения перечисляются в разделе [Разрешения (компонент Database Engine)](../../../relational-databases/security/permissions-database-engine.md) и на схеме, упоминаемой ниже.  
  
-   `ON SECURABLE::NAME` представляет тип защищаемого объекта (сервер, объект сервера, база данных или объект базы данных) и его имя. Некоторые разрешения не требуют указания `ON SECURABLE::NAME` , так как оно может быть однозначным или недопустимым в контексте. Например, для разрешения `CREATE TABLE` не требуется предложение `ON SECURABLE::NAME`. (Инструкция `GRANT CREATE TABLE TO Mary;` позволяет пользователю Mary создавать таблицы.)  
  
-   `PRINCIPAL` представляет субъект безопасности (имя входа, пользователя или роль), который получает или утрачивает разрешение. Рекомендуется по возможности предоставлять разрешения для ролей.  
  
 В следующем примере инструкция grant предоставляет разрешение `UPDATE` для представления или таблицы `Parts` , которая содержится в схеме `Production` , роли с именем `PartsTeam`:  
  
```  
GRANT UPDATE ON OBJECT::Production.Parts TO PartsTeam;  
```  
  
 Разрешения предоставляются субъектам безопасности (именам входа, пользователям и ролям) с помощью инструкции `GRANT` . Разрешения явно отклоняются с помощью команды  `DENY` . Для удаления ранее предоставленного или отклоненного разрешения используется инструкция `REVOKE` . Разрешения накапливаются, то есть пользователь получает все разрешения, предоставленные пользователю, имени входа и любой группе, членом которой он является. При этом отклонение разрешения отменяет все предоставленные ранее разрешения.  
  
> [!TIP]  
>  Распространенная ошибка — попытка удалить `GRANT` с помощью команды `DENY` вместо `REVOKE`. Это может повлечь за собой проблемы, если пользователь получает разрешения из нескольких источников, что довольно распространено. Этот принцип демонстрируется в следующем примере.  
  
 Группа Sales получает разрешения `SELECT` для доступа к таблице OrderStatus посредством инструкции `GRANT SELECT ON OBJECT::OrderStatus TO Sales;`. Пользователь Ted является членом роли Sales. Кроме того, ему предоставлено разрешение `SELECT` для таблицы OrderStatus по его собственному имени пользователя посредством инструкции  `GRANT SELECT ON OBJECT::OrderStatus TO Ted;`. Предположим, администратор хочет удалить разрешение `GRANT` для роли Sales.  
  
-   Если администратор правильно выполняет инструкцию `REVOKE SELECT ON OBJECT::OrderStatus TO Sales;`, то пользователь Ted не утрачивает доступ `SELECT` к таблице OrderStatus по своей собственной инструкции `GRANT` .  
  
-   Если администратор неправильно выполняет `DENY SELECT ON OBJECT::OrderStatus TO Sales;` , то пользователь Ted, как член роли Sales, утрачивает разрешение `SELECT` , так как команда `DENY` для роли Sales переопределяет его собственную инструкцию  `GRANT`.  
  
> [!NOTE]  
>  Разрешения можно настроить с помощью [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]. В обозревателе объектов найдите защищаемый объект, щелкните его правой кнопкой мыши и выберите пункт **Свойства**. Перейдите на страницу **Разрешения** . Справочные сведения о странице разрешений см. в разделе [Permissions or Securables Page](../../../relational-databases/security/permissions-or-securables-page.md).  
  
## <a name="permission-hierarchy"></a>Иерархия разрешений  
 К разрешениям применяется иерархия "родители — потомки". То есть при предоставлении разрешения `SELECT` для базы данных оно включает разрешение `SELECT` для всех (дочерних) схем в базе данных. При предоставлении разрешения `SELECT` для схемы оно включает разрешение `SELECT` для всех (дочерних) таблиц и представлений в схеме. Разрешения являются транзитивными; то есть при предоставлении разрешения `SELECT` для базы данных оно включает разрешение `SELECT` для всех (дочерних) схем и всех (внучатых) таблиц и представлений.  
  
 Кроме того, предусмотрены покрывающие разрешения. Разрешение `CONTROL` для объекта, как правило, предоставляет все прочие разрешения для этого объекта.  
  
 Поскольку иерархия "родители — потомки" и иерархия покрытия могут применяться к одному разрешению, с течением времени система разрешений может усложняться. Например, рассмотрим таблицу (Region) в схеме (Customers) в базе данных (SalesDB).  
  
-   `CONTROL` для таблицы Region включает все остальные разрешения для таблицы Region, в том числе `ALTER`, `SELECT`, `INSERT`,  `UPDATE`, `DELETE`и некоторые другие разрешения.  
  
-   `SELECT` для схемы Customers, к которой принадлежит таблица Region, включает разрешение `SELECT` для таблицы Region.  
  
 Таким образом, разрешение `SELECT` для таблицы Region можно предоставить с помощью любой из этих шести инструкций:  
  
```  
GRANT SELECT ON OBJECT::Region TO Ted;   
  
GRANT CONTROL ON OBJECT::Region TO Ted;   
  
GRANT SELECT ON SCHEMA::Customers TO Ted;   
  
GRANT CONTROL ON SCHEMA::Customers TO Ted;   
  
GRANT SELECT ON DATABASE::SalesDB TO Ted;   
  
GRANT CONTROL ON DATABASE::SalesDB TO Ted;  
```  
  
## <a name="grant-the-least-permission"></a>Предоставление минимального разрешения  
 Первое указанное выше разрешение (`GRANT SELECT ON OBJECT::Region TO Ted;`) — наиболее гранулярное, то есть эта инструкция предоставляет минимально возможное разрешение `SELECT`. Вместе с ним не предоставляются разрешения для каких-либо вложенных объектов. Рекомендуется всегда предоставлять минимально возможное разрешение, однако (напротив) на более высоких уровнях для упрощения системы предоставления разрешений. Таким образом, если пользователю Ted нужны разрешения для всей схемы, предоставьте разрешение `SELECT` один раз на уровне схемы, вместо того чтобы предоставлять `SELECT` на уровне таблицы или представления несколько раз. Структура базы данных имеет большое влияние на успешность применения этой стратегии. Стратегия наиболее эффективна, когда объекты в базе данных, которым требуются одинаковые разрешения, включаются в одну схему.  
  
## <a name="list-of-permissions"></a>Список разрешений  
 [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] предусмотрено 230 разрешений. [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] предусмотрено 219 разрешений. [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] предусмотрено 214 разрешений. [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] предусмотрено 195 разрешений. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], [!INCLUDE[ssDW](../../../includes/ssdw-md.md)]и [!INCLUDE[ssAPS](../../../includes/ssaps-md.md)] разрешений меньше, так как они определяют только часть ядра СУБД, тогда как отдельные их разрешения не применяются к [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 
 
 [!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]
 
 Схему связей между субъектами [!INCLUDE[ssDE](../../../includes/ssde-md.md)] и объектами сервера и базы данных см. в разделе [Иерархия разрешений (компонент Database Engine)](../../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
## <a name="permissions-vs-fixed-server-and-fixed-database-roles"></a>Различия разрешений и предопределенных ролей сервера и базы данных  
 Разрешения предопределенных ролей сервера и базы данных схожи с гранулярными разрешениями, но все же отличаются от них. Например, члены предопределенной роли сервера `sysadmin` имеют все разрешения для экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], как и имена входа с разрешением `CONTROL SERVER` . Но предоставление разрешения `CONTROL SERVER` не делает имя входа членом предопределенной роли сервера sysadmin, а добавление имени входа в предопределенную роль сервера `sysadmin` не подразумевает явного предоставления ему разрешения `CONTROL SERVER`. Иногда хранимая процедура выполняет проверку разрешений путем проверки предопределенной роли, а не гранулярного разрешения. Например, для отключения базы данных требуется членство в предопределенной роли базы данных `db_owner` . Эквивалентного разрешения `CONTROL DATABASE` недостаточно. Эти две системы работают параллельно, но редко взаимодействуют друг с другом. Специалисты Майкрософт рекомендуют по возможности использовать более новую систему гранулярных разрешений вместо предопределенных ролей.
  
## <a name="monitoring-permissions"></a>Отслеживание разрешений  
 В следующих представлениях отображаются сведения о безопасности.  
  
-   Имена входа и определяемые пользователем роли сервера на сервере можно просмотреть в представлении `sys.server_principals` . Это представление недоступно в [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Пользователей и определяемые пользователями роли в базе данных можно просмотреть в представлении `sys.database_principals` .  
  
-   Разрешения, предоставленные именам входа и определяемым пользователями предопределенным ролям сервера, можно просмотреть в представлении `sys.server_permissions` . Это представление недоступно в [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Разрешения, предоставленные пользователям и определяемым пользователем предопределенным ролям базы данных, можно просмотреть в представлении `sys.database_permissions` .  
  
-   Членство в роли базы данных можно проверить с помощью представления `sys. sys.database_role_members` .  
  
-   Членство в роли сервера можно проверить с помощью представления `sys.server_role_members` . Это представление недоступно в [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Другие представления, связанные с безопасностью, описываются в разделе [Представления каталога безопасности (Transact-SQL)](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md) .  
  
### <a name="useful-transact-sql-statements"></a>Полезные инструкции Transact-SQL  
 Следующие инструкции возвращают полезные сведения о разрешениях.  
  
 Чтобы получить список явных разрешений, предоставленных или отклоненных в базе данных ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), выполните приведенную ниже инструкцию в базе данных.  
  
```sql  
SELECT   
    perms.state_desc AS State,   
    permission_name AS [Permission],   
    obj.name AS [on Object],   
    dPrinc.name AS [to User Name]  
FROM sys.database_permissions AS perms  
JOIN sys.database_principals AS dPrinc  
    ON perms.grantee_principal_id = dPrinc.principal_id  
JOIN sys.objects AS obj  
    ON perms.major_id = obj.object_id;  
```  
  
 Чтобы получить список членов ролей сервера (только [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]), выполните приведенную ниже инструкцию.  
  
```sql  
SELECT sRole.name AS [Server Role Name] , sPrinc.name AS [Members]  
FROM sys.server_role_members AS sRo  
JOIN sys.server_principals AS sPrinc  
    ON sRo.member_principal_id = sPrinc.principal_id  
JOIN sys.server_principals AS sRole  
    ON sRo.role_principal_id = sRole.principal_id;  
```  
  
 
 Чтобы получить список членов ролей базы данных ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), выполните приведенную ниже инструкцию в базе данных.  
  
```sql  
SELECT dRole.name AS [Database Role Name], dPrinc.name AS [Members]  
FROM sys.database_role_members AS dRo  
JOIN sys.database_principals AS dPrinc  
    ON dRo.member_principal_id = dPrinc.principal_id  
JOIN sys.database_principals AS dRole  
    ON dRo.role_principal_id = dRole.principal_id;  
```  
  
## <a name="next-steps"></a>Next Steps  
 Другие разделы, посвященные началу работы:  
  
-   [Руководство. Начало работы с ядром СУБД](../../../relational-databases/tutorial-getting-started-with-the-database-engine.md) 

-   [Создание базы данных (учебник)](../../../t-sql/lesson-1-creating-database-objects.md)  
  
-   [Руководство. SQL Server Management Studio](../../../ssms/quickstarts/connect-query-sql-server.md)  
  
-   [Руководство. Составление инструкций Transact-SQL](../../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
## <a name="see-also"></a>См. также:  
 [Центр обеспечения безопасности для базы данных Azure SQL и SQL Server Database Engine](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Функции безопасности (Transact-SQL)](../../../t-sql/functions/security-functions-transact-sql.md)   
 [Динамические административные представления и функции, связанные с безопасностью (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Представления каталога безопасности (Transact-SQL)](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Определение действующих разрешений для ядра СУБД](../../../relational-databases/security/authentication-access/determining-effective-database-engine-permissions.md)
  
