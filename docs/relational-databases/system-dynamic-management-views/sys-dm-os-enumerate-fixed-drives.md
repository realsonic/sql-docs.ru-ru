---
description: sys.dm_os_enumerate_fixed_drives (Transact-SQL)
title: sys.dm_os_enumerate_fixed_drives (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_enumerate_fixed_drives
- sys.dm_os_enumerate_fixed_drives_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_enumerate_fixed_drives dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e909c2b38df99f0e2cd5aa4addc2e657f60b5cfa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097587"
---
# <a name="sysdm_os_enumerate_fixed_drives-transact-sql"></a>sys.dm_os_enumerate_fixed_drives (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Впервые представлен в SQL Server 2019.

Перечисляет тома, подключенные к буквам диска `C:\` , например.

|Имя столбца|Тип данных|Описание|
|-----------------|---------------|-----------------|  
|`fixed_drive_path`|`nvarchar(512)`|Путь к тому, например `C:\` .|  
|`drive_type`|`int`|Код для типа диска. См. раздел [ `GetDriveTypeW` функция](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`drive_type_desc`|`nvarchar(512)`|Описание типа диска. См. раздел [ `GetDriveTypeW` функция](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`free_space_in_bytes`|`bigint`|Свободное место на диске (в байтах).|

## <a name="permissions"></a>Разрешения

Пользователь должен иметь `VIEW SERVER STATE` разрешение на сервере.

## <a name="see-also"></a>См. также:  

 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с вводом-выводом &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
