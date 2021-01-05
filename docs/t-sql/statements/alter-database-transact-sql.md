---
description: ALTER DATABASE (Transact-SQL)
title: ALTER DATABASE (Transact-SQL) | Документация Майкрософт
ms.custom: references_regions
ms.date: 10/30/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_DATABASE_TSQL
- ALTER DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- databases [SQL Server], modifying
- ALTER DATABASE statement
- databases [SQL Server], renaming
- renaming databases
- database modifications [SQL Server]
- ALTER DATABASE statement, syntax described
- database names [SQL Server], ALTER DATABASE
- modifying databases
- collations [SQL Server], modifying
- database mirroring [SQL Server], Transact-SQL
ms.assetid: 15f8affd-8f39-4021-b092-0379fc6983da
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: ab21611ee584aa4fda619e37269423eb40909d76
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642203"
---
# <a name="alter-database-transact-sql"></a>ALTER DATABASE (Transact-SQL)

Изменяет определенные параметры конфигурации базы данных.

Эта статья приводит синтаксис, аргументы, комментарии, разрешения и примеры для любых выбранных продуктов SQL.

Дополнительные сведения о соглашениях о синтаксисе см. в статье [Соглашения о синтаксисе в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [База данных SQL](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Общие сведения. SQL Server

В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] эта инструкция изменяет базу данных или файлы и файловые группы, связанные с базой данных. Добавляет или удаляет файлы и файловые группы из базы данных, изменяет атрибуты базы данных или ее файлов и файловых групп, изменяет параметры сортировки базы данных и устанавливает параметры базы данных. Моментальные снимки базы данных изменить нельзя. Чтобы изменить параметры базы данных, связанные с репликацией, используйте процедуру [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).

Так как синтаксис `ALTER DATABASE` имеет значительную длину, мы разделили его описание на несколько статей.

ALTER DATABASE   
В этой статье приведен синтаксис и сопутствующие сведения по изменению имени и параметров сортировки базы данных.

[Параметры инструкции ALTER DATABASE для файлов и файловых групп](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)   
Синтаксис для добавления или удаления файлов и файловых групп в базе данных, для изменения атрибутов файлов и файловых групп, а также дополнительная информация об этом.

[Параметры ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
Синтаксис для изменения атрибутов базы данных с помощью параметров SET команды ALTER DATABASE, а также дополнительная информация об этом.

[Зеркальное отображение базы данных ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
Синтаксис параметров SET команды ALTER DATABASE, используемых в процессе зеркального отображения базы данных, а также дополнительная информация об этом.

[ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
Синтаксис для параметров [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] инструкции ALTER DATABASE для настройки базы данных-получателя вторичной реплики группы доступности AlwaysOn, а также дополнительная информация об этом.

[Уровень совместимости ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
Синтаксис параметров SET инструкции ALTER DATABASE, имеющих отношение к уровням совместимости базы данных, и дополнительная информация об этом.

[ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
Обеспечивает синтаксис для конфигурации для с областью базы данных, используемый для параметров уровня отдельной базы данных, таких как выполнение запроса и оптимизация запросов.

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- SQL Server Syntax
ALTER DATABASE { database_name | CURRENT }
{
    MODIFY NAME = new_database_name
  | COLLATE collation_name
  | <file_and_filegroup_options>
  | SET <option_spec> [ ,...n ] [ WITH <termination> ]
}
[;]

<file_and_filegroup_options>::=
  <add_or_modify_files>::=
  <filespec>::=
  <add_or_modify_filegroups>::=
  <filegroup_updatability_option>::=

<option_spec>::=
{
  | <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option><delayed_durability_option>
  | <external_access_option>
  | <FILESTREAM_options>
  | <HADR_options>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
  | <compatibility_level>
      { 150 | 140 | 130 | 120 | 110 | 100 | 90 }
}
```

## <a name="arguments"></a>Аргументы

*database_name* — имя изменяемой базы данных.

> [!NOTE]
> Этот параметр недоступен в автономной базе данных.

CURRENT   
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.

Определяет, что должна быть изменена текущая используемая база данных.

MODIFY NAME **=** _new_database_name_   
Присваивает базе данных имя, указанное в аргументе *новое_имя_базы_данных*.

COLLATE *collation_name*   
Задает параметры сортировки для базы данных. Аргументом *collation_name* может быть либо имя параметров сортировки Windows, либо имя параметров сортировки SQL. Если аргумент не указан, базе данных будут назначены параметры сортировки экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> После создания базы данных в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] параметры сортировки невозможно изменить.

При создании баз данных с параметрами сортировки, отличными от параметров сортировки по умолчанию, данные в базе данных всегда учитывают указанные параметры сортировки. Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при создании автономной базы данных сведения внутреннего каталога поддерживаются с использованием параметров сортировки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию **Latin1_General_100_CI_AS_WS_KS_SC**.

Список имен параметров сортировки Windows и SQL см. в статье [Параметры сортировки](~/t-sql/statements/collations.md).

**\<delayed_durability_option> ::=**    
**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.

Дополнительные сведения см. в статьях [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md) и [Управление устойчивостью транзакций](../../relational-databases/logs/control-transaction-durability.md).

**\<file_and_filegroup_options>::=**    
Дополнительные сведения см. в статье [Параметры инструкции ALTER DATABASE для файлов и файловых групп (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

## <a name="remarks"></a>Remarks

Чтобы удалить базу данных, используйте инструкцию [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).

Чтобы уменьшить размер базы данных, используйте предложение [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Инструкция `ALTER DATABASE` должна выполняться в режиме автоматической фиксации (режим управления транзакциями по умолчанию). Инструкция не разрешена в явной или неявной транзакции.

Состояние файла базы данных (например "в сети" или "вне сети") поддерживается независимо от состояния базы данных. Дополнительные сведения см. в разделе [Состояния файлов](../../relational-databases/databases/file-states.md). Состояние файлов в пределах файловой группы определяет доступность файловой группы в целом. Чтобы файловая группа была доступна, необходимо, чтобы все файлы в файловой группе находились в режиме в сети. Если файловая группа вне сети, то любая попытка обращения к файловой группе с помощью инструкции SQL закончится ошибкой. При создании планов запросов для инструкций SELECT оптимизатор запросов избегает некластеризованных индексов и индексированных представлений, которые находятся в файловых группах вне сети. Это позволяет успешно выполнить эти инструкции. Однако если файловая группа, находящаяся в режиме вне сети, содержит кучу или кластеризованный индекс целевой таблицы, инструкция SELECT не будет выполнена. Кроме того, любая инструкция `INSERT`, `UPDATE` или `DELETE`, изменяющая таблицу с любым индексом в файловой группе, находящихся в режиме вне сети, также не будет выполнена.

Если база данных находится в состоянии RESTORING, выполнение большинства инструкций `ALTER DATABASE` завершится сбоем. Исключением является настройка параметров зеркального отображения базы данных. База данных может находиться в состоянии RESTORING во время выполнения операции восстановления или тогда, когда при операции восстановления базы данных или файла журнала происходит сбой из-за поврежденного файла резервной копии.

Кэш планов для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] очищается при установке одного из следующих параметров.

- COLLATE
- MODIFY FILEGROUP DEFAULT
- MODIFY FILEGROUP READ_ONLY
- MODIFY FILEGROUP READ_WRITE
- MODIFY_NAME
- OFFLINE
- ONLINE
- PAGE_VERIFY
- READ_ONLY
- READ_WRITE

Очистка кэша планов становится причиной перекомпиляции всех последующих планов выполнения и приводит к непредвиденному временному снижению производительности обработки запросов. Для каждого удаленного хранилища кэша в кэше планов журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит следующее информационное сообщение: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Это сообщение добавляется в журнал каждые пять минут при сбросе кэша в течение этого интервала времени.

Кроме того, кэш планов сбрасывается в следующих случаях:

- В базе данных включен параметр базы данных `AUTO_CLOSE`. Если отсутствуют ссылки соединений пользователя или базы данных, фоновая задача предпримет попытку закрыть и отключить базу данных автоматически.
- Выполняется несколько запросов в базе данных с параметрами по умолчанию. Затем база данных уничтожается.
- Моментальный снимок базы данных для базы данных-источника удален.
- Успешное перестроение журнала транзакций базы данных.
- Восстановление резервной копии базы данных.
- Отсоединение базы данных.

## <a name="changing-the-database-collation"></a>Изменение параметров сортировки в базе данных

Прежде чем применить другие параметры сортировки к базе данных, убедитесь, что соблюдены следующие условия.

- Вы являетесь единственным пользователем базы данных в настоящее время.
- Ни один объект, привязанный к схеме, не зависит от параметров сортировки базы данных.

Если следующие объекты, зависящие от параметров сортировки базы данных, существуют в базе данных, выполнение инструкции ALTER DATABASE *database_name* COLLATE завершится ошибкой. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сообщение об ошибке для каждого объекта, блокирующего действие `ALTER`:

- определяемые пользователем функции и представления, созданные с помощью параметра SCHEMABINDING;
- Вычисляемые столбцы
- CHECK, ограничение
- функции, которые возвращают таблицы с символьными столбцами, имеющими параметры сортировки, унаследованные из параметров сортировки базы данных по умолчанию.
  
Информация о зависимостях не привязанных к схеме сущностей обновляется автоматически при изменении параметров сортировки базы данных.

При изменении параметров сортировки базы данных дубликаты любых системных имен для объектов базы данных не создаются. Если новые параметры сортировки вызывают повторяющиеся имена, следующие пространства имен могут вызвать сбой при изменении параметров сортировки базы данных:

- имена объектов, такие как процедуры, таблицы, триггеры или представления;
- имена схем;
- участники, такие как группы, роли или пользователи;
- имена скалярных типов, таких как системные и определяемые пользователем типы;
- имена полнотекстовых каталогов;
- имена столбцов или параметров в пределах объекта;
- имена индексов в пределах таблицы.

Повторяющиеся имена, полученные с помощью новых параметров сортировки, могут быть причиной сбоя операции изменения, а [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвратит сообщение об ошибке, указывающее пространство имен, в котором был найден дубликат.

## <a name="viewing-database-information"></a>Просмотр сведений о базе данных

Для возврата сведений о базах данных, файлах и файловых группах можно использовать представления каталогов, системные функции и системные хранимые процедуры.

## <a name="permissions"></a>Разрешения

Необходимо разрешение `ALTER` на базу данных.

## <a name="examples"></a>Примеры

### <a name="a-changing-the-name-of-a-database"></a>A. Изменение имени базы данных

В следующем примере имя базы данных `AdventureWorks2012` изменяется на `Northwind`.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
Modify Name = Northwind ;
GO
```

### <a name="b-changing-the-collation-of-a-database"></a>Б. Изменение параметров сортировки базы данных

В следующем примере создается база данных `testdb`, параметры сортировки которой имеют значение `SQL_Latin1_General_CP1_CI_A`. Затем имя базы данных `testdb` изменяется на `COLLATE French_CI_AI`.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
USE master;
GO

CREATE DATABASE testdb
COLLATE SQL_Latin1_General_CP1_CI_AS ;
GO

ALTER DATABASE testDB
COLLATE French_CI_AI ;
GO
```

## <a name="see-also"></a>См. также:

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Системные базы данных](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* База данных SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-database"></a>Общие сведения. База данных SQL

В базе данных SQL Azure эта инструкция предназначена для изменения базы данных. Используйте эту инструкцию, чтобы изменить имя базы данных, изменить выпуск и цель обслуживания базы данных, присоединить или удалить базы данных из эластичного пула, настроить параметры базы данных, добавить или удалить дополнительные базы данных для георепликации или настроить уровень совместимости базы данных.

Так как синтаксис `ALTER DATABASE` имеет значительную длину, мы разделили его описание на несколько статей.

ALTER DATABASE   
В этой статье приведен синтаксис и сопутствующие сведения по изменению имени и параметров сортировки базы данных.

[Параметры ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)    
Синтаксис для изменения атрибутов базы данных с помощью параметров SET команды ALTER DATABASE, а также дополнительная информация об этом.

[Уровень совместимости ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md?view=azuresqldb-current&preserve-view=true)   
Синтаксис параметров SET инструкции ALTER DATABASE, имеющих отношение к уровням совместимости базы данных, и дополнительная информация об этом.

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Azure SQL Database Syntax
ALTER DATABASE { database_name | CURRENT }
{
    MODIFY NAME = new_database_name
  | MODIFY ( <edition_options> [, ... n] )
  | MODIFY BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
  | SET { <option_spec> [ ,... n ] WITH <termination>}
  | ADD SECONDARY ON SERVER <partner_server_name>
    [WITH ( <add-secondary-option>::=[, ... n] ) ]
  | REMOVE SECONDARY ON SERVER <partner_server_name>
  | FAILOVER
  | FORCE_FAILOVER_ALLOW_DATA_LOSS
}
[;]

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 250 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale'}
  | SERVICE_OBJECTIVE =
       { <service-objective>
       | { ELASTIC_POOL (name = <elastic_pool_name>) }
       }
}

<add-secondary-option> ::=
   {
      ALLOW_CONNECTIONS = { ALL | NO }
     | SERVICE_OBJECTIVE =
       { <service-objective>
       | { ELASTIC_POOL ( name = <elastic_pool_name>) }
       }
   }

<service-objective> ::={ 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
       | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) }
      }

<option_spec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <compatibility_level>
    { 150 | 140 | 130 | 120 | 110 | 100 | 90 }

}
```

## <a name="arguments"></a>Аргументы

*database_name*   
Имя изменяемой базы данных.

CURRENT   
Определяет, что должна быть изменена текущая используемая база данных.

MODIFY NAME **=** _new_database_name_   
Присваивает базе данных имя, указанное в аргументе *новое_имя_базы_данных*. В следующем примере имя базы данных `db1` изменяется на `db2`.

```sql
ALTER DATABASE db1
    MODIFY Name = db2 ;
```

MODIFY (EDITION **=** ['Basic' \| 'Standard' \| 'Premium' \|'GeneralPurpose' \| 'BusinessCritical' \| 'Hyperscale'])   
Изменяет уровень службы базы данных.

В следующем примере выпуск изменяется на `Premium`.

```sql
ALTER DATABASE current
    MODIFY (EDITION = 'Premium');
```

> [!IMPORTANT]
> Смена значения параметра EDITION завершается неудачей, если для свойства MAXSIZE базы данных задано значение вне допустимого диапазона, поддерживаемого этим выпуском.

MODIFY (BACKUP_STORAGE_REDUNDANCY **=** ['LOCAL' \| 'ZONE' \| 'GEO'])   
Изменяет избыточность хранилища резервных копий восстановления на момент времени и долгосрочного резервного копирования (если настроено) базы данных. Изменения применяются ко всем будущим резервным копиям. Существующие резервные копии продолжают использовать предыдущий параметр. 

> [!IMPORTANT]
> Параметр BACKUP_STORAGE_REDUNDANCY для Базы данных SQL Azure сейчас предоставляется только в регионе Azure Южная Бразилия в режиме общедоступной предварительной версии и полностью в регионе Azure Юго-Восточной Азии.  

MODIFY (MAXSIZE **=** [100 MB \| 500 MB \| 1 \| 1024...4096] GB)   
Указывает максимальный размер базы данных. Максимальный размер должен соответствовать допустимому набору значений для свойства EDITION базы данных. Смена максимального размера базы данных может потребовать также смены значения EDITION базы данных.

> [!NOTE]
> Аргумент **MAXSIZE** не применяется к отдельным базам данных на уровне служб "Гипермасштабирование". Базы данных уровня обслуживания "Гипермасштабирование" увеличиваются по мере необходимости до 100 ТБ. Служба "База данных SQL" автоматически добавляет объем хранилища. Задавать максимальный размер не нужно.

**Модель DTU**

|**MAXSIZE**|**Основной**|**S0–S2**|**S3–S12**|**P1–P6**|**P11–P15**|
|-----------------|---------------|------------------|-----------------|-----------------|-----------------|-----------------|
|100 МБ|√|√|√|√|√|
|250 МБ|√|√|√|√|√|
|500 МБ|√|√|√|√|√|
|1 ГБ|√|√|√|√|√|
|2 ГБ|√ (D)|√|√|√|√|
|5 ГБ|Недоступно|√|√|√|√|
|10 ГБ|Недоступно|√|√|√|√|
|20 ГБ|Недоступно|√|√|√|√|
|30 ГБ|Недоступно|√|√|√|√|
|40 ГБ|Недоступно|√|√|√|√|
|50 ГБ|Недоступно|√|√|√|√|
|100 ГБ|Недоступно|√|√|√|√|
|150 ГБ|Недоступно|√|√|√|√|
|200 ГБ|Недоступно|√|√|√|√|
|250 ГБ|Недоступно|√ (D)|√ (D)|√|√|
|300 ГБ|Недоступно|√|√|√|√|
|400 ГБ|Н/Д|√|√|√|√|
|500 ГБ|Недоступно|√|√|√ (D)|√|
|750 ГБ|Недоступно|√|√|√|√|
|1024 ГБ|Недоступно|√|√|√|√ (D)|
|От 1024 до 4096 ГБ с шагом приращения 256 ГБ *|Недоступно|Недоступно|Недоступно|Недоступно|√|

\* P11 и P15 позволяют задавать параметру MAXSIZE значение до 4 ТБ, при этом размер по умолчанию — 1024 ГБ. P11 и P15 могут использовать до 4 ТБ включенного объема хранилища без дополнительной платы. На уровне Premium использование MAXSIZE со значением более 1 ТБ сейчас доступно в следующих регионах: восточная часть США 2, западная часть США, US Gov (Вирджиния), Западная Европа, Центральная Германия, Юго-Восточная Азия, Восточная Япония, Восточная Австралия, Центральная Канада и Восточная Канада. Дополнительные сведения об ограничениях по ресурсам для модели DTU см. в разделе [Пределы для ресурсов DTU](/azure/sql-database/sql-database-dtu-resource-limits).

Значение MAXSIZE для модели DTU, если оно задано, должно быть одним из допустимых значений, приведенных в таблице выше для указанного уровня служб.

**Модель виртуального ядра**

**Общее назначение — подготовленное вычисление — 4-е поколение (часть 1)**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Максимальный размер данных (ГБ)|1024|1024|1024|1536|1536|1536|

**Общее назначение — подготовленное вычисление — 4-е поколение (часть 2)**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Максимальный размер данных (ГБ)|1536|3072|3072|3072|4096|4096|

**Общее назначение — подготовленное вычисление — 5-е поколение (часть 1)**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Максимальный размер данных (ГБ)|1024|1024|1024|1536|1536|1536|1536|

**Общее назначение — подготовленное вычисление — 5-е поколение (часть 2)**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Максимальный размер данных (ГБ)|3072|3072|3072|4096|4096|4096|4096|

**Общее назначение — подготовленное вычисление — серия Fsv2 (часть 1)**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|Максимальный размер данных (ГБ)|1024|1024|1024|1024|1536|1536|

**Общее назначение — подготовленное вычисление — серия Fsv2 (часть 2)**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|Максимальный размер данных (ГБ)|1536|1536|3072|3072|4096|

**Общее назначение — бессерверное вычисление — 5-е поколение (часть 1)**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Максимальное число виртуальных ядер|1|2|4|6|8|

**Общее назначение — бессерверное вычисление — 5-е поколение (часть 2)**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|Максимальное число виртуальных ядер|10|12|14|16|

**Общее назначение — бессерверное вычисление — 5-е поколение (часть 3)**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Максимальное число виртуальных ядер|18|20|24|32|40|

**Критически важный для бизнеса — подготовленное вычисление — 4-е поколение (часть 1)**

|Объем вычислительных ресурсов (цель обслуживания)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|Максимальный размер данных (ГБ)|1024|1024|1024|1024|1024|1024|

**Критически важный для бизнеса — подготовленное вычисление — 4-е поколение (часть 2)**

|Объем вычислительных ресурсов (цель обслуживания)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|Максимальный размер данных (ГБ)|1024|1024|1024|1024|1024|1024|

**Критически важный для бизнеса — подготовленное вычисление — 5-е поколение (часть 1)**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|Максимальный размер данных (ГБ)|1024|1024|1024|1536|1536|1536|1536|

**Критически важный для бизнеса — подготовленное вычисление — 5-е поколение (часть 2)**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|Максимальный размер данных (ГБ)|3072|3072|3072|4096|4096|4096|4096|

**Критически важный для бизнеса — подготовленное вычисление — серия M (часть 1)**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|Максимальный размер данных (ГБ)|512|640|768|896|1024|1152|

**Критически важный для бизнеса — подготовленное вычисление — серия M (часть 2)**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|Максимальный размер данных (ГБ)|1280|1536|2048|4096|4096|

Если при использовании модели виртуальных ядер значение `MAXSIZE` не задано, используется значение по умолчанию, равное 32 ГБ. Дополнительные сведения об ограничениях ресурсов в модели виртуальных ядер см. в разделе [Ограничения ресурсов в модели виртуальных ядер](/azure/sql-database/sql-database-dtu-resource-limits).

Следующие правила применяются к аргументам MAXSIZE и EDITION:

- Если параметр EDITION указан, а параметр MAXSIZE — нет, то в качестве выпуска будет использоваться значение по умолчанию. Например, если параметру EDITION задано значение Standard, а параметр MAXSIZE не задан, то параметру MAXSIZE будет автоматически присвоено значение 250 МБ.
- Если значения MAXSIZE и EDITION не указаны, для EDITION задается значение General Purpose, а для MAXSIZE — 32 ГБ.

MODIFY (SERVICE_OBJECTIVE = \<service-objective>)   
Определяет объем вычислительных ресурсов (цель обслуживания). В следующем примере изменяется цель службы базы данных уровня Premium на `P6`:

```sql
ALTER DATABASE current
    MODIFY (SERVICE_OBJECTIVE = 'P6');
```

SERVICE_OBJECTIVE   

- **Для отдельных и включенных в пул баз данных**

  - Определяет объем вычислительных ресурсов (цель обслуживания). Доступные значения для цели обслуживания: `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128`.

- **Для отдельных баз данных на уровне бессерверных вычислений**

  - Определяет объем вычислительных ресурсов (цель обслуживания). Доступные значения для цели обслуживания: `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`.

- **Для отдельных баз данных на уровне обслуживания "Гипермасштабирование"**

  - Определяет объем вычислительных ресурсов (цель обслуживания). Доступные значения для цели обслуживания: `HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`, `HS_GEN4_24`, `HS_Gen5_2`, `HS_Gen5_4`, `HS_Gen5_8`, `HS_Gen5_16`, `HS_Gen5_24`, `HS_Gen5_32`, `HS_Gen5_48`, `HS_Gen5_80`.

Описания целей служб и дополнительные сведения о сочетаниях размеров, выпусков и целей обслуживания см. в разделах [Уровни обслуживания и уровни производительности баз данных SQL Azure](/azure/azure-sql/database/purchasing-models), [Ограничения ресурсов DTU](/azure/sql-database/sql-database-dtu-resource-limits) и [Ограничения ресурсов в модели виртуальных ядер](/azure/sql-database/sql-database-dtu-resource-limits). Поддержка целей служб PRS была удалена. При возникновении вопросов пишите на следующий адрес premium-rs@microsoft.com.

MODIFY (SERVICE_OBJECTIVE = ELASTIC\_POOL (name = \<elastic_pool_name>)   
Чтобы добавить существующую базу данных в эластичный пул, задайте параметру SERVICE_OBJECTIVE базы данных значение ELASTIC_POOL и укажите имя эластичного пула. Этот параметр также служит для изменения базы данных в другом эластичном пуле на том же сервере. Дополнительные сведения см. в разделе [Создание эластичного пула баз данных SQL и управление им](/azure/azure-sql/database/elastic-pool-overview). Чтобы удалить базу данных из эластичного пула, используйте инструкцию ALTER DATABASE, чтобы задать в качестве значения SERVICE_OBJECTIVE один объем вычислительных ресурсов (цель обслуживания) базы данных.

> [!NOTE]
> Базы данных уровня служб "Гипермасштабирование" невозможно добавить в эластичный пул.

ADD SECONDARY ON SERVER \<partner_server_name>   
Создает базу данных-получатель с георепликацией с тем же именем на сервере-участнике, преобразуя локальную базу данных в базу данных-источник с георепликацией, и начинает асинхронную репликацию данных из источника в новую базу данных-получатель. Команда завершается ошибкой, если на сервере-получателе уже существует база данных с таким именем. Команда выполняется в базе данных master на сервере с локальной базой данных, которая становится базой данных-источником.

> [!IMPORTANT]
> Сейчас уровень служб "Гипермасштабирование" не поддерживает георепликацию.

> [!IMPORTANT]
> По умолчанию вторичная база данных создается с той же избыточностью хранилища резервных копий, что и у первичной базы данных (источника). Изменение избыточности хранилища резервных копий при создании вторичной базы данных не поддерживается в T-SQL. 

WITH ALLOW_CONNECTIONS { **ALL** | NO }   
Если параметр ALLOW_CONNECTIONS не указан, ему по умолчанию присваивается значение ALL. Если указано значение ALL, это база данных только для чтения, позволяющая подключаться всем именам входа с соответствующими разрешениями.

WITH SERVICE_OBJECTIVE { `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128` }

Если параметр SERVICE_OBJECTIVE не указан, база данных-получатель создается на том же уровне службы, что и база данных-источник. Если параметр SERVICE_OBJECTIVE указан, база данных-получатель создается с указанным уровнем. Этот параметр поддерживает создание геореплицированных объектов-получателей с менее дорогими уровнями обслуживания. Указанный параметр SERVICE_OBJECTIVE должен находиться в том же выпуске, что и источник. Например, если используется выпуск Premium, параметр S0 указать невозможно.

ELASTIC_POOL (name = \<elastic_pool_name>) — если параметр ELASTIC_POOL не указан, база данных-получатель не создается в эластичном пуле. Если параметр ELASTIC_POOL указан, база данных-получатель создается в указанном пуле.

> [!IMPORTANT]
> Пользователь, выполняющий команду ADD SECONDARY, должен иметь права DBManager на сервере-источнике, членство db_owner в локальной базе данных и права DBManager на сервере-получателе. IP-адрес клиента нужно добавить в список разрешенных правилами брандмауэра для сервера-источника и сервера-получателя. Если используются разные IP-адреса клиента, для сервера-получателя нужно добавить клиентский IP-адрес, добавленный для сервера-источника. Это обязательный шаг перед выполнением команды ADD SECONDARY для запуска георепликации.

REMOVE SECONDARY ON SERVER \<partner_server_name> — удаляет указанную геореплицированную базу данных-получатель на указанном сервере. Команда выполняется в базе данных master на сервере с локальной базой данных-источником.

> [!IMPORTANT]
> Пользователь, выполняющий команду REMOVE SECONDARY, должен иметь права DBManager на сервере-источнике.

FAILOVER — повышает уровень базы данных-получателя в отношении с георепликацией, где выполняется команда, до базы данных-источника и снижает уровень текущей базы данных-источника до новой базы данных-получателя. В рамках этого процесса режим георепликации временно переключается с асинхронного на синхронный. Во время процесса отработки отказа выполняются следующий действия.

1. База данных-источник перестает принимать новые транзакции.
2. Все невыполненные транзакции переходят в базу данных-получатель.
3. База данных-получатель становится базой данных-источником и начинает асинхронную георепликацию со старым источником и новым получателем.

Эта последовательность гарантирует отсутствие потери данных. Период, в течение которого обе базы данных недоступны, составляет примерно 0–25 секунд во время переключения ролей. Вся операция не должна занимать более одной минуты. Если во время выполнения этой команды база данных-источник недоступна, команда выводит сообщение об ошибке, информирующее о недоступности базы данных-источника. Если процесс отработки отказа не завершается и зависает, можно использовать команду принудительной отработки отказа и принять потерю данных, а затем, если необходимо восстановить потерянные данные, обратиться в devops (CSS).

> [!IMPORTANT]
> Пользователь, выполняющий команду FAILOVER, должен иметь права DBManager на сервере-источнике и сервере-получателе.

FORCE_FAILOVER_ALLOW_DATA_LOSS — повышает уровень базы данных-получателя в отношении с георепликацией, где выполняется команда, до базы данных-источника и снижает уровень текущей базы данных-источника до новой базы данных-получателя. Эту команду следует использовать только в том случае, если текущая база данных-источник больше не доступна. Она предназначена для аварийного восстановления, только если очень важно восстановить доступность и допускается потеря некоторых данных.

Во время принудительной отработки отказа выполняются следующие действия.

1. Указанная база данных-получателя немедленно становится базой данных-источником и начинает принимать новые транзакции.
2. Когда исходная база данных-источник может повторно установить соединение с новой базой данных-источником, в исходной базе данных-источнике выполняется добавочное резервное копирование и она становится новой базой данных-получателем.
3. Чтобы восстановить данные из этой добавочной резервной копии в старой базе данных-источнике, пользователь обращается в devops/CSS.
4. Если имеются дополнительные базы данных-получатели, они автоматически настраиваются в качестве баз данных-получателей новой базы данных-источника. Этот процесс является асинхронным и до момента его завершения может возникнуть задержка. До завершения повторной настройки базы данных-получатели остаются базами данных-получателями старой базы данных-источника.

> [!IMPORTANT]
> Пользователю, выполняющему команду `FORCE_FAILOVER_ALLOW_DATA_LOSS`, нужно назначить роль `dbmanager` на сервере-источнике и сервере-получателе.

## <a name="remarks"></a>Remarks

Чтобы удалить базу данных, используйте инструкцию [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).
Чтобы уменьшить размер базы данных, используйте предложение [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Инструкция `ALTER DATABASE` должна выполняться в режиме автоматической фиксации (режим управления транзакциями по умолчанию). Инструкция не разрешена в явной или неявной транзакции.

Очистка кэша планов становится причиной перекомпиляции всех последующих планов выполнения и приводит к непредвиденному временному снижению производительности обработки запросов. Для каждого удаленного хранилища кэша в кэше планов журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит следующее информационное сообщение: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Это сообщение добавляется в журнал каждые пять минут при сбросе кэша в течение этого интервала времени.

Кроме того, кэш процедур сбрасывается в указанных ниже случаях. Выполняется несколько запросов в базе данных с параметрами по умолчанию. Затем база данных уничтожается.

## <a name="viewing-database-information"></a>Просмотр сведений о базе данных

Для возврата сведений о базах данных, файлах и файловых группах можно использовать представления каталогов, системные функции и системные хранимые процедуры.

## <a name="permissions"></a>Разрешения

Для изменения базы данных имя для входа должно быть либо именем для входа субъекта уровня сервера (созданного в процессе подготовки), либо членом роли `dbmanager` базы данных в базе данных master, либо членом роли `db_owner` базы данных текущей базе данных, либо `dbo` базы данных.

## <a name="examples"></a>Примеры

### <a name="a-check-the-edition-options-and-change-them"></a>A. Проверка параметров выпуска и их изменение

Позволяет задать выпуск и максимальный размер для базы данных db1:

```sql
SELECT Edition = DATABASEPROPERTYEX('db1', 'EDITION'),
        ServiceObjective = DATABASEPROPERTYEX('db1', 'ServiceObjective'),
        MaxSizeInBytes =  DATABASEPROPERTYEX('db1', 'MaxSizeInBytes');

ALTER DATABASE [db1] MODIFY (EDITION = 'Premium', MAXSIZE = 1024 GB, SERVICE_OBJECTIVE = 'P15');
```

### <a name="b-moving-a-database-to-a-different-elastic-pool"></a>Б. Перемещение базы данных в другой пул эластичных БД

Существующая база данных перемещается в пул с именем pool1:

```sql
ALTER DATABASE db1
MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = pool1 ) ) ;
```

### <a name="c-add-a-geo-replication-secondary"></a>В. Добавление базы данных-получателя с георепликацией

Создает доступную для чтения базу данных-получатель db1 на сервере `secondaryserver` для базы данных db1 на локальном сервере.

```sql
ALTER DATABASE db1
ADD SECONDARY ON SERVER secondaryserver
WITH ( ALLOW_CONNECTIONS = ALL )
```

### <a name="d-remove-a-geo-replication-secondary"></a>Г. Удаление базы данных-получается с георепликацией

Удаляет базу данных-получателя db1 на сервере `secondaryserver`.

```sql
ALTER DATABASE db1
REMOVE SECONDARY ON SERVER testsecondaryserver
```

### <a name="e-failover-to-a-geo-replication-secondary"></a>Д. Переход на базу данных-получатель с георепликацией

Повышает уровень базы данных-получателя db1 на сервере `secondaryserver` до новой базы данных-источника при выполнении на сервере `secondaryserver`.

```sql
ALTER DATABASE db1 FAILOVER
```

### <a name="e-force-failover-to-a-geo-replication-secondary-with-data-loss"></a>Д. Принудительный переход на базу данных-получатель с георепликацией с потерей данных

Принудительно повышает уровень базы данных-получателя db1 на сервере `secondaryserver` до новой базы данных-источника при выполнении на сервере `secondaryserver` в случае, если сервер-источник становится недоступен. Повышение уровня может привести к потере данных.

```sql
ALTER DATABASE db1 FORCE_FAILOVER_ALLOW_DATA_LOSS
```

### <a name="g-update-a-single-database-to-service-tier-s0-standard-edition-performance-level-0"></a>Ж. Обновление отдельной базы данных до уровня службы S0 (выпуск Standard Edition, уровень производительности 0)

Обновляет отдельную базу данных до выпуска Standard Edition (уровня службы "Стандартный") с объемом вычислительных ресурсов (целью обслуживания) S0 и максимальным размером 250 ГБ.

```sql
ALTER DATABASE [db1] MODIFY (EDITION = 'Standard', MAXSIZE = 250 GB, SERVICE_OBJECTIVE = 'S0');
```

### <a name="h-update-the-backup-storage-redundancy-of-a-database"></a>З. Обновление избыточности хранилища резервных копий базы данных

Обновляет избыточность хранилища резервных копий базы данных до избыточной между зонами. Все будущие резервные копии этой базы данных будут использовать новый параметр. Это касается как резервных копий для восстановления на определенный момент времени, так и для долгосрочного хранения (если оно настроено). 

```sql
ALTER DATABASE db1 MODIFY BACKUP_STORAGE_REDUNDANCY = 'ZONE'
```

## <a name="see-also"></a>См. также раздел

- [CREATE DATABASE — База данных SQL Azure](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Системные базы данных](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Управляемый экземпляр SQL \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::



&nbsp;

## <a name="overview-azure-sql-managed-instance"></a>Общие сведения. Управляемый экземпляр SQL Azure

В [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]используйте эту инструкцию для задания параметров базы данных.

Так как синтаксис `ALTER DATABASE` имеет значительную длину, мы разделили его описание на несколько статей.

ALTER DATABASE   
Эта статья приводит синтаксис и сопутствующие сведения по настройке параметров файлов и файловых групп, баз данных и уровня совместимости баз данных.  
  
[Параметры инструкции ALTER DATABASE для файлов и файловых групп](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md?&tabs=sqldbmi)   
Синтаксис для добавления или удаления файлов и файловых групп в базе данных, для изменения атрибутов файлов и файловых групп, а также дополнительная информация об этом.  
  
[Параметры ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md?&tabs=sqldbmi)   
Синтаксис для изменения атрибутов базы данных с помощью параметров SET команды ALTER DATABASE, а также дополнительная информация об этом.  
  
[Уровень совместимости ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md?&tabs=sqldbmi)   
Синтаксис параметров SET инструкции ALTER DATABASE, имеющих отношение к уровням совместимости базы данных, и дополнительная информация об этом.  

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Azure SQL Database Syntax  
ALTER DATABASE { database_name | CURRENT }  
{
    MODIFY NAME = new_database_name
  | COLLATE collation_name
  | <file_and_filegroup_options>  
  | SET <option_spec> [ ,...n ]  
}  
[;]

<file_and_filegroup_options>::=  
  <add_or_modify_files>::=  
  <filespec>::=
  <add_or_modify_filegroups>::=  
  <filegroup_updatability_option>::=  

<option_spec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>  
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <temporal_history_retention>
  | <compatibility_level>
      { 150 | 140 | 130 | 120 | 110 | 100 | 90 }

}  

```

## <a name="arguments"></a>Аргументы

*database_name*   
Имя изменяемой базы данных.

CURRENT   
Определяет, что должна быть изменена текущая используемая база данных.

## <a name="remarks"></a>Remarks

Чтобы удалить базу данных, используйте инструкцию [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).
Чтобы уменьшить размер базы данных, используйте предложение [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Инструкция `ALTER DATABASE` должна выполняться в режиме автоматической фиксации (режим управления транзакциями по умолчанию). Инструкция не разрешена в явной или неявной транзакции.

Очистка кэша планов становится причиной перекомпиляции всех последующих планов выполнения и приводит к непредвиденному временному снижению производительности обработки запросов. Для каждого удаленного хранилища кэша в кэше планов журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит следующее информационное сообщение: "[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обнаружил %d экземпляров, записанных на диск хранилищ кэша для хранилища кэша "%s" (части кэша планов) в результате операций по обслуживанию или изменению конфигурации базы данных". Это сообщение добавляется в журнал каждые пять минут при сбросе кэша в течение этого интервала времени.

Кроме того, кэш планов сбрасывается, когда выполняется несколько запросов к базе данных с параметрами по умолчанию. Затем база данных уничтожается.

## <a name="viewing-database-information"></a>Просмотр сведений о базе данных

Для возврата сведений о базах данных, файлах и файловых группах можно использовать представления каталогов, системные функции и системные хранимые процедуры.

## <a name="permissions"></a>Разрешения

Изменять базу данных могут только имя входа субъект серверного уровня (созданное в процессе провизионирования) или члены роли базы данных `dbcreator`.

> [!IMPORTANT]
> Владелец базы данных не может изменять базу данных, если он не является членом роли `dbcreator`.

## <a name="examples"></a>Примеры

В следующих примерах показано, как задать автоматическую настройку и как добавить файл в управляемый экземпляр.

```sql
ALTER DATABASE WideWorldImporters
  SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON)

ALTER DATABASE WideWorldImporters
  ADD FILE (NAME = 'data_17')
```

## <a name="see-also"></a>См. также раздел

- [CREATE DATABASE — База данных SQL Azure](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)[sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Системные базы данных](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Общие сведения. Azure Synapse Analytics

В Azure Synapse инструкция `ALTER DATABASE` изменяет имя, максимальный размер или цель обслуживания для базы данных.

Так как синтаксис `ALTER DATABASE` имеет значительную длину, мы разделили его описание на несколько статей.

[Параметры ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
Синтаксис для изменения атрибутов базы данных с помощью параметров SET команды ALTER DATABASE, а также дополнительная информация об этом.

## <a name="syntax"></a>Синтаксис

### <a name="dedicated-sql-pool"></a>[Выделенный пул SQL](#tab/sqlpool)
```syntaxsql
ALTER DATABASE { database_name | CURRENT }
{
  MODIFY NAME = new_database_name
| MODIFY ( <edition_option> [, ... n] )
| SET <option_spec> [ ,...n ] [ WITH <termination> ]
}
[;]

<edition_option> ::=
      MAXSIZE = {
            250 | 500 | 750 | 1024 | 5120 | 10240 | 20480
          | 30720 | 40960 | 51200 | 61440 | 71680 | 81920
          | 92160 | 102400 | 153600 | 204800 | 245760
      } GB
      | SERVICE_OBJECTIVE = {
            'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500'
          | 'DW600' | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000'
          | 'DW3000' | 'DW6000' | 'DW500c' | 'DW1000c' | 'DW1500c'
          | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c' | 'DW6000c'
          | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
      }
```
### <a name="serverless-sql-pool"></a>[Бессерверный пул SQL](#tab/sqlod)
```syntaxsql
ALTER DATABASE { database_name | Current } 
{ 
    COLLATE collation_name 
  | SET { <optionspec> [ ,...n ] } 
} 
[;] 

<optionspec> ::= 
{ 
    <auto_option> 
  | <sql_option> 
}  

<auto_option> ::= 
{ 
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] } 
  | AUTO_UPDATE_STATISTICS { ON | OFF } 
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF } 
} 

<sql_option> ::= 
{ 
    ANSI_NULL_DEFAULT { ON | OFF } 
  | ANSI_NULLS { ON | OFF } 
  | ANSI_PADDING { ON | OFF } 
  | ANSI_WARNINGS { ON | OFF } 
  | ARITHABORT { ON | OFF } 
  | COMPATIBILITY_LEVEL = { 140 | 130 | 120 | 110 | 100 } 
  | CONCAT_NULL_YIELDS_NULL { ON | OFF } 
  | NUMERIC_ROUNDABORT { ON | OFF } 
  | QUOTED_IDENTIFIER { ON | OFF } 
} 
```
---

## <a name="arguments"></a>Аргументы

*database_name*   
Задает имя изменяемой базы данных.

MODIFY NAME = *new_database_name*   
Присваивает базе данных имя, указанное в аргументе *новое_имя_базы_данных*.

MAXSIZE   
Значение по умолчанию — 245 760 ГБ (240 ТБ).

**Применимо к:** Оптимизировано для поколения вычислительных ресурсов 1

Максимально допустимый размер базы данных. Размер базы данных не может превышать MAXSIZE.

**Применимо к:** Оптимизировано для поколения вычислительных ресурсов 2

Максимально допустимый размер данных rowstore в базе данных. Размер данных, хранящихся в таблицах rowstore, разностном хранилище индекса columnstore или некластеризованном индексе на базе кластеризованного индекса columnstore, не может превышать MAXSIZE. Данные, сжатые в формат columnstore, не имеют ограничений на размер и не ограничивается значением MAXSIZE.

SERVICE_OBJECTIVE   
Определяет объем вычислительных ресурсов (цель обслуживания). Подробные сведения о целях служб для Azure Synapse см. в статье о [единицах использования хранилища данных (DWU)](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu).

## <a name="permissions"></a>Разрешения

Требуются следующие разрешения:

- имя входа субъекта серверного уровня, созданное процессом подготовки, или
- член роли базы данных `dbmanager`.

Владелец базы данных не может изменять базу данных, если он не является членом роли `dbmanager`.

## <a name="general-remarks"></a>Общие замечания

Текущая база данных должна отличаться от изменяемой, поэтому **инструкцию ALTER следует выполнять при подключении к базе данных master**.

COMPATIBILITY_LEVEL в SQL Analytics имеет значение 130 по умолчанию. Его нельзя изменить. Дополнительные сведения: [Улучшение производительности запросов с уровнем совместимости 130 в базе данных SQL Azure](./alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> COMPATIBILITY_LEVEL применяется только к подготовленным ресурсам (пулам).

## <a name="limitations-and-restrictions"></a>Ограничения

Для выполнения `ALTER DATABASE` база данных должна находиться в сети и не может быть в приостановленном состоянии.

Инструкция `ALTER DATABASE` должна выполняться в режиме автофиксации, который является режимом управления транзакциями по умолчанию. Он задается в параметрах подключения.

Инструкция `ALTER DATABASE` не может входить в определенную пользователем транзакцию.

Изменить параметры сортировки базы данных невозможно.

## <a name="examples"></a>Примеры

Перед выполнением этих примеров убедитесь в том, что изменяемая база данных не является текущей базой данных. Текущая база данных должна отличаться от изменяемой, поэтому **инструкцию ALTER следует выполнять при подключении к базе данных master**.

### <a name="a-change-the-name-of-the-database"></a>A. Изменение имени базы данных

```sql
ALTER DATABASE AdventureWorks2012
MODIFY NAME = Northwind;
```

### <a name="b-change-max-size-for-the-database"></a>Б. Изменение максимального размера базы данных

```sql
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB );
```

### <a name="c-change-the-compute-size-service-objective"></a>В. Изменение объема вычислительных ресурсов (цели обслуживания)

```sql
ALTER DATABASE dw1 MODIFY ( SERVICE_OBJECTIVE= 'DW1200' );
```

### <a name="d-change-the-max-size-and-the-compute-size-service-objective"></a>Г. Изменение максимального объема и объема вычислительных ресурсов (цели обслуживания)

```sql
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB, SERVICE_OBJECTIVE= 'DW1200' );
```

## <a name="see-also"></a>См. также:

- [CREATE DATABASE (Azure Synapse Analytics)](../../t-sql/statements/create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
- [Список справочных статей по Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [База данных SQL](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Управляемый экземпляр SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Общие сведения. Система платформы аналитики

Изменяет параметры максимального размера базы данных для реплицированных таблиц, распределенных таблиц и журнала транзакций в PDW. С помощью этой инструкции можно управлять выделением дискового пространства для базы данных по мере изменения ее размера. В статье также описан синтаксис, связанный с настройкой параметров баз данных в PDW.

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Analytics Platform System
ALTER DATABASE database_name
    SET ( <set_database_options> | <db_encryption_option> )
[;]

<set_database_options> ::=
{
    AUTOGROW = { ON | OFF }
    | REPLICATED_SIZE = size [GB]
    | DISTRIBUTED_SIZE = size [GB]
    | LOG_SIZE = size [GB]
    | SET AUTO_CREATE_STATISTICS { ON | OFF }
    | SET AUTO_UPDATE_STATISTICS { ON | OFF }
    | SET AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<db_encryption_option> ::=
    ENCRYPTION { ON | OFF }
```

## <a name="arguments"></a>Аргументы

*database_name*   
Имя изменяемой базы данных. Чтобы получить список баз данных на устройстве, используйте представление [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

AUTOGROW = { ON | OFF }   
Изменяет значение параметра AUTOGROW. Если параметр AUTOGROW имеет значение ON, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] автоматически увеличивает пространство, выделенное для реплицированных таблиц, распределенных таблиц и журнала транзакций, в соответствии с растущими требованиями к хранилищу. Если параметр AUTOGROW имеет значение OFF, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] возвращает ошибку в случае, если размер пространства, занимаемого реплицированными таблицами, распределенными таблицами или журналом транзакций, превышает заданное максимальное значение.

REPLICATED_SIZE = *размер* [ГБ]   
Задает для вычислительного узла новый максимальный размер для хранения всех реплицированных таблиц изменяемой базы данных в гигабайтах. Если вы планируете пространство для хранения данных на устройстве, значение REPLICATED_SIZE необходимо умножить на количество вычислительных узлов на устройстве.

DISTRIBUTED_SIZE = *размер* [ГБ]   
Задает для базы данных новый максимальный размер для хранения всех распределенных таблиц изменяемой базы данных в гигабайтах. Этот размер распределяется по всем вычислительным узлам на устройстве.

LOG_SIZE = *размер* [ГБ]   
Задает для базы данных новый максимальный размер для хранения всех журналов транзакций изменяемой базы данных в гигабайтах. Этот размер распределяется по всем вычислительным узлам на устройстве.

ENCRYPTION { ON | OFF }   
Включает шифрование базы данных (ON) или отключает его (OFF). Шифрование можно настроить для [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] только в том случае, если [sp_pdw_database_encryption](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md) имеет значение **1**. Перед настройкой прозрачного шифрования данных необходимо создать ключ шифрования базы данных. Дополнительные сведения о шифровании баз данных см. в статье [Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).

SET AUTO_CREATE_STATISTICS { ON | OFF }   
Если включен параметр AUTO_CREATE_STATISTICS, оптимизатор запросов при необходимости создает статистику по отдельным столбцам в предикате запроса, чтобы улучшить оценку кратности для плана запроса. Такая статистика по отдельным столбцам создается для столбцов, у которых отсутствует гистограмма в существующем объекте статистики.

Параметр включен по умолчанию для новых баз данных, созданных после обновления до AU7. Параметр отключен по умолчанию для баз данных, созданных до обновления.

Дополнительные сведения о статистике см. в статье [Статистика](../../relational-databases/statistics/statistics.md).

SET AUTO_UPDATE_STATISTICS { ON | OFF }   
Если включен параметр AUTO_UPDATE_STATISTICS, оптимизатор запросов определяет, устарела ли статистика, и при необходимости обновляет ее, если она используется в запросе. Статистика становится устаревшей, если операции вставки, обновления, удаления или слияния изменяют распределение данных в таблице или индексированном представлении. Оптимизатор запросов определяет, когда статистика может оказаться устаревшей, подсчитывая операции изменения данных с момента последнего обновления статистики и сравнивая количество изменений с пороговым значением. Пороговое значение основано на количестве строк в таблице или индексированном представлении.

Параметр включен по умолчанию для новых баз данных, созданных после обновления до AU7. Параметр отключен по умолчанию для баз данных, созданных до обновления.

Дополнительные сведения о статистике см. в статье [Статистика](../../relational-databases/statistics/statistics.md).

SET AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }   
Параметр AUTO_UPDATE_STATISTICS_ASYNC, который управляет асинхронным обновлением статистики, определяет, какой режим обновления статистики (синхронный или асинхронный) использует оптимизатор запросов. Параметр AUTO_UPDATE_STATISTICS_ASYNC применяется к объектам статистики, создаваемым для индексов, отдельных столбцов в предикатах запросов, и к статистике, создаваемой инструкцией CREATE STATISTICS.

Параметр включен по умолчанию для новых баз данных, созданных после обновления до AU7. Параметр отключен по умолчанию для баз данных, созданных до обновления.

Дополнительные сведения о статистике см. в статье [Статистика](../../relational-databases/statistics/statistics.md).

## <a name="permissions"></a>Разрешения

Необходимо разрешение `ALTER` для базы данных.

## <a name="error-messages"></a>сообщения об ошибках

Если автоматическая статистика отключена, при попытке изменить параметры статистики в PDW выводится сообщение об ошибке `This option is not supported in PDW`. Системный администратор может включить автоматическую статистику, включив переключатель [AutoStatsEnabled](../../analytics-platform-system/appliance-feature-switch.md).

## <a name="general-remarks"></a>Общие замечания

Значения параметров `REPLICATED_SIZE`, `DISTRIBUTED_SIZE` и `LOG_SIZE` могут быть больше или меньше текущих значений для базы данных либо равны им.

## <a name="limitations-and-restrictions"></a>Ограничения

Операции увеличения и уменьшения размера являются приблизительными. Полученные фактические размеры могут отличаться от значений параметров.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] выполняет инструкцию ALTER DATABASE не как атомарную операцию. Если выполнение инструкции прерывается, уже внесенные изменения сохраняются.

Параметры статистики работают, только если администратор включил автоматическую статистику. Для включения или отключения автоматической статистики служит переключатель [AutoStatsEnabled](../../analytics-platform-system/appliance-feature-switch.md).

## <a name="locking-behavior"></a>Режим блокировки

Принимает совмещаемую блокировку для объекта DATABASE. Изменить базу данных, в которой другой пользователь в настоящее время производит операции чтения или записи, невозможно. Это относится и к сеансам, в рамках которых была выполнена инструкция [USE](../language-elements/use-transact-sql.md) для базы данных.

## <a name="performance"></a>Производительность

Для сжатия базы данных может требоваться много времени и системных ресурсов в зависимости от фактического размера данных в ней и степени фрагментации на диске. Например, сжатие базы данных может длиться несколько часов.

## <a name="determining-encryption-progress"></a>Определение хода выполнения шифрования

Чтобы определить ход выполнения прозрачного шифрования данных в базе данных в процентах, используйте следующий запрос:

```sql
WITH
database_dek AS (
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id,
        dek.encryption_state, dek.percent_complete,
        dek.key_algorithm, dek.key_length, dek.encryptor_thumbprint,
        type
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek
    INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map
        ON dek.database_id = node_db_map.database_id
        AND dek.pdw_node_id = node_db_map.pdw_node_id
    LEFT JOIN sys.pdw_database_mappings AS db_map
        ON node_db_map .physical_name = db_map.physical_name
    INNER JOIN sys.dm_pdw_nodes nodes
        ON nodes.pdw_node_id = dek.pdw_node_id
    WHERE dek.encryptor_thumbprint <> 0x
),
dek_percent_complete AS (
    SELECT database_dek.database_id, AVG(database_dek.percent_complete) AS percent_complete
    FROM database_dek
    WHERE type = 'COMPUTE'
    GROUP BY database_dek.database_id
)
SELECT DB_NAME( database_dek.database_id ) AS name,
    database_dek.database_id,
    ISNULL(
       (SELECT TOP 1 dek_encryption_state.encryption_state
        FROM database_dek AS dek_encryption_state
        WHERE dek_encryption_state.database_id = database_dek.database_id
        ORDER BY (CASE encryption_state
            WHEN 3 THEN -1
            ELSE encryption_state
            END) DESC), 0)
        AS encryption_state,
dek_percent_complete.percent_complete,
database_dek.key_algorithm, database_dek.key_length, database_dek.encryptor_thumbprint
FROM database_dek
INNER JOIN dek_percent_complete
    ON dek_percent_complete.database_id = database_dek.database_id
WHERE type = 'CONTROL';
```

Подробный пример, демонстрирующий все этапы реализации прозрачного шифрования данных, см. в статье [Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).

## <a name="examples-sspdw"></a>Примеры: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-altering-the-autogrow-setting"></a>A. Изменение параметра AUTOGROW

Присвойте параметру AUTOGROW значение ON для базы данных `CustomerSales`.

```sql
ALTER DATABASE CustomerSales
    SET ( AUTOGROW = ON );
```

### <a name="b-altering-the-maximum-storage-for-replicated-tables"></a>Б. Изменение максимального пространства для хранения реплицированных таблиц

В приведенном ниже примере для базы данных `CustomerSales` задается максимальный размер хранилища для реплицированных таблиц, равный 1 ГБ. Это размер хранилища для вычислительного узла.

```sql
ALTER DATABASE CustomerSales
    SET ( REPLICATED_SIZE = 1 GB );
```

### <a name="c-altering-the-maximum-storage-for-distributed-tables"></a>В. Изменение максимального пространства для хранения распределенных таблиц

 В приведенном ниже примере для базы данных `CustomerSales` задается максимальный размер хранилища для распределенных таблиц, равный 1000 ГБ (один терабайт). Это совокупный размер хранилища для всех вычислительных узлов на устройстве, а не для отдельных вычислительных узлов.

```sql
ALTER DATABASE CustomerSales
    SET ( DISTRIBUTED_SIZE = 1000 GB );
```

### <a name="d-altering-the-maximum-storage-for-the-transaction-log"></a>Г. Изменение максимального пространства для хранения журнала транзакций

 В приведенном ниже примере для базы данных `CustomerSales` на устройстве задается максимальный размер журнала транзакций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], равный 10 ГБ.

```sql
ALTER DATABASE CustomerSales
    SET ( LOG_SIZE = 10 GB );
```

### <a name="e-check-for-current-statistics-values"></a>Д. Проверка текущих значений статистики

Следующий запрос возвращает текущие значения статистики для всех баз данных. Значение 1 означает, что функция включена, а 0 — что функция отключена.

```sql
SELECT NAME,
    is_auto_create_stats_on,
    is_auto_update_stats_on,
    is_auto_update_stats_async_on
FROM sys.databases;
```

### <a name="f-enable-auto-create-and-auto-update-stats-for-a-database"></a>Е. Включение автоматического создания и автоматического обновление статистики для базы данных

Используйте следующую инструкцию, чтобы включить автоматическое и асинхронное создание и обновление статистики для базы данных CustomerSales. В результате по мере необходимости создается и обновляется статистика по отдельным столбцам для формирования высококачественных планов запросов.

```sql
ALTER DATABASE CustomerSales
    SET AUTO_CREATE_STATISTICS ON;
ALTER DATABASE CustomerSales
    SET AUTO_UPDATE_STATISTICS ON;
ALTER DATABASE
    SET AUTO_UPDATE_STATISTICS_ASYNC ON;
```

## <a name="see-also"></a>См. также:

- [CREATE DATABASE — Analytics Platform System](../../t-sql/statements/create-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
