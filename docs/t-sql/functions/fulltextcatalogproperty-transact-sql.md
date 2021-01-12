---
description: FULLTEXTCATALOGPROPERTY (Transact-SQL)
title: FULLTEXTCATALOGPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FULLTEXTCATALOGPROPERTY_TSQL
- FULLTEXTCATALOGPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], properties
- FULLTEXTCATALOGPROPERTY function
- status information [SQL Server], full-text catalogs
ms.assetid: f841dc79-2044-4863-aff0-56b8bb61f250
author: cawrites
ms.author: chadam
ms.openlocfilehash: 57e53c26b7f1797daf6a1ccc53f7578eabda734a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087434"
---
# <a name="fulltextcatalogproperty-transact-sql"></a>FULLTEXTCATALOGPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает сведения о свойствах полнотекстовых каталогов в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
FULLTEXTCATALOGPROPERTY ('catalog_name' ,'property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
  
> [!NOTE]  
>  Следующие свойства будут удалены в будущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **LogSize** и **PopulateStatus**. Избегайте использовать эти свойства в новых разработках и запланируйте изменение приложений, где они используются в настоящий момент.  
  
_catalog\_name_  
Выражение, содержащее имя полнотекстового каталога.  
  
_property_  
Выражение, содержащее имя свойства полнотекстового каталога. В таблице перечислены свойства и описания возвращаемых сведений.  
  
|Свойство|Описание|  
|--------------|-----------------|  
|**AccentSensitivity**|Настройка учета диакритических знаков:<br /><br /> 0 = без учета диакритических знаков;<br /><br /> 1 = с учетом диакритических знаков.|  
|**IndexSize**|Логический размер полнотекстового каталога в мегабайтах (МБ). Включает размер индексов семантических ключевых фраз и индексов подобия документов.<br /><br /> Дополнительные сведения см. в подразделе «Примечания» далее в этом разделе.|  
|**ItemCount**|Число проиндексированных элементов, включая все полнотекстовые индексы, индексы ключевых фраз и индексы сходства документов, содержащихся в каталоге|  
|**LogSize**|Поддерживается только для обеспечения обратной совместимости. Всегда возвращает 0.<br /><br /> Размер в байтах связанного набора журналов ошибок, связанных с полнотекстовым каталогом [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search Service.|  
|**MergeStatus**|Указывает, выполняется ли слияние в единый файл.<br /><br /> 0 = слияние в единый файл не выполняется.<br /><br /> 1 = слияние в единый файл выполняется.|  
|**PopulateCompletionAge**|Разница в секундах между завершением последнего заполнения полнотекстового индекса и 01/01/1990 00:00:00.<br /><br /> Обновляется только для полного и последовательного сканирования. Возвращает значение 0, если заполнение не выполнялось.|  
|**PopulateStatus**|0 = бездействие<br /><br /> 1 = идет полное заполнение<br /><br /> 2 = пауза<br /><br /> 3 = ограниченный режим<br /><br /> 4 = восстановление<br /><br /> 5 = выключение<br /><br /> 6 = идет добавочное заполнение<br /><br /> 7 = построение индекса<br /><br /> 8 = диск заполнен. приостановлено<br /><br /> 9 = отслеживание изменений.|  
|**UniqueKeyCount**|Количество уникальных ключей в полнотекстовом каталоге.|  
|**ImportStatus**|Указывает, выполняется ли в настоящее время импорт полнотекстового каталога.<br /><br /> 0 = импорт полнотекстового каталога не выполняется.<br /><br /> 1 = выполняется импорт полнотекстового каталога.|  
  
## <a name="return-types"></a>Типы возвращаемых данных  
**int**  
  
## <a name="exceptions"></a>Исключения  
Возвращает значение NULL в случае ошибки или если вызывающий объект не имеет разрешений для просмотра текущего объекта.  
  
В [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] пользователь может просматривать только метаданные защищаемых объектов. Пользователь владеет этими защищаемыми объектами или на них пользователю были предоставлены разрешения. Таким образом, встроенные функции, такие как FULLTEXTCATALOGPROPERTY, создающие метаданные, могут вернуть значение NULL, если пользователь не имеет разрешений на объект. Дополнительные сведения см. в статье [sp_help_fulltext_catalogs (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md).  
  
## <a name="remarks"></a>Комментарии  
Функция FULLTEXTCATALOGPROPERTY (_catalog\_name_, **IndexSize**) анализирует только фрагменты с состоянием 4 или 6, указанным в представлении [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md). Данные фрагменты являются частью логического индекса. Таким образом, свойство **IndexSize** возвращает только логический размер индекса. 

При слиянии индексов фактический размер индекса может в два раза превышать его логический размер. Чтобы определить фактический размер, который занимает полнотекстовый индекс при слиянии, используйте системную хранимую процедуру [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md). При выполнении этой процедуры анализируются все фрагменты, связанные с полнотекстовым индексом. 

Заполнение полнотекстового индекса может завершиться ошибкой. Это может произойти, если ограничить рост файла полнотекстового каталога и не оставить достаточно места для процесса слияния. В этом случае инструкция FULLTEXTCATALOGPROPERTY (_catalog\_name_, **IndexSize**) возвращает значение 0, а сообщение об ошибке записывается в журнал полнотекстовых операций.  
  
`Error: 30059, Severity: 16, State: 1. A fatal error occurred during a full-text population and caused the population to be cancelled. Population type is: FULL; database name is FTS_Test (id: 13); catalog name is t1_cat (id: 5); table name t1 (id: 2105058535). Fix the errors that are logged in the full-text crawl log. Then, resume the population. The basic Transact-SQL syntax for this is: ALTER FULLTEXT INDEX ON table_name RESUME POPULATION.`  
  
Важно, чтобы приложение не ожидало в непрерывном цикле, проверяя, пока свойство **PopulateStatus** не будет в состоянии бездействия. Переход в состояние бездействия означает, что заполнение завершено. Эта проверка отвлекает ресурсы ЦП от обработки базы данных и процессов полнотекстового поиска и приводит к превышениям времени ожидания. Обычно лучший параметр для проверки соответствующего свойства **PopulateStatus** на уровне таблицы — свойство **TableFullTextPopulateStatus** в системной функции OBJECTPROPERTYEX. Это и другие полнотекстовые свойства в функции OBJECTPROPERTYEX предоставляют более подробные сведения о таблицах с полнотекстовым индексированием. Дополнительные сведения см. в разделе [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
Следующий пример возвращает количество полнотекстовых индексированных элементов в полнотекстовом каталоге `Cat_Desc`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT fulltextcatalogproperty('Cat_Desc', 'ItemCount');  
GO  
```  
  
## <a name="see-also"></a>См. также  
[FULLTEXTSERVICEPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
[Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
[sp_help_fulltext_catalogs (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
  
  
