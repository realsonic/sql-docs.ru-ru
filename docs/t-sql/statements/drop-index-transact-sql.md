---
description: DROP INDEX (Transact-SQL)
title: DROP INDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_INDEX_TSQL
- DROP INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- nonclustered indexes [SQL Server], removing
- MAXDOP index option, DROP INDEX statement
- index removal [SQL Server]
- spatial indexes [SQL Server], dropping
- removing indexes
- deleting indexes
- dropping indexes
- MOVE TO clause
- clustered indexes, removing
- indexes [SQL Server], dropping
- filtered indexes [SQL Server], dropping
- ONLINE option
- indexes [SQL Server], moving
- XML indexes [SQL Server], dropping
- DROP INDEX statement
ms.assetid: 2b1464c8-934c-405f-8ef7-2949346b5372
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4e6da2b809301b38c4d0a60b82c39bd9e3901641
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100824"
---
# <a name="drop-index-transact-sql"></a>DROP INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет один или несколько реляционных, пространственных, фильтруемых или XML-индексов из текущей базы данных. Можно удалить кластеризованный индекс и переместить полученную в результате таблицу в другую файловую группу или схему секционирования в одной транзакции, указав параметр MOVE TO.  
  
 Инструкция DROP INDEX неприменима к индексам, созданным при указании ограничений параметров PRIMARY KEY и UNIQUE. Для удаления ограничения и соответствующего индекса используется инструкция [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) с предложением DROP CONSTRAINT.  
  
> [!IMPORTANT]
>  Синтаксис, определяемый в `<drop_backward_compatible_index>`, не будет поддерживаться в будущих версиях [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте использования этого синтаксиса в новых разработках и учитывайте необходимость изменения в будущем приложений, использующих эти функции сейчас. Используйте синтаксис, описанный в `<drop_relational_or_xml_index>`. XML-индексы нельзя удалить с использованием обратно совместимого синтаксиса.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server (All options except filegroup and filestream apply to Azure SQL Database.)  
  
DROP INDEX [ IF EXISTS ]   
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
| <drop_backward_compatible_index> [ ,...n ]  
}  
  
<drop_relational_or_xml_or_spatial_index> ::=  
    index_name ON <object>   
    [ WITH ( <drop_clustered_index_option> [ ,...n ] ) ]  
  
<drop_backward_compatible_index> ::=  
    [ owner_name. ] table_or_view_name.index_name  
  
<object> ::=  
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
  
<drop_clustered_index_option> ::=  
{  
    MAXDOP = max_degree_of_parallelism  
  | ONLINE = { ON | OFF }  
  | MOVE TO { partition_scheme_name ( column_name )   
            | filegroup_name  
            | "default"   
            }  
  [ FILESTREAM_ON { partition_scheme_name   
            | filestream_filegroup_name   
            | "default" } ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database  
  
DROP INDEX  
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
}  
  
<drop_relational_or_xml_or_spatial_index> ::=   
    index_name ON <object>  
  
<object> ::=   
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP INDEX index_name ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Условное удаление индекса только в том случае, если он уже существует.  
  
 *index_name*  
 Имя индекса, который необходимо удалить.  
  
 *database_name*  
 Имя базы данных.  
  
 *schema_name*  
 Имя схемы, которой принадлежит таблица или представление.  
  
 *table_or_view_name*  
 Имя таблицы или представления, связанного с индексом. Пространственные индексы поддерживаются только для таблиц.  
  
 Чтобы отобразить отчет по индексам объекта, следует воспользоваться представлением каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 База данных SQL Azure поддерживает формат трехкомпонентного имени имя_базы_данных.[имя_схемы].имя_объекта, если имя_базы_данных — это текущая база данных или tempdb, а имя_объекта начинается с символа #.  
  
 \<drop_clustered_index_option>  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Управляет параметрами кластеризованного индекса. Эти параметры неприменимы к другим типам индексов.  
  
 MAXDOP = *max_degree_of_parallelism*  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] (только уровни производительности P2 и P3).  
  
 Переопределяет параметр конфигурации **max degree of parallelism** на время выполнения операции с индексами. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). MAXDOP можно использовать для ограничения числа процессоров, используемых при параллельном выполнении планов. Максимальное число процессоров — 64.  
  
> [!IMPORTANT]  
>  Параметр MAXDOP нельзя использовать для пространственных или XML-индексов.  
  
 Параметр *max_degree_of_parallelism* может иметь одно из следующих значений:  
  
 1  
 Подавляет формирование параллельных планов.  
  
 \>1  
 Ограничивает указанным значением максимальное число процессоров, используемых для параллельных операций с индексами.  
  
 0 (по умолчанию)  
 В зависимости от текущей рабочей нагрузки системы использует реальное или меньшее число процессоров.  
  
 Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]  
>  Параллельные операции с индексами доступны не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ONLINE = ON | **OFF**  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Определяет, будут ли базовые таблицы и связанные индексы доступны для запросов и изменения данных во время операций с индексами. Значение по умолчанию — OFF.  
  
 ON  
 Не устанавливаются долгосрочные блокировки таблицы. Это позволяет продолжать выполнение запросов и обновлений базовых таблиц.  
  
 OFF  
 Применяются блокировки таблиц, при этом таблицы становятся недоступны на время выполнения индексирования.  
  
 Параметр ONLINE можно указать только при удалении кластеризованных индексов. Дополнительные сведения см. в разделе «Примечания».  
  
> [!NOTE]  
>  Операции с индексами в сети доступны не во всех выпусках [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ **)**  | _filegroup\_name_ |  **"** default **"**  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий. [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] поддерживает "default" в качестве имени файловой группы.  
  
 Определяет размещение, куда будут перемещаться строки данных, находящиеся на конечном уровне кластеризованного индекса. Данные перемещаются в новое расположение со структурой типа куча. В качестве нового расположения можно указать файловую группу или схему секционирования, но они должны уже существовать. Параметр MOVE TO недопустим для индексированных представлений и некластеризованных индексов. Если ни схема секционирования, ни файловая группа не указаны, результирующая таблица помещается в схему секционирования или файловую группу, которая определена для кластеризованного индекса.  
  
 Если кластеризованный индекс удаляется с помощью параметра MOVE TO, то все некластеризованные индексы базовых таблиц создаются заново, но остаются в исходных файловых группах или схемах секционирования. Если базовая таблица перемещается в другую файловую группу или схему секционирования, некластеризованные индексы не перемещаются для совпадения с новым расположением базовой таблицы (кучи). Поэтому некластеризованные индексы могут потерять выравнивание с кучей, даже если ранее они были выровнены с кластеризованным индексом. Дополнительные сведения о выравнивании секционированных индексов см. в разделе [Секционированные таблицы и индексы](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 _имя_схемы_секционирования_ **(** _имя_столбца_ **)**  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Указывает схему секционирования, в которой будет размещена результирующая таблица. Схема секционирования должна быть создана заранее выполнением инструкции [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) или [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Если размещение не указано и таблица секционирована, таблица включается в ту же схему секционирования, где размещен существующий кластеризованный индекс.  
  
 Имя столбца в схеме не обязательно должно соответствовать столбцам из определения индекса. Можно указать любой столбец базовой таблицы.  
  
 *filegroup_name*  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Указывает файловую группу, в которую будет помещена результирующая таблица. Если размещение не указано и таблица не секционирована, тогда результирующая таблица включается в ту файловую группу, где размещен существующий кластеризованный индекс. Файловая группа должна существовать.  
  
 **"** default **"**  
 Указывает размещение по умолчанию для результирующей таблицы.  
  
> [!NOTE]
>  В этом контексте default не является ключевым словом. Это идентификатор файловой группы по умолчанию, и поэтому он должен быть заключен в разделители, например: MOVE TO **"** default **"** или MOVE TO **[** default **]** . Если указывается параметр **"** default **"** , то параметр QUOTED_IDENTIFIER для текущего сеанса должен иметь значение ON. Это параметр по умолчанию. Дополнительные сведения см. в статье [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 FILESTREAM_ON { *partition_scheme_name* | *filestream_filegroup_name* |  **"** default **"** }  
 **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
 Определяет папку, в которую будет перемещаться таблица FILESTREAM, находящаяся на конечном уровне кластеризованного индекса. Данные перемещаются в новое расположение со структурой типа куча. В качестве нового расположения можно указать файловую группу или схему секционирования, но они должны уже существовать. Параметр FILESTREAM ON недопустим для индексированных представлений или некластеризованных индексов. Если не указана схема секционирования, то данные будут размещены в той же схеме секционирования или файловой группе, которая была определена для кластеризованного индекса.  
  
 *partition_scheme_name*  
 Указывает схему секционирования для данных FILESTREAM. Схема секционирования должна быть создана заранее выполнением инструкции [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) или [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Если размещение не указано и таблица секционирована, таблица включается в ту же схему секционирования, где размещен существующий кластеризованный индекс.  
  
 При указании схемы секционирования для инструкции MOVE TO необходимо использовать ту же схему секционирования, что и для инструкции FILESTREAM ON.  
  
 *filestream_filegroup_name*  
 Указывает файловую группу FILESTREAM для данных FILESTREAM. Если расположение не указано, а таблица не секционирована, данные включаются в файловую группу FILESTREAM по умолчанию.  
  
 **"** default **"**  
 Указывает расположение по умолчанию для данных FILESTREAM.  
  
> [!NOTE]
>  В этом контексте default не является ключевым словом. Это идентификатор файловой группы по умолчанию, и поэтому он должен быть заключен в разделители, например: MOVE TO **"** default **"** или MOVE TO **[** default **]** . Если указано значение "default" (по умолчанию), параметр QUOTED_IDENTIFIER должен иметь значение ON для текущего сеанса. Это параметр по умолчанию. Дополнительные сведения см. в статье [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
## <a name="remarks"></a>Remarks  
 При удалении некластеризованного индекса его определение удаляется из метаданных, а страницы данных сбалансированного дерева индекса удаляются из файлов базы данных. При удалении кластеризованного индекса определение индекса удаляется из метаданных, а строки данных, которые хранились на конечном уровне кластеризованного индекса, сохраняются в результирующей неупорядоченной таблице — куче. Все пространство, ранее занимаемое индексом, освобождается. Оно может быть впоследствии использовано любым объектом базы данных.  
  
 Индекс невозможно удалить, если файловая группа, в которой он размещен, находится в режиме вне сети или доступна только для чтения.  
  
 Если удален кластеризованный индекс индексированного представления, то все некластеризованные индексы и автоматически создаваемые статистики в этом представлении автоматически удаляются. Статистики, созданные вручную, не удаляются.  
  
 Синтаксис _table\_or\_view\_name_ **.** _index\_name_ сохраняется для обратной совместимости. Пространственный или XML-индекс нельзя удалить с использованием синтаксиса обратной совместимости.  
  
 Если удаляемый индекс содержит 128 и более экстентов, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] откладывает действительное освобождение страниц и связанных с ними блокировок до фиксации транзакции.  
  
 Иногда индексы удаляются и пересоздаются для реорганизации или перестроения индекса, например чтобы применить новое значение коэффициента заполнения, или для реорганизации данных после массовой загрузки. Для этих задач более эффективно использование инструкции [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md), особенно для кластеризованных индексов. Инструкция ALTER INDEX REBUILD выполняется с оптимизациями, предотвращающими дополнительные издержки на перестройку некластеризованных индексов.  
  
## <a name="using-options-with-drop-index"></a>Использование параметров инструкции DROP INDEX  
 При удалении кластеризованного индекса можно установить следующие параметры: MAXDOP, ONLINE и MOVE TO.  
  
 Используйте параметр MOVE TO, чтобы удалить кластеризованный индекс и переместить результирующую таблицу в другую файловую группу или схему секционирования в одной транзакции.  
  
 При присвоении параметру ONLINE значения ON запросы и изменения базовых данных и связанных некластеризованных индексов не блокируются во время выполнения транзакции DROP INDEX. В режиме в сети одновременно может удаляться только один кластеризованный индекс. Полное описание параметра ONLINE см. в разделе [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
 Кластеризованный индекс нельзя удалить в режиме в сети, если индекс недоступен в представлении или содержит столбцы типа **text**, **ntext**, **image**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** или **xml** в строках данных конечного уровня.  
  
 Использование параметров ONLINE = ON и MOVE TO требует дополнительного временного места на диске.  
  
 После удаления индекса результирующая куча появляется в представлении каталога **sys.indexes** со значением NULL в столбце **name**. Для просмотра имени таблицы выполните соединение **sys.indexes** с **sys.tables** по **object_id**. Пример запроса см. в примере Г.  
  
 На многопроцессорных компьютерах под управлением [!INCLUDE[ssEnterpriseEd2005](../../includes/ssenterpriseed2005-md.md)] или выше инструкция DROP INDEX может использовать больше процессоров для операций просмотра и сортировки, связанных с удалением кластеризованного индекса, как и в случаях с другими инструкциями. Можно вручную настроить число процессоров, применяемых для запуска инструкции DROP INDEX, указав параметр индекса MAXDOP. Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
 При удалении кластеризованного индекса соответствующие секции кучи сохраняют настройки сжатия данных, если только не была изменена схема секционирования. Если схема секционирования подверглась изменениям, все секции перестраиваются в распакованное состояние (DATA_COMPRESSION = NONE). Чтобы удалить кластеризованный индекс и изменить схему секционирования, необходимо выполнить следующие шаги.  
  
1.  Удалить кластеризованный индекс.  
  
2.  Изменить таблицу с помощью инструкции ALTER TABLE ... и параметра REBUILD ..., определяющего степень сжатия.  
  
При удалении кластеризованного индекса в режиме не в сети удаляются только верхние уровни кластеризованных индексов, следовательно, операция выполняется довольно быстро. При удалении кластеризованного индекса в режиме ONLINE [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] перестраивает кучу два раза: один для первого шага, один для второго. Дополнительную информацию о сжатии данных см. в разделе [Сжатие данных](../../relational-databases/data-compression/data-compression.md).  
  
## <a name="xml-indexes"></a>XML-индексы  
 При удалении XML-индекса нельзя указывать параметры. Кроме того, нельзя использовать синтаксис _table\_or\_view\_name_ **.** _index\_name_. При удалении первичного XML-индекса все связанные вторичные XML-индексы удаляются автоматически. Дополнительные сведения см в разделе [XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
## <a name="spatial-indexes"></a>Пространственные индексы  
 Пространственные индексы поддерживаются только для таблиц. При удалении пространственного индекса нельзя указывать любые параметры или использовать **.** _index\_name_. Правильный синтаксис:  
  
 DROP INDEX *spatial_index_name* ON *spatial_table_name*;  
  
 Дополнительные сведения о пространственных индексах см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения инструкции DROP INDEX как минимум требуется разрешение ALTER для таблицы или представления. По умолчанию это разрешение предоставляется предопределенной роли сервера **sysadmin** и предопределенным ролям базы данных **db_ddladmin** и **db_owner** .  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-dropping-an-index"></a>A. Удаление индекса  
 В следующем примере показано, как удалить индекс `IX_ProductVendor_VendorID` в таблице `ProductVendor` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX IX_ProductVendor_BusinessEntityID   
    ON Purchasing.ProductVendor;  
GO  
```  
  
### <a name="b-dropping-multiple-indexes"></a>Б. Удаление нескольких индексов  
 В следующем примере показано, как удалить два индекса в одной транзакции в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX  
    IX_PurchaseOrderHeader_EmployeeID ON Purchasing.PurchaseOrderHeader,  
    IX_Address_StateProvinceID ON Person.Address;  
GO  
```  
  
### <a name="c-dropping-a-clustered-index-online-and-setting-the-maxdop-option"></a>В. Удаление кластеризованного индекса в режиме в сети и установка параметра MAXDOP  
 В следующем примере удаляется кластеризованный индекс с параметром `ONLINE`, установленным в значение `ON` и параметром `MAXDOP`, установленным в значение `8`. Поскольку параметр MOVE TO не был указан, результирующая таблица сохраняется в той же файловой группе, что и индекс. В этих примерах используется база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]  
  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
```sql  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials WITH (ONLINE = ON, MAXDOP = 2);  
GO  
```  
  
### <a name="d-dropping-a-clustered-index-online-and-moving-the-table-to-a-new-filegroup"></a>Г. Удаление кластеризованного индекса в режиме в сети и перемещение таблицы в другую файловую группу  
 В следующем примере кластеризованный индекс удаляется в режиме в сети и результирующая таблица (куча) перемещается в файловую группу `NewGroup` с использованием предложения `MOVE TO` . Представления каталога `sys.indexes`, `sys.tables`и `sys.filegroups` запрашиваются для проверки размещения индекса и таблицы в файловых группах до и после перемещения. (Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] можно использовать синтаксис DROP INDEX IF EXISTS.)  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
```sql  
--Create a clustered index on the PRIMARY filegroup if the index does not exist.  
CREATE UNIQUE CLUSTERED INDEX  
    AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
        ON Production.BillOfMaterials (ProductAssemblyID, ComponentID,   
        StartDate)  
    ON 'PRIMARY';  
GO  
-- Verify filegroup location of the clustered index.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U')  
GO  
--Create filegroup NewGroup if it does not exist.  
IF NOT EXISTS (SELECT name FROM sys.filegroups  
                WHERE name = N'NewGroup')  
    BEGIN  
    ALTER DATABASE AdventureWorks2012  
        ADD FILEGROUP NewGroup;  
    ALTER DATABASE AdventureWorks2012  
        ADD FILE (NAME = File1,  
            FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\File1.ndf')  
        TO FILEGROUP NewGroup;  
    END  
GO  
--Verify new filegroup  
SELECT * from sys.filegroups;  
GO  
-- Drop the clustered index and move the BillOfMaterials table to  
-- the Newgroup filegroup.  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials   
    WITH (ONLINE = ON, MOVE TO NewGroup);  
GO  
-- Verify filegroup location of the moved table.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U');  
GO  
```  
  
### <a name="e-dropping-a-primary-key-constraint-online"></a>Д. Удаление ограничения PRIMARY KEY в режиме в сети  
 Индексы, созданные в результате создания ограничений параметров PRIMARY KEY или UNIQUE, нельзя удалить с помощью инструкции DROP INDEX. Они удаляются с помощью инструкции ALTER TABLE DROP CONSTRAINT. Дополнительные сведения см. в разделе [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 Следующий пример иллюстрирует удаление кластеризованного индекса с ограничением PRIMARY KEY путем удаления ограничения. У таблицы `ProductCostHistory` нет ограничений FOREIGN KEY. Если бы они были, необходимо было бы сначала удалить их.  
  
```sql  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
ALTER TABLE Production.TransactionHistoryArchive  
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID  
WITH (ONLINE = ON);  
```  
  
### <a name="f-dropping-an-xml-index"></a>Е. Удаление XML-индекса  
 В следующем примере показано, как удалить XML-индекс в таблице `ProductModel` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX PXML_ProductModel_CatalogDescription   
    ON Production.ProductModel;  
```  
  
### <a name="g-dropping-a-clustered-index-on-a-filestream-table"></a>Ж. Удаление кластеризованного индекса для таблицы FILESTREAM  
 В следующем примере кластеризованный индекс удаляется в режиме в сети и результирующая таблица (куча) вместе с данными FILESTREAM перемещается в схему секционирования `MyPartitionScheme` с использованием предложений `MOVE TO` и `FILESTREAM ON`.  
  
**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.  
  
```sql  
DROP INDEX PK_MyClusteredIndex   
    ON dbo.MyTable   
    WITH (MOVE TO MyPartitionScheme,  
          FILESTREAM_ON MyPartitionScheme);  
GO  
```  

  
## <a name="see-also"></a>См. также:  
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [ALTER PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE XML INDEX (Transact-SQL)](../../t-sql/statements/create-xml-index-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.filegroups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)  
  
  


