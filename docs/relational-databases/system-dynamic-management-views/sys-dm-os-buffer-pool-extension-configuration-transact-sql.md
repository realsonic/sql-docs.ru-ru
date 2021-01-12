---
description: sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)
title: sys.dm_os_buffer_pool_extension_configuration (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_buffer_pool_extension_configuration
- sys.dm_os_buffer_pool_extension_configuration_TSQL
- dm_os_buffer_pool_extension_configuration_TSQL
- sys.dm_os_buffer_pool_extension_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_pool_extension_configuration dynamic management view
ms.assetid: d52cc481-4d29-4f33-b63d-231ec35d092f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5477a9da1810c64b6c6c359694f3db9254948efa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101547"
---
# <a name="sysdm_os_buffer_pool_extension_configuration-transact-sql"></a>sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о конфигурации расширения буферного пула в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возвращает по одной строке для каждого файла расширения буферного пула.  
  

  
| Имя столбца | Тип данных | Описание |
| :---------- | :-------- | :---------- |
|путь|**nvarchar**(256)|Путь и имя файла кэша расширения буферного пула. Допускает значение NULL.|  
|file_id|**int**|Идентификатор файла расширения буферного пула. Не допускает значение NULL.|  
|Состояние|**int**|Состояние расширения буферного пула. Не допускает значение NULL.<br /><br /> 0 = расширение буферного пула выключено<br /><br /> 1 = отключение расширения буферного пула<br /><br /> 2 — зарезервировано для будущего использования<br /><br /> 3 = включение расширения буферного пула<br /><br /> 4 = зарезервировано для использования в будущем<br /><br /> 5 = расширение буферного пула включено|  
|state_description|**nvarchar**(60)|Описывает состояние расширения буферного пула. Допускает значение NULL.<br /><br /> 0 = РАСШИРЕНИЕ БУФЕРНОГО ПУЛА ВЫКЛЮЧЕНО<br /><br /> 5 = РАСШИРЕНИЕ БУФЕРНОГО ПУЛА ВКЛЮЧЕНО|
|current_size_in_kb|**bigint**|Текущий размер файла расширения буферного пула. Не допускает значение NULL.|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-configuration-buffer-pool-extension-information"></a>A. Возвращает сведения о конфигурации расширения буферного пула.  
 Следующий пример возвращает все столбцы из динамического административного представления sys.dm_os_buffer_pool_extension_configruation.  
  
```sql  
SELECT path, file_id, state, state_description, current_size_in_kb  
FROM sys.dm_os_buffer_pool_extension_configuration;  
```  
  
### <a name="b-returning-the-number-of-cached-pages-in-the-buffer-pool-extension-file"></a>Б. Возвращает число кэшированных страниц в файле расширения буферного пула.  
 В следующем примере возвращается количество кэшированных страниц в каждом файле расширения буферного пула.  
  
```sql  
SELECT COUNT(*) AS cached_pages_count  
FROM sys.dm_os_buffer_descriptors  
WHERE is_in_bpool_extension <> 0  
;  
```  
  
## <a name="see-also"></a>См. также:  
 [Расширение буферного пула](../../database-engine/configure-windows/buffer-pool-extension.md)   
 [sys.dm_os_buffer_descriptors (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql.md)  
  
  
