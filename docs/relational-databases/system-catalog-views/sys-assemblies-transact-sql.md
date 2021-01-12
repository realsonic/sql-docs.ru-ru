---
description: sys.assemblies (Transact-SQL)
title: sys. assemblies (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.assemblies
- assemblies_TSQL
- sys.assemblies_TSQL
- assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assemblies catalog view
ms.assetid: e321753f-293f-42ab-b225-d118713df40b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6860b76d61730a91140ddf502f1bedf57f607b4a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102898"
---
# <a name="sysassemblies-transact-sql"></a>sys.assemblies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Возвращает по одной строке для каждой сборки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя сборки. Уникален в пределах базы данных.|  
|**principal_id**|**int**|Идентификатор участника, который является владельцем этой сборки.|  
|**assembly_id**|**int**|Идентификационный номер сборки. Уникален в базе данных.|  
|**clr_name**|**nvarchar(4000)**|Каноническая строка, кодирующая простое имя, номер версии, культуру, открытый ключ и архитектуру сборки. Данное значение однозначно идентифицирует сборку на стороне среды CLR.|  
|**permission_set**|**tinyint**|Набор разрешений/уровень безопасности для сборки.<br /><br /> 1 = безопасный доступ.<br /><br /> 2 = внешний доступ.<br /><br /> 3 = небезопасный доступ.|  
|**permission_set_desc**|**nvarchar(60)**|Описание набора разрешений/уровня безопасности для сборки.<br /><br /> SAFE_ACCESS<br /><br /> EXTERNAL_ACCESS<br /><br /> UNSAFE_ACCESS|  
|**is_visible**|**bit**|1 = сборка видна для регистрации точек входа [!INCLUDE[tsql](../../includes/tsql-md.md)].<br /><br /> 0 = сборка предназначена только для управляемых вызывающих объектов. Это означает, что сборка обеспечивает внутреннюю реализацию для других сборок в базе данных.|  
|**create_date**|**datetime**|Дата создания или регистрации сборки.|  
|**modify_date**|**datetime**|Дата изменения сборки.|  
|**is_user_defined**|**bit**|Указывает исходный файл сборки.<br /><br /> 0 = системные сборки (например, Microsoft. SqlServer. types для типа данных **hierarchyid** );<br /><br /> 1 = определяемые пользователем сборки|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога сборок среды CLR &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ASSEMBLYPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/assemblyproperty-transact-sql.md)  
  
  
