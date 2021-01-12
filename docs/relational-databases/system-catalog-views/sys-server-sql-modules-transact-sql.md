---
description: sys.server_sql_modules (Transact-SQL)
title: sys.server_sql_modules (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.server_sql_modules
- sys.server_sql_modules_TSQL
- server_sql_modules_TSQL
- server_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_sql_modules catalog view
ms.assetid: 9ef9a8b9-c470-4a61-b0c4-ee24ad871d63
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7ffae9eb88425f849b30d7b2b8fc8e63cb840870
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094503"
---
# <a name="sysserver_sql_modules-transact-sql"></a>sys.server_sql_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит набор модулей SQL, для триггеров уровня сервера типа TR. Можно соединить это отношение с sys.server_triggers. Кортеж (object_id) является ключом отношения.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ВНЕШНИЙ КЛЮЧ (FOREIGN KEY) ссылающийся обратно на триггер уровня сервера, где этот модуль определен.|  
|**definition**|**nvarchar(max)**|Текст на языке SQL, определяющий этот модуль.<br /><br /> NULL = зашифрован.|  
|**uses_ansi_nulls**|**bit**|Модуль был создан с установкой параметра ANSI NULLS в значение ON.|  
|**uses_quoted_identifier**|**bit**|Модуль был создан с установкой параметра QUOTED IDENTIFIER в значение ON.|  
|**execute_as_principal_id**|**int**|Идентификатор сервера-участника, входящего в предложение EXECUTE AS.<br /><br /> Значение NULL по умолчанию или если EXECUTE AS CALLER<br /><br /> ИДЕНТИФИКАТОР указанного субъекта, если выполняется в качестве участника-2 = выполнение от имени владельца.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
