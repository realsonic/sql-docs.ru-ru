---
description: sys.dm_os_stacks (Transact-SQL)
title: sys.dm_os_stacks (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_stacks
- dm_os_stacks_TSQL
- sys.dm_os_stacks
- sys.dm_os_stacks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_stacks dynamic management view
ms.assetid: a69b06c4-28f0-4535-8fa1-9f132db4d916
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6e5c42d0a82ce7726883e46f2e633aacc624e4db
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099725"
---
# <a name="sysdm_os_stacks-transact-sql"></a>sys.dm_os_stacks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Это динамическое административное представление используется в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для следующих внутренних целей.  
  
-   Отслеживание отладочных данных, например необработанных запросов на выделение памяти;  
  
-   Установление или проверка логики, применяющейся компонентами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] там, где компонент предполагает определенный вызов.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**stack_address**|**varbinary(8)**|Уникальный адрес для данного размещения в стеке. Не допускает значение NULL.|  
|**frame_index**|**int**|Каждая строка представляет вызов функции, который при сортировке в возрастающем порядке по индексу кадра для конкретного **stack_address** возвращает полный стек вызовов. Не допускает значение NULL.|  
|**frame_address**|**varbinary(8)**|Адрес вызова функции. Не допускает значение NULL.|  
  
## <a name="remarks"></a>Комментарии  
 **sys.dm_os_stacks** требует, чтобы символы сервера и другие компоненты присутствовали на сервере для правильного отображения информации.  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   


## <a name="see-also"></a>См. также:  
  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  
