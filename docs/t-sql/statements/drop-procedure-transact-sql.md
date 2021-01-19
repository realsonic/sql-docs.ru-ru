---
description: DROP PROCEDURE (Transact-SQL)
title: DROP PROCEDURE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP PROCEDURE
- DROP_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing stored procedures
- dropping procedure groups
- deleting stored procedures
- deleting procedure groups
- DROP PROCEDURE statement
- dropping stored procedures
- stored procedures [SQL Server], removing
- removing procedure groups
ms.assetid: 1c2d7235-7b9b-4336-8f17-429e7d82c2c3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c810d1e57b3e2cfb092e1091da0d95b082eb8c2d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172476"
---
# <a name="drop-procedure-transact-sql"></a>DROP PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет одну или несколько хранимых процедур или групп процедур из текущей базы данных в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP { PROC | PROCEDURE } [ IF EXISTS ] { [ schema_name. ] procedure } [ ,...n ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP { PROC | PROCEDURE } { [ schema_name. ] procedure_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Условное удаление процедуры только в том случае, если она уже существует.  
  
 *schema_name*  
 Имя схемы, которой принадлежит процедура. Имя сервера или базы данных задавать нельзя.  
  
 *procedure*  
 Имя удаляемой хранимой процедуры или группы хранимых процедур. Нельзя удалить отдельные процедуры из группы пронумерованных процедур. Группа процедур удаляется полностью.  
  
## <a name="best-practices"></a>Рекомендации  
 Перед удалением хранимой процедуры проверьте зависимые объекты и измените эти объекты соответствующим образом. Удаление хранимой процедуры может вызвать сбой зависимых процедур и скриптов, если эти объекты не обновлены. Дополнительные сведения см. в статье [Просмотр зависимостей хранимой процедуры](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
## <a name="metadata"></a>Метаданные  
 Для отображения списка существующих процедур запросите представление каталога **sys.objects**. Для отображения определения процедуры выполните запрос к представлению каталога **sys.sql_modules**.  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Необходимо разрешение **CONTROL** для процедуры, разрешение **ALTER** для схемы, которой принадлежит процедура, либо членство в предопределенной роли сервера **db_ddladmin**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере хранимая процедура `dbo.uspMyProc` удаляется из текущей базы данных.  
  
```sql  
DROP PROCEDURE dbo.uspMyProc;  
GO  
```  
  
 В следующем примере из текущей базы данных удаляются несколько хранимых процедур.  
  
```sql  
DROP PROCEDURE dbo.uspGetSalesbyMonth, dbo.uspUpdateSalesQuotes, dbo.uspGetSalesByYear;  
```  
  
 В приведенном ниже примере удаляется хранимая процедура `dbo.uspMyProc`, если она существует, но если она не существует, ошибка не возникает. Этот синтаксис является новым в [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)].  
  
```sql  
DROP PROCEDURE IF EXISTS dbo.uspMyProc;  
GO  
```  
  
  
## <a name="see-also"></a>См. также:  
 [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Удаление хранимой процедуры](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)  
  
  


