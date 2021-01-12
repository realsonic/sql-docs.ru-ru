---
description: sys.sysconstraints (Transact-SQL)
title: Ограничения sys.sys(Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysconstraints
- sys.sysconstraints
- sysconstraints_TSQL
- sys.sysconstraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysconstraints compatibility view
- sysconstraints system table
ms.assetid: f2b2e2ad-ba24-48a1-913c-8ee4e0895dc4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3014430aad20c74d22a72e1756503baadbe7cb68
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099152"
---
# <a name="syssysconstraints-transact-sql"></a>sys.sysconstraints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит сопоставления ограничений с объектами, владеющими ограничениями внутри базы данных.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|Номер ограничения.|  
|**id**|**int**|Идентификатор таблицы, владеющей ограничением.|  
|**идентификатора столбца**|**smallint**|Идентификатор столбца, на котором определено ограничение.<br /><br /> 0 = ограничение таблицы|  
|**spare1**|**tinyint**|Зарезервированное|  
|**status**|**int**|Псевдобитовая маска, определяющая состояние. Возможные значения:<br /><br /> 1 = ограничение PRIMARY KEY;<br /><br /> 2 = ограничение UNIQUE KEY;<br /><br /> 3 = ограничение FOREIGN KEY;<br /><br /> 4 = ограничение CHECK;<br /><br /> 5 = ограничение DEFAULT;<br /><br /> 16 = ограничение на уровне столбца;<br /><br /> 32 = ограничение уровня таблицы.|  
|**actions**|**int**|Зарезервированное|  
|**error**|**int**|Зарезервированное|  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
