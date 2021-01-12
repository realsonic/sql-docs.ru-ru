---
description: sys.xml_schema_types (Transact-SQL)
title: sys.xml_schema_types (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.xml_schema_types_TSQL
- xml_schema_types_TSQL
- sys.xml_schema_types
- xml_schema_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_types catalog view
ms.assetid: 441ba49d-f778-4fa1-98c4-ced375a01a34
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2bd282bdecec59df5c1edf611cf2e2a8d6cfa08c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097903"
---
# <a name="sysxml_schema_types-transact-sql"></a>sys.xml_schema_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает строку на каждый компонент схемы XML, являющийся типом **Symbol_space** **T**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Наследует столбцы от [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_abstract**|**bit**|1 = Это абстрактный тип. Все экземпляры элемента этого типа должны использовать **xsi: Type** для обозначения производного типа, который не является абстрактным.<br /><br /> 0 = Это неабстрактный тип. (по умолчанию).|  
|**allows_mixed_content**|**bit**|1 = Разрешено смешанное содержимое.<br /><br /> 0 = Смешанное содержимое не допускается (по умолчанию).|  
|**is_extension_blocked**|**bit**|1 = замена с расширением типа блокируется в экземплярах, если атрибуту Block в определении **complexType** или атрибуту **blockDefault** элемента-предка \<schema> задано значение "Extension" или "#all".<br /><br /> 0 = Замещение с расширением не блокировано.|  
|**is_restriction_blocked**|**bit**|1 = замена с ограничением типа блокируется в экземплярах, если атрибуту Block в определении **complexType** или атрибуту **blockDefault** элемента-предка \<schema> задано значение "restriction" или "#all".<br /><br /> 0 = замещение с ограничением не блокировано (по умолчанию).|  
|**is_final_extension**|**bit**|1 = наследование по расширению типа блокируется, если атрибуту Final в определении **complexType** или атрибуту **finalDefault** элемента-предка \<schema> задано значение "Extension" или "#all".<br /><br /> 0 = расширение разрешено (по умолчанию).|  
|**is_final_restriction**|**bit**|1 = производное ограничение типа блокируется, если атрибуту Final в определении Simple или **complexType** или атрибуту **finalDefault** элемента-предка \<schema> задано значение "restriction" или "#all".<br /><br /> 0 = ограничение разрешено (по умолчанию).|  
|**is_final_list_member**|**bit**|1 = этот простой тип не может быть использован в качестве типа элемента списка.<br /><br /> 0 = этот тип является составным, либо он может быть использован в качестве типа элемента списка (по умолчанию).|  
|**is_final_union_member**|**bit**|1 = этот простой тип не может быть использован в качестве типа члена объединения.<br /><br /> 0 = сложный тип. или он может быть использован как тип члена объединения. (по умолчанию).|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Схемы XML &#40;представления каталога системы типов XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
