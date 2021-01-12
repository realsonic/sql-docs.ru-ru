---
description: sys.filetable_system_defined_objects (Transact-SQL)
title: sys.filetable_system_defined_objects (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.filetable_system_defined_objects_TSQL
- filetable_system_defined_objects
- filetable_system_defined_objects_TSQL
- sys.filetable_system_defined_objects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.filetable_system_defined_objects catalog view
ms.assetid: 62022e6b-46f6-495f-b14b-53f41e040361
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f18fdc79d85239422d80687a8ba54b5027caae85
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098017"
---
# <a name="sysfiletable_system_defined_objects-transact-sql"></a>sys.filetable_system_defined_objects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Отображает список системных объектов, связанных с таблицами FileTable. Содержит по одной строке для каждого системного объекта.  
  
 При создании FileTable одновременно создаются связанные объекты, такие как ограничения и индексы. Эти объекты нельзя изменять или удалять, они исчезают только после удаления самой таблицы FileTable.  
  
 Дополнительные сведения о таблицах FileTable см. в разделе [Таблицы FileTable (SQL Server)](../../relational-databases/blob/filetables-sql-server.md).  
  
|Столбец|Тип данных|Описание|  
|------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта системных объектов, относящихся к FileTable.<br /><br /> Ссылается на объект в **представлении sys. Objects**.|  
|**parent_object_id**|**int**|Идентификатор объекта родительской таблицы FileTable.<br /><br /> Ссылается на объект в **представлении sys. Objects**.|  
  
## <a name="see-also"></a>См. также:  
 [Создание, изменение и удаление таблиц FileTables](../../relational-databases/blob/create-alter-and-drop-filetables.md)   
 [Управление таблицами FileTable](../../relational-databases/blob/manage-filetables.md)  
  
  
