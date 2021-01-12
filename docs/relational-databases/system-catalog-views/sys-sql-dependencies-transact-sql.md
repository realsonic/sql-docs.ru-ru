---
description: sys.sql_dependencies (Transact-SQL)
title: sys.sql_dependencies (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sql_dependencies
- sql_dependencies_TSQL
- sys.sql_dependencies_TSQL
- sys.sql_dependencies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_dependencies catalog view
ms.assetid: 1779aa87-a0b8-470a-a286-d7cc0b93ad2e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7c20d20cedd1304136af2d57f599aa2797dd4eee
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099198"
---
# <a name="syssql_dependencies-transact-sql"></a>sys.sql_dependencies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой зависимости, связанной с упоминаемой сущностью с помощью выражения или инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], определяющих какой-либо другой ссылающийся объект.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) .  

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Идентифицирует класс упоминаемой сущности:<br /><br /> 0 = объект или столбец (только ссылки, не связанные со схемами);<br /><br /> 1 = объект или столбец (ссылки, связанные со схемами);<br /><br /> 2 = типы (ссылки, связанные со схемами);<br /><br /> 3 = коллекции XML-схем (ссылки, связанные со схемами);<br /><br /> 4 = функции секционирования (ссылки, связанные со схемами).|  
|**class_desc**|**nvarchar(60)**|Описание класса упоминаемой сущности:<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_NON_SCHEMA_BOUND;**<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_SCHEMA_BOUND;**<br /><br /> **TYPE_REFERENCE**<br /><br /> **XML_SCHEMA_COLLECTION_REFERENCE;**<br /><br /> **PARTITION_FUNCTION_REFERENCE.**|  
|**object_id**|**int**|Идентификатор ссылающегося объекта.|  
|**column_id**|**int**|Если ссылающийся идентификатор является столбцом, то идентификатор; в противном случае 0.|  
|**referenced_major_id**|**int**|Идентификатор упоминаемой сущности, интерпретированный по значению класса следующим образом:<br /><br /> 0, 1 = идентификатор объекта или столбца;<br /><br /> 2 = идентификатор типа;<br /><br /> 3 = идентификатор коллекции XML-схем.|  
|**referenced_minor_id**|**int**|Вспомогательный идентификатор упоминаемой сущности, интерпретированный по значению класса, как показано ниже.<br /><br /> Когда поле «class» равно:<br /><br /> 0, **referenced_minor_id** является идентификатором столбца; или, если столбец не является столбцом, он равен 0.<br /><br /> 1, **referenced_minor_id** является идентификатором столбца; или, если столбец не является столбцом, он равен 0.<br /><br /> В противном случае **referenced_minor_id** = 0.|  
|**is_selected**|**bit**|Объект или столбец выбран.|  
|**is_updated**|**bit**|Объект или столбец обновлен.|  
|**is_select_all**|**bit**|Объект используется в предложении вида SELECT * (только уровень объектов).|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
