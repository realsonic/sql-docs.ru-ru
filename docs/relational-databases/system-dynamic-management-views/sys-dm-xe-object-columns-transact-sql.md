---
description: sys.dm_xe_object_columns (Transact-SQL)
title: sys.dm_xe_object_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_xe_object_columns
- sys.dm_xe_object_columns_TSQL
- dm_xe_object_columns_TSQL
- dm_xe_object_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_object_columns dynamic management view
- extended events [SQL Server], views
ms.assetid: d96a14f3-4284-45ff-b1fe-4858e540a013
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5da079cfecbf1ddaa622f05ce71e5c04d63f4f0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095076"
---
# <a name="sysdm_xe_object_columns-transact-sql"></a>sys.dm_xe_object_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о схеме для всех объектов.  
  
> [!NOTE]  
>  Объекты событий предоставляют фиксированные схемы как для данных, доступных только для чтения, так и для данных, доступных для чтения и записи.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Имя столбца. имя уникально в пределах объекта. Не допускает значение NULL.|  
|column_id|**int**|Идентификатор столбца. column_id уникален в пределах объекта при использовании с column_type. Не допускает значение NULL.|  
|object_name|**nvarchar(256)**|Имя объекта, которому принадлежит столбец. Обеспечивает связь «многие к одному» с sys.dm_xe_objects.id. Не допускает значения NULL.|  
|object_package_guid|**uniqueidentifier**|Идентификатор GUID пакета, в котором содержится объект. Не допускает значение NULL.|  
|type_name|**nvarchar(256)**|Имя типа для этого столбца. Не допускает значение NULL.|  
|type_package_guid|**uniqueidentifier**|Идентификатор GUID пакета, в котором содержится тип данных столбца. Не допускает значение NULL.|  
|column_type|**nvarchar(60)**|Показывает, как используется этот столбец. Не допускает значение NULL. column_type может быть одним из следующих:<br /><br /> readonly. Столбец содержит статическое значение, которое нельзя изменить.<br /><br /> данных. Столбец содержит данные времени выполнения, представленные объектом.<br /><br /> customizable. Столбец содержит значение, которое можно изменить.<br /><br /> Примечание. изменение этого значения может изменить поведение объекта.|  
|column_value|**nvarchar(256)**|Отображает статические значения, связанные со столбцом объекта. Допускает значение NULL.|  
|capabilities|**int**|Битовая карта, описывающая возможности столбца. Допускает значение NULL.|  
|capabilities_desc|**nvarchar(256)**|Описание возможностей этого столбца объекта. Значение может быть одним из следующих.<br /><br /> Mandatory. Значение должно быть задано при привязывании родительского объекта к сеансу событий.<br /><br /> Допускает значение NULL.|  
|description|**nvarchar (3072)**|Описание этого столбца объекта. Допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
### <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_object_columns.object_name, sys.dm_xe_object_columns.object_package_guid|sys.dm_xe_objects.name,<br /><br /> sys.dm_xe_objects.package_guid|«многие к одному»|  
|sys.dm_xe_object_columns.type_name<br /><br /> sys.dm_xe_object_columns.type_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_objects.package_guid|«многие к одному»|  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

