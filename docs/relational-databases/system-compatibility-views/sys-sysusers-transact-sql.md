---
description: sys.sysusers (Transact-SQL)
title: sys.sysпользователей (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysusers
- sys.sysusers_TSQL
- sysusers_TSQL
- sysusers
dev_langs:
- TSQL
helpviewer_keywords:
- sysusers system table
- sys.sysusers compatibility view
ms.assetid: 5f0e6a8d-c983-44f6-97e9-aab5bff67d18
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 812aca2816b9df05596509e4a61b50d2e4e59d56
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095342"
---
# <a name="syssysusers-transact-sql"></a>sys.sysusers (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждого [!INCLUDE[msCoName](../../includes/msconame-md.md)] пользователя Windows, группы Windows, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пользователя или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] роли в базе данных.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**uid**|**smallint**|Идентификатор пользователя, уникальный в этой базе данных.<br /><br /> 1 = владелец базы данных.<br /><br /> Вызывает переполнение или возвращает значение NULL, если количество пользователей и ролей превышает 32 767.|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**name**|**sysname**|Имя пользователя или группы, уникальное в этой базе данных.|  
|**трансляцию**|**varbinary(85)**|Идентификатор безопасности для этой записи.|  
|**роли**|**varbinary (2048)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**креатедате**|**datetime**|Дата добавления этой учетной записи.|  
|**updatedate**|**datetime**|Дата последнего изменения учетной записи.|  
|**altuid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Вызывает переполнение или возвращает значение NULL, если количество пользователей и ролей превышает 32 767.|  
|**password**|**varbinary (256)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**gid**|**smallint**|Идентификатор группы, к которой принадлежит пользователь. Если **идентификатор UID** совпадает с **GID**, эта запись определяет группу. Вызывает переполнение или возвращает значение NULL, если общее количество групп и пользователей превышает 32 767.|  
|**Environ**|**varchar (255)**|Зарезервировано.|  
|**хасдбакцесс**|**int**|1 = учетная запись обладает правами доступа к базе данных.|  
|**islogin**|**int**|1 = учетная запись входа представляет группу Windows, пользователя Windows или пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], обладающего учетной записью для входа.|  
|**isntname**|**int**|1 = учетная запись представляет группу или пользователя Windows.|  
|**иснтграуп**|**int**|1 = учетная запись представляет группу Windows.|  
|**isntuser**|**int**|1 = учетная запись представляет пользователя Windows.|  
|**issqluser**|**int**|1 = учетная запись представляет пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**isaliased**|**int**|1 = учетная запись представляет псевдоним другого пользователя.|  
|**issqlrole**|**int**|1 = учетная запись представляет роль [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**исаппроле**|**int**|1 = учетная запись представляет роль приложения.|  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
