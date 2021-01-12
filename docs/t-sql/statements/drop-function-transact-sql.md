---
description: DROP FUNCTION (Transact-SQL)
title: DROP FUNCTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/11/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_FUNCTION_TSQL
- DROP FUNCTION
dev_langs:
- TSQL
helpviewer_keywords:
- user-defined functions [SQL Server], removing
- removing user-defined functions
- DROP FUNCTION statement
- dropping user-defined functions
- deleting user-defined functions
ms.assetid: ee5ad283-9e44-4109-902f-0ce12669ee11
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 655cf93adfd1fbc84d6ab6333fb42e88a66826dd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096966"
---
# <a name="drop-function-transact-sql"></a>DROP FUNCTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет из текущей базы данных одну или несколько пользовательских функций. Определяемые пользователем функции создаются с помощью инструкции [CREATE FUNCTION](../../t-sql/statements/create-function-transact-sql.md) и изменяются с помощью инструкции [ALTER FUNCTION](../../t-sql/statements/alter-function-transact-sql.md).  
  
 Функция DROP поддерживает скомпилированные в собственном коде скалярные определяемые пользователем функции. Дополнительные сведения см. в разделе [Скалярные определяемые пользователем функции для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
 -- SQL Server, Azure SQL Database 

DROP FUNCTION [ IF EXISTS ] { [ schema_name. ] function_name } [ ,...n ]   
[;]
```

```syntaxsql
 -- Azure Synapse Analytics, Parallel Data Warehouse 

DROP FUNCTION [IF EXISTS] [ schema_name. ] function_name
[;] 
```  
   
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*    
 Условно удаляет функцию только в том случае, если она уже существует. Доступно в [!INCLUDE[ssnoversion_md](../../includes/ssnoversion-md.md)] 2016 и в [!INCLUDE[sssds_md](../../includes/sssds-md.md)].
  
 *schema_name*  
 Имя схемы, к которой принадлежит определяемая пользователем функция.  
  
 *function_name*  
 Имя удаляемой пользовательской функции или функций. Указание имени схемы является необязательным. Невозможно указать имя сервера и имя базы данных.  
  
## <a name="remarks"></a>Remarks  
 При выполнении инструкции DROP FUNCTION произойдет сбой, если в базе данных существуют функции или представления языка [!INCLUDE[tsql](../../includes/tsql-md.md)], созданные с ключевым словом SCHEMABINDING и содержащие ссылки на удаляемую функцию, или если база данных содержит вычисляемые столбцы, ограничения CHECK или DEFAULT, которые ссылаются на эту функцию.  
  
 При выполнении инструкции DROP FUNCTION произойдет сбой, если имеются вычисляемые столбцы, которые содержат ссылки на удаляемую функцию и которые были проиндексированы.  
  
## <a name="permissions"></a>Разрешения  
 Для вызова инструкции DROP FUNCTION у пользователя должно быть как минимум разрешение ALTER на схему, которой принадлежит функция, или разрешение CONTROL на функцию.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-dropping-a-function"></a>A. Удаление функции  
 В следующем примере определяемая пользователем функция `fn_SalesByStore` удаляется из схемы `Sales` в образце базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Создание этой функции см. в примере Б в разделе [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md).  
  
```sql  
DROP FUNCTION Sales.fn_SalesByStore;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER FUNCTION (Transact-SQL)](../../t-sql/statements/alter-function-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [OBJECT_ID (Transact-SQL)](../../t-sql/functions/object-id-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)  
  
  
