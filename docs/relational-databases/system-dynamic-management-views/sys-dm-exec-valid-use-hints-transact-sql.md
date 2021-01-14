---
description: sys.dm_exec_valid_use_hints (Transact-SQL)
title: sys.dm_exec_valid_use_hints (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_valid_use_hints
- sys.dm_exec_valid_use_hints_TSQL
- dm_exec_valid_use_hints
- dm_exec_valid_use_hints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_valid_use_hints management view
ms.assetid: 65d50589-39c2-4046-92b6-0c4587d8c593
author: pmasl
ms.author: pelopes
ms.openlocfilehash: de99c9372846525349df3f9f312222e322bfe751
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171396"
---
# <a name="sysdm_exec_valid_use_hints-transact-sql"></a>sys.dm_exec_valid_use_hints (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Возвращает имена подсказок, поддерживаемые [указанием USE](../../t-sql/queries/hints-transact-sql-query.md#use_hint) . В нем указывается одно имя подсказки для каждой строки.  
  
Используйте это динамическое административное представление для просмотра списка всех поддерживаемых подсказок в нотации указания USE.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Имя подсказки.|

Описания каждого Совета см. в разделе [указания запросов](../../t-sql/queries/hints-transact-sql-query.md#use_hint) .

Представлено в [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] пакете обновления 1 (SP1).
  
## <a name="see-also"></a>См. также:  
    
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные с базами данных &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  

