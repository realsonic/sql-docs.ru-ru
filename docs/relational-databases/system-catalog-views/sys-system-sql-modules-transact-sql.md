---
description: sys.system_sql_modules (Transact-SQL)
title: sys.system_sql_modules (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- system_sql_modules_TSQL
- sys.system_sql_modules
- sys.system_sql_modules_TSQL
- system_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_sql_modules catalog view
ms.assetid: ad3548bc-4780-4821-b962-b421d52daed9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d08a1f60ae706cf0327189ba596609e2640c9e59
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094511"
---
# <a name="syssystem_sql_modules-transact-sql"></a>sys.system_sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает одну запись на каждый системный объект, который содержит языковый модуль SQL. Системные объекты типа FN, IF, P, PC, TF, V обладают связанным модулем SQL. Чтобы найти содержащий объект, можно присоединить это представление к [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификационный номер контейнера уникален в пределах базы данных.|  
|**definition**|**nvarchar(max)**|Текст на языке SQL, определяющий этот модуль.|  
|**uses_ansi_nulls**|**bit**|1 = модуль был создан с параметром SET ANSI_NULLS ON.<br /><br /> Всегда возвращает 1.|  
|**uses_quoted_identifier**|**bit**|1 = модуль был создан с параметром SET QUOTED_IDENTIFIER ON.<br /><br /> Всегда возвращает 1.|  
|**is_schema_bound**|**bit**|0 = модуль был создан с параметром SCHEMABINDING.<br /><br /> Всегда возвращает 0.|  
|**uses_database_collation**|**bit**|0 = модуль не зависит от параметров сортировки базы данных по умолчанию.<br /><br /> Всегда возвращает 0.|  
|**is_recompiled**|**bit**|0 = процедура не была создана с параметром WITH RECOMPILE.<br /><br /> Всегда возвращает 0.|  
|**null_on_null_input**|**bit**|0 = модуль не был создан, чтобы обеспечить выходные значения NULL для любых входных значений NULL.<br /><br /> Всегда возвращает 0.|  
|**execute_as_principal_id**|**int**|Всегда возвращает NULL.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.all_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-sql-modules-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
