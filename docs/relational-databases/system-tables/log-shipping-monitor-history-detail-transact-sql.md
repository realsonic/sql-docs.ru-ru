---
description: log_shipping_monitor_history_detail (Transact-SQL)
title: log_shipping_monitor_history_detail (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_monitor_history_detail_TSQL
- log_shipping_monitor_history_detail
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_history_detail system table
ms.assetid: 7080c888-323b-4206-a1ab-e6c51f9e2579
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3cd8f1b5c42d62ed508527b46d4a3705ad63dd09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094853"
---
# <a name="log_shipping_monitor_history_detail-transact-sql"></a>log_shipping_monitor_history_detail (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Сохраняет подробные данные журнала для заданий доставки журналов. Эта таблица хранится в базе данных **msdb** .  
  
 Таблицы, связанные с ведением журналов и мониторингом, также используются на сервере-источнике и серверах-получателях.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**agent_id**|**uniqueidentifier**|Первичный идентификатор для резервирования или вторичный идентификатор для копирования или восстановления.|  
|**agent_type**|**tinyint**|Тип задания доставки журналов:<br /><br /> 0 = резервирование;<br /><br /> 1 = копирование;<br /><br /> 2 = восстановление.|  
|**session_id**|**int**|Идентификатор сеанса для задания резервирования/копирования/восстановления.|  
|**database_name**|**sysname**|Имя базы данных, связанной с данной записью. База данных-источник для резервирования, база данных-получатель для восстановления или пустая для копирования.|  
|**session_status**|**tinyint**|Состояние сеанса.<br /><br /> 0 = начало.<br /><br /> 1 = запуск.<br /><br /> 2 = успешно.<br /><br /> 3 = ошибка.<br /><br /> 4 = предупреждение.|  
|**log_time**|**datetime**|Дата и время создания записи.|  
|**log_time_utc**|**datetime**|Дата и время создания записи по Гринвичу.|  
|**message**|**nvarchar(max)**|Текст сообщения.|  
  
## <a name="remarks"></a>Комментарии  
 Эта таблица содержит подробное описание агентов доставки журналов. Для обнаружения сеанса агента используйте столбцы **agent_id**, **agent_type** и **session_id**. Чтобы просмотреть данные журнала для сеанса агента, отсортируйте по **log_time**.  
  
 Помимо хранения на удаленном сервере мониторинга, сведения, относящиеся к основному серверу, хранятся на сервере-источнике в его **log_shipping_monitor_history_detail** таблице, а сведения, связанные с сервером-получателем, также хранятся на сервере-получателе в своей **log_shipping_monitor_history_detail** таблице.  
  
## <a name="see-also"></a>См. также:  
 [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_cleanup_log_shipping_history (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [Системные таблицы &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [log_shipping_monitor_error_detail &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)  
  
  
