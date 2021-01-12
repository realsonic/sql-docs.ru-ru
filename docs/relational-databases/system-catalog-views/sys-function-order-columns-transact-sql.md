---
description: sys.function_order_columns (Transact-SQL)
title: sys.function_order_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- function_order_columns
- sys.function_order_columns_TSQL
- function_order_columns_TSQL
- sys.function_order_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.function_order_columns catalog view
ms.assetid: 29287973-3125-4d35-8ca9-92cb45828854
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8a3408d1f4e7a32aafed3318d32f0a9ed7a97d3f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102791"
---
# <a name="sysfunction_order_columns-transact-sql"></a>sys.function_order_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по одной строке на столбец, который является частью выражения **порядка** в функции, возвращающей табличное значение среды CLR.  

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта (функция CLR с табличным значением), на основании которого определен порядок.|  
|**order_column_id**|**int**|Идентификатор столбца сортировки. **order_column_id** уникален только в пределах **object_id**.<br /><br /> **order_column_id** представляет расположение этого столбца в упорядочении.|  
|**column_id**|**int**|Идентификатор столбца в **object_id**.<br /><br /> **column_id** уникален только в пределах **object_id**.|  
|**is_descending**|**bit**|1 — столбец сортировки имеет направление сортировки по убыванию.<br /><br /> 0 — столбец сортировки имеет направление сортировки по возрастанию.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
