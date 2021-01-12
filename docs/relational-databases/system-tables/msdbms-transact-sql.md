---
description: MSdbms (Transact-SQL)
title: MSdbms (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_TSQL
- MSdbms
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms system table
ms.assetid: 2be631bf-de09-4e7a-9ccb-d6c37b81c237
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2dcea30947cfe6e2f124854d21d0bc48a8055469
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091474"
---
# <a name="msdbms-transact-sql"></a>MSdbms (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSdbms** содержит главный список всех версий систем управления базами данных (СУБД), поддерживаемых гетерогенной репликацией базы данных. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**dbms_id**|**int**|Определяет каждую уникальную СУБД и ее версию.|  
|**DBMS**|**sysname**|Имя СУБД.<br /><br /> MSSQLSERVER<br /><br /> DB2<br /><br /> ORACLE<br /><br /> SYBASE|  
|**version**|**varchar (10)**|Версия СУБД.|  
  
## <a name="see-also"></a>См. также  
 [Разнородная репликация базы данных](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
