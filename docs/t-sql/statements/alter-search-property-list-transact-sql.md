---
description: ALTER SEARCH PROPERTY LIST (Transact-SQL)
title: ALTER SEARCH PROPERTY LIST (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SEARCH_PROPERTY_TSQL
- ALTER_SEARCH_PROPERTY_LIST_TSQL
- ALTER SEARCH PROPERTY LIST
- ALTER SEARCH PROPERTY
- ALTER_SEARCH_TSQL
- ALTER SEARCH
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], altering
- ALTER SEARCH PROPERTY LIST statement
ms.assetid: 0436e4a8-ca26-4d23-93f1-e31e2a1c8bfb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 510bd7abb561fca8a425b843a1647290054b425a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094732"
---
# <a name="alter-search-property-list-transact-sql"></a>ALTER SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Добавляет или удаляет указанное свойство поиска из указанного списка свойств поиска.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER SEARCH PROPERTY LIST list_name  
{  
   ADD 'property_name'  
     WITH   
      (   
          PROPERTY_SET_GUID = 'property_set_guid'  
        , PROPERTY_INT_ID = property_int_id  
      [ , PROPERTY_DESCRIPTION = 'property_description' ]  
      )  
 | DROP 'property_name'   
}  
;  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *list_name*  
 Имя изменяемого списка свойств. *list_name* — это идентификатор.  
  
 Чтобы просмотреть имена существующих списков свойств, используйте представление каталога [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md) следующим образом:  
  
```sql  
SELECT name FROM sys.registered_search_property_lists;  
```  
  
 ADD  
 Добавляет указанное свойство поиска в список свойств, указанный параметром *list_name*. Данное свойство зарегистрировано в списке свойств поиска. Прежде чем добавленные свойства можно будет использовать для поиска свойств, необходимо повторно заполнить связанные полнотекстовые индексы. Дополнительные сведения см. в статье [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md).  
  
> [!NOTE]  
>  Чтобы добавить данное свойство поиска в список свойств поиска, необходимо предоставить идентификатор GUID набора свойств (*property_set_guid*) и целочисленный идентификатор свойства (*property_int_id*). Дополнительные сведения см. в разделе «Получение идентификаторов GUID набора свойств и целочисленных идентификаторов» далее в этом разделе.  
  
 *property_name*  
 Указывает имя, которое будет использоваться для обозначения свойства в полнотекстовых запросах. *property_name* должно быть уникальным идентификатором свойства в наборе свойств. Имя свойства может содержать внутренние пробелы. Длина *property_name* не должна превышать 256 символов. Это имя должно быть описательным, таким как "Автор" или "Домашний адрес", или каноническим именем Windows свойства, например **System.Author** или **System.Contact.HomeAddress**.  
  
 Разработчики будут использовать указанное значение *property_name* для идентификации свойства в предикате [CONTAINS](../../t-sql/queries/contains-transact-sql.md). Поэтому при добавлении свойства важно указать значение, которое значимо представляет свойство, определенное указанным идентификатором GUID набора свойств (*property_set_guid*) и идентификатором свойства (*property_int_id*). Дополнительные сведения об именах свойств см. в подразделе «Замечания» далее в этом разделе.  
  
 Для просмотра имен свойств, существующих в списке свойств поиска для текущей базы данных, используйте представление каталога [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) следующим образом:  
  
```sql  
SELECT property_name FROM sys.registered_search_properties;  
```  
  
 PROPERTY_SET_GUID ='*property_set_guid*'  
 Указывает идентификатор набора свойств, к которому принадлежит свойство. Это глобальный уникальный идентификатор GUID. Дополнительные сведения о получении этого значения см. в подразделе «Замечания» далее в этом разделе.  
  
 Для просмотра идентификатора GUID набора свойств любого свойства, существующего в списке свойств поиска для текущей базы данных, используйте представление каталога [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) следующим образом:  
  
```sql  
SELECT property_set_guid FROM sys.registered_search_properties;  
```  
  
 PROPERTY_INT_ID =*property_int_id*  
 Указывает целое число, определяющее свойство в наборе свойств. Дополнительные сведения о получении этого значения см. в подразделе «Замечания».  
  
 Для просмотра целочисленного идентификатора любого свойства, существующего в списке свойств поиска для текущей базы данных, используйте представление каталога [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) следующим образом:  
  
```sql  
SELECT property_int_id FROM sys.registered_search_properties;  
```  
  
> [!NOTE]  
>  Заданное сочетание *property_set_guid* и *property_int_id* должно быть уникальным в пределах списка свойств поиска. При попытке добавить существующее сочетание операция ALTER SEARCH PROPERTY LIST завершается неудачей и выдается ошибка. Это означает, что для свойства можно определить только одно имя.  
  
 PROPERTY_DESCRIPTION ='*property_description*'  
 Указывает задаваемое пользователем описание свойства. *property_description* — это строка длиной не более 512 символов. Это необязательный параметр.  
  
 DROP  
 Удаляет указанное свойство из списка свойств, указанного параметром *list_name*. Удаление свойства отменяет его регистрацию, поэтому оно становится недоступным для поиска.  
  
## <a name="remarks"></a>Комментарии  
 Каждый полнотекстовый индекс может иметь только один список свойств поиска.  
  
 Чтобы разрешить запросы по данному свойству поиска, необходимо добавить его в список свойств поиска полнотекстового индекса, а затем повторно заполнить индекс.  
  
 При задании свойства предложения PROPERTY_SET_GUID, PROPERTY_INT_ID и PROPERTY_DESCRIPTION можно расположить в любом порядке, разделив их запятыми и поместив в круглые скобки, например:  
  
```sql  
ALTER SEARCH PROPERTY LIST CVitaProperties  
ADD 'System.Author'   
WITH (   
      PROPERTY_DESCRIPTION = 'Author or authors of a given document.',  
      PROPERTY_SET_GUID   = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9',   
      PROPERTY_INT_ID = 4   
      );  
```  
  
> [!NOTE]  
>  В этом примере использовано имя свойства, `System.Author`, концептуально похожее на канонические имена свойств в Windows Vista (каноническое имя Windows).  
  
## <a name="obtaining-property-values"></a>Получение значений свойств  
 Полнотекстовый поиск сопоставляет свойство поиска с полнотекстовым индексом с помощью идентификатора GUID набора свойств и целочисленного идентификатора свойства. Сведения о способе получения этих свойств, определенных корпорацией Майкрософт, см. в разделе [Поиск идентификаторов GUID для наборов свойств и целочисленных идентификаторов свойств для свойств поиска](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md). Дополнительные сведения о свойствах, определенных независимыми поставщиками программного обеспечения (ISV), см. в документации по этим поставщикам.  
  
## <a name="making-added-properties-searchable"></a>Наделение добавленных свойств возможностью поиска  
 Добавление свойства поиска в список свойств поиска приводит к регистрации свойства. Добавленное свойство может быть немедленно указано в запросах [CONTAINS](../../t-sql/queries/contains-transact-sql.md). Однако запросы полнотекстового каталога, использующие вновь добавленное свойство, не будут возвращать документы, пока не будет заполнен соответствующий полнотекстовый индекс. Например, следующий запрос, основанный на вновь добавленном свойстве *new_search_property*, не вернет документов, пока полнотекстовый индекс, связанный с целевой таблицей (*table_name*), не будет повторно заполнен:  
  
```sql  
SELECT column_name  
FROM table_name  
WHERE CONTAINS( PROPERTY( column_name, 'new_search_property' ), 
               'contains_search_condition');  
GO   
```  
  
 Чтобы запустить полное заполнение, воспользуйтесь следующей инструкцией [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md):  
  
```sql  
USE database_name;  
GO  
ALTER FULLTEXT INDEX ON table_name START FULL POPULATION;  
GO  
```  
  
> [!NOTE]  
>  Повторное заполнение не требуется после удаления свойства из списка свойств, так как только свойства, остающиеся в списке свойств поиска, доступны для полнотекстовых запросов.  
  
## <a name="related-references"></a>Связанные справочники  
 **Создание списка свойств**  
  
-   [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)  
  
 **Удаление списка свойств**  
  
-   [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
 **Добавление и удаление списка свойств в полнотекстовом индексе**  
  
-   [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
 **Запуск заполнения полнотекстового индекса**  
  
-   [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение CONTROL на список свойств.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-adding-a-property"></a>A. Добавление свойства  
 В следующем примере добавляется несколько свойств (`Title`, `Author` и `Tags`) к списку свойств с именем `DocumentPropertyList`.  
  
> [!NOTE]  
>  Пример создания такого списка свойств `DocumentPropertyList` см. в разделе [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
   ADD 'Title'   
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 2,   
      PROPERTY_DESCRIPTION = 'System.Title - Title of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Author'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 4,   
      PROPERTY_DESCRIPTION = 'System.Author - Author or authors of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Tags'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 5,   
      PROPERTY_DESCRIPTION = 
          'System.Keywords - Set of keywords (also known as tags) assigned to the item.' );  
```  
  
> [!NOTE]  
>  Необходимо связать данный список свойств поиска с полнотекстовым индексом перед его использованием для запросов в области свойства. Для этого используйте инструкцию [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) и укажите предложение SET SEARCH PROPERTY LIST.  
  
### <a name="b-dropping-a-property"></a>Б. Удаление свойства  
 В следующем примере свойство `Comments` удаляется из списка свойств `DocumentPropertyList`.  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
DROP 'Comments' ;  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [Поиск свойств документа с использованием списков свойств поиска](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Поиск идентификаторов GUID наборов свойств и целочисленных идентификаторов свойств для свойств поиска](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
