---
title: База данных tempdb | Документация Майкрософт
description: В этой статье показано, как настроить и использовать базу данных tempdb в SQL Server и Базе данных SQL Azure.
ms.custom: P360
ms.date: 09/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- temporary tables [SQL Server], tempdb database
- tempdb database [SQL Server], about tempdb
- temporary stored procedures [SQL Server]
- tempdb database [SQL Server]
ms.assetid: ce4053fb-e37a-4851-b711-8e504059a780
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 910db0887a50e12163ddeda927159f5e6c97b74b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169282"
---
# <a name="tempdb-database"></a>tempdb, база данных

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Системная база данных `tempdb` — это глобальный ресурс, доступный всем пользователям, подключенным к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или Базе данных SQL Azure. `tempdb` содержит:  
  
- Временные *пользовательские объекты*, созданные явно. К ним относятся глобальные или локальные временные таблицы и индексы, временные хранимые процедуры, табличные переменные, возвращаемые функциями с табличными значениями таблицы и курсоры.  
- *Внутренние объекты*, создаваемые ядром СУБД. К ним относятся следующие:
  - Рабочие таблицы, хранящие промежуточные результаты буферов, курсоры, сортировки и временное хранилище больших объектов (LOB).
  - рабочие файлы для операций хэш-соединения или статистических хэш-выражений;
  - промежуточные результаты сортировки для таких операций, как создание или перестроение индексов (если указать `SORT_IN_TEMPDB`), либо определенных запросов `GROUP BY`, `ORDER BY` или `UNION`.

  Каждый внутренний объект использует минимум девять страниц: страницу IAM и восьмистраничный экстент. Дополнительные сведения см. в разделе [Страницы и экстенты](../../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

  > [!IMPORTANT]
  > Отдельные базы данных и эластичные пулы Базы данных SQL Azure поддерживают глобальные временные таблицы и глобальные временные хранимые процедуры, которые хранятся в `tempdb` и имеют область действия на уровне базы данных. 
  >
  > Глобальные временные таблицы и глобальные временные хранимые процедуры являются общими для всех сеансов пользователей в рамках одной базы данных SQL. Сеансы пользователей, связанные с другими базами данных SQL, не имеют доступа к глобальным временным таблицам. Дополнительные сведения см. в разделе [Глобальные временные таблицы (база данных SQL Azure) в области базы данных](../../t-sql/statements/create-table-transact-sql.md#database-scoped-global-temporary-tables-azure-sql-database). [Управляемый экземпляр SQL Azure](/azure/sql-database/sql-database-managed-instance) поддерживает те же временные объекты, что и SQL Server.
  >
  > Для эластичных пулов и отдельных баз данных Базы данных SQL Azure применяются только базы master и `tempdb`. Дополнительные сведения см. в статье [Что являет собою сервер Базы данных SQL Azure?](/azure/sql-database/sql-database-servers-databases#what-is-an-azure-sql-database-server). Описание `tempdb` в контексте эластичных пулов и отдельных баз данных Базы данных SQL Azure см. в разделе [База данных tempdb в Базе данных SQL Azure](#tempdb-database-in-sql-database). 
  >
  > Для Управляемого экземпляра SQL Azure применяются все системные базы данных.

- *Хранилища версий*. Это коллекции страниц данных со строками данных, которые поддерживают функции управления версиями строк. Существует два типа хранилищ: общее хранилище версий и хранилище версий для построения индекса в подключенном режиме. Хранилища версий содержат следующее:
  - версии строк, создаваемые транзакциями изменения данных в базе данных, которая использует `READ COMMITTED` через транзакции изоляции моментальных снимков и транзакции изоляции управления версиями строк;  
  - версии строк, создаваемые транзакциями изменения данных для таких функций, как операции с индексами в подключенном режиме, функции MARS (множественный активный результирующий набор) и триггеры `AFTER`.  
  
Операции в `tempdb` в минимальном объеме записываются в журнал, что позволяет откатывать транзакции. `tempdb` создается заново при каждом запуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы система всегда запускалась с чистой копией базы данных. Временные таблицы и хранимые процедуры удаляются автоматически при отключении, и при выключении системы нет активных соединений. 

`tempdb` не требует сохранения каких-либо данных между сеансами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Операции резервного копирования и восстановления для `tempdb` недопустимы.  

## <a name="physical-properties-of-tempdb-in-sql-server"></a>Физические свойства tempdb в SQL Server

В следующей таблице приводятся исходные значения конфигурации для файлов данных и журналов `tempdb` в SQL Server. Значения основаны на значениях по умолчанию для базы данных `model`. Размеры этих файлов могут немного отличаться в зависимости от выпуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Файл|Логическое имя|Физическое имя|Начальный размер|Увеличение размера файлов|  
|----------|------------------|-------------------|------------------|-----------------|  
|Первичные данные|tempdev|tempdb.mdf|8 МБ|Автоматическое увеличение на 64 МБ до заполнения диска.|  
|Вторичные файлы данных.|temp#|tempdb_mssql_ # .ndf|8 МБ|Автоматическое увеличение на 64 МБ до заполнения диска.|  
|Журнал|templog|templog.ldf|8 МБ|Автоматическое увеличение на 64 МБ до максимального размера в 2 ТБ.|  
  
Количество вторичных файлов данных зависит от числа логических процессоров на компьютере. Как правило, если число логических процессоров меньше или равно восьми, используйте равное ему число файлов данных. Если число логических процессоров больше восьми, используйте восемь файлов данных. Если состязание сохраняется, увеличьте число файлов данных на значение, кратное четырем, пока состязание не снизится до приемлемого уровня, или внесите изменения в рабочую нагрузку или код.

> [!NOTE]
> Количество файлов данных по умолчанию основано на общих рекомендациях, приведенных в статье [KB 2154845](https://support.microsoft.com/kb/2154845/).  
  
### <a name="moving-the-tempdb-data-and-log-files-in-sql-server"></a>Перемещение данных и файлов журналов базы данных tempdb в SQL Server

Сведения о перемещении файлов журналов и данных `tempdb` см. в статье [Перемещение системных баз данных](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options-for-tempdb-in-sql-server"></a>Параметры базы данных для tempdb в SQL Server

В следующей таблице приводится список значений по умолчанию для каждого параметра базы данных `tempdb`, а также возможность его изменения. Чтобы просмотреть текущие настройки этих параметров, используйте представление каталога [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Параметр базы данных|Значение по умолчанию|Можно ли изменить|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|Да|  
|ANSI_NULL_DEFAULT|OFF|Да|  
|ANSI_NULLS|OFF|Да|  
|ANSI_PADDING|OFF|Да|  
|ANSI_WARNINGS|OFF|Да|  
|ARITHABORT|OFF|Да|  
|AUTO_CLOSE|OFF|нет|  
|AUTO_CREATE_STATISTICS|ON|Да|  
|AUTO_SHRINK|OFF|нет|  
|AUTO_UPDATE_STATISTICS|ON|Да|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|Да|  
|CHANGE_TRACKING|OFF|нет|  
|CONCAT_NULL_YIELDS_NULL|OFF|Да|  
|CURSOR_CLOSE_ON_COMMIT|OFF|Да|  
|CURSOR_DEFAULT|GLOBAL|Да|  
|Параметры доступности базы данных|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|нет<br /><br /> Нет<br /><br /> Нет|  
|DATE_CORRELATION_OPTIMIZATION|OFF|Да|  
|DB_CHAINING|ON|нет|  
|ENCRYPTION|OFF|нет|  
|MIXED_PAGE_ALLOCATION|OFF|нет|  
|NUMERIC_ROUNDABORT|OFF|Да|  
|PAGE_VERIFY|Значение CHECKSUM для новых установок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Значение NONE для обновлений [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Да|  
|PARAMETERIZATION|ПРОСТОЙ|Да|  
|QUOTED_IDENTIFIER|OFF|Да|  
|READ_COMMITTED_SNAPSHOT|OFF|нет|  
|RECOVERY|ПРОСТОЙ|нет|  
|RECURSIVE_TRIGGERS|OFF|Да|  
|Параметры компонента Service Broker|ENABLE_BROKER|Да|  
|TRUSTWORTHY|OFF|нет|  
  
Описание этих баз данных см. в статье [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
## <a name="tempdb-database-in-sql-database"></a>База данных tempdb в Базе данных SQL

### <a name="tempdb-sizes-for-dtu-based-service-tiers"></a>Размеры базы данных tempdb для уровней служб на основе DTU

<!-- tempdb being larger for Basic and 50 eDTU pools than for 100-400 eDTU pools reflects actual config (historical reasons) --> 

|Целевой уровень обслуживания|Максимальный размер файла данных `tempdb` (ГБ)|Число файлов данных `tempdb`|Максимальный размер данных `tempdb` (ГБ)|
|---|---:|---:|---:|
|Basic|13.9|1|13.9|
|S0|13.9|1|13.9|
|S1|13.9|1|13.9|
|S2|13.9|1|13.9|
|S3|32|1|32
|S4|32|2|64|
|S6|32|3|96|
|S7|32|6|192|
|S9|32|12|384|
|S12|32|12|384|
|P1|13.9|12|166.7|
|P2|13.9|12|166.7|
|P4|13.9|12|166.7|
|P6|13.9|12|166.7|
|P11|13.9|12|166.7|
|P15|13.9|12|166.7|
|Эластичные пулы уровня "Базовый" (все конфигурации DTU)|13.9|12|166.7|
|Эластичные пулы ценовой категории "Стандартный" (50 eDTU)|13.9|12|166.7|
|Эластичные пулы ценовой категории "Стандартный" (100 eDTU)|32|1|32|
|Эластичные пулы ценовой категории "Стандартный" (200 eDTU)|32|2|64|
|Эластичные пулы ценовой категории "Стандартный" (300 eDTU)|32|3|96|
|Эластичные пулы ценовой категории "Стандартный" (400 eDTU)|32|3|96|
|Эластичные пулы ценовой категории "Стандартный" (800 eDTU)|32|6|192|
|Эластичные пулы ценовой категории "Стандартный" (1200 eDTU)|32|10|320|
|Эластичные пулы ценовой категории "Стандартный" (1600–3000 eDTU)|32|12|384|
|Эластичные пулы уровня "Премиум" (все конфигурации DTU)|13.9|12|166.7|
||||

### <a name="tempdb-sizes-for-vcore-based-service-tiers"></a>Размеры базы данных tempdb для уровней служб на основе виртуальных ядер

См. сведения об [ограничениях ресурсов на основе виртуальных ядер](/azure/sql-database/sql-database-vcore-resource-limits).

## <a name="restrictions"></a>Ограничения

С базой данных `tempdb` нельзя выполнять следующие операции:  
  
- Добавление файловых групп.
- Резервное копирование и восстановление из копии.
- Изменение параметров сортировки. Параметрами сортировки по умолчанию являются параметры сортировки сервера.
- Изменение владельца базы данных. Владельцем `tempdb` является *sa*
- Создание моментального снимка базы данных.
- Удаление базы данных.
- Удаление пользователя *guest* из базы данных.
- Включение отслеживания измененных данных.
- Участие в зеркальном отображении базы данных.
- Удаление первичной файловой группы, первичного файла данных или файла журнала.
- Переименование базы данных или первичной файловой группы.
- Выполнение `DBCC CHECKALLOC`.
- Выполнение `DBCC CHECKCATALOG`.
- Перевод базы данных в режим `OFFLINE`.
- Перевод базы данных или первичной файловой группы в режим `READ_ONLY`.
  
## <a name="permissions"></a>Разрешения

Любой пользователь может создавать временные объекты в `tempdb`. Если не предоставлены дополнительные разрешения, пользователям доступны только принадлежащие им объекты. Можно отозвать разрешение на подключение к `tempdb`, чтобы запретить пользователю работать с `tempdb`. Но делать это не рекомендуется, так как `tempdb` требуется для выполнения некоторых стандартных операций.  

## <a name="optimizing-tempdb-performance-in-sql-server"></a>Оптимизация производительности базы данных tempdb в SQL Server
Размер и физическое размещение базы данных `tempdb` может влиять на производительность системы. Например, если для базы данных `tempdb` установлен слишком малый размер, часть системной нагрузки может приходиться на автоувеличение `tempdb` до размера, требуемого для поддержки рабочей нагрузки при каждом перезапуске экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

По возможности используйте [мгновенную инициализацию файлов](../../relational-databases/databases/database-instant-file-initialization.md), чтобы повысить производительность операций увеличения файлов данных.

Заранее выделите место для всех файлов `tempdb`, установив для файла размер, достаточный для обеспечения обычной рабочей нагрузки в среде. Предварительное выделение позволяет избежать слишком частого расширения `tempdb`, способного повлиять на производительность. Следует установить автоувеличение для базы данных `tempdb`, чтобы увеличить место на диске для незапланированных исключений.

Файлы данных в каждой [файловой группе](../../relational-databases/databases/database-files-and-filegroups.md#filegroups) должны иметь одинаковый размер, так как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует алгоритм пропорционального заполнения, который повышает вероятность выделения памяти в файлах с большим объемом свободного пространства. Разделение `tempdb` на множество файлов данных равного размера обеспечивает эффективное выполнение использующих `tempdb` операций с высокой степенью параллелизма.

Установите приемлемое значение шага увеличения размера файла, чтобы оно не было слишком низким для файлов базы данных `tempdb`. Если увеличение размера файлов будет слишком малым по сравнению с объемом записываемых в `tempdb` данных, `tempdb` может постоянно требовать расширения. Это скажется на производительности.

Чтобы проверить текущий размер и параметры увеличения для `tempdb`, используйте следующий запрос:

```sql
 SELECT name AS FileName,
    size*1.0/128 AS FileSizeInMB,
    CASE max_size
        WHEN 0 THEN 'Autogrowth is off.'
        WHEN -1 THEN 'Autogrowth is on.'
        ELSE 'Log file grows to a maximum size of 2 TB.'
    END,
    growth AS 'GrowthValue',
    'GrowthIncrement' =
        CASE
            WHEN growth = 0 THEN 'Size is fixed.'
            WHEN growth > 0 AND is_percent_growth = 0
                THEN 'Growth value is in 8-KB pages.'
            ELSE 'Growth value is a percentage.'
        END
FROM tempdb.sys.database_files;
GO
```

Поместите базу данных `tempdb` в быструю подсистему ввода-вывода. Если имеется много непосредственно присоединенных дисков, то используйте чередование дисков. Отдельные файлы данных `tempdb` или их группы не обязательно должны располагаться на разных дисках или шпинделях, если только у вы не наблюдаете узкие места в подсистеме ввода-вывода.

Расположите базу данных `tempdb` на дисках, отличающихся от используемых пользовательскими базами данных.

## <a name="performance-improvements-in-tempdb-for-sql-server"></a>Увеличение производительности базы данных tempdb в SQL Server
Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], производительность `tempdb` дополнительно оптимизирована следующим образом:  
  
- Временные таблицы и табличные переменные кэшируются. Кэширование позволяет операциям удаления и создания временных объектов выполняться очень быстро. Кэширование также снижает вероятность возникновения состязаний, связанных с метаданными и выделением страниц.  
- Усовершенствован протокол кратковременных блокировок выделения страниц для снижения количества используемых кратковременных блокировок `UP` (обновление).  
- Снижены затраты ресурсов на ведение журнала `tempdb` — уменьшено потребление пропускной способности подсистемы ввода-вывода файлом журнала `tempdb`.  
- Программа установки добавляет множество файлов данных `tempdb` при установке нового экземпляра. Эту задачу можно выполнить с помощью нового элемента управления для ввода в пользовательском интерфейсе в разделе **Настройка ядра СУБД** и параметра командной строки `/SQLTEMPDBFILECOUNT`. По умолчанию программа установки добавляет столько файлов данных `tempdb`, сколько имеется логических процессоров, но их может быть не больше восьми.  
- При наличии множества файлов данных `tempdb` автоматическое увеличение выполняется для всех файлов в одно время и в равном объеме согласно параметрам увеличения. [Флаг трассировки 1117](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) больше не требуется.  
- Для всех операций распределения в `tempdb` используются единообразные экстенты. [Флаг трассировки 1118](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) больше не требуется.  
- Для первичной файловой группы свойство `AUTOGROW_ALL_FILES` включено и не может быть изменено.

Дополнительные сведения об улучшениях производительности в `tempdb` см. в статье блога [TEMPDB — Files and Trace Flags and Updates, Oh My!](/archive/blogs/sql_server_team/tempdb-files-and-trace-flags-and-updates-oh-my) (TEMPDB — файлы, флаги трассировки и обновления).

## <a name="memory-optimized-tempdb-metadata"></a>Оптимизированные для памяти метаданные tempdb
Состязание метаданных `tempdb` всегда было узким местом для масштабируемости многих рабочих нагрузок, выполняющихся в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. В [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] появилась новая функция оптимизированных для памяти метаданных tempdb, входящая в семейство функций [выполняющейся в памяти базы данных](../in-memory-database.md). 

Она эффективно устраняет существующее узкое место и открывает новый уровень масштабируемости для рабочих нагрузок, активно использующих tempdb. В [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] системные таблицы, связанные с управлением метаданными временных таблиц, можно переместить в неустойчивые таблицы без кратковременной блокировки, оптимизированные для памяти.

Просмотрите это 7-минутное видео, чтобы узнать, как и когда следует использовать метаданные tempdb, оптимизированные для памяти:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-and-When-To-Memory-Optimized-TempDB-Metadata/player?WT.mc_id=dataexposed-c9-niner]


### <a name="configuring-and-using-memory-optimized-tempdb-metadata"></a>Настройка и использование метаданных оптимизированной для памяти базы данных tempdb

Чтобы согласиться на применение этой новой функции, используйте следующий скрипт:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
```

Чтобы это изменение конфигурации вступило в силу, нужно перезапустить службу.

Вы можете проверить, является ли `tempdb` оптимизированной для памяти, используя следующую команду T-SQL:

```sql
SELECT SERVERPROPERTY('IsTempdbMetadataMemoryOptimized');
```

Если по какой-то причине не удается запустить сервер после включения оптимизированных для памяти метаданных `tempdb`, можно обойти эту функцию, запустив экземпляр SQL Server в [минимальной конфигурации](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md) с помощью параметра запуска **-f**. После этого вы можете отключить функцию и перезапустить SQL Server в нормальном режиме.

Чтобы защитить сервер от потенциальных состояний нехватки памяти, можно привязать `tempdb` к [пулу ресурсов](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md). В этом случае вместо действий, которые обычно выполняются при привязке пула ресурсов к базе данных, следует использовать команду [`ALTER SERVER`](../../t-sql/statements/alter-server-configuration-transact-sql.md).

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
```

Кроме того, даже если метаданные оптимизированной для памяти базы данных tempdb уже включены, чтобы это изменение вступило в силу, требуется перезагрузка.

### <a name="memory-optimized-tempdb-limitations"></a>Ограничения оптимизированной для памяти базы данных tempdb

- Включение и отключение функции не является динамическим. Из-за внутренних изменений, которые необходимо внести в структуру `tempdb`, для включения или отключения этой функции требуется перезапуск.

- Отдельная транзакция не может обратиться к таблицам, оптимизированным для памяти, в более чем одной базе данных. Все транзакции, связанные с таблицей, оптимизированной для памяти, в пользовательской базе данных, не смогут обратиться к системным представлениям `tempdb` в той же транзакции. Если вы попытаетесь обратиться к системным представлениям `tempdb` в транзакции с участием таблицы, оптимизированной для памяти, в пользовательской базе данных, возникнет следующая ошибка:
    
  ```
  A user transaction that accesses memory optimized tables or natively compiled modules cannot access more than one user database or databases model and msdb, and it cannot write to master.
  ```
    
  Пример.
    
  ```sql
  BEGIN TRAN;
  
  SELECT *
  FROM tempdb.sys.tables;  -----> Creates a user in-memory OLTP transaction in tempdb
  
  INSERT INTO <user database>.<schema>.<mem-optimized table>
  VALUES (1); ----> Tries to create a user in-memory OLTP transaction in the user database but will fail
  
  COMMIT TRAN;
  ```
    
- Запросы к таблицам, оптимизированным для памяти, не поддерживают указания блокировки и изоляции, поэтому запросы к представлениям каталога оптимизированной для памяти `tempdb` не будут учитывать указания блокировки и изоляции. Как и в случае с другими системными представлениями каталога в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], все транзакции для системных представлений будут находиться в изоляции `READ COMMITTED` (или `READ COMMITTED SNAPSHOT` в нашем случае).

- Если оптимизированные для памяти метаданные `tempdb` включены, [индексы columnstore](../indexes/columnstore-indexes-overview.md) нельзя создавать во временных таблицах.

- В связи с ограничением на индексы columnstore использование системной хранимой процедуры `sp_estimate_data_compression_savings` с параметром сжатия данных `COLUMNSTORE` или `COLUMNSTORE_ARCHIVE` не поддерживается, если включены оптимизированные для памяти метаданные `tempdb`.

> [!NOTE] 
> Эти ограничения применяются только при создании ссылок на системные представления `tempdb`. При необходимости вы сможете создать временную таблицу в той же транзакции, где обращаетесь к таблице, оптимизированной для памяти, в пользовательской базе данных.

## <a name="capacity-planning-for-tempdb-in-sql-server"></a>Планирование ресурсов для tempdb в SQL Server
Определение требуемого размера `tempdb` в рабочей среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] зависит от многих факторов. Как описано выше, эти факторы включают текущую рабочую нагрузку и используемые функции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Рекомендуется проанализировать текущую рабочую нагрузку, выполнив следующие задачи в среде тестирования SQL Server:

- Включите автоувеличение для `tempdb`.
- Запускайте отдельные запросы или файлы трассировки рабочей нагрузки и следите за использованием диска базой данных `tempdb`.
- Выполняйте операции обслуживания индексов, например перестроение индексов, и следите за использованием диска базой данных `tempdb`.
- Используйте значения используемого пространства на диске из предыдущих шагов для прогнозирования общей рабочей нагрузки. Скорректируйте полученное значение с учетом предполагаемой параллельной обработки и задайте соответствующий размер `tempdb`.

## <a name="monitoring-tempdb-use"></a>Мониторинг использования tempdb
Нехватка места на диске для `tempdb` может привести к значительным сбоям рабочей среды [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а также помешать работающим приложениям завершить операции. Для отслеживания места на диске, используемого в файлах `tempdb`, можно применять динамическое административное представление [sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md):

```sql
 -- Determining the amount of free space in tempdb
SELECT SUM(unallocated_extent_page_count) AS [free pages],
  (SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by the version store
SELECT SUM(version_store_reserved_page_count) AS [version store pages used],
  (SUM(version_store_reserved_page_count)*1.0/128) AS [version store space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by internal objects
SELECT SUM(internal_object_reserved_page_count) AS [internal object pages used],
  (SUM(internal_object_reserved_page_count)*1.0/128) AS [internal object space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by user objects
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],
  (SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]
FROM sys.dm_db_file_space_usage;
 ```

Для мониторинга действий выделения и освобождения страниц в `tempdb` на уровне сеансов или задач можно использовать динамические административные представления [sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) и [sys.dm_db_task_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md). Эти представления позволяют выявлять большие запросы, временные таблицы или табличные переменные, которые используют много места на диске для `tempdb`. Кроме того, вы можете использовать несколько счетчиков для мониторинга свободного места в базе данных `tempdb` и ресурсов, использующих `tempdb`.

```sql
-- Obtaining the space consumed by internal objects in all currently running tasks in each session
SELECT session_id,
  SUM(internal_objects_alloc_page_count) AS task_internal_objects_alloc_page_count,
  SUM(internal_objects_dealloc_page_count) AS task_internal_objects_dealloc_page_count
FROM sys.dm_db_task_space_usage
GROUP BY session_id;

-- Obtaining the space consumed by internal objects in the current session for both running and completed tasks
SELECT R2.session_id,
  R1.internal_objects_alloc_page_count
  + SUM(R2.internal_objects_alloc_page_count) AS session_internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count
  + SUM(R2.internal_objects_dealloc_page_count) AS session_internal_objects_dealloc_page_count
FROM sys.dm_db_session_space_usage AS R1
INNER JOIN sys.dm_db_task_space_usage AS R2 ON R1.session_id = R2.session_id
GROUP BY R2.session_id, R1.internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count;;
```

## <a name="related-content"></a>См. также
[Параметр SORT_IN_TEMPDB для индексов](../../relational-databases/indexes/sort-in-TempDB-option-for-indexes.md)    
[Системные базы данных](../../relational-databases/databases/system-databases.md)    
[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)    
[sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)    
[Перемещение файлов базы данных](../../relational-databases/databases/move-database-files.md)    
