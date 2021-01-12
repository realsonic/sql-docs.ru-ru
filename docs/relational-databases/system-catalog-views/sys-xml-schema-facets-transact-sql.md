---
description: sys.xml_schema_facets (Transact-SQL)
title: sys.xml_schema_facets (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.xml_schema_facets
- xml_schema_facets_TSQL
- sys.xml_schema_facets_TSQL
- xml_schema_facets
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_facets catalog view
ms.assetid: 4402dde9-1877-4872-8550-140dc2a177d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4fd85732af9776c9dc0be1bb65c4402753468060
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097862"
---
# <a name="sysxml_schema_facets-transact-sql"></a>sys.xml_schema_facets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает строку для каждого аспекта (ограничения) определения типа XML (соответствует **sys.xml_types**).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**xml_component_id**|**int**|Идентификатор компонента (типа) XML, которому принадлежит данный аспект.|  
|**facet_id**|**int**|Идентификатор (порядковый номер, начиная с 1) аспекта, уникальный в пределах компонента.|  
|**особого**|**char(2)**|Тип аспекта:<br /><br /> LG = длина<br /><br /> LN = минимальная длина<br /><br /> LX = максимальная длина<br /><br /> PT = шаблон (регулярное выражение)<br /><br /> EU = перечисление<br /><br /> IN = минимальное значение (включая)<br /><br /> IX = максимальное значение (включая)<br /><br /> EN = минимальное значение (исключая)<br /><br /> EX = максимальное значение (исключая)<br /><br /> DT = общее количество цифр<br /><br /> DF = количество цифр после запятой<br /><br /> WS = нормализация пробелов|  
|**kind_desc**|**nvarchar (60)**|Описание типа аспекта:<br /><br /> LENGTH<br /><br /> MINIMUM_LENGTH<br /><br /> MAXIMUM_LENGTH<br /><br /> PATTERN<br /><br /> ENUMERATION<br /><br /> MINIMUM_INCLUSIVE_VALUE<br /><br /> MAXIMUM_INCLUSIVE_VALUE<br /><br /> MINIMUM_EXCLUSIVE_VALUE<br /><br /> MAXIMUM_EXCLUSIVE_VALUE<br /><br /> TOTAL_DIGITS<br /><br /> FRACTION_DIGITS<br /><br /> WHITESPACE_NORMALIZATION|  
|**is_fixed**|**bit**|1 = аспект имеет фиксированное, предопределенное значение.<br /><br /> 0 = нет фиксированного значения. (по умолчанию).|  
|**value**|**nvarchar (4000)**|Фиксированное, предопределенное значение аспекта.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Схемы XML &#40;представления каталога системы типов XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
