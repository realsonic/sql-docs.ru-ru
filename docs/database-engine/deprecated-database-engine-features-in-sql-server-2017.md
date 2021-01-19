---
title: Нерекомендуемые функции ядра СУБД в SQL Server 2017 | Документация Майкрософт
titleSuffix: SQL Server 2019
description: Узнайте о нерекомендуемых функциях ядра СУБД, которые по-прежнему доступны в SQL Server 2017 (14.x), но не должны использоваться в новых приложениях.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server]
- Database Engine [SQL Server], backward compatibility
- deprecation [SQL Server], feature list
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 5d9f07f96f83767a84f7ee0128d674f4ab955eed
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170856"
---
# <a name="deprecated-database-engine-features-in-sql-server-2017"></a>Нерекомендуемые функции ядра СУБД в SQL Server 2017

[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]

  В этом разделе описаны устаревшие функции компонента [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] , которые по-прежнему доступны в [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]. Не следует использовать устаревшие функции в новых приложениях.  
  
Если функция помечена как нерекомендуемая, это означает следующее:

- Функция находится в режиме обслуживания. Новые изменения, в том числе касающиеся совместимости с новыми функциями, вноситься не будут.
- Мы стараемся не удалять нерекомендуемые функции из новых выпусков, чтобы упростить обновление. Однако иногда мы можем окончательно удалять функции из [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], если они препятствуют дальнейшим инновациям.
- Нерекомендуемые функции нежелательно использовать при разработке новых приложений.      

Наблюдать за использованием устаревших функций можно с помощью объекта производительности и событий трассировки Deprecated Features, доступных в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Использование объектов SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

Значение этих счетчиков также можно получить, выполнив следующую инструкцию:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

> [!NOTE]
> Этот список идентичен списку [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]. Для [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)] не объявлено о новых нерекомендуемых или неподдерживаемых функциях ядра СУБД.

## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>Функции, не рекомендуемые в следующей версии SQL Server

Перечисленные ниже функции ядра СУБД SQL Server будут объявлены нерекомендуемыми в следующей версии SQL Server. Не используйте их при работе над новыми приложениями и как можно скорее измените приложения, в которых они в настоящее время используются. **Название функции** отображается в событиях трассировки в столбце ObjectName, а в счетчиках производительности и `sys.dm_os_performance_counters` — как имя экземпляра. Значению **Идентификатор функции** в событиях трассировки соответствует ObjectId.

### <a name="back-up-and-restore"></a>Резервное копирование и восстановление

| Устаревшая функция | Замена | Имя функции | Идентификатор функции |
|--------------------|-------------|--------------|------------|
| Инструкция RESTORE { DATABASE &#124; LOG } WITH [MEDIA]PASSWORD остается устаревшей.<br /><br />Поддержка инструкций BACKUP { DATABASE &#124; LOG } WITH PASSWORD и BACKUP { DATABASE &#124; LOG } WITH MEDIAPASSWORD прекращена. | Нет. | BACKUP DATABASE или LOG WITH PASSWORD<br /><br />BACKUP DATABASE или LOG WITH MEDIAPASSWORD | 104<br /><br /> 103 |

### <a name="compatibility-levels"></a>Уровни совместимости

| Устаревшая функция | Замена | Имя функции | Идентификатор функции |
|--------------------|-------------|--------------|------------|
Обновление с версии 100 (SQL Server 2008 и SQL Server 2008 R2). | Когда [поддержка](/lifecycle/products/?products=sql-server) версии SQL Server завершается, соответствующий уровень совместимости базы данных помечается как нерекомендуемый. Однако мы будем как можно дольше поддерживать приложения, сертифицированные для работы на любом соответствующем уровне совместимости, чтобы упростить обновление. Дополнительные сведения об уровнях совместимости см. в разделе [Уровень совместимости ALTER DATABASE (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Уровень совместимости базы данных 100 | 108 |

### <a name="database-objects"></a>Объекты базы данных

| Устаревшая функция | Замена | Имя функции | Идентификатор функции |
|--------------------|-------------|--------------|------------|
| Возможность возвращать результирующие наборы из триггеров. | None | Возврат результатов из триггера | 12 |

### <a name="encryption"></a>Шифрование

| Устаревшая функция | Замена | Имя функции | Идентификатор функции |
|--------------------|-------------|--------------|------------|
| Шифрование с использованием алгоритмов RC4 и RC4_128 является устаревшим. В следующей версии запланировано удаление его поддержки. Расшифровка с использованием алгоритмов RC4 и RC4_128 не является устаревшей. | Используйте другой алгоритм шифрования, например AES. | Устаревший алгоритм шифрования | 253 |
| Использовать MD2, MD4, MD5, SHA и SHA1 не рекомендуется. | Вместо этого используйте алгоритмы SHA2_256 или SHA2_512. Старые алгоритмы по-прежнему работают, но вызывают событие нерекомендуемого алгоритма. |Нерекомендуемый хэш-алгоритм | None |

### <a name="remote-servers"></a>Удаленные серверы

| Устаревшая функция | Замена | Имя функции | Идентификатор функции |
|--------------------|-------------|--------------|------------|
| sp_addremotelogin<br /><br />sp_addserver, хранимая процедура <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br /> хранимая процедура sp_remoteoption|Замените удаленные серверы связанными серверами. Процедуру sp_addserver можно использовать только с параметром local. | sp_addremotelogin<br /><br />sp_addserver, хранимая процедура <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br > хранимая процедура sp_remoteoption | 70 <br /><br /> 69 <br /><br /> 71 <br /><br /> 72 <br /><br /> 73 |
| \@\@remserver | Замените удаленные серверы связанными серверами. | None | None |
| SET REMOTE_PROC_TRANSACTIONS|Замените удаленные серверы связанными серверами. | SET REMOTE_PROC_TRANSACTIONS | 110 |

### <a name="transact-sql"></a>Transact-SQL

| Устаревшая функция | Замена | Имя функции | Идентификатор функции |
|--------------------|-------------|--------------|------------|
| **SET ROWCOUNT** для инструкций **INSERT**, **UPDATE** и **DELETE** . | Ключевое слово TOP | SET ROWCOUNT | 109 |
| Табличная подсказка HOLDLOCK без скобок. | Используйте HOLDLOCK со скобками. | Табличная подсказка HOLDLOCK без скобок. | 167 |

## <a name="features-deprecated-in-a-future-version-of-sql-server"></a>Функции, не рекомендуемые в будущей версии SQL Server

Перечисленные ниже функции ядра СУБД SQL Server поддерживаются в следующей версии SQL Server. Конкретная версия SQL Server пока не определена.

### <a name="back-up-and-restore"></a>Резервное копирование и восстановление

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| BACKUP { DATABASE &#124; LOG } TO TAPE <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_tape*|BACKUP { DATABASE &#124; LOG } TO DISK <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_disk* | BACKUP DATABASE или LOG TO TAPE |
| sp_addumpdevice '**tape**' | sp_addumpdevice '**disk**' | ADDING TAPE DEVICE |
| sp_helpdevice | sys.backup_devices | sp_helpdevice |

### <a name="compatibility-levels"></a>Уровни совместимости

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sp_dbcmptlevel|ALTER DATABASE ... SET COMPATIBILITY_LEVEL. Дополнительные сведения см. в разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | sp_dbcmptlevel |
| Уровень совместимости базы данных 110 и 120 | Запланируйте обновление базы данных и приложения для следующего выпуска. Однако мы будем как можно дольше поддерживать приложения, сертифицированные для работы на любом соответствующем уровне совместимости, чтобы упростить обновление. Дополнительные сведения об уровнях совместимости см. в разделе [Уровень совместимости ALTER DATABASE (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Уровень совместимости базы данных 110 <br /><br /> Уровень совместимости базы данных 120 |

### <a name="collations"></a>Параметры сортировки

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS | Нет. Эти параметры сортировки существуют в SQL Server 2005 (9.x), но их нельзя увидеть с помощью функции fn_helpcollations. | Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS |
| Hindi <br /><br /> Macedonian | Эти параметры сортировки существуют в SQL Server 2005 (9.x) и более поздних версий, но их нельзя увидеть с помощью функции fn_helpcollations. Вместо них следует использовать Macedonian_FYROM_90 и Indic_General_90.|Hindi <br /><br /> Macedonian |
| Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 | Azeri_Latin_100 <br /><br /> Azeri_Cyrilllic_100 | Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 |

### <a name="data-types"></a>Типы данных

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sp_addtype <br /><br /> хранимая процедура sp_droptype|CREATE TYPE<br /><br /> DROP TYPE | sp_addtype<br /><br /> хранимая процедура sp_droptype |
| Синтаксис **timestamp** для типа данных **rowversion** . | Синтаксис типа данных **rowversion** . | timestamp |
| Возможность вставлять значения NULL в столбцы типа **timestamp** . | Используйте вместо этого DEFAULT. | INSERT NULL в столбцах TIMESTAMP. |
| Параметр таблицы «text in row».|Используйте типы данных **varchar(max)** , **nvarchar(max)** и **varbinary(max)** . Дополнительные сведения см. в разделе [sp_tableoption (Transact-SQL)](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|Параметр таблицы «text in row» |
| Типы данных:<br /><br /> **text**<br /><br /> **ntext**<br /><br /> **image**|Используйте типы данных **varchar(max)** , **nvarchar(max)** и **varbinary(max)** .|Типы данных: **text**, **ntext** или **image** |

### <a name="database-management"></a>Управление базами данных

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sp_attach_db <br /><br /> sp_attach_single_file_db|Инструкция CREATE DATABASE с параметром FOR ATTACH. Чтобы перестроить несколько файлов журнала, если один или более файлов изменили расположение, используйте параметр FOR ATTACH_REBUILD_LOG. | sp_attach_db <br /><br /> sp_attach_single_file_db |
| sp_certify_removable<br /><br /> sp_create_removable|sp_detach_db|sp_certify_removable<br /><br /> sp_create_removable |
| sp_dbremove | DROP DATABASE | sp_dbremove |
| sp_renamedb | Параметр MODIFY NAME в инструкции ALTER DATABASE. | sp_renamedb |

### <a name="database-objects"></a>Объекты базы данных

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| CREATE DEFAULT<br /><br /> DROP DEFAULT<br /><br /> sp_bindefault<br /><br /> хранимая процедура sp_unbindefault|Ключевое слово DEFAULT в инструкциях CREATE TABLE и ALTER TABLE.|CREATE_DROP_DEFAULT<br /><br /> sp_bindefault<br /><br /> хранимая процедура sp_unbindefault |
| CREATE RULE<br /><br /> DROP RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule | Ключевое слово CHECK в инструкциях CREATE TABLE и ALTER TABLE. | CREATE_DROP_RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule |
| sp_change_users_login, хранимая процедура | Используйте команду ALTER USER. | sp_change_users_login, хранимая процедура |
| процедура sp_depends | Представления sys.dm_sql_referencing_entities и sys.dm_sql_referenced_entities. | процедура sp_depends |
| sp_getbindtoken | Использование режима MARS или распределенных транзакций. | sp_getbindtoken |

### <a name="database-options"></a>Параметры базы данных

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sp_bindsession | Использование режима MARS или распределенных транзакций. | sp_bindsession |
| sp_resetstatus | ALTER DATABASE SET { ONLINE &#124; EMERGENCY } | sp_resetstatus
| Параметр TORN_PAGE_DETECTION инструкции ALTER DATABASE.|Параметр PAGE_VERIFY TORN_PAGE_DETECTION инструкции ALTER DATABASE. | ALTER DATABASE WITH TORN_PAGE_DETECTION |

### <a name="dbcc"></a>DBCC

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| DBCC DBREINDEX|Параметр REBUILD инструкции ALTER INDEX. | DBCC DBREINDEX |
| DBCC INDEXDEFRAG|Параметр REORGANIZE инструкции ALTER INDEX | DBCC INDEXDEFRAG |
| DBCC SHOWCONTIG|sys.dm_db_index_physical_stats | DBCC SHOWCONTIG |
| DBCC PINTABLE<br /><br /> DBCC UNPINTABLE | Данный параметр не делает ничего. | DBCC [UN]PINTABLE |

### <a name="extended-properties"></a>Расширенные свойства

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Level0type = "type" и Level0type = "USER", чтобы добавить расширенные свойства к объектам типа level-1 или level-2. | Используйте Level0type = 'USER', только чтобы добавить расширенное свойство напрямую роли или пользователю.<br /><br /> Используйте Level0type = 'SCHEMA', чтобы добавить расширенное свойство к типам level-1, таким как TABLE или VIEW, или типам level-2, таким как COLUMN или TRIGGER. Дополнительные сведения см. в разделе [sp_addextendedproperty (Transact-SQL)](../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md). | EXTPROP_LEVEL0TYPE<br /><br /> EXTPROP_LEVEL0USER |

### <a name="extended-stored-procedures"></a>Расширенные хранимые процедуры

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Используйте инструкцию CREATE LOGIN.<br /><br /> Используйте аргумент DROP LOGIN IsIntegratedSecurityOnly в SERVERPROPERTY.|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginconfig |

### <a name="extended-stored-procedures-programming"></a>Программирование расширенных хранимых процедур

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| srv_alloc<br /><br /> srv_convert<br /><br /> srv_describe<br /><br /> srv_getbindtoken<br /><br /> srv_got_attention<br /><br /> srv_message_handler<br /><br /> srv_paramdata<br /><br /> srv_paraminfo<br /><br /> srv_paramlen<br /><br /> srv_parammaxlen<br /><br /> srv_paramname<br /><br /> srv_paramnumber<br /><br /> srv_paramset<br /><br /> srv_paramsetoutput<br /><br /> srv_paramstatus<br /><br /> srv_paramtype<br /><br /> srv_pfield<br /><br /> srv_pfieldex<br /><br /> srv_rpcdb<br /><br /> srv_rpcname<br /><br /> srv_rpcnumber<br /><br /> srv_rpcoptions<br /><br /> srv_rpcowner<br /><br /> srv_rpcparams<br /><br /> srv_senddone<br /><br /> srv_sendmsg<br /><br /> srv_sendrow<br /><br /> srv_setcoldata<br /><br /> srv_setcollen<br /><br /> srv_setutype<br /><br /> srv_willconvert<br /><br /> srv_wsendmsg | Используйте вместо этого интеграцию со средой CLR. | XP_API |
| sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc | Используйте вместо этого интеграцию со средой CLR. | sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc |
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Используйте инструкцию CREATE LOGIN.<br /><br /> Используйте аргумент DROP LOGIN IsIntegratedSecurityOnly в SERVERPROPERTY. | xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginconfig |

### <a name="high-availability"></a>Высокий уровень доступности

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| зеркальное отображение базы данных | Группы доступности AlwaysOn<br /><br /> Если выпуск SQL Server не поддерживает группы доступности Always On, используйте доставку журналов. | DATABASE_MIRRORING |

### <a name="index-options"></a>Параметры индекса

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sp_indexoption | ALTER INDEX | sp_indexoption |
| Синтаксис CREATE TABLE, ALTER TABLE или CREATE INDEX без заключения параметров в скобки. | Перепишите инструкции для использования текущего синтаксиса. | INDEX_OPTION |

### <a name="instance-options"></a>Параметры экземпляра

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Параметр 'allow updates' хранимой процедуры sp_configure.|Системные таблицы теперь недоступны для обновления. Параметр не делает ничего. | sp_configure 'allow updates' |
| Параметры хранимой процедуры sp_configure: <br /><br /> 'locks' <br /><br /> 'open objects'<br /><br /> 'set working set size' | Теперь настраивается автоматически. Параметр не делает ничего. | sp_configure 'locks'<br /><br /> sp_configure 'open objects'<br /><br /> sp_configure 'set working set size' |
| Параметр 'priority boost' хранимой процедуры sp_configure. | Системные таблицы теперь недоступны для обновления. Параметр не делает ничего. Используйте вместо него параметр Windows start /high ... program.exe. | sp_configure 'priority boost' |
| Параметр 'remote proc trans' хранимой процедуры sp_configure. | Системные таблицы теперь недоступны для обновления. Параметр не делает ничего. | sp_configure 'remote proc trans' |

### <a name="linked-servers"></a>Связанные серверы

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Указание поставщика SQLOLEDB для связанных серверов. | Собственный клиент SQL Server (SQLNCLI) | SQLOLEDDB для связанных серверов |

### <a name="metadata"></a>Метаданные

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| FILE_ID<br /><br /> INDEXKEY_PROPERTY | FILE_IDEX<br /><br /> sys.index_columns | FILE_ID<br /><br /> INDEXKEY_PROPERTY |

### <a name="native-xml-web-services"></a>Собственные веб-службы с поддержкой XML

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Инструкция CREATE ENDPOINT или ALTER ENDPOINT с параметром FOR SOAP.<br /><br /> sys.endpoint_webmethods<br /><br /> sys.soap_endpoints|Вместо этого следует использовать технологию WCF (Windows Communications Foundation) или ASP.NET. | CREATE/ALTER ENDPOINT<br /><br /> sys.endpoint_webmethods<br /><br /> EXT_soap_endpoints<br /><br /> sys.soap_endpoints |

### <a name="other"></a>Другие

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| DB-Library<br /><br />Embedded SQL для языка C.|Хотя ядро СУБД до сих пор поддерживает соединения из существующих приложений, использующих API DB-Library и Embedded SQL, файлы или документация, необходимые для разработки приложений с помощью этих API, не предоставляются. В следующей версии ядра СУБД SQL Server не будут поддерживаться соединения приложений DB-Library или Embedded SQL. Не используйте DB-Library или Embedded SQL для разработки новых приложений. Удалите все зависимости от DB-Library или Embedded SQL при изменении существующих приложений. Вместо этих API используйте пространство имен SQLClient или такой API, как ODBC. SQL Server 2019 (15.x) не включает DB-Library DLL, необходимую для выполнения этих приложений. Для запуска приложений DB-Library или Embedded SQL необходимо иметь доступ к DLL-библиотеке DB-Library для SQL Server 6.5, SQL Server 7.0 или SQL Server 2000 (8.x). | None |

### <a name="security"></a>Безопасность

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Синтаксис ALTER LOGIN WITH SET CREDENTIAL | Заменен новым синтаксисом ALTER LOGIN ADD и DROP CREDENTIAL | ALTER LOGIN WITH SET CREDENTIAL |
| sp_addapprole, хранимая процедура<br /><br /> sp_dropapprole, хранимая процедура | CREATE APPLICATION ROLE<br /><br /> DROP APPLICATION ROLE | sp_addapprole, хранимая процедура<br /><br /> sp_dropapprole, хранимая процедура |
| sp_addlogin<br /><br /> sp_droplogin | CREATE LOGIN<br /><br /> DROP LOGIN|sp_addlogin<br /><br /> sp_droplogin |
| sp_adduser<br /><br /> sp_dropuser | CREATE USER<br /><br /> DROP USER|sp_adduser<br /><br /> sp_dropuser |
| sp_grantdbaccess<br /><br /> sp_revokedbaccess|CREATE USER<br /><br /> DROP USER|sp_grantdbaccess<br /><br /> sp_revokedbaccess |
| хранимая процедура sp_addrole<br /><br /> sp_droprole | CREATE ROLE<br /><br /> DROP ROLE|хранимая процедура sp_addrole<br /><br /> sp_droprole |
| sp_approlepassword<br /><br /> sp_password | ALTER APPLICATION ROLE<br /><br /> ALTER LOGIN|sp_approlepassword<br /><br /> sp_password |
| sp_changedbowner|ALTER AUTHORIZATION | sp_changedbowner |
| sp_changeobjectowner|ALTER SCHEMA или ALTER AUTHORIZATION | sp_changeobjectowner |
| sp_control_dbmasterkey_password | Необходим главный ключ и правильный пароль.|sp_control_dbmasterkey_password |
| sp_defaultdb<br /><br /> sp_defaultlanguage | ALTER LOGIN|sp_defaultdb<br /><br /> sp_defaultlanguage |
| sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|ALTER LOGIN DISABLE<br /><br /> CREATE LOGIN<br /><br /> DROP LOGIN|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin |
| USER_ID|DATABASE_PRINCIPAL_ID | USER_ID |
| sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|Эти хранимые процедуры возвращают данные, которые были правильными в [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]. Выходные данные не отражают изменений в иерархии разрешений, реализованной в SQL Server 2008. Дополнительные сведения см. в разделе [Разрешения предопределенных ролей сервера](https://msdn.microsoft.com/library/ms175892\(SQL.100\).aspx). | sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission |
| GRANT ALL<br /><br /> DENY ALL<br /><br /> REVOKE ALL|Специальные разрешения GRANT, DENY и REVOKE.|Разрешение ALL. |
| Внутренняя функция PERMISSIONS. | Запросите sys.fn_my_permissions. | PERMISSIONS |
| SETUSER | EXECUTE AS | SETUSER |
| Алгоритмы шифрования RC4 и DESX|Используйте другой алгоритм, например AES. | Алгоритм DESX |

### <a name="server-configuration-options"></a>Параметры конфигурации сервера

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| параметр "c2 audit" параметр "default trace enabled"<br /><br /> default trace enabled, параметр | [Параметр конфигурации сервера common criteria compliance enabled](../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)<br /><br /> [Расширенные события](../relational-databases/extended-events/extended-events.md) | sp_configure 'c2 audit mode'<br /><br />sp_configure 'default trace enabled' |

### <a name="smo-classes"></a>Классы модели объектов SMO

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| *Microsoft.SQLServer. Класс Management.Smo.Information*<br /><br />*Microsoft.SQLServer. Класс Management.Smo.Settings*<br /><br />*Microsoft.SQLServer.Management. Класс Smo.DatabaseOptions*<br /><br />*Microsoft.SqlServer.Management.Smo. Свойство DatabaseDdlTrigger.NotForReplication* | *Microsoft.SqlServer.  Класс Management.Smo.Server*<br /><br />Класс **Microsoft.SqlServer.  Management.Smo.Server* Класс <br /><br />* Microsoft.SqlServer. Management.Smo.Database*<br /><br />None | None |

### <a name="sql-server-agent"></a>Агент SQL Server

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| уведомление **net send** .<br /><br />Уведомление по пейджеру | Уведомление по электронной почте<br /><br />Уведомление по электронной почте | None |

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Интеграция обозревателя решений в SQL Server Management Studio | | None |

### <a name="system-stored-procedures-and-functions"></a>Системные хранимые процедуры и функции

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sp_db_increased_partitions | Нет. Поддержка увеличенных секций в SQL Server 2019 (15.x) доступна по умолчанию. | sp_db_increased_partitions |
| fn_virtualservernodes<br /><br />fn_servershareddrives | sys.dm_os_cluster_nodes<br /><br />sys.dm_io_cluster_shared_drives | fn_virtualservernodes<br /><br /> fn_servershareddrives |
| fn_get_sql | sys.dm_exec_sql_text | fn_get_sql |
| sp_lock | sys.dm_tran_locks | sp_lock |

### <a name="system-tables"></a>Системные таблицы

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers|Представления совместимости. Дополнительные сведения см. в разделе [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md).<br /><br />**Внимание!** Представления совместимости не предоставляют доступ к метаданным для функций, которые использовались в SQL Server 2005 (9.x). Рекомендуется обновить приложения, чтобы они использовали представления каталога. Дополнительные сведения см. в разделе [Представления каталога (Transact-SQL)](../relational-databases/system-catalog-views/catalog-views-transact-sql.md). | sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers |
| sys.numbered_procedures<br /><br />sys.numbered_procedure_parameters | None | numbered_procedures<br /><br />numbered_procedure_parameters |

### <a name="sql-trace-stored-procedures-functions-and-catalog-views"></a>Хранимые процедуры, функции и представления каталогов трассировки SQL

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| хранимая процедура sp_trace_create<br /><br />sp_trace_setevent, хранимая процедура<br /><br />sp_trace_setfilter, хранимая процедура<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values|[Расширенные события](../relational-databases/extended-events/extended-events.md) | хранимая процедура sp_trace_create<br /><br />sp_trace_setevent, хранимая процедура<br /><br />sp_trace_setfilter, хранимая процедура<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values |

### <a name="system-views"></a>Системные представления

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| sys.sql_dependencies|sys.sql_expression_dependencies|sys.sql_dependencies |

### <a name="table-compression"></a>Сжатие таблицы

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Использование формата хранения vardecimal. | Формат хранения Vardecimal устарел. Средства сжатия данных в SQL Server 2019 (15.x) обеспечивают упаковку десятичных значений и данных других типов. Вместо формата хранения vardecimal рекомендуется использовать сжатие данных. | Формат хранения vardecimal |
| Используйте процедуру the sp_db_vardecimal_storage_format.|Формат хранения Vardecimal устарел. Средства сжатия данных в SQL Server 2019 (15.x) обеспечивают упаковку десятичных значений и данных других типов. Вместо формата хранения vardecimal рекомендуется использовать сжатие данных. | sp_db_vardecimal_storage_format |
| Используйте процедуру sp_estimated_rowsize_reduction_for_vardecimal.|Вместо этого следует использовать сжатие данных и процедуру sp_estimate_data_compression_savings. |sp_estimated_rowsize_reduction_for_vardecimal |

### <a name="text-pointers"></a>Текстовые указатели

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| WRITETEXT<br /><br />UPDATETEXT<br /><br />READTEXT|None|UPDATETEXT или WRITETEXT<br /><br />READTEXT |
| TEXTPTR()<br /><br />TEXTVALID() | None | TEXTPTR<br /><br />TEXTVALID |

### <a name="transact-sql"></a>Transact-SQL

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| :: последовательность вызова функций | Заменено на SELECT *column_list* FROM sys.\<*function_name*>().<br /><br />Например, замените `SELECT * FROM ::fn_virtualfilestats(2,1)` на `SELECT * FROM sys.fn_virtualfilestats(2,1)`. | синтаксис вызова функции «::» |
| Ссылки на столбцы с трех- и четырехкомпонентными именами. | Использование двухкомпонентных имен совместимо со стандартом.|Имя столбца, состоящее более чем из двух компонентов |
| Строка, заключенная в кавычки, использовалась как псевдоним столбца для выражения в списке SELECT:<br /><br />'*string_alias*' = *выражение* | *expression* [AS] *псевдоним_столбца*<br /><br />*expression* [AS] [*псевдоним_столбца*]<br /><br />*expression* [AS] "*псевдоним_столбца*"<br /><br />*expression* [AS] '*псевдоним_столбца*'<br /><br />*column_alias* = *выражение* | Строковые литералы в качестве псевдонимов столбцов |
| Нумерованные процедуры | Нет. Не используйте. | ProcNums |
| Синтаксис *table_name.index_name* в инструкции DROP INDEX|Синтаксис *index_name* ON *table_name* в инструкции DROP INDEX.|DROP INDEX с двухкомпонентным именем |
| Инструкции Transact-SQL не заканчиваются точкой с запятой.|Инструкции Transact-SQL заканчиваются точкой с запятой ( ; ). | None |
| GROUP BY ALL|Используйте решение с оператором UNION или производной таблицей для каждого случая отдельно. | GROUP BY ALL |
| ROWGUIDCOL в качестве имени столбца в инструкциях DML.|Используйте $rowguid.|ROWGUIDCOL |
| IDENTITYCOL в качестве имени столбца в инструкциях DML.|Используйте $identity.|IDENTITYCOL |
| Использование # и ## в качестве имен временной таблицы и временной хранимой процедуры. | Используйте по крайней мере один дополнительный символ.|Символы «#» и «##» в качестве имен временных таблиц и хранимых процедур
| Используйте \@, \@\@ или \@\@ в качестве идентификаторов Transact-SQL. | Не используйте в качестве идентификаторов \@ или \@\@, а также имена, начинающиеся символами \@\@. | "\@" и имена, начинающиеся с "\@\@", в качестве идентификаторов Transact-SQL |
| Используйте ключевое слово DEFAULT в качестве значения по умолчанию.|Не используйте слово DEFAULT в качестве значения по умолчанию. | Ключевое слово DEFAULT в качестве значения по умолчанию. |
| Использование пробела в качестве разделителя табличных подсказок.|В качестве разделителя отдельных табличных подсказок используйте запятую. | Несколько табличных указаний без запятых |
| Список выбора статистического индексированного представления должен содержать функцию COUNT_BIG (\*) в режиме совместимости 90. | Вместо этого следует использовать функцию COUNT_BIG (\*). | Индексированное представление выбирает список без COUNT_BIG(\*) |
| Косвенное применение табличных указаний для вызова функций с несколькими инструкциями, возвращающих табличное значение (TVF), через представление.|Нет.|Косвенные подсказки возвращающих табличное значение функций. |
| Синтаксис ALTER DATABASE:<br /><br />MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE | MODIFY FILEGROUP READ_ONLY<br /><br />MODIFY FILEGROUP READ_WRITE | MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE |
| Параметр базы данных SET ANSI_NULLS OFF и ANSI_NULLS OFF.<br /><br />Параметр базы данных SET ANSI_PADDING OFF и ANSI_PADDING OFF.<br /><br />Параметр базы данных SET CONCAT_NULL_YIELDS_NULL OFF и CONCAT_NULL_YIELDS_NULL OFF.<br /><br />SET OFFSETS | Нет. <br /><br /> Параметры ANSI_NULLS, ANSI_PADDING и CONCAT_NULLS_YIELDS_NULL всегда имеют значение ON. Параметр SET OFFSETS недоступен. | SET ANSI_NULLS OFF <br /><br /> SET ANSI_PADDING OFF<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS<br /><br />ALTER DATABASE SET ANSI_NULLS OFF<br /><br />ALTER DATABASE SET ANSI_PADDING OFF <br /><br /> ALTER DATABASE SET CONCAT_NULL_YIELDS_NULL OFF |
| SET FMTONLY | [sys.dm_exec_describe_first_result_set (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md), [sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md), [sp_describe_first_result_set (Transact-SQL)](../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) и [sp_describe_undeclared_parameters (Transact-SQL)](../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md). | SET FMTONLY |
| Указание параметра NOLOCK или READUNCOMMITTED в предложении FROM инструкции UPDATE или DELETE. | Удалите табличные указания NOLOCK и READUNCOMMITTED из предложения FROM. | NOLOCK или READUNCOMMITTED в инструкции UPDATE или DELETE |
| Указание табличных подсказок без ключевого слова WITH. | Использование ключевого слова WITH. | Табличное указание без ключевого слова WITH |
| INSERT_HINTS | | INSERT_HINTS |

### <a name="tools"></a>Инструменты

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Приложение SQL Server Profiler для перехвата трассировки | Использование профилировщика расширенных событий, встроенного в среду SQL Server Management Studio.|Приложение SQL Server Profiler |
| Воспроизведение трассировки с помощью приложения SQL Server Profiler | [Распределенное воспроизведение SQL Server](../tools/distributed-replay/sql-server-distributed-replay.md) |

### <a name="trace-management-objects"></a>Объекты TMO

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Пространство имен Microsoft.SqlServer.Management.Trace (содержит API для объектов Trace и Replay в SQL Server)|Настройка трассировки: <xref:Microsoft.SqlServer.Management.XEvent><br /><br />Чтение трассировки: <xref:Microsoft.SqlServer.XEvent.Linq><br /><br />Воспроизведение трассировки: None | |

### <a name="xml"></a>XML

| Устаревшая функция | Замена | Имя функции |
|--------------------|-------------|--------------|
| Создание встроенных схем XDR | Директива XMLDATA для параметра XML FOR является устаревшей. В режимах RAW и AUTO следует использовать создание XSD-схем. В режиме EXPLICT для директивы XMLDATA замены нет. | XMLDATA |

> [!NOTE]
> Параметр **OUTPUT** куки-файла для инструкции **sp_setapprole** в настоящее время описан в документации как **varbinary(8000)** , что верно определяет его максимальную длину. Однако текущая реализация возвращает параметр **varbinary(50)** . Если разработчик выделил значение **varbinary(50)** , может потребоваться внести изменения в приложения на случай изменения размера возвращаемых куки-файлов в будущих выпусках. Хотя эта проблема не связана с устареванием, она описана в данном разделе, так как требует внесения аналогичных изменений в приложения. Дополнительные сведения см. в разделе [sp_setapprole (Transact-SQL)](../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md).  
  
## <a name="see-also"></a>См. также:  
 [Неподдерживаемые функции ядра СУБД в SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)  
