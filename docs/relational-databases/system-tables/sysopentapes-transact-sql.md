---
description: sysopentapes (Transact-SQL)
title: sysopentapes удалить нельзя (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysopentapes
- sysopentapes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], sysopentapes system table
- sysopentapes system table
ms.assetid: c066ca9b-9cfd-46b1-90a3-5c8dc9e7b6ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0cb05c0ac88f7f362651b872fa84598a656ff25d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096065"
---
# <a name="sysopentapes-transact-sql"></a>sysopentapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого из открытых ленточных устройств. Это представление хранится в базе данных **master** .  
  
> [!IMPORTANT]  
>  Данная системная таблица включена в качестве представления только для целей обратной совместимости. Вместо этого используйте динамическое административное представление [sys.dm_io_backup_tapes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md) .  
  
> [!NOTE]  
>  Невозможно удалить представление **sysopentapes удалить нельзя** .  

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**openTape**|**nvarchar (64)**|Физическое имя файла открытого ленточного устройства. Дополнительные сведения об открытии и освобождении ленточных устройств см. в разделе [BACKUP &#40;Transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md) and [RESTORE &#40;transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).|  
  
## <a name="permissions"></a>Разрешения  
 Пользователю необходимо разрешение VIEW SERVER STATE на сервере.  
  
  
