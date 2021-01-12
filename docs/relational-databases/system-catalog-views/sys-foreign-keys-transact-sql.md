---
description: sys.foreign_keys (Transact-SQL)
title: sys.foreign_keys (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- foreign_keys
- sys.foreign_keys
- sys.foreign_keys_TSQL
- foreign_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.foreign_keys catalog view
ms.assetid: e960df1a-13fc-43ee-ba91-34c1b719ac2c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba0494d8ca1532f02e5f0cde92eef18d9a7d0639
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098038"
---
# <a name="sysforeign_keys-transact-sql"></a>sys.foreign_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждого объекта, который является ограничением внешнего ключа, с **sys. Object. Type** = F.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Список столбцов, наследуемых этим представлением, см. в разделе [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**referenced_object_id**|**int**|Идентификатор упоминаемого объекта.|  
|**key_index_id**|**int**|Идентификатор ключевого индекса в упоминаемом объекте.|  
|**is_disabled**|**bit**|Ограничение внешнего ключа отключено.|  
|**is_not_for_replication**|**bit**|Ограничение внешнего ключа создано с помощью параметра NOT FOR REPLICATION.|  
|**is_not_trusted**|**bit**|Ограничение внешнего ключа не было проверено системой.|  
|**delete_referential_action**|**tinyint**|Ссылочное действие, объявленное для данного внешнего ключа на случай удаления.<br /><br /> 0 = нет действий.<br /><br /> 1 = каскад.<br /><br /> 2 = задать NULL.<br /><br /> 3 = задать по умолчанию.|  
|**delete_referential_action_desc**|**nvarchar(60)**|Описание ссылочного действия, объявленного для данного внешнего ключа на случай удаления.<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**update_referential_action**|**tinyint**|Ссылочное действие, объявленное для данного внешнего ключа на случай обновления.<br /><br /> 0 = нет действий.<br /><br /> 1 = каскад.<br /><br /> 2 = задать NULL.<br /><br /> 3 = задать по умолчанию.|  
|**update_referential_action_desc**|**nvarchar(60)**|Описание ссылочного действия, объявленного для данного внешнего ключа на случай обновления.<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**is_system_named**|**bit**|1 = имя создано системой.<br /><br /> 0 = имя предоставлено пользователем.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
