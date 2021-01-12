---
description: sys.system_components_surface_area_configuration (Transact-SQL)
title: sys.system_components_surface_area_configuration (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.system_components_surface_area_configuration_TSQL
- system_components_surface_area_configuration
- system_components_surface_area_configuration_TSQL
- sys.system_components_surface_area_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_components_surface_area_configuration catalog view
ms.assetid: d9920008-3387-4f9e-8f21-47473f2ba04f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f5eecd0b62dc819f3526053fb22779299d4c449
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092984"
---
# <a name="syssystem_components_surface_area_configuration-transact-sql"></a>sys.system_components_surface_area_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по одной строке для каждого исполняемого системного объекта, который может быть включен или отключен компонентом конфигурации контактной зоны. Дополнительные сведения см. в разделе [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**component_name**|**sysname**|Название компонента. Будет содержать параметры сортировки ключевых слов Latin1_General_CI_AS_KS_WS. Не может быть NULL.|  
|**database_name**|**sysname**|База данных, содержащая объект. Будет содержать параметры сортировки ключевых слов Latin1_General_CI_AS_KS_WS. Должна быть одной из следующих:<br /><br /> **master**<br /><br /> **msdb**<br /><br /> **mssqlsystemresource**|  
|**schema_name**|**sysname**|Схема, содержащая объект. Будет содержать параметры сортировки ключевых слов Latin1_General_CI_AS_KS_WS. Не может быть NULL.|  
|**object_name**|**sysname**|Имя объекта. Будет содержать параметры сортировки ключевых слов Latin1_General_CI_AS_KS_WS. Не может быть NULL.|  
|**state**|**tinyint**|0 = отключено<br /><br /> 1 = включено|  
|**type**|**char(2)**|Тип объекта. Может принимать следующие значения:<br /><br /> P = SQL_STORED_PROCEDURE<br /><br /> PC = CLR_STORED_PROCEDURE<br /><br /> FN = SQL_SCALAR_FUNCTION<br /><br /> FS = CLR_SCALAR_FUNCTION<br /><br /> FT = CLR_TABLE_VALUED_FUNCTION<br /><br /> IF = SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> TF = SQL_TABLE_VALUED_FUNCTION<br /><br /> X = EXTENDED_STORED_PROCEDURE|  
|**type_desc**|**nvarchar(60)**|Понятное описание имени типа объекта.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
