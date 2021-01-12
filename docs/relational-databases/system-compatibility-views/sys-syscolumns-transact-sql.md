---
description: sys.syscolumns (Transact-SQL)
title: sys.sysстолбцы (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-enginel, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscolumns
- sys.syscolumns_TSQL
- syscolumns_TSQL
- syscolumns
dev_langs:
- TSQL
helpviewer_keywords:
- syscolumns system table
- sys.syscolumns compatibility view
ms.assetid: 863fd87b-ff33-4ac5-9aa9-df21140681da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9ecd4982135135fbf23c700a30318d86026d266
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099204"
---
# <a name="syssyscolumns-transact-sql"></a>sys.syscolumns (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Возвращает по одной строке на каждый из столбцов всех таблиц и представлений и по одной строке на каждый из параметров хранимых процедур в базе данных.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя столбца или параметра процедуры.|  
|**id**|**int**|Идентификатор объекта таблицы, которому принадлежит столбец, или идентификатор хранимой процедуры, с которой связан данный параметр.|  
|**кстипе**|**tinyint**|Тип физического хранилища из **sys. types**.|  
|**typestat**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|Идентификатор расширенного пользовательского типа. Вызывает переполнение или возвращает значение NULL, если количество типов данных превышает 32 767.|  
|**length**|**smallint**|Максимальная длина физического хранилища в **sys**. **типы**.|  
|**xprec**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**идентификатора столбца**|**smallint**|Идентификатор столбца или параметра.|  
|**xoffset**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**процессу**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|Идентификатор значения по умолчанию для данного столбца.|  
|**поддомен**|**int**|Идентификатор правила или ограничения CHECK для данного столбца.|  
|**number**|**smallint**|Номер подпроцедуры, если процедура сгруппирована:<br /><br /> 0 = Непроцедурные элементы.|  
|**colorder**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|**varbinary(8000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**smallint**|Смещение в строке, по которому находится данный столбец.|  
|**коллатионид**|**int**|Идентификатор параметров сортировки для столбца. Для несимвольных столбцов содержит значение NULL.|  
|**status**|**tinyint**|Битовая карта, описывающая свойства столбца или параметра:<br /><br /> 0x08 = В столбце допускаются значения NULL;<br /><br /> 0x10 = заполнение ANSI было применено при добавлении столбцов **varchar** или **varbinary** . Замыкающие пробелы сохраняются для **varchar** , а замыкающие нули сохраняются для столбцов типа **varbinary** .<br /><br /> 0x40 = Параметр OUTPUT;<br /><br /> 0x80 = Столбец является столбцом идентификаторов.|  
|**type**|**tinyint**|Тип физического хранилища из **sys**. **типы**.|  
|**usertype**|**smallint**|Идентификатор определяемого пользователем типа данных из **sys. types**. Вызывает переполнение или возвращает значение NULL, если количество типов данных превышает 32 767.|  
|**printfmt**|**varchar (255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**прек**|**smallint**|Уровень точности для данного столбца:<br /><br /> -1 = **XML** или тип больших значений.|  
|**масштаб**|**int**|Масштаб для данного столбца.<br /><br /> NULL = данные не числовые.|  
|**вычислено**|**int**|Флаг, обозначающий, является ли столбец вычисляемым:<br /><br /> 0 = невычисляемый;<br /><br /> 1 = вычисляемый.|  
|**исаутпарам**|**int**|Указывает, относится ли параметр процедуры к выходным параметрам:<br /><br /> 1 = истина<br /><br /> 0 = ложь|  
|**IsNullable**|**int**|Указывает, допускает ли столбец значения NULL:<br /><br /> 1 = истина<br /><br /> 0 = ложь|  
|**параметры сортировки**|**sysname**|Имя параметров сортировки для данного столбца. Содержит NULL, если столбец не относится к символьному типу.|  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
