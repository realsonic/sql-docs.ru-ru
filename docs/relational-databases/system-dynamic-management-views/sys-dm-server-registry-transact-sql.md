---
description: sys.dm_server_registry (Transact-SQL)
title: sys.dm_server_registry (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_server_registry_TSQL
- sys.dm_server_registry
- dm_server_registry
- sys.dm_server_registry_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_registry dynamic management view
ms.assetid: 9b3e0c74-2e99-4996-a383-104d51831e97
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 909aaa820947285da692e43a178e7580a76adf58
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096463"
---
# <a name="sysdm_server_registry-transact-sql"></a>sys.dm_server_registry (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о конфигурации и установке для текущего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые хранятся в реестре Windows. Возвращает по одной строке для каждого раздела реестра. Используйте это динамическое административное представление для получения такой информации, как, например, сведения о службах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], доступных на сервере, или значения параметров сети для данного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|registry_key|**nvarchar(256)**|Имя раздела реестра Допускает значение NULL.|  
|value_name|**nvarchar(256)**|Имя значения ключа Это элемент, отображаемый в столбце **имя** редактора реестра. Допускает значение NULL.|  
|value_data|**sql_variant**|Значение данных ключа. Это значение, отображаемое в столбце **данных** редактора реестра для заданной записи. Допускает значение NULL.|  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-display-the-sql-server-services"></a>A. Отображение служб SQL Server  
 Следующий пример возвращает значения разделов реестра для служб SQL Server и агента SQL Server для текущего экземпляра SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%ControlSet%';  
```  
  
### <a name="b-display-the-sql-server-agent-registry-key-values"></a>Б. Отображение значений раздела реестра для агента SQL Server  
 Следующий пример возвращает значения разделов реестра агента SQL Server для текущего экземпляра SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%SQLAgent%';  
```  
  
### <a name="c-display-the-current-version-of-the-instance-of-sql-server"></a>В. Отображение текущей версии экземпляра SQL Server  
 Следующий пример возвращает версию текущего экземпляра SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE value_name = N'CurrentVersion';  
```  
  
### <a name="d-display-the-parameters-passed-to-the-instance-of-sql-server-during-startup"></a>Г. Отображение параметров, переданных текущему экземпляру SQL Server во время запуска  
 Следующий пример возвращает параметры, переданные экземпляру SQL Server во время запуска.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%Parameters';  
```  
  
### <a name="e-return-network-configuration-information-for-the-instance-of-sql-server"></a>Д. Отображение сведений о конфигурации сети для экземпляра SQL Server  
 Следующий пример возвращает значения параметров сети для текущего экземпляра SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%SuperSocketNetLib%';  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_server_services &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md)  
  
  
