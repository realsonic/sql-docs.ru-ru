---
description: DATABASEPROPERTYEX (Transact-SQL)
title: DATABASEPROPERTYEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATABASEPROPERTYEX
- DATABASEPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DATABASEPROPERTYEX function
- displaying database properties
- database properties [SQL Server]
ms.assetid: 8a9e0ffb-28b5-4640-95b2-a54e3e5ad941
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 096d215aa0962b3a580cdd43a337075aab638964
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171146"
---
# <a name="databasepropertyex-transact-sql"></a>DATABASEPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Для указанной в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базы данных возвращает текущее значение заданного параметра или свойства базы данных.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DATABASEPROPERTYEX ( database , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*database*  
Выражение, указывающее имя базы данных, для которой `DATABASEPROPERTYEX` возвращает значение именованного свойства. Аргумент *database* имеет тип данных **nvarchar(128)** .  

Для [!INCLUDE[ssSDS](../../includes/sssds-md.md)]`DATABASEPROPERTYEX` должно быть именем текущей базы данных. Возвращает значение NULL для всех свойств, если указано другое имя базы данных.
  
*property*  
Выражение, которое задает имя возвращаемого свойства базы данных. Аргумент *property* имеет тип данных **varchar(128)** и может принимать одно из значений, приведенных в таблице ниже.
  
> [!NOTE]  
>  Если база данных еще не запущена, то вызовы `DATABASEPROPERTYEX` возвращают значения NULL, если `DATABASEPROPERTYEX` получает эти значения путем прямого доступа к базе данных, а не из метаданных. База данных, для которой параметр AUTO_CLOSE имеет значение ON или которая находится вне сети по иной причине, определяется как "незапущенная".  
  
|Свойство|Описание|Возвращенное значение|  
|---|---|---|
|Параметры сортировки|Имя параметров сортировки, установленных для базы данных по умолчанию.|Имя параметров сортировки.<br /><br /> NULL: база данных не запущена.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ComparisonStyle|Стиль сравнения Windows для параметров сортировки. Используйте приведенные ниже значения стиля с целью построения битовой карты для готового значения ComparisonStyle.<br /><br /> Игнорировать регистр: 1<br /><br /> Не учитывать диакритические знаки: 2<br /><br /> Не учитывать тип японской азбуки: 65536<br /><br /> Не учитывать ширину: 131072<br /><br /> <br /><br /> Например, значение по умолчанию — 196609 — образуется в результате сочетания параметров «Без учета регистра», «Без учета типа японской азбуки» и «Без учета ширины».|Возвращает стиль сравнения.<br /><br /> Возвращает значение 0 для всех двоичных параметров сортировки.<br /><br /> Базовый тип данных: **int**|  
|Выпуск|Уровень выпуска или службы базы данных.|**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br /> <br /><br /> Общее назначение<br /><br /> Критически важный для бизнеса<br /><br /> Basic<br /><br /> Standard<br /><br /> Premium<br /><br /> Системный (для базы данных master)<br /><br /> NULL: база данных не запущена.<br /><br /> Базовый тип данных: **nvarchar**(64)|  
|IsAnsiNullDefault|База данных следует правилам ISO по разрешению значений NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAnsiNullsEnabled|При всех сравнениях со значением NULL результат не определен.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAnsiPaddingEnabled|Строки перед сравнением или вставкой дополняются до одной и той же длины.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAnsiWarningsEnabled|SQL Server выводит сообщения об ошибках или предупреждения, когда возникают стандартные условия ошибки.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsArithmeticAbortEnabled|Запрос завершается, если в процессе его выполнения происходит ошибка переполнения или деления на нуль.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAutoClose|После выхода последнего пользователя база данных корректно выключается и освобождает ресурсы.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAutoCreateStatistics|Оптимизатор запросов при необходимости создает статистику по отдельным столбцам для повышения производительности запросов.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAutoCreateStatisticsIncremental|Автоматические статистики в одном столбце создаются в дополнительном виде везде, где это возможно.|**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAutoShrink|Файлы базы данных являются кандидатами на автоматическое периодическое сжатие.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsAutoUpdateStatistics|Оптимизатор запросов обновляет существующую статистику, используемую в запросе, если она может оказаться устаревшей.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|
|IsClone|База данных представляет собой копию только схемы и статистики пользовательской базы данных, созданной с помощью DBCC CLONEDATABASE. Дополнительные сведения см. в этой [статье службы поддержки Майкрософт](https://support.microsoft.com/help/3177838).|**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) и выше.<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**| 
|IsCloseCursorsOnCommitEnabled|Все открытые курсоры закрываются при фиксации транзакции.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsFulltextEnabled|В базе данных включены полнотекстовое и семантическое индексирование.|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**<br /><br /> **Примечание.** Значение этого свойства сейчас не учитывается. Полнотекстовый поиск всегда включен для пользовательских баз данных. Это свойство будет удалено в будущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Не используйте его при работе над новыми приложениями и как можно быстрее измените приложения, в которых оно в настоящее время используется.|  
|IsInStandBy|В режиме «в сети» база данных доступна только для чтения, при этом разрешен журнал восстановления.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsLocalCursorsDefault|Объявления курсора по умолчанию — LOCAL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsMemoryOptimizedElevateToSnapshotEnabled|К таблицам, оптимизированным для памяти, доступ производится с использованием изоляции SNAPSHOT, когда в TRANSACTION ISOLATION LEVEL установлен уровень изоляции READ COMMITTED, READ UNCOMMITTED или более низкий.|**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> Базовый тип данных: **int**|  
|IsMergePublished|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает публикацию таблиц базы данных для репликации слиянием, если репликация установлена.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsNullConcat|Объединение операнда со значением NULL дает значение NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsNumericRoundAbortEnabled|При потере точности в выражениях возникают ошибки.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsParameterizationForced|Параметр SET PARAMETERIZATION имеет значение FORCED.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные|  
|IsQuotedIdentifiersEnabled|Двойные кавычки можно использовать в идентификаторах.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsPublished|Если репликация установлена, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает публикацию таблиц базы данных для репликации моментальных снимков или транзакций.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsRecursiveTriggersEnabled|Рекурсивное срабатывание триггеров разрешено.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsSubscribed|База данных подписана на публикацию.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsSyncWithBackup|База данных является опубликованной либо базой данных распространителя и поддерживает восстановление без нарушения репликации транзакций.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**|  
|IsTornPageDetectionEnabled|Компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] выявляет незавершенные операции ввода-вывода, вызванные сбоями питания или другими перерывами в работе системы.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**| 
|IsVerifiedClone|База данных представляет собой копию только схемы и статистики пользовательской базы данных, созданной с помощью параметра WITH VERIFY_CLONEDB функции DBCC CLONEDATABASE. Дополнительные сведения см. в этой [статье службы поддержки Майкрософт](https://support.microsoft.com/help/3177838).|**Область применения**: начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2).<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **int**| 
|IsXTPSupported|Указывает, поддерживает ли база данных выполняющуюся в памяти OLTP, то есть создание и использование таблиц, оптимизированных для памяти, и модулей, скомпилированных в собственном коде.<br /><br /> Относится к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:<br /><br /> IsXTPSupported не зависит от наличия файловой группы MEMORY_OPTIMIZED_DATA, которая требуется для создания объектов выполняющейся в памяти OLTP.|**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные, ошибка или неприменимо<br /><br /> Базовый тип данных: **int**|  
|LastGoodCheckDbTime|Дата и время последней успешной команды DBCC CHECKDB, выполненной в указанной базе данных. <sup>1</sup> Если команда DBCC CHECKDB не была выполнена в базе данных, возвращается 1900-01-01 00:00:00.000.|**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] начиная с SP2.</br>[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] начиная с накопительного пакета обновления 9 (CU9).</br>[!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] или более поздняя версия.</br>База данных SQL Azure.<br/><br/>Значение datetime<br /><br /> NULL: недопустимые входные данные<br /><br /> Базовый тип данных: **datetime**| 
|LCID|Код языка Windows для параметров сортировки.|Значение кода языка (в десятичном формате).<br /><br /> Базовый тип данных: **int**|  
|MaxSizeInBytes|Максимальный размер базы данных в байтах.|**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br />[База данных SQL Azure и Azure Synapse Analytics](/azure/sql-database/sql-database-single-database-scale#dtu-based-purchasing-model) — значение зависит от целевого показателя обслуживания, если не куплено дополнительное хранилище.<br /><br />[Виртуальное ядро](/azure/sql-database/sql-database-single-database-scale#vcore-based-purchasing-model) — значение увеличивается на 1 ГБ до максимального размера.<br /><br />NULL: база данных не запущена<br /><br /> Базовый тип данных: **bigint**|  
|Восстановление|Модель восстановления базы данных|FULL: Модель полного восстановления<br /><br /> BULK_LOGGED: модель с неполным протоколированием<br /><br /> SIMPLE: Простая модель восстановления<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ServiceObjective|Описывает уровень производительности базы данных в [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] или [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].|Это может быть:<br /><br /> NULL = база данных не запущена<br /><br /> Общий (для выпусков Web или Business)<br /><br /> Basic<br /><br /> S0<br /><br /> S1<br /><br /> S2<br /><br /> S3<br /><br /> P1<br /><br /> P2<br /><br /> P3<br /><br /> ElasticPool<br /><br /> Системный (для базы данных master)<br /><br /> Базовый тип данных: **nvarchar(32)**|  
|ServiceObjectiveId|Идентификатор цели службы в [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].|**uniqueidentifier**, определяющий цель службы.|  
|SQLSortOrder|Идентификатор порядка сортировки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], поддерживаемого в предыдущих версиях SQL Server.|0: в базе данных используются параметры сортировки Windows<br /><br /> >0: идентификатор порядка сортировки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NULL: недопустимые входные данные, или база данных не запущена<br /><br /> Базовый тип данных: **tinyint**|  
|Состояние|Состояние базы данных.|ONLINE: база данных доступна для запросов.<br /><br /> **Примечание.** Функция может вернуть состояние ONLINE, пока база данных открывается и еще не восстановлена. Чтобы определить, может ли база данных в состоянии ONLINE принимать соединения, запросите свойство Collation функции **DATABASEPROPERTYEX**. База данных в состоянии ONLINE может принимать соединения, если параметры сортировки базы данных возвращают значение, отличное от NULL. Применительно к базам данных AlwaysOn выполните запрос к столбцу database_state или database_state_desc представления `sys.dm_hadr_database_replica_states`.<br /><br /> OFFLINE: база данных явным образом переведена в режим "вне сети".<br /><br /> RESTORING: началось восстановление базы данных.<br /><br /> RECOVERING: восстановление базы данных началось, и она еще не готова к запросам.<br /><br /> SUSPECT: база данных не восстанавливалась.<br /><br /> EMERGENCY: база данных находится в аварийном состоянии и доступна только для чтения. Доступ ограничен членами роли sysadmin.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|Updateability|Указывает, можно ли изменять данные.|READ_ONLY: база данных поддерживает операции чтения, но не изменения данных.<br /><br /> READ_WRITE: база данных поддерживает операции чтения и изменения данных.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|UserAccess|Указывает пользователей, имеющих доступ к базе данных.|SINGLE_USER: в каждый момент времени доступ имеет только один пользователь db_owner, dbcreator или sysadmin<br /><br /> RESTRICTED_USER: только члены ролей db_owner, dbcreator или sysadmin<br /><br /> MULTI_USER: все пользователи<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|Версия|Внутренний номер версии того кода [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с которым была создана база данных. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|Номер версии: база данных открыта.<br /><br /> NULL: база данных не запущена.<br /><br /> Базовый тип данных: **int**| 

<br/>   

> [!NOTE]  
> <sup>1</sup> Для баз данных, входящих в группу доступности, `LastGoodCheckDbTime` возвращает дату и время последней успешной команды DBCC CHECKDB в первичной реплике независимо от того, из какой реплики она запускалась. 

## <a name="return-types"></a>Типы возвращаемых данных
**sql_variant**
  
## <a name="exceptions"></a>Исключения  
Возвращает значение NULL в случае ошибки или если участник не имеет разрешений для просмотра объекта.
  
В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пользователь может просматривать только метаданные защищаемых объектов, которыми он владеет или на которые ему были предоставлены разрешения. Это означает, что встроенные функции, создающие метаданные, такие как `OBJECT_ID`, могут вернуть значение NULL в случае, если у пользователя нет разрешений для объекта. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Remarks  
Функция `DATABASEPROPERTYEX` возвращает каждый раз значение только одного свойства. Для отображения значений нескольких свойств используйте представление каталога [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).
  
## <a name="examples"></a>Примеры  
  
### <a name="a-retrieving-the-status-of-the-auto_shrink-database-option"></a>A. Получение состояния параметра базы данных AUTO_SHRINK  
В приведенном ниже примере возвращается состояние параметра AUTO_SHRINK базы данных `AdventureWorks`.
  
```sql
SELECT DATABASEPROPERTYEX('AdventureWorks2014', 'IsAutoShrink');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] Это означает, что параметр AUTO_SHRINK отключен.
  
```
------------------  
0  
```  
  
### <a name="b-retrieving-the-default-collation-for-a-database"></a>Б. Получение установленных по умолчанию параметров сортировки для базы данных  
В приведенном ниже примере возвращаются несколько атрибутов базы данных `AdventureWorks`.
  
```sql
SELECT   
    DATABASEPROPERTYEX('AdventureWorks2014', 'Collation') AS Collation,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'Edition') AS Edition,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'ServiceObjective') AS ServiceObjective,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'MaxSizeInBytes') AS MaxSizeInBytes  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Collation                     Edition        ServiceObjective  MaxSizeInBytes  
----------------------------  -------------  ----------------  --------------  
SQL_Latin1_General_CP1_CI_AS  DataWarehouse  DW1000            5368709120  
```  
  
## <a name="see-also"></a>См. также раздел
[ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
[Состояния базы данных](../../relational-databases/databases/database-states.md)  
[sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)
  
  
