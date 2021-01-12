---
description: sys.destination_data_spaces (Transact-SQL)
title: sys.destination_data_spaces (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.destination_data_spaces
- destination_data_spaces_TSQL
- destination_data_spaces
- sys.destination_data_spaces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.destination_data_spaces catalog view
ms.assetid: 92df932b-ad5c-43f8-81f4-b158823ab189
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d5f3134b70aff51b4e3d91361bb5dacb6cb14f92
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093133"
---
# <a name="sysdestination_data_spaces-transact-sql"></a>sys.destination_data_spaces (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой цели пространства данных в схеме секционирования.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**partition_scheme_id**|**int**|Идентификатор схемы секции, разбиваемой в пространство данных.|  
|**destination_id**|**int**|Идентификатор (порядковое числительное от 1) целевого сопоставления, уникальный в пределах схемы секционирования.|  
|**data_space_id**|**int**|Идентификатор пространства данных, с которым сопоставлены данные для цели этой схемы.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
