---
description: DROP DATABASE (Transact-SQL)
title: DROP DATABASE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP DATABASE
- DROP_DATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], deleting
- removing databases
- database snapshots [SQL Server], removing
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- DROP DATABASE statement
- database removal [SQL Server], DROP DATABASE statement
ms.assetid: 477396a9-92dc-43c9-9b97-42c8728ede8e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cef57db413b9035926e8c05446cd761cb937d538
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172116"
---
# <a name="drop-database-transact-sql"></a>DROP DATABASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Удаляет одну или несколько пользовательских баз данных или моментальных снимков базы данных из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- SQL Server Syntax
DROP DATABASE [ IF EXISTS ] { database_name | database_snapshot_name } [ ,...n ] [;]
```

```syntaxsql
-- Azure SQL Database, Azure Synapse Analytics and Analytics Platform System Syntax
DROP DATABASE database_name [;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*IF EXISTS* —
**применимо к**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]–[текущая версия](https://go.microsoft.com/fwlink/p/?LinkId=299658)).

Условное удаление базы данных только в том случае, если она уже существует.

*database_name* — задает имя удаляемой базы данных. Для просмотра списка баз данных используйте представление каталога [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

*database_snapshot_name* — 
**применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.

Задает имя удаляемого моментального снимка базы данных.

## <a name="general-remarks"></a>Общие замечания

База данных может быть удалена независимо от ее состояния: вне сети, только для чтения, подозрительная и так далее. Для просмотра текущего состояния базы данных используйте представление каталога **sys.databases**.

Удаленная база данных может быть повторно создана только с помощью восстановления из резервной копии. Резервное копирование моментальных снимков базы данных произвести невозможно, поэтому они не могут быть восстановлены.

При удалении базы данных необходимо выполнить резервное копирование базы данных [master](../../relational-databases/databases/master-database.md).

При удалении база данных удаляется из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Также с физического диска удаляются файлы, используемые базой данных. Если база данных или один из ее файлов во время удаления находится в режиме вне сети, файлы с диска не удаляются. Эти файлы можно удалить вручную при помощи обозревателя Windows. Для удаления базы данных с текущего сервера без удаления файлов из файловой системы используйте процедуру [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md).

> [!WARNING]
> Удаление файла базы данных, имеющего связанные с ним резервные копии FILE_SNAPSHOT, выполнится успешно, однако файлы базы данных, с которыми связаны моментальные снимки, не будут удалены во избежание объявления недействительными резервных копий, ссылающихся на файл базы данных. Файл усекается, но физически не удаляется, чтобы сохранить резервные копии FILE_SNAPSHOT без изменений. Дополнительные сведения см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). **Применимо к**: с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658).

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

При удалении моментального снимка базы данных он удаляется из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а его физические разреженные файлы удаляются из файловой системы NTFS. Сведения об использовании разреженных файлов для моментальных снимков баз данных см. в статье [Моментальные снимки базы данных](../../relational-databases/databases/database-snapshots-sql-server.md). Удаление моментального снимка базы данных очищает кэш планов для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Очистка кэша планов становится причиной перекомпиляции всех последующих планов выполнения и приводит к непредвиденному временному снижению производительности обработки запросов. Для каждого удаленного хранилища кэша в кэше планов журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит следующее информационное сообщение: "[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обнаружил %d экземпляров, записанных на диск хранилищ кэша для хранилища кэша "%s" (части кэша планов) в результате операций по обслуживанию или изменению конфигурации базы данных". Это сообщение добавляется в журнал каждые пять минут при сбросе кэша в течение этого интервала времени.

## <a name="interoperability"></a>Совместимость

### <a name="sql-server"></a>SQL Server

Чтобы удалить базу данных, опубликованную для репликации транзакций либо опубликованную или подписанную на репликацию слиянием, вначале необходимо удалить репликацию из базы данных. Если база данных повреждена или репликация не может быть удалена, скорее всего, базу данных все равно можно будет удалить, использовав инструкцию ALTER DATABASE для перевода базы данных в режим вне сети, после чего удалить ее.

Если база данных участвует в доставке журнала, отмените доставку журнала перед удалением базы данных. Дополнительные сведения см. в разделе [Сведения о доставке журналов](../../database-engine/log-shipping/about-log-shipping-sql-server.md).

## <a name="limitations-and-restrictions"></a>Ограничения

[Системные базы данных](../../relational-databases/databases/system-databases.md) удалить невозможно.

Инструкция DROP DATABASE должна выполняться в режиме автоматической фиксации и не разрешена в явной или неявной транзакции. Режим автоматической фиксации — это режим управления транзакцией по умолчанию.

Удалить базу данных, которая используется в текущий момент времени, невозможно. Такая база данных может использоваться каким-либо пользователем для чтения или записи данных. Одним из способов отключить пользователей от базы данных является использование инструкции ALTER DATABASE для перевода базы данных в режим SINGLE_USER.

> [!WARNING]
> Такой подход не гарантирует отсутствие сбоев, поскольку первое последовательное подключение, устанавливаемое любым потоком, получит поток SINGLE_USER, в результате чего подключение завершится сбоем. SQL Server не реализует встроенный механизм удаления баз данных под нагрузкой.

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Любые моментальные снимки базы данных должны быть удалены перед удалением базы данных.

При удалении базы данных, настроенной в качестве базы Stretch Database, не удаляются удаленные данные. В таком случае удаленные данные следует удалять вручную.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Для удаления базы данных необходимо соединение с базой данных master.

 Инструкция DROP DATABASE должна быть единственной инструкцией в пакете SQL, и ее можно удалить только одновременно с базой данных.

### [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

Для удаления базы данных необходимо соединение с базой данных master.

Инструкция DROP DATABASE должна быть единственной инструкцией в пакете SQL, и ее можно удалить только одновременно с базой данных.

## <a name="permissions"></a>Разрешения

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Требуется разрешение **CONTROL** в базе данных, разрешение **ALTER ANY DATABASE** или членство в предопределенной роли базы данных **db_owner**.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Удалить базу данных могут только пользователи с именем входа субъекта серверного уровня (созданного процессом подготовки) или члены роли **dbmanager** базы данных.

### [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Требуется разрешение **CONTROL** в базе данных, разрешение **ALTER ANY DATABASE** или членство в предопределенной роли базы данных **db_owner**.

## <a name="examples"></a>Примеры

### <a name="a-dropping-a-single-database"></a>A. Удаление одиночной базы данных

В следующем примере удаляется база данных `Sales`.

```sql
DROP DATABASE Sales;
```

### <a name="b-dropping-multiple-databases"></a>Б. Удаление нескольких баз данных

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

В следующем примере удаляется каждая из перечисленных баз данных.

```sql
DROP DATABASE Sales, NewSales;
```

### <a name="c-dropping-a-database-snapshot"></a>В. Удаление моментального снимка базы данных

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

В следующем примере из базы данных удаляется моментальный снимок с именем `sales_snapshot0600` без влияния на базу данных-источник.

```sql
DROP DATABASE sales_snapshot0600;
```

## <a name="see-also"></a>См. также:

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
