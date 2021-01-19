---
description: DROP TABLE (Transact-SQL)
title: DROP TABLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_TABLE_TSQL
- DROP TABLE
dev_langs:
- TSQL
helpviewer_keywords:
- removing indexes
- table removal [SQL Server]
- deleting indexes
- dropping data
- deleting tables
- dropping indexes
- removing constraints
- removing permissions
- DROP TABLE statement
- removing tables
- deleting triggers
- removing data
- dropping tables
- deleting permissions
- dropping triggers
- deleting constraints
- removing triggers
- deleting data
- dropping constraints
- dropping permissions
ms.assetid: 0b6f2b6f-3aa3-4767-943f-43df3c3c5cfd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7096cc309a826d88ba160882fe3b4da2bff87534
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171506"
---
# <a name="drop-table-transact-sql"></a>DROP TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет одно или больше определений таблиц и все данные, индексы, триггеры, ограничения и разрешения для этих таблиц. Любое представление или хранимая процедура, ссылающаяся на удаленную таблицу, должна быть явно удалена с помощью инструкции [DROP VIEW](../../t-sql/statements/drop-view-transact-sql.md) или [DROP PROCEDURE](../../t-sql/statements/drop-procedure-transact-sql.md). Отчет о зависимостях в таблице можно получить из представления [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP TABLE [ IF EXISTS ] { database_name.schema_name.table_name | schema_name.table_name | table_name } [ ,...n ]  
[ ; ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *database_name*  
 Имя базы данных, в которой создана таблица.  
  
 База данных SQL Azure поддерживает формат трехкомпонентного имени имя_базы_данных.[имя_схемы].имя_объекта, если имя_базы_данных — это текущая база данных или tempdb, а имя_объекта начинается с символа #. База данных SQL Azure не поддерживает четырехкомпонентные имена.  
  
 *IF EXISTS*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Условное удаление таблицы только в том случае, если она уже существует.  
  
 *schema_name*  
 Имя схемы, которой принадлежит таблица.  
  
 *table_name*  
 Имя таблицы, предназначенной для удаления.  
  
## <a name="remarks"></a>Комментарии  
 Инструкцию DROP TABLE нельзя использовать для удаления таблицы, на которую ссылается ограничение FOREIGN KEY. Сначала следует удалить ссылающееся ограничение FOREIGN KEY или ссылающуюся таблицу. Если и ссылающаяся таблица, и таблица, содержащая первичный ключ, удаляются с помощью одной инструкции DROP TABLE, ссылающаяся таблица должна быть первой в списке.  
  
 Несколько таблиц можно удалить из любой базы данных. Если удаляемая таблица ссылается на первичный ключ другой таблицы, которая также удаляется, ссылающаяся таблица с внешним ключом должна стоять в списке перед таблицей, содержащей указанный первичный ключ.  
  
 При удалении таблицы относящиеся к ней правила и значения по умолчанию теряют привязку, а любые связанные с таблицей ограничения или триггеры автоматически удаляются. Если таблица будет создана заново, нужно будет заново привязать все правила и значения по умолчанию, заново создать триггеры и добавить необходимые ограничения.  
  
 При удалении всех строк в таблице с помощью инструкции DELETE *tablename* или TRUNCATE TABLE таблица продолжает существовать, пока она не будет удалена.  
  
 Большие таблицы и индексы, использующие более 128 экстентов, удаляются в два этапа: логически и физически. На логическом этапе существующие единицы распределения, используемые в таблице, отмечаются для освобождения и остаются заблокированными до фиксации транзакции. В физической фазе страницы IAM, помеченные для освобождения, физически удаляются пакетами.  
  
 При удалении таблицы, которая содержит столбец VARBINARY(MAX) с атрибутом FILESTREAM, не будут удалены никакие данные, которые хранятся в файловой системе.  
  
> [!IMPORTANT]  
>  Инструкции DROP TABLE и CREATE TABLE нельзя выполнять для одной таблицы в одном пакете. В противном случае может произойти непредвиденная ошибка.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER на схему, к которой принадлежит эта таблица, разрешение CONTROL для этой таблицы или членство в предопределенной роли базы данных **db_ddladmin** .  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-dropping-a-table-in-the-current-database"></a>A. Удаление таблицы из текущей базы данных  
 Следующий пример удаляет таблицу `ProductVendor1`, ее данные и индексы из текущей базы данных.  
  
```sql  
DROP TABLE ProductVendor1 ;  
```  
  
### <a name="b-dropping-a-table-in-another-database"></a>Б. Удаление таблицы из другой базы данных  
 Следующий пример удаляет таблицу `SalesPerson2` из базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Пример может быть выполнен из любой базы данных на экземпляре сервера.  
  
```sql  
DROP TABLE AdventureWorks2012.dbo.SalesPerson2 ;  
```  
  
### <a name="c-dropping-a-temporary-table"></a>В. Удаление временной таблицы  
 Следующий пример создает временную таблицу, проверяет ее наличие, удаляет ее и снова проверяет ее наличие. В этом примере не используется синтаксис **IF EXISTS**, доступный с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)].  
  
```sql  
CREATE TABLE #temptable (col1 INT);  
GO  
INSERT INTO #temptable  
VALUES (10);  
GO  
SELECT * FROM #temptable;  
GO  
IF OBJECT_ID(N'tempdb..#temptable', N'U') IS NOT NULL   
DROP TABLE #temptable;  
GO  
--Test the drop.  
SELECT * FROM #temptable;  
```  
  
### <a name="d-dropping-a-table-using-if-exists"></a>Г. Удаление таблицы с помощью IF EXISTS  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 В следующем примере создается таблица с именем T1. Затем вторая инструкция удаляет таблицу. Третья инструкция не выполняет никаких действий, так как таблица уже удалена, однако это не вызывает ошибку.  
  
```sql  
CREATE TABLE T1 (Col1 INT);  
GO  
DROP TABLE IF EXISTS T1;  
GO  
DROP TABLE IF EXISTS T1;  
```  
  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [TRUNCATE TABLE (Transact-SQL)](../../t-sql/statements/truncate-table-transact-sql.md)   
 [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md)   
 [DROP PROCEDURE (Transact-SQL)](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
