---
description: sys.syscharsets (Transact-SQL)
title: sys.sysнаборов символов (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscharsets
- syscharsets
- sys.syscharsets_TSQL
- syscharsets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscharsets system table
- sys.syscharsets compatibility view
ms.assetid: f16d987c-bd19-4668-9ef7-785b8fb9ff5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 081ab46f57f78ed4e41f3a25050e9ba428c37507
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097806"
---
# <a name="syssyscharsets-transact-sql"></a>sys.syscharsets (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждой кодировки и порядок сортировки, определенный для использования в компоненте [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Один из порядков сортировки помечен в **sysconfigures** как порядок сортировки по умолчанию. Это единственный фактически используемый объект.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**type**|**smallint**|Тип сущности, представляемый этой строкой.<br /><br /> 1001 = кодировка.<br /><br /> 2001 = порядок сортировки.|  
|**id**|**tinyint**|Уникальный идентификатор для кодировки или порядка сортировки. Обратите внимание на то, что у порядков сортировки и кодировок не может быть одного и того же общего идентификатора. Идентификаторы в диапазоне от 1 до 240 зарезервированы для использования в компоненте [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|**csid**|**tinyint**|Если строка представляет кодировку, это поле остается неиспользованным. Если строка представляет порядок сортировки, в этом поле содержится идентификатор кодировки, на котором построен данный порядок сортировки. Предполагается, что в таблице содержится строка кодировки с данным идентификатором.|  
|**status**|**smallint**|Биты данных о внутреннем состоянии системы.|  
|**name**|**sysname**|Уникальное имя для кодировки или порядка сортировки. Это поле должно содержать только буквы A–Z или a–z, цифры 0–9, символ подчеркивания (_) и должно начинаться с буквы.|  
|**description**|**nvarchar(255)**|Необязательное описание характеристик кодировки или порядка сортировки.|  
|**binarydefinition**|**varbinary (6000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**definition**|**image**|Уникальное описание для кодировки или порядка сортировки. Структура данных в этом поле зависит от типа.|  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
