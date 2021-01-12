---
description: CREATE FULLTEXT STOPLIST (Transact-SQL)
title: CREATE FULLTEXT STOPLIST (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STOPLIST_TSQL
- FULLTEXT STOPLIST
- STOPLIST
- FULLTEXT_STOPLIST_TSQL
- CREATE FULLTEXT STOPLIST
- CREATE_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- CREATE FULLTEXT STOPLIST statement
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 0669b1d0-46cc-4fac-8df7-5f7fa7af5db4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40668fb2b4deecdbe64df50f68e6d318e639f717
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095841"
---
# <a name="create-fulltext-stoplist-transact-sql"></a>CREATE FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает новый полнотекстовый список стоп-слов в текущей базе данных.  
  
 Стоп-словами в базе данных можно управлять с помощью объектов, называемых *списками стоп-слов*. Списки стоп-слов взаимосвязаны с полнотекстовыми индексами и применяются при полнотекстовых запросах по этим индексам. Дополнительные сведения см. в разделе [Настройка стоп-слов и списков стоп-слов для полнотекстового поиска и управление ими](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
> [!IMPORTANT]  
>  Инструкции CREATE FULLTEXT STOPLIST, ALTER FULLTEXT STOPLIST и DROP FULLTEXT STOPLIST поддерживаются только в условиях применения уровня совместимости 100. При использовании уровней совместимости 80 и 90 эти инструкции не поддерживаются. Тем не менее при любом уровне совместимости системный список стоп-слов автоматически связывается с новыми полнотекстовыми индексами.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
CREATE FULLTEXT STOPLIST stoplist_name  
[ FROM { [ database_name.]source_stoplist_name } | SYSTEM STOPLIST ]  
[ AUTHORIZATION owner_name ]  
;  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *stoplist_name*  
 Имя списка стоп-слов. Длина *stoplist_name* не может превышать 128 символов. Аргумент *stoplist_name* должен быть уникальным среди всех списков стоп-слов текущей базы данных и соответствовать правилам для идентификаторов.  
  
 Аргумент *stoplist_name* будет использоваться при создании полнотекстового индекса.  
  
 *database_name*  
 Имя базы данных, в которой находится список стоп-слов, указанный аргументом *source_stoplist_name*. Если не указано, в качестве *database_name* по умолчанию выбирается текущая база данных.  
  
 *source_stoplist_name*  
 Указывает, что новый список стоп-слов создается копированием существующего списка стоп-слов. Если *source_stoplist_name* не существует или пользователь базы данных не обладает необходимыми разрешениями, инструкция CREATE FULLTEXT STOPLIST завершается с ошибкой. Если в текущей базе данных не зарегистрирован любой язык стоп-слов из исходного списка стоп-слов, инструкция CREATE FULLTEXT STOPLIST завершается успешно, но с предупреждениями, а соответствующие стоп-слова не добавляются.  
  
 SYSTEM STOPLIST  
 Указывает, что новый список стоп-слов создается из списка, существующего в [базе данных ресурсов](../../relational-databases/databases/resource-database.md) по умолчанию.  
  
 AUTHORIZATION *owner_name*  
 Указывает имя участника базы данных, являющейся владельцем списка стоп-слов. Аргумент *owner_name* должен быть именем участника базы данных, членом которого является текущий пользователь, или текущий пользователь должен иметь разрешение IMPERSONATE для *owner_name*. Если атрибут не указан, владельцем становится текущий пользователь.  
  
## <a name="remarks"></a>Remarks  
 Создателем списка стоп-слов является его владелец.  
  
## <a name="permissions"></a>Разрешения  
 Для создания списка стоп-слов необходимы разрешения CREATE FULLTEXT CATALOG. Владелец списка стоп-слов может явно предоставить разрешение CONTROL для списка стоп-слов, что позволит пользователям добавлять и удалять стоп-слова, а также удалять список стоп-слов.  
  
> [!NOTE]  
>  Чтобы использовать список стоп-слов с полнотекстовым индексом, необходимо разрешение REFERENCE.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-new-full-text-stoplist"></a>A. Создание полнотекстового списка стоп-слов  
 В следующем примере показано создание полнотекстового списка стоп-слов с именем `myStoplist`.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist;  
GO  
```  
  
### <a name="b-copying-a-full-text-stoplist-from-an-existing-full-text-stoplist"></a>Б. Копирование полнотекстового списка стоп-слов из существующего полнотекстового списка стоп-слов  
 В следующем примере демонстрируется создание нового полнотекстового списка стоп-слов с именем `myStoplist2` путем копирования существующего списка стоп-слов базы данных AdventureWorks с именем `Customers.otherStoplist`.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist2 FROM AdventureWorks.otherStoplist;  
GO  
```  
  
### <a name="c-copying-a-full-text-stoplist-from-the-system-full-text-stoplist"></a>В. Копирование списка полнотекстовых стоп-слов из системного полнотекстового списка стоп-слов  
 Следующий пример демонстрирует создание нового полнотекстового списка стоп-слов с именем `myStoplist3` путем копирования из системного списка стоп-слов.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist3 FROM SYSTEM STOPLIST;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [Настройка стоп-слов, списков стоп-слов и управление ими для полнотекстового поиска](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [Настройка и управление стоп-словами и списками стоп-слов для полнотекстового поиска](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
