---
description: CREATE FULLTEXT INDEX (Transact-SQL)
title: CREATE FULLTEXT INDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FULLTEXT_INDEX_TSQL
- CREATE_FULLTEXT_INDEX_TSQL
- CREATE FULLTEXT INDEX
- FULLTEXT INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], creating
- index creation [SQL Server], CREATE FULLTEXT INDEX statement
- CREATE FULLTEXT INDEX statement
ms.assetid: 8b80390f-5f8b-4e66-9bcc-cabd653c19fd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 758a8447197a5fcbb3794a7abfb152c4a430779d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095827"
---
# <a name="create-fulltext-index-transact-sql"></a>CREATE FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает полнотекстовый индекс по таблице или индексированному представлению в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Для таблицы или индексированного представления допускается только один полнотекстовый индекс, а каждый полнотекстовый индекс применяется только к одной таблице или индексированному представлению. Полнотекстовый индекс может содержать не более 1024 столбцов.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE FULLTEXT INDEX ON table_name  
   [ ( { column_name   
             [ TYPE COLUMN type_column_name ]  
             [ LANGUAGE language_term ]   
             [ STATISTICAL_SEMANTICS ]  
        } [ ,...n]   
      ) ]  
    KEY INDEX index_name   
    [ ON <catalog_filegroup_option> ]  
    [ WITH [ ( ] <with_option> [ ,...n] [ ) ] ]  
[;]  
  
<catalog_filegroup_option>::=  
 {  
    fulltext_catalog_name   
 | ( fulltext_catalog_name, FILEGROUP filegroup_name )  
 | ( FILEGROUP filegroup_name, fulltext_catalog_name )  
 | ( FILEGROUP filegroup_name )  
 }  
  
<with_option>::=  
 {  
   CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF [, NO POPULATION ] }   
 | STOPLIST [ = ] { OFF | SYSTEM | stoplist_name }  
 | SEARCH PROPERTY LIST [ = ] property_list_name   
 }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*table_name*       
Имя таблицы или индексированного представления, содержащего столбец или столбцы, включенные в полнотекстовый индекс.  
  
*column_name*       
Имя столбца, включенного в полнотекстовый индекс. Для полнотекстового поиска можно индексировать только столбцы типа **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml** и **varbinary(max)**. Чтобы задать несколько столбцов, предложение *column_name* повторяется следующим образом:  
  
CREATE FULLTEXT INDEX ON *table_name* (*column_name1* [...], *column_name2* [...]) ...  
  
TYPE COLUMN *type_column_name*       
Указывает имя столбца таблицы (*type_column_name*), в котором хранится тип документа для документа **varbinary(max)** или **image**. Этот столбец называется столбцом типа и содержит указываемое пользователем расширение файла (.DOC, .PDF, .XLS и т. д.) Столбец типа должен иметь тип **char**, **nchar**, **varchar** или **nvarchar**.  
  
Указывайте параметр TYPE COLUMN *type_column_name* только в случае, когда в параметре *column_name* указан столбец типа **varbinary(max)** или **image**, где данные хранятся в двоичном виде. В противном случае [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]возвратит ошибку.  
  
> [!NOTE]  
> Во время индексирования средство полнотекстового поиска использует сокращение в столбце типа каждой строки таблицы, чтобы определить, какой фильтр полнотекстового поиска нужно использовать для документа, указанного в параметре *column_name*. Фильтр загружает документ в виде двоичного потока, удаляет данные форматирования и отправляет текст из документа в компонент разбиения по словам. Дополнительные сведения см. в разделе [Настройка фильтров для поиска и управление ими](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
LANGUAGE *language_term*       
Язык данных, хранящихся в столбце *column_name*.  
  
Аргумент *language_term* не является обязательным и может быть строкой, целым числом или шестнадцатеричным значением, соответствующим идентификатору локали (LCID). Если не задано никакого значения, используется язык экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], установленный по умолчанию.  
  
Если аргумент *language_term* указан, то соответствующий язык будет использоваться для индексации данных, хранящихся в столбцах **char**, **nchar**, **varchar**, **nvarchar**, **text** и **ntext**. Этот язык используется в запросах по умолчанию, если аргумент *language_term* не задан как часть полнотекстового предиката, характеризующего столбец.  
  
Если аргумент *language_term* задан в виде строки, то он соответствует значению столбца alias системной таблицы syslanguages. Строка должна быть заключена в одиночные кавычки: **'** _language\_term_ **'** . Если значением аргумента *language_term* является целое число, оно представляет собой действительный код языка. Если значение *language_term* задано в шестнадцатеричной форме, то после символов "0x" должна следовать шестнадцатеричная запись кода языка. Длина шестнадцатеричного значения не должна превышать восьми цифр, включая ведущие нули.  
  
Если значение указано в двухбайтовой кодировке (DBCS), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] преобразует его в Юникод.  
  
Для указанного аргументом *language_term* языка должны быть включены ресурсы, такие как средства разбиения по словам и парадигматические модули. Если такие ресурсы не поддерживают указанный язык, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибку.  
  
Для доступа к данным о языке полнотекстового поиска по умолчанию экземпляра [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следует использовать хранимую процедуру sp_configure. Дополнительные сведения см. в подразделе [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Для столбцов с типами, отличными от BLOB и XML, содержащих текстовые данные на нескольких языках, или в случаях, когда язык хранящегося в столбце текста неизвестен, лучше использовать нейтральный (0x0) языковой ресурс. Однако следует осознавать возможные последствия использования нейтрального языкового ресурса (0x0). Информацию о возможных решениях и последствиях использования нейтрального языкового ресурса (0x0) см. в разделе [Выбор языка при создании полнотекстового индекса](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md).  
  
Для документов, хранящихся в столбцах типов XML или BLOB, во время индексирования будет использоваться внутренняя языковая кодировка документа. Например: в XML-столбцах атрибут **xml:lang** в XML-документе будет идентифицировать язык. Во время запроса значение, ранее указанное для аргумента *language_term*, становится используемым по умолчанию языком в полнотекстовых индексах, если аргумент *language_term* не указан как часть полнотекстового запроса.  
  
STATISTICAL_SEMANTICS       
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше) 
  
Создает дополнительные индексы ключевых фраз и подобия документов, которые являются частью статистического семантического индексирования. Дополнительные сведения см. в разделе [Семантический поиск (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md).  
  
KEY INDEX *index_name*       
Имя индекса уникальных ключей таблицы *table_name*. Индекс KEY INDEX должен быть уникальным столбцом с одним ключом, не допускающим значения NULL. Выбрать минимально возможный индекс ключа для полнотекстового уникального ключа.  Для оптимальной производительности рекомендуется использовать для полнотекстовых ключей тип данных integer.  
  
*fulltext_catalog_name*       
Полнотекстовый каталог, используемый для полнотекстового индекса. Этот каталог уже должен существовать в базе данных. Это предложение является необязательным. Если оно не указано, используется каталог по умолчанию. Если каталога по умолчанию не существует, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибку.  
  
FILEGROUP *filegroup_name*       
Создает указанный полнотекстовый индекс в указанной файловой группе. Файловая группа должна существовать. Если предложение FILEGROUP не задано, полнотекстовый индекс помещается для несекционированной таблицы в ту же файловую группу, что и базовая таблица и представление, для секционированной таблицы — в первичную файловую группу.  
  
 CHANGE_TRACKING [ = ] { MANUAL | **AUTO** | OFF [ , NO POPULATION ] }       
 Указывает, будет ли [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] распространять на полнотекстовый индекс изменения (обновления, удаления или вставки), выполненные в столбцах таблицы, которые включены в полнотекстовый индекс. Изменения данных, внесенные с помощью инструкций WRITETEXT и UPDATETEXT, не отражаются в полнотекстовом индексе и не отмечаются при отслеживании изменений.  
  
MANUAL       
Указывает, что отслеживаемые изменения должны распространяться вручную путем вызова инструкции ALTER FULLTEXT INDEX … Инструкция START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] (*заполнение вручную*). Агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно использовать для периодического вызова инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
**AUTO**       
Указывает, что отслеживаемые изменения будут распространяться автоматически в ходе изменения данных в базовой таблице (*автоматическое заполнение*). Изменения могут распространяться автоматически, однако это не значит, что они будут немедленно отражаться в полнотекстовом индексе. Аргумент AUTO используется по умолчанию.  
  
OFF [ `,` NO POPULATION]       
Указывает, что в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не хранится список изменений индексированных данных. Если аргумент NO POPULATION не указан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] заполняет индекс полностью, после того как он был создан.  
  
Аргумент NO POPULATION может использоваться только в том случае, если аргументу CHANGE_TRACKING присвоено значение OFF. Если указан аргумент NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не заполняет индекс после его создания. Индекс заполняется только после выполнения пользователем команды ALTER FULLTEXT INDEX с предложением START FULL POPULATION или START INCREMENTAL POPULATION.  
  
STOPLIST [ = ] { OFF | **SYSTEM** | *stoplist_name* }       
Связывает полнотекстовый список стоп-слов с индексом. Индекс не заполняется токенами, которые являются частью указанного списка стоп-слов. Если список STOPLIST не указан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] связывает с индексом системный полнотекстовый список стоп-слов.  
  
OFF       
Указывает, что с полнотекстовым индексом не связан ни один из списков стоп-слов.  
  
**SYSTEM**       
Указывает, что для полнотекстового индекса должен использоваться системный полнотекстовый список стоп-слов.  
  
*stoplist_name*       
Задает имя списка стоп-слов, который будет связан с полнотекстовым индексом.  
  
SEARCH PROPERTY LIST [ = ] *property_list_name*       
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше)  
  
Связывает список свойств поиска с индексом.  
 
OFF       
Указывает, что с полнотекстовым индексом не связан ни один список свойств.  
  
*property_list_name*       
Задает имя списка свойств поиска, который будет связан с полнотекстовым индексом.  
  
## <a name="remarks"></a>Комментарии  
Дополнительные сведения о полнотекстовых индексах см. в разделе [Создание полнотекстовых индексов и управление ими](../../relational-databases/search/create-and-manage-full-text-indexes.md).  
  
Для столбцов **xml** можно создать полнотекстовый индекс, который индексирует содержимое XML-элементов, но пропускает XML-разметку. К значениям атрибута, если они не являются числовыми значениями, применяется полнотекстовый индекс. Теги элементов используются в качестве границ токенов. Поддерживаются XML- или HTML-документы и фрагменты правильного формата, содержащие несколько языков. Дополнительные сведения см. в разделе [Использование полнотекстового поиска со столбцами XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
Рекомендуется для ключевого столбца индекса использовать тип данных integer. Это позволяет проводить оптимизацию во время выполнения запроса.  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a>Взаимодействие между отслеживанием изменений и параметром NO POPULATION  
 Заполнение полнотекстового индекса зависит от того, включено ли отслеживание изменений и указано ли предложение WITH NO POPULATION в инструкции ALTER FULLTEXT INDEX. В следующей таблице описывается результат их взаимодействия.  
  
|Отслеживание изменений|WITH NO POPULATION|Результат|  
|---------------------|------------------------|------------|  
|Не включено|Не указано|Выполняется полное заполнение полнотекстового индекса.|  
|Не включено|Указано|Заполнение полнотекстового индекса не выполняется, если не выполнена инструкция ALTER FULLTEXT INDEX...START POPULATION.|  
|Активировано|Указано|Формируется ошибка. Индекс не изменяется.|  
|Активировано|Не указано|Выполняется полное заполнение полнотекстового индекса.|  
  
 Дополнительные сведения о заполнении полнотекстовых индексов см. в разделе [Заполнение полнотекстовых индексов](../../relational-databases/search/populate-full-text-indexes.md).  
  
## <a name="permissions"></a>Разрешения  
 Пользователь должен иметь разрешение `REFERENCES` для полнотекстового каталога и разрешение `ALTER` для таблицы или индексированного представления либо являться членом предопределенной роли сервера `sysadmin` или предопределенных ролей базы данных `db_owner` или `db_ddladmin`.  
  
Если указана инструкция `SET STOPLIST`, пользователь должен иметь разрешение REFERENCES на указанный список стоп-слов. Это разрешение может быть предоставлено владельцем списка стоп-слов.  
  
> [!NOTE]  
> Любому пользователю предоставлено разрешение REFERENCE на список стоп-слов по умолчанию, поставляемый в составе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-unique-index-a-full-text-catalog-and-a-full-text-index"></a>A. Создание уникального индекса, полнотекстового каталога и полнотекстового индекса  
 В следующем примере создается уникальный индекс по столбцу `JobCandidateID` таблицы `HumanResources.JobCandidate` образца базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Затем пример создает полнотекстовый каталог по умолчанию, `ft`. Наконец, пример создает полнотекстовый индекс по столбцу `Resume` с использованием каталога `ft` и системного списка стоп-слов.  
  
```sql  
CREATE UNIQUE INDEX ui_ukJobCand ON HumanResources.JobCandidate(JobCandidateID);  
CREATE FULLTEXT CATALOG ft AS DEFAULT;  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume)   
   KEY INDEX ui_ukJobCand   
   WITH STOPLIST = SYSTEM;  
GO  
```  
  
### <a name="b-creating-a-full-text-index-on-several-table-columns"></a>Б. Создание полнотекстового индекса для нескольких столбцов таблицы  
 В следующем примере создается полнотекстовый каталог, `production_catalog`, в образце базы данных `AdventureWorks`. Следующий пример создает полнотекстовый индекс, который использует этот новый каталог. Полнотекстовый индекс построен для столбцов `ReviewerName`, `EmailAddress` и `Comments` таблицы `Production.ProductReview`. Для каждого столбца в примере указывается код английского языка `1033`, который является языком данных в столбцах. В этом полнотекстовом индексе используется существующий уникальный индекс ключа `PK_ProductReview_ProductReviewID`. Согласно рекомендациям, этот ключ индекса находится в целочисленном столбце `ProductReviewID`.  
  
```sql  
CREATE FULLTEXT CATALOG production_catalog;  
GO  
CREATE FULLTEXT INDEX ON Production.ProductReview  
 (   
  ReviewerName  
     Language 1033,  
  EmailAddress  
     Language 1033,  
  Comments   
     Language 1033       
 )   
  KEY INDEX PK_ProductReview_ProductReviewID   
      ON production_catalog;   
GO  
```  
  
### <a name="c-creating-a-full-text-index-with-a-search-property-list-without-populating-it"></a>В. Создание полнотекстового индекса со списком свойств поиска без его заполнения  
 В следующем примере создается полнотекстовый индекс по столбцам `Title`, `DocumentSummary` и `Document` таблицы `Production.Document`. В примере для каждого столбца указан код для английского языка, `1033`, который является языком данных в столбцах. Этот полнотекстовый индекс использует полнотекстовый каталог по умолчанию и существующий индекс уникального ключа — `PK_Document_DocumentID`. Согласно рекомендациям, этот ключ индекса находится в целочисленном столбце `DocumentID`.  
  
 В примере указывается системный список стоп-слов. Кроме того, задается список свойств поиска `DocumentPropertyList`. Пример создания такого списка свойств см. в разделе [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
 В примере указано, что отслеживание изменений отключено (без заполнения). Позже, в часы с наименьшей загрузкой, будет запущено полное заполнение нового индекса и включено автоматическое отслеживание изменений с помощью инструкции ALTER FULLTEXT INDEX.  
  
```sql  
CREATE FULLTEXT INDEX ON Production.Document  
  (   
  Title  
      Language 1033,   
  DocumentSummary  
      Language 1033,   
  Document   
      TYPE COLUMN FileExtension  
      Language 1033   
  )  
  KEY INDEX PK_Document_DocumentID  
          WITH STOPLIST = SYSTEM, SEARCH PROPERTY LIST = DocumentPropertyList, CHANGE_TRACKING OFF, NO POPULATION;  
   GO  
```  
  
 Позже, в часы с наименьшей загрузкой, выполняется заполнение индекса:  
  
```sql  
ALTER FULLTEXT INDEX ON Production.Document SET CHANGE_TRACKING AUTO;  
GO  
```  
  
## <a name="see-also"></a>См. также  
[Создание и управление полнотекстовыми индексами](../../relational-databases/search/create-and-manage-full-text-indexes.md)       
[ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)       
[DROP FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/drop-fulltext-index-transact-sql.md)       
[Компонент Full-text Search](../../relational-databases/search/full-text-search.md)       
[GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)       
[sys.fulltext_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)       
[Поиск свойств документа с помощью списков свойств поиска](../../relational-databases/search/search-document-properties-with-search-property-lists.md)       
  
  
