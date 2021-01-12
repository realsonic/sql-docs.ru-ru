---
description: syscollector_config_store (Transact-SQL)
title: syscollector_config_store (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syscollector_config_store_TSQL
- syscollector_config_store
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_config_store view
ms.assetid: f15f6b05-6808-4b76-b6a8-48dec844cf63
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5914dfa56d4ee6dd360c391c64e636638bb4342
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094297"
---
# <a name="syscollector_config_store-transact-sql"></a>syscollector_config_store (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращаются свойства, которые применяются к сборщику данных в целом, а не к отдельному экземпляру набора элементов сбора. Каждая строка данного представления отражает определенное свойство сборщика данных, например имя хранилища данных управления или имя экземпляра сервера, на котором расположено это хранилище данных управления.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|parameter_name|**nvarchar(128)**|Имя свойства. Не допускает значение NULL.|  
|parameter_value|**sql_variant**|Фактическое значение свойства. Допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение SELECT для представления или членства в предопределенных ролях базы данных dc_operator, dc_proxy или dc_admin.  
  
## <a name="remarks"></a>Комментарии  
 Список доступных свойств фиксирован, а их значения могут быть изменены только с помощью соответствующей хранимой процедуры. В этой таблице описываются свойства, которые доступны через данное представление.  
  
|Имя свойства|Описание|  
|-------------------|-----------------|  
|CacheDirectory|Имя каталога в файловой системе, в которой сборщик данных хранит временную информацию.<br /><br /> Если указано значение NULL, то используется временный каталог [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|CacheWindow|Указывает политику хранения данных для каталога кэша для передач данных с ошибками.<br /><br /> -1 = сохранять данные после всех передач с ошибками.<br /><br /> 0 = не сохранять данные из передач данных с ошибками.<br /><br /> *n* = хранить данные из *n* предыдущих сбоев передачи, где *n* >= 1.<br /><br /> Воспользуйтесь хранимой процедурой sp_syscollector_set_cache_window для изменения этого значения.|  
|CollectorEnabled|Указывает состояние сборщика данных.<br /><br /> 0 = отключен<br /><br /> 1 = включен<br /><br /> Значение данного свойства можно изменить с помощью хранимой процедуры sp_syscollector_enable_collector или sp_syscollector_disable_collector.|  
|MDWDatabase|Имя хранилища данных управления. Используйте хранимую процедуру sp_syscollector_set_warehouse_database_name для изменения этого значения.|  
|MDWInstance|Имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на котором расположено хранилище данных управления. Используйте хранимую процедуру sp_syscollector_set_warehouse_instance_name для изменения этого значения.|  
  
## <a name="examples"></a>Примеры  
 В следующем примере запрос отправляется представлению syscollector_config_store.  
  
```sql  
SELECT parameter_name, parameter_value  
FROM msdb.dbo.syscollector_config_store;  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры сборщика данных (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Представления сборщика данных (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Сбор данных](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_enable_collector (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syscollector-enable-collector-transact-sql.md)   
 [sp_syscollector_disable_collector (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syscollector-disable-collector-transact-sql.md)   
 [sp_syscollector_set_warehouse_database_name &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-database-name-transact-sql.md)   
 [sp_syscollector_set_warehouse_instance_name &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-instance-name-transact-sql.md)   
 [sp_syscollector_set_cache_window (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-window-transact-sql.md)  
  
  
