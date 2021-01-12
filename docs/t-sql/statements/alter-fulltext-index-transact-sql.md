---
description: ALTER FULLTEXT INDEX (Transact-SQL)
title: ALTER FULLTEXT INDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER FULLTEXT INDEX
- ALTER_FULLTEXT_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- modifying full-text indexes
- full-text indexes [SQL Server], modifying
- search property lists [SQL Server], associating with full-text indexes
- ALTER FULLTEXT INDEX statement
ms.assetid: b6fbe9e6-3033-4d1b-b6bf-1437baeefec3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d415680b64097abd9674fc6a68f9b2d3a80bb29b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082804"
---
# <a name="alter-fulltext-index-transact-sql"></a>ALTER FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Изменяет свойства полнотекстового индекса в среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER FULLTEXT INDEX ON table_name  
   { ENABLE   
   | DISABLE  
   | SET CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF }  
   | ADD ( column_name   
           [ TYPE COLUMN type_column_name ]   
           [ LANGUAGE language_term ]  
           [ STATISTICAL_SEMANTICS ]  
           [,...n]   
         )  
     [ WITH NO POPULATION ]  
   | ALTER COLUMN column_name  
     { ADD | DROP } STATISTICAL_SEMANTICS  
     [ WITH NO POPULATION ]  
   | DROP ( column_name [,...n] )  
     [ WITH NO POPULATION ]   
   | START { FULL | INCREMENTAL | UPDATE } POPULATION  
   | {STOP | PAUSE | RESUME } POPULATION   
   | SET STOPLIST [ = ] { OFF| SYSTEM | stoplist_name }  
     [ WITH NO POPULATION ]   
   | SET SEARCH PROPERTY LIST [ = ] { OFF | property_list_name }  
     [ WITH NO POPULATION ]   
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *table_name*  
 Имя таблицы или индексированного представления, содержащего столбец или столбцы, включенные в полнотекстовый индекс. Указывать имена владельцев базы данных и таблицы не обязательно.  
  
 ENABLE | DISABLE  
 Указывает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], нужно ли собирать данные полнотекстового индекса для *table_name*. Значение ENABLE активирует полнотекстовый индекс, значение DISABLE отключает. Таблица не поддерживает полнотекстовые запросы, если индекс отключен.  
  
 Отключение полнотекстового индекса позволяет отключить отслеживание изменений, но при этом сохранить полнотекстовый индекс, который можно повторно включить с помощью инструкции ENABLE. Если полнотекстовый индекс отключен, метаданные полнотекстового индекса остаются в системных таблицах. Если состояние CHANGE_TRACKING включено (автоматическое или ручное обновление) когда полнотекстовый индекс отключен, то состояние индекса закрепляется, любое текущее сканирование останавливается, новые изменения в данных таблицы не отслеживаются и не вносятся в индекс.  
  
 SET CHANGE_TRACKING {MANUAL | AUTO | OFF}  
 Указывает, будет ли [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] распространять на полнотекстовый индекс изменения (обновления, удаления или вставки), выполненные в столбцах таблицы, которые включены в полнотекстовый индекс. Изменения данных, внесенные с помощью инструкций WRITETEXT и UPDATETEXT, не отражаются в полнотекстовом индексе и не отмечаются при отслеживании изменений.  
  
> [!NOTE]  
>  Дополнительные сведения см. в разделе [Взаимодействие между отслеживанием изменений и параметром NO POPULATION](#change-tracking-no-population).
  
 MANUAL  
 Указывает, что отслеживаемые изменения будут распространяться вручную путем вызова инструкции ALTER FULLTEXT INDEX … Инструкция START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] (*заполнение вручную*). Агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно использовать для периодического вызова инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 AUTO  
 Указывает, что отслеживаемые изменения будут распространяться автоматически в ходе изменения данных в базовой таблице (*автоматическое заполнение*). Изменения могут распространяться автоматически, однако это не значит, что они будут немедленно отражаться в полнотекстовом индексе. Аргумент AUTO используется по умолчанию.  
  
 OFF  
 Указывает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не будет вести списка изменений в индексированных данных.  
  
 ADD | DROP *column_name*  
 Указывает столбцы, добавляемые или удаляемые из полнотекстового индекса. Столбец или столбцы должны иметь тип **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** или **varbinary(max)**.  
  
 Предложение DROP используется только для столбцов, ранее включенных для полнотекстового индексирования.  
  
 Следует использовать параметры TYPE COLUMN и LANGUAGE с предложением ADD, чтобы назначить эти свойства для столбца *column_name*. После добавления столбца полнотекстовый индекс таблицы должен быть заполнен повторно, чтобы обеспечить выполнение полнотекстовых запросов к этому столбцу.  
  
> [!NOTE]  
>  Заполняется ли полнотекстовый индекс после добавления или удаления столбца полнотекстового индекса, зависит от того, включено ли отслеживание изменений и указано ли предложение WITH NO POPULATION. Дополнительные сведения см. в разделе [Взаимодействие между отслеживанием изменений и параметром NO POPULATION](#change-tracking-no-population).
  
 TYPE COLUMN *type_column_name*  
 Указывает имя столбца таблицы (*type_column_name*), в котором хранится тип документа для документа **varbinary**, **varbinary(max)** или **image**. Этот столбец называется столбцом типа и содержит указываемое пользователем расширение файла (.DOC, .PDF, .XLS и т. д.) Столбец типа должен иметь тип **char**, **nchar**, **varchar** или **nvarchar**.  
  
 Указывайте параметр TYPE COLUMN *type_column_name* только в случае, когда в параметре *column_name* указан столбец типа **varbinary**, **varbinary(max)** или **image**, где данные хранятся в двоичном виде. В противном случае [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвратит ошибку.  
  
> [!NOTE]  
>  Во время индексирования средство полнотекстового поиска использует сокращение в столбце типа каждой строки таблицы, чтобы определить, какой фильтр полнотекстового поиска нужно использовать для документа, указанного в параметре *column_name*. Фильтр загружает документ в виде двоичного потока, удаляет данные форматирования и отправляет текст из документа в компонент разбиения по словам. Дополнительные сведения см. в разделе [Настройка фильтров для поиска и управление ими](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
 LANGUAGE *language_term*  
 Язык данных, хранящихся в столбце **column_name**.  
  
 Аргумент *language_term* не является обязательным и может быть строкой, целым числом или шестнадцатеричным значением, соответствующим идентификатору локали (LCID). Если аргумент *language_term* задан, то соответствующий язык будет применяться ко всем элементам условия поиска. Если не указано никакого значения, по умолчанию используется язык полнотекстового поиска экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Для доступа к данным о языке полнотекстового поиска по умолчанию экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следует использовать хранимую процедуру **sp_configure**.  
  
 Если аргумент *language_term* задан в виде строки, то он соответствует значению столбца **alias** системной таблицы **syslanguages**. Строка должна быть заключена в одиночные кавычки: '*language_term*'. Если значением аргумента *language_term* является целое число, оно представляет собой действительный код языка. Если значение *language_term* задано в шестнадцатеричной форме, то после символов "0x" должна следовать шестнадцатеричная запись кода языка. Длина шестнадцатеричного значения не должна превышать восьми цифр, включая ведущие нули.  
  
 Если значение указано в двухбайтовой кодировке (DBCS), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] преобразует его в Юникод.  
  
 Для указанного аргументом *language_term* языка должны быть включены ресурсы, такие как средства разбиения по словам и парадигматические модули. Если такие ресурсы не поддерживают указанный язык, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибку.  
  
 Для столбцов, имеющих типы, отличные от BLOB и XML, которые содержат текстовые данные на нескольких языках, или в случаях, когда язык текста в столбце неизвестен, используйте нейтральный (0x0) языковой ресурс. Для документов, хранящихся в столбцах типов XML или BLOB, во время индексирования будет использоваться внутренняя языковая кодировка документа. Например: в XML-столбцах атрибут xml:lang в XML-документе будет идентифицировать язык. Во время запроса значение, ранее указанное для аргумента *language_term*, становится используемым по умолчанию языком в полнотекстовых индексах, если аргумент *language_term* не указан как часть полнотекстового запроса.  
  
 STATISTICAL_SEMANTICS  
 **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 Создает дополнительные индексы ключевых фраз и подобия документов, которые являются частью статистического семантического индексирования. Дополнительные сведения см. в разделе [Семантический поиск (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md).  
  
 [ **,** _...n_]  
 Указывает, что для предложений ADD, ALTER и DROP можно задать несколько столбцов. Если указано несколько столбцов, то эти столбцы следует разделить запятыми.  
  
 WITH NO POPULATION  
 Указывает, что полнотекстовый индекс не будет заполнен после операции ADD или DROP над столбцом или операции SET STOPLIST. Индекс будет заполнен, только если пользователь выполняет команду START...POPULATION.  
  
 Если указан параметр NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не заполняет индекс. Индекс заполняется только после того, как пользователь выдает команду ALTER FULLTEXT INDEX...START POPULATION. Если параметр NO POPULATION не указан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] заполняет индекс.  
  
 Если состояние CHANGE_TRACKING включено и указано предложение WITH NO POPULATION, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибку. Если состояние CHANGE_TRACKING включено и не предложение WITH NO POPULATION не указано, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет полное заполнение индекса.  
  
> [!NOTE]  
>  Дополнительные сведения см. в разделе [Взаимодействие между отслеживанием изменений и параметром NO POPULATION](#change-tracking-no-population).
  
 {ADD | DROP } STATISTICAL_SEMANTICS  
 **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 Включает или отключает статистическое семантическое индексирование для указанных столбцов. Дополнительные сведения см. в разделе [Семантический поиск (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md).  
  
 START {FULL|INCREMENTAL|UPDATE} POPULATION  
 Предписывает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] начать заполнение полнотекстового индекса *table_name*. Если заполнение полнотекстового индекса уже выполняется, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает предупреждение и не начинает нового заполнения.  
  
 FULL  
 Указывает, что каждая строка таблицы должна быть извлечена для полнотекстового индексирования, даже если строки уже были индексированы.  
  
 INCREMENTAL  
 Указывает, что только строки, измененные со времени последнего заполнения, должны извлекаться для полнотекстового индексирования. Аргумент INCREMENTAL можно использовать, только если в таблице есть столбец типа **timestamp**. Если таблица в полнотекстовом каталоге не содержит столбец типа **timestamp**, то выполняется полное заполнение.  
  
 UPDATE  
 Указывает обработку всех вставок, обновлений и удалений со времени последнего обновления индекса отслеживания изменений. В таблице должно быть включено заполнение отслеживания изменений, но индекс фонового обновления или автоматическое отслеживание изменений не должны быть включены.  
  
 {STOP | PAUSE | RESUME } POPULATION  
 Останавливает или приостанавливает процесс заполнения; либо останавливает или возобновляет приостановленное заполнение.  
  
 Предложение STOP POPULATION не останавливает автоматическое отслеживание изменений или фоновое обновление индекса. Чтобы остановить отслеживание изменений, следует использовать команду SET CHANGE_TRACKING OFF.  
  
 Предложения PAUSE POPULATION и RESUME POPULATION могут использоваться только для полных заполнений. Они не существенны для других типов заполнения, так как последние возобновляют сканирование с момента, когда сканирование было остановлено.  
  
 SET STOPLIST { OFF| SYSTEM | *stoplist_name* }  
 Изменяет полнотекстовый список стоп-слов, связанный с индексом, если он есть.  
  
 OFF  
 Указывает, что с полнотекстовым индексом не связан ни один из списков стоп-слов.  
  
 SYSTEM  
 Указывает, что для полнотекстового индекса должен использоваться системный полнотекстовый список стоп-слов.  
  
 *stoplist_name*  
 Задает имя списка стоп-слов, который будет связан с полнотекстовым индексом.  
  
 Дополнительные сведения см. в разделе [Настройка стоп-слов и списков стоп-слов для полнотекстового поиска и управление ими](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
 SET SEARCH PROPERTY LIST { OFF | *property_list_name* } [ WITH NO POPULATION ]  
 **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 Изменяет список свойств поиска, связанный с индексом, если он есть.  
  
 OFF  
 Указывает, что с полнотекстовым индексом не связан ни один список свойств. При отключении списка свойств поиска полнотекстового индекса (ALTER FULLTEXT INDEX … SET SEARCH PROPERTY LIST OFF) поиск свойств в базовой таблице становится невозможным.  
  
 По умолчанию при отключении существующего списка свойств поиска автоматически производится повторное заполнение полнотекстового индекса. Если при отключении списка свойств поиска указать WITH NO POPULATION, автоматическое повторное заполнение не выполняется. Однако рекомендуется выполнить полное заполнение этого полнотекстового индекса в удобное время. При повторном заполнении полнотекстового индекса удаляются относящиеся к свойствам метаданные для каждого удаленного свойства и индекс становится компактнее и эффективнее.  
  
 *property_list_name*  
 Задает имя списка свойств поиска, который будет связан с полнотекстовым индексом.  
  
 Добавление к полнотекстовому индексу списка свойств поиска требует повторного заполнения индекса, чтобы проиндексировать свойства поиска, зарегистрированные в этом списке. Если при добавлении списка свойств поиска указать WITH NO POPULATION, то в подходящее время потребуется выполнить заполнение индекса.  
  
> [!IMPORTANT]  
>  Если полнотекстовый индекс был ранее связан с другим поиском, то потребуется перестроить список свойств, чтобы привести индекс в согласованное состояние. Этот индекс немедленно усекается и остается пустым до запуска полного заполнения. Дополнительные сведения см. в разделе [Изменение списка свойств поиска приводит к перестроению индекса](#change-search-property-rebuild-index). 
  
> [!NOTE]  
>  Один список свойств поиска может быть связан с несколькими полнотекстовыми индексами в той же базе данных.  
  
 **Поиск списков свойств поиска в текущей базе данных**  
  
-   [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
 Дополнительные сведения о списках свойств поиска см. в статье [Поиск свойств документа с использованием списков свойств поиска](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a><a name="change-tracking-no-population"></a> Взаимодействие между отслеживанием изменений и параметром NO POPULATION  
 Заполнение полнотекстового индекса зависит от того, включено ли отслеживание изменений и указано ли предложение WITH NO POPULATION в инструкции ALTER FULLTEXT INDEX. В следующей таблице описывается результат их взаимодействия.  
  
|Отслеживание изменений|WITH NO POPULATION|Результат|  
|---------------------|------------------------|------------|  
|Не включено|Не указано|Выполняется полное заполнение полнотекстового индекса.|  
|Не включено|Указано|Заполнение полнотекстового индекса не выполняется, если не выполнена инструкция ALTER FULLTEXT INDEX...START POPULATION.|  
|Активировано|Указано|Формируется ошибка. Индекс не изменяется.|  
|Активировано|Не указано|Выполняется полное заполнение полнотекстового индекса.|  
  
 Дополнительные сведения о заполнении полнотекстовых индексов см. в разделе [Заполнение полнотекстовых индексов](../../relational-databases/search/populate-full-text-indexes.md).  
  
## <a name="changing-the-search-property-list-causes-rebuilding-the-index"></a><a name="change-search-property-rebuild-index"></a> Изменение списка свойств поиска приводит к перестроению индекса  
 При первоначальном связывании полнотекстового индекса со списком свойств поиска необходимо провести повторное заполнение индекса для индексации поисковых выражений, связанных со свойствами. Существующие данные индекса не усекаются.  
  
 Однако если связать полнотекстовый индекс с другим списком свойств, то он будет перестроен. При этом полнотекстовый индекс немедленно усекается (удаляются все существующие данные) и должен быть повторно заполнен. Пока заполнение не завершено, полнотекстовые запросы к базовой таблице будут находить только те строки, для которых уже прошло заполнение. После повторного заполнения индекс будет содержать метаданные для зарегистрированных свойств вновь добавленного списка свойств поиска.  
  
 Перестроение выполняется в следующих случаях:  
  
-   Прямое переключение на другой список свойств поиска (см. «Сценарий А» ниже в этом разделе).  
  
-   Отключение списка свойств поиска с последующим связыванием индекса с другим списком (см. «Сценарий Б» ниже в этом разделе).  
  
> [!NOTE]  
>  Дополнительные сведения о работе полнотекстового поиска со списками свойств поиска см. в статье [Поиск свойств документа с использованием списков свойств поиска](../../relational-databases/search/search-document-properties-with-search-property-lists.md). Сведения о полных заполнениях см. в разделе [Заполнение полнотекстовых индексов](../../relational-databases/search/populate-full-text-indexes.md).  
  
### <a name="scenario-a-switching-directly-to-a-different-search-property-list"></a>Сценарий А. Прямое переключение на другой список свойств поиска  
  
1.  Создан полнотекстовый индекс для таблицы `table_1` со списком свойств поиска `spl_1`:  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1,   
       CHANGE_TRACKING OFF, NO POPULATION;   
    ```  
  
2.  Выполняется полное заполнение для полнотекстового индекса:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 START FULL POPULATION;  
    ```  
  
3.  Затем полнотекстовый индекс связывается с другим списком свойств поиска, `spl_2`, с помощью следующей инструкции:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_2;  
    ```  
  
     Эта инструкция вызывает полное заполнение — действие по умолчанию.  Однако перед началом заполнения индекса средство полнотекстового поиска автоматически усекает его.  
  
### <a name="scenario-b-turning-off-the-search-property-list-and-later-associating-the-index-with-any-search-property-list"></a>Сценарий Б. Отключение списка свойств поиска с последующим связыванием индекса с другим списком  
  
1.  Создан полнотекстовый индекс для таблицы `table_1` со списком свойств поиска `spl_1`, после чего автоматически выполнено полное заполнение (действие по умолчанию):  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1;   
    ```  
  
2.  Отключается список свойств поиска, следующим образом.  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1   
       SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
    ```  
  
3.  Полнотекстовый индекс снова связывается либо с тем же, либо с другим списком свойств поиска.  
  
     Например, следующая инструкция связывает полнотекстовый индекс с первоначальным списком свойств поиска — `spl_1`:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_1;  
    ```  
  
     Эта инструкция запускает полное заполнение (действие по умолчанию).  
  
    > [!NOTE]  
    >  Перестроение потребуется также и для другого списка свойств поиска — `spl_2`.  
  
## <a name="permissions"></a>Разрешения  
 Пользователь должен иметь разрешение ALTER на таблицу или индексированное представление, быть членом предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_ddladmin** или **db_owner**.  
  
 Если указано предложение SET STOPLIST, пользователь должен иметь разрешение REFERENCES на список стоп-слов. Если указано SET SEARCH PROPERTY LIST, то пользователь должен иметь разрешение REFERENCES для списка свойств поиска. Владелец указанного списка стоп-слов или списка свойств поиска может предоставить разрешение REFERENCES, если владелец имеет разрешения ALTER FULLTEXT CATALOG.  
  
> [!NOTE]  
>  Любому пользователю предоставлено разрешение REFERENCES на список стоп-слов по умолчанию, поставляемый в составе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-setting-manual-change-tracking"></a>A. Задание отслеживания изменений вручную  
 В следующем примере вручную задается отслеживание изменений для полнотекстового индекса таблицы `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate  
   SET CHANGE_TRACKING MANUAL;  
GO  
```  
  
### <a name="b-associating-a-property-list-with-a-full-text-index"></a>Б. Связывание списка свойств с полнотекстовым индексом  
  
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 В следующем примере производится связывание списка свойств `DocumentPropertyList` с полнотекстовым индексом таблицы `Production.Document`. Эта инструкция ALTER FULLTEXT INDEX начинает полное заполнение — поведением по умолчанию для предложения SET SEARCH PROPERTY LIST.  
  
> [!NOTE]  
>  Пример создания такого списка свойств `DocumentPropertyList` см. в разделе [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList;   
GO  
```  
  
### <a name="c-removing-a-search-property-list"></a>В. Удаление списка свойств поиска  
  
**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 В следующем примере производится удаление списка свойств `DocumentPropertyList` из полнотекстового индекса для таблицы `Production.Document`. В этом примере не требуется срочно удалять свойства из индекса, поэтому указан параметр WITH NO POPULATION. Однако поиск с использованием свойств выполняется дольше, чем поиск с полнотекстовым индексом.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
GO  
```  
  
### <a name="d-starting-a-full-population"></a>Г. Запуск полного заполнения  
 В следующем примере запускается выполнение полного заполнения полнотекстового индекса в таблице `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   START FULL POPULATION;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [sys.fulltext_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [DROP FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [Компонент Full-text Search](../../relational-databases/search/full-text-search.md)   
 [Заполнение полнотекстовых индексов](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
