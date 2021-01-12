---
description: sys.database_service_objectives (база данных SQL Azure)
title: sys.database_service_objectives
titleSuffix: Azure SQL Database
ms.date: 03/21/2018
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
ms.reviewer: ''
ms.topic: conceptual
keywords:
- Эластичный пул
- Эластичный пул, управление
f1_keywords:
- DATABASE_SERVICE_OBJECTIVES_TSQL
ms.assetid: cecd8c31-06c0-4aa7-85d3-ac590e6874fa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 861e8297a54cff399425e521591cd983afbb0cc6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098022"
---
# <a name="sysdatabase_service_objectives-azure-sql-database"></a>sys.database_service_objectives (база данных SQL Azure)
[!INCLUDE [asdb-asdbmi-asa](../../includes/applies-to-version/asdb-asdbmi-asa.md)]

Возвращает выпуск (уровень служб), Цель обслуживания (ценовая категория) и имя эластичного пула (если таковые имеются) для базы данных SQL Azure или Azure синапсе Analytics. В системе базы данных master на сервере Базы данных SQL Azure возвращает сведения обо всех базах данных. Для Azure синапсе Analytics необходимо подключиться к базе данных master.  
  
  
 Сведения о ценах см. в разделе [Параметры и производительность базы данных SQL: цены на базу данных SQL](https://azure.microsoft.com/pricing/details/sql-database/) и [цены на Azure синапсе Analytics](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).  
  
 Сведения об изменении параметров службы см. в статье [ALTER DATABASE (база данных SQL Azure)](../../t-sql/statements/alter-database-transact-sql.md) и [ALTER DATABASE (Azure синапсе Analytics)](../../t-sql/statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).  
  
 Представление sys.database_service_objectives содержит следующие столбцы.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|database_id|INT|Идентификатор базы данных, уникальный в пределах экземпляра сервера базы данных SQL Azure. Соединение с [sys. databases &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).|  
|edition|sysname|Уровень служб для базы данных или хранилища данных: " **базовый**", " **стандартный**", " **премиум** " или " **хранилище данных**".|  
|service_objective|sysname|Ценовая категория базы данных. Если база данных находится в эластичном пуле, возвращает **ElasticPool**.<br /><br /> На уровне " **базовый** " возвращает значение " **базовый**".<br /><br /> **Одна база данных на уровне служб уровня "Стандартный** " возвращает одно из следующих состояний: S0, S1, S2, S3, S4, S6, S7, S9 или S12.<br /><br /> **Одна база данных на уровне Premium** возвращает следующие: P1, P2, P4, P6, P11 или P15.<br /><br /> **Azure синапсе Analytics** возвращает DW100 через DW30000c.<br /><br /> Дополнительные сведения см. в разделе [отдельные базы данных](/azure/sql-database/sql-database-dtu-resource-limits-single-databases/), [эластичные пулы](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools/), [хранилища данных](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu/) .|  
|elastic_pool_name|sysname|Имя [эластичного пула](/azure/azure-sql/database/elastic-pool-overview) , к которому принадлежит база данных. Возвращает **значение NULL** , если база данных является отдельной базой данных или хранилищем данных.|  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение **dbManager** на базу данных master.  На уровне базы данных пользователь должен быть создателем или владельцем.  
  
## <a name="examples"></a>Примеры  
 Этот пример можно выполнить в базе данных master или в базах данных пользователей базы данных SQL Azure. Запрос возвращает сведения об имени, службе и уровне производительности баз данных.  
  
```sql  
SELECT  d.name,   
     slo.*    
FROM sys.databases d   
JOIN sys.database_service_objectives slo    
ON d.database_id = slo.database_id;  
  
```  
  
