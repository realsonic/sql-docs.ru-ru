---
description: syspolicy_conditions (Transact-SQL)
title: syspolicy_conditions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syspolicy_conditions
- syspolicy_conditions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_conditions view
ms.assetid: af97d26c-4bd5-4b08-be51-8419e3b2832c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: edaf380a4a6a8f938c4b8601cb60999a0e99a330
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100100"
---
# <a name="syspolicy_conditions-transact-sql"></a>syspolicy_conditions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Отображает одну строку для каждого условия управления на основе политик в данном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. syspolicy_conditions принадлежит схеме dbo в базе данных msdb. В следующей таблице описываются столбцы представления syspolicy_conditions.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|condition_id|**int**|Идентификатор условия. Каждое условие представляет собой коллекцию, состоящую из одного или нескольких выражений условий.|  
|name|**sysname**|Имя условия.|  
|date_created|**datetime**|Дата и время создания условия.|  
|description|**nvarchar(max)**|Описание условия. Столбец описания является необязательным и может принимать значение NULL.|  
|created_by|**sysname**|Имя пользователя, создавшего условие.|  
|modified_by|**sysname**|Имя пользователя, изменившего это условие последним. Содержит значение NULL, если изменений не было.|  
|date_modified|**datetime**|Дата и время создания условия. Содержит значение NULL, если изменений не было.|  
|is_name_condition|**smallint**|Указывает, является ли условие условием именования.<br /><br /> 0 = выражение условия не содержит переменной @Name.<br /><br /> 1 = выражение условия содержит переменную @Name.|  
|facet|**nvarchar(max)**|Имя аспекта, на котором основано условие.|  
|Expression|**nvarchar(max)**|Выражение для состояний аспекта.|  
|obj_name|**sysname**|Имя объекта, присвоенного @Name, если выражение условия содержит переменную.|  
  
## <a name="remarks"></a>Комментарии  
 При устранении неполадок управления на основе политик, выполнив запрос представления syspolicy_conditions, можно определить, кто создал или последним изменил условие.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли PolicyAdministratorRole базы данных msdb.  
  
## <a name="see-also"></a>См. также  
 [Администрирование серверов с помощью управления на основе политик](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Административные представления на основе политик (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
