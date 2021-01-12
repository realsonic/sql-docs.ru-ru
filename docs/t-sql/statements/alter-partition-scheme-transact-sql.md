---
description: ALTER PARTITION SCHEME (Transact-SQL)
title: ALTER PARTITION SCHEME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER PARTITION SCHEME
- ALTER_PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PARTITION SCHEME statement
- partition schemes [SQL Server], modifying
- modifying partition schemes
- adding filegroups
- NEXT USED filegroups
ms.assetid: f01d6880-9800-4cfb-8d11-d4be21efc8ca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 084263fab973e11c1c0a14e340f18ae952ca5f44
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094755"
---
# <a name="alter-partition-scheme-transact-sql"></a>ALTER PARTITION SCHEME (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Добавляет файловую группу в схему секционирования или изменяет обозначение файловой группы NEXT USED для данной схемы секционирования. 

>[!NOTE]
>В базе данных SQL Azure поддерживаются только первичные файловые группы.  
  
 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER PARTITION SCHEME partition_scheme_name   
NEXT USED [ filegroup_name ] [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *partition_scheme_name*  
 Имя изменяемой схемы секционирования.  
  
 *filegroup_name*  
 Указывает файловую группу, которую требуется пометить для схемы секционирования как NEXT USED. Это означает, что файловая группа примет новое секционирование, созданное с помощью инструкции [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
 В схеме секционирования только одна файловая группа может быть отмечена как NEXT USED. Можно указать пустую файловую группу. Если указан аргумент *filegroup_name* и ни одна файловая группа не является в данный момент NEXT USED, то группа *filegroup_name* помечается как NEXT USED. Если указан аргумент *filegroup_name* и файловая группа с пометкой NEXT USED уже существует, то свойство NEXT USED переносится от текущей файловой группы к группе *filegroup_name*.  
  
 Если аргумент *filegroup_name* не указан и файловая группа с пометкой NEXT USED уже существует, эта файловая группа теряет состояние NEXT USED, чтобы в схеме секционирования *partition_scheme_name* не осталось файловой группы NEXT USED.  
  
 Если аргумент *filegroup_name* не указан и ни одна файловая группа не отмечена как NEXT USED, инструкция ALTER PARTITION SCHEME возвращает предупреждение.  
  
## <a name="remarks"></a>Комментарии  
 Все файловые группы, на которые действует инструкция ALTER PARTITION SCHEME, должны быть в режиме "в сети".  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения инструкции ALTER PARTITION SCHEME необходимы следующие разрешения.  
  
-   Разрешение ALTER ANY DATASPACE. Это разрешение назначено по умолчанию членам предопределенной роли сервера **sysadmin** и предопределенных ролей базы данных **db_owner** и **db_ddladmin** .  
  
-   Разрешение CONTROL или ALTER на базу данных, в которой была создана схема секционирования.  
  
-   Разрешения CONTROL SERVER или ALTER ANY DATABASE на сервер базы данных, в которой была создана схема секционирования.  
  
## <a name="examples"></a>Примеры  
 В следующем примере предполагается, что в базе данных существуют схема секционирования `MyRangePS1` и файловая группа `test5fg`.  
  
```sql  
ALTER PARTITION SCHEME MyRangePS1  
NEXT USED test5fg;  
```  
  
 Файловая группа `test5fg` получает любые дополнительные секции из секционированной таблицы или индекс как результат выполнения инструкции ALTER PARTITION FUNCTION.  
  
## <a name="see-also"></a>См. также  
 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [DROP PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/drop-partition-function-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_schemes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
