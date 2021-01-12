---
description: INDEXPROPERTY (Transact-SQL)
title: INDEXPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INDEXPROPERTY
- INDEXPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INDEXPROPERTY function
- indexes [SQL Server], viewing
- indexes [SQL Server], properties
ms.assetid: 998d5788-4871-44a8-8125-0d9390868b84
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: db2534117dff3c3cfeac92f5e3cec1b12713eb33
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097238"
---
# <a name="indexproperty-transact-sql"></a>INDEXPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает значение свойства именованного индекса или статистики для указанного идентификационного номера таблицы, имени индекса или статистики, а также имени свойства. Возвращает NULL для XML-индексов.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
INDEXPROPERTY ( object_ID , index_or_statistics_name , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *object_ID*  
 Выражение, которое содержит идентификационный номер объекта таблицы или индексированного представления, для которого предоставляются сведения о свойстве индекса. Аргумент *object_ID* имеет тип **int**.  
  
 *index_or_statistics_name*  
 Выражение, которое содержит имя индекса или статистики, для которой возвращаются сведения о свойстве. Аргумент *index_or_statistics_name* имеет тип **nvarchar(128)**.  
  
 *property*  
 Выражение, которое содержит имя возвращаемого свойства базы данных. Аргумент *property* имеет тип **varchar(128)** и может принимать одно из указанных ниже значений.  
  
> [!NOTE]  
>  Если не указано иное, значение NULL возвращается в следующих случаях: если аргумент *property* не является допустимым именем свойства, если аргумент *object_ID* не является допустимым идентификатором объекта, если аргумент *object_ID* не является поддерживаемым типом объекта для указанного свойства или если вызывающий объект не имеет разрешения на просмотр метаданных объекта.  
  
|Свойство|Описание|Значение|  
|--------------|-----------------|-----------|  
|**IndexDepth**|Глубина индекса.|Количество уровней индекса.<br /><br /> NULL = Неверный XML-индекс или вход.|  
|**IndexFillFactor**|Значение коэффициента заполнения, использованное при создании индекса или при его последней перестройке.|Коэффициент заполнения.|  
|**IndexID**|Идентификатор индекса указанной таблицы или индексированного представления.|Идентификатор индекса.|  
|**IsAutoStatistics**|Статистики были сформированы параметром AUTO_CREATE_STATISTICS инструкции ALTER DATABASE.|1 = истина<br /><br /> 0 = False или XML-индекс.|  
|**IsClustered**|Кластеризованный индекс.|1 = истина<br /><br /> 0 = False или XML-индекс.|  
|**IsDisabled**|Индекс отключен.|1 = истина<br /><br /> 0 = ложь<br /><br /> NULL = Введенные значения недопустимы.|  
|**IsFulltextKey**|Индекс является ключом для полнотекстового и семантического индексирования таблицы.|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> 1 = истина<br /><br /> 0 = False или XML-индекс.<br /><br /> NULL = Введенные значения недопустимы.|  
|**IsHypothetical**|Индекс является гипотетическим и не может использоваться напрямую в качестве пути доступа к данным. Гипотетические индексы содержат статистики уровня столбца и управляются и используются помощником по настройке ядра СУБД.|1 = истина<br /><br /> 0 = False или XML-индекс.<br /><br /> NULL = Введенные значения недопустимы.|  
|**IsPadIndex**|Индекс задает пространство, которое должно оставаться открытым на каждом внутреннем узле.|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> 1 = истина<br /><br /> 0 = False или XML-индекс.|  
|**IsPageLockDisallowed**|Значение блокировки страницы, установленное параметром ALLOW_PAGE_LOCKS инструкции ALTER INDEX.|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> 1 = Блокировка страниц запрещена;<br /><br /> 0 = Блокировка страниц разрешена.<br /><br /> NULL = Введенные значения недопустимы.|  
|**IsRowLockDisallowed**|Значение блокировки строк, установленное параметром ALLOW_ROW_LOCKS инструкции ALTER INDEX.|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> 1 = Блокировка строк запрещена;<br /><br /> 0 = Блокировка строк разрешена.<br /><br /> NULL = Введенные значения недопустимы.|  
|**IsStatistics**|Аргумент *index_or_statistics_name* является статистикой, созданной инструкцией CREATE STATISTICS или параметром AUTO_CREATE_STATISTICS инструкции ALTER DATABASE.|1 = истина<br /><br /> 0 = False или XML-индекс.|  
|**IsUnique**|Индекс является уникальным.|1 = истина<br /><br /> 0 = False или XML-индекс.|  
|**IsColumnstore**|Оптимизированный для памяти xVelocity индекс columnstore.|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> 1 = истина<br /><br /> 0 = ложь| 
|**IsOptimizedForSequentialKey**|Индекс оптимизирован для включения операций вставки на последнюю страницу.|**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и более поздних версий. <br><br>1 = истина<br><br>0 = ложь| 
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
## <a name="exceptions"></a>Исключения  
 Возвращает значение NULL в случае ошибки или если участник не имеет разрешений для просмотра объекта.  
  
 Пользователь может просматривать только метаданные защищаемых объектов, которыми он владеет или на которые пользователю были предоставлены разрешения. Это означает, что создающие метаданные встроенные функции, такие как INDEXPROPERTY могут вернуть значение NULL в случае, если пользователь не имеет разрешений на объект. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере возвращаются значения свойств **IsClustered**, **IndexDepth** и **IndexFillFactor** индекса `PK_Employee_BusinessEntityID` таблицы `Employee` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT   
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IsClustered')AS [Is Clustered],  
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IndexDepth') AS [Index Depth],  
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IndexFillFactor') AS [Fill Factor];  
  
```  
  
 Результирующий набор:  
  
```  
Is Clustered Index Depth Fill Factor   
------------ ----------- -----------   
1            2           0  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В приведенном ниже примере проверяются свойства одного из индексов таблицы `FactResellerSales`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT   
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IsClustered') AS [Is Clustered],  
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IsColumnstore') AS [Is Columnstore Index],  
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IndexFillFactor') AS [Fill Factor];  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [Статистика](../../relational-databases/statistics/statistics.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

