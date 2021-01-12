---
description: sys.dm_clr_loaded_assemblies (Transact-SQL)
title: sys.dm_clr_loaded_assemblies (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_clr_loaded_assemblies
- sys.dm_clr_loaded_assemblies_TSQL
- dm_clr_loaded_assemblies_TSQL
- sys.dm_clr_loaded_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_loaded_assemblies dynamic management view
ms.assetid: 8523d8db-d8a0-4b1f-ae19-6705d633e0a6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d132a0389ca636dbc24afe016b98389021d8d848
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099966"
---
# <a name="sysdm_clr_loaded_assemblies-transact-sql"></a>sys.dm_clr_loaded_assemblies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по строке для каждой из управляемых пользовательских сборок, загруженных в адресное пространство сервера. Используйте это представление для понимания и устранения неполадок в управляемых объектах базы данных интеграции со средой CLR, которые выполняются в [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Сборки — это файлы динамических библиотек с управляемым кодом, которые используются для определения и развертывания управляемых объектов базы данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Каждый раз, когда пользователь запускает один из управляемых объектов базы данных, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и среда CLR загружают сборку (а также другие сборки, на которые она ссылается), в которой определен этот управляемый объект базы данных. Сборка остается загруженной в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для увеличения производительности, чтобы впоследствии управляемые объекты базы данных, содержащиеся в сборке, можно было бы вызывать без повторной загрузки сборки. Сборка не выгружается до тех пор, пока у [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не возникнет необходимость в освобождении памяти. Дополнительные сведения о сборках и интеграции со средой CLR см. в разделе Среда [размещения CLR](../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md). Дополнительные сведения об управляемых объектах базы данных см. в разделе [Создание объектов базы данных с помощью среды clr &#40;интеграция&#41; CLR](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|Идентификатор загруженной сборки. **Assembly_id** можно использовать для поиска дополнительных сведений о сборке в представлении каталога [sys. assemblies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) . Обратите внимание, что в [!INCLUDE[tsql](../../includes/tsql-md.md)] каталоге [sys. assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) сборки отображаются только в текущей базе данных. В представлении **SQS.dm_clr_loaded_assemblies** отображаются все загруженные сборки на сервере.|  
|**appdomain_address**|**int**|Адрес домена приложения (**AppDomain**), в котором загружается сборка. Все сборки, принадлежащие одному пользователю, всегда загружаются в одном **домене AppDomain**. **Appdomain_address** можно использовать для поиска дополнительных сведений о **домене приложения** в представлении [sys.dm_clr_appdomains](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md) .|  
|**load_time**|**datetime**|Время, когда сборка была загружена. Обратите внимание, что сборка остается загруженной до тех пор, пока не найдет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] нехватка памяти и не выгрузит **AppDomain**. Вы можете отслеживать **load_time** , чтобы понять, как часто приходится наблюдать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] недостаток памяти и выгружать **AppDomain**.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="remarks"></a>Комментарии  
 Представление **dm_clr_loaded_assemblies. appdomain_address** имеет связь "многие к одному" с  **dm_clr_appdomains. appdomain_address**. Представление **dm_clr_loaded_assemblies. assembly_id** имеет связь «один ко многим» с **sys. assemblies. assembly_id**.  
  
## <a name="examples"></a>Примеры  
 Следующий пример показывает, как можно просматривать сведения обо всех сборках, которые в данный момент загружены в текущей базе данных.  
  
```  
 SELECT a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
FROM sys.dm_clr_loaded_assemblies AS l   
INNER JOIN sys.assemblies AS a  
ON l.assembly_id = a.assembly_id;  
```  
  
 В следующем примере показано, как просмотреть сведения о **домене приложения** , в котором загружается заданная сборка.  
  
```  
SELECT appdomain_id, creation_time, db_id, user_id, state  
FROM sys.dm_clr_appdomains AS a  
WHERE appdomain_address =   
(SELECT appdomain_address   
 FROM sys.dm_clr_loaded_assemblies  
 WHERE assembly_id = 555);  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления, связанные со средой CLR &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
