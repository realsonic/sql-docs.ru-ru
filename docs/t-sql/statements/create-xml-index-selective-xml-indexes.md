---
description: CREATE XML INDEX (селективные XML-индексы)
title: CREATE XML INDEX (селективные XML-индексы) | Документы Майкрософт
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 1f510151-41d5-45c2-9cd0-b1ca0246fffe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f6f0033a1fb5944fa61460fa3945b0ebdaf0bce3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099476"
---
# <a name="create-xml-index-selective-xml-indexes"></a>CREATE XML INDEX (селективные XML-индексы)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Создает новый вторичный селективный XML-индекс по одному пути, который уже проиндексирован существующим селективным XML-индексом. Кроме того, вы можете создавать первичные селективные XML-индексы. Дополнительные сведения см. в разделе [Создание, изменение и удаление селективных XML-индексов](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE XML INDEX index_name  
    ON <table_object> ( xml_column_name )  
    USING XML INDEX sxi_index_name  
    FOR ( <xquery_or_sql_values_path> )  
    [WITH ( <index_options> )]  
  
<table_object> ::=   
{ database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<xquery_or_sql_values_path>::=   
<path_name>   
  
<path_name> ::=   
character string literal  
  
<xmlnamespace_list> ::=   
<xmlnamespace_item> [, <xmlnamespace_list>]  
  
<xmlnamespace_item> ::=   
xmlnamespace_uri AS xmlnamespace_prefix  
  
<index_options> ::=   
(    
  | PAD_INDEX  = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 *index_name*  
 Имя создаваемого нового индекса. Имена индексов должны быть уникальными в пределах таблицы, но не обязательно должны быть уникальными в пределах базы данных. Имена индексов должны удовлетворять правилам для [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
  
 ON *\<table_object>* . Таблица, которая содержит индексируемый XML-столбец. Вы можете использовать следующие форматы.  
  
-   `database_name.schema_name.table_name`  
  
-   `database_name..table_name`  
  
-   `schema_name.table_name`  
  
 *xml_column_name*  
 Имя таблицы, которая содержит XML-столбец, содержащий индексируемый путь.  
  
 USING XML INDEX *sxi_index_name*  
 Имя существующего селективного XML-индекса.  
  
 FOR **(** \<xquery_or_sql_values_path> **)** . Имя индексируемого пути, по которому создается вторичный селективный XML-индекс. Путь для индексирования является присвоенным именем из инструкции CREATE SELECTIVE XML INDEX. Дополнительные сведения см. в разделе [CREATE SELECTIVE XML INDEX (Transact-SQL)](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
 WITH \<index_options>. Сведения о параметрах индекса см. в статье [CREATE XML INDEX](../../t-sql/statements/create-xml-index-selective-xml-indexes.md).  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
 Может быть несколько вторичных селективных XML-индексов в каждом XML-столбце базовой таблицы.  
  
## <a name="limitations-and-restrictions"></a>Ограничения  
 Для создания селективных XML-индексов для столбца необходимо, чтобы селективный XML-индекс для XML-столбца был уже создан.  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER для таблицы или представления. Пользователь должен быть членом предопределенной роли сервера **sysadmin** или предопределенных ролей базы данных **db_ddladmin** и **db_owner**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается вторичный селективный XML-индекс с путем `pathabc`. Путь для индексирования является присвоенным именем из инструкции [CREATE SELECTIVE XML INDEX (Transact-SQL)](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
```sql  
CREATE XML INDEX filt_sxi_index_c  
ON Tbl(xmlcol)  
USING XML INDEX sxi_index  
FOR ( pathabc );  
```  
  
## <a name="see-also"></a>См. также:  
 [Выборочный XML-индекс (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Создание, изменение и удаление вторичных селективных XML-индексов](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md)  
  
  

