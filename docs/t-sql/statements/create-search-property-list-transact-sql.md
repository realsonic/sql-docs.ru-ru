---
description: CREATE SEARCH PROPERTY LIST (Transact-SQL)
title: CREATE SEARCH PROPERTY LIST (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SEARCH_PROPERTY_LIST_TSQL
- CREATE SEARCH
- CREATE_SEARCH_TSQL
- CREATE_SEARCH_PROPERTY_TSQL
- CREATE SEARCH PROPERTY
- CREATE SEARCH PROPERTY LIST
- sql13.swb.spl.newsearchpropertylist.f1
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], creating
- CREATE SEARCH PROPERTY LIST statement
ms.assetid: 5440cbb8-3403-4d27-a2f9-8e1f5a1bc12b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: afd220a9893f77c79d5677af135c9a640611abf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092003"
---
# <a name="create-search-property-list-transact-sql"></a>CREATE SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Создает новый список свойств поиска. В списке свойств поиска указывается одно или несколько свойств поиска, которые необходимо включить в полнотекстовый индекс.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
CREATE SEARCH PROPERTY LIST new_list_name  
   [ FROM [ database_name. ] source_list_name ]  
   [ AUTHORIZATION owner_name ]  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *new_list_name*  
 Имя нового списка свойств поиска. Аргумент *new_list_name* — это идентификатор с максимальной длиной в 128 символов. Аргумент *new_list_name* должен быть уникальным в списках свойств текущей базы данных и соответствовать правилам для идентификаторов. Аргумент *new_list_name* будет использоваться при создании полнотекстового индекса.  
  
 *database_name*  
 Имя базы данных, в которой находится список свойств, указанный параметром *source_list_name*. Если не указано, в качестве *database_name* по умолчанию выбирается текущая база данных.  
  
 Параметр *database_name* должен указывать имя существующей базы данных. Имя входа для текущего соединения должно быть связано с идентификатором пользователя, существующим в базе данных, которая указана аргументом *database_name*. Кроме того, требуются необходимые [разрешения](#Permissions) для базы данных.  
  
 *source_list_name*  
 Указывает, что новый список свойств создается путем копирования существующего списка свойств из *database_name*. Если *source_list_name* не существует, то инструкция CREATE SEARCH PROPERTY LIST завершится ошибкой. Свойства поиска в *source_list_name* наследуются *new_list_name*.  
  
 AUTHORIZATION *owner_name*  
 Указывает имя пользователя или роли-владельца списка свойств. Аргумент *owner_name* должен быть именем роли, членом которой является текущий пользователь, или текущий пользователь должен иметь разрешение IMPERSONATE для *owner_name*. Если атрибут не указан, владельцем становится текущий пользователь.  
  
> [!NOTE]  
>  Владельца можно изменить с помощью инструкции [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="remarks"></a>Комментарии  
  
> [!NOTE]  
>  Общие сведения о списках свойств см. в статье [Поиск свойств документа с использованием списков свойств поиска](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 По умолчанию новый список свойств, которые можно искать, пуст. В него необходимо вручную добавить одно или несколько свойств. В качестве альтернативы можно скопировать существующий список свойств поиска. В этом случае новый список наследует поисковые свойства своего источника, но его можно изменить, добавив или удалив определенные свойства. При следующем полном заполнении все свойства, приведенные в списке свойств поиска, заносятся в полнотекстовый индекс.  
  
 Инструкция CREATE SEARCH PROPERTY LIST завершится неуспехом при выполнении любого из следующих условий.  
  
-   Если база данных, указанная параметром *database_name*, не существует.  
  
-   Если список, указанный параметром *source_list_name*, не существует.  
  
-   Если отсутствуют нужные разрешения.  
  
 **Добавление и удаление свойств в списке**  
  
-   [ALTER SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/alter-search-property-list-transact-sql.md)  
  
-   **Удаление списка свойств**  
  
-   [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимы разрешения CREATE FULLTEXT CATALOG в текущей базе данных и разрешение REFERENCES для любой базы данных, из которой копируется исходный список свойств.  
  
> [!NOTE]  
>  Для связывания списка с полнотекстовым индексом требуется разрешение REFERENCES. Для добавления или удаления свойств, а также для удаления списка требуется разрешение CONTROL. Владелец списка свойств поиска может предоставить разрешения REFERENCES и CONTROL на список. Пользователи с разрешением CONTROL также могут предоставлять разрешение REFERENCES другим пользователям.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-an-empty-property-list-and-associating-it-with-an-index"></a>A. Создание пустого списка свойств и связывание его с индексом  
 В следующем примере создается новый список свойств поиска с именем `DocumentPropertyList`. Затем инструкция [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) связывает новый список свойств с полнотекстовым индексом таблицы `Production.Document` из базы данных `AdventureWorks` без запуска заполнения.  
  
> [!NOTE]  
>  Пример, в котором в этот список свойств поиска добавляются несколько стандартных, известных свойств поиска, приведен в разделе [ALTER SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/alter-search-property-list-transact-sql.md). После добавления свойств поиска в список администратору базы данных необходимо будет использовать другую инструкцию ALTER FULLTEXT INDEX с предложением START FULL POPULATION.  
  
```sql 
CREATE SEARCH PROPERTY LIST DocumentPropertyList;  
GO  
USE AdventureWorks2012;  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList  
   WITH NO POPULATION;   
GO   
```  
  
### <a name="b-creating-a-property-list-from-an-existing-one"></a>Б. Создание нового списка свойств на основе существующего  
 В следующем примере создается новый список свойств поиска, `JobCandidateProperties`, из списка, созданного в примере A, `DocumentPropertyList`, который будет связан с полнотекстовым индексом из базы данных `AdventureWorks2012`. Затем инструкция ALTER FULLTEXT INDEX связывает новый список свойств с полнотекстовым индексом таблицы `HumanResources.JobCandidate` из базы данных `AdventureWorks2012`. Эта инструкция ALTER FULLTEXT INDEX начинает полное заполнение — поведением по умолчанию для предложения SET SEARCH PROPERTY LIST.  
  
```sql  
CREATE SEARCH PROPERTY LIST JobCandidateProperties 
FROM AdventureWorks2012.DocumentPropertyList;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   SET SEARCH PROPERTY LIST JobCandidateProperties;  
GO
```  
  
## <a name="see-also"></a>См. также  
 [ALTER SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/alter-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [Поиск свойств документа с использованием списков свойств поиска](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Поиск идентификаторов GUID наборов свойств и целочисленных идентификаторов свойств для свойств поиска](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
