---
description: sys.spatial_index_tessellations (Transact-SQL)
title: sys.spatial_index_tessellations (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_index_tessellations
- sys.spatial_index_tessellations_TSQL
- spatial_index_tessellations_TSQL
- sys.spatial_index_tessellations
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_index_tessellations catalog view
ms.assetid: 8b17a9a4-b57f-4220-8138-fc73581b1670
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ddf7b9cd75999da6cc48f6e7a83bab7a733f822b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095425"
---
# <a name="sysspatial_index_tessellations-transact-sql"></a>sys.spatial_index_tessellations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 Представляет сведения о схеме тесселяции и параметрах каждого пространственного индекса.  
  
> [!NOTE]  
>  Сведения о тесселяции в см. в разделе [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md).  
  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Идентификатор объекта, на котором определен индекс. Каждая пара (object_id, index_id) имеет соответствующую запись в [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md).|  
|index_id|**int**|Идентификатор пространственного индекса, в котором определен столбец.|  
|tessellation_scheme|**sysname**|Имя схемы тесселяции, одно из следующих: GEOMETRY_GRID, GEOGRAPHY_GRID|  
|bounding_box_xmin|**float(53)**|Координата по оси x левого нижнего угла ограничивающего прямоугольника: значение NULL = неприменимо для данной схемы тесселяции (например, GEOGRAPHY_GRID) *n* = если tessellation_scheme GEOMETRY_GRID, значение координаты x-min.                     **Примечание.** Координаты, определенные параметрами ограничивающего прямоугольника, обрабатываются для каждого объекта в соответствии с его [идентификатором пространственной ссылки (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).|  
|bounding_box_ymin|**float(53)**|Координата по оси y левого нижнего угла ограничивающего прямоугольника: значение NULL = неприменимо для данной схемы тесселяции (например, GEOGRAPHY_GRID) *n* = если tessellation_scheme GEOMETRY_GRID, значение координаты y-min|  
|bounding_box_xmax|**float(53)**|Координата по оси X правого верхнего угла ограничивающего прямоугольника: значение NULL = неприменимо для данной схемы тесселяции (например, GEOGRAPHY_GRID) *n* = если tessellation_scheme GEOMETRY_GRID, значение координаты x-max|  
|bounding_box_ymax|**float(53)**|Координата по оси y правого верхнего угла ограничивающего прямоугольника: значение NULL = неприменимо для данной схемы тесселяции (например, GEOGRAPHY_GRID) *n* = если tessellation_scheme GEOMETRY_GRID, значение координаты y-max|  
|level_1_grid|**smallint**|Плотность сетки верхнего уровня. Если tessellation_scheme имеет значение GEOMETRY_GRID или GEOGRAPHY_GRID, то одно из следующих значений: 16 = 4 на 4 (LOW) 64 = 8 на 8 Grid (средний) 256 = 16 на 16 сетке (высокое значение), равное (HIGH), NULL = не применимо для данного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|level_1_grid_desc|**nvarchar(60)**|Плотность сетки для сетки верхнего уровня, одна из: низкая средняя высокая NULL = неприменимо для заданного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|level_2_grid|**smallint**|Плотность сетки второго уровня. Если tessellation_scheme имеет значение GEOMETRY_GRID или GEOGRAPHY_GRID, то одно из следующих значений: 16 = 4 на 4 (LOW) 64 = 8 на 8 Grid (средний) 256 = 16 на 16 сетке (высокое значение), равное (HIGH), NULL = не применимо для данного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|level_2_grid_desc|**nvarchar(60)**|Плотность сетки для сетки второго уровня, одна из следующих: низкая средняя высокая NULL = не применимо для данного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|level_3_grid|**smallint**|Плотность сетки третьего уровня.   Если tessellation_scheme имеет значение GEOMETRY_GRID или GEOGRAPHY_GRID, то одно из следующих значений: 16 = 4 на 4 (LOW) 64 = 8 на 8 Grid (средний) 256 = 16 на 16 сетке (высокое значение), равное (HIGH), NULL = не применимо для данного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|level_3_grid_desc|**nvarchar(60)**|Плотность сетки для сетки третьего уровня, одна из следующих: низкая средняя высокая NULL = не применимо для данного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|level_4_grid|**smallint**|Плотность сетки четвертого уровня. Если tessellation_scheme имеет значение GEOMETRY_GRID или GEOGRAPHY_GRID, то одно из следующих значений: 16 = 4 на 4 (LOW), 64 = 8 на 8 Grid (средний) 256 = 16 на 16 сетке (высокий) (среднее значение) NULL = не применимо для данного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|level_4_grid_desc|**nvarchar(60)**|Плотность сетки для сетки 4-го уровня, одна из: < низкий средний уровень NULL = не применимо для данного типа пространственного индекса или схемы тесселяции.  NULL возвращается в том случае, когда используется новая тесселяция SQL Server 11.|  
|cells_per_object|**int**|Число ячеек на пространственный объект, одно из следующих: Если tessellation_scheme имеет GEOMETRY_GRID или GEOGRAPHY_GRID, *n* = число ячеек на объект null = не применимо для данного типа пространственного индекса или схемы тесселяции|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
