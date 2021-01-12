---
description: MSagentparameterlist (Transact-SQL)
title: MSagentparameterlist (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSagentparameterlist_TSQL
- MSagentparameterlist
dev_langs:
- TSQL
helpviewer_keywords:
- Msagentparameterlist system table
ms.assetid: 4ea571a0-078d-4e13-95ee-f3d4bbd4dfb2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8f92e12e48a088c0328d14653ddafd069135201f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094859"
---
# <a name="msagentparameterlist-transact-sql"></a>MSagentparameterlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSagentparameterlist** содержит сведения о параметрах агента репликации и используется для указания параметров, которые могут быть установлены для данного типа агента. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**agent_type**|**tinyint**|Тип агента:<br /><br /> **1** = агент моментальных снимков.<br /><br /> **2** = агент чтения журнала.<br /><br /> **3** = агент распространения.<br /><br /> **4** = агент слияния.<br /><br /> **9** = агент чтения очереди.|  
|**parameter_name**|**sysname**|Имя действительного аргумента агента.|  
|**default_value**|**nvarchar(4000)**|Значение по умолчанию для аргумента агента, причем NULL указывает на то, что такого значения не существует.|  
|**min_value**|**int**|Устанавливает нижний предел для аргумента агента, причем NULL указывает на то, что нижнего предела нет.|  
|**max_value**|**int**|Устанавливает верхний предел для аргумента агента, причем NULL указывает на то, что верхнего предела нет.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
