---
description: MSmerge_settingshistory (Transact-SQL)
title: MSmerge_settingshistory (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_settingshistory
- MSmerge_settingshistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_settingshistory system table
ms.assetid: 0bdf2d5f-5502-44cd-aa9d-2d5006ad20ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 10beb9d65bba15ef0b21acd2242fcbaf1d515dca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098585"
---
# <a name="msmerge_settingshistory-transact-sql"></a>MSmerge_settingshistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSmerge_settingshistory** используется для ведения журнала изменений, внесенных в статьи и свойства публикации для репликации слиянием, с одной строкой для каждого изменения, внесенного в топологию репликации слиянием. Данная таблица также хранит сведения о том, когда были произведены исходные настройки свойств. Эта таблица хранится в базах данных публикации и подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**EventTime**|**datetime**|Дата и время события.|  
|**pubid**|**uniqueidentifier**|Уникальный идентификатор данной публикации.|  
|**artid**|**uniqueidentifier**|Уникальный идентификационный номер данной статьи.|  
|**EventType**|**tinyint**|Указывает тип зарегистрированного события, может принимать одно из следующих значений:<br /><br /> **1** — начальное значение свойства уровня публикации.<br /><br /> **2** — изменение свойства публикации.<br /><br /> **101** — начальное значение свойства статьи.<br /><br /> **102** — изменение свойства статьи.|  
|**propertyname**|**sysname**|Имя установленного или измененного свойства|  
|**previousvalue**|**sysname**|Предыдущее значение свойства, если свойство было изменено.|  
|**NewValue**|**sysname**|Значение, присвоенное свойству при изменении или при создании.|  
|**eventtext**|**nvarchar (2000)**|Символьная строка, описывающая событие.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
