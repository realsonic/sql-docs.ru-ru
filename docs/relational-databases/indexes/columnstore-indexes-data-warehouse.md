---
description: Хранилище данных для индексов columnstore
title: Хранилище данных для индексов columnstore | Документы Майкрософт
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 21fd153b-116d-47fc-a926-f1528299a391
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0ca8263c5d75fca3bc59164d8d3ff7acaa9c5f2e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172736"
---
# <a name="columnstore-indexes---data-warehouse"></a>Хранилище данных для индексов columnstore
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Индексы columnstore в сочетании с секционированием являются необходимым элементом для создания хранилища данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="whats-new"></a>Новые возможности  
 В[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] появились следующие функции для повышения производительности индексов columnstore:  
  
-   AlwaysOn поддерживает запросы к индексу columnstore в доступной для чтения вторичной реплике.  
-   Режим MARS поддерживает индексы columnstore.  
-   Новое динамическое административное представление [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md) предоставляет информацию об устранении неполадок производительности на уровне группы строк.  
-   Однопотоковые запросы по индексам columnstore могут выполняться в пакетном режиме. Ранее в пакетном режиме могли выполняться только многопотоковые запросы.  
-   Оператор `SORT` выполняется в пакетном режиме.  
-   Множественная операция `DISTINCT` выполняется в пакетном режиме.  
-   Статистические операции с окнами теперь выполняются в пакетном режиме для уровня совместимости базы данных 130 или более высокого.  
-   Включение статических вычислений для эффективной обработки статистических выражений. Работает с любым уровнем совместимости базы данных.  
-   Включение предиката строки для эффективной обработки предикатов строк. Работает с любым уровнем совместимости базы данных.  
-   Изоляция моментального снимка для уровня совместимости базы данных 130 или более высокого.  
  
## <a name="improve-performance-by-combining-nonclustered-and-columnstore-indexes"></a>Повышение производительности благодаря объединению некластеризованных индексов и индексов columnstore  
 Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] можно определить некластеризованные индексы в кластеризованном индексе columnstore.   
  
### <a name="example-improve-efficiency-of-table-seeks-with-a-nonclustered-index"></a>Пример. Повышение эффективности операций поиска в таблицах с помощью некластеризованного индекса  
 Для повышения эффективности операций поиска в таблицах хранилища данных можно создать некластеризованный индекс, предназначенный для запуска запросов, которые показывают максимальную производительность с операциями поиска в таблицах. Например, запросы, которые ищут совпадающие значения или возвращают небольшой диапазон значений, будут эффективнее выполняться с индексом сбалансированного дерева, а не с индексом columnstore. Они не требуют сканирования всей таблицы через индекс columnstore и вернут правильный результат быстрее, выполнив двоичный поиск по индексу сбалансированного дерева.  
  
```sql  
--BASIC EXAMPLE: Create a nonclustered index on a columnstore table.  
  
--Create the table  
CREATE TABLE t_account (  
    AccountKey int NOT NULL,  
    AccountDescription nvarchar (50),  
    AccountType nvarchar(50),  
    UnitSold int  
);  
GO  
  
--Store the table as a columnstore.  
CREATE CLUSTERED COLUMNSTORE INDEX taccount_cci ON t_account;  
GO  
  
--Add a nonclustered index.  
CREATE UNIQUE INDEX taccount_nc1 ON t_account (AccountKey);  
```  
  
### <a name="example-use-a-nonclustered-index-to-enforce-a-primary-key-constraint-on-a-columnstore-table"></a>Пример. Использование некластеризованного индекса для принудительного применения ограничения первичного ключа в таблице columnstore  
 По умолчанию таблица columnstore не позволяет установить ограничение кластеризованного первичного ключа. Теперь с помощью некластеризованного индекса для таблицы columnstore можно принудительно применить ограничение первичного ключа. Первичный ключ равнозначен ограничению UNIQUE в столбце, отличном от NULL, а [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] реализует ограничение UNIQUE как некластеризованный индекс. Эти факты, объединенные в следующем примере, определяют ограничение UNIQUE для столбца accountkey, отличного от NULL. Результат представляет собой некластеризованный индекс, принудительно применяющий ограничение первичного ключа в виде ограничения UNIQUE для столбца, отличного от NULL.  
  
 Далее таблица преобразуется в кластеризованный индекс columnstore. Во время преобразования некластеризованный индекс сохраняется. Результат представляет собой кластеризованный индекс columnstore с некластеризованным индексом, принудительно применяющим ограничение первичного ключа. Так как любое обновление или вставка в таблице columnstore также повлияет на некластеризованный индекс, все операции, которые нарушают ограничение UNIQUE и отличное от NULL значение, вызовут сбой всей операции.  
  
 Результат представляет собой индекс columnstore с некластеризованным индексом, принудительно применяющим ограничение первичного ключа для обоих индексов.  
  
```sql
--EXAMPLE: Enforce a primary key constraint on a columnstore table.   
  
--Create a rowstore table with a unique constraint.  
--The unique constraint is implemented as a nonclustered index.  
CREATE TABLE t_account (  
    AccountKey int NOT NULL,  
    AccountDescription nvarchar (50),  
    AccountType nvarchar(50),  
    UnitSold int,  
  
    CONSTRAINT uniq_account UNIQUE (AccountKey)  
);  
  
--Store the table as a columnstore.   
--The unique constraint is preserved as a nonclustered index on the columnstore table.  
CREATE CLUSTERED COLUMNSTORE INDEX t_account_cci ON t_account  
  
--By using the previous two steps, every row in the table meets the UNIQUE constraint  
--on a non-NULL column.  
--This has the same end-result as having a primary key constraint  
--All updates and inserts must meet the unique constraint on the nonclustered index or they will fail.  
  
--If desired, add a foreign key constraint on AccountKey.  
  
ALTER TABLE [dbo].[t_account]  
WITH CHECK ADD FOREIGN KEY([AccountKey]) REFERENCES my_dimension(Accountkey); 
```  
  
### <a name="improve-performance-by-enabling-row-level-and-row-group-level-locking"></a>Повышение производительности за счет включения блокировки на уровне строки и на уровне группы строк  
 В дополнение к функции некластеризованного индекса для индекса columnstore [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] предоставляет возможность детализированной блокировки для операций выбора, обновления и удаления. Запросы могут выполняться с блокировкой на уровне строки для индексных операций поиска по некластеризованному индексу и блокировкой на уровне группы строк для полного сканирования таблиц по индексу columnstore. Это позволяет повысить параллелизм чтения и записи при надлежащем использовании блокировки на уровне строки и на уровне группы строк.  
  
```sql  
--Granular locking example  
--Store table t_account as a columnstore table.  
CREATE CLUSTERED COLUMNSTORE INDEX taccount_cci ON t_account  
GO  
  
--Add a nonclustered index for use with this example  
CREATE UNIQUE INDEX taccount_nc1 ON t_account (AccountKey);  
GO  
  
--Look at locking with access through the nonclustered index  
SET TRANSACTION ISOLATION LEVEL repeatable read;  
GO  
  
BEGIN TRAN  
    -- The query plan chooses a seek operation on the nonclustered index  
    -- and takes the row lock  
    SELECT * FROM t_account WHERE AccountKey = 100;  
END TRAN  
```  
  
### <a name="snapshot-isolation-and-read-committed-snapshot-isolations"></a>Изоляция моментальных снимков и изоляция моментальных снимков с чтением фиксированных данных  
 Изоляция моментальных снимков (SI) позволяет гарантировать согласованность транзакций, а изоляция моментальных снимков с чтением фиксированных данных (RCSI) — согласованность на уровне инструкций для запросов к индексам columnstore. Это позволяет запросам выполняться без блокировки модулей записи данных. Такое неблокирующее поведение также значительно снижает вероятность взаимоблокировок в сложных транзакциях. Дополнительные сведения см. в разделе [Изоляция моментальных снимков в SQL Server](https://msdn.microsoft.com/library/tcbchxcb\(v=vs.110\).aspx) в библиотеке MSDN.  
  
## <a name="see-also"></a>См. также:  
 [Руководство по проектированию индексов columnstore](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Индексы columnstore. Руководство по загрузке данных](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Производительность запросов индексов columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Начало работы с Columnstore для получения операционной аналитики в реальном времени](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)    
 [Архитектура индексов columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index) 
  
