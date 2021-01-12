---
description: sys.table_types (Transact-SQL)
title: sys.table_types (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- table_types_TSQL
- sys.table_types
- sys.table_types_TSQL
- table_types
dev_langs:
- TSQL
helpviewer_keywords:
- table types [SQL Server]
- table-valued parameters, sys.table_types
- sys.table_types
- UDTT
ms.assetid: c05fd873-aff2-4a89-9936-a54c2ea09996
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c920317b40f0fbcd922c1fe0c440c1a210e0a984
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094452"
---
# <a name="systable_types-transact-sql"></a>sys.table_types (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Отображает свойства определенных пользователем табличных типов в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Табличный тип — это тип, на основании которого могут быть объявлены переменные таблицы или возвращающие табличное значение параметры. Каждый тип таблицы имеет **type_table_object_id** , который является внешним ключом в представлении каталога [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) . Этот столбец ИДЕНТИФИКАТОРов можно использовать для запроса различных представлений каталога способом, аналогичным **object_id** столбцу обычной таблицы, для обнаружения структуры табличного типа, например ее столбцов и ограничений.    
 
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|*\<inherited columns>*||Список столбцов, наследуемых этим представлением, см. в разделе [sys. types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md).|  
|**type_table_object_id**|**int**|Идентификационный номер объекта. Является уникальным в пределах базы данных.|  
|**is_memory_optimized**|**bit**|**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.<br /><br /> Допустимы следующие значения:<br /><br /> 0 = не оптимизировано для памяти<br /><br /> 1 = оптимизировано для памяти<br /><br /> Значение по умолчанию — 0.<br /><br /> Типы таблиц всегда создаются с DURABILITY = SCHEMA_ONLY. Только схема сохраняется на диске.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Использование Table-Valued параметров &#40;ядро СУБД&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)   
 [Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
