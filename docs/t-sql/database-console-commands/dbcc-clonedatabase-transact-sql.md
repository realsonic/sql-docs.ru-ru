---
description: DBCC CLONEDATABASE (Transact-SQL)
title: DBCC CLONEDATABASE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CLONEDATABASE
- CLONE DATABASE
- DBCC_CLONEDATABASE_TSQL
- DBCC CLONEDATABASE
- DBCC CLONE DATABASE
- CLONEDATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database cloning [SQL Server]
- cloning databases
- clone databases
- cloning database
- clone database
- copying databases
- copy databases
- copying database
- copy database
- NO_STATISTICS option
- NO_QUERYSTORE option
- VERIFY_CLONEDB option
- BACKUP_CLONEDB option
- database copying [SQL Server]
- database cloning [SQL Server]
- DBCC CLONEDATABASE statement
ms.assetid: ''
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: 2de4f0e84b39d1384e342eab3b7b3d0bfd101611
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172586"
---
# <a name="dbcc-clonedatabase-transact-sql"></a>DBCC CLONEDATABASE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

С помощью инструкции DBCC CLONEDATABASE можно клонировать только схему базы данных с целью анализа проблем производительности, связанных с оптимизатором запросов.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DBCC CLONEDATABASE   
(  
    source_database_name
    ,  target_database_name
)
    [ WITH { [ NO_STATISTICS ] [ , NO_QUERYSTORE ] [ , VERIFY_CLONEDB | SERVICEBROKER ] [ , BACKUP_CLONEDB ] } ]     
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*<имя_исходной_базы>*  
Имя копируемой базы данных. 
  
*target_database_name*  
Имя базы данных, в которую будет скопирована исходная база данных. Эта база данных будет создана инструкцией DBCC CLONEDATABASE, и не должна существовать на момент ее выполнения. 
  
NO_STATISTICS  
Указывает, необходимо ли исключить статистику по таблицам или индексу из клона. Если этот параметр не задан, статистика по таблицам или индексу включается автоматически. Этот параметр доступен начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и накопительным пакетом обновления 3 и [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1).

NO_QUERYSTORE<br>
Указывает, нужно ли исключить данные хранилища запросов из клона. Если этот параметр не задан, а в базе данных-источнике включено хранилище запросов, его данные копируются в клон. Этот параметр доступен начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1).

VERIFY_CLONEDB  
Проверяет согласованность новой базы данных.  Этот параметр обязателен, если клонируемая база данных предназначена для использования в рабочей среде.  Включение параметра VERIFY_CLONEDB также отключает сбор статистики и данных хранилища запросов, то есть эквивалентно выполнению инструкции с параметрами WITH VERIFY_CLONEDB, NO_STATISTICS, NO_QUERYSTORE.  Этот параметр доступен начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 3 (SP3), [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2) и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления 8 (CU8).

> [!NOTE]  
> С помощью следующей команды можно проверить, готова ли клонированная база данных к использованию в рабочей среде: <br/>`SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone')`


SERVICEBROKER<br>
Указывает, следует ли включать в клон связанные системные каталоги Service Broker.  Параметр SERVICEBROKER недопустимо использовать с VERIFY_CLONEDB.  Этот параметр доступен начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 3 (SP3), [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2) и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления 8 (CU8).

BACKUP_CLONEDB  
Создает и проверяет резервную копию клонированной базы данных.  При использовании в сочетании с параметром VERIFY_CLONEDB клонированная база данных проверяется перед созданием резервной копии.  Этот параметр доступен начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 3 (SP3), [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2) и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления 8 (CU8).
  
## <a name="remarks"></a>Комментарии
Инструкция DBCC CLONEDATABASE выполняет указанные ниже проверки. Если хотя бы одна из них не пройдена, команда завершается сбоем.
- База данных-источник должна быть пользовательской базой данных. Клонировать системные базы данных (master, msdb, tempdb, шаблон базы данных, базу данных распространителя и т. д.) нельзя.
- База данных-источник должна находиться в режиме "в сети" и быть доступной для чтения.
- Не должно существовать базы данных с тем же именем, что и у клонированной базы данных.
- Команда не включена в пользовательскую транзакцию.

Если все проверки пройдены успешно, база данных-источник клонируется, для чего выполняются перечисленные ниже операции.
- Создается конечная база данных с той же структурой файлов, что и в базе данных-источнике, но с размерами файлов по умолчанию из шаблона базы данных.
- Создается внутренний моментальный снимок базы данных-источника.
- Системные метаданные копируются из базы данных-источника в конечную базу данных.
- Схема всех объектов копируется из базы данных-источника в конечную базу данных.
- Статистика всех индексов копируется из базы данных-источника в конечную базу данных.

> [!NOTE]  
> Новая база данных, созданная инструкцией DBCC CLONEDATABASE, предназначена в первую очередь для устранения неполадок и диагностики.  Чтобы клонированную базу данных можно было использовать как рабочую, необходимо задать параметр VERIFY_CLONEDB.

Все файлы в конечной базе данных наследуют размер и параметры увеличения размера от шаблона базы данных. Имена файлов в конечной базе данных следуют соглашению "имя исходного файла_случайное число". Если файл с таким именем уже существует в конечной папке, инструкция DBCC CLONEDATABASE завершится сбоем.

Инструкция DBCC CLONEDATABASE не поддерживает создание клона, если в шаблоне базы данных были созданы какие-либо пользовательские объекты (таблицы, индексы, схемы, роли и т. д.). Если в шаблоне базы данных есть пользовательские объекты, клонирование базы данных завершается сбоем со следующим сообщением об ошибке:

```
Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object <system table> with unique index 'index name'. The duplicate key value is <key value>   
```

> [!IMPORTANT]
> Если у вас есть индексы columnstore, см. запись блога [Настройка запросов с индексами columnstore в клонированных базах данных](https://techcommunity.microsoft.com/t5/SQL-Server/Considerations-when-tuning-your-queries-with-columnstore-indexes/ba-p/385294), чтобы узнать, как обновить статистику индексов columnstore перед выполнением команды **DBCC CLONEDATABASE**.  Начиная с SQL Server 2019 вручную выполнять действия, описанные в этой статье, не нужно, так как команда **DBCC CLONEDATABASE** собирает эти сведения автоматически.

<a name="ctp23"></a>

## <a name="stats-blob-for-columnstore-indexes"></a>BLOB-объект статистики для индексов columnstore

[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], `DBCC CLONEDATABASE` автоматически собирает большие двоичные объекты статистики индексов columnstore, что позволяет не выполнять дополнительные действия вручную.`DBCC CLONEDATABASE` позволяет создать копию только схемы базы данных, содержащую все элементы, необходимые для устранения проблем с производительностью запросов без копирования данных. В предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] эта команда не копировала данные статистики, необходимые для точного устранения неполадок с запросами индекса columnstore, и для получения этих сведений требовалось выполнять некоторые действия вручную.

Сведения о безопасности данных в клонированных базах данных см. в записи блога [Сведения о безопасности данных в клонированных базах данных](https://techcommunity.microsoft.com/t5/SQL-Server/Understanding-data-security-in-cloned-databases-created-using/ba-p/385287).

## <a name="internal-database-snapshot"></a>Моментальный снимок внутренней базы данных
Инструкция DBCC CLONEDATABASE использует внутренний моментальный снимок базы данных-источника с целью обеспечения согласованности транзакций, необходимой для выполнения копирования. Тем самым предотвращаются проблемы блокировки и параллелизма при выполнении этих команд. Если создать моментальный снимок невозможно, команда DBCC CLONEDATABASE завершается сбоем. 

Блокировки на уровне базы данных устанавливаются во время следующих этапов процесса копирования:
- проверка базы данных-источника;
- получение блокировки S для базы данных-источника;
- создание моментального снимка базы данных-источника;
- создание клонированной базы данных (пустой базы данных, унаследованной от шаблона базы данных);
- получение блокировки X для клонированной базы данных;
- копирование метаданных в клонированную базу данных;
- снятие всех блокировок с базы данных.

Как только выполнение команды завершается, внутренний моментальный снимок удаляется. Параметры TRUSTWORTHY и DB_CHAINING для клонированной базы данных отключены. 

## <a name="supported-objects"></a>Поддерживаемые объекты
В конечной базе данных можно клонировать только перечисленные ниже объекты. Зашифрованные объекты клонируются, но недоступны для использования в клонированной базе данных. Объекты, отсутствующие в приведенном ниже списке, не поддерживаются в клоне. 
- APPLICATION ROLE
- AVAILABILITY GROUP
- COLUMNSTORE INDEX
- CDB
- CDC
- CLR (начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и накопительным пакетом обновления 3 и [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1))
- Свойства базы данных
- DEFAULT
- Файлы и файловые группы
- Полный текст (начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1) и накопительным пакетом обновления 2)
- FUNCTION
- INDEX
- Имя_для_входа
- PARTITION FUNCTION
- PARTITION SCHEME
- PROCEDURE   
> [!NOTE]   
> Процедуры [!INCLUDE[tsql](../../includes/tsql-md.md)] поддерживаются во всех выпусках начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2). Процедуры CLR поддерживаются начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и накопительным пакетом обновления 3. Скомпилированные в собственном коде процедуры поддерживаются начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1).  

- QUERY STORE (начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1))   
> [!NOTE]   
> Данные хранилища запросов копируются, только если их сбор включен в базе данных-источнике. Чтобы скопировать последнюю статистику времени выполнения из хранилища запросов, перед выполнением команды DBCC CLONEDATABASE выполните процедуру sp_query_store_flush_db, чтобы сбросить статистику времени выполнения в хранилище запросов.  

- ROLE
- RULE
- SCHEMA
- SEQUENCE
- SPATIAL INDEX
- STATISTICS
- SYNONYM
- TABLE
- Оптимизированные для памяти таблицы (только в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1) и более поздних версиях).
- Объекты FileStream и FileTable (начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и накопительным пакетом обновления 3 и [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1)) 
- TRIGGER
- TYPE
- Обновленная база данных
- Пользователь
- VIEW
- XML INDEX
- XML SCHEMA COLLECTION  

## <a name="permissions"></a>Разрешения  
Необходимо членство в предопределенной роли сервера **sysadmin** .

## <a name="error-log-messages"></a>Сообщения в журнале ошибок
Ниже приведены примеры сообщений, которые могут записываться в журнал ошибок во время клонирования.

```
2018-03-26 15:33:56.05 spid53 Database cloning for 'sourcedb' has started with target as 'sourcedb_clone'.

2018-03-26 15:33:56.46 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option TRUSTWORTHY to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option DB_CHAINING to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.88 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.91 spid53 Database 'sourcedb_clone' is a cloned database. A cloned database should be used for diagnostic purposes only and is not supported for use in a production environment.

2018-03-26 15:33:57.92 spid53 Database cloning for 'sourcedb' has finished. Cloned database is 'sourcedb_clone'.
```

## <a name="database-properties"></a>Свойства базы данных
`DATABASEPROPERTYEX('dbname', 'IsClone')` возвращает значение 1, если база данных была создана с помощью инструкции DBCC CLONEDATABASE.

`DATABASEPROPERTYEX('dbname', 'IsVerifiedClone')` возвращает значение 1, если база данных была успешно проверена с помощью WITH VERIFY_CLONEDB.

## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-clone-of-a-database-that-includes-schema-statistics-and-query-store"></a>A. Создание клона базы данных, включающего схему, статистику и хранилище запросов 
В приведенном ниже примере создается клон базы данных AdventureWorks, включающий схему, статистику и данные хранилища запросов (в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1) и более поздних версиях).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone);    
GO 
```  
  
### <a name="b-creating-a-schema-only-clone-of-a-database-without-statistics"></a>Б. Создание клона базы данных, включающего только схему, но не статистику 
В приведенном ниже примере создается клон базы данных AdventureWorks, не включающий статистику (в [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и накопительным пакетом обновления 3 и более поздних версиях).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS;    
GO 
```  

### <a name="c-creating-a-schema-only-clone-of-a-database-without-statistics-and-query-store"></a>В. Создание клона базы данных, включающего только схему без статистики и хранилища запросов 
В приведенном ниже примере создается клон базы данных AdventureWorks, не включающий статистику и данные хранилища запросов (в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1) и более поздних версиях).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS, NO_QUERYSTORE;    
GO 
```  

### <a name="d-creating-a-clone-of-a-database-that-is-verified-for-production-use"></a>Г. Создание клона базы данных с проверкой готовности к использованию в рабочей среде
В приведенном ниже примере создается клон базы данных AdventureWorks, включающий только схему без статистики и данных хранилища запросов и проверенный на готовность к использованию в рабочей среде (в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2) и более поздних версиях).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB;    
GO 
```  
  
### <a name="e-creating-a-clone-of-a-database-that-is-verified-for-production-use-that-includes-a-backup-of-the-cloned-database"></a>Д. Создание клона базы данных, включающего резервную копию клонируемой базы данных, с проверкой готовности к использованию в рабочей среде
В приведенном ниже примере создается клон базы данных AdventureWorks, включающий только схему без статистики и данных хранилища запросов и проверенный на готовность к использованию в рабочей среде.  Кроме того, создается проверенная резервная копия клонируемой базы данных (в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2) и более поздних версиях).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB, BACKUP_CLONEDB;    
GO 
```

## <a name="see-also"></a>См. также
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)    
[Формирование скрипта из необходимых метаданных с целью создания базы данных, включающей только статистику, в SQL Server](https://support.microsoft.com/help/914288)   

