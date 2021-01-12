---
description: sys.dm_db_xtp_table_memory_stats (Transact-SQL)
title: sys.dm_db_xtp_table_memory_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_table_memory_stats_TSQL
- dm_db_xtp_table_memory_stats
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats
ms.assetid: ad0efc06-3d9c-4861-9dfa-a7a87822d0c8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 12e1f0e140593526314ce12a38df96effadaafb7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099879"
---
# <a name="sysdm_db_xtp_table_memory_stats-transact-sql"></a>sys.dm_db_xtp_table_memory_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Возвращает статистику использования памяти для каждой таблицы [!INCLUDE[hek_2](../../includes/hek-2-md.md)] (пользовательской и системной) в текущей базе данных. Системные таблицы содержат отрицательные идентификаторы объектов и используются для сохранения сведений времени выполнения для компонента [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. В отличие от пользовательских объектов, системные таблицы являются внутренними и существуют только в памяти, поэтому недоступны для просмотра через представления каталога. Системные таблицы используются для хранения такой информации, как метаданные для всех файлов данных и разностных файлов в хранилище, запросы слияния, конечные точки для разностных файлов с фильтрацией строк, удаленные таблицы и соответствующие сведения для резервного копирования и восстановления. Если компонент [!INCLUDE[hek_2](../../includes/hek-2-md.md)] может содержать до 8,192 пар файлов данных и разностных файлов, то для больших баз данных в памяти может потребоваться несколько МБ памяти для системных таблиц.  
  
 Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Идентификатор объекта таблицы. NULL для системных таблиц In-Memory OLTP.|  
|memory_allocated_for_table_kb|**bigint**|Память, выделенная для этой таблицы.|  
|memory_used_by_table_kb|**bigint**|Объем памяти, используемый таблицей, включая версии строк.|  
|memory_allocated_for_indexes_kb|**bigint**|Память, выделенная для индексов в этой таблице.|  
|memory_used_by_indexes_kb|**bigint**|Объем памяти, используемый для индексов в этой таблице.|  
  
## <a name="permissions"></a>Разрешения  
 Если имеется разрешение VIEW DATABASE STATE в текущей базе данных, возвращаются все строки. В противном случае возвращается пустой набор строк.  
  
 Если разрешения VIEW DATABASE нет, возвращаются все столбцы для строк в таблицах, для которых включено разрешение SELECT.  
  
 Системные таблицы возвращаются только для пользователей с разрешением VIEW DATABASE STATE.  
  
## <a name="examples"></a>Примеры  
 Для получения объема выделенной памяти для таблиц и индексов из базы данных можно выполнить запрос к следующему динамическому административному представлению:  
  
```  
-- finding memory for objects  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
 Определение объема памяти для всех объектов в базе данных  
  
```  
SELECT SUM( memory_allocated_for_indexes_kb + memory_allocated_for_table_kb) AS  
 memoryallocated_objects_in_kb   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
## <a name="user-scenario"></a>Пользовательский сценарий  
 Сначала создайте следующие таблицы в базе данных с именем HkDb1.  
  
```  
-- set max server memory to 4 GB  
EXEC sp_configure 'max server memory (MB)', 4048  
go  
  
RECONFIGURE  
go  
  
-- create a resource pool for database with memory-optimized objects  
CREATE RESOURCE POOL PoolHkDb1 WITH (MAX_MEMORY_PERCENT = 50);  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
go  
  
--bind the pool to the database  
EXEC sp_xtp_bind_db_resource_pool 'HkDb1', 'PoolHkdb1'  
go  
  
-- take database offline/online to associate the pool  
use master  
go  
  
alter database HkDb1 set offline  
go  
alter database HkDb1 set online  
go  
  
USE HkDb1  
go  
  
CREATE TABLE dbo.t1 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t1_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t2 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t2_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t3 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t3_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 1000000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
-- load 150K rows  
declare @i int = 0  
while (@i <= 150000)  
begin  
       insert t1 values (@i, 'a', replicate ('b', 8000))  
       set @i += 1;  
end  
go  
```  
  
 При загрузке данных в таблицу можно увидеть пользовательские таблицы и объем используемого хранилища. Например, каждая строка таблицы может занимать приблизительно 8070 байт (выделяемый размер — 8 K (8192 байта)). Можно просмотреть индексы для таблицы и объем хранилища, используемого индексом. Например, 1 МБ — это 100 000 записей с округлением до следующей степени 2, (2**17) = 131 072 по 8 байт каждая. У таблицы может не быть индекса. В этом случае для нее отображается объем выделенной для индекса памяти. Другие строки могут представлять системные таблицы.  
  
```  
select convert(char(10), object_name(object_id)) as Name,*   
from sys.dm_db_xtp_table_memory_stats  
```  
  
 Здесь приводятся выходные данные из 2 частей.  
  
```  
Name       object_id   memory_allocated_for_table_kb memory_used_by_table_kb  
---------- ----------- ----------------------------- -----------------------  
t3         629577281   0                             0  
t1         565577053   1372928                       1202351  
t2         597577167   0                             0  
NULL       -6          0                             0  
NULL       -5          0                             0  
NULL       -4          0                             0  
NULL       -3          0                             0  
NULL       -2          192                           25  
  
memory_allocated_for_indexes_kb memory_used_by_indexes_kb  
------------------------------- -------------------------  
8192                            8192  
1024                            1024  
8192                            8192  
2                               2  
24                              24  
2                               2  
2                               2  
16                              16  
```  
  
 Выходные данные  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
       sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
```  
  
 следующие  
  
```  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1357                 1191  
```  
  
 Далее изучим выходные данные из пула ресурсов. Память, использованная из пула, составляет 1356 МБ.  
  
```  
select pool_id,convert(char(10), name) as Name, min_memory_percent, max_memory_percent,   
   max_memory_kb/1024 as max_memory_mb  
from sys.dm_resource_governor_resource_pools  
  
select used_memory_kb/1024 as used_memory_mb ,target_memory_kb/1024 as target_memory_mb  
from sys.dm_resource_governor_resource_pools  
```  
  
 Ниже приводятся выходные данные.  
  
```  
pool_id     Name       min_memory_percent max_memory_percent max_memory_mb  
----------- ---------- ------------------ ------------------ --------------------  
1           internal   0                  100                3845  
2           default    0                  100                3845  
259         PoolHkDb1  0                  100                3845  
  
used_memory_mb       target_memory_mb  
-------------------- --------------------  
125                  3845  
32                   3845  
1356                 3845  
```  
  
## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
