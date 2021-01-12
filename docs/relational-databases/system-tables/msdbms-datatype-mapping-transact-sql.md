---
description: MSdbms_datatype_mapping (Transact-SQL)
title: MSdbms_datatype_mapping (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_datatype_mapping_TSQL
- MSdbms_datatype_mapping
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype_mapping system table
ms.assetid: 13289a0b-dfb0-4771-ad80-4c5f83cded99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 308e11275807ff11e3700bfcb443238c8c6ffeef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098677"
---
# <a name="msdbms_datatype_mapping-transact-sql"></a>MSdbms_datatype_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSdbms_datatype_mapping** таблица содержит допустимые сопоставления типов данных из типа данных в системе управления базы данных-источника (СУБД) с одним или несколькими конкретными типами данных в целевой СУБД. Эта таблица хранится в базе данных **msdb** и используется для гетерогенной репликации базы данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**datatype_mapping_id**|**int**|Идентифицирует каждое уникальное сопоставление типа данных.|  
|**map_id**|**int**|Идентифицирует исходный тип данных.|  
|**dest_datatype_id**|**int**|Идентифицирует целевой тип данных.|  
|**dest_precision**|**bigint**|Определяет точность целевого типа данных, где значение NULL означает, что точность не используется, а значение **-1** означает, что используется точность исходного типа данных.|  
|**dest_scale**|**int**|Определяет масштаб целевого типа данных, где значение NULL означает, что масштаб не используется, а значение **-1** означает, что используется масштаб исходного типа данных.|  
|**dest_length**|**bigint**|Определяет длину целевого типа данных, где значение NULL означает, что эта длина не используется, а значение **-1** означает, что используется длина исходного типа данных.|  
|**dest_nullable**|**bit**|Показывает, допускает ли целевой столбец в данном сопоставлении значения NULL; причем NULL означает, что это определение не требуется.|  
|**dest_createparams**|**int**|Битовая карта, описывающая, какое сочетание длины, точности и масштаба применимо для каждого типа данных:<br /><br /> **0x1** = точность.<br /><br /> **0x2** = масштаб.<br /><br /> **0x4** = length.|  
  
## <a name="see-also"></a>См. также  
 [Разнородная репликация базы данных](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Указание сопоставлений типов данных для издателя Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
