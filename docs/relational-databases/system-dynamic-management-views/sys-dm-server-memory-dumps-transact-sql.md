---
description: sys.dm_server_memory_dumps (Transact-SQL)
title: sys.dm_server_memory_dumps (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_server_memory_dumps_TSQL
- dm_server_memory_dumps_TSQL
- dm_server_memory_dumps
- sys.dm_server_memory_dumps
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_memory_dumps dynamic management view
ms.assetid: 41782719-f54d-4e11-941a-c050c7576e23
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 29ae5e940daffd0487d5d3e4514b5b624dd1320e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096507"
---
# <a name="sysdm_server_memory_dumps-transact-sql"></a>sys.dm_server_memory_dumps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает одну строку для каждого файла дампа памяти, созданного [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. При помощи этого динамического административного представления можно устранять потенциальные неполадки.  
 
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**filename**|**nvarchar(256)**|Полный путь и имя файла дампа памяти. Не может иметь значение NULL.|  
|**creation_time**|**datetimeoffset(7)**|Дата и время создания файла. Не может иметь значение NULL.|  
|**size_in_bytes**|**bigint**|Размер файла (в байтах). Допускает значение NULL.|  
  
## <a name="general-remarks"></a>Общие замечания  
 Дамп может представлять собой минидамп, дамп всех потоков или полный дамп памяти. Файлы дампа имеют расширение .mdmp.  
  
## <a name="security"></a>Безопасность  
 Файлы дампа могут содержать конфиденциальные сведения. Чтобы защитить конфиденциальные сведения, можно ограничить доступ к этим файлам с помощью списка управления доступом (ACL) или скопировать их в папку, доступ к которой ограничен. Например, перед отправкой отладочных файлов в службу технической поддержки Майкрософт рекомендуется удалить из них все конфиденциальные сведения.  
  
### <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW SERVER STATE.  
  
  
