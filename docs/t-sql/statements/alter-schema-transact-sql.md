---
description: ALTER SCHEMA (Transact-SQL)
title: ALTER SCHEMA (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SCHEMA
- ALTER_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], transferring
- transferring objects between schemas
- ALTER SCHEMA statement
- schemas [SQL Server], modifying
- modifying schemas
ms.assetid: 0a760138-460e-410a-a3c1-d60af03bf2ed
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 73cd35c7f05d39d7ec6930be6273dc22c16f9c56
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092066"
---
# <a name="alter-schema-transact-sql"></a>ALTER SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Передача защищаемых сущностей между схемами.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER SCHEMA schema_name   
   TRANSFER [ <entity_type> :: ] securable_name   
[;]  
  
<entity_type> ::=  
    {  
    Object | Type | XML Schema Collection  
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER SCHEMA schema_name   
   TRANSFER [ OBJECT :: ] securable_name   
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *schema_name*  
 Имя схемы в текущей базе данных, в которую будет перемещена защищаемая сущность. Не может иметь значение SYS или INFORMATION_SCHEMA.  
  
 \<entity_type>  
 Класс сущности, для которой изменяется владелец. По умолчанию это объект.  
  
 *securable_name*  
 Одно- или двухкомпонентное имя содержащейся в схеме защищаемой сущности, которая должна быть перемещена в другую схему.  
  
## <a name="remarks"></a>Remarks  
 Пользователи и схемы полностью разделены.  
  
 Инструкция ALTER SCHEMA применяется только для перемещения защищаемых объектов между схемами в пределах одной базы данных. Для изменения или удаления защищаемой сущности в схеме используйте инструкцию ALTER или DROP, специфичную для этой сущности.  
  
 Если для аргумента *securable_name* используется однокомпонентное имя, для поиска защищаемой сущности будут использоваться текущие правила разрешения имен.  
  
 Все разрешения, связанные с защищаемой сущностью, при перемещении в другую схему будут удалены. Если владелец защищаемой сущности был явно указан, он не изменится. Если для владельца защищаемой схемы было установлено значение SCHEMA OWNER, то владельцем останется SCHEMA OWNER. Однако после перемещения SCHEMA OWNER будет относиться к владельцу новой схемы. Значение principal_id нового владельца будет равно NULL.  
  
 Перемещение хранимой процедуры, функции, представления или триггера не изменит имени схемы (при его наличии) соответствующего объекта в определении столбца представления каталога [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) или полученного с помощью встроенной функции [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md). Поэтому не рекомендуется использовать ALTER SCHEMA для переименования объектов этих типов. Лучше удалить и создать объект повторно в его новой схеме.  
  
 Перемещение такого объекта, как таблица или столбец, не приводит к автоматическому обновлению ссылок на этот объект. Необходимо вручную изменить любые объекты, которые ссылаются на перемещаемый объект. Например, если перемещается таблица и на эту таблицу имеется ссылка в триггере, то необходимо изменить триггер, указав новое имя схемы. Используйте [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md), чтобы составить список зависимостей для объекта перед его перемещением.  

 Чтобы изменить схему таблицы в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], щелкните правой кнопкой мыши таблицу в обозревателе объектов и выберите пункт **Конструктор**. Нажмите клавишу **F4**, чтобы открыть окно свойств. В поле **Схема** выберите новую схему.  
 
 ALTER SCHEMA использует блокировку на уровне схемы.
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Разрешения  
 Для передачи защищаемой сущности из другой схемы текущий пользователь должен иметь разрешения CONTROL на эту сущность (а не на схему) и разрешения ALTER на целевую схему.  
  
 Если защищаемая сущность имеет спецификацию EXECUTE AS OWNER, а ее владельцем является SCHEMA OWNER, у пользователя также должно быть разрешение IMPERSONATE на владельца целевой схемы.  
  
 Все разрешения, связанные с перемещаемой защищаемой сущностью, при перемещении удаляются.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-transferring-ownership-of-a-table"></a>A. Передача владения таблицей  
 В следующем примере схема `HumanResources` изменяется путем перемещения в схему "HumanResources" таблицы `Address` из схемы `Person`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER SCHEMA HumanResources TRANSFER Person.Address;  
GO  
```  
  
### <a name="b-transferring-ownership-of-a-type"></a>Б. Передача владения типом  
 В следующем примере создается тип в схеме `Production`, а затем этот тип передается схеме `Person`.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TYPE Production.TestType FROM [VARCHAR](10) NOT NULL ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
  
-- Change the type to the Person schema.  
ALTER SCHEMA Person TRANSFER type::Production.TestType ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-transferring-ownership-of-a-table"></a>В. Передача владения таблицей  
 В следующем примере создается таблица `Region` в схеме `dbo`, создается схема `Sales`, а затем таблица `Region` перемещается из схемы `dbo` в схему `Sales`.  
  
```sql  
CREATE TABLE dbo.Region   
    (Region_id INT NOT NULL,  
    Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
  
CREATE SCHEMA Sales;  
GO  
  
ALTER SCHEMA Sales TRANSFER OBJECT::dbo.Region;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE SCHEMA (Transact-SQL)](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

