---
description: CREATE PARTITION SCHEME (Transact-SQL)
title: CREATE PARTITION SCHEME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE PARTITION SCHEME
- SCHEME
- PARTITION SCHEME
- CREATE_PARTITION_SCHEME_TSQL
- SCHEME_TSQL
- PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- partitioned indexes [SQL Server], schemes
- partitioned tables [SQL Server], schemes
- CREATE PARTITION SCHEME statement
- partition schemes [SQL Server], creating
- filegroups [SQL Server], partitions
- partitioned indexes [SQL Server], filegroups
- partitioned tables [SQL Server], filegroups
- mapping partitions [SQL Server]
ms.assetid: 5b21c53a-b4f4-4988-89a2-801f512126e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 36b22f01bbc9bd089cf6f651d8f97d2d2d2b3d32
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093484"
---
# <a name="create-partition-scheme-transact-sql"></a>CREATE PARTITION SCHEME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает схему в текущей базе данных, которая сопоставляет секции секционированной таблицы или индекса с файловыми группами. Количество и домен секций секционированной таблицы или индекса определяются в функции секционирования. Прежде чем создавать схему секционирования, сначала в инструкции [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md) следует создать функцию секционирования.  

>[!NOTE]
>В базе данных SQL Azure поддерживаются только первичные файловые группы.  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE PARTITION SCHEME partition_scheme_name  
AS PARTITION partition_function_name  
[ ALL ] TO ( { file_group_name | [ PRIMARY ] } [ ,...n ] )  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *partition_scheme_name*  
 Имя схемы секционирования. Имена схемы секционирования должны быть уникальными в пределах базы данных и соответствовать правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
 *partition_function_name*  
 Имя функции секционирования, использующей схему секционирования. Секции, созданные функцией секционирования, сопоставляются с файловыми группами, заданными в схеме секционирования. Аргумент *partition_function_name* уже должен существовать в базе данных. Одна секция не может одновременно содержать файловые группы файлового потока и другие файловые группы.  
  
 ALL  
 Указывает, что все секции сопоставляются с файловой группой, определяемой аргументом *file_group_name*, или с первичной файловой группой, если указывается **[** PRIMARY **]**. Если указывается ALL, то может быть указано только одно значение аргумента *file_group_name*.  
  
 *file_group_name* |  **[** PRIMARY **]** [ **,** _...n_]  
 Указывает имена файловых групп, содержащих секции, указываемые аргументом *partition_function_name*. Аргумент *file_group_name* уже должен существовать в базе данных.  
  
 Если указывается **[** PRIMARY **]**, секция сохраняется в первичной файловой группе. Если указывается ALL, то может быть указано только одно значение аргумента *file_group_name*. Секции назначаются файловым группам, начиная с секции 1, в том порядке, в котором файловые группы перечисляются в [ **,** _...n_]. Одно и то же имя *file_group_name* может быть указано в [ **,** _...n_] несколько раз. Если значение *n* недостаточно для количества секций, указываемого в аргументе *partition_function_name*, CREATE PARTITION SCHEME завершается с ошибкой.  
  
 Если аргумент *partition_function_name* формирует меньше секций, чем количество файловых групп, первая неназначенная файловая группа отмечается как NEXT USED и информационное сообщение выводит наименование файловой группы NEXT USED. Если указывается параметр ALL, единственный аргумент *file_group_name* сохраняет свое свойство NEXT USED для аргумента *partition_function_name*. Файловая группа NEXT USED получит дополнительную секцию, если такая секция будет создана инструкцией ALTER PARTITION FUNCTION. Чтобы создать дополнительные неназначенные файловые группы, которые должны содержать новые секции, используйте инструкцию ALTER PARTITION SCHEME.  
  
 Когда первичная файловая группа указывается в *file_group_name* [ 1 **,** _...n_], аргумент PRIMARY должен отделяться так же, как в **[** PRIMARY **]** , так как это ключевое слово.  
  
 Только PRIMARY поддерживается для [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]. См. пример Д далее. 
  
## <a name="permissions"></a>Разрешения  
 Для выполнения CREATE PARTITION SCHEME могут использоваться следующие разрешения.  
  
-   Разрешение ALTER ANY DATASPACE. Это разрешение назначено по умолчанию членам предопределенной роли сервера **sysadmin** и предопределенных ролей базы данных **db_owner** и **db_ddladmin** .  
  
-   Разрешения CONTROL или ALTER на базу данных, в которой создается схема секционирования.  
  
-   Разрешение CONTROL SERVER или ALTER ANY DATABASE на сервере базы данных, в которой создается схема секционирования.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-partition-scheme-that-maps-each-partition-to-a-different-filegroup"></a>A. Создание схемы секционирования, которая сопоставляет все секции с разными файловыми группами  
 В следующем примере создается функция секционирования для разделения таблицы или индекса на четыре секции. Затем создается схема секционирования, которая задает файловые группы, каждая из которых должна содержать одну из четырех секций. В примере предполагается, что в базе данных уже существуют файловые группы.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
TO (test1fg, test2fg, test3fg, test4fg);  
```  
  
 Секции таблицы, использующей функцию секционирования `myRangePF1` в столбце секционирования **col1**, будут назначены так, как показано в следующей таблице.  
  
||||||  
|-|-|-|-|-|  
|**Файловая группа**|`test1fg`|`test2fg`|`test3fg`|`test4fg`|  
|**Секция**|1|2|3|4|  
|**Значения**|**col1** <= `1`|**col1** > `1` AND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="b-creating-a-partition-scheme-that-maps-multiple-partitions-to-the-same-filegroup"></a>Б. Создание схемы секционирования, которая сопоставляет несколько секций с одной файловой группой  
 Если все секции сопоставляются с одной и той же файловой группой, используйте ключевое слово ALL. Но если сопоставление нескольких секций, но не всех, выполняется к одной файловой группе, то имя файловой группы должно быть повторено, как показано в следующем примере.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF2 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS2  
AS PARTITION myRangePF2  
TO ( test1fg, test1fg, test1fg, test2fg );  
```  
  
 Секции таблицы, использующей функцию секционирования `myRangePF2` в столбце секционирования **col1**, будут назначены так, как показано в следующей таблице.  
  
||||||  
|-|-|-|-|-|  
|**Файловая группа**|`test1fg`|`test1fg`|`test1fg`|`test2fg`|  
|**Секция**|1|2|3|4|  
|**Значения**|**col1** <= `1`|**col1** > 1 AND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="c-creating-a-partition-scheme-that-maps-all-partitions-to-the-same-filegroup"></a>В. Создание схемы секционирования, сопоставляющей все секции с одной файловой группой  
 В следующем примере создается такая же функция секционирования, что и в предыдущих примерах, и далее создается схема секционирования, которая сопоставляет все секции с одной и той же файловой группой.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF3 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS3  
AS PARTITION myRangePF3  
ALL TO ( test1fg );  
```  
  
### <a name="d-creating-a-partition-scheme-that-specifies-a-next-used-filegroup"></a>Г. Создание схемы секционирования, указывающей файловую группу "NEXT USED"  
 В следующем примере создается такая же функция секционирования, что и в предыдущих примерах, и далее создается схема секционирования, в которой указывается большее количество файловых групп, чем количество секций, созданных соответствующей функцией секционирования.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF4 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS4  
AS PARTITION myRangePF4  
TO (test1fg, test2fg, test3fg, test4fg, test5fg)  
```  
  
 При выполнении инструкции выводится следующее сообщение.  
  
Схема секционирования "myRangePS4" успешно создана. "test5fg" помечена как следующая используемая файловая группа в схеме секционирования "myRangePS4".  
  
  
 Если функция секционирования `myRangePF4` изменяется для добавления секции, файловая группа `test5fg` получает вновь созданную секцию.  

### <a name="e-creating-a-partition-scheme-only-on-primary"></a>Д. Создание схемы секционирования только в PRIMARY

 В [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Azure добавление файлов и файловых групп не поддерживается, а секционирование таблиц поддерживается путем секционирования только по файловой группе PRIMARY. 

В следующем примере создается функция секционирования для разделения таблицы или индекса на четыре секции. Затем создается схема секционирования, указывающая, что все секции создаются в файловой группе PRIMARY.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
ALL TO ( [PRIMARY] );  
```
   
## <a name="see-also"></a>См. также  
 [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Создание секционированных таблиц и индексов](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)   
 [sys.partition_schemes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
