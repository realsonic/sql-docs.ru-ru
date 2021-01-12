---
description: sys.dm_exec_sessions (Transact-SQL)
title: sys.dm_exec_sessions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_sessions_TSQL
- sys.dm_exec_sessions
- dm_exec_sessions
- sys.dm_exec_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sessions dynamic management view
ms.assetid: 2b7e8e0c-eea0-431e-819f-8ccd12ec8cfa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 49f04a21e95c0cc7a63b97fd9fd4add1729b25a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099005"
---
# <a name="sysdm_exec_sessions-transact-sql"></a>sys.dm_exec_sessions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает по одной строке на каждый прошедший проверку подлинности сеанс, подключенный к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_exec_sessions — это представление области сервера, в котором отображаются сведения обо всех активных подключениях пользователей и внутренних задачах. Эти сведения включают в себя данные о версии клиента, имени клиентской программы, времени входа, имени входа пользователя, текущих настройках сеанса и т.д. Для первоначального ознакомления с текущей нагрузкой системы и определения интересующего сеанса используйте представление sys.dm_exec_sessions, после чего более подробные сведения об этом сеансе могут быть получены с помощью других динамических административных представлений или функций динамического управления.  
  
 Динамические административные представления sys.dm_exec_connections, sys.dm_exec_sessions и sys.dm_exec_requests сопоставляются с системной таблицей [sys.sys](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) .  
  
> **Примечание.** Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_exec_sessions**.  
  
|Имя столбца|Тип данных|Описание и сведения о конкретной версии|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Идентификатор сеанса, связанный со всеми активными первичными соединениями. Не допускает значение NULL.|  
|login_time|**datetime**|Время подключения сеанса. Не допускает значение NULL. Сеансы, для которых не завершен вход в систему во время запроса этого динамического административного представления, отображаются с временем входа `1900-01-01` .|  
|host_name|**nvarchar(128)**|Имя клиентской рабочей станции, указанное в сеансе. Для внутреннего сеанса это значение равно NULL. Допускает значение NULL.<br /><br /> **Примечание по безопасности.** Клиентское приложение предоставляет имя рабочей станции и может предоставлять неточные данные. Не следует полагаться на функцию HOST_NAME для обеспечения безопасности.|  
|program_name|**nvarchar(128)**|Имя клиентской программы, которая инициировала сеанс. Для внутреннего сеанса это значение равно NULL. Допускает значение NULL.|  
|host_process_id|**int**|Идентификатор процесса клиентской программы, которая инициировала сеанс. Для внутреннего сеанса это значение равно NULL. Допускает значение NULL.|  
|client_version|**int**|Версия TDS-протокола интерфейса, который используется клиентом для подключения к серверу. Для внутреннего сеанса это значение равно NULL. Допускает значение NULL.|  
|client_interface_name|**nvarchar(32)**|Имя библиотеки или драйвера, используемое клиентом для обмена данными с сервером. Для внутреннего сеанса это значение равно NULL. Допускает значение NULL.|  
|security_id|**varbinary(85)**|Идентификатор безопасности Microsoft Windows, связанный с именем входа. Не допускает значение NULL.|  
|login_name|**nvarchar(128)**|Имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], под которым выполняется текущий сеанс. Чтобы узнать первоначальное имя входа, с помощью которого был создан сеанс, см. параметр original_login_name. Может быть именем входа, прошедшим проверку подлинности, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или именем пользователя домена, прошедшего проверку подлинности Windows. Не допускает значение NULL.|  
|nt_domain|**nvarchar(128)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Домен Windows для клиента, если во время сеанса применяется проверка подлинности Windows или доверительное соединение. Для внутренних сеансов и пользователей, не принадлежащих к домену, это значение равно NULL. Допускает значение NULL.|  
|nt_user_name|**nvarchar(128)**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Имя пользователя Windows для клиента, если во время сеанса используется проверка подлинности Windows или доверительное соединение. Для внутренних сеансов и пользователей, не принадлежащих к домену, это значение равно NULL. Допускает значение NULL.|  
|status|**nvarchar(30)**|Состояние сеанса. Возможные значения:<br /><br /> **Running** — в данный момент выполняется один или несколько запросов.<br /><br /> **Sleeping** — в данный момент запросы не выполняются.<br /><br /> **Неактивный** сеанс был сброшен из-за пула соединений и теперь находится в состоянии предварительного входа.<br /><br /> **Preconnect** — Сеанс находится в классификаторе регулятора ресурсов.<br /><br /> Не допускает значение NULL.|  
|context_info|**varbinary(128)**|Значение параметра CONTEXT_INFO для сеанса. Сведения о контексте задаются пользователем с помощью инструкции [set CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md) . Допускает значение NULL.|  
|cpu_time|**int**|Время ЦП, использованное данным сеансом, в миллисекундах. Не допускает значение NULL.|  
|memory_usage|**int**|Количество 8-килобайтовых страниц памяти, используемых данным сеансом. Не допускает значение NULL.|  
|total_scheduled_time|**int**|Общее время, назначенное данному сеансу (включая его вложенные запросы) для исполнения, в миллисекундах. Не допускает значение NULL.|  
|total_elapsed_time|**int**|Время, прошедшее с момента установки сеанса в миллисекундах. Не допускает значение NULL.|  
|endpoint_id|**int**|Идентификатор конечной точки, связанный с сеансом. Не допускает значение NULL.|  
|last_request_start_time|**datetime**|Время, когда начался последний запрос данного сеанса. Это может быть запрос, выполняющийся в данный момент. Не допускает значение NULL.|  
|last_request_end_time|**datetime**|Время завершения последнего запроса в рамках данного сеанса. Допускает значение NULL.|  
|reads|**bigint**|Количество операций чтения, выполненных запросами данного сеанса. Не допускает значение NULL.|  
|writes|**bigint**|Количество операций записи, выполненных запросами данного сеанса. Не допускает значение NULL.|  
|logical_reads|**bigint**|Количество логических операций чтения, выполненных в данном сеансе. Не допускает значение NULL.|  
|is_user_process|**bit**|0, если сеанс является системным. В противном случае значение равно 1. Не допускает значение NULL.|  
|text_size|**int**|Значение параметра TEXTSIZE для данного сеанса. Не допускает значение NULL.|  
|язык|**nvarchar(128)**|Значение параметра LANGUAGE для данного сеанса. Допускает значение NULL.|  
|date_format|**nvarchar (3)**|Значение параметра DATEFORMAT для данного сеанса. Допускает значение NULL.|  
|date_first|**smallint**|Значение параметра DATEFIRST для данного сеанса. Не допускает значение NULL.|  
|quoted_identifier|**bit**|Значение параметра QUOTED_IDENTIFIER для данного сеанса. Не допускает значение NULL.|  
|arithabort|**bit**|Значение параметра ARITHABORT для данного сеанса. Не допускает значение NULL.|  
|ansi_null_dflt_on|**bit**|Значение параметра ANSI_NULL_DFLT_ON для данного сеанса. Не допускает значение NULL.|  
|ansi_defaults|**bit**|Значение параметра ANSI_DEFAULTS для данного сеанса. Не допускает значение NULL.|  
|ansi_warnings|**bit**|Значение параметра ANSI_WARNINGS для данного сеанса. Не допускает значение NULL.|  
|ansi_padding|**bit**|Значение параметра ANSI_PADDING для данного сеанса. Не допускает значение NULL.|  
|ansi_nulls|**bit**|Значение параметра ANSI_NULLS для данного сеанса. Не допускает значение NULL.|  
|concat_null_yields_null|**bit**|Значение параметра CONCAT_NULL_YIELDS_NULL для данного сеанса. Не допускает значение NULL.|  
|transaction_isolation_level|**smallint**|Уровень изоляции транзакции данного сеанса:<br /><br /> 0 = не указан;<br /><br /> 1 = ReadUncommitted<br /><br /> 2 = читать зафиксированные;<br /><br /> 3 = RepeatableRead<br /><br /> 4 = сериализуемые;<br /><br /> 5 = моментальный снимок.<br /><br /> Не допускает значение NULL.|  
|lock_timeout|**int**|Значение параметра LOCK_TIMEOUT для данного сеанса. Значение указывается в миллисекундах. Не допускает значение NULL.|  
|deadlock_priority|**int**|Значение параметра DEADLOCK_PRIORITY для данного сеанса. Не допускает значение NULL.|  
|row_count|**bigint**|Количество строк, возвращенных сеансом на текущий момент времени. Не допускает значение NULL.|  
|prev_error|**int**|Идентификатор последней ошибки, возвращенной в данном сеансе. Не допускает значение NULL.|  
|original_security_id|**varbinary(85)**|Идентификатор безопасности [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, связанный с параметром original_login_name. Не допускает значение NULL.|  
|original_login_name|**nvarchar(128)**|Имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с помощью которого клиент создал данный сеанс. Это может быть имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], прошедшее проверку подлинности, имя пользователя домена Windows, прошедшее проверку подлинности, или пользователь автономной базы данных. Обратите внимание, что после первоначального соединения для сеанса может быть выполнено много неявных или явных переключений контекста. Например, если используется параметр [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) . Не допускает значение NULL.|  
|last_successful_logon|**datetime**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Время последнего успешного входа в систему для имени original_login_name до запуска текущего сеанса.|  
|last_unsuccessful_logon|**datetime**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Время последнего неуспешного входа в систему для имени original_login_name до запуска текущего сеанса.|  
|unsuccessful_logons|**bigint**|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> Число неуспешных попыток входа в систему для имени original_login_name между временем last_successful_logon и временем login_time.|  
|group_id|**int**|Идентификатор группы рабочей нагрузки, которой принадлежит этот сеанс. Не допускает значение NULL.|  
|database_id|**smallint**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Идентификатор текущей базы данных для каждого сеанса.|  
|authenticating_database_id|**int**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Идентификатор базы данных, выполняющей проверку подлинности участника. Для имен входа это значение будет равно 0. Для пользователей автономной базы данных это значение будет содержать идентификатор автономной базы данных.|  
|open_transaction_count|**int**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Количество открытых транзакций на сеанс.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
|page_server_reads|**bigint**|Область **применения**: масштабирование базы данных SQL Azure<br /><br /> Число операций чтения сервера страниц, выполненных запросами в этом сеансе во время этого сеанса. Не допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
Все могут просматривать свои сведения о сеансе.  
**[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] :** `VIEW SERVER STATE` [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Для просмотра всех сеансов на сервере требуется разрешение.  
**[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] :** Требуется `VIEW DATABASE STATE` для просмотра всех подключений к текущей базе данных. `VIEW DATABASE STATE` не может быть предоставлено в `master` базе данных. 
  
  
## <a name="remarks"></a>Комментарии  
 Если включен параметр конфигурации сервера « **стандартные условия соответствия** », статистика входа отображается в следующих столбцах.  
  
-   last_successful_logon  
  
-   last_unsuccessful_logon  
  
-   unsuccessful_logons  
  
 Если этот параметр не включен, то данные столбцы будут возвращать значения NULL. Дополнительные сведения о том, как настроить этот параметр конфигурации сервера, см. в разделе [Common условия_отбора with Enabled Configuration параметр конфигурации сервера](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md).  
 
 
 Административные соединения в базе данных SQL Azure будут видеть одну строку для каждого сеанса, прошедшего проверку подлинности. Сеансы SA, которые отображаются в результирующем наборе, не влияют на квоту пользователя на сеансы. Подключения без прав администратора увидят только сведения, связанные с пользовательскими сеансами базы данных.
 
  
## <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Подключить/Применить|Relationship|  
|----------|--------|---------------|------------------|  
|sys.dm_exec_sessions|[sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|session_id|Один к нулю или один ко многим.|  
|sys.dm_exec_sessions|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)|session_id|Один к нулю или один ко многим.|  
|sys.dm_exec_sessions|[sys.dm_tran_session_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)|session_id|Один к нулю или один ко многим.|  
|sys.dm_exec_sessions|[sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)(session_id &#124; 0)|session_id CROSS APPLY<br /><br /> OUTER APPLY|Один к нулю или один ко многим.|  
|sys.dm_exec_sessions|[sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)|session_id|"Одна к одной"|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-finding-users-that-are-connected-to-the-server"></a>A. Поиск пользователей, подключенных к серверу  
 В следующем примере производится поиск пользователей, подключенных к серверу, и возвращаются сведения о числе сеансов для каждого пользователя.  
  
```sql  
SELECT login_name ,COUNT(session_id) AS session_count   
FROM sys.dm_exec_sessions   
GROUP BY login_name;  
```  
  
### <a name="b-finding-long-running-cursors"></a>Б. Поиск курсоров, выполняющихся продолжительное время  
 В следующем примере производится поиск курсоров, открытых дольше заданного периода времени, определяются их создатели и соответствующие им сеансы.  
  
```sql  
USE master;  
GO  
SELECT creation_time ,cursor_id   
    ,name ,c.session_id ,login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s   
   ON c.session_id = s.session_id   
WHERE DATEDIFF(mi, c.creation_time, GETDATE()) > 5;  
```  
  
### <a name="c-finding-idle-sessions-that-have-open-transactions"></a>В. Поиск бездействующих сеансов, имеющих открытые транзакции  
 В следующем производится поиск сеансов, имеющих открытые транзакции, но при этом бездействующих. Бездействующим сеансом считается сеанс, который в настоящий момент не выполняет запросов.  
  
```sql  
SELECT s.*   
FROM sys.dm_exec_sessions AS s  
WHERE EXISTS   
    (  
    SELECT *   
    FROM sys.dm_tran_session_transactions AS t  
    WHERE t.session_id = s.session_id  
    )  
    AND NOT EXISTS   
    (  
    SELECT *   
    FROM sys.dm_exec_requests AS r  
    WHERE r.session_id = s.session_id  
    );  
```  
  
### <a name="d-finding-information-about-a-queries-own-connection"></a>Г. Поиск сведений о собственном соединении запросов  
 Типичный запрос для сбора сведений о собственном соединении запросов.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с выполнением (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



