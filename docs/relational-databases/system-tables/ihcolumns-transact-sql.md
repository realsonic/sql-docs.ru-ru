---
description: IHcolumns (Transact-SQL)
title: Ихколумнс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHcolumns_TSQL
- IHcolumns
dev_langs:
- TSQL
helpviewer_keywords:
- IHcolumns system table
ms.assetid: 5bb027e5-5279-487b-9c33-5f402987253c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9d264dd2c3199527a763f5c2c8afce01dad17e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094840"
---
# <a name="ihcolumns-transact-sql"></a>IHcolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Системная таблица **ихколумнс** содержит по одной строке для каждого опубликованного столбца. Эта таблица используется для определения представления типов данных при публикации столбцов из издателя, работающего на сервере, отличном от SQL Server, который, по существу, выполняет сопоставление типов данных между СУБД сервера, отличного от SQL Server, и SQL Server. Эта таблица хранится в базе данных распространителя.  
  
## <a name="definition"></a>Определение  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**column_id**|**int**|Идентифицирует опубликованный столбец.|  
|**publishercolumn_id**|**int**|Связывает опубликованный столбец со метаданными столбца, хранящимися в системной таблице [ихпублишерколумнс](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) .|  
|**name**|**sysname**|Указывает имя столбца.|  
|**article_id**|**int**|Идентифицирует статью, к которой относится столбец.|  
|**column_ordinal**|**int**|Определяет порядковый номер столбца.|  
|**mapped_type**|**tinyint**|Тип данных целевого столбца на подписчике.|  
|**mapped_length**|**bigint**|Длина столбца на подписчике.|  
|**mapped_prec**|**int**|Точность столбца на подписчике.|  
|**mapped_scale**|**int**|Масштаб столбца на подписчике.|  
|**mapped_nullable**|**bit**|Указывает, принимает ли столбец на подписчике значения NULL, где **1** означает, что значения NULL принимаются.|  
  
## <a name="see-also"></a>См. также  
 [Разнородная репликация базы данных](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_articlecolumn (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [Системное представление sysarticlecolumns &#40;системного представления&#41; &#40;&#41;Transact-SQL ](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)   
 [Системное представление sysarticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
