---
description: sysmergesubsetfilters (Transact-SQL)
title: sysmergesubsetfilters (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergesubsetfilters
- sysmergesubsetfilters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubsetfilters system table
ms.assetid: f91d1c6c-3132-47f6-926c-88f56848cafe
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4978af42a06f1e54c8457f0db4a2d5c5fe2601ea
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099612"
---
# <a name="sysmergesubsetfilters-transact-sql"></a>sysmergesubsetfilters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит сведения о фильтрах соединения для секционированных статей. Эта таблица хранится в базах данных публикации и подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**filtername**|**sysname**|Имя фильтра, используемого для создания новых статей.|  
|**join_filterid**|**int**|Идентификатор объекта, представляющего фильтр соединения.|  
|**pubid**|**uniqueidentifier**|Идентификатор публикации.|  
|**artid**|**uniqueidentifier**|Идентификатор статьи.|  
|**art_nickname**|**int**|Псевдоним статьи.|  
|**join_articlename**|**sysname**|Имя таблицы, с которой необходимо выполнить соединение для определения принадлежности строк.|  
|**join_nickname**|**int**|Псевдоним таблицы, с которой необходимо выполнить соединение для определения принадлежности строк.|  
|**join_unique_key**|**int**|Указывает соединение с уникальным ключом **join_tablename**:<br /><br /> 0 = Не уникальный ключ.<br /><br /> 1 = Уникальный ключ.|  
|**expand_proc**|**sysname**|Имя хранимой процедуры, используемой агентом слияния для выявления строк, которые необходимо отправить или удалить на подписчике.|  
|**join_filterclause**|**nvarchar (1000)**|Предложение фильтра, используемое для соединения.|  
|**filter_type**|**tinyint**|Показывает тип фильтра, который может быть одним из следующих:<br /><br /> 1 = Фильтр соединения.<br /><br /> 2 = Связь логических записей.<br /><br /> 3 = Одновременно фильтр соединения и связь логических записей.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
