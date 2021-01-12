---
description: COLUMNPROPERTY (Transact-SQL)
title: COLUMNPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLUMNPROPERTY
- COLUMNPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column properties [SQL Server]
- parameters [SQL Server], properties
- COLUMNPROPERTY function
ms.assetid: 2408c264-6eca-4120-bb71-df043c7c2792
author: cawrites
ms.author: chadam
ms.openlocfilehash: 287b8ff6085658b863601fcc11a3e8c37cd50faa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097285"
---
# <a name="columnproperty-transact-sql"></a>COLUMNPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Эта функция возвращает сведения о столбце или параметре.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
COLUMNPROPERTY ( id , column , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*id*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md), которое содержит идентификатор таблицы или процедуры.
  
*column*  
Выражение, которое содержит имя столбца или параметра.
  
*property*  
Для аргумента *id* аргумент *property* указывает тип данных, возвращаемый функцией `COLUMNPROPERTY`. Аргумент *property* может принимать следующие значения.
  
|Значение|Описание|Возвращенное значение|  
|---|---|---|
|**AllowsNull**|Разрешение использовать NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**ColumnId**|Значение идентификатора столбца, соответствующего **sys.columns.column_id**.|Идентификатор столбца<br /><br /> **Примечание**. При запросе множества столбцов могут появиться пропуски в последовательности значений идентификаторов столбцов.|  
|**FullTextTypeColumn**|TYPE COLUMN в таблице, которая содержит информацию о типе документа столбца *column*.|Идентификатор полнотекстового TYPE COLUMN для выражений имен столбцов, переданных вторым параметром этой функции.|  
|**GeneratedAlwaysType**|Значение столбца создано системой. Соответствует **sys.columns.generated_always_type**|**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и более поздних версий.<br /><br /> 0: создается не всегда<br /><br /> 1: создается всегда как начало строки<br /><br /> 2: создается всегда как конец строки|  
|**IsColumnSet**|Столбец представляет собой набор столбцов. Дополнительные сведения см. в статье [Использование наборов столбцов](../../relational-databases/tables/use-column-sets.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsComputed**|Столбец является вычисляемым столбцом.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsCursorType**|Параметр процедуры имеет тип CURSOR.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsDeterministic**|Столбцы являются детерминированными (предсказуемыми). Это свойство применимо только к вычисляемым столбцам и столбцам представлений.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные. Невычисляемый столбец или не столбец представлений.|  
|**IsFulltextIndexed**|Столбцы зарегистрированы для полнотекстовой индексации.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsHidden**|Значение столбца создано системой. Соответствует **sys.columns.is_hidden**|**Область применения**: [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] и более поздних версий.<br /><br /> 0: не скрытый<br /><br /> 1: скрытый|  
|**IsIdentity**|Столбец использует свойство IDENTITY.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsIdNotForRepl**|Столбец проверяет настройку IDENTITY_INSERT.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsIndexable**|Столбцы не могут быть индексированы.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsOutParam**|Параметр процедуры является выходным параметром.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsPrecise**|Точный столбец. Это свойство применимо только к детерминированным столбцам.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные. Недетерминированный столбец|  
|**IsRowGuidCol**|Столбец имеет тип данных **uniqueidentifier** и определяется вместе со свойством ROWGUIDCOL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsSparse**|Столбец является разреженным столбцом. Дополнительные сведения см. в статье [Использование разреженных столбцов](../../relational-databases/tables/use-sparse-columns.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsSystemVerified**|[!INCLUDE[ssDE](../../includes/ssde-md.md)] может проверять свойства детерминированности и точности столбца. Это свойство применимо только к вычисляемым столбцам и столбцам представлений.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**IsXmlIndexable**|XML-столбец может быть использован в XML-индексе.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**Точность**|Длина типа данных столбца или параметра.|Длина заданного типа данных столбца<br /><br /> -1: **xml** или типы больших значений<br /><br /> NULL: недопустимые входные данные.|  
|**Масштаб**|Масштаб для типа данных столбца или параметра.|Значение масштаба<br /><br /> NULL: недопустимые входные данные.|  
|**StatisticalSemantics**|Столбец доступен для семантического индексирования.|1: TRUE<br /><br /> 0: FALSE|  
|**SystemDataAccess**|Столбец, полученный из функции, которая получает данные в системных каталогах или виртуальных системных таблицах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это свойство применимо только к вычисляемым столбцам и столбцам представлений.|1: TRUE (обозначает доступ только для чтения)<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**UserDataAccess**|Столбец, полученный из функции, которая получает данные в пользовательских таблицах, включая таблицы видов и временные таблицы, хранится в локальном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это свойство применимо только к вычисляемым столбцам и столбцам представлений.|1: TRUE (обозначает доступ только для чтения)<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
|**UsesAnsiTrim**|ANSI_PADDING было задано во время создания таблицы. Это свойство применимо только к столбцам или параметрам типа **char** или **varchar**.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: недопустимые входные данные.|  
  
## <a name="return-types"></a>Типы возвращаемых данных
 **int**  
  
## <a name="exceptions"></a>Исключения  
Возвращает значение NULL в случае ошибки или если участник не имеет разрешений для просмотра объекта.
  
Пользователь может просматривать только метаданные защищаемых объектов, которыми он владеет или на которые пользователю были предоставлены разрешения. Это означает, что встроенные функции, создающие метаданные, такие как `COLUMNPROPERTY`, могут вернуть значение NULL в случае, если у пользователя нет правильных разрешений на объект. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Remarks  
Если проверяется детерминистическое свойство столбца, сначала нужно проверить, является ли столбец вычисляемым. Аргумент **IsDeterministic** возвращает NULL для невычисляемых столбцов. Вычисляемые столбцы могут быть определены как индексированные столбцы.
  
## <a name="examples"></a>Примеры  
Этот пример возвращает длину столбца `LastName`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT COLUMNPROPERTY( OBJECT_ID('Person.Person'),'LastName','PRECISION')AS 'Column Length';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Column Length
-------------
50
```  
  
## <a name="see-also"></a>См. также
[Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)  
[TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md)
  
  
